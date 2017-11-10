---

copyright:
  years: 2017
lastupdated: "2017-11-10"

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

# Using a custom acoustic model
{: #acousticUse}

Once you have created and trained your custom acoustic model, you can use it in speech recognition requests. You specify the customization ID of the custom acoustic model that is to be used with a request via the `acoustic_customization_id` query parameter, as shown in the following examples. You must issue the request with the service credentials of the service instance that owns the model.
{: shortdesc}

-   For the WebSocket interface, use the `/v1/recognize` method:

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?watson-token=' + token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

   For more information, see [The WebSocket interface](/docs/services/speech-to-text/websockets.html).
-   For a sessionless request with the HTTP REST interface, use the `POST /v1/recognize` method:

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [Making sessionless requests](/docs/services/speech-to-text/http.html#HTTP-sessionless).

-   For a session-based request with the HTTP REST interface, use the `POST /v1/sessions` method:

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [Making session-based requests](/docs/services/speech-to-text/http.html#HTTP-sessions).
-   For the HTTP asynchronous interface, use the `POST /v1/recognitions` method:

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [The asynchronous HTTP interface](/docs/services/speech-to-text/async.html).

You can omit the language model from the request if the custom model is based on the default model, `en-US_BroadbandModel`; otherwise, you must use the `model` parameter to specify the base model, as shown for the WebSocket example. A custom model can be used only with the base model for which it is created.

## Using custom acoustic and custom language models together
{: #useBoth}

As described in [Using acoustic and language customization together](/docs/services/speech-to-text/custom.html#combined), you can improve speech recognition accuracy by using complementary custom language and custom acoustic models. You can use both types of model during training of your acoustic model and during speech recognition. Note that both models must be owned by the same service credentials.

Using a custom acoustic model alone or with a custom language model on which it was not trained can still be useful. If the custom acoustic model was trained on acoustic characteristics that match the audio being transcribed, it can improve the quality of transcription.

### Training a custom acoustic model with a custom language model
{: #useBothTrain}

Create and use a custom language model to help train a custom acoustic model in the following cases:

-   You have transcriptions of the audio files that you have added to the custom acoustic model (meaning that you have the verbatim text for each of the audio files).
-   You have either corpora (text files) or a list of words that are relevant to the contents of the audio files. For example, the audio data comes from call-center audio for a specific product or service, and you have transcriptions of previous calls or a list of names of specific products or services that are handled by the call center.

Either form of custom language model can improve the effectiveness of your custom acoustic model. This is particularly important if your audio data contains domain-specific words that do not exist in the service's base vocabulary. Because transcriptions contain the exact contents of the audio, creating a custom language model based on transcriptions is especially helpful. The service can parse the contents of the transcriptions in context and extract OOV words and n-grams that can help it make the most effective use of your audio. (You can also train with an existing custom language model that includes OOV words from the same domain as your audio data.)

Using a custom language model to train a custom acoustic model is effective only if the custom language model was built with direct transcriptions of the audio data or contains words from the same domain as the audio. If the audio contains many OOV words, it is wise to use a custom language model during training, even if the custom language model merely adds a list of custom words. For more information, see [Creating a custom language model](/docs/services/speech-to-text/language-create.html).

> **Note:** Using audio data alone is referred to as *unsupervised training*. Using a custom language model during training is referred to as *lightly supervised training*.

Use the optional `custom_language_model_id` query parameter of the `POST /v1/acoustic_customizations/{customization_id}/train` method to train your custom acoustic model with a custom language model, as in the following example. Pass the GUID of the acoustic model with the `customization_id` parameter and the GUID of the custom language model with the `custom_language_model_id` parameter. Both models must be owned by the service credentials passed with the request.

```bash
curl -X POST -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

If your audio data is less than an hour long, {{site.data.keyword.IBM_notm}} recommends that you provide transcriptions and create custom language models to achieve the best results. Note also that if your audio is domain-specific and contains unique words that are not found in the service's base vocabulary, acoustic model customization alone will not produce those words during transcription. Language model customization is the only means of expanding the service's base vocabulary.

### Using custom language and custom acoustic models during speech recognition
{: #useBothRecognize}

You can specify both a custom language model and a custom acoustic model with any recognition request. If a custom language model contains OOV words from the domain of the audio being recognized, using it with a custom acoustic model during speech recognition can improve transcription accuracy. This is true regardless of whether you trained your custom acoustic model with the custom language model. Using both during training improves the quality of the custom acoustic model; using both during speech recognition improves the quality of transcription.

The following example passes both types of model to the sessionless `POST /v1/recognize` method. Pass the GUID of the custom acoustic model with the `acoustic_customization_id` parameter and the GUID of the custom language model with the `customization_id` parameter. Both models must be owned by the service credentials passed with the request.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&customization_id={customization_id}"
```
{: pre}

For session-based requests, you specify the parameters when you create the session. For WebSockets, you pass them when you establish the connection.
