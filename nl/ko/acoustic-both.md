---

copyright:
  years: 2019
lastupdated: "2019-06-24"

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

# 사용자 정의 음향 및 사용자 정의 언어 모델을 함께 사용
{: #useBoth}

사용자 정의 언어 및 사용자 정의 음향 모델을 상호 보완적으로 사용하여 음성 인식 정확도를 향상시킬 수 있습니다. 음향 모델 훈련 및 음석 인식 중에 두 가지 유형의 모델을 모두 사용할 수 있습니다. 두 모델 모두 동일한 서비스 인스턴스에서 소유해야 하며 동일한 기본 언어 모델을 기반으로 해야 합니다.
{: shortdesc}

사용자 정의 음향 모델을 단독으로 사용하거나 훈련되지 않은 사용자 정의 언어 모델과 함께 사용하는 것이 여전히 유용할 수 있습니다. 사용자 정의 음향 모델이 기록 중인 오디오와 일치하는 음향 특성에 대해 훈련된 경우 변환 품질을 향상시킬 수 있습니다.

## 사용자 정의 언어 모델을 사용하여 사용자 정의 음향 모델 훈련
{: #useBothTrain}

오디오 데이터만으로 사용자 정의 음향 모델을 훈련하는 것을 *감독되지 않는 훈련(unsupervised training)*이라고 합니다. 훈련 중에 사용자 정의 언어 모델을 사용하는 것을 *부분 감독 훈련(lightly supervised training)*이라고 합니다. 부분 감독 훈련(lightly supervised training)은 사용자 정의 음향 모델의 효율성을 향상시킬 수 있습니다.

다음과 같은 경우에 부분 감독 훈련(lightly supervised training)을 사용하여 사용자 정의 언어 모델로 사용자 정의 음향 모델을 훈련하십시오.

-   사용자 정의 언어 모델이 사용자 정의 음향 모델에 추가한 오디오 파일의 변환(축약어 텍스트)을 기반으로 합니다.

    기록에 오디오의 정확한 컨텐츠가 포함되어 있으므로 기록을 기반으로 하는 사용자 정의 모델로 훈련하면 최상의 결과를 얻을 수 있습니다. 서비스가 컨텍스트에 맞게 변환의 컨텐츠를 구문 분석하고 오디오를 가장 효과적으로 사용하는 데 도움이 될 수 있는 OOV 단어 및 n-gram을 추출합니다. 오디오 데이터의 길이가 1시간 미만인 경우에 특히 그렇습니다.

    오디오 데이터 변환이 반드시 필요하지는 않습니다. 오디오를 변환하는 경우 사용자 정의 음향 모델의 품질이 향상될 수 있습니다. 변환은 오디오에 많은 OOV 단어가 포함되어 있는 경우에 특히 중요합니다.
-   사용자 정의 언어 모델이 오디오 파일의 컨텐츠와 관련된 단어의 목록 또는 말뭉치(텍스트 파일)를 기반으로 합니다.

    오디오에 서비스의 기본 어휘에서 찾을 수 없는 도메인 특정 단어가 포함되어 있는 경우 음향 모델 사용자 정의만으로 변환 중에 해당 단어를 생성할 수 없습니다. 언어 모델 사용자 정의가 서비스의 기본 어휘를 확장하는 유일한 방법입니다. 변환이 없는 경우 오디오 데이터와 동일한 도메인의 OOV 단어를 포함하는 사용자 정의 언어 모델로 훈련하십시오. 오디오에서 사용되는 OOV 단어의 목록이 포함된 사용자 정의 언어 모델로 훈련하는 것도 도움이 될 수 있습니다.

    예를 들어, 특정 제품에 대한 콜센터 오디오를 기반으로 하는 사용자 정의 음향 모델을 작성한다고 가정하십시오. 관련 통화의 기록을 기반으로 하거나 콜센터에서 처리된 특정 제품의 이름을 포함하는 사용자 정의 언어 모델로 사용자 정의 음향 모델을 훈련할 수 있습니다.

단어의 변환 또는 목록을 사용하려면 먼저 이 텍스트 데이터가 포함된 사용자 정의 언어 모델을 작성합니다. 사용자 정의 언어 모델로 사용자 정의 음향 모델을 훈련하려면 두 사용자 정의 모델이 모두 동일한 기반 모델의 동일한 버전을 기반으로 해야 합니다. 기본 모델의 새 버전이 사용 가능한 경우 훈련에 성공하려면 두 모델을 모두 기본 모델의 동일한 버전으로 업그레이드해야 합니다.

사용자 정의 언어 모델로 사용자 정의 음향 모델을 훈련하려면 `POST /v1/acoustic_customizations/{customization_id}/train` 메소드의 선택적 `custom_language_model_id` 조회 매개변수를 사용하십시오. `customization_id` 매개변수를 사용하여 음향 모델의 GUID를 전달하고 `custom_language_model_id` 매개변수를 사용하여 사용자 정의 언어 모델의 GUID를 전달하십시오. 두 모델 모두 요청과 함께 전달된 인증 정보에서 소유해야 합니다.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## 음성 인식 중에 사용자 정의 언어 및 사용자 정의 음향 모델 사용
{: #useBothRecognize}

음성 인식 요청에서 사용자 정의 언어 모델 및 사용자 정의 음향 모델 모두를 지정할 수 있습니다. 사용자 정의 언어 모델에 인식 중인 오디오 도메인의 OOV 단어가 포함되어 있는 경우 음성 인식 중에 해당 언어 모델을 사용자 정의 음향 모델과 함께 사용하여 변환 정확도를 향상시킬 수 있습니다.

사용자 정의 언어 모델로 사용자 정의 음향 모델을 훈련했는지 여부와 관계없이 사용자 정의 언어 모델을 사용하면 변환 정확도가 향상될 수 있습니다.

-   훈련 중에 사용자 정의 언어 및 사용자 정의 음향 모델을 모두 사용하면 사용자 정의 음향 모델의 품질이 향상됩니다.
-   음성 인식 중에 두 가지 유형의 모델을 모두 사용하면 변환의 품질이 향상됩니다.

사용자 정의 언어 모델에 문법이 포함되어 있는 경우 음성 인식 중에 사용자 정의 언어 모델 및 해당 문법 중 하나를 사용자 정의 음향 모델과 함께 사용할 수도 있습니다.

다음 예제에서는 두 가지 유형의 모델을 모두 HTTP `POST /v1/recognize` 메소드에 전달합니다. `acoustic_customization_id` 매개변수를 사용하여 사용자 정의 음향 모델의 GUID를 전달하고 `language_customization_id` 매개변수를 사용하여 사용자 정의 언어 모델의 GUID를 전달하십시오. 두 모델 모두 요청과 함께 전달된 인증 정보에서 소유해야 하며 동일한 기본 모델(예: `en-US_BroadbandModel`)을 기반으로 해야 합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

비동기 HTTP 요청의 경우 비동기 작업을 작성할 때 매개변수를 지정합니다. WebSocket의 경우 연결을 설정할 때 매개변수를 전달합니다.
