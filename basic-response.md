---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-13"

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

# Understanding recognition results
{: #basic-response}

Regardless of the interface that you use, the {{site.data.keyword.speechtotextfull}} service returns identical transcription results that reflect the input and output parameters that you specify. The service returns all JSON response content in the UTF-8 character set.
{: shortdesc}

## Basic transcription response
{: #response}

The service returns the following response for the examples in [Making a recognition request](/docs/services/speech-to-text/basic-request.html):

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

For these simple requests, which include no additional parameters, the service sends a single `results` field.

-   If you use the `interim_results` parameter with the WebSocket interface, the service sends evolving interim hypotheses in the form of multiple `results` fields as it transcribes the audio.
-   For other output parameters, the `results` array can include fields such as `keywords_result` and `word_alternatives`.
-   For other parameters, similarly, the `alternatives` array can include fields such as `word_confidence` and `timestamps`.

For more information about additional speech recognition parameters and response content, see the following sections:

-   [Input features](/docs/services/speech-to-text/input.html) describes parameters that control the audio that you send and how you send it.
-   [Output features](/docs/services/speech-to-text/output.html) describes parameters that request additional or different information in the response.

For a brief overview of all available parameters, see [Parameter summary](/docs/services/speech-to-text/summary.html).

### The alternatives field
{: #responseAlternatives}

The `alternatives` field provides an array of transcription results. For this request, the array includes a single element. If you use the `max_alternatives` parameter to request multiple alternative transcripts, the array includes a separate element for each alternative.

-   The `confidence` field is a score that indicates the service's confidence in the transcript, which for this example approaches 90 percent. If you request multiple alternative transcripts, the service returns a confidence score only for the best alternative.
-   The `transcript` field provides the results of the transcription. The results can include multiple `transcript` elements to indicate phrases that are separated by pauses. Concatenate the elements to assemble the complete transcript.

### The final field
{: #responseFinal}

The `final` field indicates whether the transcript shows final transcription results, results that are guaranteed not to change. The field is `true` for final results. It is `false` for interim results that are returned by a WebSocket request.

### The result_index field
{: #responseResultIndex}

The `result_index` field provides an identifier for the results that is unique for that request. Because the example shows final results for a request with only a single audio file and no additional request parameters, the service returns a single `result_index` field with an index of `0`.

For a WebSocket connection:

-   If you request interim results, all interim and final results for the request have the same index. But once you receive results for which the `final` field is `true`, the service sends no further results with that index.
-   Once you receive final results for an index, the service sends no further results with that index for the remainder of the connection. The index for the next set of results that are sent over the connection is incremented by one.

## Capitalization
{: #capitalization}

For most languages, the service does not use capitalization in response transcripts. If capitalization is important for your application, you must capitalize the first word of each sentence and any other terms for which capitalization is appropriate.

*For US English only,* the service does capitalize many proper nouns. For example, the service returns the following text for the specified phrase:

```
Barack Obama graduated from Columbia University
```
{: codeblock}

For other languages, the service returns the following text:

```
barack obama graduated from columbia university
```
{: codeblock}

The service always applies this capitalization to US English, regardless of whether you use [smart formatting](/docs/services/speech-to-text/output.html#smart_formatting).

## Hesitation markers
{: #hesitation}

The service can include hesitation markers in a transcript when it discovers brief fillers or pauses in speech. Also referred to as disfluencies, such pauses can include fillers such as "uhm", "uh", "hmm", and related non-lexical utterances. Unless you need to use them for your application, you can safely filter hesitation markers from a transcript.

In English, the service uses the hesitation token `%HESITATION`, as shown in the following example. Other languages can use different markers; for example, Japanese uses the token `D_`.

```javascript
{
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
  ],
  "result_index": 0
}
```
{: codeblock}

Hesitation markers can also appear in other fields of a transcript. For example, if you request [Word timestamps](/docs/services/speech-to-text/output.html#word_timestamps) for the individual words of a transcript, the service reports the start and end time of each hesitation marker.

```javascript
{
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
  ],
  "result_index": 0
}
```
{: codeblock}
