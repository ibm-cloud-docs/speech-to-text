---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-10"

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

# Using a custom acoustic model for speech recognition
{: #acousticUse}

Once you create and train your custom acoustic model, you can use it in speech recognition requests by using the `acoustic_customization_id` query parameter. By default, no custom acoustic model is used with a request. You can create multiple custom acoustic models for the same or different domains. But you can specify only one custom acoustic model at a time for a speech recognition request. You must issue the request with credentials for the instance of the service that owns the custom model.
{: shortdesc}

A custom model can be used only with the base model for which it is created. If your custom model is based on a model other than `en-US_BroadbandModel`, the default, you must also specify that base model with the `model` query parameter.

You can also specify a custom language model to be used with the request, which can increase transcription accuracy. For more information, see [Using custom language and custom acoustic models for speech recognition](/docs/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize).

## Examples of using a custom acoustic model
{: #acousticUse-examples}

The following examples show the use of a custom acoustic model with each speech recognition interface:

-   For the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), use the `/v1/recognize` method. The specified custom model is used for all requests that are sent over the connection.

    ```javascript
    var access_token = {access_token};
    var wsURI = '{ws_url}/v1/recognize'
      + '?access_token=' + access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   For the [synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http), use the `POST /v1/recognize` method. The specified custom model is used for that request.

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file1.flac \
    "{url}/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file1.flac \
    "{url}/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   For the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async), use the `POST /v1/recognitions` method. The specified custom model is used for that request.

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @audio-file.flac \
    "{url}/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

You can omit the language model from the request if the custom model is based on the default model, `en-US_BroadbandModel`. Otherwise, you must use the `model` parameter to specify the base model, as shown for the WebSocket example. A custom model can be used only with the base model for which it is created.

## Troubleshooting the use of custom acoustic models
{: #acousticTroubleshoot}
{: troubleshoot}
{: support}

If you apply a custom acoustic model to speech recognition but find that the quality of speech recognition does not improve, check for the following possible problems:

-   Make sure that you are correctly passing the customization ID to the recognition request as shown in the previous examples.
-   Make sure that the status of the custom model is `available`, meaning that it is fully trained and ready to use. For more information, see [Listing custom acoustic models](/docs/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).
