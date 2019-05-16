---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# リリース・ノート
{: #release-notes}

以降のセクションでは、{{site.data.keyword.speechtotextfull}} サービスの各リリースおよび更新で導入された新機能と変更点について説明します。この情報には既知の制限事項が含まれます。 別途記載がない限り、すべての変更は前のリリースと互換性があり、すべての新規および既存のアプリケーションで自動的かつ透過的に使用可能になっています。
{: shortdesc}

## 既知の制限事項
{: #limitations}

現時点では、既知の制限事項はありません。

## 2019 年 4 月 3 日
{: #April2019}

カスタム音響モデルで最大 200 時間の音声が受け入れられるようになりました。 以前の最大限度は 100 時間の音声でした。

## 2019 年 3 月 21 日
{: #March2019d}

ユーザーが、{{site.data.keyword.cloud_notm}} アカウントに割り当てられている役割に関連付けられているサービス資格情報のみを表示できるようになりました。 例えば、`reader` 役割が割り当てられている場合、`writer` 以上のレベルのサービス資格情報は表示されなくなりました。

この変更は、既存のサービス資格情報を使用したユーザーまたはアプリケーションの API アクセスには影響を与えません。 この変更は {{site.data.keyword.cloud_notm}} 内での資格情報の表示にのみ影響を与えます。

サービス・キーおよびユーザー役割について詳しくは、[IAM サービスの API キー](/docs/services/watson?topic=watson-api-key-bp#api-key-bp)を参照してください。

## 2019 年 3 月 15 日
{: #March2019c}

サービスで A-law (`audio/alaw`) フォーマットの音声がサポートされるようになりました。 詳しくは、[audio/alaw フォーマット](/docs/services/speech-to-text/audio-formats.html#alaw)を参照してください。

## さらに前のリリース
{: #older}

-   [2019 年 3 月 11 日](#March2019b)
-   [2019 年 3 月 4 日](#March2019)
-   [2019 年 1 月 28 日](#January2019)
-   [2018 年 12 月 20 日](#December2018b)
-   [2018 年 12 月 13 日](#December2018a)
-   [2018 年 11 月 12 日](#November2018b)
-   [2018 年 11 月 7 日](#November2018a)
-   [2018 年 10 月 30 日](#October2018b)
-   [2018 年 10 月 9 日](#October2018a)
-   [2018 年 9 月 10 日](#September2018b)
-   [2018 年 9 月 7 日](#September2018a)
-   [2018 年 8 月 8 日](#August2018)
-   [2018 年 7 月 13 日](#July2018)
-   [2018 年 6 月 12 日](#June2018)
-   [2018 年 5 月 15 日](#May2018)
-   [2018 年 3 月 26 日](#March2018b)
-   [2018 年 3 月 1 日](#March2018a)
-   [2018 年 2 月 1 日](#February2018)
-   [2017 年 12 月 14 日](#December2017)
-   [2017 年 10 月 2 日](#October2017)
-   [2017 年 7 月 14 日](#July2017b)
-   [2017 年 7 月 1 日](#July2017a)
-   [2017 年 5 月 22 日](#May2017)
-   [2017 年 4 月 10 日](#April2017)
-   [2017 年 3 月 8 日](#March2017)
-   [2016 年 12 月 1 日](#December2016)
-   [2016 年 9 月 22 日](#September2016)
-   [2016 年 6 月 30 日](#June2016b)
-   [2016 年 6 月 23 日](#June2016a)
-   [2016 年 3 月 10 日](#March2016)
-   [2016 年 1 月 19 日](#January2016)
-   [2015 年 12 月 17 日](#December2015)
-   [2015 年 9 月 21 日](#September2015)
-   [2015 年 7 月 1 日](#July2015)

### 2019 年 3 月 11 日
{: #March2019b}

-   サービスで `max_alternatives` パラメーターの値 `0` が再び受け入れられます。`0` を指定すると、自動的にデフォルト値 `1` が使用されます。 3 月 4 日のサービス更新での変更によって、値 `0` に対してエラーが返されるようになりました。 (負の値を指定すると、エラーが返されます。)
-   サービスで `word_alternatives_threshold` パラメーターの値 `0` が再び受け入れられます。 3 月 4 日のサービス更新での変更によって、値 `0` に対してエラーが返されるようになりました。 (負の値を指定すると、エラーが返されます。)
-   サービスによって、小数点以下が 2 桁の最大精度ですべての信頼度スコアが返されるようになりました。 これには、書き起こし、単語の信頼度、単語候補、キーワードの結果、および話者ラベルの信頼度スコアが含まれます。

### 2019 年 3 月 4 日
{: #March2019}

以下の狭帯域言語モデルが更新され、音声認識が改善されました。

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

デフォルトでは、サービスはすべての音声認識要求に対して自動的に更新されたモデルを使用します。 このモデルに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用して既存のカスタム・モデルをアップグレードし、更新を活用する必要があります。

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。

### 2019 年 1 月 28 日
{: #January2019}

WebSocket インターフェースで、ブラウザー・ベースの JavaScript コードからのトークン・ベースの Identity and Access Management (IAM) 認証がサポートされるようになりました。 反対の制限事項は除去されました。 WebSocket `/v1/recognize` メソッドを使用して、認証済み接続を確立するには、以下のようにします。

-   IAM 認証を使用する場合は、`access_token` クエリー・パラメーターを含めます。
-   Cloud Foundry サービス資格情報を使用する場合は、`watson-token` クエリー・パラメーターを含めます。

詳しくは、[接続のオープン](/docs/services/speech-to-text/websockets.html#WSopen)を参照してください。

### 2018 年 12 月 20 日
{: #December2018b}

2019 年 1 月 8 日の時点で文法インターフェースがすべての場所で完全に機能します。
{: note}

-   サービスで音声認識の文法がサポートされるようになりました。 文法は、言語モデルのカスタマイズをサポートするすべての言語について、ベータ版の機能として使用できます。 カスタム言語モデルに文法を追加し、音声からサービスが認識できる一連の句に限定するためにて使用できます。 文法は、Augmented Backus-Naur Form (ABNF) または XML Form で定義できます。

    文法の処理には、以下の 4 つのメソッドを使用できます。
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`。文法ファイルをカスタム言語モデルに追加します。
    -   `GET /v1/customizations/{customization_id}/grammars`。カスタム・モデルのすべての文法に関する情報をリストします。
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}`。カスタム・モデルの指定された文法に関する情報を返します。
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}`。カスタム・モデルから既存の文法を削除します。

    WebSocket インターフェースおよび HTTP インターフェースでの音声認識に文法を使用できます。 `language_customization_id` パラメーターおよび `grammar_name` パラメーターを使用して、使用するカスタム・モデルおよび文法を識別します。 現在、音声認識要求には単一の文法のみを使用できます。

    文法について詳しくは、以下の資料を参照してください。
    -   [カスタム言語モデルでの文法の使用](/docs/services/speech-to-text/grammar.html)
    -   [文法の理解](/docs/services/speech-to-text/grammar-understand.html)
    -   [カスタム言語モデルへの文法の追加](/docs/services/speech-to-text/grammar-add.html)
    -   [音声認識での文法の使用](/docs/services/speech-to-text/grammar-use.html)
    -   [文法の管理](/docs/services/speech-to-text/grammar-manage.html)
    -   [文法の例](/docs/services/speech-to-text/grammar-examples.html)

    インターフェースのすべてのメソッドについて詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。
-   新しい数値編集機能を使用して、連続する 3 桁以上の数値をマスクできるようになりました。 編集機能の目的は、クレジット・カード番号などの機密性の高い個人情報を書き起こし結果から削除することです。この機能を有効にするには、認識要求で `redaction` パラメーターを `true` に設定します。 この機能はベータ版の機能であり、米国英語、日本語、および韓国語のみで使用できます。 詳しくは、[数値の編集](/docs/services/speech-to-text/output.html#redaction)を参照してください。
-   サービスで以下の新しいドイツ語およびフランス語の言語モデルを使用できるようになりました。
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    新しいモデルは、両方とも言語モデル・カスタマイズ (GA) および音響モデル・カスタマイズ (ベータ版) をサポートします。 詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text/custom.html#languageSupport)を参照してください。
-   新しい米国英語の言語モデル `en-US_ShortForm_NarrowbandModel` を使用できるようになりました。 この新しいモデルは、Interactive Voice Response ソリューションおよび Automated Customer Support ソリューションでの使用を目的としています。 このモデルは、言語モデル・カスタマイズ (GA) および音響モデル・カスタマイズ (ベータ版) をサポートします。
-   以下の言語モデルが更新され、音声認識が改善されました。
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    デフォルトでは、サービスはすべての音声認識要求に対して自動的に更新されたモデルを使用します。 このモデルに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用して既存のカスタム・モデルをアップグレードし、更新を活用する必要があります。
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。
-   サービスで G.729 (`audio/g729`) フォーマットの音声がサポートされるようになりました。 サービスでは狭帯域音声の G.729 Annex D のみがサポートされます。 サポートされる音声フォーマットについて詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   話者ラベル機能を英国英語の狭帯域モデル (`en-GB_NarrowbandModel`) で使用できるようになりました。 この機能は、すべてのサポート対象言語でベータ版機能です。 詳しくは、[話者ラベル](/docs/services/speech-to-text/output.html#speaker_labels)を参照してください。
-   カスタム音響モデルに追加できる音声の最大量が 50 時間から 100 時間に増加されました。

### 2018 年 12 月 13 日
{: #December2018a}

{{site.data.keyword.cloud_notm}} ロンドン・ロケーション (**eu-gb**) でこのサービスを使用できるようになりました。 すべてのロケーションと同様に、ロンドンでもトークンベースの IAM 認証を使用します。 このロケーションで作成するすべての新規サービス・インスタンスで、IAM 認証が使用されます。

### 2018 年 11 月 12 日
{: #November2018b}

サービスで日本語音声認識のスマート・フォーマット設定がサポートされるようになりました。 以前は、米国英語とスペイン語に対するスマート・フォーマット設定のみがサポートされていました。 この機能は、すべてのサポート対象言語でベータ版機能です。 詳しくは、[スマート・フォーマット設定](/docs/services/speech-to-text/output.html#smart_formatting)を参照してください。

### 2018 年 11 月 7 日
{: #November2018a}

{{site.data.keyword.cloud_notm}} 東京ロケーション (**jp-tok**) でこのサービスを使用できるようになりました。 すべてのロケーションと同様に、東京でもトークンベースの IAM 認証を使用します。 このロケーションで作成するすべての新規サービス・インスタンスで、IAM 認証が使用されます。

### 2018 年 10 月 30 日
{: #October2018b}

サービスが、すべてのロケーションでトークンベースの IAM 認証にマイグレーションされました。 すべての {{site.data.keyword.cloud_notm}} サービスで、IAM 認証が使用されるようになりました。 各ロケーションで {{site.data.keyword.speechtotextshort}} サービスが以下の日付にマイグレーションされました。

-   ダラス (**us-south**): 2018 年 10 月 30 日
-   フランクフルト (**eu-de**): 2018 年 10 月 30 日
-   ワシントン DC (**us-east**): 2018 年 6 月 12 日
-   シドニー (**au-syd**): 2018 年 5 月 15 日

IAM 認証への移行による影響は、新規のサービス・インスタンスと既存のサービス・インスタンスとで異なります。

-   *任意のロケーションで作成したすべての新規サービス・インスタンス* で、IAM 認証を使用してサービスにアクセスするようになりました。 ベアラー・トークンまたは API キーを渡すことができます。トークンでは認証済み要求がサポートされるため、呼び出すたびにサービス資格情報を埋め込む必要がありません。API キーでは、HTTP 基本認証が使用されます。 {{site.data.keyword.watson}} SDK を使用する場合は、API キーを渡して、SDK にトークンのライフサイクルを管理させることができます。
-   *示されているマイグレーション日付よりも前にロケーションで作成した既存のサービス・インスタンス* では、IAM 認証を使用するようにマイグレーションするまで、引き続き以前の Cloud Foundry サービス資格情報からの `{username}` および `{password}` が認証に使用されます。 IAM 認証へのマイグレーションについて詳しくは、[Migrating Cloud Foundry service instances to a resource group](https://{DomainName}/docs/resources/instance_migration.html) を参照してください。

詳しくは、以下の資料を参照してください。

-   サービス・インスタンスで使用される認証メカニズムを確認するには、[{{site.data.keyword.cloud_notm}} ダッシュボード ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/dashboard/apps){: new_window} でインスタンスをクリックして、サービス資格情報を表示します。
-   Watson サービスでの IAM トークンの使用について詳しくは、[IAM トークンによる認証](/docs/services/watson/getting-started-iam.html)を参照してください。
-   Watson サービスでの IAM の API キーの使用について詳しくは、[IAM サービスの API キー](/docs/services/watson/apikey-bp.html)を参照してください。
-   IAM 認証の使用例は、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。

### 2018 年 10 月 9 日
{: #October2018a}

-   `Content-Type` ヘッダーが音声認識要求でオプションになりました。 サービスでは、ほとんどの音声の音声フォーマット (MIME タイプ) が自動的に検出されるようになりました。 引き続き以下のフォーマットのコンテンツ・タイプを指定する必要があります。
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    記載がある場合は、フォーマットに対して指定したコンテンツ・タイプにサンプリング・レートを含める必要があり、オプションでチャネル数および音声のエンディアンを含めることができます。 その他すべての音声フォーマットの場合、コンテンツ・タイプを省略するか、または、コンテンツ・タイプ `application/octet-stream` を指定して、サービスによってフォーマットが自動検出されるようにすることができます。

    `curl` コマンドを使用して HTTP インターフェースで音声認識要求を行う場合、`Content-Type` ヘッダーで音声フォーマットを指定するか、`"Content-Type: application/octet-stream"` または `"Content-Type:"` を指定する必要があります。 ヘッダー全体を省略すると、`curl` はデフォルト値 `application/x-www-form-urlencoded` を使用します。
この資料のほとんどの例では、必要かどうかに関係なく、引き続き、音声認識要求に対するフォーマットが指定されています。
    {: important}

    この変更は以下のメソッドに適用されます。
    -   WebSocket 要求の `/v1/recognize`。 WebSocket オープン接続を介して要求を開始するために送信するテキスト・メッセージの `content-type` フィールドがオプションになりました。
    -   同期 HTTP 要求の `POST /v1/recognize`。 `Content-Type` ヘッダーがオプションになりました。 (マルチパートの要求の場合は、JSON メタデータの `part_content_type` フィールドもオプションになりました。)
    -   非同期 HTTP 要求の `POST /v1/recognitions`。 `Content-Type` ヘッダーがオプションになりました。

    詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   ブラジル・ポルトガル語の広帯域モデル `pt-BR_BroadbandModel` が更新され、音声認識が改善されました。 デフォルトでは、サービスはすべての認識要求に対して自動的に更新されたモデルを使用します。 このモデルに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用して既存のカスタム・モデルをアップグレードし、更新を活用する必要があります。
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。
-   音声認識メソッドの `customization_id` パラメーターが非推奨となり、将来のリリースから削除されます。 音声認識要求のカスタム言語モデルを指定するには、代わりに `language_customization_id` パラメーターを使用します。 この変更は以下のメソッドに適用されます。
    -   WebSocket 要求の `/v1/recognize`
    -   同期 HTTP 要求の `POST /v1/recognize` (マルチパートの要求を含む)
    -   非同期 HTTP 要求の `POST /v1/recognitions`
-   2018 年 10 月 1 日の時点で、音声認識のためにサービスに渡すすべての音声について課金されるようになりました。 毎月送信する音声の最初の 1,000 分は無料ではなくなりました。 サービスの価格プランについて詳しくは、[{{site.data.keyword.cloud_notm}} カタログの {{site.data.keyword.speechtotextshort}} サービス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/catalog/services/speech-to-text){: new_window} を参照してください。

### 2018 年 9 月 10 日
{: #September2018b}

初期リリース以降に修正された問題のリストは、[解決済みの問題](#known_issues)を参照してください。
{: important}

-   サービスでドイツ語の広帯域モデル `de-DE_BroadbandModel` がサポートされるようになりました。 この新しいドイツ語モデルは、言語モデル・カスタマイズ (一般出荷可能) および音響モデル・カスタマイズ (ベータ版) をサポートします。
    -   サービスによるドイツ語のコーパスの解析方法について詳しくは、[英語、フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語の解析](/docs/services/speech-to-text/language-resource.html#corpusLanguages)を参照してください。
    -   ドイツ語でのカスタム単語の同音異字発音の作成について詳しくは、[フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語のガイドライン](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR)を参照してください。
-   既存のブラジル・ポルトガル語モデル `pt-BR_BroadbandModel` および `pt-BR_NarrowbandModel` で言語モデル・カスタマイズ (一般出荷可能) がサポートされるようになりました。 このモデルはこのサポートを有効にするために更新されていないため、既存のカスタム音響モデルをアップグレードする必要がありません。
    -   サービスによるブラジル・ポルトガル語のコーパスの解析方法について詳しくは、[英語、フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語の解析](/docs/services/speech-to-text/language-resource.html#corpusLanguages)を参照してください。
    -   ブラジル・ポルトガル語でのカスタム単語の同音異字発音の作成について詳しくは、[フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語のガイドライン](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR)を参照してください。
-   米国英語と日本語の広帯域モデルおよび狭帯域モデルの新しいバージョンを使用できます。
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    新しいモデルでは、音声認識が改善されました。 デフォルトでは、サービスはすべての認識要求に対して自動的に更新されたモデルを使用します。 このモデルに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用して既存のカスタム・モデルをアップグレードし、更新を活用する必要があります。
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。
-   キーワード検出機能と単語候補機能は、すべての言語についてベータ版機能ではなく一般出荷可能 (GA) になりました。 詳しくは、以下を参照してください。
    -   [キーワード検出](/docs/services/speech-to-text/output.html#keyword_spotting)
    -   [単語候補](/docs/services/speech-to-text/output.html#word_alternatives)

### 解決済みの問題
{: #known_issues}

以下のカスタマイズ・インターフェースに関連付けられた既知の問題は、解決されました。 このリストは、過去に問題が発生した可能性があるユーザー用に保持されます。

-   カスタム言語モデルまたはカスタム音響モデルにデータを追加する場合は、モデルをリトレーニングしてから音声認識に使用する必要があります。 問題は、以下のシナリオで発生します。

    1.  ユーザーが新しいカスタム・モデル (言語または音響) を作成し、そのモデルをトレーニングします。
    1.  ユーザーが追加のリソース (単語、コーパス、または音声) をカスタム・モデルに追加しますが、そのモデルをリトレーニングしません。
    1.  ユーザーが音声認識にカスタム・モデルを使用できません。 音声認識要求で使用された場合にサービスが以下のフォームのエラーを返します。

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    この問題を回避するには、ユーザーがカスタム・モデルをその最新データでリトレーニングする必要があります。 これにより、ユーザーは音声認識にカスタム・モデルを使用できます。
-   既存のカスタム言語モデルまたはカスタム音響モデルをトレーニングする前に、モデルをその基本モデルの最新バージョンにアップグレードする必要があります。 問題は、以下のシナリオで発生します。

    1.  ユーザーが、更新されたモデルに基づく既存のカスタム・モデル (言語または音響) を持っています。
    1.  ユーザーが、最初に基本モデルの最新バージョンにアップグレードすることなく、基本モデルの古いバージョンに対して既存のカスタム・モデルをトレーニングします。
    1.  ユーザーが音声認識にカスタム・モデルを使用できません。

    この問題を回避するには、ユーザーが `POST /v1/customizations/{customization_id}/upgrade_model` メソッドまたは `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` メソッドを使用して、カスタム・モデルをその基本モデルの最新バージョンにアップグレードする必要があります。 これにより、ユーザーは音声認識にカスタム・モデルを使用できます。

これらの問題は、両方とも実動で修正されています。

### 2018 年 9 月 7 日
{: #September2018a}

セッション・ベースの HTTP REST インターフェースがサポートされなくなりました。 セッションに関連するすべての情報が資料から削除されました。 以下のメソッドは使用できなくなりました。
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

アプリケーションでセッション・インターフェースが使用されている場合は、残りの HTTP REST インターフェースの 1 つまたは WebSocket インターフェースにマイグレーションする必要があります。 詳しくは、[2018 年 8 月 8 日](#August2018)のサービス更新を参照してください。

### 2018 年 8 月 8 日
{: #August2018}

**2018 年 8 月 8 日**の時点でセッション・ベースの HTTP REST インターフェースが非推奨になります。 **2018 年 9 月 7 日**の時点でセッション API のすべてのメソッドがサービスから削除されます。その後、セッション・ベースのインターフェースは使用できなくなります。 この即時非推奨化と 30 日後の削除の通知は、以下のメソッドに適用されます。

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

アプリケーションでセッション・インターフェースが使用されている場合は、9 月 7 日までに以下のいずれかのインターフェースにマイグレーションする必要があります。
{: important}

-   ストリーム・ベースの音声認識 (ライブ使用の場合を含む) の場合は、中間結果にアクセスでき、待ち時間が最も短い [WebSocket インターフェース](/docs/services/speech-to-text/websockets.html)を使用します。
-   ファイル・ベースの音声認識の場合は、以下のいずれかのインターフェースを使用します。

    -   音声が数分以下の短いファイルの場合は、[同期 HTTP インターフェース](/docs/services/speech-to-text/http.html) `(POST /v1/recognize`) または[非同期 HTTP インターフェース](/docs/services/speech-to-text/async.html) (`POST /v1/recognitions`) を使用します。
    -   音声が数分を超える長いファイルの場合は、非同期 HTTP インターフェースを使用します。 非同期 HTTP インターフェースは、単一の要求で 1 GB までの音声データを受け入れます。

WebSocket インターフェースおよび HTTP インターフェースでは、セッション・インターフェースと同じ結果が提供されます (WebSocket インターフェースのみで中間結果が提供されます)。 任意のインターフェースでのアプリケーション開発を簡略化する Watson SDK の 1 つを使用することもできます。 詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。

### 2018 年 7 月 13 日
{: #July2018}

スペイン語の狭帯域モデル `es-ES_NarrowbandModel` が更新され、音声認識が改善されました。 デフォルトでは、サービスはすべての認識要求に対して自動的に更新されたモデルを使用します。 このモデルに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用してご使用のカスタム・モデルをアップグレードし、更新を活用する必要があります。

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。

この更新の時点で、以下の 2 つのバージョンのスペイン語の狭帯域モデルを使用できます。

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (現在)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (以前)

以下のバージョンのモデルは使用できなくなりました。

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

現在使用不可能な基本モデルに基づくカスタム・モデルの使用を試行する認識要求では、最新の基本モデルをカスタマイズせずに使用します。 サービスは、警告メッセージ `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` を返します。 使用不可能なモデルに基づくカスタム・モデルの使用を再開するには、前述のとおり、最初に適切な `upgrade_model` メソッドを使用してモデルをアップグレードする必要があります。

### 2018 年 6 月 12 日
{: #June2018}

ワシントン DC (**us-east**) でホストされているアプリケーションで以下の機能が有効になっています。

-   サービスでは、新しい API 認証プロセスがサポートされるようになりました。 詳しくは、[2018 年 10 月 30 日のサービス更新](#October2018b)を参照してください。
-   サービスでは、`X-Watson-Metadata` ヘッダーおよび `DELETE /v1/user_data` メソッドがサポートされるようになりました。 詳しくは、[機密保護](/docs/services/speech-to-text/information-security.html)を参照してください。

### 2018 年 5 月 15 日
{: #May2018}

シドニー (**au-syd**) でホストされているアプリケーションで以下の機能が有効になっています。

-   サービスでは、新しい API 認証プロセスがサポートされるようになりました。 詳しくは、[2018 年 10 月 30 日のサービス更新](#October2018b)を参照してください。
-   サービスでは、`X-Watson-Metadata` ヘッダーおよび `DELETE /v1/user_data` メソッドがサポートされるようになりました。 詳しくは、[機密保護](/docs/services/speech-to-text/information-security.html)を参照してください。

### 2018 年 3 月 26 日
{: #March2018b}

-   サービスでは、フランス語の言語モデル `fr-FR_BroadbandModel` の言語モデル・カスタマイズがサポートされるようになりました。 フランス語モデルは、言語モデル・カスタマイズでの実動使用向けに一般出荷可能になっています。
    -   サービスによるフランス語のコーパスの解析方法について詳しくは、[英語、フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語の解析](/docs/services/speech-to-text/language-resource.html#corpusLanguages)を参照してください。
    -   フランス語でのカスタム単語の同音異字発音の作成について詳しくは、[フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語のガイドライン](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR)を参照してください。
-   スペイン語と韓国語の狭帯域モデル `es-ES_NarrowbandModel` および `ko-KR_NarrowbandModel`、およびフランス語の広帯域モデル `fr-FR_BroadbandModel` が更新され、音声認識が改善されました。 デフォルトでは、サービスはすべての認識要求に対して自動的に更新されたモデルを使用します。 これらのモデルのいずれかに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用してご使用のカスタム・モデルをアップグレードし、更新を活用する必要があります。

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。
-   以下のメソッドの `version` パラメーターの名前が `base_model_version` に変更されました。

    -   WebSocket 要求の `/v1/recognize`
    -   セッションなしの HTTP 要求の `POST /v1/recognize`
    -   セッション・ベースの HTTP 要求の `POST /v1/sessions`
    -   非同期 HTTP 要求の `POST /v1/recognitions`

    `base_model_version` パラメーターは音声認識に使用される基本モデルのバージョンを指定します。 詳しくは、[アップグレードされたカスタム・モデルを使用した認識要求の実行](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition)および[基本モデルのバージョン](/docs/services/speech-to-text/input.html#version)を参照してください。
-   スマート・フォーマット設定がスペイン語および米国英語でサポートされるようになりました。 米国英語では、この機能によってキーワード・ストリングが句読点記号 (ピリオド、コンマ、疑問符、および感嘆符) に変換されるようにもなりました。 詳しくは、[スマート・フォーマット設定](/docs/services/speech-to-text/output.html#smart_formatting)を参照してください。

### 2018 年 3 月 1 日
{: #March2018a}

スペイン語とフランス語の広帯域モデル `es-ES_BroadbandModel` および `fr-FR_BroadbandModel` が更新され、音声認識が改善されました。 デフォルトでは、サービスはすべての認識要求に対して自動的に更新されたモデルを使用します。 これらのモデルのいずれかに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用してご使用のカスタム・モデルをアップグレードし、更新を活用する必要があります。

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。 このセクションでは、カスタム・モデルのアップグレードのルール、アップグレードの影響、およびアップグレードされたモデルの使用方法が示されます。

### 2018 年 2 月 1 日
{: #February2018}

サービスで、音声認識に韓国語のモデル `ko-KR_BroadbandModel` (サンプリング・レートが 16 kHz 以上の音声用) および `ko-KR_NarrowbandModel` (サンプリング・レートが 8 kHz 以上の音声用) が提供されるようになりました。 詳しくは、[言語とモデル](/docs/services/speech-to-text/models.html)を参照してください。

言語モデル・カスタマイズの場合、韓国語モデルは、実動使用向けに一般出荷可能になっています。音響モデル・カスタマイズの場合、韓国語モデルはベータ版機能です。 詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text/custom.html#languageSupport)を参照してください。

-   サービスによる韓国語のコーパスの解析方法について詳しくは、[韓国語の解析](/docs/services/speech-to-text/language-resource.html#corpusLanguages-koKR)を参照してください。
-   韓国語でのカスタム単語の同音異字発音の作成について詳しくは、[韓国語のガイドライン](/docs/services/speech-to-text/language-resource.html#wordLanguages-koKR)を参照してください。

### 2017 年 12 月 14 日
{: #December2017}

-   米国英語モデル `en-US_BroadbandModel` および `en-US_NarrowbandModel` が更新され、音声認識が改善されました。 デフォルトでは、サービスはすべての認識要求に対して自動的に更新されたモデルを使用します。 米国英語モデルに基づくカスタム言語モデルまたはカスタム音響モデルがある場合は、以下のメソッドを使用してご使用のカスタム・モデルをアップグレードし、更新を活用する必要があります。

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    手順について詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。 このセクションでは、カスタム・モデルのアップグレードのルール、アップグレードの影響、およびアップグレードされたモデルの使用方法が示されます。 現在、このメソッドは新しい米国英語基本モデルにのみ適用されます。 ただし、他の基本モデルのアップグレードが使用可能になったときにも、同じ情報が適用されます。
-   認識要求を行うさまざまな方法として、基本モデルおよびカスタム・モデルの以前のバージョンまたはアップグレードされたバージョンを使用する要求を開始するために使用できる新しい `base_model_version` パラメーターが含まれるようになりました。 主にアップグレードされたカスタム・モデルでの使用を意図していますが、`base_model_version` パラメーターは、カスタム・モデルなしでも使用できます。 詳しくは、[基本モデルのバージョン](/docs/services/speech-to-text/input.html#version)を参照してください。
-   サービスでは、すべての使用可能な言語で音響モデル・カスタマイズがベータ版機能としてサポートされるようになりました。 すべての言語の広帯域モデルおよび狭帯域モデルのカスタム音響モデルを作成できます。 音響モデル・カスタマイズを含むカスタマイズの概要は、[カスタマイズ・インターフェース](/docs/services/speech-to-text/custom.html)を参照してください。
-   サービスでは、英国英語モデル `en-GB_BroadbandModel` および `en-GB_NarrowbandModel` の言語モデル・カスタマイズがサポートされるようになりました。 サービスでは、英国英語と米国英語のコーパスおよびカスタム単語が概ね類似した方法で処理されますが、重要な相違点がいくつかあります。
    -   サービスによる英国英語のコーパスの解析方法について詳しくは、[英語、フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語の解析](/docs/services/speech-to-text/language-resource.html#corpusLanguages)を参照してください。
    -   英国英語でのカスタム単語の同音異字発音の作成について詳しくは、[英語 (米国および英国) のガイドライン](/docs/services/speech-to-text/language-resource.html#wordLanguages-enUS-enGB)を参照してください。 特に、英国英語では同音異字発音にピリオドやダッシュを使用することはできません。
-   言語モデル・カスタマイズおよびすべての関連パラメーターが、すべてのサポート対象言語 (日本語、スペイン語、英国英語、米国英語) で一般出荷可能 (GA) になりました。

### 2017 年 10 月 2 日
{: #October2017}

-   カスタマイズ・インターフェースで、音響モデル・カスタマイズが提供されるようになりました。 サービスの基本モデルをサービス環境および話者に適合させるカスタム音響モデルを作成できます。 書き起こそうとする音声の音響的特性に対する一致度がより高い音声のカスタム音響モデルを取り込み、トレーニングします。 その後、認証要求でカスタム音響モデルを使用して、音声認識の正確度を高めます。

    カスタム音響モデルはカスタム言語モデルを補完します。 カスタム言語モデルでカスタム音響モデルをトレーニングし、音声認識中に両方のタイプのモデルを使用できます。 音響モデル・カスタマイズは、米国英語、スペイン語、および日本語でのみ使用可能なベータ版インターフェースです。

    -   カスタマイズ・インターフェースでサポートされる言語および各言語に使用可能なサポートのレベルについて詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text/custom.html#languageSupport)を参照してください。
    -   サービスのカスタマイズ・インターフェースについて詳しくは、[カスタマイズ・インターフェース](/docs/services/speech-to-text/custom.html)を参照してください。
    -   カスタム音響モデルの作成について詳しくは、[カスタム音響モデルの作成](/docs/services/speech-to-text/acoustic-create.html)を参照してください。
    -   カスタム音響モデルの使用について詳しくは、[カスタム音響モデルの使用](/docs/services/speech-to-text/acoustic-use.html)を参照してください。
    -   カスタマイズ・インターフェースのすべてのメソッドについて詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。
-   言語モデル・カスタマイズについて、サービスに、カスタム言語モデルにオプションのカスタマイズの重み付けを設定するベータ版機能が含まれるようになりました。 カスタマイズの重み付けは、カスタム言語モデルの単語とサービスの基本語彙の単語に付与される相対的重み付けを指定します。 トレーニング中および音声認識中の両方でカスタマイズの重み付けを設定できます。 詳しくは、[カスタマイズの重み付けの使用](/docs/services/speech-to-text/language-use.html#weight)を参照してください。
-   `ja-JP_BroadbandModel` 言語モデルが基本モデルの改善を反映してアップグレードされました。 このアップグレードは、このモデルに基づく既存のカスタム・モデルには影響を与えません。
-   サービスに、`audio/l16` (16 ビットのリニア PCM (Pulse-Code Modulation)) フォーマットで送信された音声のエンディアンを指定するパラメーターが含まれるようになりました。 このフォーマットで `rate` パラメーターおよび `channels` パラメーターを指定する以外に、`endianness` パラメーターで `big-endian` または `little-endian` を指定できるようになりました。 詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。

### 2017 年 7 月 14 日
{: #July2017b}

-   サービスで、MP3 フォーマットや Motion Picture Experts Group (MPEG) フォーマットの音声の書き起こしがサポートされるようになりました。 サポートされる音声フォーマットについて詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   言語モデル・カスタマイズ・インターフェースで、ベータ機能としてスペイン語がサポートされるようになりました。スペイン語の基本言語モデル `es-ES_BroadbandModel` または `es-ES_NarrowbandModel` に基づくカスタム・モデルを作成できます。詳しくは、[カスタム言語モデルの作成](/docs/services/speech-to-text/language-create.html)を参照してください。スペイン語のカスタム言語モデルを使用する認識要求の価格設定は、米国英語モデルおよび日本語モデルを使用する要求と同じです。
-   新しいカスタム言語モデルを作成するために `POST /v1/customizations` メソッドに渡す JSON `CreateLanguageModel` オブジェクトに、`dialect` フィールドが含まれるようになりました。このフィールドは、カスタム・モデルで使用される言語の方言を指定します。デフォルトでは、方言は基本モデルの言語と一致します。このパラメーターは、スペイン語モデルの場合にのみ意味があります。サービスでは、スペイン語モデルに対して、以下に示されたいずれかの方言での音声に適合するカスタム・モデルが作成されます。
    -   カスティリャ・スペイン語の場合は `es-ES` (デフォルト)
    -   ラテンアメリカ・スペイン語の場合は `es-LA`
    -   北米 (メキシコ) スペイン語の場合は `es-US`

    カスタマイズ・インターフェースの `GET /v1/customizations` メソッドと `GET /v1/customizations/{customization_id}` メソッドでは、出力にカスタム・モデルの方言が含まれます。 詳しくは、[カスタム言語モデルの作成](/docs/services/speech-to-text/language-create.html#createModel-language)および[カスタム言語モデルのリスト](/docs/services/speech-to-text/language-models.html#listModels-language)を参照してください。
-   言語モデル `en-UK_BroadbandModel` および `en-UK_NarrowbandModel` の名前が非推奨となりました。 これらのモデルは、`en-GB_BroadbandModel` および `en-GB_NarrowbandModel` という名前で使用できるようになりました。

    非推奨となった `en-UK_{model}` という名前は引き続き機能しますが、`GET /v1/models` メソッドによって、使用可能なモデルのリストでこの名前が返されなくなりました。 `GET /v1/models/{model_id}` メソッドを使用して直接この名前を照会することはできます。

### 2017 年 7 月 1 日
{: #July2017a}

-   サービスの言語モデル・カスタマイズ・インターフェースが、サポート対象言語である米国英語と日本語の両方で一般出荷可能 (GA) になりました。 {{site.data.keyword.IBM_notm}} は、カスタム言語モデルの作成、ホスティング、または管理には課金しません。 次の黒丸の項目で説明されているように、{{site.data.keyword.IBM_notm}} はカスタム・モデルを使用する認識要求の音声の 1 分あたり、$0.03 (USD) を追加で課金するようになりました。
-   {{site.data.keyword.IBM_notm}} は、以下のようにサービスの価格設定を更新しました
    -   狭帯域モデルの使用に対する追加料金の廃止
    -   高ボリューム顧客への段階的階層型価格設定の提供
    -   米国英語または日本語のカスタム言語モデルを使用する認識要求の音声の 1 分あたり、$0.03 (USD) の追加課金

    価格設定の更新について詳しくは、以下を参照してください
    -   ブログ投稿 [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   [{{site.data.keyword.cloud_notm}} カタログ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/catalog/services/speech-to-text){: new_window} の {{site.data.keyword.speechtotextshort}} サービス
    -   [料金設定に関する FAQ](/docs/services/speech-to-text/faq-pricing.html)
-   以下の `POST` 要求の本体として空のデータ・オブジェクトを渡す必要がなくなりました。
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    例えば、以下のように `curl` を使用して `POST /v1/sessions` メソッドを呼び出します。

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    要求で `curl` オプションの `--data "{}"` を渡す必要がなくなりました。

    これらの `POST` 要求のいずれかで問題が発生した場合は、要求の本体で空のデータ・オブジェクトを渡します。 空のオブジェクトを渡しても、要求の性質や意味は変更されません。
    {: note}

### 2017 年 5 月 22 日
{: #May2017}

以下の非推奨化は最初に 2017 年 3 月に告知され、現在有効になっています。

-   `continuous` パラメーターが、認識要求を開始するすべてのメソッドから削除されます。 サービスでは、音声が終了するか、タイムアウトになるまで音声ストリーム全体が書き起こされるようになりました。 この動作は、以前の `continuous` パラメーターを `true` に設定した場合と同じです。 このパラメーターが省略されたり、`false` に設定されたりした場合、デフォルトでは、サービスは最初の 0.5 秒の発話なし (通常、無音) で書き起こしを停止します。

    このパラメーターを `true` に設定する既存のアプリケーションの動作は変更されません。 このパラメーターを `false` に設定するアプリケーションまたはデフォルトの動作に依存するアプリケーションは、変更される可能性があります。 要求にこのパラメーターが指定されている場合に、サービスでは未知のパラメーターの警告メッセージを返して応答するようになりました。

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    警告にも関わらず要求は成功し、既存のセッションまたは WebSocket 接続は影響を受けません。

    `continuous=false` を指定することにはほとんどメリットがなく、書き起こし全体の正確度が低下する可能性があるという、開発者コミュニティーからの多くのフィードバックを反映し、{{site.data.keyword.IBM_notm}} はこのパラメーターを削除しました。
-   音声を送信せずにセッションのタイムアウトを回避することができなくなりました。
    -   WebSocket インターフェースを使用する場合、クライアントが、`action` パラメーターを `no-op` に設定した JSON テキスト・メッセージを送信することによって接続存続を維持することができなくなりました。 `no-op` メッセージを送信してもエラーは生成されませんが、効果もありません。
    -   HTTP インターフェースでセッションを使用する場合、クライアントが `GET /v1/sessions/{session_id}/recognize` 要求を送信してセッションを延長することができなくなりました。 このメソッドによって、引き続きアクティブ・セッションのステータスが返されますが、セッションがアクティブのままにはなりません。
    以下を行って、セッション存続を維持できるようになりました。
    -   `inactivity_timeout` パラメーターを `-1` に設定して、30 秒間の非アクティブ・タイムアウトを回避します。
    -   無音のみが含まれた任意の音声データをサービスに送信して、30 秒間のセッション・タイムアウトを回避します。 セッションを延長するために送信した無音を含め、サービスにデータを送信した期間に対して課金されます。

    詳しくは、[タイムアウト](/docs/services/speech-to-text/input.html#timeouts)を参照してください。 理想的には、書き起こし用の音声を取得する直前にセッションを確立し、リアルタイムに近いレートで音声を送信して、セッションを保持します。 また、アプリケーションがクローズされたセッションまたは接続から滑らかに復旧することを確認します。

    {{site.data.keyword.IBM_notm}} は、すべてのユーザーに最高クラスで低待ち時間の音声認識サービスを引き続き提供できるように、この機能を削除しました。

### 2017 年 4 月 10 日
{: #April2017}

-   サービスで、以下の広帯域モデルの話者ラベル機能がサポートされるようになりました。
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    詳しくは、[話者ラベル](/docs/services/speech-to-text/output.html#speaker_labels)を参照してください。
-   サービスで、Opus コーデックまたは Vorbis コーデックを使用した Web Media (WebM) 音声フォーマットがサポートされるようになりました。 また、Opus コーデック以外に Vorbis コーデックを使用した Ogg 音声フォーマットもサポートされるようになりました。 サポートされる音声フォーマットについて詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   サービスは、Cross-Origin Resource Sharing (CORS) をサポートして、ブラウザー・ベースのクライアントが直接サービスを呼び出すことを許可するようになりました。 詳しくは、『[CORS サポート](/docs/services/speech-to-text/developer-overview.html#cors)』を参照してください。
-   非同期 HTTP インターフェースで、ホワイトリストに登録されたコールバック URL の登録を削除する `POST /v1/unregister_callback` メソッドが提供されるようになりました。 詳しくは、[コールバック URL の登録解除](/docs/services/speech-to-text/async.html#unregister)を参照してください。
-   WebSocket インターフェースでは、特に長い音声ファイルの認識要求がタイムアウトしなくなりました。 タイムアウトを回避するために JSON `start` メッセージで中間結果を要求する必要がなくなりました。 (この問題は 2016 年 3 月 10 日のリリース・ノートで説明されています。)
-   存在しないカスタム・モデルを削除しようとすると、`DELETE /v1/customizations/{customization_id}` メソッドによって、HTTP 応答コード 401 が返されるようになりました。
-   存在しないコーパスを削除しようとすると、`DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドによって、HTTP 応答コード 400 が返されるようになりました。

### 2017 年 3 月 8 日
{: #March2017}

非同期 HTTP インターフェースが一般出荷可能になりました。 この日付の前までは、ベータ機能でした。

### 2016 年 12 月 1 日
{: #December2016}

-   サービスで、米国英語、スペイン語、または日本語の狭帯域音声のベータ版話者ラベル機能が提供されるようになりました。 この機能では、複数人のやり取りで、どの単語をどの話者が発話したかを識別します。 セッションなし、セッション・ベース、非同期、および WebSocket 認識メソッドには、それぞれ応答に話者ラベルが含まれるかどうかを示すブール値を受け入れる `speaker_labels` パラメーターが含まれています。 この機能について詳しくは、[話者ラベル](/docs/services/speech-to-text/output.html#speaker_labels)を参照してください。
-   ベータ版の言語モデル・カスタマイズ・インターフェースが、米国英語以外に日本語でもサポートされるようになりました。 このインターフェースのすべてのメソッドで日本語がサポートされます。 詳しくは、以下のセクションを参照してください。
    -   詳しくは、[カスタム言語モデルの作成](/docs/services/speech-to-text/language-create.html)および[カスタム言語モデルの使用](/docs/services/speech-to-text/language-use.html)を参照してください。
    -   コーパス・テキスト・ファイルの追加に関する一般的な考慮事項および日本語に固有の考慮事項は、[コーパス・ファイルの準備](/docs/services/speech-to-text/language-resource.html#prepareCorpus)および[コーパス・ファイル追加時の動作](/docs/services/speech-to-text/language-resource.html#parseCorpus)を参照してください
    -   カスタム単語の `sounds_like` フィールドの指定時の日本語に固有の考慮事項は、[日本語のガイドライン](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP)を参照してください。
    -   カスタマイズ・インターフェースのすべてのメソッドについて詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。
-   言語モデル・カスタマイズ・インターフェースに、指定されたコーパスに関する情報をリストする `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドが含まれるようになりました。 このメソッドは、コーパスをカスタム・モデルに追加する要求のステータスのモニターに役立ちます。 詳しくは、[カスタム言語モデルのコーパスのリスト](/docs/services/speech-to-text/language-corpora.html#listCorpora)を参照してください。
-   `GET /v1/customizations/{customization_id}/words` メソッドおよび `GET /v1/customizations/{customization_id}/words/{word_name}` メソッドによって返される JSON 出力に、各単語の `count` フィールドが含まれるようになりました。 このフィールドは、すべてのコーパスでその単語が見つかった回数を示します。 カスタム単語がコーパスによって追加される前に、モデルにその単語を追加した場合、カウントは `1` から開始されます。最初にコーパスまたは文法から単語が追加され、後で変更した場合、カウントにはその単語がコーパスで見つかった回数のみが反映されます。 詳しくは、[カスタム言語モデルの単語のリスト](/docs/services/speech-to-text/language-words.html#listWords)を参照してください。

    `count` フィールドが存在する前に作成されたカスタム・モデルの場合、このフィールドは常に `0` のままとなります。このようなモデルのこのフィールドを更新するには、モデルのコーパスを再度追加し、`POST /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドとともに `allow_overwrite` パラメーターを含めます。
-   `GET /v1/customizations/{customization_id}/words` メソッドに、単語がリストされる順序を制御する `sort` クエリー・パラメーターが含まれるようになりました。 このパラメーターは、単語のソート方法を示す 2 つの引数 `alphabetical` または `count` を受け入れます。 オプションの `+` または `-` を引数の前に付加して、結果を昇順または降順のいずれでソートするかを示すことができます。 デフォルトでは、このメソッドによって単語が昇順のアルファベット順で表示されます。詳しくは、[カスタム言語モデルの単語のリスト](/docs/services/speech-to-text/language-words.html#listWords)を参照してください。

    `count` フィールドが導入される前に作成されたカスタム・モデルの場合、`sort` パラメーターで `count` 引数を使用しても無意味です。 このようなモデルでは、デフォルトの `alphabetical` 引数を使用します。
-   `GET /v1/customizations/{customization_id}/words` メソッドおよび `GET /v1/customizations/{customization_id}/words/{word_name}` メソッドからの JSON 応答の一部として返されることがある `error` フィールドが配列になりました。 サービスによってカスタム単語定義で 1 つ以上の問題が検出された場合、このフィールドには定義の各問題要素がリストされ、問題を説明するメッセージが提供されます。 詳しくは、[カスタム言語モデルの単語のリスト](/docs/services/speech-to-text/language-words.html#listWords)を参照してください。
-   認識メソッドの `keywords_threshold` パラメーターおよび `word_alternatives_threshold` パラメーターでは、ヌル値が受け入れられなくなりました。 応答からキーワードと単語候補を省略するには、これらのパラメーターを省略します。 指定される値は、浮動小数点である必要があります。

サービスのインターフェースについて詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。

### 2016 年 9 月 22 日
{: #September2016}

-   このサービスで、新しいベータ版の米国英語用言語モデル・カスタマイズ・インターフェースが提供されるようになりました。 このインターフェースを使用して、分野固有の用語を含むカスタム言語モデルを作成することによって、サービスの基本語彙および言語モデルを調整できます。 カスタム単語は、ユーザーが個々に追加することも、このサービスにコーパスから抽出させることもできます。 このサービスのいずれのインターフェースで提供されている音声認識メソッドでカスタム・モデルを使用するには、`customization_id` クエリー・パラメーターを渡します。 詳しくは、以下を参照してください。
    -   [カスタム言語モデルの作成](/docs/services/speech-to-text/language-create.html)
    -   [カスタム言語モデルの使用](/docs/services/speech-to-text/language-use.html)
    -   [API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window}
-   サポートされる音声フォーマットのリストに、u-law (または mu-law) データ・アルゴリズムを使用してエンコードされた単一チャネルの音声を提供する `audio/mulaw` が追加されました。 このフォーマットを使用する場合は、音声取り込みのサンプリング・レートも指定する必要があります。 詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   `GET /v1/models` メソッドおよび `GET /v1/models/{model_id}` メソッドが、各言語モデルに対する出力の一部として `supported_features` フィールドを返すようになりました。 この追加情報により、そのモデルがカスタマイズをサポートするかどうかが示されます。 詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。

### 2016 年 6 月 30 日
{: #June2016b}

ベータ版の非同期 HTTP インターフェースが、サービスでサポートされるすべての言語をサポートするようになりました。 このインターフェースは、以前は米国英語でのみ使用できました。 詳しくは、[非同期 HTTP インターフェース](/docs/services/speech-to-text/async.html)および [API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。

### 2016 年 6 月 23 日
{: #June2016a}

-   ベータ版の非同期 HTTP インターフェースが使用可能になりました。 このインターフェースは、ノンブロッキング HTTP 呼び出しを使用することで、米国英語の書き起こしに完全な認識機能を提供します。 コールバック URL を登録し、ユーザー指定の秘密ストリングを指定することで、デジタル署名による認証とデータ保全性を実現できます。 詳しくは、[非同期 HTTP インターフェース](/docs/services/speech-to-text/async.html)および [API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。
-   ベータ版のスマート・フォーマット設定機能により、最終書き起こしで、日付、時刻、一連の数字および数値、電話番号、通貨価値、およびインターネット・アドレスを、より標準的な表現に変換されます。 この機能を有効にするには、認識要求で `smart_formatting` パラメーターを `true` に設定します。 この機能は、ベータ版であり、米国英語でのみ使用可能です。 詳しくは、[スマート・フォーマット設定](/docs/services/speech-to-text/output.html#smart_formatting)を参照してください。
-   音声認識でサポートされるモデルのリストに、フランス語の音声に対応する、最小 16 kHz のサンプリング・レートの `fr-FR_BroadbandModel` が含まれるようになりました。詳しくは、[言語とモデル](/docs/services/speech-to-text/models.html)を参照してください。
-   サポートされている音声フォーマットのリストに `audio/basic` が含まれるようになりました。 このフォーマットは、サンプリング・レート 8 kHz で、8 ビットの u-law (mu-law) データを使用してエンコードされた単一チャネル音声を提供します。 詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   各種の認識メソッドで、要求に含まれる無効なクエリー・パラメーターまたは JSON フィールドに関するメッセージが含まれる `warnings` 応答を返すことができるようになりました。 この警告のフォーマットが変更されました。 例えば、`"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` は、`"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."` になりました
-   HTTP `POST` 要求では、`{}` の形で空の要求本体を含めなければ、サービスにデータが渡されません。 `curl` コマンドでは、`--data` オプションを使用して空のデータを渡します。

### 2016 年 3 月 10 日
{: #March2016}

-   データ伝送の両方の形式 (1 回限りの送信とストリーミング) で、WebSocket インターフェースと同様に、音声データには 100 MB のサイズ制限が課されるようになりました。 以前は、1 回限りの方法では、最大 4 MB のデータ制限が課されていました。 詳しくは、[音声の伝送](/docs/services/speech-to-text/input.html#transmission) (すべてのインターフェース) および[音声の送信および認識結果の受信](/docs/services/speech-to-text/websockets.html#WSaudio) (WebSocket インターフェース) を参照してください。 WebSocket のセクションでは、WebSocket インターフェースによって適用される 4 MB の最大フレーム (メッセージ) サイズについても説明しています。
-   認識要求の JSON 応答に、要求に含まれる無効なクエリー・パラメーターまたは JSON フィールドに関する警告メッセージの配列を含めることができるようになりました。 配列の各要素は警告の性質を記述するストリングであり、無効な引数ストリングの配列が続きます。 例えば、`"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]` となります。 詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。
-   *Apple&reg; iOS オペレーティング・システム用の {{site.data.keyword.watson}} Speech Software Development Kit (SDK)* (ベータ版) は非推奨となりました。 代わりに *Apple&reg; iOS オペレーティング・システム用の {{site.data.keyword.watson}} SDK* を使用します。 新しい SDK は、GitHub 上の `watson-developer-cloud` 名前空間の [ios-sdk リポジトリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} から入手できます。

WebSocket インターフェースには、現在、以下の既知の問題があります。

-   特に長い音声ファイルの認識要求では、サービスで最終結果が生成されるまでに何分もかかることがあります。 WebSocket インターフェースでは、サービスによる応答の準備中に基盤の TCP 接続がアイドル状態のままになります。 このため、タイムアウトのために接続がクローズする可能性があります。 WebSocket インターフェースでタイムアウトを回避するには、要求を開始するための `start` メッセージの JSON で中間結果を要求します (`\"interim_results\": \"true\"`)。 中間結果は、不要であれば破棄できます。 この問題は、今後の更新で解決される予定です。

### 2016 年 1 月 19 日
{: #January2016}

サービスは、2016 年 1 月 19 日に更新され、新しい用語禁止フィルター機能が組み込まれました。 デフォルトでは、サービスは米国英語音声の書き起こし結果で禁止用語を校閲します。 詳しくは、[禁止用語フィルター](/docs/services/speech-to-text/output.html#profanity_filter)を参照してください。

### 2015 年 12 月 17 日
{: #December2015}

-   サービスでキーワード検出機能が提供されるようになりました。 入力音声で突き合わせるキーワード・ストリングの配列を指定できます。 また、キーワードに対する一致と見なされるために単語が満たす必要があるユーザー定義の信頼度レベルを指定する必要があります。 詳しくは、[キーワード検出](/docs/services/speech-to-text/output.html#keyword_spotting)を参照してください。

    キーワード検出機能はベータ機能です。
    {: note}
-   サービスで単語候補機能が提供されるようになりました。 この機能は、ユーザー定義の信頼度レベルを満たした、入力音声内の単語に対する仮説候補を返します。 詳しくは、[単語候補](/docs/services/speech-to-text/output.html#word_alternatives)を参照してください。

    単語候補機能はベータ機能です。
    {: note}
-   サービスで、書き起こしモデルでの追加の言語がサポートされるようになりました (英国英語の `en-UK_BroadbandModel` と `en-UK_NarrowbandModel`、現代標準アラビア語の `ar-AR_BroadbandModel`)。 詳しくは、[言語とモデル](/docs/services/speech-to-text/models.html)を参照してください。
-   HTTP 認識要求で、10 分のプラットフォーム・タイムアウトが適用されなくなりました。 サービスでは、認識が進行中である限り、20 秒ごとに応答 JSON オブジェクトでスペース文字を送信することで、接続を存続したままに保つようになりました。 詳しくは、[タイムアウト](/docs/services/speech-to-text/input.html#timeouts)を参照してください。
-   サービスで、セッション・ベースの HTTP メソッド `GET /v1/sessions/{session_id}/observe_result` および `POST /v1/sessions/{session_id}/recognize` に対して HTTP ステータス・コード 490 が返されなくなりました。 代わりに、サービスは HTTP ステータス・コード 400 で応答するようになりました。

    サービスでは、セッション・ベースのメソッドでのエラーに対して返される JSON 応答に、新しい `session_closed` フィールドも含まれるようになりました。 セッションがエラーの結果としてクローズされた場合、このフィールドが `true` に設定されます。 任意のメソッドで返される可能性がある戻りコードについて詳しくは、[API リファレンス![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。
-   `curl` コマンドを使用してサービスで音声を書き起こす際に、1 秒あたり 40,000 バイトより速い速度でデータを転送しないように `--limit-rate` オプションを使用する必要がなくなりました。

### 2015 年 9 月 21 日
{: #September2015}

-   2 つの新しいベータ・モバイル SDK が音声サービスで使用可能になりました。 SDK を使用して、モバイル・アプリケーションは {{site.data.keyword.speechtotextshort}} サービスと {{site.data.keyword.texttospeechshort}} サービスの両方と対話できます。
    -   *Google Android&trade; プラットフォーム用の {{site.data.keyword.watson}} Speech SDK* は、音声を {{site.data.keyword.speechtotextshort}} サービスにリアルタイムでストリーミングし、発話した音声の書き起こしを受け取るためのサポートを提供します。 このプロジェクトには、両音声サービスとの対話を示すサンプル・アプリケーションが含まれています。 SDK は、GitHub 上の `watson-developer-cloud` 名前空間の [speech-android-sdk リポジトリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window} から入手できます。
    -   *Apple&reg; iOS オペレーティング・システム用の {{site.data.keyword.watson}} Speech SDK* は、音声を {{site.data.keyword.speechtotextshort}} サービスにストリーミングし、応答で音声の書き起こしを受け取るためのサポートを提供します。 SDK は、GitHub 上の `watson-developer-cloud` 名前空間の [speech-ios-sdk リポジトリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window} から入手できます。

    いずれの SDK でも、{{site.data.keyword.cloud_notm}} サービス資格情報または認証トークンを使用した音声サービスへの認証のサポートが提供されます。

    SDK はベータ版であるため、今後変更されることがあります。
    {: note}
-   サービスで 2 つの新しい言語 (ブラジル・ポルトガル語と北京語) がサポートされるようになりました。 これらの新しい言語のモデルは、`pt-BR_BroadbandModel`、`pt-BR_NarrowbandModel`、`zh-CN_BroadbandModel`、および `zh-CN_NarrowbandModel` です。 詳しくは、[言語とモデル](/docs/services/speech-to-text/models.html)を参照してください。
-   HTTP `POST` 要求の `/v1/sessions/{session_id}/recognize` と `/v1/recognize`、および WebSocket `/v1/recognize` 要求で新しいメディア・タイプ (Opus コーデックを使用している Ogg フォーマットのファイル用の `audio/ogg;codecs=opus`) の書き起こしがサポートされます。 また、各メソッドの `audio/wav` フォーマットで任意のエンコードがサポートされるようになりました。 リニア PCM エンコードの使用に関する制限事項が削除されました。 詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   HTTP インターフェースを使用して長い音声ファイルを書き起こす際にタイムアウトを克服するためのサポートがサービスで提供されるようになりました。 セッションを使用している場合、長期間実行される認識タスクに対して `GET /v1/sessions/{session_id}/observe_result` および `POST /v1/sessions/{session_id}/recognize` メソッドを使用してシーケンス ID を指定することで、長いポーリング・パターンを利用できます。 これらのメソッドの新しい `sequence_id` パラメーターを使用すると、認識要求の送信前、送信時、または送信後に結果を要求できます。
-   米国英語モデル `en_US_BroadbandModel` および `en_US_NarrowbandModel` について、サービスで多くの代名詞の語頭が正しく大文字化されるようになりました。 例えば、サービスは、「barack obama graduated from columbia university」ではなく、「Barack Obama graduated from Columbia University」というテキストを返すようになりました。 この変更は、ご使用のアプリケーションが代名詞の大/小文字に何らかの形で依存している場合に役立つ可能性があります。
-   HTTP `DELETE /v1/sessions/{session_id}` 要求は戻りステータス・コード 415「サポートしていないメディア・タイプです」を返しません。 メソッドの資料からこの戻りコードが削除されました。

### 2015 年 7 月 1 日
{: #July2015}

サービスは、2015 年 7 月 1 日に、ベータ版から一般出荷版 (GA) に移行しました。 {{site.data.keyword.speechtotextshort}} API のベータ版と GA 版には以下のような違いがあります。 GA リリースにより、ユーザーはサービスの新しいバージョンにアップグレードする必要があります。

-   HTTP API の GA 版には、ベータ版との互換性があります。 既存のアプリケーション・コードを変更する必要が生じるのは、モデル名を明示的に指定していた場合のみです。 例えば、GitHub からサービス用に入手可能なサンプル・コードでは、ファイル `demo.js` に以下のコード行が含まれていました。

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    この行では、サービスのベータ版のデフォルト・モデル `WatsonModel` を指定しています。 ご使用のアプリケーションでもこのモデルを指定している場合は、GA 版でサポートされる新規モデルのいずれかを使用するように変更する必要があります。 詳しくは、次の黒丸の項目を参照してください。
-   サービスで、WebSocket 接続を介したクライアントとサービス間の直接対話用の新規プログラミング・モデルがサポートされるようになりました。 クライアントは、このモデルを使用して、サービスと直接通信するための認証トークンを取得できます。 このトークンによって、クライアントの代わりにサービスを呼び出す {{site.data.keyword.cloud_notm}} 内のサーバー・サイド・プロキシー・アプリケーションを使用せずに済むようになります。 トークンは、クライアントがサービスと対話する優先手段です。

    サービスでは引き続き、クライアントとサービス間での音声およびメッセージの中継についてサーバー・サイドのプロキシーに依拠していた古いプログラミング・モデルがサポートされます。 ただし、新しいモデルの方が効率的であり、スループットも高くなります。 新しいプログラミング・モデルについて詳しくは、[{{site.data.keyword.watson}} サービスのプログラミング・モデル](/docs/services/watson/getting-started-develop.html)を参照してください。
-   `POST /v1/sessions` メソッドおよび `POST /v1/recognize` メソッドと WebSocket `/v1/recognize` メソッドで、`model` クエリー・パラメーターがサポートされるようになりました。 このパラメーターを使用して、音声に関する情報を指定します。

    -   言語: *英語*、*日本語*、または*スペイン語*
    -   最小サンプリング・レート: *広帯域モデル* (16 kHz) または*狭帯域モデル* (8 kHz)

    詳しくは、[言語とモデル](/docs/services/speech-to-text/models.html)を参照してください。
-   `recognize` メソッドの `Content-Type` ヘッダーで、`audio/flac` および `audio/l16` に加え、WAV (Waveform Audio File Format) ファイルの `audio/wav` がサポートされるようになりました。 サポートされる音声フォーマットについて詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
-   `recognize` メソッドで、アプリケーションのニーズに合わせてサービスを調整するために使用できる、以下のさまざまな新規クエリー・パラメーターがサポートされるようになりました。
    -   `inactivity_timeout` パラメーター。サービスがストリーミング・モードで無音 (発話なし) を検出した場合に接続をクローズするまでのタイムアウト値 (秒) を設定します。 デフォルトでは、サービスは 30 秒無音が続くと、セッションを終了します。 [タイムアウト](/docs/services/speech-to-text/input.html#timeouts)を参照してください。
    -   `max_alternatives` パラメーターは、音声書き起こしで *N* 個の最適仮説候補を返すようにサービスに指示します。 [最大候補数](/docs/services/speech-to-text/output.html#max_alternatives)を参照してください。
    -   `word_confidence` パラメーターは、書き起こしの各単語の信頼度スコアを返すようにサービスに指示します。 [単語の信頼度](/docs/services/speech-to-text/output.html#word_confidence)を参照してください。
    -   `timestamps` パラメーターは、音声の先頭を基準とした相対時間で、書き起こしの各単語の開始時間と終了時間を返すようにサービスに指示します。 [単語のタイム・スタンプ](/docs/services/speech-to-text/output.html#word_timestamps)を参照してください。
-   `GET /v1/sessions/{session_id}/observeResult` メソッドが、`GET /v1/sessions/{session_id}/observe_result`という名前になりました。 後方互換性のため、名前 `observeResult` は引き続きサポートされます。
-   `GET /v1/sessions/{session_id}/observe_result`、`POST /v1/sessions/{session_id}/recognize`、および `POST /v1/recognize` の各メソッドに、ヘッダー・パラメーター `X-WDC-PL-OPT-OUT` が含まれるようになりました。このパラメーター、サービスが要求の音声および書き起こしデータを使用して将来の結果を改善するかどうかを制御します。 WebSocket インターフェースには、これに相当するクエリー・パラメーターが含まれています。 サービスが音声および書き起こしの結果を使用しないようにするには、値 `1` を指定します。 このパラメーターは、現在の要求にのみ適用されます。 ベータ API の `X-logging` ヘッダーは新しいヘッダーによって置き換えられています。 [{{site.data.keyword.watson}} サービスの要求ロギングの制御](../common/getting-started-logging.html)を参照してください。
-   サービスで、ストリーミング・モードにおけるセッション当たりのデータに 100 MB の制限が課されるようになりました。 ストリーミング・モードを指定するには、ヘッダー `Transfer-Encoding` で値 `chunked` を指定します。 音声ファイルの 1 回限りの送信では引き続き、送信されるデータに対して 4 MB のサイズ制限が課されます。 [音声の伝送](/docs/services/speech-to-text/input.html#transmission)を参照してください。
-   `/v1/models`、`/v1/models/{model_id}`、`/v1/sessions`、`/v1/sessions/{session_id}`、`/v1/sessions/{session_id}/observe_result`、`/v1/sessions/{session_id}/recognize`、および `/v1/recognize` の各メソッドにエラー・コード 415 (「サポートしていないメディア・タイプです」) が追加されました。
-   `/v1/sessions/{session_id}/recognize` メソッドへの `POST` および `GET` 要求の以下のエラー・コードが変更されました。
    -   エラー・コード 404 (「Session_id が見つかりません」) のメッセージの説明が詳細になりました (`POST` および `GET`)。
    -   エラー・コード 503 (「セッションは既に要求を処理しています。 同時リクエストは同じセッションで許されていません。 このエラーの後もセッションは存在し続けます」) のメッセージの説明が詳細になりました (`POST` のみ)。
-   `/v1/sessions` メソッドおよび `/v1/recognize` メソッドへの HTTP `POST` 要求で、エラー・コード 503 (「サービスが利用できません」) が返されることがあります。 このエラー・コードは、`/v1/recognize` メソッドを使用して WebSocket 接続を作成する際にも返されることがあります。
