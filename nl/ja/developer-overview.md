---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-13"

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

# 開発者向けの概要
{: #developerOverview}

{{site.data.keyword.speechtotextfull}} サービスの機能にアクセスするには、WebSocket インターフェースを使用するとともに、同期または非同期の HTTP Representational State Transfer (REST) インターフェースを使用します。 このサービスの言語モデルをカスタマイズして、対象の分野と環境に適合させることもできます。 多くのプログラミング言語でアプリケーション開発を簡素化するための SDK が提供されています。
{: shortdesc}

## サービスでのプログラミング
{: #programming}

[認識要求の実行](/docs/services/speech-to-text?topic=speech-to-text-basic-request)では、サービスの各プログラミング・インターフェースを使用して基本的な書き起こしを要求する方法を説明しています。

-   [WebSocket インターフェース](/docs/services/speech-to-text?topic=speech-to-text-websockets)は、全二重接続を使用した効率的で低遅延の高スループット実装を提供します。
-   [同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-http)は、ブロッキング要求を使用して音声を書き起こすための基本的なインターフェースを提供します。
-   [非同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-async)で提供される非ブロッキング・インターフェースを使用して、通知を受信するためのコールバック URL を登録したり、サービスにポーリングしてジョブのステータスと結果を確認するためのコールバック URL を登録したりできます。

これらのインターフェースは同じ音声認識機能を提供しますが、使用するインターフェースに応じて、JSON オブジェクトのパラメーター、クエリー・パラメーター、または要求ヘッダーと同じパラメーターを指定する場合があります。 また、WebSocket インターフェースと同期 HTTP インターフェースは、単一の要求で最大 100 MB の音声データを受け付けます。 非同期 HTTP インターフェースは最大 1 GB の音声データを受け付けます。

-   すべての使用可能な音声認識パラメーターについて詳しくは、[パラメーターの要約](/docs/services/speech-to-text?topic=speech-to-text-summary)を参照してください。
-   すべてのメソッドおよびそれらのパラメーターと使用例について詳しくは、[API リファレンス](https://{DomainName}/apidocs/speech-to-text){: external}を参照してください。

## WebSocket インターフェースの利点
{: #advantages}

WebSocket インターフェースには、HTTP インターフェースと比較して、以下に示すような多数の利点があります。

-   WebSocket インターフェースは、単一ソケットの全二重通信チャネルを提供します。 このインターフェースにより、クライアントは、非同期的に単一の接続を介して、要求および音声をサービスに送信し、結果を受信できます。
-   はるかにシンプルで強力なプログラミング・エクスペリエンスが得られます。 このサービスはイベント・ドリブンの応答をクライアントのメッセージに送信するため、クライアントがサーバーにポーリングする必要がなくなります。
-   単一の認証済み接続を確立して永続的に使用できます。 HTTP インターフェースでは、サービスに対する個別の呼び出しを認証する必要があります。
-   待ち時間が削減されます。 サービスが認識結果をクライアントに直接送信するため、認識結果の到着が早くなります。 HTTP インターフェースでは、同じ結果を得るために 4 つの異なる要求と接続が必要です。
-   ネットワークの使用率が削減されます。 WebSocket プロトコルは軽量です。 ライブ音声認識を実行するために必要な接続は 1 つのみです。
-   ブラウザー (HTML5 WebSocket クライアント) からサービスに直接、音声をストリーミングできます。

## サービスのカスタマイズ
{: #customizing}

[カスタマイズ・インターフェース](/docs/services/speech-to-text?topic=speech-to-text-customization)を使用することにより、カスタム・モデルを作成してサービスの音声認識機能を向上させることができます。

-   [カスタム言語モデル](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)を使用することにより、基本モデル用の分野固有の単語を定義できます。 カスタム言語モデルは、医療や法律などの分野固有の用語を追加してサービスの基本語彙を拡張します。
-   [カスタム音響モデル](/docs/services/speech-to-text?topic=speech-to-text-acoustic)を使用することにより、対象の環境と話者の音響特性に基本モデルを適合させることができます。 カスタム音響モデルは、特定の音響特性を対象にしたサービスの音声認識機能を向上させます。
-   [文法](/docs/services/speech-to-text?topic=speech-to-text-grammars)を使用することにより、文法のルールで定義された句のみをサービスで認識可能になるように制限できます。 有効なストリングの検索スペースを制限することで、サービスは通常より高速かつ正確に結果を返すことができます。 文法はカスタム言語モデルでサポートされています。

サービスの任意のインターフェースで、カスタム言語モデル、カスタム音響モデル、または両方のモデルを音声認識用に使用できます。

言語モデルまたは音響モデルのカスタマイズを使用するには、標準料金プランが必要です。ライト・プランのユーザーは、カスタマイズ・インターフェースを使用できません。詳しくは、{{site.data.keyword.speechtotextshort}} サービスの[価格設定のページ](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}を参照してください。
{: note}

## メトリックの取得
{: #overview-metrics}

サービスには、音声認識要求で使用できる以下の 2 つのタイプのオプション・メトリックが用意されています。

-   [処理メトリック](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)は、このサービスによる入力音声分析に関する詳細なタイミング情報を提供します。サービスは、指定された間隔で、中間結果や最終結果などの書き起こしイベントと一緒にメトリックを返します。メトリックを使用して、音声の書き起こしにおけるサービスの進行状況を測定できます。
-   [音声メトリック](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)は、入力音声の信号特性に関する詳細情報を提供します。音声処理終了時に入力音声全体の集約メトリックが結果で示されます。メトリックを使用して、音声の特性と品質を判別できます。

処理メトリックは、WebSocket インターフェースおよび非同期 HTTP インターフェースを使用して要求できます。音声メトリックは、サービスの任意のインターフェースを使用して要求できます。デフォルトでは、サービスからメトリックは返されません。

## CORS サポート
{: #cors}

サービスは Cross-Origin Resource Sharing (CORS) をサポートしています。 CORS を使用することで、Web ページは外部ドメインに対してリソースを直接要求できるようになります。 通常はこのような要求は同一生成元セキュリティー・ポリシーによって阻止されますが、CORS によって同一生成元セキュリティー・ポリシーが回避されます。 サービスは CORS をサポートしているため、Web ページは、そのページをホストしている Web サーバーを通じて要求を渡すことなく、サービスと直接通信できます。

例えば、{{site.data.keyword.cloud}} 内のサーバーからロードされる Web ページは、{{site.data.keyword.cloud_notm}} サーバーを迂回してカスタマイズ API を直接呼び出すことができます。 詳しくは、[enable-cors.org](https://enable-cors.org/){: external}を参照してください。

## Software Development Kit の使用
{: #sdks}

音声アプリケーションの開発を簡素化するための {{site.data.keyword.speechtotextshort}} サービス用の SDK が提供されています。 {{site.data.keyword.ibmwatson}} の SDK は、多くの一般的なプログラミング言語とプラットフォームに対応しています。

-   全 SDK のリストと、GitHub 上のこれらの SDK へのリンクについては、[SDK の使用](/docs/services/watson?topic=watson-using-sdks)を参照してください。
-   {{site.data.keyword.speechtotextshort}} サービスで使用できる Node、Java&trade;、Python、Ruby、Swift、および Go SDK のすべてのメソッドについて詳しくは、[API リファレンス](https://{DomainName}/apidocs/speech-to-text){: external}を参照してください。

## アプリケーション開発に関する詳細
{: #learn}

各種の {{site.data.keyword.watson}} サービスおよび {{site.data.keyword.cloud_notm}} の操作について詳しくは、以下を参照してください。

-   各種の {{site.data.keyword.watson}} サービスおよび {{site.data.keyword.cloud_notm}} の操作の概要については、[{{site.data.keyword.watson}} と {{site.data.keyword.cloud_notm}} の概要](/docs/services/watson?topic=watson-about)を参照してください。
-   すべての新規サービス・インスタンスでは、{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) が認証用に使用されます。 古いサービス・インスタンスでは、それらのインスタンスの既存の Cloud Foundry サービス資格情報に含まれる `{username}` と `{password}` が引き続き認証用に使用される場合があります。 サービスによる認証について詳しくは、リリース・ノート内の [2018 年 10 月 30 日のサービス更新](/docs/services/speech-to-text?topic=speech-to-text-release-notes#October2018b)を参照してください。
-   すべての {{site.data.keyword.watson}} サービスについて実行されるデフォルトの要求ロギングを制御する方法については、[{{site.data.keyword.watson}} サービスの要求ロギングの制御](/docs/services/watson?topic=watson-gs-logging-overview)を参照してください。
