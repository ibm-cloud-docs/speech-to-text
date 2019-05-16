---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# WebSocket 介面
{: #websockets}

{{site.data.keyword.speechtotextshort}} 服務的 WebSocket 介面是用戶端與服務互動最自然的方式。若要使用 WebSocket 介面來進行語音辨識，請先使用 `/v1/recognize` 方法來建立與服務的持續連線，然後透過連線傳送文字和二進位訊息，以起始及管理辨識要求。
{: shortdesc}

辨識要求和回應週期的步驟如下：

1.  [開啟連線](#WSopen)
1.  [起始辨識要求](#WSstart)
1.  [傳送音訊及接收辨識結果](#WSaudio)
1.  [結束辨識要求](#WSstop)
1.  [傳送其他要求及修改要求參數](#WSmore)
1.  [保持連線作用中](#WSkeep)
1.  [關閉連線](#WSclose)

當用戶端將資料傳送至服務時，*必須* 以文字訊息來傳遞所有 JSON 訊息，並以二進位訊息來傳遞所有音訊資料。

隨後的程式碼範例 Snippet 是以 JavaScript 撰寫，並以 HTML5 WebSocket API 為基礎。如需 WebSocket 通訊協定的相關資訊，請參閱「網際網路工程任務小組 (IETF)」的 [Request for Comment (RFC) 6455 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://tools.ietf.org/html/rfc6455){: new_window}。
{: note}

## 開啟連線
{: #WSopen}

{{site.data.keyword.speechtotextshort}} 服務會使用 WebSocket Secure (WSS) 通訊協定，在下列端點提供 `/v1/recognize` 方法：

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

其中 `{host_name}` 是管理應用程式的位置：

-   `stream.watsonplatform.net`，適用於達拉斯（下列範例使用此主機名稱）
-   `stream-fra.watsonplatform.net`，適用於法蘭克福
-   `gateway-syd.watsonplatform.net`，適用於雪梨
-   `gateway-wdc.watsonplatform.net`，適用於華盛頓特區
-   `gateway-tok.watsonplatform.net`，適用於東京
-   `gateway-lon.watsonplatform.net`，適用於倫敦

WebSocket 用戶端會搭配下列查詢參數來呼叫此方法，以建立與服務的鑑別連線。如果您使用 Identity and Access Management (IAM) 鑑別，請使用 `access_token` 查詢參數。如果您使用 Cloud Foundry 服務認證，請使用 `watson-token` 查詢參數。

<table>
  <caption>表 1. <code>/v1/recognize</code> 方法的參數</caption>
  <tr>
    <th style="width:23%; text-align:left">參數</th>
    <th style="width:12%; text-align:center">資料類型</th>
    <th style="text-align:left">說明</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      <em>如果使用 IAM 鑑別，</em>則會傳遞有效的 IAM 存取記號，以建立與服務的已鑑別連線。您會傳遞 IAM 存取記號，而不是使用呼叫來傳遞 API 金鑰。您必須在存取記號到期之前建立連線。如需取得存取記號的相關資訊，請參閱[使用 IAM 記號鑑別](/docs/services/watson/getting-started-iam.html)。<br/><br/>
      傳遞存取記號只是為了建立已鑑別的連線。建立連線之後，即可無限期地保持連線作用中。只要保持連線開啟，就可以維持已鑑別狀態。您不需要重新整理存取記號，作用中的連線會一直持續到記號的有效期限之後。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      <em>如果使用 Cloud Foundry 服務認證，</em>則會傳遞有效的 {{site.data.keyword.watson}} 鑑別記號，以建立與服務的已鑑別連線。您會傳遞 {{site.data.keyword.watson}} 記號，而不是使用呼叫來傳遞服務認證。
      {{site.data.keyword.watson}} 記號是以 Cloud Foundry 服務認證為基礎，這些服務認證會使用 `username` 及 `password` 進行 HTTP 基本鑑別。如需取得 {{site.data.keyword.watson}} 記號的相關資訊，請參閱 [{{site.data.keyword.watson}} 記號](/docs/services/watson/getting-started-tokens.html)。<br/><br/>
      傳遞 {{site.data.keyword.watson}} 記號只是為了建立已鑑別的連線。建立連線之後，即可無限期地保持連線作用中。只要保持連線開啟，就可以維持已鑑別狀態。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      指定要用於轉錄的語言模型。如果您未指定模型，依預設，服務會使用 <code>en-US_BroadbandModel</code> 模型。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text/models.html)。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      指定自訂語言模型的「廣域唯一 ID (GUID)」，該模型用於透過連線傳送的所有要求。自訂語言模型的基礎模型必須符合 <code>model</code> 參數的值。依預設，不會使用任何自訂語言模型。如需相關資訊，請參閱[自訂作業介面](/docs/services/speech-to-text/custom.html)。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      指定自訂聲學模型的「廣域唯一 ID (GUID)」，該模型用於透過連線傳送的所有要求。自訂聲學模型的基礎模型必須符合 <code>model</code> 參數的值。依預設，不會使用任何自訂聲學模型。如需相關資訊，請參閱[自訂作業介面](/docs/services/speech-to-text/custom.html)。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      指定基礎 `model` 的版本，該模型用於透過連線傳送的所有要求。此參數主要是與已針對新基礎模型而更新的自訂模型搭配使用。預設值取決於參數是否搭配使用自訂模型。如需相關資訊，請參閱[基礎模型版本](/docs/services/speech-to-text/input.html#version)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">布林</td>
    <td style="text-align:left">
      指出服務是否記載透過連線傳送的要求和結果。若要防止 IBM 存取您的資料進行一般服務改善，請對參數指定 <code>true</code>。如需相關資訊，請參閱[要求記載](/docs/services/speech-to-text/input.html#logging)。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      建立客戶 ID 與透過連線傳遞之所有資料的關聯。此參數接受引數 <code>customer_id={id}</code>，其中 <code>id</code> 是要與資料相關聯的隨機或通用字串。您必須將引數以 URL 編碼為參數，例如，`customer_id%3dmy_customer_ID`。依預設，沒有客戶 ID 與資料相關聯。如需相關資訊，請參閱[資訊安全](/docs/services/speech-to-text/information-security.html)。</td>
  </tr>
</table>

下列 JavaScript 程式碼 Snippet 會開啟與服務的連線。呼叫 `/v1/recognize` 方法會傳遞 `access_token` 和 `model` 查詢參數，而後者會引導服務使用西班牙文寬頻模型。建立連線之後，用戶端會定義事件接聽器（`onOpen`、`onClose` 等等），以回應來自服務的事件。用戶端可以將該連線用於多個辨識要求。

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

## 起始辨識要求
{: #WSstart}

為了起始辨識要求，用戶端會透過已建立的連線，將 JSON 文字訊息傳送至服務。用戶端必須先傳送此訊息，才能傳送任何音訊來進行轉錄。訊息中必須包含 `action` 參數，但通常可以省略 `content-type` 參數。

<table>
  <caption>表 2. JSON 文字訊息的參數</caption>
  <tr>
    <th style="width:23%; text-align:left">參數</th>
    <th style="width:12%; text-align:center">資料類型</th>
    <th style="text-align:left">說明</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      指定要執行的動作：
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> 會起始辨識要求，或是為後續的要求指定新參數。如需相關資訊，請參閱[傳送其他要求及修改要求參數](#WSmore)。
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> 會發出信號，表示已傳送要求的所有音訊。如需相關資訊，請參閱[結束辨識要求](#WSstop)。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      識別要求的音訊資料格式（MIME 類型）。此為 `audio/alaw`、`audio/basic`、`audio/l16` 及 `audio/mulaw` 格式的必要參數。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text/audio-formats.html)。</td>
  </tr>
</table>

訊息中也可以包含選用參數，以指定要如何處理要求的其他層面，以及要傳回的資訊。如需所有輸入和輸出特性的相關資訊，請參閱[參數摘要](/docs/services/speech-to-text/summary.html)。您只能指定語言模型、自訂語言模型及自訂聲學模型，作為 WebSocket URL 的查詢參數。

下列 JavaScript 程式碼 Snippet 會透過 WebSocket 連線傳送辨識要求的起始設定參數。呼叫包含在用戶端的 `onOpen` 函數中，以確保只在建立連線之後才會傳送它們。

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

如果順利收到要求，服務會傳回下列文字訊息，指出其為 `listening`：

```javascript
{'state': 'listening'}
```
{: codeblock}

`listening` 狀態表示服務實例已配置（您的 JSON `start` 訊息是有效的），並已備妥可為辨識要求處理新的話語。一旦開始接聽，服務就會處理在 `listening` 訊息之前所傳送的任何音訊。

如果用戶端為辨識要求指定的查詢參數或 JSON 欄位無效，則服務的 JSON 回應會包含 `warnings` 欄位。此欄位說明每個無效的引數。儘管出現警告，要求仍會成功。


## 傳送音訊及接收辨識結果
{: #WSaudio}

在傳送起始 `start` 訊息之後，用戶端可以開始將音訊資料傳送至服務。用戶端不需要等待服務以 `listening` 訊息來回應 `start` 訊息。服務會以非同步方式傳回轉錄結果，格式與針對 HTTP 介面傳回的結果相同。

用戶端必須以二進位資料形式傳送音訊。用戶端可以使用單一話語來傳送最多 100 MB 的音訊資料（每個 `send` 要求）。針對任何要求，必須至少傳送 100 個位元組的音訊。用戶端可以透過單一 WebSocket 連線傳送多個話語。如需使用壓縮將您可在要求中傳遞至服務的音訊量最大化的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text/audio-formats.html)。

WebSocket 介面強制訊框大小上限為 4 MB。用戶端可以將訊框大小上限設為小於 4 MB。如果設定訊框大小不切實際，用戶端可以將訊息大小上限設為小於 4 MB，並以一系列訊息傳送音效資料。

下列 JavaScript 程式碼 Snippet 會以二進位訊息 (blob) 將音訊資料傳送至服務：

```javascript
websocket.send(blob);
```
{: codeblock}

下列 Snippet 會接收服務非同步傳回的辨識假設。結果是由用戶端的 `onMessage` 函數處理。

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## 結束辨識要求
{: #WSstop}

將要求的音訊資料傳送至服務之後，用戶端*必須* 以下列其中一種方式來表示二進位音訊傳輸至服務結束：

-   傳送 JSON 文字訊息，並將 `action` 參數設為值 `stop`：

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   傳送空的二進位訊息，其中指定的二進位大型物件是空的：

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

在收到音訊傳輸已完成的確認信號之前，服務不會傳送最終結果。如果您無法發出傳輸已完成的信號，連線可能會在服務未傳送最終結果的情況下逾時。

為了在多個辨識要求之間接收最終結果，用戶端必須先發出信號表示前一個要求的傳輸已結束，才會傳送後續要求。在傳回第一個要求的最終結果之後，服務會將另一個 `{"state":"listening"}` 訊息傳回給用戶端。此訊息指出服務已備妥可接收另一個要求。

## 傳送其他要求及修改要求參數
{: #WSmore}

在 WebSocket 連線持續作用的情況下，用戶端可以繼續使用連線來傳送具有新音訊的進一步辨識要求。依預設，服務會繼續使用透過前一個 `start` 訊息傳送的參數，以透過相同連線傳送所有後續要求。

若要變更後續要求的參數，用戶端可以在從服務收到最終辨識結果和 `{"state":"listening"}` 訊息之後，使用新的參數來傳送另一個 `start` 訊息。除了連線開啟時所指定的那些參數（`model`、`language_customization_id` 等等），用戶端可以變更其他任何參數。

下列範例會針對透過該連線傳送的後續辨識要求，使用新的參數來傳送 `start` 訊息。此訊息指定與前一個範例相同的 `content-type`，但會指示服務傳回轉錄字組的信賴度測量值和時間戳記。

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

## 保持連線作用中
{: #WSkeep}

如果達到閒置或階段作業逾時值，服務會終止階段作業並關閉連線：

-   如果用戶端傳送音訊，但服務未偵測到任何語音，就會發生*閒置逾時*。依預設，閒置逾時值為 30 秒。您可以使用 `inactivity_timeout` 參數來指定其他值，包括 `-1`，將逾時值設為無限。
-   如果服務未從用戶端收到任何資料或未傳送任何臨時結果達 30 秒，就會發生*階段作業逾時*。您無法變更此逾時的長度，但您可以在發生逾時之前，傳送任何音訊資料（包括只是靜音）給服務，以延長階段作業。您還必須將 `inactivity_timeout` 設為 `-1`。您傳送至服務的任何資料（包括您傳送以延長階段作業的靜音）的持續時間都要收費。

如需相關資訊，請參閱[逾時](/docs/services/speech-to-text/input.html#timeouts)。

## 關閉連線
{: #WSclose}

當用戶端完成與服務的互動後，可以關閉 WebSocket 連線。關閉連線之後，用戶端就無法再利用它來傳送要求或接收結果。請只有在接收要求的所有結果之後，才關閉連線。如果您未明確地將其關閉，連線最後會逾時並關閉。

下列 JavaScript 程式碼 Snippet 會關閉開啟的連線：

```javascript
websocket.close();
```
{: codeblock}

## WebSocket 回覆碼
{: #WSreturn}

服務可以透過 WebSocket 連線，將下列回覆碼傳送至用戶端：

-   `1000` 指出正常關閉連線，這表示已完成建立連線的目的。
-   `1002` 指出由於通訊協定錯誤，服務正在關閉連線。
-   `1006` 指出連線異常關閉。
-   `1009` 指出訊框大小超出 4 MB 限制。
-   `1011` 指出服務正在終止連線，因為它遇到非預期的狀況，導致無法完成要求。

如果 Socket 由於錯誤而關閉，則用戶端會在 Socket 關閉之前，收到 `{"error":"{message}"}` 格式的通知訊息。如需 WebSocket 回覆碼的相關資訊，請參閱 [IETF RFC 6455 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://tools.ietf.org/html/rfc6455){: new_window}。

## WebSocket 階段作業範例
{: #WSexample}

下列範例顯示用戶端與 {{site.data.keyword.speechtotextshort}} 服務之間的 WebSocket 階段作業。此階段作業顯示成三個不同的交換，以方便檢視。然而，這三個交換都是屬於與服務的單一階段作業。此範例著重在訊息交換，並未顯示開啟和關閉連線。

### 第一個交換範例
{: #firstExample}

在第一個交換中，用戶端傳送包含字串 `Name the Mayflower` 的音訊。用戶端傳送含有單一片段 PCM (`audio/l16`) 音訊資料的二進位訊息，以指出所需的取樣率。用戶端未等待來自服務的 `{"state":"listening"}` 回應，即開始傳送音訊資料，並發出要求結束的信號。立即傳送資料可減少延遲，因為只要服務準備好要處理辨識要求，就立刻有音訊可用。

-   *用戶端* 傳送：

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

-   *服務* 回應：

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 第二個交換範例
{: #secondExample}

在第二個交換中，用戶端傳送包含字串 `Second audio transcript` 的音訊。用戶端以單一二進位訊息來傳送音訊，並使用在第一個要求中指定的相同參數。

-   *用戶端* 傳送：

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *服務* 回應：

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 第三個交換範例
{: #thirdExample}

在第三個交換中，用戶端再次傳送包含字串 `Name the Mayflower` 的音訊。用戶端再次傳送含有單一片段 PCM 音訊資料的二進位訊息。但是這次用戶端要求含有轉錄的臨時結果。

-   *用戶端* 傳送：

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

-   *服務* 回應：

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
