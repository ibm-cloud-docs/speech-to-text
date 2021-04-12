---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-11"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Release notes
{: #release-notes}

The following sections document the new features and changes that were included for each release and update of the {{site.data.keyword.speechtotextfull}} service.  The information includes any known limitations. Unless otherwise noted, all changes are compatible with earlier releases and are automatically and transparently available to all new and existing applications.
{: shortdesc}

## Known limitations
{: #limitations}

The service has the following known limitation:

-   The `GET /v1/models` and `GET /v1/models/{model_id}` methods list information about language models. Under `supported_features`, the `speaker_labels` field indicates whether you can use the `speaker_labels` parameter with a model. At this time, the field returns `true` for all models.

    However, speaker labels are supported as beta functionality only for US English, Australian English, German, Japanese, Korean, and Spanish (both broadband and narrowband models) and UK English (narrowband model only). Speaker labels are not supported for any other models. Do not rely on the field to identify which models support speaker labels.

    For more information about speaker labels and supported models, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels).

## 12 April 2021
{: #April2021}

The next-generation language models and the `low_latency` parameter are beta functionality. The next-generation models support a limited number of languages and features at this time. The supported languages, models, and features will increase with future releases.
{: beta}

The service now supports a growing number of next-generation language models. The next-generation *multimedia* and *telephony* models improve upon the speech recognition capabilities of the service's previous generation of broadband and narrowband models. The new models leverage deep neural networks and bidirectional analysis to achieve both higher throughput and greater transcription accuracy. At this time, the next-generation models support only a limited number of languages and speech recognition features.

Many of the next-generation models also support a new `low_latency` parameter that lets you request faster results at the possible expense of reduced transcription quality. When low latency is enabled, the service curtails its analysis of the audio, which can reduce the accuracy of the transcription. This trade-off might be acceptable if your application requires lower response time more than it does the highest possible accuracy.

The `low_latency` parameter impacts your use of the `interim_results` parameter with the WebSocket interface. Interim results are available only for those next-generation models that support low latency, and only if the `low_latency` parameter is set to `true`.

-   For more information about the next-generation models and their capabilities, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
-   For more information about language support for next-generation models and about which next-generation models support low latency, see [Supported language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).
-   For more information about feature support for next-generation models, see [Supported features](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features).
-   For more information about the `low_latency` parameter, see [Reducing the latency of speech recognition requests](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-low-latency).
-   For more information about the interaction between the `low_latency` and `interim_results` parameters for next-generation models, see [Requesting low latency and interim results](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-low-latency-websockets).

## 17 March 2021
{: #March2021}

**Defect fix:** The limitation that was reported with the asynchronous HTTP interface in the Dallas data center (`us-south`) on 16 December 2020 has been addressed. Previously, a small percentage of jobs were entering infinite loops that prevented their execution. Asynchronous HTTP requests in the Dallas data center no longer experience this limitation.

## 2 December 2020
{: #December2020}

The Arabic language broadband model is now named `ar-MS_BroadbandModel`. The former name, `ar-AR_BroadbandModel`, is deprecated. It will continue to function for at least one year but might be removed at a future date. You are encouraged to migrate to the new name at your earliest convenience.

## Older releases
{: #older}

-   [2 November 2020](#November2020)
-   [22 October 2020](#October2020b)
-   [7 October 2020](#October2020a)
-   [30 September 2020](#September2020)
-   [20 August 2020](#August2020b)
-   [5 August 2020](#August2020a)
-   [4 June 2020](#June2020)
-   [1 April 2020](#April2020a)
-   [28 April 2020](#April2020b)
-   [16 March 2020](#March2020)
-   [24 February 2020](#February2020)
-   [18 December 2019](#December2019c)
-   [12 December 2019](#December2019b)
-   [10 December 2019](#December2019a)
-   [25 November 2019](#November2019c)
-   [12 November 2019](#November2019b)
-   [1 November 2019](#November2019a)
-   [1 October 2019](#October2019)
-   [22 August 2019](#August2019)
-   [30 July 2019](#July2019)
-   [24 June 2019](#June2019b)
-   [10 June 2019](#June2019a)
-   [17 May 2019](#May2019b)
-   [10 May 2019](#May2019)
-   [19 April 2019](#April2019b)
-   [3 April 2019](#April2019)
-   [21 March 2019](#March2019d)
-   [15 March 2019](#March2019c)
-   [11 March 2019](#March2019b)
-   [4 March 2019](#March2019)
-   [28 January 2019](#January2019)
-   [20 December 2018](#December2018b)
-   [13 December 2018](#December2018a)
-   [12 November 2018](#November2018b)
-   [7 November 2018](#November2018a)
-   [30 October 2018](#October2018b)
-   [9 October 2018](#October2018a)
-   [10 September 2018](#September2018b)
-   [7 September 2018](#September2018a)
-   [8 August 2018](#August2018)
-   [13 July 2018](#July2018)
-   [12 June 2018](#June2018)
-   [15 May 2018](#May2018)
-   [26 March 2018](#March2018b)
-   [1 March 2018](#March2018a)
-   [1 February 2018](#February2018)
-   [14 December 2017](#December2017)
-   [2 October 2017](#October2017)
-   [14 July 2017](#July2017b)
-   [1 July 2017](#July2017a)
-   [22 May 2017](#May2017)
-   [10 April 2017](#April2017)
-   [8 March 2017](#March2017)
-   [1 December 2016](#December2016)
-   [22 September 2016](#September2016)
-   [30 June 2016](#June2016b)
-   [23 June 2016](#June2016a)
-   [10 March 2016](#March2016)
-   [19 January 2016](#January2016)
-   [17 December 2015](#December2015)
-   [21 September 2015](#September2015)
-   [1 July 2015](#July2015)

### 2 November 2020
{: #November2020}

-   The Canadian French models, `fr-CA_BroadbandModel` and `fr-CA_NarrowbandModel`, are now generally available; they were previously beta. They also now support language model and acoustic model customization.
    -   For more information about supported languages and models, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).
    -   For more information about language support for customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).

### 22 October 2020
{: #October2020b}

-   The Australian English models, `en-AU_BroadbandModel` and `en-AU_NarrowbandModel`, are now generally available; they were previously beta. They also now support language model and acoustic model customization.
    -   For more information about supported languages and models, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).
    -   For more information about language support for customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
-   The Brazilian Portuguese models, `pt-BR_BroadbandModel` and `pt-BR_NarrowbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The speech recognition parameter `split_transcript_at_phrase_end` is now generally available for all languages. Previously, it was generally available only for US and UK English. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-output#split_transcript).

### 7 October 2020
{: #October2020a}

-   The `ja-JP_BroadbandModel` model has been updated for improved speech recognition. By default, the service automatically uses the updated model for all speech recognition requests. If you have custom language or custom acoustic models that are based on this model, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

### 30 September 2020
{: #September2020}

The pricing plans for the service have changed:

-   The service continues to offer a Lite plan that provides basic no-charge access to limited minutes of speech recognition per month.
-   The service offers a new Plus plan that provides a simple tiered pricing model and access to the service's customization capabilities.
-   The service offers a new Premium plan that provides significantly greater capacity and enhanced features.

The Plus plan replaces the Standard plan. The Standard plan continues to be available for purchase for a short time. It also continues to be available indefinitely to existing users of the plan with no change in their pricing. Existing users can upgrade to the Plus plan at any time.
{: note}

For more information about the available pricing plans, see the following resources:

-   For general information about the pricing plans and answers to common questions, see the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).
-   For more information about the pricing plans or to purchase a plan, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/speech-to-text){: external}.

### 20 August 2020
{: #August2020b}

-   The service now offers beta broadband and narrowband models for Canadian French:
    -   `fr-CA_BroadbandModel`
    -   `fr-CA_NarrowbandModel`

    The new models do not support language model or acoustic model customization, speaker labels, or smart formatting. For more information about these and all supported models, see [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported).

### 5 August 2020
{: #August2020a}

-   The service now offers beta broadband and narrowband models for Australian English:
    -   `en-AU_BroadbandModel`
    -   `en-AU_NarrowbandModel`

    The new models do not support language model or acoustic model customization, or smart formatting. The new models do support speakers labels. For more information, see
    -   [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
    -   [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels)
-   The following models have been updated for improved speech recognition:
    -   French: `fr-FR_BroadbandModel`
    -   German: `de-DE_BroadbandModel` and `de-DE_NarrowbandModel`
    -   UK English: `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`
    -   US English: `en-US_ShortForm_NarrowbandModel`

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The hesitation marker that is used for the updated German broadband and narrowband models has changed from `[hesitation]` to `%HESITATION`. For more information about hesitation markers, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#hesitation).

### 4 June 2020
{: #June2020}

**Defect fix:** This release fixes a latency issue for custom language models that contain a large number of grammars. When initially used for speech recognition, such custom models could take multiple seconds to load. The custom models now load much faster, greatly reducing latency when they are used for recognition.

### 1 April 2020
{: #April2020a}

Acoustic model customization is now generally available (GA) for all supported languages. As with custom language models, {{site.data.keyword.IBM_notm}} does not charge for creating or hosting a custom acoustic model. You are charged only for using a custom model with a speech recognition request.

Using a custom language model, a custom acoustic model, or both types of model for transcription incurs an add-on charge of $0.03 (USD) per minute. This charge is in addition to the standard usage charge of $0.02 (USD) per minute, and it applies to all languages supported by the customization interface. So the total charge for using one or more custom models for speech recognition is $0.05 (USD) per minute.

-   For more information about support for individual language models, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
-   For more information about pricing, see the [pricing page](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} for the {{site.data.keyword.speechtotextshort}} service or the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).

### 28 April 2020
{: #April2020b}

-   The Italian broadband (`it-IT_BroadbandModel`) and narrowband (`it-IT_NarrowbandModel`) models have been updated for improved speech recognition. By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The Dutch and Italian language models are now generally available (GA) for speech recognition and for language model and acoustic model customization:
    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)

    For more information about all available language models, see
    -   [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport)

### 16 March 2020
{: #March2020}

-   The service now supports speaker labels (the `speaker_labels` parameter) for German and Korean language models. Speaker labels identify which individuals spoke which words in a multi-participant exchange. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   The service now supports the use of Activity Tracker events for all operations of the asynchronous HTTP interface. {{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud}}. For more information, see [Activity Tracker events](/docs/speech-to-text?topic=speech-to-text-atEvents).

### 24 February 2020
{: #February2020}

-   The following models have been updated for improved speech recognition:
    -   US English broadband model (`en-US_BroadbandModel`)
    -   Japanese narrowband model (`ja-JP_NarrowbandModel`)
    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   Language model customization is now supported for Dutch and Italian with the new versions of the broadband and narrowband models. For more information, see
    -   [Parsing of Brazilian Portuguese, Dutch, English, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)
    -   [Guidelines for Brazilian Portuguese, Dutch, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)

    Because the Dutch and Italian models are beta, their support for language model customization is also beta.
-   The Japanese narrowband model (`ja-JP_NarrowbandModel`) now includes some multigram word units for digits and decimal fractions. The service returns these multigram units regardless of whether you enable smart formatting. The smart formatting feature understands and returns the multigram units that the model generates. If you apply your own post-processing to transcription results, you need to handle these units appropriately. For more information, see [Japanese](/docs/speech-to-text?topic=speech-to-text-output#smartFormattingJapanese) in the smart formatting documentation.
-   The service now offers two new optional parameters for controlling the level of speech activity detection. The parameters can help ensure that only relevant audio is processed for speech recognition.
    -   The `speech_detector_sensitivity` parameter adjusts the sensitivity of speech activity detection. You can use the parameter to suppress word insertions from music, coughing, and other non-speech events.
    -   The `background_audio_suppression` parameter suppresses background audio based on its volume to prevent it from being transcribed or otherwise interfering with speech recognition. You can use the parameter to suppress side conversations or background noise.

    You can use the parameters individually or together. They are available for all interfaces and for most language models. For more information about the parameters, their allowable values, and their effect on the quality and latency of speech recognition, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection).
-   The service now supports the use of Activity Tracker events for all customization operations. {{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. For more information, see [Activity Tracker events](/docs/speech-to-text?topic=speech-to-text-atEvents).
-   **Defect fix:** The WebSocket interface now works seamlessly when generating processing metrics. Previously, processing metrics could continue to be delivered after the client sent a `stop` message to the service.

### 18 December 2019
{: #December2019c}

-   The service now offers beta broadband and narrowband models for the Italian language:

    -   `it-IT_BroadbandModel`
    -   `it-IT_NarrowbandModel`

    These language models support acoustic model customization. They do not support language model customization. Because they are beta, these models might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

    For more information, see the following sections:

    -   [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport)
-   For speech recognition, the service now supports the `end_of_phrase_silence_time` parameter. The parameter specifies the duration of the pause interval at which the service splits a transcript into multiple final results. Each final result indicates a pause or extended silence that exceeds the pause interval. For most languages, the default pause interval is 0.8 seconds; for Chinese the default interval is 0.6 seconds.

    You can use the parameter to effect a trade-off between how often a final result is produced and the accuracy of the transcription. Increase the interval when accuracy is more important than latency. Decrease the interval when the speaker is expected to say short phrases or single words.

    For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-output#silence_time).
-   For speech recognition, the service now supports the `split_transcript_at_phrase_end` parameter. The parameter directs the service to split the transcript into multiple final results based on semantic features of the input, such as at the conclusion of sentences. The service bases its understanding of semantic features on the base language model that you use with a request. Custom language models and grammars can also influence how and where the service splits a transcript.

    The parameter causes the service to add an `end_of_utterance` field to each final result to indicate the motivation for the split: `full_stop`, `silence`, `end_of_data`, or `reset`.

    For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-output#split_transcript).

### 12 December 2019
{: #December2019b}

-   **Full support for IBM Cloud IAM**
    -   The {{site.data.keyword.speechtotextshort}} service now supports the full implementation of {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). API keys for {{site.data.keyword.ibmwatson}} services are no longer limited to a single service instance. You can create access policies and API keys that apply to more than one service, and you can grant access between services. For more information about IAM, see [Authenticating to {{site.data.keyword.watson}} services](/docs/watson?topic=watson-iam).
    -   To support this change, the API service endpoints use a different domain and include the service instance ID. The pattern is `api.{location}.speech-to-text.watson.cloud.ibm.com/instances/{instance_id}`.

        -   Example HTTP URL for an instance hosted in the Dallas location:

            `https://api.us-south.speech-to-text.watson.cloud.ibm.com/instances/6bbda3b3-d572-45e1-8c54-22d6ed9e52c2`

        -   Example WebSocket URL for an instance hosted in the Dallas location:

            `wss://api.us-south.speech-to-text.watson.cloud.ibm.com/instances/6bbda3b3-d572-45e1-8c54-22d6ed9e52c2`

        For more information about the URLs, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text/speech-to-text#service-endpoint){: external}.

        These URLs do not constitute a breaking change. The new URLs work for both your existing service instances and for new instances. The original URLs continue to work on your existing service instances for at least one year, until December 2020.
-   **New network and data security features** for users of Premium plans:
    -   *Support for data encryption with customer-managed keys*
        -   You can integrate {{site.data.keyword.keymanagementservicefull}} with the {{site.data.keyword.speechtotextshort}} service to encrypt your data and manage encryption keys. For more information, see [Protecting sensitive information in your {{site.data.keyword.watson}} service](/docs/watson?topic=watson-keyservice).
    -   *Support for private network endpoints*
        -   You can create private network endpoints to connect to the {{site.data.keyword.speechtotextshort}} service over a private network. Connections to private network endpoints do not require public internet access. For more information, see [Public and private network endpoints](/docs/speech-to-text/?topic=watson-public-private-endpoints).

### 10 December 2019
{: #December2019a}

The service now offers beta broadband and narrowband models for the Dutch language:

-   `nl-NL_BroadbandModel`
-   `nl-NL_NarrowbandModel`

These language models support acoustic model customization. They do not support language model customization. Because they are beta, these models might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

For more information, see the following sections:

-   [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
-   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport)

### 25 November 2019
{: #November2019c}

Speaker labels are updated to improve the identification of individual speakers for further analysis of your audio sample. For more information about the speaker labels feature, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels). For more information about the improvements to the feature, see [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://www.ibm.com/blogs/research/2020/07/speaker-diarization-in-real-use-cases/){: external}.

### 12 November 2019
{: #November2019b}

The service is now available in the {{site.data.keyword.cloud_notm}} Seoul location (**kr-seo**). As with other locations, the {{site.data.keyword.cloud_notm}} location uses token-based IAM authentication. All new services instances that you create in this location use IAM authentication.

### 1 November 2019
{: #November2019a}

You can create no more than 1024 custom language models and no more than 1024 custom acoustic models per owning credentials. For more information, see [Maximum number of custom models](/docs/speech-to-text?topic=speech-to-text-customization#customMaximum).

### 1 October 2019
{: #October2019}

US HIPAA support is available for Premium plans that are hosted in the Washington, DC, location and are created on or after 1 April 2019. For more information, see [US Health Insurance Portability and Accountability Act (HIPAA)](/docs/speech-to-text?topic=speech-to-text-information-security#hipaa).

### 22 August 2019
{: #August2019}

The service was updated for small defect fixes and improvements.

### 30 July 2019
{: #July2019}

The service now offers broadband and narrowband language models in six Spanish dialects:

-   Argentinian Spanish (`es-AR_BroadbandModel` and `es-AR_NarrowbandModel`)
-   Castilian Spanish (`es-ES_BroadbandModel` and `es-ES_NarrowbandModel`)
-   Chilean Spanish (`es-CL_BroadbandModel` and `es-CL_NarrowbandModel`)
-   Colombian Spanish (`es-CO_BroadbandModel` and `es-CO_NarrowbandModel`)
-   Mexican Spanish (`es-MX_BroadbandModel` and `es-MX_NarrowbandModel`)
-   Peruvian Spanish (`es-PE_BroadbandModel` and `es-PE_NarrowbandModel`)

The Castilian Spanish models are not new. They are generally available for speech recognition and language model customization, and beta for acoustic model customization.

The other five dialects are new and are beta for all uses. Because they are beta, these additional dialects might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

For more information, see the following sections:

-   [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported)
-   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport)

### 24 June 2019
{: #June2019b}

-   The following narrowband models have been updated for improved speech recognition:
    -   US English narrowband model (`en-US_NarrowbandModel`)
    -   Brazilian Portuguese narrowband model (`pt-BR_NarrowbandModel`)

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The service now allows you to submit multiple simultaneous requests to add different audio resources to a custom acoustic model. Previously, the service allowed only one request at a time to add audio to a custom model.
-   The output of the HTTP `GET` methods that list information about custom language and custom acoustic models now includes an `updated` field. The field indicates the date and time in Coordinated Universal Time (UTC) at which the custom model was last modified.
-   The schema changed for a warning that is generated by a custom model training request when the `strict` parameter is set to `false`. The names of the fields changed from `warning_id` and `description` to `code` and `message`, respectively. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### 10 June 2019
{: #June2019a}

[Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing_metrics) are available only with the WebSocket and asynchronous HTTP interfaces. They are not supported with the synchronous HTTP interface.

### 17 May 2019
{: #May2019b}

-   The service now offers two types of optional metrics with speech recognition requests:
    -   [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing_metrics) provide detailed timing information about the service's analysis of the input audio. The service returns the metrics at specified intervals and with transcription events, such as interim and final results. Use the metrics to gauge the service's progress in transcribing the audio.
    -   [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio_metrics) provide detailed information about the signal characteristics of the input audio. The results provide aggregated metrics for the entire input audio at the conclusion of speech processing. Use the metrics to determine the characteristics and quality of the audio.

    You can request both types of metrics with any speech recognition request. By default, the service returns no metrics for a request.
-   The Japanese broadband model (`ja-JP_BroadbandModel`) has been updated for improved speech recognition. By default, the service automatically uses the updated model for all speech recognition requests. If you have custom language or custom acoustic models that are based on the model, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

### 10 May 2019
{: #May2019}

The Spanish language models have been updated for improved speech recognition:

-   `es-ES_BroadbandModel`
-   `es-ES_NarrowbandModel`

By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

### 19 April 2019
{: #April2019b}

-   The training methods of the customization interface now include a `strict` query parameter that indicates whether training is to proceed if a custom model contains a mix of valid and invalid resources. By default, training fails if a custom model contains one or more invalid resources. Set the parameter to `false` to allow training to proceed as long as the model contains at least one valid resource. The service excludes invalid resources from the training.
    -   For more information about using the `strict` parameter with the `POST /v1/customizations/{customization_id}/train` method, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language) and [Training failures](/docs/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
    -   For more information about using the `strict` parameter with the `POST /v1/acoustic_customizations/{customization_id}/train` method, see [Train the custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic) and [Training failures](/docs/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).
-   You can now add a maximum of 90 thousand out-of-vocabulary (OOV) words to the words resource of a custom language model. The previous maximum was 30 thousand OOV words. This figure includes OOV words from all sources (corpora, grammars, and individual custom words that you add directly). You can add a maximum of 10 million total words to a custom model from all sources. For more information, see [How much data do I need?](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount).

### 3 April 2019
{: #April2019}

Custom acoustic models now accept a maximum of 200 hours of audio. The previous maximum limit was 100 hours of audio.

### 21 March 2019
{: #March2019d}

Users can now see only service credential information that is associated with the role that has been assigned to their {{site.data.keyword.cloud_notm}} account. For example, if you are assigned a `reader` role, any `writer` or higher levels of service credentials are no longer visible.

This change does not affect API access for users or applications with existing service credentials. The change affects only the viewing of credentials within {{site.data.keyword.cloud_notm}}.

### 15 March 2019
{: #March2019c}

The service now supports audio in the A-law (`audio/alaw`) format. For more information, see [audio/alaw format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-alaw).

### 11 March 2019
{: #March2019b}

-   For the `max_alternatives` parameter, the service again accepts a value of `0`. If you specify `0`. the service automatically uses the default value, `1`. A change made for the March 4 service update caused a value of `0` to return an error. (The service returns an error if you specify a negative value.)
-   For the `word_alternatives_threshold` parameter, the service again accepts a value of `0` . A change made for the March 4 service update caused a value of `0` to return an error. (The service returns an error if you specify a negative value.)
-   The service now returns all confidence scores with a maximum precision of two decimal places. This change includes confidence scores for transcripts, word confidence, word alternatives, keyword results, and speaker labels.

### 4 March 2019
{: #March2019}

The following narrowband language models have been updated for improved speech recognition:

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

### 28 January 2019
{: #January2019}

The WebSocket interface now supports token-based Identity and Access Management (IAM) authentication from browser-based JavaScript code. The limitation to the contrary has been removed. To establish an authenticated connection with the WebSocket `/v1/recognize` method:

-   If you use IAM authentication, include the `access_token` query parameter.
-   If you use Cloud Foundry service credentials, include the `watson-token` query parameter.

For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#WSopen).

### 20 December 2018
{: #December2018b}

The grammar interface is fully functional in all locations as of January 8, 2019.
{: note}

-   The service now supports grammars for speech recognition. Grammars are available as beta functionality for all languages that support language model customization. You can add grammars to a custom language model and use them to restrict the set of phrases that the service can recognize from audio. You can define a grammar in Augmented Backus-Naur Form (ABNF) or XML Form.

    The following four methods are available for working with grammars:
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` adds a grammar file to a custom language model.
    -   `GET /v1/customizations/{customization_id}/grammars ` lists information about all grammars for a custom model.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` returns information about a specified grammar for a custom model.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` removes an existing grammar from a custom model.

    You can use a grammar for speech recognition with the WebSocket and HTTP interfaces. Use the `language_customization_id` and `grammar_name` parameters to identify the custom model and the grammar that you want to use. Currently, you can use only a single grammar with a speech recognition request.

    For more information about grammars, see the following documentation:
    -   [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars)
    -   [Understanding grammars](/docs/speech-to-text?topic=speech-to-text-grammarUnderstand)
    -   [Adding a grammar to a custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd)
    -   [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse)
    -   [Managing grammars](/docs/speech-to-text?topic=speech-to-text-manageGrammars)
    -   [Example grammars](/docs/speech-to-text?topic=speech-to-text-grammarExamples)

    For information about all methods of the interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   A new numeric redaction feature is now available to mask numbers that have three or more consecutive digits. Redaction is intended to remove sensitive personal information, such as credit card numbers, from transcripts. You enable the feature by setting the `redaction` parameter to `true` on a recognition request. The feature is beta functionality that is available for US English, Japanese, and Korean only. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-output#redaction).
-   The following new German and French language models are now available with the service:
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    Both new models support language model customization (GA) and acoustic model customization (beta). For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
-   A new US English language model, `en-US_ShortForm_NarrowbandModel`, is now available. The new model is intended for use in Interactive Voice Response and Automated Customer Support solutions. The model supports language model customization (GA) and acoustic model customization (beta). For more information, see [The US English short-form model](/docs/speech-to-text?topic=speech-to-text-models#modelsShortform).
-   The following language models have been updated for improved speech recognition:
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The service now supports audio in the G.729 (`audio/g729`) format. The service supports only G.729 Annex D for narrowband audio. For more information, see [audio/g729 format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-g729).
-   The speaker labels feature is now available for the narrowband model for UK English (`en-GB_NarrowbandModel`). The feature is beta functionality for all supported languages. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   The maximum amount of audio that you can add to a custom acoustic model has increased from 50 hours to 100 hours.

### 13 December 2018
{: #December2018a}

The service is now available in the {{site.data.keyword.cloud_notm}} London location (**eu-gb**). Like all locations, London uses token-based IAM authentication. All new services instances that you create in this location use IAM authentication.

### 12 November 2018
{: #November2018b}

The service now supports smart formatting for Japanese speech recognition. Previously, the service supported smart formatting for US English and Spanish only. The feature is beta functionality for all supported languages. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-output#smart_formatting).

### 7 November 2018
{: #November2018a}

The service is now available in the {{site.data.keyword.cloud_notm}} Tokyo location (**jp-tok**). Like all locations, Tokyo uses token-based IAM authentication. All new services instances that you create in this location use IAM authentication.

### 30 October 2018
{: #October2018b}

The service has migrated to token-based IAM authentication for all locations. All {{site.data.keyword.cloud_notm}} services now use IAM authentication. The {{site.data.keyword.speechtotextshort}} service migrated in each location on the following dates:

-   Dallas (**us-south**): October 30, 2018
-   Frankfurt (**eu-de**): October 30, 2018
-   Washington, DC (**us-east**): June 12, 2018
-   Sydney (**au-syd**): May 15, 2018

The migration to IAM authentication affects new and existing service instances differently:

-   *All new service instances that you create in any location* now use IAM authentication to access the service. You can pass either a bearer token or an API key: Tokens support authenticated requests without embedding service credentials in every call; API keys use HTTP basic authentication. When you use any of the {{site.data.keyword.watson}} SDKs, you can pass the API key and let the SDK manage the lifecycle of the tokens.
-   *Existing service instances that you created in a location before the indicated migration date* continue to use the `{username}` and `{password}` from their previous Cloud Foundry service credentials for authentication until you migrate them to use IAM authentication. For more information about migrating to IAM authentication, see [Migrating {{site.data.keyword.watson}} services from Cloud Foundry](/docs/text-to-speech?topic=watson-migrate).

For more information, see the following documentation:

-   To learn which authentication mechanism your service instance uses, view your service credentials by clicking the instance on the [{{site.data.keyword.cloud_notm}} dashboard](https://{DomainName}/dashboard/apps){: external}.
-   For more information about using IAM tokens with {{site.data.keyword.watson}} services, see [Authenticating to {{site.data.keyword.watson}} services](/docs/watson?topic=watson-iam).
-   For examples that use IAM authentication, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### 9 October 2018
{: #October2018a}

-   The `Content-Type` header is now optional for speech recognition requests. The service now automatically detects the audio format (MIME type) of most audio. You must continue to specify the content type for the following formats:
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Where indicated, the content type that you specify for these formats must include the sampling rate and can optionally include the number of channels and the endianness of the audio. For all other audio formats, you can omit the content type or specify a content type of `application/octet-stream` to have the service auto-detect the format.

    When you use the `curl` command to make a speech recognition request with the HTTP interface, you must specify the audio format with the `Content-Type` header, specify `"Content-Type: application/octet-stream"`, or specify `"Content-Type:"`. If you omit the header entirely, `curl` uses a default value of `application/x-www-form-urlencoded`. Most of the examples in this documentation continue to specify the format for speech recognition requests regardless of whether it's required.
    {: important}

    This change applies to the following methods:
    -   `/v1/recognize` for WebSocket requests. The `content-type` field of the text message that you send to initiate a request over an open WebSocket connection is now optional.
    -   `POST /v1/recognize` for synchronous HTTP requests. The `Content-Type` header is now optional. (For multipart requests, the `part_content_type` field of the JSON metadata is also now optional.)
    -   `POST /v1/recognitions` for asynchronous HTTP requests. The `Content-Type` header is now optional.

    For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list).
-   The Brazilian Portuguese broadband model, `pt-BR_BroadbandModel`, was updated for improved speech recognition. By default, the service automatically uses the updated model for all recognition requests. If you have custom language or custom acoustic models that are based on this model, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The `customization_id` parameter of the speech recognition methods is deprecated and will be removed in a future release. To specify a custom language model for a speech recognition request, use the `language_customization_id` parameter instead. This change applies to the following methods:
    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for synchronous HTTP requests (including multipart requests)
    -   `POST /v1/recognitions` for asynchronous HTTP requests
-   As of October 1, 2018, you are now charged for all audio that you pass to the service for speech recognition. The first one thousand minutes of audio that you send each month are no longer free. For more information about the pricing plans for the service, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud_notm}} Catalog](https://{DomainName}/catalog/services/speech-to-text){: external}.

### 10 September 2018
{: #September2018b}

For a list of issues that have been fixed since the initial release, see [Resolved issues](#known_issues).
{: important}

-   The service now supports a German broadband model, `de-DE_BroadbandModel`. The new German model supports language model customization (generally available) and acoustic model customization (beta).
    -   For information about how the service parses corpora for German, see [Parsing of Brazilian Portuguese, Dutch, English, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in German, see [Guidelines for Brazilian Portuguese, Dutch, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).
-   The existing Brazilian Portuguese models, `pt-BR_BroadbandModel` and `pt-BR_NarrowbandModel`, now support language model customization (generally available). The models were not updated to enable this support, so no upgrade of existing custom acoustic models is required.
    -   For information about how the service parses corpora for Brazilian Portuguese, see [Parsing of Brazilian Portuguese, Dutch, English, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in Brazilian Portuguese, see [Guidelines for Brazilian Portuguese, Dutch, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).
-   New versions of the US English and Japanese broadband and narrowband models are available:
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    The new models offer improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The keyword spotting and word alternatives features are now generally available (GA) rather than beta functionality for all languages. For more information, see
    -   [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-output#keyword_spotting)
    -   [Word alternatives](/docs/speech-to-text?topic=speech-to-text-output#word_alternatives)

### Resolved issues
{: #known_issues}

The following known issues that were associated with the customization interface have been resolved. This list is maintained for users who may have encountered the problems in the past.

-   If you add data to a custom language or custom acoustic model, you must retrain the model before using it for speech recognition. The problem shows up in the following scenario:

    1.  The user creates a new custom model (language or acoustic) and trains the model.
    1.  The user adds additional resources (words, corpora, or audio) to the custom model but does not retrain the model.
    1.  The user cannot use the custom model for speech recognition. The service returns an error of the following form when used with a speech recognition request:

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    To work around this issue, the user must retrain the custom model on its latest data. The user can then use the custom model with speech recognition.
-   Before training an existing custom language or custom acoustic model, you must upgrade it to the latest version of its base model. The problem shows up in the following scenario:

    1.  The user has an existing custom model (language or acoustic) that is based on a model that has been updated.
    1.  The user trains the existing custom model against the old version of the base model without first upgrading to the latest version of the base model.
    1.  The user cannot use the custom model for speech recognition.

    To work around this issue, the user must use the `POST /v1/customizations/{customization_id}/upgrade_model` or `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` method to upgrade the custom model to the latest version of its base model. The user can then use the custom model with speech recognition.

Both of these issues have been fixed in production.

### 7 September 2018
{: #September2018a}

The session-based HTTP REST interface is no longer supported. All information related to sessions is removed from the documentation. The following methods are no longer available:
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

If your application uses the sessions interface, you must migrate to one of the remaining HTTP REST interfaces or to the WebSocket interface. For more information, see the service update for [8 August 2018](#August2018).

### 8 August 2018
{: #August2018}

The session-based HTTP REST interface is deprecated as of **August 8, 2018**. All methods of the sessions API will be removed from service as of **September 7, 2018**, after which you will no longer be able to use the session-based interface. This notice of immediate deprecation and 30-day removal applies to the following methods:

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

If your application uses the sessions interface, you must migrate to one of the following interfaces by September 7:
{: important}

-   For stream-based speech recognition (including live-use cases), use the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), which provides access to interim results and the lowest latency.
-   For file-based speech recognition, use one of the following interfaces:

    -   For shorter files of up to a few minutes of audio, use either the [synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http) `(POST /v1/recognize`) or the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) (`POST /v1/recognitions`).
    -   For longer files of more than a few minutes of audio, use the asynchronous HTTP interface. The asynchronous HTTP interface accepts as much as 1 GB of audio data with a single request.

The WebSocket and HTTP interfaces provide the same results as the sessions interface (only the WebSocket interface provides interim results). You can also use one of the {{site.data.keyword.watson}} SDKs, which simplify application development with any of the interfaces. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### 13 July 2018
{: #July2018}

The Spanish Narrowband model, `es-ES_NarrowbandModel`, was updated for improved speech recognition. By default, the service automatically uses the updated model for all recognition requests. If you have custom language or custom acoustic models that are based on this model, you must upgrade your custom models to take advantage of the updates by using the following methods:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

As of this update, the following two versions of the Spanish narrowband model are available:

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (current)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (previous)

The following version of the model is no longer available:

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

A recognition request that attempts to use a custom model that is based on the now unavailable base model uses the latest base model without any customization. The service returns the following warning message: `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` To resume using a custom model that is based on the unavailable model, you must first upgrade the model by using the appropriate `upgrade_model` method described previously.

### 12 June 2018
{: #June2018}

The following features are enabled for applications that are hosted in Washington, DC (**us-east**):

-   The service now supports a new API authentication process. For more information, see the [30 October 2018 service update](#October2018b).
-   The service now supports the `X-Watson-Metadata` header and the `DELETE /v1/user_data` method. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

### 15 May 2018
{: #May2018}

The following features are enabled for applications that are hosted in Sydney (**au-syd**):

-   The service now supports a new API authentication process. For more information, see the [30 October 2018 service update](#October2018b).
-   The service now supports the `X-Watson-Metadata` header and the `DELETE /v1/user_data` method. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

### 26 March 2018
{: #March2018b}

-   The service now supports language model customization for the French language model, `fr-FR_BroadbandModel`. The French model is generally available for production use with language model customization.
    -   For more information about how the service parses corpora for French, see [Parsing of Brazilian Portuguese, Dutch, English, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in French, see [Guidelines for Brazilian Portuguese, Dutch, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).
-   The Spanish and Korean narrowband models, `es-ES_NarrowbandModel` and `ko-KR_NarrowbandModel`, and the French broadband model, `fr-FR_BroadbandModel`, were updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on either of these models, you must upgrade your custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   The `version` parameter of the following methods is renamed `base_model_version`:

    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for sessionless HTTP requests
    -   `POST /v1/sessions` for session-based HTTP requests
    -   `POST /v1/recognitions` for asynchronous HTTP requests

    The `base_model_version` parameter specifies the version of a base model that is to be used for speech recognition. For more information, see [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use) and [Making speech recognition requests with upgraded custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-recognition).
-   Smart formatting is now supported for Spanish as well as US English. For US English, the feature also now converts keyword strings into punctuation symbols for periods, commas, question marks, and exclamation points. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-output#smart_formatting).

### 1 March 2018
{: #March2018a}

The Spanish and French broadband models, `es-ES_BroadbandModel` and `fr-FR_BroadbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on either of these models, you must upgrade your custom models to take advantage of the updates by using the following methods:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade). The section presents rules for upgrading custom models, the effects of upgrading, and approaches for using upgraded models.

### 1 February 2018
{: #February2018}

The service now offers models for the Korean language for speech recognition: `ko-KR_BroadbandModel` for audio that is sampled at a minimum of 16 kHz, and `ko-KR_NarrowbandModel` for audio that is sampled at a minimum of 8 kHz. For more information, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).

For language model customization, the Korean models are generally available for production use; for acoustic model customization, they are beta functionality. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).

-   For more information about how the service parses corpora for Korean, see [Parsing of Korean](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages-koKR).
-   For more information about creating sounds-like pronunciations for custom words in Korean, see [Guidelines for Korean](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-koKR).

### 14 December 2017
{: #December2017}

-   The US English models, `en-US_BroadbandModel` and `en-US_NarrowbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on the US English models, you must upgrade your custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information about the procedure, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade). The section presents rules for upgrading custom models, the effects of upgrading, and approaches for using upgraded models. Currently, the methods apply only to the new US English base models. But the same information will apply to upgrades of other base models as they become available.
-   The various methods for making recognition requests now include a new `base_model_version` parameter that you can use to initiate requests that use either the older or upgraded versions of base and custom models. Although it is intended primarily for use with custom models that have been upgraded, the `base_model_version` parameter can also be used without custom models. For more information, see [Making speech recognition requests with upgraded custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-recognition).
-   The service now supports acoustic model customization as beta functionality for all available languages. You can create custom acoustic models for broadband or narrowband models for all languages. For an introduction to customization, including acoustic model customization, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
-   The service now supports language model customization for the UK English models, `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`. Although the service handles UK and US English corpora and custom words in a generally similar fashion, some important differences exist:
    -   For more information about how the service parses corpora for UK English, see [Parsing of Brazilian Portuguese, Dutch, English, French, German, Italian, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in UK English, see [Guidelines for English](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-english). Specifically, for UK English, you cannot use periods or dashes in sounds-like pronunciations.
-   Language model customization and all associated parameters are now generally available (GA) for all supported languages: Japanese, Spanish, UK English, and US English.

### 2 October 2017
{: #October2017}

-   The customization interface now offers acoustic model customization. You can create custom acoustic models that adapt the service's base models to your environment and speakers. You populate and train a custom acoustic model on audio that more closely matches the acoustic signature of the audio that you want to transcribe. You then use the custom acoustic model with recognition requests to increase the accuracy of speech recognition.

    Custom acoustic models complement custom language models. You can train a custom acoustic model with a custom language model, and you can use both types of model during speech recognition. Acoustic model customization is a beta interface that is available only for US English, Spanish, and Japanese.

    -   For more information about the languages that are supported by the customization interface and the level of support that is available for each language, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
    -   For more information about the service's customization interface, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
    -   For more information about creating a custom acoustic model, see [Creating a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic).
    -   For more information about using a custom acoustic model, see [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse).
    -   For more information about all methods of the customization interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   For language model customization, the service now includes a beta feature that sets an optional customization weight for a custom language model. A customization weight specifies the relative weight to be given to words from a custom language model versus words from the service's base vocabulary. You can set a customization weight during both training and speech recognition. For more information, see [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight).
-   The `ja-JP_BroadbandModel` language model was upgraded to capture improvements in the base model. The upgrade does not affect existing custom models that are based on the model.
-   The service now includes a parameter to specify the endianness of audio that is submitted in `audio/l16` (Linear 16-bit Pulse-Code Modulation (PCM)) format. In addition to specifying `rate` and `channels` parameters with the format, you can now also specify `big-endian` or `little-endian` with the `endianness` parameter. For more information, see [audio/l16 format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-l16).

### 14 July 2017
{: #July2017b}

-   The service now supports the transcription of audio in the MP3 or Motion Picture Experts Group (MPEG) format. For more information, see [audio/mp3 and audio/mpeg formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-mp3).
-   The language model customization interface now supports Spanish as beta functionality. You can create a custom model based on either of the base Spanish language models: `es-ES_BroadbandModel` or `es-ES_NarrowbandModel`; for more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate). Pricing for recognition requests that use Spanish custom language models is the same as for requests that use US English and Japanese models.
-   The JSON `CreateLanguageModel` object that you pass to the `POST /v1/customizations` method to create a new custom language model now includes a `dialect` field. The field specifies the dialect of the language that is to be used with the custom model. By default, the dialect matches the language of the base model. The parameter is meaningful only for Spanish models, for which the service can create a custom model that is suited for speech in one of the following dialects:
    -   `es-ES` for Castilian Spanish (the default)
    -   `es-LA` for Latin-American Spanish
    -   `es-US` for North-American (Mexican) Spanish

    The `GET /v1/customizations` and `GET /v1/customizations/{customization_id}` methods of the customization interface include the dialect of a custom model in their output. For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#createModel-language) and [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   The names of the language models `en-UK_BroadbandModel` and `en-UK_NarrowbandModel` have been deprecated. The models are now available with the names `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`.

    The deprecated `en-UK_{model}` names continue to function, but the `GET /v1/models` method no longer returns the names in the list of available models. You can still query the names directly with the `GET /v1/models/{model_id}` method.

### 1 July 2017
{: #July2017a}

-   The language model customization interface of the service is now generally available (GA) for both of its supported languages, US English and Japanese. {{site.data.keyword.IBM_notm}} does not charge for creating, hosting, or managing custom language models. As described in the next bullet, {{site.data.keyword.IBM_notm}} now charges an extra $0.03 (USD) per minute of audio for recognition requests that use custom models.
-   {{site.data.keyword.IBM_notm}} updated the pricing for the service by
    -   Eliminating the add-on price for using narrowband models
    -   Providing Graduated Tiered Pricing for high-volume customers
    -   Charging an additional $0.03 (USD) per minute of audio for recognition requests that use US English or Japanese custom language models

    For more information about the pricing updates, see
    -   The {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud_notm}} Catalog](https://{DomainName}/catalog/services/speech-to-text){: external}
    -   The [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing)
-   You no longer need to pass an empty data object as the body for the following `POST` requests:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    For example, you now call the `POST /v1/sessions` method with `curl` as follows:

    ```bash
    curl -X POST -u "{username}:{password}" \
    --cookie-jar cookies.txt \
    "{url}/v1/sessions"
    ```
    {: pre}

    You no longer need to pass the following `curl` option with the request: `--data "{}"`.

    If you experience any problems with one of these `POST` requests, try passing an empty data object with the body of the request. Passing an empty object does not change the nature or meaning of the request in any way.
    {: note}

### 22 May 2017
{: #May2017}

The following deprecations first announced in March 2017 are now in effect:

-   The `continuous` parameter is removed from all methods that initiate recognition requests. The service now transcribes an entire audio stream until it ends or times out, whichever occurs first. This behavior is equivalent to setting the former `continuous` parameter to `true`. By default, the service previously stopped transcription at the first half-second of non-speech (typically silence) if the parameter was omitted or set to `false`.

    Existing applications that set the parameter to `true` will see no change in behavior. Applications that set the parameter to `false` or that relied on the default behavior will likely see a change. If a request specifies the parameter, the service now responds by returning a warning message for the unknown parameter:

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    The request succeeds despite the warning, and an existing session or WebSocket connection is not affected.

    {{site.data.keyword.IBM_notm}} removed the parameter to respond to overwhelming feedback from the developer community that specifying `continuous=false` added little value and could reduce overall transcription accuracy.
-   It is no longer possible to avoid a session timeout without sending audio:
    -   When you use the WebSocket interface, the client can no longer keep a connection alive by sending a JSON text message with the `action` parameter set to `no-op`. Sending a `no-op` message does not generate an error, but it has no effect.
    -   When you use sessions with the HTTP interface, the client can no longer extend the session by sending a `GET /v1/sessions/{session_id}/recognize` request. The method still returns the status of an active session, but it does not keep the session active.
    You can now do the following to keep a session alive:
    -   Set the `inactivity_timeout` parameter to `-1` to avoid the 30-second inactivity timeout.
    -   Send any audio data, including just silence, to the service to avoid the 30-second session timeout. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

    For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts). Ideally, you would establish a session just before you obtain audio for transcription and maintain the session by sending audio at a rate that is close to real time. Also, make sure your application recovers gracefully from closed sessions or connections.

    {{site.data.keyword.IBM_notm}} removed this functionality to ensure that it continues to offer all users a best-in-class, low-latency speech recognition service.

### 10 April 2017
{: #April2017}

-   The service now supports the speaker labels feature for the following broadband models:
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   The service now supports the Web Media (WebM) audio format with the Opus or Vorbis codec. The service now also supports the Ogg audio format with the Vorbis codec in addition to the Opus codec. For more information about supported audio formats, see [audio/webm format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-webm).
-   The service now supports Cross-Origin Resource Sharing (CORS) to allow browser-based clients to call the service directly. For more information, see [Leveraging CORS support](/docs/speech-to-text?topic=speech-to-text-service-features#features-cors).
-   The asynchronous HTTP interface now offers a `POST /v1/unregister_callback` method that removes the registration for an allowlisted callback URL. For more information, see [Unregistering a callback URL](/docs/speech-to-text?topic=speech-to-text-async#unregister).
-   The WebSocket interface no longer times out for recognition requests for especially long audio files. You no longer need to request interim results with the JSON `start` message to avoid the timeout. (This issue was described in the release notes for March 10, 2016.)
-   The `DELETE /v1/customizations/{customization_id}` method now returns HTTP response code 401 if you attempt to delete a nonexistent custom model.
-   The `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` method now returns HTTP response code 400 if you attempt to delete a nonexistent corpus.

### 8 March 2017
{: #March2017}

The asynchronous HTTP interface is now generally available. Prior to this date, it was beta functionality.

### 1 December 2016
{: #December2016}

-   The service now offers a beta speaker labels feature for narrowband audio in US English, Spanish, or Japanese. The feature identifies which words were spoken by which speakers in a multi-person exchange. The sessionless, session-based, asynchronous, and WebSocket recognition methods each include a `speaker_labels` parameter that accepts a boolean value to indicate whether speaker labels are to be included in the response. For more information about the feature, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   The beta language model customization interface is now supported for Japanese in addition to US English. All methods of the interface support Japanese. For more information, see the following sections:
    -   For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate) and [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse).
    -   For general and Japanese-specific considerations for adding a corpus text file, see [Preparing a corpus text file](/docs/speech-to-text?topic=speech-to-text-corporaWords#prepareCorpus) and [What happens when I add a corpus file?](/docs/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)
    -   For Japanese-specific considerations when specifying the `sounds_like` field for a custom word, see [Guidelines for Japanese](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-jaJP).
    -   For more information about all methods of the customization interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   The language model customization interface now includes a `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method that lists information about a specified corpus. The method is useful for monitoring the status of a request to add a corpus to a custom model. For more information, see [Listing corpora for a custom language model](/docs/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).
-   The JSON output that is returned by the `GET /v1/customizations/{customization_id}/words` and `GET /v1/customizations/{customization_id}/words/{word_name}` methods now includes a `count` field for each word. The field indicates the number of times the word is found across all corpora. If you add a custom word to a model before it is added by any corpora, the count begins at `1`. If the word is added from a corpus first and later modified, the count reflects only the number of times it is found in corpora. For more information, see [Listing words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

    For custom models that were created prior to the existence of the `count` field, the field always remains at `0`. To update the field for such models, add the model's corpora again and include the `allow_overwrite` parameter with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method.
-   The `GET /v1/customizations/{customization_id}/words` method now includes a `sort` query parameter that controls the order in which the words are to be listed. The parameter accepts two arguments, `alphabetical` or `count`, to indicate how the words are to be sorted. You can prepend an optional `+` or `-` to an argument to indicate whether the results are to be sorted in ascending or descending order. By default, the method displays the words in ascending alphabetical order. For more information, see [Listing words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

    For custom models created prior to the introduction of the `count` field, use of the `count` argument with the `sort` parameter is meaningless. Use the default `alphabetical` argument with such models.
-   The `error` field that can be returned as part of the JSON response from the `GET /v1/customizations/{customization_id}/words` and `GET /v1/customizations/{customization_id}/words/{word_name}` methods is now an array. If the service discovered one or more problems with a custom word's definition, the field lists each problem element from the definition and provides a message that describes the problem. For more information, see [Listing words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   The `keywords_threshold` and `word_alternatives_threshold` parameters of the recognition methods no longer accept a null value. To omit keywords and word alternatives from the response, omit the parameters. A specified value must be a float.

For more information about the service's interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### 22 September 2016
{: #September2016}

-   The service now offers a new beta language model customization interface for US English. You can use the interface to tailor the service's base vocabulary and language models via the creation of custom language models that include domain-specific terminology. You can add custom words individually or have the service extract them from corpora. To use your custom models with the speech recognition methods that are offered by any of the service's interfaces, pass the `customization_id` query parameter. For more information, see
    -   [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate)
    -   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)
    -   [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}
-   The list of supported audio formats now includes `audio/mulaw`, which provides single-channel audio encoded using the u-law (or mu-law) data algorithm. When you use this format, you must also specify the sampling rate at which the audio is captured. For more information, see [audio/mulaw format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-mulaw).
-   The `GET /v1/models` and `GET /v1/models/{model_id}` methods now return a `supported_features` field as part of their output for each language model. The additional information describes whether the model supports customization. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### 30 June 2016
{: #June2016b}

The beta asynchronous HTTP interface now supports all languages that are supported by the service. The interface was previously available for US English only. For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) and the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### 23 June 2016
{: #June2016a}

-   A beta asynchronous HTTP interface is now available. The interface provides full recognition capabilities for US English transcription via non-blocking HTTP calls. You can register callback URLs and provide user-specified secret strings to achieve authentication and data integrity with digital signatures. For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) and the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   A beta smart formatting feature that converts dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses into more conventional representations in final transcripts. You enable the feature by setting the `smart_formatting` parameter to `true` on a recognition request. The feature is beta functionality that is available for US English only. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-output#smart_formatting).
-   The list of supported models for speech recognition now includes `fr-FR_BroadbandModel` for audio in the French language that is sampled at a minimum of 16 kHz. For more information, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).
-   The list of supported audio formats now includes `audio/basic`. The format provides single-channel audio that is encoded by using 8-bit u-law (or mu-law) data that is sampled at 8 kHz. For more information, see [audio/basic format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-basic).
-   The various recognition methods can return a `warnings` response that includes messages about invalid query parameters or JSON fields that are included with a request. The format of the warnings changed. For example, `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` is now `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`
-   For HTTP `POST` requests that do not otherwise pass data to the service, you must include an empty request body of the form `{}`. With the `curl` command, you use the `--data` option to pass the empty data.

### 10 March 2016
{: #March2016}

-   Both forms of data transmission (one-shot delivery and streaming) now impose a size limit of 100 MB on the audio data, as does the WebSocket interface. Formerly, the one-shot approach had a maximum limit of 4 MB of data. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission) (for all interfaces) and [Send audio and receive recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#WSaudio) (for the WebSocket interface). The WebSocket section also discusses the maximum frame or message size of 4 MB enforced by the WebSocket interface.
-   The JSON response for a recognition request can now include an array of warning messages for invalid query parameters or JSON fields that are included with a request. Each element of the array is a string that describes the nature of the warning followed by an array of invalid argument strings. For example, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   The beta *{{site.data.keyword.watson}} Speech Software Development Kit (SDK) for the Apple&reg; iOS operating system* is deprecated. Use the *{{site.data.keyword.watson}} SDK for the Apple&reg; iOS operating system* instead. The new SDK is available from the [ios-sdk repository](https://github.com/watson-developer-cloud/ios-sdk){: external} in the `watson-developer-cloud` namespace on GitHub.

The WebSocket interface currently has the following known issue:

-   The service can take minutes to produce final results for a recognition request for an especially long audio file. For the WebSocket interface, the underlying TCP connection remains idle while the service prepares the response. Therefore, the connection can close due to a timeout. To avoid the timeout with the WebSocket interface, request interim results (`\"interim_results\": \"true\"`) in the JSON for the `start` message to initiate the request. You can discard the interim results if you do not need them. This issue will be resolved in a future update.

### 19 January 2016
{: #January2016}

The service was updated to include a new profanity filtering feature on January 19, 2016. By default, the service censors profanity from its transcription results for US English audio. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-output#profanity_filter).

### 17 December 2015
{: #December2015}

-   The service now offers a keyword spotting feature. You can specify an array of keyword strings that are to be matched in the input audio. You must also specify a user-defined confidence level that a word must meet to be considered a match for a keyword. For more information, see [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-output#keyword_spotting). The keyword spotting feature is beta functionality.
-   The service now offers a word alternatives feature. The feature returns alternative hypotheses for words in the input audio that meet a user-defined confidence level. For more information, see [Word alternatives](/docs/speech-to-text?topic=speech-to-text-output#word_alternatives). The word alternatives feature is beta functionality.
-   The service supports more languages with its transcription models: `en-UK_BroadbandModel` and `en-UK_NarrowbandModel` for UK English, and `ar-AR_BroadbandModel` for Modern Standard Arabic. For more information, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).
-   HTTP recognition requests are no longer subject to a 10-minute platform timeout. The service now keeps the connection alive by sending a space character in the response JSON object every 20 seconds as long as recognition is ongoing. For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts).
-   The service no longer returns HTTP status code 490 for the session-based HTTP methods `GET /v1/sessions/{session_id}/observe_result` and `POST /v1/sessions/{session_id}/recognize`. The service now responds with HTTP status code 400 instead.

    In the JSON responses that it returns for errors with session-based methods, the service now also includes a new `session_closed` field. The field is set to `true` if the session is closed as a result of the error. For more information about possible return codes for any method, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
-   When you use the `curl` command to transcribe audio with the service, you no longer need to use the `--limit-rate` option to transfer data at a rate no faster than 40,000 bytes per second.

### 21 September 2015
{: #September2015}

-   Two new beta mobile SDKs are available for the speech services. The SDKs enable mobile applications to interact with both the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} services.
    -   The *{{site.data.keyword.watson}} Speech SDK for the Google Android&trade; platform* supports streaming audio to the {{site.data.keyword.speechtotextshort}} service in real time and receiving a transcript of the audio as you speak. The project includes an example application that showcases interaction with both of the speech services. The SDK is available from the [speech-android-sdk repository](https://github.com/watson-developer-cloud/speech-android-sdk){: external} in the `watson-developer-cloud` namespace on GitHub.
    -   The *{{site.data.keyword.watson}} Speech SDK for the Apple&reg; iOS operating system* supports streaming audio to the {{site.data.keyword.speechtotextshort}} service and receiving a transcript of the audio in response. The SDK is available from the [speech-ios-sdk repository](https://github.com/watson-developer-cloud/speech-ios-sdk){: external} in the `watson-developer-cloud` namespace on GitHub.

    Both SDKs support authenticating with the speech services by using either your {{site.data.keyword.cloud_notm}} service credentials or an authentication token.

    Because the SDKs are beta, they are subject to change in the future.
    {: note}
-   The service supports two new languages, Brazilian Portuguese and Mandarin Chinese. The models for these new languages are `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel`, and `zh-CN_NarrowbandModel`. For more information, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).
-   The HTTP `POST` requests `/v1/sessions/{session_id}/recognize` and `/v1/recognize`, as well as the WebSocket `/v1/recognize` request, support transcription of a new media type: `audio/ogg;codecs=opus` for Ogg format files that use the Opus codec. In addition, the `audio/wav` format for the methods now supports any encoding. The restriction about the use of linear PCM encoding is removed. For more information, see [audio/ogg format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-ogg).
-   The service now supports overcoming timeouts when you transcribe long audio files with the HTTP interface. When you use sessions, you can employ a long polling pattern by specifying sequence IDs with the `GET /v1/sessions/{session_id}/observe_result` and `POST /v1/sessions/{session_id}/recognize` methods for long-running recognition tasks. By using the new `sequence_id` parameter of these methods, you can request results before, during, or after you submit a recognition request.
-   For the US English language models, `en_US_BroadbandModel` and `en_US_NarrowbandModel`, the service now correctly capitalizes many proper nouns. For example, the service would new return text that reads "Barack Obama graduated from Columbia University" instead of "barack obama graduated from columbia university." This change might be of interest to you if your application is sensitive in any way to the case of proper nouns.
-   The HTTP `DELETE /v1/sessions/{session_id}` request does not return status code 415 "Unsupported Media Type." This return code is removed from the documentation for the method.

### 1 July 2015
{: #July2015}

The service moved from beta to general availability (GA) on July 1, 2015. The following differences exist between the beta and GA versions of the {{site.data.keyword.speechtotextshort}} APIs. The GA release requires that users upgrade to the new version of the service.

-   The GA version of the HTTP API is compatible with the beta version. You need to change your existing application code only if you explicitly specified a model name. For example, the sample code available for the service from GitHub included the following line of code in the file `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    This line specified the default model, `WatsonModel`, for the beta version of the service. If your application also specified this model, you need to change it to use one of the new models that are supported by the GA version. For more information, see the next bullet.
-   The service now supports a new programming model for direct interaction between a client and the service over a WebSocket connection. By using this model, a client can obtain an authentication token for communicating directly with the service. The token bypasses the need for a server-side proxy application in {{site.data.keyword.cloud_notm}} to call the service on the client's behalf. Tokens are the preferred means for clients to interact with the service.

    The service continues to support the old programming model that relied on a server-side proxy to relay audio and messages between the client and the service. But the new model is more efficient and provides higher throughput.
-   The `POST /v1/sessions` and `POST /v1/recognize` methods, along with the WebSocket `/v1/recognize` method, now support a `model` query parameter. You use the parameter to specify information about the audio:

    -   The language: *English*, *Japanese*, or *Spanish*
    -   The minimum sampling rate: *broadband* (16 kHz) or *narrowband* (8 kHz)

    For more information, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).
-   The `Content-Type` header of the `recognize` methods now supports `audio/wav` for Waveform Audio File Format (WAV) files, in addition to `audio/flac` and `audio/l16`. For more information, see [audio/wav format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-wav).
-   The `recognize` methods now support a number of new query parameters that you can use to tailor the service to suit your application needs:
    -   The `inactivity_timeout` parameter sets the timeout value in seconds after which the service closes the connection if it detects silence (no speech) in streaming mode. By default, the service terminates the session after 30 seconds of silence. For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts).
    -   The `max_alternatives` parameter instructs the service to return the *n*-best alternative hypotheses for the audio transcription. For more information, see [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-output#max_alternatives).
    -   The `word_confidence` parameter instructs the service to return a confidence score for each word of the transcription. For more information, see [Word confidence](/docs/speech-to-text?topic=speech-to-text-output#word_confidence).
    -   The `timestamps` parameter instructs the service to return the beginning and ending time relative to the start of the audio for each word of the transcription. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-output#word_timestamps).
-   The `GET /v1/sessions/{session_id}/observeResult` method is now named `GET /v1/sessions/{session_id}/observe_result`. The name `observeResult` is still supported for backward compatibility.
-   The `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize`, and `POST /v1/recognize` methods now include the header parameter `X-WDC-PL-OPT-OUT` to control whether the service uses the audio and transcription data from a request to improve future results. The WebSocket interface includes an equivalent query parameter. Specify a value of `1` to prevent the service from using the audio and transcription results. The parameter applies only to the current request. The new header replaces the `X-logging` header from the beta API. See [Controlling request logging for {{site.data.keyword.watson}} services](/docs/watson?topic=watson-gs-logging-overview).
-   The service now has a limit of 100 MB of data per session in streaming mode. You specify streaming mode by specifying the value `chunked` with the header `Transfer-Encoding`. One-shot delivery of an audio file still imposes a size limit of 4 MB on the data that is sent. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission).
-   For the `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize`, and `/v1/recognize` methods, error code 415 ("Unsupported Media Type") is added.
-   For `POST` and `GET` requests to the `/v1/sessions/{session_id}/recognize` method, the following error codes have been modified:
    -   Error code 404 ("Session_id not found") has a more descriptive message (`POST` and `GET`).
    -   Error code 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") has a more descriptive message (`POST` only).
-   For HTTP `POST` requests to the `/v1/sessions` and `/v1/recognize` methods, error code 503 ("Service Unavailable") can be returned. The error code can also be returned when creating a WebSocket connection with the `/v1/recognize` method.
