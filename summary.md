---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-02"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Parameter summary
{: #summary}

A summary follows of all of the parameters available for speech recognition. For more information about all methods of the {{site.data.keyword.speechtotextshort}} service, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
{: shortdesc}

## access_token
{: #summary-access-token}

A required Identity and Access Management (IAM) access token that you use to establish an authenticated connection with the WebSocket interface. For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#WSopen).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Not supported |
{: caption="Table 1. The access_token parameter"}

## acoustic_customization_id
{: #summary-acoustic-customization-id}

An optional customization ID for a custom acoustic model that is adapted for the acoustic characteristics of your environment and speakers. By default, no custom model is used. For more information, see [Custom models](/docs/speech-to-text?topic=speech-to-text-input#custom-input).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available or beta for all models that support acoustic model customization. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport). |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 2. The acoustic_customization_id parameter"}

## audio_metrics
{: #summary-audio-metrics}

An optional boolean that indicates whether the service returns metrics about the signal characteristics of the input audio. By default (`false`), the service returns no audio metrics. For more information, see [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio_metrics).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 3. The audio_metrics parameter"}

## background_audio_suppression
{: #summary-background-audio-suppression}

An optional float between 0.0 and 1.0 that indicates the level to which background audio and side conversations are to be suppressed in the input audio. The default is 0.0, which provides no suppression of background audio. For more information, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all language models except for `ar-MS_BroadbandModel`, `pt-BR_BroadbandModel`, `zh-CN_BroadbandModel`, `zh-CN_NarrowbandModel`, and `de-DE_BroadbandModel` |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 4. The background_audio_suppression parameter"}

## base_model_version
{: #summary-base-model-version}

An optional version of a base model. The parameter is intended primarily for use with custom models that are updated for a new base model, but it can be used without a custom model. The default value depends on whether the parameter is used with or without a custom model. For more information, see [Base model version](/docs/speech-to-text?topic=speech-to-text-input#version).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 5. The base_model_version parameter"}

## Content-Type
{: #summary-content-type}

An optional audio format (MIME type) that specifies the format of the audio data that you pass to the service. The service can automatically detect the format of most audio, so the parameter is optional for most formats. It is required for the `audio/alaw`, `audio/basic`, `audio/l16`, and `audio/mulaw` formats. For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | `content-type` parameter of JSON `start` message |
| Synchronous HTTP       | Request header of `POST /v1/recognize` method |
| Asynchronous HTTP      | Request header of `POST /v1/recognitions` method |
{: caption="Table 6. The Content-Type parameter"}

## customization_weight
{: #summary-customization-weight}

An optional double between 0.0 and 1.0 that indicates the relative weight that the service gives to words from a custom language model versus words from the base vocabulary. The default is 0.3 unless a different weight was specified when the custom language model was trained. For more information, see [Custom models](/docs/speech-to-text?topic=speech-to-text-input#custom-input).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available or beta for all models that support language model customization. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport). |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 7. The customization_weight parameter"}

## end_of_phrase_silence_time
{: #summary-silence-time}

An optional double between 0.0 and 120.0 that indicates the pause interval at which the service splits a transcript into multiple final results if it encounters silence. By default, the service uses a pause interval of 0.8 seconds for all languages other than Chinese, for which it uses an interval of 0.6 seconds. For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-output#silence_time).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 8. The end_of_phrase_silence_time parameter"}

## grammar_name
{: #summary-grammar-name}

An optional string that identifies a grammar that is to be used for speech recognition. The service recognizes only strings that are defined by the grammar. You must specify both the name of the grammar and the customization ID of the custom language model for which the grammar is defined. For more information, see [Grammars](/docs/speech-to-text?topic=speech-to-text-input#grammars-input).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Beta for all models that support language model customization. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport). |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 9. The grammar_name parameter"}

## inactivity_timeout
{: #summary-inactivity-timeout}

An optional integer that specifies the number of seconds for the service's inactivity timeout. Inactivity means that the service detects no speech in streaming audio. The default is 30 seconds. Use `-1` to indicate infinity. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 10. The inactivity_timeout parameter"}

## interim_results
{: #summary-interim-results}

An optional boolean that directs the service to return intermediate hypotheses that are likely to change before the final transcript. By default (`false`), interim results are not returned. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-output#interim).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Not supported |
{: caption="Table 11. The interim_results parameter"}

## keywords
{: #summary-keywords}

An optional array of keyword strings that the service spots in the input audio. By default, keyword spotting is not performed. For more information, see [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-output#keyword_spotting).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 12. The keywords parameter"}

## keywords_threshold
{: #summary-keywords-threshold}

An optional double between 0.0 and 1.0 that indicates the minimum threshold for a positive keyword match. By default, keyword spotting is not performed. For more information, see [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-output#keyword_spotting).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 13. The keywords_threshold parameter"}

## language_customization_id
{: #summary-language-customization-id}

An optional customization ID for a custom language model that includes terminology from your domain. By default, no custom model is used. For more information, see [Custom models](/docs/speech-to-text?topic=speech-to-text-input#custom-input).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available or beta for all models that support language model customization. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport). |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 14. The language_customization_id parameter"}

## max_alternatives
{: #summary-max-alternatives}

An optional integer that specifies the maximum number of alternative hypotheses that the service returns. By default, the service returns a single final hypothesis. For more information, see [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-output#max_alternatives).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 15. The max_alternatives parameter"}

## model
{: #summary-model}

An optional model that specifies the language in which the audio is spoken and the rate at which it was sampled: broadband or narrowband. By default, `en-US_BroadbandModel` is used. For more information, see [Languages and models](/docs/speech-to-text?topic=speech-to-text-models).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 16. The model parameter"}

## processing_metrics
{: #summary-processing-metrics}

An optional boolean that indicates whether the service returns metrics about its processing of the input audio. By default (`false`), the service returns no processing metrics. For more information, see [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing_metrics).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 17. The processing_metrics parameter"}

## processing_metrics_interval
{: #summary-processing-metrics-interval}

An optional float of at least 0.1 that indicates the interval at which the service is to return processing metrics. If the `processing_metrics` parameter is `true`, the service returns processing metrics every 1.0 seconds by default. For more information, see [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing_metrics).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 18. The processing_metrics_interval parameter"}

## profanity_filter
{: #summary-profanity-filter}

An optional boolean that indicates whether the service censors profanity from a transcript. By default (`true`), profanity is filtered from the transcript. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-output#profanity_filter).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for US English and Japanese |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 19. The profanity_filter parameter"}

## redaction
{: #summary-redaction}

An optional boolean that indicates whether the service redacts numeric data with three or more consecutive digits from a transcript. If you set the `redaction` parameter to `true`, the service automatically forces the `smart_formatting` parameter to be `true`. By default (`false`), numeric data is not redacted. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-output#redaction).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Beta for US English, Japanese, and Korean |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 20. The redaction parameter"}

## smart_formatting
{: #summary-smart-formatting}

An optional boolean that indicates whether the service converts dates, times, numbers, currency, and similar values into more conventional representations in the final transcript. For US English, the feature also converts certain keyword phrases into punctuation symbols. By default (`false`), smart formatting is not performed. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-output#smart_formatting).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Beta for US English, Japanese, and Spanish |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 21. The smart_formatting parameter"}

## speaker_labels
{: #summary-speaker-labels}

An optional boolean that indicates whether the service identifies which individuals spoke which words in a multi-participant exchange. If you set the `speaker_labels` parameter to `true`, the service automatically forces the `timestamps` parameter to be `true`. By default (`false`), speaker labels are not returned. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Beta for US English, Australian English, German, Japanese, Korean, and Spanish (broadband and narrowband models) and UK English (narrowband model only) |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 22. The speaker_labels parameter"}

## speech_detector_sensitivity
{: #summary-speech-detector-sensitivity}

An optional float between 0.0 and 1.0 that indicates the sensitivity of speech recognition to non-speech events in the input audio. The default is 0.5, which provides a reasonable level of sensitivity to non-speech events. For more information, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all language models except for `ar-MS_BroadbandModel`, `pt-BR_BroadbandModel`, `zh-CN_BroadbandModel`, `zh-CN_NarrowbandModel`, and `de-DE_BroadbandModel` |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 23. The speech_detector_sensitivity parameter"}

## split_transcript_at_phrase_end
{: #summary-split-transcript}

An optional boolean that indicates whether the service splits a transcript into multiple final results based on semantic features of the input such as sentences. The service bases its understanding of semantic features on the base language model, which can be further influenced by custom language models and grammars. By default (`false`), the service produces no semantic splits. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-output#split_transcript).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 24. The split_transcript_at_phrase_end parameter"}

## timestamps
{: #summary-timestamps}

An optional boolean that indicates whether the service produces timestamps for the words of the transcript. By default (`false`), timestamps are not returned. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-output#word_timestamps).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 25. The timestamps parameter"}

## Transfer-Encoding
{: #summary-transfer-encoding}

An optional value of `chunked` that causes the audio to be streamed to the service. By default, audio is sent all at once as a one-shot delivery. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Not applicable; always streamed |
| Synchronous HTTP       | Request header of `POST /v1/recognize` method |
| Asynchronous HTTP      | Request header of `POST /v1/recognitions` method |
{: caption="Table 26. The Transfer-Encoding parameter"}

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

An optional double between 0.0 and 1.0 that specifies the threshold at which the service reports acoustically similar alternatives for words of the input audio. By default, word alternatives are not returned. For more information, see [Word alternatives](/docs/speech-to-text?topic=speech-to-text-output#word_alternatives).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 27. The word_alternatives_threshold parameter"}

## word_confidence
{: #summary-word-confidence}

An optional boolean that indicates whether the service provides confidence measures for the words of the transcript. By default (`false`), word confidence measures are not returned. For more information, see [Word confidence](/docs/speech-to-text?topic=speech-to-text-output#word_confidence).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 28. The word_confidence parameter"}

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

An optional boolean that indicates whether you opt out of the default request logging that {{site.data.keyword.IBM_notm}} performs to improve the service for future users. To prevent IBM from accessing your data for general service improvements, specify `true` for the parameter. If you opt out, the service logs *no* user data from your requests, saving no audio or text to disk. For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-input#logging).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | `x-watson-learning-opt-out` query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Request header of each request |
| Asynchronous HTTP      | Request header of each request |
{: caption="Table 29. The X-Watson-Learning-Opt-Out parameter"}

## X-Watson-Metadata
{: #summary-x-watson-metadata}

An optional string that associates a customer ID with data that is passed for recognition requests. The parameter accepts the argument `customer_id={id}`. By default, no customer ID is associated with the data. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

| Availability and usage | Description |
|------------------------|-------------|
| Availability           | Generally available for all languages |
| WebSocket              | `x-watson-metadata` query parameter of `/v1/recognize` connection request. (You must URL-encode the argument, for example, `customer_id%3dmy_customer_ID`.) |
| Synchronous HTTP       | Request header of POST `/v1/recognize` request |
| Asynchronous HTTP      | Request header of `POST /v1/register_callback` and `POST /v1/recognitions` requests |
{: caption="Table 30. The X-Watson-Metadata parameter"}
