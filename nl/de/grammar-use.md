---

copyright:
  years: 2015, 2019
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

# Grammatik bei der Spracherkennung verwenden
{: #grammarUse}

Nachdem Sie Ihre Grammatik erstellt und Ihr angepasstes Sprachmodell mit der Grammatik trainiert haben, können Sie die Grammatik bei Spracherkennungsanforderungen über die WebSocket-Schnittstelle und die HTTP-Schnittstellen des Service verwenden.
{: shortdesc}

-   Geben Sie mit dem Parameter `language_customization_id` die Anpassungs-ID (GUID) des angepassten Sprachmodells an, für das die Grammatik definiert ist. Sie müssen die Anforderung mit den Berechtigungsnachweisen für die Instanz des Service ausgeben, die Eigner des Modells ist.
-   Geben Sie mit dem Parameter `grammar_name` den Namen der Grammatik an. Bei einer Anforderung können Sie nur eine einzige Grammatik angeben.

Wenn Sie eine Grammatik für die Spracherkennung verwenden, erkennt der Service ausschließlich Wörter aus der angegebenen Grammatik. Angepasste Wörter, die aus Korpora hinzugefügt wurden, die separat hinzugefügt oder geändert wurden oder die durch andere Grammatiken erkannt werden, werden durch den Service nicht verwendet.

-   Bei Verwendung der [WebSocket-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-websockets) geben Sie zuerst die Anpassungs-ID mit dem Parameter `language_customization_id` der Methode `/v1/recognize` an. Mithilfe dieser Methode stellen Sie eine WebSocket-Verbindung zum Service her.

    ```javascript
    var token = {authentifizierungstoken};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    Anschließend geben Sie den Namen der Grammatik mit dem Parameter `grammar_name` in der JSON-Nachricht `start` für die aktive Verbindung an. Die Übergabe dieses Wertes mit der Nachricht `start` versetzt Sie in die Lage, die Grammatik für jede Anforderung, die Sie über die Verbindung senden, dynamisch zu ändern.

    ```javascript
    function onOpen(evt) {
      var message = {
        action: 'start',
        content-type: 'audio/l16;rate=22050',
        grammar_name: '{grammar_name}'
      };
      websocket.send(JSON.stringify(message));
      websocket.send(blob);
    }
    ```
    {: codeblock}
-   Bei der [synchronen HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-http) übergeben Sie beide Parameter mit der Methode `POST /v1/recognize`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   Bei der [asynchronen HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-async) übergeben Sie beide Parameter mit der Methode `POST /v1/recognitions`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
