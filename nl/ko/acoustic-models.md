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

# 사용자 정의 음향 모델 관리
{: #manageAcousticModels}

사용자 정의 인터페이스에는 사용자 정의 음향 모델을 작성하기 위한 `POST /v1/acoustic_customizations` 메소드가 포함되어 있습니다. 이 인터페이스에는 최신 오디오 리소스에 대해 사용자 정의 모델을 훈련하기 위한 `POST /v1/acoustic_customizations/train` 메소드도 있습니다. 자세한 정보는 다음을 참조하십시오.
{: shortdesc}

-   [사용자 정의 음향 모델 작성](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [사용자 정의 음향 모델 훈련](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

또한 이 인터페이스에는 사용자 정의 음향 모델에 대한 정보 나열, 사용자 정의 모델을 초기 상태로 재설정, 사용자 정의 모델 업그레이드 및 사용자 정의 모델 삭제를 수행하기 위한 메소드도 포함되어 있습니다. 모델에 오디오 리소스를 추가하는 것을 비롯하여 서비스가 해당 모델에서 다른 오퍼레이션을 처리하는 중에는 사용자 정의 모델을 훈련, 재설정 또는 삭제할 수 없습니다. 

## 사용자 정의 음향 모델 나열
{: #listModels-acoustic}

사용자 정의 인터페이스는 지정된 인증 정보가 소유하는 사용자 정의 음향 모델에 대한 정보를 나열하기 위한 두 가지 메소드를 제공합니다.

-   `GET /v1/acoustic_customizations` 메소드는 모든 사용자 정의 음향 모델 또는 지정된 언어의 모든 사용자 정의 음향 모델에 대한 정보를 나열합니다.
-   `GET /v1/acoustic_customizations/{customization_id}` 메소드는 지정된 사용자 정의 음향 모델에 대한 정보를 나열합니다. 이 메소드를 사용하여 훈련 요청의 상태에 대한 서비스를 폴링할 수 있습니다.

두 메소드 모두 사용자 정의 음향 모델에 대한 다음 정보를 리턴합니다.

-   `customization_id`는 사용자 정의 모델의 GUID(Globally Unique Identifier)를 식별합니다. GUID는 인터페이스의 메소드에서 모델을 식별하는 데 사용됩니다.
-   `created`는 사용자 정의 모델이 작성된 협정 세계시(UTC) 기준 날짜 및 시간입니다.
-   `updated`는 사용자 정의 모델이 마지막으로 수정된 협정 세계시(UTC) 기준 날짜 및 시간입니다.
-   `language`는 사용자 정의 모델의 언어입니다.
-   `owner`는 사용자 정의 모델을 소유하는 서비스 인스턴스의 인증 정보를 식별합니다.
-   `name`은 사용자 정의 모델의 이름입니다.
-   `description`은 사용자 정의 모델에 대한 설명을 표시합니다(작성 시 제공된 경우).
-   `base_model_name`은 사용자 정의 모델이 작성된 언어 모델의 이름을 표시합니다.
-   `versions`는 사용자 정의 모델의 사용 가능한 버전 목록을 제공합니다. 배열의 각 요소는 사용자 정의 모델이 사용될 수 있는 기본 모델의 버전을 표시합니다. 사용자 정의 모델이 업그레이드된 경우에만 여러 버전이 존재합니다. 그렇지 않으면 하나의 버전만 표시됩니다. 자세한 정보는 [사용자 정의 모델에 대한 버전 정보 나열](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList)을 참조하십시오.

이 메소드는 사용자 정의 모델의 상태를 표시하는 `status` 필드도 리턴합니다.

-   `pending`은 모델이 작성되었음을 표시합니다. 올바른 훈련 데이터(오디오 리소스)가 추가되거나 서비스가 추가된 데이터 분석을 완료할 때까지 대기 중입니다.
-   `ready`는 모델이 올바른 오디오 데이터를 포함하고 있으며 훈련될 준비가 되었음을 표시합니다. 모델이 올바른 오디오 리소스와 올바르지 않은 오디오 리소스를 함께 포함하는 경우 `strict` 조회 매개변수를 `false`로 설정하지 않으면 모델 훈련에 실패합니다. 자세한 정보는 [훈련 실패](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic)를 참조하십시오.
-   `training`은 오디오 데이터에 대해 모델을 훈련 중임을 표시합니다.
-   `available`은 모델이 훈련되었으며 인식 요청에 사용할 준비가 되었음을 표시합니다.
-   `upgrading`은 모델을 업그레이드 중임을 표시합니다.
-   `failed`는 모델의 훈련이 실패했음을 표시합니다. 모델의 오디오 리소스를 검사하여 모델이 훈련되지 못한 원인을 판별하십시오. 가능한 오류에는 오디오 부족, 너무 많은 오디오 또는 올바르지 않은 오디오 리소스가 있습니다.

또한 출력에는 사용자 정의 모델 훈련의 현재 진행상태를 표시하는 `progress` 필드가 포함되어 있습니다. `POST /v1/acoustic_customizations/{customization_id}/train` 메소드를 사용하여 모델 훈련을 시작한 경우 이 필드에 해당 요청의 현재 진행상태가 완료율로 표시됩니다. 현재는 상태가 `available`이면 이 필드의 값이 `100`이고 그렇지 않으면 `0`입니다.

### 예제 요청 및 응답
{: #listExample-acoustic}

다음 예제에는 지정된 인증 정보가 소유하는 모든 미국 영어 사용자 정의 음향 모델을 나열하는 `language` 조회 매개변수가 포함되어 있습니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
```
{: pre}

인증 정보에는 두 개의 모델이 있습니다. 첫 번째 모델은 데이터를 대기 중이거나 서비스에서 처리 중입니다. 두 번째 모델은 훈련이 완료되었으며 사용할 준비가 되었습니다.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

다음 예제는 지정된 사용자 정의 ID가 있는 사용자 정의 모델에 대한 정보를 리턴합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## 사용자 정의 음향 모델 재설정
{: #resetModel-acoustic}

사용자 정의 음향 모델을 재설정하려면 `POST /v1/acoustic_customizations/{customization_id}/reset` 메소드를 사용하십시오. 사용자 정의 모델을 재설정하면 모델에서 모든 오디오 리소스가 제거되어 모델이 작성 시 상태로 초기화됩니다. 이 메소드는 모델 자체 또는 메타데이터(예: 이름 및 언어)를 삭제하지 않습니다. 그러나 모델을 재설정하면 해당 오디오 리소스가 제거되므로 다시 작성해야 합니다.

### 예제 요청
{: #resetExample-acoustic}

다음 예제는 지정된 사용자 정의 ID를 사용하는 사용자 정의 음향 모델을 재설정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## 사용자 정의 음향 모델 삭제
{: #deleteModel-acoustic}

더 이상 필요하지 않은 사용자 정의 음향 모델을 삭제하려면 `DELETE /v1/acoustic_customizations/{customization_id}` 메소드를 사용하십시오. 이 메소드는 사용자 정의 모델과 연관된 모든 오디오와 모델 자체를 삭제합니다. 이 메소드를 사용할 때는 주의를 기울이십시오. 모델이 삭제된 후에는 사용자 정의 모델 및 해당 데이터를 재확보할 수 없습니다.

### 예제 요청
{: #deleteExample-acoustic}

다음 예제에서는 지정된 사용자 정의 ID를 사용하는 사용자 정의 음향 모델을 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
