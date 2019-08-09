---

copyright:
  years: 2019
lastupdated: "2019-06-24"

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

# カスタム音響モデルの管理
{: #manageAcousticModels}

カスタマイズ・インターフェースには、カスタム音響モデルを作成するための `POST /v1/acoustic_customizations` メソッドが含まれています。 このインターフェースには、最新の音声リソースに関してカスタム・モデルをトレーニングするための `POST /v1/acoustic_customizations/train` メソッドも含まれています。 詳しくは、以下を参照してください。
{: shortdesc}

-   [カスタム音響モデルの作成](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [カスタム音響モデルのトレーニング](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

さらにこのインターフェースには、カスタム音響モデルに関する情報をリストするメソッド、カスタム・モデルを初期状態にリセットするメソッド、カスタム・モデルをアップグレードするメソッド、カスタム・モデルを削除するメソッドも含まれています。モデルへの音声リソースの追加など、サービスがカスタム・モデルに対する別の操作を処理している間、そのカスタム・モデルのトレーニング、リセット、アップグレード、削除は行えません。

## カスタム音響モデルのリスト
{: #listModels-acoustic}

カスタマイズ・インターフェースには、指定の資格情報が所有するカスタム音響モデルに関する情報をリストするためのメソッドが 2 つあります。

-   `GET /v1/acoustic_customizations` メソッドは、すべてのカスタム音響モデルに関する情報、または指定の言語のすべてのカスタム音響モデルに関する情報をリストします。
-   `GET /v1/acoustic_customizations/{customization_id}` メソッドは、指定のカスタム音響モデルに関する情報をリストします。 サービスをポーリングしてトレーニング要求のステータスを調べるには、このメソッドを使用します。

どちらのメソッドも、カスタム音響モデルに関する以下の情報を返します。

-   `customization_id` は、カスタム・モデルの GUID (Globally Unique Identifier) を識別します。 GUID は、インターフェースのメソッドでモデルを識別するために使用されます。
-   `created` は、カスタム・モデルが作成された日時 (協定世界時、UTC) です。
-   `updated` は、カスタム・モデルの最終変更日時 (協定世界時、UTC) です。
-   `language` は、カスタム・モデルの言語です。
-   `owner` は、カスタム・モデルを所有するサービス・インスタンスの資格情報を識別します。
-   `name` は、カスタム・モデルの名前です。
-   `description` は、カスタム・モデルの説明を示します (モデルの作成時に指定されている場合)。
-   `base_model_name`: カスタム・モデルを作成する際に使用された言語モデルの名前を示します。
-   `versions` は、カスタム・モデルの使用可能なバージョンのリストを示します。 配列の各要素は、カスタム・モデルで使用できる基本モデルのバージョンを意味します。 カスタム・モデルをアップグレードしている場合にのみ複数のバージョンが存在します。 それ以外の場合は、1 つのバージョンのみが表示されます。 詳しくは、[カスタム・モデルのバージョン情報の表示](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList)を参照してください。

これらのメソッドは、カスタム・モデルの状態を示す `status` フィールドも返します。

-   `pending` は、モデルが作成されたことを意味します。 モデルは、有効なトレーニング・データ (音声リソース) が追加されるのを待機しているか、追加されたデータの分析をサービスが完了するのを待機しています。
-   `ready` は、モデルに有効な音声データが取り込まれ、モデルのトレーニングを開始できる状態になっていることを意味します。 有効な音声リソースと無効な音声リソースがモデルに混在している場合、`strict` 照会パラメーターを `false` に設定しない限り、モデルのトレーニングは失敗します。詳しくは、[トレーニングの失敗](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic)を参照してください。
-   `training` は、モデルが音声データに関してトレーニング中であることを意味します。
-   `available` は、モデルのトレーニングが完了し、モデルが認識要求で使用可能な状態になっていることを意味します。
-   `upgrading` は、モデルがアップグレード中であることを意味します。
-   `failed` は、モデルのトレーニングが失敗したことを意味します。 モデルの音声リソースを調べ、モデルのトレーニングを妨げている原因を確認してください。 考えられるエラーには、音声の量の不足または過多、無効な音声リソースであることなどがあります。

さらに、出力には `progress` フィールドも含まれます。このフィールドは、カスタム・モデルのトレーニングの現在の進行状況を示します。 `POST /v1/acoustic_customizations/{customization_id}/train` メソッドを使用してモデルのトレーニングを開始した場合、このフィールドはその要求の進行状況を完了率 (パーセンテージ) として示します。 この時点で、ステータスが `available` の場合、フィールドの値は `100` であり、それ以外の場合は、`0` です。

### 要求と応答の例
{: #listExample-acoustic}

以下に、指定の資格情報が所有する米国英語のすべてのカスタム音響モデルをリストする場合の `language` クエリー・パラメーターの例を示します。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
```
{: pre}

資格情報により、以下の 2 つのモデルが所有されています。 1 番目のモデルは、データを待機中であるか、サービスによって処理中です。 2 番目のモデルは、トレーニングが完了しており使用できる状態です。

```javascript
{
  "customizations": [
      {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
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
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## カスタム音響モデルのリセット
{: #resetModel-acoustic}

カスタム音響モデルをリセットするには、`POST /v1/acoustic_customizations/{customization_id}/reset` メソッドを使用します。 カスタム・モデルをリセットすると、モデルから音声リソースがすべて削除され、モデルが作成時の状態に初期化されます。 このメソッドによって、モデル自体またはモデルの名前や言語などのメタデータが削除されることはありません。 ただしモデルをリセットすると、その音声リソースは削除され、再作成しなければならなくなります。

### 要求例
{: #resetExample-acoustic}

以下に、指定のカスタマイズ ID を持つカスタム音響モデルをリセットする例を示します。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## カスタム音響モデルの削除
{: #deleteModel-acoustic}

不要になったカスタム音響モデルを削除するには、`DELETE /v1/acoustic_customizations/{customization_id}` メソッドを使用します。 このメソッドは、カスタム・モデル自体と、このカスタム・モデルに関連付けられているすべての音声を削除します。 カスタム・モデルを削除した後は、そのカスタム・モデルとそのデータを取り戻すことはできないため、このメソッドは慎重に使用してください。

### 要求例
{: #deleteExample-acoustic}

以下に、指定のカスタマイズ ID を持つカスタム音響モデルを削除する例を示します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
