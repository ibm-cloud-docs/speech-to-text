---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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

# 서비스를 뒷받침하는 과학
{: #science}

[Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external}에 설명된 대로 {{site.data.keyword.IBM}}은 1960년대 초부터 음성 인식 연구를 개척해 왔습니다. 예를 들어, [Bahl, Jelinek 및 Mercer(1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983)는 기본적으로 모든 현대 음성 인식 시스템에서 사용되는 음성 인식에 대한 기본 수학적 접근법에 대해 설명합니다. 또한 [Jelinek(1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985)은 받아쓰기를 위한 최초의 실시간 대형 어휘 음성 인식 시스템의 작성에 대해 설명합니다. 이 논문은 오늘날 여전히 해결되지 않은 주제로 남아 있는 문제점에 대해서도 설명합니다.
{: shortdesc}

{{site.data.keyword.IBM_notm}}은 {{site.data.keyword.speechtotextfull}} 서비스를 사용하여 이 유서 깊은 연구와 개발을 계속하고 있습니다. {{site.data.keyword.IBM_notm}}은 CTS(Conversational Telephone Speech)([Saon 외, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) 및 BN(Broadcast News) 트랜잭션([Thomas 외, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019))을 위해 공용 벤치마크 데이터 세트에 대한 업계 최고의 음성 인식 정확성을 입증했습니다. {{site.data.keyword.IBM_notm}}은 음향 모델링의 효과를 입증하는 것 외에도 언어 모델링([Kurata 외, 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) 및 [Kurata 외, 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a))에 대한 신경망을 활용했습니다. 

다음에서는 {{site.data.keyword.IBM_notm}}의 최신 음성 인식 성과를 요약하여 발표합니다. 

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

이러한 성과는 {{site.data.keyword.IBM_notm}}의 음성 서비스를 발전시키는 데 기여합니다. 클라우드 기반 {{site.data.keyword.speechtotextshort}} 서비스에 최적인 최신 아이디어에는 다음이 포함됩니다. 

-   *언어 모델링의 경우 * {{site.data.keyword.IBM_notm}}은 신경망 기반 언어 모델을 활용하여 훈련 텍스트([Suzuki 외, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019))를 생성합니다.
-   *음향 모델링의 경우 * {{site.data.keyword.IBM_notm}}은 소형 모델을 사용하여 클라우드의 리소스 제한사항을 수용합니다. 이 소형 모델을 훈련시키기 위해 {{site.data.keyword.IBM_notm}}은 "교사-학생 훈련/지식 증류"를 사용합니다. LSTM(Long Short-Term Memory), VGG 및 ResNet(Residual Network)와 같은 강력한 대형 신경망이 먼저 훈련됩니다. 그런 다음 실제 배치([Fukuda 외, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017))를 위해 소형 모델을 훈련시키기 위해 이러한 네트워크의 출력이 교사 신호로 사용됩니다.

새로운 도전을 위해 {{site.data.keyword.IBM_notm}}은 엔드 투 엔드 모델링에도 집중하고 있습니다. 예를 들어, 현재 더욱 개선되고 있는([Saon 외, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)) 직접적인 음향 대 단어 모델([Audhkhasi 외, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) 및 [Audhkhasi 외, 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018))에 대한 강력한 모델링 파이프라인이 설정되었습니다. 또한 클라우드의 추가 배치를 위해 소형 엔드 투 엔드 모델을 작성하도록 최선을 다하고 있습니다([Kurata 및 Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)).

서비스를 뒷받침하는 과학 연구에 대한 자세한 정보는 [참고 문헌](/docs/services/speech-to-text?topic=speech-to-text-references)에 나열된 문서를 참조하십시오.
