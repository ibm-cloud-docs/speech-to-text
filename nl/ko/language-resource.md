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

# 말뭉치 및 사용자 정의 단어에 대한 작업
{: #corporaWords}

말뭉치 또는 문법을 모델에 추가하거나 사용자 정의 단어를 직접 추가하여 사용자 정의 언어 모델을 단어로 채울 수 있습니다.
{: shortdesc}

-   **말뭉치:** 사용자 정의 언어 모델을 단어로 채울 때 권장되는 방법은 모델에 하나 이상의 말뭉치를 추가하는 것입니다. 말뭉치를 추가하면 서비스가 파일을 분석하고 발견한 새 단어를 자동으로 사용자 정의 모델에 추가합니다. 사용자 정의 모델에 말뭉치를 추가하면 서비스가 컨텍스트에서 도메인 특정 단어를 추출하여 더 나은 변환 결과를 얻을 수 있습니다. 자세한 정보는 [말뭉치에 대한 작업](#workingCorpora)을 참조하십시오.
-   **문법:** 문법을 사용자 정의 모델에 추가하여 음성 인식을 문법에서 인식되는 단어 또는 구문으로 제한할 수 있습니다. 문법을 모델에 추가하면 이 서비스가 말뭉치에 대해 수행하는 것과 마찬가지로 발견한 새 단어를 자동으로 모델에 추가합니다. 자세한 정보는 [사용자 정의 언어 모델에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammars)을 참조하십시오.
-   **개별 단어:** 개별 사용자 정의 단어를 모델에 직접 추가할 수도 있습니다. 이 서비스가 말뭉치 또는 문법에서 검색한 단어를 추가하는 것과 마찬가지로 단어를 모델에 추가합니다. 단어를 직접 추가하는 경우 여러 발음을 지정하고 단어가 표시되는 방법을 표시할 수 있습니다. 기존 단어를 업데이트하여 말뭉치 또는 문법에서 추출된 정의를 수정하거나 기능 보강할 수도 있습니다. 자세한 정보는 [사용자 정의 단어에 대한 작업](#workingWords)을 참조하십시오.

추가하는 방법과 관계없이 이 서비스는 사용자 정의 언어 모델에 추가하는 모든 단어를 모델의 단어 리소스에 저장합니다.

## 단어 리소스
{: #wordsResource}

*단어 리소스*에는 말뭉치 또는 문법에서 추가하거나 직접 추가하는 모든 단어가 포함됩니다. 단어 리소스의 목적은 서비스의 기본 어휘에 이미 존재하지 않는 단어의 발음과 맞춤법을 정의하는 것입니다. 이 정의는 서비스에 이러한 *OOV(Out Of Vocabulary) 단어*를 변환하는 방법을 알립니다.

단어 리소스에는 각 OOV 단어에 대한 다음 정보가 포함됩니다. 서비스가 말뭉치 및 문법에서 추출된 단어에 대한 정의를 작성합니다. 직접 추가하거나 수정하는 단어의 특성을 지정합니다.

-   `word`: 말뭉치 또는 문법에서 발견되었거나 사용자가 추가한 단어의 맞춤법입니다.
-   `sounds_like`: 단어의 발음입니다. 말뭉치 및 문법에서 추출된 단어의 경우 이 값은 서비스에서 단어가 해당 언어 규칙에 따라 발음되는 것으로 믿는 방식을 나타냅니다. 많은 경우 발음은 `word` 필드의 맞춤법을 반영합니다.

    `sounds_like` 필드를 사용하여 단어의 발음을 수정할 수 있습니다. 이 필드를 사용하여 한 단어에 대한 여러 발음을 지정할 수도 있습니다. 자세한 정보는 [sounds_like 필드 사용](#soundsLike)을 참조하십시오.
-   `display_as`: 서비스가 음성 내용에서 사용하는 단어의 맞춤법입니다. 이 필드는 단어가 표시되는 방법을 나타냅니다. 대부분의 경우 맞춤법은 `word` 필드의 값과 일치합니다.

    `display_as` 필드를 사용하여 단어에 대한 다른 맞춤법을 지정할 수 있습니다. 자세한 정보는 [display_as 필드 사용](#displayAs)을 참조하십시오.
-   `source`: 단어가 단어 리소스에 추가된 방법을 표시합니다. 서비스가 말뭉치 또는 문법에서 단어를 추출한 경우 이 필드에는 해당 리소스의 이름이 나열됩니다. 서비스가 여러 리소스에서 동일한 단어를 발견할 수 있으므로 이 필드에 여러 말뭉치 또는 문법 이름이 나열될 수 있습니다. 단어를 직접 추가하거나 수정하는 경우 이 필드에 `user`라는 문자열이 포함됩니다.

어떤 방식으로든 모델의 단어 리소스를 업데이트하는 경우 변환 중에 변경사항을 적용하려면 모델을 훈련해야 합니다. 자세한 정보는 [사용자 정의 언어 모델 훈련](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)을 참조하십시오.

## 필요한 데이터의 양
{: #wordsResourceAmount}

다양한 요인이 효과적인 사용자 정의 언어 모델에 필요한 데이터의 양에 영향을 미칩니다. 사용자 정의 모델 또는 애플리케이션에 대해 추가해야 하는 정확한 단어의 수를 제공할 수 없습니다.

유스 케이스에 따라 몇 개의 단어를 사용자 정의 모델에 직접 추가해도 모델의 품질이 향상될 수 있습니다. 그러나 오디오에서 사용되는 컨텍스트의 단어를 표시하는 말뭉치의 OOV 단어를 추가하면 변환 정확도가 크게 향상될 수 있습니다. 자세한 정보는 [말뭉치에 대한 작업](#workingCorpora)을 참조하십시오.

이 서비스는 사용자 정의 언어 모델에 추가할 수 있는 단어의 수를 제한합니다.

-   사용자 정의 모델의 단어 리소스에 최대 9만 개의 OOV 단어를 추가할 수 있습니다. 이 기능에는 모든 소스(말뭉치, 문법 및 사용자가 직접 추가한 개별 사용자 정의 단어)의 OOV 단어가 포함됩니다.
-   모든 소스의 사용자 정의 모델에 최대 총 천만 개의 단어를 추가할 수 있습니다. 이 수치에는 말뭉치 또는 문법에 포함된 모든 단어(OOV 단어 및 이미 서비스 기본 어휘의 일부인 단어 모두)가 포함됩니다. 말뭉치의 경우 서비스가 이러한 추가 단어를 사용하여 OOV 단어가 표시될 수 있는 컨텍스트를 학습하며, 이것이 말뭉치가 인식 정확도를 향상시키는 보다 효과적인 방법인 이유입니다.

단어 리소스가 크면 음성 인식의 대기 시간이 증가할 수 있지만 정확한 영향을 수량화하거나 예측하기 어렵습니다. 효과적인 사용자 정의 모델을 생성하는 데 필요한 데이터의 양과 마찬가지로 대형 단어 리소스의 성능 영향은 여러 요인에 따라 달라집니다. 다양한 양의 데이터로 사용자 정의 모델을 테스트하여 모델 및 데이터의 성능을 판별하십시오.

## 말뭉치에 대한 작업
{: #workingCorpora}

`POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드를 사용하여 사용자 정의 모델에 말뭉치를 추가할 수 있습니다. 말뭉치는 도메인의 샘플 문장을 포함하는 일반 텍스트 파일입니다. 다음 예제는 의료 도메인에 대한 축약된 말뭉치를 보여줍니다. 말뭉치 파일은 일반적으로 훨씬 더 깁니다.

```
Am I at risk for health problems during travel?
Some people are more likely to have health problems when traveling outside the United States.
How Is Coronary Microvascular Disease Treated?
If you're diagnosed with coronary MVD and also have anemia, you may benefit from treatment for that condition.
Anemia is thought to slow the growth of cells needed to repair damaged blood vessels.
What causes autoimmune hepatitis?
A combination of autoimmunity, environmental triggers, and a genetic predisposition can lead to autoimmune hepatitis.
What research is being done for Spinal Cord Injury?
The National Institute of Neurological Disorders and Stroke NINDS conducts spinal cord research in its laboratories at the National Institutes of Health NIH.
NINDS also supports additional research through grants to major research institutions across the country.
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

음성 인식은 통계 알고리즘에 의존하여 오디오를 분석합니다. 사용자 정의 모델의 단어는 해당 모델의 다른 단어만이 아니라 서비스의 기본 어휘에 있는 단어와도 경쟁합니다. (오디오 잡음 및 화자의 악양과 같은 요인도 변환 품질에 영향을 줍니다.)

변환 정확도는 단어가 모델에서 정의된 방법과 화자가 말하는 방식에 따라 크게 달라질 수 있습니다. 서비스의 정확도를 높이려면 말뭉치를 사용하여 OOV 단어가 도메인에서 사용되는 방법에 대한 가능한 많은 예제를 제공하십시오. 말뭉치의 OOV 단어를 반복하면 사용자 정의 언어 모델의 품질이 향상될 수 있습니다. 말뭉치의 단어를 복제하는 방법은 인식될 오디오에서 사용자가 단어를 말하는 방식에 따라 다릅니다. 화자가 도메인의 단어를 사용하는 컨텍스트를 나타내는 문장을 더 많이 추가할수록 서비스의 인식 정확도가 더 높아집니다.

이 서비스는 단순 단어 일치 알고리즘을 적용하지 않습니다. 변환은 단어가 사용되는 컨텍스트에 따라 달라집니다. 말뭉치를 구문 분석하면 이 서비스에 사용자 정의 모델에 있는 말뭉치 문장의 n-그램(바이그램, 트라이그램 등)에 대한 정보가 포함됩니다. 이 정보는 서비스가 오디오를 더 정확하게 변환하는 데 도움이 되며 사용자 정의 모델을 말뭉치에 대해 훈련하는 것이 사용자 정의 단어만으로 훈련하는 것보다 더 가치 있는 이유를 설명합니다.

예를 들어, 회계사는 GAAP(Generally Accepted Accounting Principles)라고도 하는 공통 표준 및 프로시저 세트를 준수합니다. 금융 도메인에 대한 사용자 정의 모델을 작성할 때 컨텍스트에서 GAAP라는 용어를 사용하는 문장을 제공하십시오. 이 문장은 서비스가 "the gap between them is small"과 같은 일반 구문과 "GAAP provides guidelines for measuring and disclosing financial information"과 같은 도메인 특정 구문을 구별하는 데 도움이 됩니다.

일반적으로 말뭉치는 다양한 컨텍스트의 단어를 사용하는 것이 더 좋으며, 이는 서비스가 단어를 학습하는 방법을 개선할 수 있습니다. 그러나 사용자의 몇 개의 컨텍스트로만 단어를 말하면 다른 컨텍스트에서 단어를 표시해도 사용자 정의 모델의 품질이 향상되지 않습니다. 화자가 이러한 컨텍스트에서 단어를 사용하지 않습니다. 화자가 동일한 구문을 자주 사용하는 경우 말뭉치에서 해당 구문을 반복하면 모델의 품질이 향상될 수 있습니다. (일부 경우에 몇 개의 사용자 정의 단어를 사용자 정의 모델에 직접 추가해도 긍정적인 효과를 얻을 수 있습니다.)

### 말뭉치 텍스트 파일 준비
{: #prepareCorpus}

말뭉치 텍스트 파일을 준비하려면 다음 가이드라인을 따르십시오.

-   ASCII가 아닌 문자가 포함된 경우 UTF-8로 인코딩된 일반 텍스트 파일을 제공하십시오. 이러한 문자가 발생하면 서비스에서 UTF-8 인코딩으로 간주합니다.

    말뭉치 텍스트 파일의 문자 인코딩을 알고 있는지 확인하십시오. 이 서비스는 텍스트 파일에서 찾은 인코딩을 유지합니다. 사용자 정의 언어 모델의 단어에 대해 작업할 때 해당 인코딩을 사용해야 합니다. 자세한 정보는 [문자 인코딩](#charEncoding)을 참조하십시오.
    {: important}
-   말뭉치의 단어에 대해 일관된 대문자 표시를 사용하십시오. 단어 리소스는 대소문자를 구분합니다. 대문자와 소문자를 혼합하고 의도한 경우에만 대문자 표시를 사용하십시오.
-   말뭉치의 각 문장을 자체 행에 포함시키고 각 행을 캐리지 리턴으로 종료하십시오. 여러 문장을 동일한 행에 포함시키면 정확도가 떨어질 수 있습니다.
-   개인 이름은 별도의 행에 개별 단위로 추가하십시오. 별도의 행에나 개별 사용자 정의 단어로 이름의 단어를 추가하지 말고 동일한 말뭉치 행에 여러 이름을 포함하지 마십시오. 다음 예제는 세 개의 이름에 대한 인식 정확도를 향상시키는 올바른 방법을 보여줍니다.

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    사용 가능한 경우 추가 컨텍스트 정보를 포함하십시오(예: `Doctor Sebastian Leifson` 또는 `President Malcolm Ingersol`). 모든 단어와 마찬가지로, 가능한 경우 여러 컨텍스트에서 여러 번 이름을 복제하면 인식 정확도가 향상될 수 있습니다.
-   철자 오류에 주의하십시오. 이 서비스는 철자 오류를 새 단어로 간주합니다. 모델을 훈련하기 전에 오류를 정정하지 않으면 서비스가 모델의 어휘에 추가합니다. *쓰레기가 들어가면 쓰레기가 나온다!*라는 격언을 기억하십시오.

문장이 많을수록 정확도가 더 향상됩니다. 그러나 이 서비스에서는 모든 소스를 합쳐서 최대 총 천만 개의 단어와 9만 개의 OOV 단어로 모델을 제한합니다.

### 말뭉치 파일을 추가하면 어떻게 됩니까?
{: #parseCorpus}

말뭉치 파일을 추가하면 서비스가 파일의 컨텐츠를 분석합니다. 또한 발견한 새 OOV 단어를 추출하고 각 OOV 단어를 사용자 정의 모델의 단어 리소스에 추가합니다. 서비스가 컨텐츠에서 가장 많은 의미를 추출하기 위해 말뭉치 파일에서 읽은 데이터를 토큰화하고 구문 분석합니다. 다음 섹션에서는 서비스가 각각의 지원되는 언어에 대한 말뭉치 파일을 구문 분석하는 방법에 대해 설명합니다.

#### 영어, 프랑스어, 독일어, 스페인어 및 브라질 포르투갈어 구문 분석
{: #corpusLanguages}

다음 설명은 미국 및 영국 영어, 프랑스어, 독일어, 스페인어 및 브라질 포트투갈어에 적용됩니다.

-   숫자를 동등한 단어로 변환합니다. 예를 들어, 다음과 같습니다.
    -   *영어의 경우 * `500`은 `five hundred`가 되고 `0.15`는 `zero point fifteen`이 됩니다.
    -   *프랑스어의 경우 * `500`은 `cinq cents`이 되고 `0,15`는 <code>z&eacute;ro virgule quinze</code>가 됩니다.
    -   *독일어의 경우 * `500`은 <code>f&uuml;nfhundert</code>가 되고 `0,15`는 <code>null punkt f&uuml;nfzehn</code>이 됩니다.
    -   *스페인어의 경우 * `500`은 `quinientos`가 되고 `0,15`는 `cero coma quince`가 됩니다.
    -   *브라질 포르투갈어의 경우 * `500`은 `quinhentos`가 되고 `0,15`는 `zero ponto quinze`가 됩니다.
-   특정 기호를 포함하는 토큰을 의미 있는 문자열 표시로 변환합니다. 예를 들어, 다음과 같습니다.
    -   `$`(달러 기호) 및 숫자를 변환합니다.
        -   *영어의 경우 * `$100`는 `one hundred dollars`가 됩니다.
        -   *프랑스어의 경우 * `$100`는 `cent dollars`가 됩니다.
        -   *독일어의 경우 * `$100` 및 `100$`는 `einhundert dollar`가 됩니다.
        -   *스페인어의 경우 * `$100` 및 `100$`는 <code>cien d&oacute;lares</code>(또는 통용어가 `es-LA`인 경우 `cien pesos`)가 됩니다.
        -   *브라질 포르투갈어의 경우* `$100` 및 `100$`는 <code>cem d&oacute;lares</code>가 됩니다.
    -   <code>&euro;</code>(유로 기호) 및 숫자를 변환합니다.
        -   *영어의 경우* <code>&euro;100</code>는 `one hundred euros`가 됩니다.
        -   *프랑스어의 경우* <code>&euro;100</code>는 `cent euros`가 됩니다.
        -   *독일어의 경우* <code>&euro;100</code> 및 <code>100&euro;</code>는 `einhundert euro`가 됩니다.
        -   *스페인어의 경우 * <code>&euro;100</code> 및 <code>100&euro;</code>는 `cien euros`가 됩니다.
        -   *브라질 포르투갈어의 경우* <code>&euro;100</code> 및 <code>100&euro;</code>는 `cem euros`가 됩니다.
    -   숫자가 앞에 오는 `%`(퍼센트 기호)를 변환합니다.
        -   *영어의 경우* `100%`는 `one hundred percent`가 됩니다.
        -   *프랑스어의 경우* `100%` 는 `cent pour cent`가 됩니다.
        -   *독일어의 경우* `100%`는 `einhundert prozent`가 됩니다.
        -   *스페인어의 경우* `100%`는 `cien por ciento`가 됩니다.
        -   *브라질 포르투갈어의 경우* `100%`는 `cem por cento`가 됩니다.

      이 목록은 완전하지 않습니다. 이 서비스는 필요에 따라 다른 문자를 유사하게 조정합니다.
-   해당 컨텍스트에 따라 영숫자가 아닌 문자, 문장 부호 및 특수 문자를 처리합니다. 예를 들어,`$`(달러 기호) 또는 <code>&euro;</code>(유로 기호) 다음에 숫자가 오지 않으면 이 서비스가 이러한 기호를 제거합니다. 처리는 컨텍스트에 따라 다르며 지원되는 언어에서 일관됩니다.
-   `( )`(소괄호), `< >`(꺾쇠괄호), `[ ]`(대괄호) 또는 `{ }`(중괄호)로 묶인 구문을 무시합니다.

#### 일본어 구문 분석
{: #corpusLanguages-jaJP}

-   모든 문자를 전자 문자로 변환합니다.
-   숫자를 동등한 단어로 변환합니다. 예를 들어, `500`은 <code>&#20116;&#30334;</code>이 되고 `0.15`는 <code>&#12295;&#12539;&#19968;&#20116;</code>가 됩니다.
-   기호를 포함하는 토큰을 동등한 문자열로 변환하지 않습니다. 예를 들어, `100%`는 <code>&#30334;&#65285;</code>가 됩니다.
-   문장 부호를 자동으로 제거하지 않습니다. 애플리케이션이 받아쓰기 기반이 아니라 변환 기반인 경우 {{site.data.keyword.IBM_notm}}에서는 문장 부호를 제거하도록 적극 권장합니다.

#### 한국어 구문 분석
{: #corpusLanguages-koKR}

-   숫자를 동등한 단어로 변환합니다. 예를 들어, <code>10</code>은 <code>&#49901;</code>이 됩니다.
-   다음 문장 부호 및 특수 문자를 제거합니다. `- ( ) * : . , ' "`. 그러나 다른 언어의 경우에 제거되는 모든 문장 부호 및 특수 문자가 한국어에서 제거되는 것은 아닙니다. 예를 들어, 다음과 같습니다.
    -   마침표(`.`) 기호는 입력 행의 끝에 발생하는 경우에만 제거합니다.
    -   물결 기호(`~`)를 제거하지 않습니다.
    -   유니코드 와이드 문자 기호(예: <code>&#8230;</code>(3중점 또는 생략 기호))를 제거하거나 처리하지 않습니다.

    일반적으로 {{site.data.keyword.IBM_notm}}에서는 말뭉치 파일을 처리하기 전에 문장 부호, 특수 문자 및 유니코드 와이드 문자를 제거하도록 권장합니다.
-   `( )`(소괄호), `< >`(꺾쇠괄호), `[ ]`(대괄호) 또는 `{ }`(중괄호)로 묶인 구문을 제거하거나 무시하지 않습니다.
-   특정 기호를 포함하는 토큰을 의미 있는 문자열 표시로 변환합니다. 예를 들어, 다음과 같습니다.
    -   `24%`는 <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>가 됩니다.
    -   `$10`는 <code>&#49901;&#45804;&#47084;</code>가 됩니다.

      이 목록은 완전하지 않습니다. 이 서비스는 필요에 따라 다른 문자를 유사하게 조정합니다.
-   라틴(영어) 문자로 구성되거나 한글과 라틴 문자가 혼합되어 구성된 구문의 경우 서비스가 말뭉치 파일에 표시되는 대로 정확히 구문에 대한 OOV 단어를 작성합니다. 또한 한글 변환을 기반으로 하는 단어에 대한 유사 발음을 작성합니다.
   - OOV 단어 `London`에 <code>&#47088;&#45912;</code>이라는 동음어를 지정합니다.
   - OOV 단어 <code>IBM&#54856;&#54168;&#51060;&#51648;</code>에 <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>라는 동음어를 지정합니다.

## 사용자 정의 단어에 대한 작업
{: #workingWords}

`POST /v1/customizations/{customization_id}/words` 및 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 사용자 정의 모델의 단어 리소스에 새 단어를 추가할 수 있습니다. 이러한 메소드를 사용하여 단어 리소스의 단어를 수정하거나 기능 보강할 수도 있습니다.

예를 들어, 이러한 메소드를 사용하여 말뭉치에서 단어가 추가될 때 발생한 철자 오류 또는 기타 실수를 정정해야 할 수 있습니다. 기존 단어에 대해 동음어 정의를 추가해야 할 수도 있습니다. 기존 단어를 수정하는 경우 사용자가 제공하는 새 데이터가 단어 리소스에 있는 단어의 기존 정의를 겹쳐씁니다. 단어 추가 규칙은 기존 단어를 수정할 때도 적용됩니다.

### 문자 인코딩
{: #charEncoding}

일반적으로 대부분의 사용자 정의 단어를 말뭉치에서 추가합니다. 말뭉치의 텍스트 파일에서 사용되는 문자 인코딩을 알고 있는지 확인하십시오. 이 서비스는 텍스트 파일에서 찾은 인코딩을 유지합니다.

사용자 정의 언어 모델의 개별 단어에 대해 작업할 때 해당 인코딩을 사용해야 합니다. `GET`, `PUT` 또는 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 단어를 지정할 때 단어에 ACII가 아닌 문자가 포함된 경우 URL에 전달하는 `word_name`을 URL로 인코딩해야 합니다.

예를 들어, 다음 표는 동일한 문자가 두 개의 다른 인코딩, ASCII 및 UTF-8에서 어떻게 표시되는지를 보여줍니다. URL의 ASCII 문자를 `z`로 전달할 수 있습니다. UTF-8 문자를 `%EF%BD%9A`로 전달해야 합니다.

<table style="width:75%">
  <caption>표 1. 문자 인코딩 예제</caption>
  <tr>
    <th style="width:15%; text-align:center">문자</th>
    <th style="width:40%; text-align:center">인코딩</th>
    <th style="width:45%; text-align:center">값</th>
  </tr>
  <tr>
    <td style="text-align:center">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a`(`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 16진
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A`(`efbd9a`)
    </td>
  </tr>
</table>

### sounds_like 필드 사용
{: #soundsLike}

`sounds_like` 필드는 화자가 단어를 어떻게 발음하는지를 지정합니다. 기본적으로 이 서비스는 단어의 맞춤법을 사용하여 필드를 자동으로 완성합니다. 발음하기 어렵거나 여러 방식으로 발음할 수 있는 단어에 대해 5개의 대체 발음을 제공할 수 있습니다. 이 필드를 사용하여 다음을 수행하십시오.

-   *약어에 대한 여러 발음을 제공합니다.* 예를 들어, `NCAA`라는 약어는 맞춤법대로 발음되거나 *N. C. double A*와 같이 구어체로 발음될 수 있습니다. 다음 예제에서는 `NCAA`라는 단어에 대한 이러한 두 가지 유사 발음을 추가합니다.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *외국어 단어를 처리합니다.* 예를 들어, 프랑스어 단어 <code>gar&ccedil;on</code>에는 영어에서 찾을 수 없는 문자가 포함되어 있습니다. <code>&ccedil;</code>를 `s`로 대체하여 `gaarson`이라는 동음어를 지정함으로써 영어 화자가 이 단어를 발음하는 방법을 서비스에 알릴 수 있습니다.

음성 인식은 통계 알고리즘을 사용하여 오디오를 분석하므로 단어를 추가한다고 해서 서비스가 완전한 정확도로 단어를 트랜스코딩하는 것은 아닙니다. 단어를 추가할 때 해당 단어가 발음될 수 있는 방법을 고려하십시오. `sounds_like` 필드를 사용하여 단어가 어떻게 발음되는지를 반영하는 다양한 발음을 제공하십시오. 다음 섹션에서는 유사 발음을 지정하기 위한 언어별 가이드라인을 제공합니다.

#### 영어(미국 및 영국)에 대한 가이드라인
{: #wordLanguages-enUS-enGB}

*미국 영어 및 영국 영어 모두에 대한 가이드라인:*

-   영어 알파벳 문자(`a-z` 및 `A-Z`)를 사용하십시오.
-   발음하기 어려운 단어의 경우 영어로 발음 가능한 실제 단어 또는 조어를 사용하십시오(예: `Sczcesny`라는 단어의 경우 `shuchesnie`).
-   영어가 아닌 문자는 동등한 영어 문자로 대체하십시오(예: <code>&ccedil;</code>는 `s`로, <code>&ntilde;</code>는 `ny`로).
-   강세 기호가 있는 문자는 강세 기호가 없는 문자로 대체하십시오(예: <code>&agrave;</code>는 `a`로, <code>&egrave;</code>는 `e`로).
-   공백으로 구분된 여러 단어를 포함할 수 있지만 이 서비스에서는 공백을 포함하지 않고 최대 총 40자를 적용합니다.

*미국 영어에만 적용되는 가이드라인:*

-   하나의 문자를 발음하려면 문자 다음에 마침표를 사용하십시오. 마침표 다음에 다른 문자가 오는 경우 마침표와 다음 문자 사이에 공백을 사용해야 합니다. 예를 들어, `N.C.A.A.`가 *아니라* `N. C. A. A.`를 사용하십시오.
-   숫자는 문자로 풀어 사용하십시오(예: `75`의 경우 `seventy-five`).

*영국 영어에만 적용되는 가이드라인:*

-   영국 영어의 경우 마침표 또는 대시를 유사 발음에 사용할 수 **없습니다**.
-   하나의 문자를 발음하려면 문자 다음에 공백을 사용하십시오. 예를 들어, `N. C. A. A.`, `N.C.A.A.` 또는 `NCAA`가 *아니라* `N C A A`를 사용하십시오.
-   숫자는 대시 없이 문자로 풀어 사용하십시오(예: `75`의 경우 `seventy five`).

#### 프랑스어, 독일어, 스페인어 및 브라질 포르투갈어에 대한 가이드라인
{: #wordLanguages-esES-frFR}

-   유사 발음에서 대시를 사용할 수 **없습니다**.
-   유효한 강세 표시가 있는 문자를 포함하여 언어에 유효한 알파벳 문자(`a-z` 및 `A-Z`)를 사용하십시오.
-   하나의 문자를 발음하려면 문자 다음에 마침표를 사용하십시오. 마침표 다음에 다른 문자가 오는 경우 마침표와 다음 문자 사이에 공백을 사용해야 합니다. 예를 들어, `N.C.A.A.`가 *아니라* `N. C. A. A.`를 사용하십시오.
-   발음하기 어려운 단어의 경우 해당 언어로 발음 가능한 실제 단어 또는 조어를 사용하십시오.
-   숫자는 대시 없이 문자로 풀어 사용하십시오. 예를 들어, `75`의 경우 다음과 같이 사용하십시오.
    -   *프랑스어:* `soixante quinze`
    -   *독일어:* <code>f&uuml;nfundsiebzig</code>
    -   *스페인어:* `setenta y cinco`
    -   *브라질 포르투갈어:* `setenta e cinco`
-   공백으로 구분된 여러 단어를 포함할 수 있지만 이 서비스에서는 공백을 포함하지 않고 최대 총 40자를 적용합니다.

#### 일본어에 대한 가이드라인
{: #wordLanguages-jaJP}

-   <code>&#8213;</code> 장음 기호(*chou-on* 또는 일본어로 &#38263;&#38899;)를 사용하여 전자 가타카나 문자만 사용하십시오. 반자 문자는 사용하지 마십시오.
-   다음과 같은 음절 컨텍스트에서만 약음(*yoh-on* 또는 일본어로 &#25303;&#38899;)을 사용하십시오.

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   촉음(*soku-on* 또는 일본어로 &#20419;&#38899;) 다음에는 다음과 같은 음절만 사용하십시오.

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   <code>&#12531;</code>을 단어의 첫 번째 문자로 사용자지 마십시오. 예를 들어, <code>&#12531;&#12540;&#12488;</code> 대신 <code>&#12454;&#12540;&#12531;&#12488;</code>를 사용하십시오. 전자는 올바르지 않습니다.
-   많은 복합어는 *접두부+명사* 또는 *명사+접미부*로 구성됩니다. 이 서비스의 기본 어휘에는 자주 발생하는 대부분의 복합어(예: <code>&#x9577;&#x96FB;&#x8A71;</code> 및 <code>&#x53E4;&#x65B0;&#x805E;</code>)가 포함되지만 드물게 발생하는 복합어는 포함되어 있지 않습니다. 일반적으로 말뭉치에 복합어가 포함되어 있는 경우 사용자 정의의 첫 단계로 해당 복합어를 하나의 단어로 추가하십시오. 예를 들어, <code>&#x53E4;&#x925B;&#x7B46;</code>은 일반 일본어 텍스트에서 일반적이지 않습니다. 이 단어를 자주 사용하는 경우 사용자 정의 모델에 추가하여 변환 정확도를 높이십시오.
-   후행 촉음을 사용하지 마십시오.

#### 한국어에 대한 가이드라인
{: #wordLanguages-koKR}

-   한글 문자, 기호 및 음절을 사용하십시오.
-   라틴(영어) 알파벳 문자(`a-z` 및 `A-Z`)를 사용할 수도 있습니다.
-   이전 세트에 포함되지 않은 문자 또는 기호는 사용하지 마십시오.

### display_as 필드 사용
{: #displayAs}

`display_as` 필드는 단어가 음성 내용에 표시되는 방법을 지정합니다. 이 필드는 서비스가 단어의 맞춤법과 다른 문자열을 표시하도록 하려는 경우를 위한 것입니다. 예를 들어, `hhonors`라는 단어가 `hilton honors` 또는 `h honors`처럼 들리는지에 관계없이 `HHonors`로 표시되도록 지정할 수 있습니다.

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

또 다른 예로 `IBM`이라는 단어가 <code>IBM&trade;</code>으로 표시되도록 지정할 수 있습니다.

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### 스마트 형식화 및 숫자 교정과의 상호작용
{: #displaySmart}

`smart_formatting` 또는 `redaction` 매개변수를 인식 요청에 사용하는 경우 이 서비스가 단어에 대한 `display_as` 필드를 고려하기 전에 단어에 스마트 형식화 및 교정을 적용한다는 점에 유의하십시오. 결과를 시험하여 이 기능이 사용자 정의 단어가 표시되는 방식을 방해하지 않는지 확인해야 할 수 있습니다. 효과를 얻으려면 사용자 정의 단어를 추가해야 할 수도 있습니다.

예를 들어, `display_as` 필드가 `one`인 사용자 정의 단어 `one`을 추가한다고 가정하십시오. 스마트 형식화는 `one`이라는 단어를 숫자 `1`로 변경하며 display-as 값은 적용되지 않습니다. 이 문제를 해결하려면 숫자 `1`에 대한 사용자 정의 단어를 추가하고 동일한 `display_as` 필드를 해당 단어에 적용할 수 있습니다.

이러한 기능을 사용한 작업에 대한 자세한 정보는 [스마트 형식화](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting) 및 [숫자 교정](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)을 참조하십시오.

### 사용자 정의 단어를 추가하거나 수정하면 어떻게 됩니까?
{: #parseWord}

서비스가 사용자 정의 단어 추가 또는 수정 요청에 응답하는 방법은 사용자가 제공하는 값과 해당 단어가 서비스의 기본 어휘에 존재하는지 여부에 따라 달라집니다.

<table>
  <caption>표 1. 다른 필드가 있는 사용자 정의 단어 추가</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>sounds_like</code> 필드 지정 여부</th>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>display_as</code> 필드 지정 여부</th>
    <th style="text-align:left; vertical-align:bottom">응답</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      지정되지 않음
    </td>
    <td style="text-align:center; vertical-align:top">
      지정되지 않음
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>단어가 서비스의 기본 어휘에 없는 경우</em> 서비스가 <code>sounds_like</code> 필드를 단어의 발음으로 설정하고 <code>display_as</code> 필드를 <code>word</code> 필드의 값으로 설정합니다.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>단어가 서비스의 기본 어휘에 있는 경우</em> 서비스가 <code>sounds_like</code> 및 <code>display_as</code> 필드를 비워 둡니다. 이러한 필드는 단어가 서비스의 기본 어휘에 있는 경우에만 비어 있습니다. 해당 단어가 모델의 단어 리소스에 존재해도 무관하지만 이는 불필요합니다.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      지정됨
    </td>
    <td style="text-align:center; vertical-align:top">
      지정되지 않음
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> 필드가 유효한 경우</em> 서비스가 <code>display_as</code> 필드를 <code>word</code> 필드의 값으로 설정합니다.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> 필드가 유효하지 않은 경우:</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              <code>POST /v1/customizations/{customization_id}/words</code> 메소드가 모델의 단어 리소스에 있는 단어에 <code>error</code> 필드를 추가합니다.
            </li>
            <li style="margin:10px 0px; line-height:120%">
              <code>PUT /v1/customizations/{customization_id}/words/{word_name}</code> 메소드가 400 응답 코드 및 오류 메시지와 함께 실패합니다. 서비스가 단어를 단어 리소스에 추가하지 않습니다.
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      지정되지 않음
    </td>
    <td style="text-align:center; vertical-align:top">
      지정됨
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>단어가 서비스의 기본 어휘에 없는 경우</em> 서비스가 <code>sounds_like</code> 필드를 단어의 발음으로 설정하고 <code>display_as</code> 필드를 지정된 대로 남겨 둡니다.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>단어가 서비스의 기본 어휘에 있는 경우</em> 서비스가 <code>sounds_like</code>를 비워 두고 <code>display_as</code> 필드를 지정된 대로 남겨 둡니다.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      지정됨
    </td>
    <td style="text-align:center; vertical-align:top">
      지정됨
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> 필드가 유효한 경우</em> 서비스가 <code>sounds_like</code> 및 <code>display_as</code> 필드를 지정된 값으로 설정합니다.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> 필드가 유효하지 않은 경우</em> 서비스는 <code>sounds_like</code> 필드가 지정되었지만 <code>display_as</code> 필드는 지정되지 않은 경우와 같이 응답합니다.
        </li>
      </ul>
    </td>
  </tr>
</table>

## 단어 리소스 유효성 검증
{: #validateModel}

특히 말뭉치를 사용자 정의 언어 모델에 추가하거나 여러 사용자 정의 단어를 한 번에 추가하는 경우 모델의 오디오 리소스에 있는 OOV 단어를 검사하십시오.

-   *철자 오류 및 기타 오류를 찾으십시오.* 특히 크기가 클 수 있는 말뭉치를 추가할 때 쉽게 실수할 수 있습니다. 말뭉치(또는 문법) 파일의 철자 오류는 말뭉치 파일에 남아 있는 잘못된 형식의 HTML 태그와 같이 모델의 단어 리소스에 새 단어를 추가하는 의도하지 않은 결과를 초래합니다.
-   *유사 발음을 확인하십시오. * 이 서비스는 OOV 단어에 대한 유사 발음을 자동으로 생성합니다. 대부분의 경우 이러한 발음으로 충분합니다. 그러나 맞춤법이 비정상적이거나 발음하기 어려운 단어와 약어 및 기술 용어의 경우 정확성을 위해 발음을 검토하는 것이 좋습니다.

사용자 정의 모델에 대한 단어의 유효성을 검증하고 필요한 경우 정정하려면 단어가 단어 리소스에 추가된 방법과 관계없이 다음 메소드를 사용하십시오.

-   `GET /v1/customizations/{customization_id}/words` 메소드를 사용하여 사용자 정의 모델의 모든 단어를 나열하거나 `GET /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 개별 단어를 조회하십시오. 자세한 정보는 [사용자 정의 언어 모델의 단어 나열](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)을 참조하십시오.
-   `POST /v1/customizations/{customization_id}/words` 또는 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드를 통해 사용자 정의 모델의 단어를 수정하여 오류를 정정하거나 sounds-like 또는 display-as 값을 추가하십시오. 자세한 정보는 [사용자 정의 단어에 대한 작업](#workingWords)을 참조하십시오.
-   `DELETE /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 오류로 인해(예를 들어, 말뭉치의 철자 또는 기타 실수로) 도입된 불필요한 단어를 삭제합니다. 자세한 정보는 [사용자 정의 언어 모델에서 단어 삭제](/docs/services/speech-to-text?topic=speech-to-text-manageWords#deleteWord)를 참조하십시오.
    -   단어가 말뭉치에서 추출된 경우 대신 말뭉치 텍스트 파일을 업데이트하여 오류를 정정한 후 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드의 `allow_overwrite` 매개변수를 사용하여 파일을 다시 로드할 수 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 말뭉치 추가](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)를 참조하십시오.
    -   단어가 문법에서 추출된 경우 문법 파일을 업데이트하여 오류를 정정한 후 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드의 `allow_overwrite` 매개변수를 사용하여 파일을 다시 로드할 수 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 문법 추가](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)를 참조하십시오.
