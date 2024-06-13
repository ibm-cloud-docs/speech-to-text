---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-27"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading custom models
{: #custom-upgrade}

To improve the quality of speech recognition, the {{site.data.keyword.speechtotextfull}} service occasionally updates base previous- and next-generation models. Because base models for different languages are independent of each other, as are different models for the same language, updates to individual base models do not affect other models.
{: shortdesc}

When a base model is updated, you might need to upgrade any custom models that are built on that base model to take advantage of the improvements. An update to a base model that requires an upgrade produces a new version of the base model (for example, `en-US_BroadbandModel.v2020-01-16`). An update that does not require an upgrade does not produce a new version.

-   *For previous-generation models,* all updates to a base model produce a new version of the model. When a new version of a base model is released, you must upgrade any custom language and custom acoustic models that are built on the updated base model to take advantage of the improvements.
-   *For next-generation models,* most updates do not produce a new version of the model. These updates do not require that you upgrade your custom models. Some updates, however, do generate a new version of a base model. You must upgrade any custom language models that are built on the updated base model to take advantage of the improvements.

    The service released new language model customization technology on 27 February 2023. How you upgrade to the new technology is different from traditional custom model upgrades. For more information about the new technology and about upgrading, see

    -   [Improved language model customization for next-generation models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng)
    -   [Upgrading a custom language model based on an improved next-generation model](#custom-upgrade-language-ng)

If an upgrade is required, your custom models continue to use the older version of the base model until you complete the upgrade. Upgrade to the latest version of an updated base model as soon as possible. The sooner you upgrade a custom model, the sooner you can experience the improved speech recognition of the new model.

Once you upgrade a custom model, the service uses the latest version of the custom model by default when you specify that model with a speech recognition request. But you can still direct the service to use the older version of the model. Note that older versions of base models can be removed at a future time. To encourage upgrading, the service returns a warning message with the results for recognition requests that use custom models that are based on older base models.

All updates to base models and whether they require upgrades are announced in the release notes:

-   [Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.cloud_notm}}](/docs/speech-to-text?topic=speech-to-text-release-notes)
-   [Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}](/docs/speech-to-text?topic=speech-to-text-release-notes-data)

As with all customization operations, you must use credentials for the instance of the service that owns a custom model to upgrade it.

