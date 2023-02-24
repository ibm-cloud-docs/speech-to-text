---

copyright:
  years: 2018, 2023
lastupdated: "2023-02-24"

keywords: speech to text release notes,speech to text for IBM cloud pak for data release notes

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}
{: #release-notes-data}

[IBM Cloud Pak for Data]{: tag-cp4d}

The following features and changes were included for each release and update of installed or on-premises instances of {{site.data.keyword.speechtotextfull}} for {{site.data.keyword.icp4dfull_notm}}. Unless otherwise noted, all changes are compatible with earlier releases and are automatically and transparently available to all new and existing applications.
{: shortdesc}

For information about known limitations of the service, see [Known limitations](/docs/speech-to-text?topic=speech-to-text-known-limitations).

For information about releases and updates of the service for {{site.data.keyword.cloud_notm}}, see [Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.cloud_notm}}](/docs/speech-to-text?topic=speech-to-text-release-notes).
{: note}

## 23 February 2023 (Version 4.6.3)
{: #speech-to-text-data-23february2023}

Version 4.6.3 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.6.3 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.6.x and Red Hat OpenShift version 4.10. Red Hat OpenShift version 4.8 is no longer supported. For more information, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Important: All previous-generation models are deprecated and will reach end of service on 31 July 2023
:   **Important:** All previous-generation models are deprecated and will reach end of service effective **31 July 2023**. On that date, all previous-generation models will be removed from the service and the documentation. The previous deprecation date was 3 March 2023. The new date allows users more time to migrate to the appropriate next-generation models. But users must migrate to the equivalent next-generation model by 31 July 2023.

    Most previous-generation models were deprecated on 15 March 2022. Previously, the Arabic and Japanese models were not deprecated. Deprecation now applies to *all* previous-generation models.
    -   For more information about the next-generation models to which you can migrate from each of the deprecated models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
    -   For more information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
    -   For more information about all next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)

    **Note:** When the previous-generation `en-US_BroadbandModel` is removed from service, the next-generation `en-US_Multimedia` model will become the default model for speech recognition requests.

Known issue: You cannot change the installed models and voices with the advanced installation options
:   **Known issue:** You currently cannot specify different models or voices with the advanced installation options. The service always installs the defaults models and voices. For information about changing the models after installation, see *Updating models and voices for your Watson Speech services* in the *Administration* topic of [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Known issue: Upgrade to version 4.6.3 can fail to complete
:   **Known issue:**  When upgrading to version 4.6.3, the MinIO backup job can fail to be deleted upon completion. If this happens, the solution is to delete the job, after which the upgrade proceeds normally. Perform the following steps to resolve the problem.

    1.  To determine whether the MinIO backup job remains undeleted, issue the following command:

        ```sh
        oc get job --namespace {${PROJECT_CPD_INSTANCE} | grep speech-cr-ibm-minio-backup
        ```
        {: pre}

        The MinIO job that is not deleted is identified by an entry of the following form:

        ```text
        speech-cr-ibm-minio-backup   1/1   3m25s   1d
        ```
        {: codeblock}

    1.  To delete the MinIO backup job, issue the following command:

        ```sh
        oc delete job speech-cr-ibm-minio-backup --namespace ${PROJECT_CPD_INSTANCE}
        ```
        {: pre}

    Once the backup job is deleted, upgrade continues and completes.

Defect fix: Update French Canadian next-generation telephony model (upgrade required)
:   **Defect fix:** The French Canadian next-generation telephony model, `fr-CA_Telephony`, was updated to address an internal inconsistency that could cause an error during speech recognition. *You need to upgrade any custom models that are based on the `fr-CA_Telephony` model.* For more information about upgrading custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

Defect fix: The next-generation Brazilian Portuguese multimedia model is now available
:   **Defect fix:** The next-generation Brazilian Portuguese multimedia model is now available for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}. Previously, the model was unavailable.

Adding words directly to custom models that are based on next-generation models increases the training time
:   Adding custom words directly to a custom model that is based on a next-generation model causes training of a model to take a few minutes longer than it otherwise would. If you are training a model with custom words that you added by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method, allow for some minutes of extra training time for the model. For more information, see
    -   [Add words to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addWords)
    -   [Monitoring the train model request](/docs/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language)

Additional information about working with service instances
:   The documentation now includes information about creating a service instance with the command-line interface (`cpl-cli`) and about managing service instances. For more information, see the following topics of [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}:
    -   *Creating a Watson Speech services instance* under *Post-installation setup*
    -   *Managing your Watson Speech services instances* under *Administering*

<!--
Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
-->

