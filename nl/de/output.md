---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Ausgabefunktionen
{: #output}

Der {{site.data.keyword.speechtotextshort}}-Service bietet die folgenden Funktionen zum Aufrufen von Informationen, die in die Transkriptionsergebnisse für eine Spracherkennungsanforderung eingefügt werden. Alle Ausgabeparameter sind optional.
{: shortdesc}

-   Beispiele für einfache Spracherkennungsanforderungen für die einzelnen Schnittstellen des Service finden Sie im Abschnitt [Erkennungsanforderung ausgeben](/docs/services/speech-to-text/basic-request.html).
-   Beispiele und Beschreibung für Spracherkennunganforderungen finden Sie im Abschnitt [Wissenswertes über Erkennungsergebnisse](/docs/services/speech-to-text/basic-response.html). Der Service gibt alle Inhalte von JSON-Antworten mit dem Zeichensatz UTF-8 zurück.
-   Eine alphabetische Liste aller verfügbaren Spracherkennungsparameter einschließlich der zugehörigen Statuswerte (allgemein verfügbar oder als Betafunktionalität) sowie der unterstützten Sprachen finden Sie im Abschnitt [Parameterübersicht](/docs/services/speech-to-text/summary.html).

## Sprecherbezeichnungen
{: #speaker_labels}

Die Funktion für Sprecherbezeichnungen wird als Betafunktionalität bereitgestellt und ist für amerikanisches Englisch, Japanisch, Spanisch (Breit- und Schmalbandmodelle) und britisches Englisch (nur Schmalbandmodell) verfügbar.
{: note}

Sprecherbezeichnungen geben an, von welchen Personen in einer Konversation mit mehreren Beteiligten welche Worte gesprochen wurden. (Die Kennzeichnung der Sprecher und des Sprechzeitpunkts wird gelegentlich auch als *Sprecherprotokollierung* bezeichnet.) Mithilfe dieser Informationen können Sie ein nach Personen aufgeteiltes Transkript für einen Audiodatenstrom (z. B. ein Anruf bei einem Call-Center) erstellen. Oder Sie können die Informationen verwenden, um einen Dialog mit einem Roboter oder Avatar zu animieren. Um gute Leistungswerte zu erzielen, sollten die Audiodaten mindestens eine Minute umfassen.

Sprecherbezeichnungen sind auf Szenarios mit zwei Sprechern abgestimmt. Sie funktionieren am besten für Telefongespräche, bei denen zwei Personen einen längeren Dialog führen. Zwar können bis zu sechs Sprecher verarbeitet werden, aber bei mehr als zwei Sprechern ist das resultierende Leistungsverhalten wechselhaft. Gespräche zwischen zwei Personen werden in der Regel über Schmalbandmedien geführt, dabei können jedoch Sprecherbezeichnungen in unterstützten Schmal- und Breitbandmodellen zum Einsatz kommen.

Um diese Funktion zu nutzen, setzen Sie den Parameter `speaker_labels` für eine Erkennungsanforderung auf `true`. Der Parameter ist standardmäßig auf `false` gesetzt. Der Service erkennt Sprecher an einzelnen Wörtern in den Audiodaten. Der Sprecher eines Wortes wird anhand des Start- und Endzeitpunkts des betreffenden Wortes identifiziert. Für die Verwendung von Sprecherbezeichnungen muss daher auch der Parameter `timestamps` auf `true` gesetzt werden (siehe [Wortzeitmarken](/docs/services/speech-to-text/output.html#word_timestamps)).

### Beispiel für Sprecherbezeichnungen
{: #speakerLabelsExample}

Das folgende Beispiel zeigt eine Antwort mit Sprecherbezeichnungen. Die zugeordneten Zahlenwerte für die einzelnen Elemente des Arrays `timestamps` sind die Start- und Endzeitpunkte des Wortes in den Audiodaten.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

Das Feld `transcript` enthält das endgültige Transkript der Audiodaten, in dem die von allen Beteiligten gesprochenen Wörter aufgelistet sind. Durch Vergleichen und Synchronisieren der Sprecherbezeichnungen mit den Zeitmarken können Sie den Ablauf des Dialogs rekonstruieren.

<table style="width:50%">
  <caption>Tabelle 1. Beispiel für Sprecherbezeichnungen</caption>
  <tr>
    <th style="text-align:left">Zeitmarke</th>
    <th style="text-align:left">Sprecherbezeichnung</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

Das Ergebnis zeigt deutlich, wie dieser Dialog zwischen zwei Personen begann:

-   **Sprecher 2** - "Hello?"
-   **Sprecher 1** - "Yeah?"
-   **Sprecher 2** - "Yeah, how's Billy?"

Die Konfidenzfelder (`confidence`) in diesem Beispiel geben die Konfidenz des Service bei der Identifizierung der Sprecher an. Im Feld `final` ist für jedes Wort der Wert `false` angegeben. Dieser Wert bedeutet, dass die Verarbeitung der Audiodaten durch den Service noch nicht abgeschlossen ist. Der Service kann die Identifizierung des Sprechers oder den Konfidenzwert für einzelne Wörter noch ändern, bis die Verarbeitung abgeschlossen ist.

### Sprecher-IDs für Sprecherbezeichnungen
{: #speakerLabelsIDs}

Das Beispiel veranschaulicht einen interessanten Aspekt von Sprecher-IDs. In den Feldern für Sprecher (`speaker`) hat der erste Sprecher zunächst die ID `2` und der zweite Sprecher die ID `1`. Der Service verfeinert die Einteilung der Sprecherabfolge im Laufe der Verarbeitung der Audiodaten. Dabei können Sprecher-IDs für einzelne Wörter geändert sowie Sprecher hinzugefügt bzw. entfernt werden, bis das endgültige Ergebnis vorliegt.

Dies hat zur Folge, dass die Sprecher-IDs möglicherweise nicht folgerichtig, konsistent oder geordnet sind. Beispiel: Die IDs beginnen normalerweise mit der ID `0`. Im vorherigen Beispiel ist die kleinste ID jedoch `1`. Der Service hat vermutlich eine frühere Wortzuordnung für den Sprecher `0` im Verlauf der weiteren Analyse der Audiodaten verworfen. Solche Änderungen können auch für spätere Sprecher-IDs vorgenommen werden. Sie können zu Lücken in der Nummerierung führen, sodass den Sprechern im Analyseergebnis die IDs `0` und `2` zugeordnet sind. Es kann auch vorkommen, dass Sprecher den Dialog verlassen. In diesem Fall kommt ein Teilnehmer, der am Anfang des Dialogs Beiträge geliefert hat, in den späteren Ergebnissen möglicherweise nicht mehr vor.

### Zwischenergebnisse für Sprecherbezeichnungen anfordern
{: #speakerLabelsInterim}

In der WebSocket-Schnittstelle können Sie Zwischenergebnisse und Sprecherbezeichnungen anfordern (siehe [Zwischenergebnisse](/docs/services/speech-to-text/output.html#interim)). Die Endergebnisse sind in der Regel genauer als Zwischenergebnisse. Zwischenergebnisse können jedoch helfen, die Entwicklung eines Transkripts und die Zuordnung der Sprecherbezeichnungen nachzuvollziehen. Aus den Zwischenergebnisse wird möglicherweise ersichtlich, an welchen Stellen temporäre Sprecher und IDs in den Dialog eingestiegen bzw. daraus ausgestiegen sind. Es kann jedoch vorkommen, dass Sprecher-IDs, die ursprünglich identifiziert und später verworfen wurden, im weiteren Verlauf vom Service wiederverwendet werden. Dies kann dazu führen, dass dieselbe ID in den Zwischenergebnissen und in den Endergebnissen zwei verschiedene Sprecher bezeichnet.

Wenn Sie sowohl Zwischenergebnisse als auch Sprecherbezeichnungen anfordern, kann es vorkommen, dass die Endergebnisse für lange Audiodatenströme erst lange nach den anfänglichen Zwischenergebnissen zurückgegeben werden. Außerdem kann es vorkommen, dass manche Zwischenergebnisse nur ein Feld `speaker_labels` und nicht die Felder `results` und `result_index` enthalten. Wenn Sie keine Zwischenergebnisse anfordern, gibt der Service Endergebnisse mit den Feldern `results` und `result_index` und einem einzelnen Feld `speaker_labels` zurück.

Der Service sendet die Endergebnisse, wenn der Audiodatenstrom beendet ist oder als Antwort auf eine Zeitlimitüberschreitung (je nachdem, welches dieser Ereignisse zuerst eintritt). In beiden Fällen setzt der Service das Feld `final` nur für das zuletzt zurückgegebene Wort der Sprecherbezeichnungen auf `true`.

### Leistungsaspekte für Sprecherbezeichnungen
{: #speakerLabelsPerformance}

Wie bereits erwähnt, ist die Funktion für Sprecherbezeichnungen auf Dialoge zwischen zwei Personen (z. B. Anrufe bei einem Call-Center) abgestimmt. Daher sollten Sie die folgenden potenziellen Leistungsprobleme berücksichtigen: 

-   Für Audiodaten mit einem einzigen Sprecher werden möglicherweise keine guten Leistungswerte erzielt. Schwankungen in der Tonqualität oder der Stimme des Sprechers können dazu führen, dass der Service zusätzliche Sprecher erkennt, die gar nicht da sind. Solche Sprecher werden als Halluzinationen bezeichet.
-   Ebenso können die Leistungswerte für Audiodaten mit einem dominanten Sprecher (z. B. Podcasts) schlecht ausfallen. Dabei erkennt der Service Sprecher möglicherweise nicht, die nur kurze Redebeiträge leisten. Dies kann ebenfalls zu Halluzinationen führen.
-   Die Leistung bei Audiodaten mit mehr als sechs Sprechern ist nicht vorhersehbar. Die Funktion kann höchsten sechs Sprecher verarbeiten.
-   Die Erkennungsleistung bei kurzen Äußerungen kann weniger präzise sein als bei langen Äußerungen. Der Service erzielt bessere Ergebnisse, wenn jeder Teilnehmer des Gesprächs längere Redebeiträge (mindestens 30 Sekunden) liefert. Der mengenmäßige Anteil jedes einzelnen Sprechers an den Audiodaten kann sich ebenfalls auf die Leistung auswirken.
-   Die Leistung kann in den ersten 30 Sekunden des Sprechbeitrags abnehmen. Erst ab einer Dauer von 1 Minute liefern die Audiodaten in der Regel ein akzeptables Leistungsniveau.

Wie bei allen Transkriptionen kann die Leistung auch durch schlechte Tonqualität, Hintergrundgeräusche, die Sprechweise einer Person, gleichzeitig sprechende Teilnehmer und andere Aspekte der Audioerfassung beeinträchtigt werden.

## Schlüsselworterkennung
{: #keyword_spotting}

Die Funktion für Schlüsselworterkennung identifiziert angegebene Zeichenfolgen in einem Transkript. Der Service kann mehrere Vorkommen desselben Schlüsselworts erkennen und erfassen. Dabei werden Schlüsselwörter nur in den Endergebnissen erkannt und nicht in den Zwischenergebnissen. Die Funktion für Schlüsselworterkennung ist im Service standardmäßig inaktiviert.

Wenn Sie die Schlüsselworterkennung verwenden möchten, müssen Sie die beiden folgenden Parameter angeben:

-   Geben Sie im Parameter `keywords` ein Array mit den zu erkennenden Zeichenfolgen an. Der Service erkennt keine Schlüsselwörter, wenn der Parameter nicht angegeben oder leer ist. Eine Schlüsselwortzeichenfolge kann mehr als ein Token enthalten. Beispiel: Das Schlüsselwort `Speech to Text` umfasst drei Tokens.

    Für amerikanisches Englisch normalisiert der Service jedes Schlüsselwort, um gesprochen und geschriebene Zeichenfolgen abzugleichen. Beispiel: Durch Normalisieren von Zahlen werden die Sprech- und Schreibweisen der Zahlen abgeglichen. Für andere Sprachen müssen Schlüsselwörter in der gesprochenen Form angegeben werden.

    Mit einer einzigen Anforderung können bis zu  1.000 Schlüsselwörter erkannt werden. Schlüsselwörter sind von der Groß-/Kleinschreibung unabhängig.
-   Im Parameter `keywords_threshold` können Sie als Erkennungswahrscheinlichkeit für ein Schlüsselwort einen Wert zwischen 0,0 und 1,0 angeben. Dieser Schwellenwert gibt die Untergrenze für das Konfidenzniveau an, das der Service für ein Wort ermittelt haben muss, damit eine Übereinstimmung mit dem Schlüsselwort erkannt wird. Ein Schlüsselwort wird im Transkript nur erkannt, wenn das zugehörige Konfidenzniveau größer-gleich dem angegebenen Schwellenwert ist.

    Das Angeben eines niedrigen Schwellenwerts kann dazu führen, dass viele Übereinstimmungen festgestellt werden. Wenn Sie einen Schwellenwert angeben, müssen Sie auch mindestens ein Schlüsselwort angeben. Wenn Sie den Parameter nicht angeben, werden keine Übereinstimmungen zurückgegeben.

Die Schlüsselworterkennung ist erforderlich, um Schlüsselwörter in einem Audiodatenstrom zu identifizieren. Die Schlüsselworterkennung durch Verarbeiten eines endgültigen Transkripts ist nicht möglich, da das Transkript die bestmöglichen Decodierungsergebnisse des Service für die Audioeingabedaten darstellt. Es enthält keine Tokens mit niedrigen Konfidenzwerten, die möglicherweise ein infrage kommendes Wort darstellen. Mit anderen Worten: Das Anwenden von Textverarbeitungstools auf ein Transkript auf der Clientseite verhindert möglicherweise das Erkennen von Schlüsselwörtern. Daher wird eine detailliertere Darstellung der Decodierungsergebnisse benötigt, die nur auf dem Server verfügbar ist.

### Ergebnisse der Schlüsselworterkennung
{: #keywordSpottingResults}

Der Service gibt die Ergebnisse der Erkennung in einem Feld `keywords_result` im Array `results` zurück. Das Feld `keywords_result` enthält ein Wörterverzeichnis oder ein assoziatives Array der aufzulistenden Merkmale. Jedes Merkmal wird durch ein angegebenes Schlüsselwort identifiziert und enthält ein Array von Objekten. Der Service gibt für jede gefundene Übereinstimmung mit dem Schlüsselwort ein Element des Arrays zurück. Das Objekt für jede Übereinstimmung enthält die folgenden Felder: 

-   `normalized_text` ist das angegebene Schlüsselwort, normalisiert auf den gesprochenen Ausdruck, der in den Audioeingabedaten als Übereinstimmung erkannt wurde.
-   `start_time` ist der Startzeitpunkt der Übereinstimmung, angegeben in Sekunden.
-   `end_time` ist der Endzeitpunkt der Übereinstimmung, angegeben in Sekunden.
-   `confidence` ist der vom Service ermittelte Konfidenzwert für die Übereinstimmung mit dem angegebenen Schlüsselwort. Der Konfidenzwert muss größer-gleich dem angegebenen Schwellenwert sein, damit die Übereinstimmung in die Ergebnisse aufgenommen wird.

Ein Schlüsselwort, für das der Service keine Übereinstimmungen findet, wird nicht in das Array eingetragen. In den folgenden Fällen wird möglicherweise kein Schlüsselwort gefunden: 

-   Das Schlüsselwort kommt in den Audiodaten nicht vor. Das Fehlen des Schlüsselworts ist der offensichtlichste Grund.
-   Der festgelegte Schwellenwert ist zu hoch. Wenn der Service einen zu niedrigen Konfidenzwert für das Schlüsselwort ermittelt, wird die gefundene Übereinstimmung nicht in die Ergebnisse eingetragen.
-   Eine gesprochene Schlüsselwortzeichenfolge mit mehreren Tokens (z. B. `Speech to Text`) enthält zu viele Sprechpausen zwischen den Tokens. Beim Transkribieren der Audiodaten zerteilt der Service den Datenstrom in eine Reihe von Blöcken. Jeder Block besteht aus einem zusammenhängenden Segment der Audiodaten, das keine Sprechpause enthält, die länger als eine halbe Sekunde ist. Als Ergebnis wird ein Array von Objekten erstellt, das aus diesen Blöcken besteht.

    Der Service erkennt ein Schlüsselwort mit mehreren Tokens nur in den folgenden Fällen als Übereinstimmung:

    -   Die Tokens des Schlüsselworts sind im selben Block enthalten.
    -   Die Tokens folgen lückenlos aufeinander oder die Lücken sind nicht länger als 0,1 Sekunde.

    Der letzte Fall kann eintreten, wenn ein kurzes Füllwort oder eine nicht lexikalische Äußerung wie "uhm" oder "well" zwischen den Tokens des Schlüsselworts liegt. Weitere Informationen finden Sie im Abschnitt [Stockungsmarkierungen](/docs/services/speech-to-text/basic-response.html#hesitation).

### Beispiel für Schlüsselworterkennung
{: #keywordSpottingExample}

In der folgenden Beispielanforderung wird für den Parameter `keywords` ein URL-codiertes Array mit drei Zeichenfolgen (`colorado`, `tornado` und `tornadoes`) angegeben und für den Parameter `keywords_threshold` der Wert `0.5`. Der Service findet infrage kommende Vorkommen von `colorado` und `tornadoes`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.89,
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

## Maximale Anzahl Alternativen
{: #max_alternatives}

Der Parameter `max_alternatives` akzeptiert einen ganzzahligen Wert, der den Service anweist, die *n* besten alternativen Kandidaten für die Ergebnisse zurückzugeben. Standardmäßig gibt der Service nur ein einzelnes Transkriptionsergebnis zurück (dies entspricht dem Parameterwert `1`). Wenn Sie für den Parameter `max_alternatives` einen Wert größer als 1 angeben, gibt der Service diese Anzahl der besten alternativen Transkriptionen zurück. (Wenn Sie `0` angeben, verwendet der Service den Standardwert `1`.)

Der Service gibt nur für die beste zurückgegebene Alternative einen Konfidenzwert an. In den meisten Fällen wird diese Alternativ ausgewählt.

### Beispiel für maximale Anzahl Alternativen
{: #maximumAlternativesExample}

In der folgenden Beispielanforderung wird für den Parameter `max_alternatives` der Wert `3` festgelegt. Der Service gibt einen Konfidenzwert für die wahrscheinlichste der drei Alternativen an.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down is a line of
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

## Zwischenergebnisse
{: #interim}

Die Funktion für Zwischenergebnisse ist nur für die WebSocket-Schnittstelle verfügbar.
{: note}

Zwischenergebnisse sind vorläufige Hypothesen für eine Transkription, die noch geändert werden können, bevor der Service das Endergebnis zurückgibt. Zwischenergebnisse werden vom Service zurückgegeben, sobald sie erstellt wurden. Zwischenergebnisse sind in den folgenden Szenarios hilfreich:

-   Interaktive Anwendungen und Transkription in Echtzeit
-   Lange Audiodatenströme mit zeitaufwendigem Transkriptionsprozess

Zwischenergebnisse werden häufiger und schneller bereitgestellt, als Endergebnisse. Sie können verwendet werden, um schnellere Antwortzeiten für Anwendungen zu ermöglichen oder den Fortschritt des Transkriptionsprozesses zu messen.

Wenn Sie Zwischenergebnisse empfangen möchten, setzen Sie in der Nachricht `start` für eine WebSocket-Erkennungsanforderung den JSON-Parameter `interim_results`auf `true`. Wenn Sie den Parameter `interim_results` nicht angeben oder auf `false` setzen, gibt der Service nur ein einziges JSON-Transkript am Ende des Audiodatenstroms zurück. Beachten Sie die folgenden Richtlinien für die Verwendung des Parameters:

-   Geben Sie den Parameter nicht an oder setzen Sie ihn auf `false`, wenn die Transkription im Offline-Modus oder im Stapelbetrieb ausgeführt wird bzw. wenn keine Mindestlatenzzeit erforderlich ist und ein einziges JSON-Ergebnis für alle Audiodaten erstellt werden soll.
-   Setzen Sie den Parameter auf `true`, wenn der Service während der Verarbeitung der Audiodaten fortlaufend Ergebnisse liefern soll oder Ergebnisse mit einer Mindestlatenzzeit bereitgestellt werden sollen. Dabei ist zu beachten, dass die Zwischenergebnisse vom Service im Laufe der Audiodatenverarbeitung gegebenenfalls aktualisiert werden.

Zwischenergebnisse sind in der JSON-Antwort daran zu erkennen, dass das Feld `final` auf `false` gesetzt ist. Im Verlauf der weiteren Audiodatenverarbeitung werden die Zwischenergebnisse vom Service gegebenenfalls durch genauere Ergebnisse ersetzt. Die endgültigen Ergebnisse sind daran zu erkennen, dass das Feld `final` auf `true` gesetzt ist. Die Endergebnisse sind endgültig und werden vom Service nicht mehr aktualisiert.

### Beispiel für Zwischenergebnisse
{: #interimResultsExample}

Im folgenden gekürzten Beispiel werden Zwischenergebnisse für eine WebSocket-Anforderung angefordert. In der Antwort des Service wird das Attribut `final` nur für das Endergebnis auf `true` gesetzt.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
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

## Wortalternativen
{: #word_alternatives}

Die Funktion für Wortalternativen (auch als Confusion Networks bezeichnet) meldet Hypothesen (Kandidaten) für akustisch ähnliche Alternativen zu Wörtern in den Audioeingabedaten. Beispielsweise kann das Wort `Austin` die beste Hypothese für ein Wort aus den Audiodaten sein. Das Wort `Boston` kann jedoch eine weitere Hypothese im selben Zeitintervall sein. Hypothesen weisen einen gemeinsamen Start- und Endzeitpunkt auf, aber sie unterscheiden sich in Schreibweise und (in der Regel) Konfidenzwert. 

Die Funktion für Wortalternativen ist im Service standardmäßig inaktiviert. Um anzugeben, dass Sie alternative Hypothesen empfangen möchten, können Sie im Parameter `word_alternatives_threshold` einen Wahrscheinlichkeitswert zwischen 0,0 und 1,0 angeben. Der Schwellenwert gibt die Untergrenze für das Konfidenzniveau an, das der Service für eine Hypothese ermittelt haben muss, damit Sie als Wortalternative zurückgegeben wird. Eine Hypothese wird nur vorgeschlagen, wenn der zugehörige Konfidenzwert größer-gleich dem angegebenen Schwellenwert ist.

Wortalternativen sind mit der Zeitachse für ein Transkript vergleichbar, die in kleinere Abschnitte (sogenannte Fächer) aufgeteilt ist. Jedes Fach kann eine oder mehrere Hypothese(n) mit unterschiedlichen Schreibweisen und Konfidenzwerten enthalten. Der Parameter `word_alternatives_threshold` steuert die Dichte (Menge) der vom Service zurückgegebenen Ergebnisse. Wenn ein niedriger Schwellenwert angegeben wird, kann dies zu einer großen Menge von Hypothesen führen.

### Ergebnisse für Wortalternativen
{: #wordAlternativesResults}

Der Service gibt die Ergebnisse in einem Feld `word_alternatives` als Element des Arrays `results` zurück. Das Feld `word_alternatives` ist ein Array mit Objekten, die jeweils die folgenden Felder für eine Wortalternative enthalten:

-   `start_time` gibt den Zeitpunkt in Sekunden an, an dem das Wort, für das Hypothesen zurückgegeben werden, in den Audioeingabedaten beginnt.
-   `end_time` gibt den Zeitpunkt in Sekunden an, an dem das Wort, für das Hypothesen zurückgegeben werden, in den Audioeingabedaten endet.
-   `alternatives` ist ein Array mit Hypothesenobjekten. Jedes Objekt enthält ein Element `confidence`, das das vom Service ermittelte Konfidenzniveau für die Hypothese angibt, und ein Element `word`, das die Hypothese angibt. Der Konfidenzwert muss größer-gleich dem angegebenen Schwellenwert sein, damit die Hypothese in die Ergebnisse aufgenommen wird. Der Service sortiert die Alternativen nach Konfidenzwert.

### Beispiel für Wortalternativen
{: #wordAlternativesExample}

In der folgenden Beispielanforderung wird ein Schwellenwert (`word_alternatives_threshold`) von `0.9` angegeben. Die Ausgabe enthält mögliche Hypothesen und Konfidenzwerte für eine Reihe von Wörtern mit den zugehörigen Start- und Endzeitpunkten.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.89,
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

## Wortkonfidenz
{: #word_confidence}

Der Parameter `word_confidence` gibt an, ob der Service Konfidenzwerte für die Wörter des Transkripts bereitstellen soll. Standardmäßig liefert der Service nur zusammenfassende Konfidenzwerte für das endgültige Transkript. Wenn der Parameter `word_confidence` auf `true` gesetzt ist, liefert der Service für jedes Wort in dem Transkript einen Konfidenzwert.

Ein Konfidenzwert gibt an, wie hoch der Service anhand der akustischen Indikatoren die Korrektheit des transkribierten Wortes einschätzt. Konfidenzwerte liegen im Bereich von 0,0 bis 1,0.

-   Der Wert 1,0 gibt an, dass die aktuelle Transkription des Wortes mit sehr hoher Wahrscheinlichkeit das beste Ergebnis darstellt.
-   Der Wert 0,5 bedeutet, dass das Wort mit einer Wahrscheinlichkeit von 50 Prozent korrekt ist.

### Beispiel für Wortkonfidenz
{: #wordConfidenceExample}

Im folgenden Beispiel werden Wortkonfidenzwerte für die Wörter in der Transkription angefordert. 

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "confidence": 0.89,
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Wortzeitmarken
{: #word_timestamps}

Der Parameter `timestamps` gibt an, ob der Service Zeitmarken für die transkribierten Wörter erstellen soll. In der Standardeinstellung erzeugt der Service keine Zeitmarken. Wenn Sie `timestamps` auf `true` setzen, wird der Service angewiesen, die Start- und Endzeitpunkte in Sekunden (in Relation zum Start der Audiodaten) für jedes Wort zu erfassen.

### Beispiel für Wortzeitmarken
{: #wordTimestampsExample}

Im folgenden Beispiel werden Zeitmarken für die Wörter der Transkription angefordert.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.89,
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

## Intelligente Formatierung
{: #smart_formatting}

Die Funktion für intelligente Formatierung ist als Betafunktionalität nur für amerikanisches Englisch, Japanisch und Spanisch verfügbar.
{: note}

Der Parameter `smart_formatting` weist den Service an, die folgenden Zeichenfolgen in konventionellere Darstellungen umzuwandeln:

-   Datumsangaben
-   Zeitangaben
-   Ziffern- und Zahlenreihen
-   Telefonnummern
-   Währungswerte
-   Internet-E-Mail- und Webadressen

Setzen Sie den Parameter `smart_formatting` auf `true`, um die Funktion für intelligente Formatierung zu aktivieren. Die Funktion für intelligente Formatierung ist im Service standardmäßig inaktiviert.

Der Service wendet die intelligente Formatierung nur auf das endgültige Transkript einer Erkennungsanforderung an. Die intelligente Formatierung wird unmittelbar vor der Rückgabe der Ergebnisse an den Client angewendet, nachdem die Textnormalisierung abgeschlossen ist. Diese Formatierung bzw. Umwandlung macht das Transkript besser lesbar und vereinfacht die Nachbearbeitung der Transkriptionsergebnisse.

### Interpunktion (amerikanisches Englisch)
{: #smartFormattingPunctuation}

Für amerikanisches Englisch weist die Funktion den Service außerdem an, die folgenden gesprochenen Schlüsselwortzeichenfolgen in den Audiodaten durch die Interpunktionszeichen zu ersetzen.

<table style="width:50%">
  <caption>Tabelle 2. Schlüsselwörter für die Interpunktion bei der intelligenten Formatierung</caption>
  <tr>
    <th style="width:45%; text-align:left">Schlüsselwortzeichenfolge</th>
    <th style="text-align:center">Resultierende Interpunktion</th>
  </tr>
  <tr>
    <td>
      Komma
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      Punkt
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      Fragezeichen
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      Ausrufezeichen
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

Der Service wandelt diese Schlüsselwortzeichenfolgen nur an den entsprechenden Stellen in Interpunktionszeichen um. Beispielsweise wandelt der Service den gesprochenen Ausdruck

```
der Garantiezeitraum ist kurz punkt
```
{: codeblock}

in den folgenden Text im Transkript um

```
der Garantiezeitraum ist kurz.
```
{: codeblock}

### Unterschiede in den unterstützten Sprachen
{: #smartFormattingDifferences}

Die intelligente Formatierung basiert auf dem Vorhandensein erkennbarer Schlüsselwörter im Transkript. In den unterstützten Sprachen gibt es kleine Abweichungen bei der Verarbeitung der intelligenten Formatierung.

#### Amerikanisches Englisch und Spanisch
{: #smartFormattingEnglishSpanish}

-   Zeitangaben werden durch Schlüsselwörter wie `AM`, `PM` oder `EST` gekennzeichnet.
-   Zeitangaben im militärischen Zeitformat werden umgewandelt, wenn Sie durch das Schlüsselwort `hours` identifiziert werden.
-   Telefonnummern müssen entweder `911` oder eine Rufnummer mit 10 bzw. 11 Ziffern sein, die mit der Zahl `1` beginnt.

#### Japnisch
{: #smartFormattingJapanese}

-   Internet-E-Mail- und Webadressen werden nicht umgewandelt.
-   Telefonnummern müssen aus 10 bzw. 11 Ziffern bestehen und mit den gültigen Präfixen für japanische Telefonnummern beginnen. Zu den gültigen Präfixen gehören zum Beispiel `03` und `090`.
-   Englische Wörter werden in ASCII-Zeichen (*hankaku*) umgewandelt. Beispiel: <code>&#65321;&#65314;&#65325;</code> wird in `IBM` umgewandelt.
-   Mehrdeutige Begriffe werden möglicherweise nicht umgewandelt, wenn nicht genügend Kontext verfügbar ist. Beispielsweise ist nicht klar erkennbar, ob es sich bei <code>&#19968;&#26178;</code> und <code>&#21313;&#20998;</code> um Zeitangaben handelt.
-   Die Interpunktion wird mit und ohne intelligente Formatierung gleich behandelt. Beispielsweise wird auf der Grundlage von Wahrscheinlichkeitsberechnungen eine der beiden Möglichkeiten <code>&#12459;&#12531;&#12510;</code> oder `,` ausgewählt.

### Beispiel für intelligente Formatierung
{: #smartFormattingExample}

Im folgenden Beispiel wird die intelligente Formatierung mit einer Erkennungsanforderung angefordert, indem der Parameter `smart_formatting` auf `true` gesetzt wird. Die folgenden Abschnitte veranschaulichen die Auswirkungen der intelligenten Formatierung auf die Ergebnisse einer Anforderung.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### Ergebnisse der intelligenten Formatierung
{: #smartFormattingResults}

Die folgende Tabelle enthält Beispiele für endgültige Transkripte mit und ohne intelligente Formatierung. Transkripte basieren auf Audiodaten in der Sprache amerikanisches Englisch.

<table summary="Nach jeder Überschriftzeile folgen mehrere Zeilen mit Beispielen, die die Auswirkungen der intelligenten Formatierung auf das in der Überschrift angegebene Element darstellen.">
  <caption>Tabelle 3. Beispieltranskripte für intelligente Formatierung</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">Ohne
      intelligente Formatierung</th>
    <th id="with_formatting" style="text-align:left">Mit intelligenter
      Formatierung</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>Datumsangaben</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>Zeitangaben</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>Zahlen</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>Telefonnummern</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>Währungswerte</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>Internet-E-Mail- und Webadressen</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>Kombinationen</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### Beispieltranskripte mit langen Pausen
{: #smartFormattingLongPauses}

Wenn eine Äußerung Sprechpausen enthält, die lang genug sind, kann der Service das Transkript in zwei oder mehr Blöcke aufteilen. Dies wirkt sich auf die Ergebnisse der Formatierung aus, wie in den folgenden Beispielen dargestellt.

<table>
  <caption>Tabelle 4. Beispiele für intelligente Formatierung bei Transkripten mit langen Pausen</caption>
  <tr>
    <th style="width:45%; text-align:left">Gesprochene Audiodaten</th>
    <th style="text-align:left">Formatierte Ergebnisse der Transkription</th>
  </tr>
  <tr>
    <td>
      Meine Telefonnummer ist neun eins vier fünf fünf sieben drei
      drei neun zwei
    </td>
    <td>
      "Meine Telefonnummer ist 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      Meine Telefonnummer ist neun eins vier <em>&lt;pause&gt;</em> fünf fünf
      sieben drei drei neun zwei
    </td>
    <td>
      "Meine Telefonnummer ist 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## Zahlenschwärzung
{: #redaction}

Die Funktion für Zahlenschwärzung wird als Betafunktionalität bereitgestellt und ist für amerikanisches Englisch, Japanisch und Koreanisch verfügbar.
{: note}

Der Parameter `redaction` weist den Service an, Zahlenangaben in endgültigen Transkripten zu schwärzen bzw. zu maskieren. Die Funktion maskiert jede Zahl, die aus drei ober mehr aufeinanderfolgenden Ziffern besteht, indem jede Ziffer durch das Zeichen `X` ersetzt wird. Auf diese Weise sollen sensible Zahlenangaben (z. B. Kreditkartennummern) unkenntlich gemacht werden.

Die Funktion für Zahlenschwärzung ist im Service standardmäßig inaktiviert. Setzen Sie den Parameter `redaction` auf `true`, um die Zahlenschwärzung zu aktivieren. Wenn Sie die Schwärzung aktivieren, aktiviert der Service automatisch die intelligente Formatierung, auch wenn sie zuvor von Ihnen explizit inaktiviert wurde. Zur Optimierung der Sicherheit erzwingt der Service außerdem die folgenden Parameter, wenn Sie die Zahlenschwärzung aktivieren:

-   Der Service inaktiviert die Schlüsselworterkennung unabhängig davon, ob Sie Werte für die Parameter `keywords` und `keywords_threshold` angeben.
-   Der Service inaktiviert die Funktion für Zwischenergebnisse unabhängig davon, ob Sie den Parameter `interim_results` der WebSocket-Schnittstelle auf `true` setzen.
-   Der Service gib nur ein einziges, endgültiges Transkript zurück, unabhängig davon, ob Sie einen Wert für den Parameter `maximum_alternatives` angeben.

Das Design der Funktion ist der Funktion für intelligente Formatierung nachempfunden. Der Service wendet die Schwärzung nur auf das endgültige Transkript einer Erkennungsanforderung an, unmittelbar vor der Rückgabe der Ergebnisse an den Client und nach Abschluss der Textnormalisierung.

In künftigen Versionen maskiert diese Funktion möglicherweise auch Telefonnummern, Straßenadressen, Bankkontodaten, Sozialversicherungsnummern und andere sensible numerische Daten.
{: note}

### Unterschiede in den unterstützten Sprachen
{: #redactionDifferences}

Die Funktion arbeitet genau wie für Modelle für amerikanisches Englisch beschrieben, weist jedoch bei Modellen für Japanisch und Koreanisch die folgenden Unterschiede auf.

#### Japanisch
{: #redactionJapanese}

Bei der Zahlenschwärzung für Japanisch gelten die folgenden Unterschiede:

-   Neben dem Maskieren von Zeichenfolgen mit drei oder mehr aufeinanderfolgenden Ziffern werden auch Straßenadressen und Zahlen unkenntlich gemacht, selbst wenn sie weniger als drei Ziffern enthalten.
-   Ebenso maskiert die Funktion für Schwärzung Datumsangaben aus Geburtsdaten im japanischen Format. Japanische Datumsangaben werden normalerweise im lateinischen *Anno Domini*-Format angegeben, in manchen Fällen (insbesondere beim Geburtsdatum) jedoch im japanischen Format. In diesem Fall werden Jahres- und Monatsangaben maskiert, obwohl sie nur eine oder zwei Ziffern enthalten. Die Zahlenschwärzung ändert beispielsweise die folgende Zeichenfolge wie dargestellt.

    <table style="width:50%">
      <caption>Tabelle 5. Beispiel für Zahlenschwärzung bei Geburtsdaten im japanischen Format</caption>
      <tr>
        <th style="text-align:left">Ohne Schwärzung</th>
        <th style="text-align:left">Mit Schwärzung</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### Koreanisch
{: #redactionKorean}

Bei der Zahlenschwärzung für Koreanisch gelten die folgenden Unterschiede:

-   Die Funktion für intelligente Formatierung wird nicht unterstützt. Außer der Zahlenschwärzung führt der Service für Koreanisch keine weitere intelligente Formatierung durch.
-   Isolierte Ziffernzeichen werden reduziert, aber Ziffernzeichen, die möglicherweise als Bestandteile in koreanischen Ausdrücken enthalten sind, werden nicht reduziert. Beispiel: Das Zeichen <code>&#51060;</code> im folgenden Ausdruck wird nicht durch ein `X` ersetzt, da es neben dem folgenden Zeichen steht:

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    Wenn das Zeichen <code>&#51060;</code> durch ein Leerzeichen vom folgenden Zeichen getrennt ist, wird es durch ein `X` ersetzt, wie in [Ergebnisse der Zahlenschwärzung](#redactionResults) beschrieben.

### Beispiel für Zahlenschwärzung
{: #redactionExample}

Im folgenden Beispiel wird die Zahlenschwärzung mit einer Erkennungsanforderung angefordert, indem der Parameter `redaction` auf `true` gesetzt wird. Da die Anforderung die Schwärzung aktiviert, wird vom Service implizit auch die intelligente Formatierung aktiviert. Der Service inaktiviert damit die übrigen Parameter der Anforderung, d. h. sie bleiben wirkungslos. Der Service gibt ein einziges endgültiges Transkript zurück und erkennt keine Schlüsselwörter. Im folgenden Abschnitt werden die Auswirkungen der Schwärzung dargestellt.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### Ergebnisse der Zahlenschwärzung
{: #redactionResults}

Die folgende Tabelle enthält Beispiele für endgültige Transkripte mit und ohne Zahlenschwärzung für jede unterstützte Sprache.

<table style="width:90%" summary="In jeder Überschrift ist eine Sprache angegeben, gefolgt von einer einzelnen Zeile, in der dasselbe Transkript mit inaktivierter bzw. aktivierter Zahlenschwärzung angezeigt wird.">
  <caption>Tabelle 6. Beispieltranskripte für Zahlenschwärzung</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">Ohne Schwärzung</th>
    <th id="with_redaction" style="text-align:left">Mit Schwärzung</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>Amerikanisches Englisch</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>Japanisch</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>Koreanisch</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## Vulgäre Ausdrücke filtern
{: #profanity_filter}

Die Filterfunktion für vulgäre Ausdrücke ist nur für amerikanisches Englisch allgemein verfügbar.
{: note}

Der Parameter `profanity_filter` gibt an, ob der Service vulgäre Ausdrücke in den Ergebnissen zensieren soll. Standardmäßig maskiert der Service im Transkript alle vulgären Ausdrücke durch eine Reihe von Sternen. Wenn dieser Parameter auf `false` gesetzt ist, werden alle Wörter in der Ausgabe in ihrer transkribierten Form angezeigt.

Der Service maskiert vulgäre Ausdrücke in allen endgültigen Transkripten und in allen alternativen Transkripten. Außerdem werden vulgäre Ausdrücke in Ergebnissen zensiert, die Wortalternativen, Wortkonfidenz und Wortzeitmarken zugeordnet sind. Ausgenommen ist allein die Schlüsselworterkennung. Für diese Funktion gibt der Service alle Wörter wie vom Benutzer angegeben zurück, unabhängig davon, ob `profanity_filter` auf `true` gesetzt ist.

### Beispiel für die Filterfunktion für vulgäre Ausdrücke
{: #profanityFilteringExample}

Das folgende Beispiel zeigt die Ergebnisse für eine kurze Audiodatei, die unter Verwendung des Standardwerts `true` für den Parameter `profanity_filter` transkribiert wird. Außerdem legt die Anforderung für den Parameter `word_alternatives_threshold` den relativ hohen Wert `0.99` fest und setzt die Parameter `word_confidence` und `timestamps` auf `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
