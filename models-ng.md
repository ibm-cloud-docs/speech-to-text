---

copyright:
  years: 2021, 2022
lastupdated: "2022-02-11"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Next-generation languages and models
{: #models-ng}

The {{site.data.keyword.speechtotextfull}} service supports a growing collection of next-generation models that improve upon the speech recognition capabilities of the service's previous-generation models. Next-generation models have higher throughput than the previous models, so the service can return transcriptions more quickly. Next-generation models also provide noticeably better transcription accuracy.
{: shortdesc}

When you use next-generation models, the service analyzes audio bidirectionally. Using deep neural networks, the model analyzes and extracts information from the audio. The model then evaluates the information forwards and backwards to predict the transcription, effectively "listening" to the audio twice.

With the additional information and context afforded by bidirectional analysis, the service can make smarter hypotheses about the words spoken in the audio. Despite the added analysis, recognition with next-generation models is more efficient than with previous-generation models, so the service delivers results faster and with more accuracy. Some next-generation models also offer a low-latency option to receive results even faster, though low latency might impact transcription accuracy.

In addition to providing greater transcription accuracy, the models have the ability to hypothesize words that are not in the base language model and that it has not encountered in training. This capability can decrease the need for customization of domain-specific terms. A model does not need to contain a specific vocabulary term to predict that word.

-   For more information about the technology that underlies the next-generation models, see [Advancing RNN Transducer Technology for Speech Recognition](https://arxiv.org/abs/2103.09935){: external}.
-   For information about migrating from previous-generation to next-generation models, see [Watson Speech to Text: How to Plan Your Migration to the Next-Generation Models](https://medium.com/ibm-data-ai/watson-speech-to-text-how-to-plan-your-migration-to-the-next-generation-models-6b10605b3bc5){: external}.

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

The *Model name* and *Low-latency support* columns indicate the product versions for which the model and low-latency are supported. Unless otherwise labeled as *{{site.data.keyword.cloud_notm}} only* or *{{site.data.keyword.icp4dfull_notm}} only*, a model and low latency are supported for both versions of the service.

### Telephony models
{: #models-ng-telephony}

Table 1 lists the available next-generation telephony models.

| Language | Model name | Low-latency support | Status |
|----------|:----------:|:-------------------:|:------:|
| Arabic  \n (Modern Standard) | `ar-MS_Telephony` | Yes | GA |
| Chinese  \n (Mandarin) | `zh-CN_Telephony` | Yes | GA |
| Czech | `cz-CZ_Telephony` | Yes | GA |
| Dutch  \n (Belgian) | `nl-BE_Telephony` | Yes | GA |
| Dutch  \n (Netherlands) | `nl-NL_Telephony` | Yes | GA |
| English  \n (Australian) | `en-AU_Telephony` | Yes | GA |
| English  \n (Indian) | `en-IN_Telephony` | Yes | GA |
| English  \n (United Kingdom) | `en-GB_Telephony` | Yes | GA |
| English  \n (United States) | `en-US_Telephony` | Yes | GA |
| English  \n (all supported dialects) | `en-WW_Medical_Telephony`  \n {{site.data.keyword.cloud_notm}} only | No | Beta |
| French  \n (Canadian) | `fr-CA_Telephony` | Yes | GA |
| French  \n (France) | `fr-FR_Telephony` | Yes | GA |
| German | `de-DE_Telephony` | Yes | GA |
| Hindi  \n (Indian) | `hi-IN_Telephony` | Yes | GA |
| Italian | `it-IT_Telephony` | Yes | GA |
| Korean | `ko-KR_Telephony` | Yes | GA |
| Portuguese  \n (Brazilian) | `pt-BR_Telephony` | Yes | GA |
| Spanish  \n (Castilian) | `es-ES_Telephony` | Yes | GA |
| Spanish  \n (Argentinian, Chilean,  \n Colombian, Mexican,  \n and Peruvian) | `es-LA_Telephony` | Yes | GA |
{: caption="Table 1. Next-generation telephony models"}

The Latin American Spanish model, `es-LA_Telephony`, applies to all Latin American dialects. It is the equivalent of the previous-generation models that are available for the Argentinian, Chilean, Colombian, Mexican, and Peruvian dialects. If you used a previous-generation model for any of these Latin American dialects, use the `es-LA_Telephony` model to migrate to the equivalent next-generation model.
{: note}

### Multimedia models
{: #models-ng-multimedia}

Table 2 lists the available next-generation multimedia models.

| Language | Model name | Low-latency support | Status |
|----------|:----------:|:-------------------:|:------:|
| English  \n (Australian) | `en-AU_Multimedia` | No | GA |
| English  \n (United Kingdom) | `en-GB_Multimedia` | No | GA |
| English  \n (United States) | `en-US_Multimedia` | No | GA |
| French  \n (France) | `fr-FR_Multimedia` | No | GA |
| Japanese | `ja-JP_Multimedia` | Yes  \n {{site.data.keyword.cloud_notm}} only | GA |
| Korean | `ko-KR_Multimedia` | **No** | GA |
{: caption="Table 2. Next-generation multimedia models"}

### The English medical telephony model
{: #models-medical}

The beta next-generation `en-WW_Medical_Telephony` understands terms from the medical and pharmacological domains. Use the model in situations where you need to transcribe common medical terminology such as medicine names, product brands, medical procedures, illnesses, types of doctor, or COVID-19-related terminology.

Common use cases include conversations between a patient and a medical provider (for example, a doctor, nurse, or pharmacist):
-   "My head is hurting. I need an Ibuprofen, please."
-   "Can you suggest an orthopedist who specializes in osteoarthritis?"
-   "Can you please help me find an internist in Chicago?"

The new model is available for all supported English dialects: Australian, Indian, UK, and US. The new model supports language model customization and grammars as beta functionality. It supports most of the same parameters as the `en-US_Telephony` model, including `smart_formatting`. It does *not* support the following parameters: `low_latency`, `profanity_filter`, `redaction`, and `speaker_labels`.

## Specifying a model for speech recognition
{: #models-ng-specify}

You use a next-generation model in a speech recognition request just as you do a previous-generation model, by using the `model` parameter to indicate the model that is to be used. If you omit the `model` parameter from a speech recognition request, the service uses the previous-generation US English broadband model, `en-US_BroadbandModel`, by default.

### Specify a model example
{: #models-ng-specify-example}

The following example HTTP request uses the `en-US_Telephony` model for speech recognition:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}

## Supported features for next-generation models
{: #models-ng-features}

The next-generation models are supported for use with a subset of the service's features. In cases where a supported feature is restricted to certain languages, those same language restrictions apply to the next-generation models.

Table 1 lists each parameter (and request header) that is supported for use with the next-generation models. For more information about all available speech recognition parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

| Parameter | Next-generation language and model support |
|-----------|--------------------------------------------|
| `access_token` | All languages and models. For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#ws-open). |
| `audio_metrics` | All languages and models. For more information, see [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio-metrics). |
| `background_audio_suppression` | All languages and models. For more information, see [Background audio suppression](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-suppression). |
| `Content-Type` | All languages and models. For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list). |
| `customization_weight` | All languages and models. For more information, see [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight). |
| `grammar_name` | All languages and models. For more information, see [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse). |
| `inactivity_timeout` | All languages and models. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity). |
| `interim_results` | All languages and models that support low latency, but only if both the `interim_results` and `low_latency` parameters are set to `true`. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results). |
| `language_customization_id` | All languages and models. For more information, see [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse). |
| `low_latency` | Next-generation models that support low latency only. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency). |
| `model` | All languages and models. For more information, see [Specifying a model for speech recognition](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-example). |
| `profanity_filter` | US English and Japanese models only. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-formatting#profanity-filtering). |
| `redaction` | US English, Japanese, and Korean models only. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction). |
| `smart_formatting` | US English, Japanese, and Spanish (all dialects) models only. This support includes the `en-WW_Medical_Telephony` model when US English audio is recognized. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting). |
| `speaker_labels` | Czech, English (Australian, Indian, UK, and US), German, Japanese, Korean, and Spanish models only. Not supported for use with the `interim_results` or `low_latency` parameters. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels). |
| `speech_detector_sensitivity` | All languages and models. For more information, see [Speech detector sensitivity](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-sensitivity). |
| `timestamps` | All languages and models. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps). |
| `Transfer-Encoding` | All languages and models. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission). |
| `word_confidence` | All languages and models. For more information, see [Word confidence](/docs/speech-to-text?topic=speech-to-text-metadata#word-confidence). |
| `X-Watson-Learning-Opt-Out` | All languages and models. For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging). |
| `X-Watson-Metadata` | All languages and models. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security). |
{: caption="Table 2. Parameter support for next-generation languages and models"}

## Unsupported features for next-generation models
{: #models-ng-unsupported}

Previous-generation models support some features that are not available with next-generation models. Specifically, next-generation models do not support the following features:

-   Any feature not listed in the previous section is not supported for use with the next-generation models.
-   Next-generation models do not support acoustic model customization. For more information about support for customization, see [Language support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).
-   Next-generation models do not produce hesitation markers. Hesitation markers are produced only by previous-generation models. For more information about hesitation markers, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).
