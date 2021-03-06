---

copyright:
  years: 2015, 2021
lastupdated: "2020-06-09"

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

# Languages and models
{: #models}

The {{site.data.keyword.speechtotextfull}} service supports speech recognition in many languages.  For all interfaces, you can use the `model` parameter to specify the model for a speech recognition request. The model indicates the language in which the audio is spoken and the rate at which it is sampled.
{: shortdesc}

The service also offers next-generation models with enhanced qualities for improved speech recognition. For more information, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
{: note}

## Supported language models
{: #models-supported}

For most languages, the service supports both broadband and narrowband models:

-   *Broadband models* are for audio that is sampled at greater than or equal to 16 kHz. Use broadband models for responsive, real-time applications, for example, for live-speech applications.
-   *Narrowband models* are for audio that is sampled at 8 kHz. Use narrowband models for offline decoding of telephone speech, which is the typical use for this sampling rate.

Choosing the correct model for your application is important. Use the model that matches the sampling rate (and language) of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. To achieve the best recognition accuracy, you also need to consider the frequency content of your audio. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-sampling-rate) and [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-frequency).

Table 1 lists the supported models for each language. If you omit the `model` parameter from a request, the service uses the US English broadband model, `en-US_BroadbandModel`, by default.

Languages labeled *Beta* are currently beta functionality. Beta languages might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage. All other languages are generally available (*GA*) for production use.

The model name `ar-AR_BroadbandModel` is deprecated. Use the model name `ar-MS_BroadbandModel` instead. For more information, see the [2 December 2020 service update](/docs/speech-to-text?topic=speech-to-text-release-notes#December2020) in the release notes.
{: note}

| Language | Broadband model | Narrowband model |
|----------|:---------------:|:----------------:|
| Arabic (Modern Standard) | `ar-MS_BroadbandModel` | Not supported |
| Chinese (Mandarin) | `zh-CN_BroadbandModel` | `zh-CN_NarrowbandModel` |
| Dutch (Netherlands) | `nl-NL_BroadbandModel` | `nl-NL_NarrowbandModel` |
| English (Australian) | `en-AU_BroadbandModel` | `en-AU_NarrowbandModel` |
| English (United Kingdom) | `en-GB_BroadbandModel` | `en-GB_NarrowbandModel` |
| English (United States) | `en-US_BroadbandModel` | `en-US_NarrowbandModel`<br/><br/>`en-US_ShortForm_NarrowbandModel` |
| French (Canadian) | `fr-CA_BroadbandModel` | `fr-CA_NarrowbandModel` |
| French (France) | `fr-FR_BroadbandModel` | `fr-FR_NarrowbandModel` |
| German | `de-DE_BroadbandModel` | `de-DE_NarrowbandModel` |
| Italian | `it-IT_BroadbandModel` | `it-IT_NarrowbandModel` |
| Japanese | `ja-JP_BroadbandModel` | `ja-JP_NarrowbandModel` |
| Korean | `ko-KR_BroadbandModel` | `ko-KR_NarrowbandModel` |
| Portuguese (Brazilian) | `pt-BR_BroadbandModel` | `pt-BR_NarrowbandModel` |
| Spanish (Argentinian, Beta) | `es-AR_BroadbandModel` | `es-AR_NarrowbandModel` |
| Spanish (Castilian) | `es-ES_BroadbandModel` | `es-ES_NarrowbandModel` |
| Spanish (Chilean, Beta) | `es-CL_BroadbandModel` | `es-CL_NarrowbandModel` |
| Spanish (Colombian, Beta) | `es-CO_BroadbandModel` | `es-CO_NarrowbandModel` |
| Spanish (Mexican, Beta) | `es-MX_BroadbandModel` | `es-MX_NarrowbandModel` |
| Spanish (Peruvian, Beta) | `es-PE_BroadbandModel` | `es-PE_NarrowbandModel` |
{: caption="Table 1. Supported language models"}

## The US English short-form model
{: #modelsShortform}

The US English short-form model, `en-US_ShortForm_NarrowbandModel`, can improve speech recognition for Interactive Voice Response (IVR) and Automated Customer Support solutions. The short-form model is trained to recognize the short utterances that are frequently expressed in customer support settings like automated support call centers. In addition to being tuned for short utterances in general, the model is also tuned for precise utterances such as digits, single-character word and name spellings, and yes-no responses.

The `en-US_ShortForm_NarrowbandModel` is optimal for the kinds of responses that are common to human-to-machine exchanges, such as the use case of {{site.data.keyword.iva_full}}. The `en-US_NarrowbandModel` is generally optimal for human-to-human conversations. However, depending on the use case and the nature of the exchange, some users might find the short-form model suitable for human-to-human conversations as well. Given this flexibility and overlap, you might experiment with both models to determine which works best for your application. In either case, applying a custom language model with a grammar to the short-form model can further improve recognition results.

As with all models, noisy environments can adversely impact the results. For example, background acoustic noise from airports, moving vehicles, conference rooms, and multiple speakers can reduce transcription accuracy. Audio from speaker phones can also reduce accuracy due to the echo common to such devices. Using the parameters available for speech activity detection can counteract such effects and help improve speech transcription accuracy. Applying a custom acoustic model can further fine-tune the acoustics for speech recognition, but only as a final measure.

-   For more information about language model and acoustic model customization, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
-   For more information about grammars, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).
-   For more information about speech activity detection parameters, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-detection).

## Specifying a model for speech recognition
{: #modelsExample}

Use the `model` parameter of a speech recognition to indicate the model that is to be used. The following example HTTP request uses the model `en-US_NarrowbandModel` for speech recognition:

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

If you omit the `model` parameter from a speech recognition request, the service uses the US English broadband model, `en-US_BroadbandModel`, by default.

## Supported features
{: #models-features}

The previous-generation models described in this topic are supported for use with all of the service's features. Most features and models are generally available for production use. Where indicated, some features and models are beta functionality. Restrictions apply for some features, for example:

-   Features such as speaker labels, numeric redaction, and profanity filtering are limited to certain languages and models. Such restrictions are noted with the descriptions of the individual features. For more information about all parameters that also notes all restrictions, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
-   The `low_latency` parameter is supported only for next-generation models. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).
-   Language model customization is not supported for the Arabic and Chinese models. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).

Otherwise, when a feature is described as being available in general or available for a specific language or languages, it supports the previous-generation models.
