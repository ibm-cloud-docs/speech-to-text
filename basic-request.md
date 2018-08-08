---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-08"

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

# Making a recognition request
{: #basic-request}

The {{site.data.keyword.speechtotextshort}} service offers three interfaces for making a speech recognition request: a WebSocket interface, an HTTP REST interface, and an asynchronous HTTP interface. In addition, the HTTP REST interface supports requests both with and without establishing a session. Each interface provides the same basic speech recognition capabilities. To make a recognition request, you must provide
{: shortdesc}

-   *The audio that is to be transcribed.* You can pass a maximum of 100 MB of audio data with any request. The audio must be in one of the [Audio formats](/docs/services/speech-to-text/audio-formats.html) that are supported by the service.
-   *The format of the audio.* You use the `Content-Type` parameter to specify the audio format.

The following sections show basic transcription requests, with no optional parameters, for each of the service's interfaces. The examples submit a brief FLAC file named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a> and use the default language model, `en-US_BroadbandModel`. The service makes available a number of [Input features](/docs/services/speech-to-text/input.html) to control the audio that you send and many [Output features](/docs/services/speech-to-text/output.html) to request additional information in the resulting transcription. For a brief overview of all available parameters, see [Parameter summary](/docs/services/speech-to-text/summary.html).

Regardless of the interface that you use, the service responds with a transcript of your audio that reflects the input and output parameters that you specified. The final sections show the basic transcription response that is returned for the examples that follow. They describe the meaning and possible contents of the different fields of the response, including a description of the hesitation markers that can appear in a transcript.

## Using the WebSocket interface

[The WebSocket interface](/docs/services/speech-to-text/websockets.html) offers an efficient implementation that provides low latency and high throughput over a full-duplex connection. All requests and responses are sent over the same WebSocket connection. Because of their many advantages, WebSockets are the preferred mechanism for speech recognition.

To use the WebSocket interface, you first use the `/v1/recognize` method to establish a connection with the service. You specify parameters such as the language model and any custom models that are to be used for requests that are sent over the connection. You then register event listeners to handle responses from the service. To make a request, you send a JSON text message that includes the audio format and any additional parameters. You pass the audio as a binary message (blob), and then send a text message to signal the end of the audio.

The following example provides JavaScript code that establishes a connection and sends the text and binary messages for a recognition request. The example does not include the code to install the event handlers.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?watson-token=' + token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  'action': 'start',
  'content-type': 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({'action': 'stop'}));
```
{: codeblock}

## Using the sessionless HTTP interface

[Making sessionless requests](/docs/services/speech-to-text/http.html#HTTP-sessionless) is the simplest way to make a recognition request. You use the `POST /v1/recognize` method to make a sessionless request to the service. You pass the audio and all parameters with the single request.

The following cURL example shows a basic sessionless recognition request:

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Using the session-based HTTP interface

**Important:** The session-based HTTP REST interface is deprecated as of August 8, 2018. The sessions API will be removed from service as of September 7, 2018. For more information, see the August 8 service update in the [Release notes](/docs/services/speech-to-text/release-notes.html#August2018).*

[Making session-based requests](/docs/services/speech-to-text/http.html#HTTP-sessions) with the HTTP REST interface requires that you establish a session with the service. By using sessions, you can maintain a multi-turn exchange with the service or establish multiple parallel conversations with an instance of the service.

To make a session-based request, you first use the `POST /v1/sessions` method to establish a session with the service. When you create the session, you specify parameters such as the language model and any custom models that are to be used with the session. You then use the `POST /v1/sessions/{session_id}/recognize` method to make the request, identifying the session with the `{session_id}` path parameter and using headers and query parameters to pass any additional parameters for the request.

The following cURL example sends a basic recognition request over an existing session. Session-based requests require the use of cookies, which you pass with the `--cookie` option of the cURL command.

```bash
curl -X POST -u {username}:{password}
--cookie cookies.txt
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/sessions/{session_id}/recognize"
```
{: pre}

## Using the asynchronous HTTP interface

[The asynchronous HTTP interface](/docs/services/speech-to-text/async.html) provides a non-blocking interface for transcribing audio. You can use the interface with or without first registering a callback URL with the service. With a callback URL, the service sends callback notifications with job status and recognition results. The interface uses HMAC-SHA1 signatures based on a user-specified secret to provide authentication and data integrity for its notifications. Without a callback URL, you must poll the service for job status and results. With either approach, you use the `POST /v1/recognitions` method to make a recognition request.

The following cURL example shows a simple asynchronous recognition request. The request does not include a callback URL, so you must poll the service to get the job status and the resulting transcript.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

## Basic transcription response
{: #response}

For each of the previous examples, the service returns the following basic transcript of the input audio:

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

The fields of the response provide the following information:

-   `results` provides an array with information about the transcription. For a simple request, with no additional parameters, the service sends a single `results` field. If you use the `interim_results` parameter to request interim results, the service sends evolving interim hypotheses in the form of multiple `results` fields as it transcribes the audio. For more information, see [Interim results](/docs/services/speech-to-text/output.html#interim).

    The `results` field includes an array of elements that provide the following information:

    -   `alternatives` provides an array of transcription results. For a simple request, the array includes a single element. If you use the `max_alternatives` parameter to request multiple alternative transcripts, the array includes a separate element for each alternative. For more information, see [Maximum alternatives](/docs/services/speech-to-text/output.html#max_alternatives).

        -   `transcript` provides the results of the transcription.
        -   `confidence` is a score that indicates the service's confidence in the transcription results, which for this example approaches 90 percent. If you request multiple alternative transcripts, the service returns a confidence score only for the best alternative.

    -   `final` indicates whether the transcript shows final transcription results, results that are guaranteed not to change. The field is `true` for final results; it is `false` for interim results.

-   `result_index` provides an identifier for the results. Because the example shows final results for a simple transcription request with only a single audio file and no additional request parameters, the service returns a single `result_index` field with an index of `0`.

    -   If you request interim results, all interim and final results for the request have the same index. But once you receive results for which the `final` field is `true`, the service sends no further results with that index.
    -   For a WebSocket connection, once you receive final results for an index, the service sends no further results with that index for the remainder of the connection. The index for the next set of results that are sent over the connection is incremented by one.

If you include other output parameters with the request, the `results` field can include elements such as `keywords_result` and `word_alternatives`. Similarly, depending on the parameters that you include, the `alternatives` array can include fields such as `word_confidence` and `timestamps`. For more information about requesting additional information, see [Output parameters](/docs/services/speech-to-text/output.html).

## Hesitation markers
{: #hesitation}

The service can include hesitation markers in a transcript when it discovers brief fillers or pauses in speech. Also referred to as disfluencies, such pauses can include fillers such as "uhm", "uh", "hmm", and related non-lexical utterances. Unless you need to use them for your application, you can safely filter hesitation markers from a transcript.

In English, the service uses the hesitation token `%HESITATION`, as shown in the following example. Other languages can use different markers; for example, Japanese uses the token `D_`.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Hesitation markers can also appear in other fields of a transcript. For example, if you request [Word timestamps](/docs/services/speech-to-text/output.html#word_timestamps) for the individual words of a transcript, the service reports the start and end time of each hesitation marker.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
