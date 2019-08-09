---

copyright:
  years: 2015, 2019
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

# カスタム言語モデルの使用
{: #languageUse}

カスタム言語モデルを作成してトレーニングが完了した後は、音声認識要求でそのモデルを使用できます。 以下の例に示すように、`language_customization_id` クエリー・パラメーターを使用して、要求のカスタム言語モデルを指定します。 サービスに対して、カスタム・モデルからの単語に対する重み付けを指定することもできます。 詳しくは、[カスタマイズの重み付けの使用](#weight)を参照してください。 要求は、モデルを所有するサービス・インスタンスの資格情報を使用して行う必要があります。
{: shortdesc}

同じ分野または異なる分野を対象とした複数のカスタム言語モデルを作成できます。 ただし、`language_customization_id` パラメーターを使用して一度に指定できるカスタム言語モデルは 1 つに限られます。 カスタム言語モデルで文法を使用する例は、[音声認識での文法の使用](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)を参照してください。

-   [WebSocket インターフェース](/docs/services/speech-to-text?topic=speech-to-text-websockets)の場合は、`/v1/recognize` メソッドを使用します。 指定されたカスタム・モデルは、接続を介して送信されるすべての要求に使用されます。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   [同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-http)の場合は、`POST /v1/recognize` メソッドを使用します。 指定されたカスタム・モデルはこの要求で使用されます。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   [非同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-async)の場合は、`POST /v1/recognitions` メソッドを使用します。 指定されたカスタム・モデルはこの要求で使用されます。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

カスタム・モデルのベースがデフォルトの言語モデル `en-US_BroadbandModel` である場合は、要求での言語モデルの指定を省略できます。 それ以外の場合は、WebSocket の例に示すように `model` パラメーターを使用して基本モデルを指定する必要があります。 カスタム言語モデルは、そのモデルを作成する際に使用された基本モデルでのみ使用できます。

## カスタマイズの重み付けの使用
{: #weight}

カスタム言語モデルは、カスタム・モデルとカスタマイズされる基本モデルを組み合わせたものです。 サービスに対して、音声認識について、基本モデルからの単語と比較した場合のカスタム言語モデルからの単語に対する重み付けを指定できます。 カスタム・モデルに割り当てられる重み付けは、*カスタマイズの重み付け* と呼ばれます。

カスタム言語モデルの相対的重み付けとして 0.0 から 1.0 の間のダブルを指定します。 デフォルトでは、各カスタム言語モデルには 0.3 の重みがあります。 通常はデフォルトの重み付けによって最高のパフォーマンスが実現されます。 カスタム・モデルの OOV 語と基本語彙の単語の両方が認識されます。

ただし、書き起こされる音声でカスタム・モデルの OOV 語が頻繁に使用されている場合、カスタマイズの重み付けを増やすと、書き起こし結果の正確度が改善されることがあります。 カスタマイズの重み付けは注意して設定してください。 より大きい重み付けによってカスタム・モデルの分野の句の正確度が改善される可能性がありますが、非分野の句に関するパフォーマンスに悪影響を与える場合もあります。 (重み付けを 0.0 に設定した場合でも、言語モデルのカスタマイズの実装によって書き起こしにカスタム単語が含まれる可能性がわずかにあります。)

カスタマイズの重み付けを指定するには、`customization_weight` パラメーターを使用します。 このパラメーターは、カスタム言語モデルをトレーニングする場合や音声認識要求でそのモデルを使用する場合に指定できます。

-   以下の例では、トレーニング要求について、`POST /v1/customizations/{customization_id}/train` メソッドを使用してカスタマイズの重み付け `0.5` を指定しています。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    トレーニング中にカスタマイズの重み付けを設定すると、カスタム言語モデルで重み付けが保存されます。 カスタム・モデルを使用する各認識要求で重み付けを渡す必要はありません。

-   以下の例では、認識要求について、`POST /v1/recognize` メソッドを使用してカスタマイズの重み付け `0.7` を指定しています。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    音声認識中にカスタマイズの重み付けを設定すると、トレーニング中にモデルに保存された重み付けがオーバーライドされます。

## カスタム言語モデルの使用のトラブルシューティング
{: #languageTroubleshoot}

カスタム言語モデルを音声認識に適用したのに、サービスがそのモデルに含まれる単語を使用していない場合は、以下の可能性がある問題を確認します。

-   前述の例に示されているように、カスタマイズ ID を認識要求に適切に渡したことを確認します。
-   カスタム・モデルのステータスが `available` であることを確認します。これはモデルが完全にトレーニングされ、使用する準備ができていることを意味します。 詳しくは、[カスタム言語モデルのリスト](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)を参照してください。
-   新しい単語に対して生成された発音が正しいことを確認します。 詳しくは、[単語リソースの検証](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)を参照してください。
