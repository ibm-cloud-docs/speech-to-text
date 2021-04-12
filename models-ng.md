---

copyright:
  years: 2021
lastupdated: "2021-04-05"

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

The next-generation language models and the `low_latency` parameter are beta functionality. The next-generation models support a limited number of languages and features at this time. The supported languages, models, and features will increase with future releases.
{: beta}

## Supported language models
{: #models-ng-supported}

The service makes available two types of next-generation models:

-   *Multimedia* models are intended for audio that is extracted from sources with a higher sampling rate, such as video. Use a multimedia model for any audio other than telephonic audio. Like previous-generation *broadband* models, this audio has a minimum sampling rate of 16 kHz.
-   *Telephony* models are intended specifically for audio that is communicated over a telephone. Like previous-generation *narrowband* models, this audio has a minimum sampling rate of 8 kHz.

Choose the model that most closely matches the source and sampling rate of your audio. The service automatically adjusts the sampling rate of your audio to match the model that you specify. To achieve the best recognition accuracy, also consider the frequency content of your audio. For more information, see [Sampling rate](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-sampling-rate) and [Audio frequency](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-frequency).

Table 1 lists the supported next-generation languages and models. Low-latency columns indicate whether each model supports the `low_latency` parameter for speech recognition. For more information about low latency, see [Reducing the latency of speech recognition requests](#models-ng-low-latency).

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

The next-generation models described in this topic are supported for use with a subset of the service's features. In cases where a supported feature is restricted to certain languages, those same language restrictions apply to the next-generation models.

Table 1 lists each parameter (and request header) that is supported for use with the next-generation models. It provides a brief description of the parameter and cites any language or model restrictions that apply for the next-generation models. Features not listed in the table are not supported for use with the next-generation models. Next-generation models do not support language model customization, acoustic model customization, or grammars.

| <br/>Parameter | <br/>Description | Next-generation language<br/>and model support |
|-----------|-------------|--------------------------------------------|
| `access_token` | A required Identity and Access Management (IAM) access token that you use to establish an authenticated connection with the WebSocket interface. For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#WSopen). | All languages and models. |
| `background_audio_suppression` | An optional float between 0.0 and 1.0 that indicates the level to which background audio and side conversations are to be suppressed in the input audio. For more information, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection). | All languages and models. |
| `Content-Type` | An optional audio format (MIME type) that specifies the format of the audio data that you pass to the service. For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list). | All languages and models. |
| `inactivity_timeout` | An optional integer that specifies the number of seconds for the service's inactivity timeout. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity). | All languages and models. |
| `interim_results` | An optional boolean that directs the service to return intermediate hypotheses that are likely to change before the final transcript. Available only with the WebSocket interface. | All languages and models that support low latency, but only when the `low_latency` parameter is `true`. For more information, see [Requesting low latency and interim results](#models-ng-low-latency-websockets). |
| `model` | An optional model that specifies the language in which the audio is spoken and the rate at which it was sampled: broadband/multimedia or narrowband/telephony. | All languages and models. |
| `profanity_filter` | An optional boolean that indicates whether the service censors profanity from a transcript. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-output#profanity_filter). | US English models only. |
| `redaction` | An optional boolean that indicates whether the service redacts numeric data with three or more consecutive digits from a transcript. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-output#redaction). | US English models only. |
| `smart_formatting` | An optional boolean that indicates whether the service converts dates, times, numbers, currency, and similar values into more conventional representations in the final transcript. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-output#smart_formatting). | US English and Spanish models only. |
| `speech_detector_sensitivity` | An optional float between 0.0 and 1.0 that indicates the sensitivity of speech recognition to non-speech events in the input audio. For more information, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection). | All languages and models. |
| `timestamps` | An optional boolean that indicates whether the service produces timestamps for the words of the transcript. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-output#word_timestamps). | All languages and models. |
| `Transfer-Encoding` | An optional value of `chunked` that causes the audio to be streamed to the service. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission). | All languages and models. |
| `X-Watson-Learning-Opt-Out` | An optional boolean that indicates whether you opt out of the default request logging that {{site.data.keyword.IBM_notm}} performs to improve the service for future users. For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging). | All languages and models. |
| `X-Watson-Media` | An optional string that associates a customer ID with data that is passed for recognition requests. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security). | All languages and models. |
{: caption="Table 2. Parameter support for next-generation languages and models"}

