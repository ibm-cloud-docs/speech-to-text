---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# 管理语料库
{: #manageCorpora}

定制接口包含 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，用于向定制语言模型添加语料库。有关更多信息，请参阅[向定制语言模型添加语料库](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)。该接口还包含以下方法，用于列出和删除定制语言模型的语料库。
{: shortdesc}

## 列出定制语言模型的语料库
{: #listCorpora}

定制接口提供了两种方法，用于列出有关定制语言模型的语料库的信息：

-   `GET /v1/customizations/{customization_id}/corpora` 方法用于列出有关定制模型的所有语料库的信息。
-   `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法用于列出有关定制模型的指定语料库的信息。

这两个方法都会返回语料库的名称 (`name`)、从语料库中读取的总词数 (`total_words`) 以及从语料库中抽取的未登录词数 (`out-of-vocabulary_words`)。这两个方法还会列出语料库的状态 (`status`)。在检查服务对语料库的分析以响应将语料库添加到定制模型的请求时，该状态非常重要：

-   `analyzed` 指示服务已成功分析语料库。定制模型可以通过语料库中的数据进行训练。
-   `being_processed` 指示服务仍在分析语料库。在分析完成之前，服务无法接受添加新语料库或词或者训练定制模型的请求。
-   `undetermined` 指示服务在处理语料库时遇到错误。为语料库返回的信息包含错误消息，用于提供更正错误的指南。

    例如，语料库可能无效，或者您可能尝试添加的语料库与现有语料库同名。您可以重试添加该语料库，并在请求中包含 `allow_overwrite` 参数。您还可以删除该语料库，然后重试添加。

### 示例请求和响应
{: #listExample-corpora}

以下示例列出了具有指定定制标识的定制模型的所有语料库：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

已将三个语料库添加到定制模型。服务已成功分析 `corpus1`。服务仍在分析 `corpus2`，而对 `corpus3` 的分析失败。

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "语料库“corpus3.txt”分析失败。请通过将“allow_overwrite”标志设置为“true”来重试添加该语料库。"
    }
  ]
}
```
{: codeblock}

以下示例返回有关具有指定定制标识的定制模型的语料库 `corpus1` 的信息：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## 从定制语言模型中删除语料库
{: #deleteCorpus}

使用 `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法可从定制语言模型中除去现有语料库。删除语料库时，服务会从定制模型的词资源中除去与语料库关联的所有 OOV 词，但以下情况例外：

-   其他语料库或某个语法也添加了该词。
-   已使用 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法对该词进行了某种修改。

除去语料库并不会影响定制模型，直到使用 `POST /v1/customizations/{customization_id}/train` 方法，基于更新的数据对定制模型进行了训练。如果基于语料库成功训练了模型，那么语料库中的词将保留在模型的词汇表中，并应用于语音识别，直到您重新训练模型为止。

### 示例请求
{: #deleteExample-corpus}

以下示例从具有指定定制标识的定制模型中删除名为 `corpus3` 的语料库。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
