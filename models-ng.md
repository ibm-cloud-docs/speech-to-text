---

copyright:
  years: 2021, 2025
lastupdated: "2025-02-17"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Next-generation languages and models
{: #models-ng}

The {{site.data.keyword.speechtotextfull}} service supports a growing collection of next-generation models that improve upon the speech recognition capabilities of the service's previous-generation models. The model indicates the language in which the audio is spoken and the rate at which it is sampled. Next-generation models have higher throughput than the previous-generation models, so the service can return transcriptions more quickly.  Next-generation models also provide noticeably better transcription accuracy.
{: shortdesc}

When you use next-generation models, the service analyzes audio bidirectionally. Using deep neural networks, the model analyzes and extracts information from the audio. The model then evaluates the information forwards and backwards to predict the transcription, effectively "listening" to the audio twice.

With the additional information and context afforded by bidirectional analysis, the service can make smarter hypotheses about the words spoken in the audio. Despite the added analysis, recognition with next-generation models is more efficient than with previous-generation models, so the service delivers results faster and with more accuracy. Most next-generation models also offer a low-latency option to receive results even faster, though low latency might impact transcription accuracy.

In addition to providing greater transcription accuracy, the models have the ability to hypothesize words that are not in the base language model and that it has not encountered in training. This capability can decrease the need for customization of domain-specific terms. A model does not need to contain a specific vocabulary term to predict that word.

-   For an overview of the next-generation models and their technology, see [Next-Generation Watson Speech to Text](https://medium.com/ibm-watson-speech-services/next-generation-watson-speech-to-text-650fd66d95d0){: external}.
-   For more information about the technology that underlies the next-generation models, see [Advancing RNN Transducer Technology for Speech Recognition](https://arxiv.org/abs/2103.09935){: external}.
-   For information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).

## Next-generation model types
{: #models-ng-types}

The service makes available two types of next-generation models:

-   [Telephony models](#models-ng-telephony) are intended specifically for audio that is communicated over a telephone. Like previous-generation *narrowband* models, telephony models are intended for audio that has a minimum sampling rate of 8 kHz.
-   [Multimedia models](#models-ng-multimedia) are intended for audio that is extracted from sources with a higher sampling rate, such as video. Use a multimedia model for any audio other than telephonic audio. Like previous-generation *broadband* models, multimedia models are intended for audio that has a minimum sampling rate of 16 kHz.

Choose the model type that most closely matches the source and sampling rate of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. To achieve the best recognition accuracy, also consider the frequency content of your audio. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-sampling-rate) and [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-frequency).

## Supported next-generation language models
{: #models-ng-supported}

The following sections list the next-generation models of each type that are available for each language. The tables in the sections provide the following information:

-   The *Model name* column indicates the name of the model. (Unlike previous-generation models, next-generation models do not include the word `Model` in their names.)
-   The *Low-latency support* column indicates whether the model supports the `low_latency` parameter for speech recognition. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).
-   The *Status* column indicates whether the model is generally available (GA) or beta.

The *Model name* and *Low-latency support* columns indicate the product versions for which the model and low-latency are supported. Unless otherwise labeled as
[IBM Cloud]{: tag-ibm-cloud}, [IBM Cloud Pak for Data]{: tag-cp4d} or [IBM Software Hub]{: tag-teal}, a model and low latency are supported for all versions of the service.

### Telephony models
{: #models-ng-telephony}

Table 1 lists the available next-generation telephony models.

| Language | Model name | Low-latency support | Status |
|----------|:----------:|:-------------------:|:------:|
| Arabic  \n (Modern Standard) | `ar-MS_Telephony` | Yes | GA |
| Chinese  \n (Mandarin) | `zh-CN_Telephony` | Yes | GA |
| Czech | `cs-CZ_Telephony` | Yes | GA |
| Dutch  \n (Belgian) | `nl-BE_Telephony` | Yes | GA |
| Dutch  \n (Netherlands) | `nl-NL_Telephony` | Yes | GA |
| English  \n (Australian) | `en-AU_Telephony` | Yes | GA |
| English  \n (Indian) | `en-IN_Telephony` | Yes | GA |
| English  \n (United Kingdom) | `en-GB_Telephony` | Yes | GA |
| English  \n (United States) | `en-US_Telephony` | Yes | GA |
| English  \n (all supported dialects) | `en-WW_Medical_Telephony` | Yes | Beta |
| French  \n (Canadian) | `fr-CA_Telephony` | Yes | GA |
| French  \n (France) | `fr-FR_Telephony` | Yes | GA |
| German | `de-DE_Telephony` | Yes | GA |
| Hindi  \n (Indian) | `hi-IN_Telephony` | Yes | GA |
| Italian | `it-IT_Telephony` | Yes | GA |
| Japanese | `ja-JP_Telephony` | Yes | GA |
| Korean | `ko-KR_Telephony` | Yes | GA |
| Portuguese  \n (Brazilian) | `pt-BR_Telephony` | Yes | GA |
| Spanish  \n (Castilian) | `es-ES_Telephony` | Yes | GA |
| Spanish  \n (Argentinian, Chilean,  \n Colombian, Mexican,  \n and Peruvian) | `es-LA_Telephony` | Yes | GA |
| Swedish | `sv-SE_Telephony` | Yes | GA |
{: caption="Next-generation telephony models"}

The Latin American Spanish model, `es-LA_Telephony`, applies to all Latin American dialects. It is the equivalent of the previous-generation models that are available for the Argentinian, Chilean, Colombian, Mexican, and Peruvian dialects. If you used a previous-generation model for any of these Latin American dialects, use the `es-LA_Telephony` model to migrate to the equivalent next-generation model.
{: note}

### Multimedia models
{: #models-ng-multimedia}

Table 2 lists the available next-generation multimedia models.

| Language | Model name | Low-latency support | Status |
|----------|:----------:|:-------------------:|:------:|
| Dutch  \n (Netherlands) | `nl-NL_Multimedia` | Yes | GA |
| English  \n (Australian) | `en-AU_Multimedia` | Yes | GA |
| English  \n (United Kingdom) | `en-GB_Multimedia` | Yes | GA |
| English  \n (United States) | `en-US_Multimedia` | Yes | GA |
| French  \n (Canadian) | `fr-CA_Multimedia` | Yes | GA |
| French  \n (France) | `fr-FR_Multimedia` | Yes | GA |
| German | `de-DE_Multimedia` | Yes | GA |
| Italian | `it-IT_Multimedia` | Yes | GA |
| Japanese | `ja-JP_Multimedia` | Yes | GA |
| Korean | `ko-KR_Multimedia` | Yes | GA |
| Portuguese  \n (Brazilian) | `pt-BR_Multimedia` | Yes | GA |
| Spanish  \n (Castilian) | `es-ES_Multimedia` | Yes | GA |
{: caption="Next-generation multimedia models"}

### The English medical telephony model
{: #models-medical}

The beta next-generation `en-WW_Medical_Telephony` understands terms from the medical and pharmacological domains. Use the model in situations where you need to transcribe common medical terminology such as medicine names, product brands, medical procedures, illnesses, types of doctor, or COVID-19-related terminology.

Common use cases include conversations between a patient and a medical provider (for example, a doctor, nurse, or pharmacist):
-   "My head is hurting. I need an Ibuprofen, please."
-   "Can you suggest an orthopedist who specializes in osteoarthritis?"
-   "Can you please help me find an internist in Chicago?"

The new model is available for all supported English dialects: Australian, Indian, UK, and US. The new model supports language model customization and grammars as beta functionality. It supports most of the same parameters as the `en-US_Telephony` model, including `smart_formatting` for US English audio. In addition to those features listed in [Supported features for next-generation models](#models-ng-features), the model does *not* support the following parameters: `profanity_filter`, `redaction`, and `speaker_labels`.

## Supported features for next-generation models
{: #models-ng-features}

The next-generation models are supported for use with a large subset of the service's speech recognition features. In cases where a supported feature is restricted to certain languages, the same language restrictions usually apply to both previous- and next-generation models.

-   For more information about the parameters that you can use with next-generation models, including their language support and whether the parameters are GA or beta, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
-   For more information about next-generation models' support for customization, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).

Next-generation models support all speech recognition parameters and headers *except* for the following:

-   `acoustic_customization_id` (Next-generation models do not support acoustic model customization.)
-   `keywords` and `keywords_threshold`
-   `processing_metrics` and `processing_metrics_interval`
-   `word_alternatives_threshold`

Next-generation models also support the following parameters, which are not available with previous-generation models:

-   `character_insertion_bias`, which is supported by all next-generation models. For more information, see [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias).
-   `low_latency`, which is supported by most next-generation models. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

Next-generation models also differ from previous-generation models with respect to the following additional features:

-   Next-generation models do not produce hesitation markers. They instead include the actual hesitations in transcription results. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).
-   Next-generation models support automatic capitalization only for German models. Previous-generation models support automatic customization only for US English models. For more information, see [Capitalization](/docs/speech-to-text?topic=speech-to-text-basic-response#response-capitalization).
