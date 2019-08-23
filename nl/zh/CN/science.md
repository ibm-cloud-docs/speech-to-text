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

# 服务背后的科学
{: #science}

如 [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external} 中所述，{{site.data.keyword.IBM}} 自 20 世纪 60 年代初以来一直处于语音识别研究的前沿。例如，[Bahl、Jelinek 和 Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) 描述了语音识别的基本数学方法，基本上所有现代语音识别系统中都采用此方法。[Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) 描述了创建第一个用于听写的实时大型词汇表语音识别系统的情况。此论文还描述了至今仍未解决的研究主题问题。
{: shortdesc}

{{site.data.keyword.IBM_notm}} 通过 {{site.data.keyword.speechtotextfull}} 服务延续了这一丰厚的研究和开发传统。{{site.data.keyword.IBM_notm}} 已证明针对电话对话语音 (CTS) （[Saon 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)）和广播新闻 (BN) 转录（[Thomas 及其他人，2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)），其公共基准数据集的语音识别准确性创下业界记录。{{site.data.keyword.IBM_notm}} 除了证明声学建模的有效性外，还利用神经网络进行语言建模（[Kurata 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) 和 [Kurata 及其他人，2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)）。

以下公告概述了 {{site.data.keyword.IBM_notm}} 近期在语音识别方面取得的成就：

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

这些成就有助于进一步推动 {{site.data.keyword.IBM_notm}} 的语音服务进步。最适合基于云的 {{site.data.keyword.speechtotextshort}} 服务的近期构想包括：

-   *对于语言建模，*{{site.data.keyword.IBM_notm}} 利用基于神经网络的语言模型来生成训练文本（[Suzuki 及其他人，2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)）。
-   *对于声学建模，*{{site.data.keyword.IBM_notm}} 使用相当精简的模型来适应云的资源限制。为了训练这种精简模型，{{site.data.keyword.IBM_notm}} 使用了“教师-学生训练/知识蒸馏法”。首先，对长短期记忆 (LSTM)、VGG 和残差网络 (ResNet) 等强大的大型神经网络进行训练。然后将这些网络的输出用作教师信号，以训练精简模型用于实际部署（[Fukuda 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)）。

为了进一步推进包络发展，{{site.data.keyword.IBM_notm}} 还专注于端到端建模。例如，IBM 已为直接声学到词模型建立了强大的建模管道（[Audhkhasi 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) 和 [Audhkhasi 及其他人，2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)），目前仍在对该管道做进一步改进（[Saon 及其他人，2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)）。此外，IBM 还投入精力创建精简的端到端模型，以用于未来部署到云上（[Kurata 和 Audhkhasi，2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)）。

有关服务背后的科学研究的更多相关信息，请参阅[研究参考资料](/docs/services/speech-to-text?topic=speech-to-text-references)中列出的文档。
