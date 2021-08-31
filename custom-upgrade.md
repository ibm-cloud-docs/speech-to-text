---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-23"

subcollection: speech-to-text

---

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

# Upgrading custom models
{: #custom-upgrade}

Upgrading custom models is necessary only for previous-generation models. You do not need to upgrade next-generation models.
{: note}

To improve the quality of speech recognition, the {{site.data.keyword.speechtotextfull}} service occasionally updates base models. Because base models for different languages are independent of each other, as are the broadband and narrowband models for a language, updates to individual base models do not affect other models.
{: shortdesc}

When a new version of a base model is released, you must upgrade any custom language and custom acoustic models that are built on the base model to take advantage of the updates. Your custom models continue to use the older version of the base model until you complete the upgrade. As with all customization operations, you must use credentials for the instance of the service that owns a model to upgrade it.

Upgrade to the latest version of an updated base model as soon as possible. The sooner you upgrade a custom model, the sooner you can experience the improved performance of the new model. In addition, older versions of base models can be removed at a future time. To encourage upgrading, the service returns a warning message with the results for recognition requests that use custom models that are based on older base models.

All base model updates are documented in the [Release notes for IBM Cloud](/docs/speech-to-text?topic=speech-to-text-release-notes) and the [Release notes for IBM Cloud Pak for Data](/docs/speech-to-text?topic=speech-to-text-release-notes-data).
{: note}

## How upgrading works
{: #custom-upgrade-how}

When a new base model is first released, existing custom models are still based on the older version of the base model. Until a custom model is upgraded, all operations on that custom model, such as adding data to or training the model, affect the existing version of the custom model. Similarly, all recognition requests that specify the custom model use the existing version of the custom model.

Upgrading results in two versions of a custom model, one based on the older version of the base model and one based on the latest version of the base model. Once a custom model is upgraded, all operations on that custom model affect the newer, upgraded version of the custom model. It is no longer possible to add data to or to train the older version of the model. Note that you cannot undo an upgrade operation.

When you upgrade either type of custom model, you do not need to upgrade its individual resources. For a custom language model, the service automatically upgrades any corpora, grammars, and words that are defined for the model. Likewise, when you upgrade a custom acoustic model, the service automatically upgrades its audio resources. You do not need to upgrade a custom language or custom acoustic model that contains no resources.

By default, the service uses the latest version of the custom model for speech recognition requests. However, you can continue to use the older version of a custom model for speech recognition. For more information, see [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use).

## Upgrading a custom language model
{: #custom-upgrade-language}

Follow these steps to upgrade a custom language model:

1.  Ensure that the custom language model is in either the `ready` or the `available` state. You can check a model's status by using the `GET /v1/customizations/{customization_id}` method. If the model's state is `ready`, upgrade the model before training it on its latest data.

1.  Upgrade the custom language model by using the `POST /v1/customizations/{customization_id}/upgrade_model` method:

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

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

## Upgrading a custom acoustic model
{: #custom-upgrade-acoustic}

Follow these steps to upgrade a custom acoustic model. If the custom acoustic model was trained with a custom language model, you must perform two extra upgrade steps where indicated.

1.  *If the custom acoustic model was trained with a custom language model,* you must first upgrade the custom language model to the latest version of the base model. For more information, see [Upgrading a custom language model](#custom-upgrade-language).

1.  Ensure that the custom acoustic model is in either the `ready` or the `available` state. You can check a model's status by using the `GET /v1/acoustic_customizations/{customization_id}` method. If the model's state is `ready`, upgrade the model before training it on its latest data.

1.  Upgrade the custom acoustic model by using the `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` method:

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    The upgrade method is asynchronous. It can take on the order of minutes or hours to complete, depending on the amount of audio data that the model contains and the current load on the service. As with training, upgrading generally takes approximately twice the length of the model's audio data.

1.  *If the custom acoustic model was trained with a custom language model,* upgrade the custom acoustic model again, this time with the previously upgraded custom language model. Use the `custom_language_model_id` query parameter to specify the customization ID of the custom language model.

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    Once again, the upgrade method is asynchronous, and upgrading generally takes approximately twice the length of the model's audio data.

    The request to upgrade the acoustic model with the language model might fail with a 400 response code and the message `No input data modified since last training`. If this error occurs, add the boolean `force` query parameter to the request and set the parameter to `true`. Use the parameter only to force an upgrade of a custom acoustic model in this particular situation.
    {: note}

The service returns a 200 response code if the upgrade process is successfully initiated. You can monitor the status of the upgrade by using the `GET /v1/acoustic_customizations/{customization_id}` method to poll the model's status. Use a loop to check the status once a minute.

While it is being upgraded, the custom model has the status `upgrading`. When the upgrade is complete, the model resumes the status that it had before upgrade (`ready` or `available`). Checking the status of an upgrade operation is identical to checking the status of a training operation. For more information, see [Monitoring the train model request](/docs/speech-to-text?topic=speech-to-text-acoustic#monitorTraining-acoustic).

The service cannot accept requests to modify the model in any way until the upgrade request completes. However, you can continue to issue recognition requests with the existing version of the model during the upgrade.

## Upgrade failures
{: #custom-upgrade-failures}

The upgrade of a custom model fails to start if the service is handling another request for the model, such as a training request or a request to add data. The upgrade request also fails to start in the following cases:

-   The custom model is in a state other than `ready` or `available`.
-   The custom model contains no data (custom words or audio resources).
-   For a custom acoustic model that was trained with a custom language model, the custom models are based on different versions of the base model. You must upgrade the custom language model before upgrading the custom acoustic model. For more information, see [Upgrading a custom acoustic model](#custom-upgrade-acoustic).
