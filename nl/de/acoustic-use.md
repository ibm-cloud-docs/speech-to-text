---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-24"

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

# Angepasstes Akustikmodell verwenden
{: #acousticUse}

Nachdem Sie Ihr angepasstes Akustikmodell erstellt und trainiert haben, können Sie es bei Spracherkennungsanforderungen verwenden. Zum Angeben des angepassten Akustikmodells für eine Anforderung verwenden Sie den Abfrageparameter `acoustic_customization_id` wie in den Beispielen dieses Abschnitts dargestellt. Sie müssen die Anforderung mit den Berechtigungsnachweisen für die Instanz des Service ausgeben, die Eigner des Modells ist.
{: shortdesc}

Sie können außerdem ein angepasstes Sprachmodell angeben, das bei der Anforderung verwendet werden soll; dies kann die Genauigkeit der Transkription steigern. Zusätzliche Angaben enthält der Abschnitt [Angepasste Sprachmodelle und angepasste Akustikmodelle während der Spracherkennung verwenden](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize).

Sie können mehrere angepasste Akustikmodelle für dieselben oder unterschiedlichen Fachgebiete bzw. Umgebungen erstellen. Mit dem Parameter `acoustic_customization_id` können Sie jedoch jeweils nur ein einziges angepasstes Akustikmodell angeben.

-   Verwenden Sie für die [WebSocket-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-websockets) die Methode `/v1/recognize`. Das angegebene angepasste Modell wird für alle Anforderungen verwendet, die über die Verbindung gesendet werden.

    ```javascript
    var token = {authentifizierungstoken};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Verwenden Sie für die [synchrone HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-http) die Methode `POST /v1/recognize`. Das angegebene angepasste Modell wird für diese Anforderung verwendet.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   Verwenden Sie für die [asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-async) die Methode `POST /v1/recognitions`. Das angegebene angepasste Modell wird für diese Anforderung verwendet.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

Falls das angepasste Modell auf dem Standardmodell `en-US_BroadbandModel` basiert, müssen Sie das Sprachmodell bei der Anforderung nicht angeben. Andernfalls müssen Sie das Basismodell, wie im Beispiel für WebSocket gezeigt, mit dem Parameter `model` angeben. Ein angepasstes Modell kann nur in Verbindung mit dem Basismodell verwendet werden, für das es erstellt wurde.
