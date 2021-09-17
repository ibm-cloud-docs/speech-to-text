---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-12"

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

# Interim results and low latency
{: #interim}

With the WebSocket interface, the {{site.data.keyword.speechtotextfull}} service supports interim results, which are intermediate transcription hypotheses that arrive before final results. For the WebSocket and HTTP interfaces, certain next-generation models also offer low latency to return results even more quickly than they already do, though transcription accuracy might be reduced.
{: shortdesc}

When you use next-generation models that support low latency with the WebSocket interface, you must enable both interim results and low latency to get interim results. Only next-generation models that support low latency can return interim results. Note that previous- and next-generation models return slightly different results in certain cases.

## Interim results
{: #interim-results}

The interim results feature is available only with the WebSocket interface.
{: note}

Interim results are intermediate transcription hypotheses that are likely to change before the service returns its final results. The service returns interim results as soon as it generates them. Interim results are useful for interactive applications and real-time transcription, and for long audio streams, which can take a while to transcribe.

Interim results evolve as the service's processing of an utterance progresses. They arrive more often and more quickly than final results. You can use them to enable your application to respond more quickly or to gauge the progress of transcription. Once its processing of an utterance is complete, the service sends final results that represent its best transcription of the audio for that utterance.

-   *Interim results* are identified in a transcript with the field `"final": false`. The service can update interim results with more accurate transcriptions as it processes further audio. The service delivers one or more interim results for each final result.
-   *Final results* are identified with the field `"final": true`. The service makes no further updates to final results.

How you request interim results depends on the type of model that you are using:

-   *For a previous-generation model,* set the `interim_results` parameter to `true` in the JSON `start` message. Interim results are available for all previous-generation models.
-   *For a next-generation model,* set the `interim_results` and `low_latency` parameters to `true` in the JSON `start` message. Interim results are available only for next-generation models that support low latency, and only if both interim results and low latency are enabled. For more information, see [Requesting interim results and low latency](#interim-low-latency).

To disable interim results for any model, omit the `interim_results` parameter or set it to `false`. Disable interim results if you are doing offline or batch transcription.

### Interim results example
{: #interim-results-example}

The following abbreviated WebSocket example requests interim results. The service sends multiple response objects. It sets the `final` attribute to `true` only for the final results. The request implicitly uses the default previous-generation `en-US_BroadbandModel`.

```javascript
var access_token = {access_token};
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

The response includes a single utterance with no pauses.

```javascript
{
  "result_index": 0
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ]
}{
  "result_index": 0
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ]
}{
  "result_index": 0
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes swept through "
        }
      ],
      "final": false
    }
  ]
}{
  . . .
}{
  "result_index": 0
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Low latency
{: #low-latency}

The next-generation multimedia and telephony models have generally faster response times than the previous-generation models. But there may be cases where you want to receive results even more quickly. With next-generation models that support low latency, you can set the `low_latency` parameter to `true` to receive results more quickly. For more information about the next-generation models that support low latency, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).

With low latency, the service achieves faster results at the possible expense of transcription accuracy. When low latency is enabled, the service segments the audio into smaller chunks to optimize for speed over accuracy. This trade-off might be acceptable if your application needs lower response time more than it does the highest possible accuracy. For example, low latency is ideal for use cases such as closed captioning, conversational applications, and live customer service in the voice channel of the {{site.data.keyword.conversationfull}} service.

Omitting the `low_latency` parameter can produce more accurate results and is the recommended approach for most use cases. The `low_latency` parameter is `false` by default.

The nature of the response to a request that includes low latency depends on the interface that is used:

-   With the HTTP interfaces, the service waits for all of the audio to arrive before sending a response. It then sends a single stream of bytes in response. The response can include multiple transcription elements with multiple final results, and it can be sent incrementally. But it is a single stream of data.
-   With the WebSocket interface, the service sends final results as they become available. It can send multiple independent responses in the form of different streams of bytes. The connection is bidirectional and full duplex, and requests and responses can continue to flow back and forth over a single connection for as long as it remains active.

### Low-latency restrictions
{: #low-latency-restrictions}

The low-latency feature has the following usage restrictions:

-   *Low latency is available only for some next-generation models.* For next-generation models that do not support low-latency, if you include the `low_latency` parameter with a request, the service fails with status code 400:

    ```javascript
    {
      "code": 400,
      "code_description": "Bad Request",
      "error": "low_latency is not a supported feature for model {model_id}"
    }
    ```

-   *Low latency is *not* available for previous-generation models.* For previous-generation models, if you include the `low_latency` parameter with a request, the service generates a warning:

    ```javascript
    "warnings": [
      "Unknown arguments: low_latency."
    ]
    ```
    {: codeblock}

### Low-latency example
{: #low-latency-example}

The following synchronous HTTP example requests low latency with the `en-US_Telephony` model. The example sets the `low_latency` query parameter to `true`.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony&low_latency=true"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony&low_latency=true"
```
{: pre}

## Requesting interim results and low latency
{: #interim-low-latency}

To receive interim results with a next-generation model, the model must support low latency and both the `interim_results` and `low_latency` parameters must be set to `true`. The service does not support interim results for next-generation models that do not support low latency.
{: note}

When you use next-generation models that support low latency, you can request both interim results and low latency with the WebSocket interface. However, the `interim_results` parameter behaves differently when it is used with next-generation models.

The following table describes the interaction between the `interim_results` and `low_latency` parameters and the results that you receive depending on their settings. The default values for both the `interim_results` and `low_latency` parameters are `false`.

| `interim_results` | `low_latency` | Results |
|:-----------------:|:-------------:|---------|
| `interim_results=false` | `low_latency=false` | The service sends only final results. It returns a single JSON object that includes results for all utterances when transcription is complete. See [Example 1: Interim results and low latency are both false](#interim-low-latency-examples-one). |
| `interim_results=false` | `low_latency=true`  | The service sends only final results. It returns a single JSON object that includes results for all utterances when transcription is complete. But because `low_latency` is `true`, the service returns the final results more quickly. See [Example 2: Interim results is false and low latency is true](#interim-low-latency-examples-two). |
| `interim_results=true`  | `low_latency=false` | The service sends only final results. It returns multiple JSON objects for individual utterances as it performs transcription. The advantage of setting `interim_results` to `true` is that results for utterances arrive as they become complete. You do not need to wait for all utterances to be transcribed. See [Example 3: Interim results is true and low latency is false](#interim-low-latency-examples-three). |
| `interim_results=true`  | `low_latency=true`  | The service returns interim results as it develops transcription hypotheses, and it sends final results for utterances as they become complete. The service delivers one or more interim results for each final result. The quality of interim results is identical to what you receive with previous-generation models. But because `low_latency` is `true`, the service returns interim and final results more quickly. See [Example 4: Interim results and low latency are both true](#interim-low-latency-examples-four). |
{: caption="Table 1. Interaction of interim_results and low_latency parameters"}

### Interim results and low-latency examples
{: #interim-low-latency-examples}

The following abbreviated WebSocket code shows how to request interim results, low-latency results, or both with the WebSocket interface. It specifies the following parameters:

-   The `model` query parameter of the `/v1/recognize` request passes the next-generation `en-US_Telephony` model, which supports low latency.
-   The `inactivity_timeout` parameter of the JSON `start` message sets the inactivity timeout to `-1` (infinity), which prevents the request from timing out.
-   Both the `interim_results` and `low_latency` parameters are specified with the JSON `start` message of the request.

To show examples of results with all possible combinations of the parameters, the arguments for `interim_results` and `low_latency` are set to `true` or `false` in the examples in the following sections.

```javascript
var access_token = {access_token};
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + access_token
  + '&model=en-US_Telephony';
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/wav',
    inactivity_timeout: -1,
    interim_results: {true | false},
    low_latency: {true | false}
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

The WAV file that is passed to the service includes a single sentence with two embedded multi-second pauses: "Thunderstorms could produce &lt;*pause*&gt; large hail &lt;*pause*&gt; and heavy rain." The pauses are long enough to represent separate utterances and thus generate multiple final results. Because each response includes multiple final results, one per utterance, you must assemble the final results into a single string to see the full transcript.

#### Example 1: Interim results and low latency are both false
{: #interim-low-latency-examples-one}

This example sets both `interim_results` and `low_latency` to `false`. The service returns only final results as a single JSON object.

```javascript
  var message = {
    action: 'start',
    content-type: 'audio/wav',
    inactivity_timeout: -1,
    interim_results: false,
    low_latency: false
  };
```
{: codeblock}

```javascript
{
  "result_index": 0,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "thunderstorms could produce ",
          "confidence": 0.94
        }
      ]
    },
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "large hail ",
          "confidence": 0.91
        }
      ]
    },
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "and heavy rain ",
          "confidence": 0.86
        }
      ]
    }
  ]
}
```
{: codeblock}

#### Example 2: Interim results is false and low latency is true
{: #interim-low-latency-examples-two}

This example sets `interim_results` to `false` and `low_latency` to `true`. The service returns only final results as a single JSON object.

```javascript
  var message = {
    action: 'start',
    content-type: 'audio/wav',
    inactivity_timeout: -1,
    interim_results: false,
    low_latency: true
  };
```
{: codeblock}

```javascript
{
  "result_index": 0,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "thunderstorms could produce ",
          "confidence": 0.94
        }
      ]
    },
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "large hail ",
          "confidence": 0.91
        }
      ]
    },
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "and heavy rain ",
          "confidence": 0.86
        }
      ]
    }
  ]
}
```
{: codeblock}

#### Example 3: Interim results is true and low latency is false
{: #interim-low-latency-examples-three}

This example sets `interim_results` to `true` and `low_latency` to `false`. The service returns only final results, but it returns each result as a separate JSON object.

```javascript
  var message = {
    action: 'start',
    content-type: 'audio/wav',
    inactivity_timeout: -1,
    interim_results: true,
    low_latency: false
  };
```
{: codeblock}

```javascript
{
  "result_index": 0,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "thunderstorms could produce ",
          "confidence": 0.94
        }
      ]
    }
  ]
}{
  "result_index": 1,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "large hail ",
          "confidence": 0.91
        }
      ]
    }
  ]
}{
  "result_index": 2,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "and heavy rain ",
          "confidence": 0.86
        }
      ]
    }
  ]
}
```
{: codeblock}

#### Example 4: Interim results and low latency are both true
{: #interim-low-latency-examples-four}

This example sets both `interim_results` and `low_latency` to `true`. The service returns both interim and final results, and it returns each results as a separate JSON object.

```javascript
  var message = {
    action: 'start',
    content-type: 'audio/wav',
    inactivity_timeout: -1,
    interim_results: true,
    low_latency: true
  };
```
{: codeblock}

```javascript
{
  "result_index": 0,
  "results": [
    {
      "final": false,
      "alternatives": [
        {
          "transcript": "th "
        }
      ]
    }
  ]
}{
  "result_index": 0,
  "results": [
    {
      "final": false,
      "alternatives": [
        {
          "transcript": "thunderstorms "
        }
      ]
    }
  ]
}{
  "result_index": 0,
  "results": [
    {
      "final": false,
      "alternatives": [
        {
          "transcript": "thunderstorms could produc "
        }
      ]
    }
  ]
}{
  "result_index": 0,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "thunderstorms could produce ",
          "confidence": 0.94
        }
      ]
    }
  ]
}{
  "result_index": 1,
  "results": [
    {
      "final": false,
      "alternatives": [
        {
          "transcript": "large "
        }
      ]
    }
  ]
}{
  "result_index": 1,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "large hail ",
          "confidence": 0.91
        }
      ]
    }
  ]
}{
  "result_index": 2,
  "results": [
    {
      "final": false,
      "alternatives": [
        {
          "transcript": "and hea "
        }
      ]
    }
  ]
}{
  "result_index": 2,
  "results": [
    {
      "final": true,
      "alternatives": [
        {
          "transcript": "and heavy rain ",
          "confidence": 0.86
        }
      ]
    }
  ]
}
```
{: codeblock}