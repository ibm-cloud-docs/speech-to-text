---

copyright:
  years: 2017, 2019
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

# 사용자 정의 음향 모델 작성
{: #acoustic}

{{site.data.keyword.speechtotextshort}} 서비스의 사용자 정의 음향 모델을 작성하려면 다음 단계를 따르십시오.
{: shortdesc}

1.  [사용자 정의 음향 모델을 작성하십시오](#createModel-acoustic). 동일하거나 다른 도메인 또는 환경에 대한 여러 사용자 정의 모델을 작성할 수 있습니다. 작성하는 모든 모델에 대한 프로세스는 동일합니다. 그러나 인식 요청에 사용하는 경우 하나의 사용자 정의 음향 모델만 지정할 수 있습니다.
1.  [사용자 정의 음향 모델에 오디오를 추가하십시오](#addAudio). 서비스는 음성 인식에 허용되는 것과 동일한 오디오 파일 형식을 음향 모델링에 대해 허용합니다. 여러 오디오 파일을 포함하는 아카이브 파일도 허용합니다. 아카이브 파일은 선호되는 오디오 리소스 추가 방법입니다. 이 메소드를 반복하여 사용자 정의 모델에 더 많은 오디오 또는 아카이브 파일을 추가할 수 있습니다.
1.  [사용자 정의 음향 모델을 훈련하십시오](#trainModel-acoustic). 사용자 정의 모델에 오디오 리소스를 추가한 후 모델을 훈련해야 합니다. 훈련하면 사용자 정의 음향 모델이 음성 인식에 사용할 준비가 됩니다. 훈련에는 상당한 시간이 걸릴 수 있습니다. 훈련 기간은 모델에 포함된 오디오 데이터의 양에 따라 달라집니다.

    사용자 정의 음향 모델의 훈련 중에 헬퍼 사용자 정의 언어 모델을 지정할 수 있습니다. 오디오 파일 도메인의 OOV 단어 또는 오디오 파일의 변환이 포함된 사용자 정의 언어 모델은 사용자 정의 음향 모델의 품질을 향상할 수 있습니다. 자세한 정보는 [사용자 정의 언어 모델을 사용하여 사용자 정의 음향 모델 훈련](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothTrain)을 참조하십시오.
1.  사용자 정의 모델을 훈련한 후 인식 요청에 사용할 수 있습니다. 변환을 위해 전달된 오디오의 음향 품질이 사용자 정의 모델의 오디오와 유사한 경우 결과에 서비스의 향상된 이해도가 반영됩니다. 자세한 정보는 [사용자 정의 음향 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)을 참조하십시오.

    사용자 정의 음향 모델과 사용자 정의 언어 모델을 모두 동일한 인식 요청에 전달하여 인식 정확도를 더 향상시킬 수 있습니다. 자세한 정보는 [음성 인식 중에 사용자 정의 언어 및 사용자 정의 음향 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize)을 참조하십시오.

사용자 정의 음향 모델을 작성하는 단계는 반복적입니다. 필요할 때마다 오디오를 추가하거나 삭제하고 모델을 훈련하거나 재훈련할 수 있습니다. 오디오에 대한 변경사항을 적용하려면 모델을 재훈련해야 합니다. 모델을 재훈련하면 새 데이터만이 아니라 모든 오디오 데이터가 훈련에 사용됩니다. 따라서 훈련 시간은 모델에 포함된 총 오디오의 양에 비례합니다.

음향 모델 사용자 정의는 모든 언어에 대한 베타 기능으로 사용할 수 있습니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)을 참조하십시오.
{: note}

## 사용자 정의 음향 모델 작성
{: #createModel-acoustic}

`POST /v1/acoustic_customizations` 메소드를 사용하여 새 사용자 정의 음향 모델을 작성합니다. 임의의 수의 사용자 정의 음향 모델을 작성할 수 있지만 음성 인식 요청에는 한 번에 하나의 사용자 정의 음향 모델만 사용할 수 있습니다. 이 메소드는 새 사용자 정의 모델의 속성을 요청의 본문으로 정의하는 JSON 오브젝트를 받아들입니다.

<table>
  <caption>표 1. 새 사용자 정의 음향 모델의 속성</caption>
  <tr>
    <th style="width:20%; text-align:left">매개변수</th>
    <th style="width:12%; text-align:center">데이터 유형</th>
    <th style="text-align:left">설명</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>필수</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 사용자 정의 음향 모델의 사용자 정의 이름입니다. 사용자 정의 모델의 음향 환경에 대해 설명하는 이름을 사용하십시오(예: <code>Mobile custom model</code> 또는 <code>Noisy car custom model</code>). 사용자가 소유하는 모든 사용자 정의 음향 모델 중에서 고유한 이름을 사용하십시오. 사용자 정의 모델의 언어와 일치하는 현지화된 이름을 사용하십시오.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>필수</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 모델에서 사용자 정의할 기본 모델의 이름입니다. <code>GET /v1/models</code> 메소드에서 리턴되는 모델의 이름을 사용해야 합니다. 새 사용자 정의 모델은 이 모델이 사용자 정의하는 기본 모델을 통해서만 사용될 수 있습니다.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 모델에 대한 설명입니다. 사용자 정의 모델의 언어와 일치하는 현지화된 설명을 사용하십시오.
    </td>
  </tr>
</table>

다음 예제에서는 `Example acoustic model`이라는 새 사용자 정의 음향 모델을 작성합니다. 이 모델은 기본 모델 `en-US_BroadbandModel`용으로 작성되며 설명은 `Example custom acoustic model`입니다. `Content-Type` 헤더는 JSON 데이터가 메소드에 전달됨을 지정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

이 예제는 새 모델의 사용자 정의 ID를 리턴합니다. 각 사용자 정의 모델은 GUID(Globally Unique Identifier)인 고유 사용자 정의 ID로 식별됩니다. 모델과 연관된 호출의 `customization_id` 매개변수로 사용자 정의 모델의 GUID를 지정합니다.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

새 사용자 정의 모델은 해당 인증 정보를 사용하여 이를 작성한 서비스의 인스턴스가 소유합니다. 자세한 정보는 [사용자 정의 모델의 소유권](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner)을 참조하십시오.

## 사용자 정의 음향 모델에 오디오 추가
{: #addAudio}

사용자 정의 음향 모델을 작성한 후 다음 단계는 모델에 오디오 리소스를 추가하는 것입니다. `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드를 사용하여 사용자 정의 모델에 오디오 리소스를 추가합니다. 다음을 추가할 수 있습니다.

-   음성 인식에 지원되는 모든 형식의 개별 오디오 파일. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)을 참조하십시오.
-   여러 오디오 파일이 포함된 아카이브 파일(**.zip** 또는 **.tar.gz** 파일). 여러 오디오 파일을 단일 아카이브 파일에 수집하고 해당 단일 파일을 로드하는 것이 개별적으로 오디오 파일을 추가하는 것보다 훨씬 더 효율적입니다.

오디오 리소스를 요청 본문으로 전달하고 리소스에 `audio_name`을 지정합니다. 자세한 정보는 [오디오 리소스에 대한 작업](/docs/services/speech-to-text?topic=speech-to-text-audioResources)을 참조하십시오.

다음 예제는 오디오 및 아카이브 유형 리소스 모두를 추가하는 방법을 보여줍니다.

-   이 예제는 지정된 `customization_id`를 사용하는 사용자 정의 음향 모델에 오디오 유형 리소스를 추가합니다. `Content-Type` 헤더는 오디오 유형을 `audio/wav`로 식별합니다. 오디오 파일 **audio1.wav**가 요청의 본문으로 전달되고 리소스의 이름이 `audio1`로 지정됩니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   이 예제는 지정된 사용자 정의 음향 모델에 아카이브 유형 리소스를 추가합니다. `Content-Type` 헤더는 아카이브 유형을 `application/zip`으로 식별합니다. `Contained-Contented-Type` 헤더는 아카이브에 포함된 모든 파일의 형식이 `audio/l16`이며 16kHz의 속도로 샘플링되었음을 표시합니다. 아카이브 파일 **audio2.zip**이 요청의 본문으로 전달되고 리소스의 이름이 `audio2`로 지정됩니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

이 메소드는 사용자 정의 모델에 대한 기존 오디오 리소스를 겹쳐쓰기 위한 선택적 `allow_overwrite` 조회 매개변수도 허용합니다. 오디오 리소스를 모델에 추가한 후 업데이트해야하는 경우에 이 매개변수를 사용하십시오.

이 메소드는 비동기식입니다. 오디오의 지속 시간에 따라 완료하는 데 몇 초가 걸릴 수 있습니다. 아카이브 파일의 경우 오퍼레이션의 길이는 오디오 파일의 지속 시간에 따라 다릅니다. 오디오 리소스 추가 요청의 상태를 확인하는 방법에 대한 자세한 정보는 [오디오 추가 요청 모니터링](#monitorAudio)을 참조하십시오.

각 오디오 또는 아카이브 파일마다 한 번씩 메소드를 호출하여 사용자 정의 모델에 원하는 수의 오디오 리소스를 추가할 수 있습니다. 여러 요청을 작성하여 서로 다른 오디오 리소스를 동시에 추가할 수 있습니다. 

훈련하기 전에 무음이 아닌 음성이 포함된 최소 10분에서 최대 200시간 길이의 오디오를 사용자 정의 모델에 추가해야 합니다. 오디오 또는 아카이브 유형 리소스의 크기는 100MB를 초과할 수 없습니다. 자세한 정보는 [오디오 리소스 추가 관련 가이드라인](/docs/services/speech-to-text?topic=speech-to-text-audioResources#audioGuidelines)을 참조하십시오.

### 오디오 추가 요청 모니터링
{: #monitorAudio}

오디오가 유효한 경우 서비스에서 201 응답 코드를 리턴합니다. 그런 다음 오디오 파일의 컨텐츠를 비동기식으로 분석하고 길이, 샘플링 속도 및 인코딩과 같은 오디오에 대한 정보를 자동으로 추출합니다. 현재 요청에 대한 모든 오디오 리소스의 서비스 분석이 완료될 때까지 사용자 정의 모델을 훈련시킬 수 없습니다. 

요청의 상태를 판별하려면 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드를 사용하여 오디오의 상태를 폴링하십시오. 이 메소드는 사용자 정의 모델의 GUID 및 오디오 리소스의 이름을 받아들입니다. 해당 응답에는 다음 값 중 하나인 리소스의 `status`가 포함되어 있습니다.

-   `ok`는 오디오를 허용할 수 있으며 분석이 완료되었음을 표시합니다.
-   `being_processed`는 서비스가 오디오를 여전히 분석 중임을 표시합니다.
-   `invalid`는 오디오 파일을 처리하도록 허용할 수 없음을 표시합니다. 형식이나 샘플링 속도가 잘못되었거나 오디오 파일이 아닐 수 있습니다. 아카이브 파일의 경우 포함된 오디오 파일 중에 올바르지 않은 것이 있으면 전체 아카이브가 올바르지 않습니다.

응답의 컨텐츠와 `status` 필드의 위치는 리소스 유형(오디오 또는 아카이브)에 따라 다릅니다.

-   *오디오 유형 리소스의 경우 * `status` 필드는 최상위 레벨(`AudioListing`) 오브젝트에 있습니다.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *아카이브 유형 리소스의 경우* `status` 필드가 `container` 필드에 중첩된 두 번째 레벨(`AudioResource`) 오브젝트에 있습니다.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    }
    ```
    {: codeblock}

루프를 사용하여 오디오 리소스가 `ok`가 될 때까지 몇 초마다 오디오 리소스의 상태를 확인할 수 있습니다. 이 메소드에서 리턴되는 기타 필드에 대한 자세한 정보는 [사용자 정의 음향 모델에 대한 오디오 리소스 나열](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio)을 참조하십시오.

## 사용자 정의 음향 모델 훈련
{: #trainModel-acoustic}

사용자 정의 음향 모델을 오디오 리소스로 채운 후 새 데이터에 대해 모델을 훈련해야 합니다. 훈련하면 사용자 정의 모델이 음성 인식에 사용할 준비가 됩니다. 새 데이터에 대해 훈련한 후에만 모델을 인식 요청에 사용할 수 있습니다. 또한 새로 작성되거나 변경된 오디오 리소스 양식으로 사용자 정의 모델을 업데이트하면 모델을 변경사항으로 훈련할 때까지 업데이트가 모델에 반영되지 않습니다.

`POST /v1/acoustic_customizations/{customization_id}/train` 메소드를 사용하여 사용자 정의 모델을 훈련할 수 있습니다. 다음 예제에서와 같이 훈련할 모델의 사용자 정의 ID를 메소드에 전달합니다.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

이 메소드는 비동기식입니다. 사용자 정의 음향 모델에 포함된 오디오 데이터의 양과 서비스의 현재 로드에 따라 훈련을 완료하는 데 몇 분 또는 몇 시간이 걸릴 수 있습니다. 일반 가이드라인은 사용자 정의 음향 모델 훈련에는 오디오 데이터 길이의 약 2 - 4배가 걸린다는 것입니다. 시간의 범위는 훈련 중인 모델 및 오디오의 네이처(예: 오디오가 선명한지 아니면 잡음이 있는지)에 따라 달라집니다. 예를 들어, 2시간 길이의 오디오가 포함된 모델을 훈련하는 데 4 - 8시간이 걸릴 수 있습니다. 훈련 오퍼레이션의 상태를 확인하는 방법에 대한 자세한 정보는 [모델 훈련 요청 모니터링](#monitorTraining-acoustic)을 참조하십시오.

이 메소드에는 다음과 같은 선택적 조회 매개변수가 포함됩니다.

-   `custom_language_model_id` 매개변수는 훈련 중에 사용될 별도로 작성된 사용자 정의 언어 모델을 지정합니다. 오디오 파일의 변환이나 오디오 파일의 컨텐츠와 관련된 OOV 단어 또는 말뭉치가 포함된 사용자 정의 언어 모델로 훈련할 수 있습니다. 훈련에 성공하려면 사용자 정의 음향 및 사용자 정의 언어 모델이 동일한 기본 모델의 동일한 버전을 기반으로 해야 합니다. 자세한 정보는 [사용자 정의 언어 모델을 사용하여 사용자 정의 음향 모델 훈련](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothTrain)을 참조하십시오.
-   `strict` 매개변수는 사용자 정의 모델이 올바른 오디오 리소스와 올바르지 않는 오디오 리소스를 함께 포함하는 경우 훈련이 진행되는지 여부를 표시합니다. 기본적으로 모델에 하나 이상의 올바르지 않은 리소스가 포함된 경우 훈련에 실패합니다. 모델에 하나 이상의 올바른 리소스가 포함되어 있는 한 매개변수를 `false`로 설정하여 훈련을 지속시키십시오. 서비스는 훈련에서 올바르지 않은 리소스를 제외합니다. 자세한 정보는 [훈련 실패](#failedTraining-acoustic)를 참조하십시오.

### 모델 훈련 요청 모니터링
{: #monitorTraining-acoustic}

훈련 프로세스가 성공적으로 시작되면 서비스에서 200 응답 코드를 리턴합니다. 기존 훈련 요청이 완료될 때까지 서비스가 후속 훈련 요청 또는 더 많은 오디오 리소스를 추가하는 요청을 수락할 수 없습니다.

훈련 요청의 상태를 판별하려면 `GET /v1/acoustic_customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 폴링하십시오. 다음 예제에서와 같이 이 메소드는 음향 모델의 사용자 정의 ID를 받아들이고 해당 상태를 리턴합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T22:11:13.298Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

응답에는 모델의 현재 상태를 보고하는 `status` 및 `progress` 필드가 포함되어 있습니다. `progress` 필드의 의미는 모델의 상태에 따라 다릅니다. `status` 필드의 값은 다음 중 하나일 수 있습니다.

-   `pending`은 모델이 작성되었지만 올바른 훈련 데이터가 추가되거나 서비스가 추가된 데이터의 분석을 완료할 때까지 대기 중임을 표시합니다. `progress` 필드는 `0`입니다.
-   `ready`는 모델이 올바른 데이터를 포함하고 있으며 훈련될 준비가 되었음을 표시합니다. `progress` 필드는 `0`입니다.

    모델이 올바른 오디오 리소스와 올바르지 않은 오디오 리소스를 함께 포함하는 경우 `strict` 조회 매개변수를 `false`로 설정하지 않으면 모델 훈련에 실패합니다. 자세한 정보는 [훈련 실패](#failedTraining-acoustic)를 참조하십시오.
-   `training`은 모델을 훈련 중임을 표시합니다. 훈련이 완료되면 `progress` 필드가 `0`에서 `100`으로 변경됩니다. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available`은 모델이 훈련되었으며 사용할 준비가 되었음을 표시합니다. `progress` 필드는 `100`입니다.
-   `upgrading`은 모델을 업그레이드 중임을 표시합니다. `progress` 필드는 `0`입니다.
-   `failed`는 모델의 훈련이 실패했음을 표시합니다. `progress` 필드는 `0`입니다. 자세한 정보는 [훈련 실패](#failedTraining-acoustic)를 참조하십시오.

루프를 사용하여 모델이 `available`이 될 때까지 1분에 한 번씩 훈련의 상태를 확인할 수 있습니다. 이 메소드에서 리턴되는 기타 필드에 대한 자세한 정보는 [사용자 정의 음향 모델 나열](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic)을 참조하십시오.

### 훈련 실패
{: #failedTraining-acoustic}

서비스가 사용자 정의 음향 모델에 대한 다른 요청을 처리 중인 경우 훈련을 시작하는 데 실패합니다. 충돌 요청은 다른 훈련 요청이거나 모델에 오디오 리소스를 추가하는 요청일 수 있습니다. 서비스는 409의 상태 코드를 리턴합니다. 

또한 다음과 같은 이유로 훈련을 시작하는 데 실패합니다. 

-   사용자 정의 모델에 10분 미만의 오디오 데이터가 포함되어 있습니다.
-   사용자 정의 모델에 200시간을 초과하는 오디오 데이터가 포함되어 있습니다.
-   사용자 정의 모델의 오디오 리소스 중 하나 이상이 올바르지 않습니다.
-   `custom_language_model_id` 조회 매개변수를 사용하여 호환되지 않는 사용자 정의 언어 모델을 전달했습니다. 두 사용자 정의 모델은 동일한 기본 모델의 동일한 버전을 기반으로 해야 합니다.

서비스는 400의 상태 코드를 리턴하고 사용자 정의 모델의 상태를 `failed`로 설정합니다. 다음 조치 중 하나를 수행하십시오.

-   `GET /v1/acoustic_customizations/{customization_id}/audio` 및 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드를 사용하여 모델의 오디오 리소스를 검사하십시오. 자세한 정보는 [사용자 정의 음향 모델에 대한 오디오 리소스 나열](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio)을 참조하십시오.

    올바르지 않은 각 오디오 리소스에 대해 다음 중 하나를 수행하십시오.
    -   오디오 리소스를 정정하고 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드의 `allow_overwrite` 매개변수를 사용하여 올바른 오디오를 모델에 추가하십시오. 자세한 정보는 [사용자 정의 음향 모델에 오디오 추가](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)를 참조하십시오.
    -   모델에서 오디오 리소스를 삭제하려면 `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드를 사용하십시오. 자세한 정보는 [사용자 정의 음향 모델에서 오디오 리소스 삭제](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#deleteAudio)를 참조하십시오.
-   훈련에서 올바르지 않은 오디오 리소스를 제외하려면 `POST /v1/acoustic_customizations/{customization_id}/train` 메소드의 `strict` 매개변수를 `false`로 설정하십시오. 훈련이 성공하려면 모델에는 하나 이상의 올바른 오디오 리소스가 포함되어야 합니다. `strict` 매개변수는 올바른 오디오 리소스와 올바르지 않는 오디오 리소스가 함께 포함된 사용자 정의 모델을 훈련시키는 데 유용합니다. 
