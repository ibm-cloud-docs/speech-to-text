---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# 사용자 정의 인터페이스
{: #customization}

{{site.data.keyword.speechtotextfull}} 서비스는 음성 인식 기능을 보강하는 데 사용할 수 있는 사용자 정의 인터페이스를 제공합니다. 사용자 정의를 사용하면 도메인 및 오디오의 기본 모델을 사용자 정의하여 음성 인식 요청의 정확도를 높일 수 있습니다. 사용자 정의는 일부 언어에만 사용할 수 있으며 언어마다 지원 레벨이 서로 다릅니다. [사용자 정의에 대한 언어 지원](#languageSupport)을 참조하십시오.
{: shortdesc}

사용자 정의 인터페이스는 사용자 정의 언어 모델과 사용자 정의 음향 모델을 모두 지원합니다. 두 가지 유형의 사용자 정의 모델에 대한 인터페이스는 유사하며 사용하기 쉽습니다. 두 유형 중 하나의 사용자 정의 모델을 인식 요청에 사용하는 것도 간단합니다. 요청과 함께 모델의 사용자 정의 ID를 지정합니다.

음성 인식은 사용자 정의 모델이 있는지 여부와 관계없이 동일하게 작동합니다. 음성 인식에 사용자 정의 모델을 사용하는 경우 일반적으로 인식 요청에 사용 가능한 모든 입력 및 출력 매개변수를 사용할 수 있습니다. 사용 가능한 모든 매개변수에 대한 자세한 정보는 [매개변수 요약](/docs/services/speech-to-text?topic=speech-to-text-summary)을 참조하십시오.

언어 모델 또는 음향 모델 사용자 정의를 사용하려면 Standard 가격 플랜이 필요합니다. Lite 플랜 사용자는 사용자 정의 인터페이스를 사용할 수 없습니다. 자세한 정보는 {{site.data.keyword.speechtotextshort}} 서비스에 대한 [가격 페이지](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}를 참조하십시오.
{: note}

## 언어 모델 사용자 정의
{: #customLanguage-intro}

이 서비스는 광범위한 일반 대상을 염두에 두고 개발되었습니다. 이 서비스의 기본 어휘에는 일상적인 대화에서 사용되는 많은 단어가 포함되어 있습니다. 이 서비스의 모델은 다양한 애플리케이션에 대해 충분히 정확한 인식을 제공하지만 특정 도메인과 연관된 특정 용어에 대한 지식은 부족할 수 있습니다.

*언어 모델 사용자 정의* 인터페이스는 의학, 법률, 정보 기술 등과 같은 도메인에 대한 음성 인식의 정확도를 향상시킬 수 있습니다. 언어 모델 사용자 정의를 사용하여 기본 모델의 어휘를 확장하고 도메인 특정 용어를 포함하도록 사용자 조정할 수 있습니다.

사용자 정의 언어 모델을 작성하고 도메인에 특정한 말뭉치 및 단어를 추가합니다. 향상된 어휘에 대해 사용자 정의 언어 모델을 훈련한 후 사용자 정의된 음성 인식에 사용할 수 있습니다. 일반적으로 서비스는 몇 분 내에 사용자 정의 모델을 훈련할 수 있습니다. 모델을 작성하는 데 필요한 노력의 레벨은 모델에 사용 가능한 데이터에 따라 다릅니다.

자세한 정보는 다음을 참조하십시오.

-   [사용자 정의 언어 모델 작성](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
-   [사용자 정의 언어 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-languageUse)

## 음향 모델 사용자 정의
{: #customAcoustic-intro}

마찬가지로, 서비스는 다양한 오디오 특성에 대해 잘 작동하는 기본 음향 모델로 개발되었습니다. 그러나 다음과 같은 경우에 기본 모델을 오디오에 맞게 조정하면 음성 인식이 개선될 수 있습니다.

-   음향 채널 환경이 고유합니다. 예를 들어, 환경에 소음이 많거나, 마이크 품질 또는 위치가 최적이 아니거나, 오디오에 원거리 효과가 발생했습니다.
-   화자의 음성 패턴이 비정상적입니다. 예를 들어, 화가자 비정상적으로 빠르게 말하거나 오디오에 일상적인 대화가 포함되어 있습니다.
-   화자의 억양이 발음되었습니다. 예를 들어, 자국어가 아닌 언어 또는 제2언어로 말하는 화자가 오디오에 포함되어 있습니다.

*음향 모델 사용자 정의* 인터페이스는 사용자 환경 및 화자에 맞게 기본 모델을 조정할 수 있습니다. 사용자 정의 음향 모델을 작성하고 변환할 오디오의 음향 서명과 밀접하게 일치하는 오디오 데이터(오디오 리소스)를 추가합니다. 오디오 리소스로 사용자 정의 음향 모델을 훈련한 후 사용자 정의된 음성 인식에 사용할 수 있습니다.

서비스가 사용자 정의 모델을 훈련하는 데 걸리는 기간은 모델에 포함된 데이터의 양에 따라 다릅니다. 일반적으로 훈련에는 누적 오디오 길이의 두 배가 걸립니다. 모델의 작성하는 데 필요한 노력의 레벨은 모델에 사용 가능한 오디오 데이터에 따라 다릅니다. 또한 오디오의 변환을 사용하는지 여부에 따라 달라집니다.

자세한 정보는 다음을 참조하십시오.

-   [사용자 정의 음향 모델 작성](/docs/services/speech-to-text?topic=speech-to-text-acoustic)
-   [사용자 정의 음향 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## 문법
{: #grammars-intro}

사용자 정의 언어 모델을 사용하여 서비스의 기본 어휘를 확장할 수 있습니다. *문법*을 통해 서비스가 해당 어휘에서 인식할 수 있는 단어를 제한할 수 있습니다. 문법을 사용자 정의 언어 모델과 함께 음성 인식에 사용하는 경우 서비스가 문법에서 인식되는 단어, 구문 및 문자열만 인식할 수 있습니다. 문법은 올바른 일치사항에 대한 제한된 검색 공간을 정의하므로 서비스가 결과를 더 빠르고 정확하게 전달할 수 있습니다.

말뭉치에 대해 수행한 것과 마찬가지로 문법을 사용자 정의 언어 모델에 추가하고 모델을 훈련합니다. 그러나 말뭉치와 달리 문법이 음성 인식 중에 사용자 정의 모델에 사용되도록 명시적으로 지정해야 합니다.

자세한 정보는 다음을 참조하십시오.

-   [사용자 정의 언어 모델에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammars)
-   [사용자 정의 언어 모델에 문법 추가](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
-   [음성 인식에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)

## 음향 및 언어 사용자 정의를 함께 사용
{: #combined}

사용자 정의 음향 모델을 단독으로 사용하면 서비스의 인식 기능이 개선될 수 있습니다. 그러나 변환 또는 관련 말뭉치를 예제 오디오에 사용할 수 있는 경우 해당 데이터를 사용하여 사용자 정의 음향 모델을 기반으로 하는 음성 인식의 품질을 더 향상시킬 수 있습니다.

사용자 정의 음향 모델을 보완하는 사용자 정의 언어 모델을 작성하면 두 모델을 함께 사용하여 음성 인식을 향상시킬 수 있습니다. 사용자 정의 음향 모델을 훈련할 때 오디오 리소스 또는 해당 리소스에 있는 도메인 특정 단어 어휘의 변환이 포함된 사용자 정의 언어 모델을 지정할 수 있습니다. 마찬가지로, 오디오를 변환할 때 서비스가 사용자 정의 언어 모델, 사용자 정의 음향 모델 또는 둘 다를 허용합니다. 또한 사용자 정의 언어 모델에 문법이 포함되어 있는 경우 음성 인식을 위해 해당 모델 및 문법을 사용자 정의 음향 모델과 함께 사용할 수 있습니다.

자세한 정보는 [사용자 정의 음향 및 사용자 정의 언어 모델을 함께 사용](/docs/services/speech-to-text?topic=speech-to-text-useBoth)을 참조하십시오.

일부 언어는 언어 및 음향 사용자 정의를 모두 지원하지 않습니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](#languageSupport)을 참조하십시오.
{: note}

## 사용자 정의에 대한 언어 지원
{: #languageSupport}

언어 및 음향 모델 사용자 정의는 일부 언어에만 사용 가능합니다. 다음 표에는 서비스가 각 언어에 대한 사용자 정의를 지원하는 레벨이 문서화되어 있습니다.

-   *GA*는 인터페이스가 프로덕션용으로 일반적으로 사용 가능함을 표시합니다.
-   *베타*는 인터페이스가 베타 오퍼링으로 사용 가능함을 표시합니다.
-   *지원되지 않음*은 인터페이스를 해당 언어에 사용할 수 없음을 의미합니다.

광대역 및 협대역 모델이 사용 가능한 지원되는 언어로 두 모델을 모두 사용할 수 있습니다. 언어가 언어 모델 사용자 정의를 지원하는 경우에는 문법도 지원합니다. 사용 가능한 모든 모델의 목록은 [지원되는 언어 모델](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList)을 참조하십시오.

<table>
  <caption>표 1. 사용자 정의에 대한 언어 지원</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      언어
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      언어 모델 사용자 정의에 대한 지원
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      음향 모델 사용자 정의에 대한 지원
    </th>
  </tr>
  <tr>
    <td>아랍어(현대 표준)</td>
    <td style="text-align:center">지원되지 않음</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>브라질 포르투갈어</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>중국어(간체)</td>
    <td style="text-align:center">지원되지 않음</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>영어(영국)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>영어(미국)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>프랑스어</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>독일어</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>일본어</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>한국어</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>스페인어(아르헨티나어)</td>
    <td style="text-align:center">베타</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>스페인어(카스티야어)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>스페인어(칠레어)</td>
    <td style="text-align:center">베타</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>스페인어(콜롬비아어)</td>
    <td style="text-align:center">베타</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>스페인어(멕시코어)</td>
    <td style="text-align:center">베타</td>
    <td style="text-align:center">베타</td>
  </tr>
  <tr>
    <td>스페인어(페루어)</td>
    <td style="text-align:center">베타</td>
    <td style="text-align:center">베타</td>
  </tr>
</table>

`GET /v1/models` 및 `GET /v1/models/{model_id}` 메소드를 사용하여 언어 모델이 언어 모델 사용자 정의를 지원하는지 여부를 확인할 수 있습니다. 기본 모델이 언어 모델 사용자 정의를 지원하는 경우 모델에 대한 메소드 출력의 `supported_features` 필드가 `custom_language_model` 필드를 `true`로 설정합니다.

## 사용자 정의에 대한 사용 참고사항
{: #customUsage}

다음 사용 참고사항은 언어 모델 사용자 정의 및 음향 모델 사용자 정의 모두에 적용됩니다.

### 사용자 정의 모델의 소유권
{: #customOwner}

사용자 정의 모델은 해당 인증 정보를 사용하여 이를 작성한 {{site.data.keyword.speechtotextshort}} 서비스의 인스턴스가 소유합니다. 어떤 방식으로든 사용자 정의 모델에 대해 작업하려면 사용자 정의 인터페이스의 메소드를 사용하여 해당 서비스 인스턴스에 대한 인증 정보를 사용해야 합니다. 서비스의 다른 인스턴스에 대해 작성된 인증 정보는 사용자 정의 모델을 보거나 액세스할 수 없습니다.

{{site.data.keyword.speechtotextshort}} 서비스의 동일한 인스턴스에 대해 얻은 모든 인증 정보가 해당 서비스 인스턴스에 대해 작성된 모든 사용자 정의 모델에 대한 액세스를 공유합니다. 사용자 정의 모델에 대한 액세스를 제한하려면 별도의 서비스 인스턴스를 작성하고 해당 서비스 인스턴스의 인증 정보만 사용하여 모델을 작성하고 작업하십시오. 다른 서비스 인스턴스의 인증 정보는 사용자 정의 모델에 영향을 줄 수 없습니다.

서비스 인스턴스의 인증 정보 간에 소유권을 공유하면 예를 들어 인증 정보가 손상된 경우 인증 정보 세트를 취소할 수 있다는 장점이 있습니다. 그런 다음 동일한 서비스 인스턴스에 대한 새 인증 정보를 작성하고 원래 인증 정보로 작성된 사용자 정의 모델의 소유권과 액세스를 유지할 수 있습니다.

### 요청 로깅 및 데이터 개인정보 보호
{: #customLogging}

서비스가 사용자 정의 인터페이스에 대한 호출의 요청 로깅을 처리하는 방법은 요청에 따라 다릅니다.

-   서비스는 사용자 정의 모델을 빌드하는 데 사용되는 데이터를 로깅하지 *않습니다*. 예를 들어, 사용자 정의 언어 모델의 말뭉치 및 단어에 대해 작업할 때 `X-Watson-Learning-Opt-Out` 요청 헤더를 설정할 필요가 없습니다. 훈련 데이터는 서비스의 기본 모델을 개선하는 데 사용되지 않습니다.
-   사용자 정의 모델이 인식 요청에 사용되는 경우 서비스가 데이터를 로깅하지 *않습니다*. 인식 요청에 대한 로깅을 방지하려면 `X-Watson-Learning-Opt-Out` 요청 헤더를 `true`로 설정해야 합니다.

자세한 정보는 [요청 로깅](/docs/services/speech-to-text?topic=speech-to-text-input#logging)을 참조하십시오.

### 정보 보안
{: #customSecurity}

사용자 정의 언어 및 사용자 정의 음향 모델에 대해 추가되거나 업데이트된 데이터와 고객 ID를 연관시킬 수 있습니다. 다음 메소드와 함께 `X-Watson-Metadata` 헤더를 전달하여 고객 ID를 말뭉치, 사용자 정의 단어, 문법 및 오디오 리소스를 연관시킵니다.

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

그런 다음, 필요한 경우 `DELETE /v1/user_data` 메소드를 사용하여 데이터를 삭제할 수 있습니다. 자세한 정보는 [정보 보안](/docs/services/speech-to-text?topic=speech-to-text-information-security)을 참조하십시오.
