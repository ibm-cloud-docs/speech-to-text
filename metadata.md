---

copyright:
  years: 2015, 2022
lastupdated: "2022-07-12"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Response metadata
{: #metadata}

The {{site.data.keyword.speechtotextfull}} service can return three types of metadata about its transcription results. You can request maximum alternatives to see multiple possible final transcription results. You can also request word confident and word timestamps to receive confidence measures and timestamps for each word of the audio.
{: shortdesc}

## Maximum alternatives
{: #max-alternatives}

The `max_alternatives` parameter is supported only with previous-generation models, not with next-generation models.
{: note}

The `max_alternatives` parameter accepts an integer value that tells the service to return the *n*-best alternative hypotheses for the results. By default, the service returns only a single transcription result, which is equivalent to setting the parameter to `1`. By setting `max_alternatives` to a number greater than 1, you ask the service to return that number of the best alternative transcriptions. (If you specify a value of `0`, the service uses the default value of `1`.)

The service reports a confidence score only for the best alternative that it returns. In most cases, that is the alternative to choose.

Internal changes and improvements to the service can affect transcripts and confidence scores. For example, speech recognition may be improved to return more precise transcription results. Similarly, transcript and word confidence scores might change slightly as a result of improved speech recognition. Such changes are expected to be modest, but do not expect transcripts and confidence scores to remain unchanged over time.
{: note}

### Maximum alternatives example
{: #maximum-alternatives-example}

The following example request sets the `max_alternatives` parameter to `3`:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?max_alternatives=3"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?max_alternatives=3"
```
{: pre}

The service reports a confidence only for the most likely of the three alternatives:

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado and Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Word confidence
{: #word-confidence}

The `word_confidence` parameter tells the service whether to provide confidence measures for the words of the transcript. By default, the service reports a confidence measure only for the final transcript as a whole. Setting `word_confidence` to `true` directs the service to report a confidence measure for each individual word of the transcript.

A confidence measure indicates the service's estimation that the transcribed word is correct based on the acoustic evidence. Confidence scores range from 0.0 to 1.0.

-   A score of 1.0 indicates that the current transcription of the word reflects the most likely result.
-   A score of 0.5 means that the word has a 50-percent chance of being correct.

Internal changes and improvements to the service can affect transcripts and confidence scores. For example, speech recognition may be improved to return more precise transcription results. Similarly, transcript and word confidence scores might change slightly as a result of improved speech recognition. Such changes are expected to be modest, but do not expect transcripts and confidence scores to remain unchanged over time.
{: note}

### Word confidence example
{: #word-confidence-example}

The following example requests word confidence scores for the words of the transcription:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?word_confidence=true"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?word_confidence=true"
```
{: pre}

The service returns a confidence score for each word of the audio:

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Word timestamps
{: #word-timestamps}

The `timestamps` parameter tells the service whether to produce timestamps for the words it transcribes. By default, the service reports no timestamps. Setting `timestamps` to `true` directs the service to report the beginning and ending time in seconds for each word relative to the start of the audio.

Timestamps are automatically enabled when you request speaker labels. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

### Word timestamps example
{: #word-timestamps-example}

The following example requests timestamps for the words of the transcription:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?timestamps=true"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?timestamps=true"
```
{: pre}

The service returns a timestamp for each word of the audio:

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}
