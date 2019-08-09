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

# サービスの背景にある科学
{: #science}

[Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external}で説明されていれるように、{{site.data.keyword.IBM}} は 1960 年代前半から音声認識研究の最前線に立ってきました。例えば、[Bahl、Jelinek、および Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) は、現代のほぼすべての音声認識システムで採用されている音声認識への基本的な数学的アプローチを説明しています。また [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) は、初の口述筆記用のリアルタイム大規模語彙音声認識システムの作成について説明しています。この論文では、今日でも未解決の研究トピックのままである問題についても説明しています。
{: shortdesc}

{{site.data.keyword.IBM_notm}} は引き続き {{site.data.keyword.speechtotextfull}} サービスで、長きにわたる豊富な研究開発を進めています。{{site.data.keyword.IBM_notm}} は電話会話音声 (CTS) ([Saon 他、2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) およびニュース放送 (BN) 書き起こし ([Thomas 他、2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)) の公開ベンチマーク・データ・セットで、業界の記録となる音声認識精度を実証しました。{{site.data.keyword.IBM_notm}} は、音響モデリングの効果の実証の他に、言語モデリングにニューラル・ネットワークを活用しました ([Kurata 他、2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) および [Kurata 他、2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a))。

以下の発表資料に、{{site.data.keyword.IBM_notm}} の最近の音声認識に関する実績をまとめています。

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

これらの実績は、{{site.data.keyword.IBM_notm}} の音声サービスのさらなる発展に貢献します。クラウド・ベースの {{site.data.keyword.speechtotextshort}} サービスに最適な最近の研究内容には次のものがあります。

-   *言語モデリングでは、* {{site.data.keyword.IBM_notm}} はトレーニング・テキストの生成にニューラル・ネットワーク・ベースの言語モデルを活用しています ([Suzuki 他、2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019))。
-   *音響モデリングでは、*{{site.data.keyword.IBM_notm}} はクラウドのリソース制限に対応するため、かなりコンパクトなモデルを採用しています。このコンパクトなモデルをトレーニングするため、{{site.data.keyword.IBM_notm}} は「教師 - 生徒トレーニング / 知識の蒸留」を使用しています。最初に、大規模で強力なニューラル・ネットワーク (長期/短期記憶 (LSTM)、VGG、Residual Network (ResNet) など) をトレーニングしました。これらのネットワークの出力を教師信号として使用し、実際のデプロイメントに向けてコンパクト・モデルをトレーニングしました ([Fukuda 他、2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017))。

さらに高いレベルを求め、{{site.data.keyword.IBM_notm}} はエンドツーエンド・モデリングにも取り組んでいます。例えば、ダイレクト Acoustic-to-Word モデルの強力なモデリング・パイプラインを確立し ([Audhkhasi 他、2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017)、および [Audhkhasi 他、2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018))、現在さらに改良を進めています([Saon 他、2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019))。また、将来クラウドへデプロイするコンパクトなエンドツーエンド・モデルの作成に取り組んでいます ([Kurata および Audhkhasi、2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019))。

このサービスを支えている科学的研究について詳しくは、[研究資料](/docs/services/speech-to-text?topic=speech-to-text-references)にリストしている資料を参照してください。
