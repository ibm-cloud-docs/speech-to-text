---

copyright:
  years: 2017, 2018
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

# Upgrading custom models
{: #customUpgrade}

To improve the quality of speech recognition, the {{site.data.keyword.speechtotextshort}} service occasionally updates base models. Because base models for different languages are independent of each other, as are the broadband and narrowband models for a given language, updates to individual base models do not affect other models. The [Release notes](/docs/services/speech-to-text/release-notes.html) document all base model updates.
{: shortdesc}

When a new version of a base model is released, you must upgrade any custom language and custom acoustic models built on the base model to take advantage of the updates. Your custom models continue to use the older version of the base model until you complete the upgrade. As with all customization operations, you must use credentials for the instance of the service that owns a model to upgrade it.

{{site.data.keyword.IBM_notm}} recommends that you upgrade to the latest version of an updated base model as soon as possible. The sooner you upgrade a custom model, the sooner you can experience the improved performance of the new model. In addition, older versions of base models will be removed at a future time. To encourage upgrading, the service returns a warning message with the results for recognition requests that use custom models based on older base models.

## How upgrading works
{: #upgradeOverview}

When a new base model is first released, existing custom models are based on the older version of the base model. Until a custom model is upgraded, all operations on that custom model, such as adding data to or training the model, affect the existing version of the model. Similarly, all recognition requests that specify the custom model use the existing version of the model.

Upgrading results in two versions of a custom model, one based on the older version of the base model and one based on the latest version of the base model. Once a custom model is upgraded, all operations on that custom model affect the newer, upgraded version of the model. It is no longer possible to add data to or to train the older version of the model. Moreover, you cannot undo an upgrade operation.

By default, the service uses the latest version of the custom model for recognition requests. However, you can continue to use the older version of a custom model for speech recognition. For more information, see [Making recognition requests with upgraded custom models](#upgradeRecognition).

## Upgrading a custom language model
{: #upgradeLanguage}

Follow these steps to upgrade a custom language model:

1.  Ensure that the custom language model is in either the `ready` or the `available` state. You can check a model's status by using the `GET /v1/customizations/{customization_id}` method. If the model's state is `ready`, upgrade the model before training it on its latest data.

1.  Upgrade the custom language model by using the `POST /v1/customizations/{customization_id}/upgrade_model` method, as shown in the following example:

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    The upgrade method is asynchronous. It can take on the order of minutes to complete depending on the number of words in the model's words resource and the current load on the service.

The service returns a 200 response code if the upgrade process is successfully initiated. You can monitor the status of the upgrade by using the `GET /v1/customizations/{customization_id}` method to poll the model's status. Use a loop to check the status every 10 seconds.

While it is being upgraded, the custom model has the status `upgrading`. When the upgrade is complete, the model resumes the status that it had prior to upgrade (`ready` or `available`). Checking the status of an upgrade operation is identical to checking the status of a training operation; for more information, see [Monitoring the train model request](/docs/services/speech-to-text/language-create.html#monitorTraining).

The service cannot accept requests to modify the model in any way until the upgrade request completes. However, you can continue to issue recognition requests with the existing version of the model during the upgrade.

## Upgrading a custom acoustic model
{: #upgradeAcoustic}

Follow these steps to upgrade a custom acoustic model:

1.  Ensure that the custom acoustic model is in either the `ready` or the `available` state. You can check a model's status by using the `GET /v1/acoustic_customizations/{customization_id}` method. If the model's state is `ready`, upgrade the model before training it on its latest data.

1.  If the custom acoustic model was trained with a custom language model, you must first upgrade the custom language model to the latest version of the base model; see [Upgrading a custom language model](#upgradeLanguage). You then specify the customization IDs of both the custom acoustic and custom language models with the upgrade method shown in the following step.

1.  Upgrade the custom acoustic model by using the `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` method, as shown in the following example:

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    If the custom acoustic model was trained with a custom language model, use the `custom_language_model_id` query parameter to specify the customization ID of that custom language model, as shown in the following example:

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    The upgrade method is asynchronous. It can take on the order of minutes or hours to complete, depending on the amount of audio data that the model contains and the current load on the service. As with training, a rule of thumb is that upgrading takes approximately twice the length of the model's audio data.

The service returns a 200 response code if the upgrade process is successfully initiated. You can monitor the status of the upgrade by using the `GET /v1/acoustic_customizations/{customization_id}` method to poll the model's status. Use a loop to check the status once a minute.

While it is being upgraded, the custom model has the status `upgrading`. When the upgrade is complete, the model resumes the status that it had prior to upgrade (`ready` or `available`). Checking the status of an upgrade operation is identical to checking the status of a training operation; for more information, see [Monitoring the train model request](/docs/services/speech-to-text/acoustic-create.html#monitorTraining).

The service cannot accept requests to modify the model in any way until the upgrade request completes. However, you can continue to issue recognition requests with the existing version of the model during the upgrade.

## Upgrade failures
{: #upgradeFailures}

The upgrade of custom model fails to start if the service is currently handling another request for the custom model, such as a training request or a request to add data. The upgrade request also fails to start if

-   The custom model is in a state other than `ready` or `available`.
-   The custom model contains no data (custom words or audio resources).
-   For a custom acoustic model that was trained with a custom language model, the custom language model has not yet been upgraded.

## Listing information about a custom model
{: #upgradeList}

You list information about a custom model by using the `GET /v1/customizations/{customization_id}` or `GET /v1/acoustic_customizations/{customization_id}` method. The output includes a `versions` field that shows information about the base models for which versions of the custom model are available. For example, the following output shows information about an upgraded custom language model:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

The `versions` field indicates that the custom model is available for two versions of the base model: the older version, `en-US_BroadbandModel.v07-06082016.06202016`, and the newer version, `en-US_BroadbandModel.v2017-11-15`. For a custom model that has not been upgraded or for which only a single version of its base model exists, the `versions` field shows a single version.

## Making recognition requests with upgraded custom models
{: #upgradeRecognition}

By default, the service uses the latest version of a custom model specified with a recognition request. However, even after a custom model is upgraded, you can continue to make recognition requests with the older version of the model. You use the `base_model_version` parameter of a recognition method to specify the version of a base model that is to be used for speech recognition.

For example, the following sessionless HTTP request specifies that the older version of the base model is to be used, in which case the older version of the specified custom language model is also used:

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&version=en-US_BroadbandModel.v07-06082016.06202016&customization_id={customization_id}"
```
{: pre}

This feature lets you test the performance and accuracy of a custom model against both the old and new versions of the base model. If after upgrading a custom model you find that its performance is lacking in some way (perhaps certain words are no longer recognized as expected), you can continue to use the older version with recognition requests.

[Base model version](/docs/services/speech-to-text/input.html#version) describes the `base_model_version` parameter and how the service determines what versions of the base and custom models to use with a recognition request. In addition to the information in that section, keep the following in mind when passing both custom language and custom acoustic models with a recognition request:

-   If both custom models are based on the older base model, the service uses the old base model for recognition.
-   If both custom models are based on the newer base model, the service uses the new base model for recognition.
-   If only one of the two custom models has been upgraded to the newer base model, the service uses the old base model for recognition. It selects the old base model because that is the version that the two custom models have in common.
