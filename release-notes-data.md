---

copyright:
  years: 2018, 2021
lastupdated: "2021-08-16"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:deprecated: .deprecated}
{:important: .important}
{:pre: .pre}
{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Release notes for {{site.data.keyword.icp4dfull_notm}}
{: #release-notes-data}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

The following features and changes were included for each release and update of installed or on-premises instances of {{site.data.keyword.speechtotextfull}} for {{site.data.keyword.icp4dfull_notm}}. The information includes known limitations. Unless otherwise noted, all changes are compatible with earlier releases and are automatically and transparently available to all new and existing applications.
{: shortdesc}

For information about releases and updates for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.cloud_notm}}, see [Release notes for {{site.data.keyword.cloud_notm}}](/docs/speech-to-text?topic=speech-to-text-release-notes).
{: note}

## Beta features
{: release-notes-data-beta-features}

IBM occasionally releases features and language support that are classified as beta. Such features are provided so that you can evaluate their functionality. They might not provide the same level of performance or compatibility that generally available (GA) features provide. They might be unstable and are subject to change or removal with short notice. They are not intended for use in a production environment.

## Known limitations
{: #release-notes-data-limitations}

The service has the following known limitations:

-   **29 July 2021:** When you use previous-generation models with the WebSocket interface, if you request both speaker labels and interim results, the speaker labels response includes an extra object. The final results duplicate the object for the last speaker label. Instead of appearing once with `"final": true`, the object appears twice: once with `"final": false` and once with `"final": true`.

    For example, a WebSocket request that includes both speaker labels and interim results sends a `start` message like the following:

    ```javascript
    var message = {
      action: 'start',
      speaker_labels: true,
      interim_results: true
    };
    websocket.send(JSON.stringify(message));
    ```
    {: codeblock}

    To indicate final results for speaker labels, the service is supposed to return a response of the following form:

    ```javascript
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

    ```javascript
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

## Version 4.0.0 (29 July 2021)
{: #v40}

{{site.data.keyword.speechtotextfull}} for {{site.data.keyword.icp4dfull}} version 4.0.0 is now available.  Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} version 4.x and Red Hat OpenShift version 4.6. For more information about installing and managing the service, see [Installing {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}](/docs/speech-to-text?topic=speech-to-text-speech-install-data).

To install {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}, contact your IBM Support representative. They can help you install and administer the service on {{site.data.keyword.icp4dfull_notm}}. Complete installation and administration documentation for the product will be published in the coming weeks.
{: note}

Version 4.0.0 includes the changes described in the following sections.

### Unified {{site.data.keyword.speechtotextshort}} documentation
{: #v40-documentation}

The documentation for {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} is now combined with the documentation for managed instances of the {{site.data.keyword.speechtotextshort}} service that are hosted on {{site.data.keyword.cloud_notm}}. This is true of both the guide and reference documentation for the two forms of the service. Links to the formerly separate version of the {{site.data.keyword.icp4dfull_notm}} documentation for the service redirect to the unified documentation.

For more information about identifying information that pertains to only one version of the product, see [About {{site.data.keyword.speechtotextshort}}](/docs/speech-to-text?topic=speech-to-text-about#about-version).

### New next-generation language models
{: #v40-next-generation}

The next-generation language models and the `low_latency` parameter are beta functionality. The next-generation models support a limited number of languages and features at this time. The supported languages, models, and features will increase with future releases.
{: beta}

The service now supports a growing number of next-generation language models. The next-generation *multimedia* and *telephony* models improve upon the speech recognition capabilities of the service's previous generation of broadband and narrowband models. The new models leverage deep neural networks and bidirectional analysis to achieve both higher throughput and greater transcription accuracy. At this time, the next-generation models support only a limited number of languages and speech recognition features.

Many of the next-generation models also support a new `low_latency` parameter that lets you request faster results at the possible expense of reduced transcription quality. When low latency is enabled, the service curtails its analysis of the audio, which can reduce the accuracy of the transcription. This trade-off might be acceptable if your application requires lower response time more than it does the highest possible accuracy.

The `low_latency` parameter impacts your use of the `interim_results` parameter with the WebSocket interface. Interim results are available only for those next-generation models that support low latency, and only if both the `interim_results` and `low_latency` parameters are set to `true`.

-   For more information about the next-generation models and their capabilities, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
-   For more information about language support for next-generation models and about which next-generation models support low latency, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).
-   For more information about feature support for next-generation models, see [Supported features](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features) and [Unsupported features](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-unsupported).
-   For more information about the `low_latency` parameter, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).
-   For more information about the interaction between the `low_latency` and `interim_results` parameters for next-generation models, see [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency).

### Other changes
{: #v40-other-changes}

-   The Arabic language broadband model is now named `ar-MS_BroadbandModel`. The former name, `ar-AR_BroadbandModel`, is deprecated. It will continue to function for at least one year but might be removed at a future date. You are encouraged to migrate to the new name at your earliest convenience.
-   **Defect fix:** The documentation incorrectly stated that using the `smart_formatting` parameter causes the service to remove hesitation markers from final transcription results for Japanese. Smart formatting does not remove hesitation markers from final results for Japanese, only for US English. For more information, see [What results does smart formatting affect?](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-effects)
-   **Defect fix:** The documentation failed to state that next-generation models do *not* produce hesitation markers. The documentation has been updated to note that only previous-generation models produce hesitation markers. For more information, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

## Version 1.2.1 (26 March 2021)
{: #v121}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.2.1 is now available. Versions 1.2 and 1.2.1 use the same version 1.2 documentation and installation instructions. Version 1.2.1 includes the following changes:

-   **12 April:** The minimal `speech-override.yaml` file includes an extra definition, `dockerRegistryPrefix`:

    ```yaml
    global:
      dockerRegistryPrefix: "{Registry}"
      image:
        pullSecret: "{Registry_pull_secret}"
    ```
    {: codeblock}

    `{Registry}` is the path for the internal Docker registry. It must be `image-registry.openshift-image-registry.svc:5000/{namespace}`, where `{namespace}` is the namespace in which {{site.data.keyword.icp4dfull}} is installed, normally `zen`. For more information, see [The speech-override.yaml file](/docs/speech-to-text?topic=speech-to-text-speech-override-12#speech-override-file-12).
-   **9 April:** The service lets you add or remove installed models or voices for version 1.2 or 1.2.1 of the Speech services. For more information, see [Modifying the installed models and voices](/docs/speech-to-text?topic=speech-to-text-speech-cluster-12#speech-cluster-models-voices-12).
-   Version 1.2.1 supports installation on Red Hat OpenShift version 4.6 in addition to versions 4.5 and 3.11.
-   For both clusters connected to the internet and air-gapped clusters, the installation instructions include the following steps:
    -   Use the `oc label` command to set up required labels for the namespace where {{site.data.keyword.icp4dfull_notm}} is installed.
    -   Use the `oc project` command to ensure that you are pointing at the correct OpenShift project.
    -   Use the `cpd-cli install` command to install an EnterpriseDB PostgreSQL server that is used by the Speech services.

    You perform these steps before you install the Speech services. For more information, see [Installing the {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-install.html){: external}.
-   The Minio and PostgreSQL datastores require the following hard-coded values for their secrets:
    -   For *Minio*, use `minio`.
    -   For *PostgreSQL*, use `user-provided-postgressql`.

    You cannot use your own values for these secrets. The secrets must be created before you install the Speech services.
-   The following entries have been removed from the `speech-override.yaml` file. They were added to work around a problem that has now been fixed.

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

-   The abbreviated `speech-override.yaml` file has generally been reduced further by fine-tuning its contents to the essential elements. The updated version of the file appears in the following sections:
    -   [Creating an override file for {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} installation](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-override.html){: external}
    -   [Using the override file](/docs/speech-to-text?topic=speech-to-text-speech-override-12)

    You can download a complete version of the [speech-override.yaml](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/cpd-version-12/speech-override.yaml){: external} file. The complete version includes all of the detailed elements described in [Using the override file](/docs/speech-to-text?topic=speech-to-text-speech-override-12).
-   The entitled registry path from which the service pulls images for the PostgreSQL datastore has changed. The registry location changed from `cp.icr.io/cp/watson-speech` to `cp.icr.io/cp/cpd`. This change is transparent to users.
-   A step was added to the procedure for uninstalling the Speech services to clean up all of the resources from the installation. For more information, see [Uninstalling {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-uninstall.html){: external}.

## Version 1.2 (9 December 2020)
{: #v12}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.2 is now available. Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} versions 3.5 and 3.0.1, and Red Hat OpenShift versions 4.5 and 3.11. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} version 1.2](/docs/speech-to-text?topic=speech-to-text-speech-install-12).

The release includes the following functional changes and enhancements:

-   The service now offers broadband and narrowband models for Australian English and Canadian French:
    -   Australian English: `en-AU_BroadbandModel` and `en-AU_NarrowbandModel`
    -   Canadian French: `fr-CA_BroadbandModel` and `fr-CA_NarrowbandModel`

    The new models are generally available, and they support both language model and acoustic model customization.
    -   For more information about supported languages and models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).
    -   For more information about language support for customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
-   The following language models have been updated for improved speech recognition:
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
-   The speech recognition parameter `split_transcript_at_phrase_end` is now generally available for all languages. Previously, it was generally available only for US and UK English. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).
-   The hesitation marker that is used for the updated German broadband and narrowband models has changed from `[hesitation]` to `%HESITATION`. For more information about hesitation markers, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).
-   **Defect fix:** The service no longer has a latency issue for custom language models that contain a large number of grammars. When initially used for speech recognition, such custom models could take multiple seconds to load. The custom models now load much faster, greatly reducing latency when they are used for recognition.

## Version 1.1.4 (19 June 2020)
{: #v114}

{{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.4 go out of service on **30 September 2021**. You must upgrade to a later version of the services on {{site.data.keyword.icp4dfull_notm}} before that date. As of 1 October 2021, the documentation for version 1.1.4 will no longer be available.
{: important}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.4 is now available. Installation and administration of the service include many changes. This version supports {{site.data.keyword.icp4dfull_notm}} versions 2.5 and 3.0.1, and Red Hat OpenShift versions 3.11 and 4.3. For more information about installing and managing the service, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} version 1.1.4](/docs/speech-to-text?topic=speech-to-text-speech-install).

{{site.data.keyword.icp4dfull_notm}} 3.0.1 is deprecating support for Red Hat OpenShift 4.3 on 1 September 2020. Red Hat OpenShift 4.3 is going out of service on 22 October 2020. {{site.data.keyword.icp4dfull_notm}} is introducing support for Red Hat OpenShift 4.5. {{site.data.keyword.icp4dfull_notm}} is recommending that clients upgrade to Red Hat OpenShift 4.5 before 22 October 2020. IBM Support will work with any customers who already installed {{site.data.keyword.icp4dfull_notm}} 3.0.1 on Red Hat OpenShift 4.3. New customers who want to install on Red Hat OpenShift 4.x are instructed to install Red Hat OpenShift 4.5.
{: important}

The release includes the following functional changes and enhancements:

-   The service now offers greatly improved backup and restore procedures. Utilities are now available to back up data from your datastores, so you no longer need to re-create all of your data in the event of a disaster. For more information, see [Backing up and restoring your data](/docs/speech-to-text?topic=speech-to-text-speech-backup).
-   The service now offers two new optional parameters for controlling the level of speech activity detection. The parameters can help ensure that only relevant audio is processed for speech recognition.
    -   The `speech_detector_sensitivity` parameter adjusts the sensitivity of speech activity detection. You can use the parameter to suppress word insertions from music, coughing, and other non-speech events.
    -   The `background_audio_suppression` parameter suppresses background audio based on its volume to prevent it from being transcribed or otherwise interfering with speech recognition. You can use the parameter to suppress side conversations or background noise.

    You can use the parameters individually or together. They are available for all interfaces and for most language models. For more information about the parameters, their allowable values, and their effect on the quality and latency of speech recognition, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-detection).
-   The service now supports broadband and narrowband models for the Dutch and Italian languages:
    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)

    Dutch and Italian language models are generally available (GA) for speech recognition and for language model and acoustic model customization. For more information about all available language models, see
    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport)
-   The service now supports speaker labels (the `speaker_labels` parameter) for German and Korean language models. Speaker labels identify which individuals spoke which words in a multi-participant exchange. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).
-   The Japanese narrowband model (`ja-JP_NarrowbandModel`) now includes some multigram word units for digits and decimal fractions. The service returns these multigram units regardless of whether you enable smart formatting. The smart formatting feature understands and returns the multigram units that the model generates. If you apply your own post-processing to transcription results, you need to handle these units appropriately. For more information, see [Japanese](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-japanese) in the smart formatting documentation.

## Version 1.1.3 (28 February 2020)
{: #v113}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.3 is now available. The release includes the following changes:

-   **As of 1 April 2020,** acoustic model customization is now generally available (GA) for all supported languages. For more information about support for individual language models, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
-   For speech recognition, the service now supports the `end_of_phrase_silence_time` parameter. The parameter specifies the duration of the pause interval at which the service splits a transcript into multiple final results. Each final result indicates a pause or extended silence that exceeds the pause interval. For most languages, the default pause interval is 0.8 seconds; for Chinese the default interval is 0.6 seconds.

    You can use the parameter to effect a trade-off between how often a final result is produced and the accuracy of the transcription. Increase the interval when accuracy is more important than latency. Decrease the interval when the speaker is expected to say short phrases or single words.

    For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).
-   For speech recognition, the service now supports the `split_transcript_at_phrase_end` parameter. The parameter directs the service to split the transcript into multiple final results based on semantic features of the input, such as at the conclusion of sentences. The service bases its understanding of semantic features on the base language model that you use with a request. Custom language models and grammars can also influence how and where the service splits a transcript.

    The parameter causes the service to add an `end_of_utterance` field to each final result to indicate the motivation for the split: `full_stop`, `silence`, `end_of_data`, or `reset`.

    For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).
-   Speaker labels are updated to improve the identification of individual speakers for further analysis of your audio sample. For more information about the speaker labels feature, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels). For more information about the improvements to the feature, see [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://www.ibm.com/blogs/research/2020/07/speaker-diarization-in-real-use-cases/){: external}.

## Older versions
{: #release-notes-data-older}

-   [Version 1.1.2 (27 November 2019)](#v112)
-   [Version 1.0.1 (30 August 2019)](#v101)
-   [Version 1.0.0 (28 June 2019)](#v100)

### Version 1.1.2 (27 November 2019)
{: #v112}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.1.2 is now available. The release includes the following change:

-   You can create no more than 1024 custom language models and no more than 1024 custom acoustic models per owning credentials. For more information, see [Maximum number of custom models](/docs/speech-to-text?topic=speech-to-text-customization#customMaximum).

### Version 1.0.1 (30 August 2019)
{: #v101}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} version 1.0.1 is now available. The service now works with {{site.data.keyword.icp4dfull_notm}} 2.1.0.1. The release includes the following changes:

-   The service now supports installing {{site.data.keyword.icp4dfull_notm}} with Red Hat OpenShift.
-   The service now offers broadband and narrowband language models in six Spanish dialects:

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
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport)
-   Federal Information Security Management Act (FISMA) support is now available for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}. The service is FISMA High Ready.

### Version 1.0.0 (28 June 2019)
{: #v100}

The initial release of the service. {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} is based on the {{site.data.keyword.speechtotextfull}} service on the public {{site.data.keyword.cloud_notm}}. {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} differs from the public {{site.data.keyword.speechtotextshort}} service in the following ways. You might find this information helpful if you are already familiar with the {{site.data.keyword.speechtotextshort}} service on the public {{site.data.keyword.cloud_notm}}.

-   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} uses access tokens for authentication. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   The endpoints for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} are specific to your {{site.data.keyword.icp4dfull_notm}} cluster. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} does not perform any request logging. You do not need to use the `X-Watson-Learning-Opt-Out` request header.
-   {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} does not support Watson tokens. You cannot use the `X-Watson-Authorization-Token` request header to authenticate with the service.
