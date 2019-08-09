---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# 認識結果の理解
{: #basic-response}

どのインターフェースを使用している場合でも、{{site.data.keyword.speechtotextfull}} サービスは、指定した入力パラメーターと出力パラメーターを反映した書き起こし結果を返します。 このサービスは、すべての JSON 応答の内容を UTF-8 文字セットで返します。
{: shortdesc}

## 基本的な書き起こしの応答
{: #response}

[認識要求の実行](/docs/services/speech-to-text?topic=speech-to-text-basic-request)の例の場合、このサービスは次の応答を返します。 これらの例では、音声ファイルとそのコンテンツ・タイプだけが渡されます。 音声は、1 つの文を単語間に目立った休止を入れずに読み上げたものです。

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "confidence": 0.96,
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

サービスは `SpeechRecognitionResults` オブジェクト (最上位の応答オブジェクト) を返します。 こうした単純な要求の場合、オブジェクトには 1 つの `results` フィールドと 1 つの `result_index` フィールドが含まれています。

-   `results` フィールドには、書き起こし結果に関する情報の配列が示されます。 例えば、`alternatives` フィールドには `transcript` とサービスの結果に対する `confidence` が含まれています。 `final` フィールドには、結果が変更されないことを示す `true` の値が入っています。
-   `result_index` フィールドには、結果の固有 ID が含まれています。 この例は、休止のない 1 つの音声ファイルが指定され、追加のパラメーターが指定されていない要求に対する最終的な結果を示しています。 したがって、サービスは、値が `0`  (これは常に初期インデックスです) である 1 つの `result_index` フィールドを返しています。

入力音声がこれよりも複雑であるか、要求に追加のパラメーターが指定されている場合、その結果にはこれよりも多くの情報が含まれる可能性があります。

### alternatives フィールド
{: #responseAlternatives}

`alternatives` フィールドには、書き起こし結果の配列が示されます。 この要求では、配列に 1 つの要素が含まれています。

-   `confidence` フィールドは書き起こしに対するサービスの信頼度を示すスコアであり、この例ではこのスコアは約 90 % です。
-   `transcript` フィールドには、書き起こし結果が示されます。

`final` フィールドと `result_index` フィールドによって、これらの値の意味が限定されます。

### final フィールド
{: #responseFinal}

`final` フィールドは、書き起こしが最終的な書き起こし結果であるかどうかを示します。

-   変更されないことが保証されている最終結果の場合、このフィールドは `true` になります。 最終結果として返された書き起こしに対する更新は今後送信されません。
-   変更される可能性のある中間結果の場合、このフィールドは `false` になります。 WebSocket インターフェースで `interim_results` パラメーターを使用すると、音声の書き起こし時に、サービスは変化する中間仮説を複数の `results` フィールドとして返します。 中間結果の場合、`final` フィールドは常に `false` です。 音声の最終結果の場合、このフィールドは `true` に設定されます。 その音声の書き起こしに対する更新は今後送信されません。

中間結果を取得するには、WebSocket インターフェースを使用して `interim_results` パラメーターを `true` に設定します。 詳しくは、[中間結果](/docs/services/speech-to-text?topic=speech-to-text-output#interim)を参照してください。

### result_index フィールド
{: #responseResultIndex}

`result_index` フィールドは、その要求に固有の結果の ID を示します。 中間結果を要求すると、入力音声の変化する仮説としてサービスから複数の `results` フィールドが送信されます。 同一音声の最終結果の場合と同様に、同一音声の中間結果のインデックスの値は常に同一です。

音声の最終結果の受信後には、要求の残り部分でサービスがそれ以降の結果にそのインデックスを付けて送信することはありません。 それ以降の結果のインデックスは 1 ずつ増えます。

中間結果を要求したかどうかに関係なく、音声に休止や長い無音期間が含まれている場合は、異なるインデックスが付いた複数の最終結果がサービスから返されることがあります。 詳しくは、[休止と無音](#pauses-silence)を参照してください。

音声から複数の最終結果が生成される場合は、最終結果の `transcript` 要素を連結して、音声の完全な書き起こしにまとめることができます。 `result_index` の順序に結果をまとめてください。 `final` フィールドが `false` である中間結果は無視できます。

### 追加の応答コンテンツ
{: #responseAdditional}

音声認識に使用できる出力パラメーターの多くは、サービスの応答の内容に影響します。 例えば以下のパラメーターを指定すると、音声に関する追加情報が返されます。

-   `max_alternatives` パラメーターは、複数の書き起こし候補を要求します。 `alternatives` 配列には、各候補を表す個々の要素が含まれています。 配列の各要素には `transcript` フィールドが含まれます。 最も有力な候補にのみ `confidence` フィールドが含まれます。
-   `word_confidence` パラメーターと `timestamps` パラメーターは、書き起こしの個々の単語に関する追加情報を要求します。 `alternatives` 配列には、これらの名前を含む追加のフィールドが含まれます。
-   `keywords` パラメーターと `keywords_threshold` パラメーターは、キーワード検出を要求します。 `results` 配列には `keywords_result` フィールドが含まれます。
-   `word_alternatives_threshold` パラメーターは、個々の単語について音響的に類似している結果を要求します。 `results` 配列には `word_alternatives` フィールドが含まれます。
-   WebSocket インターフェースの `interim_results` パラメーターは、中間書き起こし仮説を要求します。 前述したように、サービスは音声の書き起こしに伴い複数の応答を送信します。
-   `speaker_labels` パラメーターは、複数の参加者による会話での各話者を識別します。 応答には、`results` フィールドおよび `results_index` フィールドと同じレベルに `speaker_labels` フィールドが含まれています。

これらのパラメーターと、サービスの応答に影響する可能性のあるその他のパラメーターについて詳しくは、[出力機能](/docs/services/speech-to-text?topic=speech-to-text-output)を参照してください。 使用可能なすべてのパラメーターについて詳しくは、[パラメーターの要約](/docs/services/speech-to-text?topic=speech-to-text-summary)を参照してください。

## 休止と無音
{: #pauses-silence}

このサービスは、音声ストリーム全体の書き起こしを、ストリームが終了するかタイムアウトになるまで続けます。 ただし、音声の中の発話された単語やフレーズの間に休止や長い無音の期間が入っていると、認識結果に複数の最終結果が含まれることがあります。

サービスが個々の最終結果を決定する際に使用するデフォルトの休止間隔は約 1 秒です。 ほとんどの言語では、休止間隔は厳密に 0.8 秒であり、中国語の場合は 0.6 秒です。 この休止間隔の長さを変更することはできません。

サービスからどのように結果が返されるかは、使用しているインターフェースに応じて異なります。 以下に、HTTP インターフェースと WebSocket インターフェースからの 2 つの最終結果の応答の例を示します。 いずれでも同じ入力音声が使用されています。 音声は、"one two three four five six" というフレーズを読み上げるもので、"three" と "four" の間に 1 秒の休止があります。

-   *HTTP インターフェースでは*、サービスから 1 つの `SpeechRecognitionResults` オブジェクトが送信されます。 `alternatives` 配列には各最終結果の個別の要素が含まれています。 応答には、値が `0` の `result_index` フィールドが 1 つ含まれています。

    ```javascript
    {
      "results": [
      {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
         "final": true
      },
      {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
         "final": true
      }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *WebSocket インターフェースでは*、2 つの異なる `SpeechRecognitionResults` オブジェクトが含まれた 2 つの個別の応答がサービスから送信されます。 各応答オブジェクトの`result_index` フィールドの値は異なります。最初の応答では `0`、2 番目の応答では `1` です。

    ```javascript
    {
      "results": [
      {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
      {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
         "final": true
      }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

結果に複数の最終結果が含まれている場合は、最終結果の `transcript` 要素を連結して、音声の完全な書き起こしにまとめます。

ストリーム音声に 30 秒の無音期間があると、[非アクティブ・タイムアウト](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity)となる可能性があります。
{: note}

## 言い淀みマーカー
{: #hesitation}

サービスでは、会話で短いつなぎ語や休止が検出されると、書き起こしに言い淀みマーカーが組み込まれることがあります。 このような休止は言いつなぎとも呼ばれ、"uhm"、 "uh"、"hmm" などのつなぎ語や関連した非語彙発声が含まれます。 言い淀みマーカーをアプリケーションで使用する必要がない場合は、書き起こしから削除しても問題ありません。

英語では、次の例に示すように、言い淀みトークン `%HESITATION` が使用されます。 その他の言語では異なるマーカーが使用されます。例えば日本語の場合、トークン `D_` が使用されます。

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

言い淀みマーカーは、書き起こしの他のフィールドにも含まれることもあります。 例えば、書き起こしの個々の単語の[単語のタイム・スタンプ](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)を要求すると、各言い淀みマーカーの開始時刻と終了時刻が報告されます。

```javascript
{
  "results": [
      {
      "alternatives": [
            {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
         "final": true
      }
  ],
   "result_index": 0
}
```
{: codeblock}

## 大文字化
{: #capitalization}

このサービスでは、ほとんどの言語で応答書き起こしに大文字化は使用されません。 アプリケーションで大文字化が重要な場合は、各文の最初の単語と、大文字化が適切なすべての単語を大文字化する必要があります。

*米国英語でのみ*、サービスにより適切な名詞が大文字化されます。 例えば、指定されたフレーズに対して次のようなテキストが返されます。

```
Barack Obama graduated from Columbia University
```
{: codeblock}

その他の言語では、以下のテキストが返されます。

```
barack obama graduated from columbia university
```
{: codeblock}

スマート・フォーマット設定を使用しているかどうかに関わらず、サービスは米国英語の場合に常にこの大文字化を適用します。

## 句読点
{: #punctuation}

デフォルトでは、応答の書き起こしに句読点は挿入されません。 必要な句読点をサービスの結果に追加する必要があります。

言語によっては、スマート・フォーマット設定を使用して、特定のキーワード・ストリングを句読点記号 (コンマ、ピリオド、疑問符、感嘆符など) に置き換えるよう、サービスに指示できます。詳しくは、[スマート・フォーマット設定](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)を参照してください。
