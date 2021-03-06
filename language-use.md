---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-07"

subcollection: speech-to-text

content-type: troubleshoot

---

{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}
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

# Using a custom language model for speech recognition
{: #languageUse}

Once you create and train your custom language model, you can use it in speech recognition requests by using the `language_customization_id` query parameter. By default, no custom language model is used with a request. You can create multiple custom language models for the same or different domains. But you can specify only one custom language model at a time for a speech recognition request. You must issue the request with credentials for the instance of the service that owns the custom model.
{: shortdesc}

A custom model can be used only with the base model for which it is created. If your custom model is based on a model other than `en-US_BroadbandModel`, the default, you must also specify that base model with the `model` query parameter.

For information about telling the service how much weight to give to words from a custom model, see [Using customization weight](#weight). For examples that use a grammar with a custom language model, see [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse).

## Examples of using a custom language model
{: #languageUse-examples}

The following examples show the use of a custom language model with each speech recognition interface:

-   For the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), use the `/v1/recognize` method. The specified custom model is used for all requests that are sent over the connection.

    ```javascript
    var IAM_access_token = {access_token};
    var wsURI = '{ws_url}/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   For the [synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http), use the `POST /v1/recognize` method. The specified custom model is used for that request.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   For the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async), use the `POST /v1/recognitions` method. The specified custom model is used for that request.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

## Using customization weight
{: #weight}

A custom language model is a combination of the custom model and the base model that it customizes. You can tell the service how much weight to give to words from the custom language model compared to words from the base model for speech recognition. The weight that is assigned to a custom model is referred to as its *customization weight*.

You specify the relative weight for a custom language model as a double between 0.0 to 1.0. By default, each custom language model has a weight of 0.3. The default weight yields the best performance in the general case. It allows both OOV words from the custom model and words from the base vocabulary to be recognized.

However, in cases where the audio to be transcribed makes frequent use of OOV words from the custom model, increasing the customization weight can improve the accuracy of transcription results. Exercise caution when you set the customization weight. Although a higher weight can improve the accuracy of phrases from the domain of the custom model, it can also negatively impact performance on non-domain phrases. (Even if you set the weight to 0.0, a small probability exists that the transcription can include custom words due to the implementation of language model customization.)

You specify a customization weight by using the `customization_weight` parameter. You can specify the parameter when you train a custom language model or when you use it with a speech recognition request.

-   For a training request, the following example specifies a customization weight of `0.5` with the `POST /v1/customizations/{customization_id}/train` method:

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    Setting a customization weight during training saves the weight with the custom language model. You do not need to pass the weight with each recognition request that uses the custom model.

-   For a recognition request, the following example specifies a customization weight of `0.7` with the `POST /v1/recognize` method:

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file1.flac \
    "{url}/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    Setting a customization weight during speech recognition overrides a weight that was saved with the model during training.

## Troubleshooting the use of custom language models
{: #languageTroubleshoot}
{: troubleshoot}
{: support}

If you apply a custom language model to speech recognition but find that the service does not appear to be using words that the model contains, check for the following possible problems:

-   Make sure that you are correctly passing the customization ID to the recognition request as shown in the previous examples.
-   Make sure that the status of the custom model is `available`, meaning that it is fully trained and ready to use. For more information, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Check the pronunciations that were generated for the new words to make sure that they are correct. For more information, see [Validating a words resource](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
