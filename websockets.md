---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-19"

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

> **Note:** The snippets of example code that follow are written in JavaScript and are based on the HTML5 WebSocket API. For more information about the WebSocket protocol, see the Internet Engineering Task Force (IETF) [Request for Comment (RFC) 6455 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://tools.ietf.org/html/rfc6455){: new_window}.

## Advantages of the WebSocket interface
{: #advantages}

The WebSocket interface has a number of advantages over the HTTP interface:

-   The WebSocket interface, unlike the REST interface, provides a single-socket, full-duplex communication channel. The interface lets the client send requests and audio to the service and receive results over a single connection in an asynchronous fashion.
-   It provides a much simpler and more powerful programming experience. The service sends event-driven responses to the client's messages, eliminating the need for the client to poll the server.
-   It reduces latency. Recognition results arrive faster because the service sends them directly to the client.
-   It reduces network utilization. The WebSocket protocol is lightweight. It requires only a single connection to perform live recognition. Conversely, when you use sessions with the REST interface, you need at least four connections to achieve the same results.
-   It enables audio to be streamed directly from browsers (HTML5 WebSocket clients) to the service.

## Open a connection
{: #WSopen}

The {{site.data.keyword.speechtotextshort}} service uses the WebSocket Secure (WSS) protocol to make the `/v1/recognize` method available at the following endpoint:

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

where `{host_name}` identifies the regional host for your application:

-   `stream.watsonplatform.net` for US South and UK (the following examples use this host name)
-   `stream-fra.watsonplatform.net` for Germany
-   `gateway-syd.watsonplatform.net` for Sydney and AP North
-   `gateway-wdc.watsonplatform.net` for US East

A WebSocket client calls this method with the following query parameters to establish an authenticated connection with the service.

<table>
  <caption>Table 1. Parameters of the <code>/v1/recognize</code>
    method</caption>
  <tr>
    <th style="width:23%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Passes a valid authentication token instead of passing the service
      credentials with the call. You can instead use the
      <code>X-Watson-Authorization-Token</code> header to pass the token,
      but you must pass a token in one of these two ways.<br/><br/>
      You pass a token only to establish an authenticated connection with
      the service. Once you establish a connection, you can keep it alive
      indefinitely. While the connection remains open, you do not need to
      pass the token with subsequent calls. See
      <a href="/docs/services/speech-to-text/input.html#tokens">Authentication
      tokens</a>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the language model to be used for transcription.
      If you do not specify a model, the service uses the
      <code>en-US_BroadbandModel</code> model by default. See
      <a href="/docs/services/speech-to-text/input.html#models">Languages
        and models</a>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>customization_id</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the Globally Unique Identifier (GUID) of a custom
      language model that is to be used for all requests that are sent
      over the connection. The base model of the custom language model
      must match the value of the <code>model</code> parameter. By
      default, no custom language model is used. See
      <a href="/docs/services/speech-to-text/custom.html">The
      customization interface</a>.
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
      must match the value of the <code>model</code> parameter. By
      default, no custom acoustic model is used. See
      <a href="/docs/services/speech-to-text/custom.html">The
      customization interface</a>.
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
      with or without a custom model. See
      <a href="/docs/services/speech-to-text/input.html#version">Base model
      version</a>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">Boolean</td>
    <td style="text-align:left">
      Specifies whether the service logs requests and results that are sent
      over the connection. Logging is done only to improve the service for
      future users. The logged data is not shared or made public. To prevent
      IBM from accessing your data for general service improvements, specify
      <code>true</code> for the parameter. See
      <a href="/docs/services/speech-to-text/input.html#logging">Request
      logging</a>.
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
      `customer_id%3dmy_ID`. By default, no customer ID is associated
      with the data. See
      <a href="/docs/services/speech-to-text/information-security.html">Information
      security</a>.
    </td>
  </tr>
</table>

The following snippet of JavaScript code opens a connection with the service. The call to the `/v1/recognize` method passes the `watson-token` and `model` query parameters, the latter to direct the service to use the Spanish broadband model. After it establishes the connection, the client defines the event listeners (`onOpen`, `onClose`, and so on) to respond to events from the service. The client can use the connection for multiple recognition requests.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?watson-token=' + token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

## Initiate a recognition request
{: #WSstart}

To initiate a recognition request, the client sends a JSON text message to the service over the established connection. The client must send this message before it sends any audio for transcription. The message must include the following two parameters.

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
          new parameters for subsequent requests. See
          <a href="#WSmore">Send additional requests and modify request
            parameters</a>.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> signals that all audio for a request has
          been sent. See <a href="#WSstop">End a recognition request</a>.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Identifies the format (MIME type) of the audio data for the request.
      For more information about the available audio formats, see
      <a href="/docs/services/speech-to-text/audio-formats.html">Audio
        formats</a>.
    </td>
  </tr>
</table>

The message can also include optional parameters to specify other aspects of how the request is to be processed and the information that is to be returned. For more information, see [Input features](/docs/services/speech-to-text/input.html) and [Output features](/docs/services/speech-to-text/output.html). You can specify a language model, custom language model, and custom acoustic model only as query parameters of the WebSocket URL.

The following snippet of JavaScript code sends initialization parameters for the recognition request over the WebSocket connection. The calls are included in the client's `onOpen` function to ensure that they are sent only after the connection is established.

```javascript
function onOpen(evt) {
  var message = {
    'action': 'start',
    'content-type': 'audio/l16;rate=22050'
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

After it sends the initial `start` message, the client can begin sending audio data to the service. The client does not need to wait for the service to respond to the `start` message with the `listening` message. The service returns the results of the transcription asynchronously in the same format as it returns results for the HTTP API.

The client must send the audio as binary data. The client can stream a maximum of 100 MB of audio data with a single utterance (per `send` request). The client can send multiple utterances over a single WebSocket connection.

The WebSocket interface imposes a maximum frame size of 4 MB. The client can set the maximum frame size to less than 4 MB. If it is not practical to set the frame size, the client can set the maximum message size to less than 4 MB and send the audio data as a sequence of messages.

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

When it is done sending the audio data for a request to the service, the client *must* signal the end of the binary transmission to the service:

-   By sending a JSON text message with the `action` parameter set to the value `stop`:

    ```javascript
    {'action': 'stop'}
    ```
    {: codeblock}

-   By sending an empty binary message, one in which the specified blob is empty:

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

After it returns the final result for the transcription to the client, the service returns another `{"state":"listening"}` message to the client. This message indicates that the service is ready to receive another recognition request. Before it sends another request, the client must signal the end of transmission for the previous request. Otherwise, the service returns no new results.

## Send additional requests and modify request parameters
{: #WSmore}

While the WebSocket connection remains active, the client can continue to use the connection to send further recognition requests with new audio. By default, the service continues to use the parameters that were sent with the previous `start` message for all subsequent requests that are sent over the same connection.

To change the parameters for subsequent requests, the client can send another `start` message with the new parameters after it receives the final recognition results and `{"state":"listening"}` message from the service. The client can change any parameters except for those parameters that are specified when the connection is opened (`model`, `customization_id`, and so on).

The following example sends a `start` message with new parameters for subsequent recognition requests that are sent over the connection. The message specifies the same `content-type` as the previous example, but it directs the service to return confidence measures and timestamps for the words of the transcription.

```javascript
var message = {
  'action': 'start',
  'content-type': 'audio/l16;rate=22050',
  'word_confidence': true,
  'timestamps': true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## Keep a connection alive
{: #WSkeep}

The service terminates the session and closes the connection if an inactivity or session timeout is reached:

-   An *inactivity timeout* occurs if audio is being sent by the client but the service detects no speech. The inactivity timeout is 30 seconds by default. You can use the `inactivity_timeout` parameter to specify a different value, including `-1` to set the timeout to infinity.
-   A *session timeout* occurs if the service receives no data from the client or sends no interim results for 30 seconds. You cannot change the length of this timeout, but you can extend the session by sending the service any audio data, including just silence, before the timeout occurs. You must also set the `inactivity_timeout` to `-1`. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

For more information, see [Timeouts](/docs/services/speech-to-text/input.html#timeouts).

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

If the socket closes with an error, the client receives an informative message of the form `{"error":"{message}"}` before the socket closes. For more information about WebSocket return codes, see [IETF RFC 6455 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://tools.ietf.org/html/rfc6455){: new_window}.

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
