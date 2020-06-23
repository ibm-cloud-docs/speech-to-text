---

copyright:
  years: 2015, 2020
lastupdated: "2020-06-23"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Using a grammar for speech recognition
{: #grammarUse}

Once you create and train your custom language model with your grammar, you can use the grammar in speech recognition requests with the service's WebSocket and HTTP interfaces.
{: shortdesc}

-   Use the `language_customization_id` parameter to specify the customization ID (GUID) of the custom language model for which the grammar is defined. You must issue the request with credentials for the instance of the service that owns the model.
-   Use the `grammar_name` parameter to specify the name of the grammar. You can specify only a single grammar with a request.

When you use a grammar, the service recognizes only words from the specified grammar. The service does not use custom words that were added from corpora, that were added or modified individually, or that are recognized by other grammars.

The grammars feature is beta functionality. You can use grammars with any language that supports language model customization. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: beta}

## Examples of using a grammar with a custom language model
{: #grammarUse-examples}

-   For the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), you first specify the customization ID with the `language_customization_id` parameter of the `/v1/recognize` method. You use this method to establish a WebSocket connection with the service.

    ```javascript
    var IAM_access_token = {access_token};
    var wsURI = '{ws_url}/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    You then specify the name of the grammar with the `grammar_name` parameter in the JSON `start` message for the active connection. Passing this value with the `start` message allows you to change the grammar dynamically for each request that you send over the connection.

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
-   For the [synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http), pass both parameters with the `POST /v1/recognize` method.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   For the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async), pass both parameters with the `POST /v1/recognitions` method.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
