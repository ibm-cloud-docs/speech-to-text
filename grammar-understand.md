---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Understanding grammars
{: #grammarUnderstand}

The following examples introduce the {{site.data.keyword.speechtotextfull}} service's support for grammars. The examples create two simple ABNF grammars and show possible results when they are used for speech recognition. The examples illustrate the importance of examining the confidence score that the service includes with a transcript.
{: shortdesc}

The examples provide only the results of speech recognition requests. For examples that show how to pass a grammar for speech recognition, see [Using a grammar for speech recognition](/docs/services/speech-to-text/grammar-use.html). The examples are also very basic. For examples of more complex grammars, see [Example grammars](/docs/services/speech-to-text/grammar-examples.html).

## Single-phrase matches: The yesno grammar
{: #yesnoGrammar}

The first example defines a very simple `yesno` grammar that accepts two valid single-word responses, `yes` and `no`. The grammar is useful in cases where the user must respond with only one of the two phrases.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

When you apply this grammar to a speech recognition request, the service can return a transcript with a score that indicates its confidence in the match. It can also return no result if the input clearly does not match one of the two phrases.

For instance, if the user replies `yes`, the service likely returns a response that is very much like the following result. The score in the `confidence` field indicates a perfectly reliable match.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

But suppose, for example, the user replies `nope`. The service can return either a result with a very low confidence score or no result at all. An empty result is the clearest indication that the response does not match the grammar. An empty response is more likely to occur with complex grammars, where a valid response must match a specific multi-phrase sequence.

## Multi-phrase matches: The names grammar
{: #namesGrammar}

With a multi-phrase grammar, the user's response must be complete to be recognized. The user cannot omit a word or stop in the middle of the response. The absence of even a single word can cause the service to return an empty result.

Moreover, the service can return multiple transcripts if the user speaks phrases that are separated by sufficient silence to indicate that they are independent utterances. For example, consider the simple `names` grammar, which can match one of three multi-word names.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

Suppose the user speaks one of the names from the grammar's rules, `Yon See`. The service returns a response that indicates a very high level of confidence in the match.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Now suppose that the user speaks two names separated by enough silence, at least 0.8 seconds, to indicate that they are separate utterances: `Yon See` [1.0 seconds of silence] `Yi Wen Tan`. In this case, the service sends two separate responses with a different confidence score for each transcript.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

Finally, consider the case where the user instead says something like `Yon See` [1.0 seconds of silence] `Young Says He`. For the first phrase, `Yon See`, the service sends a positive match with a confidence score, similar to the previous examples. For the second phrase, `Young Says He`, the service can have one of two possible responses:

-   It might send no response, indicating that the phrase does not match one of the grammar's rules.
-   It might instead send a response with a low confidence score, indicating that the phrase is acoustically similar to one of the rules but is not a likely match.

## Confidence scores and empty results
{: #confidenceScores}

For relatively simple grammars like those in the previous examples, the service can return a result with a low confidence score even when the response does not appear to match the grammar at all. This might seem surprising, but a response with a low confidence score can indicate the best match that the service could find for the given audio, even though the response is unlikely to match the grammar. If the response does not match the grammar, the confidence value of the results is typically low enough to indicate the unlikelihood that the grammar can generate the response.

Always consider the confidence score when evaluating whether a response satisfies a grammar. If you don't know what threshold to set for rejection of a result based on its confidence, use an initial value of 0.5. If you experience unexpectedly large rejection rates of correct results, decrease the confidence threshold; for instance, 0.1 can be a good option for some applications. If you experience large numbers of incorrect recognitions, increase the confidence threshold for your application.

In many cases, an empty result or a result with a very low confidence score is a valid response. It can indicate that the user did not understand the question or the available options. You can always recognize the user's audio response without the grammar, but you then run the risk of getting a result that is not valid for the grammar. Grammars provide more reliable results than n-grams, which always return a result for anything other than complete silence.

Knowing that the result must be one of the valid phrases defined by a grammar is a powerful feature that can simplify an application's response handling. Generally speaking, the service can return results with high confidence only for utterances that are generated by a grammar. For the `yesno` grammar in the first example to reliably recognize the phrase `nope` as a valid response, you must add that phrase to the grammar.
