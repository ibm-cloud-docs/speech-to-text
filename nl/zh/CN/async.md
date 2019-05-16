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

# 异步 HTTP 接口
{: #async}

{{site.data.keyword.speechtotextfull}} 服务的异步 HTTP 接口提供了多个方法，用于通过对服务的非阻塞调用来转录音频。该接口使用用户指定的私钥字符串和数字签名，为通过 HTTP 协议发出的请求提供安全级别。要使用异步接口，可以执行以下操作：
{: shortdesc}

-   注册回调 URL，以自动收到服务关于作业状态和结果的通知。
-   轮询服务以手动获取作业状态和结果。

这两种方法并不是互斥的。您可以选择接收回调通知，但仍轮询服务以获取最新状态，或者访问服务以手动检索结果。以下各部分描述了如何将异步 HTTP 接口与任一方法配合使用。

一个请求中提交的音频数据最大为 1 GB，最小为 100 字节。有关音频格式的信息以及有关使用压缩来最大化可以通过请求发送的音频量的信息，请参阅[音频格式](/docs/services/speech-to-text/audio-formats.html)。有关该接口的各个方法的更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/speech-to-text){: new_window}。

## 使用情况模型
{: #usage}

使用服务的异步 HTTP 接口时，可以选择通过以下方式来了解作业状态和接收结果：

-   使用回调通知：
    1.  调用 `POST /v1/register_callback` 方法向服务注册回调 URL。可以提供可选的用户指定私钥字符串，以便为发送到 URL 的回调启用认证和数据完整性。
    1.  调用 `POST /v1/recognitions` 方法，其中包含服务在作业状态更改时会向其发送通知的已注册回调 URL。指定要通知的事件的列表。缺省情况下，服务会在作业启动时、作业完成时以及发生错误时发送通知。还可以要求在完成通知中提供请求的结果。否则，需要使用 `GET /v1/recognitions/{id}` 方法来检索结果。
-   轮询服务：
    1.  调用不包含回调 URL、事件或用户令牌的 `POST /v1/recognitions` 方法。
    1.  定期调用 `GET /v1/recognitions` 方法来检查最新作业的状态，或定期调用 `GET /v1/recognitions/{id}` 方法来检查特定作业的状态。
    1.  如果使用 `GET /v1/recognitions` 方法来检查作业状态，请调用 `GET /v1/recognitions/{id}` 方法以在作业完成后检索作业的结果。

这两种方法可以一起使用。您仍可以轮询服务，以获取使用回调 URL 创建的作业的最新状态。例如，如果通知需要一段时间才能到达，您可能希望获取作业的状态。如果您怀疑由于服务或网络错误而错过了一个或多个通知，那么还可以检查状态。

## 注册回调 URL
{: #register}

通过调用 `POST /v1/register_callback` 方法可注册回调 URL。注册回调 URL 后，可以将其用于接收无限数量的作业的通知。注册过程包括四个步骤：

1.  调用 `POST /v1/register_callback` 方法并传递回调 URL。（可选）您还可以指定用户指定的私钥。服务使用此私钥来计算密钥散列消息认证代码 (HMAC) 安全散列算法 1 (SHA1) 签名，以进行认证和实现数据完整性。以下示例会注册在 URL `http://{user_callback_path}/results` 上响应的用户回调。该调用包含用户私钥 `ThisIsMySecret`。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  如果回调 URL 尚未注册，服务会尝试通过向该回调 URL 发送 `GET` 请求来对其进行验证或将其列入白名单。服务通过请求的 `challenge_string` 查询参数来传递随机字母数字质询字符串。请求包含 `Accept` 头，用于指定 `text/plain` 作为必需的响应类型。

    如果对 `register_callback` 方法的调用包含用户私钥，那么来自服务的 `GET` 请求还会包含 `X-Callback-Signature` 头，用于指定质询字符串的 HMAC-SHA1 签名。服务会将用户私钥用作密钥来计算该签名。

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  使用状态码 200 对来自服务的 `GET` 请求进行响应。请包含服务在响应中发送的质询字符串。在响应主体中包含纯文本格式的字符串，并将 `Content-Type` 响应头设置为 `text/plain`。

    如果初始 `POST` 请求包含用户私钥，那么可以将该私钥用作密钥来计算质询字符串的 HMAC-SHA1 签名。如果服务发送了 `GET` 请求，那么签名会与 `X-Callback-Signature` 头指定的值相匹配。

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  服务检查质询字符串是否在对其 `GET` 请求的响应的主体中返回。如果是，服务会将该回调 URL 列入白名单，并使用状态码 201 响应原始 `POST` 请求。响应主体包含一个 JSON 对象，其中 `status` 字段的值为 `created`，`url` 字段为该回调 URL 的值。

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

在注册过程中，服务仅向回调 URL 发送一个 `GET` 请求。服务必须在 5 秒内收到包含响应代码 200 并且主体中含有质询字符串的回复。否则，服务不会将该 URL 列入白名单，而改为发送状态码 400 来响应 `POST /v1/register_callback` 请求。如果回调 URL 已成功列入白名单，那么服务会发送状态码 200 来响应初始 `POST` 请求。

### 安全注意事项
{: #security-async}

成功使用 `POST /v1/register_callback` 方法注册了回调 URL 时，服务会将该 URL 列入白名单，以指示该 URL 已经过验证，可用于回调通知。如果在注册调用中指定了用户私钥，那么列入白名单还意味着该 URL 已经过验证，增加了安全性。如果指定用户私钥，可为通过异步 HTTP 接口使用回调 URL 的请求提供认证功能和数据完整性。

服务使用用户私钥，基于向该 URL 发送的每个回调通知的有效内容来计算 HMAC-SHA1 签名。服务通过每个通知中的 `X-Callback-Signature` 头来发送签名。客户机可以使用私钥来计算其自己针对每个通知有效内容的签名。如果其签名与 `X-Callback-Signature` 头的值相匹配，那么客户机就可确定通知是由服务发送的，并且其内容在传输期间未变更。此确认可保证客户机不是中间人攻击的受害者。

对于生产应用程序，HTTPS 最为理想。但是，在应用程序开发和原型设计期间，服务支持的基于 HTTP 的回调通知可通过避免 HTTPS 的开销，从而简化并加速开发过程。

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### 注销回调 URL
{: #unregister}

您可以随时通过调用 `POST /v1/unregister_callback` 方法来注销列入白名单的回调 URL。注销回调 URL 对于使用服务测试应用程序可能非常有用。注销回调 URL 后，即无法再将其用于异步识别请求。

## 创建作业
{: #create}

通过调用 `POST /v1/recognitions` 方法可创建识别作业。如何了解作业的状态和结果取决于您使用的方法以及传递的参数：

-   *要使用回调通知*，请包含 `callback_url` 查询参数，用于指定服务在作业状态更改时向其发送回调通知的 URL。您还可以指定以下可选查询参数：
    -   `events`，用于预订通知事件的列表。缺省情况下，作业启动时（`recognitions.started` 事件）、作业完成时（`recognitions.completed` 事件）和发生错误时（`recognitions.failed` 事件），服务会发送回调通知。可以指定事件的子集，或者使用 `recognitions.completed_with_results` 事件（而不是 `recognitions.completed` 事件）以将结果包含在作业完成通知中。
    -   `user_token`，用于指定要包含在作业的每个通知中的字符串。由于可以将同一回调 URL 用于无限数量的作业，因此可以利用用户令牌来区分不同作业的通知。
-   *要使用轮询*，请省略 `callback_url`、`events` 和 `user_token` 查询参数。然后，必须使用 `GET /v1/recognitions` 或 `GET /v1/recognitions/{id}` 方法来检查作业的状态，使用后一种方法可在作业完成时检索结果。

对于这两种方法，都可以包含 `results_ttl` 查询参数，用于指定在作业完成后结果将保持可用的时间（以分钟为单位）。

除了先前特定于异步接口的参数外，`POST /v1/recognitions` 方法支持的大部分参数与 WebSocket 和同步 HTTP 接口的相同。有关更多信息，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。

### 回调通知
{: #notifications}

如果创建作业时使用了回调 URL，那么服务会在发生指定事件时，向注册的 URL 发送 HTTP `POST` 回调通知。基本通知的主体包含一个 JSON 对象，其结构如下：

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

`id` 字段标识生成了回调的作业的标识，`event` 字段标识触发了回调的事件。如果指定了作业的用户令牌，那么 `user_token` 字段会包含此令牌。否则，此字段为空字符串。如果事件为 `recognitions.completed_with_results`，那么该对象会包含 `results` 字段，用于提供识别请求的结果。客户机可以使用状态码 200 来响应回调通知。

如果注册回调 URL 时使用了用户私钥，那么服务还会在回调通知中发送 `X-Callback-Signature` 头。此头指定请求主体的 HMAC-SHA1 签名。服务会将用户私钥用作密钥来计算该签名。客户机可以计算回调通知的有效内容的签名，以确保它与此头中的签名相匹配。例如，以下简单 Python 代码基于字符串 `notification_payload` 计算签名：

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### 回调示例
{: #callback}

以下示例创建与先前列入白名单的回调 URL `http://{user_callback_path}/results` 关联的作业。此示例传递用户令牌 `job25` 以在服务发送的回调通知中标识作业。此调用使用的是缺省事件，因此用户必须调用 `GET /v1/recognitions/{id}` 方法，以在服务发送回调通知指示作业完成时检索结果。此调用将识别请求的 `timestamps` 查询参数设置为 `true`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

服务返回了请求的状态 `waiting`，指示服务正在准备作业以供处理。响应包含作业创建时间、作业标识和用于获取作业更多相关信息的 URL。

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### 轮询示例
{: #polling}

以下示例创建未与回调 URL 关联的作业。用户必须轮询服务来了解作业何时完成，然后使用 `GET /v1/recognitions/{id}` 方法来检索结果。与先前的示例类似，此调用也将识别请求的 `timestamps` 参数设置为 `true`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

服务返回了 `processing` 状态，指示服务已经在处理作业，同时返回了作业创建时间、作业标识和用于获取作业相关信息的 URL。

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## 检查作业状态和检索作业结果
{: #job}

调用 `GET /v1/recognitions/{id}` 方法来检查使用 `id` 路径参数指定的作业的状态。响应始终包含作业标识、作业状态以及作业创建时间和更新时间。如果状态为 `completed`，那么响应还会包含识别请求的结果。

在以下情况下，只有使用 `GET /v1/recognitions/{id}` 方法才能检索作业结果：

-   提交作业时未使用回调 URL。
-   提交作业时使用了回调 URL，但未指定 `recognitions.completed_with_results` 事件。
-   作业不属于 100 个最新的未完成作业。省略 `id` 路径参数时，仅返回 100 个最新作业。

但是，您仍可以使用此方法来检索指定了回调 URL 和 `recognitions.completed_with_results` 事件的作业的结果。可以根据需要多次检索任何作业的结果，只要这些结果仍然可用。作业及其结果会保持可用，直到您使用 `DELETE /v1/recognitions/{id}` 方法将其删除，或者作业的生存时间到期，两者以最先发生的时间为准。缺省情况下，除非使用 `POST /v1/recognitions` 方法的 `results_ttl` 参数不指定了不同的生存时间，否则结果会在一周后到期。

### 没有结果的状态示例
{: #withoutResults}

以下示例检查具有指定标识的作业的状态。该作业尚未完成，因此响应未包含结果。

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

### 具有结果的状态示例
{: #withResults}

以下示例请求具有指定标识的作业的状态。该作业已完成，因此响应包含请求的结果。

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

## 检查最新作业的状态
{: #jobs}

调用 `GET /v1/recognitions` 方法来检查最新作业的状态。此方法返回与用于调用它的服务凭证关联的最近 100 个未完成作业的状态。此方法返回每个作业的标识和状态以及作业创建时间和更新时间。如果创建作业时使用了回调 URL 和用户令牌，那么此方法还会返回作业的用户令牌。

响应包含下列其中一种状态：

-   `waiting` 表示服务正在准备作业以供处理。此状态是所有作业的初始状态。作业会一直处于此状态，直到服务有能力开始处理该作业为止。
-   `processing` 表示服务正在积极处理作业。
-   `completed` 表示服务已完成作业的处理。如果作业指定了回调 URL 和 `recognitions.completed_with_results` 事件，那么服务会随回调通知一起发送结果。否则，请使用 `GET /v1/recognitions/{id}` 方法来获取结果。
-   `failed` 表示作业由于某种原因而失败。

作业及其结果会保持可用，直到您使用 `DELETE /v1/recognitions/{id}` 方法将其删除，或者作业的生存时间到期，两者以最先发生的时间为准。

### 示例
{: #statusExample}

以下示例请求与调用者的服务凭证关联的最新当前作业的状态。用户有三个未完成作业，分别处于不同的状态。第一个作业创建时使用了回调 URL 和用户令牌。

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

## 删除作业
{: #delete-async}

可以使用 `DELETE /v1/recognitions/{id}` 方法来删除使用 `id` 路径参数指定的作业。通常，在从服务获取作业的结果后，会删除该作业。删除作业后，其结果将不再可用。无法删除服务正在积极处理的作业。

缺省情况下，服务会保留每个作业的结果，直到作业的生存时间到期。缺省生存时间为一周，但您可以使用 `POST /v1/recognitions` 方法的 `results_ttl` 参数来指定服务保留结果的时间（以分钟为单位）。

### 示例
{: #deleteExample-async}

以下示例删除具有指定标识的作业：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
