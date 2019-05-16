---

copyright:
  years: 2015, 2019
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

# 정보 보안
{: #information-security}

{{site.data.keyword.IBM}}은 고객과 파트너에게 혁신적인 데이터 개인정보 보호, 보안 및 통제 솔루션을 제공하기 위해 노력하고 있습니다.
{: shortdesc}

고객은 유럽 연합 일반 개인정보 보호법률(General Data Protection Regulation)을 포함한
다양한 법령과 규정을 준수해야 할 책임이 있습니다. 고객은 고객의 비즈니스에 영향을 줄 수 있는 관련 법령 및 규정에 대한 확인과 해석, 그러한 법령 및 규정의 준수를 위해 필요한 고객의 모든 조치와 관련하여 적절한 법률 자문을 받아야 할 단독 책임이 있습니다.
{: important}

여기에서 설명하는 제품, 서비스 및 기타 기능은 일부 고객의 상황에는 해당되지 않을 수 있으며 그 가용성이 제한될 수 있습니다. {{site.data.keyword.IBM_notm}}은 법률, 회계 또는 감사 관련 자문을 제공하지 않으며, IBM의 서비스나 제품 사용이 고객의 관련 법령이나 규정 준수를 보장한다는 진술이나 보증을 제공하지 않습니다.

{{site.data.keyword.cloud}} {{site.data.keyword.watson}} 리소스에 대한 GDPR 지원을 요청해야 하는 경우

-   유럽 연합(EU)의 경우 [유럽 연합에서 작성된 {{site.data.keyword.cloud_notm}} Watson 리소스에 대한 지원 요청](/docs/services/watson/getting-started-gdpr-sar.html#request-EU)을 참조하십시오.
-   EU 이외의 지역의 경우 [유럽 연합 외부의 리소스에 대한 지원 요청](/docs/services/watson/getting-started-gdpr-sar.html#request-non-EU)을 참조하십시오.

## 유럽 연합 일반 개인정보 보호법률(General Data Protection Regulation, GDPR)
{: #gdpr}

{{site.data.keyword.IBM_notm}}은 고객과 파트너가 GDPR 규제를 준수하는 데 도움을 줄 수 있도록 혁신적인 데이터 개인정보 보호, 보안 및 통제 솔루션을 제공하기 위해 노력하고 있습니다.

[여기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/gdpr)에서 규제 준수 과정을 지원하기 위한 {{site.data.keyword.IBM_notm}}의 자체 GDPR 준비 과정과 GDPR 기능 및 오퍼링에 대해 자세히 알아보십시오.{: new_window}.

## Speech to Text 서비스에서 데이터 레이블 지정 및 삭제
{: #gdpr-speech-to-text}

{{site.data.keyword.speechtotextfull}} 서비스를 사용하여 인식 요청, 사용자 정의 언어 모델 및 사용자 정의 음향 모델과 연관된 모든 데이터를 삭제할 수 있습니다. 데이터를 삭제하려면 다음을 수행해야 합니다.

1.  `X-Watson-Metadata` 헤더를 사용하여 요청을 통해 서비스에 전달된 데이터와 고객 ID를 연관시키십시오. [고객 ID 지정](#specify-customer-id)을 참조하십시오.
1.  `DELETE /v1/user_data` 메소드를 사용하여 지정된 고객 ID와 연관된 모든 데이터를 삭제하십시오. [데이터 삭제](#delete-pi)를 참조하십시오.

기본적으로 고객 ID는 데이터와 연관되지 않습니다.

시험 및 베타 기능은 프로덕션 환경에 사용하기 위한 것이 아니므로 데이터 레이블 지정 및 데이터 삭제 시 예상대로 작동하도록 보장되지 않습니다. 데이터 레이블 지정 및 삭제가 필요한 솔루션을 구현할 때 시험 및 베타 기능을 사용해서는 안됩니다.
{: note}

### 고객 ID 지정
{: #specify-customer-id}

고객 ID를 데이터와 연관시키려면 정보를 전달하는 요청과 함께 `X-Watson-Metadata` 헤더를 포함시키십시오. `customer_id={id}`라는 문자열을 헤더의 인수로 전달합니다. 다음 예제에서는 고객 ID `my_customer_ID`를 `POST /v1/recognize` 요청과 함께 전달된 데이터와 연관시킵니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

고객 ID에는 `;`(세미콜론) 및 `=`(등호)를 제외한 모든 문자가 포함될 수 있습니다. 랜덤 또는 일반 문자열을 고객 ID로 지정하십시오. 이메일 주소 또는 Twitter ID와 같은 개인 식별이 가능한 문자열을 지정하지 마십시오. 여러 요청에 서로 다른 고객 ID를 지정할 수 있습니다. 사용자가 지정하는 고객 ID는 인증 정보가 요청에 사용되는 서비스 인스턴스와 연관되며 해당 서비스 인스턴스에 대한 인증 정보만 ID와 연관된 데이터를 삭제할 수 있습니다.

다음 메소드와 함께 `X-Watson-Metadata` 헤더를 사용하십시오.

-   WebSocket 요청 시:
    -   `/v1/recognize`

    연결 열기 요청의 `x-watson-metadata` 조회 매개변수를 사용하여 고객 ID를 지정합니다. 조회 매개변수에 대한 인수를 URL로 인코딩해야 합니다(예: `customer_id%3dmy_customer_ID`). 고객 ID가 연결을 통해 전송된 인식 요청과 함께 전달되는 모든 데이터와 연관됩니다.
-   동기 HTTP 요청 시
    -   `POST /v1/recognize`

    고객 ID가 개별 요청과 함께 전송된 데이터와 연관됩니다.
-   비동기 HTTP 요청 시
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    고객 ID가 개별 인식 요청과 함께 전송된 데이터 또는 화이트리스트 콜백 URL과 연관됩니다.
-   말뭉치, 사용자 정의 단어 또는 문법을 사용자 정의 언어 모델에 추가하는 요청에서
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    고객 ID가 요청에서 추가되거나 업데이트된 말뭉치, 사용자 정의 단어 또는 문법과 연관됩니다.
-   오디오 리소스를 사용자 정의 음향 모델에 추가하는 요청에서
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    고객 ID가 요청에서 추가되거나 업데이트된 오디오 리소스와 연관됩니다.

### 데이터 삭제
{: #delete-pi}

고객 ID와 연관된 모든 데이터를 삭제하려면 `DELETE /v1/user_data` 메소드를 사용하십시오. `customer_id={id}`라는 문자열을 요청과 함께 조회 매개변수로 전달합니다. 다음 예제에서는 고객 ID `my_customer_ID`에 대한 모든 데이터를 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

`/v1/user_data` 메소드는 정보가 추가될 때 사용된 메소드와 관계없이 지정된 고객 ID와 연관된 모든 데이터를 삭제합니다. 고객 ID와 연관된 데이터가 없는 경우에는 이 메소드가 적용되지 않습니다. 고객 ID를 데이터와 연관시키는 데 사용된 것과 동일한 서비스 인스턴스에 대한 인증 정보로 요청을 발행해야 합니다.
