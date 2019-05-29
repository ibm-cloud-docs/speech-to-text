---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# Sprachen und Modelle
{: #models}

Der {{site.data.keyword.speechtotextfull}}-Service unterstützt die Spracherkennung in vielen Sprachen. Für alle Schnittstellen können Sie den Parameter `model` verwenden, um das Modell für eine Spracherkennungsanforderung anzugeben. Das Modell gibt an, in welcher Sprache das Audiomaterial gesprochen wird und mit welcher Abtastfrequenz es erfasst wird.
{: shortdesc}

## Unterstützte Sprachmodelle
{: #modelsList}

In den meisten Sprachen unterstützt der Service sowohl Breitband- als auch Schmalbandmodelle:

-   *Breitbandmodelle* werden für Audiodaten mit einer Abtastfrequenz größer-gleich 16 kHz verwendet. Verwenden Sie Breitbandmodelle für reaktionsfähige echtzeitorientierte Anwendungen (z. B. Anwendungen für Live-Aufzeichnungen).
-   *Schmalbandmodelle* werden für Audiodaten mit einer Abtastfrequenz von 8 kHz verwendet. Verwenden Sie Schmalbandmodelle bei der Offline-Decodierung von Telefongesprächen (der typische Anwendungszweck für diese Abtastfrequenz).

Das Auswählen des richtigen Modells für Ihre Anwendung ist von großer Bedeutung. Verwenden Sie das Modell mit derselben Abtastfrequenz (und Sprache) wie Ihre Audiodaten. Der Service passt die Abtastfrequenz Ihrer Audiodaten automatisch an das Modell an, das Sie angeben. Weitere Informationen finden Sie im Abschnitt [Abtastfrequenz](/docs/services/speech-to-text/audio-formats.html#samplingRate).

Um die beste Erkennungsgenauigkeit zu erzielen, müssen Sie auch den Frequenzumfang Ihrer Audiodaten berücksichtigen. Weitere Informationen finden Sie im Abschnitt [Tonfrequenz](/docs/services/speech-to-text/audio-formats.html#frequency).
{: tip}

In Tabelle 1 sind die unterstützten Modelle für jede Sprache aufgelistet. Wenn Sie den Parameter `model` in einer Anforderung nicht angeben, verwendet der Service standardmäßig das Breitbandmodell für amerikanisches Englisch (`en-US_BroadbandModel`).

<table>
  <caption>Tabelle 1. Unterstützte Sprachmodelle</caption>
  <tr>
    <th style="text-align:left">Sprache</th>
    <th style="text-align:center">Breitbandmodell</th>
    <th style="text-align:center">Schmalbandmodell</th>
  </tr>
  <tr>
    <td>Brasilianisches Portugiesisch</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Französisch</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Deutsch</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Japanisch</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Koreanisch</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Chinesisch (Mandarin)</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Modernes Hocharabisch</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">Nicht unterstützt</td>
  </tr>
  <tr>
    <td>Spanisch</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Britisches Englisch</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Amerikanisches Englisch</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
</table>

### Kurzformmodell für amerikanisches Englisch
{: #modelsShortform}

Das Kurzformmodell für amerikanisches Englisch (`en-US_ShortForm_NarrowbandModel`) kann die Spracherkennung in Lösungen für Interactive-Voice-Response (IVR) und automatisierte Kundenunterstützung verbessern. Das Training für das Kurzformmodell ist darauf abgestimmt, kurze Äußerungen zu erkennen, die in Umgebungen für Kundenunterstützung (wie automatisierte oder bemannte Kundendienst-Call-Center) häufig vorkommen. Das Modell wurde beispielsweise für präzise Äußerungen wie Ziffern, Wörter und Namen aus einzelnen Buchstaben und Ja/Nein-Antworten optimiert. Die Verwendung einer Grammatik in Kombination mit dem Kurzformmodell kann die Ergebnisse der Erkennung noch weiter verbessern.

Wie bei allen Modellen können Umgebungen mit hohem Rauschanteil die Erkennungsergebnisse beeinträchtigen. Beispiel: Hintergrundgeräusche in Flughäfen, fahrenden Fahrzeugen und Konferenzräumen sowie mehrere beteiligte Sprecher können die Transkriptionsgenauigkeit verringern. Die Audioausgabe von Lautsprechertelefonen kann durch Rückkopplungen (Echo) ebenfalls die Erkennungsgenauigkeit beeinträchtigen. Die Verwendung eines angepassten Akustikmodells in Kombination mit dem Kurzformmodell kann solche Effekte eindämmen.

-   Weitere Informationen zur Anpassung von Sprach- und Akustikmodellen finden Sie im Abschnitt [Die Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).
-   Weitere Informationen zu Grammatiken finden Sie im Abschnitt [Grammatiken mit angepassten Sprachmodellen verwenden](/docs/services/speech-to-text/grammar.html).

### Beispiel für Sprachmodell
{: #modelsExample}

Im folgenden Beispiel für eine HTTP-Anforderung wird das Modell `en-US-NarrowbandModel` für die Spracherkennung verwendet:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Modelle auflisten
{: #listModels}

Die HTTP-Schnittstelle stellt zwei Methoden zum Auflisten von Informationen zu den unterstützten Modellen bereit:

-   Die Methode `GET /v1/models` listet Informationen zu allen verfügbaren Modellen auf.
-   Die Methode `GET /v1/models/{model_id}` listet Informationen zu einem angegebenen Modell auf.

Beide Methoden geben die folgenden Informationen zu einem Modell zurück:

-   `name` ist der Name des Modells, das Sie in einer Anforderung verwenden.
-   `language` ist die Sprachenkennung für das Modell (z. B. `en-US`).
-   `rate` gibt die Abtastfrequenz (minimale zulässige Abtastrate für Audiodaten) in Hertz an, die von dem Modell verwendet wird.
-   `url` ist der URI für das Modell.
-   `description` stellt eine kurze Beschreibung des Modells bereit.
-   `supported_features` beschreibt die zusätzlichen Funktionen, die das Modell für den Service bereitstellt.
    -   `custom_language_model` ist ein boolescher Wert, der angibt, ob Sie angepasste Sprachmodelle erstellen können, die auf dem Modell basieren.
    -   `speaker_labels` gibt an, ob der Parameter `speaker_labels` mit dem Modell verwendet werden kann.

### Beispiele für Anforderungen und Antworten
{: #listExample}

Im folgenden Beispiel werden alle Modelle aufgelistet, die vom Service unterstützt werden:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Breitbandmodell für brasilianisches Portugiesisch."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Breitbandmodell für Koreanisch."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "Breitbandmodell für Französisch."
    },
    . . .
  ]
}
```
{: codeblock}

Das folgende Beispiel zeigt Informationen zum Breitbandmodell für amerikanisches Englisch an. Das Modell bietet Unterstützung für die Sprachmodellanpassung und für Sprecherbezeichnungen.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "Breitbandmodell für amerikanisches Englisch."
}
```
{: codeblock}