## 30 January 2023 (Version 4.6.2)
{: #speech-to-text-data-30january2023}

Version 4.6.2 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.6.2 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.6.x and Red Hat OpenShift versions 4.8 and 4.10. For more information, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

The custom resource now includes a new `fileStorageClass` property
:   The custom resource for the Watson Speech services now includes a `fileStorageClass` property in addition to the existing `blockStorageClass` property. You specify both block and file storage classes when you install or upgrade a service. During upgrade from a previous version, the new property is added automatically to the custom resource by the `--file_storage_class` option on `cli manage apply-cr` command.

    For more information about the available block and file storage classes you use with each of the supported storage solutions, see the table of *Storage requirements* under *Information you need to complete this task* on the page "Installing Watson Speech services" in [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Additional information about provisioning a service instance
:   The documentation now includes information about creating a service instance programmatically. It also includes examples of listing service instances and deleting a service instance. For more information, see *Creating a Watson Speech services instance* in the *Post-installation setup* documentation in [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Server-side encryption is enabled for the MinIO datastore
:   The Speech services have now enabled server-side encryption for object storage in the MinIO datastore. No action is required on your part.

Change to audit webhooks
:  The Speech services have now removed the audit webhook dependency. The services now write audit events directly to the server. After upgrading to version 4.6.2, some webhook resources might remain until all services can remove the dependency. The remaining resources will be removed in a future release. No action is required on your part.

New Netherlands Dutch next-generation multimedia model
:   The service now offers a next-generation multimedia model for Netherlands Dutch: `nl-NL_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

New Swedish next-generation telephony model
:   The service now offers a next-generation telephony model for Swedish: `sv-SE_Telephony`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Updates to English next-generation telephony models
:   The English next-generation telephony models have been updated for improved speech recognition:
    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`

    All of these models continue to support low latency. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

The `max_alternatives` parameter is now available for use with next-generation models
:   The `max_alternatives` parameter is now available for use with all next-generation models. The parameter is generally available for all next-generation models. For more information, see [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-metadata#max-alternatives).

Defect fix: Allow use of both `max_alternatives` and `end_of_phrase_silence_time` parameters with next-generation models

:   **Defect fix:** When you use both the `max_alternatives` and `end_of_phrase_silence_time` parameters in the same request with next-generation models, the service now returns multiple alternative transcripts while also respecting the indicated pause interval. Previously, use of the two parameters in a single request generated a failure. (Use of the `max_alternatives` parameter with next-generation models was previously available as an experimental feature to a limited number of customers.)

Defect fix: Update to Japanese next-generation multimedia model (upgrade required)
:   **Defect fix:** The Japanese next-generation multimedia model, `ja-JP_Multimedia`, was updated to address an internal inconsistency that could cause an error during speech recognition with low latency. *You need to upgrade any custom models that are based on the `ja-JP_Multimedia` model.* For more information about upgrading custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

Defect fix: Add documentation guidelines for creating Japanese sounds-likes based on next-generation models
:   **Defect fix:** In sounds-likes for Japanese custom language models that are based on next-generation models, the character-sequence `ウー` is ambiguous in some left contexts. Do not use characters (syllables) that end with the phoneme `/o/`, such as `ロ` and `ト`. In such cases, use `ウウ` or just `ウ` instead of `ウー`. For example, use `ロウウマン` or `ロウマン` instead of `ロウーマン`. For more information, see [Guidelines for Japanese](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordLanguages-jaJP-ng).

Defect fix: Correct use of `display_as` field in transcription results
:   **Defect fix:** For language model customization with next-generation models, the value of the `display_as` field for a custom word now appears in all transcripts. Previously, the value of the `word` field sometimes appeared in transcription results.

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to issues in OpenSSL (CVE-2022-1434, CVE-2022-1343, CVE-2022-1292, CVE-2022-1473)](https://www.ibm.com/support/pages/node/6890819){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary command execution in OpenSSL (CVE-2022-2068)](https://www.ibm.com/support/pages/node/6890831){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in protobuf (CVE-2022-1941)](https://www.ibm.com/support/pages/node/6890835){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a buffer overflow in GNU glibc (CVE-2021-3999)](https://www.ibm.com/support/pages/node/6890837){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security bypass in GNU gzip (CVE-2022-1271)](https://www.ibm.com/support/pages/node/6890841){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-27664)](https://www.ibm.com/support/pages/node/6890843){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-2879)](https://www.ibm.com/support/pages/node/6909749){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to query parameter smuggling in Golang Go (CVE-2022-2880)](https://www.ibm.com/support/pages/node/6890847){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-32189)](https://www.ibm.com/support/pages/node/6890849){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-41715)](https://www.ibm.com/support/pages/node/6890851){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to information exposure in OpenSSL (CVE-2022-2097)](https://www.ibm.com/support/pages/node/6890853){: external}

## 30 November 2022 (Version 4.6.0)
{: #speech-to-text-data-30november2022}

Version 4.6.0 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.6.0 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.6.x and Red Hat OpenShift versions 4.8 and 4.10. For more information, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Amazon Web Services (AWS) is now supported
:   {{site.data.keyword.watson}} Speech services for {{site.data.keyword.icp4dfull_notm}} are now supported on Amazon Web Services™ (AWS™). The services support Amazon Elastic Block Store, which you specify by setting the `blockStorageClass` property of the Speech services custom resource to `gp2-csi` or `gp3-csi`.

New storage classes are now supported
:   {{site.data.keyword.watson}} Speech services for {{site.data.keyword.icp4dfull_notm}} now support two additional storage classes:
    -   IBM Cloud Block Storage (`ibmc-block-gold`)
    -   NetApp Trident (`ontap-nas`)

    You specify the storage class with the `blockStorageClass` property of the Speech services custom resource. For more information about all supported storage classes, see the following topics in [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}:
    -   *Before you begin* in *Installing Watson Speech services*
    -   *Specifying a storage class* in *Using the Watson Speech services custom resource*

Known issue: Some Watson Speech services pods do not have annotations that are used for scheduling
:   **Known issue:** Some Watson Speech services pods are missing the `cloudpakInstanceId` annotation. If you use the {{site.data.keyword.icp4dfull_notm}} scheduling service, any Watson Speech services pods without the `cloudpakInstanceId` annotation are
    -   Scheduled by the default Kubernetes scheduler rather than the scheduling service
    -   Not included in the quota enforcement

Monitoring of the PostgreSQL datastore is now available
:   You can now enable monitoring of the PostgreSQL datastore to receive updates on its usage and status by the Watson Speech services. The events can be consumed by Prometheus monitoring software or whatever application you use for monitoring. By enabling monitoring for user-defined projects in addition to the default platform monitoring, you can monitor your own projects with the Red Hat® OpenShift® Container Platform monitoring stack. This capability includes an additional property, `spec.global.datastores.postgressql.enablePodMonitor`, in the Speech services custom resource.

    For more information, see the topic *Monitoring the PostgreSQL datastore for Watson Speech services* in the *Administering* section of [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Defect fix: PostgreSQL datastore is no longer installed if only runtime microservices are enabled
:   **Defect fix:** The PostgreSQL datastore is no longer installed if only the runtime microservices are enabled. The datastore is now installed only if at least one of the `sttAsync`, `sttCustomization`, or `ttsCustomization` microservices is installed. PostgreSQL is not uninstalled if at a later date these microservices are disabled.

    Prior to version 4.6.0, PostgreSQL was always installed with the Speech services. If you are an existing customer who used only the runtime microservices of the Speech services prior to version 4.6.0, PostgreSQL remains installed but is not used. In this case, installation of PostgreSQL persists across upgrades.

    The MinIO datastore is always installed because the runtime microservices depend on it. The RabbitMQ datastore is installed only if the `sttAsync` microservice is installed.

    For more information, see *Datastore properties* in *Using the Watson Speech services custom resource* in [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.6.x?topic=services-watson-speech){: external}.

Defect fix: Creation of a Network Policy is no longer necessary for the PostgreSQL operator to monitor its operands
:   **Defect fix:** For version 4.6.0, it is not necessary to create a Network Policy to allow the PostgreSQL operator to monitor its operands, as described in the [10 November 2022 (Versions 4.0.x and 4.5.x)](#speech-to-text-data-10november2022) service update. As of version 4.6.0, the service handles this situation automatically.

Defect fix: Some next-generation models were updated to improve low-latency response time
:   **Defect fix:** The following next-generation models were updated to improve their response time when the `low_latency` parameter is used:
    -   `en-IN_Telephony`
    -   `hi-IN_Telephony`
    -   `it-IT_Multimedia`
    -   `nl-NL_Telephony`

    Previously, these models did not return recognition results as quickly as expected when the `low_latency` parameter was used. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Defect fix: Improve custom model naming documentation
:   **Defect fix:** The documentation now provides detailed rules for naming custom language models and custom acoustic models. For more information, see
    -   [Create a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)
    -   [Create a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
    -   [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a cross-configuration attack against OpenPGP (CVE-2021-40528)](https://www.ibm.com/support/pages/node/6843867){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary code execution in PCRE2 (CVE-2022-1586)](https://www.ibm.com/support/pages/node/6843869){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Vim (CVE-2022-1621)](https://www.ibm.com/support/pages/node/6843871){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a buffer overflow in Vim (CVE-2022-1629)](https://www.ibm.com/support/pages/node/6843873){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary code execution in Vim (CVE-2022-1785, CVE-2022-1897, CVE-2022-1927)](https://www.ibm.com/support/pages/node/6843875){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security restrictions bypass in cURL libcurl (CVE-2022-22576)](https://www.ibm.com/support/pages/node/6843877){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to credential exposure in cURL libcurl (CVE-2022-27774)](https://www.ibm.com/support/pages/node/6843879){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to data information exposure in cURL libcurl (CVE-2022-27776)](https://www.ibm.com/support/pages/node/6843881){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security restrictions bypass in cURL libcurl (CVE-2022-27782)](https://www.ibm.com/support/pages/node/6843883){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in GNOME libxml2 (CVE-2022-29824)](https://www.ibm.com/support/pages/node/6843885){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a SQL injection in PostgreSQL (CVE-2022-31197)](https://www.ibm.com/support/pages/node/6843887){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in libexpat (CVE-2022-25313)](https://www.ibm.com/support/pages/node/6843889){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary code execution in libexpat (CVE-2022-25314)](https://www.ibm.com/support/pages/node/6843891){: external}

## 10 November 2022 (Versions 4.0.x and 4.5.x)
{: #speech-to-text-data-10november2022}

Known issue: Updated Network Policy needed for PostgreSQL operator
:   **Known issue:** For Speech services version 4.0.x (not including version 4.0.0) and 4.5.x, if the PostgreSQL operator and the Speech services are installed in different namespaces, the PostgreSQL operator is not able to monitor the PostgreSQL operands for the Speech services. The operator is prevented from monitoring the operands by the Network Policy that is in place for the Speech services.

    This problem does not prevent the PostgreSQL cluster from functioning properly. The cluster remains active and fully functional. However, the operator is not able to update the operands when you upgrade to new versions of the Speech services.

    The solution for the problem is to create an additional Network Policy for the PostgreSQL operator, as shown in the following steps. You can perform the steps regardless of whether the PostgreSQL operator is installed in the same namespace as the Speech services or in a different namespace.

    1.  Log in as an administrator of the Red Hat® OpenShift® project where the Speech services are installed.
    2.  Enter the following command to update the Network Policy for the Speech services:

        ```sh
        cat << EOF | oc apply -f -
        apiVersion: networking.k8s.io/v1
        kind: NetworkPolicy
        metadata:
          labels:
            app.kubernetes.io/component: stt
            app.kubernetes.io/instance: {{ <custom-resource-name> }}
            app.kubernetes.io/name: speech-to-text
            release: {{ <custom-resource-name> }}
          name: <custom-resource-name>-postgres-network-policy
          namespace: {{ <cpd-instance-namespace> }}
        spec:
          ingress:
          - from:
            - namespaceSelector: {}
              podSelector:
                matchLabels:
                  app.kubernetes.io/name: cloud-native-postgresql
        EOF
        ```
        {: pre}

        where
        -   `<custom-resource-name>` is the name of the Speech services custom resource. The recommended name for version 4.0.x is `speech-prod-cr`; the recommended name for version 4.5.x is `speech-cr`.
        -   `<cpd-instance-name>` is the name of the project (namespace) in which the Speech services are installed. The documentation uses the environment variable `${PROJECT_CPD_INSTANCE}` to identity the namespace.

    3.  To verify that the updated Network Policy allows the operator to monitor the operands and that the PostgreSQL cluster is in a healthy state, enter the following command, where `<custom-resource-name>` and `<cpd-instance-name>` are the values you used in the previous step:

        ```text
        oc -get cluster {{ <custom-resource-name> }}-postgres -n {{ <cpd-instance-namespace> }}
        ```
        {: codeblock}

        If the PostgreSQL cluster is functioning properly, the command produces output similar to the following:

        ```text
        NAME                 AGE   INSTANCES   READY   STATUS                     PRIMARY
        speech-cr-postgres   14d   3           3       Cluster in healthy state   speech-cr-postgres-1
        ```
        {: codeblock}

    These steps do not cause operator to update the operands to the latest versions. However, the operands are upgraded as expected when you next upgrade the Speech services software.

## 13 October 2022 (Version 4.5.3)
{: #speech-to-text-data-13october2022}

Version 4.5.3 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.5.3 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.5.x and Red Hat OpenShift versions 4.6, 4.8, and 4.10. For more information, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=services-watson-speech){: external}.

Audit events are available for the Speech services
:   The {{site.data.keyword.icp4dfull_notm}} Audit Logging Service generates and forwards audit events for both the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} services. The audit events match those that are available for [Activity Tracker](/docs/speech-to-text?topic=speech-to-text-at-events) with the public service. For more information, see [Audit events](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=2-audit-events){: external}.

You cannot uninstall individual Speech service components
:   The documentation now notes that you cannot uninstall individual service components (microservices) once they are installed. To remove any of the following components, you must uninstall the Watson Speech services in their entirety and reinstall only the components that you need: {{site.data.keyword.speechtotextshort}} runtime, {{site.data.keyword.speechtotextshort}} asynchronous HTTP, {{site.data.keyword.speechtotextshort}} customization, {{site.data.keyword.texttospeechshort}} runtime, and {{site.data.keyword.texttospeechshort}} customization. For more information about installing the Speech services, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=services-watson-speech){: external}.

New French Canadian next-generation multimedia model
:   The service now offers a next-generation multimedia model for French Canadian: `fr-CA_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Updates to English next-generation telephony models
:   The English next-generation telephony models have been updated for improved speech recognition:
    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`

    All of these models continue to support low latency. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Italian next-generation multimedia model now supports low latency
:   The Italian next-generation multimedia model, `it-IT_Multimedia`, now supports low latency. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Troubleshooting upgrade from version 4.0.x to version 4.5.x
:   When you upgrade the Speech services from version 4.0.x to version 4.5.x, you might encounter an issue where the PostgreSQL pods become stuck in the `Terminating` state. If this problem occurs during your upgrade, perform the following steps to resolve the problem. The information and steps are also documented in *Upgrading Watson Speech services from Version 4.0 to Version 4.5* in the *Upgrading* topic of [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=services-watson-speech){: external}.

    1.  Use the following command to identify pods that remain in the `Terminating` state:

    ```sh
    oc get pods -n ${PROJECT_CPD_INSTANCE} -o wide | awk {'print $1'}
    ```
    {: codeblock}

    1.  Use the following command to set the environment variable `pods` to include the list of pods that remain in the `Terminating` state:

    ```sh
    pods=$(oc get pods -n ${PROJECT_CPD_INSTANCE} -o wide | awk {'print $1'})
    ```
    {: codeblock}

    1.  Use the following command to delete the stuck pods so that the upgrade process can continue:

    ```sh
    pods=$(oc get pods -n ${PROJECT_CPD_INSTANCE} -o wide | grep Terminating | awk {'print $1'})
    ```
    {: codeblock}

Defect fix: Fix custom resource entries documentation
:   **Defect fix:** The documentation for the Speech services custom resource now includes colons after the names of the models `koKrTelephony` and `nlNlTelephony`. Previously, the documentation for these two entries omitted the colons.

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a buffer over-read flaw in Linux Kernel (CVE-2020-28915)](https://www.ibm.com/support/pages/node/6829133){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security bypass in GNU Gzip (CVE-2022-1271)](https://www.ibm.com/support/pages/node/6829139){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to elevated privileges in Apple macOS Monterey and macOS Big Sur (CVE-2022-26691)](https://www.ibm.com/support/pages/node/6829141){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to elevated privileges in Linux Kernel (CVE-2022-27666)](https://www.ibm.com/support/pages/node/6829143){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to cross-site scripting in Apache Tomcat (CVE-2022-34305)](https://www.ibm.com/support/pages/node/6829145){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security restrictions bypass in GNU C Library (CVE-2019-19126)](https://www.ibm.com/support/pages/node/6829149){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in GNU C Library ( CVE-2020-10029)](https://www.ibm.com/support/pages/node/6829151){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in GNU glibc (CVE-2020-1751)](https://www.ibm.com/support/pages/node/6829155){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in GNU glibc (CVE-2020-1752)](https://www.ibm.com/support/pages/node/6829157){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to information disclosure or denial of service in GNU glibc (CVE-2021-35942)](https://www.ibm.com/support/pages/node/6829159){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to buffer overflow in OpenSSL (CVE-2021-3711)](https://www.ibm.com/support/pages/node/6829161){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to information disclosure or denial of service in OpenSSL (CVE-2021-3712)](https://www.ibm.com/support/pages/node/6829165){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to weakened security in OpenSSL (CVE-2021-4160)](https://www.ibm.com/support/pages/node/6829167){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in OpenSSL (CVE-2022-0778)](https://www.ibm.com/support/pages/node/6829175){: external}

## 19 August 2022 (Version 4.5.1)
{: #speech-to-text-data-19august2022}

Important: Deprecation date for most previous-generation models is now 3 March 2023
:   **Important:** On 15 March 2022, the previous-generation models for all languages other than Arabic and Japanese were deprecated. At that time, the deprecated models were to remain available until 15 September 2022. To allow users more time to migrate to the appropriate next-generation models, the deprecated models will now remain available until **3 March 2023**. As with the initial deprecation notice, the Arabic and Japanese previous-generation models are *not* deprecated. For complete list of all deprecated models, see the [15 March 2022 (Version 4.0.6) service update](#speech-to-text-data-15march2022).

    On 3 March 2023, the deprecated models will be removed from the service and the documentation. If you use any of the deprecated models, you must migrate to the equivalent next-generation model by the 3 March 2023.
    -   For more information about the next-generation models to which you can migrate from each of the deprecated models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
    -   For more information about the next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   For more information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).

    **Note:** When the previous-generation `en-US_BroadbandModel` is removed from service, the next-generation `en-US_Multimedia` model will become the default model for speech recognition requests.

## 3 August 2022 (Version 4.5.1)
{: #speech-to-text-data-3august2022}

Version 4.5.1 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.5.1 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.5.x and Red Hat OpenShift versions 4.6, 4.8, and 4.10. For more information, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=services-watson-speech){: external}.

Support for FIPS-enabled clusters
:   Both {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} and {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} now support running on Federal Information Processing Standard (FIPS)-enabled clusters. For more information, see [Services that support FIPS](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=considerations-services-that-support-fips){: external}.

Defect fix: Fix ephemeral storage calculations to prevent occasional pod evictions
:   **Defect fix:** A defect was fixed and calculation of ephemeral storage limits is now more precise for the {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} and {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} runtimes. These changes prevent occasional pod evictions when the services' runtimes are under heavy load.

Defect fix: Update speech hesitations and hesitation markers documentation
:   **Defect fix:** Documentation for speech hesitations and hesitation markers has been updated. Previous-generation models include hesitation markers in place of speech hesitations in transcription results for most languages; smart formatting removes hesitation markers from US English final transcripts. Next-generation models include the actual speech hesitations in transcription results; smart formatting has no effect on their inclusion in final transcription results.

    For more information, see:
    -   [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation)
    -   [What results does smart formatting affect?](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-effects)

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in rsyslog (CVE-2022-24903)](https://www.ibm.com/support/pages/node/6610096){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to an HTTP request smuggling issue in Twisted (CVE-2022-24801)](https://www.ibm.com/support/pages/node/6610098){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service, caused by a buffer overflow in Twisted (CVE-2022-21716)](https://www.ibm.com/support/pages/node/6610100){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service, caused by incomplete string comparison in NumPy (CVE-2021-34141)](https://www.ibm.com/support/pages/node/6610102){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service, caused by a buffer overflow in NumPy (CVE-2021-41496)](https://www.ibm.com/support/pages/node/6610104){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to cookie and authorization header exposure in Twisted (CVE-2022-21712)](https://www.ibm.com/support/pages/node/6605065){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Perl (CVE-2018-18311)](https://www.ibm.com/support/pages/node/6610118){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Perl (CVE-2018-18312)](https://www.ibm.com/support/pages/node/6610122){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Perl (CVE-2018-18313)](https://www.ibm.com/support/pages/node/6610124){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Perl (CVE-2018-18314)](https://www.ibm.com/support/pages/node/6610126){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Perl (CVE-2018-6913)](https://www.ibm.com/support/pages/node/6610128){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to CRLF injection in Python (CVE-2019-11236)](https://www.ibm.com/support/pages/node/6610234){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in GNU Tar (CVE-2019-9923)](https://www.ibm.com/support/pages/node/6610238){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in Perl (CVE-2020-10543)](https://www.ibm.com/support/pages/node/6610240){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to an integer overflow in Perl (CVE-2020-10878)](https://www.ibm.com/support/pages/node/6610242){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a buffer overflow in Perl (CVE-2020-12723)](https://www.ibm.com/support/pages/node/6610283){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in urllib3 (CVE-2021-33503)](https://www.ibm.com/support/pages/node/6610285){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to injection attacks in Ansible (CVE-2021-3583)](https://www.ibm.com/support/pages/node/6610287){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-23772)](https://www.ibm.com/support/pages/node/6610289){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to incorrect access control in Golang Go (CVE-2022-23773)](https://www.ibm.com/support/pages/node/6610291){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-23806)](https://www.ibm.com/support/pages/node/6610293){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-24675)](https://www.ibm.com/support/pages/node/6610295){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-24921)](https://www.ibm.com/support/pages/node/6610297){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Golang Go (CVE-2022-28327)](https://www.ibm.com/support/pages/node/6610299){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a heap-based buffer overflow in libssh, caused by improper bounds checking (CVE-2021-3634)](https://www.ibm.com/support/pages/node/6610303){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in Python (CVE-2021-3737)](https://www.ibm.com/support/pages/node/6610329){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a possible sensitive information exposure in Python (CVE-2021-4189)](https://www.ibm.com/support/pages/node/6610339){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security restrictions bypass in lxml (CVE-2021-43818)](https://www.ibm.com/support/pages/node/6610341){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary code execution in MS Visual Studio (CVE-2021-21300)](https://www.ibm.com/support/pages/node/6610343){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a security restrictions bypass in Git (CVE-2021-40330)](https://www.ibm.com/support/pages/node/6610345){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary code execution in MS Visual Studio (CVE-2022-24765)](https://www.ibm.com/support/pages/node/6610347){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary command execution in Git (CVE-2018-1000021)](https://www.ibm.com/support/pages/node/6610349){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to cross-site scripting in jQuery (CVE-2015-9251)](https://www.ibm.com/support/pages/node/6610351){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to cross-site scripting in jQuery (CVE-2019-11358)](https://www.ibm.com/support/pages/node/6610361){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to cross-site scripting in jQuery (CVE-2020-11022)](https://www.ibm.com/support/pages/node/6610363){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to cross-site scripting in jQuery (CVE-2020-11023)](https://www.ibm.com/support/pages/node/6610365){: external}
    - [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a data binding rules security weakness in Spring Framework (CVE-2022-22968)](https://www.ibm.com/support/pages/node/6610371){: external}

## 29 June 2022 (Version 4.5.0)
{: #speech-to-text-data-29june2022}

Version 4.5.0 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.5.0 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.5.x and Red Hat OpenShift versions 4.6, 4.8, and 4.10. For more information, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=services-watson-speech){: external}.

Unified Speech services for {{site.data.keyword.icp4dfull_notm}} documentation
:   The installation and administration documentation for both {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} is now combined in the {{site.data.keyword.icp4dfull_notm}} documentation. For more information about installing and managing the Speech services, see [{{site.data.keyword.watson}} Speech services on {{site.data.keyword.icp4dfull_notm}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.5.x?topic=services-watson-speech){: external}.

Changes to Speech services custom resource
:   The custom resource is now created when you initially install the Speech services. The process is described in the {{site.data.keyword.icp4dfull_notm}} installation documentation. The content of the custom resource has changed:
    -   The recommended name of the custom resource has changed from `speech-prod-cr` to `speech-cr`.
    -   All references to storage class have changed from variants of `storageClass` to `blockStorageClass`.
    -   The name of the Portworx block storage class has changed from `portworx-shared-gp3` to `portworx-db-gp3-sc`.
    -   The `createSecret` property has been removed for the MinIO and PostgreSQl datastores. The property is only used internally. The Speech services always use a secrets object if you create one, and they always automatically create the object if none is provided.

User-provided secrets object now supported for RabbitMQ datastore
:   You can now provide security credentials for the RabbitMQ datastore, just as you can for the MinIO and PostgreSQL datastores. The documented process is similar for all three datastores.

New Italian `it-IT_Multimedia` next-generation model
:   The service now offers a next-generation multimedia model for Italian: `it-IT_Multimedia`. The new model is generally available. It does not support low latency, but it does support language model customization and grammars. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Updated Korean telephony and multimedia next-generation models
:   The existing Korean next-generation models have been updated:
    -   The `ko-KR_Telephony` model has been updated for improved low-latency support for speech recognition.
    -   The `ko-KR_Multimedia` model has been updated for improved speech recognition. The model now also supports low latency.

    Both models are generally available, and both support language model customization and grammars. You do not need to upgrade custom language models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Updates to multiple next-generation telephony models
:   The following next-generation English language telephony models have been updated for improved speech recognition:

    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`

    You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Defect fix: Confidence scores are now reported for all transcription results
:   **Defect fix:** Confidence scores are now reported for all transcription results. Previously, when the service returned multiple transcripts for a single speech recognition request, confidence scores might not be returned for all transcripts.

Security vulnerabilities addressed
:   No security vulnerabilities were fixed for version 4.5.0.

## 25 May 2022 (Version 4.0.9)
{: #speech-to-text-data-25may2022}

Version 4.0.9 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.9 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

New Brazilian Portuguese `pt-BR_Multimedia` next-generation model
:   The service now offers a next-generation multimedia model for Brazilian Portuguese: `pt-BR_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about the next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Update to German `de-DE_Multimedia` next-generation model to support low latency
:   The next-generation German model, `de-DE_Multimedia`, now supports low latency. You do not need to upgrade custom models that are based on the updated German base model. For more information about the next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

New beta `character_insertion_bias` parameter for next-generation models
:   All next-generation models now support a new beta parameter, `character_insertion_bias`, which is available with all speech recognition interfaces. By default, the service is optimized for each individual model to balance its recognition of candidate strings of different lengths. The model-specific bias is equivalent to 0.0. Each model's default bias is sufficient for most speech recognition requests.

    However, certain use cases might benefit from favoring hypotheses with shorter or longer strings of characters. The parameter accepts values between -1.0 and 1.0 that represent a change from a model's default. Negative values instruct the service to favor shorter strings of characters. Positive values direct the service to favor longer strings of characters. For more information, see [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias).

The Speech services do not support the OADP backup and restore utility
:   Watson Speech services do not support the {{site.data.keyword.icp4dfull_notm}} OpenShift APIs for Data Protection (OADP) backup and restore utility. If the Speech services are installed on a cluster, you might not be able to use the {{site.data.keyword.icp4dfull_notm}} OADP backup and restore utility to back up other services that are installed on that cluster. This limitation applies to version 4.0.0 and later versions of the Speech services.

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable a denial of service, caused by a buffer overflow with Twisted (CVE-2022-21716)](https://www.ibm.com/support/pages/node/6593867){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service in NumPy. (CVE-2021-33430)](https://www.ibm.com/support/pages/node/6592841){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a denial of service, caused by improper input validation with Spring Framework (CVE-2022-22950)](https://www.ibm.com/support/pages/node/6593865){: external}

## 1 May 2022 (Version 1.2.x)
{: #speech-to-text-data-1may2022}

Important: End of service for {{site.data.keyword.speechtotextshort}} version 1.2.x on {{site.data.keyword.icp4dfull_notm}} version 3.5
:   **Important:** {{site.data.keyword.speechtotextshort}} version 1.2.x on {{site.data.keyword.icp4dfull_notm}} version 3.5 is out of service as of 1 May 2022. {{site.data.keyword.speechtotextshort}} version 1.2.x is no longer supported, available, or documented. For more information about End of Service for {{site.data.keyword.speechtotextshort}}, which is part of the {{site.data.keyword.watson}} API Kit, see [Software support discontinuance: IBM Watson API Kit for IBM Cloud Pak for Data 1.2.x](https://www.ibm.com/common/ssi/cgi-bin/ssialias?subtype=ca&infotype=an&appname=iSource&supplier=897&letternum=ENUS922-038).

## 27 April 2022 (Version 4.0.8)
{: #speech-to-text-data-27april2022}

Version 4.0.8 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.8 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

New environment variables used in {{site.data.keyword.icp4dfull_notm}} documentation
:   Most commands in the {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} documentation have been updated to use a common set of environment variables. The documentation provides a script to automatically export the environment variables before you run installation, upgrade, and administration commands. After you source the script, you can copy most commands from the documentation and run them without making any changes.

    The environment variables that the script defines include the following:
    -   `${PROJECT_CPD_INSTANCE}` identifies the project where you plan to install {{site.data.keyword.icp4dfull_notm}} and the Speech services.
    -   `${PROJECT_CPD_OPS}` identifies the project for the {{site.data.keyword.icp4dfull_notm}} platform operator.
    -   `${PROJECT_CPFS_OPS}` identifies the project for the {{site.data.keyword.icp4dfull_notm}} foundational services.

    For more information about using the environment variables, see [Best practice: Setting up install variables](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=installing-best-practice-setting-up-install-variables){: external}.

The `ttsVoiceMarginalCPU` property is no longer documented
:   The `ttsVoiceMarginalCPU` property has been removed from the documentation for the Speech services custom resource. The property manages the tradeoff between concurrency and speech synthesis speed. The default value of `400` ensures a reasonable balance for most customers and maintains real-time synthesis.

New German next-generation multimedia model
:   The service now offers a next-generation multimedia model for German: `de-DE_Multimedia`. The new model is generally available. It does not support low latency. It does support language model customization and grammars as generally available functionality.

    For more information about all available next-generation models and their customization support, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)

Beta next-generation `en-WW_Medical_Telephony` model now supports low latency
:   The beta next-generation `en-WW_Medical_Telephony` model now supports low latency. For more information about all next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    -   [Security Bulletin: A vulnerability with Guava affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2020-8908)](https://www.ibm.com/support/pages/node/6575479){: external}
    -   [Security Bulletin: A Google Guava vulnerability affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2018-10237)](https://www.ibm.com/support/pages/node/6575477){: external}
    -   [Security Bulletin: Vulnerabilities in Apache Tomcat affect IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2022-23181)](https://www.ibm.com/support/pages/node/6575481){: external}
    -   [Security Bulletin: A Cyrus SASL vulnerability affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2022-24407)](https://www.ibm.com/support/pages/node/6575483){: external}
    -   [Security Bulletin: A vulnerability with GNU wget affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2016-4971)](https://www.ibm.com/support/pages/node/6575485){: external}
    -   [Security Bulletin: A vulnerability with GNU Wget affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2018-0494)](https://www.ibm.com/support/pages/node/6575487){: external}
    -   [Security Bulletin: A vulnerability in 'GNU Wget' affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2018-20483)](https://www.ibm.com/support/pages/node/6575489){: external}
    -   [Security Bulletin: A vulnerability in ISC BIND affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2018-5741)](https://www.ibm.com/support/pages/node/6575493){: external}
    -   [Security Bulletin: A vulnerability in Python affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2019-20916)](https://www.ibm.com/support/pages/node/6575495){: external}
    -   [Security Bulletin: A vulnerability with ISC BIND affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-25214)](https://www.ibm.com/support/pages/node/6575497){: external}
    -   [Security Bulletin: A vulnerability in ISC BIND affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-25215)](https://www.ibm.com/support/pages/node/6575499){: external}
    -   [Security Bulletin: A vulnerability in ISC BIND affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-25216)](https://www.ibm.com/support/pages/node/6575503){: external}
    -   [Security Bulletin: A vulnerability in ISC BIND affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-25219)](https://www.ibm.com/support/pages/node/6575505){: external}
    -   [Security Bulletin: A vulnerability in PostgreSQL JDBC Driver (PgJDBC) affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2022-21724)](https://www.ibm.com/support/pages/node/6575507){: external}
    -   [Security Bulletin: A vulnerability in GNU Tar affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2019-9923)](https://www.ibm.com/support/pages/node/6575509){: external}
    -   [Security Bulletin: A vulnerability in logback-classic affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-42550)](https://www.ibm.com/support/pages/node/6575511){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a stack-based buffer overflow in GNU C Library (CVE-2022-23218)](https://www.ibm.com/support/pages/node/6578617){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to stack-based buffer overflow in GNU C Library (CVE-2022-23219)](https://www.ibm.com/support/pages/node/6578619){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to a buffer overflow and underflow in GNU C Library (CVE-2021-3999)](https://www.ibm.com/support/pages/node/6578621){: external}

## 8 April 2022 (Version 4.0.7)
{: #speech-to-text-data-8april2022}

Support for sounds-like is now documented for custom models based on next-generation models
:   For custom language models that are based on next-generation models, support is now documented for sounds-like specifications for custom words. Support for sounds-likes has been available since late 2021.

    Differences exist between the use of the `sounds_like` field for custom models that are based on next-generation and previous-generation models. For more information about using the `sounds_like` field with custom models that are based on next-generation models, see [Working with custom words for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#workingWords-ng).

Important: Deprecated `customization_id` parameter removed from the documentation
:   **Important:** On [9 October 2018](#speech-to-text-9october2018), the `customization_id` parameter of all speech recognition requests was deprecated and replaced by the `language_customization_id` parameter. The `customization_id` parameter has now been removed from the documentation for the speech recognition methods:
    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for synchronous HTTP requests (including multipart requests)
    -   `POST /v1/recognitions` for asynchronous HTTP requests

    **Note:** If you use the {{site.data.keyword.watson}} SDKs, make sure that you have updated any application code to use the `language_customization_id` parameter instead of the `customization_id` parameter. The `customization_id` parameter will no longer be available from the equivalent methods of the SDKs as of their next major release. For more information about the speech recognition methods, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text/speech-to-text#service-endpoint){: external}.

## 30 March 2022 (Version 4.0.7)
{: #speech-to-text-data-30march2022}

Version 4.0.7 is now available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 4.0.7 is now available. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift versions 4.6 and 4.8. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.

Custom resource property for specifying a default model
:   The default voice for speech recognition requests is `en-US_BroadbandModel`. If you do not install the `en-US_BroadbandModel`, you must either
    -   Use the `model` parameter to pass the voice that is to be used with each request.
    -   Specify a new default model for your installation of {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} by using the `defaultSTTModel` property in the Speech services custom resource. For more information, see  [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external} and [Using the default model](/docs/speech-to-text?topic=speech-to-text-models-use#models-use-default).

Updates to English and French next-generation multimedia models to support low latency
:   The following multimedia models have been updated to support low latency:
    -   Australian English: `en-AU_Multimedia`
    -   UK English: `en-GB_Multimedia`
    -   US English: `en-US_Multimedia`
    -   French: `fr-FR_Multimedia`

    You do not need to upgrade custom language models that are built on these base models. For more information about the next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

New Castilian Spanish next-generation multimedia model
:   The service now offers a next-generation multimedia model for Castilian Spanish: `es-ES_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars.

    For more information about all available next-generation models and their customization support, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)

Beta next-generation `en-WW_Medical_Telephony` model now supports smart formatting
:   The beta next-generation `en-WW_Medical_Telephony` model now supports the `smart_formatting` parameter for US English audio. For more information about all next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)

Security vulnerabilities addressed
:   The following security vulnerabilities have been fixed:
    -   [Red Hat CVE-2022-24407](https://access.redhat.com/security/cve/CVE-2022-24407){: external}: A flaw was found in the SQL plugin shipped with Cyrus SASL. The vulnerability occurs due to failure to properly escape SQL input and leads to an improper input validation vulnerability. This flaw allows an attacker to execute arbitrary SQL commands and the ability to change the passwords for other accounts allowing escalation of privileges.
    -   [Security Bulletin: A jwt-go vulnerability affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2020-26160)](https://www.ibm.com/support/pages/node/6574543){: external}
    -   [Security Bulletin: A vulnerability in Golang Go affects IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-29923)](https://www.ibm.com/support/pages/node/6574545){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is affected but not classified as vulnerable by a remote code execution in Spring Framework (CVE-2022-22965)](https://www.ibm.com/support/pages/node/6583151){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to arbitrary code execution with IBM WebSphere Application Server (CVE-2021-23450)](https://www.ibm.com/support/pages/node/6583149){: external}

## 17 March 2022 (Version 4.0.6)
{: #speech-to-text-data-17march2022}

Grammar support for next-generation models is now generally available
:   Grammar support is now generally available (GA) for next-general models that meet the following conditions:
    -   The models are generally available.
    -   The models support language model customization.

    For more information, see the following topics:
    -   For more information about the status of grammar support for next-generation models, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).
    -   For more information about grammars, see [Grammars](/docs/speech-to-text?topic=speech-to-text-customization#grammars-intro).

## 15 March 2022 (Version 4.0.6)
{: #speech-to-text-data-15march2022}

Important: Deprecation of most previous-generation models
:   **Important:** Effective **15 March 2022**, previous-generation models for all languages other than Arabic and Japanese are deprecated. The deprecated models remain available until **15 September 2022**, when they will be removed from the service and the documentation. The Arabic and Japanese previous-generation models are *not* deprecated.

    The following previous-generation models are now deprecated:
    -   Chinese (Mandarin): `zh-CN_NarrowbandModel` and `zh-CN_BroadbandModel`
    -   Dutch (Netherlands): `nl-NL_NarrowbandModel` and `nl-NL_BroadbandModel`
    -   English (Australian): `en-AU_NarrowbandModel` and `en-AU_BroadbandModel`
    -   English (United Kingdom): `en-UK_NarrowbandModel` and `en-UK_BroadbandModel`
    -   English (United States): `en-US_NarrowbandModel`, `en-US_BroadbandModel`, and `en-US_ShortForm_NarrowbandModel`
    -   French (Canadian): `fr-CA_NarrowbandModel` and `fr-CA_BroadbandModel`
    -   French (France): `fr-FR_NarrowbandModel` and `fr-FR_BroadbandModel`
    -   German: `de-DE_NarrowbandModel` and `de-DE_BroadbandModel`
    -   Italian: `it-IT_NarrowbandModel` and `it_IT_BroadbandModel`
    -   Korean: `ko-KR_NarrowbandModel` and `ko-KR_BroadbandModel`
    -   Portuguese (Brazilian): `pt-BR_NarrowbandModel` and `pt-BR_BroadbandModel`
    -   Spanish (Argentinian): `es-AR_NarrowbandModel` and `es-AR_BroadbandModel`
    -   Spanish (Castilian): `es-ES_NarrowbandModel` and `es-ES_BroadbandModel`
    -   Spanish (Chilean): `es-CL_NarrowbandModel` and `es-CL_BroadbandModel`
    -   Spanish (Colombian): `es-CO_NarrowbandModel` and `es-CO_BroadbandModel`
    -   Spanish (Mexican): `es-MX_NarrowbandModel` and `es-MX_BroadbandModel`
    -   Spanish (Peruvian): `es-PE_NarrowbandModel` and `es-PE_BroadbandModel`

    If you use any of these deprecated models, you must migrate to the equivalent next-generation model by the end of service date.
    -   For more information about the next-generation models to which you can migrate from each of the deprecated models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
    -   For more information about the next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   For more information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).

    **Note:** When the previous-generation `en-US_BroadbandModel` is removed from service on 15 September, the next-generation `en-US_Multimedia` model will become the default model for speech recognition requests.

Next-generation models now support audio-parsing parameters
:   All next-generation models now support the following audio-parsing parameters as generally available features:

    -   `end_of_phrase_silence_time` specifies the duration of the pause interval at which the service splits a transcript into multiple final results. For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).
    -   `split_transcript_at_phrase_end` directs the service to split the transcript into multiple final results based on semantic features of the input. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

Defect fix: Correct speaker labels documentation
:   **Defect fix:** Documentation of speaker labels included the following erroneous statement in multiple places: *For next-generation models, speaker labels are not supported for use with interim results or low latency.* Speaker labels are supported for use with interim results and low latency for next-generation models. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

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

Defect fix: Improve custom model upgrade and base model version documentation
:   **Defect fix:** The documentation that describes the upgrade of custom models and the version strings that are used for different versions of base models has been updated. The documentation now states that upgrade for language model customization also applies to next-generation models. Also, the version strings that represent different versions of base models have been updated. And the `base_model_version` parameter can also be used with upgraded next-generation models.

    For more information about custom model upgrade, when upgrade is necessary, and how to use older versions of custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

Defect fix: Update capitalization documentation
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
    -   For more information about customization support for next-generation models, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).

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

Security vulnerabilities addressed
:   The following security vulnerabilities associated with Apache Log4j have been fixed:
    -   [Security Bulletin: Vulnerability in Apache Log4j may affect IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data (CVE-2021-4104)](https://www.ibm.com/support/pages/node/6551170){: external}
    -   [Security Bulletin: IBM Watson Speech Services Cartridge for IBM Cloud Pak for Data is vulnerable to denial of service and arbitrary code execution due to Apache Log4j (CVE-2021-45105 and CVE-2021-45046)](https://www.ibm.com/support/pages/node/6551168){: external}

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

Defect fix: Correct upgrade documentation
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
    -   For more information about the status of grammar support for next-generation models, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).
    -   For more information about grammars, see [Grammars](/docs/speech-to-text?topic=speech-to-text-customization#grammars-intro).

New `custom_acoustic_model` field for supported features
:   The `GET /v1/models` and `GET /v1/models/{model_id}` methods now report whether a model supports acoustic model customization. The `SupportedFeatures` object now includes an additional field, `custom_acoustic_model`, a boolean that is `true` for a model that supports acoustic model customization and `false` otherwise. Currently, the field is `true` for all previous-generation models and `false` for all next-generation models.
    -   For more information about these methods, see [Listing information about models](/docs/speech-to-text?topic=speech-to-text-models-list).
    -   For more information about support for acoustic model customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

Security vulnerability addressed
:   The following security vulnerability associated with Apache Log4j has been fixed:
    -   [Security Bulletin: Vulnerability in Apache Log4j may affect IBM Watson Speech Services Cartridge for {{site.data.keyword.icp4dfull_notm}} (CVE-2021-4428)](https://www.ibm.com/support/pages/node/6536732){: external}

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

Defect fix: Address asynchronous HTTP failures
:   **Defect fix:** The asynchronous HTTP interface failed to transcribe some audio. In addition, the callback for the request returned status `recognitions.completed_with_results` instead of `recognitions.failed`. This error has been resolved.

Defect fix: Improve speakers labels results
:   **Defect fix:** When you use speakers labels with next-generation models, the service now identifies the speaker for all words of the input audio, including very short words that have the same start and end timestamps.

Defect fix: Update interim results and low-latency documentation
:   **Defect fix:** Documentation that describes the interim results and low-latency features with next-generation models has been rewritten for clarity and correctness. For more information, see the following topics:
    -   [Interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim), especially [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency)
    -   [How the service sends recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#ws-results)

Defect fix: Correct multitenancy documentation
:   **Defect fix:** The {{site.data.keyword.icp4dfull_notm}} topic [Multitenancy support](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=planning-multitenancy-support){: external} incorrectly stated that the Speech services do not support multitenancy. The topic has been updated to state that the Speech services support the following operations:
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

Defect fix: Improve documentation
:   **Defect fix:** The documentation has been updated to correct the following information:
    -   The documentation failed to state that next-generation models do *not* produce hesitation markers. The documentation has been updated to note that only previous-generation models produce hesitation markers. Next-generation models include the actual hesitations in transcription results. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).
    -   The documentation incorrectly stated that using the `smart_formatting` parameter causes the service to remove hesitation markers from final transcription results for Japanese. Smart formatting does not remove hesitation markers from final results for Japanese, only for US English. For more information, see [What results does smart formatting affect?](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-effects)

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

    `{Registry}` is the path for the internal Docker registry. It must be `image-registry.openshift-image-registry.svc:5000/{namespace}`, where `{namespace}` is the namespace in which {{site.data.keyword.icp4dfull}} is installed, normally `zen`.

## 9 April 2021 (Version 1.2.1)
{: #speech-to-text-data-9april2021}

Support for modifying installed models and voices
:   The Speech services let you add or remove installed models and voices for version 1.2 or 1.2.1 of the services.

## Version 1.2.1 (26 March 2021)
{: #speech-to-text-data-26march2021}

Version 1.2.1 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.2.1 is now available. Versions 1.2 and 1.2.1 use the same version 1.2 documentation and installation instructions. Version 1.2.1 supports installation on Red Hat OpenShift version 4.6 in addition to versions 4.5 and 3.11.

New installation instructions
:   For both clusters connected to the internet and air-gapped clusters, the installation instructions include the following steps:
    -   Use the `oc label` command to set up required labels for the namespace where {{site.data.keyword.icp4dfull_notm}} is installed.
    -   Use the `oc project` command to ensure that you are pointing at the correct OpenShift project.
    -   Use the `cpd-cli install` command to install an Enterprise DB PostgreSQL server that is used by the Speech services.

    You perform these steps before you install the Speech services.

New uninstallation instructions
:   A step was added to the procedure for uninstalling the Speech services to clean up all of the resources from the installation.

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


    The abbreviated `speech-override.yaml` file has generally been reduced further by fine-tuning its contents to the essential elements.

## Version 1.2 (9 December 2020)
{: #speech-to-text-data-9december2020}

Version 1.2 is available
:   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.2 is now available. Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} versions 3.5 and 3.0.1, and Red Hat OpenShift versions 4.5 and 3.11.

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
:   The hesitation marker that is used for the updated German broadband and narrowband models has changed from `[hesitation]` to `%HESITATION`. For more information about hesitation markers, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

Defect fix: Address latency issue for models with large numbers of grammars
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
