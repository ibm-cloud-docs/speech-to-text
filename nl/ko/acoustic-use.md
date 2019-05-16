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

# 사용자 정의 음향 모델 사용
{: #acousticUse}

사용자 정의 음향 모델을 작성하고 훈련한 후 음성 인식 요청에서 사용할 수 있습니다. 다음 예제에 표시된 대로 `acoustic_customization_id` 조회 매개변수를 사용하여 요청에 대한 사용자 정의 음향 모델을 지정합니다. 모델을 소유하는 서비스 인스턴스에 대한 서비스 인증 정보로 요청을 발행해야 합니다.
{: shortdesc}

또한 요청에 사용될 사용자 정의 모델을 지정할 수 있으며, 이를 통해 변환 정확도를 향상시킬 수 있습니다. 자세한 정보는 [음성 인식 중에 사용자 정의 언어 및 사용자 정의 음향 모델 사용](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize)을 참조하십시오.

동일하거나 다른 도메인 또는 환경에 대한 여러 사용자 정의 음향 모델을 작성할 수 있습니다. 그러나 `acoustic_customization_id` 매개변수와 함께 사용하는 경우 하나의 사용자 정의 음향 모델만 지정할 수 있습니다.

-   [WebSocket 인터페이스](/docs/services/speech-to-text/websockets.html)의 경우 `/v1/recognize` 메소드를 사용하십시오. 지정된 사용자 정의 모델이 연결을 통해 전송된 모든 요청에 대해 사용됩니다.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   [동기 HTTP 인터페이스](/docs/services/speech-to-text/http.html)의 경우 `POST /v1/recognize` 메소드를 사용하십시오. 지정된 사용자 정의 모델이 해당 요청에 대해 사용됩니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   [비동기 HTTP 인터페이스](/docs/services/speech-to-text/async.html)의 경우 `POST /v1/recognitions` 메소드를 사용하십시오. 지정된 사용자 정의 모델이 해당 요청에 대해 사용됩니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

사용자 정의 모델이 기본 모델인 `en-US_BroadbandModel`을 기반으로 하는 경우 요청에서 언어 모델을 생략할 수 있습니다. 그렇지 않으면 WebSocket 예제에 표시된 대로 `model` 매개변수를 사용하여 기본 모델을 지정해야 합니다. 사용자 정의 모델은 작성 시 기반이 된 기본 모델에만 사용될 수 있습니다.

