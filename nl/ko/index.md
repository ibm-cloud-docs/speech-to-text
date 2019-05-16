---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-03"

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

# 정보
{: #about}

**서비스 업데이트:** *2019년 4월 3일에 {{site.data.keyword.speechtotextshort}} 서비스가 업데이트되었습니다. 이제 이 서비스를 사용하여 사용자 정의 음향 모델에 최대 200시간의 오디오를 추가할 수 있습니다. 자세한 정보는 릴리스 정보에서 [2019년 4월 3일 서비스 업데이트](/docs/services/speech-to-text/release-notes.html#April2019)를 참조하십시오*.

{{site.data.keyword.speechtotextfull}} 서비스는 애플리케이션에 음성 변환 기능을 제공합니다. 이 서비스는 기계 학습을 활용하여 문법, 언어 구조 및 오디오와 음성 신호의 구성에 대한 지식을 결합하여 사람의 음성을 정확하게 변환합니다. 이 서비스는 더 많은 음성을 수신함에 따라 해당 변환을 지속적으로 업데이트하고 정제합니다.
{: shortdesc}

이 서비스는 음성이 입력이고 텍스트 내용이 출력인 모든 애플리케이션에 적합한 다양한 인터페이스를 제공합니다. 예제 애플리케이션은 다음과 같습니다.

-   애플리케이션, 임베드된 디바이스 및 차량 액세서리의 음성 제어
-   회의 및 컨퍼런스 콜의 변환
-   이메일 메시지 및 메모의 받아쓰기

## 지원되는 인터페이스
{: #interfaces}

{{site.data.keyword.speechtotextshort}} 서비스는 음성 인식을 위해 세 가지 인터페이스를 제공합니다.

-   [WebSocket 인터페이스](/docs/services/speech-to-text/websockets.html): 서비스에 대한 지속적이고 대기 시간이 짧은 전이중 연결을 설정합니다. 최대 100MB의 오디오 데이터를 단일 요청으로 서비스에 전달할 수 있습니다. 
-   [동기 HTTP 인터페이스](/docs/services/speech-to-text/http.html): 서비스에 대한 기본 HTTP 호출에 사용됩니다. 최대 100MB의 오디오 데이터를 단일 요청으로 전달할 수 있습니다. 
-   [비동기 HTTP 인터페이스](/docs/services/speech-to-text/async.html): 서비스에 대한 비차단 호출에 사용됩니다. 1GB의 오디오 데이터를 단일 요청으로 전달할 수 있습니다. 

이 서비스는 언어 및 음향 요구사항에 맞게 음성 인식을 조정하는 데 사용할 수 있는 [사용자 정의 인터페이스](/docs/services/speech-to-text/custom.html)도 제공합니다. 도메인 특정 용어로 모델의 어휘를 확장하거나 오디오의 음향 특성에 맞게 모델을 조정할 수 있습니다. [문법](/docs/services/speech-to-text/grammar.html)을 추가하여 서비스에서 인식할 수 있는 구문을 제한할 수도 있습니다.

-   이 서비스를 사용한 애플리케이션 개발에 대한 개괄적인 설명은 [개발자를 위한 개요](/docs/services/speech-to-text/developer-overview.html)를 참조하십시오.
-   각 서비스 인터페이스를 통한 기본 음성 인식 요청의 예제는 [인식 요청 작성](/docs/services/speech-to-text/basic-request.html)을 참조하십시오.

다양한 프로그래밍 언어에서 SDK를 사용하여 서비스 사용을 단순화할 수 있습니다. 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

## 입력 기능
{: #inputFeatures}

서비스 인터페이스는 음성을 텍스트로 변환하는 데 사용되는 공통 입력 기능을 공유합니다.

-   [오디오 형식](/docs/services/speech-to-text/audio-formats.html) - Opus 또는 Vorbis 코덱을 사용하는 Ogg 또는 WebM(Web Media) 오디오, MP3(또는 MPEG), WAV(Waveform Audio File Format), FLAC(Free Lossless Audio Codec), 선형 16비트 PCM(Pulse-Code Modulation), G.729, a-law, mu-law(또는 u-law) 및 기본 오디오를 변환할 수 있습니다. 압축을 지원하는 형식을 사용하여 요청과 함께 전송할 수 있는 오디오의 양을 최대화할 수 있습니다.
-   [언어 및 모델](/docs/services/speech-to-text/models.html) - 대부분의 언어에서 광대역 또는 협대역을 사용하여 오디오를 변환할 수 있습니다. 최소 16kHz의 속도로 샘플링된 오디오에는 광대역을 사용하십시오. 최소 8kHz의 속도로 샘플링된 오디오에는 협대역을 사용하십시오.
-   [오디오 전송](/docs/services/speech-to-text/input.html#transmission) - 데이터 청크의 연속 스트림 또는 모든 데이터를 한 번에 전달하는 원샷(one-shot) 전달로 오디오를 전송할 수 있습니다. 스트리밍을 사용하는 경우 서비스가 비활성 및 세션 [제한시간](/docs/services/speech-to-text/input.html#timeouts)을 적용합니다.

## 출력 기능
{: #outputFeatures}

이 인터페이스는 다음과 같은 공통 출력 기능도 지원합니다.

-   [화자 레이블](/docs/services/speech-to-text/output.html#speaker_labels)은 미국 영어, 영국 영어, 스페인어 또는 일본어로 된 오디오에서 여러 화자를 인식합니다. 변환 시 여러 사람이 참여한 대화에서 각 화자가 기여한 부분에 레이블이 지정됩니다(베타 기능).
-   [키워드 발견](/docs/services/speech-to-text/output.html#keyword_spotting)은 사용자 정의 신뢰수준으로 지정된 키워드 문자열과 일치하는 음성 구문을 식별합니다. 키워드 발견은 오디오의 개별 구문이 전체 변환보다 더 중요한 경우에 특히 유용합니다. 예를 들어, 고객 지원 시스템에서는 키워드를 식별하여 사용자 요청을 라우팅하는 방법을 판별할 수 있습니다.
-   [중간 결과](/docs/services/speech-to-text/output.html#interim)는 변환이 진행됨에 따라 점진적인 가설을 리턴합니다. 변환 완료 시 서비스가 최종 결과를 리턴합니다. 중간 결과는 WebSocket 인터페이스에서만 사용할 수 있습니다.
-   [최대 대체 수](/docs/services/speech-to-text/output.html#max_alternatives)는 가능한 대체 음성 내용을 제공합니다. 이 서비스는 신뢰도가 가장 높은 최종 결과를 표시합니다.
-   [단어 대체](/docs/services/speech-to-text/output.html#word_alternatives)는 음성 내용의 단어와 음향적으로 유사한 대체 단어를 요청합니다.
-   [단어 신뢰도](/docs/services/speech-to-text/output.html#word_confidence)는 음성 내용의 각 단어에 대한 신뢰수준을 리턴합니다.
-   [단어 시간소인](/docs/services/speech-to-text/output.html#word_timestamps)은 음성 내용의 각 단어에 대한 시작 및 종료 시간소인을 리턴합니다.
-   [스마트 형식화](/docs/services/speech-to-text/output.html#smart_formatting)는 최종 음성 내용에서 날짜, 시간, 숫자, 통화 가치 및 인터넷 주소를 더 읽기 쉽고 일반적인 양식으로 변환합니다. 미국 영어의 경우 최종 음성 내용에 특정 문장 부호를 포함하는 키워드 구문을 제공할 수도 있습니다. 스마트 형식화는 미국 영어, 일본어 및 스페인어 오디오에 대해 지원됩니다(베타 기능).
-   [숫자 교정](/docs/services/speech-to-text/output.html#redaction)은 최종 음성 내용의 숫자 데이터를 교정하거나 마스킹합니다. 교정은 음성 내용에서 신용카드 번호와 같은 민감한 개인 정보를 제거하기 위한 것입니다. 이 기능은 미국 영어, 일본어 및 한국어 오디오에 대해 지원됩니다(베타 기능).
-   [욕설 필터링](/docs/services/speech-to-text/output.html#profanity_filter)은 미국 영어 음성 내용에서 욕설을 검열합니다.

## 언어 지원
{: #languages}

이 서비스는 브라질 포르투갈어, 프랑스어, 독일어, 일본어, 중국어(간체), 현대 표준 아랍어, 스페인어, 영국 영어 및 미국 영어에 대한 모델을 제공합니다. 이 서비스가 모든 언어에 대해 모든 기능을 지원하지는 않습니다. 또한 일부 기능을 프로덕션용 GA(Generally Available)로 지원하고 다른 기능을 여러 언어에 대한 베타 오퍼링으로 지원합니다.

-   WebSocket 및 HTTP 인터페이스는 모든 언어에 대해 일반적으로 사용할 수 있습니다.
-   이 서비스는 여러 언어에 대한 광대역 모델, 협대역 모델 또는 둘 다를 제공합니다. 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.
-   일부 음향 인식 기능은 일부 언어에만 사용할 수 있습니다. 자세한 정보는 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오.
-   언어 모델 사용자 정의 인터페이스는 대부분의 언어에 대해 일반적으로 사용할 수 있습니다. 음향 모델 사용자 정의 인터페이스는 모든 언어에 대해 베타 기능으로 사용할 수 있습니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text/custom.html#languageSupport)을 참조하십시오.

## 가격
{: #pricing-index}

서비스의 가격 플랜에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_short}} 카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/catalog/services/speech-to-text){: new_window}에서 {{site.data.keyword.speechtotextshort}} 서비스를 참조하십시오. 가격과 관련된 질문에 대한 답변과 월별 사용 비용의 예제는 [가격 FAQ](/docs/services/speech-to-text/faq-pricing.html)를 참조하십시오.

## 서비스 사용해 보기
{: #tryOut}

작동 중인 {{site.data.keyword.speechtotextshort}} 서비스의 예제는 다음을 참조하십시오.

-   스트리밍 오디오 입력 또는 사용자가 업로드한 파일의 텍스트를 변환하는 [빠른 데모 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://speech-to-text-demo.ng.bluemix.net/){: new_window}
-   이 서비스를 시연하는 {{site.data.keyword.ibmwatson}} [스타터 킷 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window}의 애플리케이션
-   WebSocket 인터페이스를 Python과 함께 사용하여 오디오에서 음성을 추출하는 방법을 보여주는 {{site.data.keyword.watson}} 블로그 게시물 [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window}. 이 게시물은 관련된 코드와 단계를 보여주는 자세한 튜토리얼을 제공합니다.
