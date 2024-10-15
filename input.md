---

copyright:
  years: 2015, 2023
lastupdated: "2023-01-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Audio transmission and timeouts
{: #input}

The {{site.data.keyword.speechtotextfull}} service lets you pass audio to the service all at once or stream audio to the service. For audio streaming, the service enforces timeouts to ensure ongoing session activity.
{: shortdesc}

## Audio transmission
{: #transmission}

*With the WebSocket interface,* audio data is always streamed to the service over the connection. You can pass data through the socket all at once, or you can pass data for the live-use case as it becomes available. The service returns results as they become available.

*With the HTTP interfaces,* you can transmit audio to the service in either of the following ways:

-   *One-shot delivery.* You omit the `Transfer-Encoding` request header and pass all of the audio data to the service at one time as a single delivery.
-   *Streaming.* You set the `Transfer-Encoding` request header to the value `chunked` and stream the data over a persistent connection. The data does not need to exist fully before you stream it to the service. You can stream the data as it becomes available. The service sends results only when it receives the final chunk, which you indicate by sending an empty chunk.

    For more information about streaming chunked audio with the `Transfer-Encoding` header, see
    -   [Chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   [Transfer Codings](https://datatracker.ietf.org/doc/html/rfc7230#section-4){: external} in *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*

With the HTTP interfaces, the service always transcribes the entire audio stream before sending any results. The results can include multiple `transcript` elements to indicate phrases that are separated by pauses. Concatenate the `transcript` elements to assemble the complete transcript.

The service enforces timeouts on a streaming session. It can terminate a streaming session if it detects an extended period of silence or receives no audio during a 30-second period. For more information about timeouts and how to avoid them, see [Timeouts](#timeouts).

### Audio transmission example
{: #transmission-example}

The following example request specifies `chunked` for the `Transfer-Encoding` header to use streaming mode. The connection remains open to accept additional chunks of audio.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--header "Transfer-Encoding: chunked" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
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

### Inactivity timeout example
{: #timeouts-inactivity-example}

The following example request sets the inactivity timeout to 60 seconds. The request sends an initial file to begin the streaming session.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Transfer-Encoding: chunked" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?inactivity_timeout=60"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
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
