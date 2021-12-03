---

copyright:
  years: 2021
lastupdated: "2021-12-03"

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

## Supported next-generation language models
{: #models-ng-supported}

The service makes available two types of next-generation models:

-   *Multimedia* models are intended for audio that is extracted from sources with a higher sampling rate, such as video. Use a multimedia model for any audio other than telephonic audio. Like previous-generation *broadband* models, this audio has a minimum sampling rate of 16 kHz.
-   *Telephony* models are intended specifically for audio that is communicated over a telephone. Like previous-generation *narrowband* models, this audio has a minimum sampling rate of 8 kHz.

Choose the model that most closely matches the source and sampling rate of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. To achieve the best recognition accuracy, also consider the frequency content of your audio. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-sampling-rate) and [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-frequency).

Table 1 lists the next-generation models that are available for each language. Low-latency columns indicate whether each model supports the `low_latency` parameter for speech recognition. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

The table indicates the product versions for which each model and for which low-latency are supported. Unless otherwise labeled as *{{site.data.keyword.cloud_notm}} only* or *{{site.data.keyword.icp4dfull_notm}} only*, the model and low latency are supported for both versions of the service. All models are generally available (GA). (Unlike previous-generation models, next-generation models do not include the word `Model` in their names.)

| Language | Multimedia model | Multimedia low-latency support | Telephony model | Telephony low-latency support |
|----------|:----------------:|:-------------------:|:---------------:|:-------------------:|
| Arabic  \n (Modern Standard) | Model not available | Model not available | `ar-MS_Telephony` | **Yes** |
| Chinese  \n (Mandarin) | Model not available | Model not available | `zh-CN_Telephony`  \n IBM Cloud only | **Yes**  \n IBM Cloud only |
| Czech | Model not available | Model not available | `cz-CZ_Telephony` | **Yes**  |
| Dutch  \n (Belgian) | Model not available | Model not available | `nl-BE_Telephony` | **Yes** |
| Dutch  \n (Netherlands) | Model not available | Model not available | `nl-NL_Telephony` | **Yes** |
| English  \n (Australian) | `en-AU_Multimedia`  \n IBM Cloud only | **No** | `en-AU_Telephony` | **Yes** |
| English  \n (Indian) | Model not available | Model not available | `en-IN_Telephony` | **Yes** |
| English  \n (United Kingdom) | `en-GB_Multimedia`  \n IBM Cloud only | **No** | `en-GB_Telephony` | **Yes** |
| English  \n (United States) | `en-US_Multimedia` | **No** | `en-US_Telephony` | **Yes** |
| French  \n (Canadian) | Model not available | Model not available | `fr-CA_Telephony` | **Yes** |
| French  \n (France) | `fr-FR_Multimedia` | **No** | `fr-FR_Telephony` | **Yes** |
| German | Model not available | Model not available | `de-DE_Telephony` | **Yes** |
| Hindi  \n (Indian) | Model not available | Model not available | `hi-IN_Telephony` | **Yes** |
| Italian | Model not available | Model not available | `it-IT_Telephony` | **Yes** |
| Japanese | `ja-JP_Multimedia` | **No** | Model not available | Model not available |
| Korean | `ko-KR_Multimedia` | **No** | `ko-KR_Telephony` | **Yes** |
| Portuguese  \n (Brazilian) | Model not available | Model not available | `pt-BR_Telephony` | **Yes** |
| Spanish  \n (Castilian) | Model not available | Model not available | `es-ES_Telephony` | **Yes** |
| Spanish  \n (Latin American) | Model not available | Model not available | `es-LA_Telephony`  \n {{site.data.keyword.cloud_notm}} only | **Yes**  \n {{site.data.keyword.cloud_notm}} only |
{: caption="Table 1. Supported next-generation language models"}

The Latin American Spanish model, `es-LA_Telephony`, applies to all Latin American dialects. It is the equivalent of the previous-generation models that are available for the Argentinian, Chilean, Colombian, Mexican, and Peruvian dialects. If you used a previous-generation model for any of these specific dialects, use the `es-LA_Telephony` model to migrate to the equivalent next-generation model.
{: note}

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
| `grammar_name` | ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** All languages and models. For more information, see [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse). |
| `inactivity_timeout` | All languages and models. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity). |
| `interim_results` | All languages and models that support low latency, but only if both the `interim_results` and `low_latency` parameters are set to `true`. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results). |
| `language_customization_id` | All languages and models. For more information, see [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse). |
| `low_latency` | Next-generation models that support low latency only. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency). |
| `model` | All languages and models. For more information, see [Specifying a model for speech recognition](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-example). |
| `profanity_filter` | US English and Japanese models only. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-formatting#profanity-filtering). |
| `redaction` | US English, Japanese, and Korean models only. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction). |
| `smart_formatting` | US English, Japanese, and Spanish models only. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting). |
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
