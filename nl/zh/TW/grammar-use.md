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

# 使用文法進行語音辨識
{: #grammarUse}

在您搭配使用文法來建立及訓練自訂語言模型之後，您可以透過服務的 WebSocket 和 HTTP 介面，在語音辨識要求中使用該文法。
{: shortdesc}

-   使用 `language_customization_id` 參數來指定已定義此文法的自訂語言模型的自訂作業 ID (GUID)。您必須使用擁有該模型之服務實例的服務認證來發出要求。
-   使用 `grammar_name` 參數來指定文法的名稱。您只能在一個要求中指定單一文法。

當您使用文法時，該服務只會辨識指定文法中的字組。該服務不會使用從語料庫中新增、個別新增或修改或是由其他文法所辨識的自訂字組。

-   對於 [WebSocket 介面](/docs/services/speech-to-text/websockets.html)，首先請使用 `/v1/recognize` 方法的 `language_customization_id` 參數來指定自訂作業 ID。您可以使用此方法來建立與服務的 WebSocket 連線。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    然後，在針對作用中連線的 JSON `start` 訊息中，使用 `grammar_name` 參數來指定文法的名稱。使用 `start` 訊息傳遞此值，容許您針對透過連線傳送的每個要求動態變更文法。

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
-   對於[同步 HTTP 介面](/docs/services/speech-to-text/http.html)，使用 `POST /v1/recognize` 方法傳遞這兩個參數。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   對於[非同步 HTTP 介面](/docs/services/speech-to-text/async.html)，使用 `POST /v1/recognitions` 方法傳遞這兩個參數。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
