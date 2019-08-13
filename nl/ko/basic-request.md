---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# 인식 요청 작성
{: #basic-request}

{{site.data.keyword.speechtotextfull}} 서비스를 사용하여 음성 인식을 요청하려면 변환될 오디오만 제공해야 합니다. 이 서비스는 각각의 인터페이스(WebSocket 인터페이스, 동기 HTTP 인터페이스 및 비동기 HTTP 인터페이스)에 동일한 기본 변환 기능을 제공합니다.
{: shortdesc}

다음 섹션에서는 각 서비스 인터페이스에 대한 선택적 입력 또는 출력 매개변수가 없는 기본 변환 요청을 보여줍니다.

-   이 예제에서는 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>라는 간단한 FLAC 파일을 제출합니다.
-   이 예제에서는 기본 언어 모델인 `en-US_BroadbandModel`을 사용합니다.

[인식 결과 이해](/docs/services/speech-to-text?topic=speech-to-text-basic-response)에서 이러한 예제에 대한 서비스의 응답에 대해 설명합니다.

## 요청과 함께 오디오 전송
{: #basic-request-audio}

서비스에 전달하는 오디오는 서비스에서 지원되는 형식 중 하나여야 합니다. 대부분의 오디오의 경우 이 서비스가 자동으로 형식을 검색할 수 있습니다. 일부 오디오의 경우 `Content-Type` 또는 동등한 매개변수를 사용하여 형식을 지정해야 합니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)을 참조하십시오. (명확성을 위해 다음 예제에서는 모든 요청에 오디오 형식을 지정합니다.)

WebSocket 및 동기 HTTP 인터페이스를 사용하면 최대 100MB의 오디오 데이터를 단일 요청으로 전달할 수 있습니다. 비동기 HTTP 인터페이스를 사용하면 최대 1GB의 오디오 데이터를 전달할 수 있습니다. 모든 요청에 최소한 100바이트의 오디오를 전송해야 합니다.

대용량의 오디오를 인식 중인 경우 수동으로 오디오를 더 작은 청크로 나눌 수 있습니다. 그러나 일반적으로 오디오를 비압축 손실 형식으로 변환하는 것이 더 효율적이고 편리합니다. 압축은 단일 요청으로 전송할 수 있는 데이터의 양을 최대화할 수 있습니다. 특히 오디오가 WAV 또는 FLAC 형식인 경우 손실 형식으로 변환하면 상당한 차이가 발생할 수 있습니다.

-   압축을 사용하는 오디오 형식에 대한 자세한 정보는 [지원되는 오디오 형식](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#formats)을 참조하십시오.
-   압축의 영향과 압축을 사용하는 형식으로 오디오를 변환하는 방법에 대한 자세한 정보는 [데이터 한계 및 압축](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits) 및 [오디오 변환](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#conversion)을 참조하십시오.

## WebSocket 인터페이스 사용
{: #basic-request-websocket}

[WebSocket 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-websockets)는 전이중 연결을 통해 효율적이며 낮은 대기 시간과 높은 처리량을 제공하는 효율적인 구현을 제공합니다. 모든 요청 및 응답은 동일한 WebSocket 연결을 통해 전송됩니다. 이러한 장점으로 인해 WebSocket이 음성 인식에 선호되는 메커니즘입니다. 자세한 정보는 [WebSocket 인터페이스의 장점](/docs/services/speech-to-text?topic=speech-to-text-developerOverview#advantages)을 참조하십시오.

WebSocket 인터페이스를 사용하려면 먼저 `/v1/recognize` 메소드를 사용하여 서비스와의 연결을 설정해야 합니다. 연결을 통해 전송되는 요청에 사용될 언어 모델 및 모든 사용자 정의 모델과 같은 매개변수를 지정합니다. 그런 다음 서비스의 응답을 처리하기 위해 이벤트 리스너를 등록합니다. 요청을 수행하기 위해 오디오 형식과 추가 매개변수를 포함하는 JSON 텍스트 메시지를 전송합니다. 오디오를 2진 메시지(Blob)로 전달한 후 오디오의 끝을 알리는 텍스트 메시지를 전송합니다.

다음 예제에서는 연결을 설정하고 인식 요청에 대한 텍스트 및 2진 메시지를 전송하는 JavaScript 코드를 제공합니다. 이 예제에는 이벤트 핸들러를 설치하기 위한 코드가 포함되어 있지 않습니다.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  action: 'start',
  content-type: 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({action: 'stop'}));
```
{: codeblock}

## 동기 HTTP 인터페이스 사용
{: #basic-request-sync}

[동기 HTTP 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-http)는 인식 요청을 수행하는 가장 단순한 방법을 제공합니다. `POST /v1/recognize` 메소드를 사용하여 서비스에 대한 요청을 수행합니다. 오디오와 모든 매개변수를 단일 요청으로 전달합니다.

다음 `curl` 예제는 기본 HTTP 인식 요청을 보여줍니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 비동기 HTTP 인터페이스 사용
{: #basic-request-async}

[비동기 HTTP 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-async)는 오디오를 변환하기 위한 비차단 인터페이스를 제공합니다. 서비스에 대한 콜백 URL을 최초 등록하거나 등록하지 않고 이 인터페이스를 사용할 수 있습니다. 이 서비스는 콜백 URL을 사용하여 작업 상태 및 인식 결과가 포함된 콜백 알림을 전송합니다. 이 인터페이스는 사용자 지정 시크릿을 기반으로 하는 HMAC-SHA1 서명을 사용하여 알림을 위한 인증 및 데이터 무결성을 제공합니다. 콜백 URL이 없으면 작업 상태와 결과를 얻기 위해 서비스를 폴링해야 합니다. 두 접근 방식 중 하나에서 `POST /v1/recognitions` 메소드를 사용하여 인식 요청을 수행합니다.

다음 `curl` 예제는 단순 비동기 HTTP 인식 요청을 보여줍니다. 요청에 콜백 URL이 포함되어 있지 않으므로 작업 상태 및 결과 음성 내용을 얻으려면 서비스를 폴링해야 합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}
