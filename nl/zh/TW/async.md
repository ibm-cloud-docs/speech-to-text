---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 非同步 HTTP 介面
{: #async}

{{site.data.keyword.speechtotextfull}} 服務的非同步 HTTP 介面提供各種透過對該服務的非區塊處理呼叫來轉錄音訊的方法。該介面採用使用者指定的密碼字串和數位簽章，為透過 HTTP 通訊協定提出的要求提供安全等級。若要使用非同步介面，您可以執行下列動作：
{: shortdesc}

-   登錄回呼 URL，該服務會自動向此 URL 通知工作狀態和結果。
-   輪詢服務，以手動取得工作狀態和結果。

這兩種方法並不互斥。您可以選擇接收回呼通知，但仍輪詢該服務以取得最新狀態，或聯絡該服務以手動擷取結果。下列各節說明如何搭配任一方法來使用非同步 HTTP 介面。

以單一要求提交最多 1 GB、最少 100 個位元組的音訊資料。如需音訊格式及使用壓縮將您可在要求中傳送之音訊量最大化的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text/audio-formats.html)。如需此介面的個別方法的相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/speech-to-text){: new_window}。

## 使用模型
{: #usage}

當您使用服務的非同步 HTTP 介面時，可以透過下列方式選擇要瞭解工作狀態和接收結果：

-   使用回呼通知：
    1.  呼叫 `POST /v1/register_callback` 方法，向服務登錄回呼 URL。您可以提供選用的使用者指定密碼字串，針對傳送至 URL 的回呼啟用鑑別及資料完整性。
    1.  使用已登錄的回呼 URL 來呼叫 `POST /v1/recognitions` 方法，當工作狀態變更時，該服務會將通知傳送至此 URL。請指定要通知的事件清單。依預設，該服務會在工作啟動時、完成時及發生錯誤時傳送通知。您也可以在完成通知中要求此要求的結果。否則，您需要使用 `GET /v1/recognitions/{id}` 方法來擷取結果。
-   輪詢服務：
    1.  在沒有回呼 URL、事件或使用者記號的情況下，呼叫 `POST /v1/recognitions` 方法。
    1.  定期呼叫 `GET /v1/recognitions` 方法，以檢查最新工作的狀態，或呼叫 `GET /v1/recognitions/{id}` 方法，以檢查特定工作的狀態。
    1.  如果您使用 `GET /v1/recognitions` 方法檢查工作狀態，請呼叫 `GET /v1/recognitions/{id}` 方法來擷取所完成的工作的結果。

這兩種方法可以一起使用。您仍然可以輪詢服務，以取得使用回呼 URL 建立之工作的最新狀態。例如，如果通知送達時間有點久，您可能會想要取得工作的狀態。如果您懷疑因為服務或網路錯誤而遺失一個以上的通知，您也可以檢查狀態。

## 登錄回呼 URL
{: #register}

您可以呼叫 `POST /v1/register_callback` 方法來登錄回呼 URL。登錄回呼 URL 之後，您可以用它來接收無限個工作的通知。登錄程序包含四個步驟：

1.  您呼叫 `POST /v1/register_callback` 方法，並傳遞回呼 URL。您也可以選擇性地指定使用者指定的密碼。該服務會使用此密碼，針對鑑別和資料完整性來計算加密雜湊訊息鑑別碼 (HMAC) 安全雜湊演算法 1 (SHA1) 簽章。下列範例會登錄回應 URL `http://{user_callback_path}/results` 的使用者回呼。此呼叫包括使用者密碼 `ThisIsMySecret`。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  如果尚未將 `GET` 要求傳送至回呼 URL 以進行登錄，則服務會嘗試驗證此回呼 URL 或將其加入白名單。該服務會透過要求的 `challenge_string` 查詢參數，來傳遞隨機的英數字元盤查字串。該要求包括 `Accept` 標頭，其指定 `text/plain` 作為必要的回應類型。

    如果 `register_callback` 方法的呼叫包含使用者密碼，則該服務中的 `GET` 要求也會包含 `X-Callback-Signature` 標頭，用來指定盤查字串的 HMAC-SHA1 簽章。該服務會使用使用者密碼作為金鑰來計算簽章。

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  您以狀態碼為 200 來回應該服務的 `GET` 要求。將該服務所傳送的盤查字串包含在回應中。在回應內文中包含純文字字串，並將 `Content-Type` 回應標頭設為 `text/plain`。

    如果起始 `POST` 要求包含使用者密碼，您可以使用此密碼作為金鑰，來計算盤查字串的 HMAC-SHA1 簽章。如果 `GET` 要求已由該服務傳送，則此簽章符合 `X-Callback-Signature` 標頭指定的值。

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  該服務會檢查回應內文中是否將盤查字串傳回至其 `GET` 要求。如果有，則該服務會將此回呼 URL 列入白名單，並以狀態碼 201 來回應您的原始 `POST` 要求。回應內文包括的 JSON 物件具有 `status` 欄位（其值為 `created`）及 `url` 欄位（其值為您的回呼 URL）。

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

該服務在登錄程序期間只會傳送單一 `GET` 要求給回呼 URL。該服務必須在 5 秒內收到回覆，且其內文中包含了有盤查字串的回應碼 200。否則，它不會將這個 URL 列入白名單。相反地，它會傳送狀態碼 400 來回應 `POST /v1/register_callback` 要求。如果回呼 URL 已順利列入白名單，則該服務會傳送狀態碼 200 來回應起始 `POST` 要求。

### 安全考量
{: #security-async}

當您順利使用 `POST /v1/register_callback` 方法登錄回呼 URL 時，該服務會將此 URL 列入白名單，以指出它已經過驗證，可以搭配回呼通知一起使用。如果您使用登錄呼叫來指定使用者密碼，列入白名單也表示此 URL 已經過驗證可用來增加安全。對於在非同步 HTTP 介面中使用回呼 URL 的要求，指定使用者密碼可提供這些要求的鑑別和資料完整性。

該服務使用使用者密碼來計算透過其傳送至 URL 的每個回呼通知的有效負載的 HMAC-SHA1 簽章。此服務會透過每個通知的 `X-Callback-Signature` 標頭來傳送簽章。用戶端可以使用密碼來計算自己的每個通知有效負載的簽章。如果其簽章符合 `X-Callback-Signature` 標頭的值，用戶端會知道該通知是由服務傳送，且其內容在傳輸期間未曾變更。此一資訊可保證用戶端非中間人攻擊的受害者。

HTTPS 很適合用於正式作業應用程式。然而，在應用程式開發及原型化期間，該服務支援的 HTTP 型回呼通知可以簡化及加速開發程序，因為它可避免 HTTPS 的費用。

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### 取消登錄回呼 URL
{: #unregister}

您可以呼叫 `POST /v1/unregister_callback` 方法，隨時取消登錄已列入白名單的回呼 URL。取消登錄回呼 URL，對於測試應用程式搭配此服務很有幫助。在取消登錄回呼 URL 之後，您就無法再將它與非同步辨識要求搭配使用。

## 建立工作
{: #create}

您可以呼叫 `POST /v1/recognition` 方法來建立辨識工作。如何學習工作的狀態及結果，視您使用的方法及所傳遞的參數而定：

-   *若要使用回呼通知*，請包含 `callback_url` 查詢參數，以指定該服務要在工作狀態變更時傳送回呼通知的 URL。您也可以指定下列選用的查詢參數：
    -   `events`，以訂閱通知事件清單。依預設，該服務會在工作啟動時（`recognitions.started` 事件）、工作完成時（`recognitions.completed` 事件）以及發生錯誤時（`recognitions.failed` 事件）傳送回呼通知。您可以指定事件的子集，或使用 `recognitions.completed_with_results` 事件（而非 `recognitions.completed` 事件），來包含工作完成通知與結果。
    -   `user_token`，以指定要包含在工作的每個通知中的字串。因為您可以使用有無限個工作的相同回呼 URL，所以您可以運用使用者記號來區分不同工作的通知。
-   *若要使用輪詢*，請省略 `callback_url`、`events` 及 `user_token` 查詢參數。然後，您必須使用 `GET /v1/recognitions` 或 `GET /v1/recognitions/{id}` 方法來檢查工作的狀態，在工作完成時使用後者來擷取結果。

在這兩種情況下，您都可以包括 `results_ttl` 查詢參數，以指定在工作完成之後其結果保持可用的分鐘數。

除了非同步介面專用的前述參數之外，`POST /v1/recognitions` 方法還支援與 WebSocket 和同步 HTTP 介面大部分相同的參數。如需相關資訊，請參閱[參數摘要](/docs/services/speech-to-text/summary.html)。

### 回呼通知
{: #notifications}

如果是使用回呼 URL 建立工作，則當發生指定的事件時，該服務會傳送 HTTP `POST` 回呼通知給已登錄的 URL。基本通知內文包含具有下列結構的 JSON 物件：

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

`id` 欄位識別產生回呼的工作 ID，`event` 欄位識別觸發回呼的事件。`user_token` 欄位包含工作的使用者記號（如果已指定）。否則，該欄位是空字串。如果事件是 `recognitions.completed_with_results`，則物件會包含 `results` 欄位，用來提供辨識要求的結果。用戶端可以使用狀態碼 200 來回應此回呼通知。

如果已使用使用者密碼登錄回呼 URL，該服務也會在回呼通知中傳送 `X-Callback-Signature` 標頭。此標頭指定要求內文的 HMAC-SHA1 簽章。該服務會使用使用者密碼作為金鑰來計算簽章。用戶端可以計算回呼通知的有效負載簽章，以確定它符合標頭中的簽章。例如，下列簡易 Python 程式碼會計算 `notification_payload` 字串的簽章：

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### 回呼範例
{: #callback}

下列範例建立的工作與先前列入白名單的回呼 URL `http://{user_callback_path}/results` 相關聯。此範例傳遞使用者記號 `job25`，以識別該服務所傳送的回呼通知中的工作。此呼叫使用預設事件，因此，當服務傳送回呼通知以指出工作已完成時，使用者必須呼叫 `GET /v1/recognitions/{id}` 方法來擷取結果。此呼叫將辨識要求的 `timestamps` 查詢參數設為 `true`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

此服務會傳回要求的狀態，即 `waiting`，指出此服務正在準備工作以進行處理。其回應包括建立時間、工作 ID 以及可取得工作相關資訊的 URL。

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### 輪詢範例
{: #polling}

下列範例建立的工作與回呼 URL 無關。使用者必須輪詢服務，以瞭解工作何時完成，然後使用 `GET /v1/recognitions/{id}` 方法來擷取結果。如同前一個範例，此呼叫將辨識要求的 `timestamps` 參數設為 `true`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

該服務傳回 `processing` 的狀態，指出它已經在處理該工作，還有建立時間、工作 ID 以及取得工作相關資訊的 URL。

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## 檢查工作的狀態並擷取其結果
{: #job}

您呼叫 `GET /v1/recognitions/{id}` 方法，來檢查以 `id` 路徑參數指定之工作的狀態。回應一律包括工作的 ID 和狀態，以及其建立時間和更新時間。如果狀態是 `completed`，則回應也包括此辨識要求的結果。

`GET /v1/recognitions/{id}` 方法是在下列情況下擷取工作結果的唯一方法：

-   已提交工作，但沒有回呼 URL。
-   已使用回呼 URL 提交工作，但未指定 `recognitions.completed_with_results` 事件。
-   此工作不是 100 個最新未完成的工作之一。當您省略 `ID` 路徑參數時，只會傳回 100 個最新工作。

不過，對於已指定回呼 URL 和 `recognitions.completed_with_results` 事件的工作，您還是可以使用此方法來擷取工作的結果。您可以依需要多次擷取任何工作的結果，只要這些結果仍然可用。在您使用 `DELETE /v1/unrecognitions/{id}` 方法刪除結果之前或工作的存活時間到期之前（以先到者為準），工作及其結果仍然可用。除非您使用 `POST /v1/recognitions` 方法的 `results_ttl` 參數指定不同的存活時間，否則，依預設，結果會在一週後到期。

### 沒有結果的狀態範例
{: #withoutResults}

下列範例檢查具有指定 ID 之工作的狀態。工作尚未完成，因此回應不包含結果。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### 有結果的狀態範例
{: #withResults}

下列範例要求具有指定 ID 之工作的狀態。工作已完成，因此回應包括要求的結果。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## 檢查最新工作的狀態
{: #jobs}

您呼叫 `GET /v1/recognitions` 方法，來檢查最新工作的狀態。此方法傳回與服務認證（用以呼叫此方法）相關聯的最新 100 個未完成之工作的狀態。此方法傳回每個工作的 ID 和狀態，以及其建立時間和更新時間。如果工作是使用回呼 URL 和使用者記號建立的，此方法也會傳回該工作的使用者記號。

回應包括下列其中一種狀態：

-   `waiting`，表示該服務正在準備工作以進行處理。此狀態是所有工作的起始狀態。工作將保持此狀態，直到服務有容量開始處理它為止。
-   `processing`，表示該服務正積極處理工作。
-   `completed`，表示該服務已完成處理工作。如果工作指定了回呼 URL 和事件 `recognitions.completed_with_results`，該服務會在回呼通知中傳送結果。否則，請使用 `GET /v1/recognitions/{id}` 方法來取得結果。
-   `failed`，表示工作因故失敗。

在您使用 `DELETE /v1/unrecognitions/{id}` 方法刪除結果之前或工作的存活時間到期之前（以先到者為準），工作及其結果仍然可用。

### 範例
{: #statusExample}

下列範例要求與呼叫者服務認證相關聯之最新現行工作的狀態。使用者有三個未完成的工作處於不同狀態。第一個工作是使用回呼 URL 和使用者記號建立的。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## 刪除工作
{: #delete-async}

您可以使用 `DELETE /v1/recognitions/{id}` 方法，來刪除使用 `id` 路徑參數指定的工作。通常您從服務取得工作的結果之後就會刪除該工作。刪除工作之後，您就無法再使用其結果。您無法刪除服務正積極處理的工作。

依預設，該服務會維護每個工作的結果，直到工作的存活時間到期為止。預設存活時間為一週，但您可以使用 `POST /v1/recognitions` 方法的 `results_ttl` 參數來指定服務維護結果的分鐘數。

### 範例
{: #deleteExample-async}

下列範例刪除具有指定 ID 的工作：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