For more information about all available speech recognition parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

## Reducing the latency of speech recognition requests
{: #models-ng-low-latency}

Low latency is available only for the next-generation models for which support is indicated in Table 1 in [Supported language models](#models-ng-supported). Low latency is *not* available for use with previous-generation models.
{: note}

The next-generation multimedia and telephony models have generally faster response times than the previous-generation models. However, there may be cases where you want to receive results even faster. With next-generation models, you can set the `low_latency` parameter to `true` to receive results more quickly. The parameter is `false` by default.

With low latency, the service achieves faster results at the possible expense of transcription accuracy. When low-latency is enabled, the service curtails its analysis of the audio, which can reduce the accuracy of the transcription. This trade-off might be acceptable if your application needs lower response time more than it does the highest possible accuracy.

For example, low latency is ideal for use cases such as closed captioning, conversational applications, and live customer service in the voice channel of the {{site.data.keyword.conversationfull}} service. Low latency segments the audio into smaller chunks to optimize for speed, which can compromise accuracy. Speech recognition without low latency can produce more accurate results and is the recommended approach for most use cases.

The nature of the response to a request that includes low latency depends on the interface that is used:

-   With the HTTP interfaces, the service waits for all of the audio to arrive before sending a response. It then sends a single stream of bytes in response. The response can include multiple transcription elements with multiple final results, and it can be sent in increments. But it is a single stream of data.
-   With the WebSocket interface, the service sends final results as they become available. It can send multiple independent responses in the form of different streams of bytes. The connection is bidirectional and full duplex, and requests and responses can continue to flow back and forth over a single connection for as long as it remains active.

### Requesting low latency and interim results
{: #models-ng-low-latency-websockets}

The `interim_results` parameter is a boolean that lets you request intermediate hypotheses as they evolve with the WebSocket interface. Once its processing of an utterance is complete, the service sends final results that represent its best transcription of the audio for that utterance. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-output#interim).

Table 3 describes the interaction between the `low_latency` and `interim_results` parameters and the results you receive depending on their settings. It is important to understand the use of the `interim_results` parameter with next-generation models because it differs from that of the previous-generation models.

| Parameter settings | Results |
|--------------------|---------|
| `low_latency=false` | Interim results are not available, regardless of whether `interim_results` is set to `true`, set to `false`, or omitted. Omitting the `low_latency` parameter has the same effect, since its default is `false`. |
| `low_latency=true`<br/>`interim_results=false`| The service returns final results for an utterance when they are complete, as in the previous case. But because `low_latency` is `true`, the service returns final results more quickly. The results that it sends can have lower accuracy. The service does not send interim results. Omitting the `interim_results` parameter has the same effect, since its default is `false`. |
| `low_latency=true`<br/>`interim_results=true` | The service returns interim results as it develops hypotheses, and it sends final results for an utterance when they are complete. The quality of interim results is identical to what the user receives with previous-generation models. But, because low latency is enabled, the service returns interim and final results faster than it does normally with next-generation models. |
{: caption="Table 3. Interaction of low_latency and interim_results parameters"}

The service does not support interim results for next-generation models that do not support low latency. It responds differently to use of the `low_latency` and `interim_results` parameters with models that do not support low-latency:

-   For previous-generation models, if you include the `low_latency` parameter with a request, the service generates a warning:

    ```javascript
    "warnings": [
      "Unknown arguments: low_latency."
    ]
    ```
    {: codeblock}

-   For next-generation models that do not support low-latency, if you include the `low_latency` parameter with a request, the service fails with status code 400:

    ```javascript
    {
      "code": 400,
      "code_description": "Bad Request",
      "error": "low_latency is not a supported feature for model {model_id}"
    }
    ```

-   For next-generation models that do not support low latency, the service does not support interim results. If you include the `interim_results` parameter with a request, the service silently ignores the parameter.
