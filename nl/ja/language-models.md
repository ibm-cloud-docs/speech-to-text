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

# カスタム言語モデルの管理
{: #manageLanguageModels}

カスタマイズ・インターフェースには、カスタム言語モデルを作成するための `POST /v1/customizations` メソッドが含まれます。このインターフェースには、単語リソースの最新データに関してカスタム・モデルをトレーニングするための `POST /v1/customizations/train` メソッドも含まれます。詳しくは、以下の資料を参照してください。
{: shortdesc}

-   [カスタム言語モデルの作成](/docs/services/speech-to-text/language-create.html#createModel-language)
-   [カスタム言語モデルのトレーニング](/docs/services/speech-to-text/language-create.html#trainModel-language)

また、このインターフェースには、以下のような、カスタム言語モデルに関する情報をリストするメソッド、カスタム・モデルを初期状態にリセットするメソッド、カスタム・モデルを削除するメソッドが含まれます。

## カスタム言語モデルのリスト
{: #listModels-language}

カスタマイズ・インターフェースには、指定のサービス資格情報により所有されるカスタム言語モデルに関する情報をリストするメソッドが 2 つあります。

-   `GET /v1/customizations` メソッドは、すべてのカスタム言語モデルに関する情報、または指定の言語のすべてのカスタム言語モデルに関する情報をリストします。
-   `GET /v1/customizations/{customization_id}` メソッドは、指定のカスタム言語モデルに関する情報をリストします。 このメソッドを使用してサービスをポーリングし、トレーニング要求のステータスまたは新規単語の追加要求のステータスを調べます。

どちらのメソッドも、カスタム・モデルに関する以下の情報を返します。

-   `customization_id` は、カスタム・モデルの GUID (Globally Unique Identifier) を識別します。GUID は、インターフェースのメソッドでモデルを識別するために使用されます。
-   `created` は、カスタム・モデルが作成された日時 (協定世界時、UTC) です。
-   `language` は、カスタム・モデルの言語です。
-   `dialect` は、カスタム言語モデルの言語の方言です。
-   `owner` は、カスタム・モデルを所有するサービス・インスタンスの資格情報を識別します。
-   `name` は、カスタム・モデルの名前です。
-   `description` は、カスタム・モデルの説明を示します (モデルの作成時に指定されている場合)。
-   `base_model` は、カスタム言語モデルを作成する際に使用された言語モデルの名前を意味します。
-   `versions` は、カスタム・モデルの使用可能なバージョンのリストを示します。配列の各要素は、カスタム・モデルで使用できる基本モデルのバージョンを意味します。カスタム・モデルをアップグレードしている場合にのみ複数のバージョンが存在します。それ以外の場合は、1 つのバージョンのみが表示されます。詳しくは、[カスタム・モデルのバージョン情報の表示](/docs/services/speech-to-text/custom-upgrade.html#upgradeList)を参照してください。

メソッドは、カスタム・モデルの状態を示す `status` フィールドも返します。

-   `pending` は、モデルが作成されたことを意味します。モデルは、トレーニング・データの追加、またはサービスによる追加データの分析の完了を待機しています。
-   `ready` は、モデルにデータが取り込まれ、モデルのトレーニングを開始できる状態になっていることを意味します。
-   `training` は、モデルがデータに関して、トレーニング中であることを意味します。
-   `available` は、モデルのトレーニングが完了し、モデルが認識要求で使用可能な状態になっていることを意味します。
-   `upgrading` は、モデルがアップグレード中であることを意味します。
-   `failed` は、モデルのトレーニングが失敗したことを意味します。 モデルの単語リソースに含まれる単語を調べて、モデルのトレーニングの妨げとなった誤りを判別してください。

さらに、出力には `progress` フィールドも含まれます。このフィールドは、カスタム・モデルのトレーニングの現在の進行状況を示します。 `POST /v1/customizations/{customization_id}/train` メソッドを使用してモデルのトレーニングを開始した場合、このフィールドはその要求の現在の進行状況を完了率 (パーセンテージ) として示します。この時点で、ステータスが `available` の場合、フィールドの値は `100` であり、それ以外の場合は、`0` です。

### 要求と応答の例
{: #listExample-language}

以下の例では、`language` クエリー・パラメーターが含まれており、サービス資格情報により所有される米国英語のすべてのカスタム言語モデルがリストされます。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

サービス資格情報により、以下の 2 つのモデルが所有されています。1 番目のモデルは、データを待機中であるか、サービスによって処理中です。2 番目のモデルは、トレーニングが完了しており使用できる状態です。

```javascript
{
  "customizations": [
      {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model",
      "description": "Example custom language model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

以下の例は、指定のカスタマイズ ID を持つカスタム・モデルに関する情報を返します。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## カスタム言語モデルのリセット
{: #resetModel-language}

カスタム・モデルをリセットするには、`POST /v1/customizations/{customization_id}/reset` メソッドを使用します。 モデルをリセットすると、モデルからすべてのコーパスと単語が削除され、モデルが作成時の状態に初期化されます。 このメソッドによって、モデル自体またはモデルの名前や言語などのメタデータが削除されることはありません。 ただし、モデルをリセットすると、モデルの単語リソースが空になるため、コーパスや単語を追加して再作成する必要があります。

### 要求例
{: #resetExample-language}

以下に、指定のカスタマイズ ID を持つカスタム・モデルをリセットする例を示します。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## カスタム言語モデルの削除
{: #deleteModel-language}

不要になったカスタム言語モデルを削除するには、`DELETE /v1/customizations/{customization_id}` メソッドを使用します。 このメソッドは、カスタム・モデルに関連付けられているすべてのコーパスと単語、およびそのカスタム・モデル自体を削除します。カスタム・モデルを削除した後は、そのカスタム・モデルとそのデータを取り戻すことはできないため、このメソッドは慎重に使用してください。

### 要求例
{: #deleteExample-language}

以下に、指定のカスタマイズ ID を持つカスタム・モデルを削除する例を示します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
