---

copyright:
  years: 2015, 2021
lastupdated: "2021-05-20"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Speech audio parsing
{: #parsing}

The {{site.data.keyword.speechtotextfull}} service provides two features that determine how the service is to parse audio to produce final transcription results. End of phrase silence time specifies the duration of the pause interval at which the service splits a transcript into multiple final results. Split transcript at phrase end directs the services to split a transcript into multiple final results for semantic features such as sentences.
{: shortdesc}

## End of phrase silence time
{: #silence-time}

The `end_of_phrase_silence_time` parameter specifies the duration of the pause interval at which the service splits a transcript into multiple final results. If the service detects pauses or extended silence before it reaches the end of the audio stream, its response can include multiple final results. Silence indicates a point at which the speaker pauses between spoken words or phrases. For most languages, the default pause interval is 0.8 seconds; for Chinese the default interval is 0.6 seconds. For more information, see [Pauses and silence](/docs/speech-to-text?topic=speech-to-text-basic-response#response-pauses-silence).

By using the `end_of_phrase_silence_time` parameter, you can specify a double value between 0.0 and 120.0 seconds that indicates a different pause interval:

-   A value greater than 0 specifies the interval that the service is to use for speech recognition.
-   A value of 0 indicates that the service is to use the default interval. It is equivalent to omitting the parameter.

You can use the parameter to strike a balance between how often a final result is produced and the accuracy of the transcription:

-   A longer pause interval produces fewer final results, and each result covers a larger segment of audio. Larger segments tend to be more accurate because the service has more context with which to transcribe the audio.

    However, larger pause intervals directly impact the latency of the final results. After the last word of the input audio, the service must wait for the indicated interval to expire before it returns its response. The service waits to ensure that the input audio does not continue beyond the longer interval. (With the WebSocket interface, setting the parameter can affect the latency and accuracy of the final results, regardless of whether you request interim results.)
-   A shorter pause interval yields more final results, but because each segment of audio is smaller, transcription accuracy might not be as good. Smaller segments are acceptable when latency is crucial and transcription accuracy is not expected to degrade or any degradation is acceptable.

    Also, a multi-phrase grammar recognizes, or matches, responses only within a single final result. If you use a grammar to recognize multiple strings, you can increase the pause interval to avoid receiving multiple final results.

Increase the interval when accuracy is more important than latency. Decrease the interval when the speaker is expected to say short phrases or single-word responses.

*For previous-generation models,* the service generates a final result for audio when an utterance reaches a two-minute maximum. The service splits a transcript into multiple final results after two minutes of continuous processing.
{: note}

### End of phrase silence time example
{: #silence-time-example}

The following example requests show the effect of using the `end_of_phrase_silence_time` parameter. The audio speaks the phrase "one two three four five six," with a one-second pause between the words "three" and "four." The speaker might be reading a six-digit number from an identification card, for example, and pause to confirm the number.

The first example uses the default pause interval of 0.8 seconds:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize"
```
{: pre}

Because the pause is greater than the default interval, the service splits the transcript at the pause. The `confidence` of both results is `0.99`, so transcription accuracy is very good despite the pause.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "one two three "
        }
      ],
      "final": true
    },
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "four five six "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

But suppose speech recognition is performed with a grammar that expects the user to speak six digits in a single-phrase response. Because the service splits the transcript at the one-second pause, the results are empty. The grammar is applied to each final result, but neither result, "one two three" nor "four five six", contains six digits.

The second example uses the same audio but sets the `end_of_phrase_silence_time` to 1.5 seconds:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?end_of_phrase_silence_time=1.5"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?end_of_phrase_silence_time=1.5"
```
{: pre}

Because this value is greater than the length of the speaker's pause, the service returns a single final result that contains the entire spoken phrase. A grammar that expects to find six digits recognizes this transcript.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "one two three four five six "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Split transcript at phrase end
{: #split-transcript}

The `split_transcript_at_phrase_end` parameter directs the service to split the transcript into multiple final results based on semantic features of the input. Setting the parameter to `true` causes the service to split the transcript at the conclusion of meaningful phrases such as sentences. The service bases its understanding of semantic features on the base language model that you use with the request along with a custom language model or grammar that you use. Custom language models and grammars can influence how and where the service splits a transcript.

If you apply a custom language model to speech recognition, the service is likely to split the transcript based on the content and nature of the model's corpora. For example, a model whose corpora include many short sentences biases the service to mimic the corpora and split the input into multiple, shorter final results. Similarly, a grammar that expects a series of six digits can influence the service to insert splits to accommodate that series of digits.

However, the degree to which custom language models and grammars influence the service's splitting of a transcript depends on many factors. Such factors include the amount of training data with which the custom model is built and the qualities of the audio itself. Small changes in the audio can affect the results.

Regardless, the service can still produce multiple final results in response to pauses and silence. If you omit the `split_transcript_at_phrase_end` parameter or set it to `false`, the service splits transcripts based solely on the pause interval. The pause interval has precedence over splitting due to semantic features. For more information, see [End of phrase silence time](#silence-time).

### Split transcript at phrase end results
{: #split-transcript-results}

When the split transcript at phrase end feature is enabled, each final result in the transcript includes an additional `end_of_utterance` field that identifies why the service split the transcript at that point:

-   `full_stop` - A full semantic stop, such as for the conclusion of a grammatical sentence. The insertion of splits is influenced by the base language model and biased by custom language models and grammars, as described previously.
-   `silence` - A pause or silence that is at least as long as the pause interval. You can use the `end_of_phrase_silence_time` parameter to control the length of the pause interval.
-   `end_of_data` - The end of the input audio stream.
-   `reset` - The amount of audio that is currently being processed exceeds the two-minute maximum. The service splits the transcript to avoid excessive memory use.

### Split transcript at phrase end example
{: #split-transcript-example}

The following example requests demonstrate the use of the `split_transcript_at_phrase_end` parameter. The audio speaks the phrase "I have an identification card. The number is one two three four five six." The speaker pauses for one second between the words "three" and "four."

The first example shows the results for a request that omits the parameter:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize"
```
{: pre}

The service returns two final results, splitting the transcript only at the speaker's extended pause:

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.93,
          "transcript": "I have a valid identification card the number is one two three "
        }
      ],
      "final": true
    },
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "four five six "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

The second example recognizes the same audio but sets `split_transcript_at_phrase_end` to `true`:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?split_transcript_at_phrase_end=true"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?split_transcript_at_phrase_end=true"
```
{: pre}

The service returns three final results, adding a result for the semantic stop after the first sentence. Each result includes the `end_of_utterance` field to identify the reason for the split.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "I have a valid identification card "
        }
      ],
      "final": true,
      "end_of_utterance": "full_stop"
    },
    {
      "alternatives": [
        {
          "confidence": 0.97,
          "transcript": "the number is one two three "
        }
      ],
      "final": true,
      "end_of_utterance": "silence"
    },
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "four five six "
        }
      ],
      "final": true,
      "end_of_utterance": "end_of_data"
    }
  ],
  "result_index": 0
}
```
{: codeblock}
