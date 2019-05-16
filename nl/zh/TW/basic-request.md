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

# 提出辨識要求
{: #basic-request}

如果要求使用 {{site.data.keyword.speechtotextfull}} 服務進行語音辨識，您只需要提供要轉錄的音訊。此服務的每一個介面都提供相同的基本轉錄功能：WebSocket 介面、同步 HTTP 介面及非同步 HTTP 介面。
{: shortdesc}

下列各節針對此服務的每個介面，顯示沒有選用輸入或輸出參數的基本轉錄要求：

-   這些範例提交一個名為 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 的簡短 FLAC 檔。
-   這些範例使用預設語言模型 `en-US_BroadbandModel`。

[瞭解辨識結果](/docs/services/speech-to-text/basic-response.html)說明此服務對這些範例的回應。

## 使用要求傳送音訊
{: #basic-request-audio}

您傳遞至服務的音訊必須是此服務其中一個支援的格式。對於大部分音訊而言，此服務可以自動偵測格式。對於部分音訊，您必須使用 `Content-Type` 或同等參數來指定格式。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text/audio-formats.html)。（為了清晰表達，下列範例會指定所有要求的音訊格式。）

使用 WebSocket 和同步 HTTP 介面，您可以使用單一要求來傳遞最多 100 MB 的音訊資料。透過非同步 HTTP 介面，最多可以傳遞 1 GB 的音訊資料。在任何要求中，您必須至少傳送 100 個位元組的音訊。

如果您要辨識大量音訊，可以手動將音訊分割成較小的片段。然而，將音訊轉換為壓縮的有損格式通常會更有效率且方便。壓縮可以最大化您在單一要求中可傳送的資料量。尤其如果音訊是 WAV 或 FLAC 格式，將其轉換為有損格式將會有明顯的差異。

-   如需使用壓縮之音訊格式的相關資訊，請參閱[支援的音訊格式](/docs/services/speech-to-text/audio-formats.html#formats)。
-   如需壓縮效果及將音訊轉換為使用它之格式的相關資訊，請參閱[資料限制和壓縮](/docs/services/speech-to-text/audio-formats.html#limits)和[音訊轉換](/docs/services/speech-to-text/audio-formats.html#conversion)。

## 使用 WebSocket 介面
{: #basic-request-websocket}

[WebSocket 介面](/docs/services/speech-to-text/websockets.html)提供有效的實作，透過全雙工連線提供低延遲及高傳輸量。所有要求和回應都透過相同的 WebSocket 連線傳送。因為其優點，所以 WebSocket 是偏好的語音辨識機制。如需相關資訊，請參閱 [WebSocket 介面的優點](/docs/services/speech-to-text/developer-overview.html#advantages)。

若要使用 WebSocket 介面，首先請使用 `/v1/recognize` 方法來建立與服務的連線。您可以指定語言模型這類參數，以及任何要用於透過連線傳送之要求的自訂模型。然後，您將登錄事件接聽器，以處理來自服務的回應。若要提出要求，您可以傳送包含音訊格式及任何其他參數的 JSON 文字訊息。您以二進位訊息（二進位大型物件）傳遞音訊，然後傳送文字訊息以指出音訊結束。

下列範例提供 JavaScript 程式碼來建立連線，並傳送用於辨識要求的文字和二進位訊息。該範例不包含用來安裝事件處理程式的程式碼。

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

## 使用同步 HTTP 介面
{: #basic-request-sync}

[同步 HTTP 介面](/docs/services/speech-to-text/http.html)提供最簡單的方法來提出辨識要求。您可以使用 `POST /v1/recognize` 方法對服務提出要求。您使用單一要求來傳遞音訊及所有參數。

下列 `curl` 範例顯示基本 HTTP 辨識要求：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 使用非同步 HTTP 介面
{: #basic-request-async}

[非同步 HTTP 介面](/docs/services/speech-to-text/async.html)提供非區塊處理介面來轉錄音訊。無論是否先向服務登錄回呼 URL，您都可以使用此介面。使用回呼 URL，該服務會傳回含有工作狀態和辨識結果的回呼通知。該介面會根據使用者指定的密碼來使用 HMAC-SHA1 簽章，為其通知提供鑑別和資料完整性。若沒有回呼 URL，您必須輪詢該服務，才能取得工作狀態和結果。不論哪一種方法，您都可以使用 `POST /v1/recognitions` 方法來提出辨識要求。

下列 `curl` 範例顯示簡易非同步 HTTP 辨識要求。此要求不包括回呼 URL，因此您必須輪詢服務，以取得工作狀態和產生的文字記錄。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}
