---

copyright:
  years: 2019
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

# 音声リソースの管理
{: #manageAudio}

カスタマイズ・インターフェースには、音声リソースをカスタム音響モデルに追加するときに使用する `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドが含まれています。詳しくは、[カスタム音響モデルへの音声の追加](/docs/services/speech-to-text/acoustic-create.html#addAudio)を参照してください。このインターフェースには、カスタム音響モデルの音声リソースをリストおよび削除するための以下のメソッドも含まれています。
{: shortdesc}

## カスタム音響モデルの音声リソースのリスト
{: #listAudio}

カスタマイズ・インターフェースには、カスタム音響モデルの音声リソースに関する情報をリストするためのメソッドが 2 つあります。

-   `GET /v1/acoustic_customizations/{customization_id}/audio` メソッドは、カスタム・モデルに追加されたすべての音声リソースに関する情報をリストします。
-   `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドは、カスタム・モデルの指定の音声リソースに関する情報をリストします。

どちらのメソッドでも、音声リソースの `name` と以下の追加情報が返されます。

-   `duration`: 音声の長さ (秒単位)。アーカイブ・ファイルの場合、この数値はアーカイブに含まれているすべての音声ファイルの長さの合計を表します。
-   `details`: リソースのタイプ (`audio` または `archive`) を示します。(サービスでリソースを検証できない (その原因としては、ユーザーが誤って音声が含まれていないファイルを渡したことが考えられる) 場合、このタイプは `undetermined` になります。)音声ファイルの場合、詳細には音声の `codec` と `frequency` が含まれています。アーカイブ・ファイルの場合、詳細にはその `compression` タイプが含まれています。

また、1 つのモデルのすべての音声リソースをリストすると、そのモデルで有効なすべての音声リソースを合計した `total_minutes_of_audio` が返されます。この値から、カスタム・モデルに含まれている音声がトレーニング開始に十分であるかまたは不足しているかを判断できます。

これらのメソッドでは音声データのステータスもリストされます。ステータスは、カスタム・モデルへの音声ファイル追加の要求に対する応答として返される、サービスによる音声ファイルの分析を確認する際に重要です。

-   `ok`: サービスによる音声データの分析が正常に完了したことを示します。データはカスタム・モデルのトレーニングに使用できます。
-   `being_processed`: このサービスによる音声データの分析がまだ進行中であることを示します。分析が完了するまで、サービスは新しい音声の追加要求や、カスタム・モデルのトレーニング要求を受け入れることができません。
-   `invalid`: 音声データがモデルのトレーニングに無効であることを示します (音声データのフォーマットまたはサンプリング・レートが誤っているか、音声データが破損していることが原因と考えられます)。

### 要求の例: すべての音声リソースのリスト
{: #listExample-audio}

以下に、指定のカスタマイズ ID を持つカスタム音響モデルの音声リソースをすべてリストする例を示します。この音響モデルには 3 つの音声リソースがあります。サービスによる `audio1` および `audio2` の分析は正常に完了しており、`audio3` の分析が進行中です。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### 要求の例: 音声タイプのリソースに関する情報の取得
{: #getExampleAudio}

以下の例は、`audio1` という名前の音声タイプのリソースに関する情報を返します。このリソースの長さは 131 秒で、`pcm_s16le` コーデックによりエンコードされています。モデルに正常に追加されています。

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

### 要求の例: アーカイブ・タイプのリソースに関する情報の取得
{: #getExampleArchive}

以下の例は、`audio2` という名前のアーカイブ・タイプのリソースに関する情報を返します。このリソースは、9 分を超えるの音声が含まれている **.zip** ファイルです。このリソースも正常にモデルに追加されています。例に示すように、アーカイブ・タイプのリソースに関する情報を照会すると、リソースに含まれているファイルの情報も返されます。

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
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## カスタム音響モデルからの音声リソースの削除
{: #deleteAudio}

カスタム音響モデルから既存の音声リソースを削除するには、`DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用します。アーカイブ・タイプの音声リソースを削除すると、ファイルのアーカイブ全体が削除されます。現行インターフェースではアーカイブ・リソースからファイルを個別に削除することはサポートされていません。

音声リソースを削除しても、`POST /v1/acoustic_customizations/{customization_id}/train` メソッドを使用して、更新後のデータに関してカスタム・モデルをトレーニングするまでは、そのモデルに影響はありません。 リソースに関してモデルが正常にトレーニングされている場合は、モデルをリトレーニングするまで、既存の音声データが引き続き音声認識に使用されます。

### 要求例
{: #deleteExample-audio}

以下のメソッドは、指定のカスタマイズ ID を持つカスタム・モデルから `audio3` という名前の音声リソースを削除します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
