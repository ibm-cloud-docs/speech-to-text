---

copyright:
  years: 2015, 2023
lastupdated: "2024-04-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Service features
{: #service-features}

The {{site.data.keyword.speechtotextfull}} service offers many advanced features to help you get the most from your audio transcription. The service offers multiple speech recognition interfaces, and these interfaces support many features that you can use to manage how you pass your audio to the service and the results that the service returns. You can also customize the service to enhance its vocabulary and to accommodate the acoustic characteristics of your audio. And as with all {{site.data.keyword.watson}} services, SDKs are available to simplify application development in many programming languages.
{: shortdesc}

## Using languages and models
{: #features-languages}

The service supports speech recognition for the many languages listed in [Language support](/docs/speech-to-text?topic=speech-to-text-about#about-languages). The service provides different models for the languages that it supports. Most language models are generally available (GA) for production use; a few are beta and subject to change.

-   For most languages, the service offers previous-generation *Broadband* and *Narrowband* models. Most previous-generation models are GA. For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).
-   For some languages, the service offers large speech models. For more information, see [Supported large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages).
-   The service also offers next-generation *Multimedia* and *Telephony* models that improve upon the speech recognition capabilities of the previous-generation models. All next-generation models are GA. Next-generation models return results with greater throughput and higher accuracy than previous-generation models. For more information, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

For most languages, you can transcribe audio at one of two sampling rates:

-   Use *Broadband* or *Multimedia* models for audio that is sampled at a minimum sampling rate of 16 kHz.
-   Use *Narrowband* or *Telephony* models for audio that is sampled at a minimum sampling rate of 8 kHz.
-   Large speech models supports both audio sampled at sampling rates of either 8 kHz or 16 kHz.

Starting **August 1, 2023**, all previous-generation models are now **discontinued** from the service. New clients must now only use the next-generation models. All existing clients must now migrate to the equivalent next-generation model. For more information, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
{: attention}

## Using audio formats
{: #features-audio}

The service supports speech recognition for the many audio formats listed in [Audio support](/docs/speech-to-text?topic=speech-to-text-about#about-formats). Different formats support different sampling rates and other characteristics. By using a format that supports compression, you can maximize the amount of audio data that you can send with a request.

-   For more information about understanding audio concepts, see [Audio terminology and characteristics](/docs/speech-to-text?topic=speech-to-text-audio-terminology).
-   For more information about the audio formats that you can use with the service, see [Supported audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats).

## Recognizing speech with the service
{: #features-recognition}

The {{site.data.keyword.speechtotextshort}} service offers a WebSocket interface and synchronous and asynchronous HTTP Representational State Transfer (REST) interfaces.

-   [The WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets) offers an efficient, low-latency, and high-throughput implementation over a full-duplex connection.
-   [The synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http) provides a basic interface to transcribe audio with blocking requests.
-   [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) provides a non-blocking interface that lets you register a callback URL to receive notifications or poll the service for job status and results.

All interfaces provide the same basic speech recognition capabilities, but you might specify the same parameter as a request header, a query parameter, or a parameter of a JSON object depending on the interface that you use. The service can also return different results depending on the interface and parameters that you use with a request.

-   For information about making a speech recognition requests with each of the service's interfaces, see [Making a speech recognition request](/docs/speech-to-text?topic=speech-to-text-basic-request).
-   For information about the results of a speech recognition request, see [Understanding speech recognition results](/docs/speech-to-text?topic=speech-to-text-basic-response).

### Data limits
{: #features-data-limits}

The interfaces accept the following maximum amounts of audio data with a single request:

-   The WebSocket interface accepts a maximum of 100 MB of audio.
-   The synchronous HTTP interface accepts a maximum of 100 MB of audio.
-   The asynchronous HTTP interface accepts a maximum of 1 GB of audio.

For more information about using compression to maximize the amount of data that you can send to the service, see [Data limits and compression](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-limits).

### Advantages of the WebSocket interface
{: #features-websocket-advantages}

The WebSocket interface has a number of advantages over the HTTP interface. The WebSocket interface

-   Provides a single-socket, full-duplex communication channel. The interface lets the client send multiple requests to the service and receive results over a single connection in an asynchronous fashion.
-   Provides a much simpler and more powerful programming experience. The service sends event-driven responses to the client's messages, eliminating the need for the client to poll the server.
-   Allows you to establish and use a single authenticated connection indefinitely. The HTTP interfaces require you to authenticate each call to the service.
-   Reduces latency. Recognition results arrive faster because the service sends them directly to the client. The HTTP interface requires four distinct requests and connections to achieve the same results.
-   Reduces network utilization. The WebSocket protocol is lightweight. It requires only a single connection to perform live-speech recognition.
-   Enables audio to be streamed directly from browsers (HTML5 WebSocket clients) to the service.
-   Returns results as soon as they are available when you use a large speech model, next-generation model or request interim results.

## Using speech recognition parameters
{: #features-parameters}

The service's speech recognition interfaces share largely common parameters for transcribing speech to text. The parameters let you tailor aspects of your request, such as whether the data is streamed or sent all at once, and the information that the service includes in its response.

The following sections introduce the speech recognition parameters and their functionality. Some parameters are available only for some speech recognition interfaces or for some languages and models. For information about all parameters and their interface and language support, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

### Speech or word detection
{: #features-speech-word-detection}

Use the new parameter speech_begin_event to receive a notification event the moment speech is detected in the audio stream. This feature allows real time applications to learn when you start speaking. A common use case for this feature is implementing barge-in in automated agent systems. Barge-in consists of interrupting audio playback when the caller starts speaking. Set the value to true to make the Speech to Text service send back a speech_begin_event response, which contains the time when speech activity is first detected within the audio stream. You can use this parameter in both standard and low latency mode.
- Parameter name: speech_begin_event
- Request parameter: speech_begin_event = true/false (boolean)
- Response object: "speech_begin_event.begin", for example: {"speech_begin_event": { "begin": }}

### Audio transmission and timeouts
{: #features-input}

-   [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission) describes how you can pass audio as a continuous stream of data chunks or as a one-shot delivery that passes all of the data at one time. With the WebSocket interface, audio data is always streamed to the service over the connection. With the HTTP interfaces, you can stream the audio or send it all at once.
-   [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts) are used by the service to ensure an active flow of data during audio streaming. When you initiate a streaming session, the service enforces inactivity and session timeouts from which your application must recover gracefully. If a timeout lapses during a streaming session, the service closes the connection.

### Interim results and low latency
{: #features-interim-results}

-   [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results) are intermediate hypotheses that the service returns as transcription progresses. They are available only with the WebSocket interface. The service returns final results when a transcript is complete. With the HTTP interfaces, the service always transcribes the entire audio stream before sending any results.
-   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency), when used with certain next-generation models, directs the service to produce final results even more quickly than the models usually do. Low latency is available with the WebSocket and HTTP interfaces. Although low latency further enhances the already improved response times of the models, it might reduce transcription accuracy. When you use the next-generation models with the WebSocket interface, low latency is required to obtain interim results.

### Speech activity detection
{: #features-detection}

-   [Speech detector sensitivity](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-sensitivity) adjusts the sensitivity of the service's detection of speech activity. Use the parameter to suppress word insertions from music, coughing, and other non-speech events that can adversely affect the quality of speech recognition.
-   [Background audio suppression](/docs/speech-to-text?topic=speech-to-text-detection#detection-parameters-suppression) suppresses background audio based on its volume to prevent it from being transcribed as speech. Use the parameter to suppress side conversations or background noise from speech recognition.

### Speech audio parsing
{: #features-audio-parsing}

-   [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time) specifies the duration of the pause interval at which the service splits a transcript into multiple final results in response to silence. If the service detects pauses or extended silence before it reaches the end of the audio stream, its response can include multiple final results. You can increase or decrease the pause interval to affect the results that you receive.
-   [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript) directs the services to split a transcript into multiple final results for semantic features such as sentences. The service bases its understanding of semantic features on the base language model that you use with a request. Custom language models and grammars can also influence how and where the service splits a transcript.
-   [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias) specifies whether a next-generation model is to favor shorter or longer strings as it develops hypotheses during speech recognition. As it develops transcription hypotheses, the service optimizes how it parses audio to balance between competing strings of different lengths. You can indicate that the service is to bias its analysis toward shorter or longer strings.

### Speaker labels
{: #features-speaker-labels}

-   [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels) identify different speakers from the audio of a multi-participant exchange. The transcription labels the words and times of each speaker's contributions to a multi-participant conversation. Speakers labels are beta functionality.

### Keyword spotting and word alternatives
{: #features-keyword-spotting}

-   [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-spotting#keyword-spotting) identifies spoken phrases that match specified keyword strings with a user-defined level of confidence. Keyword spotting is especially useful when individual phrases from the audio are more important than the full transcription. For example, a customer support system might identify keywords to determine how to route user requests.
-   [Word alternatives](/docs/speech-to-text?topic=speech-to-text-spotting#word-alternatives) request alternative words that are acoustically similar to the words of a transcript. The words that it identifies must meet a minimum confidence threshold that is specified by the user. The service identifies similar-sounding words and provides their start and end times, as well as its confidence in the possible alternatives.

### Response formatting and filtering
{: #features-response-formatting}

-   [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting) converts dates, times, numbers, currency values, phone numbers, and internet addresses into more readable, conventional forms in final transcripts. For US English, you can also provide keyword phrases to include certain punctuation symbols in final transcripts. Smart formatting is beta functionality.
-   [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction) redacts, or masks, numeric data from a final transcript. Redaction is intended to remove sensitive personal information, such as credit card numbers, from final transcripts. Numeric redaction is beta functionality.
-   [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-formatting#profanity-filtering) censors profanity from transcripts and metadata.

### Response metadata
{: #features-response-metadata}

-   [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-metadata#max-alternatives) provide possible alternative transcripts. The service indicates final results in which it has the greatest confidence.
-   [Word confidence](/docs/speech-to-text?topic=speech-to-text-metadata#word-confidence) returns confidence levels for each word of a transcript.
-   [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps) return timestamps for the start and end of each word of a transcript.

### Processing and audio metrics
{: #features-metrics}

-   [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing-metrics) provide detailed timing information about the service's analysis of the input audio. The service returns the metrics at specified intervals and with transcription events, such as interim and final results. You can use the metrics to gauge the service's progress in transcribing the audio. You can request processing metrics with the WebSocket and asynchronous HTTP interfaces.
-   [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio-metrics) provide detailed information about the signal characteristics of the input audio. The results provide aggregated metrics for the entire input audio at the conclusion of speech processing. You can use the metrics to determine the characteristics and quality of the audio. You can request audio metrics with any of the service's interfaces.

## Customizing the service
{: #features-customization}

The customization interface lets you create custom models to improve the service's speech recognition capabilities:

-   [Custom language models](/docs/speech-to-text?topic=speech-to-text-languageCreate) let you define domain-specific words for a base model. Custom language models can expand the service's base vocabulary with terminology specific to domains such as medicine and law. Language model customization is available for both previous- and next-generation models, though it works differently for the two types of models.
-   [Custom acoustic models](/docs/speech-to-text?topic=speech-to-text-acoustic) let you adapt a base model for the acoustic characteristics of your environment and speakers. Custom acoustic models improve the service's ability to recognize speech with distinctive acoustic characteristics. Acoustic model customization is available only for previous-generation models.
-   [Grammars](/docs/speech-to-text?topic=speech-to-text-grammars) let you restrict the phrases that the service can recognize to those defined in a grammar's rules. By limiting the search space for valid strings, the service can deliver results faster and more accurately. Grammars are created for and used with custom language models. The service generally supports grammars for languages and models for which it supports language model customization.

You can use a custom language model (with or without a grammar), a custom acoustic model, or both for speech recognition with any of the service's interfaces.

-   For more information about customization and an overview of its capabilities, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
-   For more information about which languages support customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

[IBM Cloud]{: tag-ibm-cloud} You must have the Plus, Standard, or Premium pricing plan to use language model or acoustic model customization. Users of the Lite plan cannot use the customization interface, but they can upgrade to the Plus plan to gain access to customization. For more information, see the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).
{: note}

## Using software development kits
{: #features-sdks}

SDKs are available for the {{site.data.keyword.speechtotextshort}} service to simplify the development of speech applications. The SDKs support many popular programming languages and platforms.

-   For a complete list of SDKs and links to the SDKs on GitHub, see [{{site.data.keyword.watson}} SDKs](/docs/speech-to-text?topic=watson-using-sdks).
-   For more information about all methods of the SDKs for the {{site.data.keyword.speechtotextshort}} service, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## Learning more about application development
{: #features-learn}

For more information about working with {{site.data.keyword.watson}} services and {{site.data.keyword.cloud_notm}}:

-   For an introduction, see [Getting started with {{site.data.keyword.watson}} and {{site.data.keyword.cloud_notm}}](/docs/watson?topic=watson-about).
-   For information about using {{site.data.keyword.cloud_notm}} Identity and Access Management, see [Authenticating to {{site.data.keyword.watson}} services](/docs/watson?topic=watson-iam).

## Next steps
{: #features-next-steps}

Explore the features introduced in this topic to gain a more in-depth understanding of the service's capabilities. Each feature includes links to topics that describe it in much greater detail.

-   [Using languages and models](#features-languages) and [Using audio formats](#features-audio) describe the basic underpinnings of the service's capabilities. You must choose a language and model that are suitable for your audio, and you must understand the characteristics of your audio to make that choice and to pass your audio to the service.
-   [Recognizing speech with the service](#features-recognition) provides links to simple examples of speech recognition requests and responses. There are also links to detailed presentations of each of the service's interfaces. Learn more about and experiment with the interfaces to determine which is best suited to your application needs.
-   [Using speech recognition parameters](#features-parameters) introduces the many parameters that you can use to tailor speech recognition requests and transcription responses to your needs. The service's WebSocket and HTTP interfaces support an impressive array of capabilities, most of which are common to all supported interfaces. Use the links to find the parameters that are right for you.
-   [Customizing the service](#features-customization) describes the more advanced topics of language model and acoustic model customization, which can help you gain the most from the service's capabilities. The section also presents grammars, which you can use with language models to limit possible responses to precise strings and phrases.
-   [Using software development kits](#features-sdks) provide links to the SDKs that are available to simplify application development in many programming languages.
-   [Learning more about application development](#features-learn) provides links to help you get started with {{site.data.keyword.watson}} services and understand authentication.
