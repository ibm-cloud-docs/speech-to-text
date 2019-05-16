---

copyright:
  years: 2017, 2019
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

# 升级定制模型
{: #customUpgrade}

为了提高语音识别的质量，{{site.data.keyword.speechtotextfull}} 服务会不定期更新基本模型。由于不同语言的基本模型相互独立（正如语言的宽带和窄带模型那样），因此对单个基本模型的更新不会影响其他模型。[发行说明](/docs/services/speech-to-text/release-notes.html)记录了所有基本模型更新。
{: shortdesc}

基本模型的新版本发布后，您必须升级基于基本模型构建的所有定制语言模型和定制声学模型，才能利用这些更新。在完成升级之前，定制模型会继续使用基本模型的旧版本。与所有定制操作一样，您必须使用拥有模型的服务实例的凭证，才能对该模型升级。

请尽快升级到已更新基本模型的最新版本。越早升级定制模型，就能越早体验新模型的更高性能。此外，未来可以除去基本模型的旧版本。为了鼓励升级，服务会返回一条警告消息，其中包含使用基于旧版基本模型的定制模型的识别请求的结果。

## 升级工作方式
{: #upgradeOverview}

首次发布新的基本模型时，现有定制模型仍基于基本模型的旧版本。在升级定制模型之前，对该定制模型执行的所有操作（例如，向模型添加数据或训练模型）影响的都是模型的现有版本。与此类似，指定定制模型的所有识别请求都会使用模型的现有版本。

升级会生成两个版本的定制模型，一个基于基本模型的旧版本，一个基于基本模型的最新版本。升级定制模型后，对该定制模型执行的所有操作影响的都是模型的已升级新版本。无法再向模型的旧版本添加数据或对其进行训练。此外，无法撤销升级操作。

升级任一类型的定制模型时，无需升级其单个资源。对于定制语言模型，服务会自动升级为模型定义的任何语料库、语法和词。同样，升级定制声学模型时，服务也会自动升级其音频资源。

缺省情况下，服务会将定制模型的最新版本用于识别请求。但是，您可以继续使用定制模型的旧版本进行语音识别。有关更多信息，请参阅[使用升级的定制模型发出识别请求](#upgradeRecognition)。

## 升级定制语言模型
{: #upgradeLanguage}

执行以下步骤来升级定制语言模型：

1.  确保定制语言模型处于 `ready` 或 `available` 状态。可以使用 `GET /v1/customizations/{customization_id}` 方法来检查模型的状态。如果模型的状态为 `ready`，请先升级模型，然后基于其最新数据对模型进行训练。

1.  使用 `POST /v1/customizations/{customization_id}/upgrade_model` 方法来升级定制语言模型：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    升级方法是异步执行的。根据模型的词资源中的字数和服务上的当前负载，升级可能大约需要几分钟才能完成。

如果升级过程成功启动，服务会返回 200 响应代码。可以使用 `GET /v1/customizations/{customization_id}` 方法来轮询模型的状态，从而监视升级的状态。使用循环每 10 秒检查一次状态。

定制模型升级期间，其状态为 `upgrading`。升级完成后，模型的状态会恢复为其升级之前的状态（`ready` 或 `available`）。检查升级操作的状态完全等同于检查训练操作的状态。有关更多信息，请参阅[监视训练模型请求](/docs/services/speech-to-text/language-create.html#monitorTraining-language)。

升级请求完成之前，服务无法接受以任何方式修改模型的请求。但是，在升级期间，可以继续使用模型的现有版本发出识别请求。

## 升级定制声学模型
{: #upgradeAcoustic}

执行以下步骤来升级定制声学模型。如果使用定制语言模型训练了定制声学模型，那么必须如文中所述执行两个额外的升级步骤。

1.  *如果使用定制语言模型训练了定制声学模型*，那么必须首先将定制语言模型升级到基本模型的最新版本。有关更多信息，请参阅[升级定制语言模型](#upgradeLanguage)。

1.  确保定制声学模型处于 `ready` 或 `available` 状态。可以使用 `GET /v1/acoustic_customizations/{customization_id}` 方法来检查模型的状态。如果模型的状态为 `ready`，请先升级模型，然后基于其最新数据对模型进行训练。

1.  使用 `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` 方法来升级定制声学模型：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    升级方法是异步执行的。根据模型包含的音频数据量以及服务上的当前负载，升级可能大约需要几分钟或几小时才能完成。与训练一样，升级需要的时间通常大约是模型音频数据长度的两倍。

1.  *如果使用定制语言模型训练了定制声学模型*，请再次升级定制声学模型，这次使用先前已升级的定制语言模型。使用 `custom_language_model_id` 查询参数来指定定制语言模型的定制标识。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    同样，升级方法是异步执行的，并且升级需要的时间通常大约是模型音频数据长度的两倍。

    使用语言模型升级声学模型的请求可能会失败，并返回 400 响应代码和消息：`自上次训练以来未修改过输入数据`。如果发生此错误，请将布尔值 `force` 查询参数添加到请求，并将此参数设置为 `true`。请将此参数仅用于在此特定情境中强制升级定制声学模型。
    {: note}

如果升级过程成功启动，服务会返回 200 响应代码。可以使用 `GET /v1/acoustic_customizations/{customization_id}` 方法来轮询模型的状态，从而监视升级的状态。使用循环每分钟检查一次状态。

定制模型升级期间，其状态为 `upgrading`。升级完成后，模型的状态会恢复为其升级之前的状态（`ready` 或 `available`）。检查升级操作的状态完全等同于检查训练操作的状态。有关更多信息，请参阅[监视训练模型请求](/docs/services/speech-to-text/acoustic-create.html#monitorTraining-acoustic)。

升级请求完成之前，服务无法接受以任何方式修改模型的请求。但是，在升级期间，可以继续使用模型的现有版本发出识别请求。

## 升级失败
{: #upgradeFailures}

如果服务正在处理定制模型的另一个请求（例如，训练请求或添加数据的请求），那么该模型的升级无法启动。在以下情况下，升级请求也无法启动：

-   定制模型的状态不是 `ready` 或 `available`。
-   定制模型不包含任何数据（定制词或音频资源）。
-   对于使用定制语言模型训练的定制声学模型，定制模型基于的是基本模型的不同版本。升级定制声学模型之前，必须先升级定制语言模型。

## 列出定制模型的版本信息
{: #upgradeList}

要查看定制模型可用于的基本模型的版本，请使用以下方法：

-   要列出有关定制语言模型的信息，请使用 `GET /v1/customizations/{customization_id}` 方法。有关更多信息，请参阅[列出定制语言模型](/docs/services/speech-to-text/language-models.html#listModels-language)。
-   要列出有关定制声学模型的信息，请使用 `GET /v1/acoustic_customizations/{customization_id}` 方法。有关更多信息，请参阅[列出定制声学模型](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic)。

在这两种情况下，输出都会包含 `versions` 字段，用于显示有关定制模型的基本模型的信息。以下输出显示了已升级定制语言模型的信息：

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

`versions` 字段指示定制模型可用于两个版本的基本模型：旧版本 `en-US_BroadbandModel.v07-06082016.06202016` 和新版本 `en-US_BroadbandModel.v2017-11-15`。如果定制模型未升级或仅存在其基本模型的单个版本，那么 `versions` 字段会显示单个版本。

## 使用升级的定制模型发出识别请求
{: #upgradeRecognition}

缺省情况下，服务会使用通过识别请求指定的定制模型最新版本。但是，即使在升级定制模型之后，也仍可以继续使用模型的旧版本发出识别请求。可以使用识别方法的 `base_model_version` 参数来指定要用于语音识别的基本模型版本。

例如，以下 HTTP 请求指定要使用基本模型的旧版本。因此，还会使用指定定制语言模型的旧版本。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

可以使用此功能来针对定制模型的基本模型的旧版本和新版本，测试定制模型的性能和准确性。如果发现某个已升级模型的性能在某个方面有欠缺（例如，无法再识别某些词），那么可以继续在识别请求中使用旧版本。

[基本模型版本](/docs/services/speech-to-text/input.html#version)描述了 `base_model_version` 参数以及服务如何确定基本模型和定制模型的哪些版本要用于识别请求。除了这些信息外，在通过识别请求同时传递定制语言模型和定制声学模型时，请考虑以下问题：

-   这两种定制模型必须基于同一基本模型（例如，`en-US_BroadbandModel`）。
-   如果这两种定制模型都基于旧版基本模型，那么服务将使用旧版基本模型进行识别。
-   如果这两种定制模型都基于新版基本模型，那么服务将使用新版基本模型进行识别。
-   如果这两种定制模型中只有一种升级到新版基本模型，那么服务将使用旧版基本模型进行识别。服务之所以会选择旧版基本模型，是因为这是两种定制模型的共有版本。
