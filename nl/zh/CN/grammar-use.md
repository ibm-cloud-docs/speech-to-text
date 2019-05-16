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

# 将语法用于语音识别
{: #grammarUse}

使用语法创建并训练定制语言模型后，可以将语法用于通过服务的 WebSocket 和 HTTP 接口发出的语音识别请求。
{: shortdesc}

-   使用 `language_customization_id` 参数可为已定义语法的定制语言模型指定定制标识 (GUID)。您必须使用拥有该模型的服务实例的服务凭证来发出请求。
-   使用 `grammar_name` 参数可指定语法的名称。一个请求只能指定一个语法。

使用语法时，服务仅会根据指定的语法来识别词。服务不会使用从语料库中添加的定制词、单独添加或修改的定制词，也不会使用其他语法可识别的定制词。

-   对于 [WebSocket 接口](/docs/services/speech-to-text/websockets.html)，请首先使用 `/v1/recognize` 方法的 `language_customization_id` 参数来指定定制标识。使用此方法可建立与服务的 WebSocket 连接。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    然后，在活动连接的 JSON `start` 消息中，使用 `grammar_name` 参数来指定语法的名称。使用 `start` 消息传递此值时，可以为通过连接发送的每个请求动态更改语法。

    ```javascript
    function onOpen(evt) {
      var message = {
        action: 'start',
        content-type: 'audio/l16;rate=22050',
        grammar_name: '{grammar_name}'
      };
      websocket.send(JSON.stringify(message));
      websocket.send(blob);
    }
    ```
    {: codeblock}
-   对于[同步 HTTP 接口](/docs/services/speech-to-text/http.html)，请使用 `POST /v1/recognize` 方法来传递这两个参数。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   对于[异步 HTTP 接口](/docs/services/speech-to-text/async.html)，请使用 `POST /v1/recognitions` 方法来传递这两个参数。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
