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

# The WebSocket interface
{: #websockets}

The WebSocket interface of the {{site.data.keyword.speechtotextshort}} service is the most natural way for a client to interact with the service. It has a number of advantages over the HTTP interface:
{: shortdesc}

-   The WebSocket interface, unlike the REST interface, provides a single-socket, full-duplex communication channel. The interface lets the client send requests and audio to the service and receive results over a single connection in an asynchronous fashion.
-   It provides a much simpler and more powerful programming experience. The service can send event-driven responses to the client's messages, eliminating the need for the client to poll the server.
-   It reduces latency. Recognition results arrive faster because the service sends them directly to the client.
-   It reduces network utilization. The WebSocket protocol is very lightweight. It requires only a single connection to perform live recognition. When using sessions with the REST interface, conversely, you need at least four connections to achieve the same results.
-   It enables audio to be streamed directly from browsers (HTML5 WebSocket clients) to the service.

The WebSocket interface uses the `recognize` method to establish a connection with the service. It then relies on text and binary messages sent over the persistent connection to initiate and manage recognition requests. (If your application needs to call the `GET /v1/models` method, you must use the HTTP interface.)

For information about the steps you need to follow to use the WebSocket interface, see [Making recognition requests](#WSsteps). Subsequent sections describe [WebSocket return codes](#WSreturn) and present an [Example WebSocket session](#WSexample) that shows example messages exchanged between the client and the service.

> **Note:** The snippets of example code in the following sections are written in JavaScript and are based on the HTML5 WebSocket API. For more information about the WebSocket protocol, see the Internet Engineering Task Force (IETF) [Request for Comment (RFC) 6455 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://tools.ietf.org/html/rfc6455){: new_window}.

## Making recognition requests
{: #WSsteps}

The recognition request and response cycle comprises the following steps:

1.  [Opening a connection and passing credentials](#WSopen)
1.  [Initiating a recognition request](#WSstart)
1.  [Sending audio and receiving recognition results](#WSaudio)
1.  [Ending a recognition request](#WSstop)
1.  [Sending additional requests and modifying parameters](#WSmore)
1.  [Keeping a connection alive](#WSkeep)
1.  [Closing a connection](#WSclose)

When the client sends data to the service, it *must* pass all JSON messages as text messages and all audio data as binary messages.

### Opening a connection and passing credentials
{: #WSopen}

The {{site.data.keyword.speechtotextshort}} service uses the WebSocket Secure (WSS) protocol to make the `recognize` method available at the following endpoint:

```
wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize
```
{: codeblock}

A WebSocket client calls this method with the following query parameters to establish an authenticated connection with the service.

<table>
  <caption>Table 1. Parameters of the <code>recognize</code>
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
      but you must pass a token in one of these two ways. You pass a token
      only to establish an authenticated connection with the service. Once
      you establish a connection, you can keep it alive indefinitely. As
      long as the connection remains open, you do not need to pass the
      token with subsequent calls. For more information, see
      <a href="/docs/services/watson/getting-started-tokens.html">Tokens
        for authentication</a>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the language and model to be used for transcription.
      If you do not specify a model, the service uses the
      <code>en-US_BroadbandModel</code> model by default. For more
      information, see
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
      language model that is to be used for all requests sent over
      the connection. The base model of the custom language model
      must match the value of the <code>model</code> parameter. By
      default, no custom language model is used. For more information, see
      <a href="/docs/services/speech-to-text/input.html#custom">Custom
        models</a>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Specifies the Globally Unique Identifier (GUID) of a custom
      acoustic model that is to be used for all requests sent over
      the connection. The base model of the custom acoustic model
      must match the value of the <code>model</code> parameter. By
      default, no custom acoustic model is used. For more information, see
      <a href="/docs/services/speech-to-text/input.html#custom">Custom
        models</a>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">Boolean</td>
    <td style="text-align:left">
      Specifies whether the service logs requests and results sent over
      the connection. Logging is done only to improve the service for
      future users. The logged data is not shared or made public. To
      prevent IBM from accessing your data for general service
      improvements, specify <code>true</code> for the parameter.
      You can also opt out of request logging by passing a value of
      <code>true</code> with the <code>X-Watson-Learning-Opt-Out</code>
      request header. For more information, see
      <a href="/docs/services/watson/getting-started-logging.html">Controlling
        request logging for Watson services</a>.
    </td>
  </tr>
</table>

The following snippet of JavaScript code opens a connection with the service. The call to the `recognize` method passes the `watson-token` and `model` query parameters, the latter to direct the service to use the Spanish broadband model. Once the connection is established, the event listeners (`onOpen`, `onClose`, and so on) are defined to respond to events from the service, and the client can use the connection for multiple recognition requests.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize?watson-token=' +
  token + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

### Initiating a recognition request
{: #WSstart}

To initiate a recognition request, the client sends a JSON text message to the service over the established connection. The client must send this message before it sends any audio to be transcribed. The message must include the following two parameters.

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
      The action to be performed:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> initiates a recognition request or specifies
          new parameters for subsequent requests (see
          <a href="#WSmore">Sending additional requests and modifying
            parameters</a>).
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> signals that all audio for a request has
          been sent (see <a href="#WSstop">Ending a recognition
            request</a>).
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      The format (MIME type) of the audio data for the request. For
      detailed information about the available audio formats, see
      <a href="/docs/services/speech-to-text/input.html#formats">Audio
        formats</a>.
    </td>
  </tr>
</table>

The message can also include optional parameters to specify additional aspects of how the request is to be processed and the information that is to be returned. For more information, see [Input features and parameters](/docs/services/speech-to-text/input.html) and [Output features and parameters](/docs/services/speech-to-text/output.html). Note that a language model and the ID of a custom language model can be specified only as query parameters of the WebSocket URL. (The same is true of the authentication token and the parameter that controls request logging.)

The following snippet of JavaScript code sends initialization parameters for the recognition request over the WebSocket connection. The calls are included in the client's `onOpen` function defined to ensure that they are sent only after the connection is established.

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

If it receives the request successfully, the service returns the following text message:

```javascript
{'state': 'listening'}
```
{: codeblock}

If you specify an invalid query parameter or JSON field as part of the input for a recognition request, the JSON that is returned by the service includes a `warnings` field that describes and lists each invalid argument. The request succeeds despite the warnings.

### Sending audio and receiving recognition results
{: #WSaudio}

After it sends the initial `start` message, the client can begin sending the audio data to the service. The client does not need to wait for the service to respond to the `start` message. The service returns the results of the transcription asynchronously in the same format as it returns results for the HTTP API.

The client must send the audio as binary data. The client can stream a maximum of 100 MB of audio data with a single utterance (per `send` request). The client can send multiple utterances over a single WebSocket connection.

The WebSocket interface imposes a maximum frame size of 4 MB. The client can set the maximum frame size to less than 4 MB or, if that is not practical, set the maximum message size to less than 4 MB and send the audio data as a sequence of messages.

The following snippet of JavaScript code sends audio data to the service as a binary message (blob):

```javascript
websocket.send(blob);
```
{: codeblock}

The following snippet receives recognition hypotheses that the service returns asynchronously. The results are handled in the `onMessage` function defined for the client.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

### Ending a recognition request
{: #WSstop}

When it is done sending the audio data for a request to the service, the client *must* signal the end of the binary transmission to the service in one of two ways:

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

After it returns the final result for the transcription to the client, the service returns another `{"state":"listening"}` message to the client. This message indicates that the service is ready to receive another recognition request. Before sending another request, the client must already have signaled the end of transmission for the previous request as just described. Otherwise, the service returns no new results.

### Sending additional requests and modifying parameters
{: #WSmore}

As long as the WebSocket connection remains active, the client can continue to use the connection to make additional recognition requests with new audio. By default, the service continues to use the parameters sent with the previous `start` message for all subsequent requests sent over the same connection.

To change the parameters for subsequent requests, the client sends another `start` message with the new parameters after it receives the final recognition results and `{"state":"listening"}` message from the service. The client can change any parameters except for those specified when the connection is opened: the `model` and the `customization_id`.

The following example sends a `start` message with new parameters for subsequent recognition requests sent over the connection. The message specifies the same `content-type` as the previous example, but it directs the service to return confidence measures and timestamps for the words of the transcription.

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

### Keeping a connection alive
{: #WSkeep}

The service terminates the session and closes the connection if an inactivity or session timeout is reached:

-   An *inactivity timeout* occurs if audio is being sent by the client but the service detects no speech. The inactivity timeout is 30 seconds by default. You can use the `inactivity_timeout` parameter to specify a different value, including `-1` to set the timeout to infinity.
-   A *session timeout* occurs if the service receives no data from the client or sends no interim results for 30 seconds. You cannot change the length of this timeout, but you can extend the session by sending the service any audio data, including just silence, before the timeout occurs. You must also set the `inactivity_timeout` to `-1`. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

For more information, see [Timeouts](/docs/services/speech-to-text/input.html#timeouts).

### Closing a connection
{: #WSclose}

When the client is done interacting with the service, it should close the WebSocket connection. Close the connection only after you have received all results; once the connection is closed, you can no longer use it to send requests or to receive results. The connection eventually times out and closes if you do not explicitly close it. The following snippet of JavaScript code closes an open connection:

```javascript
websocket.close();
```
{: codeblock}

## WebSocket return codes
{: #WSreturn}

The service can send the following return codes to the client over the WebSocket connection:

-   `1000` indicates normal closure of the connection, meaning that the purpose for which the connection was established has been fulfilled.
-   `1002` indicates that the service is closing the connection due to a protocol error.
-   `1006` indicates that the connection was closed abnormally.
-   `1009` indicates that the frame size exceeded the 4 MB limit.
-   `1011` indicates that the service is terminating the connection because it encountered an unexpected condition that prevents it from fulfilling the request.

If the socket closes with an error, it sends the client an informative message of the form `{"error":"{message}"}` before closing. For more information about WebSocket return codes, see [IETF RFC 6455 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://tools.ietf.org/html/rfc6455){: new_window}.

## Example WebSocket session
{: #WSexample}

The following exchange shows an example WebSocket session between a client and the {{site.data.keyword.speechtotextshort}} service. Each line indicates whether the message is sent from the *Client* or returned by the *Server*. The messages are shown in three separate pieces to make it easier to follow, but all three pieces represent a single session with the service. The example focuses on the exchange of messages and does not reflect opening and closing the connection.

In the first example, the client sends audio that contains the string `Name the Mayflower`. The client sends the audio in two chunks in PCM (`audio/l16`) format, for which it indicates the required sampling rate. Note that the client does not wait for the `{"state":"listening"}` response from the service to begin sending the audio data. Sending the data immediately reduces latency because the audio is available to the service as soon as it is ready to handle a recognition request.

```
Client>> {"action": "start", "content-type": "audio/l16;rate=22050"}
Client>> <audio data chunk>
Server<< {"state": "listening"}
Client>> <audio data chunk>
Client>> {"action": "stop"}
Server<< {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
          "final": true}],"result_index": 0}
Server<< {"state":"listening"}
```
{: codeblock}

In the second example, the client sends audio that contains the string `Second audio transcript`. The client sends the audio in a single binary message and uses the same parameters that it specified for the first request.

```
Client>> <audio data chunk>
Client>> {"action": "stop"}
Server<< {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
          "final": true}],"result_index": 0}
Server<< {"state":"listening"}
```
{: codeblock}

In the third example, the client again sends audio that contains the string `Name the Mayflower`. As with the first example, the client sends the audio in two chunks in PCM format. This time, the client asks the server to send interim results for the transcription.

```
Client>> {"action": "start", "content-type": "audio/l16;rate=22050",
          "interim_results": true}
Server<< {"state":"listening"}
Client>> <audio data chunk>
Server<< {"results": [{"alternatives": [{"transcript": "name "}],
          "final": false}],"result_index": 0}
Server<< {"results": [{"alternatives": [{"transcript": "name may "}],
          "final": false}],"result_index": 0}
Client>> <audio data chunk>
Client>> {"action": "stop"}
Server<< {"results": [{"alternatives": [{"transcript": "name may flour "}],
          "final": false}],"result_index": 0}
Server<< {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
          "final": true}],"result_index": 0}
Server<< {"state":"listening"}
```
{: codeblock}
