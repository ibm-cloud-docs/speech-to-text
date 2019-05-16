---

copyright:
  years: 2017, 2019
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

# 사용자 정의 모델 업그레이드
{: #customUpgrade}

음성 인식의 품질을 개선하기 위해 {{site.data.keyword.speechtotextfull}} 서비스는 경우에 따라 기본 모델을 업데이트합니다. 언어의 광대역 및 협대역 모델과 마찬가지로 다른 언어에 대한 기본 모델은 서로 독립적이므로 개별 기본 모델을 업데이트해도 다른 모델에 영향을 미치지 않습니다. [릴리스 정보](/docs/services/speech-to-text/release-notes.html)에 모든 기본 모델 업데이트가 문서화되어 있습니다.
{: shortdesc}

기본 모델의 새 버전이 릴리스되면 업데이트를 활용할 수 있도록 기본 모델을 기반으로 빌드된 사용자 정의 언어 및 사용자 정의 음향 모델을 업그레이드해야 합니다. 업그레이드를 완료할 때까지 사용자 정의 모델이 이전 버전의 기본 모델을 계속 사용합니다. 모든 사용자 정의 오퍼레이션과 마찬가지로, 업그레이드하려면 모델을 소유하는 서비스의 인스턴스에 대한 인증 정보를 사용해야 합니다.

가능한 빨리 업데트된 기본 모델의 최신 버전으로 업그레이드하십시오. 사용자 정의 모델을 더 빨리 업그레이드할수록 새 모델의 향상된 성능을 더 빨리 경험할 수 있습니다. 또한 나중에 기본 모델의 이전 버전을 제거할 수 있습니다. 업그레이드를 권장하기 위해 서비스에서 이전 기본 모델을 기반으로 하는 사용자 정의 모델을 사용하는 인식 요청에 대한 결과와 함께 경고 메시지를 리턴합니다.

## 업그레이드 작동 방법
{: #upgradeOverview}

새 기본 모델이 처음으로 릴리스될 때 기존 사용자 정의 모델은 여전히 이전 버전의 기본 모델을 기반으로 하고 있습니다. 사용자 정의 모델이 업그레이드될 때까지 해당 사용자 정의 모델에 대한 모든 오퍼레이션(예: 데이터 추가 또는 모델 훈련)이 기존 버전의 모델에 영향을 줍니다. 마찬가지로, 사용자 정의 모델을 지정하는 모든 인식 요청이 기존 버전의 모델을 사용합니다.

업그레이드하면 두 가지 버전의 사용자 정의 모델(하나는 이전 버전의 기본 모델을 기반으로 하고 다른 하나는 최신 버전의 기본 모델을 기반으로 함)이 생성됩니다. 사용자 정의 모델이 업그레이드되면 해당 사용자 정의 모델에 대한 모든 오퍼레이션이 업그레이드된 새 버전의 모델에 영향을 줍니다. 더 이상 데이터를 추가하거나 이전 버전의 모델을 훈련할 수 없습니다. 또한 업그레이드 오퍼레이션을 실행 취소할 수 없습니다.

두 유형 중 하나의 사용자 정의 모델을 업그레이드할 때 개별 리소스를 업그레이드할 필요가 없습니다. 사용자 정의 언어 모델의 경우 서비스가 해당 모델에 대해 정의된 말뭉치, 문법 및 단어를 자동으로 업그레이드합니다. 마찬가지로, 사용자 정의 음향 모델을 업그레이드하면 서비스가 해당 오디오 리소스를 자동으로 업그레이드합니다.

기본적으로 이 서비스는 사용자 정의 모델의 최신 버전을 인식 요청에 사용합니다. 그러나 이전 버전의 사용자 정의 모델을 음석 인식에 계속할 수 있습니다. 자세한 정보는 [업그레이드된 사용자 정의 모델로 인식 요청 작성](#upgradeRecognition)을 참조하십시오.

## 사용자 정의 언어 모델 업그레이드
{: #upgradeLanguage}

사용자 정의 언어 모델을 업그레이드하려면 다음 단계를 따르십시오.

1.  사용자 정의 언어 모델이 `ready` 또는 `available` 상태인지 확인하십시오. `GET /v1/customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 확인할 수 있습니다. 모델의 상태가 `ready`이면 최신 데이터에 대해 훈련하기 전에 모델을 업그레이드하십시오.

1.  `POST /v1/customizations/{customization_id}/upgrade_model` 메소드를 사용하여 사용자 정의 언어 모델을 업그레이드하십시오.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    업그레이드 메소드는 비동기식입니다. 모델의 단어 리소스에 있는 단어의 수와 서비스의 현재 로드에 따라 완료하는 데 몇 분이 걸릴 수 있습니다.

업그레이드 프로세스가 성공적으로 시작되면 서비스에서 200 응답 코드를 리턴합니다. `GET /v1/customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 폴링하여 업그레이드의 상태를 모니터할 수 있습니다. 루프를 사용하여 10초마다 상태를 확인하십시오.

업그레이드되는 동안 사용자 정의 모델의 상태는 `upgrading`입니다. 업그레이드가 완료되면 모델이 업그레이드 이전의 상태를 재개합니다(`ready` 또는 `available`). 업그레이드 오퍼레이션의 상태를 확인하는 것은 훈련 오퍼레이션의 상태를 확인하는 것과 동일합니다. 자세한 정보는 [모델 훈련 요청 모니터링](/docs/services/speech-to-text/language-create.html#monitorTraining-language)을 참조하십시오.

업그레이드 요청이 완료될 때까지 서비스가 어떤 방식으로는 모델을 수정하는 요청을 승인할 수 없습니다. 그러나 업그레이드 중에 기존 버전의 모델로 인식 요청을 계속 발행할 수 있습니다.

## 사용자 정의 음향 모델 업그레이드
{: #upgradeAcoustic}

사용자 정의 음향 모델을 업그레이드하려면 다음 단계를 따르십시오. 사용자 정의 음향 모델이 사용자 정의 언어 모델로 훈련된 경우 표시된 대로 두 가지 추가 업그레이드 단계를 수행해야 합니다.

1.  *사용자 정의 음향 모델이 사용자 정의 언어 모델로 훈련된 경우* 먼저 사용자 정의 언어 모델을 기본 모델의 최신 버전으로 업그레이드해야 합니다. 자세한 정보는 [사용자 정의 언어 모델 업그레이드](#upgradeLanguage)를 참조하십시오.

1.  사용자 정의 음향 모델이 `ready` 또는 `available` 상태인지 확인하십시오. `GET /v1/acoustic_customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 확인할 수 있습니다. 모델의 상태가 `ready`이면 최신 데이터에 대해 훈련하기 전에 모델을 업그레이드하십시오.

1.  `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` 메소드를 사용하여 사용자 정의 음향 모델을 업그레이드하십시오.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    업그레이드 메소드는 비동기식입니다. 모델에 포함된 오디오 데이터의 양과 서비스의 현재 로드에 따라 완료하는 데 몇 분 또는 몇 시간이 걸릴 수 있습니다. 훈련에서와 마찬가지로 업그레이드에는 일반적으로 모델 오디오 데이터 길이의 약 2배가 걸립니다.

1.  *사용자 정의 음향 모델이 사용자 정의 언어 모델로 훈련된 경우* 이번에는 이전에 업그레이드된 사용자 정의 언어 모델을 사용하여 사용자 정의 음향 모델을 다시 업그레이드하십시오. `custom_language_model_id` 조회 매개변수를 사용하여 사용자 정의 언어 모델의 사용자 정의 ID를 지정하십시오.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    업그레이드 메소드는 비동기식이며 일반적으로 업그레이드에는 모델 오디오 데이터 길이의 약 2배가 걸립니다.

    언어 모델을 사용하여 음향 모델을 업그레이드하는 요청이 400 응답 코드 및 `No input data modified since last training` 메시지와 함께 실패할 수 있습니다. 이 오류가 발생하는 경우 부울 `force` 조회 매개변수를 요청에 추가하고 이 매개변수를 `true`로 설정하십시오. 이 특정 상황에서 사용자 정의 음향 모델의 업그레이드를 강제 실행하려는 경우에만 이 매개변수를 사용하십시오.
    {: note}

업그레이드 프로세스가 성공적으로 시작되면 서비스에서 200 응답 코드를 리턴합니다. `GET /v1/acoustic_customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 폴링하여 업그레이드의 상태를 모니터할 수 있습니다. 루프를 사용하여 1분에 한 번씩 상태를 확인하십시오.

업그레이드되는 동안 사용자 정의 모델의 상태는 `upgrading`입니다. 업그레이드가 완료되면 모델이 업그레이드 이전의 상태를 재개합니다(`ready` 또는 `available`). 업그레이드 오퍼레이션의 상태를 확인하는 것은 훈련 오퍼레이션의 상태를 확인하는 것과 동일합니다. 자세한 정보는 [모델 훈련 요청 모니터링](/docs/services/speech-to-text/acoustic-create.html#monitorTraining-acoustic)을 참조하십시오.

업그레이드 요청이 완료될 때까지 서비스가 어떤 방식으로는 모델을 수정하는 요청을 승인할 수 없습니다. 그러나 업그레이드 중에 기존 버전의 모델로 인식 요청을 계속 발행할 수 있습니다.

## 업그레이드 실패
{: #upgradeFailures}

서비스가 모델에 대한 다른 요청(예: 훈련 요청 또는 데이터 추가 요청)을 처리 중인 경우 사용자 정의 모델의 업그레이드를 시작하는 데 실패합니다. 다음 경우에도 업그레이드 요청을 시작하는 데 실패합니다.

-   사용자 정의 모델이 `ready` 또는 `available` 이외의 상태입니다.
-   사용자 정의 모델에 데이터(사용자 정의 단어 또는 오디오 리소스)가 없습니다.
-   사용자 정의 언어 모델로 훈련된 사용자 정의 음향 모델의 경우 사용자 정의 모델이 여러 버전의 기본 모델을 기반으로 합니다. 사용자 정의 음향 모델을 업그레이드하기 전에 사용자 정의 언어 모델을 업그레이드해야 합니다.

## 사용자 정의 모델에 대한 버전 정보 나열
{: #upgradeList}

사용자 정의 모델이 사용 가능한 기본 모델의 버전을 보려면 다음 메소드를 사용하십시오.

-   사용자 정의 언어 모델에 대한 정보를 나열하려면 `GET /v1/customizations/{customization_id}` 메소드를 사용하십시오. 자세한 정보는 [사용자 정의 언어 모델 나열](/docs/services/speech-to-text/language-models.html#listModels-language)을 참조하십시오.
-   사용자 정의 음향 모델에 대한 정보를 나열하려면 `GET /v1/acoustic_customizations/{customization_id}` 메소드를 사용하십시오. 자세한 정보는 [사용자 정의 음향 모델 나열](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic)을 참조하십시오.

두 경우 모두 사용자 정의 모델의 기본 모델에 대한 정보를 표시하는 `versions` 필드가 출력에 포함됩니다. 다음 출력은 업그레이드된 사용자 정의 언어 모델에 대한 정보를 표시합니다.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

`versions` 필드는 사용자 정의 모델을 기본 모델의 두 버전(이전 버전 `en-US_BroadbandModel.v07-06082016.06202016` 및 새 버전 `en-US_BroadbandModel.v2017-11-15`)에서 사용할 수 있음을 표시합니다. 사용자 정의 모델이 업그레이드되지 않았거나 기본 모델의 단일 버전만 존재하는 경우 `versions` 필드가 단일 버전을 표시합니다.

## 업그레이드된 사용자 정의 모델로 인식 요청 작성
{: #upgradeRecognition}

기본적으로 이 서비스는 인식 요청에 지정된 사용자 정의 모델의 최신 버전을 사용합니다. 그러나 사용자 정의 모델이 업그레이드된 후에도 이전 버전의 모델을 사용하여 인식 요청을 계속 수행할 수 있습니다. 인식 메소드의 `base_model_version` 매개변수를 사용하여 음성 인식에 사용될 기본 모델의 버전을 지정합니다.

예를 들어, 다음 HTTP 요청은 이전 버전의 기본 모델이 사용되도록 지정합니다. 따라서 지정된 사용자 정의 언어 모델의 이전 버전도 사용됩니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

이 기능을 사용하여 기본 모델의 이전 버전과 새 버전 모두에 대해 사용자 정의 모델의 성능 및 정확도를 테스트할 수 있습니다. 어떤 방식으로든 업그레이드된 모델의 성능이 부족하다는 것을 알게 되면(예를 들어, 특정 단어가 더 이상 인식되지 않음) 이전 버전을 인식 요청에 계속 사용할 수 있습니다.

[기본 모델 버전](/docs/services/speech-to-text/input.html#version)은 `base_model_version` 매개변수와 서비스가 인식 요청에 사용할 기본 및 사용자 정의 모델의 버전을 판별하는 방법을 설명합니다. 이 정보 이외에 사용자 정의 언어 및 사용자 정의 음향 모델을 모두 인식 요청과 함께 전달할 때 다음 문제를 고려하십시오.

-   두 사용자 정의 모델 모두 동일한 기본 모델(예: `en-US_BroadbandModel`)을 기반으로 해야 합니다.
-   두 사용자 정의 모델 모두 이전 기본 모델을 기반으로 하는 경우 서비스에서 이전 기본 모델을 인식에 사용합니다.
-   두 사용자 정의 모델 모두 새 기본 모델을 기반으로 하는 경우 서비스에서 새 기본 모델을 인식에 사용합니다.
-   두 사용자 정의 모델 중 하나만 새 기본 모델로 업그레이드된 경우 서비스에서 이전 기본 모델을 인식에 사용합니다. 두 사용자 정의 모델에 공통으로 있는 버전이므로 이전 기본 모델을 선택합니다.
