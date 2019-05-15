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

# 言語とモデル
{: #models}

{{site.data.keyword.speechtotextfull}} サービスでは、数多くの言語での音声認識がサポートされています。すべてのインターフェースで、`model` パラメーターを使用して音声認識要求のモデルを指定できます。モデルは音声が発話されている言語とそのサンプリング・レートを示します。
{: shortdesc}

## サポート対象の言語モデル
{: #modelsList}

ほとんどの言語について、このサービスでは、広帯域と狭帯域の 2 つのモデルがサポートされます。

-   *広帯域モデル*。サンプリング・レートが 16 kHz 以上の音声に使用します。広帯域モデルは、ライブ音声アプリケーションなど、応答性が高いリアルタイム・アプリケーションに使用します。
-   *狭帯域モデル*。サンプリング・レートが 8 kHz の音声に使用します。狭帯域モデルは、通常、このサンプリング・レートを使用する電話音声のオフラインのデコードに使用します。

アプリケーションに正しいモデルを選択することが重要です。音声のサンプリング・レート (および言語) に一致するモデルを使用します。このサービスは、指定したモデルに合うように音声のサンプリング・レートを自動的に調整します。詳しくは、[サンプリング・レート](/docs/services/speech-to-text/audio-formats.html#samplingRate)を参照してください。

最大限の認識の正確度を実現するには、音声の周波数成分も考慮する必要があります。詳しくは、[可聴周波数](/docs/services/speech-to-text/audio-formats.html#frequency)を参照してください。
{: tip}

表 1 には、各言語のサポート対象のモデルがリストされています。要求で `model` パラメーターを省略すると、デフォルトでは、サービスは米国英語の広帯域モデル `en-US_BroadbandModel` を使用します。

<table>
  <caption>テーブル 1. サポート対象の言語モデル</caption>
  <tr>
    <th style="text-align:left">言語</th>
    <th style="text-align:center">広帯域モデル</th>
    <th style="text-align:center">狭帯域モデル</th>
  </tr>
  <tr>
    <td>ブラジル・ポルトガル語</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>フランス語</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>ドイツ語</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>日本語</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>韓国語</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>北京語</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>現代標準アラビア語</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">サポートされません</td>
  </tr>
  <tr>
    <td>スペイン語</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>英国英語</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>米国英語</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
</table>

### 米国英語の短文式モデル
{: #modelsShortform}

米国英語の短文式モデル `en-US_ShortForm_NarrowbandModel` は、Interactive Voice Response (IVR) ソリューションおよび Automated Customer Support ソリューションの音声認識を改善できます。短文式モデルは、自動コール・センターや有人コール・センターなど、顧客サポートの設定で頻繁に表現される短い発話を認識するようにトレーニングされます。このモデルは、例えば、数字、1 文字の単語および名前のつづり、はい/いいえの回答など、正確な発話に合わせて調整されます。短文式モデルと組み合わせて文法を使用すると、認識結果をさらに改善できます。

すべてのモデルと同様に、ノイズの多い環境は結果に悪影響を与えます。例えば、空港、移動中の車両、会議室、複数の話者による背景音響ノイズによって、書き起こしの正確度が低下する場合があります。スピーカーフォンからの音声も、このようなデバイスによくある残響によって正確度が低下する場合があります。短文式モデルとともにカスタム音響モデルを使用すると、このような悪影響を回避できます。

-   言語モデルおよび音響モデルのカスタマイズについて詳しくは、[カスタマイズ・インターフェース](/docs/services/speech-to-text/custom.html)を参照してください。
-   文法について詳しくは、[カスタム言語モデルでの文法の使用](/docs/services/speech-to-text/grammar.html)を参照してください。

### 言語モデルの例
{: #modelsExample}

以下の HTTP 要求の例では、音声認識にモデル `en-US-NarrowbandModel` が使用されています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## モデルのリスト
{: #listModels}

HTTP インターフェースには、サポート対象のモデルに関する情報をリストするためのメソッドが 2 つあります。

-   `GET /v1/models` メソッドは、使用可能なすべてのモデルに関する情報をリストします。
-   `GET /v1/models/{model_id}` メソッドは、指定のモデルに関する情報をリストします。

どちらのメソッドも、モデルに関する以下の情報を返します。

-   `name`。要求で使用するモデルの名前。
-   `language`。モデルの言語 ID (`en-US` など)。
-   `rate`。モデルで使用されるヘルツ単位のサンプリング・レート (音声の許容される最小レート) を識別します。
-   `url`。モデルの URI。
-   `description`。モデルの要旨を示します。
-   `supported_features`。モデルでサポートされる以下の追加のサービス機能を記述します。
    -   `custom_language_model`。そのモデルに基づくカスタム言語モデルを作成できるかどうかを示すブール値。
    -   `speaker_labels`。モデルで `speaker_labels` パラメーターを使用できるかどうかを示します。

### 要求と応答の例
{: #listExample}

以下の例では、サービスでサポートされるすべてのモデルをリストしています。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
      {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

以下の例は、米国英語の広帯域モデルに関する情報を示しています。モデルは言語モデルのカスタマイズと話者ラベルの両方をサポートしています。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
