---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-11"

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

# FAQs
{: #faq}

The following frequently asked questions (FAQs) address issues commonly associated with using the {{site.data.keyword.speechtotextshort}} service. The questions are grouped by topic. Some answers include links to the documentation for more information.
{: shortdesc}

## Basic usage
{: #basic}

1.  <span style="color:#003F69">My audio file is larger than 100 MB, which is too big for the service to accept. Is there a way that I can process the file?</span>

    One solution is to manually divide the file into smaller pieces. Alternatively, although you risk degrading the quality of the audio, you can compress the file by using tools such as Sound eXchange ([sox.sourceforge.net ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://sox.sourceforge.net){: new_window}) or FFmpeg ([www.ffmpeg.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ffmpeg.org){: new_window}). For information about encoding your audio efficiently, see [Data limits and compression](/docs/services/speech-to-text/input.html#limits).

1.  <span style="color:#003F69">I passed a one-hour long audio file to the service but did not see results until the full transcription completed. How can I see my results sooner?</span>

    Use HTTP sessions or the WebSocket interface, and set the `interim_results` parameter to `true`. By default, the service returns results only when it detects long silences in the audio. See [Interim results](/docs/services/speech-to-text/output.html#interim).

1.  <span style="color:#003F69">My request times out with an HTTP status code of 400 and the message `No speech detected for 30s`. What can I do?</span>

    By default, the service responds with an inactivity timeout if it detects 30 seconds of continuous silence or non-speech activity at any time. To prevent the timeout, set the `inactivity_timeout` parameter for your request to a value greater than 30 seconds; set the parameter to `-1` to specify an inactivity timeout of infinity. See [Timeouts](/docs/services/speech-to-text/input.html#timeouts).

1.  <span style="color:#003F69">My audio includes pauses of varying lengths. How does the service respond to them?</span>

    The service transcribes an entire audio stream until either the stream ends or a timeout occurs, whichever comes first. If your input includes pauses, transcription results can include multiple `transcript` elements to indicate phrases separated by the pauses. Concatenate the `transcript` elements to assemble the complete transcription of the audio stream.

    All methods that initiate recognition requests previously included a `continuous` parameter that you could set to `true` to avoid having the service stop transcription at the first end of speech incident, typically silence. The service no longer stops transcription at pauses, and the parameter has been removed from all methods.

1.  <span style="color:#003F69">Can I change the time or pause interval that determines when the end of a phrase occurs?</span>

    No. This capability is not currently available with the service.

1.  <span style="color:#003F69">My output includes multiple transcriptions of my content. Which one should I use?</span>

    Use the transcription for which the JSON `final` attribute is `true`; ignore the intermediate results for which the attribute is `false`.

1.  <span style="color:#003F69">What is {{site.data.keyword.IBM_notm}}'s data-storage policy for audio that I submit to the service?</span>

    By default, {{site.data.keyword.IBM_notm}} logs each request to the service and its results; {{site.data.keyword.IBM_notm}} uses the data only to improve the service's base speech models for future users. You can prevent {{site.data.keyword.IBM_notm}} from storing your data by setting the `X-Watson-Learning-Opt-Out` request header to `true` for all requests. When you use the header to opt out of request logging, {{site.data.keyword.IBM_notm}} stores none of your data; your data exists within {{site.data.keyword.watson}} only while it is in transit (while the service is processing your request). In either case, your data is always encrypted both in motion and at rest. For more information, see [Authentication tokens and request logging](/docs/services/speech-to-text/input.html#common).

## HTTP sessions
{: #sessions}

1.  <span style="color:#003F69">Why would I use sessions?</span>

    The use of sessions is encouraged to send multiple requests to the same server (transcription engine). One advantage of using sessions is reduced latency on subsequent calls because a new connection does not need to be initialized for each call. Sessions further reduce latency if you use custom models. Using the same server can also improve the accuracy of subsequent recognition requests. You can realize these same advantages by using the WebSocket interface.

1.  <span style="color:#003F69">How do I use cookies with sessions?</span>

    When you use session-based methods, the initial `POST sessions` request to create the session returns a cookie in the `Set-Cookie` response header. You must return this cookie with each call that uses that session. For more information, see [Using cookies with sessions](/docs/services/speech-to-text/http.html#cookies).

1.  <span style="color:#003F69">How do I diagnose request failures related to the use of cookies with sessions?</span>

    Two common errors associated with the use of cookies are forgetting to set the cookie on a session-based request and passing an invalid cookie. If you neglect to pass the cookie, the service returns HTTP status code 400 with the message `Cookie must be set.` If you pass an invalid cookie, the service returns HTTP status code 404 with the message `Session does not exist.` To help you diagnose the second type of error, [Using cookies with sessions](/docs/services/speech-to-text/http.html#cookies) describes the session cookie and how to ensure that it matches the session ID.

## WebSocket API
{: #websockets}

1.  <span style="color:#003F69">If I use the WebSocket API, do I need to use sessions?</span>

    No. When you use the WebSocket interface, the WebSocket is the connection to the service, and the connection is the session. A WebSocket connection provides all of the advantages of sessions and more. For example, it provides a single-socket, full-duplex communication channel; the client sends requests to the service and receives results over a single connection in an asynchronous fashion. In addition, the WebSocket protocol is very lightweight, which further reduces network latency. See [Using the WebSocket interface](/docs/services/speech-to-text/websockets.html).

1.  <span style="color:#003F69">My WebSocket connection times out before I receive the final hypothesis. What can I do?</span>

    Make sure that you signal the end of the audio by sending either a `stop` message or an empty binary message to the service. See [Ending a recognition request](/docs/services/speech-to-text/websockets.html#WSstop).

## Recognition accuracy
{: #accuracy}

1.  <span style="color:#003F69">Which model, broadband or narrowband, should I use with my audio to achieve the best recognition accuracy?</span>

    You need to consider the frequency content of the audio. Use the model that matches the sampling rate (and language) of the audio. For descriptions of the models and their minimum sampling rates, see [Languages and models](/docs/services/speech-to-text/input.html#models).

    In general, if the sampling rate is 8 KHz or lower, use a narrowband model; otherwise, use a broadband model. Moreover, if the sampling rate is greater than 8 KHz but a spectrographic examination reveals no frequency content above 4 KHz, you may be better off downsampling to 8 KHz. If this explanation is unclear, please post a question to the {{site.data.keyword.watson}} forums on [StackOverflow ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://stackoverflow.com/questions/tagged/ibm-watson-cognitive){: new_window} or [dW Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/speech-to-text/){: new_window}.

1.  <span style="color:#003F69">To improve recognition accuracy, I upsampled my audio file and sent it to the broadband model, but the results are not what I expected. Why?</span>

    Upsampling is unlikely to help and might actually degrade performance. The narrowband models are built with audio sampled at 8 KHz, while the broadband models are built with audio sampled at 16 KHz. A sampling rate of 16 KHz means that the maximum frequency present in the sampled signal is 8kHz. In other words, the original signal has to be filtered at 8 KHz before sampling it with a rate of 16 KHz; otherwise, degradation occurs due to the phenomenon known as *aliasing*. (To understand why, see [Nyquist frequency ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}). So when comparing broadband to narrowband, the broadband model expects to find information in the 4-8 KHz range, whereas the narrowband model expects to find information in a range less than or equal to 4 KHz.

    Information in audio originally sampled at a narrowband frequency is limited to the 0-4 KHz range, and the upsampled audio lacks information in the range expected by the broadband models, resulting in degraded recognition accuracy. Furthermore, the information found in the expected range for a narrowband sample is qualitatively different from the information found in the same range of a broadband sample. The training data for the models are derived from very different channels (telephony for narrowband models), and the models reflect the characteristics of the channels on which they were trained.

    A useful comparison might be to imagine viewing a VHS tape on a large flat-screen HDTV. The image is blurry because playing the tape on a high-definition device cannot really add new information to the stream; it simply makes the format compatible with the better device. The same is true of upsampling audio.

1.  <span style="color:#003F69">My application depends on knowing whether a speaker replies *yes* or *no* to a question. However, the transcription does not always return *yes* or *no* but can include other words that sound similar (such as *nine* when *no* is expected). How can I improve the recognition in such cases?</span>

    You can often find the expected word by examining alternative transcription results and words. These can include words that are possible alternatives to the final results but for some reason were not selected as the best choice by the service. You can see such alternatives in one of two ways:
    -   Specify an integer for the `max_alternatives` parameter. For example, specify a value of `5` to see that number of alternative transcriptions. See [Maximum alternatives](/docs/services/speech-to-text/output.html#max_alternatives).
    -   Specify a probability for the `word_alternatives_threshold` parameter. For example, specify a value of `0.5` to see possible alternative words for which the service has a minimum confidence of 50 percent. See [Word alternatives](/docs/services/speech-to-text/output.html#word_alternatives).

    In general, the service is very sensitive to background noise: Engine noise, working devices, street noise, talking, and so forth can significantly reduce accuracy. In addition, the microphones typically installed on mobile devices and tablets are often inadequate; the service performs best when professional microphones are used to capture audio with better quality. Furthermore, the service's accuracy declines as the speaker moves farther from the microphone; for example, at a distance of ten feet, the service struggles to produce any adequate results.

## Speaker labels
{: #speakerLabels}

1.  <span style="color:#003F69">Can the service create a transcript that identifies individual speakers?</span>

    Yes. The speaker labels feature lets you identify which individuals spoke which words in a multi-participant exchange. To use the feature, set the `speaker_labels` parameter to `true` in a recognition request. See [Speaker labels](/docs/services/speech-to-text/output.html#speaker_labels).

1.  <span style="color:#003F69">How do I synchronize the results from the JSON `speaker_labels` field with individual words of the transcription to re-create a conversation?</span>

    When you set the `speaker_labels` parameter to `true`, the service also forces the `timestamps` parameter to be `true`. You need to match the `from` and `to` fields of the individual speaker labels to the equivalent start and end times of the individually timestamped words. For an example of the fields to use and the results that can be achieved, see [Speaker labels](/docs/services/speech-to-text/output.html#speaker_labels).

1.  <span style="color:#003F69">I have an audio file with ten individual speakers, but the service identifies no more than six of the speakers. Why?</span>

    Currently, the feature is optimized for two-speaker conversations but can identify up to a maximum of six speakers only.

1.  <span style="color:#003F69">I have an audio file with a single speaker, but when I set `speaker_labels` to `true`, the service identifies multiple speakers. Why?</span>

    Because the feature is optimized for two-speaker conversations, applying it to audio with a single speaker can produce incorrect results. Variations in audio quality or in the speaker's voice can cause the service to identify additional speakers who are not present (sometimes called *hallucinations*). Do not use the feature for audio that has only a single speaker.

1.  <span style="color:#003F69">I have a recording of an interview with two speakers, but the speaker labels feature does not always identify the correct speaker. In fact, only a single speaker is identified in the final transcript. Why?</span>

    Many factors can affect performance. These include audio quality and background noise, how much audio is available for each speaker (it is better to have more than 30 seconds per speaker), the relative amount of audio that is available for each speaker, and so on. The nature of your audio and the conversation it contains may by inappropriate for the feature.

## Smart formatting
{: #smartFormatting}

1.  <span style="color:#003F69">How can I get numbers to appear as digits instead of being spelled out as words in a transcription?</span>

    Set the `smart_formatting` parameter to `true`. This feature converts dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses into more conventional representations in the final transcript of a recognition request. See [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting). Note that smart formatting is currently available only for US English.

1.  <span style="color:#003F69">How can I add punctuation to a transcript? And why does the demo have punctuation but the service does not?</span>

    The service does not indicate punctuation in a transcription. The `smart_formatting` parameter improves the default formatting of the transcription but does not provide punctuation. The most effective approach is to rely on the JSON attribute `final`; when the attribute is `true` to indicate the end of a phrase, add a period to the end of the phrase and capitalize the next word of the transcript. The service demo performs such parsing of the transcription.

## Customization
{: #customization}

1.  <span style="color:#003F69">Can I add my own specialized vocabulary to the service?</span>

    Yes. The service's base vocabulary contains many words that are used in everyday conversation by a broad, general audience. Its models provide sufficiently accurate recognition for a variety of applications but can lack knowledge of specific terms that are associated with particular domains; these are referred to as *out-of-vocabulary (OOV) words*.

    The language model customization interface lets you improve the accuracy of speech recognition for specialized domains. You can use customization to expand and tailor the vocabulary of a base model to include domain-specific data and terminology. See [Using customization](/docs/services/speech-to-text/custom.html). Note that customization is currently available only for US English and Japanese (GA) and for Spanish (beta).

1.  <span style="color:#003F69">How secure is the data that I add to a custom model?</span>

    A custom language model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use service credentials created for that instance of the service. Credentials created for other instances of the service cannot view or access the custom model. In addition, the data associated with a custom model is encrypted both at rest (when it is stored) and in motion (when it is used). For more information, see [Ownership of custom language models](/docs/services/speech-to-text/custom.html#customOwner).

1.  <span style="color:#003F69">A number of customization methods are asynchronous. How do I check their results to know when they are finished?</span>

    Three customization operations are asynchronous. The following sections describe how to check the results of the operations:
    -   To check the status of a request to add a corpus to a model with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method, see [Monitoring the add corpus request](/docs/services/speech-to-text/custom.html#monitorCorpus).
    -   To check the status of a request to add words to a model with the `POST /v1/customizations/{customization_ID}/words` method, see [Monitoring the add words request](/docs/services/speech-to-text/custom.html#monitorWords).
    -   To check the status of a request to train a model with the `POST /v1/customizations/{customization_id}/train` method, see [Monitoring the train model request](/docs/services/speech-to-text/custom.html#monitorTraining).

    See [Example scripts](/docs/services/speech-to-text/custom.html#exampleScripts) for sample code that monitors the operations in a Python or shell script.

1.  <span style="color:#003F69">I created a custom model, but the service does not appear to be using any of the new words that it contains during recognition?</span>

    Make sure that you are correctly passing the customization ID to the recognition request; for more information, see [Using a custom language model](/docs/services/speech-to-text/custom.html#useModel). Also check the pronunciations that were generated for the new words to make sure that they are correct; see [Validating a custom model's words resource](/docs/services/speech-to-text/custom.html#validateModel).

## Pricing
{: #pricing}

1.  <span style="color:#003F69">What is the price for using the {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} standard service?</span>

    The {{site.data.keyword.speechtotextshort}} service is priced at $0.02 (USD) per minute. This price applies to use of both broadband and narrowband models. For more information, see the {{site.data.keyword.speechtotextshort}} [service landing page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}.

1.  <span style="color:#003F69">What does "pricing per minute" mean? Do you charge for the number of minutes of audio that I send to the service or the number of minutes that it takes the service to process the audio?</span>

    The price is based on the amount of audio that is sent to the service, not on the time that it takes the service to process the audio.

1.  <span style="color:#003F69">Do you round up to the nearest minute for every call to the API? For example, if I send two audio files that are each 30 seconds in length, am I charged for two minutes or one minute?</span>

    {{site.data.keyword.IBM_notm}} does not round up the length of the audio for every API call that the service receives. Instead, {{site.data.keyword.IBM_notm}} aggregates all usage for the month and rounds to the nearest minute at the end of the month. In this example, if you send two audio files that are each 30 seconds long, {{site.data.keyword.IBM_notm}} sums the duration of the total audio to one minute and charges $0.02 (USD).

1.  <span id="graduated" style="color:#003F69">How does volume-based Graduated Tiered Pricing work?</span>

    The tiered pricing model is intended to give high-volume users additional discounts as they continue to use the service. Per-minute pricing is reduced for additional minutes of audio once certain thresholds for total monthly audio are met. For more information, see the [pricing page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/services/speech-to-text){: new_window} for the service.

1.  <span style="color:#003F69">When using tiered pricing, what would my total charge be if I used the service to transcribe, for example, 275 thousand audio minutes in one month?</span>

    -   Every month, you receive 1000 free minutes of audio usage. So you would be charged only for 274 thousand minutes of audio: 275,000 - 1000 = 274,000.
    -   For the first 250-thousand minutes of audio, you would be charged at $0.02 (USD) / minute: 250,000 * $0.02 = $5000.00 (USD).
    -   For the remaining 24 thousand minutes of audio, you would be charged at the reduced rate of $0.015 (USD) / minute: 24,000 * $0.015 = $360.00 (USD).
    -   In this scenario, therefore, your total charge for the month would be $5360.00 (USD).

1.  <span style="color:#003F69">What is the price for using the service's language model customization interface? Am I charged for creating and storing a custom language model?</span>

    Using a custom language model for transcription incurs an add-on charge of $0.03 (USD) per minute. This is in addition to the standard usage charge of $0.02 (USD) per minute and applies to all languages supported by the customization interface. So the total charge for using a custom language model during transcription is $0.05 (USD) / minute. {{site.data.keyword.IBM_notm}} does not charge for creating or hosting a custom language model, only for using the model with a recognition request.
