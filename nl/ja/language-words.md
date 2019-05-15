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

# カスタム単語の管理
{: #manageWords}

カスタマイズ・インターフェースには、カスタム・モデルの単語を追加または変更する場合に使用する `POST /v1/customizations/{customization_id}/words` メソッドおよび `PUT /v1/customizations/{customization_id}/words/{word_name}` メソッドが含まれています。 詳しくは、[カスタム言語モデルへの単語の追加](/docs/services/speech-to-text/language-create.html#addWords)を参照してください。 このインターフェースには、カスタム言語モデルの単語をリストおよび削除する以下のメソッドも含まれています。
{: shortdesc}

通常、ほとんどのカスタム単語をコーパスから追加します。 コーパスのテキスト・ファイルの文字エンコードを確認してください。 サービスはテキスト・ファイルで検出したエンコードを保持します。 カスタム言語モデルで個別の単語を処理する場合は、このエンコードを使用する必要があります。 `GET` メソッド、`PUT` メソッド、または `DELETE /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して単語を指定する際は、単語に非 ASCII 文字が含まれている場合、URL で渡す `word_name` を URL エンコードする必要があります。 詳しくは、[文字エンコード](/docs/services/speech-to-text/language-resource.html#charEncoding)を参照してください。
{: important}

## カスタム言語モデルの単語のリスト
{: #listWords}

カスタマイズ・インターフェースには、カスタム言語モデルの単語をリストするメソッドが 2 つあります。

-   `GET /v1/customizations/{customization_id}/words` メソッドは、カスタム・モデルの単語リソースに含まれる単語に関する情報をリストします。 このメソッドには、2 つのオプションのクエリー・パラメーターが含まれています。
    -   `word_type` パラメーターはリストされる単語を指定します。

        -   `all` (デフォルト) は、すべての単語を表示します。
        -   `user` は、ユーザーによって追加または変更されたカスタム単語のみを表示します。
        -   `corpora` は、コーパスから抽出された OOV 語のみを表示します。
        -   `grammars` は、文法から抽出された OOV 語のみを表示します。
    -   `sort` パラメーターは、単語の順序付け方法を示します。 このパラメーターは、単語のソート方法を示す 2 つの引数 `alphabetical` および `count` を受け入れます。 オプションの `+` または `-` を引数の前に追加して、結果を昇順または降順のいずれでソートするかを示すことができます。 デフォルトでは、このメソッドによって単語が昇順のアルファベット順で表示されます。
-   `GET /v1/customizations/{customization_id}/words/{word_name}` メソッドは、モデルの単語リソースに含まれる単語のうち、指定された 1 つの単語に関する情報をリストします。

単語を識別する `word` フィールド以外に、両方のメソッドは各単語に関する以下の情報を返します。

-   `sounds_like` フィールド: 単語の発音の配列を表示します。 単語に同音異字発音が指定されていない場合、この配列には、サービスによって生成された同音異字発音が自動的に取り込まれます。 詳しくは、[sounds_like フィールドの使用](/docs/services/speech-to-text/language-resource.html#soundsLike)を参照してください。
-   `display_as` フィールド: サービスが書き起こしで表示するカスタム単語のつづりを表示します。 単語に表示形式の値が指定されていなければ、このフィールドには空のストリングが含まれます。その場合、単語はつづり通りに表示されます。 詳しくは、[display_as フィールドの使用](/docs/services/speech-to-text/language-resource.html#displayAs)を参照してください。
-   `source` フィールド: 単語がカスタム・モデルの単語リソースに追加された方法を示します。 このフィールドには、サービスがその単語を抽出した各コーパスおよび文法の名前が格納されます。 単語がユーザーによって変更または追加された場合、このフィールドにはストリング `user` が格納されます。
-   `count` フィールド: すべてのコーパスおよび文法でその単語が見つかった回数が示されます。 例えば、単語が 1 つのコーパスで 5 回、別のコーパスで 7 回見つかった場合そのカウントは `12` になります。

    カスタム単語がコーパスまたは文法によって追加される前に、モデルにその単語を追加した場合、カウントは `1` から開始されます。最初にコーパスまたは文法から単語が追加され、後で変更した場合、カウントにはその単語がコーパスまたは文法で見つかった回数のみが反映されます。

    `count` フィールドの導入前に作成されたカスタム・モデルの場合、このフィールドは常に `0` のままとなります。このようなモデルのこのフィールドを更新するには、モデルのコーパスおよび文法を再度追加し、要求に `allow_overwrite` パラメーターを含めます。
    {: note}

サービスがカスタム単語の定義で 1 つ以上の問題を検出した場合、出力には `error` フィールドが含まれます。 このフィールドには、定義の各問題要素がリストされた配列と問題を説明するメッセージが表示されます。

エラーが発生するのは、例えば、発音を追加する際のルールのいずれかに違反する間違った `sounds_like` フィールドを指定してカスタム単語を追加した場合です。単語リソースに誤りのある単語が含まれるカスタム・モデルをトレーニングすることはできません。モデルをトレーニングするには、その前に、そのような単語を修正または削除する必要があります。

### 要求と応答の例
{: #listExample-words}

以下に、指定のカスタマイズ ID を持つカスタム・モデルのすべての単語をタイプに関わらずリストする例を示します。 単語はデフォルトのソート順 (アルファベットの昇順) で表示されます。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

このモデルの単語リソースには 4 つの単語が含まれています。 最初の単語はユーザーによって直接追加されたものですが、`sounds_like` フィールドにエラーがあります。このフィールドに数字を含めることはできません。 その他の単語はユーザーまたはユーザーとコーパスの両方によって追加されました。

```javascript
{
  "words": [
      {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

以下に、指定のモデルの単語リソースに含まれる単語 `NCAA` に関する情報を表示する例を示します。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

ユーザーが最初に単語を追加しました。 その後、サービスが `corpus3` でその単語を 2 回見つけました。

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## カスタム言語モデルからの単語の削除
{: #deleteWord}

カスタム言語モデルから単語を削除するには、`DELETE /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用します。 このメソッドを使用して、例えば、欠陥のあるデータが含まれるコーパスから、誤って追加された単語を削除します。

その単語をカスタム・モデルの単語リソースに追加した方法に関わらず、任意の単語をカスタム・モデルから削除できます。 ただし、サービスの基本語彙に含まれる単語を削除することはできません。 そのような単語をカスタム・モデルから削除すると、その単語のカスタム発音のみが削除されます。基本語彙では、その単語は維持されます。

カスタム・モデルから単語を削除しても、`POST /v1/customizations/{customization_id}/train` メソッドを使用してモデルをリトレーニングするまでは、そのモデルに影響はありません。 削除された単語に関してモデルが以前にトレーニングされている場合、その単語を単語リソースから削除しても、モデルは引き続きその単語を音声認識に適用します。 削除を反映するには、モデルをリトレーニングする必要があります。

### 要求例
{: #deleteExample-word}

以下に、指定のカスタマイズ ID を持つカスタム・モデルから `IEEE` という単語を削除する例を示します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
