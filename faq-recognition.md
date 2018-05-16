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

# Recognition accuracy
{: #accuracy}

1.  <span style="color:#003F69">Which model, broadband or narrowband, do I use with my audio to achieve the best recognition accuracy?</span>

    You need to consider the frequency content of the audio. Use the model that matches the sampling rate (and language) of the audio. For descriptions of the models and their minimum sampling rates, see [Languages and models](/docs/services/speech-to-text/input.html#models).

    In general, if the sampling rate is 8 kHz or less, use a narrowband model. Otherwise, use a broadband model. Moreover, you might be better off downsampling the audio to 8 kHz if

    -   The sampling rate is greater than 8 kHz.
    -   A spectrographic examination reveals no frequency content that is more than 4 kHz.

    If this explanation is unclear, post a question to the {{site.data.keyword.watson}} forums on [StackOverflow ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://stackoverflow.com/questions/tagged/ibm-watson-cognitive){: new_window} or [dW Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/speech-to-text/){: new_window}.

1.  <span style="color:#003F69">To improve recognition accuracy, I upsampled my audio file and sent it to the broadband model, but the results are not what I expected. Why?</span>

    Upsampling is unlikely to help and might degrade performance.

    -   The narrowband models are built with audio that is sampled at 8 kHz.
    -   The broadband models are built with audio that is sampled at 16 kHz.

    A sampling rate of 16 kHz means that the maximum frequency present in the sampled signal is 8kHz. Therefore, you must filter the original signal at 8 kHz before you sample it with a rate of 16 kHz. Otherwise, degradation occurs due to the phenomenon known as *aliasing*. (To understand why, see [Nyquist frequency ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}). The broadband model expects to find information in the 4 - 8 kHz range. The narrowband model expects to find information in a range that is less than or equal to 4 kHz.

    The information in audio that is originally sampled at a narrowband frequency is limited to the 0 - 4 kHz range. The upsampled audio lacks information in the range that is expected by the broadband models, which results in degraded recognition accuracy. Furthermore, the information that is found in the expected range of a narrowband sample is qualitatively different from the information that is found in the same range of a broadband sample. The training data for the models is derived from different channels (telephony for narrowband models). The models reflect the characteristics of the channels on which they were trained.

    A useful comparison might be to imagine viewing a VHS tape on a large flat-screen HDTV. The image is blurry because playing the tape on a high-definition device cannot really add new information to the stream. It simply makes the format compatible with the better device. The same is true of upsampling audio.

1.  <span style="color:#003F69">My application depends on knowing whether a speaker replies *yes* or *no* to a question. However, the transcription does not always return *yes* or *no* but can include other words that sound similar (such as *nine* when *no* is expected). How can I improve the recognition in such cases?</span>

    You can often find the expected word by examining alternative transcription results and words. These sources can include words that are possible alternatives to the final results but for some reason were not selected as the best choice by the service. You can see such alternatives in one of two ways:
    -   Specify an integer for the `max_alternatives` parameter. For example, specify a value of `5` to see that number of alternative transcriptions. See [Maximum alternatives](/docs/services/speech-to-text/output.html#max_alternatives).
    -   Specify a probability for the `word_alternatives_threshold` parameter. For example, specify a value of `0.5` to see possible alternative words for which the service has a minimum confidence of 50 percent. See [Word alternatives](/docs/services/speech-to-text/output.html#word_alternatives).

    In general, the service is sensitive to background noise. For instance, engine noise, working devices, street noise, and talking can significantly reduce accuracy. In addition, the microphones that are typically installed on mobile devices and tablets are often inadequate. The service performs best when professional microphones are used to capture audio with better quality. Furthermore, the service's accuracy declines as the speaker moves farther from the microphone. For example, at a distance of 10 feet, the service struggles to produce any adequate results.
