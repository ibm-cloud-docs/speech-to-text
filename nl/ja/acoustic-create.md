---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# カスタム音響モデルの作成
{: #acoustic}

{{site.data.keyword.speechtotextshort}} サービスのカスタム音響モデルを作成するには、以下の手順に従います。
{: shortdesc}

1.  [カスタム音響モデルを作成します](#createModel-acoustic)。 同じ分野/環境または異なる分野/環境を対象とした複数のカスタム・モデルを作成できます。そのプロセスは、作成するすべてのモデルで同一です。ただし、認識要求で一度に指定できるカスタム音響モデルは 1 つに限られます。
1.  [カスタム音響モデルに音声を追加します](#addAudio)。このサービスは、音声認識のために受け入れる音声ファイル・フォーマットを音響モデリングでも受け入れます。また、複数の音声ファイルを含むアーカイブ・ファイルも受け入れます。音声リソースを追加する方法としてはアーカイブ・ファイルが推奨されます。カスタム・モデルにさらに音声ファイルまたはアーカイブ・ファイルを追加するには、この方法を繰り返します。
1.  [カスタム音響モデルをトレーニングします](#trainModel-acoustic)。 音声リソースをカスタム・モデルに追加したら、モデルをトレーニングする必要があります。トレーニングによって、カスタム音響モデルを音声認識で使用できる状態に準備します。 トレーニングにはかなりの時間がかかることがあります。トレーニングの長さは、モデルに含まれている音声データの量に応じて異なります。

カスタム音響モデルのトレーニングではヘルパー・カスタム言語モデルを指定できます。音声ファイルの書き起こしが含まれたカスタム言語モデル、または音声ファイルの分野の OOV 語が含まれたカスタム言語モデルを使用すると、カスタム音響モデルの品質を改善できます。詳しくは、[カスタム言語モデルを使用したカスタム音響モデルのトレーニング](/docs/services/speech-to-text/acoustic-both.html#useBothTrain)を参照してください。
1.  カスタム・モデルのトレーニングが完了した後、そのモデルを認識要求で使用できます。書き起こしのために渡される音声の音質がカスタム・モデルの音声と類似している場合は、その結果に、サービスの理解が向上したことが反映されます。詳しくは、[カスタム音響モデルの使用](/docs/services/speech-to-text/acoustic-use.html)を参照してください。

    1 つの認識要求でカスタム音響モデルとカスタム言語モデルの両方を渡すと、認識の正確度をさらに改善できます。詳しくは、[音声認識でのカスタム言語モデルとカスタム音響モデルの使用](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize)を参照してください。

カスタム音響モデルの作成手順は反復型です。音声の追加または削除と、モデルのトレーニングまたはリトレーニングを必要に応じて何度でも行うことができます。音声の変更内容を反映するには、モデルをリトレーニングする必要があります。モデルのリトレーニング時には、(新しいデータだけではなく) すべての音声データがトレーニングに使用されます。したがってトレーニング時間は、モデルに含まれている音声の合計量に応じた長さになります。

音響モデルのカスタマイズは、すべての言語でベータ版機能として利用できます。詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text/custom.html#languageSupport)を参照してください。
{: note}

## カスタム音響モデルの作成
{: #createModel-acoustic}

新しいカスタム音響モデルを作成するには、`POST /v1/acoustic_customizations` メソッドを使用します。 作成できるカスタム音響モデルの数に制限はありませんが、音声認識要求で一度に使用できるカスタム音響モデルは 1 つに限られます。このメソッドは、新しいカスタム・モデルの属性を定義する JSON オブジェクトを、要求の本体として受け入れます。

<table>
  <caption>表 1. 新しいカスタム音響モデルの属性</caption>
  <tr>
    <th style="width:20%; text-align:left">パラメーター</th>
    <th style="width:12%; text-align:center">データ型</th>
    <th style="text-align:left">説明</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>必須</em></td>
    <td style="text-align:center">String</td>
    <td>
      新規カスタム音響モデルのユーザー定義の名前。 カスタム・モデルの音響環境を表す名前を使用します (<code>Mobile custom model</code>、<code>Noisy car custom
      model</code> など)。ユーザーが所有するすべてのカスタム音響モデルの間で一意となる
      名前を使用します。 カスタム・モデルの言語と一致するローカライズされた名前を使用します。</td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>必須</em></td>
    <td style="text-align:center">String</td>
    <td>
      新規モデルによってカスタマイズする基本モデルの名前。<code>GET /v1/models</code> メソッドによって返されたモデルの名前を使用する必要があります。新規カスタム・モデルは、
      カスタマイズ対象の基本モデルでのみ使用できます。
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td>
      新規モデルの説明。 カスタム・モデルの言語と一致するローカライズされた説明を使用します。</td>
  </tr>
</table>

以下に、`Example acoustic model` という名前の新規カスタム音響モデルを作成する例を示します。このモデルは基本モデル `en-US_BroadbandModel` を対象に作成され、`Example custom acoustic model` という説明が指定されています。 `Content-Type` ヘッダーで、メソッドに JSON データが渡されることを指定します。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

この例では、新規モデルのカスタマイズ ID が返されます。 各カスタムモデルは、GUID (Globally Unique Identifier) で表されるカスタマイズ ID によって識別されます。 カスタム・モデルに関連する呼び出しで、そのモデルの GUID を指定するには、`customization_id` パラメーターを使用します。

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

新規カスタム・モデルの所有者は、そのモデルを作成するために使用された資格情報を持つサービス・インスタンスです。
詳しくは、[カスタム・モデルの所有権](/docs/services/speech-to-text/custom.html#customOwner)を参照してください。

## カスタム音響モデルへの音声の追加
{: #addAudio}

カスタム音響モデルを作成した後は、次のステップとしてそのモデルに音声リソースを追加します。音声リソースをカスタム・モデルに追加するには、`POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用します。次のアイテムを追加できます。

-   音声認識でサポートされているフォーマットの個別の音声ファイル ([音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照)。
-   複数の音声ファイルが含まれているアーカイブ・ファイル (**.zip** または **.tar.gz** ファイル)。複数の音声ファイルを 1 つのアーカイブ・ファイルにまとめ、このアーカイブ・ファイルをロードする操作は、音声ファイルを個別に追加する操作よりもずっと効率的です。

音声リソースを要求の本体として渡し、リソースに `audio_name` を割り当てます。詳しくは、[音声リソースの処理](/docs/services/speech-to-text/acoustic-resource.html)を参照してください。

以下に、音声タイプのリソースとアーカイブ・タイプのリソースの両方を追加する例を示します。

-   この例では、指定の `customization_id` を持つカスタム音響モデルに音声タイプのリソースを追加します。`Content-Type` ヘッダーでは、音声タイプが `audio/wav` と指定されています。音声ファイル **audio1.wav** が要求本体として渡され、リソースに `audio1` という名前が指定されています。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   以下に、指定のカスタム音響モデルにアーカイブ・タイプのリソースを追加する例を示します。`Content-Type` ヘッダーでは、アーカイブのタイプが `application/zip` として指定されています。`Contained-Contented-Type` ヘッダーでは、アーカイブに含まれるすべてのファイルのフォーマットが `audio/l16` であり、サンプリング・レートが 16 kHz であることが指定されています。アーカイブ・ファイル **audio2.zip** が要求本体として渡され、リソースに `audio2` という名前が指定されています。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

これらのメソッドは、カスタム・モデルの既存の音声リソースを上書きするオプションの `allow_overwrite` クエリー・パラメーターも受け入れます。モデルに追加した音声ファイルを更新する必要がある場合には、このパラメーターを使用してください。

このメソッドは非同期です。音声の長さに応じて、完了するまでに数秒かかることがあります。アーカイブ・ファイルの場合、操作にかかる時間は音声ファイルの時間よって異なります。音声リソースの追加要求のステータスの確認について詳しくは、[音声追加要求のモニタリング](#monitorAudio)を参照してください。

1 つのカスタム・モデルに追加できる音声リソースの数に制限はありません。音声リソースを追加するには、音声ファイルまたはアーカイブ・ファイルごとにメソッドを 1 回ずつ呼び出します。1 つの音声リソースの追加が完全に完了してから、別の音声リソースを追加できます。カスタム・モデルをトレーニングする前に、カスタム・モデルに、最小 10 分から最大 200 時間までの長さで、無音ではなく発話が含まれている音声を追加する必要があります。音声タイプのリソースとアーカイブ・タイプのリソースのサイズは 100 MB 以下でなければなりません。詳しくは、[音声リソースの追加に関するガイドライン](/docs/services/speech-to-text/acoustic-resource.html#audioGuidelines)を参照してください。

### 音声追加要求のモニタリング
{: #monitorAudio}

音声が有効な場合にはサービスから 201 応答コードが返されます。その後、1 つ以上の音声ファイルの内容が非同期に分析され、音声に関する情報 (長さ、サンプリング・レート、エンコードなど) が自動的に抽出されます。サービスによる現行要求のすべての音声ファイルの分析が完了するまでは、カスタム音響モデルにさらに音声を追加する要求や、モデルのトレーニング要求を送信できません。

要求のステータスを確認するには、`GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用して音声のステータスをポーリングします。このメソッドはカスタム・モデルの GUID と音声リソースの名前を受け入れます。応答にはリソースの `status` が含まれています。これは次のいずれかの値になります。

-   `ok`: 音声が受け入れ可能であり、分析が完了していることを示します。
-   `being_processed`: このサービスによる音声の分析がまだ進行中であることを示します。
-   `invalid`: 音声ファイルが処理対象として受け入れられないことを示します。ファイルのフォーマットまたはサンプリング・レートが誤っているか、音声ファイルではありません。アーカイブ・ファイルの場合、無効な音声ファイルがアーカイブ・ファイルに含まれていると、アーカイブ全体が無効になります。

応答の内容と `status` フィールドの位置は、リソースのタイプ (音声またはアーカイブ) に応じて異なります。

-   *音声タイプのリソースでは、*`status` フィールドは最上位 (`AudioListing`) オブジェクト内にあります。

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *アーカイブ・タイプのリソースでは、*`status` フィールドは `container` フィールドにネストされた 2 番目のレベルの (`AudioResource`) オブジェクト内にあります。

    ```bash
    curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    ```
    {: codeblock}

ステータスが `ok` になるまで、ループを使用して数秒間隔で音声ステータスを確認します。このメソッドにより返されるその他のフィールドについて詳しくは、[カスタム音響モデルの音声リソースのリスト](/docs/services/speech-to-text/acoustic-audio.html#listAudio)を参照してください。

## カスタム音響モデルのトレーニング
{: #trainModel-acoustic}

カスタム音響モデルに音声リソースを追加したら、新しいデータに関してモデルをトレーニングする必要があります。トレーニングによって、カスタム・モデルを音声認識で使用できる状態に準備します。 新しいデータに関してトレーニングされるまでは、モデルは認識要求には使用できません。また、新しい音声リソースまたは変更された音声リソースの形式でのカスタム・モデルの更新は、その変更内容でモデルをトレーニングするまでは、モデルに反映されません。

カスタム・モデルをトレーニングするには、`POST /v1/acoustic_customizations/{customization_id}/train` メソッドを使用します。以下の例に示すように、トレーニングするモデルのカスタマイズ ID をこのメソッドに渡します。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

これらのメソッドは、オプションの `custom_language_model_id` クエリー・パラメーターも受け入れます。このパラメーターは、トレーニング中に使用する個別に作成されたカスタム言語モデルを指定するためのものです。トレーニングには、音声ファイルの書き起こしが含まれているカスタム言語モデル、または音声ファイルのコンテンツに関連するコーパスまたは OOV 語が含まれているカスタム言語モデルを使用できます。トレーニングを成功させるには、両方のカスタム・モデルが、同じ基本モデルの同じバージョンに基づいている必要があります。詳しくは、[カスタム言語モデルを使用したカスタム音響モデルのトレーニング](/docs/services/speech-to-text/acoustic-both.html#useBothTrain)を参照してください。


トレーニング・メソッドは非同期です。カスタム音響モデルに含まれる音声データの量とサービスに現在かかっている負荷によっては、トレーニングが完了するまでに数分から数時間かかることがあります。カスタム音響モデルのトレーニングにかかる時間の目安は、音声データの長さの約 2 倍から 4 倍です。この時間は、トレーニング対象のモデルと、音声の性質 (音声がクリアであるかまたはノイズが入っているかなど) に応じて異なります。例えば、2 時間分の音声が含まれているモデルのトレーニングには、約 4 時間から 8 時間かかります。トレーニング操作のステータスの確認について詳しくは、[モデルのトレーニング要求のモニタリング](#monitorTraining-acoustic)を参照してください。

### モデルのトレーニング要求のモニタリング
{: #monitorTraining-acoustic}

トレーニング・プロセスが正常に開始されると、サービスは 200 応答コードを返します。 既存の要求が完了するまでは、サービスはこれ以降のトレーニング要求や音声リソース追加要求を受け入れることができません。

トレーニング要求のステータスを確認するには、`GET /v1/acoustic_customizations/{customization_id}` メソッドを使用してモデルのステータスをポーリングします。以下の例に示すように、このメソッドは音響モデルのカスタマイズ ID を受け入れ、そのモデルのステータスを返します。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

リソースには、モデルの現在のステータスを示す `status` フィールドと `progress` フィールドが含まれています。`progress` フィールドの意味は、モデルのステータスに応じて異なります。`status` フィールドには、以下のいずれかの値が設定されます。

-   `pending` は、モデルは作成されたが、トレーニング・データの追加を待機しているか、サービスによる追加データの分析完了を待機していることを意味します。 この場合、`progress`
フィールドは `0` になります。
-   `ready` は、モデルのトレーニングを開始できる状態になっていることを意味します。 この場合、`progress`
フィールドは `0` になります。
-   `training` は、モデルがトレーニング中であることを意味します。トレーニングが完了した時点で、`progress` フィールドの値は `0` から `100` に変わります。<!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` は、モデルのトレーニングが完了し、モデルが使用可能な状態になっていることを意味します。 この場合、`progress`
フィールドは `100` になります。
-   `upgrading` は、モデルがアップグレード中であることを意味します。この場合、`progress`
フィールドは `0` になります。
-   `failed` は、モデルのトレーニングが失敗したことを意味します。 この場合、`progress`
フィールドは `0` になります。

モデルが `available` になるまで、ループを使用して 1 分間隔でトレーニングのステータスを確認します。このメソッドにより返されるその他のフィールドについて詳しくは、[カスタム音響モデルのリスト](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic)を参照してください。

### トレーニングの失敗
{: #failedTraining-acoustic}

サービスがカスタム音響モデルに対する別の要求を処理中の場合、そのカスタム音響モデルのトレーニングは開始できません。競合する要求としては、別のトレーニング要求や、モデルへの音声リソース追加要求などがあります。また、以下の理由でトレーニングを開始できないことがあります。

-   カスタム・モデルに含まれている音声データが 10 分未満である。
-   カスタム・モデルに含まれている音声データが 200 時間を超えている。
-   カスタム・モデルの 1 つ以上の音声リソースが無効である。
-   `custom_language_model_id` クエリー・パラメーターで互換性のないカスタム言語モデルを渡した。両方のカスタム・モデルが、同じ基本モデルの同じバージョンに基づいている必要があります。

カスタム・モデルのトレーニングのステータスが `failed` の場合は、`GET /v1/acoustic_customizations/{customization_id}/audio` メソッドと `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用して、モデルの音声リソースを調べ、検出した問題をすべて解決してください。詳しくは、[カスタム音響モデルの音声リソースのリスト](/docs/services/speech-to-text/acoustic-audio.html#listAudio)を参照してください。
