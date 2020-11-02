---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-27"

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

The {{site.data.keyword.speechtotextfull}} service supports speech recognition in many languages. For all interfaces, you can use the `model` parameter to specify the model for a speech recognition request. The model indicates the language in which the audio is spoken and the rate at which it is sampled.
{: shortdesc}

## Supported language models
{: #modelsList}

For most languages, the service supports both broadband and narrowband models:

-   *Broadband models* are for audio that is sampled at greater than or equal to 16 kHz. Use broadband models for responsive, real-time applications, for example, for live-speech applications.
-   *Narrowband models* are for audio that is sampled at 8 kHz. Use narrowband models for offline decoding of telephone speech, which is the typical use for this sampling rate.

Choosing the correct model for your application is important. Use the model that matches the sampling rate (and language) of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-formats#samplingRate).

To achieve the best recognition accuracy, you also need to consider the frequency content of your audio. For more information, see [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-formats#frequency).

Table 1 lists the supported models for each language. If you omit the `model` parameter from a request, the service uses the US English broadband model, `en-US_BroadbandModel`, by default.

Languages labeled *Beta* are currently beta functionality. Beta languages might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage. All other languages are generally available (*GA*) for production use.
{: beta}

| Language | Broadband model | Narrowband model |
|----------|:---------------:|:----------------:|
| Arabic (Modern Standard) | `ar-AR_BroadbandModel` | Not supported |
| Brazilian Portuguese | `pt-BR_BroadbandModel` | `pt-BR_NarrowbandModel` |
| Chinese (Mandarin) | `zh-CN_BroadbandModel` | `zh-CN_NarrowbandModel` |
| Dutch | `nl-NL_BroadbandModel` | `nl-NL_NarrowbandModel` |
| English (Australian) | `en-AU_BroadbandModel` | `en-AU_NarrowbandModel` |
| English (United Kingdom) | `en-GB_BroadbandModel` | `en-GB_NarrowbandModel` |
| English (United States) | `en-US_BroadbandModel` | `en-US_NarrowbandModel`<br/><br/>`en-US_ShortForm_NarrowbandModel` |
| French| `fr-FR_BroadbandModel` | `fr-FR_NarrowbandModel` |
| French (Canadian) | `fr-CA_BroadbandModel` | `fr-CA_NarrowbandModel` |
| German | `de-DE_BroadbandModel` | `de-DE_NarrowbandModel` |
| Italian | `it-IT_BroadbandModel` | `it-IT_NarrowbandModel` |
| Japanese | `ja-JP_BroadbandModel` | `ja-JP_NarrowbandModel` |
| Korean | `ko-KR_BroadbandModel` | `ko-KR_NarrowbandModel` |
| Spanish (Argentinian, Beta) | `es-AR_BroadbandModel` | `es-AR_NarrowbandModel` |
| Spanish (Castilian) | `es-ES_BroadbandModel` | `es-ES_NarrowbandModel` |
| Spanish (Chilean, Beta) | `es-CL_BroadbandModel` | `es-CL_NarrowbandModel` |
| Spanish (Colombian, Beta) | `es-CO_BroadbandModel` | `es-CO_NarrowbandModel` |
| Spanish (Mexican, Beta) | `es-MX_BroadbandModel` | `es-MX_NarrowbandModel` |
| Spanish (Peruvian, Beta) | `es-PE_BroadbandModel` | `es-PE_NarrowbandModel` |
{: caption="Table 1. Supported language models"}

### The US English short-form model
{: #modelsShortform}

The US English short-form model, `en-US_ShortForm_NarrowbandModel`, can improve speech recognition for Interactive Voice Response (IVR) and Automated Customer Support solutions. The short-form model is trained to recognize the short utterances that are frequently expressed in customer support settings like automated support call centers. In addition to being tuned for short utterances in general, the model is also tuned for precise utterances such as digits, single-character word and name spellings, and yes-no responses.

The `en-US_ShortForm_NarrowbandModel` is optimal for the kinds of responses that are common to human-to-machine exchanges, such as the use case of {{site.data.keyword.iva_full}}. The `en-US_NarrowbandModel` is generally optimal for human-to-human conversations. However, depending on the use case and the nature of the exchange, some users might find the short-form model suitable for human-to-human conversations as well. Given this flexibility and overlap, you might experiment with both models to determine which works best for your application. In either case, applying a custom language model with a grammar to the short-form model can further improve recognition results.

As with all models, noisy environments can adversely impact the results. For example, background acoustic noise from airports, moving vehicles, conference rooms, and multiple speakers can reduce transcription accuracy. Audio from speaker phones can also reduce accuracy due to the echo common to such devices. Using the parameters available for speech activity detection can counteract such effects and help improve speech transcription accuracy. Applying a custom acoustic model can further fine-tune the acoustics for speech recognition, but only as a final measure.

-   For more information about language model and acoustic model customization, see [The customization interface](/docs/speech-to-text?topic=speech-to-text-customization).
-   For more information about grammars, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).
-   For more information about speech activity detection parameters, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection).

### Language model example
{: #modelsExample}

The following example HTTP request uses the model `en-US-NarrowbandModel` for speech recognition:

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Listing models
{: #listModels}

The HTTP interface provides two methods for listing information about the supported models:

-   The `GET /v1/models` method lists information about all available models.
-   The `GET /v1/models/{model_id}` method lists information about a specified model.

Both methods return the following information about a model:

-   `name` is the name of the model that you use in a request.
-   `language` is the language identifier of the model (for example, `en-US`).
-   `rate` identifies the sampling rate (minimum acceptable rate for audio) that is used by the model in Hertz.
-   `url` is the URI for the model.
-   `description` provides a brief description of the model.
-   `supported_features` describes the additional service features that are supported with the model:
    -   `custom_language_model` is a boolean that indicates whether you can create custom language models that are based on the model.
    -   `speaker_labels` indicates whether you can use the `speaker_labels` parameter with the model.

    The `speaker_labels` field returns `true` for all models. However, speaker labels are supported as beta functionality only for US English, Australian English, German, Japanese, Korean, and Spanish (both broadband and narrowband models) and UK English (narrowband model only). Speaker labels are not supported for any other models. Do not rely on the field to identify which models support speaker labels.
    {: note}

The order in which the service returns models can change from call to call. Do not rely on an alphabetized or static list of models. Because the models are returned as an array of JSON objects, the order has no bearing on programmatic uses of the response.
{: note}

### Example requests and responses
{: #listExample}

The following example lists all models that are supported by the service:

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "{url}/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "{url}/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "{url}/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

The following example shows information about the US English broadband model. The model supports both language model customization and speakers labels.

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "{url}/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
