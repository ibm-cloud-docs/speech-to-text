---

copyright:
  years: 2015, 2021
lastupdated: "2021-12-14"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Using a grammar for speech recognition
{: #grammarUse}

Grammars are available only for some previous-generation and next-generation models. Support can differ between {{site.data.keyword.cloud_notm}} and {{site.data.keyword.icp4dfull_notm}}, and grammars can be generally available for some models and beta for other models. For more information about the available models and the features they support, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).
{: note}

Once you create and train your custom language model with your grammar, you can use the grammar in speech recognition requests:
{: shortdesc}

-   Use the `language_customization_id` query parameter to specify the customization ID (GUID) of the custom language model for which the grammar is defined. A custom model can be used only with the base model for which it is created. If your custom model is based on a model other than `en-US_BroadbandModel`, the default, you must also specify that base model with the `model` query parameter. You must issue the request with credentials for the instance of the service that owns the model.
-   Use the `grammar_name` parameter to specify the name of the grammar. You can specify only a single grammar with a request.

When you use a grammar, the service recognizes only words from the specified grammar. The service does not use custom words that were added from corpora, that were added or modified individually, or that are recognized by other grammars.

## Examples of using a grammar with a custom language model
{: #grammarUse-examples}

The following examples show the use of a grammar with a custom language model for each speech recognition interface:

-   For the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), you first specify the customization ID with the `language_customization_id` parameter of the `/v1/recognize` method. You use this method to establish a WebSocket connection with the service.

    ```javascript
    var access_token = {access_token};
    var wsURI = '{ws_url}/v1/recognize'
      + '?access_token=' + access_token
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

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X POST  \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}

-   For the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async), pass both parameters with the `POST /v1/recognitions` method.

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
