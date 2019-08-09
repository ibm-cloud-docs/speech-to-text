---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# コーパスの管理
{: #manageCorpora}

カスタマイズ・インターフェースには、カスタム言語モデルにコーパスを追加するための `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドが含まれます。 詳しくは、[カスタム言語モデルへのコーパスの追加](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)を参照してください。 インターフェースには、カスタム言語モデルのコーパスをリストおよび削除するために、以下のようなメソッドも含まれています。
{: shortdesc}

## カスタム言語モデルのコーパスのリスト
{: #listCorpora}

カスタマイズ・インターフェースには、カスタム言語モデルのコーパスに関する情報をリストするメソッドが 2 つあります。

-   `GET /v1/customizations/{customization_id}/corpora` メソッドは、カスタム・モデルのすべてのコーパスに関する情報をリストします。
-   `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドは、カスタム・モデルの指定のコーパスに関する情報をリストします。

両方のメソッドは、コーパスの `name`、コーパスから読み取られた `total_words`、コーパスから抽出された `out-of-vocabulary_words` の数を返します。 また、これらのメソッドは、コーパスの `status` もリストします。 ステータスは、カスタム・モデルへの追加要求の応答として、サービスでのコーパスの分析状況を確認するために重要です。

-   `analyzed` は、サービスによるコーパスの分析が正常に完了したことを意味します。 コーパスのデータを使用してカスタム・モデルをトレーニングできます。
-   `being_processed` は、サービスによるコーパスの分析がまだ進行中であることを意味します。 分析が完了するまで、サービスはコーパスまたは単語の追加要求や、カスタム・モデルのトレーニング要求を受け入れることができません。
-   `undetermined` は、コーパスの処理中にサービスでエラーが発生したことを意味します。 コーパスに関して返される情報には、エラー修正のガイダンスを示すエラー・メッセージが含まれます。

    例えば、コーパスが無効である、または既存のコーパスと同じ名前のコーパスを追加しようとしたなどです。 コーパスの追加を再試行でき、その場合は、要求で `allow_overwrite` パラメーターを含めます。 また、既存のコーパスを削除してから再度コーパスの追加を試みることもできます。

### 要求と応答の例
{: #listExample-corpora}

以下に、指定のカスタマイズ ID を持つカスタム・モデルのすべてのコーパスをリストする例を示します。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

カスタム・モデルに 3 つのコーパスが追加されました。 サービスで `corpus1` は正常に分析されました。 `corpus2` は分析が進行中で、`corpus3` の分析は失敗しました。

```javascript
{
  "corpora": [
      {
      "name": "corpus1",
         "total_words": 5037,
         "out_of_vocabulary_words": 401,
         "status": "analyzed"
    },
    {
      "name": "corpus2",
         "total_words": 0,
         "out_of_vocabulary_words": 0,
         "status": "being_processed"
    },
    {
      "name": "corpus3",
         "total_words": 0,
         "out_of_vocabulary_words": 0,
         "status": "undetermined",
         "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by
setting the 'allow_overwrite' flag to 'true'."
      }
  ]
}
```
{: codeblock}

以下の例では、指定のカスタマイズ ID を持つカスタム・モデルの `corpus1` という名前のコーパスに関する情報が返されます。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
         "total_words": 5037,
         "out_of_vocabulary_words": 401,
         "status": "analyzed"
}
```
{: codeblock}

## カスタム言語モデルからのコーパスの削除
{: #deleteCorpus}

カスタム言語モデルから既存のコーパスを削除するには、`DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドを使用します。 このメソッドによってコーパスが削除されると、そのコーパスに関連付けられているすべての OOV 語がカスタム・モデルの単語リソースから削除されます。ただし、以下の場合は例外です。

-   単語が別のコーパスまたは文法からも追加されている場合。
-   単語が `POST /v1/customizations/{customization_id}/words` メソッドまたは `PUT /v1/customizations/{customization_id}/words/{word_name}` メソッドによって何らかの形で変更されている場合。

コーパスを削除しても、`POST /v1/customizations/{customization_id}/train` メソッドを使用して、更新後のデータに関してカスタム・モデルをトレーニングするまでは、そのモデルに影響はありません。 該当するコーパスに関してモデルが正常にトレーニングされている場合、モデルをリトレーニングするまでは、そのコーパスの単語はモデルの語彙に残り、音声認識に適用されます。

### 要求例
{: #deleteExample-corpus}

以下の例では、指定のカスタマイズ ID を持つカスタム・モデルから `corpus3` という名前のコーパスが削除されます。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
