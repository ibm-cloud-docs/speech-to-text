---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# 同步 HTTP 介面
{: #http}

{{site.data.keyword.speechtotextfull}} 服務的同步 HTTP 介面提供了單一 `POST /v1/recognize` 方法，以便向服務要求語音辨識。這個方法是取得文字記錄的最簡單方法。它提供兩種方式來提交語音辨識要求：
{: shortdesc}

-   第一種會透過要求的內文，在單一串流中傳送所有音訊。請將作業的參數指定為要求標頭和查詢參數。如需相關資訊，請參閱[提出基本 HTTP 要求](#HTTP-basic)。
-   第二種會將音訊作為多組件要求進行傳送。請將要求的參數指定為要求標頭、查詢參數和 JSON meta 資料的組合。如需相關資訊，請參閱[提出多組件 HTTP 要求](#HTTP-multi)。

在單一要求中請提交最多 100 MB、最少 100 個位元組的音訊資料。如需音訊格式和使用壓縮讓用要求傳送之音訊量最大化的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text/audio-formats.html)。如需 HTTP 介面所有方法的相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/speech-to-text){: new_window}。


## 提出基本 HTTP 要求
{: #HTTP-basic}

HTTP `POST /v1/recognize` 方法提供了簡單的音訊轉錄方法。請透過要求的內文傳遞所有音訊，並指定參數作為要求標頭和查詢參數。

下列 `curl` 範例會針對名為 `audio-file.flac` 的單一 FLAC 檔傳送辨識要求。要求會省略 `model` 查詢參數，以使用預設語言模型 `en-US_BroadbandModel`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

此範例傳回音訊的下列文字記錄：

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

`POST /v1/recognize` 方法只有在處理要求的所有音訊之後才會傳回結果。此方法適用於批次處理，但不適用於即時語音辨識。請使用 WebSocket 介面來轉錄即時音訊。

如果您的資料包含多個音訊檔，則要提交音訊的建議方法是傳送多個要求，每個音訊檔一個要求。您可以用迴圈來提交要求，並選擇性地使用平行化，以增進效能。您也可以使用多組件語音辨識，以單一要求傳遞多個音訊檔。

## 提出多組件 HTTP 要求
{: #HTTP-multi}

`POST /v1/recognize` 方法也支援多組件要求。請將所有音訊資料當作多組件表單資料來傳遞。請將部分參數指定為要求標頭及查詢參數，但將 JSON meta 資料以表單資料的形式傳遞，以控制轉錄的最多層面。

多組件語音辨識是要用於下列使用案例：

-   使用單一語音辨識要求來傳遞多個音訊檔。
-   使用已停用 JavaScript 的瀏覽器。根據表單資料的多組件要求不需要使用 JavaScript。
-   辨識要求的參數大於 8 KB 時（這是大部分 HTTP 伺服器和 Proxy 所強制的限制）。例如，辨識非常大量的關鍵字可能使要求的大小增加超出此限制。多組件要求使用表單資料來避免此限制。

下列各節說明您用於多組件要求的參數，並顯示要求範例。

### 多組件要求的參數
{: #multipartParameters}

您可以將多組件語音辨識的下列參數指定為要求標頭、查詢參數和表單資料。

<table summary="表格的每一列說明多組件辨識要求的一個可能參數使用。">
  <caption>表 1. 多組件要求的參數</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">參數</th>
    <th id="description" style="text-align:center; width:80%">說明</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>表單資料</em>
      <br/><em>物件</em>
    </td>
    <td>
      <em>必要。</em>JSON 物件，提供要求的轉錄參數。物件必須是表單資料的第一部分。該資訊說明表單資料後續部分中的音訊。請參閱[多組件要求的 JSON meta 資料](#multipartJSON)。</td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>表單資料</em>
      <br/><em>檔案</em>
    </td>
    <td>
      <em>必要。</em>一個以上的音訊檔，作為要求之表單資料其餘部分。所有音訊檔的格式必須相同。利用 `curl` 指令，針對要求的每個檔案，包含個別的 <code>--form</code> 選項。</td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>標頭</em>
      <br/><em>字串</em>
    </td>
    <td>
      <em>必要。</em>請指定 `multipart/form-data` 以指出如何將資料傳遞至方法。請用 JSON `part_content_type` 參數指定音訊的內容類型。
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>標頭</em>
      <br/><em>字串</em>
    </td>
    <td>
      <em>選用。</em>請指定 `chunked` 以將音訊資料串流至服務。如果您使用單一要求來傳送所有音訊，請省略此參數。</td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>查詢</em>
      <br/><em>字串</em>
    </td>
    <td>
      <em>選用。</em>要與要求搭配使用之模型的 ID。預設值是 `en-US_BroadbandModel`。</td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>查詢</em>
      <br/><em>字串</em>
    </td>
    <td>
      <em>選用。</em>要與要求搭配使用之自訂語言模型的 GUID。</td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>查詢</em>
      <br/><em>字串</em>
    </td>
    <td>
      <em>選用。</em>要與要求搭配使用之自訂聲學模型的 GUID。</td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>查詢</em>
      <br/><em>字串</em>
    </td>
    <td>
      <em>選用。</em>要與要求搭配使用之指定基礎模型的版本。</td>
  </tr>
</table>

如需查詢參數的相關資訊，請參閱[參數摘要](/docs/services/speech-to-text/summary.html)。

### 多組件要求的 JSON meta 資料
{: #multipartJSON}

您使用多組件要求傳遞的 JSON meta 資料可以包含下列欄位：

-   `part_content_type` (string)
-   `data_parts_count` (integer)
-   `customization_weight` (number)
-   `inactivity_timeout` (integer)
-   `keywords` (string[])
-   `keywords_threshold` (number)
-   `max_alternatives` (integer)
-   `word_alternatives_threshold` (number)
-   `word_confidence` (boolean)
-   `timestamps` (boolean)
-   `profanity_filter` (boolean)
-   `smart_formatting` (boolean)
-   `speaker_labels` (boolean)
-   `grammar_name` (string)
-   `redaction` (boolean)

只有下列兩個參數是多組件要求所特有：

-   對於大部分音訊格式，`part_content_type` 欄位為*選用性*。對於 `audio/alaw`、`audio/basic`、`audio/l16` 及 `audio/mulaw` 格式而言是必要。它指定要求接下來各組件中的音訊格式。所有音訊檔的格式必須相同。
-   對於所有要求，`data_parts_count` 欄位為*選用性*。它指定隨要求一起傳送的音訊檔個數。服務會將「串流結束」偵測套用到最後一個（可能是唯一的一個）資料組件。如果您省略該參數，則服務會從要求判斷組件數。

meta 資料的所有其他參數都是選用性的。如需所有可用參數的說明，請參閱[參數摘要](/docs/services/speech-to-text/summary.html)。

### 多組件要求範例

下列 `curl` 範例顯示如何使用 `POST /v1/recognize` 方法來傳遞多組件辨識要求。要求會傳遞兩個音訊檔：**audio-file1.flac** 和 **audio-file2.flac**。`metadata` 參數提供要求的大部分參數；`upload` 參數提供音訊檔。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

此範例傳回音訊檔的下列文字記錄。服務會依其傳送順序傳回兩個檔案的結果。（輸出範例中已將第二個檔案的結果縮寫。）

```javascript
{
   "results": [
      {
         "word_alternatives": [
            {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
               "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
            {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
               "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
               "end_time": 6.04
            }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
