---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# 入門チュートリアル
{: #gettingStarted}

{{site.data.keyword.speechtotextfull}} サービスは音声をテキストに書き起こすことで、さまざまな用途に音声書き起こし機能を使用できるようにします。 このチュートリアルでは curl コマンドを活用して、サービスを素早く使用開始することを支援します。 さまざまな事例を通じて、サービスの `POST /v1/recognize` メソッドを呼び出して書き起こしを要求する方法を示します。
{: shortdesc}

## 始めに
{: #before-you-begin}

- {: hide-dashboard}  サービスのインスタンスを作成します。
    1.  {: hide-dashboard} {{site.data.keyword.cloud_notm}} カタログの「[{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external}」ページに移動します。
    1.  {: hide-dashboard} 無料の {{site.data.keyword.cloud_notm}} アカウントを登録するか、ログインします。
    1.  {: hide-dashboard} **「作成」**をクリックします。
-   認証用の資格情報をサービス・インスタンスにコピーします。
    1.  {: hide-dashboard} [{{site.data.keyword.cloud_notm}} リソース・リスト](https://{DomainName}/resources){: external}から、ご使用の {{site.data.keyword.speechtotextshort}} サービス・インスタンスをクリックして、{{site.data.keyword.speechtotextshort}} サービス・ダッシュボード・ページに移動します。
    1.  **「管理」**ページで、 **「表示」**をクリックして資格情報を表示します。
    1.  `「API キー」`の値と`「URL」`の値をコピーします。

### curl の使用例
{: #getting-started-curl}

このチュートリアルでは、`curl` コマンドを使用して、サービスの HTTP インターフェースのメソッドを呼び出します。ご使用のシステムに `curl` コマンドがインストールされていることを確認してください。

1.  `curl` がインストールされているかどうかをテストするには、コマンド・ラインで次のコマンドを実行します。Secure Sockets Layer (SSL) をサポートする `curl` のバージョンが出力にリストされたら、チュートリアルの準備ができています。

    ```bash
    curl -V
    ```
    {: pre}

1.  必要であれば、ご使用のオペレーティング・システムに合った SSL 対応バージョンの `curl` を [curl.haxx.se](https://curl.haxx.se/){: external} からインストールします。

例で使用されている中括弧は省略してください。それらは可変値を表しています。
{: tip}

## ステップ 1: オプションを使用せずに音声を書き起こす
{: #transcribe}

追加の要求パラメーターなしで `POST /v1/recognize` メソッドを呼び出して、FLAC 音声ファイルの基本的な書き起こしを要求します。

1.  サンプル音声ファイル <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a> をダウンロードします。
1.  次のコマンドを実行して、基本的な書き起こしを実行するためのサービスの `/v1/recognize` メソッドをパラメーターなしで呼び出します。 この例では、`Content-Type` ヘッダーを使用して `audio/flac` という音声タイプを指定しています。 この例では、デフォルトの言語モデル `en-US_BroadbandModel` を書き起こし用に使用しています。
    -   {: hide-dashboard} `{apikey}` と `{url}` をご使用の API キーと URL に置き換えます。
    -   `{path_to_file}` は、`audio-file.flac` ファイルの場所を指定するように変更します。

    *Windows ユーザー*: 各行末のバックスラッシュ (``\`) をキャレット (``^`) に置き換えてください。末尾にスペースがないようにしてください。
    {: tip}

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    サービスから以下の書き起こし結果が返されます。

    ```javascript
    {
      "results": [
      {
          "alternatives": [
            {
              "confidence": 0.96
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

## ステップ 2: オプションを使用して音声を書き起こす
{: #transcribeOptions}

`POST /v1/recognize` メソッドを呼び出して同じ FLAC 音声ファイルを書き起こしますが、ここでは 2 つの書き起こしパラメーターを指定します。

1.  必要に応じて、サンプル音声ファイル <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a> をダウンロードします。
1.  次のコマンドを実行して、サービスの `/v1/recognize` メソッドを 2 つの追加パラメーター付きで呼び出します。 `timestamps` パラメーターを `true` に設定して、音声ストリーム内の各単語の始まりと終わりを示します。 `max_alternatives` パラメーターを `3` に設定して、最も可能性が高い代替の書き起こし結果を 3 つ受け取るようにします。 この例では、`Content-Type` ヘッダーを使用して `audio/flac` という音声タイプを指定しており、この要求ではデフォルト・モデル `en-US_BroadbandModel` を使用しています。
    -   {: hide-dashboard} `{apikey}` と `{url}` をご使用の API キーと URL に置き換えます。
    -   `{path_to_file}` は、`audio-file.flac` ファイルの場所を指定するように変更します。

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    サービスから返される次の結果には、タイム・スタンプと 3 つの代替書き起こし内容が含まれています。

    ```javascript
    {
      "results": [
      {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                  ["tornadoes":, 1.51, 2.15],
                  ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado and Sunday "
            }
          ],
         "final": true
      }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## 次のステップ

-   音声認識要求を発行するために使用できるインターフェースと SDK について詳しくは、[開発者向けの概要](/docs/services/speech-to-text?topic=speech-to-text-developerOverview)を参照してください。
-   サービスの各インターフェースで使用する基本的な音声認識要求については、[認識要求の実行](/docs/services/speech-to-text?topic=speech-to-text-basic-request)を参照してください。
-   サービスのインターフェースのすべてのメソッドについて詳しくは、[API リファレンス](https://{DomainName}/apidocs/speech-to-text){: external}を参照してください。
