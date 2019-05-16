---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# 同期 HTTP インターフェース
{: #http}

{{site.data.keyword.speechtotextfull}} サービスの同期 HTTP インターフェースでは、サービスによる音声認識を要求するための単一の `POST /v1/recognize` メソッドが用意されています。このメソッドは、書き起こし結果を得るための最も簡単な方法です。このメソッドでは、次の 2 つの方法で音声認識要求を送信できます。
{: shortdesc}

-   最初の方法では、要求の本体を介して単一のストリームですべての音声を送信します。 操作のパラメーターは、要求ヘッダーおよびクエリー・パラメーターとして指定します。 詳しくは、[基本的な HTTP 要求の実行](#HTTP-basic)を参照してください。
-   2 番目の方法では、音声をマルチパート要求として送信します。 この要求のパラメーターは、要求ヘッダー、クエリー・パラメーター、および JSON メタデータの組み合わせとして指定します。詳しくは、[マルチパート HTTP 要求の実行](#HTTP-multi)を参照してください。

単一の要求で、100 バイトから 100 MB までの音声データを送信できます。音声フォーマットと、圧縮を使用して 1 つの要求で送信できる音声の量を最大化する方法について詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。HTTP インターフェースのすべてのメソッドについては、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window} を参照してください。

## 基本的な HTTP 要求の実行
{: #HTTP-basic}

HTTP の `POST /v1/recognize` メソッドを使用すると、音声を簡単に書き起こすことができます。要求の本体を通じてすべての音声を渡して、要求ヘッダーおよびクエリー・パラメーターとしてパラメーターを指定します。

次の `curl` の例では、`audio-file.flac` という名前の単一の FLAC ファイルを対象にした認識要求を送信します。この要求では `model` クエリー・パラメーターを省略して、デフォルト言語モデル `en-US_BroadbandModel` を使用しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

上記の例では、対象の音声に対して次の書き起こし結果が返されます。

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

`POST /v1/recognize` メソッドは、要求対象の音声をすべて処理した後に初めて結果を返します。このメソッドはバッチ処理には適していますが、ライブ音声認識には適していません。ライブ音声を書き起こす場合は、WebSocket インターフェースを使用してください。

データが複数の音声ファイルからなる場合に推奨される音声の送信方法は、複数の要求 (音声ファイルごとに 1 つの要求) を送信することです。 複数の要求をループで送信して、オプションで並列処理を使用してパフォーマンスを向上させることができます。マルチパート音声認識を使用して、複数の音声ファイルを単一の要求を通じて渡すこともできます。

## マルチパート HTTP 要求の実行
{: #HTTP-multi}

`POST /v1/recognize` メソッドは、マルチパート要求もサポートしています。すべての音声データをマルチパート・フォーム・データとして渡します。一部のパラメーターは要求ヘッダーおよびクエリー・パラメーターとして指定しますが、JSON メタデータをフォーム・データとして渡すことで、書き起こしのほとんどの局面を制御します。

マルチパート音声認識は、次のユース・ケースを想定しています。

-   複数の音声ファイルを単一の音声認識要求を通じて渡す場合。
-   JavaScript が無効化されているブラウザーを使用する場合。フォーム・データに基づくマルチパート要求には、JavaScript を使用する必要がありません。
-   認識要求のパラメーターが、ほとんどの HTTP サーバーおよびプロキシーで上限として設定されている 8 KB を超えている場合。例えば、非常に多くのキーワードを検出する場合は、要求のサイズがこの上限を超えることがあります。マルチパート要求は、フォーム・データを使用してこの制約を回避します。

以降のセクションでは、マルチパート要求で使用するパラメーターを説明して、要求の例を紹介します。

### マルチパート要求のパラメーター
{: #multipartParameters}

マルチパート音声認識の以下のパラメーターを要求ヘッダー、クエリー・パラメーター、およびフォーム・データとして指定します。

<table summary="この表の各行では、マルチパート認識要求で使用可能な 1 つのパラメーターの用途を説明しています。">
  <caption>表 1. マルチパート要求のパラメーター</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">パラメーター</th>
    <th id="description" style="text-align:center; width:80%">説明</th>
  </tr>
  <tr>
    <td>
      <code>metadata </code>
      <br/><em>フォーム・データ</em>
      <br/><em>オブジェクト</em>
    </td>
    <td>
      <em>必須。</em> 要求の書き起こしパラメーター
を提供する JSON オブジェクト。このオブジェクトはフォーム・データの最初のパート
でなければなりません。この情報は、フォーム・データの後続パート内の
音声を説明しています。[マルチパート要求
の JSON メタデータ](#multipartJSON)を参照してください。
</td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>フォーム・データ</em>
      <br/><em>ファイル</em>
    </td>
    <td>
      <em>必須。</em> 要求のフォーム・データの残り部分
としての 1 つ以上の音声ファイル。すべての音声ファイルは同じフォーマットである必要があります。
`curl` コマンドでは、要求のファイルごとに 1 つの
別個の <code>--form</code> オプションを指定してください。
</td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Header</em>
      <br/><em>ストリング</em>
    </td>
    <td>
      <em>必須。</em> `multipart/form-data` を指定して、このメソッドにデータが
どのように渡されるのかを指定します。JSON の `part_content_type` パラメーターを使用して、
音声のコンテンツ・タイプを指定します。
</td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Header</em>
      <br/><em>ストリング</em>
    </td>
    <td>
      <em>オプション。</em> `chunked` を指定して、音声データをサービスに
      ストリーミングします。すべての音声を単一の要求で送信する場合は、このパラメーターを省略します。
</td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Query</em>
      <br/><em>ストリング</em>
    </td>
    <td>
      <em>オプション。</em> 要求で使用するモデル
      の ID。デフォルトは `en-US_BroadbandModel` です。
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Query</em>
      <br/><em>ストリング</em>
    </td>
    <td>
      <em>オプション。</em> 要求で使用するカスタム
      言語モデルの GUID。
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Query</em>
      <br/><em>ストリング</em>
    </td>
    <td>
      <em>オプション。</em> 要求で使用する
      カスタム音響モデルの GUID。
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Query</em>
      <br/><em>ストリング</em>
    </td>
    <td>
      <em>オプション。</em> 要求で使用する指定した
      基本モデルのバージョン。
</td>
  </tr>
</table>

クエリー・パラメーターについて詳しくは、[パラメーターの要約](/docs/services/speech-to-text/summary.html)を参照してください。

### マルチパート要求の JSON メタデータ
{: #multipartJSON}

マルチパート要求を通じて渡す JSON メタデータには、以下のフィールドを含めることができます。

-   `part_content_type` (string)
-   `data_parts_count` (integer)
-   `customization_weight` (number)
-   `inactivity_timeout` (integer)
-   `keywords` (string[])
-   `keywords_threshold` (number)
-   `max_alternatives` (integer)
-   `word_alternatives_threshold` (number)
-   `word_confidence` (boolean)
-   `timestamps` (boolean)
-   `profanity_filter` (boolean)
-   `smart_formatting` (boolean)
-   `speaker_labels` (boolean)
-   `grammar_name` (string)
-   `redaction` (boolean)

マルチパート要求専用のパラメーターは以下の 2 つのみです。

-   `part_content_type` フィールドは、ほとんどの音声フォーマットでは*オプション* です。このフィールドが必須となるフォーマットは、`audio/alaw`、 `audio/basic`、`audio/l16`、および `audio/mulaw` です。このフィールドでは、要求の後続パート内の音声のフォーマットを指定します。すべての音声ファイルは同じフォーマットである必要があります。
-   `data_parts_count` フィールドは、すべての要求で*オプション* です。このフィールドでは、その要求を通じて送信される音声ファイルの数を指定します。サービスは、ストリーム終了検知を最後の (おそらく唯一の) データ・パートで実施します。 このパラメーターを省略すると、サービスによって要求からパートの数が判別されます。

メタデータの他のすべてのパラメーターはオプションです。すべての使用可能なパラメーターについて詳しくは、[パラメーターの要約](/docs/services/speech-to-text/summary.html)を参照してください。

### マルチパート要求の例

次の `curl` の例では、`POST /v1/recognize` メソッドを使用してマルチパート認識要求を渡す方法を示しています。この要求によって、2 つの音声ファイル **audio-file1.flac** および **audio-file2.flac** を渡します。`metadata` パラメーターでは、この要求のほとんどのパラメーターを指定し、`upload` パラメーターでは音声ファイルを指定します。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

上記の例では、指定された音声ファイルに対して次の書き起こし結果が返されます。これら 2 つのファイルが送信された順序で、これらのファイルの結果が返されます (この例の出力では、2 つ目のファイルの結果を省略表記しています)。

```javascript
{
   "results": [
      {
         "word_alternatives": [
         {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
          "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
         {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
          "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
          "end_time": 6.04
        }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
