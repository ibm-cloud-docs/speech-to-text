---

copyright:
  years: 2021
lastupdated: "2021-09-14"

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

The table indicates the product versions for which each model and for which low-latency are supported. Unless otherwise indicated, the model and low latency are supported for both {{site.data.keyword.cloud_notm}} and {{site.data.keyword.icp4dfull_notm}}. The Czech and Netherlands Dutch models are *Beta* functionality. All other models are generally available (GA).

| <br/>Language | <br/>Multimedia model | Multimedia<br/>low-latency support | <br/>Telephony model | Telephony<br/>low-latency support |
|----------|:----------------:|:-------------------:|:---------------:|:-------------------:|
| Arabic<br/>(Modern Standard) | Not available | Not available | `ar-MS_Telephony` | **Yes**<br/>IBM Cloud only |
| Czech | Not available | Not available | `cz-CZ_Telephony`<br/>IBM Cloud only (*Beta*) | **Yes**<br/>IBM Cloud only |
| Dutch<br/>(Belgian) | Not available | Not available | `nl-BE_Telephony`<br/>IBM Cloud only | **Yes**<br/>IBM Cloud only |
| Dutch<br/>(Netherlands) | Not available | Not available | `nl-NL_Telephony`<br/>IBM Cloud only (*Beta*) | **No** |
| English<br/>(Australian) | Not available | Not available | `en-AU_Telephony` | **Yes** |
| English<br/>(Indian) | Not available | Not available | `en-IN_Telephony`<br/>IBM Cloud only | **Yes**<br/>IBM Cloud only |
| English<br/>(United Kingdom) | Not available | Not available | `en-GB_Telephony` | **Yes** |
| English<br/>(United States) | `en-US_Multimedia` | **No** | `en-US_Telephony` | **Yes** |
| French<br/>(Canadian) | Not available | Not available | `fr-CA_Telephony` | **Yes**<br/>IBM Cloud only |
| French<br/>(France) | `fr-FR_Multimedia`<br/>IBM Cloud only | **No** | `fr-FR_Telephony` | **Yes** |
| German | Not available | Not available | `de-DE_Telephony` | **Yes** |
| Hindi<br/>(Indian) | Not available | Not available | `hi-IN_Telephony`<br/>IBM Cloud only | **Yes**<br/>IBM Cloud only |
| Italian | Not available | Not available | `it-IT_Telephony` | **Yes**<br/>IBM Cloud only |
| Japanese | `ja-JP_Multimedia`<br/>IBM Cloud only | **No** | Not available | Not available |
| Korean | `ko-KR_Multimedia`<br/>IBM Cloud only | **No** | `ko-KR_Telephony`<br/>IBM Cloud only | **Yes**<br/>IBM Cloud only |
| Portuguese<br/>(Brazilian) | Not available | Not available | `pt-BR_Telephony` | **Yes** |
| Spanish<br/>(Castilian) | Not available | Not available | `es-ES_Telephony` | **Yes** |
{: caption="Table 1. Supported next-generation language models"}

Unlike previous-generation models, next-generation models do not include the word `model` in their names.
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
-   Next-generation models do not support acoustic model customization or grammars.
-   Next-generation models do not produce hesitation markers. Hesitation markers are produced only by previous-generation models. For more information about hesitation markers, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

For more information about all available speech recognition parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
