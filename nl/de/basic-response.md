---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# Wissenswertes über Erkennungsergebnisse
{: #basic-response}

Ungeachtet der verwendeten Schnittstelle gibt der {{site.data.keyword.speechtotextfull}}-Service Transkriptionsergebnisse zurück, die die von Ihnen angegebenen Eingabe- und Ausgabeparameter abbilden. Der Service gibt den gesamten JSON-Antwortinhalt im UTF-8-Zeichensatz zurück.
{: shortdesc}

## Antwort für Basistranskription
{: #response}

Für die Beispiele im Abschnitt [Erkennungsanforderung ausgeben](/docs/services/speech-to-text?topic=speech-to-text-basic-request) gibt der Service die folgende Antwort zurück. In den Beispielen wird lediglich eine Audiodatei und ihr Inhaltstyp übergeben. In der Audiodatei wird ein einziger Satz ohne nennenswerte Sprechpausen zwischen den Wörtern gesprochen.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Der Service gibt ein Objekt `SpeechRecognitionResults` zurück; dies ist das Antwortobjekt der höchsten Ebene. Für diese einfachen Anforderungen enthält das Objekt ein Feld `results` und ein Feld `result_index`:

-   Das Feld `results` stellt eine Reihe von Informationen zu den Transkriptionsergebnissen bereit. Für dieses Beispiel enthält das Feld `alternatives` in den Ergebnissen die Transkription (`transcript`) und die Konfidenz des Service (`confidence`). Das Feld `final` enthält den Wert `true` und gibt hierdurch an, dass sich diese Ergebnisse nicht ändern.
-   Das Feld `result_index` stellt eine eindeutige ID für die Ergebnisse bereit. Das Beispiel zeigt die Endergebnisse für eine Anforderung mit einer einzigen Audiodatei ohne Sprechpausen; weitere Parameter sind in der Anforderung nicht enthalten. Der Service gibt daher ein einziges Feld `result_index` mit dem Wert `0` zurück, der immer der Anfangsindex ist.

Falls die Audioeingabedaten komplexer sind oder die Anforderung zusätzliche Parameter enthält, können die Ergebnisse weitere Informationen enthalten.

### Feld 'alternatives'
{: #responseAlternatives}

Das Feld `alternatives` stellt ein Array von Transkriptionsergebnissen bereit. Bei dieser Anforderung enthält das Array ein einziges Element.

-   Das Feld `confidence` gibt eine Bewertung für die Konfidenz des Service bei der Transkription an, die bei diesem Beispiel nahezu 90 Prozent erreicht.
-   Im Feld `transcript` werden die Ergebnisse der Transkription bereitgestellt.

Die Felder `final` und `result_index` qualifizieren die Bedeutung dieser Felder.

### Feld 'final'
{: #responseFinal}

Das Feld `final` gibt an, ob das Feld 'transcript' die Endergebnisse für die Transkription enthält:

-   Für Endergebnisse, die sich garantiert nicht ändern, enthält das Feld den Wert `true`. Für Transkriptionen, die der Service als Endergebnisse zurückgibt, sendet er keine weiteren Aktualisierungen.
-   Für Zwischenergebnisse, bei denen Änderungen vorbehalten sind, enthält das Feld den Wert `false`. Falls Sie bei der WebSocket-Schnittstelle den Parameter `interim_results` verwenden, gibt der Service sich ergebende Zwischenhypothesen in Form von mehreren Feldern `results` zurück, während er die Audiodaten transkribiert. Für Zwischenergebnisse hat das Feld `final` immer den Wert `false`. Für die Endergebnisse der Audiodaten setzt der Service das Feld auf den Wert `true`. Für die Transkription solcher Audiodaten sendet der Service keine weiteren Aktualisierungen.

Um Zwischenergebnisse zu erhalten, verwenden Sie die WebSocket-Schnittstelle und geben Sie für den Parameter `interim_results` die Einstellung `true` an. Weitere Informationen finden Sie im Abschnitt [Zwischenergebnisse](/docs/services/speech-to-text?topic=speech-to-text-output#interim).

### Feld 'result_index'
{: #responseResultIndex}

Das Feld `result_index` stellt für die Ergebnisse eine ID zur Verfügung, die für diese Anforderung eindeutig ist. Falls Sie Zwischenergebnisse anfordern, sendet der Service mehrere Felder `results` mit den sich ergebenden Hypothesen für die Eingabeaudiodaten. Die Indizes der Zwischenergebnisse für dieselben Audiodaten haben - wie auch die Endergebnisse für dieselben Audiodaten - immer denselben Wert.

Sobald Sie für Audiodaten Endergebnisse empfangen haben, sendet der Service für den Rest der Anforderung keine weiteren Ergebnisse mit diesem Index. Der Index für alle weiteren Ergebnisse wird jeweils um 1 erhöht.

Unabhängig davon, ob Sie Zwischenergebnisse anfordern, kann der Service mehrere Endergebnisse mit unterschiedlichen Indizes zurückgeben, falls Ihre Audiodaten Sprechpausen oder längere Phasen des Schweigens enthalten. Weitere Informationen finden Sie unter [Sprechpausen und Schweigen](#pauses-silence).

Falls Ihre Audiodaten zu mehreren Endergebnissen führen, verketten Sie die Elemente `transcript` der Endergebnisse, um die vollständige Transkription der Audiodaten zusammenzusetzen. Setzen Sie die Ergebnisse in der Reihenfolge der Werte in den Feldern `result_index` zusammen. Zwischenergebnisse, für die das Feld `final` den Wert `false` aufweist, können Sie hierbei ignorieren.

### Zusätzlicher Antwortinhalt
{: #responseAdditional}

Viele Ausgabeparameter, die für die Spracherkennung verfügbar sind, wirken sich auf den Inhalt der Serviceantwort aus. Die folgenden Parameter bewirken beispielsweise, dass der Service zusätzliche Informationen zu den Audiodaten zurückgibt:

-   Der Parameter `max_alternatives` fordert mehrere alternative Transkriptionen an. Das Array `alternatives` enthält für jede Alternative ein separates Element. Jedes Element des Arrays enthält ein Feld `transcript`. Nur die Alternative mit der größten Wahrscheinlichkeit enthält ein Feld `confidence`.
-   Die Parameter `word_confidence` und `timestamps` fordern zusätzliche Informationen zu einzelnen Wörtern der Transkription an. Das Array `alternatives` enthält zusätzliche Felder mit diesen Namen.
-   Die Parameter `keywords` und `keywords_threshold` fordern eine Erkennung von Schlüsselwörtern (das so genannte 'Keyword Spotting') an. Das Array `results` enthält ein Feld `keywords_result`.
-   Der Parameter `word_alternatives_threshold` fordert akustisch ähnliche Ergebnisse für einzelne Wörter an. Das Array `results` enthält das Feld `word_alternatives`.
-   Der Parameter `interim_results` der WebSocket-Schnittstelle fordert vorläufige Hypothesen für die Transkription an. Wie bereits erläutert sendet der Service mehrere Antworten, während der die Audiodaten transkribiert.
-   Der Parameter `speaker_labels` identifiziert die einzelnen Sprecher eines Gesprächs mit mehreren Teilnehmern. Die Antwort enthält ein Feld `speaker_labels`, das sich auf derselben Ebene wie die Felder `results` und `results_index` befindet.

Weitere Informationen zu diesen und anderen Parametern, die sich auf die Antwort des Service auswirken können, finden Sie unter [Ausgabefunktionen](/docs/services/speech-to-text?topic=speech-to-text-output). Zusätzliche Angaben über alle verfügbaren Parameter enthält der Abschnitt [Parameterübersicht](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Sprechpausen und Schweigen
{: #pauses-silence}

Der Service transkribiert einen vollständigen Audiodatenstrom, bis entweder der Datenstrom endet oder ein Zeitlimit überschritten wird. Wenn die Audiodaten jedoch zwischen gesprochenen Wörtern oder Ausdrücken Sprechpausen oder längere Phasen des Schweigens beinhalten, können die Erkennungsergebnisse aus mehreren Endergebnissen bestehen.

Das Standardpausenintervall, das der Service verwendet, um separate Endergebnisse zu bestimmen, beträgt etwa eine Sekunde. Bei den meisten Sprachen beträgt das Pausenintervall genau 0,8 Sekunden; bei Chinesisch liegt das Pausenintervall bei 0,6 Sekunden. Die Dauer des Pausenintervalls kann nicht geändert werden.

Wie der Service die Ergebnisse zurückgibt, hängt von der verwendeten Schnittstelle ab. Die folgenden Beispiele zeigen Antworten mit zwei Endergebnissen aus der HTTP-Schnittstelle und der WebSocket-Schnittstelle. In beiden Fällen wird dieselbe Audiodateneingabe verwendet. In den Audiodaten wird der Ausdruck 'one two three four five six' mit einer einsekündigen Pause zwischen den Wörtern 'three' und 'four' gesprochen.

-   *Für die HTTP-Schnittstelle* sendet der Service ein einziges Objekt `SpeechRecognitionResults`. Das Array `alternatives` enthält für jedes Endergebnis ein separates Element. Die Antwort beinhaltet ein einziges Element `result_index` mit dem Wert `0`.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *Für die WebSocket-Schnittstelle* sendet der Service zwei separate Antworten mit zwei unterschiedlichen Objekten `SpeechRecognitionResults`. Jedes Antwortobjekt besitzt ein unterschiedliches Feld `result_index`, das bei der ersten Antwort den Wert `0` und bei der zweiten Antwort den Wert `1` enthält.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

Falls Ihre Ergebnisse mehrere Endergebnisse enthalten, verketten Sie die Elemente `transcript` der Endergebnisse, um die vollständige Transkription der Audiodaten zusammenzusetzen.

Ein 30 Sekunden langes Schweigen in gestreamten Audiodaten kann zu einem [Inaktivitätszeitlimit](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity) führen.
{: note}

## Stockungsmarkierungen
{: #hesitation}

Der Service kann Stockungsmarkierungen in eine Transkription einbeziehen, wenn er in der Sprache Verlegenheitslaute oder Sprechpausen erkennt. Solche auch als 'Unflüssigkeiten' bezeichneten Sprechpausen können Flicklaute wie 'uhm', 'hmm' oder 'äh' und ähnliche nicht verbale Äußerungen enthalten. Sofern Sie Stockungsmarkierungen für Ihre Anwendung nicht benötigen, können Sie sie problemlos aus einer Transkription herausfiltern.

In Englisch verwendet der Service das Stockungstoken `%HESITATION`, das im folgenden Beispiel dargestellt ist. Andere Sprachen können andere Markierungen verwenden; beispielsweise wird bei Japanisch das Token `D_` verwendet.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Stockungsmarkierungen können auch in anderen Feldern einer Transkription auftreten. Falls Sie beispielsweise [Wortzeitmarken](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) für die einzelnen Wörter einer Transkription anfordern, meldet der Service die Anfangs- und Endzeit jeder Stockungsmarkierung.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Großbuchstaben am Wortanfang
{: #capitalization}

Bei den meisten Sprachen verwendet der Service in Antworttranskriptionen keine Großbuchstaben am Wortanfang. Wenn die Großschreibung für Ihre Anwendung wichtig ist, müssen Sie beim ersten Wort jedes Satzes sowie bei allen anderen Begriffen, für die die Großschreibung geeignet ist, den ersten Buchstaben in einen Großbuchstaben ändern.

Viele Eigennamen werden vom Service groß geschrieben; *allerdings nur bei amerikanischem Englisch*. Für den angegebenen Ausdruck gibt der Service beispielsweise den folgenden Text zurück:

```
Barack Obama graduated from Columbia University
```
{: codeblock}

Bei anderen Sprachen gibt der Service den folgenden Text zurück:

```
barack obama graduated from columbia university
```
{: codeblock}

Der Service wendet diese Großschreibung auf amerikanisches Englisch immer an, unabhängig davon, ob Sie die intelligente Formatierung verwenden.

## Interpunktion
{: #punctuation}

Standardmäßig fügt der Service keine Interpunktion in Antworttranskriptionen ein. Gegebenenfalls benötigte Interpunktion müssen Sie selbst zu den Ergebnissen des Service hinzufügen.

Bei einigen Sprachen können Sie den Service durch die Verwendung der intelligenten Formatierung anweisen, bestimmte Schlüsselwortzeichenfolgen durch Interpunktionssymbole wie Kommas, Punkte, Fragezeichen und Ausrufezeichen zu ersetzen. Weitere Informationen finden Sie unter [Intelligente Formatierung](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).
