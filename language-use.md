---

copyright:
  years: 2015, 2023
lastupdated: "2024-05-10"

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Using a custom language model for speech recognition
{: #languageUse}

Once you create and train your custom language model, you can use it in speech recognition requests by using the `language_customization_id` query parameter. By default, no custom language model is used with a request. You can create multiple custom language models for the same or different domains. But you can specify only one custom language model at a time for a speech recognition request. You must issue the request with credentials for the instance of the service that owns the custom model.
{: shortdesc}

A custom model can be used only with the base model for which it is created. If your custom model is based on a model other than the default, you must also specify that base model with the `model` query parameter. For more information, see [Using the default model](/docs/speech-to-text?topic=speech-to-text-models-use#models-use-default).

For information about telling the service how much weight to give to words from a custom model, see [Using customization weight](#weight). For examples that use a grammar with a custom language model, see [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse).

## Examples of using a custom language model
{: #languageUse-examples}

The following examples show the use of a custom language model with each speech recognition interface. In this case, the custom model that is used is based on the next-generation model `en-US_Telephony`.

-   For the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), use the `/v1/recognize` method. The specified custom model is used for all requests that are sent over the connection.

    ```javascript
    var access_token = {access_token};
    var wsURI = '{ws_url}/v1/recognize'
      + '?access_token=' + access_token
      + '&model=en-US_Telephony'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

-   For the [synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http), use the `POST /v1/recognize` method. The specified custom model is used for that request.

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognize?model=en-US_Telephony&language_customization_id={customization_id}"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognize?model=en-US_Telephony&language_customization_id={customization_id}"
    ```
    {: pre}

-   For the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async), use the `POST /v1/recognitions` method. The specified custom model is used for that request.

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?model=en-US_Telephony&language_customization_id={customization_id}"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?model=en-US_Telephony&language_customization_id={customization_id}"
    ```
    {: pre}

## Using customization weight
{: #weight}

A custom language model is a combination of the custom model and the base model that it customizes. You can tell the service how much weight to give to words from the custom language model compared to words from the base model for speech recognition. The weight that is assigned to a custom model is referred to as its *customization weight*.

You specify the relative weight for a custom language model as a double between 0.0 to 1.0:

-   For custom models that are based on large speech models, the default customization weight is 0.5.
-   For custom models that are based on previous-generation models, the default customization weight is 0.3.
-   For custom models that are based on most next-generation models, the default customization weight is 0.2.
-   For custom models that are based on improved next-generation models, the default customization weight is 0.1.

To identify models that use improved language model customization, look for an *(improved)* date in the *Language model customization* column of table 2 in [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng). Make sure to check for a date for the version of the service that you use, *{{site.data.keyword.cloud_notm}}* or *{{site.data.keyword.icp4dfull_notm}}*.
{: tip}

The default weight yields the best performance in the general case. It allows both words from the custom model and words from the base vocabulary to be recognized.

However, in cases where the audio to be transcribed makes frequent use of words from the custom model, increasing the customization weight can improve the accuracy of transcription results. Exercise caution when you set the customization weight. Although a higher weight can improve the accuracy of phrases from the domain of the custom model, it can also negatively impact performance on non-domain phrases. (Even if you set the weight to 0.0, a small probability exists that the transcription can include custom words due to the implementation of language model customization.)

You specify a customization weight by using the `customization_weight` parameter. You can specify the parameter when you train a custom language model or when you use it with a speech recognition request.

-   For a training request, the following example specifies a customization weight of `0.5` with the `POST /v1/customizations/{customization_id}/train` method:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    Setting a customization weight during training saves the weight with the custom language model. You do not need to pass the weight with each recognition request that uses the custom model.

-   For a recognition request, the following example specifies a customization weight of `0.7` with the `POST /v1/recognize` method:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file1.flac \
    "{url}/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
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
-   Check the pronunciations that were generated for the new words to make sure that they are correct. For more information, see
    -   [Validating a words resource for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel)
    -   [Validating a words resource for large speech models and next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#validateModel-ng)
