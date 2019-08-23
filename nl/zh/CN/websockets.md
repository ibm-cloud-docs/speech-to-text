---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-24"

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

# WebSocket 接口
{: #websockets}

{{site.data.keyword.speechtotextshort}} 服务的 WebSocket 接口是客户机与服务进行交互的最自然的方法。要将 WebSocket 接口用于语音识别，请首先使用 `/v1/recognize` 方法来建立与服务的持久连接。然后，通过该连接来发送文本和二进制消息，以启动和管理识别请求。
{: shortdesc}

识别请求和响应周期包含以下步骤：

1.  [打开连接](#WSopen)
1.  [启动识别请求](#WSstart)
1.  [发送音频和接收识别结果](#WSaudio)
1.  [结束识别请求](#WSstop)
1.  [发送其他请求和修改请求参数](#WSmore)
1.  [使连接保持活动](#WSkeep)
1.  [关闭连接](#WSclose)

客户机将数据发送到服务时，所有 JSON 消息*必须*作为文本消息传递，并且所有音频数据必须作为二进制消息传递。

以下示例代码片段是用 JavaScript 编写的，并基于 HTML5 WebSocket API。有关 WebSocket 协议的更多信息，请参阅因特网工程任务组织 (IETF) 的 [Request for Comments (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}。
{: note}

## 打开连接
{: #WSopen}

{{site.data.keyword.speechtotextshort}} 服务使用 WebSocket Secure (WSS) 协议使 `/v1/recognize` 方法在以下端点可用：

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

其中，`{host_name}` 是托管应用程序的位置：

-   `stream.watsonplatform.net` 用于达拉斯（以下各示例使用此主机名）
-   `stream-fra.watsonplatform.net` 用于法兰克福
-   `gateway-syd.watsonplatform.net` 用于悉尼
-   `gateway-wdc.watsonplatform.net` 用于华盛顿
-   `gateway-tok.watsonplatform.net` 用于东京
-   `gateway-lon.watsonplatform.net` 用于伦敦

WebSocket 客户机使用以下查询参数调用此方法，以建立与服务的已认证连接。如果使用的是 Identity and Access Management (IAM) 认证，请使用 `access_token` 查询参数。如果使用的是 Cloud Foundry 服务凭证，请使用 `watson-token` 查询参数。

<table>
  <caption>表 1. <code>/v1/recognize</code> 方法的参数</caption>
  <tr>
    <th style="width:23%; text-align:left">参数</th>
    <th style="width:12%; text-align:center">数据类型</th>
    <th style="text-align:left">描述</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      <em>如果使用的是 IAM 认证</em>，请传递有效的 IAM 访问令牌，以建立与服务的已认证连接。在调用中传递的是 IAM 访问令牌，而不是传递 API 密钥。必须在访问令牌到期之前建立连接。有关获取访问令牌的信息，请参阅[使用 IAM 令牌进行认证](/docs/services/watson?topic=watson-iam)。<br/><br/>
      传递访问令牌仅用于建立已认证的连接。建立连接后，可以使其无限期保持活动。只要连接保持打开，您始终会处于已认证状态。对于在令牌到期时间后持续存在的活动连接，无需刷新访问令牌。
    即使在删除了连接的令牌或其 API 密钥之后，该连接仍可以保持活动状态。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      <em>如果使用的是 Cloud Foundry 服务凭证</em>，请传递有效的 {{site.data.keyword.watson}} 认证令牌，以建立与服务的已认证连接。在调用中传递的是 {{site.data.keyword.watson}} 令牌，而不是传递服务凭证。{{site.data.keyword.watson}} 令牌基于 Cloud Foundry 服务凭证，这些凭证使用 `username` 和 `password` 进行 HTTP 基本认证。有关获取 {{site.data.keyword.watson}} 令牌的信息，请参阅 [{{site.data.keyword.watson}} 令牌](/docs/services/watson?topic=watson-gs-tokens-watson-tokens)。<br/><br/>
      传递 {{site.data.keyword.watson}} 令牌仅用于建立已认证的连接。建立连接后，可以使其无限期保持活动。只要连接保持打开，您始终会处于已认证状态。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      指定要用于转录的语言模型。如果未指定模型，那么缺省情况下服务会使用 <code>en-US_BroadbandModel</code> 模型。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      指定要用于通过连接发送的所有请求的定制语言模型的全局唯一标识 (GUID)。定制语言模型的基本模型必须与 <code>model</code> 参数的值相匹配。如果要包含定制标识，必须使用拥有该定制模型的服务实例的凭证来发出请求。缺省情况下，不会使用定制语言模型。有关更多信息，请参阅[定制接口](/docs/services/speech-to-text?topic=speech-to-text-customization)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      指定要用于通过连接发送的所有请求的定制声学模型的全局唯一标识 (GUID)。定制声学模型的基本模型必须与 <code>model</code> 参数的值相匹配。如果要包含定制标识，必须使用拥有该定制模型的服务实例的凭证来发出请求。缺省情况下，不会使用定制声学模型。有关更多信息，请参阅[定制接口](/docs/services/speech-to-text?topic=speech-to-text-customization)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      指定要用于通过连接发送的所有请求的基本 `model` 的版本。此参数主要与针对新基本模型升级的定制模型配合使用。缺省值取决于此参数是否与定制模型配合使用。有关更多信息，请参阅[基本模型版本](/docs/services/speech-to-text?topic=speech-to-text-input#version)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">布尔值</td>
    <td style="text-align:left">
      指示服务是否记录通过连接发送的请求和结果。要阻止 IBM 访问您的数据以进行一般服务改进，请为此参数指定 <code>true</code>。有关更多信息，请参阅[请求日志记录](/docs/services/speech-to-text?topic=speech-to-text-input#logging)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      将客户标识与通过连接传递的所有数据相关联。此参数接受自变量 <code>customer_id={id}</code>，其中 <code>id</code> 是要与数据关联的随机或通用字符串。您必须对自变量进行 URL 编码，使其成为参数，例如 `customer_id%3dmy_customer_ID`。缺省情况下，没有任何客户标识与数据相关联。有关更多信息，请参阅[信息安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。
    </td>
  </tr>
</table>

以下 JavaScript 代码片段打开与服务的连接。对 `/v1/recognize` 方法的调用传递 `access_token` 和 `model` 查询参数，后者用于指示服务使用西班牙语宽带模型。建立连接后，客户机定义了事件侦听器（`onOpen`、`onClose` 等）以响应来自服务的事件。客户机可以将连接用于多个识别请求。

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

客户机可以打开与服务的多个并行 WebSocket 连接。并行连接数仅受限于服务的容量，这通常不会对用户造成任何问题。

## 启动识别请求
{: #WSstart}

要启动识别请求，客户机将通过已建立的连接向服务发送 JSON 文本消息。客户机在发送要转录的任何音频之前，必须先发送此消息。消息必须包含 `action` 参数，但通常可以省略 `content-type` 参数。

<table>
  <caption>表 2. JSON 文本消息的参数</caption>
  <tr>
    <th style="width:23%; text-align:left">参数</th>
    <th style="width:12%; text-align:center">数据类型</th>
    <th style="text-align:left">描述</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>      必需
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      指定要执行的操作：
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> 启动识别请求，或为后续请求指定新参数。有关更多信息，请参阅[发送其他请求和修改请求参数](#WSmore)。
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> 发出信号，指示已发送请求的所有音频。有关更多信息，请参阅[结束识别请求](#WSstop)。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td style="text-align:left">
      标识请求的音频数据的格式（MIME 类型）。对于 `audio/alaw`、`audio/basic`、`audio/l16` 和 `audio/mulaw` 格式，此参数是必需的。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
    </td>
  </tr>
</table>

消息还可以包含可选参数，以指定请求处理方式的其他方面以及要返回的信息。有关所有输入和输出功能的信息，请参阅[参数摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。只能将语言模型、定制语言模型和定制声学模型指定为 WebSocket URL 的查询参数。

以下 JavaScript 代码片段通过 WebSocket 连接发送用于识别请求的初始化参数。这些调用包含在客户机的 `onOpen` 函数中，以确保这些调用仅在建立连接后发送。

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

如果服务成功收到请求，那么服务会返回以下文本消息以指示自己处于 `listening` 状态：

```javascript
{'state': 'listening'}
```
{: codeblock}

`listening` 状态指示服务实例已配置（JSON `start` 消息有效），并且已准备好处理识别请求的新话语。开始侦听后，服务会处理在 `listening` 消息之前发送的任何音频。

如果客户机为识别请求指定了无效的查询参数或 JSON 字段，那么服务的 JSON 响应会包含 `warnings` 字段。此字段描述每个无效自变量。不管是否有警告，请求都会成功。

## 发送音频和接收识别结果
{: #WSaudio}

在发送初始 `start` 消息后，客户机就可以开始向服务发送音频数据。客户机无需等待服务使用 `listening` 消息来响应 `start` 消息。服务将采用与对 HTTP 接口返回的结果相同的格式，异步返回转录的结果。

客户机必须将音频作为二进制数据发送。客户机在单个话语中最多可以发送 100 MB 的音频数据（每个 `send` 请求）。对于任何请求，客户机都必须发送至少 100 字节的音频。客户机可以通过单个 WebSocket 连接来发送多个话语。有关使用压缩来最大限度提高可以通过请求向服务传递的音频量的信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。

WebSocket 接口将最大帧大小限制为 4 MB。客户机可以将最大帧大小设置为小于 4 MB。如果设置帧大小不现实，那么客户机可以将最大消息大小设置为小于 4 MB，并将音频数据作为消息序列发送。有关 WebSocket 帧的更多信息，请参阅 [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}。

以下 JavaScript 代码片段将音频数据作为二进制消息 (blob) 发送到服务：

```javascript
websocket.send(blob);
```
{: codeblock}

以下片段接收服务异步返回的识别假设。结果由客户机的 `onMessage` 函数进行处理。

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## 结束识别请求
{: #WSstop}

完成将请求的音频数据发送到服务的操作时，客户机*必须*通过下列其中一种方式来发出信号，指示结束将二进制音频传输到服务：

-   发送将 `action` 参数设置为值 `stop` 的 JSON 文本消息：

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   发送空的二进制消息，其中指定的 blob 为空：

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

服务在收到音频传输完成的确认之前，不会发送最终结果。如果未发出信号指示传输完成，那么可能直到连接超时，服务也不会发送最终结果。

要在多个识别请求之间接收最终结果，客户机必须在发送下一个请求之前，发出信号指示结束前一个请求的传输。返回第一个请求的最终结果后，服务会向客户机返回另一个 `{"state":"listening"}` 消息。此消息指示服务已准备好接收另一个请求。

## 发送其他请求和修改请求参数
{: #WSmore}

WebSocket 连接保持活动时，客户机可以继续使用该连接来发送对新音频的进一步识别请求。缺省情况下，对于通过同一连接发送的所有后续请求，服务会继续使用随先前 `start` 消息一起发送的参数。

要更改后续请求的参数，客户机可以在收到服务的最终识别结果和 `{"state":"listening"}` 消息后，发送包含新参数的另一个 `start` 消息。客户机可以更改任何参数，但在打开连接时指定的参数（`model`、`language_customization_id` 等）除外。

以下示例为通过连接发送的后续识别请求，发送包含新参数的 `start` 消息。该消息指定的 `content-type` 与先前示例相同，但它指示服务为转录的词返回置信度度量和时间戳记。

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

## 使连接保持活动
{: #WSkeep}

如果发生不活动状态超时或会话超时，服务将终止会话并关闭连接：

-   如果客户机正在发送音频，但服务检测不到任何语音，那么会发生*不活动状态超时*。缺省情况下，不活动状态超时为 30 秒。可以使用 `inactivity_timeout` 参数指定其他值，包括 `-1`，这表示将超时设置为无穷大。有关更多信息，请参阅[不活动状态超时](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity)。
-   如果服务在 30 秒内未从客户机收到任何数据或未发送中间结果，那么会发生*会话超时*。您不能更改此超时的长度，但可以通过在发生超时之前向服务发送任何音频数据（包括仅含有静默的数据）来延长会话。此外，还必须将 `inactivity_timeout` 设置为 `-1`。但会根据发送到服务的任何数据的持续时间向您收费，包括发送用于延长会话的静默。有关更多信息，请参阅[会话超时](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-session)。

WebSocket 客户机和服务器还可以通过定期交换少量数据来交换 *Ping/Pong 帧*以避免读取超时。许多 WebSocket 堆栈都会交换 Ping/Pong 帧，但有些不交换。要确定您的实现是否使用了 Ping/Pong 帧，请检查其功能列表。您无法以编程方式确定或管理 Ping/Pong 帧。

如果 WebSocket 堆栈未实现 Ping/Pong 帧，并且要发送长音频文件，那么连接可能会遇到读取超时。要避免此类超时，请连续将音频流式传输到服务，或向服务请求中间结果。这两种方法中的任一种都可以确保缺少 Ping/Pong 帧不会导致连接关闭。

有关 Ping/Pong 帧的更多信息，请参阅 IETF RFC 6455 中的 [5.5.2 Ping](http://tools.ietf.org/html/rfc6455#section-5.5.2){: external} 部分和 [5.5.3 Pong](http://tools.ietf.org/html/rfc6455#section-5.5.3){: external} 部分。

## 关闭连接
{: #WSclose}

客户机完成与服务的交互时，可以关闭 WebSocket 连接。连接关闭后，客户机就无法再使用该连接来发送请求或接收结果。请仅在收到请求的所有结果后再关闭连接。如果未显式关闭连接，那么连接最终会超时并关闭。

以下 JavaScript 代码片段关闭打开的连接：

```javascript
websocket.close();
```
{: codeblock}

## WebSocket 返回码
{: #WSreturn}

服务可以通过 WebSocket 连接向客户机发送以下返回码：

-   `1000` 指示连接正常关闭，表示已实现建立连接的目的。
-   `1002` 指示由于协议错误，服务正在关闭连接。
-   `1006` 指示连接异常关闭。
-   `1009` 指示帧大小超过 4 MB 限制。
-   `1011` 指示服务正在终止连接，因为遇到了阻止服务执行请求的意外状况。

如果套接字由于错误而关闭，那么在套接字关闭之前，客户机会收到格式为 `{"error":"{message}"}` 的参考消息。有关 WebSocket 返回码的更多信息，请参阅 [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}。

## 示例 WebSocket 会话
{: #WSexample}

以下示例说明了客户机与 {{site.data.keyword.speechtotextshort}} 服务之间的 WebSocket 会话。会话显示为三个独立的交换，以便更轻松地理解。但是，所有这三个交换都是与服务的单个会话的一部分。示例专注于交换消息；并未显示打开和关闭连接。

### 第一个示例交换
{: #firstExample}

在第一个交换中，客户机发送包含字符串 `Name the Mayflower` 的音频。客户机发送包含单个 PCM (`audio/l16`) 音频数据块的二进制消息，并指示必需的采样率。客户机并不等待来自服务的 `{"state":"listening"}` 响应，而是直接开始发送音频数据，然后发出信号指示结束请求。立即发送数据可缩短等待时间，因为一旦服务准备好处理识别请求，音频将立即可供服务使用。

-   *客户机*发送：

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

-   *服务*响应：

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 第二个示例交换
{: #secondExample}

在第二个交换中，客户机发送包含字符串 `Second audio transcript` 的音频。客户机在单个二进制消息中发送音频，并使用在第一个请求中指定的相同参数。

-   *客户机*发送：

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *服务*响应：

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 第三个示例交换
{: #thirdExample}

在第三个交换中，客户机再次发送包含字符串 `Name the Mayflower` 的音频。客户机再次发送包含单个 PCM 音频数据块的二进制消息。但是这次，客户机请求对转录使用中间结果。

-   *客户机*发送：

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

-   *服务*响应：

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
