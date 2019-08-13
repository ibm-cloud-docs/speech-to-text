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

# 사용자 정의 언어 모델에 문법 사용
{: #grammars}

{{site.data.keyword.speechtotextfull}} 서비스는 문법을 사용자 정의 언어 모델에 사용하도록 지원합니다. 문법을 사용자 정의 언어 모델에 추가하고 음성 인식에 사용할 수 있습니다. 문법은 서비스가 오디오에서 인식할 수 있는 구문 세트를 제한합니다.
{: shortdesc}

문법은 정규 언어 스펙을 사용하여 문자열을 변환하기 위한 프로덕션 규칙 세트를 정의합니다. 이 규칙은 언어의 알파벳에서 올바른 문자열을 형성하는 방법을 지정합니다. 문법을 음성 인식에 적용하는 경우 서비스가 문법에서 생성된 하나 이상의 구문만 리턴할 수 있습니다.

예를 들어, *예* 또는 *아니오*와 같은 특정 단어나 구문, 개별 문자나 숫자 또는 이름 목록을 인식해야 하는 경우 문법을 사용하는 것이 대체 단어 및 음성 내용을 검사하는 것보다 더 효과적일 수 있습니다. 또한 이 서비스는 올바른 문자열에 대한 검색 공간을 제한하여 더 빠르고 정확하게 결과를 전달할 수 있습니다.

사용자 정의 언어 모델 및 문법을 음성 인식에 사용하는 경우 서비스가 문법에서 올바른 구문을 리턴하거나 비어 있는 결과를 리턴할 수 있습니다. 결과가 비어 있지 않은 경우 이 서비스는 모든 인식 요청에 대해 수행하는 것과 마찬가지로 최종 음성 내용에 신뢰도 점수를 포함합니다. 문법의 경우 이 점수는 응답이 문법과 일치한 확률을 표시합니다. 특히 단순 문법의 경우 거짓 긍정이 항상 가능하므로 응답을 평가할 때 항상 서비스 결과의 신뢰도를 고려해야 합니다.

문법 기능은 베타 기능입니다. 이 서비스는 언어 모델 사용자 정의를 지원하는 모든 언어에 대한 문법을 지원합니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)을 참조하십시오.
{: note}

## 지원되는 문법 형식
{: #grammarFormats}

{{site.data.keyword.speechtotextshort}} 서비스는 다음과 같은 표준 형식으로 정의된 문법을 지원합니다.

-   기존의 BNF 문법과 유사한 일반 텍스트 표시를 사용하는 *ABNF(Augmented Backus-Naur Form)*. 이 형식의 매체 유형은 `application/srgs`입니다.
-   XML 요소를 사용하여 문법을 표시하는 *XML 양식*. 이 형식의 매체 유형은 `application/srgs+xml`입니다.

두 문법 형식 모두에는 CFG(Context-Free Grammar)의 표현 능력이 있습니다. 그러나 이 서비스는 촘스키 계층의 유형 3 정규 문법만 디코딩할 수 있습니다. 이러한 문법은 FSA(Finite State Automata)를 나타냅니다.

문법에 대한 일반 정보는 다음 Wikipedia 페이지를 참조하십시오.

-   [Speech Recognition Grammar Specification](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [Augmented Backus-Naur form](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [Chomsky hierarchy](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## 음성 인식 문법 스펙
{: #grammarSpecification}

{{site.data.keyword.speechtotextshort}} 서비스는 W3C [Speech Recognition Grammar Specification 버전 1.0](https://www.w3.org/TR/speech-grammar/){: external}에 정의된 문법을 지원합니다. 이 스펙은 지원되는 형식과 문법 정의에 대한 자세한 정보를 제공합니다. 지원되는 매체 유형에 대한 정보는 이 스펙의 [Appendix G. Media Types and File Suffix](https://www.w3.org/TR/speech-grammar/#AppG){: external}를 참조하십시오.

이 서비스는 현재 Speech Recognition Grammar Specification의 모든 기능을 지원하지 *않습니다*. 특히 이 서비스는 스펙의 다음 섹션에 설명된 기능을 지원하지 않습니다.

-   [섹션 1.4 Semantic Interpretation](https://www.w3.org/TR/speech-grammar/#S1.4){: external}. {{site.data.keyword.IBM_notm}}은 이 서비스의 향후 릴리스에서 이 기능을 지원하기 위해 노력하고 있습니다.
-   [섹션 1.5 Embedded Grammars](https://www.w3.org/TR/speech-grammar/#S1.5){: external}. {{site.data.keyword.IBM_notm}}은 이 서비스의 향후 릴리스에서 이 기능을 지원하기 위해 노력하고 있습니다.
-   [섹션 2.2.2 External Reference by URI](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external}. 이 서비스는 [섹션 2.2.1 Local References](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external}에 설명된 대로 로컬 참조만 지원합니다. 즉, 문법은 자체 포함되어야 합니다.
-   [섹션 2.2.3 Special Rules](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external}.
-   [섹션 2.2.4 Referencing N-gram Documents (Informative)](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external}.
-   [섹션 2.7 Language](https://www.w3.org/TR/speech-grammar/#S2.7){: external}. 이 서비스는 언어 전환을 지원하지 않으며 문법당 하나의 글로벌 언어만 지원합니다.

문법의 단어는 UTF-8 인코딩(ASCII는 UTF-8의 서브세트임)으로 되어 있어야 합니다. 다른 인코딩을 사용하면 문법을 컴파일할 때 문제가 발생하거나 디코딩 시 예상치 못한 결과가 발생할 수 있습니다. 이 서비스는 문법의 헤더에 지정된 인코딩을 무시합니다.
{: note}
