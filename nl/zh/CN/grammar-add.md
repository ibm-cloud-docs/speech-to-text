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

# 向定制语言模型添加语法
{: #grammarAdd}

必须先使用定制接口将语法添加到定制语言模型，然后才能将该语法用于语音识别。向定制语言模型添加语法的步骤类似于用于添加语料库或定制词的步骤：
{: shortdesc}

1.  [创建定制语言模型](#createModel-grammar)。可以创建新的定制模型，也可以使用现有模型。
1.  [向定制语言模型添加语法](#addGrammar)。服务会验证语法以确保其正确性。
1.  [验证根据语法确定的词](#validateGrammar)。您将验证语法识别到的任何未登录 (OOV) 词的发音相似的读法是否正确。
1.  [训练定制语言模型](#trainGrammar)。服务会准备定制模型和语法以用于语音识别。
1.  现在，可以在语音识别请求中使用定制模型和语法。有关更多信息，请参阅[将语法用于语音识别](/docs/services/speech-to-text/grammar-use.html)。

这些步骤是迭代的。您可以根据需要，随时向定制语言模型添加语法以及语料库和定制词。必须基于添加的任何新数据资源（语法、语料库或定制词）来训练定制模型。将定制模型用于语音识别时，该模型反映的是最后一次训练所基于的数据。

可以将语法用于支持语言模型定制的任何语言。语言模型定制可用于大多数语言。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text/custom.html#languageSupport)。
{: note}

## 创建定制语言模型
{: #createModel-grammar}

要将语法用于语音识别，必须将其添加到定制语言模型。可以使用现有定制语言模型，也可以使用 `POST /v1/customizations` 方法来创建新的定制模型。有关创建新定制模型的信息，请参阅[创建定制语言模型](/docs/services/speech-to-text/language-create.html#createModel-language)。

定制语言模型可以包含语料库和定制词以及语法。在语音识别期间，可以使用包含或不包含语法的定制模型。但是，使用语法时，服务仅根据指定的语法来识别词。

## 向定制语言模型添加语法
{: #addGrammar}

使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法来向定制模型添加语法文件。

-   使用 `customization_id` 路径参数指定定制语言模型的定制标识。
-   使用 `grammar_name` 路径参数指定语法的名称。请使用与定制模型的语言相匹配并反映语法内容的本地化名称。
    -   名称中最多可包含 128 个字符。
    -   不要在名称中包含空格、斜杠或反斜杠。
    -   不要使用已添加到定制模型的语法或语料库的名称。
    -   不要使用名称 `user`，此名称由服务保留用于表示用户添加或修改的定制词。

-   使用 `Content-Type` 请求头指定语法的格式：
    -   `application/srgs` 表示 ABNF 语法
    -   `application/srgs+xml` 表示 XML 语法

-   将包含语法的文件作为请求主体传递。

以下示例将名为 `confirm.abnf` 的语法文件添加到具有指定标识的定制模型。示例将语法命名为 `confirm-abnf`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

此方法还接受可选的 `allow_overwrite` 查询参数，可以包含此参数来覆盖同名的现有语法。如果在将语法添加到模型后需要更新语法，请使用此参数。

这是异步方法。根据语法的大小和服务上的当前负载，服务可能需要几秒钟来分析语法。有关检查语法状态的信息，请参阅[监视添加语法请求](#monitorGrammar)。

通过对每个语法文件调用此方法一次，可以将任意数量的语法添加到定制模型。一个语法的添加操作必须完全完成后，才能添加其他语法。

### 监视添加语法请求
{: #monitorGrammar}

如果语法有效，服务会返回 201 响应代码。然后，服务会异步处理语法的内容，并自动抽取语法可以识别到的新词。在服务对当前请求的语法的分析完成之前，无法提交向定制模型添加更多数据资源的请求。

要确定分析的状态，请使用 `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法来轮询语法的状态。此方法接受定制模型的标识和语法的名称。以下示例检查名为 `confirm-abnf` 的语法的状态。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

响应包含该语法的状态：

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

`status` 字段具有下列其中一个值：

-   `analyzed` 指示服务已成功分析语法。
-   `being_processed` 指示服务仍在分析语法。
-   `undetermined` 指示服务在处理语法时遇到错误。

使用循环每 10 秒检查一次语法的状态，直到它变为 `analyzed`。有关检查语法状态的更多信息，请参阅[列出定制语言模型的语法](/docs/services/speech-to-text/grammar-manage.html#listGrammars)。

### 处理添加语法失败
{: #handleFailures}

如果对语法的分析失败，那么服务会将语法的状态设置为 `undetermined`，并包含 `error` 字段，用于描述失败及语法状态。您还可以使用 `GET /v1/customizations/{customization_id}` 方法来检查定制模型的状态。如果添加语法失败，那么输出会包含类似以下内容的错误消息：

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

定制模型的状态不受此错误的影响，但语法不能用于该模型。通过更正语法文件并重复将其添加到定制模型的请求，可以解决此问题。将 `allow_overwrite` 参数设置为 `true` 可覆盖失败的语法文件版本。

您还可以从定制模型中暂时删除语法，然后更正语法文件，并在未来重新添加该语法文件。有关删除语法的更多信息，请参阅[从定制语言模型中删除语法](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar)。

## 验证根据语法确定的词
{: #validateGrammar}

将语法文件添加到定制语言模型时，服务会解析语法，以确定语法是否可识别到任何尚不属于服务基本词汇表的词。此类词称为未登录 (OOV) 词。服务会将 OOV 词添加到定制模型的词资源。词资源的用途是定义服务的词汇表中尚不存在的词。

词资源中的定义会指示服务如何转录 OOV 词。信息包含 `sounds_like` 字段（指示服务如何对词发音）、`display_as` 字段（指示服务如何显示词）和 `source` 字段（指示将词添加到定制模型的方式）。有关词资源和 OOV 词的更多信息，请参阅[词资源](/docs/services/speech-to-text/language-resource.html#wordsResource)。

将语法添加到定制模型后，检查模型词资源中的 OOV 词以验证其发音相似的读法是一种不错的做法。并非所有语法都包含 OOV 词，但验证词资源通常是很好的想法。要在添加语法后检查定制模型的词，请使用以下方法：

-   使用 `GET /v1/customizations/{customization_id}/words` 方法来列出定制模型中的所有词。使用此方法的 `word_type` 参数传递值 `grammars` 可仅列出通过语法添加的词。
-   使用 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法来查看模型中的单个词。

验证词的发音相似的读法是否准确且正确。另请在词本身中查找拼写和其他错误。有关验证和更正定制模型中词的更多信息，请参阅[验证词资源](/docs/services/speech-to-text/language-resource.html#validateModel)。

## 训练定制语言模型
{: #trainGrammar}

在可以将语法用于定制语言模型之前，要执行的最后一步是训练模型。使用的过程与用于基于新语料库或新定制词来训练定制模型的过程相同。通过训练模型，可对语法进行编译，以在语音识别期间使用。服务会将原始的基于文本的格式的语法处理为用于语音识别的二进制运行时格式。训练模型后，才能使用语法。

使用 `POST /v1/customizations/{customization_id}/train` 方法来训练定制模型。您将向此方法传递要训练的模型的定制标识，如以下示例中所示。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

训练方法是异步执行的。训练通常需要几秒钟才能完成。但也可能需要更长时间，具体取决于语法的大小和复杂性以及服务上的当前负载。有关检查训练操作的状态的信息，请参阅[监视训练请求](#monitorTraining-grammar)。

### 监视训练请求
{: #monitorTraining-grammar}

如果训练过程成功启动，服务会返回 200 响应代码。在当前训练请求完成之前，服务无法接受后续训练请求，也无法请求向定制模型添加新的语法、语料库或词。

要确定训练请求的状态，请使用 `GET /v1/customizations/{customization_id}` 方法来轮询模型的状态。此方法接受模型的定制标识，并返回类似以下内容的模型相关信息：

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

正在训练模型期间，`status` 字段的值为 `training`。使用循环每 10 秒检查一次模型的状态，直到它变为 `available`。有关监视训练请求和其他可能状态值的更多信息，请参阅[监视训练模型请求](/docs/services/speech-to-text/language-create.html#monitorTraining-language)。
