---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-11"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Using upgraded custom models for speech recognition
{: #custom-upgrade-use}

Once you upgrade a custom model, the service uses the latest version of the custom model by default when you specify that model with a speech recognition request. However, you can still direct the service to use the older version of the model. You need to list information about the custom model to determine the available versions.
{: shortdesc}

## Listing version information for a custom model
{: #custom-upgrade-use-listing}

Listing information about a custom model is the only way to see the available versions of a base model. To see the versions of the base model for which a custom model is available, use the following methods:

-   To list information about a custom language model, use the `GET /v1/customizations/{customization_id}` method. For more information, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   To list information about a custom acoustic model, use the `GET /v1/acoustic_customizations/{customization_id}` method. For more information, see [Listing custom acoustic models](/docs/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).

In both cases, the output includes a `versions` field that shows information about the base models that are available for the custom model. If the custom model has not been upgraded or only a single version of its base model exists, the `versions` field shows a single version.

### Example of listing version information
{: #custom-upgrade-use-listing-example}

The following example request shows information for an upgraded custom language model:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations?language=en-US"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations?language=en-US"
```
{: pre}

The `versions` field indicates that the custom model is available for two versions of the base model: the older version, `en-US_BroadbandModel.v07-06082016.06202016`, and the newer version, `en-US_BroadbandModel.v2017-11-15`.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T14:21:26.894Z",
  "updated": "2020-01-18T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v2018-07-31",
    "en-US_BroadbandModel.v2020-01-16"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "available",
  "progress": 100
}
```
{: codeblock}

## Making speech recognition requests with upgraded custom models
{: #custom-upgrade-use-recognition}

By default, the service uses the latest version of a custom model that you pass with a speech recognition request. You issue requests with upgraded custom models as you do with any model. For more information, see

-   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)
-   [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse)
-   [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse)

However, you can also continue to make speech recognition requests with the older version of the custom model by using the `base_model_version` query parameter of a request. You specify the version of the base model that is identified in the `versions` field of the listing output, and the service uses that base model for speech recognition.

You can use the parameter to test the performance and accuracy of a custom model against both the old and new versions of its base model. If you find that an upgraded model's performance is lacking in some way (for instance, certain words are no longer recognized), you can continue to use the older version with recognition requests while you address the inadequacies.

The `base_model_version` parameter is intended primarily for use with custom models that are updated for a new base model, but it can be used without a custom model. The version of a base model that is used for a request depends both on whether you pass the `base_model_version` parameter and on whether you specify a custom model (language, acoustic, or both) with the request:

-   *If you specify a custom model with the request:*

    -   Omit the `base_model_version` parameter to use the latest version of the base model to which the custom model is upgraded. For example, if the custom model is upgraded to the latest version of the base model, the service uses the latest versions of the base and custom models.
    -   Specify the `base_model_version` parameter to use the specified versions of both the base and custom models.

-   *If you do not specify a custom model with the request:*

    -   Omit the `base_model_version` parameter to use the latest version of the base model.
    -   Specify the `base_model_version` parameter to use a specific version of the base model.

To encourage upgrading, the service returns a warning message with speech recognition results that use custom models that are based on older base models.

Because the `base_model_version` parameter is intended primarily for use with custom models, listing information about a custom model is the only way to see the available versions of a base model.
{: note}

### Considerations for using custom acoustic and custom language models together
{: #custom-upgrade-use-recognition-both}

In addition to the behavior described in the previous topic, the service considers the upgrade status of the models that are passed with a speech recognition request. If you pass both custom language and custom acoustic models with a request, the service applies the following guidelines when deciding on the base model version to use with the request:

-   If both custom models are based on the older base model, the service uses the old base model for recognition.
-   If both custom models are based on the newer base model, the service uses the new base model for recognition.
-   If only one of the two custom models is upgraded to the newer base model, the service uses the old base model for recognition. The old base model is the version that the two custom models have in common.

Both custom models must be owned by the credentials that are passed with the request, both must be based on the same base model (for example, `en-US_BroadbandModel`), and both must be in the `available` state. For more information about using both types of custom models with a recognition request, see [Using custom language and custom acoustic models for speech recognition](/docs/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize).

### Example of using an upgraded custom model
{: #custom-upgrade-use-recognition-example}

The earlier example listed the following available versions for a custom language model:

```javascript
"versions": [
  "en-US_BroadbandModel.v2018-07-31",
  "en-US_BroadbandModel.v2020-01-16"
],
```
{: codeblock}

The following synchronous HTTP request specifies that the older version of the base model is to be used. Thus, the older version of the specified custom language model is also used.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v2018-07-31&language_customization_id={customization_id}"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v2018-07-31&language_customization_id={customization_id}"
```
{: pre}

For an asynchronous HTTP request, you specify the parameters when you create the asynchronous job. For a WebSocket request, you pass the parameters when you establish a connection.
