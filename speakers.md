---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-19"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Speaker labels
{: #speaker-labels}

The speaker labels feature is beta functionality that is available for Czech, English (Australian, Indian, UK (narrowband/telephony), and US), German, Japanese, Korean, and Spanish only. For next-generation models, speaker labels are not supported for use with interim results or low latency.
{: beta}

With speaker labels, the {{site.data.keyword.speechtotextfull}} service identifies which individuals spoke which words in a multi-participant exchange. You can use the feature to create a person-by-person transcript of an audio stream. For example, you can use it to develop analytics for a call-center or meeting transcript, or to animate an exchange with a conversational robot or avatar. For best performance, use audio that is at least a minute long. (Labeling who spoke what and when is sometimes referred to as *speaker diarization*.)
{: shortdesc}

Speaker labels are optimized for two-speaker scenarios. They work best for telephone conversations that involve two people in an extended exchange. They can handle up to six speakers, but more than two speakers can result in variable performance. Two-person exchanges are typically conducted over narrowband (telephony) media, but you can use speaker labels with supported narrowband and broadband (multimedia) models.

To use the feature, you set the `speaker_labels` parameter to `true` for a recognition request; the parameter is `false` by default. The service identifies speakers by individual words of the audio. It relies on a word's start and end time to identify its speaker. (If you are recognizing multichannel audio, you can instead send each channel for transcription separately. For more information about this alternative approach, see [Speaker labels for multichannel audio](#speaker-labels-multichannel).)

Setting the `speaker_labels` parameter to `true` forces the `timestamps` parameter to be `true`, regardless of whether you disable timestamps with the request. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps).
{: note}

