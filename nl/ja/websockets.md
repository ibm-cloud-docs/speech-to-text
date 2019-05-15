---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# WebSocket インターフェース
{: #websockets}

{{site.data.keyword.speechtotextshort}} サービスの WebSocket インターフェースは、クライアントがサービスと対話するための最も自然な方法です。音声認識のために WebSocket インターフェースを使用するには、最初に `/v1/recognize` メソッドを使用して、サービスとの持続的な接続を確立します。その後は、接続を介してテキストおよびバイナリー・メッセージを送信して認識要求を開始し、管理します。
{: shortdesc}

認識要求と応答のサイクルには、以下のステップが含まれます。

1.  [接続のオープン](#WSopen)
1.  [認識要求の開始](#WSstart)
1.  [音声の送信および認識結果の受信](#WSaudio)
      
1.  [認識要求の終了](#WSstop)
1.  [追加要求の送信と要求パラメーターの変更](#WSmore)
1.  [接続存続の維持](#WSkeep)
1.  [接続のクローズ](#WSclose)

クライアントは、データをサービスに送信する際に、すべての JSON メッセージをテキスト・メッセージとして、またすべての音声データをバイナリー・メッセージとして渡す*必要があります*。

以降のコード例のスニペットは、JavaScript で作成されており、HTML5 WebSocket API に基づいています。WebSocket プロトコルについて詳しくは、Internet Engineering Task Force (IETF) の [Request for Comment (RFC) 6455 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://tools.ietf.org/html/rfc6455){: new_window} を参照してください。
{: note}

## 接続のオープン
{: #WSopen}

{{site.data.keyword.speechtotextshort}} サービスは WebSocket Secure (WSS) プロトコルを使用して、以下のエンドポイントで `/v1/recognize` メソッドを使用可能にします。

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

ここで、`{host_name}` はアプリケーションがホストされている場所です。

-   `stream.watsonplatform.net` (ダラス) (以下の例ではこのホスト名を使用します)
-   `stream-fra.watsonplatform.net` (フランクフルト)
-   `gateway-syd.watsonplatform.net` (シドニー)
-   `gateway-wdc.watsonplatform.net` (ワシントン DC)
-   `gateway-tok.watsonplatform.net` (東京)
-   `gateway-lon.watsonplatform.net` (ロンドン)

WebSocket クライアントは、以下の照会パラメーターを指定してこのメソッドを呼び出して、サービスとの認証済み接続を確立します。ID およびアクセス管理 (IAM) 認証を使用する場合、`access_token` 照会パラメーターを使用します。Cloud Foundry サービス資格情報を使用する場合、`watson-token` 照会パラメーターを使用します。

<table>
  <caption>表 1. <code>/v1/recognize</code> メソッドの
    パラメーター</caption>
  <tr>
    <th style="width:23%; text-align:left">パラメーター</th>
    <th style="width:12%; text-align:center">データ型</th>
    <th style="text-align:left">説明</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      <em>IAM 認証を使用する場合は、</em>
有効な IAM アクセス・トークンを渡してサービスとの認証接続を確立します。
呼び出しでは API キーではなく、IAM アクセス・トークンを渡します。
アクセス・トークンの有効期限が切れる前に接続を確立する必要があります。
アクセス・トークンを使用する方法の詳細については、
[IAM トークンによる認証](/docs/services/watson/getting-started-iam.html)を参照してください。<br/><br/>
アクセス・トークンを渡すのは、認証済み接続を確立する際のみです。接続が確立されると、その接続を無期限に存続させることができます。
      接続をオープンのままにし続ける限り、認証された状態が維持されます。
トークンの有効期限を過ぎてなお存続しているアクティブな接続については、アクセス・トークンを更新する必要はありません。
</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      <em>Cloud Foundry サービス資格情報を使用する場合は、</em>
有効な {{site.data.keyword.watson}} 認証トークンを渡してサービスとの認証接続を確立します。
呼び出しではサービス資格情報ではなく、{{site.data.keyword.watson}} トークンを渡します。{{site.data.keyword.watson}} トークンは、HTTP 基本認証に `username` および `password`を使用する Cloud Foundry サービス資格情報に基づいています。{{site.data.keyword.watson}} トークンの取得方法については、</docs/services/watson/getting-started-tokens.html>{{site.data.keyword.watson}}を参照してください。
<br/><br/>
{{site.data.keyword.watson}} トークンを渡すのは、認証済み接続を確立する際のみです。接続が確立されると、その接続を無期限に存続させることができます。 接続をオープンのままにし続ける限り、認証された状態が維持されます。
</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
書き起こしで使用する言語モデルを指定します。モデルを指定しなかった場合、デフォルトで <code>en-US_BroadbandModel</code> モデルが使用されます。 詳しくは、[言語とモデル](/docs/services/speech-to-text/models.html)を参照してください。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
この接続を介して送信されるすべての要求で使用するカスタム言語モデルの GUID (Globally Unique Identifier) を指定します。カスタム言語モデルの基本モデルは、<code>model</code> パラメーターの値と一致する必要があります。デフォルトでは、カスタム言語モデルは使用されません。 詳しくは、[カスタマイズ・インターフェース](/docs/services/speech-to-text/custom.html)を参照してください。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
この接続を介して送信されるすべての要求で使用するカスタム音響モデルの GUID (Globally Unique Identifier) を指定します。カスタム音響モデルの基本モデルは、<code>model</code> パラメーターの値と一致する必要があります。デフォルトでは、カスタム音響モデルは使用されません。 詳しくは、[カスタマイズ・インターフェース](/docs/services/speech-to-text/custom.html)を参照してください。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
この接続を介して送信されるすべての要求で使用する基本`モデル`のバージョンを指定します。このパラメーターは、主に新しい基本モデル用にアップグレードされたカスタム・モデルでの使用を目的としています。デフォルト値は、このパラメーターをカスタム・モデルと共に使用するかどうかによって異なります。詳しくは、[基本モデルのバージョン](/docs/services/speech-to-text/input.html#version)を参照してください。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">Boolean</td>
    <td style="text-align:left">
この接続で送信される要求と結果がサービスによってログに記録されるかどうかを指定します。一般的なサービス改善の目的で IBM がお客様のデータにアクセスすることを阻止するには、このパラメーターに対して <code>true</code> を指定します。詳しくは、[要求ロギング](/docs/services/speech-to-text/input.html#logging)を参照してください。</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
接続を介して渡されるすべてのデータにカスタマー ID を関連付けます。このパラメーターは引数 <code>customer_id={id}</code> を受け入れます。ここで、<code>id</code> はデータに関連付けられるランダムまたは汎用の文字列です。
パラメーターの引数は URL エンコードにする必要があります。例: `customer_id%3dmy_customer_ID`。
デフォルトでは、データにカスタマー ID は関連付けられません。詳しくは、[機密保護](/docs/services/speech-to-text/information-security.html)を参照してください。
    </td>
  </tr>
</table>

以下の JavaScript コード・スニペットでは、サービスとの接続をオープンしています。 `/v1/recognize` メソッドの呼び出しで、`access_token` および `model` クエリー・パラメーターが渡されています。後者では、スペイン語の広帯域モデルを使用するようにサービスに指示しています。接続の確立後に、クライアントはサービスからのイベントに対応するためのイベント・リスナー (`onOpen`、`onClose` など) を定義します。クライアントは複数の認識要求に接続を使用できます。

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

## 認識要求の開始
{: #WSstart}

認識要求を開始するために、クライアントは確立済みの接続を介してサービスに JSON テキスト・メッセージを送信します。 クライアントは、書き起こし対象の音声を送信する前に、このメッセージを送信する必要があります。このメッセージには、`action` パラメーターが含まれている必要がありますが、`content-type` パラメーターは通常は省略しても構いません。

<table>
  <caption>表 2. JSON テキスト・メッセージのパラメーター</caption>
  <tr>
    <th style="width:23%; text-align:left">パラメーター</th>
    <th style="width:12%; text-align:center">データ型</th>
    <th style="text-align:left">説明</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>必須</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
実行するアクションを以下のように指定します。
<ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> は認識要求を開始するか、後続の要求に対する新しいパラメーターを指定します。
詳しくは、[追加要求の送信と要求パラメーターの変更](#WSmore)を参照してください。</li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> は要求のすべての音声が送信されたことを通知します。
詳しくは、[認識要求の終了](#WSstop)を参照してください。
</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>オプション</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
要求の音声データのフォーマット (MIME タイプ) を識別します。
`audio/alaw`、`audio/basic`、`audio/l16`、および `audio/mulaw` のフォーマットの場合、このパラメーターは必須です。詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。
    </td>
  </tr>
</table>

メッセージには、要求の処理方法に関する他の局面や、返す情報を指定するオプション・パラメーターを含めることもできます。すべての入力機能と出力機能については、[パラメーターの要約](/docs/services/speech-to-text/summary.html)を参照してください。言語モデル、カスタム言語モデル、およびカスタム音響モデルは、WebSocket URL のクエリー・パラメーターとしてのみ指定できます。

以下の JavaScript コード・スニペットでは、WebSocket 接続を介して認識要求の初期設定パラメーターを送信しています。 呼び出しは、クライアントの `onOpen` 関数に組み込まれて、接続の確立後にのみ送られるようになっています。

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

サービスは、要求を正常に受信すると、`listening` 状態であることを示す以下のテキスト・メッセージを返します。

```javascript
{'state': 'listening'}
```
{: codeblock}

`listening` 状態は、サービス・インスタンスが設定されており（JSON `start` メッセージが有効)、認識要求の新しい発話を処理する準備ができていることを示します。サービスはリスニングを開始すると、`listening` メッセージの前に送信されたすべての音声を処理します。

クライアントが認識要求に無効なクエリー・パラメーターまたは JSON フィールドを指定した場合、サービスの JSON 応答には、`warnings` フィールドが含まれます。このフィールドはそれぞれの無効な引数について説明します。警告に関係なく要求は成功します。

## 音声の送信および認識結果の受信
{: #WSaudio}

クライアントは初期 `start` メッセージを送信した後に、サービスへの音声データの送信を開始できます。クライアントは、サービスが `start` メッセージに `listening` メッセージで応答するのを待機する必要はありません。サービスは、HTTP インターフェースで書き起こしの結果を返す場合と同じフォーマットを使用して、非同期的に書き起こしの結果を返します。

クライアントは、音声をバイナリー・データとして送信する必要があります。 クライアントは、1 回の発話で (`send` 要求ごとに) 最大 100 MB の音声データを送信できます。どんな要求に対しても少なくとも 100 バイトの音声を送信する必要があります。クライアントは単一の WebSocket 接続で複数の発話を送信できます。要求でサービスに渡すことができる音声の量を最大化するための圧縮の使用については、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。

WebSocket インターフェースでは、最大 4 MB のフレーム・サイズ制限が課されます。 クライアントは、最大フレーム・サイズを 4 MB 未満に設定できます。フレーム・サイズを設定することが実際的でない場合、クライアントは最大メッセージ・サイズを 4 MB 未満に設定し、音声データを一連のメッセージとして送信できます。

以下の JavaScript コード・スニペットでは、バイナリー・メッセージ (blob) として音声データをサービスに送信しています。

```javascript
websocket.send(blob);
```
{: codeblock}

以下のスニペットでは、非同期的にサービスが返す認識の仮説を受け取っています。 この結果は、クライアントの `onMessage` 関数によって処理されます。

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## 認識要求の終了
{: #WSstop}

サービスへの要求の音声データの送信が完了したら、クライアントは、以下の方法のいずれかで、バイナリー音声の送信の終了をサービスに通知する*必要があります*。

-   `action` パラメーターを値 `stop` に設定した JSON テキスト・メッセージを送信する。

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   blob が指定されていない、空のバイナリー・メッセージを送信する。

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

このサービスは、音声の送信が完了したという確認を受信するまで、最終結果を送信しません。送信の完了が通知されないと、サービスが最終結果を送信しないまま接続がタイムアウトする可能性があります。

複数の認識要求の間で最終結果を受信するには、クライアントは、後続の要求を送信する前に、直前の要求の送信の終了を通知する必要があります。サービスは、最初の要求の最終結果を返した後に、別の `{"state":"listening"}` メッセージをクライアントに返します。このメッセージは、サービスで別の要求を受信する準備ができていることを示します。

## 追加要求の送信と要求パラメーターの変更
{: #WSmore}

WebSocket 接続がアクティブである間、クライアントはその接続を引き続き使用して、新しい音声の認識要求をさらに送信できます。デフォルトでは、サービスは、同じ接続を介して送信される後続のすべての要求に対して、直前の `start` メッセージで送信されたパラメーターを引き続き使用します。

後続の要求のパラメーターを変更する場合は、クライアントは、サービスから最終認識結果および `{"state":"listening"}` メッセージを受信した後に、新しいパラメーターを指定した別の `start` メッセージを送信できます。クライアントは、接続のオープン時に指定されたパラメーター (`model`、`language_customization_id` など) 以外の任意のパラメーターを変更できます。

次の例は、接続を介して送信される後続の認識要求に対する新しいパラメーターを指定して `start` メッセージを送信する例です。このメッセージは、直前の例と同じ `content-type` を指定していますが、書き起こしの単語の信頼度とタイム・スタンプを返すようにサービスに指示しています。

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## 接続存続の維持
{: #WSkeep}

サービスは、非アクティブ・タイムアウトまたはセッション・タイムアウトに達すると、セッションを終了し、接続をクローズします。

-   *非アクティブ・タイムアウト* が発生するのは、音声がクライアントから送信されているが、サービスで発話なし状態が検出された場合です。 非アクティブ・タイムアウトは、デフォルトで 30 秒に設定されています。`inactivity_timeout` パラメーターを使用して、`-1` (タイムアウトを無期限に設定する) などの別の値を指定できます。
-   *セッション・タイムアウト*が発生するのは、 30 秒間サービスがクライアントからデータを受信しなかった、または中間結果を送信しなかった場合です。このタイムアウトの長さは変更できませんが、タイムアウトが発生する前に、単なる無音を含む任意の音声データをサービスに送信することでセッションを延長できます。さらに `inactivity_timeout` を `-1` に設定する必要もあります。セッションを延長するために送信した無音を含め、サービスにデータを送信した期間に対して課金されます。

詳しくは、[タイムアウト](/docs/services/speech-to-text/input.html#timeouts)を参照してください。

## 接続のクローズ
{: #WSclose}

クライアントは、サービスとの対話が完了した場合、WebSocket 接続をクローズすることができます。接続がクローズされると、クライアントはその接続を使用して要求を送信したり結果を受信したりすることができなくなります。接続をクローズするのは、要求に対するすべての結果を受け取った後だけにしてください。接続を明示的にクローズしなかった場合、接続は最終的にはタイムアウトになってクローズされます。

以下の JavaScript コード・スニペットでは、オープン接続をクローズしています。

```javascript
websocket.close();
```
{: codeblock}

## WebSocket 戻りコード
{: #WSreturn}

サービスは、WebSocket 接続を介して、以下の戻りコードをクライアントに送信できます。

-   `1000`。接続の正常なクローズを示し、接続が確立された目的が満たされたことを意味します。
-   `1002`。プロトコル・エラーのため、サービスが接続をクローズすることを示します。
-   `1006`。接続が異常にクローズされたことを示します。
-   `1009`。フレーム・サイズが 4 MB の制限を超えたことを示します。
-   `1011`。要求を満たすことができなくなる予期しない状態が発生したため、サービスが接続を終了することを示します。

ソケットがエラーでクローズされる場合、クライアントは、ソケットがクローズされる前に `{"error":"{message}"}` という形式の情報メッセージを受信します。WebSocket 戻りコードについて詳しくは、[IETF RFC 6455![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://tools.ietf.org/html/rfc6455){: new_window} を参照してください。

## WebSocket セッションの例
{: #WSexample}

次の例は、クライアントと {{site.data.keyword.speechtotextshort}} サービスの間の WebSocket セッションを示しています。フォローしやすくするために、セッションを 3 つの個別のやり取りとして示しています。しかし、これらの 3 つのやり取りはすべて、サービスとの単一のセッションの一部です。この例では、メッセージのやり取りに焦点を絞っており。接続のオープンおよびクローズの部分は示していません。

### 最初のやり取りの例
{: #firstExample}

最初のやり取りでは、クライアントは、ストリング「`Name the Mayflower`」が含まれている音声を送信しています。クライアントは、PCM (`audio/l16`) 音声データの 1 つのチャンクを含んだバイナリー・メッセージを送信しています (そのために必要なサンプリング・レートを指定しています)。クライアントは、サービスからの `{"state":"listening"}` 応答を待機せずに、音声データの送信を開始し、要求の終了を通知しています。データを即時に送信することで、待ち時間が削減されます。これは、サービスで認識要求の処理の準備ができたときにすぐに音声を使用できるからです。

-   *クライアント*は以下を送信します。

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *サービス*は以下のように応答します。

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 2 番目のやり取りの例
{: #secondExample}

2 番目のやり取りでは、クライアントは、ストリング「`Second audio transcript`」が含まれている音声を送信しています。クライアントは、単一のバイナリー・メッセージで音声を送信し、最初の要求で指定したのと同じパラメーターを使用しています。

-   *クライアント*は以下を送信します。

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *サービス*は以下のように応答します。

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### 3 番目のやり取りの例
{: #thirdExample}

3 番目のやり取りでは、クライアントは、再度、ストリング「`Name the Mayflower`」が含まれている音声を送信しています。クライアントは、PCM 音声データの 1 つのチャンクを含んだバイナリー・メッセージを再度送信しています。ただし、今回は、クライアントは、書き起こしの中間結果を要求しています。

-   *クライアント*は以下を送信します。

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   *サービス*は以下のように応答します。

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
