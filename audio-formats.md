---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-03"

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Supported audio formats
{: #audio-formats}

The {{site.data.keyword.speechtotextfull}} service can extract speech from audio in many formats.  Later sections of this topic can help you get the most from your use of the service. If you are unfamiliar with audio processing, start with [Audio terminology and characteristics](/docs/speech-to-text?topic=speech-to-text-audio-terminology) for information about audio concepts.
{: shortdesc}

## Audio formats
{: #audio-formats-list}

Table 1 provides a summary of the audio formats that the service supports.

-   *Audio format* identifies each supported format by its `Content-Type` specification.
-   *Compression* indicates the format's support for compression. By using a format that supports compression, you can reduce the size of your audio to maximize the amount of data that you can pass to the service. But you need to consider the possible effects of compression on the quality of the audio. For more information, see [Data limits and compression](#audio-formats-limits).
-   *Content-type specification* indicates whether you must use the `Content-Type` header or equivalent parameter to specify the format (MIME type) of the audio that you send to the service. For more information, see [Specifying an audio format](#audio-formats-specifying).

The final columns identify additional *Required parameters* and *Optional parameters* for each format. The following sections provide more information about these parameters.

| <br/>Audio format | <br/>Compression | Content-type<br/>specification | Required<br/>parameters | Optional<br/>parameters |
|-------------------|:----------------:|:------------------------------:|:-----------------------:|:------------------:|
| [audio/alaw](#audio-formats-alaw) | Lossy | Required | `rate={integer}` | None |
| [audio/basic](#audio-formats-basic) | Lossy| Required| None| None |
| [audio/flac](#audio-formats-flac) | Lossless| Optional | None | None |
| [audio/g729](#audio-formats-g729) | Lossy | Optional | None | None |
| [audio/l16](#audio-formats-l16) | None | Required | `rate={integer}` | `channels={integer}`<br/>`endianness=big-endian`<br/>`endianness=little-endian` |
| [audio/mp3](#audio-formats-mp3)<br/>[audio/mpeg](#audio-formats-mp3) | Lossy | Optional | None | None |
| [audio/mulaw](#audio-formats-mulaw) | Lossy | Required | `rate={integer}` | None |
| [audio/ogg](#audio-formats-ogg) | Lossy | Optional | None | `codecs=opus`<br/>`codecs=vorbis` |
| [audio/wav](#audio-formats-wav) | None, lossless, or lossy | Optional | None | None |
| [audio/webm](#audio-formats-webm) | Lossy | Optional | None | `codecs=opus`<br/>`codecs=vorbis` |
{: caption="Table 1. Summary of supported audio formats"}

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

OGG Opus is the preferred codec. It is the logical successor to OGG Vorbis because of its low latency, high audio quality, and reduced size. It is standardized by the Internet Engineering Task Force (IETF) as [Request for Comment (RFC) 6716](https://tools.ietf.org/html/rfc6716){: external}.

If you omit the codec from the content type, the service automatically detects it from the input audio.

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

## Specifying an audio format
{: #audio-formats-specifying}

You use the `Content-Type` request header or equivalent parameter to specify the format (MIME type) of the audio that you send to the service. You can specify the audio format for any request, but that's not always necessary:

-   For most formats, the content type is optional. You can omit the content type or specify `application/octet-stream` to have the service automatically detect the format.
-   For other formats, the content type is required. These formats do not provide the information, such as the sampling rate, that the service needs to auto-detect their format. You must specify a content type for the `audio/alaw`, `audio/basic`, `audio/l16`, and `audio/mulaw` formats.

For more information about the formats that require a content-type specification, see Table 1 in [Audio formats](#audio-formats-list). The table's *Content-type specification* column indicates whether you must specify the content type.

For examples of specifying a content type with each of the service's interfaces, see [Making a speech recognition request](/docs/speech-to-text?topic=speech-to-text-basic-request). All of the examples in that topic specify a content type.

When you use the `curl` command to make a speech recognition request with the HTTP interfaces, you must either specify the audio format with the `Content-Type` header, specify `"Content-Type: application/octet-stream"`, or specify just `"Content-Type:"`. If you omit the header entirely, `curl` uses a default value of `application/x-www-form-urlencoded`.
{: important}

## Data limits and compression
{: #audio-formats-limits}

The service accepts a maximum of 100 MB of audio data for transcription with a synchronous HTTP or WebSocket request, and 1 GB of audio data with an asynchronous HTTP request. When you recognize long continuous audio streams or large audio files, you need to consider and accommodate these data limits.

One way to maximize the amount of audio data that you can pass with a speech recognition request is to use a format that offers [Compression](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-compression). There are two basic types of compression, lossy and lossless. The audio format and compression algorithm that you choose can have a direct impact on the accuracy of speech recognition.

Audio formats that use lossy compression significantly reduce the size of your audio stream. But compressing the audio too severely can result in lower transcription accuracy. You can't hear the difference, but the service is much more sensitive to such data loss.

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

In testing to compare the different audio formats, {{site.data.keyword.IBM}} determined that the WAV and FLAC formats delivered the best word error rate (WER). These formats can serve as a baseline for transcription accuracy because they maintain the audio intact with no data loss. The Ogg format with the Opus coded showed a slight degradation of 2% WER relative to the baseline. The MP3 format delivered the worst results, with a 10% degradation of WER relative to the baseline.

The `audio/ogg;codecs=opus` and `audio/webm;codecs=opus` formats are generally equivalent, and their sizes are almost identical. They use the same codec internally; only the container format is different.
{: note}

### Maximizing transcription accuracy
{: #audio-formats-recommendations}

When choosing an audio format and compression algorithm, consider the following recommendations to maximize transcription accuracy:

-   *Use an uncompressed and lossless audio format.* If the duration of your audio is less than 55 minutes (less than 100 MB), consider using the `audio/wav` format. Although the WAV format can accommodate only 55 minutes of audio, that is often sufficient for most transcription applications, such as customer support calls. And uncompressed WAV audio can produce more accurate transcription.
-   *Use the asynchronous HTTP interface.* If you elect to use the WAV format but your audio exceeds the 100 MB limit, the asynchronous interface allows you to send up to 1 GB of data.
-   *Use a compressed but lossless audio format.* If you have to compress your audio file, use the `audio/flac` format, which employs lossless compression. Lossless compression reduces the size of the audio but maintains its quality. The FLAC format is a good candidate for maximizing transcription accuracy.
-   *Use lossy compression as a last resort.* If you need even greater compression, use the `audio/ogg` format with the Opus codec. Although the Ogg format uses lossy compression, the combination of the Ogg format with the Opus codec showed the least degradation in speech accuracy among lossy compression algorithms.

Using other formats with greater levels of compression can compromise the accuracy of transcription. Experiment with the service to determine which format is best for your audio and application. For more ways to improve speech recognition, see [Tips for improving speech recognition](#audio-formats-tips).

## Audio conversion
{: #audio-formats-conversion}

You can use various tools to convert your audio to a different format. The tools can be helpful when your audio is in a format that is not supported by the service or is in an uncompressed or lossless format. In the latter case, you can convert the audio to a lossy format to reduce its size.

The following freeware tools are available to convert your audio from one format to another:

-   Sound eXchange (SoX) ([sox.sourceforge.net](http://sox.sourceforge.net){: external}).
-   FFmpeg ([ffmpeg.org](https://www.ffmpeg.org){: external}). You can also use FFmpeg to separate audio from a multimedia file that contains both audio and video data. For more information, see [Transcribing speech from video files](#audio-formats-video).
-   Audacity&reg; ([audacityteam.org](https://www.audacityteam.org/){: external}).
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
-   Use a sampling rate no greater than 16 kHz (for broadband models) or 8 kHz (for narrowband models), and use 16 bits per sample. The service converts audio recorded at a sampling rate that is higher than the target model (16 kHz or 8 kHz) to the rate of the model. So larger frequencies do not result in enhanced recognition accuracy, but they do increase the size of the audio stream.
-   Encode your audio in a format that offers data compression. By encoding your data more efficiently, you can send far more audio without exceeding the 100 MB limit. Because the dynamic range of the human voice is more limited than, say, music, speech can accommodate a bit rate that is lower than other types of audio. Nonetheless, {{site.data.keyword.IBM}} recommends that you choose an audio format and compression algorithm carefully. For more information, see [Maximizing transcription accuracy](#audio-formats-recommendations).
-   Speech recognition is sensitive to background noise and nuances of human speech.
    -   Engine noise, working devices, street noise, and background conversations can significantly reduce recognition accuracy.
    -   Regional accents and differences in pronunciation can also reduce accuracy.

    If your audio has these characteristics, consider using acoustic model customization to improve the accuracy of speech recognition. For more information, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
-   To learn more about the characteristics of your audio, consider using audio metrics with your speech recognition request. If you are knowledgeable about audio-signal processing, the metrics can provide meaningful and detailed insight into your audio characteristics. For more information, see [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio-metrics).
