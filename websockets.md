---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-19"

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# The WebSocket interface
{: #websockets}

The WebSocket interface of the {{site.data.keyword.speechtotextfull}} service is the most natural way for a client to interact with the service. To use the WebSocket interface for speech recognition, you first use the `/v1/recognize` method to establish a persistent connection with the service. You then send text and binary messages over the connection to initiate and manage recognition requests.
{: shortdesc}

Because of their advantages, WebSockets are the preferred mechanism for speech recognition. For more information, see [Advantages of the WebSocket interface](/docs/speech-to-text?topic=speech-to-text-service-features#features-websocket-advantages). For more information about the WebSocket interface and its parameters, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## Managing a WebSocket connection
{: #ws-managing}

The WebSocket recognition request and response cycle has the following steps:

1.  [Open a connection](#ws-open)
1.  [Initiate a recognition request](#ws-start)
1.  [Send audio and receive recognition results](#ws-audio)
1.  [End a recognition request](#ws-stop)
1.  [Send additional requests and modify request parameters](#ws-more)
1.  [Keep a connection alive](#ws-keep)
1.  [Close a connection](#ws-close)

When the client sends data to the service, it *must* pass all JSON messages as text messages and all audio data as binary messages.

The snippets of example code that follow are written in JavaScript and are based on the HTML5 WebSocket API. For more information about the WebSocket protocol, see the Internet Engineering Task Force (IETF) [Request for Comment (RFC) 6455](https://tools.ietf.org/html/rfc6455){: external}.
{: note}

### Open a connection
{: #ws-open}

The {{site.data.keyword.speechtotextshort}} service uses the WebSocket Secure (WSS) protocol to make the `/v1/recognize` method available at the following endpoint:

```
wss://api.{location}.speech-to-text.watson.cloud.ibm.com/instances/{instance_id}/v1/recognize
```
{: codeblock}

where `{location}` indicates where your application is hosted:

-   `us-south` for Dallas
-   `us-east` for Washington, DC
-   `eu-de` for Frankfurt
-   `au-syd` for Sydney
-   `jp-tok` for Tokyo
-   `eu-gb` for London
-   `kr-seo` for Seoul

And `{instance_id}` is the unique identifier of your service instance.

The examples in the documentation abbreviate `wss://api.{location}.speech-to-text.watson.cloud.ibm.com/instances/{instance_id}` to `{ws_url}`. All WebSocket examples call the method as `{ws_url}/v1/recognize`.
{: note}

A WebSocket client calls the `/v1/recognize` method with the following query parameters to establish an authenticated connection with the service. You can specify these aspects of the request only as query parameters of the WebSocket URL.

-   `access_token` (*required* string) - Pass a valid access token to establish an authenticated connection with the service. You must establish the connection before the access token expires. You pass an access token only to establish an authenticated connection. Once you establish a connection, you can keep it alive indefinitely. You remain authenticated for as long as you keep the connection open. You do not need to refresh the access token for an active connection that lasts beyond the token's expiration time. Once a connection is established, it can remain active even after the token or its credentials are deleted.

    -   ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** Pass an Identity and Access Management (IAM) access token to authenticate with the service. You pass an IAM access token instead of passing an API key with the call. For more information, see [Authenticating to {{site.data.keyword.cloud_notm}}](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-authentication-cloud).
    -   ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only.** Pass an access token as you would with the `Authorization` header of an HTTP request. For more information, see [Authenticating to {{site.data.keyword.icp4dfull_notm}}](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-authentication-icpd).

-   `model` (*optional* string) - Specifies the language model to be used for transcription. If you do not specify a model, the service uses `en-US_BroadbandModel` by default. For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models) and [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
-   `language_customization_id` (*optional* string) - Specifies the Globally Unique Identifier (GUID) of a custom language model that is to be used for all requests that are sent over the connection. The base model of the custom language model must match the value of the `model` parameter. If you include a custom language model ID, you must make the request with credentials for the instance of the service that owns the custom model. By default, no custom language model is used. For more information, see [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse).
-   `acoustic_customization_id` (*optional* string) - Specifies the GUID of a custom acoustic model that is to be used for all requests that are sent over the connection. The base model of the custom acoustic model must match the value of the `model` parameter. If you include a custom acoustic model ID, you must make the request with credentials for the instance of the service that owns the custom model. By default, no custom acoustic model is used. For more information, see [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse).
-   `base_model_version` (*optional* string) - Specifies the version of the base `model` that is to be used for all requests that are sent over the connection. The parameter is intended primarily for use with custom models that are upgraded for a new base model. The default value depends on whether the parameter is used with or without a custom model. For more information, see [Making speech recognition requests with upgraded custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-recognition).
-   `x-watson-metadata` (*optional* string) - Associates a customer ID with all data that is passed over the connection. The parameter accepts the argument `customer_id={id}`, where `id` is a random or generic string that is to be associated with the data. You must URL-encode the argument to the parameter, for example, `customer_id%3dmy_customer_ID`. By default, no customer ID is associated with the data. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).
-   `x-watson-learning-opt-out` (*optional* boolean) - ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** Indicates whether the service logs requests and results that are sent over the connection. To prevent IBM from accessing your data for general service improvements, specify `true` for the parameter. For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging).

The following snippet of JavaScript code opens a connection with the service. The call to the `/v1/recognize` method passes the `access_token` and `model` query parameters, the latter to direct the service to use the Spanish broadband model. After it establishes the connection, the client defines the event listeners (`onOpen`, `onClose`, and so on) to respond to events from the service. The client can use the connection for multiple recognition requests.

```javascript
var access_token = '{access_token}';
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

The client can open multiple concurrent WebSocket connections to the service. The number of concurrent connections is limited only by the capacity of the service, which generally poses no problems for users.

### Initiate a recognition request
{: #ws-start}

To initiate a recognition request, the client sends a JSON text message to the service over the established connection. The client must send this message before it sends any audio for transcription. The message must include the `action` parameter but can usually omit the `content-type` parameter.

-   `action` (*required* string) - Specifies the action to be performed:
    -   `start` begins a recognition request. It can also specify new parameters for subsequent requests. For more information, see [Send additional requests and modify request parameters](#ws-more).
    -   `stop` signals that all audio for a request has been sent. For more information, see [End a recognition request](#ws-stop).
-   `content-type` (*optional* string) - Identifies the format (MIME type) of the audio data for the request. The parameter is required for the `audio/alaw`, `audio/basic`, `audio/l16`, and `audio/mulaw` formats. For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list).

The message can also include optional parameters to specify other aspects of how the request is to be processed and the information that is to be returned. These additional parameters include the `interim_results` parameter, which is available only with the WebSocket interface.

-   For more information about all speech recognition features, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
-   For more information about the `interim_results` parameter, see [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results).

The following snippet of JavaScript code sends initialization parameters for the recognition request over the WebSocket connection. The calls are included in the client's `onOpen` function to ensure that they are sent only after the connection is established.

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

If it receives the request successfully, the service returns the following text message to indicate that it is `listening`. The `listening` state indicates that the service instance is configured (the JSON `start` message was valid) and is ready to accept audio for a recognition request.

```javascript
{'state': 'listening'}
```
{: codeblock}

If the client specifies an invalid query parameter or JSON field for the recognition request, the service's JSON response includes a `warnings` field. The field describes each invalid argument. The request succeeds despite the warnings.

### Send audio and receive recognition results
{: #ws-audio}

After it sends the initial `start` message, the client can begin to send audio data to the service. The client does not need to wait for the service to respond to the `start` message with the `listening` message. Once it begins listening, the service processes any audio that was sent before the `listening` message.

The client must send the audio as binary data. The client can send a maximum of 100 MB of audio data per `send` request. It must send at least 100 bytes of audio for any request. The client can send multiple requests over a single WebSocket connection. For information about using compression to maximize the amount of audio that you can pass to the service with a request, see [Data limits and compression](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-limits).

The WebSocket interface imposes a maximum frame size of 4 MB. The client can set the maximum frame size to less than 4 MB. If it is not practical to set the frame size, the client can set the maximum message size to less than 4 MB and send the audio data as a sequence of messages. For more information about WebSocket frames, see [IETF RFC 6455](https://tools.ietf.org/html/rfc6455){: external}.

How the service sends recognition results over a WebSocket connection depends on whether the client requests interim results. For more information, see [How the service sends recognition results](#ws-results).

The following snippet of JavaScript code sends audio data to the service as a binary message (blob):

```javascript
websocket.send(blob);
```
{: codeblock}

The following snippet receives recognition hypotheses that the service returns asynchronously. The results are handled by the client's `onMessage` function.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

Your code must be prepared to handle return return codes from the service. For more information, see [WebSocket return codes](#ws-return).

### End a recognition request
{: #ws-stop}

When it is done sending the audio data for a request to the service, the client *must* signal the end of the binary audio transmission to the service in one of the following ways:

-   By sending a JSON text message with the `action` parameter set to the value `stop`:

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   By sending an empty binary message, one in which the specified blob is empty:

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

If the client fails to signal that the transmission is complete, the connection can time out without the service sending final results. To receive final results between multiple recognition requests, the client must signal the end of transmission for the previous request before it sends a subsequent request. After it returns the final results for the first request, the service returns another `{"state":"listening"}` message to the client. This message indicates that the service is ready to receive another request.

### Send additional requests and modify request parameters
{: #ws-more}

While the WebSocket connection remains active, the client can continue to use the connection to send further recognition requests with new audio. By default, the service continues to use the parameters that were sent with the previous `start` message for all subsequent requests that are sent over the same connection.

To change the parameters for subsequent requests, the client can send another `start` message with the new parameters after it receives the final recognition results and a new `{"state":"listening"}` message from the service. The client can change any parameters except for those parameters that are specified when the connection is opened (`model`, `language_customization_id`, and so on).

The following example sends a `start` message with new parameters for subsequent recognition requests that are sent over the connection. The message specifies the same `content-type` as the previous example, but it directs the service to return confidence measures and timestamps for the words of the transcription.

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

### Keep a connection alive
{: #ws-keep}

The service terminates the session and closes the connection if an inactivity or session timeout occurs:

-   An *inactivity timeout* occurs if audio is being sent by the client but the service detects no speech. The inactivity timeout is 30 seconds by default. You can use the `inactivity_timeout` parameter to specify a different value, including `-1` to set the timeout to infinity. For more information, see [Inactivity timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
-   A *session timeout* occurs if the service receives no data from the client or sends no interim results for 30 seconds. You cannot change the length of this timeout, but you can extend the session by sending the service any audio data, including just silence, before the timeout occurs. You must also set the `inactivity_timeout` to `-1`. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session. For more information, see [Session timeout](/docs/speech-to-text?topic=speech-to-text-input#timeouts-session).

WebSocket clients and servers can also exchange *ping-pong frames* to avoid read timeouts by periodically exchanging small amounts of data. Many WebSocket stacks exchange ping-pong frames, but some do not. To determine whether your implementation uses ping-pong frames, check its list of features. You cannot programmatically determine or manage ping-pong frames.

If your WebSocket stack does not implement ping-pong frames and your are sending long audio files, your connection can experience a read timeout. To avoid such timeouts, continuously stream audio to the service or request interim results from the service. Either approach can ensure that the lack of ping-pong frames does not cause your connection to close.

For more information about ping-pong frames, see [Section 5.5.2 Ping](http://tools.ietf.org/html/rfc6455#section-5.5.2){: external} and [Section 5.5.3 Pong](https://tools.ietf.org/html/rfc6455#section-5.5.3){: external} of IETF RFC 6455.

### Close a connection
{: #ws-close}

When the client is done interacting with the service, it can close the WebSocket connection. Once the connection is closed, the client can no longer use it to send requests or to receive results. Close the connection only after the client receives all results for a request. The connection eventually times out and closes if the client does not explicitly close it.

The following snippet of JavaScript code closes an open connection:

```javascript
websocket.close();
```
{: codeblock}

## How the service sends recognition results
{: #ws-results}

How the service sends speech recognition results to the client depends on whether the client requests interim results. In the JSON response to a request, final results are labeled `"final": true`, and interim results are labeled `"final": false`. For more information, see [Interim results](/docs/speech-to-text?topic=speech-to-text-interim#interim-results).

In the following examples, the results show the service's response for the same input audio submitted both with and without interim results. The audio speaks the phrase "one two &lt;*pause*&gt; three four," with a one-second pause between the words "two" and "three." The pause is long enough to represent separate utterances. An utterance is a component of the input audio that elicits a response, usually as a result of extended silence. For more information, see [Understanding speech recognition results](/docs/speech-to-text?topic=speech-to-text-basic-response).

If your results include multiple final results, concatenate the `transcript` elements of the final results to assemble the complete transcription of the audio. For more information, see [The result_index field](/docs/speech-to-text?topic=speech-to-text-basic-response#response-result-index).

### Example request without interim results
{: #ws-results-without-interim}

The client disables interim results by setting the `interim_results` parameter to `false` or by omitting the parameter from a request (the default argument for the parameter is `false`). The client receives a single JSON object in response only after it sends a `stop` message.

The response object can contain multiple final results for separate utterances of the audio. The service does not send the single response object until it receives a `stop` message to indicate that audio transmission for the request is complete. The  structure and format of the service's response is the same regardless of whether you use a previous- or next-generation model.

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "one two "
        }
      ],
      "final": true
    },
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "three four "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

### Example request with interim results
{: #ws-results-with-interim}

The client requests interim results as follows:

-   *For previous-generation models,* by setting the `interim_results` parameter to `true`.
-   *For next-generation models,* by setting both the `interim_results` and `low_latency` parameters to `true`. To receive interim results with a next-generation model, the model must support low latency and both the `interim_results` and `low_latency` parameters must be set to `true`.
    -   For more information about which next-generation models support low latency, see [Supported next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).
    -   For more information about the interaction of the `interim_results` and `low_latency` parameters with next-generation models, including  examples that demonstrate the different combinations of parameters, see [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency).

The client receives multiple JSON objects in response. The service returns separate response objects for each interim result and for each each final result that is generated by the audio. The service sends at least one interim result for each final result.

The service sends responses as soon as they are available. It does not wait for a `stop` message to send its results, though the `stop` message is still required to signal the end of transmission for the request. The structure and format of the service's response is the same regardless of whether you use a previous- or next-generation model.

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "transcript": "one "
        }
      ],
      "final": false
    }
  ]
}{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "transcript": "one two "
        }
      ],
      "final": false
    }
  ]
}{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "one two "
        }
      ],
      "final": true
    }
  ]
}{
  "result_index": 1,
  "results": [
    {
      "alternatives": [
        {
          "transcript": "three "
        }
      ],
      "final": false
    }
  ]
}{
  "result_index": 1,
  "results": [
    {
      "alternatives": [
        {
          "transcript": "three four "
        }
      ],
      "final": false
    }
  ]
}{
  "result_index": 1,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": "three four "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Example WebSocket exchanges
{: #ws-examples}

The following examples show a series of exchanges between a client and the {{site.data.keyword.speechtotextshort}} service over a single WebSocket connection. The examples focus on the exchange of messages and data. They do not show opening and closing the connection. (The examples are based on a previous-generation model, so the final transcript for each response includes a `confidence` field.)

### First example exchange
{: #ws-first-example}

In the first exchange, the client sends audio that contains the string `Name the Mayflower`. The client sends a binary message with a single chunk of PCM (`audio/l16`) audio data, for which it indicates the required sampling rate. The client does not wait for the `{"state":"listening"}` response from the service to begin sending the audio data and to signal the end of the request. Sending the data immediately reduces latency because the audio is available to the service as soon as it is ready to handle a recognition request.

-   The client sends:

    ```json
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   The service responds:

    ```json
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower ",
                 "confidence": 0.91}], "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Second example exchange
{: #ws-second-example}

In the second exchange, the client sends audio that contains the string `Second audio transcript`. The client sends the audio in a single binary message and uses the same parameters that it specified in the first request.

-   The client sends:

    ```json
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   The service responds:

    ```json
    {"results": [{"alternatives": [{"transcript": "second audio transcript ",
                 "confidence": 0.99}], "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Third example exchange
{: #ws-third-example}

In the third exchange, the client again sends audio that contains the string `Name the Mayflower`. It sends a binary message with a single chunk of PCM audio data. But this time, the client sends a new `start` message that requests interim results from the service.

-   The client sends:

    ```json
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   The service responds:

    ```json
    {"results": [{"alternatives": [{"transcript": "name "}],
                 "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                 "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                 "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower ",
                 "confidence": 0.91}], "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

## WebSocket return codes
{: #ws-return}
{: troubleshoot}
{: support}

The service can send the following return codes to the client over the WebSocket connection:

-   `1000` indicates normal closure of the connection, meaning that the purpose for which the connection was established has been fulfilled.
-   `1002` indicates that the service is closing the connection due to a protocol error.
-   `1006` indicates that the connection closed abnormally.
-   `1009` indicates that the frame size exceeded the 4 MB limit.
-   `1011` indicates that the service is terminating the connection because it encountered an unexpected condition that prevents it from fulfilling the request.

If the socket closes with an error, the client receives an informative message of the form `{"error":"{message}"}` before the socket closes. Use the `onerror` event handler to respond appropriately. For more information about WebSocket return codes, see [IETF RFC 6455](https://tools.ietf.org/html/rfc6455){: external}.

The WebSocket implementations of the SDKs can return different or additional response codes. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
{: note}
