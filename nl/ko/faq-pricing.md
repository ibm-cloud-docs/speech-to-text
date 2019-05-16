---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-10"

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

# 가격 FAQ
{: #faq-pricing}

1.  <span style="color:#003F69">{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} 표준 서비스를 사용하기 위한 가격은 얼마입니까?</span>

    {{site.data.keyword.speechtotextshort}} 서비스는 분당 $0.02(USD)로 가격이 책정됩니다. 이 가격은 광대역 및 협대역 모델 모두를 사용하는 데 적용됩니다. 자세한 정보는 {{site.data.keyword.speechtotextshort}} [서비스 랜딩 페이지 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}를 참조하십시오.

1.  <span style="color:#003F69">"분당 가격"은 무엇을 의미합니까? 서비스에 전송하는 오디오의 시간(분) 또는 서비스가 오디오를 처리하는 데 걸리는 시간(분)에 대한 비용을 청구합니까?</span>

    가격은 서비스가 오디오를 처리하는 데 걸리는 시간이 아니라 서비스에 전송된 오디오의 양을 기반으로 합니다.

1.  <span style="color:#003F69">API를 호출할 때마다 가장 근접한 분으로 반올림합니까? 예를 들어, 각각 30초 길이인 두 개의 오디오 파일을 전송하는 경우 2분 또는 1분에 대한 요금이 부과됩니까?</span>

    {{site.data.keyword.IBM_notm}}에서는 이 서비스가 수신하는 모든 API 호출에 대한 오디오의 길이를 반올림하지 않습니다. 대신 {{site.data.keyword.IBM_notm}}에서 한 달 동안의 모든 사용량을 집계하여 월말에 가장 근접한 분으로 반올림합니다. 이 예제에서 각각 30초 길이인 두 개의 오디오 파일을 전송하는 경우 {{site.data.keyword.IBM_notm}}에서 총 오디오의 지속 시간을 1분으로 합산하여 $0.02(USD)를 청구합니다.

1.  <span id="graduated" style="color:#003F69">볼륨 기반의 누진 계층 가격은 어떻게 책정됩니까?</span>

    계층 가격 모델은 사용량이 많은 사용자가 이 서비스를 계속 사용하는 경우 추가 할인을 제공하기 위한 것입니다. 월별 오디오 총계에 대한 특정 임계값이 충족되면 추가 오디오 시간(분)에 대한 분당 가격이 감소됩니다. 자세한 정보는 서비스에 대한 [가격 페이지 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/catalog/services/speech-to-text){: new_window}를 참조하십시오.

1.  <span style="color:#003F69">계층 가격의 경우, 예를 들어 이 서비스를 사용하여 한 달에 275,000분의 오디오를 변환했다면 총 비용이 얼마입니까?</span>

    -   처음 250,000분의 오디오에는 분당 $0.02(USD)로 청구됩니다(250,000 * $0.02 = $5000.00(USD)).
    -   나머지 250,000분의 오디오에는 할인 요금인 분당 $0.015(USD)로 청구됩니다(25,000 * $0.015 = $375.00(USD)).
    -   이 시나리오에서는 해당 월의 총 비용이 $5375.00(USD)입니다.

1.  <span style="color:#003F69">서비스의 사용자 정의 인터페이스를 사용하는 데 필요한 가격은 얼마입니까? 사용자 정의 모델을 작성하고 저장하는 비용이 청구됩니까?</span>

    *언어 모델 사용자 정의*의 경우 {{site.data.keyword.IBM_notm}}에서는 사용자 정의 언어 모델을 작성하거나 호스팅하기 위한 비용이 아니라 모델을 인식 요청에 사용하기 위한 비용만 청구합니다. 변환에 사용자 정의 언어 모델을 사용하면 분당 $0.03(USD)의 추가 요금이 발생합니다. 이 요금은 분당 $0.02(USD)의 표준 사용 요금에 추가되며 사용자 정의 인터페이스에서 지원되는 모든 언어에 적용됩니다. 따라서 사용자 정의 언어 모델을 음성 인식에 사용하기 위한 총 요금은 분당 $0.05(USD)입니다.

    지원되는 모든 언어에 대한 베타 인터페이스인 *음향 모델 사용자 정의*의 경우 {{site.data.keyword.IBM_notm}}에서는 사용자 정의 음향 모델로 음성을 작성, 호스팅 또는 인식하는 데 필요한 비용을 청구하지 않습니다. 인식 요청을 위한 사용자 정의 음향 모델의 무료 사용은 나중에 변경될 수 있습니다.
