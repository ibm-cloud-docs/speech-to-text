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

# Speaker labels
{: #speakerLabels}

1.  <span style="color:#003F69">Can the service create a transcript that identifies individual speakers?</span>

    Yes. The speaker labels feature identifies which individuals spoke which words in a multi-participant exchange. To use the feature, set the `speaker_labels` parameter to `true` in a recognition request. See [Speaker labels](/docs/services/speech-to-text/output.html#speaker_labels).

1.  <span style="color:#003F69">How do I synchronize the results from the JSON `speaker_labels` field with individual words of the transcription to re-create a conversation?</span>

    When you set the `speaker_labels` parameter to `true`, the service also forces the `timestamps` parameter to be `true`. You need to match the `from` and `to` fields of the individual speaker labels to the equivalent start and end times of the individually timestamped words. For an example of the fields to use and the results that can be achieved, see [Speaker labels example](/docs/services/speech-to-text/output.html#speakerLabelsExample).

1.  <span style="color:#003F69">I have an audio file with ten individual speakers, but the service identifies no more than six of the speakers. Why?</span>

    Currently, the feature is optimized for two-speaker conversations but can identify up to a maximum of six speakers only.

1.  <span style="color:#003F69">I have an audio file with a single speaker, but when I set `speaker_labels` to `true`, the service identifies multiple speakers. Why?</span>

    Because the feature is optimized for two-speaker conversations, applying it to audio with a single speaker can produce incorrect results. Variations in audio quality or in the speaker's voice can cause the service to identify speakers who are not present (sometimes called *hallucinations*). Do not use the feature for audio that has only a single speaker.

1.  <span style="color:#003F69">I have a recording of an interview with two speakers, but the speaker labels feature does not always identify the correct speaker. In fact, only a single speaker is identified in the final transcript. Why?</span>

    Many factors can affect performance. The factors include

    -   Audio quality and background noise.
    -   How much audio is available for each speaker (it is better to have more than 30 seconds per speaker).
    -   The relative amount of audio that is available for each speaker.

   The nature of your audio and the conversation it contains might be inappropriate for the feature.
