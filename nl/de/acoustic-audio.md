---

copyright:
  years: 2019
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

# Audioressourcen verwalten
{: #manageAudio}

Die Anpassungsschnittstelle bietet die Methode `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`, mit der Sie eine Audioressource zu einem angepassten Akustikmodell hinzufügen können. (Weitere Informationen enthält der Abschnitt [Audiodaten zum angepassten Akustikmodell hinzufügen](/docs/services/speech-to-text/acoustic-create.html#addAudio)). Die Schnittstelle enthält außerdem die hier aufgeführten Methoden für das Auflisten und Löschen der Audioressourcen für ein angepasstes Akustikmodell.
{: shortdesc}

## Audioressourcen für ein angepasstes Akustikmodell auflisten
{: #listAudio}

Die Anpassungsschnittstelle bietet zwei Methoden für das Auflisten von Informationen zu den Audioressourcen eines angepassten Akustikmodells:

-   Die Methode `GET /v1/acoustic_customizations/{customization_id}/audio` listet Informationen zu allen Audioressourcen auf, die zu einem angepassten Modell hinzugefügt wurden.
-   Die Methode `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` listet Informationen zu einer angegebenen Audioressource für ein angepasstes Modell auf.

Beide Methoden geben im Feld `name` den Namen der Audioressource sowie die folgenden zusätzlichen Informationen zurück:

-   `duration`: Die Länge der Audioressource in Sekunden. Bei einer Archivdatei stellt der Wert die summierte Dauer aller Audiodateien dar, die im Archiv enthalten sind.
-   `details`: Gibt den Typ der Ressource an, also `audio` oder `archive`. (Der Typ lautet `undetermined` (= unbestimmt), falls der Service die Ressource nicht validieren kann, weil der Benutzer möglicherweise versehentlich eine Datei übergeben hat, die keine Audiodaten enthält.) Für eine Audiodatei enthalten die Details den Codec (Feld `codec`) und die Frequenz (Feld `frequency`) der Audiodaten. Für eine Archivdatei enthalten Sie den Komprimierungstyp (Feld `compression`).

Beim Auflisten aller Audioressourcen für ein Modell wird außerdem mit dem Wert für `total_minutes_of_audio` die Summe aller gültigen Audioressourcen für das Modell zurückgegeben. Anhand dieses Wertes können Sie ermitteln, ob das angepasste Modell ausreichend oder zu viele Audiodaten für den Beginn des Trainings enthält.

Mit den Methoden wird auch der Status der Audiodaten aufgelistet. Der Status ist von Bedeutung, um die Analyse von Audiodaten durch den Service als Reaktion auf eine Anforderung zu überprüfen, die Dateien zu einem angepassten Modell hinzuzufügen:

-   `ok`: Der Service hat die Audiodaten erfolgreich analysiert. Die Daten können für das Training des angepassten Modells verwendet werden.
-   `being_processed`: Der Service ist gegenwärtig noch mit der Analyse der Audiodaten beschäftigt. Er kann Anforderungen für das Hinzufügen neuer Audiodaten oder das Trainieren des angepassten Modells erst nach Abschluss der Analyse akzeptieren.
-   `invalid`: Die Audiodaten sind zum Trainieren des Modells nicht gültig (möglicherweise weisen sie ein falsches Format oder eine nicht korrekte Abtastfrequenz auf oder sind beschädigt).

### Beispielanforderung: Alle Audioressourcen auflisten
{: #listExample-audio}

Im folgenden Beispiel werden alle Audioressourcen für das angepasste Akustikmodell mit der angegebenen Anpassungs-ID aufgelistet. Für das Akustikmodell gibt es drei Audioressourcen. Der Service hat die Ressourcen `audio1` und `audio2` erfolgreich analysiert; die Ressource `audio3` wird gegenwärtig noch analysiert.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### Beispielanforderung: Informationen für eine Audioressource abrufen
{: #getExampleAudio}

Im folgenden Beispiel werden Informationen zur Audioressource namens `audio1`zurückgegeben. Die Ressource ist 131 Sekunden lang und mit dem Codec `pcm_s16le` codiert. Sie wurde erfolgreich zum Modell hinzugefügt.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

```javascript
{
  "duration": 131,
  "name": "audio1",
  "details": {
    "codec": "pcm_s16le",
    "type": "audio",
    "frequency": 22050
  }
  "status": "ok"
}
```
{: codeblock}

### Beispielanforderung: Informationen für eine Archivressource abrufen
{: #getExampleArchive}

Im folgenden Beispiel werden Informationen zur Archivressource namens `audio2` zurückgegeben. Die Ressource ist eine Datei **.zip**, die Audiodaten mit einer Länge von mehr als 9 Minuten enthält. Auch sie wurde erfolgreich zum Modell hinzugefügt. Wie das Beispiel zeigt, stellt die Abfrage von Informationen zu einer Archivtypressource auch Informationen zu den darin enthaltenen Dateien bereit.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

```javascript
{
  "container": {
    "duration": 556,
    "name": "audio2",
    "details": {
      "type": "archive",
      "compression": "zip"
    },
    "status": "ok"
  },
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## Audioressourcen aus einem angepassten Akustikmodell löschen
{: #deleteAudio}

Mit der Methode `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` können Sie eine vorhandene Audioressource aus einem angepassten Akustikmodell entfernen. Wenn Sie eine Archivressource löschen, entfernt der Service das gesamte Dateiarchiv. Das Löschen einzelner Dateien aus einer Archivressource ist in der aktuellen Schnittstelle nicht zulässig.

Das Entfernen einer Audioressource wirkt sich erst dann auf das angepasste Modell aus, wenn Sie es unter Verwendung der Methode `POST /v1/acoustic_customizations/{customization_id}/train` mit seinen aktualisierten Daten trainieren. Falls Sie das Modell mit der Ressource erfolgreich trainiert hatten, werden die vorhandenen Audiodaten weiterhin für die Spracherkennung verwendet, bis Sie das Modell erneut trainieren.

### Beispielanforderung
{: #deleteExample-audio}

Mit der folgenden Methode wird die Audioressource namens `audio3` aus dem angepassten Modell mit der angegebenen Anpassungs-ID gelöscht:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
