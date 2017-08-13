---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-11"

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

> ** Service update:** *The {{site.data.keyword.speechtotextshort}} service was updated on July 14, 2017. The service now supports the transcription of audio in the MP3 (MPEG) format, the language model customization interface now supports Spanish as beta functionality, and the names of the UK English language models have changed from `en-UK*` to `en-GB*`. The service was previously updated on July 1 to change language model customization for US English and Japanese from beta to generally available (GA) and to change the service's pricing models. For more information about these and all recent changes to the service, see the [Release notes](/docs/services/speech-to-text/release-notes.html).*

The {{site.data.keyword.speechtotextfull}} service provides an Application Programming Interface (API) that lets you add speech transcription capabilities to your applications. To transcribe the human voice accurately, the service leverages machine intelligence to combine information about grammar and language structure with knowledge of the composition of the audio signal. The service continuously returns and retroactively updates the transcription as more speech is heard.
{: shortdesc}

[Overview for developers](/docs/services/speech-to-text/developer-overview.html) introduces the three interfaces provided by the service: a WebSocket interface, an HTTP REST interface, and an asynchronous HTTP interface. It also introduces the HTTP customization interface for creating custom language models for US English and Japanese (GA) and for Spanish (beta). In addition, it provides information about the SDKs that are available for working with the service and about application development with {{site.data.keyword.watson}} services in general.

## Input features

The WebSocket, HTTP, and asynchronous HTTP interfaces share many common features for transcribing speech to text. The interfaces support the following [Input features](/docs/services/speech-to-text/input.html):

-   **Languages:** Supports Brazilian Portuguese, French, Japanese, Mandarin Chinese, Modern Standard Arabic, Spanish, UK English, and US English.
-   **Models:** For most languages, supports both broadband (for audio that is sampled at a minimum rate of 16 KHz) and narrowband (for audio that is sampled at a minimum rate of 8 KHz) models.
-   **Audio formats:** Transcribes Free Lossless Audio Codec (FLAC), MP3 (Motion Picture Experts Group, or MPEG) format, Linear 16-bit Pulse-Code Modulation (PCM), Waveform Audio File Format (WAV), Ogg format with the Opus or Vorbis codec, Web Media (WebM) format with the Opus or Vorbis codec, mu-law (or u-law) audio data, or basic audio.
-   **Audio transmission:** Lets the client pass as much as 100 MB of audio to the service as a continuous stream of data chunks or as a one-shot delivery, passing all of the data at one time. With streaming, the service enforces various timeouts to preserve resources.

## Output features

The three interfaces also support the following [Output features](/docs/services/speech-to-text/output.html):

-   **Speaker labels (beta):** Recognizes different speakers from audio in US English, Spanish, or Japanese. This feature provides a transcription that labels each speaker's contributions to a multi-participant conversation.
-   **Keyword spotting (beta):** Identifies spoken phrases from the audio that match specified keyword strings with a user-defined level of confidence. This feature is especially useful when individual words or topics from the input are more important than the full transcription. For example, it can be used with a customer support system to determine how to route or categorize a customer request.
-   **Word alternatives (beta), confidence, and timestamps:** Reports alternative words that are acoustically similar to the words that it transcribes, confidence levels for each of the words that it transcribes, and timestamps for the start and end of each word.
-   **Maximum alternatives and interim results:** Returns alternative and interim transcription results. The former provide different possible hypotheses; the latter represent interim hypotheses as the transcription progresses. In both cases, the service indicates final results in which it has the greatest confidence.
-   **Profanity filtering:** Censors profanity from US English transcriptions by default. You can use the filtering to sanitize the service's output.
-   **Smart formatting (beta):** Converts dates, times, numbers, phone numbers, and currency values in final transcripts of US English audio into more readable, conventional forms.

## Use cases
{: #usecases}

The {{site.data.keyword.speechtotextshort}} service can be used in any application where speech or audio files are used as input and in which text is the desired output format. Examples of these types of applications include

-   Voice control of applications, embedded devices, vehicle accessories, and so on
-   Transcribing meetings and conference calls
-   Dictating email messages, notes, and so on

## Pricing
{: #pricing}

For information about the pricing plans available for the service, see the [{{site.data.keyword.speechtotextshort}} service in the {{site.data.keyword.Bluemix_short}} Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/services/speech-to-text){: new_window}. For answers to questions related to pricing and examples of monthly usage costs, see the [Pricing FAQs](/docs/services/speech-to-text/faq-pricing.html).

## Try out the service

For examples of the service in action, see

-   A [quick demo ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://speech-to-text-demo.mybluemix.net/){: new_window} of the {{site.data.keyword.speechtotextshort}} service that lets you transcribe text from streaming audio input or from a file that you upload.
-   Applications in {{site.data.keyword.watson}} Developer Cloud [Starter Kits ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} that demonstrate the {{site.data.keyword.speechtotextshort}} service.
-   An [Application Starter Kit ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://audio-analysis-application-starter-kit.mybluemix.net/){: new_window} that uses the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.alchemylanguageshort}} services to demonstrate live audio analysis.
-   The {{site.data.keyword.IBM_notm}} Watson blog post [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window} that shows how to use the service's WebSocket interface with Python to extract speech from audio. The post provides a thorough tutorial that demonstrates the steps and the code involved.
