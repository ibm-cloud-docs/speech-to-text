---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 비동기 HTTP 인터페이스
{: #async}

{{site.data.keyword.speechtotextfull}} 서비스의 비동기 HTTP 인터페이스는 서비스에 대한 비차단 호출을 통해 오디오를 변환하기 위한 메소드를 제공합니다. 이 인터페이스는 사용자 정의 시크릿 문자열 및 디지털 서명을 사용하여 HTTP 프로토콜을 통해 수행되는 요청에 대한 보안 레벨을 제공합니다. 비동기 인터페이스를 사용하기 위해 다음을 수행할 수 있습니다.
{: shortdesc}

-   서비스가 자동으로 작업 상태 및 결과를 알릴 콜백 URL을 등록합니다.
-   서비스를 폴링하여 수동으로 작업 상태 및 결과를 얻습니다.

두 가지 접근 방식은 상호 배타적이지 않습니다. 콜백 알림을 수신하도록 선택하지만 여전히 서비스를 폴링하여 최신 상태를 얻거나 서비스에 접속하여 수동으로 결과를 검색할 수 있습니다. 다음 섹션에서는 각 접근 방식으로 비동기 HTTP 인터페이스를 사용하는 방법에 대해 설명합니다.

최대 1GB 및 최소 100바이트의 오디오 데이터를 단일 요청으로 제출하십시오. 오디오 형식에 대한 정보와 압축을 사용하여 요청과 함께 전송할 수 있는 오디오의 양을 최대화하는 방법에 대한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오. 인터페이스의 개별 메소드에 대한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

## 사용 모델
{: #usage}

서비스의 비동기 HTTP 인터페이스에 대해 작업할 때 다음과 같은 방법으로 작업 상태에 대해 알아보고 결과를 수신하도록 선택할 수 있습니다.

-   콜백 알림을 사용하여 다음을 수행하십시오.
    1.  `POST /v1/register_callback` 메소드를 호출하여 콜백 URL을 서비스에 등록하십시오. URL에 전송된 콜백에 대한 인증 및 데이터 무결성을 사용하도록 선택적 사용자 지정 시크릿 문자열을 제공할 수 있습니다.
    1.  작업 상태가 변경될 때 서비스가 알림을 전송하는 이미 등록된 콜백 URL을 사용하여 `POST /v1/recognitions` 메소드를 호출하십시오. 알림을 받을 이벤트의 목록을 지정합니다. 기본적으로 서비스는 작업 시작 시, 작업 완료 시 및 오류 발생 시 알림을 전송합니다. 완료 알림에서 요청의 결과를 요청할 수도 있습니다. 그렇지 않으면 `GET /v1/recognitions/{id}` 메소드를 사용하여 결과를 검색해야 합니다.
-   서비스를 폴링하여 다음을 수행하십시오.
    1.  콜백 URL, 이벤트 또는 사용자 토큰 없이 `POST /v1/recognitions` 메소드를 호출하십시오.
    1.  주기적으로 `GET /v1/recognitions` 메소드를 호출하여 최신 작업의 상태를 확인하거나 `GET /v1/recognitions/{id}` 메소드를 사용하여 특정 작업의 상태를 확인하십시오.
    1.  `GET /v1/recognitions` 메소드로 작업 상태를 확인하는 경우 작업이 완료된 후 `GET /v1/recognitions/{id}` 메소드를 사용하여 작업의 결과를 검색하십시오.

두 가지 접근 방식을 함께 사용할 수 있습니다. 여전히 서비스를 폴링하여 콜백 URL로 작성된 작업에 대한 최신 상태를 얻을 수 있습니다. 예를 들어, 알림이 도착하는 데 시간이 걸리는 경우 작업의 상태를 얻을 수 있습니다. 서비스 또는 네트워크 오류로 인해 하나 이상의 알림이 누락된 것으로 의심되는 경우에 상태를 확인할 수도 있습니다.

## 콜백 URL 등록
{: #register}

`POST /v1/register_callback` 메소드를 호출하여 콜백 URL을 등록할 수 있습니다. 콜백 URL을 등록하면 이 URL을 사용하여 무제한 수의 작업에 대한 알림을 받을 수 있습니다. 등록 프로세스는 다음 네 단계로 구성됩니다.

1.  `POST /v1/register_callback` 메소드를 호출하고 콜백 URL을 전달합니다. 선택적으로 사용자 지정 시크릿을 지정할 수도 있습니다. 이 서비스는 시크릿을 사용하여 인증 및 데이터 무결성을 위한 키 해시 메시지 인증 코드(HMAC) SHA1(Secure Hash Algorithm 1) 서명을 계산합니다. 다음 예제에서는 URL `http://{user_callback_path}/results`에서 응답하는 사용자 콜백을 등록합니다. 호출에 `ThisIsMySecret`이라는 사용자 시크릿이 포함되어 있습니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  이 서비스는 콜백 URL에 `GET` 요청을 전송하여 아직 등록되지 않은 경우 콜백 URL의 유효성을 검증하거나 화이트리스트에 추가하려고 시도합니다. 이 서비스는 요청의 `challenge_string` 조회 매개변수를 통해 랜덤 영숫자 인증 확인 문자열을 전달합니다. 요청에는 `text/plain`을 필수 응답 유형으로 지정하는 `Accept` 헤더가 포함되어 있습니다.

    `register_callback` 메소드에 대한 호출에 사용자 시크릿이 포함된 경우 서비스로부터의 `GET` 요청에도 인증 확인 문자열의 HMAC-SHA1 서명을 지정하는 `X-Callback-Signature` 헤더가 포함됩니다. 이 서비스는 사용자 시크릿을 키로 사용하여 서명을 계산합니다.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  상태 코드 200으로 서비스의 `GET` 요청에 응답합니다. 서비스에서 전송된 인증 확인 문자열을 응답에 포함시키십시오. 일반 텍스트의 문자열을 응답 본문에 포함시키고 `Content-Type` 응답 헤더를 `text/plain`으로 설정하십시오.

    초기 `POST` 요청에 사용자 시크릿이 포함된 경우 이 시크릿을 키로 사용하여 인증 확인 문자열의 HMAC-SHA1 서명을 계산할 수 있습니다. 서비스에서 `GET` 요청이 전송된 경우 서명이 `X-Callback-Signature` 헤더에 지정된 값과 일치합니다.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  이 서비스는 해당 `GET` 요청에 대한 응답의 본문에 인증 확인 문자열이 리턴되는지 여부를 확인합니다. 그러한 경우 서비스가 콜백 URL을 화이트리스트에 추가하고 상태 코드 201로 원래 `POST` 요청에 응답합니다. 요청의 본문에 `status` 필드의 값이 `created`이고 `url` 필드의 값이 해당 콜백 URL인 JSON 오브젝트가 포함됩니다.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

이 서비스는 등록 프로세스 중에 콜백 URL에 대한 단일 `GET` 요청만 전송합니다. 서비스가 5초 이내에 본문에 인증 코드 문자열이 포함된 응답을 응답 코드 200으로 수신해야 합니다. 그렇지 않으면 URL을 화이트리스트에 추가하지 않습니다. 대신, `POST /v1/register_callback` 요청에 대한 응답으로 상태 코드 400을 전송합니다. 콜백 URL이 이미 화이트리스트에 있는 경우 서비스가 초기 `POST` 요청에 대한 응답으로 상태 코드 200을 전송합니다.

### 보안 고려사항
{: #security-async}

`POST /v1/register_callback` 메소드를 사용하여 콜백 URL을 등록한 경우 서비스가 URL을 화이트리스트에 추가하여 해당 URL이 콜백 알림에 사용할 수 있도록 확인되었음을 표시합니다. 등록 호출을 사용하여 사용자 시크릿을 지정하는 경우 화이트리스트에 추가하는 것은 보안 강화를 위해 해당 URL의 유효성이 검증되었음을 의미합니다. 사용자 시크릿을 지정하면 비동기 HTTP 인터페이스를 사용하여 콜백 URL을 사용하는 요청에 대한 인증 및 데이터 무결성이 제공됩니다.

이 서비스는 사용자 시크릿을 사용하여 URL에 전송하는 모든 콜백 알림의 페이로드에 대한 HMAC-SHA1 서명을 계산합니다. 서비스가 각 알림과 함께 `X-Callback-Signature` 헤더를 통해 서명을 전송합니다. 클라이언트는 시크릿을 사용하여 각 알림 페이로드의 고유 서명을 계산할 수 있습니다. 해당 서명이 `X-Callback-Signature` 헤더의 값과 일치하는 경우 클라이언트는 서비스에서 알림이 전송되었고 알림 컨텐츠가 전송 중에 변경되지 않았음을 알게 됩니다. 이를 통해 클라이언트가 중간자 공격의 대상자가 아님이 보증됩니다.

HTTPS는 프로덕션 애플리케이션에 적합합니다. 그러나 애플리케이션 개발 및 프로토타입 작성 중에 서비스에서 지원되는 HTTP 기반 콜백 알림은 HTTPS의 비용을 방지하여 개발 프로세스를 단순화하고 가속화할 수 있습니다.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### 콜백 URL 등록 취소
{: #unregister}

언제든지 `POST /v1/unregister_callback` 메소드를 호출하여 화이트리스트에 있는 콜백 URL의 등록을 취소할 수 있습니다. 콜백 URL의 등록 취소는 서비스를 사용하여 애플리케이션을 테스트하는 데 유용할 수 있습니다. 콜백 URL의 등록을 취소하면 콜백 URL을 비동기 인식 요청에 더 이상 사용할 수 없습니다.

## 작업 작성
{: #create}

`POST /v1/recognitions` 메소드를 호출하여 인식 작업을 작성할 수 있습니다. 작업의 상태 및 결과를 알아보는 방법은 사용하는 접근 방식과 전달하는 매개변수에 따라 달라집니다.

-   *콜백 알림을 사용하려면* 작업 상태가 변경될 때 서비스가 콜백 알림을 전송하는 URL을 지정하는 `callback_url` 조회 매개변수를 포함시키십시오. 다음과 같은 선택적 조회 매개변수를 지정할 수도 있습니다.
    -   알림 이벤트 목록에 등록하기 위한 `events`. 기본적으로 서비스는 작업 시작 시(`recognitions.started` 이벤트), 작업 완료 시(`recognitions.completed` 이벤트) 및 오류 발생 시(`recognitions.failed` 이벤트) 콜백 알림을 전송합니다. 이벤트의 서브세트를 지정하거나 `recognitions.completed` 이벤트 대신 `recognitions.completed_with_results` 이벤트를 사용하여 작업 완료 알림에 결과를 포함시킬 수 있습니다.
    -   작업에 대한 각 알림에 포함될 문자열을 지정하기 위한 `user_token`. 무제한 수의 작업에 동일한 콜백 URL을 사용할 수 있으므로 사용자 토큰을 사용하여 여러 작업에 대한 알림을 구별할 수 있습니다.
-   *폴링을 사용하려면* `callback_url`, `events` 및 `user_token` 조회 매개변수를 생략하십시오. 그런 다음 `GET /v1/recognitions` 또는 `GET /v1/recognitions/{id}` 메소드를 사용하여 작업의 상태를 확인해야 합니다. 작업이 완료되면 후자를 사용하여 결과를 검색합니다.

두 경우 모두에 `results_ttl` 조회 매개변수를 포함하여 작업이 완료된 후 결과가 사용 가능한 상태로 유지되는 시간(분)을 지정할 수 있습니다.

비동기 인터페이스에 특정한 이전 매개변수 이외에 `POST /v1/recognitions` 메소드는 WebSocket 및 동기 HTTP 인터페이스와 동일한 매개변수 중 대부분을 지원됩니다. 자세한 정보는 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오.

### 콜백 알림
{: #notifications}

작업이 콜백 URL로 작성된 경우 지정된 이벤트가 발생하면 서비스가 등록된 URL에 HTTP `POST` 콜백 알림을 전송합니다. 기본 알림의 본문은 다음과 같은 구조의 JSON 오브젝트로 구성됩니다.

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

`id` 필드는 콜백을 생성한 작업의 ID를 식별하며 `event` 필드는 콜백을 트리거한 이벤트를 식별합니다. `user_token` 필드에는 작업에 대한 사용자 토큰이 포함됩니다(지정된 경우). 그렇지 않으면 이 필드는 비어 있는 문자열입니다. 이벤트가 `recognitions.completed_with_results`이면 인식 요청의 결과를 제공하는 `results` 필드가 오브젝트에 포함됩니다. 클라이언트가 상태 코드 200으로 콜백 알림에 응답할 수 있습니다.

콜백 URL이 사용자 시크릿으로 등록된 경우 서비스가 `X-Callback-Signature` 헤더를 콜백 알림과 함께 전송합니다. 이 헤더는 요청 본문의 HMAC-SHA1 서명을 지정합니다. 서비스가 사용자 시크릿을 키로 사용하여 서명을 계산합니다. 클라이언트는 콜백 알림에 대한 페이로드의 서명을 계산하여 헤더의 서명과 일치하는지 확인할 수 있습니다. 예를 들어, 다음의 단순한 Python 코드는 `notification_payload` 문자열에 대한 서명을 계산합니다.

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### 콜백 예제
{: #callback}

다음 예제에서는 이전에 화이트리스트에 추가된 콜백 URL `http://{user_callback_path}/results`와 연관된 작업을 작성합니다. 이 예제에서는 사용자 토큰 `job25`를 전달하여 서비스에서 전송된 콜백 알림의 작업을 식별합니다. 이 호출은 기본 이벤트를 사용하므로 서비스가 작업이 완료되었음을 표시하기 위해 콜백 알림을 전송할 때 `GET /v1/recognitions/{id}` 메소드를 호출하여 결과를 검색해야 합니다. 이 호출은 인식 요청의 `timestamps` 조회 매개변수를 `true`로 설정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

서비스가 `waiting`인 요청의 상태를 리턴하여 서비스가 처리를 위해 작업을 준비 중임을 표시합니다. 응답에는 작성 시간, 작업 ID 및 작업에 대한 자세한 정보를 얻을 수 있는 URL이 포함됩니다.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### 폴링 예제
{: #polling}

다음 예제에서는 콜백 URL과 연관되지 않은 작업을 작성합니다. 사용자가 서비스를 폴링하여 작업이 완료된 시기를 알아본 다음 `GET /v1/recognitions/{id}` 메소드를 사용하여 결과를 검색해야 합니다. 이전 예제와 마찬가지로 호출이 인식 매개변수의 `timestamps` 매개변수를 `true`로 설정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

서비스가 작성 시간, 작업 ID 및 작업에 대한 정보를 얻을 수 있는 URL과 함께 `processing` 상태를 리턴하여 이미 작업을 처리 중임을 표시합니다.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## 상태 확인 및 작업의 결과 검색
{: #job}

`GET /v1/recognitions/{id}` 메소드를 호출하여 `id` 경로 매개변수로 지정된 작업의 상태를 확인할 수 있습니다. 응답에는 항상 작업의 ID 및 상태와 작업 작성 및 업데이트 시간이 포함됩니다. 상태가 `completed`이면 인식 요청의 결과도 응답에 포함됩니다.

다음과 같은 경우 `GET /v1/recognitions/{id}` 메소드가 작업 결과를 검색하는 유일한 방법입니다.

-   작업이 콜백 URL 없이 제출되었습니다.
-   작업이 콜백 URL과 함께 제출되었지만 `recognitions.completed_with_results` 이벤트를 지정하지 않았습니다.
-   작업이 100개의 최신 미해결 작업 중 하나가 아닙니다. `id` 경로 매개변수를 생략하면 100개의 최신 작업만 리턴됩니다.

그러나 여전히 이 메소드를 사용하여 콜백 URL 및 `recognitions.completed_with_results` 이벤트를 지정한 작업의 결과를 검색할 수 있습니다. 사용 가능한 상태로 유지되는 동안 원하는 횟수만큼 작업의 결과를 검색할 수 있습니다. `DELETE /v1/recognitions/{id}` 메소드를 사용하여 삭제하거나 작업의 TTL(Time To Live)이 만료될 때까지 작업 및 해당 결과가 사용 가능한 상태로 유지됩니다. 기본적으로 `POST /v1/recognitions` 메소드의 `results_ttl` 매개변수를 사용하여 다른 TTL(Time To Live)을 지정하지 않는 한 1주일 후에 결과가 만료됩니다. 

### 결과가 없는 상태의 예제
{: #withoutResults}

다음 예제에서는 지정된 ID의 작업에 대한 상태를 확인합니다. 작업이 아직 완료되지 않았으므로 응답에 결과가 포함되지 않습니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### 결과가 있는 상태의 예제
{: #withResults}

다음 예제에서는 지정된 ID의 작업에 대한 상태를 요청합니다. 작업이 완료되었으므로 응답에 요청 결과가 포함됩니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## 최신 작업의 상태 확인
{: #jobs}

`GET /v1/recognitions` 메소드를 호출하여 최신 작업의 상태를 확인할 수 있습니다. 이 메소드는 호출 시 사용된 서비스 인증 정보와 연관된 100개의 최신 미해결 작업의 상태를 리턴합니다. 이 메소드는 각 작업의 ID 및 상태를 작성 및 업데이트 시간과 함께 리턴합니다. 작업이 콜백 URL 및 사용자 토큰으로 작성된 경우 이 메소드가 작업에 대한 사용자 토큰도 리턴합니다.

응답에는 다음 상태 중 하나가 포함됩니다.

-   `waiting`: 서비스가 처리를 위해 작업을 준비하는 중입니다. 이 상태는 모든 작업의 초기 상태입니다. 서비스에 처리를 시작할 수 있는 용량이 있을 때까지 작업이 이 상태로 남아 있습니다.
-   `processing`: 서비스가 활발하게 작업을 처리 중입니다.
-   `completed`: 서비스가 작업 처리를 완료했습니다. 작업이 콜백 URL 및 `recognitions.completed_with_results` 이벤트를 지정한 경우 서비스가 결과를 콜백 알림과 함께 전송했습니다. 그렇지 않으면 `GET /v1/recognitions/{id}` 메소드를 사용하여 결과를 얻으십시오.
-   `failed`: 어떤 이유로든 작업이 실패했습니다.

`DELETE /v1/recognitions/{id}` 메소드를 사용하여 삭제하거나 작업의 TTL(Time To Live)이 만료될 때까지 작업 및 해당 결과가 사용 가능한 상태로 유지됩니다. 

### 예제
{: #statusExample}

다음 예제에서는 호출자의 서비스 인증 정보와 연관된 최신 현재 작업의 상태를 요청합니다. 사용자에게 다양한 상태에 있는 세 개의 미해결 작업이 있습니다. 첫 번째 작업은 콜백 URL 및 사용자 토큰으로 작성되었습니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## 작업 삭제
{: #delete-async}

`DELETE /v1/recognitions/{id}` 메소드를 사용하면 `id` 경로 매개변수로 지정된 작업을 삭제할 수 있습니다. 일반적으로 서비스에서 해당 결과를 얻은 후 작업을 삭제합니다. 작업을 삭제하면 해당 결과를 더 이상 사용할 수 없습니다. 서비스가 활발히 처리 중인 작업은 삭제할 수 없습니다.

기본적으로 이 서비스는 작업의 TTL(Time to Live)이 만료될 때까지 각 작업의 결과를 유지보수합니다. 기본 TTL(Time to Live)은 1주이지만 `POST /v1/recognitions` 메소드의 `results_ttl` 매개변수를 사용하여 서비스가 결과를 유지보수하는 기간(분)을 지정할 수 있습니다.

### 예제
{: #deleteExample-async}

다음 예제에서는 지정된 ID의 작업을 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
