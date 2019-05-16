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

# 管理定制语言模型
{: #manageLanguageModels}

定制接口包含用于创建定制语言模型的 `POST /v1/customizations` 方法。该接口还包含 `POST /v1/customizations/train` 方法，用于基于定制模型词资源的最新数据来训练该模型。有关更多信息，请参阅以下文档：
{: shortdesc}

-   [创建定制语言模型](/docs/services/speech-to-text/language-create.html#createModel-language)
-   [训练定制语言模型](/docs/services/speech-to-text/language-create.html#trainModel-language)

此外，该接口还包含以下多个方法，用于列出有关定制语言模型的信息，将定制模型重置为其初始状态以及删除定制模型。

## 列出定制语言模型
{: #listModels-language}

定制接口提供了两种方法，用于列出有关指定服务凭证所拥有的定制语言模型的信息：

-   `GET /v1/customizations` 方法用于列出有关所有定制语言模型的信息，或有关指定语言的所有定制语言模型的信息。
-   `GET /v1/customizations/{customization_id}` 方法用于列出有关指定定制语言模型的信息。使用此方法可轮询服务以获取有关训练请求或新词添加请求的状态的信息。

这两种方法都会返回有关定制模型的以下信息：

-   `customization_id` 标识定制模型的全局唯一标识 (GUID)。GUID 用于在接口的各方法中标识模型。
-   `created` 是创建定制模型的日期和时间，采用全球标准时间 (UTC)。
-   `language` 是定制模型的语言。
-   `dialect` 是语言中用于定制语言模型的方言。
-   `owner` 标识拥有定制模型的服务实例的凭证。
-   `name` 是定制模型的名称。
-   `description` 显示定制模型的描述（如果在创建时提供）。
-   `base_model` 指示定制模型创建用于的语言模型的名称。
-   `versions` 提供定制模型的可用版本列表。数组中的每个元素指示定制模型可以用于的基本模型的一个版本。仅当升级过定制模型时，才会存在多个版本。否则，只会显示一个版本。有关更多信息，请参阅[列出定制模型的版本信息](/docs/services/speech-to-text/custom-upgrade.html#upgradeList)。

此方法还会返回 `status` 字段，用于指示定制模型的状态：

-   `pending` 指示模型已创建。模型正在等待添加训练数据或等待服务完成对已添加数据的分析。
-   `ready` 指示模型包含数据，已准备好进行训练。
-   `training` 指示正在基于数据对模型进行训练。
-   `available` 指示模型已训练并已准备好用于识别请求。
-   `upgrading` 指示正在升级模型。
-   `failed` 指示模型训练失败。请检查模型的词资源中的词，以确定阻止训练模型的错误。

此外，输出还包含 `progress` 字段，用于指示定制模型训练的当前进度。如果是使用 `POST /v1/customizations/{customization_id}/train` 方法启动的模型训练，那么此字段会将该请求的当前进度以完成百分比的形式指示。目前，如果状态为 `available`，那么此字段的值为 `100`；否则，值为 `0`。

### 示例请求和响应
{: #listExample-language}

以下示例包含 `language` 查询参数，用于列出服务凭证拥有的所有美国英语定制语言模型：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

服务凭证拥有两个此类模型。第一个模型正在等待数据，或者正在由服务进行处理。第二个模型已经过完全训练并准备好供使用。

```javascript
{
  "customizations": [
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
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

以下示例返回有关具有指定定制标识的定制模型的信息：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

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

## 重置定制语言模型
{: #resetModel-language}

使用 `POST /v1/customizations/{customization_id}/reset` 方法可重置定制模型。重置模型会从模型中除去所有语料库和词，并将模型初始化为创建时的状态。此方法不会删除模型本身或元数据，如模型名称和语言。但是，重置模型后，其词资源为空，因此必须通过添加语料库和词来重新创建模型词资源。

### 示例请求
{: #resetExample-language}

以下示例重置具有指定定制标识的定制模型：

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## 删除定制语言模型
{: #deleteModel-language}

使用 `DELETE /v1/customizations/{customization_id}` 方法可删除不再需要的定制语言模型。此方法会删除与定制模型关联的所有语料库和词以及模型本身。请谨慎使用此方法：删除定制模型后，无法取回该模型及其数据。

### 示例请求
{: #deleteExample-language}

以下示例删除具有指定定制标识的定制模型：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
