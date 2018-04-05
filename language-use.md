---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Using a custom language model
{: #languageUse}

Once you have created and trained your custom language model, you can use it in speech recognition requests. You use the `customization_id` query parameter to specify the custom language model for a request, as shown in the following examples. You can also tell the service how much weight to give to words from the custom model; for more information, see [Using customization weight](#weight). You must issue the request with service credentials for the instance of the service that owns the model.
{: shortdesc}

You can create multiple custom language models for the same or different domains. However, you can specify only one custom language model at a time with the `customization_id` parameter.

-   For the WebSocket interface, use the `/v1/recognize` method. The specified custom model is used for all requests sent over the connection.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?watson-token=' + token
      + '&model=es-ES_BroadbandModel'
      + '&customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    For more information, see [The WebSocket interface](/docs/services/speech-to-text/websockets.html).
-   For a sessionless request with the HTTP REST interface, use the `POST /v1/recognize` method. The specified custom model is used for that request.

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [Making sessionless requests](/docs/services/speech-to-text/http.html#HTTP-sessionless).
-   For a session-based request with the HTTP REST interface, use the `POST /v1/sessions` method. The specified custom model is used for all requests sent over the session.

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions?customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [Making session-based requests](/docs/services/speech-to-text/http.html#HTTP-sessions).
-   For the HTTP asynchronous interface, use the `POST /v1/recognitions` method. The specified custom model is used for that request.

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [The asynchronous HTTP interface](/docs/services/speech-to-text/async.html).

You can omit the language model from the request if the custom model is based on the default language model, `en-US_BroadbandModel`; otherwise, you must use the `model` parameter to specify the base model, as shown for the WebSocket example. A custom model can be used only with the base model for which it is created.

## Using customization weight
{: #weight}

> **Note:** Customization weight is a beta feature that is available for all languages supported for language model customization.

A custom language model is a combination of the custom model and the base model that it customizes. You can tell the service how much weight to give to words from the custom language model compared to those from the base model for speech recognition. The weight assigned to a custom model is referred to as its *customization weight*.

You specify the relative weight given to a custom language model as a double between 0.0 to 1.0. By default, each custom language model has a weight of 0.3. The default weight yields the best performance in the general case; it allows both OOV words from the custom model and words from the base vocabulary to be recognized.

However, in cases where the audio to be transcribed makes frequent use of OOV words from the custom model, increasing the customization weight can improve the accuracy of transcription results. Exercise caution when setting the customization weight. While a higher weight can improve the accuracy of phrases from the domain of the custom model, it can also negatively impact performance on non-domain phrases. (Note that even when setting the weight to 0.0, there is a small probability that the transcription can include custom words due to the implementation of language model customization.)

You specify the customization weight for a custom language model by using the `customization_weight` query parameter with a request. You can specify the parameter when you train a custom language model or when you use it for speech recognition. (For session-based requests, you specify the weight when you create the session, and for WebSockets, when you establish the connection.)

-   For a training request, the following example specifies a customization weight of `0.5` with the `POST /v1/customizations/{customization_id}/train` method:

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    Setting a customization weight during training saves the weight with the custom language model. You do not need to pass the weight with each recognition request that uses the custom model.

-   For a recognition request, the following example specifies a customization weight of `0.7` with the sessionless `POST /v1/recognize` method:

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    Setting a customization weight during recognition overrides a weight that was saved with the model during training.
