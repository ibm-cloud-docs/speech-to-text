---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 出力機能
{: #output}

{{site.data.keyword.speechtotextshort}} サービスには、サービスが音声認識要求の書き起こし結果に含まれるという情報を示す以下の機能が用意されています。出力パラメーターはすべてオプションです。
{: shortdesc}

-   サービスのインターフェースごとの単純な音声認識要求の例については、[認識要求の実行](/docs/services/speech-to-text/basic-request.html)を参照してください。
-   音声認識応答の例と説明は、[認識結果の理解](/docs/services/speech-to-text/basic-response.html)を参照してください。このサービスは、すべての JSON 応答の内容を UTF-8 文字セットで返します。

-   使用可能なすべての音声認識パラメーターのアルファベット順リストについては、それらの状況 (一般出荷可能またはベータ) およびサポートされる言語を含めて、[パラメーターの要約](/docs/services/speech-to-text/summary.html)を参照してください。

## 話者ラベル
{: #speaker_labels}

話者ラベル機能は、米国英語、日本語、およびスペイン語 (広帯域モデルと狭帯域モデルの両方) と英国英語 (狭帯域モデルのみ) で使用可能なベータ機能です。
{: note}

話者ラベルは、どの個人が多重参加者交換でどの単語を話したかを識別します。(誰がいつ話したかのラベル付けは、*話者ダイアライゼーション*と呼ばれることもあります。)この情報を使用して、コール・センターへの連絡など、音声ストリームの個人別の書き起こしを作成できます。あるいは、この情報を使用して、会話ロボットまたはアバターとのやり取りをアニメーションにすることができます。最高のパフォーマンスを得るため、少なくとも 1 分以上の長さの音声を使用します。

話者ラベルは、話者 2 名のシナリオ用に最適化されます。話者ラベルは、2 名の個人が長時間のやり取りを行う電話での会話に最適です。最大 6 名の話者を処理できますが、3 名以上の話者の場合、パフォーマンスが安定しないことがあります。2 名のやり取りは、通常、狭帯域メディアで行われますが、話者ラベルは、サポート対象の狭帯域モデルおよび広帯域モデルで使用できます。

この機能を使用するには、認識要求の `speaker_labels` パラメーターを `true` に設定します。このパラメーターはデフォルトでは `false` です。サービスは、音声の個別の単語によって話者を識別します。話者の識別では、単語の開始時間と終了時間に依存します。このため、話者ラベルを有効にすると、`timestamps` パラメーターが強制的 `true` になります ([単語のタイム・スタンプ](/docs/services/speech-to-text/output.html#word_timestamps)を参照)。

### 話者ラベルの例
{: #speakerLabelsExample}

以下の要求例は、話者ラベルを含む応答を示しています。`timestamps` 配列の各要素に関連付けられている数値は、音声内の単語の開始時間と終了時間です。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
         "final": true
      }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

`transcript` フィールドには、すべての参加者が発話したとおりの単語がリストされた音声の最終書き起こしが示されます。話者ラベルをタイム・スタンプと比較および同期すると、会話を発生順に再構築できます。

<table style="width:50%">
  <caption>表 1. 話者ラベルの例</caption>
  <tr>
    <th style="text-align:left">タイム・スタンプ</th>
    <th style="text-align:left">話者ラベル</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

結果には、2 名のやり取りがどのように開始されたかが明確に示されています。

-   **話者 2** - "Hello?"
-   **話者 1** - "Yeah?"
-   **話者 2** - "Yeah, how's Billy?"

この例では、`confidence` フィールドにサービスの話者の識別における信頼度が示されています。 `final` フィールドには、各単語の値 `false` が格納されています。 この値は、サービスが音声を処理中であることを示します。 サービスが、終了する前に話者の識別や個別の単語の信頼度を変更する場合があります。

### 話者ラベルの話者 ID
{: #speakerLabelsIDs}

この例では、話者 ID の興味深い側面も示しています。`speaker` フィールドで、最初の話者の ID は `2` で、2 番目の話者の ID は `1` になっています。サービスは、音声の処理時に話者のパターンを理解していきます。 したがって、最終結果を生成する前に、個別の単語の話者 ID を変更し、話者を追加および削除する場合があります。

その結果、話者 ID が順次、連続、または順序付けされた状態ではない場合があります。 例えば、ID は通常 `0` で開始されますが、前出の例では最も古い ID が `1` です。サービスが、音声の追加分析に基づいて話者 `0` の以前の単語割り当てを省略したと考えられます。 省略は、後の話者にも発生する可能性があります。 省略によって、番号付けにギャップが生じ、`0` および `2` などとラベル付けされた 2 名の話者が作成されることがあります。別の考慮事項として、話者が会話を終了できるということがあります。 このため、会話の早い段階にのみ参加した参加者はその後の結果に表示されない場合があります。

### 話者ラベルの中間結果の要求
{: #speakerLabelsInterim}

WebSocket インターフェースでは、話者ラベルとともに中間結果を要求できます ([中間結果](/docs/services/speech-to-text/output.html#interim)を参照)。最終結果は、通常、中間結果よりも改善されていますが、中間結果は書き起こしの進行と話者ラベルの割り当ての識別に役立ちます。中間結果は一時的な話者および ID が出現および消失した時期を示すことができます。ただし、サービスは最初に識別し、その後再考して省略した話者の ID を再利用できます。このため、中間結果および最終結果で 1 つの ID が 2 名の異なる話者に関連する場合があります。

中間結果と話者ラベルの両方を要求すると、長い音声ストリームの最終結果を、最初の中間結果が返されてしばらくたってから受け取る場合があります。また、一部の中間結果に `results` フィールドおよび `result_index` フィールドがなく、`speaker_labels` フィールドのみが含まれる場合があります。中間結果を要求しない場合、サービスは `results` フィールドと `result_index` フィールドおよび単一の `speaker_labels` フィールドを含む最終結果を返します。

サービスは、音声ストリームの完了またはタイムアウトのいずれかが発生すると、最終結果を送信します。サービスは、いずれの場合でも返す話者ラベルの最後の単語についてのみ、`final` フィールドを `true` に設定します。

### 話者ラベルのパフォーマンスに関する考慮事項
{: #speakerLabelsPerformance}

上記のとおり、話者ラベル機能は、コール・センターとのやり取りなど 2 名での会話向けに最適化されています。このため、以下の可能性があるパフォーマンス上の問題を考慮する必要があります。

-   話者が 1 名の音声に関するパフォーマンスが貧弱である可能性があります。音声の品質や話者の音声のバリエーションによって、サービスが存在しない追加の話者を識別する場合があります。このような話者は、幻覚と呼ばれます。
-   同様に、ポッドキャストなど、独占的な話者による音声に関するパフォーマンスが貧弱である可能性があります。サービスは、短時間話す話者を見逃す傾向があり、幻覚を作成する可能性もあります。
-   6 名を超える話者の音声に関するパフォーマンスは、明確ではありません。この機能は、最大で 6 名の話者を処理できます。
-   短い発話に関するパフォーマンスは、長い発話よりも正確度が低下します。 このサービスでは、参加者が長い時間 (話者ごとに 30 秒以上) 発話すると、より良い結果を得ることができます。 話者ごとに使用可能な音声の相対的な量もパフォーマンスに影響を与えます。
-   発話の最初の 30 秒間はパフォーマンスが低下する可能性があります。 これは通常、音声が 1 分を経過すると改善されます。

すべての書き起こしと同様に、パフォーマンスも低品質の音声、背景ノイズ、個人の話し方、話者の重複、および音声のその他の側面の影響を受けます。

## キーワード検出
{: #keyword_spotting}

キーワード検出機能は、書き起こしで指定されたストリングを検出します。 サービスでは、同じキーワードを何度でも検出して、各出現を報告できます。 キーワードは、最終結果でのみ識別されます。中間結果では識別されません。 デフォルトでは、サービスではキーワード検出は行われません。

キーワード検出を使用するには、以下の両方のパラメーターを指定する必要があります。

-   `keywords` パラメーターを使用して、検出するストリングの配列を指定します。 このパラメーターを省略した場合、または空の配列を指定した場合には、サービスでキーワード検出は行われません。 単一のキーワード・ストリングに、複数のトークンを含めることができます。 例えば、キーワード `Speech to Text` には 3 つのトークンが含まれています。

    米国英語の場合、発話ストリングと記述ストリングが一致するように各キーワードがサービスによって正規化されます。 例えば、発話数値と記述数値が一致するように数値が正規化されます。 他の言語の場合、キーワードは発話されるとおりに指定する必要があります。

    単一の要求で最大 1000 個のキーワードを検出できます。 キーワードは大/小文字を区別しません。
-   `keywords_threshold` パラメーターを使用して、キーワードの一致と見なす、0.0 以上 1.0 以下の確率を指定します。 このしきい値は、単語がキーワードに一致するためにサービスで必要な信頼度レベルの下限を示します。 キーワードが書き起こしで検出されるのは、その信頼度が指定しきい値以上の場合のみです。

    小さいしきい値を指定すると、多数の一致が生成される可能性があります。 しきい値を指定した場合、1 つ以上のキーワードも指定する必要があります。 一致を返さない場合は、このパラメーターを省略します。

音声ストリーム内のキーワードを識別するには、キーワード検出が必要です。 最終書き起こしを処理することでキーワードを識別することはできません。これは、書き起こしが入力音声の最適なデコード結果を表すためです。 信頼度スコアが低いトークンは含まれていませんが、そのトークンが対象単語を表す可能性があります。 その場合、クライアント・サイドでテキスト処理ツールを書き起こしに適用しても、キーワードが識別されません。 これよりリッチなデコード結果表現が必要ですが、そのような表現はサーバーでのみ使用可能です。

### キーワード検出の結果
{: #keywordSpottingResults}

サービスでは、`results` 配列の要素である `keywords_result` フィールドに結果が返されます。 `keywords_result` フィールドは、列挙可能プロパティーの辞書 (連想配列) です。 各プロパティーは、指定キーワードによって識別され、オブジェクトの配列が含まれます。 サービスは、キーワードに関して検出した一致ごとに配列の要素を 1 つ返します。 各一致のオブジェクトには、以下のフィールドが含まれます。

-   `normalized_text`。入力音声に一致した発話句に正規化された指定キーワードです。
-   `start_time` は、一致の開始時間 (秒単位) です。
-   `end_time` は、一致の終了時間 (秒単位) です。
-   `confidence`。一致したものが指定キーワードを表す、サービスの信頼度です。 結果に含まれるには、信頼度が指定しきい値以上でなければなりません。

サービスで一致するものが見つからなかったキーワードは、配列から省略されます。 以下の場合、キーワードは見つからない可能性があります。

-   単に音声にキーワードが含まれていない。 キーワードがないことが、最も明らかな説明です。
-   設定されているしきい値が大きすぎる。 サービスでキーワードは識別されているが、その信頼度レベルが低いことがあります。この場合、その一致は結果から省略されます。
-   複数のトークンが含まれているキーワード・ストリング (例えば、`Speech to Text`) が発話されたが、トークン間の無音が長すぎた。 サービスでは、音声の書き起こし時に、ストリームを一連のブロックに分割します。 各ブロックは、0.5 秒を超える無音間隔が含まれていない連続した音声チャンクです。 このようなブロックで構成された、結果オブジェクトの配列が構成されます。

    サービスでは、以下の場合にのみ複数のトークンから成るキーワードが一致と見なされます。

    -   キーワードのトークンが同じブロック内にある、
    -   トークンが隣接しているか 0.1 秒以下のギャップで区切られている。

    後者のケースは、短いつなぎ語 (「うーん」や「えーっと」など、非語彙的発話) がキーワードの 2 つのトークンの間に挟まれた場合に発生する可能性があります。 詳しくは、[言い淀みマーカー](/docs/services/speech-to-text/basic-response.html#hesitation)を参照してください。

### キーワード検出の例
{: #keywordSpottingExample}

以下の要求例は、`keywords` パラメーターを 3 つのストリング (`colorado`、`tornado`、および `tornadoes`) の URL エンコードされた配列に設定し、`keywords_threshold` パラメーターを `0.5` に設定しています。 サービスは、適格な `colorado` と `tornadoes` を見つけています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
      {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
                  "start_time": 1.52,
                  "confidence": 1.0,
                  "end_time": 2.15
               }
        ]
      },
         "alternatives": [
            {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## 最大候補数
{: #max_alternatives}

`max_alternatives` パラメーターは、*N* 個の最適な結果の仮説候補を返すようにサービスに指示する整数値を受け入れます。 デフォルトでは、サービスは単一の書き起こし結果のみを返します。これは、このパラメーターを `1` に設定した場合と等価です。`max_alternatives` を 1 より大きい数値に設定すると、その数の最適な書き起こし候補を返すようにサービスに要求することになります。 (値 `0` を指定すると、サービスはデフォルト値の `1` を使用します。)

信頼度スコアが報告されるのは、サービスが返す最適候補に対してのみです。 ほとんどの場合、これが、選択される候補です。

### 最大候補数の例
{: #maximumAlternativesExample}

以下の要求例では、`max_alternatives` パラメーターが `3` に設定されています。サービスによって、3 つの候補のうち、最適候補に対する信頼度が報告されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down is a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## 中間結果
{: #interim}

中間結果機能は、WebSocket インターフェースでのみ使用できます。
{: note}

中間結果は、サービスで最終結果が返される前に変更される可能性が高い、書き起こしの中間仮説です。 サービスは中間結果を生成すると、すぐに返します。 中間結果は、以下に役立ちます。

-   対話式アプリケーションとリアルタイムの書き起こし
-   書き起こしに時間のかかる長い音声ストリーム

中間結果は最終結果よりも頻繁かつ迅速に到着します。 中間結果を使用して、アプリケーションがより迅速に応答したり、書き起こしの進捗を測定したりできるようにすることができます。

中間結果を受け取るには、WebSocket 認識要求の `start` メッセージで `interim_results` JSON パラメーターを `true` に設定します。 `interim_results` パラメーターを省略したり、`false` に設定したりすると、サービスは音声の最後に単一の JSON 書き起こしのみを返します。 このパラメーターを使用するには、以下のガイドラインに従います。

-   オフラインの書き起こしまたはバッチ書き起こしを行っており、最小待ち時間が必要なく、すべての音声の単一の JSON 結果が必要な場合は、このパラメーターを省略するか、`false` に設定します。
-   サービスによる音声の処理に伴って順次結果が到着するようにする場合や最小待ち時間内に結果が必要である場合は、このパラメーターを `true` に設定します。 サービスは、さらに音声を処理しながら中間結果を更新できることに注意してください。

中間結果は、`final` フィールドが `false` に設定された JSON 応答で示されます。 サービスは、さらに音声を処理しながらより正確な書き起こしでこのような結果を更新できます。 最終結果は、`true` に設定された `final` フィールドで識別されます。 サービスは最終結果を更新しません。

### 中間結果の例
{: #interimResultsExample}

以下の短縮された例では、WebSocket 要求の中間結果を要求しています。 その応答で、サービスは最終結果の場合にのみ `final` 属性を `true` に設定します。

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "transcript": "several to "
        }
      ],
         "final": false
      }
  ],
   "result_index": 0
}{
  "results": [
      {
      "alternatives": [
            {
          "transcript": "several tornadoes "
        }
      ],
         "final": false
      }
  ],
   "result_index": 0
}{
  . . .
}{
  "results": [
      {
      "alternatives": [
            {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": false
      }
  ],
   "result_index": 0
}{
  "results": [
      {
      "alternatives": [
            {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## 単語候補
{: #word_alternatives}

単語候補機能 (「コンフュージョン・ネットワーク」とも呼ばれる) は、入力音声の単語について音響的に類似した候補の仮説を報告します。 例えば、単語 `Austin` が音声の単語の最適仮説であるが、 単語 `Boston` が同じ時間間隔における別の考えられる仮説である場合があります。 仮説では、共通の開始時間と終了時間が共有されますが、つづりは異なり、通常は信頼度スコアが異なります。

デフォルトでは、サービスでは単語候補は報告されません。 仮説候補を受け取る必要があることを指定するには、`word_alternatives_threshold` パラメーターを使用して、0.0 以上 1.0 以下の確率を指定します。 このしきい値は、仮説を単語候補として返すためにサービスで必要な仮説の信頼度レベルの下限を示します。 仮説が返されるのは、その信頼度が指定しきい値以上の場合のみです。

単語候補は、書き起こしのタイムラインを小さな間隔 (ビン) に分割したものと捉えることができます。 各ビンには、つづりと信頼度が異なる 1 つ以上の仮説を含めることができます。 `word_alternatives_threshold` パラメーターは、サービスで返される結果の密度を制御します。 小さいしきい値を指定すると、多数の仮説が生成される可能性があります。 

### 単語候補の結果
{: #wordAlternativesResults}

サービスでは、`results` 配列の要素である `word_alternatives` フィールドに結果が返されます。 `word_alternatives` フィールドはオブジェクトの配列であり、各オブジェクトでは、単語候補に関する以下のフィールドが示されます。

-   `start_time` は、仮説が返された単語の入力音声での開始時間 (秒単位) を示します。
-   `end_time` は、仮説が返された単語の入力音声での終了時間 (秒単位) を示します。
-   `alternatives`。仮説オブジェクトの配列です。 各オブジェクトには、サービスの仮説の信頼度スコアを示す `confidence` および仮説を識別する `word` が含まれます。 結果に含まれるには、信頼度が指定しきい値以上でなければなりません。 サービスは信頼度によって候補を順序付けます。

### 単語候補の例
{: #wordAlternativesExample}

以下の要求例では、`word_alternatives_threshold` が `0.9` に指定されています。 この出力には、複数の単語の仮説候補と信頼度スコア、および開始時間と終了時間が含まれています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
      {
      "word_alternatives": [
            {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## 単語の信頼度
{: #word_confidence}

`word_confidence` パラメーターは、書き起こしの単語の信頼度測定値を提供するかどうかをサービスに指示します。 デフォルトでは、サービスは、最終的な書き起こし全体に対してのみ信頼度測定値を報告します。書き起こしに含まれる個々の単語ごとに信頼度測定値を報告するようにサービスに指示するには、`word_confidence` を `true` に設定します。

信頼度測定値は、書き起こされた単語の正確性に関する音響的証拠に基づいたサービスの予測を示します。 信頼度スコアの範囲は 0.0 から 1.0 です。

-   スコアが 1.0 の場合、単語の現在の書き起こしが、最高確率の結果を表していることを示します。
-   スコアが 0.5 の場合は、単語が正しい確率が 50 パーセントであることを意味します。

### 単語の信頼度の例
{: #wordConfidenceExample}

以下の例では、書き起こしの単語の単語の信頼度スコアを要求しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "confidence": 0.89,
          "word_confidence": [
            [
              "several",
                     1.0
            ],
            [
              "tornadoes",
                     1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## 単語のタイム・スタンプ
{: #word_timestamps}

`timestamps` パラメーターは、書き起こした単語のタイム・スタンプを生成するかどうかをサービスに指示します。 デフォルトでは、サービスではタイム・スタンプは報告されません。 音声の先頭を基準とした相対時間として、各単語の開始時間と終了時間 (秒単位) を報告するようにサービスに指示するには、`timestamps` を `true` に設定します。

### 単語のタイム・スタンプの例
{: #wordTimestampsExample}

以下の例では、書き起こしの単語のタイム・スタンプを要求しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
                           1.52,
                           2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## スマート・フォーマット設定
{: #smart_formatting}

スマート・フォーマット設定はベータ版の機能であり、米国英語、日本語、およびスペイン語で使用できます。
{: note}

`smart_formatting` パラメーターは、以下のストリングを標準的表現に変換するようサービスに指示します。

-   日付
-   時刻
-   一連の数字および数値
-   電話番号
-   通貨価値
-   インターネットの E メール・アドレスおよび Web アドレス

スマート・フォーマット設定を有効にするには、`smart_formatting` パラメーターを `true` に設定します。 デフォルトでは、サービスはスマート・フォーマット設定を実行しません。

サービスでは、認識要求の最終書き起こしにのみスマート・フォーマット設定が適用されます。 スマート・フォーマット設定は、テキスト正規化が完了し、結果がクライアントに返される直前に適用されます。 このように変換することにより、書き起こしが読みやすくなり、書き起こしの結果をより効果的に後処理できるようになります。

### 句読点 (米国英語)
{: #smartFormattingPunctuation}

米国英語では、この機能によって、音声の以下の発話キーワード・ストリングを句読点記号に置き換えるようサービスに指示されます。

<table style="width:50%">
  <caption>表 2. スマート・フォーマット設定の句読点キーワード</caption>
  <tr>
    <th style="width:45%; text-align:left">キーワード・ストリング</th>
    <th style="text-align:center">結果の句読点</th>
  </tr>
  <tr>
    <td>
      コンマ
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      ピリオド
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      疑問符
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      感嘆符
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

サービスは、これらのキーワード・ストリングを適切な場所でのみ記号に変換します。 例えば、以下の発話句は、

```
the warranty period is short period
```
{: codeblock}

書き起こしでは以下のテキストに変換されます。

```
the warranty period is short.
```
{: codeblock}

### 言語の相違
{: #smartFormattingDifferences}

スマート・フォーマット設定は、書き起こしに明確なキーワードが含まれるかどうかに基づいて行われます。 サポート対象の各言語では、スマート・フォーマット設定の処理が若干異なります。

#### 米国英語およびスペイン語
{: #smartFormattingEnglishSpanish}

-   時刻は、`AM`、`PM`、`EST` などのキーワードで識別されます。
-   24 時間表記は、キーワード `hours` で識別される場合に変換されます。
-   電話番号は、`911` であるか、または数字 `1` で始まる 10 桁または 11 桁の番号である必要があります。

#### 日本語
{: #smartFormattingJapanese}

-   インターネットの E メール・アドレスおよび Web アドレスは変換されません。
-   電話番号は、日本の電話番号の有効な接頭部で始まる 10 桁または 11 桁の番号である必要があります。 例えば、有効な接頭部には `03` や `090` などがあります。
-   英単語は ASCII (*半角*) 文字に変換されます。 例えば、<code>&#65321;&#65314;&#65325;</code> は `IBM` に変換されます。
-   あいまいな用語はコンテキストが十分ではない場合、変換されません。例えば、<code>&#19968;&#26178;</code> および <code>&#21313;&#20998;</code> は、時間を表しているのかどうかが明確ではありません。
-   句読点はスマート・フォーマット設定の有無に関わらず、同様に処理されます。 例えば、確率の計算に基づいて、<code>&#12459;&#12531;&#12510;</code> または `,` が選択されます。

### スマート・フォーマット設定の例
{: #smartFormattingExample}

以下の例では、認識要求で `smart_formatting` パラメーターを `true` に設定して、スマート・フォーマット設定を要求しています。 以降のセクションでは、要求の結果におけるスマート・フォーマット設定の効果を示しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### スマート・フォーマット設定の結果
{: #smartFormattingResults}

以下の表は、スマート・フォーマット設定を使用した場合と使用しない場合の最終書き起こしの例を示しています。 書き起こしは、米国英語の音声に基づいています。

<table summary="各見出し行の後に、見出しで識別される要素に対するスマート・フォーマット設定の効果を示す例を含む複数の行が続きます。">
  <caption>表 3. スマート・フォーマット設定の書き起こし例</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">スマート・フォーマット設定を使用しない場合</th>
    <th id="with_formatting" style="text-align:left">スマート・フォーマット設定を使用した場合</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>日付</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>時刻</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>数値</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>電話番号</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>通貨価値</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
        cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
        one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>インターネットの E メール・アドレスおよび Web アドレス</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>組み合わせ</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### 長い休止を含む書き起こしの例
{: #smartFormattingLongPauses}

発話に長時間の無音が含まれる場合、最終書き起こしが 2 つ以上のチャンクに分割される可能性があります。 この分割は、以下の例に示すようにフォーマット設定の結果に影響を与えます。

<table>
  <caption>表 4. 長い休止を含むスマート・フォーマット設定の書き起こし例</caption>
  <tr>
    <th style="width:45%; text-align:left">発話音声</th>
    <th style="text-align:left">フォーマット設定された書き起こしの結果</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
        three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;休止&gt;</em> five five
        seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## 数値編集
{: #redaction}

数値編集機能はベータ版の機能であり、米国英語、日本語、および韓国語で使用できます。
{: note}

`redaction` パラメーターは、最終書き起こしの数値データを編集またはマスクするようサービスに指示します。この機能では、連続する 3 桁以上の数値の各桁を `X` 文字で置き換えて編集します。これは、クレジット・カード番号などの機密数値データを編集することを目的としています。

デフォルトでは、サービスは数値データを編集しません。数値編集を有効にするには、`redaction` パラメーターを `true` に設定します。編集を有効にすると、スマート・フォーマット設定を明示的に無効にしているかどうかに関わらず、この機能が自動的に有効になります。また、最大限のセキュリティーを確保するために、編集を有効にすると、サービスでは以下のパラメーター値が適用されます。

-   `keywords` パラメーターおよび `keywords_threshold` パラメーターの値を指定しているかどうかに関わらず、サービスではキーワード検出が無効になります。
-   WebSocket インターフェースの `interim_results` パラメーターを `true` に設定しているかどうかに関わらず、サービスでは中間結果が無効になります。
-   `maximum_alternatives` パラメーターの値を指定しているかどうかに関わらず、サービスでは単一の最終書き起こしのみが返されます。

この機能の設計は、既存のスマート・フォーマット設定機能と類似しています。 サービスでは、編集がテキスト正規化が完了し、結果がクライアントに返される直前に認識要求の最終書き起こしにのみ適用されます。

この機能の今後のバージョンでは、電話番号、番地、銀行口座、社会保障番号、およびその他の機密数値データがすべてマスクされるように編集が拡張される可能性があります。
{: note}

### 言語の相違
{: #redactionDifferences}

この機能は米国英語モデルで説明されているとおりに機能しますが、日本語モデルと韓国語モデルでは以下の相違点があります。

#### 日本語
{: #redactionJapanese}

日本語の編集には以下の相違点があります。

-   連続する 3 桁以上のストリングをマスクする以外に、編集によって、3 桁未満であっても番地がマスクされます。
-   同様に、日本語スタイルの生年月日の日付情報も編集によってマスクされます。 日本語では、日付情報は、通常、ラテン語の *Anno Domini (紀元後)* フォーマットで表されますが、特に生年月日の場合は日本語スタイルに従うことがあります。 この場合、年と月が 1 桁または 2 桁であってもマスクされます。 例えば、数値編集によって以下のストリングが示されているように変更されます。

    <table style="width:50%">
      <caption>表 5. 日本語スタイルの生年月日の編集例</caption>
      <tr>
        <th style="text-align:left">編集しない場合</th>
        <th style="text-align:left">編集した場合</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### 韓国語
{: #redactionKorean}

韓国語の編集には以下の相違点があります。

-   スマート・フォーマット設定機能はサポートされません。 サービスでは韓国語の数値編集が行われますが、その他のスマート・フォーマット設定は行われません。
-   切り離された数字は削減されますが、韓国語の句に含まれる可能性のある数字は削減されません。 例えば、以下の句の文字 <code>&#51060;</code> は `X` で置き換えられません。これは、この文字が次の文字に隣接しているためです。 

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    <code>&#51060;</code> 文字が次の文字とスペースで区切られている場合は、[数値編集の結果](#redactionResults)で説明されているように、`X` で置き換えられます。

### 数値編集の例
{: #redactionExample}

以下の例では、認識要求で `redaction` パラメーターを `true` に設定して、数値編集を要求しています。 要求によって編集が有効になるため、この要求によってサービスでは暗黙的にスマート・フォーマット設定が有効になります。 サービスでは、要求のパラメーターが影響を与えないように効率的に無効になります。単一の最終書き起こしが返され、キーワードは認識されません。 以下のセクションでは、編集の効果を示しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### 数値編集の結果
{: #redactionResults}

以下の表は、各サポート対象言語で数値編集を使用した場合と使用しない場合の最終書き起こしの例を示しています。

<table style="width:90%" summary="各見出し行で言語が識別され、その後に数値編集を有効にした場合としない場合の同じ書き起こしを示す単一の行が続きます。">
  <caption>表 6. 数値編集の書き起こし例</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">編集しない場合</th>
    <th id="with_redaction" style="text-align:left">編集した場合</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>米国英語</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>日本語</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>韓国語</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## 禁止用語フィルター
{: #profanity_filter}

禁止用語フィルター機能は、通常、米国英語でのみ一般出荷可能です。
{: note}

`profanity_filter` パラメーターは、サービスの結果に含まれる禁止用語を検閲するかどうかを指定します。 デフォルトでは、書き起こし内の禁止用語を一連のアスタリスクで置き換えることにより、すべての禁止用語が覆い隠されます。 このパラメーターを `false` に設定すると、出力で、書き起こされたとおりに単語が表示されます。

サービスはすべての最終書き起こしおよび書き起こし候補で禁止用語を校閲します。 また、単語候補、単語の信頼度、単語のタイム・スタンプに関連付けられている結果でも、禁止用語を検閲します。 唯一の例外はキーワード検出であり、`profanity_filter` が `true` かどうかに関係なく、サービスはユーザーによって指定されたすべての単語を返します。

### 禁止用語フィルターの例
{: #profanityFilteringExample}

以下の例では、`profanity_filter` パラメーターにデフォルトの `true` 値を指定して書き起こされた短い音声ファイルの結果を示します。 この要求では、`word_alternatives_threshold` パラメーターを比較的大きい値の `0.99` に、そして `word_confidence` パラメーターと `timestamps` パラメーターを `true` にも設定しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
      {
      "word_alternatives": [
            {
          "start_time": 0.03,
               "alternatives": [
                  {
              "confidence": 1.0,
                     "word": "****"
                  }
          ],
               "end_time": 0.25
        },
        {
          "start_time": 0.25,
               "alternatives": [
                  {
              "confidence": 0.99,
              "word": "you"
            }
          ],
               "end_time": 0.56
            }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
               "timestamps": [
            ["****", 0.03, 0.25],
                  ["you", 0.25, 0.56]
               ]
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}
