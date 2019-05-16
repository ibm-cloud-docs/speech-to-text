---

copyright:
  years: 2015, 2019
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

# 사용자 정의 언어 모델 사용
{: #languageUse}

사용자 정의 언어 모델을 작성하고 훈련한 후 음성 인식 요청에서 사용할 수 있습니다. 다음 예제에 표시된 대로 `language_customization_id` 조회 매개변수를 사용하여 요청에 대한 사용자 정의 언어 모델을 지정합니다. 사용자 정의 모델의 단어에 지정할 가중치를 서비스에 알릴 수도 있습니다. 자세한 정보는 [사용자 정의 가중치 사용](#weight)을 참조하십시오. 모델을 소유하는 서비스 인스턴스에 대한 서비스 인증 정보로 요청을 발행해야 합니다.
{: shortdesc}

동일하거나 다른 도메인에 대한 여러 사용자 정의 언어 모델을 작성할 수 있습니다. 그러나 `language_customization_id` 매개변수와 함께 사용하는 경우 하나의 사용자 정의 언어 모델만 지정할 수 있습니다. 사용자 정의 모델에 문법을 사용하는 예제는 [음성 인식에 문법 사용](/docs/services/speech-to-text/grammar-use.html)을 참조하십시오.

-   [WebSocket 인터페이스](/docs/services/speech-to-text/websockets.html)의 경우 `/v1/recognize` 메소드를 사용하십시오. 지정된 사용자 정의 모델이 연결을 통해 전송된 모든 요청에 대해 사용됩니다.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   [동기 HTTP 인터페이스](/docs/services/speech-to-text/http.html)의 경우 `POST /v1/recognize` 메소드를 사용하십시오. 지정된 사용자 정의 모델이 해당 요청에 대해 사용됩니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   [비동기 HTTP 인터페이스](/docs/services/speech-to-text/async.html)의 경우 `POST /v1/recognitions` 메소드를 사용하십시오. 지정된 사용자 정의 모델이 해당 요청에 대해 사용됩니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

사용자 정의 모델이 기본 언어 모델인 `en-US_BroadbandModel`을 기반으로 하는 경우 요청에서 언어 모델을 생략할 수 있습니다. 그렇지 않으면 WebSocket 예제에 표시된 대로 `model` 매개변수를 사용하여 기본 모델을 지정해야 합니다. 사용자 정의 모델은 작성 시 기반이 된 기본 모델에만 사용될 수 있습니다.


## 사용자 정의 가중치 사용
{: #weight}

사용자 정의 언어 모델은 사용자 정의 모델과 이 모델이 사용자 정의하는 기본 모델의 조합입니다. 음성 인식을 위한 기본 모델의 단어와 비교하여 사용자 정의 언어 모델의 단어에 지정할 가중치를 서비스에 알릴 수 있습니다. 사용자 정의 모델에 지정된 가중치를 *사용자 정의 가중치*라고 합니다.

사용자 정의 언어 모델의 상태 가중치를 0.0 - 1.0 사이의 Double로 지정합니다. 기본적으로 각 사용자 정의 언어 모델의 가중치는 0.3입니다. 일반적인 경우 기본 가중치가 최상의 성능을 제공합니다. 사용자 정의 모델의 OOV 단어 및 기본 어휘의 단어가 모두 인식될 수 있습니다.

그러나 변환될 오디오가 사용자 정의 모델의 OOV 단어를 빈번히 사용하는 경우 사용자 정의 가중치를 늘리면 변환 결과의 정확도가 향상될 수 있습니다. 사용자 정의 가중치를 설정할 때 주의하십시오. 가중치를 더 높게 설정하면 사용자 정의 모델 도메인의 구문 정확도가 향상될 수 있지만 도메인이 아닌 구문에 대한 성능에 부정적인 영향을 미칠 수도 있습니다. (가중치를 0.0으로 설정하는 경우에도 언어 모델 사용자 정의의 구현으로 인해 변환에 사용자 정의 단어가 포함될 수 있는 약간의 가능성이 있습니다.)

`customization_weight` 매개변수를 사용하여 사용자 정의 가중치를 지정합니다. 사용자 정의 언어 모델을 훈련하거나 음성 인식 요청에 사용하는 경우 이 매개변수를 지정할 수 있습니다.

-   훈련 요청의 경우, 다음 예제는 `POST /v1/customizations/{customization_id}/train` 메소드를 사용하여 사용자 정의 가중치를 `0.5`로 지정합니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    훈련 중에 사용자 정의 가중치를 설정하면 사용자 정의 언어 모델과 함께 가중치가 저장됩니다. 사용자 정의 모델을 사용하는 각 인식 요청과 함께 가중치를 전달할 필요가 없습니다.

-   인식 요청의 경우, 다음 예제는 `POST /v1/recognize` 메소드를 사용하여 사용자 정의 가중치를 `0.7`로 지정합니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    음성 인식 중에 사용자 정의 가중치를 설정하면 훈련 중에 모델과 함께 저장된 가중치가 대체됩니다.

## 사용자 정의 언어 모델 사용 문제점 해결
{: #languageTroubleshoot}

사용자 정의 언어 모델을 음성 인식에 적용했지만 서비스가 모델에 포함된 단어를 사용 중이지 않은 것으로 나타나면 다음과 같은 가능한 문제점을 확인하십시오.

-   [사용자 정의 언어 모델 사용](#languageUse)에 표시된 대로 사용자 정의 ID를 인식 요청에 올바르게 전달하는지 확인하십시오.
-   사용자 정의 모델의 상태가 `available`인지 확인하십시오. 이는 완전히 훈련되었고 사용할 준비가 되었음을 의미합니다. 자세한 정보는 [사용자 정의 언어 모델 나열](/docs/services/speech-to-text/language-models.html#listModels-language)을 참조하십시오.
-   새 단어에 대해 생성된 발음이 올바른지 확인하십시오. 자세한 정보는 [단어 리소스 유효성 검증](/docs/services/speech-to-text/language-resource.html#validateModel)을 참조하십시오.
