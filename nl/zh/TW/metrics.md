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

# 度量值特性
{: #metrics}

針對語音辨識要求，{{site.data.keyword.speechtotextfull}} 服務可以傳回兩種類型的選用度量值：

-   [處理度量值](#processing_metrics)提供關於服務對輸入音訊進行處理時的定期資訊。請使用度量值來測量服務的音訊轉錄進度。處理度量值可用於 WebSocket 和非同步 HTTP 介面。
-   [音訊度量值](#audio_metrics)提供輸入音訊信號特徵的資訊。請使用度量值來判斷音訊的特徵及品質。音訊度量值可用於所有語音辨識介面。

依預設，服務不會針對要求傳回任何度量值。

## 處理度量值
{: #processing_metrics}

處理度量值提供關於服務對輸入音訊進行分析時的詳細計時資訊。服務會以指定的間隔傳回度量值，並且附上轉錄事件，例如過渡期間和最終結果。

這些度量值包括有關服務已接收的音訊量、服務已傳輸到語音辨識引擎的音訊量，以及服務已處理音訊的時間長度的統計資料。如果要求說話者標籤，則這些資訊中還會顯示服務為了確定說話者標籤而處理的音訊量。

處理度量值可協助您量測辨識要求的進度。此外，處理度量值還有助於識別由於以下原因造成的結果缺失：

-   缺少音訊。
-   提交的音訊中缺少語音。
-   伺服器上的引擎停滯，以及用戶端與伺服器之間的網路停滯。為了區分是引擎還是網路停滯，結果為定期，而非基於事件。

這些度量值還可以藉由檢查定期到達時間來協助您估計回應抖動。度量值以恆定間隔產生，因此到達時間的任何差異都是由抖動引起的。

### 要求處理度量值
{: #processing_metrics_request}

若要要求處理度量值，請使用下列選用參數：

-   `processing_metrics` 是一個布林值，用於指出服務是否要傳回處理度量值。指定 `true` 以要求這些度量值。依預設，服務不會傳回任何度量值。
-   `processing_metrics_interval` 是一個浮點數，用於指定服務傳回度量值的間隔（按實際時鐘時間，以秒為單位）。依預設，服務會每秒傳回度量值一次。除非 `processing_metrics` 參數設定為 `true`，否則服務會忽略此參數。

    此參數接受的最小值為 0.1 秒。精準度層次不受限制，因此可以指定 0.25 和 0.125 這類的值。服務不會強制實施最大值。

如何提供參數以及服務如何傳回處理度量值因介面而異：

-   使用 WebSocket 介面時，可為語音辨識要求指定具有 JSON `start` 訊息的參數。服務將依所要求的間隔即時計算並傳回度量值。
-   使用非同步 HTTP 介面時，可在語音辨識要求中指定查詢參數。服務將依所要求的間隔計算度量值，但會傳回音訊的所有度量值與最終轉錄結果。

服務還會傳回轉錄事件的處理度量值。如果使用 WebSocket 介面要求過渡期間結果，則接收度量值的頻率可能大於所要求的間隔。

若只要接收針對轉錄事件而非定期間隔的處理度量值，請將處理間隔設定為較大的數字。如果間隔大於音訊的持續時間，則服務只會傳回針對轉錄事件的處理度量值。

### 瞭解處理度量值
{: #processing_metrics_understand}

服務將在 `SpeechRecognitionResults` 物件的 `processing_metrics` 欄位中傳回處理度量值。對於依定期間隔產生的度量值，物件僅包含 `processing_metrics` 欄位，如下列範例中所示。

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": float,
      "seen_by_engine": float,
      "transcription": float,
      "speaker_labels": float
    },
    "wall_clock_since_first_byte_received": float,
    "periodic": boolean
  }
}
```
{: codeblock}

`processing_metrics` 欄位包含的 `ProcessingMetrics` 物件具有下列欄位：

-   `wall_clock_since_first_byte_received` 是自服務收到輸入音訊的第一個位元組以來經過的實際時間長度（以秒為單位）。此欄位中的值通常是指定度量值間隔的倍數，但有兩點不同：
    -   值可能反映的不是精確的間隔，如 0.25、0.5 等。例如，實際值可能是 0.27、0.52 等，具體取決於服務何時收到和處理音訊。
    -   轉錄事件的值與處理間隔無關。該服務會在發生事件驅動結果時將其傳回。
-   `periodic` 指出度量值要套用至定期間隔還是套用至轉錄事件：
    -   `true` 表示回應是由處理間隔所觸發。此資訊僅包含處理度量值。
    -   `false` 表示回應是由轉錄事件所觸發。此資訊包含處理度量值與轉錄結果。

    使用此欄位可確定服務產生回應的原因，並根據需要過濾不同的結果。
-   `processed_audio` 包含 `ProcessedAudio` 物件，用於提供有關服務的輸入音訊處理的詳細計時資訊。

`ProcessedAudio` 物件包含下列欄位。所有欄位都指的是與此回應相關的音訊秒數。僅 `wall_clock_since_first_byte_received` 欄位指的是經過的實際時間。

-   `received` 是服務收到的音訊秒數。
-   `seen_by_engine` 是服務已傳遞到其語音處理引擎的音訊秒數。
-   `transcription` 是服務為了進行語音辨識而處理的音訊秒數。
-   `speaker_labels` 是服務為了獲取說話者標籤而處理的音訊秒數。僅當要求說話者標籤時，回應才會包含此欄位。

語音處理引擎會對輸入音訊進行多次分析。`processed_audio` 物件顯示引擎已處理且不會再次讀取的音訊的值。已處理的音訊對未來的辨識假設沒有效果。

度量值指出引擎處理的進度和複雜性：

-   `seen_by_engine` 是服務已讀取並傳遞到引擎至少一次的音訊。
-   `received` - `seen_by_engine` 是已在服務中緩衝但尚未由引擎查看或處理的音訊。
-   這些時間之間的關係是 `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`。

下列關係也有助於瞭解結果：

-   在語音辨識處理期間，`received` 和 `seen_by_engine` 欄位的值大於 `transcription` 和 `speaker_labels` 欄位的值。服務必須收到音訊後，才能開始處理結果。
-   服務處理完音訊後，`received` 和 `seen_by_engine` 欄位的值完全相同。這兩個欄位的最終值可能比 `transcription` 和 `speaker_labels` 欄位的值稍大幾秒。
-   在語音辨識處理期間，`speaker_labels` 欄位的值通常小於 `transcription` 欄位的值。但服務處理完音訊後，`transcription` 和 `speaker_labels` 欄位的值完全相同。

### 處理度量值範例：WebSocket 介面
{: #processing_metrics_example_websocket}

下列範例顯示為對 WebSocket 介面發出的要求傳遞的 `start` 訊息。要求會啟用處理度量值，並將處理度量值間隔設定為 0.25 秒。此外，還會將 `interim_results` 和 `speaker_labels` 參數設定為 `true`。音訊包含簡單訊息 "hello world long pause stop"。

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/flac',
    processing_metrics: true,
    processing_metrics_interval: 0.25,
    interim_results: true,
    speaker_labels: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}
```
{: codeblock}

下列範例輸出顯示服務為要求傳回的前幾個處理度量值結果。

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.51,
    "periodic": false
  },
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.43,
              0.76
            ],
            [
              "world",
              0.76,
              1.22
            ]
          ],
          "transcript": "hello world "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

