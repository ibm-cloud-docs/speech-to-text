---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# About

> ** Service update:** *The {{site.data.keyword.speechtotextshort}} service was updated on October 2, 2017. The service now supports beta acoustic model customization, which lets you develop custom acoustic models that are trained on the acoustic characteristics of your environment and speakers. The service now also supports a beta customization weight feature for custom language models, and it allows you to specify the endianness of audio submitted in `audio/l16` format. For information about all recent changes to the service, see the [Release notes](/docs/services/speech-to-text/release-notes.html).*

The {{site.data.keyword.speechtotextfull}} service provides an Application Programming Interface (API) that lets you add speech transcription capabilities to your applications. To transcribe the human voice accurately, the service leverages machine intelligence to combine information about grammar and language structure with knowledge of the composition of the audio signal. The service continuously returns and retroactively updates the transcription as more speech is heard.
{: shortdesc}

The service provides a variety of interfaces to suit the needs of your application. It supports many features that make it suitable for numerous use-cases. And it provides a customization interface that lets you enhance its base language and acoustic capabilities with vocabularies and acoustic characteristics specific to your domain, environment, and speakers.

## Supported interfaces
{: #interfaces}

The {{site.data.keyword.speechtotextshort}} service offers four interfaces:

-   A WebSocket interface for establishing persistent, full-duplex connections with the service for speech transcription.
-   An HTTP REST interface that supports both sessionless and session-based calls to the service for speech recognition.
-   An asynchronous HTTP interface that provides non-blocking calls to the service for speech recognition.
-   A customization interface that lets you expand the vocabulary of a base model with domain-specific terminology or adapt a base model for the acoustic characteristics of your audio.

SDKs are also available that simplify using the service's interfaces in various programming languages. For more information about application development with the service, see [Overview for developers](/docs/services/speech-to-text/developer-overview.html).

## Input features
{: #inputFeatures}

The WebSocket, HTTP, and asynchronous HTTP interfaces share many common [Input features](/docs/services/speech-to-text/input.html) for transcribing speech to text:

-   **Models:** For most languages, support both broadband (for audio that is sampled at a minimum rate of 16 kHz) and narrowband (for audio that is sampled at a minimum rate of 8 kHz) models.
-   **Audio formats:** Transcribe Free Lossless Audio Codec (FLAC), MP3 (Motion Picture Experts Group, or MPEG) format, Linear 16-bit Pulse-Code Modulation (PCM), Waveform Audio File Format (WAV), Ogg format with the Opus or Vorbis codec, Web Media (WebM) format with the Opus or Vorbis codec, mu-law (or u-law) audio data, and basic audio.
-   **Audio transmission:** Let the client pass as much as 100 MB of audio to the service as a continuous stream of data chunks or as a one-shot delivery, passing all of the data at one time. With streaming, the service enforces various timeouts to preserve resources.

## Output features
{: #outputFeatures}

The three interfaces also support the following [Output features](/docs/services/speech-to-text/output.html):

-   **Speaker labels (beta):** Recognize different speakers from audio in US English, Spanish, or Japanese. This feature provides a transcription that labels each speaker's contributions to a multi-participant conversation.
-   **Keyword spotting (beta):** Identify spoken phrases from the audio that match specified keyword strings with a user-defined level of confidence. This feature is especially useful when individual words or topics from the input are more important than the full transcription. For example, it can be used with a customer support system to determine how to route or categorize a customer request.
-   **Word alternatives (beta), confidence, and timestamps:** Report alternative words that are acoustically similar to the words being transcribed, confidence levels for each of the words, and timestamps for the start and end of each word.
-   **Maximum alternatives and interim results:** Return alternative and interim transcription results. The former provide different possible hypotheses; the latter represent interim hypotheses as the transcription progresses. In both cases, the service indicates final results in which it has the greatest confidence.
-   **Profanity filtering:** Censor profanity from US English transcriptions by default. You can use the filtering to sanitize the service's output.
-   **Smart formatting (beta):** Convert dates, times, numbers, phone numbers, and currency values in final transcripts of US English audio into more readable, conventional forms.

## Language support
{: #languages}

The service offers models for the following languages: Brazilian Portuguese, French, Japanese, Mandarin Chinese, Modern Standard Arabic, Spanish, UK English, and US English. The service does not support all features for all languages. In addition, it supports some features as generally available (GA) for production use and others as beta offerings for different languages.

-   The WebSocket, HTTP REST, and asynchronous HTTP interfaces are generally available for all supported languages.
-   The service offers broadband, narrowband, or both types of model for different languages. For more information, see [Languages and models](/docs/services/speech-to-text/input.html#models).
-   Some features are available for only some languages. For more information, see [Input features](/docs/services/speech-to-text/input.html) and [Output features](/docs/services/speech-to-text/output.html).
-   The language and acoustic model customization interfaces are available for different languages at different levels of support. For more information, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

## Use cases
{: #usecases}

The {{site.data.keyword.speechtotextshort}} service can be used in any application where speech or audio files are used as input and in which text is the desired output. Example applications include voice control of applications, embedded devices, and vehicle accessories; transcribing meetings and conference calls; and dictating email messages and notes.

## Pricing
{: #pricing}

For information about the pricing plans available for the service, see the [{{site.data.keyword.speechtotextshort}} service in the {{site.data.keyword.Bluemix_short}} Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/services/speech-to-text){: new_window}. For answers to questions related to pricing and examples of monthly usage costs, see the [Pricing FAQs](/docs/services/speech-to-text/faq-pricing.html).

## Try out the service
{: #tryOut}

For examples of the service in action, see

-   A [quick demo ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://speech-to-text-demo.mybluemix.net/){: new_window} of the {{site.data.keyword.speechtotextshort}} service that lets you transcribe text from streaming audio input or from a file that you upload.
-   Applications in {{site.data.keyword.watson}} Developer Cloud [Starter Kits ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} that demonstrate the {{site.data.keyword.speechtotextshort}} service.
-   An [Application Starter Kit ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://audio-analysis-application-starter-kit.mybluemix.net/){: new_window} that uses the {{site.data.keyword.speechtotextshort}} and Natural Language Understanding services to demonstrate live audio analysis.
-   The {{site.data.keyword.IBM_notm}} Watson blog post [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window} that shows how to use the service's WebSocket interface with Python to extract speech from audio. The post provides a thorough tutorial that demonstrates the steps and the code involved.
