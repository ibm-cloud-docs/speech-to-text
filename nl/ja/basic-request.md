---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# 認識要求の実行
{: #basic-request}

{{site.data.keyword.speechtotextfull}} サービスによる音声認識を要求するには、書き起こす音声のみを提供する必要があります。 サービスの各インターフェース (WebSocket インターフェース、同期 HTTP インターフェース、および非同期 HTTP インターフェース) には同一の基本的な書き起こし機能が備わっています。
{: shortdesc}

以下のセクションでは、オプションの入力パラメーターまたは出力パラメーターが指定されていない基本的な書き起こし要求をサービスのインターフェース別に示します。

-   これらの例では、小さな FLAC ファイル <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン" title="外部リンク・アイコン"></a> が送信されます。
-   これらの例では、デフォルトの言語モデル `en-US_BroadbandModel` を使用します。

これらの例に対するサービスの応答については、[認識結果の理解](/docs/services/speech-to-text?topic=speech-to-text-basic-response)で説明します。

## 要求での音声の送信
{: #basic-request-audio}

サービスに渡す音声は、サービスでサポートされているフォーマットでなければなりません。 サービスはほとんどの音声のフォーマットを自動的に検出できます。 音声によっては、`Content-Type` またはこれに相当するパラメーターを使用してフォーマットを指定する必要があります。 詳しくは、[音声フォーマット](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)を参照してください。 (以下の例では、わかりやすくするために、すべての要求で音声フォーマットが指定されています。)

WebSocket インターフェースと同期 HTTP インターフェースでは、1 つの要求で最大 100 MB の音声データを渡すことができます。 非同期 HTTP インターフェースでは、最大 1 GB の音声データを渡すことができます。 要求で音声データを送信するときは、常に 100 バイト以上の音声データを送信する必要があります。

大量の音声を認識する場合は、音声を複数の小さなデータに手動で分割できます。 ただし、通常は音声を非可逆圧縮フォーマットに変換する方が効率的で便利です。 圧縮により、1 つの要求で送信できるデータの量を最大化できます。 特に音声が WAV または FLAC フォーマットの場合、非可逆フォーマットに変換するとかなりの差が出ます。

-   圧縮を使用する音声フォーマットについて詳しくは、[サポートされている音声フォーマット](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#formats)を参照してください。
-   圧縮の効果と、圧縮を使用するフォーマットへの音声の変換について詳しくは、[データ制限および圧縮](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits)および[音声の変換](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#conversion)を参照してください。

## WebSocket インターフェースの使用
{: #basic-request-websocket}

[WebSocket インターフェース](/docs/services/speech-to-text?topic=speech-to-text-websockets)では、全二重接続により低遅延と高いスループットを実現する効率的な実装が提供されます。 すべての要求と応答が、同一 WebSocket 接続を介して送信されます。 WebSocket はその特長から、推奨される音声認識メカニズムです。 詳しくは、[WebSocket インターフェースの利点](/docs/services/speech-to-text?topic=speech-to-text-developerOverview#advantages)を参照してください。

WebSocket インターフェースを使用するには、最初に `/v1/recognize` メソッドを使用してサービスとの接続を確立します。 この接続を介して送信される要求に使用する言語モデルやカスタム・モデルなどのパラメーターを指定します。 次に、サービスからの応答を処理するイベント・リスナーを登録します。 要求を行うには、音声フォーマットとその他の追加パラメーターを指定した JSON テキスト・メッセージを送信します。 音声をバイナリー・メッセージ (blob) として渡し、音声の終わりを示すテキスト・メッセージを送信します。

以下に、接続を確立して認識要求のテキスト・メッセージとバイナリー・メッセージを送信する JavaScript コードの例を示します。 この例には、イベント・ハンドラーをインストールするコードは含まれていません。

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  action: 'start',
  content-type: 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({action: 'stop'}));
```
{: codeblock}

## 同期 HTTP インターフェースの使用
{: #basic-request-sync}

[同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-http)では、最も簡単に認識要求を行うことができます。 サービスに対して要求を行うには、`POST /v1/recognize` メソッドを使用します。 音声とすべてのパラメーターを 1 つの要求で渡します。

以下の `curl` の例は、基本的な HTTP 認識要求を示します。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 非同期 HTTP インターフェースの使用
{: #basic-request-async}

[非同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-async)は、音声書き起こしのためのノンブロッキング・インターフェースです。 このインターフェースは、最初にサービスにコールバック URL を登録してもしなくても使用できます。 コールバック URL を指定すると、サービスはジョブ・ステータスと認識結果を含むコールバック通知を送信します。 このインターフェースは、ユーザー指定の秘密に基づく HMAC-SHA1 署名を使用して、通知のための認証とデータ保全性を確保します。 コールバック URL を指定しない場合は、ジョブ・ステータスと結果を確認するためにサービスをポーリングする必要があります。 いずれの場合でも、認証要求を行うには `POST /v1/recognitions` メソッドを使用します。

以下の `curl` の例は、単純な非同期 HTTP 認識要求を示します。 要求にはコールバック URL が指定されていないので、ジョブ・ステータスと結果の書き起こしを取得するためにサービスをポーリングする必要があります。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}
