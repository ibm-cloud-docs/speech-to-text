---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-06"

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

# 使用语料库和定制词
{: #corporaWords}

您可以通过将语料库或语法添加到模型，或者通过直接添加定制词，从而使用词填充定制语言模型。
{: shortdesc}

-   **语料库：**使用词填充定制语言模型的建议方法是向模型添加一个或多个语料库。添加语料库时，服务会分析该文件，并自动将其找到的任何新词添加到定制模型。通过向定制模型添加语料库，服务可以在上下文中抽取特定于领域的词，这有助于确保更好的转录结果。有关更多信息，请参阅[使用语料库](#workingCorpora)。
-   **语法：**可以向定制模型添加语法，以将语音识别限制为语法识别到的词或短语。向模型添加语法时，服务会自动将其找到的任何新词添加到模型，就像添加语料库一样。有关更多信息，请参阅[将语法用于定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-grammars)。
-   **单个词：**还可以将单个定制词直接添加到模型。服务向模型添加词的方式，就像添加从语料库或语法中发现的词一样。直接添加词时，可以指定多种读法并指示词的显示方式。此外，还可以更新现有词，以修改或扩充从语料库或语法中抽取的定义。有关更多信息，请参阅[使用定制词](#workingWords)。

无论采用哪种方式添加词，服务都会将添加到定制语言模型的所有词存储在模型的词资源中。

## 词资源
{: #wordsResource}

*词资源*包含通过语料库添加、通过语法添加或直接添加的所有词。词资源的用途是定义服务基本词汇表中尚不存在的词的读法和拼写。这些定义指示服务如何对这些*未登录 (OOV) 词*进行转录。

词资源包含有关每个 OOV 词的以下信息。服务会为从语料库和语法中抽取的词创建定义。您可为直接添加或修改的词指定特征。

-   `word`：在语料库或语法中找到的词或您添加的词的拼写。
-   `sounds_like`：词的读法。对于从语料库和语法中抽取的词，此值表示服务如何认为词的读法是基于其语言规则的。在许多情况下，读法反映的是 `word` 字段的拼写。

    可以使用 `sounds_like` 字段来修改词的读法。还可以使用此字段为一个词指定多种读法。有关更多信息，请参阅[使用 sounds_like 字段](#soundsLike)。
-   `display_as`：服务在文字记录中使用的词的拼写。此字段指示词的显示方式。在大多数情况下，拼写与 `word` 字段的值相匹配。

    可以使用 `display_as` 字段为词指定不同的拼写。有关更多信息，请参阅[使用 display_as 字段](#displayAs)。
-   `source`：将词添加到词资源的方式。如果服务是从语料库或语法中抽取的词，那么此字段会列出该资源的名称。由于服务可能在多个资源中遇到同一词，因此该字段可能列出多个语料库或语法名称。如果是直接添加或修改的词，那么此字段包含字符串 `user`。

以任何方式更新模型的词资源后，必须训练模型以使更改在转录期间生效。有关更多信息，请参阅[训练定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)。

## 我需要多少数据？
{: #wordsResourceAmount}

许多因素会影响有效定制语言模型所需的数据量。因此，无法提供需要为任何定制模型或应用程序添加的确切词数。

根据用例，即使只将几个词直接添加到定制模型，也可能会提高模型的质量。但是，如果通过语料库添加 OOV 词，而该语料库在音频中使用这些词的上下文中显示这些词，那么可以大大提高转录准确性。有关更多信息，请参阅[使用语料库](#workingCorpora)。

服务限制了可以添加到定制语言模型的词数：

-   最多可以向定制模型的词资源添加 9 万个 OOV 词。此数字包括来自所有源（语料库、语法和直接添加的单个定制词）的 OOV 词。
-   最多可以从所有源向定制模型添加共 1000 万个词。此数字包含所有词，包括 OOV 词、服务基本词汇表中已包含的词以及语料库或语法中包含的词。对于语料库，服务会使用这些额外的词来了解可能出现 OOV 词的上下文，这就是为什么语料库是提高识别准确性的更有效方法的原因。

大型词资源可能会增加语音识别的等待时间，但确切的影响很难量化或预测。与生成有效定制模型所需的数据量一样，大型词资源的性能影响也取决于诸多因素。请使用不同数据量来测试定制模型，以确定模型和数据的性能。

## 使用语料库
{: #workingCorpora}

使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法来向定制模型添加语料库。语料库是一个纯文本文件，包含您领域中的样本句子。以下示例显示了缩略的医疗保健领域语料库。语料库文件通常要长得多。

```
我在旅行期间有发生健康问题的风险吗？
有些人在美国境外旅行时更有可能遇到健康问题。
如何治疗冠状动脉微血管病（冠状动脉 MVD）？
如果您被诊断患有冠状动脉 MVD，并且还患有贫血，那么通过治疗可能会使症状得到缓解。
贫血被认为会减缓修复受损血管所需细胞的增长速度。
导致自体免疫性肝炎的原因是什么？
自体免疫性、环境诱发因素和遗传倾向性综合起来可能会导致自体免疫性肝炎。
目前对脊髓损伤在开展哪些研究？
美国国家神经障碍与中风研究所 (NINDS) 在美国国家卫生研究院 (NIH) 的实验室开展脊髓研究。
NINDS 还支持通过向美国全国各地主要研究机构拨款来开展额外研究。
一些更有希望的康复方法可帮助脊髓损伤患者提高活动能力。
什么是成骨不全症 (OI)？
. . .
```
{: codeblock}

语音识别依赖于统计算法来分析音频。定制模型中的词会与服务基本词汇表中的词以及模型中的其他词进行竞争。（音频噪声和说话者口音等因素也会影响转录的质量。）

转录的准确性可能在很大程度上取决于模型中定义词的方式以及说话者对这些词的读法。为了提高服务的准确性，请使用语料库尽可能多地提供在相关领域中如何使用 OOV 词的示例。在语料库中重复 OOV 词可以提高定制语言模型的质量。如何重复语料库中的词取决于您预期用户在要识别的音频中对这些词的读法。用于表示说话者使用相关领域中词所处的上下文的句子添加得越多，服务的识别准确性越高。

服务并不是应用简单的词匹配算法。服务的转录取决于使用词的上下文。服务对语料库进行解析时，会包含有关定制模型中语料库句子中 n 元语法（两元语法、三元语法等）的信息。这些信息可帮助服务提高音频转录准确性，并且说明了为什么基于语料库训练定制模型比仅基于定制词训练模型更有价值。

例如，会计师遵循称为《公认会计原则》(GAAP) 的一组通用标准和过程。为金融领域创建定制模型时，请提供在上下文中使用术语 GAAP 的句子。句子可帮助服务区分一般短语（如“the gap between them is small”）和领域专用短语（如“GAAP provides guidelines for measuring and disclosing financial information”）。

通常，语料库最好在不同的上下文中使用词，这可以改进服务学习词的方式。但是，如果用户仅在几个上下文中说这些词，那么在其他上下文中显示这些词并不会提高定制模型的质量。因为说话者从不在这些上下文中使用这些词。如果说话者可能经常使用相同的短语，那么在语料库中重复该短语可以提高模型的质量。（在某些情况下，即使只将几个定制词直接添加到定制模型，也可能会产生积极的影响。）

### 准备语料库文本文件
{: #prepareCorpus}

遵循以下准则来准备语料库文本文件：

-   提供以 UTF-8 编码的纯文本文件（如果文件包含非 ASCII 字符）。如果服务遇到此类字符，将采用 UTF-8 编码。

    确保您知道语料库文本文件的字符编码。服务会保留在文本文件中找到的编码。在定制语言模型中使用这些词时，必须使用该编码。有关更多信息，请参阅[字符编码](#charEncoding)。
    {: important}
-   对语料库中的词使用一致的首字母大写。词资源是区分大小写的。请仅在需要时混合使用大小写字母以及使用首字母大写。
-   语料库中每个句子单独一行，并且每行以回车符终止。在同一行中包含多个句子会降低准确性。
-   将个人姓名作为独立的单元添加，一个姓名一行。不要在单独的行上添加一个姓名的各个词或将姓名添加为单独的定制词，也不要在语料库的同一行上包含多个姓名。以下示例显示了提高三个姓名的识别准确性的正确方法：

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    在可用之处包含其他上下文信息，例如 `Doctor Sebastian Leifson` 或 `President Malcolm Ingersol`。与所有词一样，请多次重复这些姓名，并且如果可能，在不同的上下文中重复姓名，这可以提高识别准确性。
-   注意拼写错误。服务会假定拼写错误是新词。除非在训练模型之前更正这些词，否则服务会将它们添加到模型的词汇表中。请记住这句格言：*垃圾进，垃圾出！*

句子越多，准确性越高。但是，服务确实将从所有源添加到模型的总词数限制为 1000 万个词，OOV 词数限制为 9 万个词。

### 添加语料库文件时的处理过程
{: #parseCorpus}

添加语料库文件时，服务会分析文件的内容。服务会抽取找到的任何新 OOV 词，并将每个 OOV 词添加到定制模型的词资源。为了从内容中提取最多的意义，服务会对它从语料库文件中读取的数据进行记号化并解析。以下各部分描述了服务如何为每种支持的语言解析语料库文件。

#### 解析英语、法语、德语、西班牙语和巴西葡萄牙语
{: #corpusLanguages}

以下描述适用于美国英语和英国英语、法语、德语、西班牙语和巴西葡萄牙语。

-   将数字转换为其等效词，例如：
    -   *对于英语*，`500` 转换为 `five hundred`，`0.15` 转换为 `zero point fifteen`。
    -   *对于法语*，`500` 转换为 `cinq cents`，`0,15` 转换为 <code>z&eacute;ro virgule quinze</code>。
    -   *对于德语*，`500` 转换为 <code>f&uuml;nfhundert</code>，`0,15` 转换为 <code>null punkt f&uuml;nfzehn</code>。
    -   *对于西班牙语*，`500` 转换为 `quinientos`，`0,15` 转换为 `cero coma quince`。
    -   *对于巴西葡萄牙语*，`500` 转换为 `quinhentos`，`0,15` 转换为 `zero ponto quinze`。
-   将包含特定符号的记号转换为有意义的字符串表示法，例如：
    -   转换 `$`（美元符号）和数字：
        -   *对于英语*，`$100` 转换为 `one hundred dollars`。
        -   *对于法语*，`$100` 转换为 `cent dollars`。
        -   *对于德语*，`$100` 和 `100$` 转换为 `einhundert dollar`。
        -   *对于西班牙语*，`$100` 和 `100$` 转换为 <code>cien d&oacute;lares</code>（或者如果方言为 `es-LA`，那么转换为 `cien pesos`）。
        -   *对于巴西葡萄牙语*，`$100` 和 `100$` 转换为 <code>cem d&oacute;lares</code>。
    -   转换 <code>&euro;</code>（欧元符号）和数字：
        -   *对于英语*，<code>&euro;100</code> 转换为 `one hundred euros`。
        -   *对于法语*，<code>&euro;100</code> 转换为 `cent euros`。
        -   *对于德语*，<code>&euro;100</code> 和 <code>100&euro;</code> 转换为 `einhundert euro`。
        -   *对于西班牙*，<code>&euro;100</code> 和 <code>100&euro;</code> 转换为 `cien euros`。
        -   *对于巴西葡萄牙*，<code>&euro;100</code> 和 <code>100&euro;</code> 转换为 `cem euros`。
    -   转换后跟 `%`（百分比符号）的数字：
        -   *对于英语，* `100%` 转换为 `one hundred percent`。
        -   *对于法语*，`100%` 转换为 `cent pour cent`。
        -   *对于德语*，`100%` 转换为 `einhundert prozent`。
        -   *对于西班牙语*，`100%` 转换为 `cien por ciento`。
        -   *对于巴西葡萄牙语*，`100%` 转换为 `cem por cento`。

    此列表并非详尽无遗。服务会根据需要对其他字符进行类似调整。
-   根据上下文处理非字母数字、标点和特殊字符。例如，如果 `$`（美元符号）或 <code>&euro;</code>（欧元符号）未后跟数字，那么服务会将其除去。处理与上下文相关，并且在支持的语言中保持一致。
-   忽略用 `( )`（圆括号）、`< >`（尖括号）、`[ ]`（方括号）或 `{ }`（花括号）括起的短语。

#### 解析日语
{: #corpusLanguages-jaJP}

-   将所有字符转换为全角字符。
-   将数字转换为其等效词，例如 `500` 转换为 <code>&#20116;&#30334;</code>，`0.15` 转换为 <code>&#12295;&#12539;&#19968;&#20116;</code>。
-   不将包含符号的记号转换为等效字符串，例如 `100%` 转换为 <code>&#30334;&#65285;</code>。
-   不自动除去标点。如果应用程序是基于转录，而不是基于听写的，那么 {{site.data.keyword.IBM_notm}} 强烈建议您除去标点。

#### 解析韩语
{: #corpusLanguages-koKR}

-   将数字转换为其等效词，例如 <code>10</code> 转换为 <code>&#49901;</code>。
-   除去以下标点和特殊字符：`- ( ) * : . , ' "`. 但是，对于其他语言会除去的标点和特殊字符，对于韩语不一定会全部除去，例如：
    -   仅当句点 (`.`) 符号出现在输入行末时才将其除去。
    -   不除去波浪号 (`~`)。
    -   不除去或以其他方式处理 Unicode 宽字符符号，例如 <code>&#8230;</code>（三个点或省略符）。

    通常，{{site.data.keyword.IBM_notm}} 建议您在处理语料库文件之前，先除去标点、特殊字符和 Unicode 宽字符。
-   不除去或忽略 `( )`（圆括号）、`< >`（尖括号）、`[ ]`（方括号）或 `{ }`（花括号）括起的短语。
-   将包含特定符号的记号转换为有意义的字符串表示法，例如：
    -   `24%` 转换为<code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>。
    -   `$10` 转换为<code>&#49901;&#45804;&#47084;</code>。

    此列表并非详尽无遗。服务会根据需要对其他字符进行类似调整。
-   对于由拉丁语（英语）字符组成的短语或由韩语和拉丁语字符混合组成的短语，服务会为短语创建 OOV 词，与其在语料库文件中显示的完全一样。服务会基于韩语转录，为词创建发音相似的读法。
   - 服务为 OOV 词 `London` 提供的发音相似的读法为 <code>&#47088;&#45912;</code>。
   - 服务为 OOV 词 <code>IBM&#54856;&#54168;&#51060;&#51648;</code> 提供的发音相似的读法为 <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>。

## 使用定制词
{: #workingWords}

您可以使用 `POST /v1/customizations/{customization_id}/words` 和 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法将新词添加到定制模型的词资源。还可以使用这两个方法来修改或扩充词资源中的词。

例如，您可能需要使用这两个方法来更正通过语料库添加词时所犯的拼写错误或其他错误。您可能还需要为现有词添加发音相似的读法定义。如果修改现有词，那么提供的新数据会覆盖词资源中该词的现有定义。添加词的规则也适用于修改现有词。

### 字符编码
{: #charEncoding}

通常，您很可能会通过语料库来添加大多数定制词。请确保您知道在语料库的文本文件中使用的字符编码。服务会保留在文本文件中找到的编码。

在定制语言模型中使用各个词时，必须使用该编码。使用 `GET`、`PUT` 或 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法来指定词时，必须对在 URL 中传递的 `word_name` 进行 URL 编码（如果词包含非 ASCII 字符）。

例如，下表显示了相同字母采用两种不同编码（ASCII 和 UTF-8）的情况。在 URL 中可以将 ASCII 字符作为 `z` 传递。但 UTF-8 字符必须作为 `%EF%BD%9A` 传递。
<table style="width:75%">
  <caption>表 1. 字符编码示例</caption>
  <tr>
    <th style="width:15%; text-align:center">字母</th>
    <th style="width:40%; text-align:center">编码</th>
    <th style="width:45%; text-align:center">值</th>
  </tr>
  <tr>
    <td style="text-align:center">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a` (`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 十六进制
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### 使用 sounds_like 字段
{: #soundsLike}

`sounds_like` 字段指定说话者对词的读法。缺省情况下，服务会自动使用词的拼写来填写此字段。对于很难发音或可采用不同读法的词，可以提供最多五种替代读法。请考虑将此字段用于：

-   *为首字母缩略词提供不同的读法*。例如，首字母缩略词 `NCAA` 可以按字母拼读，也可以口语化地读成 *N. C. double A*。以下示例为词 `NCAA` 添加了这两种发音相似的读法：

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *处理外来词*。例如，法语词 <code>gar&ccedil;on</code> 包含在英语语言中找不到的字符。您可以指定发音相似的 `gaarson`，其中将 <code>&ccedil;</code> 替换为 `s`，以向服务指示英语说话者对该词的读法。

语音识别使用统计算法来分析音频，因此添加一个词并不能保证服务可对其进行完全准确的转码。添加词时，请考虑该词可能的读法。请使用 `sounds_like` 字段来提供各种读法，以反映词可能的读法。以下各部分提供了用于指定发音相似的读法的特定于语言的准则。

#### 适用于英语（美国和英国）的准则
{: #wordLanguages-enUS-enGB}

*适用于美国英语和英国英语的准则：*

-   使用英语字母字符：`a-z` 和 `A-Z`。
-   对于很难发音的词，使用英语中可发音的实际词或自造词，例如，词 `Sczcesny` 读作 `shuchesnie`。
-   用等效的英语字母替代非英语字母，例如，用 `s` 替代 <code>&ccedil;</code>，或 `ny` 替代 <code>&ntilde;</code>。
-   用非重音字母替代重音字母，例如，用 `a` 替代 <code>&agrave;</code>，或 `e` 替代 <code>&egrave;</code>。
-   可以包含使用空格分隔的多个词，但服务会强制实施总共最多 40 个字符（不包括空格）的限制。

*仅适用于美国英语的准则：*

-   要读单个字母，请使用后跟句点的字母。如果句点后跟其他字符，请务必在句点和下一个字符之间使用一个空格。例如，请使用 `N. C. A. A.`，而*不要*使用 `N.C.A.A.`。
-   对数字使用拼写，例如用 `seventy-five` 表示 `75`。

*仅适用于英国英语的准则：*

-   对于英国英语，**不能**在发音相似的读法中使用句点或短划线。
-   要读单个字母，请使用后跟空格的字母。例如，请使用 `N C A A`，而*不要*使用 `N. C. A. A.`、`N.C.A.A.` 或 `NCAA`。
-   对数字使用不带短划线的拼写，例如用 `seventy five` 表示 `75`。

#### 适用于法语、德语、西班牙语和巴西葡萄牙语的准则
{: #wordLanguages-esES-frFR}

-   **不能**在发音相似的读法中使用短划线。
-   使用对于语言有效的字母字符：`a-z` 和 `A-Z`，包括有效的重音字母。
-   要读单个字母，请使用后跟句点的字母。如果句点后跟其他字符，请务必在句点和下一个字符之间使用一个空格。例如，请使用 `N. C. A. A.`，而*不要*使用 `N.C.A.A.`。
-   对于很难发音的词，请使用相应语言中可发音的实际词或自造词。
-   对数字使用不带短划线的拼写，例如，对于 `75`，请使用：
    -   *法语*：`soixante quinze`
    -   *德语*：<code>f&uuml;nfundsiebzig</code>
    -   *西班牙语*：`setenta y cinco`
    -   *巴西葡萄牙语*：`setenta e cinco`
-   可以包含使用空格分隔的多个词，但服务会强制实施总共最多 40 个字符（不包括空格）的限制。

#### 适用于日语的准则
{: #wordLanguages-jaJP}

-   通过 <code>&#8213;</code> 长音符号（*chou-on* 或日语的&#38263;&#38899;），仅使用全角片假名字符。不要使用半角字符。
-   仅在以下音节上下文中使用拗音（*yoh-on* 或日语的&#25303;&#38899;）：

    <code>&#12452;&#12455;</code>、<code>&#12454;&#12451;</code>、<code>&#12454;&#12455;</code>、<code>&#12454;&#12457;</code>、<code>&#12461;&#12451;</code>、<code>&#12461;&#12515;</code>、<code>&#12461;&#12517;</code>、<code>&#12461;&#12519;</code>、<code>&#12462;&#12515;</code>、<code>&#12462;&#12517;</code>、<code>&#12462;&#12519;</code>、<code>&#12463;&#12449;</code>、<code>&#12463;&#12451;</code>、<code>&#12463;&#12455;</code>、<code>&#12463;&#12457;</code>、<br/>
<code>&#12464;&#12449;</code>、<code>&#12464;&#12457;</code>、<code>&#12471;&#12451;</code>、<code>&#12471;&#12455;</code>、<code>&#12471;&#12515;</code>、<code>&#12471;&#12517;</code>、<code>&#12471;&#12519;</code>、<code>&#12472;&#12451;</code>、<code>&#12472;&#12455;</code>、<code>&#12472;&#12515;</code>、<code>&#12472;&#12517;</code>、<code>&#12472;&#12519;</code>、<code>&#12473;&#12451;</code>、<code>&#12474;&#12451;</code>、<code>&#12481;&#12455;</code>、<br/>
<code>&#12481;&#12515;</code>、<code>&#12481;&#12517;</code>、<code>&#12481;&#12519;</code>、<code>&#12482;&#12455;</code>、<code>&#12482;&#12515;</code>、<code>&#12482;&#12517;</code>、<code>&#12482;&#12519;</code>、<code>&#12484;&#12449;</code>、<code>&#12484;&#12451;</code>、<code>&#12484;&#12455;</code>、<code>&#12484;&#12457;</code>、<code>&#12486;&#12451;</code>、<code>&#12486;&#12517;</code>、<code>&#12487;&#12451;</code>、<code>&#12487;&#12515;</code>、<br/>
<code>&#12487;&#12517;</code>、<code>&#12487;&#12519;</code>、<code>&#12488;&#12453;</code>、<code>&#12489;&#12453;</code>、<code>&#12491;&#12455;</code>、<code>&#12491;&#12515;</code>、<code>&#12491;&#12517;</code>、<code>&#12491;&#12519;</code>、<code>&#12498;&#12515;</code>、<code>&#12498;&#12517;</code>、<code>&#12498;&#12519;</code>、<code>&#12499;&#12515;</code>、<code>&#12499;&#12517;</code>、<code>&#12499;&#12519;</code>、<code>&#12500;&#12451;</code>、<br/>
<code>&#12500;&#12515;</code>、<code>&#12500;&#12517;</code>、<code>&#12500;&#12519;</code>、<code>&#12501;&#12449;</code>、<code>&#12501;&#12451;</code>、<code>&#12501;&#12455;</code>、<code>&#12501;&#12457;</code>、<code>&#12501;&#12517;</code>、<code>&#12511;&#12515;</code>、<code>&#12511;&#12517;</code>、<code>&#12511;&#12519;</code>、<code>&#12522;&#12451;</code>、<code>&#12522;&#12455;</code>、<code>&#12522;&#12515;</code>、<code>&#12522;&#12517;</code>、<br/>
<code>&#12522;&#12519;</code>、<code>&#12532;&#12449;</code>、<code>&#12532;&#12451;</code>、<code>&#12532;&#12455;</code>、<code>&#12532;&#12457;</code>、<code>&#12532;&#12517;</code>

-   请仅在促音（*soku-on* 或日语的&#20419;&#38899;）后使用以下音节：

    <code>&#12496;</code>、<code>&#12499;</code>、<code>&#12502;</code>、<code>&#12505;</code>、<code>&#12508;</code>、<code>&#12481;</code>、<code>&#12481;&#12455;</code>、<code>&#12481;&#12515;</code>、<code>&#12481;&#12517;</code>、<code>&#12481;&#12519;</code>、<code>&#12480;</code>、<code>&#12487;</code>、<code>&#12487;&#12451;</code>、<code>&#12489;</code>、<code>&#12489;&#12453;</code>、<code>&#12501;</code>、<br/>
<code>&#12501;&#12449;</code>、<code>&#12501;&#12451;</code>、<code>&#12501;&#12455;</code>、<code>&#12501;&#12457;</code>、<code>&#12460;</code>、<code>&#12462;</code>、<code>&#12464;</code>、<code>&#12466;</code>、<code>&#12468;</code>、<code>&#12495;</code>、<code>&#12498;</code>、<code>&#12504;</code>、<code>&#12507;</code>、<code>&#12472;</code>、<code>&#12472;&#12455;</code>、<code>&#12472;&#12515;</code>、<br/>
<code>&#12472;&#12517;</code>、<code>&#12472;&#12519;</code>、<code>&#12459;</code>、<code>&#12461;</code>、<code>&#12463;</code>、<code>&#12465;</code>、<code>&#12467;</code>、<code>&#12461;&#12515;</code>、<code>&#12461;&#12517;</code>、<code>&#12461;&#12519;</code>、<code>&#12497;</code>、<code>&#12500;</code>、<code>&#12503;</code>、<code>&#12506;</code>、<code>&#12509;</code>、<code>&#12500;&#12515;</code>、<br/>
<code>&#12500;&#12517;</code>、<code>&#12500;&#12519;</code>、<code>&#12469;</code>、<code>&#12473;</code>、<code>&#12475;</code>、<code>&#12477;</code>、<code>&#12471;</code>、<code>&#12471;&#12455;</code>、<code>&#12471;&#12515;</code>、<code>&#12471;&#12517;</code>、<code>&#12471;&#12519;</code>、<code>&#12479;</code>、<code>&#12486;</code>、<code>&#12488;</code>、<code>&#12484;</code>、<code>&#12470;</code>、<br/>
<code>&#12474;</code>、<code>&#12476;</code>、<code>&#12478;</code>

-   不要将<code>&#12531;</code>用作词的第一个字符。例如，请使用<code>&#12454;&#12540;&#12531;&#12488;</code>，而不要使用<code>&#12531;&#12540;&#12488;</code>，后者是无效的。
-   许多复合词由*前缀+名词*或*名词+后缀*构成。服务的基本词汇表涵盖了频繁出现的大多数复合词（例如，<code>&#x9577;&#x96FB;&#x8A71;</code>和<code>&#x53E4;&#x65B0;&#x805E;</code>），但未涵盖那些不常出现的复合词。如果您的语料库通常包含复合词，那么作为定制的第一步，请将复合词添加为一个词。例如，<code>&#x53E4;&#x925B;&#x7B46;</code>在常规日语文本中并不常见；如果您经常使用该词，请将其添加到定制模型，以提高转录准确性。
-   不要使用尾部促音。

#### 适用于韩语的准则
{: #wordLanguages-koKR}

-   使用韩语字符、符号和音节。
-   还可以使用拉丁语（英语）字母字符：`a-z` 和 `A-Z`。
-   不要使用先前集内不包含的任何字符或符号。

### 使用 display_as 字段
{: #displayAs}

`display_as` 字段指定词在文字记录中的显示方式。此字段适用于您希望服务显示与词拼写不同的字符串的情况。例如，可以指示词 `hhonors` 显示为 `HHonors`，而不管其发音相似的读法是 `hilton honors` 还是 `h honors`。

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

再例如，可以指示词 `IBM` 要显示为 <code>IBM&trade;</code>。

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### 与智能格式设置和数字编辑进行交互
{: #displaySmart}

如果将 `smart_formatting` 或 `redaction` 参数用于识别请求，请注意，服务会先将智能格式设置和编辑应用于词，然后再考虑词的 `display_as` 字段。您可能需要对结果进行试验，以确保这些功能不会妨碍定制词的显示方式。您可能还需要添加定制词以包含这些结果。

例如，假设添加了`display_as` 字段为 `one` 的定制词 `one`。智能格式设置会将词 `one` 更改为数字 `1`，并且不会应用 display-as 值。要解决此问题，可以为数字 `1` 添加定制词，并将相同的 `display_as` 字段应用于该词。

有关使用这些功能的更多信息，请参阅[智能格式设置](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)和[数字编辑](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)。

### 添加或修改定制词时的处理过程
{: #parseWord}

服务如何响应添加或修改定制词的请求取决于您提供的值以及该词是否存在于服务的基本词汇表中。

<table>
  <caption>表 1. 添加具有不同字段的定制词</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>sounds_like</code> 字段为...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>display_as</code> 字段为...</th>
    <th style="text-align:left; vertical-align:bottom">响应</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果词在服务的基本词汇表中不存在</em>，服务会将 <code>sounds_like</code> 字段设置为该词的读法。将 <code>display_as</code> 字段设置为 <code>word</code> 字段的值。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果词在服务的基本词汇表中存在</em>，服务会将 <code>sounds_like</code> 和 <code>display_as</code> 字段保留为空。仅当词在服务的基本词汇表中存在时，这两个字段才为空。该词在模型的词资源中存在没有不良影响，但也没有必要。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 字段有效</em>，服务会将 <code>display_as</code> 字段设置为 <code>word</code> 字段的值。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 字段无效</em>：
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              <code>POST /v1/customizations/{customization_id}/words</code> 方法会向模型词资源中的该词添加 <code>error</code> 字段。
            </li>
            <li style="margin:10px 0px; line-height:120%">
              <code>PUT /v1/customizations/{customization_id}/words/{word_name}</code> 方法失败，并返回 400 响应代码和错误消息。服务不会向词资源添加该词。
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果词在服务的基本词汇表中不存在</em>，服务会将 <code>sounds_like</code> 字段设置为该词的读法，并将 <code>display_as</code> 字段保留为指定值。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果词在服务的基本词汇表中存在</em>，服务会将 <code>sounds_like</code> 保留为空，并将 <code>display_as</code> 字段保留为指定值。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 字段有效</em>，服务会将 <code>sounds_like</code> 和 <code>display_as</code> 字段设置为指定值。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 字段无效</em>，服务会按照指定了 <code>sounds_like</code> 字段但未指定 <code>display_as</code> 字段的情况进行响应。
        </li>
      </ul>
    </td>
  </tr>
</table>

## 验证词资源
{: #validateModel}

尤其是向定制语言模型添加了语料库或一次添加了多个定制词时，请检查模型词资源中的 OOV 词。

-   *查找拼写错误和其他错误*。尤其是添加了语料库（可能很大）时，很容易会犯错误。语料库（或语法）文件中的拼写错误会导致将新词添加到模型的词资源的意外结果，就如语料库文件中留下的格式错误的 HTML 标记一样。
-   *验证发音相似的读法*。服务会自动为 OOV 词生成发音相似的读法。在大多数情况下，这些读法已足够。但对于有异常拼写或很难发音的词，以及对于首字母缩略词和技术术语，建议复查这些读法的准确性。

要验证定制模型的词并根据需要进行更正（不管词是以何种方式添加到词资源的），请使用以下方法：

-   使用 `GET /v1/customizations/{customization_id}/words` 方法来列出定制模型中的所有词，或使用 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法来查询单个词。有关更多信息，请参阅[列出定制语言模型中的词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。
-   修改定制模型中的词以更正错误，或者使用 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法来添加 sounds-like 或 display-as 值。有关更多信息，请参阅[使用定制词](#workingWords)。
-   使用 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法来删除错误地（例如，因语料库中的拼写错误或其他错误）引入的无关词。有关更多信息，请参阅[从定制语言模型中删除词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#deleteWord)。
    -   如果词是从语料库中抽取的，那么可以改为更新语料库文本文件来更正错误，然后使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法的 `allow_overwrite` 参数来重新装入该文件。有关更多信息，请参阅[向定制语言模型添加语料库](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)。
    -   如果词是从语法中抽取的，那么可以更新语法文件来更正错误，然后使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法的 `allow_overwrite` 参数来重新装入该文件。有关更多信息，请参阅[向定制语言模型添加语法](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)。
