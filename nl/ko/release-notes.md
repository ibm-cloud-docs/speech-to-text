---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# 릴리스 정보
{: #release-notes}

다음 섹션에는 {{site.data.keyword.speechtotextfull}} 서비스의 각 릴리스와 업데이트에 대해 포함된 새로운 기능 및 변경사항이 문서화되어 있습니다. 이 정보에는 알려진 제한사항이 포함되어 있습니다. 별도로 언급하지 않는 한, 모든 변경사항은 이전 릴리스와 호환 가능하며 모든 신규 및 기존 애플리케이션에서 자동으로 투명하게 사용할 수 있습니다.
{: shortdesc}

## 알려진 제한사항
{: #limitations}

현재 알려진 제한사항이 없습니다.

## 2019년 4월 3일
{: #April2019}

이제 사용자 정의 음향 모델이 최대 200시간의 오디오를 허용합니다. 이전 최대 한계는 100시간의 오디오였습니다.

## 2019년 3월 21일
{: #March2019d}

이제 사용자는 해당 {{site.data.keyword.cloud_notm}} 계정에 지정된 역할과 연관된 서비스 인증 정보만 볼 수 있습니다. 예를 들어, `reader` 역할이 지정된 경우 `writer` 또는 상위 레벨의 서비스 인증 정보가 더 이상 표시되지 않습니다. 

이 변경사항은 기존 서비스 인증 정보를 사용하는 사용자 또는 애플리케이션에 대한 API 액세스에는 영향을 주지 않습니다. 이 변경사항은 {{site.data.keyword.cloud_notm}} 내의 인증 정보를 보는 경우에만 영향을 줍니다.

서비스 키 및 사용자 역할에 대한 자세한 정보는 [IAM 서비스 API 키](/docs/services/watson?topic=watson-api-key-bp#api-key-bp)를 참조하십시오.

## 2019년 3월 15일
{: #March2019c}

이제 이 서비스는 A-law(`audio/alaw`) 형식의 오디오를 지원합니다. 자세한 정보는 [audio/alaw 형식](/docs/services/speech-to-text/audio-formats.html#alaw)을 참조하십시오.

## 이전 릴리스
{: #older}

-   [2019년 3월 11일](#March2019b)
-   [2019년 3월 4일](#March2019)
-   [2019년 1월 28일](#January2019)
-   [2018년 12월 20일](#December2018b)
-   [2018년 12월 13일](#December2018a)
-   [2018년 11월 12일](#November2018b)
-   [2018년 11월 7일](#November2018a)
-   [2018년 10월 30일](#October2018b)
-   [2018년 10월 9일](#October2018a)
-   [2018년 9월 10일](#September2018b)
-   [2018년 9월 7일](#September2018a)
-   [2018년 8월 8일](#August2018)
-   [2018년 7월 13일](#July2018)
-   [2018년 6월 12일](#June2018)
-   [2018년 5월 15일](#May2018)
-   [2018년 3월 26일](#March2018b)
-   [2018년 3월 1일](#March2018a)
-   [2018년 2월 1일](#February2018)
-   [2017년 12월 14일](#December2017)
-   [2017년 10월 2일](#October2017)
-   [2017년 7월 14일](#July2017b)
-   [2017년 7월 1일](#July2017a)
-   [2017년 5월 22일](#May2017)
-   [2017년 4월 10일](#April2017)
-   [2017년 3월 8일](#March2017)
-   [2016년 12월 1일](#December2016)
-   [2016년 9월 22일](#September2016)
-   [2016년 6월 30일](#June2016b)
-   [2016년 6월 23일](#June2016a)
-   [2016년 3월 10일](#March2016)
-   [2016년 1월 19일](#January2016)
-   [2015년 12월 17일](#December2015)
-   [2015년 9월 21일](#September2015)
-   [2015년 7월 1일](#July2015)

### 2019년 3월 11일
{: #March2019b}

-   이 서비스가 `max_alternatives` 매개변수에 대해 `0` 값을 다시 허용합니다. `0`을 지정하면 서비스가 자동으로 기본값인 `1`을 사용합니다. 3월 4일 서비스 업데이트에 대한 변경사항으로 인해 `0` 값이 오류를 리턴했습니다. (음수 값을 지정하는 경우 서비스가 오류를 리턴합니다.)
-   이 서비스가 `word_alternatives_threshold` 매개변수에 대해 `0` 값을 다시 허용합니다. 3월 4일 서비스 업데이트에 대한 변경사항으로 인해 `0` 값이 오류를 리턴했습니다. (음수 값을 지정하는 경우 서비스가 오류를 리턴합니다.)
-   이제 이 서비스는 최대 소수점 이하 두 자리의 정밀도로 모든 신뢰도 점수를 리턴합니다. 여기에는 음성 내용, 단어 신뢰도, 단어 대체, 키워드 결과 및 화자 레이블에 대한 신뢰도 점수가 포함됩니다.

### 2019년 3월 4일
{: #March2019}

향상된 음성 인식을 위해 다음과 같은 협대역 언어 모델이 업데이트되었습니다.

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

기본적으로 이 서비스는 모든 음성 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 모델을 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 기존 사용자 정의 모델을 업그레이드해야 합니다.

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오.

### 2019년 1월 28일
{: #January2019}

이제 WebSocket 인터페이스가 브라우저 기반 JavaScript 코드에서 토큰 기반 IAM(Identity and Access Management) 인증을 지원합니다. 그 반대에 대한 제한사항이 제거되었습니다. WebSocket `/v1/recognize` 메소드를 사용하여 인증된 연결을 설정하려면 다음을 수행하십시오.

-   IAM 인증을 사용하는 경우 `access_token` 조회 매개변수를 포함시키십시오.
-   Cloud Foundry 서비스 인증 정보를 사용하는 경우 `watson-token` 조회 매개변수를 포함시키십시오.

자세한 정보는 [연결 열기](/docs/services/speech-to-text/websockets.html#WSopen)를 참조하십시오.

### 2018년 12월 20일
{: #December2018b}

2019년 1월 8일을 기준으로 문법 인터페이스가 모든 위치에서 완전히 작동합니다.
{: note}

-   이제 서비스가 음성 인식에 문법을 지원합니다. 문법은 언어 모델 사용자 정의를 지원하는 모든 언어에 대해 베타 기능으로 사용할 수 있습니다. 문법을 사용자 정의 언어 모델에 추가하고 이를 사용하여 서비스가 오디오에서 인식할 수 있는 구문 세트를 제한할 수 있습니다. ABNF(Augmented Backus-Naur Form) 또는 XML 양식으로 문법을 정의할 수 있습니다.

    다음 4개의 메소드를 문법에 대한 작업에 사용할 수 있습니다.
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`은 사용자 정의 언어 모델에 문법 파일을 추가합니다.
    -   `GET /v1/customizations/{customization_id}/grammars`는 사용자 정의 모델의 모든 문법에 대한 정보를 나열합니다.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}`은 사용자 정의 모델의 지정된 문법에 대한 정보를 리턴합니다.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}`은 사용자 정의 모델에서 기존 문법을 제거합니다.

    WebSocket 및 HTTP 인터페이스를 사용하여 문법을 음성 인식에 사용할 수 있습니다. 사용하려는 사용자 정의 모델 및 문법을 식별하려면 `language_customization_id` 및 `grammar_name` 매개변수를 사용하십시오. 현재 음성 인식 요청에는 하나의 문법만 사용할 수 있습니다.

    문법에 대한 자세한 정보는 다음 문서를 참조하십시오.
    -   [사용자 정의 언어 모델에 문법 사용](/docs/services/speech-to-text/grammar.html)
    -   [문법 이해](/docs/services/speech-to-text/grammar-understand.html)
    -   [사용자 정의 언어 모델에 문법 추가](/docs/services/speech-to-text/grammar-add.html)
    -   [음성 인식에 문법 사용](/docs/services/speech-to-text/grammar-use.html)
    -   [문법 관리](/docs/services/speech-to-text/grammar-manage.html)
    -   [예제 문법](/docs/services/speech-to-text/grammar-examples.html)

    인터페이스의 모든 메소드에 대한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.
-   이제 새로운 숫자 교정 기능을 사용하여 세 개 이상의 연속 숫자가 포함된 번호를 마스킹할 수 있습니다. 교정은 음성 내용에서 신용카드 번호와 같은 민감한 개인 정보를 제거하기 위한 것입니다. 인식 요청에서 `redaction` 매개변수를 `true`로 설정하여 이 기능을 사용으로 설정합니다. 이 기능은 미국 영어, 일본어 및 한국어에만 사용할 수 있는 베타 기능입니다. 자세한 정보는 [숫자 교정](/docs/services/speech-to-text/output.html#redaction)을 참조하십시오.
-   이제 다음과 같은 새 독일어 및 프랑스어 모델을 이 서비스에 사용할 수 있습니다.
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    새 모델은 언어 모델 사용자 정의(GA) 및 음향 모델 사용자 정의(베타)를 모두 지원합니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text/custom.html#languageSupport)을 참조하십시오.
-   새로운 미국 영어 언어 모델 `en-US_ShortForm_NarrowbandModel`이 사용 가능합니다. 이 새 모델은 대화식 음성 응답 및 자동화된 고객 지원 지원 솔루션에 사용하기 위한 것입니다. 이 모델은 언어 모델 사용자 정의(GA) 및 음향 모델 사용자 정의(베타)를 지원합니다. 
-   향상된 음성 인식을 위해 다음 언어 모델이 업데이트되었습니다.
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    기본적으로 이 서비스는 모든 음성 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 모델을 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 기존 사용자 정의 모델을 업그레이드해야 합니다.
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

   자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오.
-   이제 서비스가 G.729(`audio/g729`) 형식의 오디오를 지원합니다. 이 서비스는 협대역 오디오에 대해서만 G.729 Annex D를 지원합니다. 지원되는 오디오 형식에 대한 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   이제 영국 영어에 대한 협대역 모델(`en-GB_NarrowbandModel`)에 화자 레이블 기능을 사용할 수 있습니다. 이 기능은 지원되는 모든 언어에 대한 베타 기능입니다. 자세한 정보는 [화자 레이블](/docs/services/speech-to-text/output.html#speaker_labels)을 참조하십시오.
-   사용자 정의 음향 모델에 추가할 수 있는 최대 오디오의 양이 50시간에서 100시간으로 증가되었습니다.

### 2018년 12월 13일
{: #December2018a}

이제 {{site.data.keyword.cloud_notm}} 런던 위치(**eu-gb**)에서 이 서비스를 사용할 수 있습니다. 모든 위치와 마찬가지로 런던에서도 토큰 기반 IAM 인증을 사용합니다. 이 위치에서 작성하는 모든 새 서비스 인스턴스가 IAM 인증을 사용합니다.

### 2018년 11월 12일
{: #November2018b}

이제 이 서비스는 일본어 음성 인식에 대한 스마트 형식화를 지원합니다. 이전에는 이 서비스가 미국 영어 및 스페인어에 대해서만 스마트 형식화를 지원했습니다. 이 기능은 지원되는 모든 언어에 대한 베타 기능입니다. 자세한 정보는 [스마트 형식화](/docs/services/speech-to-text/output.html#smart_formatting)를 참조하십시오.

### 2018년 11월 7일
{: #November2018a}

이제 {{site.data.keyword.cloud_notm}} 도쿄 위치(**jp-tok**)에서 이 서비스를 사용할 수 있습니다. 모든 위치와 마찬가지로 도쿄에서도 토큰 기반 IAM 인증을 사용합니다. 이 위치에서 작성하는 모든 새 서비스 인스턴스가 IAM 인증을 사용합니다.

### 2018년 10월 30일
{: #October2018b}

이 서비스가 모든 위치에 대해 토큰 기반 IAM 인증으로 마이그레이션되었습니다. 이제 모든 {{site.data.keyword.cloud_notm}} 서비스가 IAM 인증을 사용합니다. {{site.data.keyword.speechtotextshort}} 서비스가 다음 날짜에 각 위치에서 마이그레이션되었습니다.

-   댈러스(**us-south**): 2018년 10월 30일
-   프랑크푸르트(**eu-de**): 2018년 10월 30일
-   워싱턴, DC(**us-east**): 2018년 6월 12일
-   시드니(**au-syd**): 2018년 5월 15일

IAM 인증으로의 마이그레이션은 새 서비스 인스턴스와 기존 서비스 인스턴스에 서로 다른 영향을 미칩니다.

-   *임의의 위치에서 작성하는 모든 새 서비스 인스턴스*가 이제 IAM 인증을 사용하여 서비스에 액세스합니다. 전달자 토큰 또는 API 키를 전달할 수 있습니다. 토큰은 호출 시마다 서비스 인증 정보를 임베드하지 않고 인증된 요청을 지원합니다. API 키는 HTTP 기본 인증을 사용합니다. {{site.data.keyword.watson}} SDK를 사용하는 경우 API 키를 전달하고 SDK에서 토큰의 라이프사이클을 관리하도록 할 수 있습니다.
-   *표시된 마이그레이션 날짜 이전에 위치에서 작성한 기존 서비스 인스턴스*는 IAM 인증을 사용하도록 마이그레이션할 때까지 이전 Cloud Foundry 서비스 인증 정보의 `{username}` 및 `{password}`를 계속 인증에 사용할 수 있습니다. IAM 인증으로 마이그레이션하는 방법에 대한 자세한 정보는 [Cloud Foundry 서비스 인스턴스를 리소스 그룹으로 마이그레이션](https://{DomainName}/docs/resources/instance_migration.html)을 참조하십시오.

자세한 정보는 다음 문서를 참조하십시오.

-   서비스 인스턴스가 사용하는 인증 메커니즘에 대해 알아보려면 [{{site.data.keyword.cloud_notm}} 대시보드 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/dashboard/apps){: new_window}에서 해당 인스턴스를 클릭하여 서비스 인증 정보를 보십시오.
-   Watson 서비스에 IAM 토큰을 사용하는 데 대한 자세한 정보는 [IAM 토큰을 사용한 인증](/docs/services/watson/getting-started-iam.html)을 참조하십시오.
-   Watson 서비스에 IAM API 키를 사용하는 데 대한 자세한 정보는 [IAM 서비스 API 키](/docs/services/watson/apikey-bp.html)를 참조하십시오.
-   IAM 인증을 사용하는 예제는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

### 2018년 10월 9일
{: #October2018a}

-   이제 `Content-Type` 헤더가 음성 인식 요청에 대해 선택사항입니다. 이 서비스가 대부분의 오디오에 대한 오디오 형식(MIME 유형)을 자동으로 발견합니다. 다음 형식의 컨텐츠 유형을 계속 지정해야 합니다.
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    표시된 경우 이러한 형식에 대해 지정된 컨텐츠 유형에 샘플링 간격이 포함되어야 하며 선택적으로 오디오의 채널 수와 엔디안이 포함될 수 있습니다. 기타 모든 오디오 형식의 경우 컨텐츠 유형을 생략하거나 컨텐츠 유형을 `application/octet-stream`으로 지정하여 서비스가 형식을 자동으로 검색하도록 할 수 있습니다.

    `curl` 명령을 사용하여 HTTP 인터페이스로 음성 인식 요청을 수행하는 경우 `Content-Type` 헤더를 사용하여 오디오 형식을 지정하거나 `"Content-Type: application/octet-stream"` 또는 `"Content-Type:"`을 지정해야 합니다. 헤더를 완전히 생략하면 `curl`이 기본값인 `application/x-www-form-urlencoded`를 사용합니다. 이 문서에 있는 대부분의 예제에서는 필요한지 여부에 관계없이 음성 인식 요청에 대한 형식을 계속 지정합니다.
    {: important}

    이 변경사항은 다음 메소드에 적용됩니다.
    -   WebSocket 요청의 경우 `/v1/recognize`. 열린 WebSocket 연결을 통해 요청을 시작하기 위해 전송하는 텍스트 메시지의 `content-type` 필드가 이제 선택사항입니다.
    -   동기 HTTP 요청의 경우 `POST /v1/recognize`. `Content-Type` 헤더가 이제 선택사항입니다. (다중 파트 요청의 경우 JSON 메타데이터의 `part_content_type` 필드도 이제 선택사항입니다.)
    -   비동기 HTTP 요청의 경우 `POST /v1/recognitions`. `Content-Type` 헤더가 이제 선택사항입니다. 

    자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   향상된 음성 인식을 위해 브라질 포르투갈어 광대역 모델 `pt-BR_BroadbandModel`이 업데이트되었습니다. 기본적으로 이 서비스는 모든 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 이 모델을 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 기존 사용자 정의 모델을 업그레이드해야 합니다.
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

   자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오.
-   음성 인식 메소드의 `customization_id` 매개변수가 더 이상 사용되지 않으며 향후 릴리스에서 제거됩니다. 음성 인식 요청에 대한 사용자 정의 언어 모델을 지정하려면 대신 `language_customization_id` 매개변수를 사용하십시오. 이 변경사항은 다음 메소드에 적용됩니다.
    -   WebSocket 요청의 경우 `/v1/recognize`
    -   동기 HTTP 요청(다중 파트 요청 포함)의 경우 `POST /v1/recognize`
    -   비동기 HTTP 요청의 경우 `POST /v1/recognitions`
-   2018년 10월 1일을 기준으로 음성 인식을 위해 서비스에 전달하는 모든 오디오에 대해 청구됩니다. 매월 전송하는 오디오의 처음 1,000분이 더 이상 무료로 제공되지 않습니다. 서비스의 가격 플랜에 대한 자세한 정보는 [{{site.data.keyword.cloud_notm}} 카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/catalog/services/speech-to-text){: new_window}에서 {{site.data.keyword.speechtotextshort}} 서비스를 참조하십시오.

### 2018년 12월 10일
{: #September2018b}

초기 릴리스 이후 해결된 문제의 목록은 [해결된 문제](#known_issues)를 참조하십시오.
{: important}

-   이제 이 서비스가 독일어 광대역 모델 `de-DE_BroadbandModel`을 지원합니다. 새 독일어 모델은 언어 모델 사용자 정의(GA) 및 음향 모델 사용자 정의(베타)를 지원합니다.
    -   서비스가 독일어의 말뭉치를 구문 분석하는 방법에 대한 정보는 [영어, 프랑스어, 독일어, 스페인어 및 브라질 포르투갈어 구문 분석](/docs/services/speech-to-text/language-resource.html#corpusLanguages)을 참조하십시오.
    -   독일어로 된 사용자 정의 단어의 유사 발음 작성에 대한 자세한 정보는 [프랑스어, 독일어, 스페인어 및 브라질 포르투갈어에 대한 가이드라인](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR)을 참조하십시오.
-   기존 브라질 포르투갈어 모델 `pt-BR_BroadbandModel` 및 `pt-BR_NarrowbandModel`이 이제 언어 모델 사용자 정의(GA)를 지원합니다. 이러한 모델이 이 지원을 사용하도록 업데이트되지 않았으므로 기존 사용자 정의 음향 모델의 업데이트가 필요하지 않습니다.
    -   서비스가 브라질 포르투갈어의 말뭉치를 구문 분석하는 방법에 대한 정보는 [영어, 프랑스어, 독일어, 스페인어 및 브라질 포르투갈어 구문 분석](/docs/services/speech-to-text/language-resource.html#corpusLanguages)을 참조하십시오.
    -   브라질 포르투갈어로 된 사용자 정의 단어의 유사 발음 작성에 대한 자세한 정보는 [프랑스어, 독일어, 스페인어 및 브라질 포르투갈어에 대한 가이드라인](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR)을 참조하십시오.
-   미국 영어 및 일본어의 광대역 및 협대역 모델의 새 버전을 사용할 수 있습니다.
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    새 모델은 향상된 음성 인식을 제공합니다. 기본적으로 이 서비스는 모든 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 이러한 모델을 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 기존 사용자 정의 모델을 업그레이드해야 합니다.
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

   자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오.
-   키워드 발견 및 단어 대체 기능이 이제 모든 언어에 대해 베타 기능이 아니라 GA(Generally Available)되었습니다. 자세한 정보는 다음을 참조하십시오.
    -   [키워드 발견](/docs/services/speech-to-text/output.html#keyword_spotting)
    -   [단어 대체](/docs/services/speech-to-text/output.html#word_alternatives)

### 해결된 문제
{: #known_issues}

사용자 정의 인터페이스와 연관된 다음과 같은 알려진 문제가 해결되었습니다. 이 목록은 과거에 이 문제점을 경험했을 수 있는 사용자를 위해 유지보수됩니다.

-   사용자 정의 언어 또는 사용자 정의 음향 모델에 데이터를 추가하는 경우 음성 인식에 사용하기 전에 모델을 재훈련해야 합니다. 이 문제점은 다음 시나리오에서 발생합니다.

    1.  사용자가 새 사용자 정의 모델(언어 또는 음향)을 작성하고 모델을 훈련합니다.
    1.  사용자가 추가 리소스(단어, 말뭉치 또는 오디어)를 사용자 정의 모델에 추가하지만 모델을 재훈련하지 않습니다.
    1.  사용자가 사용자 정의 모델을 음성 인식에 사용할 수 없습니다. 서비스가 음성 인식 요청에 사용될 때 다음 양식의 오류를 리턴합니다.

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    이 문제를 해결하려면 사용자가 최신 데이터에 대해 사용자 정의 모델을 재훈련해야 합니다. 그런 다음 사용자가 사용자 정의 모델을 음성 인식에 사용할 수 있습니다.
-   기존 사용자 정의 언어 또는 사용자 정의 음향 모델을 훈련하기 전에 기본 모델의 최신 버전으로 업그레이드해야 합니다. 이 문제점은 다음 시나리오에서 발생합니다.

    1.  사용자에게 업데이트된 모델을 기반으로 하는 기존 사용자 정의 모델(언어 또는 음향)이 있습니다.
    1.  사용자가 먼저 최신 버전의 기본 모델로 업그레이드하지 않고 이전 버전의 기본 모델에 대해 기존 사용자 정의 모델을 훈련합니다.
    1.  사용자가 사용자 정의 모델을 음성 인식에 사용할 수 없습니다. 

    이 문제를 해결하려면 사용자가 `POST /v1/customizations/{customization_id}/upgrade_model` 또는 `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` 메소드를 사용하여 사용자 정의 모델을 최신 버전의 기본 모델로 업그레이드해야 합니다. 그런 다음 사용자가 사용자 정의 모델을 음성 인식에 사용할 수 있습니다.

이러한 문제는 모두 프로덕션에서 해결되었습니다.

### 2018년 9월 7일
{: #September2018a}

세션 기반 HTTP REST 인터페이스가 더 이상 지원되지 않습니다. 세션과 관련된 모든 정보가 문서에서 제거되었습니다. 다음 메소드를 더 이상 사용할 수 없습니다.
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

애플리케이션이 세션 인터페이스를 사용하는 경우 나머지 HTTP REST 인터페이스 중 하나 또는 WebSocket 인터페이스로 마이그레이션해야 합니다. 자세한 정보는 [2018년 8월 8일](#August2018)의 서비스 업데이트를 참조하십시오.

### 2018년 8월 8일
{: #August2018}

**2018년 8월 8일**을 기준으로 세션 기반 HTTP REST 인터페이스가 더 이상 사용되지 않습니다. **2018년 9월 7일**을 기준으로 세션 API의 모든 메소드가 제거되므로 이후 세션 기반 인터페이스를 더 이상 사용할 수 없습니다. 이 즉각적인 지원 중단 및 30일 제거 알림은 다음 메소드에 적용됩니다.

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

애플리케이션이 세션 인터페이스를 사용하는 경우 9월 7일까지 다음 인터페이스 중 하나로 마이그레이션해야 합니다.
{: important}

-   스트림 기반 음성 인식(라이브 유스 케이스 포함)의 경우 중간 결과 및 가장 낮은 대기 시간에 대한 액세스를 제공하는 [WebSocket 인터페이스](/docs/services/speech-to-text/websockets.html)를 사용하십시오.
-   파일 기반 음성 인식의 경우 다음 인터페이스 중 하나를 사용하십시오.

    -   최대 몇 분 길이의 오디오로 구성된 짧은 파일의 경우 [동기 HTTP 인터페이스](/docs/services/speech-to-text/http.html)`(POST /v1/recognize`) 또는 [비동기 HTTP 인터페이스](/docs/services/speech-to-text/async.html)(`POST /v1/recognitions`)를 사용하십시오.
    -   몇 분 이상의 오디오로 구성된 긴 파일의 경우 비동기 HTTP 인터페이스를 사용하십시오. 비동기 HTTP 인터페이스는 단일 요청으로 1GB의 오디오 데이터를 허용합니다.

WebSocket 및 HTTP 인터페이스는 세션 인터페이스와 동일한 결과를 제공합니다(WebSocket 인터페이스만 중간 결과를 제공함). 임의의 인터페이스를 사용하여 애플리케이션 개발을 단순화하는 Watson SDK 중 하나를 사용할 수도 있습니다. 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

### 2018년 7월 13일
{: #July2018}

향상된 음성 인식을 위해 스페인어 협대역 모델 `es-ES_NarrowbandModel`이 업데이트되었습니다. 기본적으로 이 서비스는 모든 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 이 모델을 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 사용자 정의 모델을 업그레이드해야 합니다.

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오.

이 업데이트부터 다음과 같은 두 개 버전의 스페인어 협대역 모델을 사용할 수 있습니다.

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959`(현재)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959`(이전)

다음 버전의 모델을 더 이상 사용할 수 없습니다.

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

현재 사용 불가능한 기본 모델을 기반으로 하는 사용자 정의 모델을 사용하려고 시도하는 인식 요청은 사용자 정의 없이 최신 기본 모델을 사용합니다. 서비스에서 다음과 같은 경고 메시지를 리턴합니다. `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` 사용 불가능한 모델을 기반으로 하는 사용자 정의 모델의 사용을 재개하려면 먼저 이전에 설명된 해당 `upgrade_model` 메소드를 사용하여 모델을 업그레이드해야 합니다.

### 2018년 6월 12일
{: #June2018}

다음 기능은 워싱턴, DC(**us-east**)에서 호스팅되는 애플리케이션에 사용됩니다.

-   이제 이 서비스가 새 API 인증 프로세스를 지원합니다. 자세한 정보는 [2018년 10월 30일 서비스 업데이트](#October2018b)를 참조하십시오.
-   이제 이 서비스가 `X-Watson-Metadata` 헤더 및 `DELETE /v1/user_data` 메소드를 지원합니다. 자세한 정보는 [정보 보안](/docs/services/speech-to-text/information-security.html)을 참조하십시오.

### 2018년 5월 15일
{: #May2018}

다음 기능은 시드니(**au-syd**)에서 호스팅되는 애플리케이션에 사용됩니다.

-   이제 이 서비스가 새 API 인증 프로세스를 지원합니다. 자세한 정보는 [2018년 10월 30일 서비스 업데이트](#October2018b)를 참조하십시오.
-   이제 서비스가 `X-Watson-Metadata` 헤더 및 `DELETE /v1/user_data` 메소드를 지원합니다. 자세한 정보는 [정보 보안](/docs/services/speech-to-text/information-security.html)을 참조하십시오.

### 2018년 3월 26일
{: #March2018b}

-   이제 서비스가 프랑스어 언어 모델 `fr-FR_BroadbandModel`에 대한 언어 모델 사용자 정의를 지원합니다. 프랑스어 모델은 프로덕션에서 언어 모델 사용자 정의에 일반적으로 사용할 수 있습니다.
    -   이 서비스가 프랑스어의 말뭉치를 구문 분석하는 방법에 대한 자세한 정보는 [영어, 프랑스어, 독일어, 스페인어 및 브라질 포르투갈어 구문 분석](/docs/services/speech-to-text/language-resource.html#corpusLanguages)을 참조하십시오.
    -   프랑스어로 된 사용자 정의 단어의 유사 발음 작성에 대한 자세한 정보는 [프랑스어, 독일어, 스페인어 및 브라질 포르투갈어에 대한 가이드라인](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR)을 참조하십시오.
-   향상된 음성 인식을 위해 스페인 및 한국어 협대역 모델 `es-ES_NarrowbandModel` 및 `ko-KR_NarrowbandModel`과 프랑스어 광대역 모델 `fr-FR_BroadbandModel`이 업데이트되었습니다. 기본적으로 이 서비스는 모든 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 이러한 모델 중 하나를 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 사용자 정의 모델을 업그레이드해야 합니다.

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

   자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오.
-   다음 메소드의 `version` 매개변수의 이름이 `base_model_version`으로 변경되었습니다.

    -   WebSocket 요청의 경우 `/v1/recognize`
    -   세션 없는 HTTP 요청의 경우 `POST /v1/recognize`
    -   세션 기반 HTTP 요청의 경우 `POST /v1/sessions`
    -   비동기 HTTP 요청의 경우 `POST /v1/recognitions`

    `base_model_version` 매개변수는 음성 인식에 사용될 기본 모델의 버전을 지정합니다. 자세한 정보는 [업그레이드된 사용자 정의 모델로 인식 요청 작성](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition) 및 [기본 모델 버전](/docs/services/speech-to-text/input.html#version)을 참조하십시오.
-   이제 스마트 형식화가 미국 영어뿐만 아니라 스페인어에 대해서도 지원됩니다. 또한 미국 영어의 경우 이 기능이 키워드 문자열을 마침표, 쉼표, 물음표 및 느낌표의 문장 부호로 변환합니다. 자세한 정보는 [스마트 형식화](/docs/services/speech-to-text/output.html#smart_formatting)를 참조하십시오.

### 2018년 3월 1일
{: #March2018a}

향상된 음성 인식을 위해 스페인어 및 프랑스어 광대역 모델 `es-ES_BroadbandModel` 및 `fr-FR_BroadbandModel`이 업데이트되었습니다. 기본적으로 이 서비스는 모든 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 이러한 모델 중 하나를 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 사용자 정의 모델을 업그레이드해야 합니다.

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오. 이 섹션에는 사용자 정의 모델 업그레이드 규칙, 업그레이드 영향 및 업그레이드된 모델을 사용하기 위한 접근 방식이 제공됩니다. 

### 2018년 2월 1일
{: #February2018}

이제 이 서비스가 음성 인식을 위해 한국어 모델(최소 16kHz로 샘플링된 오디오의 경우 `ko-KR_BroadbandModel` 및 최소 8kHz로 샘플링된 오디오의 경우 `ko-KR_NarrowbandModel`)을 제공합니다. 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.

언어 모델 사용자 정의의 경우 한국어 모델이 프로덕션용으로 GA(Generally Available)되었습니다. 음향 모델 사용자 정의의 경우에는 베타 기능입니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text/custom.html#languageSupport)을 참조하십시오.

-   서비스가 한국어의 말뭉치를 구문 분석하는 방법에 대한 자세한 정보는 [한국어 구문 분석](/docs/services/speech-to-text/language-resource.html#corpusLanguages-koKR)을 참조하십시오.
-   한국어로 된 사용자 정의 단어의 유사 발음 작성에 대한 자세한 정보는 [한국어에 대한 가이드라인](/docs/services/speech-to-text/language-resource.html#wordLanguages-koKR)을 참조하십시오. 

### 2017년 12월 14일
{: #December2017}

-   향상된 음성 인식을 위해 미국 영어 모델 `en-US_BroadbandModel` 및 `en-US_NarrowbandModel`이 업데이트되었습니다. 기본적으로 이 서비스는 모든 인식 요청에 대해 업데이트된 모델을 자동으로 사용합니다. 미국 영어 모델을 기반으로 하는 사용자 정의 언어 또는 사용자 정의 음향 모델이 있는 경우 다음 메소드를 사용하여 업데이트를 활용하도록 사용자 정의 모델을 업그레이드해야 합니다.

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    이 프로시저에 대한 자세한 정보는 [사용자 정의 모델 업그레이드](/docs/services/speech-to-text/custom-upgrade.html)를 참조하십시오. 이 섹션에는 사용자 정의 모델 업그레이드 규칙, 업그레이드 영향 및 업그레이드된 모델을 사용하기 위한 접근 방식이 제공됩니다. 현재 이러한 메소드는 새로운 미국 영어 기본 모델에만 적용됩니다. 그러나 다른 기본 모델의 업그레이드가 사용 가능하게 되면 해당 업그레이드에도 동일한 정보가 적용됩니다.
-   이제 인식 요청을 수행하는 다양한 메소드에 기본 및 사용자 정의 모델의 이전 버전 또는 업그레이드된 버전을 사용하여 요청을 시작하는 데 사용할 수 있는 새로운 `base_model_version` 매개변수가 포함됩니다. `base_model_version` 매개변수는 주로 업그레이드된 사용자 정의 모델에 사용하기 위한 것이지만 사용자 정의 모델 없이 사용할 수도 있습니다. 자세한 정보는 [기본 모델 버전](/docs/services/speech-to-text/input.html#version)을 참조하십시오. 
-   이제 이 서비스가 음향 모델 사용자 정의를 사용 가능한 모든 언어에 대한 베타 기능으로 지원합니다. 모든 언어의 광대역 또는 협대역 모델에 대한 사용자 정의 음향 모델을 작성할 수 있습니다. 음향 모델 사용자 정의를 포함한 사용자 정의에 대한 소개는 [사용자 정의 인터페이스](/docs/services/speech-to-text/custom.html)를 참조하십시오.
-   이제 이 서비스가 영국 영어 모델 `en-GB_BroadbandModel` 및 `en-GB_NarrowbandModel`에 대한 언어 모델 사용자 정의를 지원합니다. 이 서비스는 영국 및 미국 영어 말뭉치와 사용자 정의 단어를 일반적으로 유사한 방식으로 처리하지만 몇 가지 중요한 차이점이 있습니다.
    -   서비스가 영국 영어의 말뭉치를 구문 분석하는 방법에 대한 자세한 정보는 [영어, 프랑스어, 독일어, 스페인어 및 브라질 포르투갈어 구문 분석](/docs/services/speech-to-text/language-resource.html#corpusLanguages)을 참조하십시오.
    -   영국 영어로 된 사용자 정의 단어의 유사 발음 작성에 대한 자세한 정보는 [영어(미국 및 영국)에 대한 가이드라인](/docs/services/speech-to-text/language-resource.html#wordLanguages-enUS-enGB)을 참조하십시오. 특히 영국 영어에는 마침표 또는 대시를 유사 발음에 사용할 수 없습니다.
-   이제 언어 모델 사용자 정의 및 연관된 모든 매개변수가 지원되는 모든 언어(일본어, 스페인어, 영국 영어 및 미국 영어)에 대해 GA(Generally Available)되었습니다.


### 2017년 10월 2일
{: #October2017}

-   이제 사용자 정의 인터페이스가 음향 모델 사용자 정의를 지원합니다. 서비스의 기본 모델을 사용자 환경 및 화자에 맞게 조정하는 사용자 정의 음향 모델을 작성할 수 있습니다. 변환할 오디오의 음향 서명과 보다 밀접하게 일치하는 오디오의 사용자 정의 음향 모델을 채우고 훈련합니다. 그런 다음, 사용자 정의 음향 모델을 인식 요청에 사용하여 음성 인식의 정확도를 높입니다.

    사용자 정의 음향 모델은 사용자 정의 언어 모델을 보완합니다. 사용자 정의 언어 모델로 사용자 정의 음향 모델을 훈련할 수 있으며 음성 인식 중에 두 가지 유형의 모델을 모두 사용할 수 있습니다. 음향 모델 사용자 정의는 미국 영어, 스페인어 및 일본어에만 사용할 수 있는 베타 인터페이스입니다.

    -   사용자 정의 인터페이스에서 지원되는 언어와 각 언어에 대해 사용 가능한 지원 레벨에 대한 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text/custom.html#languageSupport)을 참조하십시오.
    -   서비스의 사용자 정의 인터페이스에 대한 자세한 정보는 [사용자 정의 인터페이스](/docs/services/speech-to-text/custom.html)를 참조하십시오.
    -   사용자 정의 음향 모델 작성에 대한 자세한 정보는 [사용자 정의 음향 모델 작성](/docs/services/speech-to-text/acoustic-create.html)을 참조하십시오.
    -   사용자 정의 음향 모델 사용에 대한 자세한 정보는 [사용자 정의 음향 모델 사용](/docs/services/speech-to-text/acoustic-use.html)을 참조하십시오.
    -   사용자 정의 인터페이스의 모든 메소드에 대한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.
-   이제 언어 모델 사용자 정의를 위해 사용자 정의 언어 모델의 선택적 사용자 정의 가중치를 설정하는 베타 기능이 서비스에 포함됩니다. 사용자 정의 가중치는 서비스 기본 어휘의 단어와 비교하여 사용자 정의 언어 모델의 단어에 지정될 상대적인 가중치를 지정합니다. 훈련 및 음성 인식 중에 사용자 정의 가중치를 설정할 수 있습니다. 자세한 정보는 [사용자 정의 가중치 사용](/docs/services/speech-to-text/language-use.html#weight)을 참조하십시오.
-   `ja-JP_BroadbandModel` 언어 모델이 기본 모델의 개선사항을 적용하도록 업그레이드되었습니다. 업그레이드가 모델을 기반으로 하는 기존 사용자 정의 모델에는 영향을 미치지 않습니다.
-   이제 이 서비스에는 `audio/l16`(선형 16비트 PCM(Pulse-Code Modulation)) 형식으로 제출된 오디오의 엔디안을 지정하는 매개변수가 포함되어 있습니다. 이 형식으로 `rate` 및 `channels` 매개변수를 지정하는 것 이외에 이제 `endianness` 매개변수를 사용하여 `big-endian` `little-endian`을 지정할 수도 있습니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.

### 2017년 7월 14일
{: #July2017b}

-   이제 서비스가 MP3 또는 MPEG(Motion Picture Experts Group) 형식의 오디오 변환을 지원합니다. 지원되는 오디오 형식에 대한 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   언어 모델 사용자 정의 인터페이스가 이제 스페인어를 베타 기능으로 지원합니다. 기본 스페인어 언어 모델 `es-ES_BroadbandModel` 또는 `es-ES_NarrowbandModel` 중 하나를 기반으로 사용자 정의 모델을 작성할 수 있습니다. [사용자 정의 언어 모델 작성](/docs/services/speech-to-text/language-create.html)을 참조하십시오. 스페인어 사용자 정의 언어 모델을 사용하는 인식 요청에 대한 가격은 미국 영어 및 일본어 모델을 사용하는 요청과 동일합니다.
-   이제 새 사용자 정의 언어 모델을 작성하기 위해 `POST /v1/customizations` 메소드에 전달하는 JSON `CreateLanguageModel` 오브젝트에 `dialect` 필드가 포함됩니다. 이 필드는 사용자 정의 모델에 사용될 언어의 통용어를 지정합니다. 기본적으로 통용어는 기본 모델의 언어와 일치합니다. 이 매개변수는 서비스가 다음 통용어 중 하나로 된 음성에 적합한 사용자 정의 모델을 작성할 수 있는 스페인어 모델의 경우에만 의미가 있습니다.
    -   카스티야 스페인어의 경우 `es-ES`(기본값)
    -   라틴 아메리아 스페인어의 경우 `es-LA`
    -   북아메리카(멕시코어) 스페인어의 경우 `es-US` 

    사용자 정의 인터페이스의 `GET /v1/customizations` 및 `GET /v1/customizations/{customization_id}` 메소드는 사용자 정의 모델의 통용어를 출력에 포함합니다. 자세한 정보는 [사용자 정의 언어 모델 작성](/docs/services/speech-to-text/language-create.html#createModel-language) 및 [사용자 정의 언어 모델 나열](/docs/services/speech-to-text/language-models.html#listModels-language)을 참조하십시오.
-   언어 모델 `en-UK_BroadbandModel` 및 `en-UK_NarrowbandModel`의 이름이 더 이상 사용되지 않습니다. 이제 `en-GB_BroadbandModel` 및 `en-GB_NarrowbandModel`이라는 이름으로 모델을 사용할 수 있습니다.

    더 이상 사용되지 않는 `en-UK_{model}` 이름은 계속 작동하지만 `GET /v1/models` 메소드가 해당 이름을 사용 가능한 모델 목록에 더 이상 리턴하지 않습니다. 여전히 `GET /v1/models/{model_id}` 메소드를 사용하여 이름을 직접 조회할 수 있습니다.

### 2017년 7월 1일
{: #July2017a}

-   서비스의 언어 모델 사용자 정의 인터페이스가 지원되는 언어(미국 영어 및 일본어) 모두에 대해 GA(Generally Available)되었습니다. {{site.data.keyword.IBM_notm}}에서는 사용자 정의 언어 모델 작성, 호스팅 또는 관리에 대한 비용을 청구하지 않습니다. 다음 글머리 기호에 설명된 대로 이제 {{site.data.keyword.IBM_notm}}에서 사용자 정의 모델을 사용하는 인식 요청에 대한 오디오의 분당 추가 $0.03(USD)를 청구합니다.
-   {{site.data.keyword.IBM_notm}}에서 다음을 수행하여 서비스에 대한 가격을 업데이트했습니다.
    -   협대역 모델 사용에 대한 추가 가격 제거
    -   대용량 고객에게 누진 계층 가격 제공
    -   미국 영어 또는 일본어 사용자 정의 언어 모델을 사용하는 인식 요청에 대한 오디오의 분당 추가 $0.03(USD) 청구

    가격 업데이트에 대한 자세한 정보는 다음을 참조하십시오.
    -   블로그 게시물 [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   [{{site.data.keyword.cloud_notm}} 카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/catalog/services/speech-to-text){: new_window}의 {{site.data.keyword.speechtotextshort}} 서비스
    -   [가격 FAQ](/docs/services/speech-to-text/faq-pricing.html)
-   더 이상 비어 있는 데이터 오브젝트를 다음 `POST` 요청에 대한 본문으로 전달할 필요가 없습니다.
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    예를 들어, 이제는 다음과 같이 `curl`을 사용하여 `POST /v1/sessions` 메소드를 호출합니다.

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    이제 `curl` 옵션 `--data "{}"`를 옵션과 함께 전달할 필요가 없습니다.

    이러한 `POST` 요청 중 하나에 문제점이 발생하는 경우 비어 있는 데이터 오브젝트를 요청의 본문으로 전달해 보십시오. 비어 있는 오브젝트를 전달해도 어떤 방식으로든 요청의 속성 또는 의미가 변경되지 않습니다.
    {: note}

### 2017년 5월 22일
{: #May2017}

2017년 3월에 처음 발표된 다음과 같은 지원 중단이 이제 적용됩니다.

-   `continuous` 매개변수가 인식 요청을 시작하는 모든 메소드에서 제거됩니다. 이제 서비스가 종료되거나 제한시간이 초과될 때까지(둘 중 먼저 발생하는 것을 기준으로 함) 전체 오디오 스트림을 변환합니다. 이 동작은 이전의 `continuous` 매개변수를 `true`로 설정하는 것과 동등합니다. 기본적으로 이 매개변수가 생략되거나 `false`로 설정된 경우 이전에는 서비스가 비음성 구간(일반적으로 무음)의 처음 0.5초에 변환을 중지했습니다.

    `true`로 설정된 기존 애플리케이션에서는 동작이 변경되지 않습니다. 이 매개변수를 `false`로 설정했거나 기본 동작에 의존한 애플리케이션은 변경될 수 있습니다. 요청에 이 매개변수가 지정된 경우 이제 서비스가 알 수 없는 매개변수에 대한 경고 메시지를 리턴하여 응답합니다.

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    경고가 발생해도 요청에 성공하며 기존 세션 또는 WebSocket 연결에는 영향을 미치지 않습니다.

    {{site.data.keyword.IBM_notm}}에서는 `continuous=false`를 지정하는 것은 가치가 거의 없으며 변환 정확도를 감소시킬 수 있다는 개발자 커뮤니티의 압도적인 피드백에 응답하여 이 매개변수를 제거했습니다.
-   오디오를 전송하지 않고 세션 제한시간 초과를 방지할 수는 없습니다.
    -   WebSocket 인터페이스를 사용하는 경우 클라이언트는 더 이상 `action` 매개변수가 `no-op`로 설정된 상태로 JSON 텍스트 메시지를 전송하여 연결을 유지할 수 없습니다. `no-op` 메시지를 전송하면 오류가 생성되지는 않지만 적용되지 않습니다.
    -   HTTP 인터페이스를 통해 세션을 사용하는 경우 클라이언트는 더 이상 `GET /v1/sessions/{session_id}/recognize` 요청을 전송하여 세션을 연장할 수 없습니다. 이 메소드는 활성 세션의 상태를 계속 리턴하지만 세션을 활성 상태로 유지하지 않습니다. 이제 다음을 수행하여 세션을 활성 상태로 유지할 수 있습니다.
    -   30초 비활성 제한시간 초과를 방지하려면 `inactivity_timeout` 매개변수를 `-1`로 설정하십시오.
    -   30초 세션 제한시간 초과를 방지하려면 무음을 포함한 임의의 오디오 데이터를 서비스에 전송하십시오. 세션을 연장하기 위해 전송하는 무음을 포함하여 서비스에 전송하는 모든 데이터의 지속 시간 동안 비용이 청구됩니다.

    자세한 정보는 [제한시간](/docs/services/speech-to-text/input.html#timeouts)을 참조하십시오. 이상적으로, 변환을 위해 오디오를 얻기 직전에 세션을 설정하고 실시간에 가까운 속도로 오디오를 전송하여 세션을 유지할 수 있습니다. 또한 애플리케이션이 닫힌 세션 또는 연결에서 정상적으로 복구되는지 확인하십시오.

    {{site.data.keyword.IBM_notm}}에서 모든 사용자에게 동급 최고의 대기 시간이 짧은 음성 인식 서비스를 계속 제공할 수 있도록 이 기능을 제거했습니다.

### 2017년 4월 10일
{: #April2017}

-   이제 서비스가 다음 광대역 모델에 대해 화자 레이블 기능을 지원합니다.
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    자세한 정보는 [화자 레이블](/docs/services/speech-to-text/output.html#speaker_labels)을 참조하십시오.
-   이제 이 서비스는 Opus 또는 Vorbis 코덱을 사용하는 WebM(Web Media) 오디오 형식을 지원합니다. 또한 Opus 코덱 외에도 Vorbis 코덱을 사용하는 Ogg 오디오 형식을 지원합니다. 지원되는 오디오 형식에 대한 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   이제 이 서비스는 CORS(Cross-Origin Resource Sharing)를 지원하여 브라우저 기반 클라이언트가 서비스를 직접 호출할 수 있도록 합니다. 자세한 정보는 [CORS 지원](/docs/services/speech-to-text/developer-overview.html#cors)을 참조하십시오.
-   비동기 HTTP 인터페이스가 화이트리스트에 있는 콜백 URL에 대한 등록을 제거하는 `POST /v1/unregister_callback` 메소드를 제공합니다. 자세한 정보는 [콜백 URL 등록 취소](/docs/services/speech-to-text/async.html#unregister)를 참조하십시오.
-   특히 긴 오디오 파일에 대한 인식 요청 중에 WebSocket 인터페이스가 더 이상 제한시간 초과되지 않습니다. 제한시간이 초과되지 않도록 JSON `start` 메시지에 중간 결과를 요청할 필요가 없습니다. (이 문제는 2016년 3월 10일의 릴리스 정보에서 설명되었습니다.)
-   존재하지 않는 사용자 정의 모델을 삭제하려고 시도하면 `DELETE /v1/customizations/{customization_id}` 메소드가 HTTP 응답 코드 401을 리턴합니다.
-   존재하지 않는 말뭉치를 삭제하려고 시도하면 `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드가 HTTP 응답 코드 400을 리턴합니다.

### 2017년 3월 8일
{: #March2017}

이제 비동기 HTTP 인터페이스가 GA(Generally Available)되었습니다. 이 날짜 이전에는 베타 기능이었습니다.

### 2016년 12월 1일
{: #December2016}

-   이제 이 서비스가 미국 영어, 스페인어 또는 일본어로 된 협대역 오디오에 대해 베타 화자 레이블 기능을 제공합니다. 이 기능은 여러 사람 간의 대화에서 어떤 화자가 어떤 단어를 말했는지를 식별합니다. 세션 없음, 세션 기반, 비동기 및 WebSocket 인식 메소드 각각에 `speaker_labels` 매개변수가 포함됩니다. 이 매개변수는 화자 레이블이 응답에 포함될지 여부를 표시하기 위한 부울 값을 허용합니다. 이 기능에 대한 자세한 정보는 [화자 레이블](/docs/services/speech-to-text/output.html#speaker_labels)을 참조하십시오.
-   이제 베타 언어 모델 사용자 정의 인터페이스가 미국 영어 이외에 일본어에도 지원됩니다. 이 인터페이스의 모든 메소드가 일본어를 지원합니다. 자세한 정보는 다음 섹션을 참조하십시오.
    -   자세한 정보는 [사용자 정의 언어 모델 작성](/docs/services/speech-to-text/language-create.html) 및 [사용자 정의 언어 모델 사용](/docs/services/speech-to-text/language-use.html)을 참조하십시오.
    -   말뭉치 텍스트 파일 추가에 대한 일반 및 일본어 특정 고려사항은 [말뭉치 파일 준비](/docs/services/speech-to-text/language-resource.html#prepareCorpus) 및 [말뭉치 파일을 추가하면 어떻게 됩니까?](/docs/services/speech-to-text/language-resource.html#parseCorpus)를 참조하십시오.
    -   사용자 정의 언어에 대한 `sounds_like` 필드 추가 시 일본어 특정 가이드라인은 [일본어에 대한 가이드라인](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP)을 참조하십시오.
    -   사용자 정의 인터페이스의 모든 메소드에 대한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.
-   이제 언어 모델 사용자 정의 인터페이스에 지정된 말뭉치 관련 정보를 나열하는 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드가 포함됩니다. 이 메소드는 사용자 정의 모델에 말뭉치를 추가하는 요청의 상태를 모니터하는 데 유용합니다. 자세한 정보는 [사용자 정의 언어 모델의 말뭉치 나열](/docs/services/speech-to-text/language-corpora.html#listCorpora)을 참조하십시오.
-   `GET /v1/customizations/{customization_id}/words` 및 `GET /v1/customizations/{customization_id}/words/{word_name}` 메소드에서 리턴되는 JSON 출력에 각 단어의 `count` 필드가 포함됩니다. 이 필드는 던어가 모든 말뭉치에서 발견된 횟수를 표시합니다. 단어가 말뭉치에서 추가되기 전에 모델에 사용자 정의 단어를 추가하는 경우 count는 `1`에서 시작합니다. 단어가 먼저 말뭉치에서 추가되고 나중에 수정된 경우 count에는 단어가 말뭉치에서 발견된 횟수만 반영됩니다. 자세한 정보는 [사용자 정의 언어 모델의 단어 나열](/docs/services/speech-to-text/language-words.html#listWords)을 참조하십시오.

`count` 필드가 존재하기 전에 작성된 사용자 정의 모델의 경우 이 필드는 항상 `0`으로 유지됩니다. 이러한 모델에 대한 필드를 업데이트하려면 모델의 말뭉치를 다시 추가하고 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 요청에 `allow_overwrite` 매개변수를 포함하십시오.
    -   이제 `GET /v1/customizations/{customization_id}/words` 메소드에 단어가 나열될 순서를 제어하는 `sort` 조회 매개변수가 포함됩니다. 이 매개변수는 단어가 정렬되는 방식을 표시하는 두 개의 인수 `alphabetical` 또는 `count`를 허용합니다. 선택적 `+` 또는 `-`를 인수 앞에 추가하여 결과가 오름차순 또는 내림차순으로 정렬되는지를 표시할 수 있습니다. 기본적으로 이 메소드는 단어를 알파벳 오름차순으로 표시합니다. 자세한 정보는 [사용자 정의 언어 모델의 단어 나열](/docs/services/speech-to-text/language-words.html#listWords)을 참조하십시오.

 `count` 필드가 도입되기 전에 작성된 사용자 정의 모델의 경우 `count` 인수를 `sort` 매개변수와 함께 사용하는 것은 의미가 없습니다. 이러한 모델에는 기본 `alphabetical` 인수를 사용하십시오.
-   `GET /v1/customizations/{customization_id}/words` 및 `GET /v1/customizations/{customization_id}/words/{word_name}` 메소드에서 JSON 응답의 일부로 리턴될 수 있는 `error` 필드가 이제 배열입니다. 서비스에서 사용자 정의 단어의 정의에 대한 하나 이상의 문제점을 발견한 경우 이 필드에 정의의 각 문제점 요소가 나열되고 해당 문제점에 대해 설명하는 메시지가 제공됩니다. 자세한 정보는 [사용자 정의 언어 모델의 단어 나열](/docs/services/speech-to-text/language-words.html#listWords)을 참조하십시오.
-   인식 메소드의 `keywords_threshold` 및 `word_alternatives_threshold` 매개변수가 더 이상 널값을 허용하지 않습니다. 응답에서 키워드 및 단어 대체를 생략하려면 이러한 매개변수를 생략하십시오. 지정된 값은 float여야 합니다.

서비스 인스턴스에 대한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

### 2016년 9월 22일
{: #September2016}

-   이제 이 서비스가 미국 영어에 대한 새로운 베타 언어 모델 사용자 정의 인터페이스를 제공합니다. 이 인터페이스를 사용하면 도메인 특정 용어를 포함하는 사용자 정의 언어 모델을 작성하여 서비스의 기본 어휘 및 언어 모델을 사용자 조정할 수 있습니다. 사용자 정의 단어를 개별적으로 추가하거나 서비스가 말뭉치에서 추출하도록 할 수 있습니다. 서비스의 인터페이스 중 하나에서 제공되는 음성 인식 메소드에 사용자 정의 모델을 사용하려면 `customization_id` 조회 매개변수를 전달하십시오. 자세한 정보는 다음을 참조하십시오.
    -   [사용자 정의 언어 모델 작성](/docs/services/speech-to-text/language-create.html)
    -   [사용자 정의 언어 모델 사용](/docs/services/speech-to-text/language-use.html)
    -   [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}
-   이제 지원되는 오디오 형식의 모델에 `audio/mulaw`가 포함됩니다. 이 형식은 u-law(또는 mu-law) 데이터 알고리즘을 사용하여 인코딩된 단일 채널 오디오를 제공합니다. 이 형식을 사용하는 경우 오디오가 캡처되는 샘플링 속도로 지정해야 합니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   이제 `GET /v1/models` 및 `GET /v1/models/{model_id}` 메소드가 각 언어 모델에 대한 출력의 일부로 `supported_features` 필드를 리턴합니다. 추가 정보에 모델이 사용자 정의를 지원하는지 여부가 설명되어 있습니다. 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

### 2016년 6월 30일
{: #June2016b}

이제 베타 비동기 HTTP 인터페이스가 서비스에서 지원되는 모든 언어를 지원합니다. 이 인터페이스는 이전에 미국 영어에만 사용 가능했습니다. 자세한 정보는 [비동기 HTTP 인터페이스](/docs/services/speech-to-text/async.html) 및 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

### 2016년 6월 23일
{: #June2016a}

-   이제 베타 비동기 HTTP 인터페이스를 사용할 수 있습니다. 이 인터페이스는 비차단 HTTP 호출을 통해 미국 언어 변환에 대한 전체 인식 기능을 제공합니다. 콜백 URL을 등록하고 사용자 지정 시크릿 문자열을 제공하여 디지털 서명으로 인증 및 데이터 무결성을 달성할 수 있습니다. 자세한 정보는 [비동기 HTTP 인터페이스](/docs/services/speech-to-text/async.html) 및 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.
-   최종 음성 내용에서 날짜, 시간, 일련의 숫자 및 번호, 전화번호, 통화 가치 및 인터넷 주소를 보다 일반적인 표시로 변환하는 베타 스마트 형식화 기능. 인식 요청에 대해 `smart_formatting` 매개변수를 `true`로 설정하여 이 기능을 사용으로 설정합니다. 이 기능은 미국 영어에만 사용할 수 있는 베타 기능입니다. 자세한 정보는 [스마트 형식화](/docs/services/speech-to-text/output.html#smart_formatting)를 참조하십시오.
-   이제 음성 인식에 지원되는 모델의 목록에 최소 16kHz로 샘플링되는 프랑스어로 된 오디오에 대한 `fr-FR_BroadbandModel`이 포함됩니다. 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.
-   이제 지원되는 오디오 형식의 목록에 `audio/basic`이 포함됩니다. 이 형식은 8kHz로 샘플링되는 8비트 u-law(또는 mu-law) 데이터를 사용하여 인코딩되는 단일 채널 오디오를 제공합니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   다양한 인식 메소드가 요청에 포함된 올바르지 않은 조회 매개변수 또는 JSON 필드에 대한 메시지를 포함하는 `warnings` 응답을 리턴할 수 있습니다. 경고의 형식이 변경되었습니다. 예를 들어, `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."`가 이제는 `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`입니다.
-   데이터를 서비스에 전달하지 않는 HTTP `POST` 요청의 경우 `{}` 양식의 비어 있는 요청 본문을 포함시켜야 합니다. `curl` 명령에 `--data` 옵션을 사용하여 비어 있는 데이터를 전달합니다.

### 2016년 3월 10일
{: #March2016}

-   이제 두 가지 데이터 전송 양식(원샷 전달 및 스트리밍) 모두는 WebSocket 인터페이스에서와 마찬가지로 오디오 데이터에 100MB의 크기 한계를 부과합니다. 이전에는 원샷 접근 방식의 최대 데이터 한계가 4MB였습니다. 자세한 정보는 [오디오 전송](/docs/services/speech-to-text/input.html#transmission)(모든 인터페이스의 경우) 및 [오디오 전송 및 인식 결과 수신](/docs/services/speech-to-text/websockets.html#WSaudio)(WebSocket 인터페이스의 경우)을 참조하십시오. WebSocket 섹션에서도 WebSocket 인터페이스에서 적용되는 4MB의 최대 프레임 또는 메시지 크기에 대해 설명합니다.
-   이제 요청에 포함된 올바르지 않은 조회 매개변수 또는 JSON 필드에 대한 경고 메시지의 배열이 인식 요청의 JSON 응답에 포함될 수 있습니다. 배열의 각 요소는 경고의 속성에 대해 설명하는 문자열이며 올바르지 않은 인수 문자열의 배열이 다음에 나옵니다. 예를 들어, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`입니다. 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.
-   베타 *Apple&reg; iOS 운영 체제용 {{site.data.keyword.watson}} Speech SDK(Software Development Kit)*는 더 이상 사용되지 않습니다. 대신 *Apple&reg; iOS 운영 체제용 {{site.data.keyword.watson}} SDK*를 사용하십시오. 새 SDK는 GitHub의 `watson-developer-cloud` 네임스페이스에 있는 [ios-sdk 저장소 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/ios-sdk){: new_window}에서 사용할 수 있습니다.

현재 WebSocket 인터페이스의 알려진 문제는 다음과 같습니다.

-   이 서비스가 특히 긴 오디오 파일의 인식 요청에 대한 최종 결과를 생성하는 데 몇 분이 걸릴 수 있습니다. WebSocket 인터페이스의 경우 서비스가 응답을 준비하는 동안 기본 TCP 연결이 유휴 상태로 유지됩니다. 따라서 제한시간 초과로 인해 연결이 닫힐 수 있습니다. WebSocket 인터페이스가 제한시간 초과되지 않도록 하려면 요청을 시작하기 위한 `start` 메시지의 JSON에서 중간 결과를 요청하십시오(`\"interim_results\": \"true\"`). 필요하지 않은 경우 중간 결과를 버릴 수 있습니다. 이 문제는 향후 업데이트에서 해결됩니다.

### 2016년 1월 19일
{: #January2016}

2016년 1월 19일에 이 서비스가 새로운 욕설 필터링 기능을 포함하도록 업데이트되었습니다. 기본적으로 이 서비스는 미국 영어 오디오의 변환 결과에서 욕설을 검열합니다. 자세한 정보는 [욕설 필터링](/docs/services/speech-to-text/output.html#profanity_filter)을 참조하십시오.

### 2015년 12월 17일
{: #December2015}

-   이제 이 서비스가 키워드 발견 기능을 제공합니다. 입력 오디오에서 일치될 키워드 문자열의 배열을 지정할 수 있습니다. 단어가 키워드의 일치사항으로 간주되기 위해 충족해야 하는 사용자 정의 신뢰수준도 정의해야 합니다. 자세한 정보는 [키워드 발견](/docs/services/speech-to-text/output.html#keyword_spotting)을 참조하십시오.

    키워드 발견 기능은 베타 기능입니다.
    {: note}
-   이제 이 서비스가 단어 대체 기능을 지원합니다. 이 기능은 사용자 정의 신뢰수준을 충족하는 입력 오디오의 단어에 대한 대체 가설을 리턴합니다. 자세한 정보는 [단어 대체](/docs/services/speech-to-text/output.html#word_alternatives)를 참조하십시오.

    단어 대체 기능은 베타 기능입니다.
    {: note}
-   이 서비스는 변환 모델에 더 많은 언어를 지원합니다(영국 영어에 대한 `en-UK_BroadbandModel` 및 `en-UK_NarrowbandModel`과 현대 표준 아랍어에 대한 `ar-AR_BroadbandModel`). 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.
-   HTTP 인식 요청에 10분 플랫폼 제한시간이 더 이상 적용되지 않습니다. 이제 이 서비스는 인식이 진행 중인 동안 20초마다 응답 JSON 오브젝트에 공백 문자를 전송하여 연결을 활성 상태로 유지합니다. 자세한 정보는 [제한시간](/docs/services/speech-to-text/input.html#timeouts)을 참조하십시오.
-   이 서비스는 세션 기반 HTTP 메소드 `GET /v1/sessions/{session_id}/observe_result` 및 `POST /v1/sessions/{session_id}/recognize`에 대해 HTTP 상태 코드 490을 더 이상 리턴하지 않으며 대신 HTTP 상태 코드 400으로 응답합니다.

    이제 이 서비스는 세션 기반 메소드의 오류에 대해 리턴하는 JSON 응답에 새로운 `session_closed` 필드도 포함합니다. 오류의 결과로 세션이 닫히는 경우 이 필드가 `true`로 설정됩니다. 메소드에 대해 가능한 리턴 코드에 대한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.
-   `curl` 명령을 사용하여 이 서비스로 오디오를 변환하는 경우 더 이상 `--limit-rate` 옵션을 사용하여 초당 40,000바이트보다 더 빠른 속도로 데이터를 전송할 필요가 없습니다.

### 2015년 9월 21일
{: #September2015}

-   두 개의 새 베타 모바일 SDK를 음성 서비스에 사용할 수 있습니다. SDK를 사용하면 모바일 애플리케이션에서 {{site.data.keyword.speechtotextshort}} 및 {{site.data.keyword.texttospeechshort}} 서비스 모두와 상호작용할 수 있습니다.
    -   *Google Android&trade; 플랫폼용 {{site.data.keyword.watson}} Speech SDK*는 실시간으로 {{site.data.keyword.speechtotextshort}} 서비스에 오디오를 스트리밍하고 사용자가 말할 때 오디오의 음성 내용을 수신하도록 지원합니다. 이 프로젝트에는 두 음성 서비스 모두와의 상호작용을 보여주는 예제 애플리케이션이 포함되어 있습니다. 이 SDK는 GitHub의 `watson-developer-cloud` 네임스페이스에 있는 [speech-android-sdk 저장소 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window}에서 사용할 수 있습니다.
    -   *Apple&reg; iOS 운영 체제용 {{site.data.keyword.watson}} Speech SDK*는 {{site.data.keyword.speechtotextshort}} 서비스에 오디오를 스트리밍하고 응답으로 오디오의 음성 내용을 수신하도록 지원합니다. 이 SDK는 GitHub의 `watson-developer-cloud` 네임스페이스에 있는 [speech-ios-sdk 저장소 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window}에서 사용할 수 있습니다.

    두 SDK는 모두 {{site.data.keyword.cloud_notm}} 서비스 인증 정보 또는 인증 토큰을 사용하여 음성 서비스에 인증하도록 지원합니다.

    SDK는 베타이므로 나중에 변경될 수 있습니다.
    {: note}
-   이 서비스는 두 개의 새로운 언어, 브라질 포르투갈어 및 중국어(간체)를 지원합니다. 이러한 새로운 언어에 대한 모델은 `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel` 및 `zh-CN_NarrowbandModel`입니다. 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.
-   HTTP `POST` 요청 `/v1/sessions/{session_id}/recognize` 및 `/v1/recognize`와 WebSocket `/v1/recognize` 요청은 Opus 코덱을 사용하는 Ogg 형식 파일에 대한 새 매체 유형 `audio/ogg;codecs=opus`의 변환을 지원합니다. 또한 이러한 메소드에 대한 `audio/wav` 형식이 모든 인코딩을 지원합니다. 선형 PCM 인코딩 사용에 대한 제한사항이 제거되었습니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   이제 이 서비스는 HTTP 인터페이스를 사용하여 긴 오디오 파일을 변환할 때 제한시간 초과를 해결하도록 지원합니다. 세션을 사용할 때 `GET /v1/sessions/{session_id}/observe_result` 및 `POST /v1/sessions/{session_id}/recognize` 메소드에 장기 수행 인식 태스크의 순서 ID를 지정하여 긴 폴링 패턴을 사용할 수 있습니다. 이러한 메소드의 새 `sequence_id` 매개변수를 사용하면 인식 요청을 제출하기 이전, 도중 또는 이후에 결과를 요청할 수 있습니다.
-   이제 이 서비스는 미국 영어 언어 모델 `en_US_BroadbandModel` 및 `en_US_NarrowbandModel`에 대해 많은 고유 명사를 올바르게 대문자로 시작합니다. 예를 들어, 이 서비스가 "barack obama graduated from columbia university" 대신 "Barack Obama graduated from Columbia University"라고 읽는 텍스트를 새롭게 리턴합니다. 애플리케이션이 어떤 방식으로든 고유 명사의 대소문자에 민감한 경우 이 변경사항에 관심이 있을 수 있습니다.
-   HTTP `DELETE /v1/sessions/{session_id}` 요청이 상태 코드 415 "지원되지 않는 매체 유형"을 리턴하지 않습니다. 이 리턴 코드는 메소드에 대한 문서에서 제거되었습니다.

### 2015년 7월 1일
{: #July2015}

2015년 7월 1일에 이 서비스가 베타에서 GA(General Availability)로 변경되었습니다. 베타 및 GA 버전의 {{site.data.keyword.speechtotextshort}} API 간에 다음과 같은 차이가 있습니다. GA 릴리스에서는 사용자가 서비스의 새 버전으로 업그레이드해야 합니다.

-   GA 버전의 HTTP API는 베타 버전과 호환 가능합니다. 명시적으로 모델 이름을 지정한 경우에만 기존 애플리케이션 코드를 변경해야 합니다. 예를 들어, GitHub의 서비스에 사용 가능한 샘플 코드에는 `demo.js` 파일에 다음과 같은 코드 행이 포함되어 있습니다.

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    이 행은 베타 버전의 서비스에 대한 기본 모델인 `WatsonModel`을 지정했습니다. 애플리케이션에서도 이 모델을 지정한 경우에는 GA 버전에서 지원되는 새 모델 중 하나를 사용하도록 변경해야 합니다. 자세한 정보는 다음 글머리 기호를 참조하십시오.
-   이제 이 서비스가 WebSocket 연결을 통해 클라이언트와 서비스 간의 직접 상호작용을 위한 새 프로그래밍 모델을 지원합니다. 클라이언트는 이 모델을 사용하여 서비스와 직접 통신하기 위한 인증 토큰을 얻을 수 있습니다. 이 토큰을 사용하면 {{site.data.keyword.cloud_notm}}의 서버 측 프록시 애플리케이션에서 클라이언트 대신 서비스를 호출할 필요가 없게 됩니다. 토큰은 클라이언트가 서비스와 상호작용하는 데 선호되는 방법입니다.

    이 서비스는 서버 측 프록시에 의존하여 클라이언트와 서비스 간에 오디오 및 메시지를 릴레이하는 이전 프로그래밍 모델을 계속 지원합니다. 그러나 새 모델이 더 효율적이며 더 높은 처리량을 제공합니다. 새 프로그래밍 모델에 대한 자세한 정보는 [{{site.data.keyword.watson}} 서비스에 대한 프로그래밍 모델](/docs/services/watson/getting-started-develop.html)을 참조하십시오.
-   이제 `POST /v1/sessions` 및 `POST /v1/recognize` 메소드가 WebSocket `/v1/recognize` 메소드와 함께 `model` 조회 매개변수를 지원합니다. 이 매개변수를 사용하여 오디오에 대한 다음 정보를 지정할 수 있습니다.

    -   언어: *영어*, *일본어* 또는 *스페인어*
    -   최소 샘플링 속도: *광대역*(16kHz) 또는 *협대역*(8kHz)

    자세한 정보는 [언어 및 모델](/docs/services/speech-to-text/models.html)을 참조하십시오.
    -   이제 `recognize` 메소드의 `Content-Type` 헤더가 `audio/flac` 및 `audio/l16` 이외에 WAV(Waveform Audio File Format) 파일용 `audio/wav`를 지원합니다. 지원되는 오디오 형식에 대한 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.
-   이제 `recognize` 메소드가 애플리케이션 요구사항에 맞게 서비스를 사용자 조정하는 데 사용할 수 있는 다수의 새 조회 매개변수를 지원합니다.
    -   `inactivity_timeout` 매개변수는 서비스가 스트리밍 모드에서 무음(음성 없음)을 발견하는 경우 연결을 닫는 제한시간 값(초)을 설정합니다. 기본적으로 이 서비스는 30초 동안의 무음 후 세션을 종료합니다. [제한시간](/docs/services/speech-to-text/input.html#timeouts)을 참조하십시오.
    -   `max_alternatives` 매개변수는 오디오 음성 내용에 가장 적합한 *n*개의 대체 가설을 리턴하도록 서비스에 지시합니다. [최대 대체 수](/docs/services/speech-to-text/output.html#max_alternatives)를 참조하십시오.
    -   `word_confidence` 매개변수는 변환의 각 단어에 대한 신뢰도 점수를 리턴하도록 서비스에 지시합니다. [단어 신뢰도](/docs/services/speech-to-text/output.html#word_confidence)를 참조하십시오.
    -   `timestamps` 매개변수는 변환의 각 단어에 대해 오디오 시작에 상대적인 시작 및 종료 시간을 리턴하도록 서비스에 지시합니다. [단어 시간소인](/docs/services/speech-to-text/output.html#word_timestamps)을 참조하십시오.
-   이제 `GET /v1/sessions/{session_id}/observeResult` 메소드의 이름이 `GET /v1/sessions/{session_id}/observe_result`로 변경되었습니다. 이전 버전과의 호환성을 위해 `observeResult`라는 이름이 계속 지원됩니다.
-   이제 `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize` 및 `POST /v1/recognize` 메소드에 헤더 매개변수 `X-WDC-PL-OPT-OUT`이 포함됩니다. 이 매개변수는 서비스가 향후 결과를 개선하기 위해 요청의 오디오 및 변환 데이터를 사용하는지 여부를 제어합니다. WebSocket 인터페이스에 동등한 조회 매개변수가 포함되어 있습니다. 서비스가 오디오 및 변환 결과를 사용하지 못하도록 하려면 `1` 값을 지정하십시오. 이 매개변수는 현재 요청에만 적용됩니다. 새 헤더가 베타 API의 `X-logging` 헤더를 대체합니다. [{{site.data.keyword.watson}} 서비스에 대한 요청 로깅 제어](../common/getting-started-logging.html)를 참조하십시오.
-   이제 이 서비스의 스트리밍 모드에서 세션당 데이터의 한계는 100MB입니다. `Transfer-Encoding` 헤더에 `chunked` 값을 지정하여 스트리밍 모드를 지정합니다. 오디오 파일의 원샷(One-shot) 전달 시 전송되는 데이터에는 여전히 4MB의 크기 한계가 부과됩니다. [오디오 전송](/docs/services/speech-to-text/input.html#transmission)을 참조하십시오.
-   `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize` 및 `/v1/recognize` 메소드에 대한 오류 코드 415("지원되지 않는 매체 유형")가 추가되었습니다.
-   `/v1/sessions/{session_id}/recognize` 메소드에 대한 `POST` 및 `GET` 요청의 경우 다음과 같은 오류 코드가 수정되었습니다.
    -   오류 코드 404("Session_id를 찾을 수 없음")에 더 구체적인 메시지가 있습니다(`POST` 및 `GET`).
    -   오류 코드 503("세션이 이미 요청을 처리 중입니다. 동일한 세션에서 동시 요청이 허용되지 않습니다. 이 오류 후 세션이 활성 상태로 유지됩니다.")에 더 구체적인 메시지가 있습니다(`POST`만 해당).
-   `/v1/sessions` 및 `/v1/recognize` 메소드에 대한 HTTP `POST` 요청에 대해 오류 코드 503("사용 불가능한 서비스")이 리턴될 수 있습니다. `/v1/recognize` 메소드를 사용하여 WebSocket 연결을 작성하는 경우 이 오류 코드가 리턴될 수도 있습니다.
