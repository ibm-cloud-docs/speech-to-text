---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# 创建定制声学模型
{: #acoustic}

执行以下步骤为 {{site.data.keyword.speechtotextshort}} 服务创建定制声学模型：
{: shortdesc}

1.  [创建定制声学模型](#createModel-acoustic)。可以针对相同或不同的域或环境创建多个定制模型。对于创建的任何模型，此过程都是相同的。但是，在识别请求中一次只能指定一个定制声学模型。
1.  [向定制声学模型添加音频](#addAudio)。服务针对声学建模所接受的音频文件格式与针对语音识别所接受的格式相同。此外，服务还接受包含多个音频文件的归档文件。归档文件是添加音频资源的首选方法。可以重复此方法向定制模型添加更多音频或归档文件。
1.  [训练定制声学模型](#trainModel-acoustic)。将音频资源添加到定制模型后，必须对该模型进行训练。通过训练，可准备定制声学模型以用于语音识别。训练可能需要大量时间。训练的长度取决于模型包含的音频数据量。

    您可以在训练定制声学模型期间指定用于帮助的定制语言模型。包含音频文件转录或音频文件域中 OOV 词的定制语言模型可以提高定制声学模型的质量。有关更多信息，请参阅[使用定制语言模型训练定制声学模型](/docs/services/speech-to-text/acoustic-both.html#useBothTrain)。
1.  训练定制模型后，可以将其用于识别请求。如果传递要转录的音频的声学质量与定制模型的音频类似，那么结果会反映为服务的理解能力增强。有关更多信息，请参阅[使用定制声学模型](/docs/services/speech-to-text/acoustic-use.html)。

    可以在同一识别请求中同时传递定制声学模型和定制语言模型，以进一步提高识别准确性。有关更多信息，请参阅[在语音识别期间使用定制语言模型和定制声学模型](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize)。

创建定制声学模型的步骤是迭代的。您可以根据需要随时添加或删除音频，以及训练或重新训练模型。如果对模型的音频进行了任何更改，那么必须重新训练该模型才能使更改生效。重新训练模型时，训练中将使用所有音频数据（而不仅仅是新数据）。因此，训练时间与模型中包含的音频总量相当。

声学模型定制可作为所有语言的 Beta 功能使用。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text/custom.html#languageSupport)。
{: note}

## 创建定制声学模型
{: #createModel-acoustic}

使用 `POST /v1/acoustic_customizations` 方法来创建新的定制声学模型。您可以创建任意数量的定制声学模型，但在语音识别请求中一次只能使用一个定制声学模型。此方法接受用于定义新定制模型属性的 JSON 对象作为请求主体。

<table>
  <caption>表 1. 新定制声学模型的属性</caption>
  <tr>
    <th style="width:20%; text-align:left">参数</th>
    <th style="width:12%; text-align:center">数据类型</th>
    <th style="text-align:left">描述</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>必需</em></td>
    <td style="text-align:center">字符串</td>
    <td>
      新定制声学模型的用户定义名称。请使用描述定制模型声学环境的名称，例如<code>移动定制模型</code>或<code>汽车噪声定制模型</code>。使用的名称应在您拥有的所有定制声学模型中唯一。请使用与定制模型的语言相匹配的本地化名称。
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>必需</em></td>
    <td style="text-align:center">字符串</td>
    <td>
      要由新模型定制的基本模型的名称。必须使用由 <code>GET /v1/models</code> 方法返回的模型的名称。新的定制模型只能用于它所定制的基本模型。</td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>可选</em></td>
    <td style="text-align:center">字符串</td>
    <td>
      新模型的描述。请使用与定制模型的语言相匹配的本地化描述。
    </td>
  </tr>
</table>

以下示例创建名为 `Example acoustic model` 的新定制声学模型。此模型创建用于基本模型 `en-US_BroadbandModel`，并且描述为 `Example custom acoustic model`。`Content-Type` 头指定要将 JSON 数据传递到此方法。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

示例返回了新模型的定制标识。每个定制模型都通过唯一的定制标识进行标识，这是全局唯一标识 (GUID)。使用与模型关联的调用的 `customization_id` 参数可指定定制模型的 GUID。

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

拥有新定制模型的服务实例是其凭证用于创建该模型的实例。有关更多信息，请参阅[定制模型的所有权](/docs/services/speech-to-text/custom.html#customOwner)。

## 向定制声学模型添加音频
{: #addAudio}

创建定制声学模型后，下一步是向其添加音频资源。使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法来向定制模型添加音频资源。可以添加

-   支持语音识别的任何格式的单个音频文件（请参阅[音频格式](/docs/services/speech-to-text/audio-formats.html)）。
-   包含多个音频文件的归档文件（**.zip** 或 **.tar.gz** 文件）。将多个音频文件收集到单个归档文件中并装入该单个文件要比逐个添加音频文件高效得多。

可将音频资源作为请求主体传递，并为资源分配 `audio_name`。有关更多信息，请参阅[使用音频资源](/docs/services/speech-to-text/acoustic-resource.html)。

以下示例说明了音频类型和归档类型资源的添加操作：

-   此示例向具有指定 `customization_id` 的定制声学模型添加音频类型资源。`Content-Type` 头将音频类型标识为 `audio/wav`。音频文件 **audio1.wav** 作为请求的主体传递，并且将该资源命名为 `audio1`。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   此示例向指定的定制声学模型添加归档类型资源。`Content-Type` 头将归档类型标识为 `application/zip`。`Contained-Contented-Type` 头指示归档中包含的所有文件的格式均为 `audio/l16`，采样率为 16 千赫兹。归档文件 **audio2.zip** 作为请求的主体传递，并且将该资源命名为 `audio2`。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

此方法还接受可选的 `allow_overwrite` 查询参数，用于覆盖定制模型的现有音频资源。如果在将音频资源添加到模型后需要更新该资源，请使用该参数。

这是异步方法。根据音频的持续时间，可能需要几秒钟才能完成。对于归档文件，操作的长度取决于音频文件的持续时间。有关检查用于添加音频资源的请求的状态的更多信息，请参阅[监视添加音频请求](#monitorAudio)。

通过对每个音频或归档文件调用此方法一次，可以将任意数量的音频资源添加到定制模型。一个音频资源的添加操作必须完全完成后，才能添加其他音频资源。您必须向定制模型添加最短 10 分钟、最长 200 小时的音频（包含语音，但不包含静默），然后才能对其进行训练。任何音频类型或归档类型资源的大小都不能超过 100 MB。有关更多信息，请参阅[添加音频资源的准则](/docs/services/speech-to-text/acoustic-resource.html#audioGuidelines)。

### 监视添加音频请求
{: #monitorAudio}

如果音频有效，服务会返回 201 响应代码。然后，服务会对音频文件的内容进行异步分析，并自动抽取有关音频的信息，例如长度、采样率和编码。在服务对当前请求的所有音频文件的分析完成之前，无法提交向定制声学模型添加更多音频或训练模型的请求。

要确定请求的状态，请使用 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法来轮询音频的状态。此方法接受定制模型的 GUID 和音频资源的名称。其响应包含资源的状态，即 `status` 字段，它具有下列其中一个值：

-   `ok` 指示音频可接受且分析完成。
-   `being_processed` 指示服务仍在分析音频。
-   `invalid` 指示音频文件是不可接受的，无法进行处理。此文件可能格式错误、采样率错误或者不是音频文件。对于归档文件，如果其中包含的任何音频文件无效，那么整个归档无效。

响应的内容和 `status` 字段的位置取决于资源类型：音频或归档。

-   *对于音频类型资源*，`status` 字段位于顶级 (`AudioListing`) 对象中。

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

-   *对于归档类型资源*，`status` 字段位于嵌套在 `container` 字段中的二级 (`AudioResource`) 对象中。

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
      . . .
    ```
    {: codeblock}

使用循环每几秒检查一次音频资源的状态，直到它变为 `ok`。有关此方法返回的其他字段的更多信息，请参阅[列出定制声学模型的音频资源](/docs/services/speech-to-text/acoustic-audio.html#listAudio)。

## 训练定制声学模型
{: #trainModel-acoustic}

使用音频资源填充定制声学模型后，必须基于新数据来训练模型。通过训练，可准备定制模型以用于语音识别。在基于新数据训练模型之前，无法将该模型用于识别请求。此外，以新增或更改音频资源的形式对定制模型的更新，要在您使用这些更改对该模型进行训练之后，才会由该模型反映出。

使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法来训练定制模型。您将向此方法传递要训练的模型的定制标识，如以下示例中所示。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

此方法还接受可选的 `custom_language_model_id` 查询参数，用于指定要在训练期间使用的单独创建的定制语言模型。可以使用定制语言模型进行训练，该模型包含音频文件的转录，或者包含与音频文件内容相关的语料库或 OOV 词。这两种定制模型都必须基于同一基本模型的相同版本，才能成功进行训练。有关更多信息，请参阅[使用定制语言模型训练定制声学模型](/docs/services/speech-to-text/acoustic-both.html#useBothTrain)。


训练方法是异步执行的。根据定制声学模型包含的音频数据量以及服务上的当前负载，训练可能需要几分钟或几小时才能完成。一般准则是，训练定制声学模型需要的时间大约是其音频数据长度的两到四倍。时间范围取决于所训练的模型以及音频的性质，如音频是干净还是有噪声。例如，训练包含 2 小时音频的模型可能需要 4 到 8 小时。有关检查训练操作的状态的更多信息，请参阅[监视训练模型请求](#monitorTraining-acoustic)。

### 监视训练模型请求
{: #monitorTraining-acoustic}

如果训练过程成功启动，服务会返回 200 响应代码。在现有请求完成之前，服务无法接受后续训练请求，也无法接受添加更多音频资源的请求。

要确定训练请求的状态，请使用 `GET /v1/acoustic_customizations/{customization_id}` 方法来轮询模型的状态。此方法接受声学模型的定制标识并返回其状态，如以下示例所示：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

响应包含报告模型当前状态的 `status` 和 `progress` 字段。`progress` 字段的含义取决于模型的状态。`status` 字段可以具有下列其中一个值：

-   `pending` 指示模型已创建，但正在等待添加训练数据或等待服务完成对已添加数据的分析。`progress` 字段为 `0`。
-   `ready` 指示模型已准备好进行训练。`progress` 字段为 `0`。
-   `training` 指示正在训练模型。训练完成时，`progress` 字段会从 `0` 更改为 `100`。<!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` 指示模型已训练并已准备好供使用。`progress` 字段为 `100`。
-   `upgrading` 指示正在升级模型。`progress` 字段为 `0`。
-   `failed` 指示模型训练失败。`progress` 字段为 `0`。

使用循环每分钟检查一次训练的状态，直到模型变为 `available`。有关此方法返回的其他字段的更多信息，请参阅[列出定制声学模型](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic)。

### 训练失败
{: #failedTraining-acoustic}

如果服务正在处理定制模型的其他请求，那么定制声学模型的训练无法启动。冲突请求可能是其他训练请求，也可能是向模型添加音频资源的请求。训练还可能由于以下原因而无法启动：

-   定制模型包含的音频数据不到 10 分钟。
-   定制模型包含的音频数据超过 200 小时。
-   定制模型的一个或多个音频资源无效。
-   通过 `custom_language_model_id` 查询参数传递了不兼容的定制语言模型。这两种定制模型都必须基于同一基本模型的相同版本。

如果定制模型的训练状态为 `failed`，请使用 `GET /v1/acoustic_customizations/{customization_id}/audio` 和 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法来检查模型的音频资源，并解决发现的任何问题。有关更多信息，请参阅[列出定制声学模型的音频资源](/docs/services/speech-to-text/acoustic-audio.html#listAudio)。
