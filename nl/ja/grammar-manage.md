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

# 文法の管理
{: #manageGrammars}

カスタマイズ・インターフェースには、カスタム言語モデルに文法を追加するための `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドが含まれています。詳しくは、[カスタム言語モデルへの文法の追加](/docs/services/speech-to-text/grammar-add.html#addGrammar)を参照してください。このインターフェースには、カスタム言語モデルの文法をリストおよび削除するための以下のメソッドも含まれています。
{: shortdesc}

## カスタム言語モデルの文法のリスト
{: #listGrammars}

カスタマイズ・インターフェースでは、カスタム言語モデルの文法に関する情報をリストするための次の 2 つのメソッドが用意されています。

-   `GET /v1/customizations/{customization_id}/grammars` メソッドは、カスタム・モデルのすべての文法に関する情報をリストします。
-   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドは、カスタム・モデルの指定された文法に関する情報をリストします。

どちらのメソッドも、文法に関する同じ情報を返します。この情報には、その文法の `name` と、その文法によって認識される `out-of_vocabulary_words` の数が含まれています。この応答には、その文法の `status` も含まれています。この値は、その文法をカスタム・モデルに追加したときに、サービスによるその文法の分析を確認するために重要であり、以下のいずれかです。

-   `being_processed` は、`POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 要求に応えるために、サービスによる文法の処理がまだ進行中であることを意味します。
-   `analyzed` は、サービスによって文法が正常に処理されて、カスタム・モデルに追加されたことを意味します。
-   `undetermined` は、文法の処理中にサービスでエラーが発生したことを意味します。文法について返される情報には、エラーを修正するための指針を示すエラー・メッセージが含まれています。

    例えば、文法が無効である可能性や、既存の文法と同じ名前の文法を追加しようとした可能性があります。要求で `allow_overwrite` パラメーターを指定して、文法の追加を再試行できます。文法を削除してから、文法の追加を再試行することもできます。

### 要求と応答の例
{: #listExample-grammars}

次の例では、指定されたカスタマイズ ID を持つカスタム・モデルに追加されたすべての文法に関する情報をリストしています。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

3 つの文法 (`confirm-xml`、`confirm-abnf`、および `list-abnf`) がカスタム・モデルに正常に追加されました。

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

次の例では、指定された文法 `list-abnf` に関する情報を示しています。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## カスタム言語モデルからの文法の削除
{: #deleteGrammar}

`DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドを使用して、カスタム言語モデルから既存の文法を削除します。このメソッドによって文法が削除されると、その文法に関連付けられているすべての OOV 語がカスタム・モデルの単語リソースから削除されます。ただし、以下の場合は例外です。

-   その単語が別の文法によってまたはコーパスによって追加された場合。
-   単語が `POST /v1/customizations/{customization_id}/words` メソッドまたは `PUT /v1/customizations/{customization_id}/words/{word_name}` メソッドによって何らかの形で変更されている場合。

`POST /v1/customizations/{customization_id}/train` メソッドを使用して、更新後のデータに基づいてカスタム・モデルをトレーニングするまでは、文法を削除してもそのモデルに影響はありません。文法に基づいてモデルを正常にトレーニングできた場合は、モデルをリトレーニングするまでは、その文法を音声認識用に使用し続けることができます。

### 要求例
{: #deleteExample-grammar}

次の例では、指定したカスタマイズ ID を持つカスタム・モデルから `list-abnf` という名前の文法を削除します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