## Speaker labels example
{: #speaker-labels-example}

An example is the best way to show how speaker labels work. The following example requests speaker labels with a speech recognition request:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-multi.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-multi.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

The response includes arrays of timestamps and speaker labels. The numeric values that are associated with each element of the `timestamps` array are the start and end times of each word in the `speaker_labels` array.

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.91
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            [
              "good",
              4.01,
              4.30
            ]
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy good "
        }
      ],
      "final": true
    }
  ],
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.62,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.51,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.51,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.51,
      "final": false
    },
    {
      "from": 4.01,
      "to": 4.30,
      "speaker": 1,
      "confidence": 0.63,
      "final": true
    }
  ]
}
```
{: codeblock}

The `transcript` field shows the final transcript of the audio, which lists the words as they were spoken by all participants. By comparing and synchronizing the speaker labels with the timestamps, you can reassemble the conversation as it occurred. The `confidence` field for each speaker label indicates the service's confidence in its identification of the speaker.

<table style="width:50%">
  <caption>Table 1. Speaker labels example</caption>
  <tr>
    <th style="text-align:left">Timestamp</th>
    <th style="text-align:left">Speaker label</th>
  </tr>
  <tr>
    <td>
      <code>"hello",</code><br/>
      <code>0.68,</code><br/>
      <code>1.19</code>
    </td>
    <td>
      <code>"from": 0.68,</code><br/>
      <code>"to": 1.19,</code><br/>
      <code>"speaker": 2,</code><br/>
      <code>"confidence": 0.52,</code><br/>
      <code>"final": false</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>"yeah",</code><br/>
      <code>1.47,</code><br/>
      <code>1.93</code>
    </td>
    <td>
      <code>"from": 1.47,</code><br/>
      <code>"to": 1.93,</code><br/>
      <code>"speaker": 1,</code><br/>
      <code>"confidence": 0.62,</code><br/>
      <code>"final": false</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>"yeah",</code><br/>
      <code>1.96,</code><br/>
      <code>2.12</code>
    </td>
    <td>
      <code>"from": 1.96,</code><br/>
      <code>"to": 2.12,</code><br/>
      <code>"speaker": 2,</code><br/>
      <code>"confidence": 0.51,</code><br/>
      <code>"final": false</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>"how's",</code><br/>
      <code>2.12,</code><br/>
      <code>2.59</code>
    </td>
    <td>
      <code>"from": 2.12,</code><br/>
      <code>"to": 2.59,</code><br/>
      <code>"speaker": 2,</code><br/>
      <code>"confidence": 0.51,</code><br/>
      <code>"final": false</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>"Billy",</code><br/>
      <code>2.59,</code><br/>
      <code>3.17</code>
    </td>
    <td>
      <code>"from": 2.59,</code><br/>
      <code>"to": 3.17,</code><br/>
      <code>"speaker": 2,</code><br/>
      <code>"confidence": 0.51,</code><br/>
      <code>"final": false</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>"good",</code><br/>
      <code>4.01,</code><br/>
      <code>4.30</code>
    </td>
    <td>
      <code>"from": 4.01,</code><br/>
      <code>"to": 4.30,</code><br/>
      <code>"speaker": 1,</code><br/>
      <code>"confidence": 0.63,</code><br/>
      <code>"final": true</code>
    </td>
  </tr>
</table>

The results clearly capture the brief two-person exchange:

-   **Speaker 2** - "Hello?"
-   **Speaker 1** - "Yeah?"
-   **Speaker 2** - "Yeah, how's Billy?"
-   **Speaker 1** - "Good."

In the example, the `final` field for each speaker label but the last is `false`. The service sets the `final` field to `true` only for the last speaker label that it returns. That value of `true` indicates that the service has completed its analysis of the speaker labels.

## Speaker IDs for speaker labels
{: #speaker-labels-ids}

The example also illustrates an interesting aspect of speaker IDs. In the `speaker` fields, the first speaker has an ID of `2` and the second has an ID of `1`. The service develops a better understanding of the speaker patterns as it processes the audio. Therefore, it can change speaker IDs for individual words, and can also add and remove speakers, until it generates its final results.

As a result, speaker IDs might not be sequential, contiguous, or ordered. For instance, IDs typically start at `0`, but in the previous example the earliest ID is `1`. The service likely omitted an earlier word assignment for speaker `0` based on further analysis of the audio. Omissions can happen for later speakers, as well. Omissions can result in gaps in the numbering and produce results for two speakers who are labeled, for example, `0` and `2`. Another consideration is that speakers can leave a conversation. So a participant who contributes only to the early stages of a conversation might not appear in later results.

## Requesting interim results for speaker labels
{: #speaker-labels-interim}

When you use speaker labels with interim results, the service duplicates the final speaker label object. For more information, see the known limitations in the [Release notes for IBM Cloud](/docs/speech-to-text?topic=speech-to-text-release-notes) or the [Release notes for IBM Cloud Pak for Data](/docs/speech-to-text?topic=speech-to-text-release-notes-data).
{: important}

With the WebSocket interface, you can request interim results as well as speaker labels (for more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results)). Final results are generally better than interim results. But interim results can help identify the evolution of a transcript and the assignment of speaker labels. Interim results can indicate where transient speakers and IDs appeared or disappeared. However, the service can reuse the IDs of speakers that it initially identifies and later reconsiders and omits. Therefore, an ID might refer to two different speakers in interim and final results.

When you request both interim results and speaker labels, final results for long audio streams might arrive well after initial interim results are returned. It is also possible for some interim results to include only a `speaker_labels` field without the `results` and `result_index` fields for the transcript as a whole. If you do not request interim results, the service returns final results that include `results` and `result_index` fields and a single `speaker_labels` field.

With interim results, a `final` field with a value of `false` for a word in the `speaker_labels` field can indicate that the service is still processing the audio. The service might change its identification of the speaker or its confidence for individual words before it is done. The service sends final results when the audio stream is complete or in response to a timeout, whichever occurs first. The service sets the `final` field to `true` only for the last word of the speaker labels that it returns in either case.

## Performance considerations for speaker labels
{: #speaker-labels-performance}

As noted previously, the speaker labels feature is optimized for two-person conversations, such as communications with a call center. Therefore, you need to consider the following potential performance issues:

-   Performance for audio with a single speaker can be poor. Variations in audio quality or in the speaker's voice can cause the service to identify extra speakers who are not present. Such speakers are referred to as hallucinations.
-   Similarly, performance for audio with a dominant speaker, such as a podcast, can be poor. The service tends to miss speakers who talk for shorter amounts of time, and it can also produce hallucinations.
-   Performance for audio with more than six speakers is undefined. The feature can handle a maximum of six speakers.
-   Performance for cross-talk, or speaker overlap, is poor. Cross-talk can be difficult or impossible to transcribe accurately for any audio.
-   Performance for short utterances can be less accurate than for long utterances. The service produces better results when participants speak for longer amounts of time, at least 30 seconds per speaker. The relative amount of audio that is available for each speaker can also affect performance.
-   Performance can degrade for the first 30 seconds of speech. It usually improves to a reasonable level after 1 minute of audio, as the service receives more data to work with.

As with all transcription, performance can also be affected by poor audio quality, background noise, a person's manner of speech, and other aspects of the audio. {{site.data.keyword.IBM_notm}} continues to refine and improve the performance of the speaker labels feature. For more information about the latest improvements, see [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://www.ibm.com/blogs/research/2020/07/speaker-diarization-in-real-use-cases/){: external}.

## Speaker labels for multichannel audio
{: #speaker-labels-multichannel}

As described in [Channels](/docs/speech-to-text?topic=speech-to-text-audio-terminology#audio-terminology-channels), the {{site.data.keyword.speechtotextshort}} service downmixes audio with multiple channels to one-channel mono and uses that audio for speech recognition. The service performs this conversion for all audio, including audio that is passed to generate speaker labels.

If you have multichannel audio that records each speaker's contributions on a separate channel, you can use the following alternative approach to achieve the equivalent of speaker labels:

1.  Extract each individual channel from the audio.
1.  Transcribe the audio from each channel separately and without using the `speaker_labels` parameter. Include the `timestamps` parameter to learn precise timing information for the words of the transcripts.
1.  Merge the results of the individual requests to re-create a single transcript of the complete exchange. You can use the timestamps of the individual requests to reconstruct the conversation.

The multichannel approach has some advantages over the use of the `speaker_labels` parameter:

-   Because it relies on basic speech recognition, the service can transcribe audio that is in any supported language. Speakers labels are restricted to a subset of the available languages.
-   Because each speaker has a dedicated channel, the service can provide more accurate results than it can for speaker labels. The service does not need to determine which speaker is talking.
-   Because each speaker has a dedicated channel, the service can accurately transcribe cross-talk, or speaker overlap. On a merged channel, cross-talk can be difficult or impossible to recognize accurately.

But the approach also has some disadvantages that you need to be aware of:

-   You must separate the channels prior to sending the audio and then merge the resulting transcripts to reconstruct the conversation. The use of timestamps greatly simplifies the reconstruction process.
-   ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** You are charged for all audio that you send to the service, including silence. If you send the audio from all channels in its entirety, you effectively multiply the amount of audio being recognized by the number of channels. For example, if you submit separate requests for both channels of a two-channel exchange that lasts for five minutes, your requests require ten minutes of audio processing. If your pricing plan uses per-minute pricing, this doubles the cost of speech recognition. You can consider preprocessing the audio to eliminate silence, but that approach makes it much more difficult to merge the results of the separate requests.
