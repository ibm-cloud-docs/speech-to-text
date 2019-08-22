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

# 服務背後的科學
{: #science}

如 [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external} 中所述，{{site.data.keyword.IBM}} 自 20 世紀 60 年代初以來一直處於語音辨識研究的前沿。例如，[Bahl、Jelinek 和 Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) 說明基本上在所有現代語音辨識系統中，採用的基本語音辨識數學方法。[Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) 說明聽寫用的第一個即時大型詞彙語音辨識系統的建立。本論文也說明至今仍是未解研究主題的問題。
{: shortdesc}

{{site.data.keyword.IBM_notm}} 使用 {{site.data.keyword.speechtotextfull}} 服務延續這個豐富的研究和開發傳統。{{site.data.keyword.IBM_notm}} 已示範針對 Conversational Telephone Speech (CTS)（[Saon 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)）和播送新聞 (BN) 轉錄（[Thomas 及其他人，2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)），其公用基準資料集的語音辨識準確性創下業界記錄。{{site.data.keyword.IBM_notm}} 除了示範聲學建模的有效性外，還利用類神經網路進行語言建模（[Kurata 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) 和 [Kurata 及其他人，2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)）。

下列公告概述 {{site.data.keyword.IBM_notm}} 近期在語音辨識方面取得的成就：

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

這些成就有助於進一步推動 {{site.data.keyword.IBM_notm}} 的語音服務進步。最適合雲端 {{site.data.keyword.speechtotextshort}} 服務的近期構想包括：

-   *對於語言建模，*{{site.data.keyword.IBM_notm}} 利用類神經網路型語言模型來產生訓練文字（[Suzuki 及其他人，2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)）。
-   *對於聲學建模，*{{site.data.keyword.IBM_notm}} 使用相當精簡的模型來適應雲端的資源限制。為了訓練這種精簡模型，{{site.data.keyword.IBM_notm}} 使用「教師-學生訓練/知識蒸餾」。首先，對長短期記憶 (LSTM)、VGG 和殘差網路 (ResNet) 等強大的大型類神經網路進行訓練。然後將這些網路的輸出用作教師信號，以訓練精簡模型用於實際部署（[Fukuda 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)）。

為了進一步推送封套，{{site.data.keyword.IBM_notm}} 還專注於端對端建模。例如，IBM 已為直接聲學到字組模型建立強大的建模管線（[Audhkhasi 及其他人，2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) 和 [Audhkhasi 及其他人，2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)），目前仍在對該管線做進一步改進（[Saon 及其他人，2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)）。此外，IBM 還投入精力建立精簡的端對端模型，以用於未來部署到雲端（[Kurata 和 Audhkhasi，2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)）。

如需服務背後的科學研究相關資訊，請參閱[研究參照](/docs/services/speech-to-text?topic=speech-to-text-references)中列出的文件。
