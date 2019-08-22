---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# 輸出特性
{: #output}

{{site.data.keyword.speechtotextfull}} 服務提供下列特性，以指出服務要在其轉錄結果中包含語音辨識要求的資訊。所有輸出參數都是選用參數。
{: shortdesc}

-   如需每個服務介面的簡單語音辨識要求的範例，請參閱[提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)。
-   如需語音辨識回應的範例和說明，請參閱[瞭解辨識結果](/docs/services/speech-to-text?topic=speech-to-text-basic-response)。服務會以 UTF-8 字集傳回所有 JSON 回應內容。
-   如需所有可用語音辨識參數按字母順序排列的清單，包括其狀態（正式發行版本或測試版）和支援的語言，請參閱[參數摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。

## 說話者標籤
{: #speaker_labels}

說話者標籤特性是適用於美式英文、日文及西班牙文（寬頻及窄頻模型）及英式英文（僅限窄頻模型）的測試版功能。
{: note}

說話者標籤可識別哪些人在多參與者交流中講了哪些字組。（標示誰說話以及何時說話，有時稱為*語者自動分段標記*。）您可以使用資訊來建立音訊串流的逐人文字記錄，例如與客戶中心的聯絡。或者，您可以用它將與交談式機器人或虛擬人物的交流變成動態。為了達到最佳效能，請使用至少一分鐘長的音訊。

說話者標籤是針對兩位說話者的情境而最佳化。它們最適合用於在長時間交流中涉及兩個人的電話交談。它們可以處理最多六位說話者，但超過兩位說話者可能會導致效能變化。兩個人的交流通常是透過窄頻媒體進行，但您可以使用說話者標籤搭配受支援的窄頻與寬頻模型。

若要使用此特性，請針對辨識要求將 `speaker_labels` 參數設定為 `true`；依預設，此參數為 `false`。服務會依音訊的個別字組來識別說話者。它依賴字詞的開始和結束時間來識別其說話者。因此，啟用說話者標籤也會強制 `timestamps` 參數成為 `true`（請參閱[字組時間戳記](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)）。

### 說話者標籤範例
{: #speakerLabelsExample}

下列範例要求顯示包含說話者標籤的回應。與 `timestamps` 陣列每個元素相關聯的數值，是音訊中字組的開始及結束時間。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

`transcript` 欄位顯示音訊的最終文字記錄，這會列出所有參與者說出的字組。藉由比較說話者標籤與時間戳記並加以同步化，您可以將交談重新組合成為它發生時的狀態。

<table style="width:50%">
  <caption>表 1. 說話者標籤範例</caption>
  <tr>
    <th style="text-align:left">時間戳記</th>
    <th style="text-align:left">說話者標籤</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

結果會清楚地指出這段兩人交流如何開始：

-   **說話者 2** - "Hello?"
-   **說話者 1** - "Yeah?"
-   **說話者 2** - "Yeah, how's Billy?"

在此範例中，`confidence` 欄位指出服務對其說話者識別的信賴度。針對每個字組，`final` 欄位的值是 `false`。這個值指出服務仍在處理音訊。在完成之前，服務可能會針對個別字組變更其說話者識別或信賴度。

### 說話者標籤的說話者 ID
{: #speakerLabelsIDs}

此範例也說明說話者 ID 有趣的一面。在 `speaker` 欄位中，第一個說話者的 ID 是 `2`，第二個的 ID 是 `1`。在服務處理音訊時，它會逐漸建立起對說話者模式的深入瞭解。因此，它可能會變更個別字組的說話者 ID，也可能新增及移除說話者，直到產生最終結果為止。

因此，說話者 ID 可能不是循序、連續或依序。例如，ID 通常是從 `0` 開始，但在前一個範例中，最早的 ID 是 `1`。服務可能根據音訊的進一步分析，省略較早的說話者 `0` 字組指派。也有可能針對較晚的說話者發生省略。省略可能導致編號的間隙，並為兩位說話者（例如分別標示為 `0` 和 `2`）產生結果。另一個考慮重點是說話者可能離開交談。因此，只參與交談較早階段的參與者，可能不會出現在稍後的結果中。

### 要求說話者標籤的過渡期間結果
{: #speakerLabelsInterim}

透過 WebSocket 介面，您可以要求過渡期間結果，以及說話者標籤（請參閱[過渡期間結果](/docs/services/speech-to-text?topic=speech-to-text-output#interim)）。最終結果通常比過渡期間結果好。但是，過渡期間結果有助於識別文字記錄的發展，及說話者標籤的指派。過渡期間結果可以指出暫時性說話者和 ID 在何處出現或消失。不過，服務可以重複使用它一開始識別但之後重新考量而省略的說話者 ID。因此，ID 可能在過渡期間結果和最終結果中參照兩位不同的說話者。

當您同時要求過渡期間結果和說話者標籤時，長音訊串流的最終結果可能會在起始過渡期間結果傳回之後許久才送達。也可能某些過渡期間結果只包含 `speaker_labels` 欄位，而沒有 `results` 及 `result_index` 欄位。如果您未要求過渡期間結果，服務會傳回最終結果，其中包含 `results` 和 `result_index` 欄位，以及單一個 `speaker_labels` 欄位。

當音訊串流已完成或回應逾時（視何者先發生），服務會傳送最終結果。在任一種情況下，服務只會針對它傳回之說話者標籤的最後一個字組，將 `final` 欄位設為 `true`。

### 說話者標籤的效能考量
{: #speakerLabelsPerformance}

如前所述，說話者標籤特性已針對兩人交談（例如與客服中心的通訊）進行最佳化。因此，您需要考量下列潛在的效能問題：

-   具有單一說話者的音訊效能可能不佳。音訊品質或說話者語音的變化可能會導致服務識別出不存在的額外說話者。這類的說話者被稱為幻覺。
-   同樣地，有一個主要說話者（例如播客）的音訊效能可能不佳。服務傾向於遺漏說話時間量較短的說話者，因此也可能產生幻覺。
-   超過六位說話者的音訊效能未定義。此特性最多可以處理 6 位說話者。
-   短話語的效能可能比長話語的效能不正確。當參與者說話時間量較長時，服務會產生較佳的結果，每位說話者至少需 30 秒。每位說話者可以使用的相對音訊量也可能影響效能。
-   語音的前 30 秒效能可能會降低。它通常會在音訊的 1 分鐘之後，改善到合理的層次。

與所有轉錄一樣，效能也可能因為音訊品質不佳、背景雜訊、人員的說話方式、重疊的說話者及其他音訊層面等而受到影響。

## 關鍵字辨識
{: #keyword_spotting}

關鍵字辨識特性會偵測文字記錄中的指定字串。服務可以多次辨識出相同的關鍵字，並報告每次出現。服務只會在最終結果中辨識關鍵字，而不會在過渡期間結果中辨識。依預設，服務不會執行關鍵字辨識。

若要使用關鍵字辨識，您必須指定下列兩個參數：

-   使用 `keywords` 參數指定要辨識的字串陣列。如果您省略參數或指定空的陣列，則服務不會辨識任何關鍵字。關鍵字字串可以包含多個記號。例如，關鍵字 `Speech to Text` 有三個記號。

    對於美式英文，服務會將每個關鍵字正規化，以符合口說與書寫的字串。例如，它會將數字正規化，以符合它們口說和書寫時的方式。對於其他語言，關鍵字必須指定為口說方式。

    您在單一要求中最多可以辨識 1000 個關鍵字。關鍵字不區分大小寫。
-   使用 `keywords_threshold` 參數，針對關鍵字相符項指定 0.0 到 1.0 之間的機率。臨界值指出服務必須具備的信賴水準下界，如此字組才會符合關鍵字。只有在關鍵字的信賴度大於或等於指定的臨界值時，才會在文字記錄中辨識該關鍵字。

    指定小的臨界值可能會產生許多相符項。如果您指定臨界值，則也必須指定一個以上的關鍵字。省略參數不會傳回任何相符項。

在識別音訊串流中的關鍵字時，關鍵字辨識是必要的。您無法透過處理最終文字記錄來識別關鍵字，因為文字記錄代表服務對於輸入音訊的最佳解碼結果。它不包含信賴分數較低但可能表示有興趣字組的記號。因此，在用戶端上將文字處理工具套用至文字記錄時，可能不會識別關鍵字。需要較豐富的解碼結果表示法，而該表示法只能在伺服器上使用。

### 關鍵字辨識結果
{: #keywordSpottingResults}

服務會在 `keywords_result` 欄位中傳回結果，該欄位是 `results` 陣列的元素。`keywords_result` 欄位是可列舉內容的字典或聯想陣列。每一個內容由指定的關鍵字識別，並包含物件的陣列。服務會針對它為關鍵字找到的每個相符項，傳回一個陣列元素。每個相符項的物件都包含下列欄位：

-   `normalized_text` 是指定的關鍵字，它已正規化為符合輸入音訊的口說詞組。
-   `start_time` 是相符項的開始時間（秒）。
-   `end_time` 是相符項的結束時間（秒）。
-   `confidence` 是服務對於相符項能代表指定關鍵字的信賴度。信賴度必須至少與指定臨界值一樣大，才會包含在結果中。

服務找不到相符項的關鍵字會從陣列中省略。在下列情況下，可能找不到關鍵字：

-   音訊純粹不包含關鍵字。缺少關鍵字是最明顯的說明。
-   臨界值設定太高。服務可能識別出關鍵字，但信賴水準較低，在此情況下它會從結果中省略相符項。
-   包含多個記號的關鍵字字串（例如 `Speech to Text`）說出時在其記號之間有太多靜音。當服務轉錄音訊時，會將串流切成一系列的區塊。每個區塊代表連續的音訊片段，其靜音間隔不會超過半秒鐘。它會建構由這些區塊組成的結果物件陣列。

    只有在下列情況下，服務才會符合多記號關鍵字：

    -   關鍵字的記號位於相同的區塊中。
    -   記號是相鄰的，或者中間隔開的間隙不超過 0.1 秒。

    如果在關鍵字的兩個記號之間有簡短的填補音或非詞彙的話語，例如 "uhm" 或 "well"，則可能發生後者的情況。如需相關資訊，請參閱[猶豫標記](/docs/services/speech-to-text?topic=speech-to-text-basic-response#hesitation)。

### 關鍵字辨識範例
{: #keywordSpottingExample}

下列範例要求會將 `keywords` 參數設定為三個字串（`colorado`、`tornado` 及 `tornadoes`）的 URL 編碼陣列，並將 `keywords_threshold` 參數設定為 `0.5`。服務會找到 `colorado` 及 `tornadoes` 的合格出現項目。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 替代項目數上限
{: #max_alternatives}

`max_alternatives` 參數接受一個整數值，該整數值告訴服務傳回結果的 *n* 個最佳替代假設。依預設，服務只會傳回單一轉錄結果，這相當於將參數設定為 `1`。藉由將 `max_alternatives` 設定為大於 1 的數字，您會要求服務傳回該數目的最佳替代轉錄。（如果您指定 `0` 值，則服務會使用預設值 `1`。）

服務只會針對它傳回的最佳替代項目，報告信賴分數。在大部分情況下，那是選擇的替代項目。

### 替代項目數上限範例
{: #maximumAlternativesExample}

下列範例要求會將 `max_alternatives` 參數設定為 `3`；服務會針對三個替代項目中最可能的項目報告信賴度。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado and Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 過渡期間結果
{: #interim}

只有 WebSocket 介面才能使用過渡期間結果特性。
{: note}

過渡期間結果是轉錄的中間假設，它們在服務傳回其最終結果之前可能會變更。服務只要產生過渡期間結果便會傳回過渡期間結果。過渡期間結果適用於：

-   互動式應用程式和即時轉錄
-   長的音訊串流，這可能需要一些時間才能轉錄

和最終結果相比，過渡期間結果到達的頻率更頻繁也更快。您可以用它們來讓應用程式能更快速回應，或測量轉錄的進度。

若要接收過渡期間結果，請在 WebSocket 辨識要求的 `start` 訊息中，將 `interim_results` JSON 參數設定為 `true`。如果您省略 `interim_results` 參數，或將它設定為 `false`，則服務只會在音訊結束時傳回單一 JSON 文字記錄。請遵循下列準則來使用參數：

-   如果您要進行離線或批次轉錄、不需要最少延遲，並且想要所有音訊有單一 JSON 結果，請省略該參數或將它設定為 `false`。
-   如果您想要讓結果在服務處理音訊的同時以漸進方式送達，或者您想要最少延遲的結果，請將參數設定為 `true`。請記住，服務可以在處理更多音訊之後更新過渡期間結果。

過渡期間結果在 JSON 回應中的表示是將 `final` 欄位設定為 `false`。服務在更進一步處理音訊之後，可以用更正確的轉錄來更新此類結果。最終結果會以 `final` 欄位設定為 `true` 來識別。服務不會對最終結果進行進一步的更新。

### 過渡期間結果範例
{: #interimResultsExample}

下列縮寫範例會要求 WebSocket 要求的過渡期間結果。在其回應中，服務只會針對最終結果將 `final` 屬性設定為 `true`。

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 替代字組
{: #word_alternatives}

替代字組特性（也稱為「混淆網路」，Confusion Network）會針對輸入音訊的字組，報告聲學類似替代項目的假設。例如，`Austin` 字組可能是音訊字組的最佳假設。但 `Boston` 這個字組是相同時間間隔內的另一個可能假設。假設會分享共同的開始和結束時間，但會有不同的拼字，而且通常有不同的信賴分數。

依預設，服務不會報告替代字組。為了指出您想要接收替代項目假設，請使用 `word_alternatives_threshold` 參數，以指定 0.0 與 1.0 之間的機率。臨界值指出服務對於假設必須具備的信賴水準下界，如此才會將假設傳回作為替代字組。只有在假設的信賴度大於或等於指定的臨界值時，才會傳回該假設。

您可以將替代字組想成文字記錄的時間表，該文字記錄被切成較小的間隔，或是匣子。每個匣子可以有一個以上的假設，它們具有不同的拼字和信賴度。`word_alternatives_threshold` 參數控制服務傳回的結果密度。指定小的臨界值可能會產生許多假設。

### 替代字組結果
{: #wordAlternativesResults}

服務會在 `word_alternatives` 欄位中傳回結果，該欄位是 `results` 陣列的元素。`word_alternatives` 欄位是物件的陣列，每個物件都提供替代字組的下列欄位：

-   `start_time` 指出針對其傳回假設的字組，在輸入音訊中開始的時間（以秒為單位）。
-   `end_time` 指出針對其傳回假設的字組，在輸入音訊中結束的時間（以秒為單位）。
-   `alternatives` 是假設物件的陣列。每個物件都包含 `confidence`，它指出服務對於假設的信賴分數；物件也包含 `word`，它會識別假設。信賴度必須至少與指定臨界值一樣大，才會包含在結果中。服務會依信賴度排序替代項目。

### 替代字組範例
{: #wordAlternativesExample}

下列要求範例會指定 `0.9` 的 `word_alternatives_threshold`。輸出包含許多字組的潛在假設和信賴分數，以及其開始與結束時間。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 字組信賴度
{: #word_confidence}

`word_confidence` 參數會告訴服務是否為文字記錄的字組提供信賴度測量值。依預設，服務只會針對整體的最終文字記錄報告信賴度測量。將 `word_confidence` 設定為
`true` 會指示服務針對文字記錄的每個個別字組報告信賴度測量。

信賴度測量指出根據聲學證明，服務預估所轉錄字組正確的程度。信賴分數的範圍是從 0.0 到 1.0。

-   分數為 1.0 指出字組的現行轉錄反映最可能的結果。
-   分數為 0.5 指出字組有百分之 50 的機會為正確。

### 字組信賴度範例
{: #wordConfidenceExample}

下列範例會要求轉錄字組的信賴分數。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 字組時間戳記
{: #word_timestamps}

`timestamps` 參數會告知服務是否針對它轉錄的字組產生時間戳記。依預設，服務不會報告時間戳記。將 `timestamps` 設定為 `true` 會指示服務報告每個字組相對於音訊開始的開始和結束時間（以秒為單位）。

### 字組時間戳記範例
{: #wordTimestampsExample}

下列範例會要求轉錄字組的時間戳記。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 智慧型格式化
{: #smart_formatting}

智慧型格式化特性是適用於美式英文、日文及西班牙文的測試版功能。
{: note}

`smart_formatting` 參數指示服務將下列字串轉換為更為慣用的表示法：

-   日期
-   時間
-   數字的系列
-   電話號碼
-   貨幣值
-   網際網路電子郵件及網址

請將 `smart_formatting` 參數設定為 `true`，以啟用智慧型格式化。依預設，服務不會執行智慧型格式化。

服務只會將智慧型格式化套用至辨識要求的最終文字記錄。它會在將結果傳回給用戶端之前、文字正規化完成之時，套用智慧型格式化。轉換讓文字記錄更容易閱讀，並且可以更妥善地進行轉錄結果的後處理。

### 標點符號（美式英文）
{: #smartFormattingPunctuation}

針對美式英文，此特性還會指示服務將音訊中的下列口說關鍵字字串替換成標點符號。

<table style="width:50%">
  <caption>表 2. 智慧型格式化標點符號關鍵字</caption>
  <tr>
    <th style="width:45%; text-align:left">關鍵字字串</th>
    <th style="text-align:center">產生的標點符號</th>
  </tr>
  <tr>
    <td>
      Comma
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      Period
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      Question mark
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      Exclamation point
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

服務只會在適當的位置將這些關鍵字字串轉換成符號。例如，服務會將口說詞組

```
the warranty period is short period
```
{: codeblock}

轉換成文字記錄中的下列文字

```
the warranty period is short.
```
{: codeblock}

### 語言差異
{: #smartFormattingDifferences}

智慧型格式化是根據文字記錄中出現的明顯關鍵字。支援的語言在處理智慧型格式化時略有不同。

#### 美式英文及西班牙文
{: #smartFormattingEnglishSpanish}

-   時間是以關鍵字例如 `AM`、`PM` 或 `EST` 識別。
-   軍用時間如果以關鍵字 `hours` 識別時會予以轉換。
-   電話號碼必須是 `911` 或具有 10 或 11 位數的號碼，且開頭數字為 `1`。

#### 日文
{: #smartFormattingJapanese}

-   不會轉換網際網路電子郵件及網址。
-   電話號碼必須是 10 或 11 位數，且開頭為日本電話號碼的有效字首。例如，有效的字首包含 `03` 及 `090`。
-   英文字組會轉換為 ASCII（*半角*）字元。例如，<code>&#65321;&#65314;&#65325;</code> 會轉換成 `IBM`。
-   如果沒有足夠的上下文，可能不會轉換語義不明確的術語。例如，我們並不清楚 <code>&#19968;&#26178;</code> 及 <code>&#21313;&#20998;</code> 是否指時間。
-   標點符號的處理方式不論是否有智慧型格式化都相同。例如，根據機率計算，會選取 <code>&#12459;&#12531;&#12510;</code> 或 `,` 其中一個。

### 智慧型格式化範例
{: #smartFormattingExample}

下列範例會要求針對辨識要求使用智慧型格式化，方法是將 `smart_formatting` 參數設定為 `true`。下列幾節顯示智慧型格式化對要求結果的效果。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### 智慧型格式化結果
{: #smartFormattingResults}

下表顯示使用及不使用智慧型格式化的最終文字記錄範例。文字記錄是以美式英文音訊為基礎。

<table summary="每個標題列會接著多列的範例，顯示針對標題中識別之元素的智慧型格式化效果。">
  <caption>表 3. 智慧型格式化範例文字記錄</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">不使用智慧型格式化</th>
    <th id="with_formatting" style="text-align:left">使用智慧型格式化</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>日期</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>時間</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>數字</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>電話號碼</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>貨幣值</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>網際網路電子郵件及網址</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>組合</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### 有長暫停的文字記錄範例
{: #smartFormattingLongPauses}

當話語包含足夠長的靜音時，服務可能將文字記錄分割為兩個以上的最終片段。這會影響格式化的結果，如下列範例所示。

<table>
  <caption>表 4. 長暫停的智慧型格式化範例文字記錄</caption>
  <tr>
    <th style="width:45%; text-align:left">音訊語音</th>
    <th style="text-align:left">格式化的轉錄結果</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## 數字編寫
{: #redaction}

數字編寫特性是適用於美式英文、日文及韓文的測試版功能。
{: note}

`redaction` 參數會指示服務編寫（或遮蔽）最終文字記錄中的數字資料。此特性會編寫連續三位數以上的任何數字，並將每位數取代為一個 `X` 字元。其目的是要編寫機密數字資料，例如信用卡號碼。

依預設，服務不會編寫數字資料。請將 `redaction` 參數設定為 `true`，以啟用數字編寫。當您啟用編寫時，服務會自動啟用智慧型格式化，而不論您是否明確停用該特性。為了確保最大安全，服務也會在您啟用編寫時，施行下列參數值：

-   服務會停用關鍵字辨識，而不論您是否指定 `keywords` 及 `keywords_threshold` 參數的值。
-   服務會停用過渡期間結果，而不論您是否將 WebSocket 介面的 `interim_results` 參數設定為 `true`。
-   服務只會傳回單一個最終文字記錄，而不論您是否指定 `maximum_alternatives` 參數的值。

特性的設計與現有的智慧型格式化特性相似。服務只會在將結果傳回給用戶端之前、文字正規化完成之後，對最終文字記錄套用編寫。

特性的未來版本可能會推廣編寫，遮蔽所有電話號碼、街道地址、銀行帳戶、社會安全號碼及其他機密數字資料。
{: note}

### 語言差異
{: #redactionDifferences}

此特性針對美式英文模型的運作完全如此處所述，但針對日文與韓文模型有下列差異。

#### 日文
{: #redactionJapanese}

日文編寫有下列差異：

-   除了遮蔽連續三位數的字串，編寫也會遮蔽街道地址和數字，不論它們是否包含少於三位數。
-   同樣地，編寫也會遮蔽日文樣式出生日期中的日期資訊。在日文，日期資訊通常以拉丁文 *Anno Domini* 格式呈現，但有時會遵循日文樣式，特別是出生日期。在此情況下，年份和月份會經過遮蔽，即使它們只包含一位數或兩位數也一樣。例如，數字編寫會變更下列字串，如下所示。

    <table style="width:50%">
      <caption>表 5. 日文樣式出生日期的編寫範例</caption>
      <tr>
        <th style="text-align:left">不使用編寫</th>
        <th style="text-align:left">使用編寫</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### 韓文
{: #redactionKorean}

韓文編寫有下列差異：

-   不支援智慧型格式化特性。服務仍會針對韓文執行數字編寫，但它不會執行其他智慧型格式化。
-   隔離的數字字元會減少，但包含在韓文詞組當中的可能數字字元則否。例如，下列詞組中的字元 <code>&#51060;</code> 不會取代為 `X`，因為它與下個字元相鄰：

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    如果 <code>&#51060;</code> 字元與接著的字元以空格區隔，則它會取代為 `X`，如[數字編寫結果](#redactionResults)中所述。

### 數字編寫範例
{: #redactionExample}

下列範例會要求針對辨識要求使用數字編寫，方法是將 `redaction` 參數設定為 `true`。因為要求會啟用編寫，所以服務會隱含地啟用要求的智慧型格式化。服務實際上會停用要求的其他參數，讓它們沒有效果：服務傳回單一最終文字記錄，並且未辨識任何關鍵字。下節顯示編寫的效果。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### 數字編寫結果
{: #redactionResults}

下表顯示每個支援語言中，使用及不使用數字編寫的最終文字記錄範例。

<table style="width:90%" summary="每個標題列會識別一種語言，且後面接著單一列，其中顯示不啟用和啟用數字編寫情況下的相同文字記錄。">
  <caption>表 6. 數字編寫範例文字記錄</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">不使用編寫</th>
    <th id="with_redaction" style="text-align:left">使用編寫</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>美式英文</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>日文</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>韓文</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## 褻瀆過濾
{: #profanity_filter}

褻瀆過濾特性只針對美式英文正式發行。
{: note}

`profanity_filter` 參數指出服務是否要審查其結果的褻瀆。依預設，服務會在文字記錄中將所有褻瀆取代為一系列的星號，以遮蔽所有褻瀆。將參數設定為 `false` 會完全依照轉錄顯示輸出裡的字組。

服務會審查所有最終文字記錄以及任何替代文字記錄的褻瀆。它也會審查與替代字組、字組信賴度及字組時間戳記相關聯之結果的褻瀆。唯一的例外是關鍵字辨識，針對關鍵字辨識，服務會如使用者所指定地傳回所有字組，而不論 `profanity_filter` 是否為 `true`。

### 褻瀆過濾範例
{: #profanityFilteringExample}

下列範例顯示簡短音訊檔的結果，該音訊檔在轉錄時使用了 `profanity_filter` 參數的預設 `true` 值。要求也將 `word_alternatives_threshold` 參數設定為相當高的值 `0.99`，並將 `word_confidence` 及 `timestamps` 參數設定為 `true`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
