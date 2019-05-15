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

# 文法の例
{: #grammarExamples}

以下の例では、音声認識における文法の複雑な使用法を説明しています。ほとんどの例は、ABNF (`application/srgs`) フォーマットと XML (`application/srgs+xml`) フォーマットの両方で示されています。これらの例を参考にして、文法の使用を開始してください。
{: shortdesc}

文法の作成ルールに「\」 (円記号) を含めた場合、ABNF では構文エラーとなり、XML ではリテラル値として解釈されます。
{: tip}

## 確認文法
{: #confirmation}

確認文法は、質問に対する応答として 1 語の回答を想定しているアプリケーションの場合に便利です。次の文法では、「はい」と「いいえ」という選択可能な応答のリストを定義しています。

<table style="width:50%">
  <caption>表 1. 確認文法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>
    </td>
  </tr>
</table>

## リスト文法
{: #list}

リスト文法は、事前に定義されたストリングのセットからユーザーが 1 つの選択肢を選択することを想定しているアプリケーションの場合に便利です。このセットはフラット・リストと呼ばれることがあります。このセットは一般に終端要素のリストからなり、複雑なルール構造を含んでいません。

-   次の文法では、数字の句のリストを定義しています。

    <table style="width:50%">
      <caption>表 2. 数字リスト文法</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>
        </td>
      </tr>
    </table>

-   次の文法では、ユーザーが選択できる有効な名前のリストを定義しています (電話帳から個人を選択する場合など)。

    <table style="width:50%">
      <caption>表 3. 名前リスト文法</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>
        </td>
        <td style="text-align:center">
          使用不可
        </td>
      </tr>
    </table>

## 車両識別番号文法
{: #vin}

車両識別番号 (VIN) では、クレジット・カード番号、電話番号、米国社会保障番号のように固定英数字フォーマットが使用されます。従来の n-gram にはない文法の大きな利点は、文法はこのようなフォーマットを指定するのに非常に有効である点です。

VIN フォーマットは高度に標準化されており、文字数が固定されています。従来の n-gram は、これらのような値を扱うのに適していません。文法とは異なり、n-gram は必要な数の文字が指定されていることを保証できません。

次の文法では、ホンダ車の VIN コードを認識します。これらの文法はこれまでに紹介した文法例よりも複雑ですが、文法の機能をうまく示しています。

<table style="width:50%">
  <caption>表 4. VIN 文法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>.
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>.
    </td>
  </tr>
</table>

VIN フォーマットについて詳しくは、Wikipedia で [Vehicle identification number ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://en.wikipedia.org/wiki/Vehicle_identification_number){: new_window} を参照してください。

## オプション要素が含まれた文法
{: #optionalElements}

応答の特定の要素をオプションにすると、ユーザーの応答内容を予測して文法の柔軟性を高めることができます。次の文法では、`$optionalphrase` 要素を大括弧で囲んでこの要素をオプションにします。ユーザーは、社会保障番号を述べる前にいくつかの追加の句のいずれかを話すことができます。例えば、ユーザーは「私の社会保障番号は xxx xx xxxx」と話しても、「xxx xx xxxx」とだけ話してもかまいません。

<table style="width:50%">
  <caption>表 5. オプション要素文法</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a>
    </td>
    <td style="text-align:center">
      使用不可
    </td>
  </tr>
</table>

文法内のオプション拡張について詳しくは、Speech Recognition Grammar Specification の [Section 2.5 Repeats ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.w3.org/TR/speech-grammar/#S2.5){: new_window} を参照してください。

## 明確化文法
{: #disambiguation}

他の考えられる文法例としては、認識された応答の信頼度はあまり高くないが、アプリケーション側ではユーザーの応答を確定する必要がある状況が挙げられます。この場合は、アプリケーションは最も可能性の高いいくつかの応答が含まれた文法を動的に作成して、ユーザーに回答を繰り返すように要請できます。例えば、アプリケーションは最も可能性の高い 2 つのオプションが含まれた文法を作成して、これら 2 つのうち 1 つをユーザーに選択させることができます (`Did you mean 'option 1' or 'option 9'?`)。

明確化文法はプログラムで生成されます。このため、例を示していません。
