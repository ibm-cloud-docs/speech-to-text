---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# 将语法用于定制语言模型
{: #grammars}

{{site.data.keyword.speechtotextfull}} 服务支持将语法用于定制语言模型。可以向定制语言模型添加语法，并将其用于语音识别。语法用于限制服务可以从音频中识别的短语集。
{: shortdesc}

语法使用正式的语言规范来定义一组用于转录字符串的生产规则。这些规则指定如何通过语言的字母表来形成有效的字符串。将语法应用于语音识别时，服务只能返回该语法生成的一个或多个短语。

例如，需要识别特定词或短语（如 *yes* 或 *no*）、单独的字母或数字或者名称列表时，使用语法会比检查替代词和文字记录更有效。此外，通过将搜索空间限制为搜索有效字符串，服务可以更快、更准确地交付结果。

将定制语言模型与语法一起用于语音识别时，服务可能返回根据语法确定的有效短语，也可能返回空结果。如果结果不为空，那么服务会在最终文字记录中包含置信度分数，就像对所有识别请求所做的一样。对于语法，分数指示的是响应与语法匹配的可能性。误报的可能性始终存在，对于简单的语法尤其如此，因此在评估服务的响应时，必须始终考虑服务结果的置信度。

语法是 Beta 功能。服务支持语法用于所有支持语言模型定制的语言。
有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

## 支持的语法格式
{: #grammarFormats}

{{site.data.keyword.speechtotextshort}} 服务支持采用以下标准格式定义的语法：

-   *扩充巴科斯范式 (ABNF)*，使用类似于传统 BNF 语法的纯文本表示法。此格式的媒体类型为 `application/srgs`。
-   *XML 格式*，使用 XML 元素来表示语法。此格式的媒体类型为 `application/srgs+xml`。

这两种语法格式都具有上下文无关语法 (CFG) 的表达能力。但是，服务只能对乔姆斯基谱系中的 3 型正则语法解码。此类语法表示有限状态自动机。

有关语法的常规信息，请参阅以下维基百科页面：

-   [Speech Recognition Grammar Specification](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [Augmented Backus-Naur form](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [Chomsky hierarchy](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## Speech Recognition Grammar Specification
{: #grammarSpecification}

{{site.data.keyword.speechtotextshort}} 服务支持 W3C [Speech Recognition Grammar Specification Version 1.0](https://www.w3.org/TR/speech-grammar/){: external} 定义的语法。该规范提供了有关受支持格式的详细信息以及有关定义语法的详细信息。有关受支持媒体类型的信息，请参阅该规范的 [Appendix G. Media Types and File Suffix](https://www.w3.org/TR/speech-grammar/#AppG){: external}。

目前，服务*不*支持该 Speech Recognition Grammar Specification 的所有功能。具体而言，服务不支持该规范以下部分中描述的功能：

-   [1.4 Semantic Interpretation](https://www.w3.org/TR/speech-grammar/#S1.4){: external} 部分。{{site.data.keyword.IBM_notm}} 将争取在服务的未来发行版中支持此功能。
-   [1.5 Embedded Grammars](https://www.w3.org/TR/speech-grammar/#S1.5){: external} 部分。{{site.data.keyword.IBM_notm}} 将争取在服务的未来发行版中支持此功能。
-   [2.2.2 External Reference by URI](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external} 部分。服务仅支持本地引用，如 [2.2.1 Local References](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external} 部分中所述。换句话说，语法必须是自包含的。
-   [2.2.3 Special Rules](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external} 部分。
-   [2.2.4 Referencing N-gram Documents (Informative)](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external} 部分。
-   [2.7 Language](https://www.w3.org/TR/speech-grammar/#S2.7){: external} 部分。服务不支持语言切换。对于每种语法，服务仅支持一种全局语言。

语法中的词必须采用 UTF-8 编码（ASCII 是 UTF-8 的子集）。使用其他任何编码都可能导致编译语法时发生问题，或者在解码时产生意外结果。服务会忽略语法头中指定的编码。
{: note}
