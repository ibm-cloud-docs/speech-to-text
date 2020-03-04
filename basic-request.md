---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-04"

subcollection: speech-to-text

---

{:support: data-reuse='support'}
{:help: .help}
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

# Making a recognition request
{: #basic-request}

To request speech recognition with the {{site.data.keyword.speechtotextfull}} service, you need to provide only the audio that is to be transcribed. The service offers the same basic transcription capabilities with each of its interfaces: the WebSocket interface, the synchronous HTTP interface, and the asynchronous HTTP interface.
{: shortdesc}

The following sections show basic transcription requests, with no optional input or output parameters, for each of the service's interfaces:

-   The examples submit a brief FLAC file named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a>.
-   The examples use the default language model, `en-US_BroadbandModel`.

[Understanding recognition results](/docs/speech-to-text?topic=speech-to-text-basic-response) describes the service's response for these examples.

## Usage requirements
{: #basic-request-usage}

Consider the following basic usage requirements when you make a speech recognition request:

-   Method names are case-sensitive.
-   HTTP request headers are case-insensitive.
-   HTTP and WebSocket query parameters are case-sensitive.
-   JSON field names are case-sensitive.
-   All JSON response content is in the UTF-8 character set.

Also consider the following service-specific requirements:

-   You need to specify only the input audio. All other parameters are optional.
-   If you specify an invalid query parameter or JSON field as part of the input, the response includes a `warnings` field that describes the invalid argument. The request succeeds despite any warnings.

## Sending audio with a request
{: #basic-request-audio}

The audio that you pass to the service must be in one of the service's supported formats. For most audio, the service can automatically detect the format. For some audio, you must specify the format with the `Content-Type` or equivalent parameter. For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats). (For clarity, the following examples specify the audio format with all requests.)

With the WebSocket and synchronous HTTP interfaces, you can pass a maximum of 100 MB of audio data with a single request. With the asynchronous HTTP interface, you can pass a maximum of 1 GB of audio data. You must send at least 100 bytes of audio with any request.

If you are recognizing large amounts of audio, you can manually divide the audio into smaller chunks. But it is usually more efficient and convenient to convert the audio to a compressed, lossy format. Compression can maximize the amount of data that you can send with a single request. Especially if the audio is in WAV or FLAC format, converting it to a lossy format can make an appreciable difference.

-   For more information about audio formats that use compression, see [Supported audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#formats).
-   For more information about the effects of compression and about converting your audio to a format that uses it, see [Data limits and compression](/docs/speech-to-text?topic=speech-to-text-audio-formats#limits) and [Audio conversion](/docs/speech-to-text?topic=speech-to-text-audio-formats#conversion).

## Using the WebSocket interface
{: #basic-request-websocket}
{: help}
{: support}

[The WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets) offers an efficient implementation that provides low latency and high throughput over a full-duplex connection. All requests and responses are sent over the same WebSocket connection. Because of their advantages, WebSockets are the preferred mechanism for speech recognition. For more information, see [Advantages of the WebSocket interface](/docs/speech-to-text?topic=speech-to-text-developerOverview#advantages).

To use the WebSocket interface, you first use the `/v1/recognize` method to establish a connection with the service. You specify parameters such as the language model and any custom models that are to be used for requests that are sent over the connection. You then register event listeners to handle responses from the service. To make a request, you send a JSON text message that includes the audio format and any additional parameters. You pass the audio as a binary message (blob), and then send a text message to signal the end of the audio.

The following example provides JavaScript code that establishes a connection and sends the text and binary messages for a recognition request. The example does not include the code to install the event handlers.

```javascript
var IAM_access_token = {access_token};
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  action: 'start',
  content-type: 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({action: 'stop'}));
```
{: codeblock}

## Using the synchronous HTTP interface
{: #basic-request-sync}
{: help}
{: support}

[The synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http) provides the simplest way to make a recognition request. You use the `POST /v1/recognize` method to make a request to the service. You pass the audio and all parameters with the single request.

The following `curl` example shows a basic HTTP recognition request:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"{url}/v1/recognize"
```
{: pre}

## Using the asynchronous HTTP interface
{: #basic-request-async}
{: help}
{: support}

[The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) provides a non-blocking interface for transcribing audio. You can use the interface with or without first registering a callback URL with the service. With a callback URL, the service sends callback notifications with job status and recognition results. The interface uses HMAC-SHA1 signatures based on a user-specified secret to provide authentication and data integrity for its notifications. Without a callback URL, you must poll the service for job status and results. With either approach, you use the `POST /v1/recognitions` method to make a recognition request.

The following `curl` example shows a simple asynchronous HTTP recognition request. The request does not include a callback URL, so you must poll the service to get the job status and the resulting transcript.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"{url}/v1/recognitions"
```
{: pre}
