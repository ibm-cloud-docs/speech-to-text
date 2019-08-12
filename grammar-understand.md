---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Wissenswertes über Grammatiken
{: #grammarUnderstand}

In diesem Abschnitt wird die Unterstützung des {{site.data.keyword.speechtotextfull}}-Service für Grammatiken anhand von Beispielen vorgestellt. In den Beispielen werden zwei einfache ABNF-Grammatiken erstellt und die möglichen Ergebnisse für ihren Einsatz bei der Spracherkennung gezeigt. Die Beispiele veranschaulichen, wie wichtig die Untersuchung der Konfidenzbewertung ist, die der Service in eine Transkription einbezieht.
{: shortdesc}

In den Beispielen werden nur die Ergebnisse von Spracherkennungsanforderungen bereitgestellt. Beispiele, die veranschaulichen, wie Sie eine Grammatik für die Spracherkennung übergeben, finden Sie unter [Grammatik für die Spracherkennung verwenden](/docs/services/speech-to-text?topic=speech-to-text-grammarUse). Außerdem sind die Beispiele sehr einfach gehalten. Beispiele für komplexere Grammatiken enthält der Abschnitt [Beispielgrammatiken](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples).

## Übereinstimmung mit einzelnem Ausdruck: Grammatiktyp 'yesno'
{: #yesnoGrammar}

Im ersten Beispiel wird eine sehr einfache Grammatik des Typs `yesno` definiert, die zwei gültige Ein-Wort-Antworten akzeptiert, nämlich `yes` und `no`. Die Grammatik ist zweckmäßig, wenn der Benutzer nur mit einem von beiden Ausdrücken antworten muss.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

Wenn Sie diese Grammatik auf eine Spracherkennungsanforderung anwenden, kann der Service eine Transkription mit einer Bewertung zurückgeben, die die Konfidenz der Übereinstimmung angibt. Er hat außerdem die Möglichkeit, kein Ergebnis zurückzugeben, wenn die Eingabe eindeutig nicht mit einem der beiden Ausdrücke übereinstimmt.

Falls der Benutzer beispielsweise mit `yes` antwortet, gibt der Service wahrscheinlich eine Antwort zurück, die große Ähnlichkeit mit dem folgenden Ergebnis hat. Die Bewertung im Feld `confidence` deutet auf eine absolut zuverlässige Übereinstimmung hin.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Angenommen, der Benutzer antwortet jedoch beispielsweise mit `nope`. Der Service kann hier entweder ein Ergebnis mit einer sehr niedrigen Konfidenzbewertung oder gar kein Ergebnis zurückgeben. Ein leeres Ergebnis ist der deutlichste Hinweis darauf, dass die Antwort nicht mit der Grammatik übereinstimmt. Eine leere Antwort tritt eher bei komplexen Grammatiken auf, bei denen eine gültige Antwort mit einer Folge von mehreren Ausdrücken übereinstimmen muss.

## Übereinstimmungen mit mehreren Ausdrücken: Grammatiktyp 'names'
{: #namesGrammar}

Bei einer Grammatik für mehrere Ausdrücke muss die Antwort des Benutzers vollständig sein, damit sie erkannt wird. Der Benutzer kann weder ein Wort weglassen, noch mitten in der Antwort enden. Schon das Fehlen eines einzigen Wortes kann dazu führen, dass der Service ein leeres Ergebnis zurückgibt.

Darüber hinaus kann der Service mehrere Transkriptionen zurückgeben, wenn die vom Benutzer gesprochenen Ausdrücke durch ausreichende Sprechpausen voneinander abgegrenzt sind, die deutlich machen, dass es sich um separate verbale Äußerungen handelt. Beispiel: Die folgende einfache Grammatik des Typs `names` kann einen von drei aus mehreren Wörtern bestehenden Namen abgleichen.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

Angenommen, der Benutzer spricht mit `Yon See` einen der Namen aus den Grammatikregeln. Der Service gibt dann eine Antwort zurück, die einen sehr hohen Konfidenzgrad für die Übereinstimmung angibt.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Nehmen wird nun an, dass der Benutzer zwei Namen durch eine ausreichende Sprechpause (mindestens 0,8 Sekunden) voneinander getrennt spricht, um deutlich zu machen, dass es sich um separate verbale Äußerungen handelt: `Yon See` [1 Sekunde Sprechpause] `Yi Wen Tan`. In diesem Fall sendet der Service zwei separate Antworten mit einer eigenen Konfidenzbewertung für jede Transkription.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

Nehmen wir abschließend an, dass der Benutzer etwas wie `Yon See` [1 Sekunde Sprechpause] `Young Says He` spricht. Für den ersten Ausdruck (`Yon See`) sendet der Service eine positive Übereinstimmung mit einer ähnlichen Konfidenzbewertung wie in den vorherigen Beispielen. Für den zweiten Ausdruck (`Young Says He`) kann der Service eine von zwei möglichen Antworten bereitstellen:

-   Möglicherweise sendet er gar keine Antwort, um kenntlich zu machen, dass der Ausdruck keiner der Regeln in der Grammatik entspricht.
-   Möglicherweise sendet er stattdessen eine Antwort mit einer niedrigen Konfidenzbewertung und macht somit deutlich, dass der Ausdruck einer der Regeln akustisch ähnelt, jedoch wahrscheinlich keine Übereinstimmung darstellt.

## Konfidenzbewertungen und leere Ergebnisse
{: #confidenceScores}

Bei relativ einfachen Grammatiken wie in den obigen Beispielen kann der Service selbst dann ein Ergebnis mit einer geringen Konfidenzbewertung zurückgeben, wenn die Antwort überhaupt nicht in der Grammatik enthalten ist. Dies mag zwar überraschend erscheinen, aber eine Antwort mit einer niedrigen Konfidenzbewertung kann die beste Übereinstimmung angeben, die der Service für die bereitgestellten Audiodaten ermitteln konnte, selbst wenn die Antwort wahrscheinlich nicht mit der Grammatik übereinstimmt. Falls die Antwort nicht mit der Grammatik übereinstimmt, ist der Konfidenzwert der Ergebnisse normalerweise niedrig genug, damit erkennbar ist, dass eine Generierung der Antwort durch die Grammatik unwahrscheinlich ist.

Berücksichtigen Sie immer die Konfidenzbewertung, wenn Sie auswerten, ob eine Antwort eine Grammatik erfüllt. Wenn Sie nicht wissen, welchen Schwellenwert Sie für die Zurückweisung eines Ergebnisses aufgrund seiner Konfidenz festlegen sollen, verwenden Sie für den Anfang den Wert 0,5. Falls Sie anschließend unerwartet große Zurückweisungsraten für korrekte Ergebnisse feststellen, setzen Sie den Konfidenzschwellenwert herab; beispielsweise kann 0,1 für einige Anwendungen eine gute Option sein. Falls Sie jedoch viele falsche Erkennungen feststellen, setzen Sie den Konfidenzschwellenwert für Ihre Anwendung herauf.

Ein leeres Ergebnis oder ein Ergebnis mit einer niedrigen Konfidenzbewertung ist in vielen Fällen eine gültige Antwort. Sie kann darauf hindeuten, dass der Benutzer die Frage oder die verfügbaren Antwortmöglichkeiten nicht verstanden hat. Sie können zwar in jedem Fall die Audioantwort des Benutzers ohne die Grammatik erkennen, laufen dann aber Gefahr, ein Ergebnis zu erhalten, das für die Grammatik nicht gültig ist. Grammatiken liefern zuverlässigere Ergebnisse als N-Gramme, die für alles andere als vollständiges Schweigen immer ein Ergebnis zurückgeben.

Das Wissen, dass das Ergebnis einer der von einer Grammatik definierten gültigen Ausdrücke sein muss, ist eine leistungsfähige Funktion, die die Antwortverarbeitung einer Anwendung vereinfachen kann. Allgemein kann der Service Ergebnisse mit einer hohen Konfidenz nur für Äußerungen zurückgeben, die durch eine Grammatik generiert werden. Damit die Grammatik des Typs `yesno` aus dem ersten Beispiel den Ausdruck `nope` zuverlässig als gültige Antwort erkennt, müssen Sie diesen Ausdruck zur Grammatik hinzufügen.
