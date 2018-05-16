---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-14"

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

# Basic usage
{: #basic}

1.  <span style="color:#003F69">My audio data is larger than 100 MB, which is too large for the service to accept. Is there a way that I can process the data?</span>

    One solution is to manually divide the audio data into smaller chunks. Alternatively, because the service supports numerous [Audio formats](/docs/services/speech-to-text/audio-formats.html), you can convert the audio to a compressed, lossy format. Conversion can increase the amount of data that you can send with a single request. Especially if the audio is in WAV or FLAC format, converting it to a lossy format can make an enormous difference.

    -   [Data limits and compression](/docs/services/speech-to-text/audio-formats.html#limits) compares the approximate duration of the audio that you can send with different formats.
    -   [Audio conversion](/docs/services/speech-to-text/audio-formats.html#conversion) describes the tools that you can use to convert your audio to a more efficient format
    -   [Converting to audio/ogg with the Opus codec](/docs/services/speech-to-text/audio-formats.html#conversionOgg) describes the **opusenc** tool. You can use the tool to convert a WAV or FLAC file to an Ogg file that uses the Opus codec. This conversion can greatly reduce the size of the audio so that you can send much more data with a request.

1.  <span style="color:#003F69">I passed a one-hour long audio file to the service but did not see results until the full transcription completed. How can I see my results sooner?</span>

    Use HTTP sessions or the WebSocket interface, and set the `interim_results` parameter to `true`. By default, the service returns results only when it detects long silences in the audio. See [Interim results](/docs/services/speech-to-text/output.html#interim).

1.  <span style="color:#003F69">My request times out with an HTTP status code of 400 and the message `No speech detected for 30s`. What can I do?</span>

    By default, the service responds with an inactivity timeout if it detects 30 seconds of continuous silence or non-speech activity at any time. To prevent the timeout, set the `inactivity_timeout` parameter for your request to a value greater than 30 seconds; set the parameter to `-1` to specify an inactivity timeout of infinity. See [Timeouts](/docs/services/speech-to-text/input.html#timeouts).

1.  <span style="color:#003F69">My request is being made from a browser that does not enable JavaScript, or the size of my request exceeds the maximum request length because it includes many keywords. How can I submit my request?</span>

    You can submit a multipart recognition request with the sessionless (`POST /v1/recognize`) or session-based (`POST /v1/sessions/{session_id}/recognize`) method. With multipart requests, you pass all audio data as multipart form data. You specify some parameters as request headers and query parameters, but you pass JSON metadata as form data to control most aspects of the transcription. See [Submitting multipart requests as form data](/docs/services/speech-to-text/http.html#HTTP-multi).

1.  <span style="color:#003F69">My audio includes pauses of varying lengths. How does the service respond to them?</span>

    The service transcribes an entire audio stream until either the stream ends or a timeout occurs, whichever comes first. If your input includes pauses, transcription results can include multiple `transcript` elements to indicate phrases that are separated by the pauses. Concatenate the `transcript` elements to assemble the complete transcription of the audio stream.

    All methods that initiate recognition requests previously included a `continuous` parameter that you could set to `true` to avoid having the service stop transcription at the first end of speech incident, typically silence. The service no longer stops transcription at pauses, so the parameter is removed from all methods.

1.  <span style="color:#003F69">Can I change the time or pause interval that determines when the end of a phrase occurs?</span>

    No. This capability is not currently available with the service.

1.  <span style="color:#003F69">My output includes multiple transcriptions of my content. Which one should I use?</span>

    Use the transcription for which the JSON `final` attribute is `true`. Ignore the intermediate results for which the attribute is `false`.

1.  <span style="color:#003F69">What is {{site.data.keyword.IBM_notm}}'s data-storage policy for audio that I submit to the service?</span>

    By default, {{site.data.keyword.IBM_notm}} logs each request to the service and its results. {{site.data.keyword.IBM_notm}} uses the data only to improve the service's base speech models for future users. You can prevent {{site.data.keyword.IBM_notm}} from using your data for this purpose by setting the `X-Watson-Learning-Opt-Out` request header to `true` for all requests. When you use the header to opt out of request logging, {{site.data.keyword.IBM_notm}} does not use your data to improve the service. The service accesses your data only to process your request. In either case, your data is always encrypted both in motion and at rest. For more information, see [Request logging](/docs/services/speech-to-text/input.html#logging).
