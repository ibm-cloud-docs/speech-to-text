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

# 语言和模型
{: #models}

{{site.data.keyword.speechtotextfull}} 服务支持多种语言的语音识别。对于所有接口，可以使用 `model` 参数来指定用于语音识别请求的模型。模型指示音频中所讲的语言以及音频采样率。
{: shortdesc}

## 支持的语言模型
{: #modelsList}

对于大多数语言，服务同时支持宽带和窄带模型：

-   *宽带模型*用于采样率大于或等于 16 千赫兹的音频。请将宽带模型用于响应式实时应用，例如用于实时语音应用。
-   *窄带模型*用于采样率为 8 千赫兹的音频。请将窄带模型用于电话语音的脱机解码，这是此采样率的典型用途。

针对您的应用选择正确的模型很重要。请使用与音频采样率（和语言）相匹配的模型。服务会自动调整音频的采样率，以匹配指定的模型。有关更多信息，请参阅[采样率](/docs/services/speech-to-text/audio-formats.html#samplingRate)。

为了实现最佳识别准确性，还需要考虑音频的频率内容。有关更多信息，请参阅[音频频率](/docs/services/speech-to-text/audio-formats.html#frequency)。
{: tip}

表 1 列出了每种语言支持的模型。如果在请求中省略 `model` 参数，那么缺省情况下，服务会使用美国英语宽带模型 `en-US_BroadbandModel`。

<table>
  <caption>表 1. 支持的语言模型</caption>
  <tr>
    <th style="text-align:left">语言</th>
    <th style="text-align:center">宽带模型</th>
    <th style="text-align:center">窄带模型</th>
  </tr>
  <tr>
    <td>巴西葡萄牙语</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>法语</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>德语</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>日语</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>韩语</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>普通话</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>现代标准阿拉伯语</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">不支持</td>
  </tr>
  <tr>
    <td>西班牙语</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>英国英语</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>美国英语</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
</table>

### 美国英语短格式模型
{: #modelsShortform}

美国英语短格式模型 `en-US_ShortForm_NarrowbandModel` 可以改进用于交互式语音响应 (IVR) 和自动客户支持解决方案的语音识别。短格式模型经过训练，可识别客户支持设置（如自动化和人工支持呼叫中心）中经常表达的简短话语。例如，模型经过调整后，可获得精确的话语，如数字、单字符词和姓名拼写以及是/否响应。将语法与短格式模型组合使用，可以进一步提高识别结果。

与所有模型一样，噪声环境可能会对结果产生负面影响。例如，来自机场、行驶车辆、会议室和多个说话者的背景声学噪声可能会降低转录准确性。说话者的电话有回音很常见，因此来自此类设备的音频也可能会降低准确性。将定制声学模型与短格式模型配合使用可以应对此类影响。

-   有关语言模型和声学模型定制的更多信息，请参阅[定制接口](/docs/services/speech-to-text/custom.html)。
-   有关语法的更多信息，请参阅[将语法用于定制语言模型](/docs/services/speech-to-text/grammar.html)。

### 语言模型示例
{: #modelsExample}

以下示例 HTTP 请求将模型 `en-US-NarrowbandModel` 用于语音识别：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## 列出模型
{: #listModels}

HTTP 接口提供了两种方法，用于列出有关受支持模型的信息：

-   `GET /v1/models` 方法用于列出有关所有可用模型的信息。
-   `GET /v1/models/{model_id}` 方法用于列出有关指定模型的信息。

这两种方法都会返回有关模型的以下信息：

-   `name` 是在请求中使用的模型的名称。
-   `language` 是模型的语言标识（例如，`en-US`）。
-   `rate` 标识模型使用的采样率（音频的最低可接受采样率，以赫兹为单位）。
-   `url` 是模型的 URI。
-   `description` 提供模型的简要描述。
-   `supported_features` 描述模型支持的其他服务功能：
    -   `custom_language_model` 是布尔值，指示是否可以创建基于模型的定制语言模型。
    -   `speaker_labels` 指示是否可以将 `speaker_labels` 参数用于模型。

### 示例请求和响应
{: #listExample}

以下示例列出了服务支持的所有模型：

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

以下示例显示了有关美国英语宽带模型的信息。该模型同时支持语言模型定制和说话者标签。

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
