---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-08"

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

# Input features
{: #input}

The {{site.data.keyword.speechtotextfull}} service offers the following features to specify how the service is to perform a speech recognition request.  All of the input parameters that are described in the following sections are optional. Only the input audio is required.
{: shortdesc}

-   For examples of simple speech recognition requests for each of the service's interfaces, see [Making a speech recognition request](/docs/speech-to-text?topic=speech-to-text-basic-request).
-   For an alphabetized list of all available speech recognition parameters, including their status (generally available or beta) and supported languages, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

## Speech activity detection
{: #detection}

Speech activity detection is supported for most language models. For more information, see [Language model support](/docs/speech-to-text?topic=speech-to-text-input#detection_support).
{: note}

Speech activity detection consumes the input audio stream and determines which parts of the stream to pass for speech recognition. Speech recognition is adversely affected by background speech and noise, causing the service to transcribe the wrong words, to produce words where none are present, or to omit words that are part of the input audio. The speech activity detection feature can help ensure that only relevant audio is processed for speech recognition.

You can use the feature to control the following aspects of speech recognition:

-   *Suppress background speech.* Call-center data often contains cross-talk ("overhearing") from other agents. You can set a volume threshold below which such background speech is ignored.
-   *Suppress background noise.* Some audio, such as speech recorded in a factory, can contain a high level of background noise. You can set a threshold below which such background noise is ignored.
-   *Suppress non-speech audio events.* Background music and tone events, such as audio played to a client who is waiting on hold on a telephone line, can cause inaccurate recognition. Silence can also result in unnecessary recognition or transcription errors. You can set a threshold below which such events are ignored.

By default, speech activity detection is configured to provide optimal performance for the general case for each model. For specific cases, the default settings might not be optimal and can lead either to slow transcription or to word insertions and deletions. You are encouraged to experiment with different settings to determine which values work best for your audio.

### Speech activity detection parameters
{: #detection_parameters}

Two parameters provide control of speech activity detection. The parameters are available as query parameters for the synchronous and asynchronous HTTP interfaces. They are available as part of the JSON `start` message for the WebSocket interface.

-   `speech_detector_sensitivity`

    Use this parameter to adjust the sensitivity of speech activity detection. Use the parameter to suppress word insertions from music, coughing, and other non-speech events. The service biases the audio it passes for speech recognition by evaluating chunks of the input audio against prior models of speech and non-speech activity.

    Specify a float value between 0.0 and 1.0. The default value is 0.5, which provides a reasonable compromise for the level of sensitivity. A value of 0.0 suppresses all audio (no speech is transcribed). A value of 1.0 suppresses no audio (speech detection sensitivity is disabled). The values increase on a monotonic curve of sensitivity versus speech.

    This parameter can affect both the quality and the latency of speech recognition:

    -   Lower values can decrease latency because less audio is potentially passed for speech recognition. However, a low setting might discard chunks of audio that contain actual speech, losing viable content from the transcript.
    -   Higher values can increase latency because more audio is potentially passed for speech recognition. However, a high setting might pass chunks of audio that contain non-speech events, adding spurious content to the transcript.
-   `background_audio_suppression`

    Use this parameter to suppress background audio based on its volume to prevent it from being transcribed as speech. Use the parameter to suppress side conversations or background noise. For example, use this parameter when there is a relatively steady and quiet (low signal energy) background sound. Because such noise can interfere with transcription, it can produce content where no actual speech occurs in the audio.

    Specify a float value in the range of 0.0 to 1.0. The default value is 0.0, which provides no suppression (background audio suppression is disabled). A value of 0.5 provides a reasonable level of audio suppression for general usage. A value of 1.0 suppresses all audio (no speech is transcribed). The values increase on a monotonic curve.

    This  parameter can also affect both the quality and the latency of speech recognition. However, because background noise suppression is disabled by default, setting the parameter to a value greater than zero can only improve latency. But higher values can gradually reduce the audio that is passed for speech recognition, which can cause valid content to be lost from the transcript.

The parameters are independent. You can use them individually or together.

### Speech activity detection examples
{: #detection_examples}

The following example request specifies a value of 0.6 for the `speech_detector_sensitivity` parameter with the synchronous HTTP interface. The service recognizes slightly more potential non-speech events than it would by default.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?speech_detector_sensitivity=0.6"
```
{: pre}

The following example request specifies a value of 0.5 for the `background_audio_suppression` parameter with the synchronous HTTP interface. The service suppresses a reasonable level of background audio.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?background_audio_suppression=0.5"
```
{: pre}

### Language model support
{: #detection_support}

The following language models do *not* support speech activity detection at this time. The parameters are ignored if used with these models.

-   Arabic broadband model (`ar-MS_BroadbandModel`)
-   Brazilian Portuguese broadband model (`pt-BR_BroadbandModel`)
-   Chinese broadband model (`zh-CN_BroadbandModel`)
-   Chinese narrowband model (`zh-CN_NarrowbandModel`)
-   German broadband model (`de-DE_BroadbandModel`)

The latest versions of all other language models support speech activity detection. If a model is updated for the [24 February 2020 service update](/docs/speech-to-text?topic=speech-to-text-release-notes#February2020), only the latest version of the model supports speech activity detection. By default, the service uses the latest version of a model for speech recognition. If you use customization with a model that has been updated, you must upgrade to the latest version of the model to be able to use speech activity detection.

## Audio transmission
{: #transmission}

*With the WebSocket interface,* audio data is always streamed to the service over the connection. You can pass data through the socket all at once, or you can pass data for the live-use case as it becomes available. The service returns results as they become available.

*With the HTTP interfaces,* you can transmit audio to the service in either of the following ways:

-   *One-shot delivery.* You omit the `Transfer-Encoding` header and pass all of the audio data to the service at one time as a single delivery.
-   *Streaming.* You set the `Transfer-Encoding` request header to the value `chunked` and stream the data over a persistent connection. The data does not need to exist fully before you stream it to the service. You can stream the data as it becomes available. The service sends results only when it receives the final chunk, which you indicate by sending an empty chunk.

    For more information about streaming chunked audio with the `Transfer-Encoding` header, see
    -   [Chunked transfer encoding](https://wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   [Transfer Codings](https://tools.ietf.org/html/rfc7230#section-4){: external} in *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*

With the HTTP interfaces, the service always transcribes the entire audio stream before sending any results. The results can include multiple `transcript` elements to indicate phrases that are separated by pauses. Concatenate the `transcript` elements to assemble the complete transcript.

The service enforces timeouts on a streaming session. It can terminate a streaming session if it detects an extended period of silence or receives no audio during a 30-second period. For more information about timeouts and how to avoid them, see [Timeouts](#timeouts).

### Audio transmission example
{: #transmissionExample}

The following example request specifies `chunked` for the `Transfer-Encoding` header to use streaming mode. The connection remains open to accept additional chunks of audio.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--header "Transfer-Encoding: chunked" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize"
```
{: pre}

## Timeouts
{: #timeouts}

When you initiate a streaming session with the HTTP or WebSocket speech recognition methods, the service enforces inactivity and session timeouts. If a timeout lapses during a streaming session, the service closes the connection. Your application must recover gracefully from possible closed connections.

When you stream audio over HTTP, the service sends a space character in its response every 20 seconds. The service does this to improve usability by avoiding the 30-second HTTP REST inactivity timeout. To keep the connection alive while recognition is ongoing, the service continues to send this space character until it completes its transcription. The space character has no effect on JSON-encoded response data.

This HTTP inactivity timeout is different from the service's inactivity timeout. The WebSocket interface is not subject to this HTTP timeout.
{: note}

### Inactivity timeout
{: #timeouts-inactivity}

An *inactivity timeout* (HTTP status code 400) occurs when the service is receiving audio but detects only continuous silence or non-speech activity (no speech) for 30 seconds. The service sends the error message `No speech detected for 30s`. The inactivity timeout is useful, for example, for terminating a session when a user simply walks away from a live microphone.

The default inactivity timeout is 30 seconds. You can override this value by using the `inactivity_timeout` parameter. Specify a larger value to increase the inactivity timeout. Specify a value of `-1` to set the inactivity timeout to infinity. You are charged for all audio that you send to the service, including silence, so increasing the inactivity timeout can incur additional charges for a streaming session that sends only silence.

The following example request sets the inactivity timeout to 60 seconds. The request sends an initial file to begin the streaming session.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Transfer-Encoding: chunked" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?inactivity_timeout=60"
```
{: pre}

### Session timeout
{: #timeouts-session}

A *session timeout* (HTTP status code 408) occurs when you fail to send sufficient audio to keep a streaming session active. The service can consider a session idle and trigger a session timeout for the following reasons:

-   You fail to send at least 15 seconds of audio to the service in any 30-second window.

    Until you send the last chunk to indicate the end of the stream, you must send at least 15 seconds of audio within any 30-second period. The audio can be silence if you set the `inactivity_timeout` parameter to a larger value or to `-1`. You are charged for the duration of any audio that you send to the service, including silence.
-   You stream audio at a rate that is much slower than real-time.

    Ideally, you would initiate a request to establish a session just before you obtain audio for transcription. You would then maintain the session by sending audio at a rate that is close to real-time.

You do not need to worry about the session timeout after you send the last chunk to indicate the end of the stream. The service continues to process the audio until it returns the final transcription results.

When you transcribe a long audio stream, the service can take more than 30 seconds to process the audio and generate a response. The service does not begin to calculate the session timeout until it finishes processing all audio that it has received. The service's processing time cannot cause the session to exceed the 30-second session timeout.

For example, if you send one hour of audio in the first 10 seconds of a session, the service might take 300 seconds to process the audio.  To keep this session alive, you would need to send at least 15 more seconds of some audio, including silence, no later than 340 seconds into the session.

In this example, if you were to send another 15 seconds of audio at the 100-second mark of the session, the service might spend an additional two seconds processing this audio. In this case, you would need to send 15 more seconds of audio no later 342 seconds into the session.

Do not rely on processing time or on whether you have received results to determine whether a streaming session is idle. Assume that the service can process all audio instantly, and send data to the service accordingly. If you stream audio in real-time, do not fall behind in sending audio at one-half real-time (15 seconds of audio) in any 30-second window. This rate is typically sufficient to accommodate network latency and delays.
{: important}
