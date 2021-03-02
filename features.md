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
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Service features
{: #service-features}

The {{site.data.keyword.speechtotextfull}} service offers many advanced features to help you get the most from your audio transcription. The service offers multiple speech recognition interfaces, and these interfaces support many features that you can use to manage how you pass your audio to the service and the results that the service returns. You can also customize the service to enhance its vocabulary and to accommodate the acoustic characteristics of your audio.  And as with all {{site.data.keyword.watson}} services, SDKs are available to simplify application development in many programming languages.
{: shortdesc}

## Recognizing speech with the service
{: #features-recognition}

The {{site.data.keyword.speechtotextshort}} service offers a WebSocket interface and synchronous and asynchronous HTTP Representational State Transfer (REST) interfaces.

-   [The WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets) offers an efficient, low-latency, and high-throughput implementation over a full-duplex connection.
-   [The synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http) provides a basic interface to transcribe audio with blocking requests.
-   [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) provides a non-blocking interface that lets you register a callback URL to receive notifications or poll the service for job status and results.

All interfaces provide the same basic speech recognition capabilities, but you might specify the same parameter as a request header, a query parameter, or a parameter of a JSON object depending on the interface that you use.

-   For an overview of the parameters that you can use to tailor your speech recognition requests, see [Using speech recognition parameters](#features-parameters).
-   For examples of basic speech recognition requests with each of the service's interfaces, see [Making a recognition request](/docs/speech-to-text?topic=speech-to-text-basic-request).
-   For detailed descriptions and examples of the speech recognition methods, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

### Data limits
{: #features-data-limits}

The interfaces accept the following maximum amounts of audio data with a single request:

-   The WebSocket interface accepts a maximum of 100 MB of audio.
-   The synchronous HTTP interface accepts a maximum of 100 MB of audio.
-   The asynchronous HTTP interface accepts a maximum of 1 GB of audio.

### Language and audio support
{: #features-languages-formats}

The service supports speech recognition for many languages and audio formats.

-   For information about the supported languages, see [Language support](/docs/speech-to-text?topic=speech-to-text-about#about-languages).
-   For information about the supported audio formats, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-about#about-formats).

### Advantages of the WebSocket interface
{: #features-websocket-advantages}

The WebSocket interface has a number of advantages over the HTTP interface. The WebSocket interface

-   Provides a single-socket, full-duplex communication channel. The interface lets the client send requests and audio to the service and receive results over a single connection in an asynchronous fashion.
-   Provides a much simpler and more powerful programming experience. The service sends event-driven responses to the client's messages, eliminating the need for the client to poll the server.
-   Allows you to establish and use a single authenticated connection indefinitely. The HTTP interfaces require you to authenticate each call to the service.
-   Reduces latency. Recognition results arrive faster because the service sends them directly to the client. The HTTP interface requires four distinct requests and connections to achieve the same results.
-   Reduces network utilization. The WebSocket protocol is lightweight. It requires only a single connection to perform live-speech recognition.
-   Enables audio to be streamed directly from browsers (HTML5 WebSocket clients) to the service.

## Using speech recognition parameters
{: #features-parameters}

The service's speech recognition interfaces share common parameters for transcribing speech to text. The parameters let you tailor aspects of your request, such as whether the data is streamed or sent all at once, and the information that the service includes in its response. A few parameters are available only for some speech recognition interfaces or for some languages.

The following sections introduce the speech recognition parameters and their functionality. For information about all parameters and their interface and language support, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

### Audio transmission
{: #features-input}

You can pass audio as a continuous stream of data chunks or as a one-shot delivery that passes all of the data at one time. With the WebSocket interface, audio data is always streamed to the service over the connection. With the HTTP interfaces, you can stream the audio or send it all at once. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission).

With the WebSocket interface, you can request interim results, which provide interim hypotheses as the transcription progresses. The service returns final results when transcription is complete. With the HTTP interfaces, the service always transcribes the entire audio stream before sending any results. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-output#interim).

For most languages, you can use a pair of related parameters to control which parts of the audio stream are used for speech recognition. The parameters can help ensure that only relevant audio is processed for speech recognition by suppressing background noise and non-speech events that can adversely affect the quality of speech recognition. For more information, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-input#detection).

### Audio parsing
{: #features-audio-parsing}

You can direct the service to parse the audio in ways that determine how the service returns final transcription results:

-   [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-output#silence_time) specifies the duration of the pause interval at which the service splits a transcript into multiple final results in response to silence.
-   [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-output#split_transcript) directs the services to split a transcript into multiple final results for semantic features such as sentences. The service bases its understanding of semantic features on the base language model that you use with a request. Custom language models and grammars can also influence how and where the service splits a transcript. The feature is supported for US English and UK English audio.

### Speaker labels
{: #features-speaker-labels}

[Speaker labels](/docs/speech-to-text?topic=speech-to-text-output#speaker_labels) identify different speakers from the input audio. The transcription labels each speaker's contributions to a multi-participant conversation. Speakers labels are beta functionality that is available for US English, UK English, Australian English, German, Japanese, Korean, and Spanish.

### Keyword spotting and word alternatives
{: #features-keyword-spotting}

The service can identify user-specified keywords in a transcript and suggest possible alternative words that meet a specified confidence threshold:

-   [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-output#keyword_spotting) identifies spoken phrases that match specified keyword strings with a user-defined level of confidence. Keyword spotting is especially useful when individual phrases from the audio are more important than the full transcription. For example, a customer support system might identify keywords to determine how to route user requests.
-   [Word alternatives](/docs/speech-to-text?topic=speech-to-text-output#word_alternatives) request alternative words that are acoustically similar to the words of a transcript. The service identifies similar-sounding words and provides their start and end times, as well as its confidence in the possible alternatives.

### Response formatting
{: #features-response-formatting}

The service can filter or parse its results to provide more meaningful transcriptions:

-   [Smart formatting](/docs/speech-to-text?topic=speech-to-text-output#smart_formatting) converts dates, times, numbers, currency values, phone numbers, and internet addresses into more readable, conventional forms in final transcripts. For US English, you can also provide keyword phrases to include certain punctuation symbols in final transcripts. Smart formatting is beta functionality that is supported for US English, Japanese, and Spanish audio.
-   [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-output#redaction) redacts, or masks, numeric data from a final transcript. Redaction is intended to remove sensitive personal information, such as credit card numbers, from transcripts. Numeric redaction is beta functionality that is supported for US English, Japanese, and Korean audio.
-   [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-output#profanity_filter) censors profanity from US English and Japanese transcripts and metadata.

### Response metadata
{: #features-response-metadata}

The service can provide different metadata about a transcript and the words that the transcript contains:

-   [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-output#max_alternatives) provide possible alternative transcripts. The service indicates final results in which it has the greatest confidence.
-   [Word confidence](/docs/speech-to-text?topic=speech-to-text-output#word_confidence) returns confidence levels for each word of a transcript.
-   [Word timestamps](/docs/speech-to-text?topic=speech-to-text-output#word_timestamps) return timestamps for the start and end of each word of a transcript.

### Processing and audio metrics
{: #features-metrics}

The service offers two types of optional metrics for speech recognition requests:

-   [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing_metrics) provide detailed timing information about the service's analysis of the input audio. The service returns the metrics at specified intervals and with transcription events, such as interim and final results. You can use the metrics to gauge the service's progress in transcribing the audio. You can request processing metrics with the WebSocket and asynchronous HTTP interfaces.
-   [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio_metrics) provide detailed information about the signal characteristics of the input audio. The results provide aggregated metrics for the entire input audio at the conclusion of speech processing. You can use the metrics to determine the characteristics and quality of the audio. You can request audio metrics with any of the service's interfaces.

## Customizing the service
{: #features-customization}

[The customization interface](/docs/speech-to-text?topic=speech-to-text-customization) lets you create custom models to improve the service's speech recognition capabilities:

-   [Custom language models](/docs/speech-to-text?topic=speech-to-text-languageCreate) let you define domain-specific words for a base model. Custom language models can expand the service's base vocabulary with terminology specific to domains such as medicine and law.
-   [Custom acoustic models](/docs/speech-to-text?topic=speech-to-text-acoustic) let you adapt a base model for the acoustic characteristics of your environment and speakers. Custom acoustic models improve the service's ability to recognize speech with distinctive acoustic characteristics.
-   [Grammars](/docs/speech-to-text?topic=speech-to-text-grammars) let you restrict the phrases that the service can recognize to those defined in a grammar's rules. By limiting the search space for valid strings, the service can deliver results faster and more accurately. Grammars are created for and used with custom language models. The service supports grammars for all languages for which it supports language model customization. (The grammars feature is beta functionality.)

You can use a custom language model, a custom acoustic model, or both for speech recognition with any of the service's interfaces. For detailed descriptions of all customization methods, along with examples, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

You must have the Plus, Standard, or Premium pricing plan to use language model or acoustic model customization. Users of the Lite plan cannot use the customization interface. For more information, see the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).
{: note}

## Understanding data security
{: #features-data-security}

The {{site.data.keyword.speechtotextshort}} service provides the following security features to help you protect your user data:

-   The service provides security for all user data both in motion and at rest:

    -   Transport Layer Security (TLS) 1.2 is used to secure data in transit.
    -   Advanced Encryption Standard (AES)-256 with Secure Hash Algorithm (SHA)-256 is used to secure data at rest.

    For more information about data security for cloud applications, see [Security architecture for cloud applications](https://www.ibm.com/cloud/architecture/architectures/securityArchitecture/security-for-data){: external}.

-   The service offers Plus and Premium pricing plans for its users:

    -   Plus plans are multi-tenant solutions that provide logical separation of data by using common encryption keys.
    -   Premium plans are single-tenant solutions that provide physical separation of data. Premium plans provide dedicated data storage accounts that use unique encryption keys.

    Users of Premium plans can also integrate with {{site.data.keyword.keymanagementservicefull}} to create, import, and manage their encryption keys. This process is commonly referred to as *Bring your own keys* (BYOK). For more information about using {{site.data.keyword.keymanagementserviceshort}}, see [Protecting sensitive information in your Watson service](/docs/watson?topic=watson-keyservice).

-   The service lets you control the default request logging that is performed for all {{site.data.keyword.watson}} services. The service logs request and response data only to improve the service for future users. The logged data is never shared or made public.

    You can opt out of the default logging to prevent the service from logging your request and response data. If you opt out, the service logs *no* user data from your requests, saving no audio or text to disk. You can choose to opt out of logging at either the account level or the API request level. For more information, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/watson?topic=watson-gs-logging-overview).

-   The service supports the European Union General Data Protection Regulation (GDPR) to manage user data. For Premium plans, it also offers US Health Insurance Portability and Accountability Act (HIPAA) readiness. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

## Leveraging CORS support
{: #features-cors}

The service supports Cross-Origin Resource Sharing (CORS). By using CORS, web pages can request resources directly from a foreign domain. CORS circumvents the same-origin security policy, which otherwise prevents such requests. Because the service supports CORS, a web page can communicate directly with the service without passing the request through the web server that hosts the page.

For instance, a web page that is loaded from a server in {{site.data.keyword.cloud}} can call the customization API directly, bypassing the {{site.data.keyword.cloud_notm}} server. For more information, see [enable-cors.org](https://enable-cors.org/){: external}.

## Learning more about application development
{: #features-learn}

For more information about working with {{site.data.keyword.watson}} services and {{site.data.keyword.cloud_notm}}:

-   For an introduction, see [Getting started with {{site.data.keyword.watson}} and {{site.data.keyword.cloud_notm}}](/docs/watson?topic=watson-about).
-   For information about using {{site.data.keyword.cloud_notm}} Identity and Access Management, see [Authenticating to {{site.data.keyword.watson}} services](/docs/watson?topic=watson-iam).

SDKs are available for the {{site.data.keyword.speechtotextshort}} service to simplify the development of speech applications. The SDKs support many popular programming languages and platforms.

-   For a complete list of SDKs and links to the SDKs on GitHub, see [{{site.data.keyword.watson}} SDKs](/docs/speech-to-text?topic=watson-using-sdks).
-   For more information about all methods of the SDKs for the {{site.data.keyword.speechtotextshort}} service, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
