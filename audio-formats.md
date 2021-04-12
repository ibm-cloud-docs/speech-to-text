---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-04"

subcollection: speech-to-text

content-type: troubleshoot

---

{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}
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

# Supported audio formats
{: #audio-formats}

The {{site.data.keyword.speechtotextfull}} service can extract speech from audio in many formats.  Later sections of this topic can help you get the most from your use of the service. If you are unfamiliar with audio processing, start with [Audio terminology and characteristics](/docs/speech-to-text?topic=speech-to-text-audio-terminology) for information about audio concepts.
{: shortdesc}

## Audio formats
{: #audio-formats-list}

Table 1 provides a summary of the audio formats that the service supports.

-   *Audio format and compression* identifies each format and indicates its supported compression. You can send a maximum of 100 MB of audio to the service with a single synchronous HTTP or WebSocket request. By using a format that supports compression, you can reduce the size of your audio to maximize the amount of data that you can pass to the service. For more information, see [Data limits and compression](#audio-formats-limits).
-   *Content-type specification* indicates whether you must use the `Content-Type` header or equivalent parameter to specify the format (MIME type) of the audio that you send to the service. You can specify the audio format for any request, but that's not always necessary:
    -   For most formats, the content type is optional. You can omit the content type or specify `application/octet-stream` to have the service automatically detect the format.
    -   For others, the content type is required. These formats do not provide the information, such as the sampling rate, that the service needs to auto-detect their format.
-   The final columns identify additional *Required parameters* and *Optional parameters* for each format. The following sections provide more information about these parameters.

| Audio format<br>and compression | Content-type<br/>specification | Required<br/>parameters | Optional<br/>parameters |
|---------------------------------|:------------------------------:|:-----------------------:|:------------------:|
| [audio/alaw](#audio-formats-alaw)<br/>Lossy | Required | `rate={integer}` | None |
| [audio/basic](#audio-formats-basic)<br/>Lossy| Required| None| None |
| [audio/flac](#audio-formats-flac)<br/>Lossless| Optional | None | None |
| [audio/g729](#audio-formats-g729)<br/>Lossy | Optional | None | None |
| [audio/l16](#audio-formats-l16)<br/>None | Required | `rate={integer}` | `channels={integer}`<br/>`endianness=big-endian`<br/>`endianness=little-endian` |
| [audio/mp3](#audio-formats-mp3)<br/>[audio/mpeg](#audio-formats-mp3)<br/>Lossy | Optional | None | None |
| [audio/mulaw](#audio-formats-mulaw)<br/>Lossy | Required | `rate={integer}` | None |
| [audio/ogg](#audio-formats-ogg)<br/>Lossy | Optional | None | `codecs=opus`<br/>`codecs=vorbis` |
| [audio/wav](#audio-formats-wav)<br/>None, lossless,<br/>or lossy | Optional | None | None |
| [audio/webm](#audio-formats-webm)<br/>Lossy | Optional | None | `codecs=opus`<br/>`codecs=vorbis` |
{: caption="Table 1. Summary of supported audio formats"}

When you use the `curl` command to make a speech recognition request with the HTTP interfaces, you must either specify the audio format with the `Content-Type` header, specify `"Content-Type: application/octet-stream"`, or specify `"Content-Type:"`. If you omit the header entirely, `curl` uses a default value of `application/x-www-form-urlencoded`.
{: important}

### audio/alaw format
{: #audio-formats-alaw}

*A-law* (`audio/alaw`) is a single-channel, lossy audio format. It uses an algorithm that is similar to the u-law algorithm applied by the `audio/basic` and `audio/mulaw` formats, though the A-law algorithm produces different signal characteristics. When you use this format, the service requires an extra parameter on the format specification.

| Parameter | Description |
|-----------|-------------|
| `rate`<br/>*Required* | An integer that specifies the sampling rate at which the audio is captured. For example, specify the following parameter for audio data that is captured at 8 kHz:<br/><br/>`audio/alaw;rate=8000` |
{: caption="Table 2. Parameter for audio/alaw format"}

For more information, see [A-law algorithm](https://wikipedia.org/wiki/A-law_algorithm){: external}.

### audio/basic format
{: #audio-formats-basic}

*Basic audio* (`audio/basic`) is a single-channel, lossy audio format that is encoded by using 8-bit u-law (or mu-law) data that is sampled at 8 kHz. This format provides a lowest-common denominator for indicating the media type of audio. The service supports the use of files in `audio/basic` format only with narrowband models.

For more information, see the Internet Engineering Task Force (IETF) [Request for Comment (RFC) 2046](https://tools.ietf.org/html/rfc2046){: external} and [iana.org/assignments/media-types/audio/basic](http://www.iana.org/assignments/media-types/audio/basic){: external}.

### audio/flac format
{: #audio-formats-flac}

*Free Lossless Audio Codec (FLAC)* (`audio/flac`) is a lossless audio format. For more information, see [FLAC](https://wikipedia.org/wiki/FLAC){: external}.

### audio/g729 format
{: #audio-formats-g729}

*G.729* (`audio/g729`) is a lossy audio format that supports data that is encoded at 8 kHz. The service supports only G.729 Annex D, not Annex J. The service supports the use of files in `audio/g729` format only with narrowband models. For more information, see [G.729](https://wikipedia.org/wiki/G.729){: external}.

### audio/l16 format
{: #audio-formats-l16}

*Linear 16-bit Pulse-Code Modulation (PCM)* (`audio/l16`) is an uncompressed audio format. Use this format to pass a raw PCM file. Linear PCM audio can also be carried inside of a container Waveform Audio File Format (WAV) file. When you use the `audio/l16` format, the service accepts extra required and optional parameters on the format specification.

| Parameter | Description |
|-----------|-------------|
| `rate`<br/>*Required* | An integer that specifies the sampling rate at which the audio is captured. For example, specify the following parameter for audio data that is captured at 16 kHz:<br/><br/>`audio/l16;rate=16000` |
| `channels`<br/>*Optional* | By default, the service treats the audio as if it has a single channel. *If the audio has more than one channel,* you must specify an integer that identifies the number of channels. For example, specify the following parameter for two-channel audio data that is captured at 16 kHz:<br/><br/>`audio/l16;rate=16000;channels=2`<br/><br/>The service accepts a maximum of 16 channels. It downmixes the audio to one channel during transcoding. |
| `endianness`<br/>*Optional* | By default, the service auto-detects the endianness of incoming audio. But its auto-detection can sometimes fail and drop the connection for short audio in `audio/l16` format. Specifying the endianness disables auto-detection. Specify either `big-endian` or `little-endian`. For example, specify the following parameter for audio data that is captured at 16 kHz in little-endian format:<br/><br/>`audio/l16;rate=16000;endianness=little-endian`<br/><br/>Section 5.1 of [Request for Comment (RFC) 2045](https://tools.ietf.org/html/rfc2045#section-5.1) specifies big-endian format for `audio/l16` data, but many people use little-endian format. |
{: caption="Table 3. Parameters for audio/l16 format"}

For more information, see the IETF [Request for Comment (RFC) 2586](https://tools.ietf.org/html/rfc2586){: external} and [Pulse-code modulation](https://wikipedia.org/wiki/Pulse-code_modulation){: external}.

### audio/mp3 and audio/mpeg formats
{: #audio-formats-mp3}

*MP3* (`audio/mp3`) or *Motion Picture Experts Group (MPEG)* (`audio/mpeg`) is a lossy audio format. (MP3 and MPEG refer to the same format.) For more information, see [MP3](https://wikipedia.org/wiki/MP3){: external}.

### audio/mulaw format
{: #audio-formats-mulaw}

*Mu-law* (`audio/mulaw`) is a single-channel, lossy audio format. The data is encoded by using the u-law (or mu-law) algorithm. The `audio/basic` format is an equivalent format that is always sampled at 8 kHz. When you use this format, the service requires an extra parameter on the format specification.

| Parameter | Description |
|-----------|-------------|
| `rate`<br/>*Required* | An integer that specifies the sampling rate at which the audio is captured. For example, specify the following parameter for audio data that is captured at 8 kHz:<br/><br/>`audio/mulaw;rate=8000` |
{: caption="Table 4. Parameter for audio/mulaw format"}

For more information, see [M-law algorithm](https://wikipedia.org/wiki/M-law_algorithm){: external}.

### audio/ogg format
{: #audio-formats-ogg}

*Ogg* (`audio/ogg`) is an open container format that is maintained by the Xiph.org Foundation ([xiph.org/ogg](https://www.xiph.org/ogg){: external}). You can use audio streams that are compressed with the following lossy codecs:

-   *Opus* (`audio/ogg;codecs=opus`). For more information, see [opus-codec.org](https://www.opus-codec.org/){: external} and [Opus (audio format)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external}. Look especially at the *Containers* section.
-   *Vorbis* (`audio/ogg;codecs=vorbis`). For more information, see [xiph.org/vorbis](https://xiph.org/vorbis/){: external} and [Vorbis](https://wikipedia.org/wiki/Vorbis){: external}.

If you omit the codec, the service automatically detects it from the input audio. Opus is the preferred codec; it is standardized by the Internet Engineering Task Force (IETF) as [Request for Comment (RFC) 6716](https://tools.ietf.org/html/rfc6716){: external}.

### audio/wav format
{: #audio-formats-wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) is a container format that is often used for uncompressed audio streams, but it can contain compressed audio, as well. The service supports WAV audio that uses any encoding. It accepts WAV audio with a maximum of nine channels (due to an FFmpeg limitation).

For more information about the WAV format, see [WAV](https://wikipedia.org/wiki/WAV){: external}. For more information about reducing the size of WAV audio by converting it to the Opus codec, see [Converting to audio/ogg with the Opus codec](#audio-formats-conversion-ogg).

### audio/webm format
{: #audio-formats-webm}

*Web Media (WebM)* (`audio/webm`) is an open container format that is maintained by the WebM project ([webmproject.org](https://www.webmproject.org/){: external}). You can use audio streams that are compressed with the following lossy codecs:

-   *Opus* (`audio/webm;codecs=opus`). For more information, see [opus-codec.org](https://www.opus-codec.org/){: external} and [Opus (audio format)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external}. Look especially at the *Containers* section.
-   *Vorbis* (`audio/webm;codecs=vorbis`). For more information, see [xiph.org/vorbis](https://xiph.org/vorbis/){: external} and [Vorbis](https://wikipedia.org/wiki/Vorbis){: external}.

If you omit the codec, the service automatically detects it from the input audio.

For JavaScript code that shows how to capture audio from a microphone in a Chrome browser and encode it into a WebM data stream, see [jsbin.com/hedujihuqo/edit?js,console](https://jsbin.com/hedujihuqo/edit?js,console){: external}. The code does not submit the captured audio to the service.
{: tip}

## Data limits and compression
{: #audio-formats-limits}

The service accepts a maximum of 100 MB of audio data for transcription with a synchronous HTTP or WebSocket request. When you recognize long continuous audio streams or large files, take the following steps to ensure that your audio does not exceed the 100 MB data limit:

-   Use a sampling rate no greater than 16 kHz (for broadband models) or 8 kHz (for narrowband models), and use 16 bits per sample. The service converts audio recorded at a sampling rate that is higher than the target model (16 kHz or 8 kHz) to the rate of the model. So larger frequencies do not result in enhanced recognition accuracy, but they do increase the size of the audio stream.
-   Encode your audio in a format that offers data compression. By encoding your data more efficiently, you can send far more audio without exceeding the 100 MB limit. Audio formats such as `audio/ogg` and `audio/mp3` significantly reduce the size of your audio stream. You can use these formats to send greater amounts of audio with a single request.

If you compress the audio too severely with the `audio/ogg` format, you can lose some recognition accuracy. To be safe, retain a bit rate of at least 24 kbps for your audio.

### Comparing approximate audio sizes
{: #audio-formats-compare-sizes}

Consider the approximate size of the data stream that results from 2 hours of continuous speech transmission that is sampled at 16 kHz and at 16 bits per sample:

-   If the data is encoded with the `audio/wav` format, the two-hour stream has a size of 230 MB, well beyond the service's 100 MB limit.
-   If the data is encoded in the `audio/ogg` format, the two-hour stream has a size of only 23 MB, well beneath the service's limit.

The following table approximates the maximum duration of audio that can be sent for speech recognition with a synchronous HTTP or WebSocket request in different formats. The duration considers the 100 MB service limit. Actual values can vary depending on the complexity of the audio and the achieved compression rate.

| Audio format | Maximum duration of audio (approximate) |
|--------------|:---------------------------------------:|
| `audio/wav` | 55 minutes |
| `audio/flac` | 1 hour 40 minutes |
| `audio/mp3` | 3 hours 20 minutes |
| `audio/ogg` | 8 hours 40 minutes |
{: caption="Table 5. Maximum duration of audio in different formats"}

The `audio/ogg;codecs=opus` and `audio/webm;codecs=opus` formats are generally equivalent, and their sizes are almost identical. They use the same codec internally; only the container format is different.

## Audio conversion
{: #audio-formats-conversion}

You can use various tools to convert your audio to a different format. The tools can be helpful when your audio is in a format that is not supported by the service or is in an uncompressed or lossless format. In the latter case, you can convert the audio to a lossy format to reduce its size.

The following freeware tools are available to convert your audio from one format to another:

-   Sound eXchange (SoX) ([sox.sourceforge.net](http://sox.sourceforge.net){: external}).
-   FFmpeg ([ffmpeg.org](https://www.ffmpeg.org){: external}). You can also use FFmpeg to separate audio from a multimedia file that contains both audio and video data. For more information, see [Transcribing speech from video files](#audio-formats-video).
-   Audacity&reg; ([audacityteam.org](http://www.audacityteam.org/){: external}).
-   For Ogg format with the Opus codec, **opus-tools** ([opus-codec.org](https://opus-codec.org/){: external}).

These tools offer cross-platform support for multiple audio formats. Moreover, you can use many of the tools to play your audio. Do not use the tools to violate applicable copyright laws.

### Converting to audio/ogg with the Opus codec
{: #audio-formats-conversion-ogg}

The [**opus-tools**](https://opus-codec.org/release/dev/2018/09/18/opus-tools-0_2.html){: external} include three command-line utilities for working with Ogg audio in the Opus codec:

-   The [**opusenc**](https://opus-codec.org/docs/opus-tools/opusenc.html){: external} utility encodes audio from WAV, FLAC, and other formats to Ogg with the Opus codec. The page shows how to compress audio streams. Compression is useful for passing real-time audio to the service.
-   The [**opusdec**](https://opus-codec.org/docs/opus-tools/opusdec.html){: external} utility decodes audio from the Opus codec to uncompressed PCM WAV files.
-   The [**opusinfo**](https://opus-codec.org/docs/opus-tools/opusinfo.html){: external} utility provides information about and validity checking for Opus files.

Many users send WAV files for speech recognition. With the service's 100 MB data limit for synchronous HTTP and WebSocket requests, the WAV format reduces the amount of audio that can be recognized with a single request. Using the **opusenc** command to convert the audio to the preferred `audio/ogg:codecs=opus` format can greatly increase the amount of audio that you can send with a recognition request.

For example, consider an uncompressed broadband (16 kHz) WAV file (**input.wav**) that uses 16 bits per sample for a bit rate of 256 kbps. The following command converts the audio to a file (**output.opus**) that uses the Opus codec:

```bash
opusenc input.wav output.opus
```
{: pre}

The conversion compresses the audio by a factor of four and produces an output file with a bit rate of 64 kbps. However, according to the [Opus Recommended Settings](https://wiki.xiph.org/Opus_Recommended_Settings){: external}, you can safely reduce the bit rate to 24 kbps and still retain a full band for speech audio. This reduction compresses the audio by a factor of 10. The following command uses the `--bitrate` option to produce an output file with a bit rate of 24 kbps:

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

Compression with the **opusenc** utility is fast. Compression happens at a rate that is approximately 100 times faster than real time. When it finishes, the command writes output to the console that provides complete details about its running time and the resulting audio data.

## Transcribing speech from video files
{: #audio-formats-video}

You cannot transcribe speech from a multimedia file that contains both audio and video. The service accepts only audio data for speech recognition.

To transcribe the speech from a multimedia file that contains audio and video, you must separate the audio data from the video data. You can use the FFmpeg utility to separate the audio from the video source. For more information, see [ffmpeg.org](https://www.ffmpeg.org){: external}.

## Tips for improving speech recognition
{: #audio-formats-tips}
{: troubleshoot}
{: support}

The following tips can help you improve the quality of speech recognition:

-   How you record audio can make a big difference in the service's results. Speech recognition can be very sensitive to input audio quality. To obtain the best possible accuracy, ensure that the input audio quality is as good as possible.
    -   Use a close, speech-oriented microphone (such as a headset) whenever possible and adjust the microphone settings if necessary. The service performs best when professional microphones are used to capture audio.
    -   Avoid using a system's built-in microphone. The microphones that are typically installed on mobile devices and tablets are often inadequate.
    -   Ensure that speakers are close to microphones. Accuracy declines as a speaker moves farther from a microphone. At a distance of 10 feet, for example, the service struggles to produce adequate results.
-   Speech recognition is sensitive to background noise and nuances of human speech.
    -   Engine noise, working devices, street noise, and background conversations can significantly reduce recognition accuracy.
    -   Regional accents and differences in pronunciation can also reduce accuracy.

    If your audio has these characteristics, consider using acoustic model customization to improve the accuracy of speech recognition. For more information, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
