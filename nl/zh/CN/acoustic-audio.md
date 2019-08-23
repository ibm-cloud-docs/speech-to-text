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

# 管理音频资源
{: #manageAudio}

定制接口包含 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法，用于向定制声学模型添加音频资源。有关更多信息，请参阅[向定制声学模型添加音频](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)。该接口还包含以下方法，用于列出和删除定制声学模型的音频资源。
{: shortdesc}

## 列出定制声学模型的音频资源
{: #listAudio}

定制接口提供了两种方法，用于列出有关定制声学模型的音频资源的信息：

-   `GET /v1/acoustic_customizations/{customization_id}/audio` 方法用于列出有关已添加到定制模型的所有音频资源的信息。
-   `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法用于列出有关定制模型的指定音频资源的信息。

这两种方法都会返回音频资源的名称 (`name`) 以及以下更多信息：

-   `duration` 是音频的长度（以秒为单位）。对于归档文件，此数字表示归档中包含的所有音频文件的累计持续时间。
-   `details` 标识资源的类型：`audio` 或 `archive`。（如果服务无法验证资源，例如可能因为用户错误地传递了不包含音频的文件，那么类型为 `undetermined`。）对于音频文件，详细信息包含音频的 `codec` 和 `frequency`。对于归档文件，详细信息包含其 `compression` 类型。

此外，列出模型的所有音频资源将返回对该模型的所有有效音频资源求和的 `total_minutes_of_audio`。可以使用此值来确定定制模型用于开始训练的音频是足够还是太多。

这些方法还会列出音频数据的状态。在检查服务对音频文件的分析以响应将音频文件添加到定制模型的请求时，该状态非常重要：

-   `ok` 指示服务已成功分析了音频数据。数据可用于训练定制模型。
-   `being_processed` 指示服务仍在分析音频数据。在分析完成之前，服务无法接受添加新音频或训练定制模型的请求。
-   `invalid` 指示音频数据对于训练模型无效（可能是因为音频数据的格式或采样率不正确，或者因为音频数据已损坏）。

### 示例请求：列出所有音频资源
{: #listExample-audio}

以下示例列出了具有指定定制标识的定制声学模型的所有音频资源。声学模型有三个音频资源。服务已成功分析 `audio1` 和 `audio2`；服务仍在分析 `audio3`。

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

### 示例请求：获取音频类型资源的信息
{: #getExampleAudio}

以下示例返回有关名为 `audio1` 的音频类型资源的信息。资源的长度为 131 秒，并已使用 `pcm_s16le` 编码解码器进行编码。此资源已成功添加到模型。

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

### 示例请求：获取归档类型资源的信息
{: #getExampleArchive}

以下示例返回有关名为 `audio2` 的归档类型资源的信息。资源是包含超过 9 分钟音频的 **.zip** 文件。此资源也已成功添加到模型。如示例所示，查询有关归档类型资源的信息还会提供有关其所包含文件的信息。

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

## 从定制声学模型中删除音频资源
{: #deleteAudio}

使用 `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法可从定制声学模型中除去现有音频资源。删除归档类型音频资源时，服务会除去整个文件归档。服务不允许从归档资源中删除单个文件。

除去音频资源并不会影响定制模型，直到使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法，基于更新的数据对定制模型进行了训练。如果基于资源成功训练了模型，那么现有音频数据会继续用于语音识别，直到您重新训练模型为止。

### 示例请求
{: #deleteExample-audio}

以下方法从具有指定定制标识的定制模型中删除名为 `audio3` 的音频资源：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
