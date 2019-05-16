---

copyright:
  years: 2015, 2019
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

# 定制接口
{: #customization}

{{site.data.keyword.speechtotextfull}} 服务提供了定制接口，可用于扩充其语音识别功能。通过对您的域和音频的基本模型进行定制，可以使用定制来提高语音识别请求的准确性。定制仅可用于某些语言，并且对于不同的语言，存在不同的支持级别；请参阅[支持定制的语言](#languageSupport)。
{: shortdesc}

定制接口支持定制语言模型和定制声学模型。这两种类型的定制模型的接口比较类似，并且易于使用。将任一类型的定制模型用于识别请求也很简单：在请求中指定模型的定制标识即可。

无论是否使用定制模型，语音识别的工作方式都一样。将定制模型用于语音识别时，可以使用通常可用于识别请求的所有输入和输出参数。有关所有可用参数的更多信息，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。

## 语言模型定制
{: #customLanguage-intro}

服务是面向广泛的一般受众而开发的。服务的基本词汇表包含日常会话中使用的许多词。服务的模型为许多应用提供了足够准确的识别。但是，模型可能缺少与特定领域关联的特定词汇的知识。

*语言模型定制*接口可以提高医学、法律、信息技术等领域的语音识别准确性。通过使用语言模型定制，可以扩展和定制基本模型的词汇表以包含特定于领域的术语。

您可创建定制语言模型并添加特定于您的领域的语料库和词。基于增强的词汇表来训练定制语言模型后，可以将该模型用于定制语音识别。服务通常可在几分钟内训练好任何定制模型。创建模型所需的工作量取决于可用于模型的数据。

有关更多信息，请参阅：

-   [创建定制语言模型](/docs/services/speech-to-text/language-create.html)
-   [使用定制语言模型](/docs/services/speech-to-text/language-use.html)

## 声学模型定制
{: #customAcoustic-intro}

与此类似，服务是使用基本声学模型开发的，这些模型能有效地处理各种音频特征。但在类似下面的情况下，通过调整基本模型来适应音频，可以改进语音识别：

-   您的声道环境是独一无二的。例如，环境嘈杂，麦克风质量或定位不够理想，或者音频受到远场效应的影响。
-   说话者的语音模式非典型。例如，说话者语速异常快，或者音频包含随意会话。
-   说话者的口音很重。例如，音频包含以非母语或第二语言讲话的说话者。

*声学模型定制*接口可以调整基本模型，以适应您的环境和说话者。您可以创建定制声学模型，然后添加与要转录的音频的声学特征匹配度很高的音频数据（音频资源）。使用这些音频资源训练定制声学模型后，可以将该模型用于定制语音识别。

服务训练定制模型所需的时间长度取决于模型包含的音频数据量。通常，训练需要的时间是累计音频长度的两倍。创建模型所需的工作量取决于可用于模型的音频数据。此外，还取决于是否使用音频的转录。

有关更多信息，请参阅：

-   [创建定制声学模型](/docs/services/speech-to-text/acoustic-create.html)
-   [使用定制声学模型](/docs/services/speech-to-text/acoustic-use.html)

## 语法
{: #grammars-intro}

定制语言模型允许您扩展服务的基本词汇表。通过*语法*，可以限制服务可以在该词汇表中识别到的词。将语法与定制语言模型一起用于语音识别时，服务可以仅识别语法识别到的词、短语和字符串。由于语法为有效匹配项定义了有限搜索空间，因此服务可以更快、更准确地交付结果。

您可将语法添加到定制语言模型，然后训练模型，就像对语料库所执行的操作一样。但是，与语料库不同的是，必须显式指定在语音识别期间语法要用于定制模型。

有关更多信息，请参阅：

-   [将语法用于定制语言模型](/docs/services/speech-to-text/grammar.html)
-   [向定制语言模型添加语法](/docs/services/speech-to-text/grammar-add.html)
-   [将语法用于语音识别](/docs/services/speech-to-text/grammar-use.html)

## 将声学和语言定制一起使用
{: #combined}

仅使用定制声学模型就可提高服务的识别能力。但是，如果转录或相关语料库可用于示例音频，那么可以使用这些数据来进一步提高基于定制声学模型的语音识别质量。

通过创建用于对定制声学模型进行补充的定制语言模型，可以将这两种模型一起使用以增强语音识别功能。在训练定制声学模型时，可以指定定制语言模型，其中包括音频资源的转录或资源中特定于领域的词的词汇表。与此类似，转录音频时，服务可接受定制语言模型和/或定制声学模型。如果定制语言模型包含语法，那么可以将该模型和语法用于定制声学模型以进行语音识别。

有关更多信息，请参阅：

-   [将定制声学模型和定制语言模型一起使用](/docs/services/speech-to-text/acoustic-both.html)

某些语言不同时支持语言和声学定制。有关更多信息，请参阅[支持定制的语言](#languageSupport)。
{: note}

## 支持定制的语言
{: #languageSupport}

语言和声学模型定制仅可用于某些语言。下表记录了服务对每种语言的定制支持级别。

-   *GA* 指示接口已一般可用，可供生产环境使用。
-   *Beta* 指示接口作为 Beta 产品提供。
-   *不支持*表示接口不可用于该语言。

可以将宽带和窄带模型同时用于这些模型可用于的任何受支持语言。如果一种语言支持语言模型定制，那么也支持语法。有关所有可用模型的列表，请参阅[支持的语言模型](/docs/services/speech-to-text/models.html#modelsList)。

<table>
  <caption>表 1. 支持定制的语言</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      语言
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      支持语言模型定制
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      支持声学模型定制
    </th>
  </tr>
  <tr>
    <td>巴西葡萄牙语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>法语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>德语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>日语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>韩语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>普通话</td>
    <td style="text-align:center">不支持</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>现代标准阿拉伯语</td>
    <td style="text-align:center">不支持</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>西班牙语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>英国英语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>美国英语</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
</table>

可以使用 `GET /v1/models` 和 `GET /v1/models/{model_id}` 方法来检查语言模型是否支持语言模型定制。如果基本模型支持语言模型定制，那么模型的方法输出中的 `supported_features` 字段会将 `custom_language_model` 字段设置为 `true`。

## 定制用法说明
{: #customUsage}

以下用法说明同时适用于语言模型定制和声学模型定制。

### 定制模型的所有权
{: #customOwner}

拥有定制模型的 {{site.data.keyword.speechtotextshort}} 服务实例是其凭证用于创建该模型的实例。要以任何方式使用定制模型，必须在定制接口的方法中使用为该服务实例创建的服务凭证。为其他服务实例创建的凭证无法查看或访问该定制模型。

针对同一 {{site.data.keyword.speechtotextshort}} 服务实例获取的所有服务凭证，共享对创建用于该服务实例的所有定制模型的访问权。要限制对定制模型的访问权，请创建单独的服务实例，并仅使用该服务实例的凭证来创建和使用该模型。其他服务实例的凭证无法影响该定制模型。

在服务实例的凭证之间共享所有权的优点是可以取消一组凭证，例如在凭证泄露时。然后，可以为同一服务实例创建新凭证，并仍然保留使用原始凭证创建的定制模型的所有权和访问权。

### 请求日志记录和数据隐私
{: #customLogging}

服务如何处理定制接口调用的请求日志记录取决于请求：

-   服务*不会*记录用于构建定制模型的数据。例如，使用定制语言模型中的语料库和词时，无需设置 `X-Watson-Learning-Opt-Out` 请求头。您的训练数据绝不会用于改进服务的基本模型。
-   定制模型用于识别请求时，服务*会*记录数据。必须将 `X-Watson-Learning-Opt-Out` 请求头设置为 `true`，才能阻止对识别请求进行日志记录。

有关更多信息，请参阅[请求日志记录](/docs/services/speech-to-text/input.html#logging)。

### 信息安全
{: #customSecurity}

可以将客户标识与针对定制语言模型和定制声学模型添加或更新的数据相关联。通过使用以下方法传递 `X-Watson-Metadata` 头，可将客户标识与语料库、定制词、语法和音频资源相关联：

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

如果需要，随后可以使用 `DELETE /v1/user_data` 方法来删除数据。有关更多信息，请参阅[信息安全](/docs/services/speech-to-text/information-security.html)。
