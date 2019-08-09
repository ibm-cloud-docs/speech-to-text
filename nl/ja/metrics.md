---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# メトリック機能
{: #metrics}

{{site.data.keyword.speechtotextfull}} サービスは、音声認識要求に対して 2 つのオプション・メトリックを返すことができます。

-   [処理メトリック](#processing_metrics)は、このサービスによる入力音声分析に関する定期的な情報を提供します。このメトリックを使用して、サービスによる音声書き起こしの進行状況を測定します。処理メトリックは、WebSocket インターフェースおよび非同期 HTTP インターフェースで使用できます。
-   [音声メトリック](#audio_metrics)は、入力音声の信号特性に関する情報を提供します。このメトリックを使用して、音声の特性と品質を判別します。音声メトリックは、すべての音声認識インターフェースで使用できます。

デフォルトでは、サービスは要求に対しメトリックを返しません。

## 処理メトリック
{: #processing_metrics}

処理メトリックは、サービスによる入力音声分析に関する詳細なタイミング情報を提供します。サービスは、指定された間隔で、中間結果や最終結果などの書き起こしイベントと一緒にメトリックを返します。

メトリックには、サービスが受信した音声の量、サービスが音声認識エンジンに転送した音声の量、およびサービスによる音声処理にかかった時間に関する統計が含まれます。 話者ラベルを要求すると、話者ラベルを判別するためにサービスが処理した音声の量も示されます。

処理メトリックは、認識要求の進行状況を測定する際に役立ちます。また処理メトリックから、以下の状況が原因で発生する結果が存在しない状況を確認できます。

-   音声がない。
-   送信された音声に発話がない。
-   エンジンがサーバーで停止しており、クライアントとサーバー間でネットワークが停止している。エンジンの停止とネットワークの停止を区別するため、結果はイベント・ベースではなく定期的に生成されます。

メトリックは、定期的な到着時間を調べて応答ジッターを推定する際にも役立ちます。メトリックは一定間隔で生成されるので、到着時間の違いはジッターに起因します。

### 処理メトリックの要求
{: #processing_metrics_request}

処理メトリックを要求するには、以下のオプション・パラメーターを使用します。

-   `processing_metrics` は、サービスが処理メトリックを返すかどうかを示すブール値です。メトリックを要求するには、`true`を指定します。デフォルトでは、サービスからメトリックは返されません。
-   `processing_metrics_interval` は、サービスがメトリックを返す間隔 (実経過時間) を秒単位で指定する浮動小数点数です。デフォルトでは、サービスからメトリックが 1 秒に 1 つずつ返されます。`processing_metrics` パラメーターが `true` に設定されていない場合、サービスはこのパラメーターを無視します。

    このパラメーターが受け入れる最小値は 0.1 秒です。精度レベルは制限されていないため、0.25 や 0.125 などの値を指定できます。サービスは最大値を設けていません。

パラメーターを指定する方法とサービスから処理メトリックが返される方法は、インターフェースによって異なります。

-   WebSocket インターフェースでは、音声認識要求の JSON `start` メッセージにパラメーターを指定します。サービスは、要求された間隔でメトリックをリアルタイムで計算して返します。
-   非同期 HTTP インターフェースでは、音声認識要求に照会パラメーターを指定します。サービスは要求された間隔でメトリックを計算しますが、音声のすべてのメトリックを最終書き起こし結果とともに返します。

サービスからは書き起こしイベントの処理メトリックも返されます。WebSocket インターフェースで中間結果を要求すると、要求した間隔よりも頻繁にメトリックを受信することがあります。

定期的な間隔ではなく、書き起こしイベントでのみ処理メトリックを受信するには、処理間隔として大きな値を設定します。間隔が音声の長さよりも大きい場合、サービスは書き起こしイベントでのみ処理メトリックを返します。

### 処理メトリックについて
{: #processing_metrics_understand}

処理メトリックは、`SpeechRecognitionResults` オブジェクトの `processing_metrics` フィールドに入れて返されます。定期的に生成されるメトリックの場合、以下の例に示すようにオブジェクトには `processing_metrics` フィールドのみが含まれます。

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": float,
      "seen_by_engine": float,
      "transcription": float,
      "speaker_labels": float
    },
    "wall_clock_since_first_byte_received": float,
    "periodic": boolean
  }
}
```
{: codeblock}

`processing_metrics` フィールドに含まれる `ProcessingMetrics` オブジェクトには、以下のフィールドがあります。

-   `wall_clock_since_first_byte_received` は、サービスが入力音声の最初のバイトを受信した時点からの実経過時間 (秒単位) です。このフィールドの値は通常、指定されたメトリック間隔の倍数ですが、以下の 2 つの違いがあります。
    -   値が正確な間隔 (0.25、0.5 など) を反映していないことがあります。例えば実際の値は、サービスが音声を受信して処理する時点に応じて 0.27、0.52 などである場合があります。
    -   書き起こしイベントの値は、処理間隔に関連しません。サービスからはイベントの発生時にイベント・ドリブンの結果が返されます。
-   `periodic` は、メトリックが定期的な間隔と書き起こしイベントのいずれに適用されるかを示します。
    -   `true` の場合は、応答が処理間隔によりトリガーされています。情報には処理メトリックのみが含まれています。
    -   `false` の場合は、応答が書き起こしイベントによりトリガーされています。情報には処理メトリックと書き起こし結果が含まれています。

    このフィールドを使用して、サービスが応答を生成した理由を確認し、必要に応じて異なる結果をフィルタリングします。
-   `processed_audio` に含まれている `ProcessedAudio` オブジェクトは、サービスによる入力音声分析に関するタイミング情報を提供します。

`ProcessedAudio` オブジェクトには次のフィールドがあります。すべてのフィールドは、この応答の音声の秒数を参照します。`wall_clock_since_first_byte_received` フィールドのみが実経過時間を参照します。

-   `received` は、サービスが受信した音声の秒数です。
-   `seen_by_engine` は、サービスがその音声処理エンジンに渡した音声の秒数です。
-   `transcription` は、サービスが音声認識のために処理した音声の秒数です。
-   `speaker_labels` は、サービスが話者ラベルのために処理した音声の秒数です。話者ラベルを要求した場合は、応答にはこのフィールドのみが含まれています。

音声処理エンジンは、入力音声を複数回分析します。`processed_audio` オブジェクトは、エンジンにより処理され、今後再び読み取られることのない音声の値を表示します。処理済み音声は、今後の認識の仮説には影響しません。

メトリックは、エンジンの処理の進行状況と複雑さを示します。

-   `seen_by_engine` は、サービスが少なくとも 1 回読み取ってエンジンに渡した音声です。
-   `received` - `seen_by_engine` は、サービスでバッファーに入れられたものの、エンジンがまだ確認も処理もしていない音声です。
-   時間の関係は、`received` >= `seen_by_engine` >= `transcription` >= `speaker_labels` です。

結果を理解する際には以下の関係も役立ちます。

-   `received` フィールドと `seen_by_engine` フィールドの値は、音声認識処理中の `transcription` フィールドと `speaker_labels` フィールドの値よりも大きくなります。サービスは、結果の処理を開始する前に音声を受信する必要があります。
-   サービスが音声の処理を終了すると、`received` フィールドと `seen_by_engine` フィールドの値が同一になります。これらのフィールドの最終値は、`transcription` フィールドと `speaker_labels` フィールドの値よりも 1 秒未満の差で大きいことがあります。
-   `speaker_labels` フィールドの値は通常、音声認識処理中の `transcription` フィールドの値を追跡します。サービスが音声の処理を完了すると、`transcription` フィールドと `speaker_labels` フィールドの値は同一になります。

### 処理メトリックの例: WebSocket インターフェース
{: #processing_metrics_example_websocket}

以下の例に、WebSocket インターフェースに渡される要求の `start` メッセージを示します。この要求により処理メトリックが有効になり、処理メトリック間隔が 0.25 秒に設定されます。また、`interim_results` パラメーターと `speaker_labels` パラメーターが `true` に設定されます。音声ファイルには "hello world long pause stop" という単純なメッセージが含まれています。

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/flac',
    processing_metrics: true,
    processing_metrics_interval: 0.25,
    interim_results: true,
    speaker_labels: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}
```
{: codeblock}

以下の出力例に、サービスがこの要求に対して返す最初のいくつかの処理メトリック結果を示します。

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.51,
    "periodic": false
  },
  "results": [
    {
      "alternatives": [
            {
          "timestamps": [
            [
              "hello",
              0.43,
              0.76
            ],
            [
              "world",
              0.76,
              1.22
            ]
          ],
          "transcript": "hello world "
        }
      ],
         "final": false
      }
  ],
   "result_index": 0
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

### 処理メトリックの例: 非同期 HTTP インターフェース
{: #processing_metrics_example_http}

以下の例に、非同期 HTTP インターフェースの `/v1/recognitions` メソッドに対する音声認識要求を示します。この要求により処理メトリックが有効になり、間隔が 0.25 秒に設定されます。音声ファイルには、"hello world long pause stop" というメッセージが含まれています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

以下の出力例に、サービスが要求に対して返す最初の 2 つの処理メトリック結果を示します。

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

## 音声メトリック
{: #audio_metrics}

音声メトリックは、入力音声の信号特性に関する詳細情報を提供します。音声処理終了時に入力音声全体の集約メトリックが結果で示されます。技術的な知識のあるユーザーは、音声の詳細な特性についての重要な洞察をこのメトリックから得ることができます。

音声メトリックを使用することで、入力音声の問題をリアルタイムで示すことができ、場合によっては解決策も提供できます。例えば、「背景に雑音が多過ぎます (There is too much noise in the background)」や「マイクに近づいてください (Please come closer to the microphone)」などのメッセージを指示できます。また、オフライン分析ツールを使用して信号特性を確認し、将来のモデル更新に適したデータを提案することもできます。

### 音声メトリックの要求
{: #audio_metrics_request}

音声メトリックを要求するには、`audio_metrics` ブール値パラメーターを `true` に設定します。デフォルトでは、サービスからメトリックは返されません。

-   WebSocket インターフェースでは、音声認識要求の JSON `start` メッセージにパラメーターを指定します。
-   HTTP インターフェースでは、音声認識要求に照会パラメーターを指定します。

サービスから、音声メトリックと最終書き起こし結果が返されます。メトリックはオーディオ・ストリーム全体で 1 回だけ返されます。サービスから (異なる音声ブロックの) 複数の書き起こし結果が返される場合、結果の最後にメトリックのインスタンスが 1 つだけ返されます。

WebSocket インターフェースは 1 つの接続で複数のオーディオ・ストリーム (またはファイル) を受け入れます。異なるストリームを区切るには、`stop` メッセージまたは空のバイナリー blob をサービスに送信します。この場合、区切られたオーディオ・ストリームごとに個別のメトリックが返されます。

### 音声メトリックについて
{: #audio_metrics_understand}

サービスは、このメトリックを `SpeechRecognitionResults` オブジェクトの `audio_metrics` フィールドに入れて返します。

```javascript
{
  "results": [
    . . .
  ],
  "result_index": integer,
  "audio_metrics": {
    "sampling_interval": float,
    "accumulated": {
      "final": boolean,
      "end_time": float,
      "signal_to_noise_ratio": float,
      "speech_ratio": float,
      "high_frequency_loss": float,
      "direct_current_offset": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "clipping_rate": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "non_speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ]
    }
  }
}
```
{: codeblock}

`audio_metrics` フィールドに含まれる `AudioMetrics` オブジェクトには、次の 2 つのフィールドがあります。

-   `sampling_interval` は、サービスが音声メトリックを計算する秒単位の間隔 (通常は 0.1 秒) を示します。
-   `accumulated` には、入力音声の信号特性に関する詳細情報を提供する `AudioMetricsDetails` オブジェクトが含まれています。

`AudioMetricsDetails` オブジェクトの以下のフィールドは、単項値を示します。

-   `final` は、メトリックがオーディオ・ストリームの終了時点のメトリックであるかどうか、つまり書き起こしが完了しているかどうかを示します。現在、このフィールドは常に `true` です。サービスでは、メトリックはオーディオ・ストリームごとに 1 回だけ返されます。結果には、ストリーム全体の集約メトリックが示されます。
-   `end_time` は、メトリックが適用される音声の終了時間 (秒単位) を示します。メトリックはオーディオ・ストリーム全体に適用されるので、終了時間は常に音声の長さになります。
-   `signal_to_noise_ratio` は、音声信号の信号対雑音比 (SNR) を示します。この値は、音声における発話対雑音の比率を示します。有効な値の範囲は 0 から 100 デシベル (dB) です。サービスは、音声の SNR を計算できない場合にはこのフィールドを省略します。
-   `speech_ratio` は、音声信号での発話セグメント対発話なしセグメントの比率です。この値の範囲は 0.0 から 1.0 です。
-   `high_frequency_loss` は、音声信号で周波数成分の上位半分が欠落している確率を示します。
    -   1.0 に近い値は通常、人為的にアップサンプリングされた音声を示します。これは、書き起こし結果の正確性に悪影響を及ぼします。
    -   0.0 または 0.0 に近い値は、音声信号が良好であり、フルスペクトルであることを示します。
    -   0.5 前後の値は、周波数成分の検出に信頼性がないか、または周波数成分の検出を使用できないことを示します。

`AudioMetricsDetails` オブジェクトの以下のフィールドは、信号特性のヒストグラムを示します。各フィールドには `AudioMetricsHistogramBin` オブジェクトの配列が含まれています。各ヒストグラムの単一の単位は、音声の `sampling_interval` の長さに基づいて計算されます。

-   `direct_current_offset` は、音声信号の累積直流 (DC) 成分のヒストグラムを定義します。
-   `clipping_rate` は、音声セグメントのクリッピング率のヒストグラムを定義します。クリッピング率は、セグメントにおいて、音声量子化範囲により示される最大値または最小値に到達するサンプルの割合として定義されます。

このサービスは、16 ビット・パルス符号変調 (PCM) 音声範囲 (-32768 から +32767) または単位範囲 (-1.0 から +1.0) のいずれかを自動検出します。クリッピング率は 0.0 から 1.0 です。値が大きいほど、音声認識が低下する可能性があります。
-   `speech_level` は、発話を含む音声セグメントにおける信号レベルのヒストグラムを定義します。信号レベルは、0.0 (最小レベル) から 1.0 (最大レベル) の範囲に正規化されたデシベル (dB) スケールの二乗平均平方根 (RMS) 値として計算されます。
-   `non_speech_level` は、発話を含まない音声セグメントの信号レベルのヒストグラムを定義します。信号レベルは、0.0 (最小レベル) から 1.0 (最大レベル) の範囲に正規化されたデシベル・スケールの RMS 値として計算されます。

各 `AudioMetricsHistogramBin` オブジェクトは、定義済みの `begin` 境界および `end` 境界を使用してビンを記述します。 各ビンは、そのビンの信号特性範囲内の値の `count` (数) を示します。ヒストグラムの最初と最後のビンが境界ビンです。これらのビンはそれぞれ、負の無限大から最初の境界までの間隔と、最後の境界から正の無限大までの間隔です。

### 音声メトリックの例
{: #audio_metrics_example}

以下の例に、音声メトリックを返す同期 HTTP インターフェースでの音声認識要求を示します。音声ファイルには "hello world long pause stop" という単純なメッセージが含まれています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

応答には、入力音声全体 (長さ 7.0 秒) の音声メトリックが含まれています。入力音声では、発話セグメントが発話なしセグメントよりも多くなっています (37 セグメント対 33 セグメント、`speech_ratio` は 0.529)。クリッピング率は非常に低く、高品質の入力音声であることを示しています。

`high_frequency_loss` フィールドの値は 0.5 です。これは、サービスによる周波数成分の検出に信頼性がないか、または周波数成分の検出が使用できないことを意味します。サービスは音声の SNR を計算できなかったため、結果では `signal_to_noise_ratio` フィールドは無視されます。

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "confidence": 0.96,
          "transcript": "hello world "
        }
      ],
         "final": true
      },
      {
      "alternatives": [
            {
          "confidence": 0.79,
          "transcript": "long pause "
        }
      ],
         "final": true
      },
      {
      "alternatives": [
            {
          "confidence": 0.97,
          "transcript": "stop "
        }
      ],
         "final": true
      }
  ],
  "result_index": 0,
  "audio_metrics": {
    "sampling_interval": 0.1,
    "accumulated": {
      "final": true,
      "end_time": 7.0,
      "speech_ratio": 0.529,
      "high_frequency_loss": 0.5,
      "direct_current_offset": [
        {"begin": -1.0, "end": -0.9, "count": 0},
        {"begin": -0.9, "end": -0.7, "count": 0},
        {"begin": -0.7, "end": -0.5, "count": 0},
        {"begin": -0.5, "end": -0.3, "count": 0},
        {"begin": -0.3, "end": -0.1, "count": 0},
        {"begin": -0.1, "end": 0.1, "count": 70},
        {"begin": 0.1, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "clipping_rate": [
        {"begin": 0.0, "end": 1e-05, "count": 70},
        {"begin": 1e-05, "end": 0.0001, "count": 0},
        {"begin": 0.0001, "end": 0.001, "count": 0},
        {"begin": 0.001, "end": 0.01, "count": 0},
        {"begin": 0.01, "end": 0.1, "count": 0},
        {"begin": 0.1, "end": 1.0, "count": 0}
      ],
      "speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 37},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "non_speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 33},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ]
    }
  }
}
```
{: codeblock}
