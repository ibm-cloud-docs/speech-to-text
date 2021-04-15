---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-15"

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

With the WebSocket interface, the {{site.data.keyword.speechtotextfull}} service supports interim results, which are intermediate transcription hypotheses that arrive before the final results. For the WebSocket and HTTP interfaces, certain next-generation models also offer low latency to return results even more quickly than they already do, though transcription accuracy might be reduced.
{: shortdesc}

When you use next-generation models that support low latency, you can request both interim results and low latency with the WebSocket interface. However, interim results behave differently when they are used with next-generation models. You must understand the interaction of the interim results and low latency features to use them together.

## Interim results
{: #interim-results}

The interim results feature is available only with the WebSocket interface. Interim results are available for all previous-generation models. For next-generation models, they are available only with those models that support low latency and only if low latency is enabled. For more information, see [Requesting interim results and low latency](#interim-low-latency).
{: note}

Interim results are intermediate transcription hypotheses that are likely to change before the service returns its final results. The service returns interim results as soon as it generates them. Interim results are useful for

-   Interactive applications and real-time transcription
-   Long audio streams, which can take a while to transcribe

Interim results evolve as the service's processing of an utterance progresses. They arrive more often and more quickly than final results. You can use them to enable your application to respond more quickly or to gauge the progress of the transcription. Once its processing of an utterance is complete, the service sends final results that represent its best transcription of the audio for that utterance.

To receive interim results, set the `interim_results` parameter to `true` in the JSON `start` message for a WebSocket recognition request. If you omit the `interim_results` parameter or set it to `false`, the service returns only a single final transcript at the end of the audio. Follow these guidelines to use the parameter:

-   Omit the parameter or set it to `false` if you are doing offline or batch transcription, do not need minimum latency, and want a single transcription result for your audio.
-   Set the parameter to `true` if you want results to arrive progressively as the service processes the audio or if you want the results with minimum latency. Keep in mind that the service can update interim results as it processes more audio.

Interim results are indicated in a transcript with the `final` field set to `false`. The service can update such results with more accurate transcriptions as it processes further audio. Final results are identified with the `final` field set to `true`. The service makes no further updates to final results.

### Interim results example
{: #interim-results-example}

The following abbreviated WebSocket example requests interim results for a WebSocket request. In its response, the service sets the `final` attribute to `true` only for the final results.

```javascript
var IAM_access_token = {access_token};
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + IAM_access_token;
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
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
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
          "confidence": 0.96,
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

## Low latency
{: #low-latency}

The low-latency feature is beta functionality that is available only for some next-generation models. For more information about the next-generation models that support low latency, see [Supported language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported) for next-generation models. Low latency is *not* available for use with previous-generation models.
{: beta}

The next-generation multimedia and telephony models have generally faster response times than the previous-generation models. But there may be cases where you want to receive results even faster. With some next-generation models, you can set the `low_latency` parameter to `true` to receive results more quickly. The parameter is `false` by default.

With low latency, the service achieves faster results at the possible expense of transcription accuracy. When low-latency is enabled, the service curtails its analysis of the audio, which can reduce the accuracy of the transcription. This trade-off might be acceptable if your application needs lower response time more than it does the highest possible accuracy.

For example, low latency is ideal for use cases such as closed captioning, conversational applications, and live customer service in the voice channel of the {{site.data.keyword.conversationfull}} service. Low latency segments the audio into smaller chunks to optimize for speed. However, omitting the `low_latency` parameter can produce more accurate results and is the recommended approach for most use cases.

The nature of the response to a request that includes low latency depends on the interface that is used:

-   With the HTTP interfaces, the service waits for all of the audio to arrive before sending a response. It then sends a single stream of bytes in response. The response can include multiple transcription elements with multiple final results, and it can be sent in increments. But it is a single stream of data.
-   With the WebSocket interface, the service sends final results as they become available. It can send multiple independent responses in the form of different streams of bytes. The connection is bidirectional and full duplex, and requests and responses can continue to flow back and forth over a single connection for as long as it remains active.

### Low-latency example
{: #low-latency-example}

The following synchronous HTTP example requests low latency with the `en-US_Telephony` model. The example sets the `low_latency` query parameter to `true`.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony&low_latency=true"
```
{: pre}

## Requesting interim results and low latency
{: #interim-low-latency}

To receive interim results with a next-generation model, the model must support low latency and both the `interim_results` and `low_latency` parameters must be set to `true`.
{: note}

When you use next-generation models that support low latency, you can request both interim results and low latency with the WebSocket interface. However, the `interim_results` parameter behaves differently when it is used with next-generation models. Interim results are still available only with the WebSocket interface. But they are available only with next-generation models that support low latency and only if low latency is enabled.

Table 3 describes the interaction between the `low_latency` and `interim_results` parameters and the results you receive depending on their settings.

| Parameter settings | Results |
|--------------------|---------|
| `low_latency=false` | Interim results are not available, regardless of whether `interim_results` is set to `true`, set to `false`, or omitted. Omitting the `low_latency` parameter has the same effect, since its default is `false`. |
| `low_latency=true`<br/>`interim_results=false`| The service returns final results for an utterance when they are complete, as in the previous case. But because `low_latency` is `true`, the service returns the final results more quickly, and the results can have lower accuracy. The service does not send interim results. Omitting the `interim_results` parameter has the same effect, since its default is `false`. |
| `low_latency=true`<br/>`interim_results=true` | The service returns interim results as it develops hypotheses, and it sends final results for an utterance when they are complete. The quality of interim results is identical to what the user receives with previous-generation models. But, because low latency is enabled, the service returns interim and final results faster than it does normally with next-generation models. |
{: caption="Table 3. Interaction of low_latency and interim_results parameters"}

The service does not support interim results for next-generation models that do not support low latency. It responds differently to use of the `low_latency` and `interim_results` parameters with models that do not support low-latency:

-   For previous-generation models, if you include the `low_latency` parameter with a request, the service generates a warning:

    ```javascript
    "warnings": [
      "Unknown arguments: low_latency."
    ]
    ```
    {: codeblock}

-   For next-generation models that do not support low-latency, if you include the `low_latency` parameter with a request, the service fails with status code 400:

    ```javascript
    {
      "code": 400,
      "code_description": "Bad Request",
      "error": "low_latency is not a supported feature for model {model_id}"
    }
    ```

-   For next-generation models that do not support low latency, the service does not support interim results. If you include the `interim_results` parameter with a request, the service silently ignores the parameter.

### Interim results and low-latency example
{: #interim-low-latency-example}

The following abbreviated WebSocket example requests both interim results and low latency. The request is identical to the [Interim results example](#interim-results-example) except for the following differences:

-   It specifies the `model` query parameter with the `/v1/recognize` request to pass the next-generation `en-US_Telephony` model, which supports low latency.
-   It specifies the `low_latency` parameter as `true` with the JSON `start` message for the request.

The results are the generally the same as those for the previous interim results example. They just arrive more quickly.

```javascript
var IAM_access_token = {access_token};
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=en-US_Telephony';
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true,
    low_latency: true
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
