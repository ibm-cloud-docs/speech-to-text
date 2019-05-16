---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# WebSocket 인터페이스
{: #websockets}

{{site.data.keyword.speechtotextshort}} 서비스의 WebSocket 인터페이스는 클라이언트가 서비스와 상호작용하는 가장 자연스러운 방법입니다. WebSocket 인터페이스를 음성 인식에 사용하려면 먼저 `/v1/recognize` 메소드를 사용하여 서비스와의 지속적 연결을 설정합니다. 그런 다음 연결을 통해 텍스트 및 2진 메시지를 전송하여 인식 요청을 시작하고 관리합니다.
{: shortdesc}

인식 요청 및 응답 주기의 단계는 다음과 같습니다.

1.  [연결 열기](#WSopen)
1.  [인식 요청 시작](#WSstart)
1.  [오디오 전송 및 인식 결과 수신](#WSaudio)
1.  [인식 요청 종료](#WSstop)
1.  [추가 요청 전송 및 요청 매개변수 수정](#WSmore)
1.  [연결 활성 상태 유지](#WSkeep)
1.  [연결 닫기](#WSclose)

클라이언트가 서비스에 데이터를 전송할 때 *반드시* 모든 JSON 메시지를 텍스트 메시지로 전달하고 모든 오디오 데이터를 2진 메시지로 전달해야 합니다.

다음의 예제 코드 스니펫은 JavaScript로 작성되었으며 HTML5 WebSocket API를 기반으로 합니다. WebSocket 프로토콜에 대한 자세한 정보는 IETF(Internet Engineering Task Force) [RFC(Request for Comment) 6455 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://tools.ietf.org/html/rfc6455){: new_window}를 참조하십시오.
{: note}

## 연결 열기
{: #WSopen}

{{site.data.keyword.speechtotextshort}} 서비스는 WSS(WebSocket Secure) 프로토콜을 사용하여 `/v1/recognize` 메소드를 다음 엔드포인트에서 사용할 수 있도록 합니다.

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

여기서 `{host_name}`은 애플리케이션이 호스팅되는 위치입니다.

-   `stream.watsonplatform.net` - 댈러스(다음 예제에서는 이 호스트 이름을 사용함)
-   `stream-fra.watsonplatform.net` - 프랑크푸르트
-   `gateway-syd.watsonplatform.net` - 시드니
-   `gateway-wdc.watsonplatform.net` - 워싱턴 DC
-   `gateway-tok.watsonplatform.net` - 도쿄
-   `gateway-lon.watsonplatform.net` - 런던

WebSocket 클라이언트는 다음 조회 매개변수와 함께 이 메소드를 호출하여 서비스에 대한 인증된 연결을 설정합니다. IAM(Identity and Access Management) 인증을 사용하는 경우 `access_token` 조회 매개변수를 사용하십시오. Cloud Foundry 서비스 인증 정보를 사용하는 경우 `watson-token` 조회 매개변수를 사용하십시오.

<table>
  <caption>표 1. <code>/v1/recognize</code> 메소드의 매개변수</caption>
  <tr>
    <th style="width:23%; text-align:left">매개변수</th>
    <th style="width:12%; text-align:center">데이터 유형</th>
    <th style="text-align:left">설명</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      <em>IAM 인증을 사용하는 경우</em> 서비스에 대한 인증된 연결을 설정하려면 유효한 IAM 액세스 토큰을 전달하십시오.
      호출 시 API 키를 전달하는 대신 IAM 액세스 토큰을 전달합니다. 액세스 토큰이 만료되기 전에 연결을 설정해야 합니다. 액세스 토큰을 얻는 방법에 대한 정보는 [IAM 토큰을 사용한 인증](/docs/services/watson/getting-started-iam.html)을 참조하십시오.<br/><br/>
      인증된 연결을 설정하기 위해서만 액세스 토큰을 전달합니다.
      연결을 설정하면 무기한으로 활성 상태로 유지할 수 있습니다.
      연결이 열려 있는 동안에는 계속 인증된 상태로 유지됩니다.
      토큰 만기 시간 이후 지속되는 활성 연결에 대한 액세스 토큰을 새로 고칠 필요가 없습니다.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      <em>Cloud Foundry 서비스 인증 정보를 사용하는 경우</em> 서비스에 대한 인증된 연결을 설정하려면 유효한 {{site.data.keyword.watson}} 인증 토큰을 전달하십시오. 호출 시 서비스 인증 정보를 전달하는 대신 {{site.data.keyword.watson}} 토큰을 전달합니다. {{site.data.keyword.watson}} 토큰은 HTTP 기본 인증에 `username` 및 `password`를 사용하는 Cloud Foundry 서비스 인증 정보를 기반으로 합니다. {{site.data.keyword.watson}} 토큰을 얻는 방법에 대한 정보는 [{{site.data.keyword.watson}} 토큰](/docs/services/watson/getting-started-tokens.html)을 참조하십시오.<br/><br/>
      인증된 연결을 설정하기 위해서만 {{site.data.keyword.watson}} 토큰을 전달합니다. 연결을 설정하면 무기한으로 활성 상태로 유지할 수 있습니다. 연결이 열려 있는 동안에는 계속 인증된 상태로 유지됩니다.
      </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      변환에 사용될 언어 모델을 지정합니다.
      모델을 지정하지 않으면 서비스에서는 기본적으로 <code>en-US_BroadbandModel</code> 모델을 사용합니다. 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      연결을 통해 전송되는 모든 요청에 사용될 사용자 정의 언어 모델의 GUID(Globally Unique Identifier)를 지정합니다. 사용자 정의 언어 모델의 기본 모델은 <code>model</code> 매개변수의 값과 일치해야 합니다. 기본적으로 사용자 정의 언어 모델은 사용되지 않습니다. 자세한 정보는 [사용자 정의 인터페이스](/docs/services/speech-to-text/custom.html)를 참조하십시오.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      연결을 통해 전송되는 모든 요청에 사용될 사용자 정의 음향 모델의 GUID(Globally Unique Identifier)를 지정합니다. 사용자 정의 음향 모델의 기본 모델은 <code>model</code> 매개변수의 값과 일치해야 합니다. 기본적으로 사용자 정의 음향 모델은 사용되지 않습니다. 자세한 정보는 [사용자 정의 인터페이스](/docs/services/speech-to-text/custom.html)를 참조하십시오.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      연결을 통해 전송되는 모든 요청에 사용될 기본 `model`의 버전을 지정합니다. 이 매개변수는 주로 새 기본 모델용으로 업그레이드된 사용자 정의 모델과 함께 사용하기 위한 것입니다. 기본값은 매개변수가 사용자 정의 모델과 함께 사용되는지 여부에 따라 다릅니다. 자세한 정보는 [기본 모델 버전](/docs/services/speech-to-text/input.html#version)을 참조하십시오.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">부울</td>
    <td style="text-align:left">
      서비스가 연결을 통해 전송된 요청 및 결과를 로그하는지 여부를 표시합니다. IBM에서 일반적인 서비스 개선을 위해 데이터에 액세스하지 못하게 하려면 이 매개변수를 <code>true</code>로 지정하십시오. 자세한 정보는 [요청 로깅](/docs/services/speech-to-text/input.html#logging)을 참조하십시오.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      연결을 통해 전달되는 모든 데이터와 고객 ID를 연관시킵니다. 이 매개변수는 <code>customer_id={id}</code> 인수를 받아들입니다. 여기서 <code>id</code>는 데이터와 연관될 랜덤 또는 일반 문자열입니다. 매개변수에 대한 인수를 URL 인코딩해야 합니다(예: `customer_id%3dmy_customer_ID`). 기본적으로 고객 ID는 데이터와 연관되지 않습니다. 자세한 정보는 [정보 보안](/docs/services/speech-to-text/information-security.html)을 참조하십시오.
    </td>
  </tr>
</table>

다음 JavaScript 코드 스니펫은 서비스와의 연결을 엽니다. `/v1/recognize` 메소드에 대한 호출이 `access_token` 및 `model` 조회 매개변수를 전달하며, model 매개변수는 스페인어 광대역 모델을 사용하도록 서비스에 지시합니다. 클라이언트가 연결을 설정한 후 서비스의 이벤트에 응답하기 위해 이벤트 리스너(`onOpen`, `onClose` 등)를 정의합니다. 클라이언트는 여러 인식 요청에 대해 연결을 사용할 수 있습니다.

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

## 인식 요청 시작
{: #WSstart}

인식 요청을 시작하기 위해 클라이언트가 설정된 연결을 통해 서비스에 JSON 텍스트 메시지를 전송합니다. 클라이언트는 변환을 위해 오디오를 전송하기 전에 이 메시지를 전송해야 합니다. 이 메시지에는 `action` 매개변수가 포함되어야 하지만 일반적으로 `content-type` 매개변수를 생략할 수 있습니다.

<table>
  <caption>표 2. JSON 텍스트 메시지의 매개변수</caption>
  <tr>
    <th style="width:23%; text-align:left">매개변수</th>
    <th style="width:12%; text-align:center">데이터 유형</th>
    <th style="text-align:left">설명</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>필수</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      수행될 조치를 지정합니다.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code>는 인식 요청을 시작하거나 후속 요청에 대한 새 매개변수를 지정합니다. 자세한 정보는 [추가 요청 전송 및 요청 매개변수 수정](#WSmore)을 참조하십시오.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code>은 요청에 대한 모든 오디오가 전송되었다는 신호를 보냅니다. 자세한 정보는 [인식 요청 종료](#WSstop)를 참조하십시오.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td style="text-align:left">
      요청에 대한 오디오 데이터의 형식(MIME 유형)을 식별합니다.
      이 매개변수는 `audio/alaw`, `audio/basic`, `audio/l16` 및 `audio/mulaw` 형식에 필요합니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
    </td>
  </tr>
</table>

메시지에는 요청이 처리되는 방법 및 리턴될 정보의 다른 측면을 지정하기 위한 선택적 매개변수가 포함될 수도 있습니다. 모든 입력 및 출력 기능에 대한 정보는 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오. 언어 모델, 사용자 정의 언어 모델 및 사용자 정의 음향 모델을 WebSocket URL의 조회 매개변수로만 지정할 수 있습니다.

다음 JavaScript 코드 스니펫은 WebSocket 연결을 통해 인식 요청에 대한 초기화 매개변수를 전송합니다. 호출은 연결이 설정된 후에만 전송되도록 클라이언트의 `onOpen` 함수에 포함됩니다.

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

서비스가 요청을 성공적으로 수신하면 다음과 같은 텍스트 메시지를 리턴하여 `listening` 상태임을 표시합니다.

```javascript
{'state': 'listening'}
```
{: codeblock}

`listening` 상태는 서비스 인스턴스가 구성되었고(JSON `start` 메시지가 유효함) 인식 요청에 대한 새 발화를 처리할 준비가 되었음을 표시합니다. 청취가 시작되면 서비스가 `listening` 메시지 이전에 전송된 오디오를 처리합니다.

클라이언트가 인식 요청을 위해 올바르지 않은 조회 매개변수 또는 JSON 필드를 지정하면 서비스의 JSON 응답에 `warnings` 필드가 포함됩니다. 이 필드는 각각의 올바르지 않은 인수에 대해 설명합니다. 경고가 발생해도 요청에 성공합니다.

## 오디오 전송 및 인식 결과 수신
{: #WSaudio}

초기 `start` 메시지를 전송한 후 클라이언트가 오디오 데이터를 서비스에 전송하기 시작할 수 있습니다. 클라이언트는 서비스가 `listening` 메시지로 `start` 메시지에 응답할 때까지 대기할 필요가 없습니다. 이 서비스는 HTTP 인터페이스에 대한 결과를 리턴하는 것과 동일한 형식으로 변환 결과를 비동기식으로 리턴합니다.

클라이언트는 오디오를 2진 데이터로 전송해야 합니다. 클라이언트는 `send` 요청당 하나의 발화로 최대 100MB의 오디오 데이터를 전송할 수 있습니다. 모든 요청에 대해 최소한 100바이트의 오디오를 전송해야 합니다. 클라이언트는 단일 WebSocket 연결을 통해 여러 발화를 전송할 수 있습니다. 압축을 사용하여 요청과 함께 서비스에 전달할 수 있는 오디오의 양을 최대화하는 방법에 대한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.

WebSocket 인터페이스는 최대 4MB의 프레임 크기를 부과합니다. 클라이언트는 최대 프레임 크기를 4MB 미만으로 설정할 수 있습니다. 프레임 크기를 설정하는 것이 실용적이지 않은 경우 클라이언트가 최대 메시지 크기를 4MB 미만으로 설정하고 오디오 데이터를 일련의 메시지로 전송할 수 있습니다.

다음 JavaScript 코드 스니펫은 오디오 데이터를 2진 메시지(blob)로 서비스에 전송합니다.

```javascript
websocket.send(blob);
```
{: codeblock}

다음 스니펫은 서비스가 비동기식으로 리턴하는 인식 가설을 수신합니다. 클라이언트의 `onMessage` 함수에서 결과를 처리합니다.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## 인식 요청 종료
{: #WSstop}

요청에 대한 오디오 데이터를 서비스에 전송하는 작업이 완료되면 클라이언트가 다음 방법 중 하나로 서비스에 2진 오디오 전송의 종료 신호를 *반드시* 보내야 합니다.

-   `action` 매개변수가 `stop` 값으로 설정된 JSON 텍스트 메시지 전송:

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   비어 있는 2진 메시지(지정된 blob이 비어 있음) 전송:

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

오디오 전송이 완료되었다는 확인을 수신할 때까지 서비스가 최종 결과를 전송하지 않습니다. 전송이 완료되었다는 신호를 보내는 데 실패하면 서비스가 최종 결과를 전송하지 않고 연결 제한시간이 초과될 수 있습니다.

여러 인식 요청 사이에 최종 결과를 수신하려면 클라이언트가 후속 요청을 전송하기 전에 이전 요청에 대한 전송이 종료되었다는 신호를 보내야 합니다. 서비스가 첫 번째 요청에 대한 최종 결과를 리턴한 후 다른 `{"state":"listening"}` 메시지를 클라이언트에 리턴합니다. 이 메시지는 서비스가 다른 요청을 수신할 준비가 되었음을 표시합니다.

## 추가 요청 전송 및 요청 매개변수 수정
{: #WSmore}

WebSocket 연결이 활성 상태로 유지되는 동안 클라이언트가 계속해서 연결을 사용하여 새 오디오로 추가 인식 요청을 전송할 수 있습니다. 기본적으로 서비스는 동일한 연결을 통해 전송된 모든 후속 요청에 대해 이전 `start` 메시지와 함께 전송된 매개변수를 계속 사용합니다.

클라이언트는 후속 요청에 대한 매개변수를 변경하기 위해 최종 인식 결과와 `{"state":"listening"}` 메시지를 서비스에서 수신한 후 다른 `start` 메시지를 새 매개변수와 함께 전송할 수 있습니다. 클라이언트는 연결이 열릴 때 지정된 매개변수(`model`, `language_customization_id` 등)를 제외한 모든 매개변수를 변경할 수 있습니다.

다음 예제에서는 연결을 통해 전송된 후속 인식 요청에 대한 새 매개변수가 포함된 `start` 메시지를 전송합니다. 이 메시지는 이전 예제와 동일한 `content-type`을 지정하지만 변환 단어에 대한 신뢰도 측정값 및 시간소인을 리턴하도록 서비스에 지시합니다.

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## 연결 활성 상태 유지
{: #WSkeep}

비활성 또는 세션 제한시간에 도달하면 서비스가 세션을 종료하고 연결을 닫습니다.

-   클라이언트가 오디오를 전송 중이지만 서비스가 음성을 감지하지 못하는 경우 *비활성 제한시간 초과*가 발생합니다. 기본적으로 비활성 제한시간은 30초입니다. `inactivity_timeout` 매개변수를 사용하여 제한시간을 무한대로 설정하는 `-1`을 포함한 다른 값으로 지정할 수 있습니다. 
-   서비스가 클라이언트로부터 데이터를 수신하지 않거나 30초 동안 중간 결과를 전송하지 않으면 *세션 제한시간 초과*가 발생합니다. 이 제한시간의 기간을 변경할 수는 없지만 제한시간 초과가 발생하기 전에 무음을 포함한 오디오 데이터를 서비스에 전송하여 세션을 연장할 수 있습니다. 또한 `inactivity_timeout`을 `-1`로 설정해야 합니다. 세션을 연장하기 위해 전송하는 무음을 포함하여 서비스에 전송하는 모든 데이터의 지속 시간 동안 비용이 청구됩니다.

자세한 정보는 [제한시간](/docs/services/speech-to-text/input.html#timeouts)을 참조하십시오.

## 연결 닫기
{: #WSclose}

클라이언트가 서비스와의 상호작용을 완료하면 WebSocket 연결을 닫을 수 있습니다. 연결이 닫히면 클라이언트가 더 이상 요청을 전송하거나 결과를 수신할 수 없습니다. 요청에 대한 모든 결과를 수신한 후에만 연결을 닫으십시오. 결국 연결이 제한시간 초과되고 명시적으로 닫지 않아도 닫힙니다.

다음 JavaScript 코드 스니펫은 열린 연결을 닫습니다.

```javascript
websocket.close();
```
{: codeblock}

## WebSocket 리턴 코드
{: #WSreturn}

이 서비스는 WebSocket 연결을 통해 클라이언트에 다음과 같은 리턴 코드를 전송할 수 있습니다.

-   `1000`은 연결이 정상적으로 종료되었음을 표시합니다. 이는 연결이 설정된 목적이 충족되었음을 의미합니다.
-   `1002`는 서비스가 프로토콜 오류로 인해 연결을 닫는 중임을 표시합니다.
-   `1006`은 연결이 비정상적으로 닫혔음을 표시합니다.
-   `1009`는 프레임 크기가 4MB 한계를 초과했음을 표시합니다.
-   `1011`은 요청을 수행하지 못하게 하는 예기치 않은 조건이 발생하여 서비스가 연결을 종료 중임을 표시합니다.

소켓이 오류로 닫히면 소켓이 닫히기 전에 클라이언트가 `{"error":"{message}"}` 양식의 정보 메시지를 수신합니다. WebSocket 리턴 코드에 대한 자세한 정보는 [IETF RFC 6455 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://tools.ietf.org/html/rfc6455){: new_window}를 참조하십시오.

## 예제 WebSocket 세션
{: #WSexample}

다음 예제에서는 클라이언트와 {{site.data.keyword.speechtotextshort}} 서비스 간의 WebSocket 세션을 보여줍니다. 보다 쉽게 이해할 수 있도록 세션이 세 개의 별도 교환으로 표시됩니다. 그러나 세 개의 교환 모두 서비스와의 단일 세션의 일부입니다. 이 예제는 메시지 교환에 중점을 두고 있으며 연결 열기 및 닫기는 표시하지 않습니다.

### 첫 번째 예제 교환
{: #firstExample}

첫 번째 예제 교환에서는 클라이언트가 `Name the Mayflower`라는 문자열이 포함된 오디오를 전송합니다. 클라이언트가 PCM(`audio/l16`) 오디오 데이터의 단일 청크로 2진 메시지를 전송합니다. 이는 필수 샘플링 속도를 표시합니다. 클라이언트는 오디오 데이터 전송을 시작하고 요청 끝 신호를 보내기 위해 서비스의 `{"state":"listening"}` 응답을 대기하지 않습니다. 데이터를 즉시 전송하면 인식 요청을 처리할 준비가 되자마자 오디오를 서비스에 사용할 수 있으므로 대기 시간이 단축됩니다.

-   *클라이언트*가 다음을 전송합니다.

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *서비스*가 다음과 같이 응답합니다.

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 두 번째 예제 교환
{: #secondExample}

두 번째 교환에서는 클라이언트가 `Second audio transcript`라는 문자열이 포함된 오디오를 전송합니다. 클라이언트가 단일 2진 메시지로 오디오를 전송하고 첫 번째 요청에서 지정한 것과 동일한 매개변수를 사용합니다.

-   *클라이언트*가 다음을 전송합니다.

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *서비스*가 다음과 같이 응답합니다.

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 세 번째 예제 교환
{: #thirdExample}

세 번째 교환에서는 클라이언트가 `Name the Mayflower`라는 문자열이 포함된 오디오를 다시 전송합니다. 클라이언트가 PCM 오디오 데이터의 단일 청크로 2진 메시지를 다시 전송합니다. 그러나 이번에는 클라이언트가 변환을 포함한 중간 결과를 요청합니다.

-   *클라이언트*가 다음을 전송합니다.

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *서비스*가 다음과 같이 응답합니다.

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
