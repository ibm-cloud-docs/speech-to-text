---

copyright:
  years: 2019
lastupdated: "2019-03-10"

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

# 将定制声学模型和定制语言模型一起使用
{: #useBoth}

您可以使用补充的定制语言模型和定制声学模型来提高语音识别准确性。在训练声学模型期间和语音识别期间，可以同时使用这两种类型的模型。这两种模型必须由同一服务实例拥有，并且必须基于同一基本语言模型。
{: shortdesc}

单独使用定制声学模型或将该模型与对其进行训练所基于的定制语言模型一起使用仍会很有用。如果基于与被转录的音频相匹配的声学特征训练了定制声学模型，那么该模型仍可以提高转录质量。

## 使用定制语言模型训练定制声学模型
{: #useBothTrain}

仅使用音频数据来训练定制声学模型称为*无监督训练*。在训练期间使用定制语言模型称为*轻度监督训练*。轻度监督训练可以改进定制声学模型的有效性。

在以下情况下，使用轻度监督训练通过定制语言模型来训练定制声学模型：

-   定制语言模型基于已添加到定制声学模型的音频文件中的转录（逐字文本）。

    由于转录包含音频的确切内容，因此使用基于转录的定制语言模型进行训练可以生成最佳结果。服务可以在上下文中解析转录的内容，并抽取 OOV 词和 n 元语法，以帮助最有效地利用音频。如果音频数据的长度短于一小时，这种方法尤其有效。

    转录音频数据并不是绝对必要的。但是，如果有音频转录，这些转录可以提高定制声学模型的质量。如果音频包含许多 OOV 词，那么转录会特别有用。
-   定制语言模型基于语料库（文本文件）或与音频文件内容相关的词的列表。

    如果音频包含在服务基本词汇表中找不到的特定于域的词，那么在转录期间，仅执行声学模型定制并不会生成这些词。语言模型定制是扩展服务基本词汇表的唯一方法。如果您没有转录，请使用包含音频数据所在域中的 OOV 词的定制语言模型进行训练。实践证明，即使使用包含音频中所用 OOV 词列表的定制语言模型进行训练也很有帮助。

    例如，假设要创建基于特定产品呼叫中心音频的定制声学模型。您可以使用基于相关呼叫的转录的定制语言模型来训练定制声学模型，或使用包含呼叫中心所处理的特定产品名称的定制语言模型来训练定制声学模型。

要使用转录或词列表，可首先创建包含此文本数据的定制语言模型。要使用定制语言模型来训练定制声学模型，这两种定制模型必须基于同一基本模型的相同版本。如果基本模型的新版本可用，那么必须将这两种模型升级到基本模型的相同版本，才能成功进行训练。

使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法的可选 `custom_language_model_id` 查询参数通过定制语言模型来训练定制声学模型。使用 `customization_id` 参数和 `custom_language_model_id` 参数分别传递声学模型的 GUID 和定制语言模型的 GUID。这两种模型必须由随请求一起传递的服务凭证所拥有。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## 在语音识别期间使用定制语言模型和定制声学模型
{: #useBothRecognize}

可以在任何识别请求中同时指定定制语言模型和定制声学模型。如果定制语言模型包含要识别的音频域中的 OOV 词，那么可以在语音识别期间将其用于定制声学模型，以提高转录准确性。

使用定制语言模型可以提高转录准确性，不管是否使用定制语言模型训练了定制声学模型：

-   在训练期间同时使用定制语言和定制声学模型可提高定制声学模型的质量。
-   在语音识别期间同时使用这两种类型的模型可提高转录质量。

如果定制语言模型包含语法，那么还可以在语音识别期间将定制语言模型以及其中一个语法用于定制声学模型。

以下示例将这两种类型的模型传递到 HTTP `POST /v1/recognize` 方法。使用 `acoustic_customization_id` 参数和 `language_customization_id` 参数分别传递定制声学模型的 GUID 和定制语言模型的 GUID。这两种模型必须由随请求一起传递的服务凭证所拥有，并且必须基于同一基本模型（例如，`en-US_BroadbandModel`）。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

对于异步 HTTP 请求，可在创建异步作业时指定这些参数。对于 WebSocket，可在建立连接时传递这些参数。
