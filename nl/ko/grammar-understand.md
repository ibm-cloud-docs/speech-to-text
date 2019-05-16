---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 문법 이해
{: #grammarUnderstand}

다음 예제는 문법에 대한 {{site.data.keyword.speechtotextfull}} 서비스의 지원을 소개합니다. 이 예제에서는 두 개의 단순 ABNF 문법을 작성하고 이러한 문법이 음성 인식에 사용되는 경우 가능한 결과를 표시합니다. 이 예제는 서비스가 음성 내용에 포함하는 신뢰도 점수 검사의 중요성을 보여줍니다.
{: shortdesc}

이 예제는 음성 인식 요청의 결과만 제공합니다. 음성 인식을 위해 문법을 전달하는 방법을 보여주는 예제는 [음성 인식에 문법 사용](/docs/services/speech-to-text/grammar-use.html)을 참조하십시오. 또한 이 예제는 매우 기본적인 것입니다. 보다 복잡한 문법의 예제는 [예제 문법](/docs/services/speech-to-text/grammar-examples.html)을 참조하십시오.

## 단일 구문 일치: yesno 문법
{: #yesnoGrammar}

첫 번째 예제는 두 개의 유효한 단일 단어 응답 `yes` 및 `no`를 허용하는 매우 단순한 `yesno` 문법을 정의합니다. 이 문법은 사용자가 두 개의 구문 중 하나로만 응답해야 하는 경우에 유용합니다.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

이 문법을 음성 인식 요청에 적용하면 이 서비스가 일치사항에 대한 해당 신뢰도를 표시하는 점수가 포함된 음성 내용을 리턴할 수 있습니다. 또한 입력이 두 개의 구문 중 하나와 명확하게 일치하지 않으면 결과를 리턴할 수 없습니다.

예를 들어, 사용자가 `yes`로 응답하는 경우 이 서비스가 다음 결과와 매우 유사한 응답을 리턴합니다. `confidence` 필드의 점수는 완벽하게 신뢰할 수 있는 일치를 표시합니다.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

그러나 예를 들어, 사용자가 `nope`을 응답한다고 가정하십시오. 이 서비스는 신뢰도 점수가 매우 낮은 결과를 리턴하거나 결과를 리턴하지 않을 수 있습니다. 비어 있는 결과는 응답이 문법과 일치하지 않는다는 가장 명확한 표시입니다. 유효한 응답이 특정 다중 구문 순서와 일치해야 하는 복잡한 문법에서는 비어 있는 응답이 발생할 가능성이 높습니다.

## 다중 구문 일치: names 문법
{: #namesGrammar}

다중 구문 문법을 사용하는 경우 사용자의 응답이 인식되려면 완전해야 합니다. 사용자가 단어를 생략하거나 응답 중간에 중지할 수 없습니다. 하나의 단어만 없어도 서비스가 비어 있는 결과를 리턴할 수 있습니다.

또한 구문이 독립적인 발화임을 표시하기 위해 사용자가 충분한 무음으로 분리된 구문을 말하는 경우 이 서비스가 여러 음성 내용을 리턴할 수 있습니다. 예를 들어, 세 개의 여러 단어로 이루어진 이름 중 하나와 일치할 수 있는 단순 `names` 문법을 고려하십시오.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

사용자가 문법 규칙에 있는 이름 중 하나인 `Yon See`를 말한다고 가정하십시오. 이 서비스는 일치사항에 대한 매우 높은 신뢰수준을 표시하는 응답을 리턴합니다.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

이제 사용자가 각각이 별도의 발화임을 표시하기 위해 0.8초 이상의 충분한 무음으로 분리된 두 개의 이름을 말한다고 가정하십시오(`Yon See` [1.0초의 무음] `Yi Wen Tan`). 이 경우 이 서비스는 각 음성 내용에 대한 신뢰도 점수가 서로 다른 두 개의 개별 응답을 전송합니다.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

마지막으로 사용자가 대신 `Yon See` [1.0초의 무음] `Young Says He`와 같은 내용을 말하는 경우를 고려하십시오. 첫 번째 구문 `Yon See`의 경우 이 서비스가 이전 예제와 유사한 신뢰도 정수로 긍정 일치를 전송합니다. 두 번째 구문 `Young Says He`의 경우 이 서비스는 두 가지 가능한 응답 중 하나를 얻을 수 있습니다.

-   응답을 전송하지 않을 수 있습니다. 이는 구문이 문법의 규칙 중 하나와 일치하지 않음을 표시합니다.
-   대신 신뢰도 점수가 낮은 응답을 전송할 수 있습니다. 이는 구문이 규칙 중 하나와 음향적으로 유사하지만 일치할 가능성이 낮음을 표시합니다.

## 신뢰도 점수 및 비어 있는 결과
{: #confidenceScores}

이전 예제의 문법과 같은 비교적 단순한 문법의 경우 응답이 문법과 전혀 일치하지 않는 것으로 보이는 경우에도 이 서비스는 신뢰도 점수가 낮은 결과를 리턴할 수 있습니다. 놀라워 보일 수 있지만 신뢰도 점수가 낮은 응답은 응답이 문법과 일치할 가능성이 낮은 경우에도 이 서비스가 지정된 오디오에 대해 찾을 수 있는 가장 적합한 일치사항을 표시할 수 있습니다. 응답이 문법과 일치하지 않으면 일반적으로 결과의 신뢰도 값이 매우 낮으며, 이는 문법이 응답을 생성할 가능성이 없음을 표시합니다.

응답이 문법을 충족하는지 여부를 평가할 때 항상 신뢰도 점수를 고려하십시오. 신뢰도에 따라 결과를 거부하기 위해 설정할 임계값을 모르는 경우 초기값 0.5를 사용하십시오. 올바른 결과의 거부율이 예상치 못하게 큰 경우 신뢰도 임계값을 줄이십시오. 예를 들어, 일부 애플리케이션에는 0.1이 적합한 옵션일 수 있습니다. 다수의 올바르지 않은 인식이 발생하는 경우 해당 애플리케이션에 대한 신뢰도 임계값을 늘리십시오.

많은 경우에 비어 있는 결과 또는 신뢰도 점수가 매우 낮은 결과는 유효한 응답입니다. 이는 사용자가 질문 또는 사용 가능한 옵션을 이해하지 못했음을 표시할 수 있습니다. 항상 문법 없이 사용자의 오디오 응답을 인식할 수 있지만 문법에 유효하지 않은 결과를 얻게 될 위험이 있습니다. 문법은 전체 무음 이외의 모든 음성에 대한 결과를 항상 리턴하는 n-그램보다 더 신뢰할 수 있는 결과를 제공합니다.

결과가 문법에 정의된 유효한 구문 중 하나여야 함을 아는 것은 애플리케이션의 응답 처리를 단순화할 수 있는 강력한 기능입니다. 일반적으로 이 서비스는 문법에서 생성된 발화에 대한 신뢰도가 높은 결과만 리턴할 수 있습니다. 첫 번째 예제의 `yesno` 문법이 `nope` 구문을 유효한 응답으로 확실하게 인식하려면 해당 구문을 문법에 추가해야 합니다.
