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

# 예제 문법
{: #grammarExamples}

다음 예제는 문법을 음성 인식에 사용하는 더 복잡한 경우에 대해 설명합니다. 대부분의 예제는 ABNF(`application/srgs`) 및 XML(`application/srgs+xml`) 형식 모두로 제공됩니다. 다음 예제를 사용하여 문법을 시작하는 데 도움을 받으십시오.
{: shortdesc}

문법의 프로덕션 규칙에서 `\`(백슬래시)를 포함하는 것이 ABNF에서는 구문 오류이며 XML에서는 리터럴 값으로 해석됩니다.
{: tip}

## 확인 문법
{: #confirmation}

확인 문법은 질문에 대한 응답으로 한 단어 응답을 예상하는 애플리케이션에 유용합니다. 다음 문법은 가능한 예 및 아니오 응답의 목록을 정의합니다.

<table style="width:50%">
  <caption>표 1. 확인 문법</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
    </td>
  </tr>
</table>

## 목록 문법
{: #list}

목록 문법은 사용자가 사전 정의된 문자열 세트에서 옵션을 선택할 것으로 예상하는 애플리케이션에 유용합니다. 이 세트를 일반 목록이라고도 합니다. 이 문법은 일반적으로 터미널 요소의 목록으로 구성되며 복잡한 규칙 구조를 포함하지 않습니다.

-   다음 문법은 숫자에 대한 구문 목록을 정의합니다.

    <table style="width:50%">
      <caption>표 2. 숫자 나열 문법</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
        </td>
      </tr>
    </table>

-   다음 문법은 사용자가 전화번호부에서 개인을 선택하기 위해 선택할 수 있는 유효한 이름의 목록을 정의합니다.

    <table style="width:50%">
      <caption>표 3. 이름 나열 문법</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
        </td>
        <td style="text-align:center">
       사용할 수 없음
        </td>
      </tr>
    </table>

## 차대 번호(VIN) 문법
{: #vin}

차대 번호(VIN)는 신용카드 번호, 전화번호 및 미국 사회 보장 번호와 마찬가지로 고정된 영숫자 형식을 사용합니다. 기존의 n-그램에 비해 문법의 큰 장점은 문법이 이러한 형식을 지정하는 데 매우 효과적이라는 점입니다.

VIN 형식은 잘 표준화되어 있으며 고정된 수의 문자를 포함합니다. 기존의 n-그램은 이러한 유형의 태스크에 제대로 작동하지 않습니다. 문법과 달리 n-그램은 필수 문자 수가 제공되었는지 확인할 수 없습니다.

다음 문법은 Honda VIN 코드를 인식합니다. 이전 예제보다 더 복잡하지만 문법의 강점을 잘 보여줍니다.

<table style="width:50%">
  <caption>표 4. VIN 문법</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
    </td>
  </tr>
</table>

VIN 형식에 대한 자세한 정보는 [Vehicle identification number](https://wikipedia.org/wiki/Vehicle_identification_number){: external}를 참조하십시오. 

## 선택적 요소가 있는 문법
{: #optionalElements}

응답의 특정 요소를 선택 가능하게 하여 사용자가 응답하는 방식을 예측함으로써 문법을 보다 유연하게 만들 수 있습니다. 다음 문법은 `$optionalphrase` 요소 주위에 대괄호를 추가하여 선택사항으로 만듭니다. 사용자가 사회 보장 번호를 언급하기 전에 몇 가지 추가 구문 중 하나를 말할 수 있습니다. 예를 들어, 사용자가 "my social is xxx xx xxxx"라고 말하거나 "xxx xx xxxx"만 말할 수 있습니다.

<table style="width:50%">
  <caption>표 5. 선택적 요소 문법</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>
    </td>
    <td style="text-align:center">
      사용할 수 없음
    </td>
  </tr>
</table>

문법의 선택적 확장에 대한 자세한 정보는 Speech Recognition Grammar Specification의 [섹션 2.5 Repeats](https://www.w3.org/TR/speech-grammar/#S2.5){: external}를 참조하십시오.

## 명확화 문법
{: #disambiguation}

가능한 다른 예제 문법에는 인식된 응답의 신뢰도가 높지 않지만 애플리케이션이 사용자의 응답을 확신해야 하는 상황이 포함됩니다. 이 경우 애플리케이션이 가장 가능성 있는 응답을 포함하는 문법을 동적으로 작성하고 사용자에게 응답을 반복하도록 요청할 수 있습니다. 예를 들어, 애플리케이션이 가장 가능성 있는 두 개의 옵션을 포함하는 문법을 작성하고 둘 중 하나를 선택하도록 사용자에게 요청할 수 있습니다(`Did you mean 'option1' or 'option 9'?`).

명확화 문법은 프로그래밍 방식으로 생성됩니다. 예제는 제공되지 않습니다.
