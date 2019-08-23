---

copyright:
  years: 2017, 2019
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

# 使用定制声学模型
{: #acousticUse}

创建并训练定制声学模型后，可以将其用于语音识别请求。使用 `acoustic_customization_id` 查询参数来指定请求的定制声学模型，如以下示例中所示。您必须使用拥有该模型的服务实例的凭证来发出请求。
{: shortdesc}

此外，还可以在请求中指定要使用的定制语言模型，这可以提高转录准确性。有关更多信息，请参阅[在语音识别期间使用定制语言模型和定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize)。

可以针对相同或不同的域或环境创建多个定制声学模型。但是，使用 `acoustic_customization_id` 参数一次只能指定一个定制声学模型。

-   对于 [WebSocket 接口](/docs/services/speech-to-text?topic=speech-to-text-websockets)，请使用 `/v1/recognize` 方法。指定的定制模型将用于通过连接发送的所有请求。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   对于[同步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-http)，请使用 `POST /v1/recognize` 方法。指定的定制模型将用于该请求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   对于[异步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-async)，请使用 `POST /v1/recognitions` 方法。指定的定制模型将用于该请求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

如果定制模型基于缺省模型 `en-US_BroadbandModel`，那么可以在请求中省略该语言模型。否则，必须使用 `model` 参数来指定基本模型，如 WebSocket 示例中所示。为某个基本模型创建的定制模型只能用于该基本模型。
