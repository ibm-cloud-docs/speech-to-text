---

copyright:
  years: 2015, 2019
lastupdated: "2019-12-12"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
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

# The WebSocket interface
{: #websockets}

The WebSocket interface of the {{site.data.keyword.speechtotextshort}} service is the most natural way for a client to interact with the service. To use the WebSocket interface for speech recognition, you first use the `/v1/recognize` method to establish a persistent connection with the service. You then send text and binary messages over the connection to initiate and manage the recognition requests.
{: shortdesc}

The recognition request and response cycle has the following steps:

1.  [Open a connection](#WSopen)
1.  [Initiate a recognition request](#WSstart)
1.  [Send audio and receive recognition results](#WSaudio)
1.  [End a recognition request](#WSstop)
1.  [Send additional requests and modify request parameters](#WSmore)
1.  [Keep a connection alive](#WSkeep)
1.  [Close a connection](#WSclose)

When the client sends data to the service, it *must* pass all JSON messages as text messages and all audio data as binary messages.

The snippets of example code that follow are written in JavaScript and are based on the HTML5 WebSocket API. For more information about the WebSocket protocol, see the Internet Engineering Task Force (IETF) [Request for Comment (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}.
{: note}

## Open a connection
{: #WSopen}

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

And `{instance_id}` is the unique identifier of the service instance.

The examples in the documentation abbreviate `wss://api.{location}.speech-to-text.watson.cloud.ibm.com/instances/{instance_id}` to `{ws_url}`. So all WebSocket examples call the method as `{ws_url}/v1/recognize`.
{: note}

A WebSocket client calls this method with the following query parameters to establish an authenticated connection with the service. If you use Identity and Access Management (IAM) authentication, use the `access_token` query parameter. If you use Cloud Foundry service credentials, use the `watson-token` query parameter.

<table>
  <caption>Table 1. Parameters of the <code>/v1/recognize</code>
    method</caption>
  <tr>
    <th style="width:23%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      <em>If you use IAM authentication,</em> pass a valid IAM access
      token to establish an authenticated connection with the service.
      You pass an IAM access token instead of passing an API key with
      the call. You must establish the connection before the access
      token expires. For information about obtaining an access token, see
      [Authenticating to Watson services](/docs/services/watson?topic=watson-iam).<br/><br/>
      You pass an access token only to establish an authenticated connection.
      Once you establish a connection, you can keep it alive indefinitely.
      You remain authenticated for as long as you keep the connection open.
      You do not need to refresh the access token for an active connection
      that lasts beyond the token's expiration time. A connection can remain
      active even after the token or its API key are deleted.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      <em>If you use Cloud Foundry service credentials,</em> pass a valid
      {{site.data.keyword.watson}} authentication token to establish an
      authenticated connection with the service. You pass a
      {{site.data.keyword.watson}} token instead of passing service
      credentials with the call. {{site.data.keyword.watson}} tokens are
      based on Cloud Foundry service credentials, which use a `username`
      and `password` for HTTP basic authentication. For information about
      obtaining a {{site.data.keyword.watson}} token, see
      [{{site.data.keyword.watson}} tokens](/docs/services/watson?topic=watson-gs-tokens-watson-tokens).<br/><br/>
      You pass a {{site.data.keyword.watson}} token only to establish an
      authenticated connection. Once you establish a connection, you can
      keep it alive indefinitely. You remain authenticated for as long as
      you keep the connection open.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the language model to be used for transcription.
      If you do not specify a model, the service uses the
      <code>en-US_BroadbandModel</code> model by default. For more
      information, see
      [Languages and models](/docs/services/speech-to-text?topic=speech-to-text-models).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the Globally Unique Identifier (GUID) of a custom
      language model that is to be used for all requests that are sent
      over the connection. The base model of the custom language model
      must match the value of the <code>model</code> parameter. If you
      include a customization ID, you must make the request with credentials
      for the instance of the service that owns the custom model. By
      default, no custom language model is used. For more information, see
      [The customization interface](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the Globally Unique Identifier (GUID) of a custom
      acoustic model that is to be used for all requests that are sent
      over the connection. The base model of the custom acoustic model
      must match the value of the <code>model</code> parameter. If you
      include a customization ID, you must make the request with credentials
      for the instance of the service that owns the custom model. By
      default, no custom acoustic model is used. For more information, see
      [The customization interface](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the version of the base `model` that is to be used for all
      requests that are sent over the connection. The parameter is intended
      primarily for use with custom models that are upgraded for a new base
      model. The default value depends on whether the parameter is used
      with or without a custom model. For more information, see
      [Base model version](/docs/services/speech-to-text?topic=speech-to-text-input#version).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">Boolean</td>
    <td style="text-align:left">
      Indicates whether the service logs requests and results that are sent
      over the connection. To prevent IBM from accessing your data for general
      service improvements, specify <code>true</code> for the parameter. For
      more information, see
      [Request logging](/docs/services/speech-to-text?topic=speech-to-text-input#logging).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Associates a customer ID with all data that is passed over the
      connection. The parameter accepts the argument
      <code>customer_id={id}</code>, where <code>id</code> is a random
      or generic string that is to be associated with the data. You must
      URL-encode the argument to the parameter, for example,
      `customer_id%3dmy_customer_ID`. By default, no customer ID is associated
      with the data. For more information, see
      [Information security](/docs/services/speech-to-text?topic=speech-to-text-information-security).
    </td>
  </tr>
</table>

The following snippet of JavaScript code opens a connection with the service. The call to the `/v1/recognize` method passes the `access_token` and `model` query parameters, the latter to direct the service to use the Spanish broadband model. After it establishes the connection, the client defines the event listeners (`onOpen`, `onClose`, and so on) to respond to events from the service. The client can use the connection for multiple recognition requests.

```javascript
var IAM_access_token = '{access_token}';
var wsURI = '{ws_url}/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

The client can open multiple concurrent WebSocket connections to the service. The number of concurrent connections is limited only by the capacity of the service, which generally poses no problems for users.

## Initiate a recognition request
{: #WSstart}

To initiate a recognition request, the client sends a JSON text message to the service over the established connection. The client must send this message before it sends any audio for transcription. The message must include the `action` parameter but can usually omit the `content-type` parameter.

<table>
  <caption>Table 2. Parameters of the JSON text message</caption>
  <tr>
    <th style="width:23%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the action to be performed:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> starts a recognition request or specifies
          new parameters for subsequent requests. For more information, see
          [Send additional requests and modify request parameters](#WSmore).
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> signals that all audio for a request has
          been sent. For more information, see
          [End a recognition request](#WSstop).
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Identifies the format (MIME type) of the audio data for the request.
      The parameter is required for the `audio/alaw`, `audio/basic`,
      `audio/l16`, and `audio/mulaw` formats. For more information, see
      [Audio formats](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
    </td>
  </tr>
</table>

The message can also include optional parameters to specify other aspects of how the request is to be processed and the information that is to be returned. For information about all input and output features, see the [Parameter summary](/docs/services/speech-to-text?topic=speech-to-text-summary). You can specify a language model, custom language model, and custom acoustic model only as query parameters of the WebSocket URL.

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

If it receives the request successfully, the service returns the following text message to indicate that it is `listening`:

```javascript
{'state': 'listening'}
```
{: codeblock}

The `listening` state indicates that the service instance is configured (your JSON `start` message was valid) and is ready to process a new utterance for a recognition request. Once it begins listening, the service processes any audio that was sent before the `listening` message.

If the client specifies an invalid query parameter or JSON field for the recognition request, the service's JSON response includes a `warnings` field. The field describes each invalid argument. The request succeeds despite the warnings.

## Send audio and receive recognition results
{: #WSaudio}

After it sends the initial `start` message, the client can begin sending audio data to the service. The client does not need to wait for the service to respond to the `start` message with the `listening` message. The service returns the results of the transcription asynchronously in the same format as it returns results for the HTTP interfaces.

The client must send the audio as binary data. The client can send a maximum of 100 MB of audio data with a single utterance (per `send` request). It must send at least 100 bytes of audio for any request. The client can send multiple utterances over a single WebSocket connection. For information about using compression to maximize the amount of audio that you can pass to the service with a request, see [Audio formats](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

The WebSocket interface imposes a maximum frame size of 4 MB. The client can set the maximum frame size to less than 4 MB. If it is not practical to set the frame size, the client can set the maximum message size to less than 4 MB and send the audio data as a sequence of messages. For more information about WebSocket frames, see [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

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

## End a recognition request
{: #WSstop}

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

The service does not send final results until it receives confirmation that the audio transmission is complete. If you fail to signal that the transmission is complete, the connection can time out without the service sending final results.

To receive final results between multiple recognition requests, the client must signal the end of transmission for the previous request before it sends a subsequent request. After it returns the final results for the first request, the service returns another `{"state":"listening"}` message to the client. This message indicates that the service is ready to receive another request.

## Send additional requests and modify request parameters
{: #WSmore}

While the WebSocket connection remains active, the client can continue to use the connection to send further recognition requests with new audio. By default, the service continues to use the parameters that were sent with the previous `start` message for all subsequent requests that are sent over the same connection.

To change the parameters for subsequent requests, the client can send another `start` message with the new parameters after it receives the final recognition results and `{"state":"listening"}` message from the service. The client can change any parameters except for those parameters that are specified when the connection is opened (`model`, `language_customization_id`, and so on).

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

## Keep a connection alive
{: #WSkeep}

The service terminates the session and closes the connection if an inactivity or session timeout occurs:

-   An *inactivity timeout* occurs if audio is being sent by the client but the service detects no speech. The inactivity timeout is 30 seconds by default. You can use the `inactivity_timeout` parameter to specify a different value, including `-1` to set the timeout to infinity. For more information, see [Inactivity timeout](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
-   A *session timeout* occurs if the service receives no data from the client or sends no interim results for 30 seconds. You cannot change the length of this timeout, but you can extend the session by sending the service any audio data, including just silence, before the timeout occurs. You must also set the `inactivity_timeout` to `-1`. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session. For more information, see [Session timeout](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-session).

WebSocket clients and servers can also exchange *ping-pong frames* to avoid read timeouts by periodically exchanging small amounts of data. Many WebSocket stacks exchange ping-pong frames, but some do not. To determine whether your implementation uses ping-pong frames, check its list of features. You cannot programmatically determine or manage ping-pong frames.

If your WebSocket stack does not implement ping-pong frames and your are sending long audio files, your connection can experience a read timeout. To avoid such timeouts, continuously stream audio to the service or request interim results from the service. Either approach can ensure that the lack of ping-pong frames does not cause your connection to close.

For more information about ping-pong frames, see [Section 5.5.2 Ping](http://tools.ietf.org/html/rfc6455#section-5.5.2){: external} and [Section 5.5.3 Pong](http://tools.ietf.org/html/rfc6455#section-5.5.3){: external} of IETF RFC 6455.

## Close a connection
{: #WSclose}

When the client is done interacting with the service, it can close the WebSocket connection. Once the connection is closed, the client can no longer use it to send requests or to receive results. Close the connection only after you receive all results for a request. The connection eventually times out and closes if you do not explicitly close it.

The following snippet of JavaScript code closes an open connection:

```javascript
websocket.close();
```
{: codeblock}

## WebSocket return codes
{: #WSreturn}

The service can send the following return codes to the client over the WebSocket connection:

-   `1000` indicates normal closure of the connection, meaning that the purpose for which the connection was established has been fulfilled.
-   `1002` indicates that the service is closing the connection due to a protocol error.
-   `1006` indicates that the connection closed abnormally.
-   `1009` indicates that the frame size exceeded the 4 MB limit.
-   `1011` indicates that the service is terminating the connection because it encountered an unexpected condition that prevents it from fulfilling the request.

If the socket closes with an error, the client receives an informative message of the form `{"error":"{message}"}` before the socket closes. For more information about WebSocket return codes, see [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

## Example WebSocket session
{: #WSexample}

The following example shows a WebSocket session between a client and the {{site.data.keyword.speechtotextshort}} service. The session is shown as three separate exchanges to make it easier to follow. But all three exchanges are part of a single session with the service. The example focuses on the exchange of messages; it does not show opening and closing the connection.

### First example exchange
{: #firstExample}

In the first exchange, the client sends audio that contains the string `Name the Mayflower`. The client sends a binary message with a single chunk of PCM (`audio/l16`) audio data, for which it indicates the required sampling rate. The client does not wait for the `{"state":"listening"}` response from the service to begin sending the audio data and to signal the end of the request. Sending the data immediately reduces latency because the audio is available to the service as soon as it is ready to handle a recognition request.

-   The *client* sends:

    ```javascript
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

-   The *service* responds:

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Second example exchange
{: #secondExample}

In the second exchange, the client sends audio that contains the string `Second audio transcript`. The client sends the audio in a single binary message and uses the same parameters that it specified in the first request.

-   The *client* sends:

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   The *service* responds:

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Third example exchange
{: #thirdExample}

In the third exchange, the client again sends audio that contains the string `Name the Mayflower`. The client again sends a binary message with a single chunk of PCM audio data. But this time, the client requests interim results with the transcription.

-   The *client* sends:

    ```javascript
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

-   The *service* responds:

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
