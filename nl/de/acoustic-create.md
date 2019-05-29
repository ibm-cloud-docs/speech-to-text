---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# Angepasstes Akustikmodell erstellen
{: #acoustic}

Mit den hier beschriebenen Schritten können Sie ein angepasstes Akustikmodell für den {{site.data.keyword.speechtotextshort}}-Service erstellen.
{: shortdesc}

1.  [Erstellen Sie ein angepasstes Akustikmodell](#createModel-acoustic). Sie können mehrere angepasste Modelle für dieselben oder unterschiedlichen Fachgebiete bzw. Umgebungen erstellen; der Prozess ist für jedes Modell, das Sie erstellen, identisch. Bei einer Erkennungsanforderung können Sie jedoch jeweils nur ein einziges angepasstes Akustikmodell angeben.
1.  [Fügen Sie Audiodaten zum angepassten Akustikmodell hinzu](#addAudio). Für die akustische Modellierung akzeptiert der Service dieselben Audiodateiformate wie für die Spracherkennung. Er akzeptiert ebenfalls Archivdateien, die mehrere Audiodateien enthalten. Archivdateien sind das bevorzugte Verfahren für das Hinzufügen von Audioressourcen. Sie können die Methode wiederholen, um weitere Audio- oder Archivdateien zu einem angepassten Modell hinzuzufügen.
1.  [Trainieren Sie das angepasste Akustikmodell](#trainModel-acoustic). Nachdem Sie Audioressourcen zum angepassten Modell hinzugefügt haben, müssen Sie das Modell trainieren. Das Training bereitet das angepasste Akustikmodell für die Verwendung bei der Spracherkennung vor. Das Training kann beträchtliche Zeit in Anspruch nehmen; die Länge des Trainings ist vom Umfang der Audiodaten abhängig, die das Modell enthält.

    Während des Trainings Ihres angepassten Akustikmodells können Sie zur weiteren Unterstützung ein angepasstes Sprachmodell angeben. Ein angepasstes Sprachmodell, das Transkriptionen Ihrer Audiodateien oder vokabularexterne Wörter aus dem Fachgebiet Ihrer Audiodateien enthält, kann die Qualität des angepassten Akustikmodells verbessern. Weitere Informationen finden Sie unter [Angepasstes Akustikmodell mit einem angepassten Sprachmodell trainieren](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).
1.  Nachdem Sie Ihr angepasstes Modell trainiert haben, können Sie es bei Erkennungsanforderungen verwenden. Falls die zur Transkription übergebenen Audiodaten eine ähnliche akustische Qualität wie die Audiodaten des angepassten Modells aufweisen, bilden die Ergebnisse das erweiterte Erkenntnisvermögen des Service ab. Weitere Informationen finden Sie unter [Angepasstes Akustikmodell verwenden](/docs/services/speech-to-text/acoustic-use.html).

    In einer Erkennungsanforderung können Sie sowohl ein angepasstes Akustikmodell als auch ein angepasstes Sprachmodell übergeben, um die Genauigkeit bei der Erkennung weiter zu verbessern. Zusätzliche Angaben enthält der Abschnitt [Angepasste Sprachmodelle und angepasste Akustikmodelle während der Spracherkennung verwenden](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

Die Schritte für die Erstellung eines angepassten Akustikmodells sind iterativ. Das Hinzufügen oder Löschen von Audiodaten und das Trainieren oder erneute Trainieren eines Modells können Sie so häufig wie benötigt ausführen. Sie müssen ein Modell erneut trainieren, damit Änderungen an seinen Audiodaten wirksam werden. Beim erneuten Training eines Modells werden alle Audiodaten (und nicht nur die neuen Daten) für das Training verwendet. Die Trainingsdauer entspricht also dem Gesamtumfang der Audiodaten, die im Modell enthalten sind.

Die Anpassung von Akustikmodellen ist als Betafunktionalität für alle Sprachen verfügbar. Weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Angepasstes Akustikmodell erstellen
{: #createModel-acoustic}

Zum Erstellen eines neuen angepassten Akustikmodells verwenden Sie die Methode `POST /v1/acoustic_customizations`. Sie können beliebig viele angepasste Akustikmodelle erstellen, bei einer Spracherkennungsanforderung jedoch jeweils immer nur ein einziges angepasstes Akustikmodell verwenden. Die Methode akzeptiert ein JSON-Objekt, das die Attribute des neuen angepassten Modells als Hauptteil der Anforderung definiert.

<table>
  <caption>Tabelle 1. Attribute eines neuen angepassten Akustikmodells</caption>
  <tr>
    <th style="width:20%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Datentyp</th>
    <th style="text-align:left">Beschreibung</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Erforderlich</em></td>
    <td style="text-align:center">Zeichenfolge</td>
    <td>
      Ein benutzerdefinierter Name für das neue angepasste Akustikmodell. Verwenden Sie einen Namen, der die akustische Umgebung des angepassten Modells beschreibt, z. B. <code>Angepasstes Modell für Mobilgerät</code> oder <code>Angepasstes Modell für lautes Fahrzeug</code>. Verwenden Sie einen Namen, der unter allen Akustikmodellen, deren Eigner Sie sind, eindeutig ist. Verwenden Sie einen Namen in der Sprache des angepassten Modells.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Erforderlich</em></td>
    <td style="text-align:center">Zeichenfolge</td>
    <td>
      Der Name des Basismodells, das durch das neue Modell angepasst werden soll. Sie müssen den Namen eines Modells verwenden, der von der Methode <code>GET /v1/models</code> zurückgegeben wird. Das neue angepasste Modell kann nur in Verbindung mit dem Basismodell verwendet werden, dessen Anpassung es darstellt.
</td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Optional</em></td>
    <td style="text-align:center">Zeichenfolge</td>
    <td>
      Eine Beschreibung des neuen Modells. Verwenden Sie eine Beschreibung in der Sprache des angepassten Modells.
    </td>
  </tr>
</table>

Im folgenden Beispiel wird ein neues Akustikmodell namens `Example acoustic model` erstellt. Das Modell wird für das Basismodell `en-US_BroadbandModel` erstellt und enthält die Beschreibung `Example custom acoustic model`. Der Header `Content-Type` gibt an, dass JSON-Daten an die Methode übergeben werden.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

Das Beispiel gibt die Anpassungs-ID des neuen Modells zurück. Jedes angepasste Modell wird durch eine eindeutige Anpassungs-ID in Form einer global eindeutigen ID (GUID) gekennzeichnet. Die GUID eines angepassten Modells geben Sie im Parameter `customization_id` von Aufrufen an, die dem Modell zugeordnet sind.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

Eigner des neuen angepassten Modells ist die Serviceinstanz, deren Berechtigungsnachweise für die Erstellung verwendet werden. Weitere Informationen hierzu finden Sie im Abschnitt [Eigentumsrecht an angepassten Modellen](/docs/services/speech-to-text/custom.html#customOwner).

## Audiodaten zum angepassten Akustikmodell hinzufügen
{: #addAudio}

Nachdem Sie Ihr angepasstes Akustikmodell erstellt haben, müssen Sie im nächsten Schritt Audioressourcen zum Modell hinzufügen. Zum Hinzufügen einer Audioressource zu einem angepassten Modell verwenden Sie die Methode `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`. Sie können Folgendes hinzufügen:

-   Einzelne Audiodatei in einem beliebigen Format, das für die Spracherkennung unterstützt wird (siehe [Audioformate](/docs/services/speech-to-text/audio-formats.html)).
-   Archivdatei (Erweiterung **.zip** oder **.tar.gz**), die mehrere Audiodateien enthält. Das Zusammenstellen mehrerer Audiodateien in einer einzigen Archivdatei und Laden dieser einzigen Datei ist wesentlich effizienter als das Hinzufügen einzelner Audiodateien.

Sie übergeben die Audioressource als Hauptteil der Anforderung und weisen der Ressource einen Namen (Wert für `audio_name`) zu. Weitere Informationen enthält der Abschnitt [Mit Audioressourcen arbeiten](/docs/services/speech-to-text/acoustic-resource.html).

Die folgenden Beispiele zeigen das Hinzufügen sowohl von einzelnen Audioressourcen als auch von Archivdateien:

-   Das folgende Beispiel fügt eine einzelne Audioressource zum angepassten Akustikmodell mit der angegebenen `customization_id` hinzu. Der Header `Content-Type` gibt den Typ der Audiodaten mit `audio/wav` an. Die Audiodatei namens **audio1.wav** wird als Hauptteil der Anforderung übergeben; die Ressource erhält den Namen `audio1`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   Im folgenden Beispiel wird eine Archivdateiressource zum angegebenen angepassten Akustikmodell hinzugefügt. Der Header `Content-Type` gibt den Typ des Archivs mit `application/zip` an. Der Header `Contained-Contented-Type` gibt an, dass alle Dateien, die im Archiv enthalten sind, das Format `audio/l16` aufweisen und mit einer Frequenz von 16 kHz abgetastet werden. Die Archivdatei namens **audio2.zip** wird als Hauptteil der Anforderung übergeben; die Ressource erhält den Namen `audio2`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

Die Methode akzeptiert auch den optionalen Abfrageparameter `allow_overwrite`, um eine vorhandene Audioressource für ein angepasstes Modell zu überschreiben. Verwenden Sie den Parameter, wenn Sie eine Audioressource aktualisieren müssen, nachdem Sie sie zu einem Modell hinzugefügt haben.

Die Methode wird asynchron ausgeführt. Je nach zeitlicher Länge der Audiodaten kann ihr Abschluss mehrere Sekunden dauern. Bei einer Archivdatei ist die Länge der Operation von der Dauer der Audiodateien abhängig. Im Abschnitt [Anforderung zum Hinzufügen von Audiodaten überwachen](#monitorAudio) erfahren Sie genauer, wie Sie den Status einer Anforderung zum Hinzufügen einer Audioressource überprüfen können.

Sie können beliebig viele Audioressourcen zu einem angepassten Modell hinzufügen, indem Sie die Methode für jede Audio- oder Archivdatei separat aufrufen. Eine Audioressource muss vollständig hinzugefügt worden sein, bevor Sie eine weitere Ressource hinzufügen können. Bevor Sie ein angepasstes Modell trainieren können, müssen Sie Audiodaten mit einer Länge von mindestens 10 Minuten und höchstens 200 Stunden zum Modell hinzufügen, wobei sich die Dauer ausschließlich auf Sprache bezieht und Schweigen nicht berücksichtigt. Keine Audio- oder Archivressource kann größer als 100 MB sein. Weitere Informationen finden Sie im Abschnitt [Richtlinien für das Hinzufügen von Audioressourcen](/docs/services/speech-to-text/acoustic-resource.html#audioGuidelines).

### Anforderung zum Hinzufügen von Audiodaten überwachen
{: #monitorAudio}

Der Service gibt den Antwortcode 201 zurück, wenn die Audiodaten gültig sind. Anschließend analysiert er den Inhalt der Audiodatei(en) im asynchronen Modus und extrahiert automatisch Informationen zu den Audiodaten wie Länge, Abtastfrequenz und Codierung. Anforderungen zum Hinzufügen weiterer Audiodaten zu einem angepassten Akustikmodell oder zum Trainieren des Modells können Sie erst übergeben, wenn der Service die Analyse aller Audiodateien für die aktuelle Anforderung abgeschlossen hat.

Mit der Methode `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` können Sie den Status der Audiodaten abfragen und so den Status der Anforderung ermitteln. Die Methode akzeptiert die GUID des angepassten Modells und den Namen der Audioressource. Ihre Antwort enthält für die Ressource das Feld `status` mit einem der folgenden Werte:

-   `ok`: Die Audiodaten sind annehmbar und die Analyse ist abgeschlossen.
-   `being_processed`: Der Service ist gegenwärtig noch mit der Analyse der Audiodaten beschäftigt.
-   `invalid`: Die Audiodatei ist für die Verarbeitung nicht geeignet. Möglicherweise hat sie ein falsches Format, eine falsche Abtastfrequenz oder ist keine Audiodatei. Falls bei einer Archivdatei eine der enthaltenen Audiodateien ungültig ist, ist das gesamte Archiv ungültig.

Der Inhalt der Antwort und die Position des Feldes `status` sind vom Typ der Ressource (also Audiodatei oder Archiv) abhängig.

-   Bei einer *Audiodateiressource* befindet sich das Feld `status` im Objekt der höchsten Ebene (`AudioListing`).

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{anpassungs_id}/audio/audio1"
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

-   Bei einer *Archivdateiressource* befindet sich das Feld `status` im Objekt der zweiten Ebene (`AudioResource`), das im Feld `container` verschachtelt ist.

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
      . . .
    ```
    {: codeblock}

Mithilfe einer Schleife können Sie den Status der Audioressource alle paar Sekunden prüfen, bis er sich in `ok` ändert. Weitere Informationen zu anderen Feldern, die von der Methode zurückgegeben werden, finden Sie im Abschnitt [Audioressourcen für ein angepasstes Akustikmodell auflisten](/docs/services/speech-to-text/acoustic-audio.html#listAudio).

## Angepasstes Akustikmodell trainieren
{: #trainModel-acoustic}

Nachdem Sie ein angepasstes Akustikmodell mit Audioressourcen gefüllt haben, müssen Sie das Modell mit den neuen Daten trainieren. Das Training bereitet ein angepasstes Modell für die Verwendung bei der Spracherkennung vor. Ein Modell kann erst dann für Erkennungsanforderungen verwendet werden, wenn Sie es mit den neuen Daten trainiert haben. Auch Aktualisierungen eines angepassten Modells, die in Form von neuen oder geänderten Audioressourcen erfolgen, werden erst dann abgebildet, wenn Sie das Modell mit den Änderungen trainiert haben.

Zum Trainieren eines angepassten Modells verwenden Sie die Methode `POST /v1/acoustic_customizations/{customization_id}/train`. An die Methode übergeben Sie wie im folgenden Beispiel die Anpassungs-ID des Modells, das Sie trainieren wollen.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

Die Methode akzeptiert auch den optionalen Abfrageparameter `custom_language_model_id`, mit dem Sie ein separat erstelltes angepasstes Sprachmodell für die Verwendung während des Trainings angeben können. Sie können beim Training ein angepasstes Sprachmodell einbeziehen, das Transkriptionen Ihrer Audiodateien oder Korpora bzw. vokabularexterne Wörter enthält, die für den Inhalt der Audiodateien relevant sind. Die beiden angepassten Modelle müssen auf derselben Version desselben Basismodells basieren, damit das Training erfolgreich verläuft. Weitere Informationen finden Sie unter [Angepasstes Akustikmodell mit einem angepassten Sprachmodell trainieren](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).


Die Methode für das Training wird asynchron ausgeführt. Das Training kann abhängig von der aktuellen Auslastung des Service und vom Umfang der Audiodaten, die das angepasste Akustikmodell enthält, Minuten oder auch Stunden dauern. Als Faustregel kann davon ausgegangen werden, dass das Training eines Akustikmodells ungefähr zwei bis vier Mal so lang wie seine Audiodaten dauert. Die Dauer ist von dem trainierten Modell und der Spezifik der Audiodaten abhängig, also beispielsweise davon, ob die Audiodaten klar oder verrauscht sind. So kann das Training eines Modells, das Audiodaten mit einer Länge von 2 Stunden enthält, zum Beispiel zwischen 4 und 8 Stunden dauern. Im Abschnitt [Anforderung zum Trainieren eines Modells überwachen](#monitorTraining-acoustic) erfahren Sie genauer, wie Sie den Status einer Trainingsoperation überprüfen können.

### Anforderung zum Training eines Modells überwachen
{: #monitorTraining-acoustic}

Der Service gibt den Antwortcode 200 zurück, wenn der Trainingsprozess erfolgreich gestartet wurde. Der Service kann nachfolgende Trainingsanforderungen oder Anforderungen zum Hinzufügen weiterer Audioressourcen erst akzeptieren, wenn die vorhandene Anforderung abgeschlossen ist.

Mit der Methode `GET /v1/acoustic_customizations/{customization_id}` können Sie den Status des Modells abfragen und so den Status einer Trainingsanforderung ermitteln. Die Methode akzeptiert wie im folgenden Beispiel die Anpassungs-ID des Akustikmodells und gibt seinen Status zurück:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

Die Antwort enthält die Felder `status` und `progress`, in denen der aktuelle Status des Modells gemeldet wird. Die Bedeutung des Feldes `progress` hängt vom Status des Modells ab. Das Feld `status` kann einen der folgenden Werte enthalten:

-   `pending`: Das Modell wurde erstellt, aber es wird entweder auf das Hinzufügen von Trainingsdaten oder auf den Abschluss der Analyse der hinzugefügten Daten durch den Service gewartet. Das Feld `progress` hat den Wert `0`.
-   `ready`: Das Modell ist für das Training bereit. Das Feld `progress` hat den Wert `0`.
-   `training`: Das Modell wird gegenwärtig trainiert. Der Wert im Feld `progress` ändert sich von `0` in `100`, wenn das Training abgeschlossen ist.<!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available`: Das Modell wurde trainiert und kann verwendet werden. Das Feld `progress` hat den Wert `100`.
-   `upgrading`: Für das Modell wird gegenwärtig ein Upgrade durchgeführt. Das Feld `progress` hat den Wert `0`.
-   `failed`: Das Training des Modells ist fehlgeschlagen. Das Feld `progress` hat den Wert `0`.

Mithilfe einer Schleife können Sie den Status des Trainings einmal pro Minute prüfen, bis das Modell den Status `available` aufweist. Weitere Informationen zu anderen Feldern, die von der Methode zurückgegeben werden, finden Sie im Abschnitt [Angepasste Akustikmodelle auflisten](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic).

### Fehler beim Training
{: #failedTraining-acoustic}

Das Training eines angepassten Akustikmodells kann nicht gestartet werden, wenn der Service eine andere Anforderung für das angepasste Modell verarbeitet. Bei der kollidierenden Anforderung könnte es sich um eine andere Trainingsanforderung oder um eine Anforderung zum Hinzufügen von Audioressourcen zum Modell handeln. Der Start des Trainings kann außerdem aus den folgenden Gründen fehlschlagen:

-   Das angepasste Modell enthält Audiodaten mit einer Länge von weniger als 10 Minuten.
-   Das angepasste Modell enthält Audiodaten mit einer Länge von mehr als 200 Minuten.
-   Eine oder mehrere Audioressourcen des angepassten Modells sind ungültig.
-   Sie haben ein nicht kompatibles angepasstes Sprachmodell mit dem Abfrageparameter `custom_language_model_id` übergeben. Die beiden angepassten Modelle müssen auf derselben Version desselben Basismodells basieren.

Falls der Status für das Training eines angepassten Modells `failed` lautet, können Sie mit den Methoden `GET /v1/acoustic_customizations/{customization_id}/audio` und `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` die Audioressourcen des Modells untersuchen und gegebenenfalls festgestellte Probleme beheben. Weitere Informationen enthält der Abschnitt [Audioressourcen für ein angepasstes Akustikmodell auflisten](/docs/services/speech-to-text/acoustic-audio.html#listAudio).
