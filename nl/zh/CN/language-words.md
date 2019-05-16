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

# 管理定制词
{: #manageWords}

定制接口包含 `POST /v1/customizations/{customization_id}/words` 和 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法，用于添加或修改定制模型的词。有关更多信息，请参阅[向定制语言模型添加词](/docs/services/speech-to-text/language-create.html#addWords)。该接口还包含以下方法，用于列出和删除定制语言模型的词。
{: shortdesc}

您很可能会通过语料库来添加大多数定制词。请确保您知道在语料库的文本文件中使用的字符编码。服务会保留在文本文件中找到的编码。在定制语言模型中使用各个词时，必须使用该编码。使用 `GET`、`PUT` 或 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法来指定词时，必须对在 URL 中传递的 `word_name` 进行 URL 编码（如果词包含非 ASCII 字符）。有关更多信息，请参阅[字符编码](/docs/services/speech-to-text/language-resource.html#charEncoding)。
{: important}

## 列出定制语言模型中的词
{: #listWords}

定制接口提供了两种方法，用于列出定制语言模型中的词：

-   `GET /v1/customizations/{customization_id}/words` 方法用于列出定制模型的词资源中词的相关信息。此方法包含两个可选的查询参数：
    -   `word_type` 参数指定要列出哪些词：

        -   `all`（缺省值）显示所有词。
        -   `user` 仅显示由用户添加或修改的定制词。
        -   `corpora` 仅显示从语料库中抽取的 OOV 词。
        -   `grammars` 仅显示从语法中抽取的 OOV 词。
    -   `sort` 参数指示如何对词排序。此参数接受两个自变量，用于指示如何对词排序：`alphabetical` 和 `count`。可以在自变量前面添加可选的 `+` 或 `-`，以指示结果是按升序还是降序排序。缺省情况下，此方法会按字母顺序升序显示词。
-   `GET /v1/customizations/{customization_id}/words/{word_name}` 方法用于列出模型的词资源中单个指定词的相关信息。

除了用于标识词的 `word` 字段外，这两种方法都会返回有关每个词的以下信息：

-   `sounds_like` 字段，用于提供词的读法数组。如果没有为词提供 sounds_like 值，那么此数组会包含服务自动生成的发音相似的读法。有关更多信息，请参阅[使用 sounds_like 字段](/docs/services/speech-to-text/language-resource.html#soundsLike)。

-   `display_as` 字段，用于显示服务在转录中显示的定制词的拼写。如果没有为词提供 display_as 值，那么此字段包含空字符串，在这种情况下，词会显示为其拼写形式。有关更多信息，请参阅[使用 display_as 字段](/docs/services/speech-to-text/language-resource.html#displayAs)。
-   `source` 字段，用于指示将词添加到定制模型的词资源的方式。此字段包含服务从中抽取词的每个语料库和语法的名称。如果是直接修改或添加的词，那么此字段包含字符串 `user`。
-   `count` 字段，用于指示在所有语料库和语法中找到该词的次数。例如，如果该词在一个语料库中出现了 5 次，在另一个语料库中出现了 7 次，那么其计数为 `12`。

    如果先将某个定制词添加到模型，然后该词由任何语料库或语法添加，那么计数会从 `1` 开始。如果该词先从语料库或语法添加，并在以后进行修改，那么计数仅反映在语料库和语法中找到该词的次数。

    对于在引入 `count` 字段之前创建的定制模型，此字段始终保持为 `0`。要针对此类模型更新此字段，请重新添加模型的语料库和语法，并在请求中包含 `allow_overwrite` 参数。
    {: note}

如果服务发现定制词的定义中有一个或多个问题，那么输出会包含 `error` 字段。此字段提供了用于列出定义中每个有问题元素的数组，并提供了用于描述问题的消息。

例如，如果添加了具有无效 `sounds_like` 字段的定制词，这违反了其中一个读法添加规则，那么会发生错误。如果定制模型的词资源包含带有错误的词，那么无法训练该模型。必须更正或删除该词后，才能训练该模型。

### 示例请求和响应
{: #listExample-words}

以下示例列出了具有指定定制标识的定制模型中的所有词，而不管其类型是什么。词按缺省排序顺序（字母顺序升序）显示。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

此模型的词资源包含四个词。第一个词是用户直接添加的，但其 `sounds_like` 字段包含错误：此字段不能包含数字。其他词是由用户添加的，或者是同时由用户添加并从语料库添加的。

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

以下示例显示了有关指定模型的词资源中词 `NCAA` 的信息：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

用户初始添加了该词。然后，服务在 `corpus3` 中找到该词两次。

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## 从定制语言模型中删除词
{: #deleteWord}

使用 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法可从定制语言模型中删除词。使用此方法可除去错误添加的词，例如，从包含错误数据的语料库添加的词。

可以除去通过任何方法添加到定制模型词资源的任何词。但是，无法从服务的基本词汇表中删除词。从定制模型中删除词只会删除该词的定制读法；该词仍会保留在基本词汇表中。

从定制模型中除去词不会影响该模型，直到使用 `POST /v1/customizations/{customization_id}/train` 方法来重新训练该模型为止。如果模型先前曾基于该词进行过训练，那么模型会继续将该词应用于语音识别，即使从模型的词资源中删除了该词后也是如此。您必须重新训练模型才能反映出此删除操作。

### 示例请求
{: #deleteExample-word}

以下示例从具有指定定制标识的定制模型中删除词 `IEEE`。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
