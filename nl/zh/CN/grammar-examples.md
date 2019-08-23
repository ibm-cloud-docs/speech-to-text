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

# 示例语法
{: #grammarExamples}

以下示例描述了将语法用于语音识别的更复杂用法。所提供的大部分示例都有 ABNF (`application/srgs`) 和 XML (`application/srgs+xml`) 这两种格式。使用这些示例有助于了解如何开始使用语法。
{: shortdesc}

在语法的生产规则中，如果包含 `\`（反斜杠），那么在 ABNF 中会产生语法错误，而在 XML 中会解释为字面值。
{: tip}

## 确认语法
{: #confirmation}

对于期望问题答案是一个词的应用程序，确认语法非常有用。以下语法定义了可能的 yes 和 no 响应的列表。

<table style="width:50%">
  <caption>表 1. 确认语法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>
    </td>
  </tr>
</table>

## 列出语法
{: #list}

对于期望用户从预定义字符串集中选择某个选项的应用程序，列出语法非常有用。这种字符串集有时称为平面列表。它通常由一组终端元素组成，而不包含复杂的规则结构。

-   以下语法定义了一个数字短语列表。

    <table style="width:50%">
      <caption>表 2. 列出数字语法</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>
        </td>
      </tr>
    </table>

-   以下语法定义了一个用户可从中选择的有效姓名列表，可能是从电话簿中选择某个人。

    <table style="width:50%">
      <caption>表 3. 列出姓名语法</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>
        </td>
        <td style="text-align:center">
          不可用
        </td>
      </tr>
    </table>

## 车辆识别号码语法
{: #vin}

车辆识别号码 (VIN) 使用固定的字母数字格式，这与信用卡号、电话号码和美国社会保障号一样。相对于传统的 n 元语法，这种语法的一大优点是对于指定此类格式非常有效。

VIN 格式的标准化程度很高，具有固定的字符数。对于这些类型的任务，传统 n 元语法的表现不佳。与这种语法不同的是，n 元语法无法确保提供所需的字符数。

以下语法可识别本田 VIN 代码。此示例比先前的示例更复杂，但很好地演示了这种语法的优点。

<table style="width:50%">
  <caption>表 4. VIN 语法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>。
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>。
    </td>
  </tr>
</table>

有关 VIN 格式的更多信息，请参阅 [Vehicle identification number](https://wikipedia.org/wiki/Vehicle_identification_number){: external}。

## 具有可选元素的语法
{: #optionalElements}

您可以将响应的某些元素设为可选项，让语法更灵活地预测可能的用户响应。以下语法使用了方括号将 `$optionalphrase` 元素括起，使其成为可选项。用户在说出社会保障号之前，可能会说一些额外的短语。例如，用户可能会说“my social is xxx xx xxxx”或仅仅说“xxx xx xxxx”。

<table style="width:50%">
  <caption>表 5. 可选元素语法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>
    </td>
    <td style="text-align:center">
      不可用
    </td>
  </tr>
</table>

有关语法中可选扩展的更多信息，请参阅 Speech Recognition Grammar Specification 中的 [2.5 Repeats](https://www.w3.org/TR/speech-grammar/#S2.5){: external} 部分。

## 消歧语法
{: #disambiguation}

其他可能的示例语法包括以下情况：所识别的响应的置信度并不是非常高，但应用程序需要确定用户的响应。在这种情况下，应用程序可以动态创建语法来包含最有可能的响应，然后要求用户重复其答案。例如，应用程序可以创建一个语法来包含两个最有可能的选项，然后要求用户从中选择一个：`Did you mean 'option 1' or 'option 9'?`

消歧语法是通过编程方式生成的。没有示例可供参考。
