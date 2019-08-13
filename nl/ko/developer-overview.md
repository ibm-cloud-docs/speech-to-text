---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-13"

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

# 개발자를 위한 개요
{: #developerOverview}

WebSocket 인터페이스를 사용하거나 동기 또는 비동기 HTTP REST(Representational State Transfer) 인터페이스를 사용하여 {{site.data.keyword.speechtotextfull}} 서비스의 기능에 액세스할 수 있습니다. 사용자 도메인 및 환경에 맞게 서비스의 언어 모델을 사용자 정의할 수도 있습니다. SDK를 사용하여 많은 프로그래밍 언어에서 애플리케이션 개발을 단순화할 수 있습니다.
{: shortdesc}

## 서비스 프로그래밍
{: #programming}

[인식 요청 작성](/docs/services/speech-to-text?topic=speech-to-text-basic-request)은 서비스 프로그래밍 인터페이스를 사용하여 기본 변환을 요청하는 방법을 보여줍니다.

-   [WebSocket 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-websockets)는 전이중 연결을 통해 효율적이며 대기 시간이 짧고 처리량이 높은 구현을 제공합니다.
-   [동기 HTTP 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-http)는 차단 요청으로 오디오를 변환하기 위한 기본 인터페이스를 제공합니다.
-   [비동기 HTTP 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-async)는 콜백 URL을 등록하여 알림을 수신하거나 작업 상태 및 결과를 위해 서비스를 폴링할 수 있도록 하는 비차단 인터페이스를 제공합니다.

이러한 인터페이스는 동일한 음성 인식 기능을 제공하지만 사용하는 인터페이스에 따라 요청 헤더, 조회 매개변수 또는 JSON 오브젝트의 매개변수와 동일한 매개변수를 지정할 수 있습니다. 또한 WebSocket 및 동기 HTTP 인터페이스는 단일 요청으로 최대 100MB의 오디오 데이터를 허용합니다. 비동기 HTTP 인터페이스는 최대 1GB의 오디오 데이터를 허용합니다.

-   사용 가능한 모든 음성 인식 매개변수에 대한 설명은 [매개변수 요약](/docs/services/speech-to-text?topic=speech-to-text-summary)을 참조하십시오.
-   모든 메소드 및 해당 매개변수에 대한 설명 및 예제는 [API 참조](https://{DomainName}/apidocs/speech-to-text){: external}를 참조하십시오.

## WebSocket 인터페이스의 장점
{: #advantages}

WebSocket 인터페이스에는 HTTP 인터페이스에 비해 다음과 같은 많은 장점이 있습니다.

-   WebSocket 인터페이스는 단일 소켓, 전이중 통신 채널을 제공합니다. 클라이언트는 이 인터페이스를 사용하여 요청 및 오디오를 서비스에 전송하고 비동기 방식으로 단일 연결을 통해 결과를 수신할 수 있습니다.
-   훨씬 더 단순하고 강력한 프로그래밍 경험을 제공합니다. 이 서비스는 클라이언트의 메시지에 이벤트 중심 응답을 전송하므로 클라이언트가 서버를 폴링할 필요가 없습니다.
-   하나의 인증된 연결을 설정하고 무기한으로 사용할 수 있습니다. HTTP 인터페이스를 사용하려면 서비스에 대한 각 호출을 인증해야 합니다.
-   대기 시간을 줄입니다. 서비스가 인식 결과를 클라이언트에 직접 전송하므로 인식 결과가 더 빨리 도착합니다. HTTP 인터페이스에서 동일한 결과를 얻으려면 4개의 개별 요청 및 연결이 필요합니다.
-   네트워크 사용률을 줄입니다. WebSocket 프로토콜은 경량입니다. 실시간 음성 인식을 수행하려면 하나의 연결만 있으면 됩니다.
-   오디오가 브라우저(HTML5 WebSocket 클라이언트)에서 서비스로 직접 스트리밍되도록 합니다.

## 서비스 사용자 정의
{: #customizing}

[사용자 정의 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-customization)를 통해 사용자 정의 모델을 작성하여 서비스의 음성 인식 기능을 개선할 수 있습니다.

-   [사용자 정의 언어 모델](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)을 사용하여 기본 모델에 대한 도메인 특정 단어를 정의할 수 있습니다. 사용자 정의 언어 모델은 의학 및 법률과 같은 도메인에 특정한 용어로 서비스의 기본 어휘를 확장합니다.
-   [사용자 정의 음향 모델](/docs/services/speech-to-text?topic=speech-to-text-acoustic)을 사용하여 사용자 환경 및 화자의 음향 특성에 맞게 기본 모델을 조정할 수 있습니다. 사용자 정의 음향 모델은 특정 음향 특성에 대한 음성을 인식하는 서비스의 기능을 개선합니다.
-   [문법](/docs/services/speech-to-text?topic=speech-to-text-grammars)을 사용하여 서비스가 인식할 수 있는 구문을 문법의 규칙에 정의된 구문으로 제한할 수 있습니다. 서비스가 올바른 문자열에 대한 검색 공간을 제한하여 결과를 더 빠르고 정확하게 전달할 수 있습니다. 문법은 사용자 정의 언어 모델에서 지원됩니다.

임의의 서비스 인터페이스를 통해 사용자 정의 언어 모델, 사용자 정의 음향 모델 또는 둘 다를 음성 인식에 사용할 수 있습니다.

언어 모델 또는 음향 모델 사용자 정의를 사용하려면 Standard 가격 플랜이 필요합니다. Lite 플랜 사용자는 사용자 정의 인터페이스를 사용할 수 없습니다. 자세한 정보는 {{site.data.keyword.speechtotextshort}} 서비스에 대한 [가격 페이지](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}를 참조하십시오.
{: note}

## 메트릭 얻기
{: #overview-metrics}

서비스는 음성 인식 요청에 대한 두 가지 유형의 선택적 메트릭을 제공합니다. 

-   [처리 메트릭](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)은 입력 오디오의 서비스 분석에 대한 자세한 타이밍 정보를 제공합니다. 서비스는 지정된 간격으로 변환 이벤트와 함께 메트릭을 리턴합니다(예: 중간 및 최종 결과). 메트릭을 사용하여 오디오를 변환하는 중에 서비스 진행상태를 알아볼 수 있습니다. 
-   [오디오 메트릭](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)은 입력 오디오의 신호 특성에 대한 자세한 정보를 제공합니다. 결과는 음성 처리 종료 시 전체 입력 오디오에 대해 집계된 메트릭을 제공합니다. 메트릭을 사용하여 오디오의 특성 및 품질을 판별할 수 있습니다. 

WebSocket 및 비동기 HTTP 인터페이스를 사용하면 처리 메트릭을 요청할 수 있습니다. 모든 서비스의 인터페이스를 사용하여 오디오 메트릭을 요청할 수 있습니다. 기본적으로 이 서비스는 메트릭을 리턴하지 않습니다. 

## CORS 지원
{: #cors}

이 서비스는 CORS(Cross-Origin Resource Sharing)를 지원합니다. 웹 페이지는 CORS를 사용하여 외부 도메인에서 직접 리소스를 요청할 수 있습니다. CORS는 이러한 요청을 방지하는 동일 원본 보안 정책을 우회합니다. 이 서비스는 CORS를 지원하므로 웹 페이지가 페이지를 호스팅하는 웹 서버를 통해 요청을 전달하지 않고 서비스와 직접 통신할 수 있습니다.

예를 들어, {{site.data.keyword.cloud}}의 서버에서 로드된 웹 페이지는 {{site.data.keyword.cloud_notm}} 서버를 우회하여 사용자 정의 API를 직접 호출할 수 있습니다. 자세한 정보는 [enable-cors.org](https://enable-cors.org/){: external}를 참조하십시오.

## SDK(Software Development Kit) 사용
{: #sdks}

SDK를 {{site.data.keyword.speechtotextshort}} 서비스에 사용하여 음성 애플리케이션의 개발을 단순화할 수 있습니다. {{site.data.keyword.ibmwatson}} SDK는 여러 인기 있는 프로그래밍 언어 및 플랫폼에서 사용할 수 있습니다.

-   SDK의 전체 목록과 GitHub의 SDK에 대한 링크는 [SDK 사용](/docs/services/watson?topic=watson-using-sdks)을 참조하십시오.
-   {{site.data.keyword.speechtotextshort}} 서비스용 Node, Java&trade;, Python, Ruby, Swift 및 Go SDK의 모든 메소드에 대한 자세한 정보는 [API 참조](https://{DomainName}/apidocs/speech-to-text){: external}를 참조하십시오.

## 애플리케이션 개발에 대해 자세히 보기
{: #learn}

{{site.data.keyword.watson}} 서비스 및 {{site.data.keyword.cloud_notm}} 작업에 대한 자세한 정보는 다음을 참조하십시오.

-   {{site.data.keyword.watson}} 서비스 및 {{site.data.keyword.cloud_notm}} 작업에 대한 소개는 [{{site.data.keyword.watson}} 및 {{site.data.keyword.cloud_notm}} 시작하기](/docs/services/watson?topic=watson-about)를 참조하십시오.
-   새 서비스 인스턴스는 모두 {{site.data.keyword.cloud_notm}} IAM(Identity and Access Management)을 인증에 사용합니다. 이전 서비스 인스턴스는 기존 Cloud Foundry 서비스 인증 정보의 `{username}` 및 `{password}`를 계속 인증에 사용할 수 있습니다. 서비스 인증에 대한 자세한 정보는 릴리스 정보의 [2018년 10월 30일 서비스 업데이트](/docs/services/speech-to-text?topic=speech-to-text-release-notes#October2018b)를 참조하십시오.
-   모든 {{site.data.keyword.watson}} 서비스에 대해 수행되는 기본 요청 로깅을 제어하는 방법에 대한 정보는 [{{site.data.keyword.watson}} 서비스에 대한 요청 로깅 제어](/docs/services/watson?topic=watson-gs-logging-overview)를 참조하십시오.
