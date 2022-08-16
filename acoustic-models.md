---

copyright:
  years: 2019, 2022
lastupdated: "2022-08-12"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Managing custom acoustic models
{: #manageAcousticModels}

Acoustic model customization is available only for previous-generation models. It is not available for next-generation models.
{: note}

The customization interface includes the `POST /v1/acoustic_customizations` method for creating a custom acoustic model. The interface also includes the `POST /v1/acoustic_customizations/train` method for training a custom model on its latest audio resources. For more information, see
{: shortdesc}

-   [Create a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [Train the custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

In addition, the interface includes methods for listing information about custom acoustic models, resetting a custom model to its initial state, upgrading a custom model, and deleting a custom model. You cannot train, reset, upgrade, or delete a custom model while the service is handling another operation on that model, including adding audio resources to the model.

## Listing custom acoustic models
{: #listModels-acoustic}

The customization interface provides two methods for listing information about the custom acoustic models that are owned by the specified credentials:

-   The `GET /v1/acoustic_customizations` method lists information about all custom acoustic models or about all custom acoustic models for a specified language.
-   The `GET /v1/acoustic_customizations/{customization_id}` method lists information about a specified custom acoustic model. Use this method to poll the service about the status of a training request.

Both methods return the following information about a custom acoustic model:

-   `customization_id` identifies the custom model's Globally Unique Identifier (GUID). The GUID is used to identify the model in methods of the interface.
-   `created` is the date and time in Coordinated Universal Time (UTC) at which the custom model was created.
-   `updated` is the date and time in Coordinated Universal Time (UTC) at which the custom model was last modified.
-   `language` is the language of the custom model.
-   `owner` identifies the credentials of the service instance that owns the custom model.
-   `name` is the name of the custom model.
-   `description` shows the description of the custom model, if one was provided at its creation.
-   `base_model_name` indicates the name of the language model for which the custom model was created.
-   `versions` provides a list of the available versions of the custom model. Each element of the array indicates a version of the base model with which the custom model can be used. Multiple versions exist only if the custom model is upgraded to a new version of its base model. Otherwise, only a single version is shown. For more information, see [Listing version information for a custom model](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-listing).

The methods also return a `status` field that indicates the state of the custom model:

-   `pending` indicates that the model was created. It is waiting either for valid training data (audio resources) to be added or for the service to finish analyzing data that was added.
-   `ready` indicates that the model contains valid audio data and is ready to be trained. If the model contains a mix of valid and invalid audio resources, training of the model fails unless you set the `strict` query parameter to `false`. For more information, see [Training failures](/docs/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).
-   `training` indicates that the model is being trained on audio data.
-   `available` indicates that the model is trained and ready to use with recognition requests.
-   `upgrading` indicates that the model is being upgraded.
-   `failed` indicates that training of the model failed. Examine the model's audio resources to determine what prevented the model from being trained. Possible errors include not enough audio, too much audio, or an invalid audio resource.

Additionally, the output includes a `progress` field that indicates the current progress of the custom model's training. If you used the `POST /v1/acoustic_customizations/{customization_id}/train` method to start training the model, this field indicates the current progress of that request as a percentage complete. At this time, the value of the field is `100` if the status is `available`; otherwise, it is `0`.

When you monitor the training or upgrading of a custom model, poll the value of the `status` field, not the value of the `progress` field. If the operation fails for any reason, the value of the `status` field changes to reflect the failure; the value of the `progress` field remains `0`.
{: tip}

### List all custom acoustic models example
{: #listExample-acoustic-all}

The following example includes the `language` query parameter to list all US English custom acoustic models that are owned by the specified credentials:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations?language=en-US"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations?language=en-US"
```
{: pre}

The credentials own two such models. The first model is awaiting data or is being processed by the service. The second model is fully trained and ready for use.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fa97",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2020-01-19T11:12:02.296Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2018-07-31",
        "en-US_BroadbandModel.v2020-01-16"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732faa3312",
      "created": "2017-12-01T18:51:37.291Z",
      "updated": "2017-12-02T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

### List a specific custom acoustic model example
{: #listExample-acoustic-specific}

The following example returns information about the custom model that has the specified customization ID:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}"
```
{: pre}

The response duplicates the information from the previous example:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fa97",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2020-01-19T11:12:02.296Z",
  "language": "en-US",
  "versions": [
    "en-US_BroadbandModel.v2018-07-31",
    "en-US_BroadbandModel.v2020-01-16"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Resetting a custom acoustic model
{: #resetModel-acoustic}

Use the `POST /v1/acoustic_customizations/{customization_id}/reset` method to reset a custom acoustic model. Resetting a custom model removes all of the audio resources from the model, initializing the model to its state at creation. The method does not delete the model itself or metadata such as its name and language. However, when you reset a model, its audio resources are removed and must be re-created.

### Reset a custom acoustic model example
{: #resetExample-acoustic}

The following example resets the custom acoustic model with the specified customization ID:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## Deleting a custom acoustic model
{: #deleteModel-acoustic}

Use the `DELETE /v1/acoustic_customizations/{customization_id}` method to delete a custom acoustic model that you no longer need. The method deletes all audio that is associated with the custom model and the model itself. Use this method with caution: A custom model and its data cannot be reclaimed after you delete the model.

### Delete a custom acoustic model example
{: #deleteExample-acoustic}

The following example deletes the custom acoustic model with the specified customization ID:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X DELETE -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X DELETE \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}"
```
{: pre}
