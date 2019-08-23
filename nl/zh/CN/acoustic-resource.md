---

copyright:
  years: 2017, 2019
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

# 使用音频资源
{: #audioResources}

您可以将单个音频文件或包含多个音频文件的归档文件添加到定制声学模型。建议通过添加归档文件来添加音频资源。创建和添加单个归档文件要比逐个添加多个音频文件高效得多。
您还可以提交请求以同时添加多个不同的音频资源。
{: shortdesc}

## 添加音频资源
{: #addAudioResource}

使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法来向定制声学模型添加任一类型的音频资源。可将音频资源作为请求主体传递，并包含以下参数：

-   `customization_id` 路径参数，用于指定模型的定制标识。
-   `audio_name` 路径参数，用于指定音频资源的名称。
    -   使用与定制模型的语言相匹配并反映资源内容的本地化名称。
    -   名称中最多可包含 128 个字符。
    -   不要使用需要进行 URL 编码的字符。例如，不要在名称中使用空格、斜杠、反斜杠、冒号、& 符号、双引号、加号、等号和问号等。（服务不会阻止使用这些字符。但是，由于这些字符在使用的任何位置都必须进行 URL 编码，因此强烈建议不要使用。）
    -   不要使用已添加到定制模型的音频资源的名称。

更新模型的音频资源时，必须训练模型以使更改在转录期间生效。有关更多信息，请参阅[训练定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)。

## 添加音频文件
{: #addAudioType}

要将单个音频文件添加到定制声学模型，请使用 `Content-Type` 头指定音频的格式（MIME 类型）。可以添加支持用于识别请求的任何格式的音频。根据需要，在格式规范中包含 `rate`、`channels` 和 `endianness` 参数。有关受支持音频格式的更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。

音频资源不支持音频格式的 `application/octet-stream` 规范。
{: note}

[向定制声学模型添加音频](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)中的以下示例添加了 `audio/wav` 文件：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## 添加归档文件
{: #addArchiveType}

向定制声学模型添加音频的首选方法是添加包含多个音频文件的归档文件。可以使用 `Content-Type` 请求头来指定归档类型，从而添加以下类型的归档文件：

-   指定 `application/zip` 可添加 **.zip** 文件
-   指定 `application/gzip` 可添加 **.tar.gz** 文件

根据要添加的文件的格式，可能还需要指定 `Contained-Content-Type` 头：

-   对于类型为 `audio/alaw`、`audio/basic`、`audio/l16` 或 `audio/mulaw` 的音频文件，必须使用 `Contained-Content-Type` 头来指定音频文件的格式。如果需要，请包含 `rate`、`channels` 和 `endianness` 参数。在这种情况下，归档文件中包含的所有音频文件都必须具有相同的音频格式。
-   对于所有类型的音频文件，都可以省略 `Contained-Content-Type` 头。在这种情况下，归档文件中包含的音频文件可以具有先前项目符号中未列出的任何格式。这些文件不必具有相同的格式。

添加音频类型资源时，不要使用 `Contained-Content-Type` 头。
{: note}

包含在归档类型资源中的音频文件的名称最多可以包含 128 个字符。这包括文件扩展名和名称的所有元素（例如，斜杠）。

[向定制声学模型添加音频](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)中的以下示例添加了 `application/zip` 文件，其中包含采样率为 16 千赫兹的 `audio/l16` 格式的音频文件：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## 添加音频资源的准则
{: #audioGuidelines}

使用定制声学模型时识别准确性的预期改进程度取决于多个因素。这些因素包括定制声学模型包含的音频数据量以及这些数据与所转录音频的类似程度。此改进还取决于定制声学模型是否使用相应的定制语言模型进行了训练。

将音频资源添加到定制声学模型时，请遵循以下准则：

-   将不少于 10 分钟但不超过 200 小时的音频添加到定制声学模型。音频必须包含语音，而不是静默。

    确定要添加的音频量时，音频的质量有着重要影响。模型的音频越能反映出要识别的音频的特征，用于语音识别的定制模型的质量越高。如果音频质量良好，那么添加更多的音频可提高转录准确性。但是，添加 5 到 10 小时的优质音频就可以产生积极的影响。
-   添加不超过 100 MB 的音频资源。所有音频类型和归档类型资源的最大大小限制均为 100 MB。

    要最大限度提高可通过单个资源添加的音频量，请考虑使用提供压缩的音频格式。有关更多信息，请参阅[数据限制和压缩](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits)。
-   将大型音频文件拆分为多个较小的文件。确保在词之间的静默位置拆分音频。

    由于可以同时提交多个请求来添加不同的音频资源，因此可以并行添加更小的文件。这种并行添加音频资源的方法可以加快服务的音频分析速度。
-   添加可反映出计划转录的音频的声道状况的音频内容。例如，如果应用程序处理的音频有来自行驶车辆的背景噪声，请使用相同类型的数据来构建定制模型。
-   确保音频文件的采样率与定制声学模型的基本模型的采样率相匹配：
    -   对于宽带模型，采样率必须至少为 16 千赫兹（每秒 16,000 个样本）。
    -   对于窄带模型，采样率必须至少为 8 千赫兹（每秒 8000 个样本）。

    如果音频采样率高于必需的最低采样率，服务会将音频的采样率降低到适当的速率。如果音频采样率低于必需的最低采样率，服务会将音频文件标注为 `invalid`。如果归档文件中包含的任何音频文件无效，服务会将整个归档视为无效。
-   在以下情况下，请创建定制语言模型以用于定制声学模型：
    -   如果音频长度不到一小时，请根据音频的转录来创建定制语言模型，以获得最佳结果。
    -   如果音频是特定于域的，并且包含在服务基本词汇表中找不到的特殊词，请使用语言模型定制来扩展服务的基本词汇表。在转录期间，仅执行声学模型定制不会生成这些词。

    有关更多信息，请参阅[将定制声学模型和定制语言模型一起使用](/docs/services/speech-to-text?topic=speech-to-text-useBoth)。
