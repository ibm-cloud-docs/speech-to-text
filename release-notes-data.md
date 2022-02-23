---

copyright:
  years: 2018, 2022
lastupdated: "2022-02-23"

keywords: speech to text release notes,speech to text for IBM cloud pak for data release notes

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}
{: #release-notes-data}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

The following features and changes were included for each release and update of installed or on-premises instances of {{site.data.keyword.speechtotextfull}} for {{site.data.keyword.icp4dfull_notm}}. The information includes known limitations. Unless otherwise noted, all changes are compatible with earlier releases and are automatically and transparently available to all new and existing applications.
{: shortdesc}

For information about releases and updates for {{site.data.keyword.cloud_notm}}, see [Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.cloud_notm}}](/docs/speech-to-text?topic=speech-to-text-release-notes).
{: note}

## Known limitations
{: #release-notes-data-limitations}

The service has the following known limitations:

-   **29 July 2021:** When you use previous-generation models with the WebSocket interface, if you request both speaker labels and interim results, the speaker labels response includes an extra object. The final results duplicate the object for the last speaker label. Instead of appearing once with `"final": true`, the object appears twice: once with `"final": false` and once with `"final": true`.

    For example, a WebSocket request that includes both speaker labels and interim results sends a `start` message like the following:

    ```json
    var message = {
      action: 'start',
      speaker_labels: true,
      interim_results: true
    };
    websocket.send(JSON.stringify(message));
    ```
    {: codeblock}

    To indicate final results for speaker labels, the service is supposed to return a response of the following form:

    ```json
    {
      "speaker_labels": [
        . . .
        {
          "from": 1.01,
          "to": 1.75,
          "speaker": 0,
          "confidence": 0.75,
          "final": false
        },
        {
          "from": 1.76,
          "to": 2.50,
          "speaker": 1,
          "confidence": 0.80,
          "final": true
        }
      ]
    }
    ```
    {: codeblock}

    Instead, the service returns final results like the following. Note that the object for the last speaker label, which begins at `1.76` and ends at `2.50`, is returned twice. The `"final": true` indication is associated with the second instance of the label.

    ```json
    {
      "speaker_labels": [
        . . .
        {
          "from": 1.01,
          "to": 1.75,
          "speaker": 0,
          "confidence": 0.75,
          "final": false
        },
        {
          "from": 1.76,
          "to": 2.50,
          "speaker": 1,
          "confidence": 0.80,
          "final": false
        }
      ]
    }{
      "from": 1.76,
      "to": 2.50,
      "speaker": 1,
      "confidence": 0.80,
      "final": true
    }
    ```
    {: codeblock}

    For more information about using speaker labels with interim results, see [Requesting interim results for speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels#speaker-labels-interim).

-   **9 December 2020:** The `GET /v1/models` and `GET /v1/models/{model_id}` methods list information about language models. Under `supported_features`, the `speaker_labels` field indicates whether you can use the `speaker_labels` parameter with a model. At this time, the field returns `true` for all models.

    However, speaker labels are supported as beta functionality only for US English, Australian English, German, Japanese, Korean, and Spanish (both broadband and narrowband models) and UK English (narrowband model only). Speaker labels are not supported for any other models. Do not rely on the field to identify which models support speaker labels.

    For more information about speaker labels and supported models, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

## 23 February 2022 (Version 4.0.6)
{: #speech-to-text-data-23february2022}

Version 4.0.6 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.6 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Updates to import/export scripts
:   The `import_export.sh` and `transfer_ownership.sh` scripts have been updated. These scripts are used to import and export data between clusters, back up and restore data, and migrate data from version 3.5 to version 4.0.x. The scripts have been modified and improved as follows:
    -   The `transfer_ownership.sh` script now requires a `-c` option to be included on the command line before the `<custom_resource_name>` argument.
    -   The `transfer_ownership.sh` script now requires a `-v <version>` option and argument to indicate the version to which ownership of resources is being transferred. Specify `35` for version 3.5 or `40` for version 4.0.x.
    -   The `transfer_ownership.sh` script now requires a `-p` option to be included on the command line before the `<postgres_auth_secret_name>` argument.
    -   The `<postgres_auth_secret_name>` argument provides the Kubernetes secret that is used to authenticate to the PostgreSQL datastore to which you are transferring ownership. You can omit the authentication secret if is the same as the default value (`<custom-resource-name>-postgres-auth-secret` for version 4.0.x, `user-provided-postgressql` for version 3.5). You must provide the secret if it is different from the default value.
    -   Both scripts now include a `-h` (`--help`) option to display information about the script and its usage.

    For more information, see
    -   [Administering {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-administering-watson-speech){: external}, specifically *Importing and exporting data* and *Backing up and restoring data*.
    -   [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}, specifically *Migrating data from {{site.data.keyword.icp4dfull_notm}} Version 3.5*.

Updated recommendation for OpenShift Container Storage
:   Starting with Speech services version 4.0.6, the recommended storage class for OpenShift Container Storage is `ocs-storagecluster-ceph-rbd`.
    -   If you are installing Speech services 4.0.6 or upgrading to Speech services 4.0.6 from IBM Cloud Pak for Data version 3.5, specify the `ocs-storagecluster-ceph-rbd` storage class during installation or upgrade.
    -   If you are upgrading to Speech services 4.0.6 from a previous refresh of Cloud Pak for Data version 4.0, continue to use `ocs-storagecluster-cephfs`. You cannot change the storage that is used in an existing deployment.

    The value is specified with the `storageClass` property in the Speech services custom resource:

    ```yaml
    ################
    # Storage class
    ################
      storageClass: "ocs-storagecluster-ceph-rbd"
    ```
    {: codeblock}

    The Speech services work with either version of OpenShift Container Storage. The newly recommended version has more restrictive access permissions. For more information, see
    -   [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}
    -   [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}

New beta `en-WW_Medical_Telephony` model is now available
:   A new beta next-generation `en-WW_Medical_Telephony` is now available. The new model understands terms from the medical and pharmacological domains. Use the model in situations where you need to transcribe common medical terminology such as medicine names, product brands, medical procedures, illnesses, types of doctor, or COVID-19-related terminology. Common use cases include conversations between a patient and a medical provider (for example, a doctor, nurse, or pharmacist).

    The new model is installed from the Speech services custom resource by setting `enWwMedicalTelephony` to `enabled: true`. The model is available for all supported English dialects: Australian, Indian, UK, and US.
    -   The model supports language model customization and grammars as beta functionality.
    -   It supports most of the same parameters as the `en-US_Telephony` model.
    -   It does *not* support the following parameters: `low_latency`, `profanity_filter`, `redaction`, and `speaker_labels`.
    -   At this time, it does *not* support `smart_formatting` for {{site.data.keyword.icp4dfull_notm}}.

    For more information, see [The English medical telephony model](/docs/speech-to-text?topic=speech-to-text-models-ng#models-medical).

Update to Chinese `zh-CN_Telephony` model
:   The next-generation Chinese model `zh-CN_Telephony` has been updated for improved speech recognition. The model continues to support low latency. By default, the service automatically uses the updated model for all speech recognition requests. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

    If you have custom language models that are based on the updated model, you must upgrade your existing custom models to take advantage of the updates by using the `POST /v1/customizations/{customization_id}/upgrade_model` method. For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

Update to Japanese `ja-JP_Multimedia` model to support low latency
:   The next-generation Japanese model `ja-JP_Multimedia` now supports low latency. You can use the `low_latency` parameter with speech recognition requests that use the model. You do not need to upgrade custom models that are based on the updated Japanese base model.  For more information about the next-generation models and low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

## 11 February 2022 (Version 4.0.5)
{: #speech-to-text-data-11february2022}

Defect fix for custom model upgrade and base model version documentation
:   **Defect fix:** The documentation that describes the upgrade of custom models and the version strings that are used for different versions of base models has been updated. The documentation now states that upgrade for language model customization also applies to next-generation models. Also, the version strings that represent different versions of base models have been updated. And the `base_model_version` parameter can also be used with upgraded next-generation models.

    For more information about custom model upgrade, when upgrade is necessary, and how to use older versions of custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

Defect fix for capitalization documentation
:   **Defect fix:** The documentation that describes the service's automatic capitalization of transcripts has been updated. The service capitalizes appropriate nouns only for the following languages and models:
    -   All previous-generation US English models
    -   The next-generation German model

    For more information, see [Capitalization](/docs/speech-to-text?topic=speech-to-text-basic-response#response-capitalization).

## 31 January 2022 (Version 4.0.5)
{: #speech-to-text-data-31january2022}

Version 4.0.5 has been updated
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.5 has been updated to address installation issues. The case package version is now 4.0.6. Use this package instead of the version 4.0.5 package. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Important: Extra steps for mirrored installation are no longer necessary
:   *Important:* The [26 January 2022 release notes](#speech-to-text-data-26january2022) included important notes for the following steps:

    -   Additional step for performing a mirrored installation of Minio datastore
    -   Additional steps for performing a mirrored installation of new next-generation models

    These additional steps are no longer needed. The case package has been updated to correct the installation issues.

## 26 January 2022 (Version 4.0.5)
{: #speech-to-text-data-26january2022}

Version 4.0.5 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.5 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Important: Additional step for performing a mirrored installation of Minio datastore
:   **Important:** These steps are no longer needed if you install case package 4.0.6. For more information, see [28 January 2022 (Version 4.0.5)](#speech-to-text-data-28january2022).

    If you are performing a mirrored installation (for example, in an air-gapped environment), you need to perform an additional step *before* completing either of the following steps:

    -   Step 7 [Mirroring the images to the private registry](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=registry-mirroring-images-bastion-node#reference_g4h_z1q_tpb__mirror-to-target){: external} of *Mirroring images with a bastion model*
    -   Step 8 [Mirroring the images to the intermediary container registry](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=registry-mirroring-images-intermediary-container#preinstall_container_registry_air_gapped__mirror-to-target){: external} of *Mirroring images with an intermediary container registry*

    This step is mandatory to copy the necessary images for the Minio datastore:

    ```sh
    echo 'cp.icr.io,cp/opencontent-minio-client,1.1.4,sha256:7b4cf5e47a0455cfa7ca9ab246b80916e4dccbc1483b3e0f276fb7b0ab3e5c60,IMAGE,linux,x86_64,"",0,CASE,"",""' \
    >> $CASE_PATH/ibm-watson-speech-4.0.5-images.csv
    ```
    {: codeblock}

    Failure to perform this step will cause installation errors for both {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}}.

Important: Additional steps for performing a mirrored installation of new next-generation models
:   **Important:** These steps are no longer needed if you install case package 4.0.6. For more information, see [28 January 2022 (Version 4.0.5)](#speech-to-text-data-28january2022).

    If you are performing a mirrored installation (for example, for an air-gapped environment) and plan to install any of the new next-generation models for {{site.data.keyword.speechtotextshort}} (for more information, see the later release note), you must perform an additional step *before* completing either of the following steps:

    -   Step 7 [Mirroring the images to the private container registry](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=registry-mirroring-images-bastion-node#reference_g4h_z1q_tpb__mirror-to-target){: external} of *Mirroring images with a bastion model*
    -   Step 8 [Mirroring the images to the intermediary container registry](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=registry-mirroring-images-intermediary-container#preinstall_container_registry_air_gapped__mirror-to-target){: external} of *Mirroring images with an intermediary container registry*

    Each additional step is unique to the model that is being installed. If you install more than one of the new models, issue the indicated command for each model that you are installing.

    -   For the Chinese telephony model (`zh-CN_Telephony`):

        ```sh
        echo 'cp.icr.io,cp/watson-speech/zh-cn-telephony,2022-01-05-405models,sha256:52af6dfccd64ccd81b409936442a51a71f4ee96d980e1fc6a343a05bd4ed7fbc,IMAGE,linux,x86_64,"",0,CASE,"",""' \
        >> $CASE_PATH/ibm-watson-speech-4.0.5-images.csv
        ```
        {: codeblock}

    -   For the Latin American Spanish telephony model (`es-LA_Telephony`):

        ```sh
        echo 'cp.icr.io,cp/watson-speech/es-la-telephony,2022-01-05-405models,sha256:58e8c04abe9659472e89bf0778b7dc66e0ddceb4ea18d9d3e048a08c72125ea2,IMAGE,linux,x86_64,"",0,CASE,"",""' \
        >> $CASE_PATH/ibm-watson-speech-4.0.5-images.csv
        ```
        {: codeblock}

    -   For the Australian English multimedia model (`en-AU_Multimedia`):

        ```sh
        echo 'cp.icr.io,cp/watson-speech/en-au-multimedia,2022-01-05-405models,sha256:167f9a76258530a56a6abdd1c311f2ea05d6820ee0e802fbf2f96f08fb8a7646,IMAGE,linux,x86_64,"",0,CASE,"",""' \
        >> $CASE_PATH/ibm-watson-speech-4.0.5-images.csv
        ```
        {: codeblock}

    -   For the UK English multimedia model (`en-GB_Multimedia`):

        ```sh
        echo 'cp.icr.io,cp/watson-speech/en-gb-multimedia,2022-01-05-405models,sha256:167f9a76258530a56a6abdd1c311f2ea05d6820ee0e802fbf2f96f08fb8a7646,IMAGE,linux,x86_64,"",0,CASE,"",""' \
        >> $CASE_PATH/ibm-watson-speech-4.0.5-images.csv
        ```
        {: codeblock}

License Server is now automatically installed
:   The Speech services operator now automatically installs the required License Server when it installs the Speech services. You no longer need to install the License Server from the {{site.data.keyword.icp4dfull_notm}} foundational services, and you no longer need to use additional YAML content to create an OperandRequest with the necessary bindings.

Removal of steps specific to PostgreSQL EnterpriseDB server
:   The previous version of the documentation included steps for the PostgreSQL EnterpriseDB server that were specific to the Speech services. These steps were documented in the topics *Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} (Version 4.0)* and *Uninstalling {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}*. These additional steps are no longer necessary and have been removed from the documentation.

Security vulnerabilities addressed
:   The following security vulnerabilities associated with Apache Log4j have been addressed:
    -   [Security Bulletin: Vulnerability in Apache Log4j may affect IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-4104)](https://www.ibm.com/support/pages/node/6551170){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to denial of service and arbitrary code execution due to Apache Log4j (CVE-2021-45105 and CVE-2021-45046)](https://www.ibm.com/support/pages/node/6551168){: external}
    -   [Security Bulletin: Vulnerability in Apache Log4j may affect IBM Watson Speech Services Cartridge for {{site.data.keyword.icp4dfull_notm}} (CVE-2021-4428)](https://www.ibm.com/support/pages/node/6536732){: external} (this vulnerability was fixed with Speech services version 4.0.4)

RabbitMQ datastore is now used only by the `sttAsync` component
:   The RabbitMQ datastore was previously used by components of both Speech services, {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}}. It now handles non-persistent message queuing for the Speech to Text asynchronous HTTP component (`sttAsync`) only. It is used only if the `sttAsync` component is installed and enabled.

New next-generation models
:   The service now supports the following next-generation models with {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}:
    -   Chinese (Mandarin) telephony model (`zh-CN_Telephony`). The new model supports low latency.
    -   English (Australian) multimedia model (`en-AU_Multimedia`). The new model does not support low latency.
    -   English (UK) multimedia model (`en-GB_Multimedia`). The new model does not support low latency.
    -   Spanish (Latin American) telephony model (`es-LA_Telephony`). The new model supports low latency.

    **Note:** The Latin American Spanish model, `es-LA_Telephony`, applies to all Latin American dialects. It is the equivalent of the previous-generation models that are available for the Argentinian, Chilean, Colombian, Mexican, and Peruvian dialects. If you used a previous-generation model for any of these specific dialects, use the `es-LA_Telephony` model to migrate to the equivalent next-generation model.

    The new models are generally available for speech recognition. They are generally available for language model customization and beta for grammars. They are not supported for acoustic model customization.
    -   **Important:** If you are performing a mirrored installation (for example, in an air-gapped environment) and plan to install any of the new next-generation models for {{site.data.keyword.speechtotextshort}}, you must perform additional steps *before* mirroring the images. For more information, see the earlier release note.
    -   For more information about using the custom resource to install models, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.
    -   For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
    -   For more information about customization support for next-generation models, see [Language support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).

Next-generation US English models are now installed by default
:    The next-generation US English models, `en-US_Multimedia` and `en-US_Telephony`, are now installed by default with {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}. These models join `en-US_BroadbandModel`, `en-US_NarrowbandModel`, `en-US_ShortForm_NarrowbandModel` as the models that are installed by default. The models now have the following entries in the Speech services custom resource:

    ```yaml
    ########################################
    # Speech to Text next-generation models
    ########################################
          enUsMultimedia:    # US English (en-US) Multimedia model
            enabled: true
          enUsTelephony:     # US English (en-US) Telephony model
            enabled: true
    ```
    {: codeblock}

    For more information about using the custom resource to install models, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

## 20 December 2021 (Version 4.0.4)
{: #speech-to-text-data-20december2021}

Version 4.0.4 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.4 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Important: Changes to properties for disabling the storage and logging of user data
:   **Important:** The names of the properties of the Speech services custom resource that specify whether user data is stored and logged have changed. The custom resource formerly contained the following properties:

    ```yaml
    #################
    # Anonymize logs
    #################
      sttRuntime:
        anonymizeLogs: "false"  # If true, disables storage and logging of user data
      sttAMPatcher:
        anonymizeLogs: "false"  # If true, disables storage and logging of user data
      ttsRuntime:
        anonymizeLogs: "false"  # If true, disables storage and logging of user data
    ```
    {: codeblock}

    These properties are now named as follows:

    ```yaml
    ###################################
    # Storage and logging of user data
    ###################################
      sttRuntime:
        skipAudioAndResultLogging: "false"  # If true, disables storage and logging of user data
      sttAMPatcher:
        skipAudioAndResultLogging: "false"  # If true, disables storage and logging of user data
      ttsRuntime:
        skipAudioAndResultLogging: "false"  # If true, disables storage and logging of user data
    ```
    {: codeblock}

    If you already set these properties in your custom resource to change the default value of `false` to `true`, you need to edit your custom resource. You must manually change the names of the properties to the new values and save the updated custom resource. For more information, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Important: Changes to properties of PostgreSQL secrets object
:   **Important:** When you install the Speech services, an object that contains a randomly generated password for the PostgreSQL datastore is created by default. You can choose instead to specify the password manually. If you do, the properties of the YAML file for the secrets object have changed. For more information, see the topic about managing your datastores in [Administering {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-administering-watson-speech){: external}.

Important: PostgreSQL pods do not start with EnterpriseDB version 1.10 operator
:   **Important:** With {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.3, PostgreSQL pods based on the EnterpriseDB version 1.10 operator can fail to start. This prevents the Speech services from starting. A workaround exists for this problem. If your Speech services fail to start, see [PostgreSQL pods do not start with EnterpriseDB version 1.10 operator](https://www.ibm.com/support/pages/node/6525340){: external} for information about diagnosing and resolving the problem.

    This problem is fixed in {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.4.

New support for IBM Spectrum Scale Container Native storage class
:   Since version 4.0.3, the Speech services support the IBM Spectrum® Scale Container Native storage class. To use IBM Spectrum Scale, specify `"ibm-spectrum-scale-sc"` for the `storageClass` property of the Speech services custom resource. For more information, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Interaction of Speech services with MinIO datastore during installation
:   The Speech services runtime components, `sttRuntime` and `ttsRuntime`, cannot start until the models and voices for the services are fully uploaded into the MinIO datastore. During installation, the services might fail and automatically restart themselves one or more times until upload of the models and voices is complete. They then start properly. No user action is required.

Defect fix for upgrade documentation
:   **Defect fix:** Documentation for upgrading the Speech services to new versions of {{site.data.keyword.icp4dfull_notm}} version 4.0.x included incorrect references in some commands. These references are now correct:
    -   The strings `watsonSpeechToTextStatus` and `watsonTextToSpeechStatus` have been changed to `speechStatus` in both cases.
    -   The strings `status.watsonSpeechToTextVersion` and `status.watsonTextToSpeechVersion` have been changed to `.spec.version` in both cases.

    For more information, see [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}.

Important: Custom language models based on certain next-generation models must be re-created
:   **Important:** If you created custom language models based on certain next-generation models, you must re-create the custom models. Until you re-create the custom language models, speech recognition requests that attempt to use the custom models fail with HTTP error code 400.

    You need to re-create custom language models that you created based on the following versions of next-generation models:
    -   For the `en-AU_Telephony` model, custom models that you created from `en-AU_Telephony.v2021-03-03` to `en-AU_Telephony.v2021-10-04`.
    -   For the `en-GB_Telephony` model, custom models that you created from `en-GB_Telephony.v2021-03-03` to `en-GB_Telephony.v2021-10-04`.
    -   For the `en-US_Telephony` model, custom models that you created from `en-US_Telephony.v2021-06-17` to `en-US_Telephony.v2021-10-04`.
    -   For the `en-US_Multimedia` model, custom models that you created from `en-US_Multimedia.v2021-03-03` to `en-US_Multimedia.v2021-10-04`.

    **To identify the version of a model on which a custom language model is based,** use the `GET /v1/customizations` method to list all of your custom language models or the `GET /v1/customizations/{customization_id}` method to list a specific custom language model. The `versions` field of the output shows the base model for a custom language model. For more information, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).

    **To re-create a custom language model,** first create a new custom model. Then add all of the previous custom model's corpora and custom words to the new model. You can then delete the previous custom model. For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate).

Updates to multiple next-generation models for improved speech recognition
:   The following next-generation models have been updated for improved speech recognition:
    -   Australian English telephony model (`en-AU_Telephony`)
    -   UK English telephony model (`en-GB_Telephony`)
    -   US English multimedia model (`en-US_Multimedia`)
    -   US English telephony model (`en-US_Telephony`)
    -   Castilian Spanish telephony model (`es-ES_Telephony`)

    For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

New beta grammar support for next-generation models
:   Grammar support is now available as beta functionality for all available next-generation models. All next-generation models are generally available (GA) and support language model customization. For more information, see the following topics:
    -   For more information about the status of grammar support for next-generation models, see [Language support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).
    -   For more information about grammars, see [Grammars](/docs/speech-to-text?topic=speech-to-text-customization#grammars-intro).

New `custom_acoustic_model` field for supported features
:   The `GET /v1/models` and `GET /v1/models/{model_id}` methods now report whether a model supports acoustic model customization. The `SupportedFeatures` object now includes an additional field, `custom_acoustic_model`, a boolean that is `true` for a model that supports acoustic model customization and `false` otherwise. Currently, the field is `true` for all previous-generation models and `false` for all next-generation models.
    -   For more information about these methods, see [Listing information about models](/docs/speech-to-text?topic=speech-to-text-models-list).
    -   For more information about support for acoustic model customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

## 20 December 2021 (Version 1.2.x)
{: #speech-to-text-data-20december2021-12}

Important: You can no longer install {{site.data.keyword.speechtotextshort}} version 1.2.x on {{site.data.keyword.icp4dfull_notm}} version 3.5
:   **Important:** You can no longer perform new installations of {{site.data.keyword.speechtotextshort}} version 1.2.x on {{site.data.keyword.icp4dfull_notm}} version 3.5. You can install only {{site.data.keyword.speechtotextshort}} version 4.0.x on {{site.data.keyword.icp4dfull_notm}} version 4.x. For more information, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

    The Speech services for {{site.data.keyword.icp4dfull_notm}} version 3.5 reach their End of Support date on 30 April 2022. You are encouraged to upgrade to the latest version 4.0.x release of the services at your earliest convenience. For more information, see [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}.

## 30 November 2021 (Version 4.0.3)
{: #speech-to-text-data-30november2021}

Version 4.0.3 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.3 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

License Server now a mandatory prerequisite
:   You must now install the License Server from the {{site.data.keyword.icp4dfull_notm}} foundational services. You must install the License Server by using the YAML content that is provided to create an OperandRequest with the necessary bindings. You must also install the License Service in the same namespace as the service (operand), which is also where {{site.data.keyword.icp4dfull_notm}} is installed. For more information, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

New support for in-place upgrade
:   The service now supports in-place, operator-based upgrade from version 4.0.0 to version 4.0.3. Moving from {{site.data.keyword.icp4dfull_notm}} version 3.5 to version 4.0.3 continues to require use of migration utilities. For more information, see [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}.

EDB PostgreSQL operator and license installation changes
:   Installation, upgrade, and uninstallation for the Enterprise DB PostgreSQL operator and license have changed:
    -   Instructions for installing the EDB PostgreSQL operator and license are now included with the {{site.data.keyword.icp4dfull_notm}} foundational services. The instructions for installing the Speech services have been updated accordingly. For more information, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.
    -   Instructions for upgrading from {{site.data.keyword.speechtotextshort}} version 4.0.0 to 4.0.3 include instructions for uninstalling the previous EDB PostgreSQL operator and license and reinstalling them with the {{site.data.keyword.icp4dfull_notm}} foundational services. For more information, see [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}.
    -   Instructions for uninstalling the Speech services now include steps for removing the EDB PostgreSQL operator and license that were previously installed with  {{site.data.keyword.speechtotextshort}}. For more information, see [Uninstalling {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-uninstalling-watson-speech){: external}.

New guidance for scaling up your installation
:   The service now provides updated guidance about scaling up your installation. The information includes specifying the number of pods, the number of CPUs allocated per pod, and the maximum number of concurrent sessions with previous- and next-generation models. For more information, see [Administering {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-administering-watson-speech){: external}.

Command-line updates to import and export utilities
:   The commands that are used with the import and export utilities for the Speech services include new options and arguments. The import and export utilities are also the foundation for backing up and restoring the services and for migrating from {{site.data.keyword.icp4dfull_notm}} version 3.5 to version 4.0.3. For more information about using the utilities, see

    -   [Administering {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-administering-watson-speech){: external}
    -   [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-upgrading-watson-speech){: external}

New property for specifying the CPUs for acoustic model training
:   The `sttAMPatcher` microservice manages acoustic model customization for the service. The AM Patcher uses a dedicated number of CPUs to handle requests. You can use the new `sttAMPatcher.resources.requestsCPU` property to increase the number of CPUs that are dedicated to handling acoustic model training requests by the AM Patcher. This may be necessary if you experience training failures during acoustic model training. For more information, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

New next-generation models
:   The service now supports the following new next-generation language models. All of the new models are generally available.
    -   Czech: `cs-CZ_Telephony`. The model supports low latency.
    -   Belgian Dutch (Flemish): `nl-BE_Telephony`. The model supports low latency.
    -   French: `fr-FR_Multimedia`. The new model does not support low latency.
    -   Indian English: `en-IN_Telephony`. The model supports low latency.
    -   Indian Hindi: `hi-IN_Telephony`. The model supports low latency.
    -   Japanese: `ja-JP_Multimedia`. The model does not support low latency.
    -   Korean: `ko-KR_Multimedia`. The model does not support low latency.
    -   Korean: `ko-KR_Telephony`. The model supports low latency.
    -   Netherlands Dutch: `nl-NL_Telephony`. The model supports low latency.

    For more information about all next-generation models and about low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

Updates to next-generation models
:   The following next-generation models have been updated for improved speech recognition. All of the models are generally available.
    -   Arabic: `ar-MS_Telephony`. The model now supports low latency.
    -   Brazilian Portuguese: `pt-BR_Telephony`. The model continues to support low latency.
    -   US English: `en-US_Telephony`. The model continues to support low latency.
    -   Canadian French: `fr-CA_Telephony`. The model now supports low latency.
    -   Italian: `it-IT_Telephony`. The model now supports low latency.

    For more information about all next-generation models and about low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

Defect fix for asynchronous HTTP failures
:   **Defect fix:** The asynchronous HTTP interface failed to transcribe some audio. In addition, the callback for the request returned status `recognitions.completed_with_results` instead of `recognitions.failed`. This error has been resolved.

Defect fix for speakers labels
:   **Defect fix:** When you use speakers labels with next-generation models, the service now identifies the speaker for all words of the input audio, including very short words that have the same start and end timestamps.

Defect fix for interim results and low-latency documentation
:   **Defect fix:** Documentation that describes the interim results and low-latency features with next-generation models has been rewritten for clarity and correctness. For more information, see the following topics:
    -   [Interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim), especially [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency)
    -   [How the service sends recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#ws-results)

Defect fix for multitenancy documentation
:   The {{site.data.keyword.icp4dfull_notm}} topic [Multitenancy support](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=planning-multitenancy-support){: external} incorrectly stated that the Speech services do not support multitenancy. The topic has been updated to state that the Speech services support the following operations:

    -   Install the service in separate projects
    -   Install the service multiple times in the same project
    -   Install the service once and deploy multiple instances in the same project

    The documentation that is specific to the Speech services correctly stated the multitenancy support.

## 1 October 2021 (Version 1.1.x)
{: #speech-to-text-data-1october2021}

Version 1.1.x is out of service
:   {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.x went out of service on 30 September 2021. As of 1 October 2021, the documentation for version 1.1.x is no longer available. For more information, see [Software withdrawal and support discontinuance](https://www.ibm.com/common/ssi/ShowDoc.wss?docURL=/common/ssi/rep_ca/9/899/ENUSLP21-0099/index.html&request_locale=en){: external}.

## 31 August 2021 (Version 4.0.0)
{: #speech-to-text-data-31august2021}

All next-generation models are now generally available
:   All next-generation language models are now generally available (GA). They are supported for use in production environments and applications.
    -   For more information about all next-generation language models and which models are currently available for {{site.data.keyword.icp4dfull_notm}}, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
    -   For more information about the features that are supported for each next-generation model, see [Supported features for next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features).

Language model customization for next-generation models is now generally available
:   Language model customization is now generally available (GA) for all available next-generation languages and models. Language model customization for next-generation models is supported for use in production environments and applications.

    You use the same commands to create, manage, and use custom language models, corpora, and custom words for next-generation models as you do for previous-generation models. But customization for next-generation models works differently from customization for previous-generation models. For custom models that are based on next-generation models:
    -   The custom models have no concept of out-of-vocabulary (OOV) words.
    -   Words from corpora are not added to the words resource.
    -   You cannot currently use the sounds-like feature for custom words.
    -   You do not need to upgrade custom models when base language models are updated.
    -   Grammars are not currently supported.

    For more information about using language model customization for next-generation models, see
    -   [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)
    -   [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate)
    -   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)
    -   [Working with corpora and custom words for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng)

    Additional topics describe managing custom language models, corpora, and custom words.

## 29 July 2021 (Version 4.0.0)
{: #speech-to-text-data-29july2021}

Version 4.0.0 is available
:   {{site.data.keyword.speechtotextfull}} for {{site.data.keyword.icp4dfull}} version 4.0.0 is now available.  Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift version 4.6. For more information about installing and managing the service, see [Installing {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}](/docs/speech-to-text?topic=speech-to-text-speech-install-data).

New next-generation language models
:   The service now supports a growing number of next-generation language models. The next-generation *multimedia* and *telephony* models improve upon the speech recognition capabilities of the service's previous generation of broadband and narrowband models. The new models leverage deep neural networks and bidirectional analysis to achieve both higher throughput and greater transcription accuracy.

    At this time, the next-generation language models and the `low_latency` parameter are beta functionality. The next-generation models support a limited number of languages and speech recognition features. The supported languages, models, and features will increase with future releases.

    Many of the next-generation models also support a new `low_latency` parameter that lets you request faster results at the possible expense of reduced transcription quality. When low latency is enabled, the service curtails its analysis of the audio, which can reduce the accuracy of the transcription. This trade-off might be acceptable if your application requires lower response time more than it does the highest possible accuracy.

    The `low_latency` parameter impacts your use of the `interim_results` parameter with the WebSocket interface. Interim results are available only for those next-generation models that support low latency, and only if both the `interim_results` and `low_latency` parameters are set to `true`.

    -   For more information about the next-generation models and their capabilities, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
    -   For more information about language support for next-generation models and about which next-generation models support low latency, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).
    -   For more information about feature support for next-generation models, see [Supported features for next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features) and [Unsupported features for next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-unsupported).
    -   For more information about the `low_latency` parameter, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).
    -   For more information about the interaction between the `low_latency` and `interim_results` parameters for next-generation models, see [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency).

Arabic language broadband model renamed
:   The Arabic language broadband model is now named `ar-MS_BroadbandModel`. The former name, `ar-AR_BroadbandModel`, is deprecated. It will continue to function for at least one year but might be removed at a future date. You are encouraged to migrate to the new name at your earliest convenience.

Unified {{site.data.keyword.speechtotextshort}} documentation
:   The documentation for {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} is now combined with the documentation for managed instances of the {{site.data.keyword.speechtotextshort}} service that are hosted on {{site.data.keyword.cloud_notm}}. This is true of both the guide and reference documentation for the two forms of the service. Links to the formerly separate version of the {{site.data.keyword.icp4dfull_notm}} documentation for the service redirect to the unified documentation.

    For more information about identifying information that pertains to only one version of the product, see [About {{site.data.keyword.speechtotextshort}}](/docs/speech-to-text?topic=speech-to-text-about#about-version).

Defect fixes for documentation
:   **Defect fixes:**  The documentation has been updated to correct the following information:

    -   The documentation incorrectly stated that using the `smart_formatting` parameter causes the service to remove hesitation markers from final transcription results for Japanese. Smart formatting does not remove hesitation markers from final results for Japanese, only for US English. For more information, see [What results does smart formatting affect?](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-effects)
    -   The documentation failed to state that next-generation models do *not* produce hesitation markers. The documentation has been updated to note that only previous-generation models produce hesitation markers. For more information, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

Version 1.1.x is going out of service
:   {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.x go out of service on **30 September 2021**. You must upgrade to a later version of the services on {{site.data.keyword.icp4dfull_notm}} before that date. As of 1 October 2021, the documentation for version 1.1.4 will no longer be available.

## 12 April 2021 (Version 1.2.1)
{: #speech-to-text-data-12april2021}

Addition to `speech-override.yaml` file
:   The minimal `speech-override.yaml` file includes an extra definition, `dockerRegistryPrefix`:

    ```yaml
    global:
      dockerRegistryPrefix: "{Registry}"
      image:
        pullSecret: "{Registry_pull_secret}"
    ```
    {: codeblock}

    `{Registry}` is the path for the internal Docker registry. It must be `image-registry.openshift-image-registry.svc:5000/{namespace}`, where `{namespace}` is the namespace in which {{site.data.keyword.icp4dfull}} is installed, normally `zen`. For more information, see [The speech-override.yaml file](/docs/speech-to-text?topic=speech-to-text-speech-override-12#speech-override-file-12).

## 9 April 2021 (Version 1.2.1)
{: #speech-to-text-data-9april2021}

Support for modifying installed models and voices
:   The Speech services let you add or remove installed models and voices for version 1.2 or 1.2.1 of the services. For more information, see [Modifying the installed models and voices](/docs/speech-to-text?topic=speech-to-text-speech-cluster-12#speech-cluster-models-voices-12).

## Version 1.2.1 (26 March 2021)
{: #speech-to-text-data-26march2021}

Version 1.2.1 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.2.1 is now available. Versions 1.2 and 1.2.1 use the same version 1.2 documentation and installation instructions. Version 1.2.1 supports installation on Red Hat OpenShift version 4.6 in addition to versions 4.5 and 3.11.

New installation instructions
:   For both clusters connected to the internet and air-gapped clusters, the installation instructions include the following steps:
    -   Use the `oc label` command to set up required labels for the namespace where {{site.data.keyword.icp4dfull_notm}} is installed.
    -   Use the `oc project` command to ensure that you are pointing at the correct OpenShift project.
    -   Use the `cpd-cli install` command to install an Enterprise DB PostgreSQL server that is used by the Speech services.

    You perform these steps before you install the Speech services. For more information, see [Installing the {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-install.html){: external}.

New uninstallation instructions
:   A step was added to the procedure for uninstalling the Speech services to clean up all of the resources from the installation. For more information, see [Uninstalling {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-uninstall.html){: external}.

Entitled registry for PostgreSQL datastore
:   The entitled registry path from which the service pulls images for the PostgreSQL datastore has changed. The registry location changed from `cp.icr.io/cp/watson-speech` to `cp.icr.io/cp/cpd`. This change is transparent to users.

Secrets for Minio and PostgreSQL datastores
:   The Minio and PostgreSQL datastores require the following hard-coded values for their secrets:
    -   For *Minio*, use `minio`.
    -   For *PostgreSQL*, use `user-provided-postgressql`.

    You cannot use your own values for these secrets. The secrets must be created before you install the Speech services.

Deletions from `speech-override.yaml` file
:   The following entries have been removed from the `speech-override.yaml` file. They were added to work around a problem that has now been fixed.

    ```yaml
    sttRuntime:
      images:
        miniomc:
          tag:
            1.0.5
    sttAMPatcher:
      images:
        miniomc:
          tag:
            1.0.5
    ttsRuntime:
      images:
        miniomc:
          tag:
            1.0.5
    ```
    {: codeblock}


    The abbreviated `speech-override.yaml` file has generally been reduced further by fine-tuning its contents to the essential elements. The updated version of the file appears in the following sections:
    -   [Creating an override file for {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} installation](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-override.html){: external}
    -   [Using the override file](/docs/speech-to-text?topic=speech-to-text-speech-override-12)

    You can download a complete version of the [speech-override.yaml](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/cpd-version-12/speech-override.yaml){: external} file. The complete version includes all of the detailed elements described in [Using the override file](/docs/speech-to-text?topic=speech-to-text-speech-override-12).

## Version 1.2 (9 December 2020)
{: #speech-to-text-data-9december2020}

Version 1.2 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.2 is now available. Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} versions 3.5 and 3.0.1, and Red Hat OpenShift versions 4.5 and 3.11. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} version 1.2](/docs/speech-to-text?topic=speech-to-text-speech-install-12).

New Australian and French Canadian models
:   The service now offers broadband and narrowband models for Australian English and Canadian French:
    -   Australian English: `en-AU_BroadbandModel` and `en-AU_NarrowbandModel`
    -   Canadian French: `fr-CA_BroadbandModel` and `fr-CA_NarrowbandModel`

    The new models are generally available, and they support both language model and acoustic model customization.
    -   For more information about supported languages and models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).
    -   For more information about language support for customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

Updated models for improved speech recognition
:   The following language models have been updated for improved speech recognition:
    -   Brazilian Portuguese: `pt-BR_BroadbandModel` and `pt-BR_NarrowbandModel`
    -   French: `fr-FR_BroadbandModel`
    -   German: `de-DE_BroadbandModel` and `de-DE_NarrowbandModel`
    -   Japanese: `ja-JP_BroadbandModel`
    -   UK English: `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`
    -   US English: `en-US_ShortForm_NarrowbandModel`

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

The `split_transcript_at_phrase_end` parameter is now generally available for all languages
:   The speech recognition parameter `split_transcript_at_phrase_end` is now generally available for all languages. Previously, it was generally available only for US and UK English. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

Hesitation marker for German has changed
:   The hesitation marker that is used for the updated German broadband and narrowband models has changed from `[hesitation]` to `%HESITATION`. For more information about hesitation markers, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

Defect fix to address latency issue for models with large numbers of grammars
:   **Defect fix:** The service no longer has a latency issue for custom language models that contain a large number of grammars. When initially used for speech recognition, such custom models could take multiple seconds to load. The custom models now load much faster, greatly reducing latency when they are used for recognition.

## 15 July 2020 (Version 1.1.4)
{: #speech-to-text-data-15july2020}

Red Hat OpenShift version 4.3 is going out of service
:   {{site.data.keyword.icp4dfull_notm}} 3.0.1 is deprecating support for Red Hat OpenShift 4.3 on 1 September 2020. Red Hat OpenShift 4.3 is going out of service on **22 October 2020**. {{site.data.keyword.icp4dfull_notm}} is introducing support for Red Hat OpenShift 4.5. {{site.data.keyword.icp4dfull_notm}} is recommending that clients upgrade to Red Hat OpenShift 4.5 before 22 October 2020. IBM Support will work with any customers who already installed {{site.data.keyword.icp4dfull_notm}} 3.0.1 on Red Hat OpenShift 4.3. New customers who want to install on Red Hat OpenShift 4.x are instructed to install Red Hat OpenShift 4.5.

## 19 June 2020 (Version 1.1.4)
{: #speech-to-text-data-19june2020}

Version 1.1.4 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.4 is now available. Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} versions 2.5 and 3.0.1, and Red Hat OpenShift versions 3.11 and 4.3. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} version 1.1.4](/docs/speech-to-text?topic=speech-to-text-speech-install).

New parameters to control the level of speech activity detection
:   The service now offers two new optional parameters for controlling the level of speech activity detection. The parameters can help ensure that only relevant audio is processed for speech recognition.
    -   The `speech_detector_sensitivity` parameter adjusts the sensitivity of speech activity detection. You can use the parameter to suppress word insertions from music, coughing, and other non-speech events.
    -   The `background_audio_suppression` parameter suppresses background audio based on its volume to prevent it from being transcribed or otherwise interfering with speech recognition. You can use the parameter to suppress side conversations or background noise.

    You can use the parameters individually or together. They are available for all interfaces and for most language models. For more information about the parameters, their allowable values, and their effect on the quality and latency of speech recognition, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-detection).

New broadband and narrowband models for Dutch and Italian
:   The service now supports broadband and narrowband models for the Dutch and Italian languages:
    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)

    Dutch and Italian language models are generally available (GA) for speech recognition and for language model and acoustic model customization. For more information about all available language models, see
    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)

Support for `speaker_labels` parameter for German and Korean
:   The service now supports speaker labels (the `speaker_labels` parameter) for German and Korean language models. Speaker labels identify which individuals spoke which words in a multi-participant exchange. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

Improved speech recognition for Japanese narrowband model
:   The Japanese narrowband model (`ja-JP_NarrowbandModel`) now includes some multigram word units for digits and decimal fractions. The service returns these multigram units regardless of whether you enable smart formatting. The smart formatting feature understands and returns the multigram units that the model generates. If you apply your own post-processing to transcription results, you need to handle these units appropriately. For more information, see [Japanese](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-japanese) in the smart formatting documentation.

Simplified backup and restore
:   The service now offers greatly improved backup and restore procedures. Utilities are now available to back up data from your datastores, so you no longer need to re-create all of your data in the event of a disaster. For more information, see [Backing up and restoring your data](/docs/speech-to-text?topic=speech-to-text-speech-backup).

## 1 April 2020 (Version 1.1.3)
{: #speech-to-text-data-1april2020}

Acoustic model customization is now generally available
:   Acoustic model customization is now generally available (GA) for all supported languages. For more information about support for individual language models, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

## 28 February 2020 (Version 1.1.3)
{: #speech-to-text-data-28february2020}

Version 1.1.3 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.3 is now available.

New `end_of_phrase_silence_time` parameter
:   For speech recognition, the service now supports the `end_of_phrase_silence_time` parameter. The parameter specifies the duration of the pause interval at which the service splits a transcript into multiple final results. Each final result indicates a pause or extended silence that exceeds the pause interval. For most languages, the default pause interval is 0.8 seconds; for Chinese the default interval is 0.6 seconds.

    You can use the parameter to effect a trade-off between how often a final result is produced and the accuracy of the transcription. Increase the interval when accuracy is more important than latency. Decrease the interval when the speaker is expected to say short phrases or single words.

    For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).

New `split_transcript_at_phrase_end` parameter
:   For speech recognition, the service now supports the `split_transcript_at_phrase_end` parameter. The parameter directs the service to split the transcript into multiple final results based on semantic features of the input, such as at the conclusion of sentences. The service bases its understanding of semantic features on the base language model that you use with a request. Custom language models and grammars can also influence how and where the service splits a transcript.

    The parameter causes the service to add an `end_of_utterance` field to each final result to indicate the motivation for the split: `full_stop`, `silence`, `end_of_data`, or `reset`.

    For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

Improved `speaker_labels` parameter
:   For speech recognition, the `speaker_labels` parameter has been updated to improve the identification of individual speakers for further analysis of your audio sample. For more information about the speaker labels feature, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels). For more information about the improvements to the feature, see [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://www.ibm.com/blogs/research/2020/07/speaker-diarization-in-real-use-cases/){: external}.

## 27 November 2019 (Version 1.1.2)
{: #speech-to-text-data-27november2019}

Version 1.1.2 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.2 is now available.

Maximum number of custom models
:   You can create no more than 1024 custom language models and no more than 1024 custom acoustic models per owning credentials. For more information, see [Maximum number of custom models](/docs/speech-to-text?topic=speech-to-text-customization#customMaximum).

## 30 August 2019 (Version 1.0.1)
{: #speech-to-text-data-30august2019}

Version 1.0.1 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.0.1 is now available. The service now works with {{site.data.keyword.icp4dfull_notm}} 2.1.0.1. The service now supports installing {{site.data.keyword.icp4dfull_notm}} with Red Hat OpenShift.

New broadband and narrowband models for Spanish dialects
:   The service now offers broadband and narrowband language models in six Spanish dialects:

    -   Argentinian Spanish (`es-AR_BroadbandModel` and `es-AR_NarrowbandModel`)
    -   Castilian Spanish (`es-ES_BroadbandModel` and `es-ES_NarrowbandModel`)
    -   Chilean Spanish (`es-CL_BroadbandModel` and `es-CL_NarrowbandModel`)
    -   Colombian Spanish (`es-CO_BroadbandModel` and `es-CO_NarrowbandModel`)
    -   Mexican Spanish (`es-MX_BroadbandModel` and `es-MX_NarrowbandModel`)
    -   Peruvian Spanish (`es-PE_BroadbandModel` and `es-PE_NarrowbandModel`)

    The Castilian Spanish models are not new. They are generally available for speech recognition and language model customization, and beta for acoustic model customization.

    The models for the other five dialects are new and are beta for all uses. Because they are beta, these additional dialects might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

    For more information, see the following sections:

    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)

FISMA support
:   Federal Information Security Management Act (FISMA) support is now available for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}. The service is FISMA High Ready.

## 28 June 2019 (Version 1.0.0)
{: #speech-to-text-data-28june2019}

Version 1.0.0 is available
:   Version 1.0.0, the initial release of the service, is now available. {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} is based on the {{site.data.keyword.speechtotextfull}} service on the public {{site.data.keyword.cloud_notm}}. {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} differs from the public {{site.data.keyword.speechtotextshort}} service in the following ways. You might find this information helpful if you are already familiar with the {{site.data.keyword.speechtotextshort}} service on the public {{site.data.keyword.cloud_notm}}.

    -   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} uses access tokens for authentication. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
    -   The endpoints for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} are specific to your {{site.data.keyword.icp4dfull_notm}} cluster. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
    -   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} does not perform any request logging. You do not need to use the `X-Watson-Learning-Opt-Out` request header.
    -   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} does not support Watson tokens. You cannot use the `X-Watson-Authorization-Token` request header to authenticate with the service.
