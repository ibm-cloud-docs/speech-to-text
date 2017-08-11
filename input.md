---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-09"

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

# Input features and parameters
{: #input}

The {{site.data.keyword.speechtotextshort}} service offers many features for specifying the audio that is to be transcribed and how the service is to perform the transcription. The following sections describe the parameters that you can use to control how the service performs a transcription. The information also introduces the service-independent authentication token and request logging features.
{: shortdesc}

You need to specify only the audio format of the input; all other parameters are optional. If you specify an invalid query parameter or JSON field as part of the input for a recognition request, the JSON object that is returned by the service includes a `warnings` field that describes each invalid argument. The request succeeds despite the warnings.

## Authentication tokens and request logging
{: #common}

The {{site.data.keyword.speechtotextshort}} service leverages the following two {{site.data.keyword.watson}} service-independent features and parameters:

-   **Authentication tokens** are an alternative to service credentials that allow you to make authenticated requests to {{site.data.keyword.watson}} services without embedding your service credentials in every call. You use your service credentials to obtain a token for the service and then call the service directly, without relying on an intermediate server-side application for handling communications to and from the service. Note that you must use authentication tokens when working with the WebSocket interface. For more information, see [Tokens for authentication ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/common/getting-started-tokens.html){: new_window}.

    <table>
      <caption>Table 1. Specifying an authentication token</caption>
      <tr>
        <th style="width:25%; text-align:center">WebSocket</th>
        <th style="width:25%; text-align:center">HTTP with Sessions</th>
        <th style="width:25%; text-align:center">HTTP Sessionless</th>
        <th style="text-align:center">Asynchronous HTTP</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <code>watson-token</code> query parameter of <code>recognize</code>
          connection request
        </td>
        <td style="text-align:center">
          <code>watson-token</code> query parameter or
          <code>X-Watson-Authorization-Token</code> header of each request
        </td>
        <td style="text-align:center">
          <code>watson-token</code> query parameter or
          <code>X-Watson-Authorization-Token</code> header of each
          request
        </td>
        <td style="text-align:center">
          <code>watson-token</code> query parameter or
          <code>X-Watson-Authorization-Token</code> header of each
          request
        </td>
      </tr>
    </table>

-   **Request logging** is used by all {{site.data.keyword.watson}} services to log each request to a service and its results. When you agree (opt in) to have your data logged, {{site.data.keyword.IBM_notm}} reserves the right to store and use the data to improve the service's base speech models. {{site.data.keyword.IBM_notm}} stores the data only to improve the service for future users; the logged data is never shared or made public. Once you opt in, {{site.data.keyword.IBM_notm}} offers no mechanism to delete the stored audio or transcripts.

    To prevent {{site.data.keyword.IBM_notm}} from accessing your data for general service improvements, set the `X-Watson-Learning-Opt-Out` request header to `true` for all requests. When you use the header to opt out of request logging, {{site.data.keyword.IBM_notm}} stores none of your data. Your data exists within {{site.data.keyword.watson}} only while it is in transit (in memory while the service processes your request). Because the data is never stored in any way, {{site.data.keyword.IBM_notm}} provides no mechanism to delete it.

    In either case, your data is always encrypted both in motion and at rest. For more information about opting out of request logging, see [Controlling request logging for {{site.data.keyword.watson}} services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/common/getting-started-logging.html){: new_window}.

    <table>
      <caption>Table 2. Specifying request logging</caption>
      <tr>
        <th style="width:25%; text-align:center">WebSocket</th>
        <th style="width:25%; text-align:center">HTTP with Sessions</th>
        <th style="width:25%; text-align:center">HTTP Sessionless</th>
        <th style="text-align:center">Asynchronous HTTP</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <code>x-watson-learning-opt-out</code> query parameter of
          <code>recognize</code> connection request
        </td>
        <td style="text-align:center">
          <code>X-Watson-Learning-Opt-Out</code> header of each request
        </td>
        <td style="text-align:center">
          <code>X-Watson-Learning-Opt-Out</code> header of each request
        </td>
        <td style="text-align:center">
          <code>X-Watson-Learning-Opt-Out</code> header of each request
        </td>
      </tr>
    </table>

## Audio formats
{: #formats}

<table>
  <caption>Table 3. Specifying an audio format</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>content-type</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>Content-Type</code> header of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>Content-Type</code> header of <code>POST recognize</code>
      method
    </td>
    <td style="text-align:center">
      <code>Content-Type</code> header of <code>POST recognitions</code>
      method
    </td>
  </tr>
</table>

You must specify the audio format, or MIME type, of the data that you pass to the service. The service automatically detects the endianness of the incoming audio. For audio that includes multiple channels, the service downmixes the audio to one-channel mono during transcoding. The following table lists the supported formats and, where necessary, documents the maximum number of supported channels.

<table>
  <caption>Table 4. Supported audio formats</caption>
  <tr>
    <th style="width:20%; text-align:left">Audio format</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td>
      <code>audio/flac</code>
    </td>
    <td>
      <em>Free Lossless Audio Codec (FLAC)</em>, a lossless compressed
      audio coding format. For more information, see
      <a target="_blank" href="https://en.wikipedia.org/wiki/FLAC">en.wikipedia.org/wiki/FLAC ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/mp3</code><br/>
      <code>audio/mpeg</code>
    </td>
    <td>
      <em>MP3</em> or <em>Motion Picture Experts Group (MPEG)</em>, a lossy
      data compression format (MP3 and MPEG refer to the same format).
      For more information, see
      <a target="_blank" href="https://en.wikipedia.org/wiki/MP3">en.wikipedia.org/wiki/MP3 ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/l16</code>
    </td>
    <td>
      <em>Linear 16-bit Pulse-Code Modulation (PCM)</em>, an uncompressed
      audio data format. Use this media type to pass a raw PCM file.
      Note that linear PCM audio can also reside inside a container
      Waveform Audio File Format (WAV) file. For more information,
      see the Internet Engineering Task Force (IETF)
      <a target="_blank" href="https://tools.ietf.org/html/rfc2586">Request
        for Comment (RFC) 2586 ![External link icon](../../icons/launch-glyph.svg "External link icon")</a> and
      <a target="_blank" href="https://en.wikipedia.org/wiki/Pulse-code_modulation">en.wikipedia.org/wiki/Pulse-code_modulation ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.<br/><br/>
      When you use this media type, you must specify the sampling rate
      at which the audio is captured. You must also specify the number of
      channels for the recording if it is greater than one. The service
      can accept a maximum of 16 channels; it downmixes the audio to one
      channel during transcoding. For example, specify the following for
      two-channel, 16-bit PCM data captured at 48 KHz:
      <code>audio/l16;rate=48000;channels=2</code>.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/wav</code>
    </td>
    <td>
      <em>Waveform Audio File Format (WAV)</em>, a standard created
      by Microsoft&reg; and IBM. A WAV file is a container that is
      often used for uncompressed audio bitstreams but can contain
      compressed audio, as well. For more information, see
      <a target="_blank" href="https://en.wikipedia.org/wiki/WAV">en.wikipedia.org/wiki/WAV ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.<br/><br/>
      The service supports WAV files that use any encoding. It accepts
      audio with a maximum of nine channels (due to an FFmpeg limitation).
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/ogg</code><br/>
      <code>audio/ogg;codecs=opus</code><br/>
      <code>audio/ogg;codecs=vorbis</code>
    </td>
    <td>
      <em>Ogg</em> is a free, open container format maintained by the
      Xiph.org Foundation; for more information, see
      <a target="_blank" href="https://www.xiph.org/ogg/">www.xiph.org/ogg/ ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.
      You can use audio streams compressed with the following codecs:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <em>Opus</em>. For more information, see
          <a target="_blank" href="https://www.opus-codec.org/">opus-codec.org ![External link icon](../../icons/launch-glyph.svg "External link icon")</a> and
          <a target="_blank" href="https://en.wikipedia.org/wiki/Opus">en.wikipedia.org/wiki/Opus (audio format) ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>, especially the
          <em>Containers</em> section.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <em>Vorbis</em>. For more information, see
          <a target="_blank" href="https://xiph.org/vorbis/">xiph.org/vorbis ![External link icon](../../icons/launch-glyph.svg "External link icon")</a> and
          <a target="_blank" href="https://en.wikipedia.org/wiki/Vorbis">en.wikipedia.org/wiki/Vorbis ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.
        </li>
      </ul>
       Both codecs are free, open, lossy audio-compression formats. Opus is
      the preferred codec. If you omit the codec, the service automatically
      detects it from the input audio.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/webm</code><br/>
      <code>audio/webm;codecs=opus</code><br/>
      <code>audio/webm;codecs=vorbis</code>
    </td>
    <td>
      <em>Web Media (WebM)</em> is an open media-file format; for more
      information, see
      <a target="_blank" href="https://www.webmproject.org/">webmproject.org ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.
      WebM supports audio streams compressed with the Opus and Vorbis audio
      codecs; Opus is the preferred codec. If you omit the codec, the service
      automatically detects it from the input audio.<br/><br/>
      For JavaScript code that shows how to capture audio from a microphone
      in a Chrome browser and encode it into a WebM data stream, see
      <a target="_blank" href="https://jsbin.com/hedujihuqo/edit?js,console">jsbin.com/hedujihuqo/edit?js,console ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>. Note that the code does not submit
      the captured audio to the service.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/mulaw</code>
    </td>
    <td>
      <em>Mu-law (or u-law) audio files</em> provide single-channel
      audio encoded using the u-law (or mu-law) data algorithm. For
      more information, see
      <a target="_blank" href="https://en.wikipedia.org/wiki/M-law_algorithm">en.wikipedia.org/wiki/M-law_algorithm ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.<br/><br/>
      When you use this media type, you must specify the sampling rate
      at which the audio is captured. For example, specify the following
      for audio data sampled at 8 KHz: <code>audio/mulaw;rate=8000</code>.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/basic</code>
    </td>
    <td>
      <em>Basic audio files</em> provide single-channel audio that is encoded
      using 8-bit u-law (or mu-law) data sampled at 8 KHz. This subtype
      provides a lowest-common denominator format to indicate a media type
      of audio. For more information, see the IETF
      <a target="_blank" href="https://tools.ietf.org/html/rfc2046">Request
        for Comment (RFC) 2046 ![External link icon](../../icons/launch-glyph.svg "External link icon")</a> and
      <a target="_blank" href="http://www.iana.org/assignments/media-types/audio/basic">iana.org/assignments/media-types/audio/basic ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.<br/><br/>
      The service supports the use of files in <code>audio/basic</code>
      format only with narrowband models (for example,
      <code>en-US_NarrowbandModel</code>).
    </td>
  </tr>
</table>

### Audio tools
{: #audioTools}

If your audio files are not in a format supported by the service, you can use one of a number of freeware tools available for converting audio between formats and for playing audio files:

-   Sound eXchange (SoX) ([sox.sourceforge.net ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.audacityteam.org/){: new_window})

Each of these tools offers cross-platform support for multiple audio formats. Please do not use the tools to violate applicable copyright laws.

## Languages and models
{: #models}

<table>
  <caption>Table 5. Specifying a model</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>model</code> query parameter of <code>recognize</code>
      connection request
    </td>
    <td style="text-align:center">
      <code>model</code> query parameter of <code>POST sessions</code>
      method
    </td>
    <td style="text-align:center">
      <code>model</code> query parameter of <code>POST recognize</code>
      method
    </td>
    <td style="text-align:center">
      <code>model</code> query parameter of <code>POST recognitions</code>
      method
    </td>
  </tr>
</table>

You can specify the language in which the audio is spoken and the rate at which it was sampled. For most languages, the service supports two models:

-   Use the *broadband model* for audio that is sampled at greater than or equal to 16 KHz. {{site.data.keyword.IBM}} recommends that you use the broadband model for responsive, real-time applications (for example, for live-speech applications).
-   Use the *narrowband model* for audio that is sampled at 8 KHz. This rate typically corresponds to audio derived from the telephone. (Narrowband is not supported for Modern Standard Arabic or French.)

The service automatically adjusts the incoming sampling rate to match the model. For example, the service converts audio recorded at higher sampling rates to 16 KHz prior to performing speech recognition with broadband models. In theory, therefore, you can send 44 KHz audio with the narrowband model. Note, however, that the service does not accept audio sampled at a lower rate than the intrinsic sampling rate of the model. The following table lists the supported model names, their languages, and their minimum sampling rates.

<table>
  <caption>Table 6. Supported models</caption>
  <tr>
    <th style="text-align:left">Model Name</th>
    <th style="text-align:center">Language</th>
    <th style="text-align:center">Minimum Sampling Rate</th>
  </tr>
  <tr>
    <td><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">Modern Standard Arabic</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center">UK English</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>en-GB_NarrowbandModel</code></td>
    <td style="text-align:center">UK English</td>
    <td style="text-align:center">8 KHz</td>
  </tr>
  <tr>
    <td><code>en-US_BroadbandModel</code> (<em>Default</em>)</td>
    <td style="text-align:center">US English</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>en-US_NarrowbandModel</code></td>
    <td style="text-align:center">US English</td>
    <td style="text-align:center">8 KHz</td>
  </tr>
  <tr>
    <td><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center">Spanish</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>es-ES_NarrowbandModel</code></td>
    <td style="text-align:center">Spanish</td>
    <td style="text-align:center">8 KHz</td>
  </tr>
  <tr>
    <td><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center">French</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center">Japanese</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>ja-JP_NarrowbandModel</code></td>
    <td style="text-align:center">Japanese</td>
    <td style="text-align:center">8 KHz</td>
  </tr>
  <tr>
    <td><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center">Brazilian Portuguese</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>pt-BR_NarrowbandModel</code></td>
    <td style="text-align:center">Brazilian Portuguese</td>
    <td style="text-align:center">8 KHz</td>
  </tr>
  <tr>
    <td><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center">Mandarin Chinese</td>
    <td style="text-align:center">16 KHz</td>
  </tr>
  <tr>
    <td><code>zh-CN_NarrowbandModel</code></td>
    <td style="text-align:center">Mandarin Chinese</td>
    <td style="text-align:center">8 KHz</td>
  </tr>
</table>

To retrieve the complete list of supported model names, use the `GET models` method. To retrieve detailed information about a specific model, use the `GET models/{model_id}` method.

> **Note:** The service capitalizes many proper nouns for the US English language models, `en-US_BroadbandModel` and `en-US_NarrowbandModel`. For example, for US English transcriptions, the service returns text that reads "Barack Obama graduated from Columbia University" instead of "barack obama graduated from columbia university," as it does for other language models.

## Custom language models
{: #custom}

<table>
  <caption>Table 7. Specifying a custom language model</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>customization_id</code> query parameter of
      <code>recognize</code> connection request
    </td>
    <td style="text-align:center">
      <code>customization_id</code> query parameter of <code>POST
        sessions</code> method
    </td>
    <td style="text-align:center">
      <code>customization_id</code> query parameter of <code>POST
        recognize</code> method
    </td>
    <td style="text-align:center">
      <code>customization_id</code> query parameter of <code>POST
        recognitions</code> method
    </td>
  </tr>
</table>

All interfaces allow you to pass a custom language model to the service for use in a recognition request. Custom language models let you expand the service's base vocabulary with terminology from specific domains. In all cases, you pass the customization ID (GUID) of the custom model via a method's `customization_id` query parameter. You must issue the request with the service credentials of the model's owner.

Each custom language model is based on one of the language models described in [Language and models](#models) and can be used only with that base language model. If your custom language model is based on a model other than `en-US_BroadbandModel`, the default, you must also specify the name of the model with the request. For more information about creating and using custom language models, see [Using customization](/docs/services/speech-to-text/custom.html).

## Audio transmission
{: #transmission}

<table>
  <caption>Table 8. Specifying audio transmission</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      Always streamed
    </td>
    <td style="text-align:center">
      <code>Transfer-Encoding</code> header of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>Transfer-Encoding</code> header of <code>POST recognize</code>
      method
    </td>
    <td style="text-align:center">
      <code>Transfer-Encoding</code> header of <code>POST recognitions</code>
      method
    </td>
  </tr>
</table>

With the WebSocket interface, audio data is always streamed to the {{site.data.keyword.speechtotextshort}} service over the connection. You can pass data through the socket all at once or, for the live-use case, as it becomes available.

With the HTTP interface, you can transmit audio to the service in one of two ways:

-   *One-shot delivery:* You omit the `Transfer-Encoding` header and pass all of the audio data to the service at one time as a single delivery.
-   *Streaming:* You pass the `Transfer-Encoding` header with the value `chunked` and stream the data over a persistent connection. The data does not need to exist fully before being streamed to the service; you can stream the data as it becomes available. For more information about the header, see [en.wikipedia.org/wiki/Chunked_transfer_encoding ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}.

With either interface, the service always transcribes the entire audio stream until it terminates. Transcription results can include multiple `transcript` elements to indicate phrases separated by pauses. Concatenate the `transcript` elements to assemble the complete transcription of the audio stream.

To preserve system resources when you stream audio data, the service enforces timeouts, terminating the request if

-   The operation is started but the service receives no audio.
-   The service is receiving audio but it detects an extended period of silence.

For information about the different timeouts associated with audio transmission, see [Timeouts](#timeouts).

## Data limits and compression
{: #limits}

With either approach, streaming or one-shot delivery, the service imposes a size limit of 100 MB on the audio data that you can transcribe at one time. When recognizing long continuous audio streams or large files, you can take the following steps to ensure that your audio does not exceed the 100 MB limit:

-   Use a sampling rate no greater than 16 KHz (16,000 samples per second) and 16 bits per sample. Because the service converts audio recorded at higher sampling rates to 16 KHz prior to recognition, larger frequencies do not result in enhanced recognition accuracy.
-   Encode your audio with a format such as `audio/ogg;codecs=opus` or `audio/flac`, which offer far better data compression.

By encoding your data more efficiently, you can send far more audio without exceeding the 100 MB data limit. The following table reports the approximate size of the data stream that results from two hours of continuous speech transmission sampled at 16 KHz and at 16 bits per sample. Actual values can vary depending on the compression rate and the complexity of the audio.

<table>
  <caption>Table 9. Approximate audio stream sizes</caption>
  <tr>
    <th style="text-align:left">Audio format</th>
    <th style="text-align:left">Size of data stream</th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td>230 MB (well beyond the 100 MB limit)</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td>115 MB (slightly beyond the 100 MB limit)</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td>60 MB (below the 100 MB limit)</td>
  </tr>
  <tr>
    <td><code>audio/ogg;codecs=opus</code></td>
    <td>23 MB (well below the 100 MB limit)</td>
  </tr>
</table>

By using `audio/flac`, `audio/mp3`, or `audio/ogg;codecs=opus`, you can reduce significantly the size of your audio stream.

Note that if you compress the audio too severely with the `audio/ogg` format, you can lose some recognition accuracy; retaining at least 5 KB per second of audio is typically safe. Note also that `audio/ogg;codecs=opus` and `audio/webm;codecs=opus` are essentially equivalent, and their file sizes should be almost identical. They use the same codec internally; only the container format is different.

## Timeouts
{: #timeouts}

<table>
  <caption>Table 11. Specifying an inactivity timeout</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>inactivity_timeout</code> JSON parameter of
      <code>start</code> message
    </td>
    <td style="text-align:center">
      <code>inactivity_timeout</code> query parameter of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>inactivity_timeout</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>inactivity_timeout</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

To preserve resources when you stream audio data, the service enforces various timeouts. If one of these timeouts lapses, the service closes the connection.

-   A *session timeout* (HTTP status code 408) occurs when a client starts a session but the service receives no audio for 30 seconds. It also occurs when a session is active but no request is received from the client for 30 seconds. The latter condition occurs only if the service receives no data from the client for 30 seconds and it has not yet received the last chunk of data. If the client has sent all data, the service can take more than 30 seconds to generate a response; in this case, the request does not time out.

    For both WebSocket connections and HTTP sessions, you can keep a session active by sending any audio data, including just silence, before the 30-second session timeout occurs. (You must also set the `inactivity_timeout` parameter to `-1`, as described in the next bullet.) You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

    Ideally, you would establish a session just before you obtain audio for transcription and maintain it by sending audio at a rate that is close to real time. Your application should also recover gracefully from closed connections.
-   An *inactivity timeout* (HTTP status code 400) occurs when the service is receiving audio from the client but it detects silence (no speech) for 30 seconds. The service uses the inactivity timeout to ensure that a session remains active. The timeout is useful, for example, for terminating a session when a user simply walks away from a live microphone.

    You can override this timeout by specifying a different value for the `inactivity_timeout` parameter. Specify a value of `-1` to set the inactivity timeout to infinity.

To improve usability for long audio files, the service avoids HTTP REST inactivity timeouts by sending a space character every 20 seconds in the response JSON object to keep the connection alive as long as recognition is ongoing. The WebSocket interface is not subject to such platform timeouts.
