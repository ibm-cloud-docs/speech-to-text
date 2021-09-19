---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-03"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Audio terminology and characteristics
{: #audio-terminology}

The following terminology is used to describe the characteristics of audio data and its processing. This information is helpful for using your audio with the {{site.data.keyword.speechtotextfull}} service.
{: shortdesc}

-   If you are unfamiliar with audio and how it is described and specified, begin with this topic to help you get started.
-   If you already understand how to work with audio data, start with [Supported audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats).

## Sampling rate
{: #audio-terminology-sampling-rate}

*Sampling rate* (or sampling frequency) is the number of audio samples that are taken per second. Sampling rate is measured in Hertz (Hz) or kilohertz (kHz). For example, a rate of 16,000 samples per second is equal to 16,000 Hz (or 16 kHz). With the {{site.data.keyword.speechtotextshort}} service, you specify a model to indicate the sampling rate of your audio:

-   *Broadband* and *multimedia* models are used for audio that is sampled at no less than 16 kHz, which {{site.data.keyword.IBM}} recommends for responsive, real-time applications (for example, for live-speech applications).
-   *Narrowband* and *telephony* models are used for audio that is sampled at no less than 8 kHz, which is the rate that is typically used for telephonic audio.

The service supports both sampling rates for most languages and formats. It automatically adjusts the sampling rate of your audio to match the model that you specify before it recognizes speech.

-   For broadband and multimedia models, the service converts audio recorded at higher sampling rates to 16 kHz.
-   For narrowband and telephony models, it converts audio recorded at higher sampling rates to 8 kHz.

You can, for instance, send 44 kHz audio with any model, but that needlessly increases the size of the audio. To maximize the amount of audio that you can send, match the sampling rate of your audio to the model that you use.

The service does not accept audio that is sampled at a rate that is less than the sampling rate of the model. For example, you cannot use a broadband or multimedia model to recognize audio that is sampled at a rate of 8 kHz.

### Notes about audio formats
{: #audio-terminology-sampling-rate-notes}

-   For the `audio/alaw`, `audio/l16`, and `audio/mulaw` formats, you must specify the rate of your audio.
-   For the `audio/basic` and `audio/g729` formats, the service supports only narrowband audio.

### More information
{: #audio-terminology-sampling-rate-more}

-   For more information about sampling rates, see [Sampling (signal processing)](https://wikipedia.org/wiki/Sampling_%28signal_processing%29){: external}.
-   For more information about the models that the service offers for each supported language, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models) and [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

## Bit rate
{: #audio-terminology-bit-rate}

*Bit rate* is the number of bits of data that is sent per second. The bit rate for an audio stream is measured in kilobits per second (kbps). The bit rate is calculated from the sampling rate and the number of bits stored per sample. For speech recognition, {{site.data.keyword.IBM}} recommends that you record 16 bits per sample for your audio.

For example, audio that uses a broadband sampling rate of 16 kHz and 16 bits per sample has a bit rate of 256 kbps: `(16,000 * 16) / 1000`.

### More information
{: #audio-terminology-bit-rate-more}

-   For more information about bit rates, see [Bit rate](https://wikipedia.org/wiki/Bit_rate){: external}.
-   For a general discussion of sampling rates and bit rates, see [What are bit rates?](http://www.richardfarrar.com/what-are-bit-rates/){: external} and [Choosing bit rates for podcasts](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: external}.

## Compression
{: #audio-terminology-compression}

*Compression* is used by many audio formats to reduce the size of the audio data. Compression reduces the number of bits stored per sample and thus the bit rate. Some formats use no compression, but most offer one of the two basic types:

-   *Lossless* compression reduces the size of the audio with no loss of quality, but the compression ratio is typically small.
-   *Lossy* compression reduces the size of the audio by as much as 10 times, but some data and some quality is irretrievably lost in the compression.

You can use compression to accommodate more audio data with your speech recognition request. But the type of compression that you use has implications for transcription quality.

### Notes about audio formats
{: #audio-terminology-compression-notes}

-   The `audio/ogg` and `audio/webm` formats are containers whose compression relies on the codec that you use to encode the data: Opus or Vorbis.
-   The `audio/wav` format is a container that can include uncompressed, lossless, or lossy data.

### More information
{: #audio-terminology-compression-more}

-   For more information about audio compression, see [Data compression (Audio)](https://wikipedia.org/wiki/Data_compression#Audio){: external}.
-   For more information about the compression that is available with the audio formats that the service supports, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list).
-   For more information about using data compression to increase the amount of audio that you can send with a request, see [Data limits and compression](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-limits).

## Channels
{: #audio-terminology-channels}

*Channels* indicate the number of streams of the recorded audio:

-   *Monaural* (or mono) audio has only a single channel.
-   *Stereophonic* (or stereo) audio typically has two channels.

The {{site.data.keyword.speechtotextshort}} service accepts audio with a maximum of 16 channels. Because it uses only a single channel for speech recognition, the service downmixes audio with multiple channels to one-channel mono during transcoding.

### Notes about audio formats
{: #audio-terminology-channels-notes}

-   For the `audio/l16` format, you must specify the number of channels if your audio has more than one channel.
-   For the `audio/wav` format, the service accepts audio with a maximum of nine channels.

### More information
{: #audio-terminology-channels-more}

-   For more information about audio channels, see [Audio signal](https://wikipedia.org/wiki/Audio_signal){: external}.

## Endianness
{: #audio-terminology-endianness}

*Endianness* indicates how bytes of data are organized by the underlying computer architecture:

-   *Big-endian* orders data by most-significant bit.
-   *Little-endian* orders data by least-significant bit.

The {{site.data.keyword.speechtotextshort}} service automatically detects the endianness of the incoming audio.

### Notes about audio formats
{: #audio-terminology-endianness-notes}

-   For the `audio/l16` format, you can specify the endianness to disable auto-detection if needed.

### More information
{: #audio-terminology-endianness-more}

-   For more information about endianness, see [Endianness](https://wikipedia.org/wiki/Endianness){: external}.

## Audio frequency
{: #audio-terminology-frequency}

*Audio frequency* refers to the range of audible frequencies in the audio. The standard audible frequency for humans is generally accepted as 20 to 20,000 Hz. You can use spectrographic analysis to produce a spectrogram that reveals the frequency content of your audio.

The sampling rate that is applied to audio is typically twice the maximum frequency of the audio. For example, a sampling rate of 16 kHz means that the maximum frequency of the sampled audio signal is 8 kHz. The service's models are created with this is mind.

-   The narrowband models are built with audio that is sampled at 8 kHz. Narrowband models expect to find information in a range that is less than or equal to 4 kHz.
-   The broadband models are built with audio that is sampled at 16 kHz. Broadband models expect to find information in the 4-8 kHz range.

The training data for the models is derived from different channels (telephony for narrowband models). The models reflect the characteristics of the channels on which they were trained.

### Upsampling
{: #audio-terminology-upsampling}

*Upsampling* increases the sampling rate of the audio but introduces no new information into the audio. It produces an approximation of the audio signal that would have been obtained by sampling the audio at a higher rate. It increases the size of the audio data.

The information in audio that is originally sampled at a narrowband frequency is limited to the 0-4 kHz range. Upsampling narrowband audio to a higher sampling rate is unlikely to improve speech recognition accuracy. If you upsample narrowband audio, it lacks information in the range that the broadband models expect. Furthermore, the information that is found in the expected range of a narrowband sample is qualitatively different from the information that is found in the same range of a broadband sample. So upsampling actually results in degraded recognition accuracy.

For a broadband sampling rate of 16 kHz, the maximum frequency present in the sampled audio signal is expected to be 8 kHz. Therefore, you must filter the original signal at 8 kHz before you sample it with a rate of 16 kHz. Otherwise, degradation occurs due to the phenomenon known as *aliasing*. To understand why, see [Nyquist frequency](https://wikipedia.org/wiki/Nyquist_frequency){: external}.

A useful comparison might be to imagine viewing a VHS tape on a large flat-screen HDTV. The image is blurry because playing the tape on a high-definition device cannot really add new information to the stream. It simply makes the format compatible with the better device. The same is true of upsampling audio.

### Downsampling
{: #audio-terminology-downsampling}

*Downsampling* decreases the sampling rate of the audio. It produces an approximation of the audio signal that would have been obtained by sampling the audio at a lower rate. Downsampling removes no information from the audio signal, but it does reduce the size of the audio data.

Downsampling your audio can be effective in some cases. For example, if the sampling rate of your audio is greater than 8 kHz *and* a spectrographic examination reveals no frequency content that is greater than 4 kHz, consider downsampling the audio to 8 kHz.

### More information
{: #audio-terminology-frequency-more}

-   For more information about audio frequency, see [Audio frequency](https://wikipedia.org/wiki/Audio_frequency){: external}.
-   For more information about upsampling, see [Upsampling](https://wikipedia.org/wiki/Upsampling){: external}.
-   For more information about downsampling, see [Downsampling](https://wikipedia.org/wiki/Downsampling_%28signal_processing%29){: external}.
