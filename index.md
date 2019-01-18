---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# About
{: #about}

**Service update:** *The {{site.data.keyword.speechtotextshort}} service was updated on December 20, 2018. The service now offers new beta grammar support for all languages that support language model customization. The service now supports new French and German narrowband models, a new short-form narrowband model for US English, and updated narrowband models for UK English and Spanish. It also offers a new parameter that redacts numbers with three or more digits from US English, Japanese, and Korean transcripts. For more information about these and other changes, see the [20 December 2018 service update](/docs/services/speech-to-text/release-notes.html#December2018b) in the release notes*.

The {{site.data.keyword.speechtotextfull}} service provides speech transcription capabilities for your applications. The service leverages machine learning to combine knowledge of grammar, language structure, and the composition of audio and voice signals to accurately transcribe the human voice. It continuously updates and refines its transcription as it receives more speech.
{: shortdesc}

The service provides various interfaces that make it suitable for any application where speech is the input and a textual transcript is the output. Example applications include

-   Voice control of applications, embedded devices, and vehicle accessories
-   Transcribing meetings and conference calls
-   Dictating email messages and notes

## Supported interfaces
{: #interfaces}

The {{site.data.keyword.speechtotextshort}} service offers three interfaces for speech recognition:

-   A [WebSocket interface](/docs/services/speech-to-text/websockets.html) for establishing persistent, full-duplex, low-latency connections with the service.
-   A [synchronous HTTP interface](/docs/services/speech-to-text/http.html) for basic HTTP calls to the service.
-   An [asynchronous HTTP interface](/docs/services/speech-to-text/async.html) for non-blocking calls to the service.

The service also provides a [customization interface](/docs/services/speech-to-text/custom.html) that you can use to tune speech recognition for your language and acoustic requirements. You can expand the vocabulary of a model with domain-specific terminology or adapt a model for the acoustic characteristics of your audio. You can also add [grammars](/docs/services/speech-to-text/grammar.html) to restrict the phrases that the service can recognize.

-   For a high-level description of application development with the service, see [Overview for developers](/docs/services/speech-to-text/developer-overview.html).
-   For examples of basic speech recognition requests with each of the service's interfaces, see [Making a recognition request](/docs/services/speech-to-text/basic-request.html).

SDKs are available in many programming languages to simplify your use of the service. For more information, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Input features
{: #inputFeatures}

The service's interfaces share common input features for transcribing speech to text:

-   [Audio formats](/docs/services/speech-to-text/audio-formats.html) - You can transcribe Ogg or Web Media (WebM) audio with the Opus or Vorbis codec, MP3 (or MPEG), Waveform Audio File Format (WAV), Free Lossless Audio Codec (FLAC), Linear 16-bit Pulse-Code Modulation (PCM), G.729, mu-law (or u-law), and basic audio.
-   [Languages and models](/docs/services/speech-to-text/models.html) - For most languages, you can transcribe audio by using broadband or narrowband models. Use broadband for audio that is sampled at a minimum rate of 16 kHz. Use narrowband for audio that is sampled at a minimum rate of 8 kHz.
-   [Audio transmission](/docs/services/speech-to-text/input.html#transmission) - You can pass as much as 100 MB of audio to the service (you must send at least 100 bytes of audio). You can pass the audio as a continuous stream of data chunks or as a one-shot delivery that passes all of the data at one time. With streaming, the service enforces [timeouts](/docs/services/speech-to-text/input.html#timeouts) to preserve resources.

## Output features
{: #outputFeatures}

The interfaces also support the following common output features:

-   [Speaker labels](/docs/services/speech-to-text/output.html#speaker_labels) recognize different speakers from audio in US English, UK English, Spanish, or Japanese. The transcription labels each speaker's contributions to a multi-participant conversation. (Beta functionality.)
-   [Keyword spotting](/docs/services/speech-to-text/output.html#keyword_spotting) identifies spoken phrases that match specified keyword strings with a user-defined level of confidence. Keyword spotting is especially useful when individual phrases from the audio are more important than the full transcription. For example, a customer support system might identify keywords to determine how to route user requests.
-   [Interim results](/docs/services/speech-to-text/output.html#interim) return progressive hypotheses as transcription progresses. The service returns final results when transcription is complete. Interim results are available only with the WebSocket interface.
-   [Maximum alternatives](/docs/services/speech-to-text/output.html#max_alternatives) provide possible alternative transcripts. The service indicates final results in which it has the greatest confidence.
-   [Word alternatives](/docs/services/speech-to-text/output.html#word_alternatives) request alternative words that are acoustically similar to the words of a transcript.
-   [Word confidence](/docs/services/speech-to-text/output.html#word_confidence) returns confidence levels for each word of a transcript.
-   [Word timestamps](/docs/services/speech-to-text/output.html#word_timestamps) return timestamps for the start and end of each word of a transcript.
-   [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting) converts dates, times, numbers, currency values, phone numbers, and internet addresses into more readable, conventional forms in final transcripts. For US English, you can also provide keyword phrases to include certain punctuation symbols in final transcripts. Smart formatting is supported for US English, Japanese, and Spanish audio. (Beta functionality.)
-   [Numeric redaction](/docs/services/speech-to-text/output.html#redaction) redacts, or masks, numeric data from a final transcript. Redaction is intended to remove sensitive personal information, such as credit card numbers, from transcripts. The feature is supported for US English, Japanese, and Korean audio. (Beta functionality.)
-   [Profanity filtering](/docs/services/speech-to-text/output.html#profanity_filter) censors profanity from US English transcripts.

## Language support
{: #languages}

The service offers models for the following languages: Brazilian Portuguese, French, German, Japanese, Korean, Mandarin Chinese, Modern Standard Arabic, Spanish, UK English, and US English. The service does not support all features for all languages. Moreover, it supports some features as generally available (GA) for production use and others as beta offerings for different languages.

-   The WebSocket and HTTP interfaces are generally available for all languages.
-   The service offers broadband models, narrowband models, or both for different languages. For more information, see [Languages and models](/docs/services/speech-to-text/models.html).
-   Some speech recognition features are available only for some languages. For more information, see the [Parameter summary](/docs/services/speech-to-text/summary.html).
-   The language model customization interface is generally available for most languages. The acoustic model customization interface is available as beta functionality for all languages. For more information, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

## Pricing
{: #pricing}

For more information about the pricing plans for the service, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.Bluemix_short}} Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/catalog/services/speech-to-text){: new_window}. For answers to questions related to pricing and examples of monthly usage costs, see the [Pricing FAQs](/docs/services/speech-to-text/faq-pricing.html).

## Try out the service
{: #tryOut}

For examples of the {{site.data.keyword.speechtotextshort}} service in action, see

-   A [quick demo ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://speech-to-text-demo.ng.bluemix.net/){: new_window} that transcribes text from streaming audio input or from a file that you upload.
-   Applications in the {{site.data.keyword.ibmwatson}} [Starter Kits ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} that demonstrate the service.
-   The {{site.data.keyword.watson}} blog post [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window} that shows how to use the service's WebSocket interface with Python to extract speech from audio. The post provides a thorough tutorial that demonstrates the code and steps involved.