### 處理度量值範例：非同步 HTTP 介面
{: #processing_metrics_example_http}

下列範例顯示非同步 HTTP 介面的 `/v1/recognitions` 方法的語音辨識要求。要求會啟用處理度量值，並將間隔指定為 0.25 秒。音訊檔同樣包含訊息 "hello world long pause stop"。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

下列範例輸出顯示服務為要求傳回的前兩個處理度量值結果。

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

## 音訊度量值
{: #audio_metrics}

音訊度量值提供輸入音訊信號特徵的詳細資訊。結果會在語音處理結束時提供整個輸入音訊的聚集度量值。對於技術成熟的使用者，這些度量值可以提供對音訊詳細特徵的有意義洞察。

您可以使用音訊度量值來即時指出輸入音訊中的問題，甚至可能提供潛在的解決方案。例如，可以提供「背景中雜訊太多」或「請更靠近麥克風」等訊息。您還可以使用離線分析工具來檢查信號特徵，並建議適合未來模型更新的資料。

### 要求音訊度量值
{: #audio_metrics_request}

若要要求音訊度量值，請將 `audio_metrics` 布林值參數設定為 `true`。依預設，服務不會傳回任何度量值。

-   使用 WebSocket 介面時，可為語音辨識要求指定具有 JSON `start` 訊息的參數。
-   使用 HTTP 介面時，可在語音辨識要求中指定查詢參數。

服務會傳回音訊度量值與最終轉錄結果。對於整個音訊串流，服務僅會傳回度量值一次。即使服務會傳回多個轉錄結果（對於不同的音訊區塊），也僅在結果的最尾端傳回度量值的單一實例。

WebSocket 介面使用單一連線接受多個音訊串流（或檔案）。向服務傳送 `stop` 訊息或空的二進位 Blob，可對不同的串流進行定界。在此情況下，服務將針對每個定界的音訊串流傳回個別的度量值。

### 瞭解音訊度量值
{: #audio_metrics_understand}

服務將在 `SpeechRecognitionResults` 物件的 `audio_metrics` 欄位中傳回度量值。

```javascript
{
  "results": [
    . . .
  ],
  "result_index": integer,
  "audio_metrics": {
    "sampling_interval": float,
    "accumulated": {
      "final": boolean,
      "end_time": float,
      "signal_to_noise_ratio": float,
      "speech_ratio": float,
      "high_frequency_loss": float,
      "direct_current_offset": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "clipping_rate": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "non_speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ]
    }
  }
}
```
{: codeblock}

`audio_metrics` 欄位包含的 `AudioMetrics` 物件具有兩個欄位：

-   `sampling_interval` 指出服務計算音訊度量值的間隔（以秒為單位，通常為 0.1 秒）。
-   `accumulated` 包含 `AudioMetricsDetails` 物件，用於提供輸入音訊信號特徵的詳細資訊。

`AudioMetricsDetails` 物件的下列欄位提供一元值：

-   `final` 指出度量值是否在音訊串流結束（即轉錄完成時）時提供。目前，該欄位一律為 `true`。服務只會為每個音訊串流傳回度量值一次。結果會提供整個串流的聚集度量值。
-   `end_time` 指定音訊中套用度量值的結束時間（以秒為單位）。因為度量值會套用至整個音訊串流，所以結束時間一律會等於音訊的長度。
-   `signal_to_noise_ratio` 提供音訊信號的訊噪比 (SNR)。該值指出音訊中語音與雜訊的比例。有效值位於 0 到 100 分貝 (dB) 範圍內。如果服務無法計算音訊的 SNR，則將省略此欄位。
-   `speech_ratio` 是音訊信號中語音段與非語音段的比例。該值位於 0.0 到 1.0 範圍內。
-   `high_frequency_loss` 指出音訊信號的頻率內容缺少上半部分的機率。
    -   接近 1.0 的值通常指出人工升頻取樣的音訊，這會對轉錄結果的準確性產生負面影響。
    -   值等於或接近 0.0 指出音訊信號良好且有完整的頻譜。
    -   值約為 0.5 表示頻率內容偵測不可靠或無法使用。

`AudioMetricsDetails` 物件的下列欄位提供信號特徵的直方圖。每個欄位都提供 `AudioMetricsHistogramBin` 物件的陣列。每個直方圖中的單一單位根據音訊的 `sampling_interval` 長度進行計算。

-   `direct_current_offset` 定義音訊信號的累積直流 (DC) 元件的直方圖。
-   `clipping_rate` 定義音訊段削波率的直方圖。削波率定義為區段中達到音訊量化範圍所提供的最大值或最小值的範例所佔比例。

    服務會自動偵測 16 位元脈衝編碼調變 (PCM) 音訊範圍（-32768 到 +32767）或單位範圍（-1.0 到 +1.0）。削波率在 0.0 到 1.0 之間。較高的值指出語音辨識可能退化。
-   `speech_level` 定義包含語音的音訊段中信號層次的直方圖。信號層次以均方根 (RMS) 值進行運算，以分貝 (dB) 為量程，正規化為 0.0（最小層次）到 1.0（最大層次）範圍。
-   `non_speech_level` 定義未包含語音的音訊段中信號層次的直方圖。信號層次以 RMS 值進行運算，以分貝為量程，正規化為 0.0（最小層次）到 1.0（最大層次）範圍。

每個 `AudioMetricsHistogramBin` 物件都說明一個已定義 `begin` 和 `end` 界限的 Bin。每個 Bin 都指出該 Bin 的信號特徵範圍內值的 `count`（即，值的數目）。直方圖的第一個和最後一個 Bin 是界限 Bin。它們分別涵蓋負無限與第一個界限的間隔，以及最後一個界限與正無限的間隔。

### 音訊度量值範例
{: #audio_metrics_example}

下列範例顯示同步 HTTP 介面的語音辨識要求，用於傳回音訊度量值。音訊檔包含簡單訊息 "hello world long pause stop"。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

回應包括完整輸入音訊的音訊度量值，音訊持續時間為 7.0 秒。輸入音訊的語音段（37 段）比非語音段（33 段）略多，`speech_ratio` 為 0.529。削波率極低，指出具有高品質的輸入音訊。

`high_frequency_loss` 欄位的值為 0.5，表示服務的頻率內容偵測不可靠或無法使用。因為服務無法計算音訊的 SNR，所以結果會省略 `signal_to_noise_ratio` 欄位。

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "hello world "
        }
      ],
         "final": true
      },
      {
         "alternatives": [
        {
          "confidence": 0.79,
          "transcript": "long pause "
        }
      ],
         "final": true
      },
      {
         "alternatives": [
        {
          "confidence": 0.97,
          "transcript": "stop "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "audio_metrics": {
    "sampling_interval": 0.1,
    "accumulated": {
      "final": true,
      "end_time": 7.0,
      "speech_ratio": 0.529,
      "high_frequency_loss": 0.5,
      "direct_current_offset": [
        {"begin": -1.0, "end": -0.9, "count": 0},
        {"begin": -0.9, "end": -0.7, "count": 0},
        {"begin": -0.7, "end": -0.5, "count": 0},
        {"begin": -0.5, "end": -0.3, "count": 0},
        {"begin": -0.3, "end": -0.1, "count": 0},
        {"begin": -0.1, "end": 0.1, "count": 70},
        {"begin": 0.1, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "clipping_rate": [
        {"begin": 0.0, "end": 1e-05, "count": 70},
        {"begin": 1e-05, "end": 0.0001, "count": 0},
        {"begin": 0.0001, "end": 0.001, "count": 0},
        {"begin": 0.001, "end": 0.01, "count": 0},
        {"begin": 0.01, "end": 0.1, "count": 0},
        {"begin": 0.1, "end": 1.0, "count": 0}
      ],
      "speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 37},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "non_speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 33},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ]
    }
  }
}
```
{: codeblock}
