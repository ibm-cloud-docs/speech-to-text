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

# 언어 및 모델
{: #models}

{{site.data.keyword.speechtotextfull}} 서비스는 많은 언어의 음성 인식을 지원합니다. 모든 인터페이스에 대해 `model` 매개변수를 사용하여 음성 인식 요청을 위한 모델을 지정할 수 있습니다. 모델은 오디오에서 사용된 언어와 오디오가 샘플링된 속도를 표시합니다.
{: shortdesc}

## 지원되는 언어 모델
{: #modelsList}

이 서비스는 대부분의 언어에 대해 광대역 및 협대역 모델을 모두 지원합니다.

-   *광대역 모델*은 16KHz 이상으로 샘플링된 오디오에 사용합니다. 응답성이 뛰어난 실시간 애플리케이션(라이브 음성 애플리케이션)의 경우 광대역 모델을 사용하십시오.
-   *협대역 모델*은 8KHz로 샘플링된 오디오에 사용합니다. 이 샘플링 속도의 일반적인 용도인 전화 음성의 오프라인 디코딩에는 협대역 모델을 사용하십시오.

애플리케이션에 적합한 모델을 선택하는 것이 중요합니다. 오디오의 샘플링 속도(및 언어)와 일치하는 모델을 사용하십시오. 이 서비스는 사용자가 지정하는 모델과 일치하도록 오디오의 샘플링 속도를 자동으로 조정합니다. 자세한 정보는 [샘플링 속도](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#samplingRate)를 참조하십시오.

최상의 인식 정확도를 얻으려면 오디오의 주파수 컨텐츠도 고려해야 합니다. 자세한 정보는 [오디오 주파수](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#frequency)를 참조하십시오.
{: tip}

표 1에는 각 언어에 대해 지원되는 모델이 나열되어 있습니다. 요청에서 `model` 매개변수를 생략하면 기본적으로 서비스가 미국 영어 광대역 모델인 `en-US_BroadbandModel`을 사용합니다. *베타*로 표시되지 않으면 일반적으로 모든 언어가 프로덕션용으로 GA(Generally Available)됩니다. 

<table>
  <caption>표 1. 지원되는 언어 모델</caption>
  <tr>
    <th style="text-align:left">언어</th>
    <th style="text-align:center">광대역 모델</th>
    <th style="text-align:center">협대역 모델</th>
  </tr>
  <tr>
    <td>아랍어(현대 표준)</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">지원되지 않음</td>
  </tr>
  <tr>
    <td>브라질 포르투갈어</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>중국어(간체)</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>영어(영국)</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>영어(미국)</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>프랑스어</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>독일어</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>일본어</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>한국어</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>스페인어(아르헨티나어, 베타)</td>
    <td style="text-align:center"><code>es-AR_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-AR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>스페인어(카스티야어)</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>스페인어(칠레어, 베타)</td>
    <td style="text-align:center"><code>es-CL_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-CL_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>스페인어(콜롬비아어, 베타)</td>
    <td style="text-align:center"><code>es-CO_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-CO_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>스페인어(멕시코어, 베타)</td>
    <td style="text-align:center"><code>es-MX_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-MX_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>스페인어(페루어, 베타)</td>
    <td style="text-align:center"><code>es-PE_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-PE_NarrowbandModel</code></td>
  </tr>
</table>

### 미국 영어 약식 모델
{: #modelsShortform}

미국 영어 약식 모델인 `en-US_ShortForm_NarrowbandModel`이 대화식 음성 응답(IVR) 및 자동화된 고객 지원 솔루션에 대한 음성 인식을 개선할 수 있습니다. 약식 모델은 자동화된 콜센터 및 인간 지원 콜센터와 같은 고객 지원 설정에서 자주 표현되는 짧은 발화를 인식하도록 훈련됩니다. 예를 들어, 이 모델은 숫자, 단일 문자로 된 단어 및 이름 맞춤법과 같은 정밀한 발화와 예-아니오 응답을 위해 조정됩니다. 문법을 약식 모델과 함께 사용하면 인식 결과가 더 개선될 수 있습니다.

모든 모델과 마찬가지로, 잡음 환경은 결과에 부정적인 영향을 미칠 수 있습니다. 예를 들어, 공항, 이동 중인 차량, 회의실 및 여러 화자의 배경 음향 잡음은 변환 정확도를 감소시킬 수 있습니다.  스피커폰의 오디오도 이러한 디바이스에 공통적인 반향으로 인해 정확도를 감소시킬 수 있습니다. 사용자 정의 음향 모델을 약식 모델과 함께 사용하면 이러한 영향이 상쇄될 수 있습니다.

-   언어 모델 및 음향 모델 사용자 정의에 대한 자세한 정보는 [사용자 정의 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-customization)를 참조하십시오.
-   문법에 대한 자세한 정보는 [사용자 정의 언어 모델에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammars)을 참조하십시오.

### 언어 모델 예제
{: #modelsExample}

다음 예제 HTTP 요청은 `en-US-NarrowbandModel` 모델을 음성 인식에 사용합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## 모델 나열
{: #listModels}

HTTP 인터페이스는 지원되는 모델에 대한 정보를 나열하기 위한 두 가지 메소드를 제공합니다.

-   `GET /v1/models` 메소드는 사용 가능한 모든 모델에 대한 정보를 나열합니다.
-   `GET /v1/models/{model_id}` 메소드는 지정된 모델에 대한 정보를 나열합니다.

두 메소드 모두 모델에 대한 다음 정보를 리턴합니다.

-   `name`은 요청에서 사용하는 모델의 이름입니다.
-   `language`는 모델의 언어 ID입니다(예: `en-US`).
-   `rate`는 모델에서 사용되는 Hz 단위의 샘플링 속도(오디오에 허용 가능한 최소 속도)를 식별합니다.
-   `url`은 모델의 URI입니다.
-   `description`은 모델에 대한 간략한 설명을 제공합니다.
-   `supported_features`는 모델에 지원되는 추가 서비스 기능에 대해 설명합니다.
    -   `custom_language_model`은 모델을 기반으로 하는 사용자 정의 언어 모델을 작성할 수 있는지 여부를 표시하는 부울입니다.
    -   `speaker_labels`는 `speaker_labels` 매개변수를 모델에 사용할 수 있는지 여부를 표시합니다.

### 예제 요청 및 응답
{: #listExample}

다음 예제는 서비스에서 지원되는 모든 모델을 나열합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

다음 예제는 미국 영어 광대역 모델에 대한 정보를 표시합니다. 이 모델은 언어 모델 사용자 정의와 화자 레이블을 모두 지원합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
