---

copyright:
  years: 2015, 2023
lastupdated: "2024-05-02"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Parameter summary
{: #summary}

The following sections provide a summary of all of the parameters that are available for speech recognition. The information includes availability for large speech models, previous- and next-generation models, and support and usage for speech recognition interfaces.
{: shortdesc}

-   For more information about previous-generation languages and models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).
-   For more information about next-generation languages and models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
-   For more information about large speech languages and models, see [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages);

## access_token
{: #summary-access-token}

A required access token that you use to establish an authenticated connection with the WebSocket interface. For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#ws-open).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Not supported |
{: caption="Table 1. The access_token parameter"}

## acoustic_customization_id
{: #summary-acoustic-customization-id}

An optional customization ID for a custom acoustic model that is adapted for the acoustic characteristics of your environment and speakers. By default, no custom model is used. For more information, see [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models    | Not available. |
| Previous-generation models | Generally available or beta for all models that support acoustic model customization. For more information, see [Customization support for previous-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-pg). |
| Next-generation models | Not available. |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 2. The acoustic_customization_id parameter"}

## audio_metrics
{: #summary-audio-metrics}

An optional boolean that indicates whether the service returns metrics about the signal characteristics of the input audio. By default (`false`), the service returns no audio metrics. For more information, see [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio-metrics).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 3. The audio_metrics parameter"}

## background_audio_suppression
{: #summary-background-audio-suppression}

An optional float between 0.0 and 1.0 that indicates the level to which background audio and side conversations are to be suppressed in the input audio. The default is 0.0, which provides no suppression of background audio. For more information, see [Background audio suppression](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-suppression).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all language models except for `ar-MS_BroadbandModel`, `pt-BR_BroadbandModel`, `zh-CN_BroadbandModel`, `zh-CN_NarrowbandModel`, and `de-DE_BroadbandModel`. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 4. The background_audio_suppression parameter"}

## base_model_version
{: #summary-base-model-version}

An optional version of a base model. The parameter is intended primarily for use with custom models that are updated for a new base model, but it can be used without a custom model. The default value depends on whether the parameter is used with or without a custom model. For more information, see [Making speech recognition requests with upgraded custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-recognition).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 5. The base_model_version parameter"}

## character_insertion_bias
{: #summary-character-insertion-bias}

An optional float between -1.0 and 1.0 that indicates whether the service is biased to recognize shorter (negative values) or longer (positive values) strings of characters when developing transcription hypotheses. By default, the service uses a default bias of 0.0. The value that you specify represents a change from a model's default. For more information, see [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Not available. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 6. The character_insertion_bias parameter"}

## Content-Type
{: #summary-content-type}

An optional audio format (MIME type) that specifies the format of the audio data that you pass to the service. The service can automatically detect the format of most audio, so the parameter is optional for most formats. It is required for the `audio/alaw`, `audio/basic`, `audio/l16`, and `audio/mulaw` formats. For more information, see [Specifying an audio format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-specifying).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | `content-type` parameter of JSON `start` message |
| Synchronous HTTP       | Request header of `POST /v1/recognize` method |
| Asynchronous HTTP      | Request header of `POST /v1/recognitions` method |
{: caption="Table 7. The Content-Type parameter"}

## customization_weight
{: #summary-customization-weight}

An optional double between 0.0 and 1.0 that indicates the relative weight that the service gives to words from a custom language model versus words from the base vocabulary. The default differs for different types of models. You can specify a value either when a custom model is trained or when it is used for speech recognition. For more information, see [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available or beta for all models that support language model customization. The default value is 0.3. For more information, see [Customization support for previous-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-pg). |
| Next-generation models     | Generally available or beta for all models that support language model customization. The default value is 0.2 for most next-generation models; it is 0.1 for models that are based on new language model customization technology. For more information, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng). |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 8. The customization_weight parameter"}

## end_of_phrase_silence_time
{: #summary-silence-time}

An optional double between 0.0 and 120.0 that indicates the pause interval at which the service splits a transcript into multiple final results if it encounters silence. By default, the service uses a pause interval of 0.8 seconds for all languages other than Chinese, for which it uses an interval of 0.6 seconds. For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 9. The end_of_phrase_silence_time parameter"}

## grammar_name
{: #summary-grammar-name}

An optional string that identifies a grammar that is to be used for speech recognition. The service recognizes only strings that are defined by the grammar. You must specify both the name of the grammar and the customization ID of the custom language model for which the grammar is defined. For more information, see [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Not available. |
| Previous-generation models | Generally available or beta for all models that support language model customization. For more information, see [Customization support for previous-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-pg). |
| Next-generation models     | Generally available or beta for all models that support language model customization. For more information, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng). |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 10. The grammar_name parameter"}

## inactivity_timeout
{: #summary-inactivity-timeout}

An optional integer that specifies the number of seconds for the service's inactivity timeout. Inactivity means that the service detects no speech in streaming audio. The default is 30 seconds. Use `-1` to indicate infinity. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 11. The inactivity_timeout parameter"}

## interim_results
{: #summary-interim-results}

An optional boolean that directs the service to return intermediate hypotheses that are likely to change before the final transcript. By default (`false`), interim results are not returned. Interim results are available only with the WebSocket interface. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Generally available for all languages. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for next-generation models that support low latency, but only if both the `interim_results` and `low_latency` parameters are set to `true`. For more information, see [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency). |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Not supported |
{: caption="Table 12. The interim_results parameter"}

## keywords
{: #summary-keywords}

An optional array of keyword strings that the service spots in the input audio. By default, keyword spotting is not performed. For more information, see [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-spotting#keyword-spotting).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Not available. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Not available. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 13. The keywords parameter"}

## keywords_threshold
{: #summary-keywords-threshold}

An optional double between 0.0 and 1.0 that indicates the minimum threshold for a positive keyword match. By default, keyword spotting is not performed. For more information, see [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-spotting#keyword-spotting).

| Availability and usage | Description |
|------------------------|-------------|
| Large speech models        | Not available. |
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Not available. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 14. The keywords_threshold parameter"}

## language_customization_id
{: #summary-language-customization-id}

An optional customization ID for a custom language model that includes terminology from your domain. By default, no custom model is used. For more information, see [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse).

| Availability and usage | Description |
|------------------------|-------------|
Large speech models          | Generally available for all languages. |
| Previous-generation models | Generally available or beta for all models that support language model customization. For more information, see [Customization support for previous-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-pg). |
| Next-generation models     | Generally available or beta for all models that support language model customization. For more information, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng). |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 15. The language_customization_id parameter"}

## low_latency
{: #summary-low-latency}

An optional boolean that indicates whether the service is to produce results more quickly at the possible expense of transcription accuracy. By default (`false`), low latency is not enabled. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Not available. |
| Next-generation models     | Generally available or beta for next-generation models that support low latency. For more information, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported). |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 16. The low_latency parameter"}

## max_alternatives
{: #summary-max-alternatives}

An optional integer that specifies the maximum number of alternative hypotheses that the service returns. By default, the service returns a single final hypothesis. For more information, see [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-metadata#max-alternatives).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 17. The max_alternatives parameter"}

## model
{: #summary-model}

An optional model that specifies the language in which the audio is spoken and the rate at which it was sampled: broadband/multimedia or narrowband/telephony. By default, `en-US_BroadbandModel` is used. For more information, see [Using a model for speech recognition](/docs/speech-to-text?topic=speech-to-text-models-use).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 18. The model parameter"}

## processing_metrics
{: #summary-processing-metrics}

An optional boolean that indicates whether the service returns metrics about its processing of the input audio. By default (`false`), the service returns no processing metrics. For more information, see [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing-metrics).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Not available. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 19. The processing_metrics parameter"}

## processing_metrics_interval
{: #summary-processing-metrics-interval}

An optional float of at least 0.1 that indicates the interval at which the service is to return processing metrics. If the `processing_metrics` parameter is `true`, the service returns processing metrics every 1.0 seconds by default. For more information, see [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing-metrics).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Not available. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Not supported |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 20. The processing_metrics_interval parameter"}

## profanity_filter
{: #summary-profanity-filter}

An optional boolean that indicates whether the service censors profanity from a transcript. By default (`true`), profanity is filtered from the transcript. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-formatting#profanity-filtering).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for US English and Japanese. |
| Next-generation models     | Generally available for US English and Japanese. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 21. The profanity_filter parameter"}

## redaction
{: #summary-redaction}

An optional boolean that indicates whether the service redacts numeric data with three or more consecutive digits from a transcript. By default (`false`), numeric data is not redacted. If you set the `redaction` parameter to `true`, the service automatically forces the `smart_formatting` parameter to be `true`, and it disables the `keywords`, `keywords_threshold`, `max_alternatives`, and (for the WebSocket interface) `interim_results` parameters. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Beta for US English, Japanese, and Korean. |
| Next-generation models     | Beta for US English, Japanese, and Korean. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 22. The redaction parameter"}

## smart_formatting
{: #summary-smart-formatting}

An optional boolean that indicates whether the service converts dates, times, numbers, currency, and similar values into more conventional representations in the final transcript. For US English, the feature also converts certain keyword phrases into punctuation symbols. By default (`false`), smart formatting is not performed. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Beta for US English, Japanese, and Spanish (all dialects). |
| Next-generation models     | Beta for US English, Japanese, and Spanish (all dialects). It also also available for the `en-WW_Medical_Telephony` model when US English audio is recognized. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 23. The smart_formatting parameter"}

## smart_formatting_version
{: #summary-smart-formatting_version}

An optional boolean that indicates whether the service converts dates, times, numbers, currency, and similar values into more conventional representations in the final transcript. For more information, see [Smart formatting version](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-version).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Not supported.|
| Next-generation models | Available for US English (including en-WW_Medical_Telephony), Brazilian Portuguese, French and German. |
| WebSocket              | The service disables interim results. |
{: caption="Table 23a. The smart_formatting_version parameter"}

## speaker_labels
{: #summary-speaker-labels}

An optional boolean that indicates whether the service identifies which individuals spoke which words in a multi-participant exchange. If you set the `speaker_labels` parameter to `true`, the service automatically forces the `timestamps` parameter to be `true`. By default (`false`), speaker labels are not returned. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Beta for US English, Australian English, German, Japanese, Korean, and Spanish (broadband and narrowband models) and UK English (narrowband model only). |
| Next-generation models     | Beta for Czech, English (Australian, Indian, UK, and US), German, Japanese, Korean, and Spanish. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 24. The speaker_labels parameter"}

## speech_detector_sensitivity
{: #summary-speech-detector-sensitivity}

An optional float between 0.0 and 1.0 that indicates the sensitivity of speech recognition to non-speech events in the input audio. The default is 0.5, which provides a reasonable level of sensitivity to non-speech events. For more information, see [Speech detector sensitivity](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-sensitivity).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all language models except for `ar-MS_BroadbandModel`, `pt-BR_BroadbandModel`, `zh-CN_BroadbandModel`, `zh-CN_NarrowbandModel`, and `de-DE_BroadbandModel`. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 25. The speech_detector_sensitivity parameter"}

## split_transcript_at_phrase_end
{: #summary-split-transcript}

An optional boolean that indicates whether the service splits a transcript into multiple final results based on semantic features of the input such as sentences. The service bases its understanding of semantic features on the base language model, which can be further influenced by custom language models and grammars. By default (`false`), the service produces no semantic splits. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 26. The split_transcript_at_phrase_end parameter"}

## timestamps
{: #summary-timestamps}

An optional boolean that indicates whether the service produces timestamps for the words of the transcript. By default (`false`), timestamps are not returned. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 27. The timestamps parameter"}

## Transfer-Encoding
{: #summary-transfer-encoding}

An optional value of `chunked` that causes the audio to be streamed to the service. By default, audio is sent all at once as a one-shot delivery. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Not applicable; always streamed |
| Synchronous HTTP       | Request header of `POST /v1/recognize` method |
| Asynchronous HTTP      | Request header of `POST /v1/recognitions` method |
{: caption="Table 28. The Transfer-Encoding parameter"}

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

An optional double between 0.0 and 1.0 that specifies the threshold at which the service reports acoustically similar alternatives for words of the input audio. By default, word alternatives are not returned. For more information, see [Word alternatives](/docs/speech-to-text?topic=speech-to-text-spotting#word-alternatives).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Not available. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 29. The word_alternatives_threshold parameter"}

## word_confidence
{: #summary-word-confidence}

An optional boolean that indicates whether the service provides confidence measures for the words of the transcript. By default (`false`), word confidence measures are not returned. For more information, see [Word confidence](/docs/speech-to-text?topic=speech-to-text-metadata#word-confidence).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | Parameter of JSON `start` message |
| Synchronous HTTP       | Query parameter of `POST /v1/recognize` method |
| Asynchronous HTTP      | Query parameter of `POST /v1/recognitions` method |
{: caption="Table 30. The word_confidence parameter"}

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

[IBM Cloud]{: tag-ibm-cloud}

An optional boolean that indicates whether you opt out of the default request logging that {{site.data.keyword.IBM_notm}} performs to improve the service for future users. To prevent IBM from accessing your data for general service improvements, specify `true` for the parameter. If you opt out, the service logs *no* user data from your requests, saving no audio or text to disk. You can also opt out at the account level. For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | `x-watson-learning-opt-out` query parameter of `/v1/recognize` connection request |
| Synchronous HTTP       | Request header of each request |
| Asynchronous HTTP      | Request header of each request |
{: caption="Table 31. The X-Watson-Learning-Opt-Out parameter"}

## X-Watson-Metadata
{: #summary-x-watson-metadata}

An optional string that associates a customer ID with data that is passed for recognition requests. The parameter accepts the argument `customer_id={id}`. By default, no customer ID is associated with the data. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

| Availability and usage | Description |
|------------------------|-------------|
| Previous-generation models | Generally available for all languages. |
| Next-generation models     | Generally available for all languages. |
| WebSocket              | `x-watson-metadata` query parameter of `/v1/recognize` connection request. (You must URL-encode the argument, for example, `customer_id%3dmy_customer_ID`.) |
| Synchronous HTTP       | Request header of POST `/v1/recognize` request |
| Asynchronous HTTP      | Request header of `POST /v1/register_callback` and `POST /v1/recognitions` requests |
{: caption="Table 32. The X-Watson-Metadata parameter"}
