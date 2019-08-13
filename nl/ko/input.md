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

# 입력 기능
{: #input}

{{site.data.keyword.speechtotextfull}} 서비스는 이 서비스가 음성 인식 요청을 수행하는 방법을 지정하기 위해 다음과 같은 기능을 제공합니다. 다음 섹션에서 설명된 모든 입력 매개변수는 선택사항입니다. 입력 오디오만 필수입니다.
{: shortdesc}

-   각 서비스 인스턴스에 대한 단순 음성 인식 요청의 예제는 [인식 요청 작성](/docs/services/speech-to-text?topic=speech-to-text-basic-request)을 참조하십시오.
-   상태(GA 또는 베타) 및 지원되는 언어를 포함한 사용 가능한 모든 음성 인식 매개변수의 알파벳순 목록은 [매개변수 요약](/docs/services/speech-to-text?topic=speech-to-text-summary)을 참조하십시오.

## 사용자 정의 모델
{: #custom-input}

언어 및 음향 모델 사용자 정의는 여러 언어에 대한 다양한 레벨의 지원(GA(Generally Aavailable) 또는 베타)으로 사용 가능합니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)을 참조하십시오.
{: note}

모든 인터페이스는 인식 요청에 사용할 사용자 정의 모델을 허용합니다.

-   *사용자 정의 언어 모델*은 특정 도메인의 용어로 서비스의 기본 어휘를 확장합니다. 사용자 정의 언어 모델을 요청에 포함하려면 `language_customization_id` 매개변수를 사용하십시오. 선택적 `customization_weight` 매개변수를 지정할 수도 있습니다. 이 매개변수는 기본 어휘의 단어와 비교하여 사용자 정의 모델의 단어에 지정된 상대적인 가중치를 표시합니다.

자세한 정보는 [사용자 정의 언어 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-languageUse)을 참조하십시오.
-   *사용자 정의 음향 모델*은 사용자 환경 및 화자의 음향 특성에 맞게 서비스의 기본 음향 모델을 조정합니다. 사용자 정의 음향 모델을 요청에 포함하려면 `acoustic_customization_id` 매개변수를 사용하십시오. 사용자 정의 언어 모델 및 사용자 정의 음향 모델을 모두 요청과 함께 지정할 수 있습니다.

    자세한 정보는 [사용자 정의 음향 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)을 참조하십시오.

사용자 정의 모델은 [언어 및 모델](/docs/services/speech-to-text?topic=speech-to-text-models)에 설명된 언어 모델 중 하나를 기반으로 합니다. 사용자 정의 모델은 작성 시 기반이 된 기본 모델에만 사용될 수 있습니다. 사용자 정의 모델이 기본값인 `en-US_BroadbandModel` 이외의 모델을 기반으로 하는 경우 해당 모델의 이름도 요청과 함께 지정해야 합니다. 사용자 정의 모델을 사용하려면 사용자 정의 모델을 소유하는 서비스 인스턴스에 대한 인증 정보로 요청을 발행해야 합니다.

사용자 정의에 대한 소개는 [사용자 정의 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-customization)를 참조하십시오.

### 사용자 정의 모델 예제
{: #customExample}

다음 예제 요청에는 지정된 ID의 사용자 정의 언어 모델을 사용하기 위한 `language_customization_id` 매개변수가 포함되어 있습니다. 또한 사용자 정의 모델의 단어에 상대 가중치 `0.5`가 지정됨을 표시하는 `customization_weight` 매개변수가 포함되어 있습니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

다음 예제 요청에서는 사용자 정의 언어 모델과 사용자 정의 음향 모델을 모두 사용합니다. 전자는 `language_customization_id` 매개변수로 식별되고 후자는 `acoustic_customization_id` 매개변수로 식별됩니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

각 서비스 인터페이스를 통해 사용자 정의 모델을 사용하는 예제는 다음을 참조하십시오.

-   [사용자 정의 언어 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
-   [사용자 정의 음향 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## 문법
{: #grammars-input}

문법 기능은 베타 기능입니다. 이 서비스는 언어 모델 사용자 정의를 지원하는 모든 언어에 대한 문법을 지원합니다.
{: note}

문법을 사용자 정의 언어 모델에 추가하고 음성 인식에 사용할 수 있습니다. 문법은 정규 언어 스펙을 사용하여 문자열을 변환하기 위한 프로덕션 규칙 세트를 정의합니다. 이 규칙은 언어의 알파벳에서 올바른 문자열을 형성하는 방법을 지정합니다.

문법을 음성 인식에 사용하는 경우 서비스가 문법에서 인식되는 구문만 인식합니다. 서비스가 올바른 문자열에 대한 검색 공간을 제한하여 결과를 더 빠르고 정확하게 전달할 수 있습니다.

모든 인터페이스는 인식 요청에 대해 다음 매개변수를 허용합니다.

-   `language_customization_id` 매개변수는 문법이 정의된 사용자 정의 언어 모델을 식별합니다. 모델을 소유하는 서비스 인스턴스에 대한 인증 정보로 요청을 발행해야 합니다.
-   `grammar_name` 매개변수는 사용할 문법을 지정합니다. 요청에 하나의 문법만을 지정할 수 있습니다.

자세한 정보는 [사용자 정의 언어 모델에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammars)을 참조하십시오.

### 문법 예제
{: #grammarsExample}

다음 예제 요청에는 서비스의 응답을 `list-abnf`라는 문법에 정의된 문자열로 제한하기 위한 `language_customization_id` 및 `grammar_name` 매개변수가 포함되어 있습니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

각 서비스 인터페이스에서 문법을 사용하는 예제는 [음성 인식에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)을 참조하십시오.

## 기본 모델 버전
{: #version}

음성 인식의 품질을 개선하기 위해 이 서비스는 경우에 따라 [언어 및 모델](/docs/services/speech-to-text?topic=speech-to-text-models)에 설명된 기본 언어 모델을 업데이트합니다. 언어의 광대역 및 협대역 모델과 마찬가지로 언어의 기본 모델은 서로 독립적입니다. 따라서 기본 모델에 대한 업데이트는 서로 독립적으로 발생하며 다른 모델에 영향을 주지 않습니다.

기본 모델의 여러 버전이 존재하는 경우 선택적 `base_model_version` 매개변수가 인식 요청에 사용될 모델의 버전을 지정합니다. 이 매개변수는 주로 새 기본 모델용으로 업데이트된 사용자 정의 모델에 사용하기 위한 것이지만 사용자 정의 모델 없이 사용할 수 있습니다. 요청에 사용되는 기본 모델의 버전은 `base_model_version` 매개변수를 전달하는지 여부에 따라 다릅니다. 또한 요청에 사용자 정의 모델(언어, 음향 또는 둘 다)을 지정하는지 여부에 따라 달라집니다.

-   *요청에 사용자 정의 모델을 지정하지 않는 경우*

    -   기본 모델의 최신 버전을 사용하려면 `base_model_version` 매개변수를 생략하십시오.
    -   기본 모델의 특정 버전을 사용하려면 `base_model_version` 매개변수를 지정하십시오.

-   *요청에 사용자 정의 모델을 지정하는 경우*

    -   사용자 정의 모델이 업그레이드된 기본 모델의 최신 버전을 사용하려면 `base_model_version` 매개변수를 생략하십시오. 예를 들어, 사용자 정의 모델이 기본 모델의 최신 버전으로 업그레이드된 경우 서비스는 기본 및 사용자 정의 모델의 최신 버전을 사용합니다.
    -   기본 및 사용자 정의 모델 둘 다의 지정된 버전을 사용하려면 `base_model_version` 매개변수를 지정하십시오.

이 매개변수는 사용자 정의 모델에 사용하기 위한 것입니다. 따라서 기본 모델을 기반으로 하는 사용자 정의 모델에 대한 정보를 나열하여 기본 모델의 사용 가능한 버전에 대해 알아볼 수 있습니다.

-   사용자 정의 모델 업그레이드에 대한 자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)를 참조하십시오.
-   여러 버전의 기본 및 사용자 정의 모델을 음성 인식에 사용하는 데 대한 자세한 정보는 [업그레이드된 사용자 정의 모델로 인식 요청 작성](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition)을 참조하십시오.

## 오디오 전송
{: #transmission}

*WebSocket 인터페이스*를 사용하면 오디오 데이터가 항상 연결을 통해 서비스로 스트리밍됩니다. 소켓을 통해 데이터를 동시에 전달하거나 라이브 유스 케이스에 대한 데이터의 경우 사용 가능하게 될 때 전달할 수 있습니다. 이 서비스는 사용 가능하게 될 때 결과를 리턴합니다.

*HTTP 인터페이스를 사용하면* 다음 두 가지 방법 중 하나로 오디오를 서비스에 전송할 수 있습니다.

-   *원샷(One-shot) 전달* - `Transfer-Encoding` 헤더를 생략하고 단일 전달로 모든 오디오 데이터를 한번에 서비스에 전달합니다.
-   *스트리밍* - `Transfer-Encoding` 요청 헤더를 `chunked` 값으로 설정하고 지속적 연결을 통해 데이터를 스트리밍합니다. 데이터를 서비스에 스트리밍하기 전에 데이터가 완전히 존재하지 않아도 됩니다. 사용 가능하게 될 때 데이터를 스트리밍할 수 있습니다. 이 서비스는 비어 있는 청크를 전송하여 표시하는 최종 청크를 수신하는 경우에만 결과를 전송합니다.

    `Transfer-Encoding` 헤더를 통한 청크 분할 오디오 스트리밍에 대한 자세한 정보는 다음을 참조하십시오.
    -   [Chunked transfer encoding](https://wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*의 [Transfer Codings](https://tools.ietf.org/html/rfc7230#section-4){: external}

HTTP 인터페이스를 사용하는 경우 이 서비스는 결과를 전송하기 전에 항상 전체 오디오 스트림을 변환합니다. 결과에는 일시정지로 구분된 구문을 표시하기 위한 여러 `transcript` 요소가 포함될 수 있습니다. `transcript` 요소를 연결하여 전체 음성 내용을 어셈블하십시오.

이 서비스는 스트리밍 세션에 제한시간을 적용합니다. 긴 시간 동안의 무음을 발견하거나 30초 동안 오디오를 수신하지 않는 경우 스트리밍 세션을 종료할 수 있습니다. 제한시간 초과 및 이를 방지하는 방법에 대한 자세한 정보는 [제한시간](#timeouts)을 참조하십시오.

### 오디오 전송 예제
{: #transmissionExample}

다음 예제 요청은 스트리밍 모드를 사용하도록 `Transfer-Encoding` 헤더에 `chunked`를 지정합니다. 오디오의 추가 청크를 받아들이기 위해 연결이 계속 열려 있습니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 제한시간
{: #timeouts}

HTTP 또는 WebSocket 음성 인식 메소드로 스트리밍 세션을 시작할 때 서비스가 비활성 및 세션 제한시간을 적용합니다. 스트리밍 세션 중에 제한시간이 경과하면 서비스가 연결을 닫습니다. 애플리케이션이 가능한 닫힌 연결에서 정상적으로 복구되어야 합니다.

HTTP를 통해 오디오를 스트리밍하는 경우 서비스가 20초마다 응답으로 공백 문자를 전송합니다. 이 서비스는 30초 HTTP REST 비활성 제한시간 초과를 방지하여 사용성을 향상시키기 위해 이를 수행합니다. 인식이 지속되는 동안 연결을 활성 상태로 유지하기 위해 이 서비스는 변환을 완료할 때까지 이 공백 문자를 계속 전송합니다. 이 공백 문자는 JSON으로 인코딩된 응답 데이터에는 영향을 주지 않습니다.

이 HTTP 비활성 제한시간은 서비스의 비활성 제한시간과 다릅니다. WebSocket 인터페이스에는 이 HTTP 제한시간이 적용되지 않습니다.
{: note}

### 비활성 제한시간
{: #timeouts-inactivity}

*비활성 제한시간 초과*(HTTP 상태 코드 400)는 서비스가 오디오를 수신하지만 30초 동안 연속적인 무음 또는 비음성 활동(음성 없음)만 감지하는 경우에 발생합니다. 이 서비스는 `No speech detected for 30s`라는 오류 메시지를 전송합니다. 예를 들어, 비활성 제한시간 초과는 사용자가 단순히 라이브 마이크에서 떨어질 때 세션을 종료하는 데 유용합니다.

기본 비활성 제한시간은 30초입니다. `inactivity_timeout` 매개변수를 사용하여 이 값을 대체할 수 있습니다. 비활성 제한시간을 늘리려면 더 큰 값을 지정하십시오. 비활성 제한시간을 무한대로 설정하려면 `-1` 값을 지정하십시오. 무음을 포함하여 서비스에 전송하는 모든 오디오에 대해 비용이 청구되기 때문에 비활성 제한시간을 늘리면 무음만 전송하는 스트리밍 세션에 대한 추가 비용이 발생할 수 있습니다.

다음 예제 요청은 비활성 제한시간을 60초로 설정합니다. 이 요청은 스트리밍 세션을 시작하기 이해 초기 파일을 전송합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### 세션 제한시간
{: #timeouts-session}

*세션 제한시간 초과*(HTTP 상태 코드 408)는 스트리밍 세션을 활성 상태로 유지하기에 충분한 오디오를 전송하는 데 실패하는 경우에 발생합니다. 다음과 같은 이유로 이 서비스가 세션을 유휴 상태로 간주하고 세션 제한시간 초과를 트리거할 수 있습니다.

-   30초 창에서 15초 이상의 오디오를 서비스에 전송하는 데 실패합니다.

    스트림의 끝을 표시하기 위해 마지막 청크를 전송할 때까지 30초 기간 내에 15초 이상의 오디오를 전송해야 합니다. `inactivity_timeout` 매개변수를 더 큰 값이나 `-1`로 설정한 경우 오디오가 무음일 수 있습니다. 무음을 포함하여 서비스에 전송하는 모든 오디오의 지속 시간 동안 비용이 청구됩니다.
-   실시간보다 훨씬 느린 속도로 오디오를 스트리밍합니다.

    이상적으로는 변환을 위한 오디오를 얻기 직전에 세션을 설정하도록 요청을 시작할 수 있습니다. 그런 다음 실시간에 가까운 속도로 오디오를 전송하여 세션을 유지할 수 있습니다.

스트림의 끝을 표시하기 위해 마지막 청크를 전송할 수 세션 제한시간에 대해 걱정할 필요가 없습니다. 이 서비스는 최종 변환 결과를 리턴할 때까지 오디오를 계속 처리합니다.

긴 오디오 스트림을 변환하는 경우 이 서비스가 오디오를 처리하고 응답을 생성하는 데 30초 이상이 걸릴 수 있습니다. 이 서비스는 수신한 모든 오디오의 처리를 완료할 때까지 세션 제한시간 계산을 시작하지 않습니다. 서비스의 처리 시간으로 인해 세션이 30초 세션 제한시간을 초과할 수 없습니다.

예를 들어, 세션의 처음 10초 동안 1시간 길이의 오디오를 전송하는 경우 이 서비스가 오디오를 처리하는 데 300초가 걸릴 수 있습니다.  이 세션을 활성 상태로 유지하려면 늦어도 340초까지는 무음을 포함하여 15초 이상의 오디오를 세션에 추가로 전송해야 합니다.

이 예제에서는 세션의 100초 표시에 다른 15초 길이의 오디오를 전송할 경우 이 서비스가 이 오디오를 처리하는 데 2초가 더 걸릴 수 있습니다. 이 경우 늦어도 342초에 15초 길이의 오디오를 세션에 추가로 전송해야 합니다.

처리 시간이나 결과를 수신했는지 여부에 따라 스트리밍 세션이 유휴 상태인지 여부를 판별하지 마십시오. 서비스가 모든 오디오를 즉시 처리할 수 있다고 가정하고 그에 따라 서비스에 데이터를 전송하십시오. 실시간으로 오디오를 스트리밍하는 경우 30초 창에서 1/2의 오디오(15초 길이의 오디오) 실시간으로 늦지 않게 전송하십시오. 이 속도는 일반적으로 네트워크 대기 시간을 지연을 수용하기에 충분합니다.
{: important}

## 요청 로깅
{: #logging}

기본적으로 {{site.data.keyword.IBM_notm}}에서는 {{site.data.keyword.watson}} 서비스에 대한 모든 요청과 해당 결과를 로그합니다. 로깅은 향후 사용자를 위해 서비스를 개선할 목적으로만 수행됩니다. 로그된 데이터는 공유되거나 공개되지 않습니다.

사용자의 개인정보를 보호하는 데 관심이 있거나 IBM에서 요청을 로그하지 않도록 하려면 IBM 로그 데이터를 보유하지 않도록(사용하지 않음) 선택할 수 있습니다. 계정 레벨 또는 API 요청 레벨에서 로깅을 사용하지 않도록 선택할 수 있습니다. 자세한 정보는 [{{site.data.keyword.watson}} 서비스에 대한 요청 로깅 제어](/docs/services/watson?topic=watson-gs-logging-overview)를 참조하십시오.

## 정보 보안
{: #security-input}

정보 보안에는 요청과 함께 서비스에 전달된 데이터와 고객 ID를 연관시키는 기능이 포함되어 있습니다. 요청과 함께 `X-Watson-Metadata` 헤더를 전달하여 고객 ID를 데이터와 연관시킵니다. 그런 다음, 필요한 경우 `DELETE /v1/user_data` 메소드를 사용하여 데이터를 삭제할 수 있습니다. 자세한 정보는 [정보 보안](/docs/services/speech-to-text?topic=speech-to-text-information-security)을 참조하십시오.
