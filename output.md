---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-02"

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

# Output features and parameters
{: #output}

The {{site.data.keyword.speechtotextshort}} service offers many significant features for indicating what the service is to return from its transcription. The following sections describe the parameters that you can use to control the output that the service produces. All of the output parameters are optional. By default, the service omits the additional information enabled by the features (profanity filtering is enabled by default for US English audio). The first section shows a default basic transcription without any additional output parameters.
{: shortdesc}

If you specify an invalid query parameter or JSON field as part of the input for a recognition request, the JSON object that is returned by the service includes a `warnings` field that describes each invalid argument. The request succeeds despite the warnings.

## Basic transcription
{: #basic}

By default, the service returns a basic transcription of the input audio. The following example cURL command submits a brief FLAC file named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a> with no additional output parameters, and the service returns basic transcription results.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

The `results` field provides an array with a single `alternatives` element. As indicated by the value `true` in the `final` field, this is the final (and, in this case, only) transcription result for the audio. Because this is the final result, the output includes the service's `confidence` in the transcription, which approaches 90 percent. The `result_index` is `0` because this is the first (and only) result grouping provided for the response.

### Hesitation markers
{: #hesitation}

The service can include hesitation markers in a transcript when it discovers brief fillers or pauses in speech. Also referred to as disfluencies, such pauses can include fillers such as "uhm", "uh", "hmm", and related non-lexical utterances. In English, the service uses the hesitation token `%HESITATION`, as shown in the following example.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
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
              "that",
              7.98,
              8.31
            ],
            [
              "there's",
              8.31,
              8.53
            ],
            [
              "a",
              8.53,
              8.6
            ],
            [
              "company",
              8.6,
              8.98
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": "that %HESITATION that there's a company "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Other languages can use different markers; for example, Japanese uses the token `D_`. As shown in the example, the `%HESITATION` marker can appear in multiple fields of the output, such as the `transcript` and `timestamps` fields. Unless you need to use it for your application, you can safely filter the marker from the output.

## Speaker labels
{: #speaker_labels}

<table>
  <caption>Table 1. Requesting speaker labels</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>speaker_labels</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>speaker_labels</code> query parameter of <code>POST
        sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>speaker_labels</code> query parameter of <code>POST
        recognize</code> method
    </td>
    <td style="text-align:center">
      <code>speaker_labels</code> query parameter of <code>POST
        recognitions</code> method
    </td>
  </tr>
</table>

> **Note:** The speaker labels feature is currently beta functionality.

Speaker labels let you identify which individuals spoke which words in a multi-participant exchange. You can use the information to develop a person-by-person transcript of an audio stream, such as contact to a call center, or to animate an exchange with a conversational robot or avatar. The feature works best for audio files of telephone conversations that involve two people in an extended conversation. For best performance, the audio should be at least a minute in length. (Labelling who spoke and when is sometimes referred to as *speaker diarization*.)

The feature is optimized for two-speaker scenarios. It can handle up to six speakers, but more than two speakers can result in variable performance. Two-person exchanges are typically conducted over narrowband media, but the feature is supported for the following models:

-   `en-US_NarrowbandModel` and `en-US_BroadbandModel`
-   `es-ES_NarrowbandModel` and `es-ES_BroadbandModel`
-   `ja-JP_NarrowbandModel` and `ja-JP_BroadbandModel`

To use the feature, you set the `speaker_labels` parameter to `true` for a recognition request; the parameter is `false` by default. Because the service identifies speakers by individual words of the audio, relying on a word's start and end time to identify its speaker, enabling speaker labels also forces the `timestamps` parameter to be `true` (see [Word timestamps](/docs/services/speech-to-text/output.html#word_timestamps)).

The following example request shows a response that includes speaker labels. The numeric values associated with each element of the `timestamps` array are the start and end time of the word in the audio.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
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
              1.93
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
            . . .
          ]
          "confidence": 0.821,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.418,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.521,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.407,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.407,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.407,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

The `transcript` field shows the final transcript of the audio, which simply lists the words as they were spoken by all participants. By comparing the speaker labels with the timestamps, however, you can reassemble the conversation as it actually occurred.

<table style="width:50%">
  <caption>Table 2. Speaker labels example</caption>
  <tr>
    <th style="text-align:left">Timestamp</th>
    <th style="text-align:left">Speaker label</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.418,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.521,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.407,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.407,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.407,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

From the results, it is clear that this two-person exchange began with the following statements:

-   **Speaker 2:** "Hello?"
-   **Speaker 1:** "Yeah?"
-   **Speaker 2:** "Yeah, how's Billy?"

In the example, the `confidence` fields indicate the level to which the service is confident in its identification of the speakers. The `final` field has a value of `false` for each word, indicating that the service is still processing the audio and might change its identification of the speaker or its confidence for individual words before it is done.

The example also illustrates an interesting aspect of speaker IDs. In the `speaker` fields, note that the first speaker has an ID of `2` and the second has an ID of `1`. The service develops a better understanding of the speaker patterns as it processes the audio. Therefore, it can change speaker IDs for individual words, and can also add and remove speakers, until it generates its final results.

As a result, speaker IDs might not be sequential, contiguous, or ordered. For instance, IDs typically start at `0`, but in the previous example, the earliest ID is `1`. The service likely omitted an earlier word assignment for speaker `0` based on further analysis of the audio. This can happen for later speakers, as well, resulting in gaps in the numbering and producing results for two speakers who are labeled, for example, `0` and `2`. Another consideration is that speakers can leave a conversation, so a participant who contributes only to the early stages of a conversation does not appear in later results.

You can request both speaker labels and interim results with a recognition request. On average, final results are generally better than interim results. But interim results can help identify the evolution of the transcription and the assignment of speaker labels, indicating where transient speakers and IDs appeared or disappeared. Be aware that the service can reuse the IDs of speakers that it initially identifies and later reconsiders and omits, so an ID might refer to two different speakers in interim and final results.

The service sends final results when the audio stream is complete or in response to a timeout period, whichever occurs first. The service sets the `final` field to `true` only for the last word that it returns in either case. If you request interim results, final results for long audio streams might arrive well after initial interim results are returned. If you do not request interim results, the service returns final results as a single JSON `speaker_labels` object.

### Performance considerations

As noted previously, the speaker labels feature is optimized for two-person conversations, such as communications with a call center. With this in mind, you need to be aware of the following potential issues with the feature's performance:

-   Performance for audio with a single speaker can be poor. Variations in audio quality or in the speaker's voice can cause the service to identify additional users who are not present (sometimes called hallucinations).
-   Similarly, performance for audio with a dominant speaker, such as a podcast, can be poor. The service tends to miss speakers who talk for shorter amounts of time, and it can also produce hallucinations.
-   Performance for short utterances can be less accurate than those for long utterances. The service produces better results when participants speak for longer amounts of time.
-   Performance can be somewhat degraded for the first 30 seconds of speech. It usually improves to a reasonable level after one minute of audio.

Bear in mind that, as with all transcription, performance can also be affected by audio noise, a person's manner of speech, overlapping speakers, and other aspects of the audio.

## Keyword spotting
{: #keyword_spotting}

<table>
  <caption>Table 3. Requesting keyword spotting</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>keywords</code> and <code>keywords_threshold</code> JSON
      parameters of <code>start</code> message
    </td>
    <td style="text-align:center">
      <code>keywords</code> and <code>keywords_threshold</code> query
      parameters of <code>POST sessions/{session_id}/recognize</code>
      method
    </td>
    <td style="text-align:center">
      <code>keywords</code> and <code>keywords_threshold</code> query
      parameters of <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>keywords</code> and <code>keywords_threshold</code> query
      parameters of <code>POST recognitions</code> method
    </td>
  </tr>
</table>

> **Note:** The keyword spotting feature is currently beta functionality.

The keyword spotting feature lets you detect specified strings in the transcript generated for input audio by the service. The service can spot the same keyword multiple times and report each occurrence. The service spots keywords only in the final hypothesis, not in interim results.

By default, the service does no keyword spotting. You must specify both of the following parameters to use the feature:

-   Use the `keywords` parameter to specify an array of strings to be spotted. The service spots no keywords if you omit the parameter or specify an empty array. A keyword string can include more than one token; for example, the keyword `Speech to Text` has three tokens. For US English, the service normalizes each keyword to match spoken versus written strings; for example, it normalizes numbers to match how they are spoken as opposed to written. For other languages, keywords must be specified as they are spoken.
-   Use the `keywords_threshold` parameter to specify a probability between `0` and `1` inclusive for a keyword match. The threshold indicates the lower bound for the level of confidence the service must have for a word to match the keyword. A keyword is spotted in the transcript only if its confidence is greater than or equal to the specified threshold. Specifying a small threshold can potentially produce a large number of matches. Omit the parameter to return no matches. If you specify a threshold, you must also specify one or more keywords.

The service returns the results as a `keywords_result` object that is an element of the `results` array. The `keywords_result` object is a dictionary, or associative array, of enumerable properties of the object. Each property is identified by a specified keyword and includes an array of objects, with one element of the array returned for each match found for the keyword. The object for each match includes the following fields:

-   `normalized_text` is the specified keyword normalized to the spoken phrase that matched in the input audio.
-   `start_time` is the start time in seconds of the match.
-   `confidence` is the service's confidence that the match represents the specified keyword. The confidence must be at least as great as the specified threshold to be included in the results.
-   `end_time` is the end time in seconds of the match.

A keyword for which the service finds no matches is omitted from the array. A keyword might not be found if

-   The audio simply does not include the keyword. This is the most obvious explanation.
-   The threshold is set too high. The service might identify the keyword but with a lower level of confidence, in which case it omits the match from the results.
-   A keyword string that contains multiple tokens (for example, `Speech to Text`) is spoken with too much silence between its tokens. When the service transcribes audio, it chops the stream into a series of blocks. Each block represents a continuous chunk of audio that does not have an interval of silence that exceeds 0.5 second. It constructs an array of result objects that consists of these blocks.

    The service matches a multi-token keyword only if the keyword's tokens are in the same block *and* they are either adjacent or separated by a gap of no more than 0.1 second. (The latter case can occur if a brief filler or non-lexical utterance, such as "uhm" or "well," lies between two tokens of the keyword.)

The following example request sets the `keywords` parameter to a URL-encoded array of three strings (`colorado`, `tornado`, and `tornadoes`) and the `keywords_threshold` parameter is `0.5`. The service finds qualifying occurrences of `colorado` and `tornadoes`.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.913,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Keyword spotting is necessary to identify keywords in an audio stream. You cannot identify keywords by processing the transcript that the service returns to the client because the transcript represents the best decoding results for the input audio. It does not include tokens with lower confidence scores that might represent a word of interest. So applying text processing tools to a transcript on the client side does not catch keywords. A richer representation of decoding results is needed, and such a representation is available only at the server.

## Maximum alternatives
{: #max_alternatives}

<table>
  <caption>Table 4. Requesting maximum alternatives</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>max_alternatives</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>max_alternatives</code> query parameter of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>max_alternatives</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>max_alternatives</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

The `max_alternatives` parameter accepts an integer value that tells the service to return the *n*-best alternative hypotheses. By default, the service returns only a single transcription result, which is equivalent to setting the parameter to `1`. By setting `max_alternatives` to a number greater than 1, you ask the service to return that number of the best alternative transcriptions.

The service reports a confidence score only for the best alternative that it returns. In most cases, that is the alternative to choose. The following example request sets the `max_alternatives` parameter to `3`; the service reports a confidence for the most likely of the three alternatives.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down is a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Interim results
{: #interim}

<table>
  <caption>Table 5. Requesting interim results</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>interim_results</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>interim_results</code> query parameter of
      <code>GET sessions/{session_id}/observe_result</code> method
    </td>
    <td style="text-align:center">
      Not supported
    </td>
    <td style="text-align:center">
      Not supported
    </td>
  </tr>
</table>

Interim results are intermediate hypotheses of a transcription that are likely to change before the service returns its final results. The service returns interim results as soon as it generates them. This is useful for interactive applications and for real-time transcription. To receive interim results

-   *With the HTTP interface*, call the `GET /v1/sessions/{session_id}/observe_result` method with the `interim_results` query parameter set to `true`. You must request interim results before the call to the `POST /v1/sessions/{session_id}/recognize` method completes. You can obtain interim results only when you use sessions.
-   *With the WebSocket interface*, set the `interim_results` JSON parameter to `true` in the `start` message for a recognition request.

If you omit the `interim_results` parameter or set it to `false`, the service returns only a single JSON transcription result at the end of the audio. Follow these guidelines when using the parameter:

-   Use `false` if you are doing offline or batch transcription, do not need minimum latency, and want a single JSON result for all audio.
-   Use `true` if you want results to arrive progressively as the service processes the audio or if you want the results with minimum latency. Keep in mind that the service can update interim results as it processes more audio.

Interim results are indicated in the JSON response with the `final` field set to `false`; the service can update such results with more accurate transcriptions as it processes additional audio. Final results are identified with the `final` field set to `true`; the service makes no further updates to such results.

The following example requests interim results. The `final` attribute is set to `true` only for the final result.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?interim_results=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
   . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Word alternatives
{: #word_alternatives}

<table>
  <caption>Table 6. Requesting word alternatives</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>word\_alternatives\_<br/>threshold</code> JSON parameter of
      <code>start</code> message
    </td>
    <td style="text-align:center">
      <code>word\_alternatives\_<br/>threshold</code> query parameter of
      <code>POST sessions/{session\_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>word\_alternatives\_<br/>threshold</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>word\_alternatives\_<br/>threshold</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

> **Note:** The word alternatives feature is currently beta functionality.

The word alternatives feature (also known as Confusion Networks) reports hypotheses for acoustically similar alternatives for words of the input audio. For instance, the word `Austin` might be the best hypothesis for a word from the audio, but the word `Boston` is another possible hypothesis in the same time interval. Hypotheses share a common start and end time but have different spellings and usually different confidence scores.

By default, the service does not report word alternatives. To indicate that you want to receive alternative hypotheses, you use the `word_alternatives_threshold` parameter to specify a probability between `0` and `1` inclusive. The threshold indicates the lower bound for the level of confidence the service must have in a hypothesis to return it as a word alternative. A hypothesis is returned only if its confidence is greater than or equal to the specified threshold.

You can think of word alternatives as the timeline for a transcript chopped into smaller intervals, or bins. Each bin can have one or more hypotheses with different spellings and confidence. The `word_alternatives_threshold` parameter controls the density of the results that the service returns. Specifying a small threshold can potentially produce a large number of hypotheses.

The service returns the results as an array of `word_alternatives` that is an element of the `results` array. Each element of the `word_alternatives` array is an object that includes the following fields:

-   `start_time` indicates the time in seconds at which the word for which hypotheses are returned starts in the input audio.
-   `alternatives` is an array of hypothesis objects. Each object includes a `confidence` that indicates the service's confidence score for the hypothesis and a `word` that identifies the hypothesis. The confidence must be at least as great as the specified threshold to be included in the results. The alternatives are ordered by confidence.
-   `end_time` indicates the time in seconds at which the word for which hypotheses are returned ends in the input audio.

The following example request specifies a `word_alternatives_threshold` of `0.9`. The output includes potential hypotheses and confidence scores for a number of words, along with their start and end times.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.9634,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.991,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.9729,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Word confidence
{: #word_confidence}

<table>
  <caption>Table 7. Requesting word confidence</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>word_confidence</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>word_confidence</code> query parameter of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>word_confidence</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>word_confidence</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

The `word_confidence` parameter tells the service whether to provide confidence measures for the words of the transcription. By default, the service reports a confidence measure only for the final phrase as a whole. Setting `word_confidence` to `true` directs the service to report a confidence measure for each individual word of the phrase.

A confidence measure indicates the service's estimation that the transcribed word is correct. Confidences scores range from 0 to 1.0. A score of 1.0 for a word indicates that the current transcription reflects the most likely result based on the acoustic evidence; a score of 0.5 means that the word has a 50-percent chance of being correct.

The following example requests word confidence scores for the words of the transcription.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
          "confidence": 0.891,
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
              0.904
            ],
            . . .
            [
              "on",
              0.311
            ],
            [
              "Sunday",
              0.986
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Word timestamps
{: #word_timestamps}

<table>
  <caption>Table 8. Requesting word timestamps</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="width:25%; text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>timestamps</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>timestamps</code> query parameter of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>timestamps</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>timestamps</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

The `timestamps` parameter tells the service whether to produce timestamps for the words it transcribes. By default, the service reports no timestamps. Setting `timestamps` to `true` directs the service to report the beginning and ending time in seconds for each word relative to the start of the audio.

The following example requests timestamps for the words of the transcription.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
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
          "confidence": 0.891,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Profanity filtering
{: #profanity_filter}

<table>
  <caption>Table 9. Requesting profanity filtering</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>profanity_filter</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>profanity_filter</code> query parameter of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>profanity_filter</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>profanity_filter</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

The `profanity_filter` parameter indicates whether the service is to censor profanity from its results. By default, the service obscures all profanity by replacing it with a series of asterisks in the output. Setting the parameter to `false` displays words in the output exactly as transcoded. Profanity filtering is available only for US English transcription.

The service censors profanity from all final and any alternative transcripts and from results associated with word alternatives, word confidence, and word timestamps. The sole exception is keyword spotting, for which the service returns all words as specified by the user, regardless of whether `profanity_filter` is `true`.

The following example shows the results for a brief audio file that is transcribed with the default `true` value for the `profanity_filter` parameter. The request also sets the `word_alternatives_threshold` parameter to a relatively high value of `0.99` and the `word_confidence` and `timestamps` parameters to `true`.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.9976,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.992,
          "word_confidence": [
            ["****", 0.9999999999999918],
            ["you", 0.986436366840706]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Smart formatting
{: #smart_formatting}

<table>
  <caption>Table 10. Requesting smart formatting</caption>
  <tr>
    <th style="width:25%; text-align:center">WebSocket</th>
    <th style="width:25%; text-align:center">HTTP with Sessions</th>
    <th style="width:25%; text-align:center">HTTP Sessionless</th>
    <th style="text-align:center">Asynchronous HTTP</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>smart_formatting</code> JSON parameter of <code>start</code>
      message
    </td>
    <td style="text-align:center">
      <code>smart_formatting</code> query parameter of
      <code>POST sessions/{session_id}/recognize</code> method
    </td>
    <td style="text-align:center">
      <code>smart_formatting</code> query parameter of
      <code>POST recognize</code> method
    </td>
    <td style="text-align:center">
      <code>smart-formatting</code> query parameter of
      <code>POST recognitions</code> method
    </td>
  </tr>
</table>

> **Note:** The smart formatting feature is currently beta functionality that is available for US English only.

The `smart_formatting` parameter converts dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses into more conventional representations in the final transcript of a recognition request. The conversion makes the transcript more readable and enables better post-processing of the transcription results. You set the parameter to `true` to enable smart formatting, as in the following example; by default, the parameter is `false` and smart formatting is not performed.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

Smart formatting is based on the presence of obvious keywords in the transcription. For example, times are identified by keywords such as *AM* or *EST*, and military times are converted if identified by the keyword *hours*. Phone numbers must be either *911* or a number with 10 digits or with 11 digits that start with the number *1*. The following table shows examples of final transcripts both with and without smart formatting.

<table>
  <caption>Table 11. Smart formatting examples</caption>
  <tr>
    <th style="width:45%; text-align:left">Without smart formatting</th>
    <th style="text-align:left">With smart formatting</th>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Dates</strong>
    </td>
  </tr>
  <tr>
    <td>
      I was born on ten oh six nineteen seventy
    </td>
    <td>
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td>
      I was born on the ninth of December nineteen hundred
    </td>
    <td>
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td>
      Today is June sixth
    </td>
    <td>
      Today is June 6
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Times</strong>
    </td>
  </tr>
  <tr>
    <td>
      The meeting starts at nine thirty AM
    </td>
    <td>
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td>
      I am available at seven EST
    </td>
    <td>
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td>
      We meet at oh seven hundred hours
    </td>
    <td>
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Numbers</strong>
    </td>
  </tr>
  <tr>
    <td>
      The quantity is one million one hundred and one
    </td>
    <td>
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td>
      One point five is between one and two
    </td>
    <td>
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Phone numbers</strong>
    </td>
  </tr>
  <tr>
    <td>
      Call me at nine one four two three seven one thousand
    </td>
    <td>
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td>
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td>
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Currency values</strong>
    </td>
  </tr>
  <tr>
    <td>
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td>
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td>
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td>
      the dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Internet addresses</strong>
    </td>
  </tr>
  <tr>
    <td>
      My email address is john dot doe at foo dot com
    </td>
    <td>
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td>
      I saw the story on yahoo dot com
    </td>
    <td>
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <strong>Combinations</strong>
    </td>
  </tr>
  <tr>
    <td>
      CPT code is zero two four eight one and the date of service is
      May fifth two thousand and one
    </td>
    <td>
      CPT code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td>
      There are forty seven links on Yahoo dot com now
    </td>
    <td>
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

Smart formatting is performed only on the final transcript just before the result is returned to the client, when text normalization is complete. In cases where an utterance contains long enough silences, the transcript can be split into two or more final chunks; this affects the results of the formatting. The following examples show how a long pause can affect the results of the formatting.

<table>
  <caption>Table 12. Smart formatting examples for long pauses</caption>
  <tr>
    <th style="width:45%; text-align:left">Audio speech</th>
    <th style="text-align:left">Formatted transcription results</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>
