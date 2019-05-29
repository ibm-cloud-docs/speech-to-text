---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# Synchrone HTTP-Schnittstelle
{: #http}

Die synchrone HTTP-Schnittstelle des {{site.data.keyword.speechtotextfull}}-Service bietet eine einzige Methode namens `POST /v1/recognize`, mit der die Spracherkennung beim Service angefordert wird. Diese Methode ist das einfachste Verfahren für die Anforderung einer Transkription. Zur Übergabe einer Spracherkennungsanforderung haben Sie zwei Möglichkeiten.
{: shortdesc}

-   Beim ersten Verfahren senden Sie im Hauptteil der Anforderung alle Audiodaten in einem einzigen Datenstrom. Die Parameter der Operation geben Sie in Form von Anforderungsheadern und Abfrageparametern an. Weitere Informationen finden Sie unter [Einfache HTTP-Anforderung ausgeben](#HTTP-basic).
-   Beim zweiten Verfahren werden die Audiodaten als mehrteilige Anforderung gesendet. Die Parameter der Anforderung geben Sie hier als Kombination aus Anforderungsheadern, Abfrageparametern und JSON-Metadaten an. Weitere Informationen finden Sie unter [Mehrteilige HTTP-Anforderung ausgeben](#HTTP-multi).

Übergeben Sie mit einer einzelnen Anforderung mindestens 100 Byte und höchstens 100 MB Audiodaten. Angaben über Audioformate und die Verwendung der Komprimierung zur Maximierung der Audiodatenmenge, die mit einer Anforderung gesendet werden kann, enthält der Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html). Informationen zu allen Methoden der HTTP-Schnittstelle finden Sie in der [API-Referenz. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}

## Einfache HTTP-Anforderung ausgeben
{: #HTTP-basic}

Die HTTP-Methode `POST /v1/recognize` ist ein einfaches Verfahren zur Transkription von Audiodaten. Sie übergeben alle Audiodaten im Hauptteil der Anforderung und geben die Parameter als Anforderungsheader und Abfrageparameter an.

Das folgende `curl` -Beispiel sendet eine Erkennungsanforderung für eine einzelne FLAC-Datei namens `audio-file.flac`. In der Anforderung ist der Abfrageparameter `model` nicht angegeben, damit das Standardsprachmodell `en-US_BroadbandModel` verwendet wird.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Im Beispiel wird für die Audiodaten die folgende Transkription zurückgegeben:

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Die Methode `POST /v1/recognize` gibt Ergebnisse erst dann zurück, nachdem alle Audiodaten für eine Anforderung verarbeitet wurden. Sie eignet sich für die Stapelverarbeitung, jedoch nicht für eine Live-Spracherkennung. Verwenden Sie zum Transkribieren von Live-Audiodaten die WebSocket-Schnittstelle.

Falls Ihre Daten aus mehreren Audiodateien bestehen, empfiehlt es sich, die Audiodaten durch das Senden mehrerer Anforderungen (1 pro Audiodatei) zu übergeben. Sie können die Anforderungen in einer Schleife mit optionaler Parallelverarbeitung übergeben, um das Leistungsverhalten zu verbessern. Sie haben auch die Möglichkeit, die mehrteilige Spracherkennung zu verwenden, um mehrere Audiodateien in einer einzigen Anforderung zu übergeben.

## Mehrteilige HTTP-Anforderung ausgeben
{: #HTTP-multi}

Die Methode `POST /v1/recognize` unterstützt ebenfalls mehrteilige Anforderungen. Hierbei übergeben Sie alle Audiodaten als mehrteilige Formulardaten. Einige Parameter geben Sie als Anforderungsheader und Abfrageparameter an; die meisten Aspekte der Transkription steuern Sie jedoch durch die Übergabe von JSON-Metadaten als Formulardaten.

Die mehrteilige Spracherkennung ist für die folgenden Anwendungsfälle vorgesehen:

-   Zur Übergabe von mehreren Audiodateien mit einer einzigen Spracherkennungsanforderung.
-   Bei Verwendung von Browsern mit inaktiviertem JavaScript. Auf Formulardaten basierende mehrteilige Anforderungen machen die Verwendung von JavaScript nicht erforderlich.
-   Bei einer Größe der Parameter einer Erkennungsanforderung von mehr als 8 KB, also dem durch die meisten HTTP-Server und -Proxys vorgegebenen Grenzwert. Beispielsweise kann die Erkennung einer großen Anzahl von Schlüsselwörtern die Größe einer Anforderung über diesen Grenzwert hinaus erhöhen. Mehrteilige Anforderungen verwenden Formulardaten, um diese Einschränkung zu vermeiden.

In den folgenden Abschnitten sind die Parameter beschrieben, die Sie für mehrteilige Anforderung verwenden. Außerdem ist eine Beispielanforderung dargestellt.

### Parameter für mehrteilige Anforderungen
{: #multipartParameters}

Für die mehrteilige Spracherkennung geben Sie die folgenden Parameter als Anforderungsheader, Abfrageparameter und Formulardaten an.

<table summary="Jede Zeile der Tabelle beschreibt die Verwendung eines möglichen Parameters für eine mehrteilige Erkennungsanforderung.">
  <caption>Tabelle 1. Parameter für mehrteilige Anforderungen</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">Parameter</th>
    <th id="description" style="text-align:center; width:80%">Beschreibung</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>Formulardaten,</em>
      <br/><em>Objekt</em>
    </td>
    <td>
      <em>Erforderlich.</em> Ein JSON-Objekt, das die Transkriptionsparameter für die Anforderung bereitstellt. Das Objekt muss der erste Teil der Formulardaten sein. Die Informationen beschreiben die Audiodaten in den nachfolgenden Teilen der Formulardaten. Weitere Informationen finden Sie unter [JSON-Metadaten für mehrteilige Anforderungen](#multipartJSON).
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>Formulardaten,</em>
      <br/><em>Datei</em>
    </td>
    <td>
      <em>Erforderlich.</em> Eine oder mehrere Audiodateien als weitere Formulardaten für die Anforderung. Alle Audiodateien müssen dasselbe Format besitzen. Beziehen Sie in den Befehl `curl` für jede Datei der Anforderung eine separate Option <code>--form</code> ein.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Header,</em>
      <br/><em>Zeichenfolge</em>
    </td>
    <td>
      <em>Erforderlich.</em> Geben Sie mit `multipart/form-data` an, wie Daten an die Methode übergeben werden. Den Inhaltstyp der Audiodaten geben Sie mit dem JSON-Parameter       `part_content_type` an.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Header,</em>
      <br/><em>Zeichenfolge</em>
    </td>
    <td>
      <em>Optional.</em> Geben Sie `chunked` an, um die Audiodaten an den Service zu streamen. Lassen Sie den Parameter weg, wenn Sie alle Audiodaten mit einer einzigen Anforderung senden.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Abfrage,</em>
      <br/><em>Zeichenfolge</em>
    </td>
    <td>
      <em>Optional.</em> Die ID des Modells, das bei der Anforderung verwendet werden soll. Der Standardwert ist `en-US_BroadbandModel`.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Abfrage,</em>
      <br/><em>Zeichenfolge</em>
    </td>
    <td>
      <em>Optional.</em> Die GUID eines angepassten Sprachmodells, das bei der Anforderung verwendet werden soll.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Abfrage,</em>
      <br/><em>Zeichenfolge</em>
    </td>
    <td>
      <em>Optional.</em> Die GUID eines angepassten Akustikmodells, das bei der Anforderung verwendet werden soll.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Abfrage,</em>
      <br/><em>Zeichenfolge</em>
    </td>
    <td>
      <em>Optional.</em> Die Version eines Basismodells, das bei der Anforderung verwendet werden soll.
    </td>
  </tr>
</table>

Weitere Informationen zu den Abfrageparametern enthält der Abschnitt [Parameterübersicht](/docs/services/speech-to-text/summary.html).

### JSON-Metadaten für mehrteilige Anforderungen
{: #multipartJSON}

Die JSON-Metadaten, die Sie mit einer mehrteiligen Anforderung übergeben, können die folgenden Felder enthalten:

-   `part_content_type` (Zeichenfolge)
-   `data_parts_count` (Ganzzahl)
-   `customization_weight` (Zahl)
-   `inactivity_timeout` (Ganzzahl)
-   `keywords` (Zeichenfolge[])
-   `keywords_threshold` (Zahl)
-   `max_alternatives` (Ganzzahl)
-   `word_alternatives_threshold` (Zahl)
-   `word_confidence` (Boolescher Wert)
-   `timestamps` (Boolescher Wert)
-   `profanity_filter` (Boolescher Wert)
-   `smart_formatting` (Boolescher Wert)
-   `speaker_labels` (Boolescher Wert)
-   `grammar_name` (Zeichenfolge)
-   `redaction` (Boolescher Wert)

Nur die beiden folgenden Parameter sind speziell für mehrteilige Anforderungen vorgesehen:

-   Das Feld `part_content_type` ist bei den meisten Audioformaten *optional*. Für die Formate `audio/alaw`, `audio/basic`, `audio/l16` und `audio/mulaw` ist es erforderlich. Es gibt das Format der Audiodaten in den anschließenden Teilen der Anforderung an. Alle Audiodateien müssen dasselbe Format besitzen.
-   Das Feld `data_parts_count` ist bei allen Anforderungen *optional*. Es gibt die Anzahl der Audiodateien an, die mit der Anforderung gesendet werden. Der Service wendet die Datenstromendeerkennung auf den letzten (und möglicherweise einzigen) Datenteil an. Falls Sie den Parameter weglassen, ermittelt der Service die Anzahl der Teile aus der Anforderung.

Alle anderen Parameter der Metadaten sind optional. Eine Beschreibung aller verfügbaren Parameter finden Sie in der [Parameterübersicht](/docs/services/speech-to-text/summary.html).

### Beispiel für mehrteilige Anforderung

Das folgende `curl`-Beispiel zeigt, wie eine mehrteilige Erkennungsanforderung mit der Methode `POST /v1/recognition` übergeben wird. Die Anforderung übergibt zwei Audiodateien namens **audio-file1.flac** und **audio-file2.flac**. Der Parameter `metadata` stellt die meisten Parameter für die Anforderung zur Verfügung. Die Parameter `upload` stellen die Audiodateien zur Verfügung.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Im Beispiel wird für die Audiodaten die folgende Transkription zurückgegeben. Der Service gibt die Ergebnisse für die beiden Dateien in der Reihenfolge zurück, in der sie gesendet wurden. (In der Beispielausgabe sind die Ergebnisse für die zweite Datei abgekürzt.)

```javascript
{
   "results": [
      {
         "word_alternatives": [
            {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
               "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
            {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
               "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
               "end_time": 6.04
            }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
