---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# 发出识别请求
{: #basic-request}

要使用 {{site.data.keyword.speechtotextfull}} 服务请求语音识别，您只需提供要转录的音频。服务为其每个接口（WebSocket 接口、同步 HTTP 接口和异步 HTTP 接口）提供了相同的基本转录功能。
{: shortdesc}

以下各部分说明了每个服务接口的基本转录请求（不使用可选的输入或输出参数）：

-   示例提交名为 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 的简短 FLAC 文件。
-   示例使用缺省语言模型 `en-US_BroadbandModel`。

[了解识别结果](/docs/services/speech-to-text/basic-response.html)描述了服务对这些示例的响应。

## 使用请求发送音频
{: #basic-request-audio}

传递给服务的音频必须是服务支持的其中一种格式。对于大多数音频，服务可以自动检测格式。但对于某些音频，必须使用 `Content-Type` 或等效参数来指定格式。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text/audio-formats.html)。（为了清楚起见，以下示例在所有请求中都指定了音频格式。）

使用 WebSocket 和同步 HTTP 接口时，通过单个请求最多可以传递 100 MB 音频数据。使用异步 HTTP 接口时，最多可以传递 1 GB 音频数据。在任何请求中，都必须发送至少 100 字节的音频。

如果要识别的音频量很大，那么可以手动将音频划分成较小的区块。但是，将音频转换为压缩的有损格式通常更高效、更方便。压缩可以最大限度提高可通过单个请求发送的数据量。尤其是音频为 WAV 或 FLAC 格式时，将其转换为有损格式会产生显著的效果。

-   有关使用压缩的音频格式的更多信息，请参阅[支持的音频格式](/docs/services/speech-to-text/audio-formats.html#formats)。
-   有关压缩效果以及将音频转换为使用压缩的格式的更多信息，请参阅[数据限制和压缩](/docs/services/speech-to-text/audio-formats.html#limits)以及[音频转换](/docs/services/speech-to-text/audio-formats.html#conversion)。

## 使用 WebSocket 接口
{: #basic-request-websocket}

[WebSocket 接口](/docs/services/speech-to-text/websockets.html)通过全双工连接提供低延迟和高吞吐量，从而支持高效实现。所有请求和响应都通过同一 WebSocket 连接发送。WebSocket 凭借自身的优点，成为语音识别的首选机制。有关更多信息，请参阅 [WebSocket 接口的优点](/docs/services/speech-to-text/developer-overview.html#advantages)。

要使用 WebSocket 接口，请首先使用 `/v1/recognize` 方法来建立与服务的连接。可以指定要用于通过连接发送的请求的参数，例如，语言模型和任何定制模型。然后，注册事件侦听器来处理来自服务的响应。要发出请求，请发送包含音频格式和任何其他参数的 JSON 文本消息。可将音频作为二进制消息 (blob) 传递，然后发送文本消息以指示音频结束。

以下示例提供的 JavaScript 代码用于建立连接，并发送用于识别请求的文本和二进制消息。此示例不包含用于安装事件处理程序的代码。

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  action: 'start',
  content-type: 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({action: 'stop'}));
```
{: codeblock}

## 使用同步 HTTP 接口
{: #basic-request-sync}

[同步 HTTP 接口](/docs/services/speech-to-text/http.html)为发出识别请求提供了最简单的方法。您可使用 `POST /v1/recognize` 方法向服务发出请求。通过单个请求可传递音频和所有参数。

以下 `curl` 示例显示了基本 HTTP 识别请求：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 使用异步 HTTP 接口
{: #basic-request-async}

[异步 HTTP 接口](/docs/services/speech-to-text/async.html)提供了用于转录音频的非阻塞接口。使用此接口时，可以先向服务注册回调 URL，也可以不注册。有回调 URL 时，服务可发送包含作业状态和识别结果的回调通知。此接口使用基于用户指定私钥的 HMAC-SHA1 签名，为其通知提供认证和数据完整性。没有回调 URL 时，必须轮询服务来获取作业状态和结果。无论采用哪种方法，都可使用 `POST /v1/recognitions` 方法来发出识别请求。

以下 `curl` 示例显示了简单的异步 HTTP 识别请求。该请求不包含回调 URL，因此必须轮询服务来获取作业状态和生成的抄本。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}
