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

# 非同期 HTTP インターフェース
{: #async}

{{site.data.keyword.speechtotextfull}} サービスの非同期 HTTP インターフェースには、サービスに対するノンブロッキング呼び出しで音声を書き起こすためのメソッドがあります。このインターフェースでは、ユーザー指定の秘密ストリングとデジタル署名が採用されており、HTTP プロトコルで行われる要求に対し、一定のレベルのセキュリティーを確保できるようになっています。非同期インターフェースを使用するには、次の操作を行います。
{: shortdesc}

-   ジョブのステータスおよび結果の通知をサービスから自動的に受け取るためのコールバック URL を登録します。
-   サービスをポーリングして、ジョブのステータスと結果を手動で取得します。

この 2 つの方法は相互に排他的ではありません。 コールバック通知を受信することを選択するとしても、それと同時に、サービスをポーリングして最新のステータスを確認したり、サービスにアクセスして結果を手動で取得したりすることができます。以降のセクションで、これらの各方法で非同期 HTTP インターフェースを使用する方法を説明します。

1 つの要求で 100 バイトから 1 GB までの音声データを送信します。音声フォーマットと、圧縮を使用して 1 つの要求で送信できる音声の量を最大化する方法について詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。インターフェースの個々のメソッドについて詳しくは、[API リファレンス![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/speech-to-text){: new_window}を参照してください。

## 使用モデル
{: #usage}

サービスの非同期 HTTP インターフェースを操作する際は、ジョブのステータスを確認したり、結果を受信したりするために、以下の方法を選択できます。

-   コールバック通知を使用する場合:
    1.  `POST /v1/register_callback` メソッドを呼び出して、コールバック URL をサービスに登録します。この URL に送信するコールバックの認証およびデータ保全性を実現するために、オプションでユーザー指定の秘密ストリングを指定できます。
    1.  ジョブのステータス変更時にサービスが通知を送信する宛先として登録済みのコールバック URL を指定して、`POST /v1/recognitions` メソッドを呼び出します。通知対象のイベントのリストを指定してください。 デフォルトでは、サービスはジョブの開始時、ジョブの完了時、およびエラーが発生した場合に通知を送信します。 完了の通知に要求の結果も含めるように要求することもできます。通知に結果を含めない場合は、`GET /v1/recognitions/{id}` メソッドを使用して結果を取得する必要があります。
-   サービスをポーリングする場合:
    1.  コールバック URL、イベント、ユーザー・トークンを指定せずに `POST /v1/recognitions` メソッドを呼び出します。
    1.  定期的に `GET /v1/recognitions` メソッドを呼び出して最近のジョブのステータスを確認するか、または `GET /v1/recognitions/{id}` メソッドを呼び出して特定のジョブのステータスを確認します。
    1.  `GET /v1/recognitions` メソッドでジョブのステータスを確認する場合は、ジョブが完了したら、`GET /v1/recognitions/{id}` メソッドを呼び出してジョブの結果を取得します。

この 2 つの方法は一緒に使用できます。コールバック URL を指定して作成したジョブでも、サービスをポーリングして、そのジョブの最新のステータスを取得することができます。 例えば、通知が到着するまでに時間がかかっている場合には、ジョブのステータスを手動で取得するとよいでしょう。また、サービスまたはネットワークのエラーが原因で 1 つ以上の通知を見逃した可能性がある場合にも、ステータスを確認できます。

## コールバック URL の登録
{: #register}

コールバック URL を登録するには、`POST /v1/register_callback` メソッドを呼び出します。 コールバック URL を登録した後は、そのコールバック URL を無制限の数のジョブの通知の受信に使用できます。 登録プロセスは、4 つのステップからなります。

1.  `POST /v1/register_callback` メソッドを呼び出してコールバック URL を渡します。オプションで、ユーザー指定の秘密を指定することもできます。このサービスでは認証およびデータ保全性を実現するために、その秘密を使用して、メッセージ認証のための鍵付きハッシング (HMAC) セキュア・ハッシュ・アルゴリズム 1 (SHA1) の署名を計算します。 以下に、URL `http://{user_callback_path}/results` で応答するユーザー・コールバックを登録する例を記載します。 この呼び出しには、ユーザーの秘密 `ThisIsMySecret` を含めます。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  コールバック URL がまだ登録されていない場合、サービスは URL を検証する  (ホワイトリストに登録する) ために、`GET` 要求をコールバック URL に送信します。要求の `challenge_string` クエリー・パラメーターにランダムな英数字のチャレンジ・ストリングを指定して渡します。この要求の `Accept` ヘッダーには、必要な応答タイプとして `text/plain` が指定されています。

    `register_callback` メソッドの呼び出しにユーザーの秘密が含まれている場合、サービスからの `GET` 要求には `X-Callback-Signature` ヘッダーも組み込まれます。このヘッダーにはチャレンジ・ストリングの HMAC-SHA1 署名が指定されます。この署名は、サービスがユーザーの秘密を鍵として使用して算出したものです。

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  サービスからの `GET` 要求に対し、ユーザーがステータス・コード 200 の応答を返します。応答に、サービスが送信したチャレンジ・ストリングを組み込みます。このストリングをプレーン・テキストとして応答の本体に組み込み、`Content-Type` 応答ヘッダーを `text/plain` に設定します。

    最初の `POST` 要求にユーザーの秘密が含まれている場合、その秘密を鍵として使用して、チャレンジ・ストリングの HMAC-SHA1 署名を計算できます。 サービスから `GET` 要求が送信された場合、計算した署名は、`X-Callback-Signature` ヘッダーで指定された値と一致します。

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  サービスは、送信した `GET`要求に対する応答の本体でチャレンジ・ストリングが返されているかどうかをチェックします。 返されている場合、サービスはコールバック URL をホワイトリストに登録し、元の `POST` 要求に対してステータス・コード 201 の応答を返します。 応答本体に含まれる JSON オブジェクトの `status` フィールドには値 `created` が指定され、`url` フィールドには値としてコールバック URL が指定されます。

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

サービスが登録プロセスでコールバック URL に送信する `GET` 要求は 1 つだけです。 サービスは、本体にチャレンジ・ストリングが組み込まれている応答コード 200 の応答を 5 秒以内に受信する必要があります。この応答を受信しないと、サービスは URL をホワイトリストに登録しません。サービスは、代わりに、`POST /v1/register_callback` 要求に対する応答としてステータス・コード 400 を送信します。コールバック URL が既に正常にホワイトリストに登録されていた場合、サービスは最初の `POST` 要求に対する応答としてステータス・コード 200 を送信します。

### セキュリティーに関する考慮事項
{: #security-async}

`POST /v1/register_callback` メソッドを使用してコールバック URL が正常に登録されると、サービスはその URL をホワイトリストに登録します。ホワイトリストに登録されたということは、その URL がコールバック通知で使用する URL として検証済みであることを意味します。また、登録の呼び出しでユーザーの秘密を指定した場合、ホワイトリストに登録されたということは、より強力なセキュリティーのためにその URL が検証済みであることを意味します。ユーザーの秘密を指定すると、非同期 HTTP インターフェースでコールバック URL を使用する要求の認証およびデータ保全性が実現されます。

サービスは、コールバック URL に送信する各コールバック通知のペイロード内の HMAC-SHA1 署名を計算するために、ユーザーの秘密を使用します。 サービスは各通知でその署名を `X-Callback-Signature`
ヘッダーに含めて送信します。 クライアントはこの秘密を使用して、各通知のペイロード内の自己の署名を計算できます。 署名が `X-Callback-Signature` ヘッダーの値と一致すれば、クライアントはその通知がサービスから送信されたものであること、そして送信中に通知の内容が変更されていないことを確認できます。 この確認により、クライアントが中間者攻撃の犠牲にならないことが保証されます。

実動アプリケーションには HTTPS が理想的です。 ただし、アプリケーション開発およびプロトタイピングの間は、このサービスでサポートされている HTTP ベースのコールバック通知を使用して HTTPS によるコストを回避することで、開発プロセスを簡素化し、促進することができます。

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### コールバック URL の登録解除
{: #unregister}

ホワイトリストに登録されている URL は、`POST /v1/unregister_callback` メソッドを呼び出すことでいつでも登録解除できます。コールバック URL の登録解除は、サービスを使用してアプリケーションをテストする際に役立ちます。登録解除したコールバック URL は、非同期認識要求では使用できなくなります。

## ジョブの作成
{: #create}

認識ジョブを作成するには、`POST /v1/recognitions` メソッドを呼び出します。 ジョブのステータスと結果を調べる方法は、以下のどちらの方法を使用するか、および呼び出しでどのパラメーターを渡すかによって決まります。

-   *コールバック通知を使用する場合*、コールバック通知の送信先 URL を指定する `callback_url` クエリー・パラメーターを含めます。ジョブのステータスが変わると、サービスはこの URL にコールバック通知を送信します。 オプションで、以下のクエリー・パラメーターを指定することもできます。
    -   `events`: サブスクライブする通知イベントのリストを指定します。 デフォルトでは、サービスはジョブの開始時 (`recognitions.started` イベント)、ジョブの完了時 (`recognitions.completed` イベント)、およびエラーが発生した場合 (`recognitions.failed` イベント) にコールバック通知を送信します。 これらのイベントのサブセットを指定したり、`recognitions.completed` イベントの代わりに `recognitions.completed_with_results` イベントを使用してジョブ完了の通知に結果を含めたりすることもできます。
    -   `user_token`: ジョブの各通知に含めるストリングを指定します。 無制限の数のジョブで同じコールバック URL を使用できるため、ユーザー・トークンを利用することで、異なるジョブの通知を区別できます。
-   *ポーリングを使用する場合*、`callback_url`、`events`、および `user_token` の各クエリー・パラメーターを省略します。 この場合、`GET /v1/recognitions` または `GET /v1/recognitions/{id}` メソッドを使用して、ジョブのステータスを確認する必要があります。ジョブの完了時に結果を取得する場合は、後者のメソッドを使用します。

いずれの場合も、`results_ttl` クエリー・パラメーターを含めることで、ジョブの完了後にその結果を取得可能な状態に維持する期間 (分数) を指定できます。

`POST /v1/recognitions` メソッドは、非同期インターフェースに固有の上記のパラメーターに加え、WebSocket インターフェースおよび同期 HTTP インターフェースで使用するパラメーターのほとんどをサポートしています。詳しくは、[パラメーターの要約](/docs/services/speech-to-text/summary.html)を参照してください。

### コールバック通知
{: #notifications}

コールバック URL を指定してジョブが作成されている場合、指定されたイベントが発生すると、サービスにより `POST` コールバック通知が登録済み URL に送信されます。基本的な通知の本文は、以下の構造の JSON オブジェクトで構成されます。

```javascript
{
  "id": "{job_id}",
   "event": "{recognitions_status}",
   "user_token": "{user_token}"
}
```
{: codeblock}

`id` フィールドはコールバックを生成したジョブの ID を示し、`event` フィールドはコールバックをトリガーしたイベントを示します。 ジョブのユーザー・トークンが指定されている場合、`user_token` フィールドに、そのユーザー・トークンが取り込まれます。ユーザー・トークンが指定されていなければ、このフィールドは空のストリングになります。イベントが `recognitions.completed_with_results` の場合、このオブジェクトには、認識要求の結果を示す `results` フィールドが含まれます。 コールバック通知に対し、クライアントがステータス・コード 200 で応答することがあります。

ユーザーの秘密を指定してコールバック URL が登録されている場合、サービスはコールバック通知で `X-Callback-Signature` ヘッダーも送信します。 このヘッダーには要求本体の HMAC-SHA1 署名が指定されます。この署名は、サービスがユーザーの秘密を鍵として使用して算出したものです。クライアントは、コールバック通知のペイロードの署名を計算することで、その署名がヘッダー内の署名と一致していることを確認できます。 例えば、以下の単純な Python コードは、ストリング `notification_payload` で署名を計算します。

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### コールバックの例
{: #callback}

以下に、ホワイトリストに登録済みのコールバック URL `http://{user_callback_path}/results` に関連付けられたジョブを作成する例を示します。 この例では、サービスから送信されるコールバック通知でこのジョブを識別するためにユーザー・トークン `job25` が渡されます。この呼び出しではデフォルトのイベントが使用されます。したがって、ジョブが完了したことを示すコールバック通知がサービスから送信されたら、ユーザーは `GET /v1/recognitions/{id}` メソッドを呼び出してジョブの結果を取得する必要があります。この呼び出しでは、認識要求の `timestamps` クエリー・パラメーターが `true` に設定されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

サービスは要求のステータスとして `waiting` を返します。これは、サービスが処理用にジョブを準備していることを意味します。 応答には、作成時刻とジョブ ID、およびジョブに関する詳細を取得するための URL が含まれます。

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
   "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
   "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
   "status": "waiting"
}
```
{: codeblock}

### ポーリングの例
{: #polling}

以下に、コールバック URL を関連付けずにジョブを作成する例を示します。 ユーザーは、ジョブが完了したことを確認するためにサービスをポーリングし、ジョブが完了したら `GET /v1/recognitions/{id}` メソッドを使用してジョブの結果を取得する必要があります。 前の例と同じように、この呼び出しでは、認識要求の `timestamps` パラメーターを `true` に設定しています。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

サービスは、ジョブを既に処理中であることを示すステータス `processing` を、作成時刻とジョブ ID、およびジョブに関する情報を取得するための URL とともに返します。

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
   "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
   "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
   "status": "processing"
}
```
{: codeblock}

## ジョブのステータスの確認と結果の取得
{: #job}

ジョブのステータスを確認するには、`id` パス・パラメーターに対象のジョブを指定して `GET /v1/recognitions/{id}` メソッドを呼び出します。この応答には常に、ジョブの ID、ステータス、作成時刻、および更新時刻が組み込まれます。ステータスが `completed` の場合、応答には認識要求の結果も組み込まれます。

以下の場合、ジョブの結果を取得するには、`GET /v1/recognitions/{id}` メソッドが唯一の手段になります。

-   コールバック URL を指定せずにジョブが実行依頼された場合。
-   コールバック URL を指定したが、`recognitions.completed_with_results` イベントを指定せずにジョブが実行依頼された場合。
-   ジョブが最新の 100 件の処理待ちジョブの 1 つではない場合。`id` パス・パラメーターを省略すると、最新の 100 件のジョブだけが返されます。

ただし、コールバック URL と `recognitions.completed_with_results` イベントが指定されていたジョブについても、このメソッドを使用して結果を取得することはできます。 ジョブの結果が取得可能な状態である間は、必要なだけ何度も結果を取得できます。ジョブとその結果は、`DELETE /v1/recognitions/{id}` メソッドで結果が削除されるか、あるいはジョブの存続時間が満了するかのうち、いずれか早いほうの時点まで取得可能な状態が維持されます。 `POST /v1/recognitions` メソッドの `results_ttl` パラメーターで存続時間を別の値に設定しない限り、結果はデフォルトで 1 週間後に期限切れになります。

### 結果が含まれないステータスの例
{: #withoutResults}

以下に、指定の ID のジョブのステータスを確認する例を示します。このジョブはまだ完了していないので、応答に結果は含まれません。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
         "created": "2016-08-17T19:13:23.622Z",
         "updated": "2016-08-17T19:13:24.434Z",
         "status": "processing"
}
```
{: codeblock}

### 結果が含まれるステータスの例
{: #withResults}

以下に、指定の ID のジョブのステータスを要求する例を示します。このジョブは完了しているため、応答には要求の結果が組み込まれます。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
         "results": [
            {
          "final": true,
               "alternatives": [
                  {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
                     "timestamps": [
                        [
                  "several",
                           1,
                           1.52
                ],
                [
                  "tornadoes",
                           1.52,
                           2.15
                ],
                . . .
                [
                  "Sunday",
                           5.74,
                           6.33
                        ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
   "created": "2016-08-17T19:11:04.298Z",
   "updated": "2016-08-17T19:11:16.003Z",
   "status": "completed"
}
```
{: codeblock}

## 最近のジョブのステータスの確認
{: #jobs}

最近のジョブのステータスを確認するには、`GET /v1/recognitions` メソッドを呼び出します。このメソッドは、呼び出しに使用されたサービス資格情報に関連付けられている最近の 100 件の処理待ちジョブのステータスを返します。このメソッドは、各ジョブの ID、ステータス、作成時刻、および更新時刻を返します。コールバック URL とユーザー・トークンを指定して作成されたジョブの場合、このメソッドはジョブのユーザー・トークンも返します。

応答には以下のいずれかのステータスが含まれています。

-   `waiting`: サービスが処理用にジョブを準備している場合。 このステータスは、すべてのジョブの初期状態です。サービスがジョブの処理を開始できるだけのキャパシティーを確保するまで、ジョブはこのステータスのままになります。
-   `processing`: サービスがアクティブにジョブを処理している場合。
-   `completed`: サービスがジョブの処理を完了した場合。 ジョブにコールバック URL とイベント `recognitions.completed_with_results` が指定されている場合、サービスはコールバック通知で結果を送信済みです。 そうでない場合は、`GET /v1/recognitions/{id}` メソッドを使用して結果を取得する必要があります。
-   `failed`: 何らかの理由でジョブが失敗した場合。

ジョブとその結果は、`DELETE /v1/recognitions/{id}` メソッドで結果が削除されるか、あるいはジョブの存続時間が満了するかのうち、いずれか早いほうの時点まで取得可能な状態が維持されます。

### 例
{: #statusExample}

以下に、呼び出し側のサービス資格情報に関連付けられている最近のジョブのステータスを要求する例を示します。このユーザーには、ステータスの異なる 3 つの処理待ちジョブがあります。最初のジョブは、コールバック URL とユーザー・トークンを指定して作成されています。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
      {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
         "created": "2016-08-17T19:15:17.926Z",
         "updated": "2016-08-17T19:15:17.926Z",
         "status": "waiting",
         "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
         "created": "2016-08-17T19:13:23.622Z",
         "updated": "2016-08-17T19:13:24.434Z",
         "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
         "created": "2016-08-17T19:11:04.298Z",
         "updated": "2016-08-17T19:11:16.003Z",
         "status": "completed"
      }
  ]
}
```
{: codeblock}

## ジョブの削除
{: #delete-async}

ジョブを削除するには、`id` パス・パラメーターに対象のジョブを指定して `DELETE /v1/recognitions/{id}` メソッドを使用します。通常は、サービスからジョブの結果を取得した後でジョブを削除します。ジョブを削除すると、そのジョブの結果を入手できなくなります。 サービスがアクティブに処理中のジョブを削除することはできません。

デフォルトでは、ジョブの有効期限が切れるまで、サービスは各ジョブの結果を維持します。デフォルトの存続時間は 1 週間ですが、`POST /v1/recognitions` メソッドの `results_ttl` パラメーターを使用して、サービスが結果を維持する期間 (分数) を指定できます。

### 例
{: #deleteExample-async}

以下に、指定の ID を持つジョブを削除する例を示します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
