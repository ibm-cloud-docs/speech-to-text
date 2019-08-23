---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# 创建定制语言模型
{: #languageCreate}

执行以下步骤为 {{site.data.keyword.speechtotextshort}} 服务创建定制语言模型：
{: shortdesc}

1.  [创建定制语言模型](#createModel-language)。可以针对相同或不同的领域创建多个定制模型。对于创建的任何模型，此过程都是相同的。但是，在识别请求中一次只能指定一个定制模型。
1.  [向定制语言模型添加语料库](#addCorpus)。语料库是一个纯文本文档，在上下文中使用某个领域的术语。服务通过从语料库中抽取其基本词汇表中不存在的术语，为定制模型构建词汇表。可以向一个定制模型添加多个语料库。
1.  [向定制语言模型添加词](#addWords)。您还可以单独向模型添加定制词。此外，还可以使用相同的方法来修改从语料库中抽取的定制词。通过这些方法，可以指定词的读法以及词在语音文字记录中的显示方式。
1.  [训练定制语言模型](#trainModel-language)。将词添加到定制模型后，必须基于这些词对该模型进行训练。通过训练，可准备定制模型以用于语音识别。在训练模型之前，模型不会使用新词或修改的词。
1.  训练定制模型后，可以将其用于识别请求。如果传递用于转录的音频包含定制模型中定义的特定于领域的词，那么请求的结果会反映出模型的增强词汇表。有关更多信息，请参阅[使用定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)。

创建定制语言模型的步骤是迭代的。您可以根据需要，随时添加语料库，添加词，以及训练或重新训练模型。

您还可以向定制语言模型添加语法。语法将服务的响应限制为仅语法识别到的那些词。有关更多信息，请参阅[将语法用于定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-grammars)。

语言模型定制可用于大多数语言。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

## 创建定制语言模型
{: #createModel-language}

使用 `POST /v1/customizations` 方法来创建新的定制语言模型。您可以创建任意数量的定制语言模型，但在语音识别请求中一次只能使用一个模型。此方法接受用于定义新定制模型属性的 JSON 对象作为请求主体。

<table>
  <caption>表 1. 新定制语言模型的属性</caption>
  <tr>
    <th style="width:20%; text-align:left">参数</th>
    <th style="width:12%; text-align:center">数据类型</th>
    <th style="text-align:left">描述</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>      必需
    </em></td>
    <td style="text-align:center">字符串</td>
    <td>
      新定制模型的用户定义名称。请使用描述定制模型领域的名称，例如<code>医疗定制模型</code>或<code>法律定制模型</code>。使用的名称应在您拥有的所有定制语言模型中唯一。请使用与定制模型的语言相匹配的本地化名称。
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>      必需
    </em></td>
    <td style="text-align:center">字符串</td>
    <td>
      要由新定制模型定制的语言模型的名称。指定由 <code>GET /v1/models</code> 方法返回的其中一个支持的语言模型。新模型只能用于它所定制的基本模型。</td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td>
      指定语言中要用于新定制模型的方言。对于大多数语言，缺省情况下，方言与基本模型的语言相匹配。例如，`en-US` 用于任一美国英语语言模型。<br><br>
      对于西班牙语语言，服务会创建适合下列其中一种方言的语音的定制模型：
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          `es-ES`，用于卡斯蒂利亚西班牙语（`es-ES` 模型）
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-LA`，用于拉丁美洲西班牙语（`es-AR`、`es-CL`、`es-CO` 和 `es-PE` 模型）
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-US` 用于墨西哥（北美）西班牙语（`es-MX` 模型）
        </li>
      </ul>
      此参数仅对西班牙语模型有意义，您始终可以安全地省略此参数，以使服务创建正确的映射。
        <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          如果对非西班牙语语言模型指定 `dialect` 参数，那么其值必须与基本模型的语言相匹配。
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          如果为西班牙语语言模型指定 `dialect` ，那么其值必须与所示的其中一个已定义映射（`es-ES`、`es-LA` 或 `es-MX`）相匹配。
        </li>
      </ul>
      所有 dialect 值都不区分大小写。
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>      可选
    </em></td>
    <td style="text-align:center">字符串</td>
    <td>
      新模型的描述。请使用与定制模型的语言相匹配的本地化描述。
    </td>
  </tr>
</table>

以下示例创建名为 `Example model` 的新定制语言模型。此模型创建用于基本模型 `en-US_BroadbandModel`，并且描述为 `Example custom language model`。`Content-Type` 头指定要将 JSON 数据传递到此方法。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

该示例将返回新模型的定制标识。每个定制模型都通过唯一的定制标识进行标识，这是全局唯一标识 (GUID)。使用与模型关联的调用的 `customization_id` 参数可指定定制模型的 GUID。

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

拥有新定制模型的服务实例是其凭证用于创建该模型的实例。有关更多信息，请参阅[定制模型的所有权](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner)。

## 向定制语言模型添加语料库
{: #addCorpus}

创建定制语言模型后，下一步是向模型添加数据（特定于领域的词）。使用新词填充定制模型的建议方法是添加一个或多个语料库。

语料库是一个纯文本文件，理想情况下包含您领域中的样本句子。服务会解析语料库文件的内容，并抽取其基本词汇表中不包含的任何词。此类词称为未登录 (OOV) 词。

-   有关使用语料库的更多信息，请参阅[使用语料库](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingCorpora)。
-   有关服务如何向模型添加语料库的更多信息，请参阅[添加语料库文件时的处理过程](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)。

通过提供包含新词的句子，语料库允许服务在上下文中学习词。然后，可以单独扩充或修改模型的词。相对于基于从语料库添加的词来训练模型，仅基于单个词对模型进行训练会耗时更长，并且可能会生成有效性更低的结果。
{: tip}

使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法来向定制模型添加语料库：

-   使用 `customization_id` 路径参数指定定制模型的定制标识。
-   使用 `corpus_name` 路径参数指定语料库的名称。请使用与定制模型的语言相匹配并反映语料库内容的本地化名称。
    -   名称中最多可包含 128 个字符。
    -   不要使用需要进行 URL 编码的字符。例如，不要在名称中使用空格、斜杠、反斜杠、冒号、& 符号、双引号、加号、等号和问号等。（服务不会阻止使用这些字符。但是，由于这些字符在使用的任何位置都必须进行 URL 编码，因此强烈建议不要使用。）
    -   不要使用已添加到定制模型的语料库或语法的名称。
    -   不要使用名称 `user`，此名称由服务保留用于表示用户添加或修改的定制词。
    -   不要使用名称 `base_lm` 或 `default_lm`。这两个名称都保留供服务未来使用。
-   将语料库文本文件作为请求主体传递。

最多可以从所有源中添加 9 万个 OOV 词，总共可添加 1000 万个词。这包括语料库和语法中的词，以及您直接添加的词。有关更多信息，请参阅[我需要多少数据？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)
{: note}

以下示例将语料库文本文件 `healthcare.txt` 添加到具有指定标识的定制模型。示例将语料库命名为 `healthcare`。

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

此方法还接受可选的 `allow_overwrite` 查询参数，用于覆盖定制模型的现有语料库。如果在将语料库文件添加到模型后需要更新该文件，请使用此参数。

这是异步方法。可能需要大约一两分钟时间才能完成。操作的持续时间取决于语料库中的总词数、服务在语料库中找到的新词数以及服务上的当前负载。有关检查语料库的状态的更多信息，请参阅[监视添加语料库请求](#monitorCorpus)。

通过对每个语料库文本文件调用此方法一次，可以将任意数量的语料库添加到定制模型。一个语料库的添加操作必须完全完成后，才能添加其他语料库。向定制模型添加语料库后，请检查新的定制词以确定是否有拼写错误和其他错误。有关更多信息，请参阅[验证词资源](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)。

### 监视添加语料库请求
{: #monitorCorpus}

如果语料库有效，服务会返回 201 响应代码。然后，服务会异步处理语料库的内容，并自动抽取新词。在服务对当前请求的语料库的分析完成之前，无法提交向定制模型添加数据或训练模型的请求。

要确定分析的状态，请使用 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法来轮询语料库的状态。此方法接受模型的标识和语料库的名称，如以下示例中所示：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

响应包含该语料库的状态：

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

`status` 字段具有下列其中一个值：

-   `analyzed` 指示服务已成功分析语料库。
-   `being_processed` 指示服务仍在分析语料库。
-   `undetermined` 指示服务在处理语料库时遇到错误。

使用循环每 10 秒检查一次语料库的状态，直到它变为 `analyzed`。有关检查模型的语料库状态的更多信息，请参阅[列出定制语言模型的语料库](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)。

## 向定制语言模型添加词
{: #addWords}

虽然向定制语言模型添加词的建议方法是添加语料库，但也可以将单个定制词直接添加到模型中。服务向定制模型添加定制词的方式，就像添加从语料库中发现的 OOV 词一样。

如果您只有一个或几个词要添加到模型中，使用语料库来添加词可能并不实用，甚至并不可行。最简单的方法是添加仅包含其拼写的词。但是，您也可以为词提供多种读法，并指示词的显示方式。

-   有关直接添加词的更多信息，请参阅[使用定制词](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingWords)。
-   有关服务如何向模型添加定制词的更多信息，请参阅[添加或修改定制词时的处理过程](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseWord)。

可以使用以下方法向定制模型添加词：

-   `POST /v1/customizations/{customization_id}/words` 方法可一次添加多个词。您可传递一个 JSON 对象，用于通过请求主体提供有关每个词的信息。以下示例将两个定制词 `HHonors` 和 `IEEE` 添加到具有指定标识的定制模型。`Content-Type` 头指定要将 JSON 数据传递到此方法。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    您还可以通过文件来添加词。例如，`words.json` 文件定义了相同的两个定制词。

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    以下命令添加该文件中的词：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    这是异步方法。完成所需的时间取决于添加的词数和服务上的当前负载。有关检查该操作的状态的更多信息，请参阅[监视添加词请求](#monitorWords)。
-   `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法可添加单个词。您可传递一个 JSON 对象，用于提供有关词的信息。以下示例将词 `NCAA` 添加到具有指定标识的模型。同样，`Content-Type` 头指示要将 JSON 数据传递到此方法。

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    这是同步方法。服务会返回响应代码，以立即报告请求是成功还是失败。

与添加语料库一样，添加词后，请检查新的定制词以确定是否有拼写错误和其他错误。一次添加多个词时，此检查特别重要。有关更多信息，请参阅[验证词资源](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)。

### 监视添加词请求
{: #monitorWords}

使用 `POST /v1/customizations/{customization_id}/words` 方法时，如果输入数据有效，服务会返回 201 响应代码。然后，服务会异步处理这些词以将其添加到模型中。此操作通常比添加语料库或训练模型更快，但其持续时间取决于添加的新词数和服务上的当前负载。在服务完成添加新词的请求之前，无法提交向定制模型添加数据或训练模型的请求。

要确定请求的状态，请使用 `GET /v1/customizations/{customization_id}` 方法来轮询模型的状态。此方法接受模型的定制标识并返回包含模型状态的信息，如以下示例中所示：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

`status` 字段报告模型的当前状态。服务处理新词期间，状态会保持为 `pending`。使用循环每 10 秒检查一次状态，直到它变为 `ready`，这指示操作完成。有关可能的 `status` 值的更多信息，请参阅[监视训练模型请求](#monitorTraining-language)。

### 修改定制模型中的词
{: #modifyWord}

您还可以使用 `POST /v1/customizations/{customization_id}/words` 和 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法修改或扩充定制模型中的词。您可能需要使用这两个方法来更正将词添加到模型时所犯的拼写错误或其他错误。您可能还需要为现有词添加发音相似的读法定义。

使用这两个方法可修改现有词的定义，具体做法与添加词的完全一样。为词提供的新数据将覆盖该词的现有定义。

## 训练定制语言模型
{: #trainModel-language}

使用新词填充定制语言模型（通过添加语料库、添加语法或直接添加词）后，必须基于新数据来训练模型。通过训练，可准备定制模型以将这些数据用于语音识别。对于通过任何方法添加的词，在基于这些数据训练模型之前，模型不会使用这些词。

使用 `POST /v1/customizations/{customization_id}/train` 方法来训练定制模型。您将向此方法传递要训练的模型的定制标识，如以下示例中所示：

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

这是异步方法。根据训练模型所基于的新词数和服务上的当前负载，训练可能大约需要几分钟才能完成。有关检查训练操作的状态的更多信息，请参阅[监视训练模型请求](#monitorTraining-language)。

此方法包含以下可选的查询参数：

-   `word_type_to_add` 参数指定训练定制模型所要基于的词：
    -   指定 `all` 或省略此参数将基于模型的所有词（不管其来源是什么）来训练模型。
    -   指定 `user` 将仅基于用户添加或修改的词来训练模型，而忽略仅从语料库或语法中抽取的词。

如果添加的语料库具有噪声数据（例如，包含拼写错误的词），那么此选项非常有用。在基于此类数据训练模型之前，可以使用 `GET /v1/customizations/{customization_id}/words` 方法的 `word_type` 查询参数来复查从语料库和语法中抽取的词。有关更多信息，请参阅[列出定制语言模型中的词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。
-   `customization_weight` 参数指定在将定制模型用于语音识别时，给予定制模型中词相对于基本词汇表中词的权重。您还可以在使用定制模型的任何识别请求中指定定制权重。有关更多信息，请参阅[使用定制权重](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight)。
-   `strict` 参数指示在定制模型包含有效和无效资源的组合（语料库、语法和词）时，训练是否继续进行。缺省情况下，如果模型包含一个或多个无效资源，训练会失败。将此参数设置为 `false`，以允许只要模型包含至少一个有效资源，就继续进行训练。服务会从训练中排除无效的资源。有关更多信息，请参阅[训练失败](#failedTraining-language)。

### 监视训练模型请求
{: #monitorTraining-language}

如果服务成功启动训练过程，那么服务会返回 200 响应代码。在现有训练请求完成之前，服务无法接受后续训练请求，也无法接受添加新的语料库、语法或词的请求。

要确定训练请求的状态，请使用 `GET /v1/customizations/{customization_id}` 方法来轮询模型的状态。此方法接受模型的定制标识，并返回有关模型的信息。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
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

响应包含用于报告定制模型状态的 `status` 和 `progress` 字段。`progress` 字段的含义取决于模型的状态。`status` 字段可以具有下列其中一个值：

-   `pending` 指示模型已创建，但正在等待添加有效训练数据或等待服务完成对已添加的数据的分析。`progress` 字段为 `0`。
-   `ready` 指示模型包含有效数据，已准备好进行训练。`progress` 字段为 `0`。

    如果模型包含有效和无效资源的组合（例如，有效和无效的定制词），那么除非您将 `strict` 查询参数设置为 `false`，否则模型训练会失败。有关更多信息，请参阅[训练失败](#failedTraining-language)。
-   `training` 指示正在训练模型。训练完成时，`progress` 字段会从 `0` 更改为 `100`。<!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` 指示模型已训练并已准备好供使用。`progress` 字段为 `100`。
-   `upgrading` 指示正在升级模型。`progress` 字段为 `0`。
-   `failed` 指示模型训练失败。`progress` 字段为 `0`。有关更多信息，请参阅[训练失败](#failedTraining-language)。

使用循环每 10 秒检查一次状态，直到它变为 `available`。有关检查定制模型的状态的更多信息，请参阅[列出定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)。

### 训练失败
{: #failedTraining-language}

如果服务正在处理定制语言模型的其他请求，那么训练无法启动。例如，服务正在执行以下操作时，训练请求无法启动，状态码为 409：

-   处理语料库或语法以生成 OOV 词的列表
-   处理定制词以验证或自动生成发音相似的读法
-   处理其他训练请求

如果定制模型存在以下情况，训练也无法启动，状态码为 400：

-   自定制模型创建或上次训练以来，未包含任何新的有效训练数据（语料库、语法或词）
-   包含一个或多个无效的语料库、语法或词（例如，定制词具有无效的发音相似的读法）

如果训练请求失败且状态码为 400，那么服务会将定制模型的状态设置为 `failed`。请执行下列其中一项操作：

-   使用定制接口的方法来检查模型的资源，并修正发现的任何错误：
    -   对于无效的语料库，可以更正语料库文本文件，然后使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法的 `allow_overwrite` 参数将更正后的文件添加到模型。有关更多信息，请参阅[向定制语言模型添加语料库](#addCorpus)。
    -   对于无效的语法，可以更正语法文件，然后使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法的 `allow_overwrite` 参数将更正后的文件添加到模型。有关更多信息，请参阅[向定制语言模型添加语法](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)。
    -   对于无效的定制词，可以使用 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法直接在模型的词资源中修改该词。有关更多信息，请参阅[修改定制模型中的词](#modifyWord)。

    有关验证定制语言模型中词的更多信息，请参阅[验证词资源](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)。
-   将 `POST /v1/customizations/{customization_id}/train` 方法的 `strict` 参数设置为 `false`，以从训练中排除无效的资源。模型必须包含至少一个有效资源（语料库、语法或词），才能成功进行训练。`strict` 参数对于训练包含有效和无效资源组合的定制模型非常有用。

## 示例脚本
{: #exampleScripts}

可以使用以下脚本来试验创建定制语言模型的步骤：

-   名为 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 的 Python 脚本。有关更多信息，请参阅[示例 Python 脚本](#pythonScript)。
-   名为 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 的 Bash shell 脚本。有关更多信息，请参阅[示例 shell 脚本](#shellScript)。

这两个脚本提供的功能完全相同。每个脚本都可创建定制模型，通过语料库文本文件添加词，以及将一个和多个词直接添加到模型中。脚本可查询模型，以列出通过语料库添加的词以及直接由用户添加的词。脚本还可训练模型，并演示建议用于监视异步操作结果的轮询。

可以将提供的两个语料库文本文件中的任一个文件用于脚本，也可以使用您自己的语料库文件进行测试：

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 是一个缩略的 6 KB 语料库，用于向模型添加 6 个与医疗保健相关的术语。此文件用于脚本时，会生成少量输出。缺省情况下，脚本会使用此文件。
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 是一个更丰富的 164 KB 语料库，用于向模型添加许多与医疗保健相关的术语。此文件用于脚本时，生成的输出会多得多。

缺省情况下，使用任一脚本创建的新定制模型都可用于识别请求。脚本包含用于删除新定制模型的可选步骤，这在您试用该过程时非常有用。请遵循脚本中的注释来启用删除步骤。

### 示例 Python 脚本
{: #pythonScript}

执行以下步骤来使用 Python 脚本：

1.  下载名为 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 的 Python 脚本。
1.  下载要用于脚本的示例语料库文本文件。您可以自由使用任一语料库文本文件进行测试，也可以使用您自己选择的文件进行测试。缺省情况下，所有语料库文本文件都必须位于脚本所在的目录中。
1.  脚本使用 Python `requests` 库向服务发出 HTTP 请求。使用 `pip` 或 `easy_install` 来安装该库以供脚本使用，例如：

    ```bash
    pip install requests
    ```
    {: pre}

    有关该库的更多信息，请参阅 [pypi.python.org/pypi/requests](https://pypi.python.org/pypi/requests){: external}。
1.  编辑脚本，将 `password` 字符串 `iam_apikey` 替换为 {{site.data.keyword.speechtotextshort}} 凭证中的 API 密钥：

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  通过输入以下命令来运行脚本：

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

脚本使用达拉斯位置的以下缺省 URL：`https://stream.watsonplatform.net`。如果在其他位置创建了服务实例，请修改 `uri` 变量以使用您的位置。例如，如果服务实例位于华盛顿位置，请使用 `https://gateway-wdc.watsonplatform.net`。
{: note}

### 示例 shell 脚本
{: #shellScript}

执行以下步骤来使用 Bash shell 脚本：

1.  下载名为 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a> 的 shell 脚本。
1.  下载要用于脚本的示例语料库文本文件。您可以自由使用任一语料库文本文件进行测试，也可以使用您自己选择的文件进行测试。缺省情况下，所有语料库文本文件都必须位于脚本所在的目录中。
1.  脚本使用 `curl` 命令向服务发出 HTTP 请求。如果尚未下载 `curl`，那么可以从 [curl.haxx.se](http://curl.haxx.se){: external} 安装适用于您操作系统的版本。安装支持安全套接字层 (SSL) 协议的版本，并确保在 `PATH` 环境变量中包含已安装的二进制文件。
1.  编辑脚本，将 `PASSWORD` 字符串 `iam_apikey` 替换为 {{site.data.keyword.speechtotextshort}} 凭证中的 API 密钥：

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  编辑脚本，将 `URL` 字符串替换为在其中创建服务实例的位置的 URL。脚本使用达拉斯位置的以下缺省 URL：

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  确保脚本具有可执行许可权，然后输入以下命令来运行该脚本：

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
