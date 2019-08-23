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

# 管理语法
{: #manageGrammars}

定制接口包含 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法，用于向定制语言模型添加语法。有关更多信息，请参阅[向定制语言模型添加语法](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)。该接口还包含以下方法，用于列出和删除定制语言模型的语法。
{: shortdesc}

## 列出定制语言模型的语法
{: #listGrammars}

定制接口提供了两种方法，用于列出有关定制语言模型的语法的信息：

-   `GET /v1/customizations/{customization_id}/grammars` 方法用于列出有关定制模型的所有语法的信息。
-   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法用于列出有关定制模型的指定语法的信息。

这两个方法所返回的语法相关信息是相同的。这些信息包括语法的名称 (`name`) 和语法所识别的未登录词 (`out_of_vocabulary_words`) 数。响应还包含语法的状态 (`status`)，该信息非常重要，在将语法添加到定制模型后，需要通过状态来检查服务对语法的分析：

-   `being_processed` 指示服务仍在处理语法，用于响应 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 请求。
-   `analyzed` 指示服务已成功处理语法，并将其添加到定制模型。
-   `undetermined` 表示服务在处理语法时遇到错误。所返回的语法相关信息中会包含一条错误消息，从此消息可了解如何更正错误。

    例如，语法可能无效，或者所尝试添加的语法可能与现有语法同名。您可以重新尝试添加语法，并在请求中包含 `allow_overwrite` 参数。还可以先删除语法，然后再重新尝试添加。

### 示例请求和响应
{: #listExample-grammars}

以下示例列出了有关已添加到具有指定定制标识的定制模型的所有语法的信息。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

已成功将三个语法添加到定制模型：`confirm-xml`、`confirm-abnf` 和 `list-abnf`。

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

以下示例显示了有关指定语法 `list-abnf` 的信息。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## 从定制语言模型中删除语法
{: #deleteGrammar}

使用 `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法，可以从定制语言模型中除去现有语法。删除语法时，服务会从定制模型的词资源中除去与语法关联的任何 OOV 词，但以下情况例外：

-   其他语法或语料库也添加了该词。
-   已使用 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法对该词进行了某种修改。

除去语法并不会影响定制模型，直到使用 `POST /v1/customizations/{customization_id}/train` 方法，基于更新的数据对定制模型进行了训练。如果基于语法成功训练了模型，那么语法会继续可用于语音识别，直到您重新训练模型为止。

### 示例请求
{: #deleteExample-grammar}

以下示例会从具有指定定制标识的定制模型中删除名为 `list-abnf` 的语法。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
