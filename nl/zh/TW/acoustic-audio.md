---

copyright:
  years: 2019
lastupdated: "2019-06-19"

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

# 管理音訊資源
{: #manageAudio}

自訂作業介面包含 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法，這用來新增音訊資源到自訂聲學模型。如需相關資訊，請參閱[將音訊新增至自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)。介面也包含下列方法，以便列出和刪除自訂聲學模型的音訊資源。
{: shortdesc}

## 列出自訂聲學模型的音訊資源
{: #listAudio}

自訂作業介面提供兩個方法來列出自訂聲學模型的音訊資源相關資訊：

-   `GET /v1/acoustic_customizations/{customization_id}/audio` 方法會列出已新增至自訂模型之所有音訊資源的相關資訊。
-   `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法會列出自訂模型之指定音訊資源的相關資訊。

兩個方法都會傳回音訊資源的 `name` 以及下列其他資訊：

-   `duration` 是音訊的長度（以秒為單位）。針對保存檔，此數字代表保存檔中所包含之所有音訊檔的累計持續時間。
-   `details` 識別資源的類型：`audio` 或 `archive`。（如果服務無法驗證資源，則類型是 `undetermined`，無法驗證的原因有可能是因為使用者誤傳遞了不包含音訊的檔案。）針對音訊檔，詳細資料包含音訊的 `codec` 和 `frequency`。針對保存檔，它們則包含其 `compression` 類型。

此外，列出模型的所有音訊資源，會傳回針對模型所有有效音訊資源加總的 `total_minutes_of_audio`。您可以使用這個值來判斷自訂模型是否有足夠的音訊可以開始訓練，或是具有過多音訊。

方法也會列出音訊資料的狀態。狀態對於檢查服務所進行的音訊檔分析，以便回應將它們新增至自訂模型的要求而言很重要：

-   `ok` 指出服務已順利分析音訊資料。資料可以用來訓練自訂模型。
-   `being_processed` 指出服務仍在分析音訊資料。在分析完成之前，服務無法接受新增音訊或訓練自訂模型的要求。
-   `invalid` 指出音訊資料對於訓練模型而言無效（可能是因為它具有錯誤的格式或取樣率，或是因為它已毀損）。

### 要求範例：列出所有音訊資源
{: #listExample-audio}

下列範例列出具有指定自訂作業 ID 之自訂聲學模型的所有音訊資源。聲學模型有三個音訊資源。服務已順利分析 `audio1` 及 `audio2`；它仍在分析 `audio3`。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### 要求範例：取得音訊類型資源的資訊
{: #getExampleAudio}

下列範例會傳回名為 `audio1` 之音訊類型資源的相關資訊。資源長度為 131 秒，並且以 `pcm_s16le` 轉碼器編碼。它已順利新增至模型。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

```javascript
{
  "duration": 131,
  "name": "audio1",
  "details": {
    "codec": "pcm_s16le",
    "type": "audio",
    "frequency": 22050
  }
  "status": "ok"
}
```
{: codeblock}

### 要求範例：取得保存檔類型資源的資訊
{: #getExampleArchive}

下列範例會傳回名為 `audio2` 之保存檔類型資源的相關資訊。資源是一個 **.zip** 檔，檔案中包含超過 9 分鐘的音訊。它也已順利新增至模型。如範例所示，查詢保存檔類型資源的相關資訊，也會提供它所包含之檔案的相關資訊。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

```javascript
{
  "container": {
    "duration": 556,
    "name": "audio2",
    "details": {
      "type": "archive",
      "compression": "zip"
    },
    "status": "ok"
  },
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## 從自訂聲學模型刪除音訊資源
{: #deleteAudio}

請使用 `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法，從自訂聲學模型移除現有音訊資源。當您刪除保存檔類型的音訊資源時，服務會移除檔案的整個保存檔。此服務不容許從保存檔資源刪除個別檔案。

移除音訊資源並不會影響自訂模型，直到您使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法，根據更新過的資料來訓練模型為止。如果您順利地根據資源來訓練模型，則在您重新訓練模型之前，現有音訊資料會繼續用於語音辨識。

### 要求範例
{: #deleteExample-audio}

下列方法會從具有指定自訂作業 ID 的自訂模型，刪除名為 `audio3` 的音訊資源：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
