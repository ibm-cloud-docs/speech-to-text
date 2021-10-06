---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-30"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Speech activity detection
{: #detection}

The {{site.data.keyword.speechtotextfull}} service offers two speech activity detection parameters to control what audio is used for speech recognition. The parameters specify the service's sensitivity to non-speech events and to background noise. The parameters are independent: You can use them individually or together.
{: shortdesc}

Speech activity detection is supported for most language models. For more information, see [Language model support](/docs/speech-to-text?topic=speech-to-text-input#detection-support).
{: note}

## How speech activity detection works
{: detection-works}

Speech activity detection consumes the input audio stream and determines which parts of the stream to pass for speech recognition. Speech recognition is adversely affected by background speech and noise, causing the service to transcribe the wrong words, to produce words where none are present, or to omit words that are part of the input audio. The speech activity detection feature can help ensure that only relevant audio is processed for speech recognition.

You can use the feature to control the following aspects of speech recognition:

-   *Suppress background speech.* Call-center data often contains cross-talk ("overhearing") from other agents. You can set a volume threshold below which such background speech is ignored.
-   *Suppress background noise.* Some audio, such as speech recorded in a factory, can contain a high level of background noise. You can set a threshold below which such background noise is ignored.
-   *Suppress non-speech audio events.* Background music and tone events, such as audio played to a client who is waiting on hold on a telephone line, can cause inaccurate recognition. Silence can also result in unnecessary recognition or transcription errors. You can set a threshold below which such events are ignored.

By default, speech activity detection is configured to provide optimal performance for the general case for each model. For specific cases, the default settings might not be optimal and can lead either to slow transcription or to word insertions and deletions. You are encouraged to experiment with different settings to determine which values work best for your audio.

## Speech detector sensitivity
{: #detection-parameters-sensitivity}

Use the `speech_detector_sensitivity` parameter to adjust the sensitivity of speech activity detection. Use the parameter to suppress word insertions from music, coughing, and other non-speech events. The service biases the audio it passes for speech recognition by evaluating chunks of the input audio against prior models of speech and non-speech activity.

Specify a float value between 0.0 and 1.0. The default value is 0.5, which provides a reasonable compromise for the level of sensitivity. A value of 0.0 suppresses all audio (no speech is transcribed). A value of 1.0 suppresses no audio (speech detection sensitivity is disabled). The values increase on a monotonic curve of sensitivity versus speech.

This parameter can affect both the quality and the latency of speech recognition:

-   Lower values can decrease latency because less audio is potentially passed for speech recognition. However, a low setting might discard chunks of audio that contain actual speech, losing viable content from the transcript.
-   Higher values can increase latency because more audio is potentially passed for speech recognition. However, a high setting might pass chunks of audio that contain non-speech events, adding spurious content to the transcript.

### Speech detector sensitivity example
{: #detection-parameters-sensitivity-example}

The following example request specifies a value of 0.6 for the `speech_detector_sensitivity` parameter with the synchronous HTTP interface. The service recognizes slightly more potential non-speech events than it would by default.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?speech_detector_sensitivity=0.6"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?speech_detector_sensitivity=0.6"
```
{: pre}

## Background audio suppression
{: #detection-parameters-suppression}

Use the `background_audio_suppression` parameter to suppress background audio based on its volume to prevent it from being transcribed as speech. Use the parameter to suppress side conversations or background noise. For example, use this parameter when there is a relatively steady and quiet (low signal energy) background sound. Because such noise can interfere with transcription, it can produce content where no actual speech occurs in the audio.

Specify a float value in the range of 0.0 to 1.0. The default value is 0.0, which provides no suppression (background audio suppression is disabled). A value of 0.5 provides a reasonable level of audio suppression for general usage. A value of 1.0 suppresses all audio (no speech is transcribed). The values increase on a monotonic curve.

This  parameter can also affect both the quality and the latency of speech recognition. However, because background noise suppression is disabled by default, setting the parameter to a value greater than zero can only improve latency. But higher values can gradually reduce the audio that is passed for speech recognition, which can cause valid content to be lost from the transcript.

### Background audio suppression example
{: #detection-parameters-suppression-example}

The following example request specifies a value of 0.5 for the `background_audio_suppression` parameter with the synchronous HTTP interface. The service suppresses a reasonable level of background audio.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?background_audio_suppression=0.5"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file1.flac \
"{url}/v1/recognize?background_audio_suppression=0.5"
```
{: pre}

## Language model support
{: #detection-support}

The `speech_detector_sensitivity` and `background_audio_suppression` parameters are supported for use with the following language models:

-   *For next-generation models,* the parameters are supported with all models.
-   *For previous-generation models,* the parameters are supported with most models. The following models do *not* support speech activity detection at this time. The parameters are ignored if used with these models.
    -   Arabic broadband model (`ar-MS_BroadbandModel`)
    -   Brazilian Portuguese broadband model (`pt-BR_BroadbandModel`)
    -   Chinese broadband model (`zh-CN_BroadbandModel`)
    -   Chinese narrowband model (`zh-CN_NarrowbandModel`)
    -   German broadband model (`de-DE_BroadbandModel`)
