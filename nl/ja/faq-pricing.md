---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

subcollection: speech-to-text

---

{:faq: .faq}
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

# 価格設定に関する FAQ
{: #faq-pricing}

## {{site.data.keyword.speechtotextshort}} ライト・プランの使用料はいくらですか。
{: #faq-pricing-zero}
{: faq}

ライト・プランを選択した場合は、1 カ月あたり 500 分間の無料の音声認識を開始できます。ライト・プランでは、サービスのカスタマイズ・インターフェースにアクセスできません。詳しくは、{{site.data.keyword.speechtotextshort}} サービスの[価格設定のページ](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}を参照してください。

## {{site.data.keyword.speechtotextshort}} 標準プランの使用料はいくらですか。
{: #faq-pricing-one}
{: faq}

標準料金プランは、認識する音声 1 分当たり $0.02 (米ドル) に料金設定されています。この料金は、広帯域モデルと狭帯域モデルの両方の使用に適用されます。 このプランでは階層型の料金モデルを使用しており、追加料金でカスタマイズ・インターフェースを使用できるようになります。詳しくは、{{site.data.keyword.speechtotextshort}} サービスの[価格設定のページ](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}を参照してください。

## 「分あたりの料金」とはどういう意味ですか。
{: #faq-pricing-two}
{: faq}

お客様がサービスに送信した音声の長さ (分数) に基づいて料金が決まります。サービスが音声を処理するのに要した時間の長さによって料金が決まるわけではありません。

## 当 API の呼び出しごとに、最も近い分数に切り上げられますか。
{: #faq-pricing-three}
{: faq}

{{site.data.keyword.IBM_notm}} では、サービスで受信される API 呼び出しごとに、音声の長さを切り上げることはありません。 代わりに、{{site.data.keyword.IBM_notm}} では各月の総使用時間を集計して、月末に端数を丸めて最も近い分数を算出します。 例えば、それぞれが 30 秒の長さの 2 つの音声ファイルを送信した場合、{{site.data.keyword.IBM_notm}} は音声の合計長を 1 分間として算出して、$0.02 (米ドル) を課金します。

## 従量制の段階的階層型価格設定とはどのような仕組みですか。
{: #faq-pricing-four}
{: faq}

階層型価格設定モデルの目的は、サービスを継続使用する大口ユーザーに追加の割引を適用することです。 月あたりの合計音声量が既定の基準量に達すると、それ以降の分数については分あたりの料金が割引されます。 詳しくは、サービスの[価格設定のページ](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}を参照してください。

## 階層型価格設定の場合は、サービスを使用して 1 カ月に例えば 275,000 分間の音声を書き起こした場合、課金総額はいくらになりますか。
{: #faq-pricing-five}
{: faq}

最初の 250,000 分間の音声については、分あたり $0.02 (米ドル) が課金されるため、250,000 × $0.02 = $5000.00 (米ドル) となります。残りの 25,000 分間の音声については、割引料金が適用されて分あたり $0.015 (米ドル) が課金されるため、25,000 × $0.015 = $375.00 (米ドル) となります。この場合は、この月の課金総額は $5375.00 (米ドル) となります。

## サービスのカスタマイズ・インターフェースを使用するためには、どの料金プランが必要ですか。
{: #faq-pricing-six}
{: faq}

言語モデルまたは音響モデルのカスタマイズを使用するには、標準料金プランが必要です。ライト・プランのユーザーは、カスタマイズ・インターフェースを使用できません。詳しくは、{{site.data.keyword.speechtotextshort}} サービスの[価格設定のページ](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}を参照してください。

## サービスのカスタマイズ・インターフェースの使用料はいくらですか。
{: #faq-pricing-seven}
{: faq}

*言語モデルのカスタマイズ* の場合は、{{site.data.keyword.IBM_notm}} では、カスタム言語モデルの作成やホスティングに対して課金せず、認識要求でのカスタム言語モデルの使用に対してのみ課金します。 カスタム言語モデルを書き起こし用に使用する場合は、分あたり $0.03 (米ドル) という追加料金が発生します。 この料金は、分あたり $0.02 (米ドル) という標準使用料に加えて課金されます。 この料金制度は、カスタマイズ・インターフェースでサポートされているすべての言語に適用されます。 したがって、カスタム言語モデルを音声認識用に使用する場合の課金総額は分あたり $0.05 (米ドル) となります。

*音響モデルのカスタマイズ* の場合は、すべてのサポート対象言語についてベータ版のインターフェースが提供されますが、{{site.data.keyword.IBM_notm}} では、カスタム音響モデルの作成とホスティングに対しても、カスタム音響モデルを使用した音声認識に対しても課金しません。 認識要求でのカスタム音響モデルの無料使用は、将来的に変更される可能性があります。
