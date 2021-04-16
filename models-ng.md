---

copyright:
  years: 2021
lastupdated: "2021-04-16"

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

The {{site.data.keyword.speechtotextfull}} service supports a growing collection of next-generation models that improve upon the speech recognition capabilities of the service's previous generation of models. Next-generation models have higher throughput than the previous models, so the service can return transcriptions more quickly. Next-generation models also provide noticeably better transcription accuracy.
{: shortdesc}

When you use next-generation models, the service analyzes audio bidirectionally. Using deep neural networks, the model analyzes and extracts information from the audio. The model then evaluates the information forwards and backwards to predict the transcription, effectively "listening" to the audio twice.

With the additional information and context afforded by bidirectional analysis, the service can make smarter hypotheses about the words spoken in the audio. Despite the added analysis, recognition with next-generation models is more efficient than with previous-generation models, so the service delivers results faster and with more accuracy. Some next-generation models also offer a low-latency option to receive results even faster, though low latency might impact transcription accuracy.

In addition to providing greater transcription accuracy, the models have the ability to hypothesize words that are not in the base language model and that it has not encountered in training. This capability can decrease the need for customization of domain-specific terms. A model does not need to contain a specific vocabulary term to predict that word.

The next-generation language models are beta functionality. They support a limited number of languages and features at this time. The supported languages, models, and features will increase with future releases.
{: beta}

## Supported language models
{: #models-ng-supported}

The service makes available two types of next-generation models:

-   *Multimedia* models are intended for audio that is extracted from sources with a higher sampling rate, such as video. Use a multimedia model for any audio other than telephonic audio. Like previous-generation *broadband* models, this audio has a minimum sampling rate of 16 kHz.
-   *Telephony* models are intended specifically for audio that is communicated over a telephone. Like previous-generation *narrowband* models, this audio has a minimum sampling rate of 8 kHz.

Choose the model that most closely matches the source and sampling rate of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. To achieve the best recognition accuracy, also consider the frequency content of your audio. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-sampling-rate) and [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-frequency).

Table 1 lists the supported next-generation languages and models. Low-latency columns indicate whether each model supports the `low_latency` parameter for speech recognition. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

| <br/>Language | <br/>Multimedia model | Multimedia<br/>low-latency support | <br/>Telephony model | Telephony<br/>low-latency support |
|----------|:----------------:|:-------------------:|:---------------:|:-------------------:|
| English (Australian) | Not available | Not available | `en-AU_Telephony` | **Yes** |
| English (United Kingdom) | Not available | Not available | `en-GB_Telephony` | **Yes** |
| English (United States) | `en-US_Multimedia` | No | `en-US_Telephony` | **Yes** |
| French | Not available | Not available | `fr-FR_Telephony` | **Yes** |
| French (Canadian) | Not available | Not available | `fr-CA_Telephony` | No |
| German | Not available | Not available | `de-DE_Telephony` | **Yes** |
| Italian | Not available | Not available | `it-IT_Telephony` | No |
| Spanish (Castilian) | Not available | Not available | `es-ES_Telephony` | No |
{: caption="Table 1. Supported next-generation language models"}

Unlike previous-generation models, next-generation models do not include the word `model` in their names.
{: note}

## Specifying a model for speech recognition
{: #models-ng-example}

You use a next-generation model in a speech recognition request just as you do a previous-generation model, by using the `model` parameter to indicate the model that is to be used. The following example HTTP request uses the `en-US_Telephony` model for speech recognition:

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}

If you omit the `model` parameter from a speech recognition request, the service uses the previous-generation US English broadband model, `en-US_BroadbandModel`, by default.

## Supported features
{: #models-ng-features}

The next-generation models described in this topic are supported for use with a subset of the service's features. In cases where a supported feature is restricted to certain languages, those same language restrictions apply to the next-generation models. Also, when you use a next-generation model for speech recognition, final transcription results do not include the `confidence` field. When you use a previous-generation model, final transcription results always include the `confidence` field.

Table 1 lists each parameter (and request header) that is supported for use with the next-generation models. It provides a brief description of the parameter and cites any language or model restrictions that apply for the next-generation models. Features not listed in the table are not supported for use with the next-generation models. Next-generation models do not support language model customization, acoustic model customization, or grammars.

| <br/>Parameter | <br/>Description | Next-generation language<br/>and model support |
|-----------|-------------|--------------------------------------------|
| `access_token` | A required Identity and Access Management (IAM) access token that you use to establish an authenticated connection with the WebSocket interface. For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#WSopen). | All languages and models. |
| `background_audio_suppression` | An optional float between 0.0 and 1.0 that indicates the level to which background audio and side conversations are to be suppressed in the input audio. For more information, see [Background audio suppression](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-suppression). | All languages and models. |
| `Content-Type` | An optional audio format (MIME type) that specifies the format of the audio data that you pass to the service. For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list). | All languages and models. |
| `inactivity_timeout` | An optional integer that specifies the number of seconds for the service's inactivity timeout. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity). | All languages and models. |
| `interim_results` | An optional boolean that directs the service to return intermediate hypotheses that are likely to change before the final transcript. Available only with the WebSocket interface. | All languages and models that support low latency, but only when the `low_latency` parameter is `true`. For more information, see [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency). |
| `low_latency` | An optional boolean that indicates whether the service is to produce results more quickly at the possible expense of transcription accuracy. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency). | Next-generation models that support low latency only. |
| `model` | An optional model that specifies the language in which the audio is spoken and the rate at which it was sampled: broadband/multimedia or narrowband/telephony. | All languages and models. |
| `profanity_filter` | An optional boolean that indicates whether the service censors profanity from a transcript. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-formatting#profanity-filtering). | US English models only. |
| `redaction` | An optional boolean that indicates whether the service redacts numeric data with three or more consecutive digits from a transcript. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction). | US English models only. |
| `smart_formatting` | An optional boolean that indicates whether the service converts dates, times, numbers, currency, and similar values into more conventional representations in the final transcript. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting). | US English and Spanish models only. |
| `speech_detector_sensitivity` | An optional float between 0.0 and 1.0 that indicates the sensitivity of speech recognition to non-speech events in the input audio. For more information, see [Speech detector sensitivity](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-sensitivity). | All languages and models. |
| `timestamps` | An optional boolean that indicates whether the service produces timestamps for the words of the transcript. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps). | All languages and models. |
| `Transfer-Encoding` | An optional value of `chunked` that causes the audio to be streamed to the service. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission). | All languages and models. |
| `X-Watson-Learning-Opt-Out` | An optional boolean that indicates whether you opt out of the default request logging that {{site.data.keyword.IBM_notm}} performs to improve the service for future users. For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging). | All languages and models. |
| `X-Watson-Metadata` | An optional string that associates a customer ID with data that is passed for recognition requests. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security). | All languages and models. |
{: caption="Table 2. Parameter support for next-generation languages and models"}

For more information about all available speech recognition parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
