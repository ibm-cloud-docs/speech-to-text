---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# 瞭解辨識結果
{: #basic-response}

不論您使用的介面為何，{{site.data.keyword.speechtotextfull}} 服務都會傳回轉錄結果，其中反映您指定的輸入及輸出參數。服務會以 UTF-8 字集傳回所有 JSON 回應內容。
{: shortdesc}

## 基本轉錄回應
{: #response}

服務會針對[提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)中的範例，傳回下列回應。範例只會傳遞音訊檔及其內容類型。音訊會講出單一句子，並且在字組之間沒有顯著的暫停。

```javascript
{
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

服務會傳回 `SpeechRecognitionResults` 物件，這是最上層回應物件。對於這些簡單的要求，物件包含了一個 `results` 欄位和一個 `result_index` 欄位：

-   `results` 欄位提供轉錄結果的相關資訊陣列。在此範例中，`alternatives` 欄位包含結果中的 `transcript` 及服務的 `confidence`。`final` 欄位的值是 `true`，以指出這些結果不會變更。
-   `result_index` 欄位提供結果的唯一 ID。此範例顯示具有無暫停之單一音訊檔的要求最終結果，且要求不包含其他任何參數。因此，服務會傳回單一 `result_index` 欄位，其值為 `0`，這永遠是起始索引。

如果輸入音訊更複雜，或是要求包含了其他參數，則結果包含的資訊可能會多出許多。

### alternatives 欄位
{: #responseAlternatives}

`alternatives` 欄位提供轉錄結果的陣列。對於此要求，陣列包含單一元素。

-   `confidence` 欄位是一個分數，指出服務對於文字記錄的信賴度，在此範例中這接近百分之 90。
-   `transcript` 欄位提供轉錄的結果。

`final` 和 `result_index` 欄位符合這些欄位的意義。

### final 欄位
{: #responseFinal}

`final` 欄位指出文字記錄是否顯示最終轉錄結果：

-   對於最終結果（保證不會變更），此欄位是 `true`。服務對於它傳回作為最終結果的文字記錄，不會再傳送進一步的更新。
-   對於過渡期間結果（可能會變更），此欄位是 `false`。如果您使用 `interim_results` 參數搭配 WebSocket 介面，服務會在轉錄音訊時傳回發展中的過渡期間假設，其格式為多個 `results` 欄位。對於過渡期間結果，`final` 欄位一律為 `false`。服務會針對音訊的最終結果將欄位設定為 `true`。服務不會再傳送該音訊轉錄的進一步更新。

若要取得過渡期間結果，請使用 WebSocket 介面並將 `interim_results` 參數設定為 `true`。如需相關資訊，請參閱[過渡期間結果](/docs/services/speech-to-text?topic=speech-to-text-output#interim)。

### result_index 欄位
{: #responseResultIndex}

`result_index` 欄位提供對於該要求而言唯一的結果 ID。如果您要求過渡期間結果，則服務會針對輸入音訊的發展中假設傳送多個 `results` 欄位。相同音訊的過渡期間結果索引一律會有相同的值，相同音訊的最終結果也是一樣。

您收到任何音訊的最終結果之後，服務就不會針對要求的其餘部分以該索引傳送任何進一步的結果。任何進一步結果的索引會增加 1。

不論您是否要求過渡期間結果，如果您的音訊包含暫停或有長時段為靜音，則服務可能會以不同的索引傳回多個最終結果。如需相關資訊，請參閱[暫停與靜音](#pauses-silence)。

如果您的音訊產生多個最終結果，請連結最終結果的 `transcript` 元素，以組合音訊的完整轉錄。請依 `result_index` 的順序來組合結果。您可以忽略 `final` 欄位為 `false` 的過渡期間結果。

### 其他回應內容
{: #responseAdditional}

許多可用於語音辨識的輸出參數都會影響服務回應的內容。例如，下列參數會導致服務傳回音訊的其他相關資訊：

-   `max_alternatives` 參數會要求多個替代文字記錄。`alternatives` 陣列包含每個替代項目的個別元素。陣列的每一個元素都包含 `transcript` 欄位。只有最可能的替代項目會包含 `confidence` 欄位。
-   `word_confidence` 及 `timestamps` 參數會針對文字記錄的個別字組要求其他相關資訊。`alternatives` 陣列包含具有那些名稱的其他欄位。
-   `keywords` 及 `keywords_threshold` 參數會要求關鍵字辨識。`results` 陣列會包含 `keywords_result` 欄位。
-   `word_alternatives_threshold` 參數會要求個別字組的聲學類似結果。`results` 陣列會包含 `word_alternatives` 欄位。
-   WebSocket 介面的 `interim_results` 參數會要求過渡期間轉錄假設。如前所述，服務會在轉錄音訊時傳送多個回應。
-   `speaker_labels` 參數會識別多參與者交流的個別說話者。回應包含 `speaker_labels` 欄位，其層次與 `results` 及 `results_index` 欄位相同。

如需這些參數及其他可能影響服務回應之參數的相關資訊，請參閱[輸出特性](/docs/services/speech-to-text?topic=speech-to-text-output)。如需所有可用參數的相關資訊，請參閱[參數摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。

## 暫停與靜音
{: #pauses-silence}

服務會轉錄整個音訊串流，直到串流結束或發生逾時為止。不過，如果音訊在說出的字組或詞組之間包含了暫停或長時間的靜音，辨識結果可能會包含多個最終結果。

服務用來判斷不同最終結果的預設暫停間隔大約是一秒鐘。對於大部分語言而言，暫停間隔精確地說是 0.8 秒；中文的間隔是 0.6 秒。您無法變更暫停間隔的持續時間。

服務傳回結果的方式，取決於您使用的介面。下列範例顯示來自 HTTP 及 WebSocket 介面的兩個最終結果的回應。在兩個案例中都使用相同的輸入音訊。音訊會說出詞組 "one two three four five six"，並且在 "three" 和 "four" 兩個字組之間有一秒鐘的暫停。

-   *對於 HTTP 介面，*服務會傳送單一 `SpeechRecognitionResults` 物件。`alternatives` 陣列針對每個最終結果都有個別的元素。回應具有單一 `result_index` 欄位，其值為 `0`。

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *對於 WebSocket 介面，*服務會傳送兩個不同的回應，具有兩個不同的 `SpeechRecognitionResults` 物件。每個回應物件有不同的 `result_index` 欄位，第一個回應的值為 `0`，第二個回應的值則是 `1`。

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

如果您的結果包含多個最終結果，請連結最終結果的 `transcript` 元素，以組合音訊的完整轉錄。

串流音訊中長達 30 秒的靜音可能導致[閒置逾時](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity)。
{: note}

## 猶豫標記
{: #hesitation}

當服務發現語音中有簡短的填補音或暫停時，可以在文字記錄中包含猶豫標記。這類的暫停也稱為「不流利」，可能包含例如 "uhm"、"uh"、"hmm" 之類的填補音，和相關的非詞彙話語。除非您的應用程式需要用到它們，否則可以安全地過濾掉文字記錄中的猶豫標記。

在英文中，服務會使用猶豫記號 `%HESITATION`，如下列範例所示。其他語言可能使用不同的標記；例如，日文使用記號 `D_`。

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

猶豫標記也可能出現在文字記錄的其他欄位。例如，如果您要求文字記錄之個別字組的[字組時間戳記](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)，則服務會報告每個猶豫標記的開始和結束時間。

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 大寫
{: #capitalization}

對於大部分的語言，服務不會在回應文字記錄中使用大寫。如果大寫對您的應用程式而言非常重要，您必須將每一句的第一個字組以及適合大寫的任何其他詞彙都大寫。

*僅限於美式英文，*服務會將許多專有名詞大寫。例如，服務會針對指定詞組傳回下列文字：

```
Barack Obama graduated from Columbia University
```
{: codeblock}

對於其他語言，服務會傳回下列文字：

```
barack obama graduated from columbia university
```
{: codeblock}

服務一律會將此大寫套用至美式英文，而不論您是否使用智慧型格式化。

## 標點符號
{: #punctuation}

依預設，服務不會在回應文字記錄中插入標點符號。您必須將您需要的任何標點符號新增至服務的結果中。

對於某些語言，您可以使用智慧型格式化，來指示服務用標點符號（例如逗點、句點、問號和驚嘆號）替代某些關鍵字字串。如需相關資訊，請參閱[智慧型格式化](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。

