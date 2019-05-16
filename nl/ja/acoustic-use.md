---

copyright:
  years: 2017, 2019
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

# カスタム音響モデルの使用
{: #acousticUse}

カスタム音響モデルを作成してトレーニングが完了したら、そのモデルを音声認識要求で使用できます。以下の例に示すように、要求にカスタム音響モデルを指定するには `acoustic_customization_id` クエリー・パラメーターを使用します。要求は、モデルを所有するサービス・インスタンスのサービス資格情報を使用して行う必要があります。
{: shortdesc}

使用するカスタム言語モデルを要求に指定することもできます。これにより、書き起こしの正確度が改善されます。詳しくは、[音声認識でのカスタム言語モデルとカスタム音響モデルの使用](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize)を参照してください。

同一の分野/環境または異なる分野/環境を対象とした複数のカスタム音響モデルを作成できます。ただし、`acoustic_customization_id` パラメーターで一度に指定できるカスタム音響モデルは 1 つに限られます。

-   [WebSocket インターフェース](/docs/services/speech-to-text/websockets.html)の場合は、`/v1/recognize` メソッドを使用します。指定されたカスタム・モデルは、接続を介して送信されるすべての要求に使用されます。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   [同期 HTTP インターフェース](/docs/services/speech-to-text/http.html)の場合は、`POST /v1/recognize` メソッドを使用します。指定されたカスタム・モデルはこの要求で使用されます。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   [非同期 HTTP インターフェース](/docs/services/speech-to-text/async.html)の場合は、`POST /v1/recognitions` メソッドを使用します。指定されたカスタム・モデルはこの要求で使用されます。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

カスタム・モデルのベースがデフォルトのモデル (`en-US_BroadbandModel`) である場合は、要求で言語モデルを省略できます。それ以外の場合は、WebSocket の例に示すように `model` パラメーターを使用して基本モデルを指定する必要があります。カスタム言語モデルは、そのモデルを作成する際に使用された基本モデルでのみ使用できます。
