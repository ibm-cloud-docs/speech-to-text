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

# 使用自訂聲學模型
{: #acousticUse}

建立和訓練自訂聲學模型之後，就可以在語音辨識要求中使用它。請使用 `acoustic_customization_id` 查詢參數來指定要求的自訂聲學模型，如下列範例所示。您必須使用擁有該模型之服務實例的認證來發出要求。
{: shortdesc}

您也可以指定要與要求一起使用的自訂語言模型，以提高轉錄正確性。如需相關資訊，請參閱[在語音辨識期間使用自訂語言模型及自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize)。

您可以為相同或不同的領域或環境建立多個自訂聲學模型。不過，在 `acoustic_customization_id` 參數中，您一次只能指定一個自訂聲學模型。

-   對於 [WebSocket 介面](/docs/services/speech-to-text?topic=speech-to-text-websockets)，請使用 `/v1/recognize` 方法。指定的自訂模型會用於透過連線傳送的所有要求。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   對於[同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-http)，請使用 `POST /v1/recognize` 方法。指定的自訂模型會用於該要求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   對於[非同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-async)，請使用 `POST /v1/recognitions` 方法。指定的自訂模型會用於該要求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

如果自訂模型是根據預設模型 `en-US_BroadbandModel`，您可以省略要求中的語言模型。否則，您必須使用 `model` 參數來指定基礎模型，如 WebSocket 範例所示。自訂模型只能與其建立目的的基礎模型搭配使用。
