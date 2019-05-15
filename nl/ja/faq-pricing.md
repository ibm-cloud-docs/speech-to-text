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

# 価格設定に関する FAQ
{: #faq-pricing}

1.  <span style="color:#003F69">{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} 標準サービスの使用料はいくらですか。</span>

    {{site.data.keyword.speechtotextshort}} サービスの料金は分あたり $0.02 (米ドル) です。この料金は、広帯域モデルと狭帯域モデルの両方の使用に適用されます。詳しくは、{{site.data.keyword.speechtotextshort}} [サービスのランディング・ページ ![Externallink icon](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window} を参照してください。

1.  <span style="color:#003F69">「分あたりの料金」とはどういう意味ですか。課金の対象となるのは、ユーザーからサービスに送信される音声の分数、またはサービスによってその音声が処理されるのにかかる分数のどちらですか。</span>

    料金の基準となるのは、サービスに送信される音声の長さであり、サービスによってその音声が処理されるのにかかる時間ではありません。

1.  <span style="color:#003F69">当 API の呼び出しごとに、最も近い分数に切り上げられますか。例えば、それぞれが 30 秒の長さの 2 つの音声ファイルを送信した場合は、課金の対象となるのは 2 分間または 1 分間のどちらですか。</span>

    {{site.data.keyword.IBM_notm}} では、サービスで受信される API 呼び出しごとに、音声の長さを切り上げることはありません。代わりに、{{site.data.keyword.IBM_notm}} では各月の総使用時間を集計して、月末に端数を丸めて最も近い分数を算出します。この例では、それぞれが 30 秒の長さの 2 つの音声ファイルが送信された場合は、{{site.data.keyword.IBM_notm}} では音声の合計長を 1 分間として算出して、$0.02 (米ドル) を課金します。

1.  <span id="graduated" style="color:#003F69">従量制の段階的階層型価格設定とはどのような仕組みですか。</span>

    階層型価格設定モデルの目的は、サービスを継続使用する大口ユーザーに追加の割引を適用することです。月あたりの合計音声量が既定の基準量に達すると、それ以降の分数については分あたりの料金が割引されます。詳しくは、サービスの[価格設定のページ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/catalog/services/speech-to-text){: new_window} を参照してください。

1.  <span style="color:#003F69">階層型価格設定の場合は、サービスを使用して 1 カ月に例えば 275,000 分間の音声を書き起こした場合、課金総額はいくらになりますか。</span>

    -   最初の 250,000 分間の音声については、分あたり $0.02 (米ドル) が課金されるため、250,000 × $0.02 = $5000.00 (米ドル) となります。
    -   残りの 25,000 分間の音声については、割引が適用されて分あたり $0.015 (米ドル) が課金されるため、25,000 × $0.015 = $375.00 (米ドル) となります。
    -   この例では、この月の課金総額は $5375.00 (米ドル) となります。

1.  <span style="color:#003F69">サービスのカスタマイズ・インターフェースの使用料はいくらですか。カスタム・モデルの作成と保存に対して課金されますか。</span>

    *言語モデルのカスタマイズ* の場合は、{{site.data.keyword.IBM_notm}} では、カスタム言語モデルの作成やホスティングに対して課金せず、認識要求でのカスタム言語モデルの使用に対してのみ課金します。カスタム言語モデルを書き起こし用に使用する場合は、分あたり $0.03 (米ドル) という追加料金が発生します。この料金は、分あたり $0.02 (米ドル) という標準使用料に加えて課金されます。この料金制度は、カスタマイズ・インターフェースでサポートされているすべての言語に適用されます。したがって、カスタム言語モデルを音声認識用に使用する場合の課金総額は分あたり $0.05 (米ドル) となります。

    *音響モデルのカスタマイズ* の場合は、すべてのサポート対象言語についてベータ版のインターフェースが提供されますが、{{site.data.keyword.IBM_notm}} では、カスタム音響モデルの作成とホスティングに対しても、カスタム音響モデルを使用した音声認識に対しても課金しません。認識要求でのカスタム音響モデルの無料使用は、将来的に変更される可能性があります。
