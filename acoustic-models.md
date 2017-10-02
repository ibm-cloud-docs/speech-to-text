---

copyright:
  years: 2017
lastupdated: "2017-10-02"

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

# Managing custom acoustic models
{: #manageModels}

The customization interface offers the `POST /v1/acoustic_customizations` method for creating a custom acoustic model (see [Create a new custom acoustic model](/docs/services/speech-to-text/acoustic-create.html#createModel)) and the `POST /v1/acoustic_customizations/train` method for training a custom model on its latest audio resources (see [Train the custom acoustic model](/docs/services/speech-to-text/acoustic-create.html#trainModel)). In addition, the interface includes methods for listing information about custom acoustic models, for resetting a custom model to its initial state, and for deleting a custom model.

## Listing custom acoustic models
{: #listModels}

The customization interface provides two methods for listing information about the custom acoustic models owned by the specified service credentials:

-   The `GET /v1/acoustic_customizations` method lists information about all custom acoustic models or about all custom acoustic models for a specified language.
-   The `GET /v1/acoustic_customizations/{customization_id}` method lists information about a specified custom acoustic model. Use this method to poll the service about the status of a training request.

Both methods return the following information about a custom acoustic model:

-   `customization_id`: The custom model's Globally Unique Identifier (GUID), which is used to identify the model in methods of the interface.
-   `created`: The date and time in Coordinated Universal Time (UTC) at which the custom model was created.
-   `language`: The language of the custom model.
-   `owner`: The credentials of the service instance that owns the custom model.
-   `name`: The name of the custom model.
-   `description`: The description of the custom model, if one was provided at its creation.
-   `base_model_name`: The name of the language model for which the custom model was created.

The methods also return a `status` field that indicates the current status of the custom model:

-   `pending` indicates that the model was created but is waiting either for training data to be added or for the service to finish analyzing data that has been added.
-   `ready` indicates that the model contains audio data and is ready to be trained.
-   `training` indicates that the model is currently being trained on audio data.
-   `available` indicates that the model is trained and ready to use with recognition requests.
-   `failed` indicates that training of the model failed. Examine the model's audio resources to determine the errors that prevented the model from being trained (for example, not enough audio, too much audio, or an invalid audio resource).

Additionally, the output includes a `progress` field that indicates the current progress of the custom model's training. If you used the `POST /v1/acoustic_customizations/{customization_id}/train` method to initiate training of the model, this field indicates the current progress of that request as a percentage complete. At this time, the value of the field is `0` if the status is `pending`, `ready`, `training`, or `failed`; the value is `100` if the status is `available`.

### Example requests and responses
{: #listExample}

The following example includes the `language` query parameter to list all US English custom acoustic models owned by the service credentials:

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
```
{: pre}

Two such models are owned by the service credentials. The first is awaiting data or is currently being processed by the service; the second is fully trained and ready for use.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "language": "en-US",
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
      "base_model_name": "en-US_NarrowbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

The following example returns information about the custom model that has the specified customization ID:

```bash
curl -X GET -u {username}:{password}
https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
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
{: #resetModel}

Use the `POST /v1/acoustic_customizations/{customization_id}/reset` method to reset a custom acoustic model. Resetting a custom model removes all of the audio resources from the model, initializing the model to its state at creation. The method does not delete the model itself or metadata such as its name and language. However, once you reset a model, its audio resources are removed and must be re-created.

### Example request
{: #resetExample}

The following example resets the custom acoustic model with the specified customization ID:

```bash
curl -X POST -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## Deleting a custom acoustic model
{: #deleteModel}

Use the `DELETE /v1/acoustic_customizations/{customization_id}` method to delete a custom acoustic model that you no longer need. The method deletes all audio associated with the custom model as well as the model itself. Use this method with caution: A custom model and the data associated with it cannot be reclaimed once you delete the model.

### Example request
{: #deleteExample}

The following example deletes the custom acoustic model with the specified customization ID:

```bash
curl -X DELETE -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
