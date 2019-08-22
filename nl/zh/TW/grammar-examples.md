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

# 文法範例
{: #grammarExamples}

下列範例說明文法在語音辨識中的更複雜用法。大部分範例是以 ABNF (`application/srgs`) 和 XML (`application/srgs+xml`) 格式提供。請使用這些範例來協助您開始使用文法。
{: shortdesc}

在文法的正式作業規則中，包含 `\`（反斜線）是 ABNF 中的語法錯誤，會被解譯為 XML 中的文字值。
{: tip}

## 確認文法
{: #confirmation}

確認文法對於預期以一個字的答案來回應問題的應用程式很有幫助。下列文法定義可能的 yes 及 no 回應清單。

<table style="width:50%">
  <caption>表 1. 確認文法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>
    </td>
  </tr>
</table>

## 清單文法
{: #list}

清單文法對於預期使用者從一組預先定義的字串中選取選項的應用程式很有幫助。此組有時稱為階層清單。它通常是由終端元素清單組成，不包含複雜的規則結構。

-   下列文法定義數字詞組清單。

    <table style="width:50%">
      <caption>表 2. 清單數字文法</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>
        </td>
      </tr>
    </table>

-   下列文法定義使用者可以從中選擇的有效名稱清單，也許是從電話簿中選取個人。

    <table style="width:50%">
      <caption>表 3. 清單名稱文法</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>
        </td>
        <td style="text-align:center">
          無法使用
        </td>
      </tr>
    </table>

## 車輛識別號碼文法
{: #vin}

車輛識別號碼 (VIN) 使用固定英數格式，就像信用卡號碼、電話號碼和美國社會保險號碼那樣。文法勝於典型 n-grams 的一大優點是，文法在指定這類格式時非常有效率。

VIN 格式已充分標準化，具有固定數目的字元。典型 n-grams 對於這些類型作業的執行效能不佳。與文法不同，它們無法確定已提供所需的字元數。

下列文法辨識 Honda VIN 碼。它們比先前的範例更複雜，但對於文法的強大功能有很好的示範。

<table style="width:50%">
  <caption>表 4. VIN 文法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>。
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>。
    </td>
  </tr>
</table>

如需 VIN 格式的相關資訊，請參閱[車輛識別號碼](https://wikipedia.org/wiki/Vehicle_identification_number){: external}。

## 具有選用元素的文法
{: #optionalElements}

藉由讓回應的某些元素成為選用項目，您可以預測使用者可能的回應，讓文法更有彈性。下列文法在 `$optionalphrase` 元素周圍加上方括弧，讓它成為選用項目。使用者在陳述社會保險號碼之前，可以先說一些其他詞組。例如，使用者可以說 "my social is xxx xx xxxx" 或只說 "xxx xx xxxx"。

<table style="width:50%">
  <caption>表 5. 選用性元素文法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>
    </td>
    <td style="text-align:center">
      無法使用
    </td>
  </tr>
</table>

如需文法中選用擴充的相關資訊，請參閱「語音辨識語法規格」的 [2.5 節：重複](https://www.w3.org/TR/speech-grammar/#S2.5){: external}。

## 澄清文法
{: #disambiguation}

其他可能的範例文法包括這類狀況：所辨識回應的信賴度並不是很高，但是應用程式必須確定使用者的回應。在此情況下，應用程式可以動態建立包含最可能的回應的文法，並要求使用者重複該回答。例如，應用程式可以建立包含兩個最可能選項的文法，並要求使用者選取這兩者其中之一：`Did you mean 'option 1' or 'option 9'?`

澄清文法是以程式設計方式產生。未提供任何範例。
