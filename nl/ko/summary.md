---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# 매개변수 요약
{: #summary}

음성 인식에 사용할 수 있는 모든 매개변수에 대한 요약은 다음과 같습니다. {{site.data.keyword.speechtotextshort}} 서비스의 모든 메소드에 대한 자세한 정보는 [API 참조](https://{DomainName}/apidocs/speech-to-text){: external}를 참조하십시오.
{: shortdesc}

음성 인식 요청을 작성할 때 다음과 같은 기본 요구사항을 고려하십시오.

-   메소드 이름은 대소문자를 구분합니다.
-   HTTP 요청 헤더는 대소문자를 구분하지 않습니다.
-   HTTP 및 WebSocket 조회 매개변수는 대소문자를 구분합니다.
-   JSON 필드 이름은 대소문자를 구분합니다.
-   모든 JSON 응답 컨텐츠는 UTF-8 문자 세트로 되어 있습니다.

또한 다음과 같은 서비스 특정 요구사항을 고려하십시오.

-   하나의 입력 오디오만 지정해야 합니다. 기타 모든 매개변수는 선택사항입니다.
-   올바르지 않은 조회 매개변수 또는 JSON 필드를 입력의 일부로 지정하면 올바르지 않은 인수에 대해 설명하는 `warnings` 필드가 응답에 포함됩니다. 경고가 발생해도 요청에 성공합니다.

## access_token
{: #summary-access-token}

IAM(Identity and Access Management) 인증을 사용하는 경우 WebSocket 인터페이스와의 인증된 연결을 설정하는 데 사용하는 선택적 IAM 액세스 토큰입니다. 자세한 정보는 [연결 열기](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen)를 참조하십시오.

<table>
  <caption>표 1. access_token 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

사용자 환경 및 화자의 음향 특성에 맞게 조정된 사용자 정의 음향 모델의 선택적 사용자 정의 ID입니다. 기본적으로 사용자 정의 모델은 사용되지 않습니다. 자세한 정보는 [사용자 정의 모델](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input)을 참조하십시오.

<table>
  <caption>표 2. acoustic_customization_id 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      모든 언어에 대한 베타
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## audio_metrics
{: #summary-audio-metrics}

서비스가 입력 오디오의 신호 특성에 대한 메트릭을 리턴하는지 여부를 표시하는 선택적 부울입니다. 기본적으로(`false`) 서비스는 오디오 메트릭을 리턴하지 않습니다. 자세한 정보는 [오디오 메트릭](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)을 참조하십시오.

<table>
  <caption>표 3. audio_metrics 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

기본 모델의 선택적 버전입니다. 이 매개변수는 주로 새 기본 모델용으로 업데이트된 사용자 정의 모델에 사용하기 위한 것이지만 사용자 정의 모델 없이 사용할 수 있습니다. 기본값은 매개변수가 사용자 정의 모델과 함께 사용되는지 여부에 따라 다릅니다. 자세한 정보는 [기본 모델 버전](/docs/services/speech-to-text?topic=speech-to-text-input#version)을 참조하십시오.

<table>
  <caption>표 4. base_model_version 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

서비스에 전달하는 오디오 데이터의 형식을 지정하는 선택적 오디오 형식(MIME 유형)입니다. 서비스가 대부분의 오디오 형식을 자동으로 발견할 수 있으므로 이 매개변수는 대부분의 형식에 대해 선택사항입니다. 이 필드는 `audio/alaw`, `audio/basic`, `audio/l16` 및 `audio/mulaw` 형식에 필요합니다. 자세한 정보는 [오디오 형식](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)을 참조하십시오.

<table>
  <caption>표 5. Content-Type 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 <code>content-type</code> 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 요청 헤더
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 요청 헤더
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

서비스가 기본 어휘의 단어와 비교하여 사용자 정의 언어 모델의 단어에 지정하는 상대적인 가중치를 표시하는 0.0 - 0.1 사이의 선택적 Double입니다. 사용자 정의 언어 모델이 훈련될 때 다른 가중치가 지정되지 않은 경우 기본값은 0.3입니다. 자세한 정보는 [사용자 정의 모델](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input)을 참조하십시오.

<table>
  <caption>표 6. customization_weight 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어, 영국 영어, 브라질 포르투갈어, 프랑스어, 독일어, 일본어, 한국어 및 스페인어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

음성 인식에 사용될 문법을 식별하는 선택적 문자열입니다. 서비스는 문법으로 정의된 문자열만 인식합니다. 문법의 이름과 문법이 정의된 사용자 정의 언어 모델의 사용자 정의 ID를 둘 다 지정해야 합니다. 자세한 정보는 [문법](/docs/services/speech-to-text?topic=speech-to-text-input#grammars-input)을 참조하십시오.

<table>
  <caption>표 7. grammar_name 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어, 영국 영어, 브라질 포르투갈어, 프랑스어, 독일어, 일본어, 한국어 및 스페인어에 대한 베타
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

서비스의 비활성 제한시간에 대한 초 수를 지정하는 선택적 정수입니다. 비활성은 서비스가 스트리밍 오디오에서 음성을 발견하지 않음을 의미합니다. 기본값은 30초입니다. 무한대를 표시하려면 `-1`을 사용하십시오. 자세한 정보는 [비활성 제한시간](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity)을 참조하십시오.

<table>
  <caption>표 8. inactivity_timeout 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

최종 음성 내용 전에 변경될 가능성이 있는 중간 가설을 리턴하도록 서비스에 지시하는 선택적 부울입니다. 기본적으로(`false`) 중간 결과는 리턴되지 않습니다. 자세한 정보는 [중간 결과](/docs/services/speech-to-text?topic=speech-to-text-output#interim)를 참조하십시오.

<table>
  <caption>표 9. interim_results 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

서비스가 입력 오디오에서 발견하는 선택적 키워드 문자열 배열입니다. 기본적으로 키워드 발견은 수행되지 않습니다. 자세한 정보는 [키워드 발견](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)을 참조하십시오.

<table>
  <caption>표 10. keywords 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

긍정 키워드 일치에 대한 최소 임계값을 표시하는 0.0 - 1.0 사이의 선택적 Double입니다. 기본적으로 키워드 발견은 수행되지 않습니다. 자세한 정보는 [키워드 발견](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)을 참조하십시오.

<table>
  <caption>표 11. keywords_threshold 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

도메인의 용어가 포함된 사용자 정의 언어 모델의 선택적 사용자 정의 ID입니다. 기본적으로 사용자 정의 모델은 사용되지 않습니다. 자세한 정보는 [사용자 정의 모델](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input)을 참조하십시오.

<table>
  <caption>표 12. language_customization_id 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어, 영국 영어, 브라질 포르투갈어, 프랑스어, 독일어, 일본어, 한국어 및 스페인어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

서비스가 리턴하는 최대 대체 가설 수를 지정하는 선택적 정수입니다. 기본적으로 이 서비스는 하나의 최종 가설을 리턴합니다. 자세한 정보는 [최대 대체 수](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives)를 참조하십시오.

<table>
  <caption>표 13. max_alternatives 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## model
{: #summary-model}

오디오에서 사용된 언어와 오디오가 샘플링된 속도(광대역 또는 협대역)를 지정하는 선택적 모델입니다. 기본적으로 `en-US_BroadbandModel`이 사용됩니다. 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text?topic=speech-to-text-models)을 참조하십시오.

<table>
  <caption>표 14. model 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## processing_metrics
{: #summary-processing-metrics}

서비스가 입력 오디오의 처리에 대한 메트릭을 리턴하는지 여부를 표시하는 선택적 부울입니다. 기본적으로(`false`) 서비스는 처리 메트릭을 리턴하지 않습니다. 자세한 정보는 [처리 메트릭](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)을 참조하십시오.

<table>
  <caption>표 15. processing_metrics 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## processing_metrics_interval
{: #summary-processing-metrics-interval}

서비스가 처리 메트릭을 리턴할 간격을 표시하는 0.1 이상의 선택적 부동 소수점입니다. `processing_metrics` 매개변수가 `true`인 경우 서비스는 기본적으로 1.0초마다 처리 메트릭을 리턴합니다. 자세한 정보는 [처리 메트릭](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)을 참조하십시오.

<table>
  <caption>표 16. processing_metrics_interval 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

서비스가 음성 내용에서 욕설을 검열하는지 여부를 표시하는 선택적 부울입니다. 기본적으로(`true`) 음성 내용에서 욕설이 필터링됩니다. 자세한 정보는 [욕설 필터링](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter)을 참조하십시오.

<table>
  <caption>표 17. profanity_filter 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

서비스가 음성 내용에서 세 개 이상의 연속 숫자가 포함된 숫자 데이터를 교정하는지 여부를 표시하는 선택적 부울입니다. `redaction` 매개변수를 `true`로 설정하면 서비스가 자동으로 `smart_formatting` 매개변수를 `true`로 설정합니다. 기본적으로(`false`) 숫자 데이터는 교정되지 않습니다. 자세한 정보는 [숫자 교정](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)을 참조하십시오.

<table>
  <caption>표 18. redaction 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어, 일본어 및 한국어에 대한 베타
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

서비스가 최종 음성 내용에서 날짜, 시간, 숫자, 통화 및 유사한 값을 더 일반적인 표시로 변환하는지 여부를 표시하는 부울 값입니다. 미국 영어의 경우 이 기능은 특정 키워드 구문을 문장 부호로 변환합니다. 기본적으로(`false`) 스마트 형식화는 수행되지 않습니다. 자세한 정보는 [스마트 형식화](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)를 참조하십시오.

<table>
  <caption>표 19. smart_formatting 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어, 일본어 및 스페인에 대한 베타
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

서비스가 여러 참여자 간의 대화에서 어떤 개인이 어떤 단어를 말했는지를 식별하는지 여부를 표시하는 선택적 부울입니다. `speaker_labels` 매개변수를 `true`로 설정하면 서비스가 자동으로 `timestamps` 매개변수를 `true`로 설정합니다. 기본적으로(`false`) 화자 레이블은 리턴되지 않습니다. 자세한 정보는 [화자 레이블](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)을 참조하십시오.

<table>
  <caption>표 20. speaker_labels 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
      미국 영어, 일본어 및 스페인어(광대역 및 협대역 모델) 및 영국 영어(협대역 모델 전용)에 대한 베타
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

서비스가 음성 내용의 단어에 대한 시간소인을 생성하는지 여부를 표시하는 선택적 부울입니다. 기본적으로(`false`) 시간소인은 리턴되지 않습니다. 자세한 정보는 [단어 시간소인](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)을 참조하십시오.

<table>
  <caption>표 21. timestamps 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

오디오가 서비스로 스트리밍되도록 하는 선택적 `chunked` 값입니다. 기본적으로 오디오는 원샷(one-shot) 전달로 한 번에 모두 전송됩니다. 자세한 정보는 [오디오 전송](/docs/services/speech-to-text?topic=speech-to-text-input#transmission)을 참조하십시오.

<table>
  <caption>표 22. Transfer-Encoding 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      해당되지 않음, 항상 스트리밍됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 요청 헤더
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 요청 헤더
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

Cloud Foundry 서비스 인증 정보를 사용하는 경우 WebSocket 인터페이스와의 인증된 연결을 설정하는 데 사용하는 선택적 {{site.data.keyword.watson}} 인증 토큰입니다. 자세한 정보는 [연결 열기](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen)를 참조하십시오.

<table>
  <caption>표 23. watson-token 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
지원되지 않음
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

서비스가 입력 오디오의 단어에 대해 음향적으로 유사한 대안을 보고하는 임계값을 지정하는 0.0 - 1.0 사이의 선택적 Double입니다. 기본적으로 단어 대체는 리턴되지 않습니다. 자세한 정보는 [단어 대체](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)를 참조하십시오.

<table>
  <caption>표 24. word_alternatives_threshold 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

서비스가 음성 내용의 단어에 대한 신뢰도 측정값을 제공하는지 여부를 표시하는 선택적 부울입니다. 기본적으로(`false`) 단어 신뢰도 측정값은 리턴되지 않습니다. 자세한 정보는 [단어 신뢰도](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence)를 참조하십시오.

<table>
  <caption>표 25. word_confidence 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 메시지의 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 메소드의 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 메소드의 조회 매개변수
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

모든 호출에 서비스 인증 정보를 임베드하지 않고 서비스에 대한 인증된 요청을 작성하는 선택적 인증 토큰입니다. 기본적으로 서비스 인증 정보가 각 요청과 함께 전달되어야 합니다. Watson 인증 토큰은 `{username}` 및 `{password}`를 인증에 사용하는 Cloud Foundry 서비스 인증 정보를 기반으로 합니다.

`X-Watson-Authorization-Token` 헤더는 IAM 토큰 또는 API 키를 허용하지 않습니다.
{: note}

<table>
  <caption>표 26. X-Watson-Authorization-Token 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      지원되지 않음, <code>watson-token</code> 조회 매개변수를 사용하십시오.
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      각 요청의 요청 헤더
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      각 요청의 요청 헤더
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

{{site.data.keyword.IBM_notm}}이 향후 사용자를 위한 서비스 개선을 위해 수행하는 기본 요청 로깅을 사용하지 않는지 여부를 표시하는 선택적 부울입니다. IBM에서 일반적인 서비스 개선을 위해 데이터에 액세스하지 못하게 하려면 이 매개변수를 <code>true</code>로 지정하십시오. 자세한 정보는 [요청 로깅](/docs/services/speech-to-text?topic=speech-to-text-input#logging)을 참조하십시오.

<table>
  <caption>표 27. X-Watson-Learning-Opt-Out 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 <code>x-watson-learning-opt-out</code> 조회 매개변수
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      각 요청의 요청 헤더
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      각 요청의 요청 헤더
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

인식 요청을 위해 전달된 데이터와 고객 ID를 연관시키는 선택적 문자열입니다. 이 매개변수는 `customer_id={id}` 인수를 받아들입니다. 기본적으로 고객 ID는 데이터와 연관되지 않습니다. 자세한 정보는 [정보 보안](/docs/services/speech-to-text?topic=speech-to-text-information-security)을 참조하십시오.

<table>
  <caption>표 28. X-Watson-Metadata 매개변수</caption>
  <tr>
    <th>가용성 및 사용법</th>
    <th style="vertical-align:bottom">설명</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **가용성**
    </td>
    <td style="text-align:left">
     모든 언어에 대해 GA(Generally Available)됨
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 연결 요청의 <code>x-watson-metadata</code> 조회 매개변수(인수를 URL로 인코딩해야 합니다. 예: `customer_id%3dmy_customer_ID`)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **동기 HTTP**
    </td>
    <td style="text-align:left">
      POST <code>/v1/recognize</code> 요청의 요청 헤더
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **비동기 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/register_callback</code> 및 <code>POST /v1/recognitions</code> 요청의 요청 헤더
    </td>
  </tr>
</table>
