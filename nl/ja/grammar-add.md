---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# カスタム言語モデルへの文法の追加
{: #grammarAdd}

文法を音声認識用に使用するには、まずカスタマイズ・インターフェースを使用してその文法をカスタム言語モデルに追加する必要があります。 文法をカスタム言語モデルに追加する手順は、コーパスやカスタム単語を追加する手順と同様です。
{: shortdesc}

1.  [カスタム言語モデルを作成します](#createModel-grammar)。 新しいカスタム・モデルを作成することも、既存のモデルを使用することもできます。
1.  [カスタム言語モデルに文法を追加します](#addGrammar)。 サービスによって、この文法が正しいかどうかが検証されます。
1.  [文法内の単語を検証します](#validateGrammar)。 この文法によって認識される任意の未知 (OOV) 語の同音異字発音が正しいかどうかを確認します。
1.  [カスタム言語モデルをトレーニングします](#trainGrammar)。 サービスによって、カスタム・モデルと文法が音声認識で使用可能になるように準備されます。
1.  これで、このカスタム・モデルと文法を音声認識要求で使用できるようになりました。 詳しくは、[音声認識での文法の使用](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)を参照してください。

上記の手順は反復的なものです。 文法、コーパス、およびカスタム単語をカスタム言語モデルに必要な回数だけ追加できます。 新たに追加したデータ・リソース (文法、コーパス、またはカスタム単語) に基づいて、カスタム・モデルをトレーニングする必要があります。 カスタム・モデルを音声認識用に使用する際に、そのカスタム・モデルが前回にトレーニングされたときに使用されたデータがそのカスタム・モデルに反映されます。

文法機能はベータ機能です。 言語モデルのカスタマイズをサポートしている任意の言語で、文法を使用できます。 言語モデル・カスタマイズは、ほとんどの言語で有効です。 詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)を参照してください。
{: note}

## カスタム言語モデルの作成
{: #createModel-grammar}

文法を音声認識で使用するには、その文法をカスタム言語モデルに追加する必要があります。 既存のカスタム言語モデルを使用することも、`POST /v1/customizations` メソッドを使用して新しいカスタム・モデルを作成することもできます。 新しいカスタム・モデルの作成については、[カスタム言語モデルの作成](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)を参照してください。

カスタム言語モデルには、コーパス、カスタム単語、および文法を含めることができます。 音声認識時に、そのカスタム・モデルを文法とともに使用することも、文法なしで使用することもできます。 ただし、文法を使用する際は、指定された文法内の単語のみがサービスで認識されます。

## カスタム言語モデルへの文法の追加
{: #addGrammar}

`POST /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドを使用して、文法ファイルをカスタム・モデルに追加します。

-   `customization_id` パス・パラメーターを使用して、カスタム言語モデルのカスタマイズ ID を指定します。
-   `grammar_name` パス・パラメーターを使用して、文法の名前を指定します。 カスタム・モデルと同じ言語で表現した名前を使用して、この名前にはその文法の内容を反映させてください。
    -   名前の最大文字数は 128 文字です。
    -   URL エンコードする必要のある文字は使用しないでください。例えば、スペース、スラッシュ、円記号、コロン、アンパーサンド、二重引用符、正符号、等号、疑問符などは、名前に使用しないでください。(サービスはこれらの文字の使用を妨げません。しかし、これらの文字は、使用されるところでは必ず URL エンコードされなければならないため、使用しないことを強くお勧めします。)
    -   カスタム・モデルに既に追加されている文法やコーパスの名前を使用しないでください。
    -   名前 `user` を使用しないでください。これは、ユーザーによって追加または変更されるカスタム単語を表すためにサービスによって予約されています。
    -   `base_lm` および `default_lm` という名前は使用しないでください。いずれの名前も、サービスで将来使用するために予約されています。
-   `Content-Type` 要求ヘッダーを使用して文法のフォーマットを指定します。
    -   ABNF 文法の場合は `application/srgs`
    -   XML 文法の場合は `application/srgs+xml`
-   その文法が含まれたファイルを要求の本体として渡します。

次の例では、`confirm.abnf` という名前の文法ファイルを指定された ID を持つカスタム・モデルに追加します。 この例では、文法に `confirm-abnf` という名前を付けています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

このメソッドは、同じ名前の既存の文法を上書きするために指定できるオプションの `allow_overwrite` クエリー・パラメーターも受け付けます。 文法をモデルに追加した後にその文法を更新する必要がある場合は、このパラメーターを使用してください。

このメソッドは非同期です。 文法のサイズとサービスに対する現在の負荷によっては、サービスで文法を分析するのに数秒間かかることがあります。 文法のステータスの確認については、[文法追加要求のモニター](#monitorGrammar)を参照してください。

このメソッドを文法ファイルごとに 1 回呼び出すことで、任意の数の文法を同一カスタム・モデルに追加できます。 文法の追加が完全に完了してから、次の文法を追加する必要があります。

### 文法追加要求のモニター
{: #monitorGrammar}

文法が有効な場合、サービスは 201 応答コードを返します。 その後、サービスはその文法の内容を非同期的に処理して、その文法によって認識できる新しい単語を自動的に抽出します。 現在の要求についてサービスによる文法の分析が完了するまでは、カスタム・モデルにデータ・リソースを追加する要求や、モデルをトレーニングする要求を送信することはできません。

分析のステータスを確認するには、`GET /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドを使用して文法のステータスをポーリングします。 このメソッドには、カスタム・モデルの ID と文法の名前を渡すことができます。 次の例では、`confirm-abnf` という名前の文法のステータスを確認しています。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

応答には文法のステータスが含まれています。

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

`status` フィールドには、以下のいずれかの値が設定されます。

-   `analyzed` は、サービスによる文法の分析が正常に完了したことを意味します。
-   `being_processed` は、サービスによる文法の分析がまだ進行中であることを意味します。
-   `undetermined` は、文法の処理中にサービスでエラーが発生したことを意味します。

ステータスが `analyzed` になるまで、ループを使用して 10 秒間隔で文法のステータスを確認します。 文法のステータスの確認について詳しくは、[カスタム言語モデルの文法のリスト](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars)を参照してください。

### 文法の追加失敗の処理
{: #handleFailures}

サービスによる文法の分析に失敗した場合は、文法のステータスが `undetermined` に設定されて、その失敗を説明する `error` フィールドが文法のステータスに付加されます。 `GET /v1/customizations/{customization_id}` メソッドを使用して、カスタム・モデルのステータスを確認することもできます。 文法の追加に失敗した場合は、出力には次のようなエラー・メッセージが含まれます。

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

カスタム・モデルのステータスはこのエラーによる影響を受けませんが、この文法をそのモデルで使用することはできません。 この問題に対処するには、文法ファイルを修正して、同じ要求を再実行して文法ファイルをカスタム・モデルに追加します。 `allow_overwrite` パラメーターを `true` に設定して、追加できなかった文法ファイルのバージョンを上書きします。

また、文法をカスタム・モデルから一時的に削除してから、文法ファイルを修正して将来に再び追加することもできます。 文法の削除について詳しくは、[カスタム言語モデルからの文法の削除](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#deleteGrammar)を参照してください。

## 文法内の単語の検証
{: #validateGrammar}

文法ファイルをカスタム言語モデルに追加すると、サービスによってその文法が解析されて、サービスの基本語彙にまだ含まれていない単語がその文法で認識されるかどうかが確認されます。 このような単語は、未知 (OOV) 語と呼ばれます。 OOV 語はそのカスタム・モデルの単語リソースに追加されます。 単語リソースの目的は、サービスの語彙にまだ含まれていない単語を定義することです。

単語リソース内の定義を通じて、OOV 語の書き起こし方法がサービスに通知されます。 この定義情報に含まれるのは、その単語の発音方法をサービスに通知する `sounds_like` フィールド、その単語の表示方法をサービスに通知する `display_as` フィールド、その単語がどのようにカスタム・モデルに追加されたのかを示す `source` フィールドです。 単語リソースと OOV 語について詳しくは、[単語リソース](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResource)を参照してください。

文法をカスタム・モデルに追加した後に、そのモデルの単語リソースに含まれている OOV 語を調べて、それらの語の同音異字発音を確認することをお勧めします。 すべての文法に OOV 語が存在するわけではありませんが、通常は単語リソースを確認することが望ましいです。 文法を追加した後にカスタム・モデルの単語を確認するには、次のメソッドを使用します。

-   `GET /v1/customizations/{customization_id}/words` メソッドを使用して、カスタム・モデル内のすべての単語をリストします。 このメソッドの `word_type` パラメーターで `grammars` という値を指定することで、文法から追加された単語のみをリストします。
-   `GET /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して、モデル内の個別の単語を表示します。

単語の同音異字発音が正確で正しいことを確認してください。 また、これらの単語自体に誤字などの誤りがないかどうかも調べてください。 カスタム・モデル内の単語の検証と修正について詳しくは、[単語リソースの検証](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)を参照してください。

## カスタム言語モデルのトレーニング
{: #trainGrammar}

カスタム言語モデルで文法を使用可能にするための最後のステップは、そのモデルをトレーニングすることです。 このプロセスは、新しいコーパスや新しいカスタム単語に基づいてカスタム・モデルをトレーニングするプロセスと同じです。 モデルをトレーニングすると、文法がコンパイルされて音声認識時に使用できるようになります。 サービスによって、文法が元のテキスト・ベース・フォーマットから音声認識用のバイナリー・ランタイム・フォーマットに処理されます。 モデルをトレーニングするまで、文法を使用することはできません。

カスタム・モデルをトレーニングするには、`POST /v1/customizations/{customization_id}/train` メソッドを使用します。 以下の例に示すように、トレーニングするモデルのカスタマイズ ID をこのメソッドに渡します。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

トレーニング・メソッドは非同期です。 トレーニングが完了するのに、一般に数秒しかかかりません。 ただし、文法の複雑さとサービスに対する現在の負荷によっては、これより長い時間がかかることがあります。 トレーニング操作のステータスの確認については、[トレーニング要求のモニター](#monitorTraining-grammar)を参照してください。

### トレーニング要求のモニター
{: #monitorTraining-grammar}

トレーニング・プロセスが正常に開始されると、サービスは 200 応答コードを返します。 現在のトレーニング要求が完了するまで、サービスは後続のトレーニング要求を受け付けることはできず、新しい文法、コーパス、または単語をカスタム・モデルに追加する要求を受け付けることもできません。

トレーニング要求のステータスを判別するには、`GET /v1/customizations/{customization_id}` メソッドを使用して、モデルのステータスをポーリングします。 このメソッドはモデルのカスタマイズ ID を受け取って、モデルに関する次のような情報を返します。

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

モデルのトレーニング中は、`status` フィールドの値は `training` になります。 ステータスが `available` になるまで、ループを使用して 10 秒間隔でモデルのステータスを確認します。 トレーニング要求および他のステータス値のモニタリングについて詳しくは、[モデルのトレーニング要求のモニタリング](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language)を参照してください。