## How upgrading works
{: #custom-upgrade-how}

When a new version of a base model is first released, existing custom models are still based on the older version of the base model. Until a custom model is upgraded, all operations on that custom model, such as adding data to or training the model, affect the existing version of the custom model. Similarly, all recognition requests that specify the custom model use the existing version of the custom model.

Upgrading results in two versions of a custom model, one based on the older version of the base model and one based on the latest version of the base model. Once a custom model is upgraded, all operations on that custom model affect the newer, upgraded version of the custom model. It is no longer possible to add data to or to train the older version of the model. You cannot undo an upgrade operation.

When you upgrade a custom model, you do not need to upgrade its individual resources. For a custom language model, the service automatically upgrades any corpora, grammars, and words that are defined for the model. Likewise, when you upgrade a custom acoustic model, the service automatically upgrades its audio resources. You do not need to upgrade a custom language or custom acoustic model that contains no resources.

By default, the service uses the latest version of a custom model for speech recognition requests. However, you can continue to use the older version of a custom model for speech recognition. For more information, see [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use).

## Upgrading a custom language model
{: #custom-upgrade-language}

Follow these steps to upgrade a custom language model:

1.  Ensure that the custom language model is in either the `ready` or the `available` state. You can check a model's status by using the `GET /v1/customizations/{customization_id}` method. If the model's state is `ready`, upgrade the model before training it on its latest data.

1.  Upgrade the custom language model by using the `POST /v1/customizations/{customization_id}/upgrade_model` method:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    The upgrade method is asynchronous. It can take on the order of minutes to complete depending on the number of words in the model's words resource and the current load on the service.

The service returns a 200 response code if the upgrade process is successfully initiated. You can monitor the status of the upgrade by using the `GET /v1/customizations/{customization_id}` method to poll the model's status. Use a loop to check the status every 10 seconds.

While it is being upgraded, the custom model has the status `upgrading`. When the upgrade is complete, the model resumes the status that it had before upgrade (`ready` or `available`). Checking the status of an upgrade operation is identical to checking the status of a training operation. For more information, see [Monitoring the train model request](/docs/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language).

The service cannot accept requests to modify the model in any way until the upgrade request completes. However, you can continue to issue recognition requests with the existing version of the model during the upgrade.

### Upgrading a custom language model based on an improved next-generation model
{: #custom-upgrade-language-ng}

For custom language models that are based on improved next-generation models, upgrade is performed automatically when you retrain the custom model. And once a custom model is based on an improved next-generation model, the service continues to perform any necessary upgrades when the custom model is retrained.

You cannot use the `POST /v1/customizations/{customization_id}/upgrade_model` method to upgrade a custom model to an improved next-generation base model.
{: note}

To upgrade a custom model that is based on an improved next-generation model, perform one of the following procedures:

-   Change the custom model and then train it.

    1.  Change your custom model by adding or modifying a custom word, corpus, or grammar that the model contains. Any change that you make moves the model to the `ready` state.
    1.  Use the `POST /v1/customizations/{customization_id}/train` method to retrain the model. Retraining upgrades the custom model and moves it to the `available` state.

-   [IBM Cloud]{: tag-ibm-cloud} Train the custom model by using the `force` query parameter.

    If you include the parameter `force=true` with the `POST /v1/customizations/{customization_id}/train` request, you do not need to change the model's custom words, corpora, or grammars to enable upgrading. The parameter forces the training, and thus the upgrade, of the custom model regardless of whether it is in the `ready` or `available` state. The following example shows the use of the `force` parameter:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/customizations/{customization_id}/train?force=true"
    ```
    {: pre}



For more information about training a custom model and about monitoring a training request, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

To identify models that use improved language model customization, look for an *(improved)* date in the *Language model customization* column of table 2 in [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng). Make sure to check for a date for the version of the service that you use, *{{site.data.keyword.cloud_notm}}* or *{{site.data.keyword.icp4dfull_notm}}*.
{: tip}

## Upgrading a custom acoustic model
{: #custom-upgrade-acoustic}

Follow these steps to upgrade a custom acoustic model. If the custom acoustic model was trained with a custom language model, you must perform two extra upgrade steps where indicated.

1.  *If the custom acoustic model was trained with a custom language model,* you must first upgrade the custom language model to the latest version of the base model. For more information, see [Upgrading a custom language model](#custom-upgrade-language).

1.  Ensure that the custom acoustic model is in either the `ready` or the `available` state. You can check a model's status by using the `GET /v1/acoustic_customizations/{customization_id}` method. If the model's state is `ready`, upgrade the model before training it on its latest data.

1.  Upgrade the custom acoustic model by using the `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` method:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    The upgrade method is asynchronous. It can take on the order of minutes or hours to complete, depending on the amount of audio data that the model contains and the current load on the service. As with training, upgrading generally takes as long as the cumulative amount of audio data that the model contains.

1.  *If the custom acoustic model was trained with a custom language model,* upgrade the custom acoustic model again, this time with the previously upgraded custom language model. Use the `custom_language_model_id` query parameter to specify the customization ID of the custom language model.

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    Once again, the upgrade method is asynchronous, and upgrading generally takes as long as the cumulative amount of audio that the model contains.

    The request to upgrade the acoustic model with the language model might fail with a 400 response code and the message `No input data modified since last training`. If this error occurs, add the boolean `force` query parameter to the request and set the parameter to `true`. Use the parameter only to force an upgrade of a custom acoustic model in this particular situation.
    {: note}

The service returns a 200 response code if the upgrade process is successfully initiated. You can monitor the status of the upgrade by using the `GET /v1/acoustic_customizations/{customization_id}` method to poll the model's status. Use a loop to check the status periodically (for example, once a minute).

While it is being upgraded, the custom model has the status `upgrading`. When the upgrade is complete, the model resumes the status that it had before upgrade (`ready` or `available`). Checking the status of an upgrade operation is identical to checking the status of a training operation. For more information, see [Monitoring the train model request](/docs/speech-to-text?topic=speech-to-text-acoustic#monitorTraining-acoustic).

The service cannot accept requests to modify the model in any way until the upgrade request completes. However, you can continue to issue recognition requests with the existing version of the model during the upgrade.

## Upgrade failures
{: #custom-upgrade-failures}

The upgrade of a custom model fails to start if the service is handling another request for the model, such as a request to add data or a training request. The upgrade request also fails to start in the following cases:

-   The custom model is in a state other than `ready` or `available`.
-   The custom model contains no data (corpora, custom words, grammars, or audio resources).
-   For a custom acoustic model that was trained with a custom language model, the custom models are based on different versions of the base model. You must upgrade the custom language model before upgrading the custom acoustic model. For more information, see [Upgrading a custom acoustic model](#custom-upgrade-acoustic).
