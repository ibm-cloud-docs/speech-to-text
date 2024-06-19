---

copyright:
  years: 2015, 2023
lastupdated: "2023-07-28"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Previous-generation languages and models
{: #models}

Starting **August 1, 2023**, all previous-generation models are now **discontinued** from the service. New clients must now only use the next-generation models. All existing clients must now migrate to the equivalent next-generation model. For more information, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
{: attention}

The {{site.data.keyword.speechtotextfull}} service supports speech recognition with previous-generation models in many languages. The model indicates the language in which the audio is spoken and the rate at which it is sampled.
{: shortdesc}

The models described on this page are referred to as *previous-generation models*. The service also offers *next-generation models* with enhanced qualities for improved speech recognition. For more information, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

## Previous-generation model types
{: #models-pg-types}

For most languages, the service makes available two types of previous-generation models:

-   *Narrowband models* are intended for audio that has a minimum sampling rate of 8 kHz. Use narrowband models for offline decoding of telephone speech, which is the typical use for this sampling rate.
-   *Broadband models* are for intended audio that has a minimum sampling rate of 16 kHz. Use broadband models for responsive, real-time applications, for example, for live-speech applications.

Choosing the correct model for your application is important. Use the model that matches the sampling rate (and language) of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. To achieve the best recognition accuracy, you also need to consider the frequency content of your audio. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-sampling-rate) and [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-frequency).

## Supported previous-generation language models
{: #model-pg-supported}

The following sections list the previous-generation models of each type that are available for each language. The tables in the sections provide the following information:

-   The *Model name* column indicates the name of the model.
-   The *Status* column indicates whether the model is generally available (*GA*) or *Beta*.
-   The *Recommended next-generation model* identifies the next-generation model that you can use instead of a deprecated model.

    Currently, not all broadband models have equivalent multimedia models. In such cases, consider using the telephony model for that language. The service downsamples the audio to the rate of the model that you use. So sending broadband audio to a telephony model might prove a sufficient alternative in cases where no equivalent multimedia model is currently available.
    {: note}

All models are available for both product versions, {{site.data.keyword.cloud_notm}} and {{site.data.keyword.icp4dfull_notm}}.

### Narrowband models
{: #models-pg-narrowband}

Table 1 lists the previous-generation narrowband models that are available.

| Language | Model name       | Status | Recommended next-generation model |
|----------|:----------------:|:------:|:---------------------------------:|
| Chinese (Mandarin) | `zh-CN_NarrowbandModel` | GA  \n Discontinued | `zh-CN_Telephony` |
| Dutch (Netherlands) | `nl-NL_NarrowbandModel` | GA  \n Discontinued | `nl-NL_Telephony` |
| English (Australian) | `en-AU_NarrowbandModel` | GA  \n Discontinued | `en-AU_Telephony` |
| English (United Kingdom) | `en-GB_NarrowbandModel` | GA  \n Discontinued | `en-GB_Telephony` |
| English (United States) | `en-US_NarrowbandModel` | GA  \n Discontinued | `en-US_Telephony` |
| | `en-US_ShortForm_NarrowbandModel` | GA  \n Discontinued | `en-US_Telephony` |
| French (Canadian) | `fr-CA_NarrowbandModel` | GA  \n Discontinued | `fr-CA_Telephony` |
| French (France) | `fr-FR_NarrowbandModel` | GA  \n Discontinued | `fr-FR_Telephony` |
| German | `de-DE_NarrowbandModel` | GA  \n Discontinued | `de-DE_Telephony` |
| Italian | `it-IT_NarrowbandModel` | GA  \n Discontinued | `it-IT_Telephony` |
| Japanese | `ja-JP_NarrowbandModel` | GA  \n Discontinued | `ja-JP_Telephony`  \n [IBM Cloud]{: tag-ibm-cloud} |
| Korean | `ko-KR_NarrowbandModel` | GA  \n Discontinued | `ko-KR_Telephony` |
| Portuguese (Brazilian) | `pt-BR_NarrowbandModel` | GA  \n Discontinued | `pt-BR_Telephony` |
| Spanish (Argentinian, Beta) | `es-AR_NarrowbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Castilian) | `es-ES_NarrowbandModel` | GA  \n Discontinued | `es-ES_Telephony` |
| Spanish (Chilean, Beta) | `es-CL_NarrowbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Colombian, Beta) | `es-CO_NarrowbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Mexican, Beta) | `es-MX_NarrowbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Peruvian, Beta) | `es-PE_NarrowbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
{: caption="Table 1. Supported previous-generation narrowband models"}

### Broadband models
{: #models-pg-broadband}

Table 2 lists the previous-generation broadband models that are available.

| Language | Model name       | Status | Recommended next-generation model |
|----------|:----------------:|:------:|:---------------------------------:|
| Arabic (Modern Standard) | `ar-MS_BroadbandModel` | GA  \n Discontinued | `ar-MS_Telephony` |
| Chinese (Mandarin) | `zh-CN_BroadbandModel` | GA  \n Discontinued | `zh-CN_Telephony` |
| Dutch (Netherlands) | `nl-NL_BroadbandModel` | GA  \n Discontinued | `nl-NL_Multimedia` |
| English (Australian) | `en-AU_BroadbandModel` | GA  \n Discontinued | `en-AU_Multimedia` |
| English (United Kingdom) | `en-GB_BroadbandModel` | GA  \n Discontinued | `en-GB_Multimedia` |
| English (United States) | `en-US_BroadbandModel` | GA  \n Discontinued | `en-US_Multimedia` |
| French (Canadian) | `fr-CA_BroadbandModel` | GA  \n Discontinued | `fr-CA_Multimedia` |
| French (France) | `fr-FR_BroadbandModel` | GA  \n Discontinued | `fr-FR_Multimedia` |
| German | `de-DE_BroadbandModel` | GA  \n Discontinued | `de-DE_Multimedia` |
| Italian | `it-IT_BroadbandModel` | GA  \n Discontinued | `it-IT_Multimedia` |
| Japanese | `ja-JP_BroadbandModel` | GA  \n Discontinued | `ja-JP_Multimedia` |
| Korean | `ko-KR_BroadbandModel` | GA  \n Discontinued | `ko-KR_Multimedia`|
| Portuguese (Brazilian) | `pt-BR_BroadbandModel` | GA  \n Discontinued | `pt-BR_Multimedia` |
| Spanish (Argentinian, Beta) | `es-AR_BroadbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Castilian) | `es-ES_BroadbandModel` | GA  \n Discontinued | `es-ES_Multimedia` |
| Spanish (Chilean, Beta) | `es-CL_BroadbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Colombian, Beta) | `es-CO_BroadbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Mexican, Beta) | `es-MX_BroadbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
| Spanish (Peruvian, Beta) | `es-PE_BroadbandModel` | Beta  \n Discontinued | `es-LA_Telephony` |
{: caption="Table 2. Supported previous-generation broadband models"}

## The US English short-form model (Deprecated)
{: #modelsShortform}

The US English short-form model, `en-US_ShortForm_NarrowbandModel`, can improve speech recognition for Interactive Voice Response (IVR) and Automated Customer Support solutions. The short-form model is trained to recognize the short utterances that are frequently expressed in customer support settings like automated support call centers. In addition to being tuned for short utterances in general, the model is also tuned for precise utterances such as digits, single-character word and name spellings, and yes-no responses.

The `en-US_ShortForm_NarrowbandModel` is optimal for the kinds of responses that are common to human-to-machine exchanges, such as the use case of {{site.data.keyword.iva_full}}. The `en-US_NarrowbandModel` is generally optimal for human-to-human conversations. However, depending on the use case and the nature of the exchange, some users might find the short-form model suitable for human-to-human conversations as well. Given this flexibility and overlap, you might experiment with both models to determine which works best for your application. In either case, applying a custom language model with a grammar to the short-form model can further improve recognition results.

As with all models, noisy environments can adversely impact the results. For example, background acoustic noise from airports, moving vehicles, conference rooms, and multiple speakers can reduce transcription accuracy. Audio from speaker phones can also reduce accuracy due to the echo common to such devices. Using the parameters available for speech activity detection can counteract such effects and help improve speech transcription accuracy. Applying a custom acoustic model can further fine-tune the acoustics for speech recognition, but only as a final measure.

-   For more information about language model and acoustic model customization, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
-   For more information about grammars, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).
-   For more information about speech activity detection parameters, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-detection).

## Supported features for previous-generation models
{: #models-features}

Previous-generation models are supported for use with almost all of the service's features. Most features and models are generally available for production use. Where indicated, some features and models are beta functionality. Restrictions apply for some features, for example:

-   Features such as speaker labels, numeric redaction, and profanity filtering are limited to certain languages and models. Such restrictions are noted with the descriptions of the individual features. For more information about all available speech recognition parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
-   The `low_latency` parameter is supported only for next-generation models. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).
-   For more information about previous-generation models' support for customization, see [Customization support for previous-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-pg).

Otherwise, when a feature is described as being available in general or available for a specific language or languages, it supports the previous-generation models.
