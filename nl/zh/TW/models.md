---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# 語言和模型
{: #models}

{{site.data.keyword.speechtotextfull}} 服務支援許多語言的語音辨識。對於所有介面，您可以使用 `model` 參數來指定語音辨識要求的模型。模型指出說出該音訊所用的語言，以及它的取樣率。
{: shortdesc}

## 支援的語言模型
{: #modelsList}

對於大部分的語言，服務同時支援寬頻及窄頻模型：

-   *寬頻模型* 適用於以大於或等於 16 kHz 取樣的音訊。針對可回應的即時應用程式（例如，即時語音應用程式），請使用寬頻模型。
-   *窄頻模型* 適用於以 8 kHz 取樣的音訊。針對電話語音的離線解碼（這是此取樣率的一般用途），請使用窄頻模型。

為您的應用程式選擇正確的模型很重要。請使用符合您音訊取樣率（和語言）的模型。服務會自動調整音訊的取樣率，以符合您指定的模型。如需相關資訊，請參閱[取樣率](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#samplingRate)。

為了達到最佳辨識正確性，您還需要考慮音訊的頻率內容。如需相關資訊，請參閱[音訊頻率](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#frequency)。
{: tip}

表 1 列出每一種語言的支援模型。如果您從要求省略 `model` 參數，依預設，服務會使用美式英文寬頻模型 `en-US_BroadbandModel`。除了標示為*測試版* 的語言外，其他所有語言均已正式發行 (*GA*)，可供正式作業使用。

<table>
  <caption>表 1. 支援的語言模型</caption>
  <tr>
    <th style="text-align:left">語言 </th>
    <th style="text-align:center">寬頻模型</th>
    <th style="text-align:center">窄頻模型</th>
  </tr>
  <tr>
    <td>阿拉伯文（現代標準）</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">不支援</td>
  </tr>
  <tr>
    <td>巴西葡萄牙文</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>中文（普通話）</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>英文（英國）</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>英文（美國）</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>法文</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>德文</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>日文</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>韓文</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>西班牙文（阿根廷，測試版）</td>
    <td style="text-align:center"><code>es-AR_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-AR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>西班牙文（卡斯提亞）</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>西班牙文（智利，測試版）</td>
    <td style="text-align:center"><code>es-CL_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-CL_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>西班牙文（哥倫比亞，測試版）</td>
    <td style="text-align:center"><code>es-CO_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-CO_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>西班牙文（墨西哥，測試版）</td>
    <td style="text-align:center"><code>es-MX_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-MX_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>西班牙文（秘魯，測試版）</td>
    <td style="text-align:center"><code>es-PE_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-PE_NarrowbandModel</code></td>
  </tr>
</table>

### 美式英文短格式模型
{: #modelsShortform}

美式英文短格式模型 `en-US_ShortForm_NarrowbandModel` 可以改善「互動式語音回應 (IVR)」及「自動化客戶支援」解決方案的語音辨識。短格式模型經過訓練，能夠辨識客戶支援中心設定（例如自動化和人工支援客服中心）中常用的短話語。例如，模型會經過調整，以適應精確的話語，例如數字、單字元字組和名稱拼字，以及是與否的回應。將文法與短格式模型一起使用，可以進一步改善辨識結果。

如同所有模型一樣，吵雜的環境可能會對結果造成不利的影響。例如，來自機場、移動中的車輛、會議室及多位說話者等的背景聲音雜訊可能會降低轉錄正確性。來自會議喇叭的音訊也可能因為這類裝置常見的回音而降低正確性。搭配使用自訂聲學模型和短格式模型，可以抵消這類的效果。

-   如需語言模型及聲學模型自訂作業的相關資訊，請參閱[自訂作業介面](/docs/services/speech-to-text?topic=speech-to-text-customization)。
-   如需文法的相關資訊，請參閱[將文法與自訂語言模型搭配使用](/docs/services/speech-to-text?topic=speech-to-text-grammars)。

### 語言模型範例
{: #modelsExample}

下列 HTTP 要求範例會使用 `en-US-NarrowbandModel` 模型進行語音辨識：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## 列出模型
{: #listModels}

HTTP 介面提供兩個方法來列出所支援模型的相關資訊：

-   `GET /v1/models` 方法會列出所有可用模型的相關資訊。
-   `GET /v1/models/{model_id}` 方法會列出指定模型的相關資訊。

兩種方法都會傳回模型的下列相關資訊：

-   `name` 是您在要求中使用的模型名稱。
-   `language` 是模型的語言 ID（例如 `en-US`）。
-   `rate` 會識別模型使用的取樣率（音訊的最低可接受速率），以赫茲為單位。
-   `url` 是模型的 URI。
-   `description` 提供模型的簡要說明。
-   `supported_features` 說明模型所支援的其他服務特性：
    -   `custom_language_model` 是一個布林值，指出您是否可以建立以該模型為基礎的自訂語言模型。
    -   `speaker_labels` 指出您是否可以使用 `speaker_labels` 參數來搭配該模型。

### 要求與回應範例
{: #listExample}

下列範例列出服務支援的所有模型：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

下列範例顯示美式英文寬頻模型的相關資訊。此模型同時支援語言模型自訂作業及說話者標籤。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
