---

copyright:
  years: 2015, 2019
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

# 음성 인식에 문법 사용
{: #grammarUse}

사용자 정의 언어 모델을 작성하고 문법으로 훈련한 후 서비스의 WebSocket 및 HTTP 인터페이스를 통해 음성 인식 요청에서 문법을 사용할 수 있습니다.
{: shortdesc}

-   문법이 정의된 사용자 정의 언어 모델의 사용자 정의 ID(GUID)를 지정하려면 `language_customization_id` 매개변수를 사용하십시오. 모델을 소유하는 서비스 인스턴스에 대한 인증 정보로 요청을 발행해야 합니다.
-   문법의 이름을 지정하려면 `grammar_name` 매개변수를 사용하십시오. 요청에 하나의 문법만을 지정할 수 있습니다.

문법을 사용하는 경우 서비스가 지정된 문법의 단어만 인식합니다. 이 서비스는 말뭉치에서 추가되거나, 개별적으로 추가 또는 수정되거나, 다른 문법에서 인식된 사용자 정의 단어를 사용하지 않습니다.

-   [WebSocket 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-websockets)의 경우 먼저 `/v1/recognize` 메소드의 `language_customization_id` 매개변수를 사용하여 사용자 정의 ID를 지정합니다. 이 메소드를 사용하여 서비스에 대한 WebSocket 연결을 설정합니다.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    그런 다음, 활성 연결을 위해 JSON `start` 메시지에서 `grammar_name` 매개변수를 사용하여 문법의 이름을 지정합니다. `start` 메시지와 함께 이 값을 전달하면 연결을 통해 전송하는 각 요청에 대해 동적으로 문법을 변경할 수 있습니다.

    ```javascript
    function onOpen(evt) {
      var message = {
        action: 'start',
        content-type: 'audio/l16;rate=22050',
        grammar_name: '{grammar_name}'
      };
      websocket.send(JSON.stringify(message));
      websocket.send(blob);
    }
    ```
    {: codeblock}
-   [동기 HTTP 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-http)의 경우 `POST /v1/recognize` 메소드와 함께 두 매개변수를 모두 전달하십시오.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   [비동기 HTTP 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-async)의 경우 `POST /v1/recognitions` 메소드와 함께 두 매개변수를 모두 전달하십시오.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
