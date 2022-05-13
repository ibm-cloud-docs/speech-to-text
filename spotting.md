---

copyright:
  years: 2015, 2022
lastupdated: "2022-05-13"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Keyword spotting and word alternatives
{: #spotting}

The {{site.data.keyword.speechtotextfull}} service can identify user-specified keywords in its transcription results. It can also suggest alternative words that are acoustically similar to the words of a transcript. In both cases, the keywords and word alternatives must meet a user-specified level of confidence.
{: shortdesc}

## Keyword spotting
{: #keyword-spotting}

The `keywords` and `keywords_threshold` parameters are supported only with previous-generation models, not with next-generation models.
{: note}

The keyword spotting feature detects specified strings in a transcript. The service can spot the same keyword multiple times and report each occurrence. The service spots keywords only in the final results, not in interim results. By default, the service does no keyword spotting.

To use keyword spotting, you must specify both of the following parameters:

-   Use the `keywords` parameter to specify an array of strings to be spotted. The service spots no keywords if you omit the parameter or specify an empty array. A keyword string can include more than one token. For example, the keyword `Speech to Text` has three tokens. Keyword matching is case-insensitive, so `Speech to Text` is effectively equivalent to `speech to text`.

    For US English, the service normalizes each keyword to match spoken versus written strings. For example, it normalizes numbers to match how they are spoken as opposed to written. For other languages, keywords must be specified as they are spoken.
-   Use the `keywords_threshold` parameter to specify a probability between 0.0 and 1.0 for a keyword match. The threshold indicates the lower bound for the level of confidence the service must have for a word to match the keyword. A keyword is spotted in the transcript only if its confidence is greater than or equal to the specified threshold.

    Specifying a small threshold can potentially produce many matches. If you specify a threshold, you must also specify one or more keywords. Omit the parameter to return no matches.

The following limits apply to keyword spotting:

-   You can spot a maximum of 1000 keywords with a single request.
-   A single keyword can have a maximum length of 1024 characters. The maximum effective length for double-byte languages might be shorter.
-   Most HTTP servers and proxies impose a limit of 8 KB on the parameters of a request. Spotting a very large number of keywords or many lengthy keywords can exceed this limit. If you need to match more keywords, consider using a [multipart HTTP request](/docs/speech-to-text?topic=speech-to-text-http#HTTP-multi).

Keyword spotting is necessary to identify keywords in an audio stream. You cannot identify keywords by processing a final transcript because the transcript represents the service's best decoding results for the input audio. It does not include tokens with lower confidence scores that might represent a word of interest. So applying text processing tools to a transcript on the client side might not identify keywords. A richer representation of decoding results is needed, and that representation is available only at the server.

### Keyword spotting results
{: #keyword-spotting-results}

The service returns the results in a `keywords_result` field that is an element of the `results` array. The `keywords_result` field is a dictionary, or associative array, of enumerable properties. Each property is identified by a specified keyword and includes an array of objects. The service returns one element of the array for each match that it finds for the keyword. The object for each match includes the following fields:

-   `normalized_text` is the specified keyword that is normalized to the spoken phrase that matched in the input audio.
-   `start_time` is the start time in seconds of the match.
-   `end_time` is the end time in seconds of the match.
-   `confidence` is the service's confidence that the match represents the specified keyword. The confidence must be at least as great as the specified threshold to be included in the results.

A keyword for which the service finds no matches is omitted from the array. A keyword might not be found if

-   The audio simply does not include the keyword. Absence of the keyword is the most obvious explanation.
-   The threshold is set too high. The service might identify the keyword but with a lower level of confidence, in which case it omits the match from the results.
-   A keyword string that contains multiple tokens (for example, `Speech to Text`) is spoken with too much silence between its tokens. When the service transcribes audio, it chops the stream into a series of blocks. Each block represents a continuous chunk of audio that does not have an interval of silence that exceeds half of a second. It constructs an array of result objects that consists of these blocks.

    The service matches a multi-token keyword only if

    -   The keyword's tokens are in the same block.
    -   The tokens are either adjacent or separated by a gap of no more than 0.1 seconds.

    In the latter case, a brief filler or non-lexical utterance, such as "uhm" or "well," can lie between two tokens of the keyword. In this case, previous-generation models can insert a hesitation marker at this position of the transcript. For more information, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

### Keyword spotting example
{: #keyword-spotting-example}

The following example request sets the `keywords` parameter to a URL-encoded array of three strings (`colorado`, `tornado`, and `tornadoes`) and the `keywords_threshold` parameter to `0.5`:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?keywords=colorado%2Ctornado%2Ctornadoes&keywords_threshold=0.5"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?keywords=colorado%2Ctornado%2Ctornadoes&keywords_threshold=0.5"
```
{: pre}

The service finds qualifying occurrences of `colorado` and `tornadoes`:

```javascript
{
  "result_index": 0,
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
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

## Word alternatives
{: #word-alternatives}

The `word_alternatives_threshold` parameter is supported only with previous-generation models, not with next-generation models.
{: note}

The word alternatives feature (also known as *Confusion Networks*) reports hypotheses for acoustically similar alternatives for words of the input audio. For instance, the word `Austin` might be the best hypothesis for a word from the audio. But the word `Boston` is another possible hypothesis in the same time interval. Hypotheses share a common start and end time but have different spellings and usually different confidence scores.

By default, the service does not report word alternatives. To indicate that you want to receive alternative hypotheses, you use the `word_alternatives_threshold` parameter to specify a probability between 0.0 and 1.0. The threshold indicates the lower bound for the level of confidence the service must have in a hypothesis to return it as a word alternative. A hypothesis is returned only if its confidence is greater than or equal to the specified threshold.

You can think of word alternatives as the timeline for a transcript that is chopped into smaller intervals, or bins. Each bin can have one or more hypotheses with different spellings and confidence. The `word_alternatives_threshold` parameter controls the density of the results that the service returns. Specifying a small threshold can potentially produce many hypotheses.

### Word alternatives results
{: #word-alternatives-results}

The service returns the results in a `word_alternatives` field that is an element of the `results` array. The `word_alternatives` field is an array of objects, each of which provides the following fields for an alternative word:

-   `start_time` indicates the time in seconds at which the word for which hypotheses are returned starts in the input audio.
-   `end_time` indicates the time in seconds at which the word for which hypotheses are returned ends in the input audio.
-   `alternatives` is an array of hypothesis objects. Each object includes a `confidence` that indicates the service's confidence score for the hypothesis and a `word` that identifies the hypothesis. The confidence must be at least as great as the specified threshold to be included in the results. The service orders the alternatives by confidence.

### Word alternatives example
{: #word-alternatives-example}

The following example request specifies a rather low `word_alternatives_threshold` of `0.10`. The simple input audio includes words with common homonyms and different possible acoustic interpretations.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?word_alternatives_threshold=0.10"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?word_alternatives_threshold=0.10"
```
{: pre}

Because of the low threshold, the service returns multiple hypotheses and confidence scores for some of the words, along with the start and end times of all words. The final transcript correctly recognizes the input audio.

```javascript
{
   "result_index": 0,
   "results": [
      {
         "final": true,
         "alternatives": [
            {
               "transcript": "yes I ate that tuna ",
               "confidence": 0.82
            }
         ],
         "word_alternatives": [
            {
               "start_time": 0.0,
               "end_time": 0.31,
               "alternatives": [
                  {
                     "word": "yes",
                     "confidence": 1.0
                  }
               ]
            },
            {
               "start_time": 0.31,
               "end_time": 0.46,
               "alternatives": [
                  {
                     "word": "I",
                     "confidence": 1.0
                  }
               ]
            },
            {
               "start_time": 0.46,
               "end_time": 0.63,
               "alternatives": [
                  {
                     "word": "ate",
                     "confidence": 0.89
                  },
                  {
                     "word": "eat",
                     "confidence": 0.11
                  }
               ]
            },
            {
               "start_time": 0.63,
               "end_time": 0.77,
               "alternatives": [
                  {
                     "word": "that",
                     "confidence": 0.72
                  },
                  {
                     "word": "the",
                     "confidence": 0.27
                  }
               ]
            },
            {
               "start_time": 0.77,
               "end_time": 1.2,
               "alternatives": [
                  {
                     "word": "tuna",
                     "confidence": 0.94
                  }
               ]
            }
         ]
      }
   ]
}
```
{: codeblock}
