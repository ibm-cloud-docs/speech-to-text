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

# 入力機能
{: #input}

{{site.data.keyword.speechtotextshort}} サービスでは、音声認識要求をサービスで実行する方法を指定する以下の機能が提供されています。以下のセクションで説明されている入力パラメーターはすべてオプションです。入力音声のみが必須です。
{: shortdesc}

-   サービスのインターフェースごとの単純な音声認識要求の例については、[認識要求の実行](/docs/services/speech-to-text/basic-request.html)を参照してください。
-   使用可能なすべての音声認識パラメーターのアルファベット順リストについては、それらの状況 (一般出荷可能またはベータ) およびサポートされる言語を含めて、[パラメーターの要約](/docs/services/speech-to-text/summary.html)を参照してください。

## カスタム・モデル
{: #custom-input}

言語および音響のモデルのカスタマイズが、さまざまな言語の異なるサポート・レベル (一般出荷可能またはベータ) で使用可能です。詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text/custom.html#languageSupport)を参照してください。
{: note}

すべてのインターフェースで、認識要求でのカスタム・モデルの使用が受け入れられます。

-   *カスタム言語モデル* により、特定の分野の用語を追加してサービスの基本語彙を拡張します。要求で `language_customization_id` パラメーターを使用して、カスタム言語モデルを含めます。オプションの `customization_weight` パラメーターを指定することもできます。このパラメーターによって、基本語彙の単語と対比してカスタム・モデルの単語に付与される相対的な重みを示します。

    詳しくは、[カスタム言語モデルの使用](/docs/services/speech-to-text/language-use.html)を参照してください。
-   *カスタム音響モデル* により、環境や話者の音響特性に対してサービスの基本音響モデルを適応させます。要求で `acoustic_customization_id` パラメーターを使用して、カスタム音響モデルを含めます。カスタム言語モデルとカスタム音響モデルの両方を要求で指定できます。

    詳しくは、[カスタム音響モデルの使用](/docs/services/speech-to-text/acoustic-use.html)を参照してください。

カスタム・モデルは、[言語とモデル](/docs/services/speech-to-text/models.html)で説明されている言語モデルのいずれか 1 つに基づいています。カスタム言語モデルは、そのモデルを作成する際に使用された基本モデルでのみ使用できます。 カスタム・モデルがデフォルトの `en-US_BroadbandModel` 以外のモデルをベースとしている場合、要求でそのモデルの名前も指定する必要があります。 カスタム・モデルを使用するには、カスタム・モデルを所有するサービスのインスタンスに対して作成されたサービス資格情報を使用して要求を発行する必要があります。

カスタマイズの概要については、[カスタマイズ・インターフェース](/docs/services/speech-to-text/custom.html)を参照してください。

### カスタム・モデルの例
{: #customExample}

以下の要求例では、`language_customization_id` パラメーターが含まれており、指定の ID を持つカスタム言語モデルが使用されます。`customization_weight` パラメーターが含まれており、カスタム・モデルの単語に相対重み付けの `0.5` が付与されることを示しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

以下の要求例では、カスタム言語モデルとカスタム音響モデルの両方が使用されます。前者は `language_customization_id` パラメーターで識別され、後者は `acoustic_customization_id` パラメーターで識別されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

サービスのインターフェースごとのカスタム・モデルの使用例については、以下を参照してください。

-   [カスタム言語モデルの使用](/docs/services/speech-to-text/language-use.html)
-   [カスタム音響モデルの使用](/docs/services/speech-to-text/acoustic-use.html)

## 文法
{: #grammars-input}

文法機能はベータ機能です。サービスでは、言語モデルのカスタマイズをサポートしているすべての言語の文法がサポートされます。
{: note}

文法をカスタム言語モデルに追加して、音声認識のために使用できます。文法では正式な言語仕様が使用され、ストリングの書き起こしに一連の作成ルールが定義されます。これらのルールによって、言語の文字体系に基づいて有効なストリングを形成する方法が指定されます。

音声認識に文法を使用する場合、サービスでは、文法によって識別される句のみが識別されます。有効なストリングの検索スペースを制限することで、サービスは通常より高速かつ正確に結果を返すことができます。

すべてのインターフェースで、認識要求の以下のパラメーターが受け入れられます。

-   `language_customization_id` パラメーターによって、文法が定義されるカスタム言語モデルが識別されます。要求は、モデルを所有するサービス・インスタンスのサービス資格情報を使用して行う必要があります。

-   `grammar_name` パラメーターによって、使用する文法が指定されます。1 つの要求で指定できるのは、単一の文法のみです。

詳しくは、[カスタム言語モデルでの文法の使用](/docs/services/speech-to-text/grammar.html)を参照してください。

### 文法の例
{: #grammarsExample}

以下の要求例では、`language_customization_id` パラメーターおよび `grammar_name` パラメーターが含まれており、サービスの応答が、`list-abnf` という名前の文法で定義されるストリングに制限されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

サービスのインターフェースごとの文法の使用例については、[音声認識での文法の使用](/docs/services/speech-to-text/grammar-use.html)を参照してください。

## 基本モデルのバージョン
{: #version}

サービスでは、音声認識の質を向上させるため、[言語とモデル](/docs/services/speech-to-text/models.html)で説明されている基本言語モデルが更新される場合があります。言語の基本モデルは、言語の広帯域モデルと狭帯域モデルのように、相互に独立しています。したがって、基本モデルの更新は、それぞれ独立して行われ、他のモデルには影響しません。

基本モデルに複数のバージョンがある場合、認識要求で使用されるモデルのバージョンは、オプションの `base_model_version` パラメーターによって指定されます。このパラメーターは、主に、新規基本モデルのために更新されるカスタム・モデルで使用することを意図していますが、カスタム・モデル以外でも使用できます。要求で使用される基本モデルのバージョンは、`base_model_version` パラメーターを受け渡すかどうかで決まります。また、要求でカスタム・モデル (言語、音響、または両方) を指定するかどうかでも変わります。

-   *要求でカスタム・モデルを指定しない場合*

    -   `base_model_version` パラメーターを省略して、基本モデルの最新バージョンを使用します。
    -   `base_model_version` パラメーターを指定して、基本モデルの特定のバージョンを使用します。

-   *要求でカスタム・モデルを指定する場合*

    -   `base_model_version` パラメーターを省略して、カスタム・モデルがアップグレードされる基本モデルの最新バージョンを使用します。例えば、カスタム・モデルが基本モデルの最新バージョンのためにアップグレードされる場合、サービスでは、基本モデルとカスタム・モデルの最新バージョンが使用されます。
    -   `base_model_version` パラメーターを指定して、基本モデルとカスタム・モデル両方の指定バージョンを使用します。

このパラメーターは、カスタム・モデルで使用することを意図しています。したがって、基本モデルに基づくカスタム・モデルに関する情報をリストすることによってのみ、基本モデルの使用可能なバージョンについてわかります。

-   カスタム・モデルのアップグレードについて詳しくは、[カスタム・モデルのアップグレード](/docs/services/speech-to-text/custom-upgrade.html)を参照してください。
-   音声認識に基本モデルとカスタム・モデルの別のバージョンを使用する方法について詳しくは、[アップグレードされたカスタム・モデルを使用した認識要求の実行](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition)を参照してください。

## 音声の伝送
{: #transmission}

*WebSocket インターフェースの場合*、音声データは常に、接続を介してサービスにストリーミングされます。ソケットを使用して一度にすべてのデータを渡すか、ライブ使用の場合は使用可能になったデータを順次渡すことができます。サービスからは、使用可能になった順に結果が返されます。

*HTTP インターフェースの場合*、以下の方法のいずれかで音声をサービスに伝送できます。

-   *1 回限りの送信* - `Transfer-Encoding` ヘッダーを省略して、すべての音声データをサービスに単一の送信として一度に渡します。
-   *ストリーミング* - `Transfer-Encoding` 要求ヘッダーを値 `chunked` に設定して、持続接続を介してデータをストリーミングします。サービスへのデータのストリーミングを開始する前に、データが完全に揃っている必要はありません。使用可能になった順にデータをストリーミングできます。サービスは、最終チャンクを受信した、つまり、最終を示す空のチャンクが送信されてきた場合にのみ、結果を送信します。

    `Transfer-Encoding` ヘッダーを使用してチャンク形式の音声をストリーミングする方法について詳しくは、以下を参照してください。
    -   [en.wikipedia.org/wiki/Chunked_transfer_encoding ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}
    -   *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing* の [Transfer Codings ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://tools.ietf.org/html/rfc7230#section-4){: new_window}

HTTP インターフェースの場合、サービスでは常に、結果の送信前に音声ストリーム全体が書き起こされます。結果には、休止で区切られた句を示す複数の `transcript` 要素が含まれることがあります。`transcript` 要素を連結して、完全な書き起こしを組み立ててください。

サービスでは、ストリーミング・セッションのタイムアウトが適用されます。長時間の無音を検出した場合、または 30 秒間音声を受信しなかった場合、ストリーミング・セッションが終了されることがあります。タイムアウトおよびその回避方法について詳しくは、[タイムアウト](#timeouts)を参照してください。

### 音声の伝送の例
{: #transmissionExample}

以下の要求例では、`Transfer-Encoding` ヘッダーに `chunked` が指定されており、ストリーミング・モードが使用されます。接続はオープンのままで、音声の追加チャンクが受け入れられます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## タイムアウト
{: #timeouts}

HTTP 音声認識メソッドまたは WebSocket 音声認識メソッドでストリーミング・セッションを開始すると、サービスでは、非アクティブ・タイムアウトおよびセッション・タイムアウトが適用されます。ストリーミング・セッション中にタイムアウトが経過した場合、サービスによって接続がクローズされます。クローズされたと考えられる接続は、アプリケーションで正常に回復する必要があります。

HTTP を介して音声をストリーミングしている場合、20 秒ごとにサービスから応答でスペース文字が送信されます。サービスでこれを行うことにより、30 秒の HTTP REST 非アクティブ・タイムアウトが回避されてユーザビリティーが向上します。認識が進行している間中接続を保持するために、サービスでは、書き起こしが完了するまでこのスペース文字の送信が続けられます。スペース文字は、JSON エンコードされた応答データには影響しません。

この HTTP 非アクティブ・タイムアウトは、サービスの非アクティブ・タイムアウトとは異なります。WebSocket インターフェースでは、この HTTP タイムアウトは適用されません。
{: note}

### 非アクティブ・タイムアウト
{: #timeouts-inactivity}

*非アクティブ・タイムアウト* (HTTP ステータス・コード 400) が発生するのは、サービスで音声を受信しているが、連続した無音または非音声アクティビティー (発話なし) が 30 秒間検出された場合です。サービスはエラー・メッセージ `No speech detected for 30s` を送信します。この非アクティブ・タイムアウトは、例えばユーザーが単にライブ・マイクから離れた場合にセッションを終了する際に役立ちます。

デフォルトの非アクティブ・タイムアウトは 30 秒です。この値は、`inactivity_timeout` パラメーターを使用して指定変更できます。非アクティブ・タイムアウトを増加するには、より大きな値を指定します。非アクティブ・タイムアウトを無期限に設定するには、値 `-1` を指定します。 サービスに送信する音声は、無音も含めてすべて課金されるので、非アクティブ・タイムアウトを増加すると、無音のみを送信するストリーミング・セッションのために追加料金がかかる場合があります。

以下の要求例では、非アクティブ・タイムアウトを 60 秒に設定しています。要求では、初期ファイルが送信されて、ストリーミング・セッションが開始されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### セッション・タイムアウト
{: #timeouts-session}

*セッション・タイムアウト* (HTTP ステータス・コード 408) が発生するのは、ストリーミング・セッションをアクティブに保つのに十分な音声を送信できなかった場合です。以下の理由により、サービスでは、セッションがアイドル状態であると見なされ、セッション・タイムアウトがトリガーされる場合があります。

-   30 秒間のうち 15 秒以上の音声をサービスに送信できなかった。

    最終チャンクを送信してストリームの終了を示すまでは、30 秒間のうち 15 秒以上音声を送信する必要があります。`inactivity_timeout` パラメーターがより大きい値または `-1` に設定されている場合は、音声は無音でもかまいません。サービスに送信するすべての音声の所要時間には、無音も含めて課金されます。
-   リアルタイムよりも遅い速度で音声をストリーミングした。

    理想としては、書き起こす音声を取得する直前にセッションを確立する要求を開始します。その後、リアルタイムに近い速度で音声を送信して、セッションを維持します。

最終チャンクを送信してストリームの終了を示した後は、セッション・タイムアウトを気にする必要はありません。サービスは、書き起こしの最終結果を返すまで、音声の処理を継続します。

長い音声ストリームの書き起こし時に、サービスで音声を処理して応答を生成するのに 30 秒を超える時間がかかることがあります。サービスでは、受信したすべての音声の処理を終了するまでは、セッション・タイムアウトを計算し始めません。サービスの処理時間が原因となって、セッションで 30 秒のセッション・タイムアウトを超えることはありません。

例えば、セッションの最初の 10 秒に 1 時間の音声を送信した場合、サービスが音声を処理するのに 300 秒かかるとします。このセッションを保持するには、340 秒以内にセッションに無音を含む何らかの音声を 15 秒以上送信する必要があります。

この例で、セッションの 100 秒のマークで別の 15 秒の音声を送信した場合、サービスがこの音声を処理するのに追加で 2 秒かかるとします。この場合、342 秒以内にセッションに音声を 15 秒以上送信する必要があります。

ストリーミング・セッションがアイドル状態かどうかを判別するために、処理時間に依存したり、結果を受信したかどうかに依存したりしないでください。サービスではすべての音声が即時に処理されると想定して、それに応じてサービスにデータを送信してください。リアルタイムで音声をストリーミングする場合、音声の送信が 30 秒間のうちリアルタイムの半分 (15 秒の音声) より遅れないようにしてください。通常、ネットワーク待ち時間や遅延に適応するには、この速度で十分です。
{: important}

## 要求ロギング
{: #logging}

デフォルトで、{{site.data.keyword.IBM_notm}} では、{{site.data.keyword.watson}} サービスへのすべての要求およびそれらの結果をログに記録します。ロギングは、今後のユーザー・サービスの向上の目的でのみ行われます。ログ・データは共有されることも、公開されることもありません。

ユーザーの個人情報のプライバシー保護について懸念がある場合や、IBM によって要求がログに記録されることを望まない場合は、IBM にログ・データを取らせないことを選択できます (オプトアウト)。アカウント・レベルまたは API 要求レベルのいずれかでロギングのオプトアウトを選択できます。詳しくは、[{{site.data.keyword.watson}} サービスの要求ロギングの制御](/docs/services/watson/getting-started-logging.html)を参照してください。

## 機密保護
{: #security-input}

機密保護には、要求でサービスに渡されるデータと顧客 ID を関連付ける機能が含まれます。要求で `X-Watson-Metadata` ヘッダーを渡すことによって、データと顧客 ID を関連付けます。必要に応じて、後で `DELETE /v1/user_data` メソッドを使用してデータを削除できます。詳しくは、[機密保護](/docs/services/speech-to-text/information-security.html)を参照してください。
