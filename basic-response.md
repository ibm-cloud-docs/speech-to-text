---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-21"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Understanding speech recognition results
{: #basic-response}

Regardless of the interface that you use, the {{site.data.keyword.speechtotextfull}} service returns transcription results that reflect the parameters that you specify. The service returns all JSON response content in the UTF-8 character set.
{: shortdesc}

## Basic transcription response
{: #response-content}

The service returns the following response for the examples in [Making a speech recognition request](/docs/speech-to-text?topic=speech-to-text-basic-request). The examples pass only an audio file and its content type. The audio speaks a single sentence with no noticeable pauses between words.

```json
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

The service returns a `SpeechRecognitionResults` object, which is the top-level response object. For these simple requests, the object includes one `results` field and and one `result_index` field:

-   The `results` field provides an array of information about the transcription results. For this example, the `alternatives` field includes the `transcript` and the service's `confidence` in the results. The `final` field has a value of `true` to indicate that these results will not change.
-   The `result_index` field provides a unique identifier for the results. The example shows final results for a request with a single audio file that has no pauses, and the request includes no additional parameters. So the service returns a single `result_index` field with a value of `0`, which is always the initial index.

If the input audio is more complex or the request includes additional parameters, the results can contain much more information.

### The alternatives field
{: #response-alternatives}

The `alternatives` field provides an array of transcription results. For this request, the array includes just one element.

-   The `transcript` field provides the results of the transcription.
-   The `confidence` field is a score that indicates the service's confidence in the transcript, which for this example exceeds 90 percent.

The `final` and `result_index` fields qualify the meaning of these fields.

### The final field
{: #response-final}

The `final` field indicates whether the transcript shows final transcription results:

-   The field is `true` for final results, which are guaranteed not to change. The service sends no further updates for final results.
-   The field is `false` for interim results, which are subject to change. If you use the `interim_results` parameter with the WebSocket interface, the service returns evolving hypotheses in the form of multiple `results` fields as it transcribes the audio. For interim results, the `final` field is always `false` and the `confidence` field is always omitted.

For more information about using the WebSocket interface to obtain interim results with previous- and next-generation models, see the following topics:

-   [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results)
-   [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency)
-   [How the service sends recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#ws-results)

### The result_index field
{: #response-result-index}

The `result_index` field provides an identifier for the results that is unique for that request. If you request interim results, the service sends multiple `results` fields for evolving hypotheses of the input audio. The indexes for interim results for the same audio always have the same value, as do the final results for the same audio.

The same index can also be used for multiple final results of a single request. Regardless of whether you request interim results, the service can return multiple final results with the same index if your audio includes pauses or extended periods of silence. For more information, see [Pauses and silence](#response-pauses-silence).

Once you receive final results for any audio, the service sends no further results with that index for the remainder of the request. The index for any further results is incremented by one.

If your audio produces multiple final results, concatenate the `transcript` elements of the final results to assemble the complete transcription of the audio. Assemble the results in the order in which you receive them. When you assemble a complete final transcript, you can ignore interim results for which the `final` field is `false`.

### Additional response content
{: #response-additional-parameters}

Many speech recognition parameters impact the contents of the service's response. Some parameters can cause the service to return multiple transcription results:

-   `end_of_phrase_silence_time`
-   `interim_results`
-   `split_transcript_at_phrase_end`

Some parameters can modify the contents of a transcript:

-   `profanity_filter`
-   `redaction`
-   `smart_formatting`

Other parameters can add more information to the results:

-   `audio_metrics`
-   `keywords` and `keywords_threshold`
-   `max_alternatives`
-   `processing_metrics` and `processing_metrics_interval`
-   `speaker_labels`
-   `timestamps`
-   `word_alternatives_threshold`
-   `word_confidence`

For more information about the available parameters, see [Using speech recognition parameters](/docs/speech-to-text?topic=speech-to-text-service-features#features-parameters) and the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

## Pauses and silence
{: #response-pauses-silence}

How the service returns results depends on the interface and the model that you use for speech recognition, as well as the audio that you pass to the service. By default, the service transcribes an entire audio stream as a single utterance and returns a single final result for all of the audio. However, the service can return multiple final results in response to the following conditions:

-   *For previous-generation models,* the utterance reaches a two-minute maximum. The service splits a transcript into multiple final results after two minutes of continuous processing.
-   The audio contains a pause or extended silence between spoken words or phrases. For most languages, the default pause interval that the service uses to determine separate final results is 0.8 seconds. For Chinese, the default interval is 0.6 seconds.

    *For previous-generation models,* you can use the `end_of_phrase_silence_time` parameter to change the duration of the pause interval. For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).

The following examples show responses with two final results from the HTTP and WebSocket interfaces. The same input audio is used in both cases. The audio speaks the phrase "one two three four five six," with a one-second pause between the words "three" and "four." The examples use the default pause interval for speech recognition.

-   *For the HTTP interfaces,* the service always sends a single `SpeechRecognitionResults` object. The `alternatives` array has a separate element for each final result. The response has a single `result_index` field with a value of `0`.

    ```json
    {
      "result_index": 0,
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
      ]
    }
    ```
    {: codeblock}

-   *For the WebSocket interface,* the service sends the same results as the previous example. The response includes a single `SpeechRecognitionResults` object,
the `alternatives` array has a separate element for each final result, and the response has a single `result_index` field with a value of `0`.

    ```json
    {
     "result_index": 0,
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
      ]
    }
    ```
    {: codeblock}

    With the WebSocket interface, responses for interim results contain more JSON objects. For more information about using the WebSocket interface to obtain interim results with previous- and next-generation models, see the following topics:

    -   [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results)
    -   [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency)
    -   [How the service sends recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#ws-results)

Silence of 30 seconds in streamed audio can result in an inactivity timeout. For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts).
{: note}

## Hesitation markers
{: #response-hesitation}

Hesitation markers are produced only by previous-generation models. Next-generation models do not produce hesitation markers.
{: note}

For most languages, the service can include hesitation markers in transcription results when it discovers brief fillers or pauses in speech. Also referred to as disfluencies, such pauses can include fillers such as "uhm", "uh", "hmm", and related non-lexical utterances. Unless you need to use them for your application, you can safely filter hesitation markers from a transcript.

Different languages can use different hesitation markers or not indicate hesitation at all:

-   *For Spanish,* the service does not produce hesitation markers.
-   *For Japanese,* hesitation markers typically begin with `D_`.
-   *For US English,* hesitation markers are indicated by the token `%HESITATION`.

The following example shows the token `%HESITATION` for a US English transcript:

```json
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

Hesitation markers can appear in both interim and final results. Enabling smart formatting prevents hesitation markers from appearing in final results for US English. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).
{: note}

Hesitation markers can also appear in other fields of a transcript. For example, if you request [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps) for the individual words of a transcript, the service reports the start and end time of each hesitation marker.

```json
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Capitalization
{: #response-capitalization}

For most languages, the service does not use capitalization in response transcripts. If capitalization is important for your application, you must capitalize the first word of each sentence and any other terms for which capitalization is appropriate.

*For US English only,* the service does capitalize many proper nouns. For example, the service returns the following text for the specified phrase:

```text
Barack Obama graduated from Columbia University
```
{: codeblock}

For other languages, the service returns the following text:

```text
barack obama graduated from columbia university
```
{: codeblock}

The service always applies this capitalization to US English, regardless of whether you use smart formatting.

## Punctuation
{: #response-punctuation}

The service does not insert punctuation in response transcripts by default. You must add any punctuation that you need to the service's results.

For some languages, you can use smart formatting to direct the service to substitute punctuation symbols, such as commas, periods, question marks, and exclamation points, for certain keyword strings. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).
