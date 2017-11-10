---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-07"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# The asynchronous HTTP interface
{: #async}

The asynchronous HTTP interface of the {{site.data.keyword.speechtotextshort}} service provides methods for transcribing audio via non-blocking calls to the service. The interface lets you employ user-specified secret strings and digital signatures to provide a level of security for requests made over the HTTP protocol. You can use the asynchronous interface in one of two ways:
{: shortdesc}

-   Register a callback URL to be notified by the service of the job status and, optionally, the results automatically.
-   Poll the service to obtain the job status and the results manually.

The two approaches are not mutually exclusive; you can elect to receive callback notifications but still poll the service for the latest status or contact the service to retrieve results manually. The following sections describe how to use the asynchronous HTTP interface with either approach. For detailed information about the individual methods of the interface, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

## Usage models
{: #usage}

When working with the service's asynchronous HTTP interface, you can elect to learn about job status and receive results in the following ways:

-   By using callback notifications:
    1.  Call the `POST /v1/register_callback` method to register a callback URL with the service. You can provide an optional user-specified secret string to enable authentication and data integrity for callbacks sent to the URL.
    1.  Call the `POST /v1/recognitions` method with an already registered callback URL to which the service sends notifications when the status of the job changes. You specify a list of events of which to be notified. By default, the service sends notifications when a job is started, when it is complete, and if an error occurs. You can request that the completion notification also include the results of the request; otherwise, you need to use the `GET /v1/recognitions/{id}` method to retrieve the results.
-   By polling the service:
    1.  Call the `POST /v1/recognitions` method without a callback URL, events, or a user token.
    1.  Periodically call the `GET /v1/recognitions` method to check the status of all of your current jobs or the `GET /v1/recognitions/{id}` method to check the status of the specific job.
    1.  If you check job status with the `GET /v1/recognitions` method, call the `GET /v1/recognitions/{id}` method to retrieve the results of the job once it is complete.

As mentioned previously, the two approaches are not mutually exclusive. You can still poll the service to obtain the latest status for a job that is created with a callback URL. For example, you may want to obtain the status of a job if notifications are taking a while to arrive or if you suspect that you may have missed one or more notifications due to a service or network error.

## Registering a callback URL
{: #register}

You register a callback URL by calling the `POST /v1/register_callback` method. Once you register a callback URL, you can use it to receive notifications for an indefinite number of jobs. The registration process comprises four steps:

1.  You call the `POST /v1/register_callback` method and pass a callback URL and, optionally, a user-specified secret. The service uses the secret to compute keyed-hash message authentication code (HMAC) Secure Hash Algorithm 1 (SHA1) signatures for authentication and data integrity. The following example registers a user callback that responds at the URL `http://{user_callback_path}/results`. The call includes a user secret of `ThisIsMySecret`.

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  The service attempts to validate, or white-list, the callback URL if it is not already registered by sending a `GET` request to the callback URL. The service passes a random alphanumeric challenge string via the `challenge_string` query parameter of the request. The request includes an `Accept` header that specifies `text/plain` as the required response type.

    If the call to the `register_callback` method included a user secret, the `GET` request from the service also includes an `X-Callback-Signature` header that specifies the HMAC-SHA1 signature of the challenge string. The service calculates the signature by using the user secret as the key.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  You respond to the `GET` request from the service with status code 200. Include the challenge string sent by the service in plain text in the body of the response, and set the `Content-Type` response header to `text/plain`.

    If the initial `POST` request included a user secret, you can calculate an HMAC-SHA1 signature of the challenge string by using the secret as the key. If the `GET` request was sent by the service, the signature matches the value specified by the `X-Callback-Signature` header.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  The service checks whether the challenge string is returned in the body of the response to its `GET` request. If it is, the service white-lists the callback URL and responds to your original `POST` request with status code 201. The body of the response includes a JSON object with a `status` field that has the value `created` and a `url` field that has the value of your callback URL.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

The service sends only a single `GET` request to a callback URL during the registration process. If the service does not receive a reply with response code 200 that includes the challenge string in its body within five seconds, it does not white-list the URL; it instead sends status code 400 in response to the `POST /v1/register_callback` request. If the callback URL was already successfully white-listed, the service sends status code 200 in response to the initial `POST` request.

### Security considerations
{: #security}

When you successfully use the `POST /v1/register_callback` method to register a callback URL, the service white-lists the URL to indicate that it has been verified for use with callback notifications. If you specify a user secret with the registration call, white-listing also means that the URL has been validated for additional security. Specifying a user secret provides authentication and data integrity for requests that use the callback URL with the asynchronous HTTP interface.

The service uses the user secret to compute an HMAC-SHA1 signature over the payload of every callback notification that it sends to the URL. The service sends the signature via the `X-Callback-Signature` header with each notification. The client can use the secret to compute its own signature of each notification payload. If its signature matches the value of the `X-Callback-Signature` header, the client knows that the notification was sent by the service and that its contents have not been altered during transmission. This guarantees that the client is not the victim of a man-in-middle-attack.

HTTPS is ideal for production applications. But during application development and prototyping, the HTTP-based callback notifications supported by the service can simplify and accelerate the development process by avoiding the overhead of HTTPS.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### Unregistering a callback URL
{: #unregister}

You can unregister a white-listed callback URL at any time by calling the `POST /v1/unregister_callback` method. Unregistering a callback URL can be useful for testing your application with the service. Once you unregister a callback URL, you can no longer use it with asynchronous recognition requests.

## Creating a job
{: #create}

You create a recognition job by calling the `POST /v1/recognitions` method. How you learn the status and results of the job depends on the approach you use and the parameters you pass:

-   *To use callback notifications*, include the `callback_url` query parameter to specify a URL to which the service is to send callback notifications when the status of the job changes. You can also specify the following optional query parameters:
    -   `events` to subscribe to a list of notification events. By default, the service sends callback notifications when the job is started (the `recognitions.started` event), when the job is complete (the `recognitions.completed` event), and if an error occurs (the `recognitions.failed` event). You can specify a subset of the events or use the `recognitions.completed_with_results` event instead of the `recognitions.completed` event to include the results with the job-completed notification.
    -   `user_token` to specify a string that is to be included with each notification for the job. Because you can use the same callback URL with an indefinite number of jobs, you can leverage user tokens to differentiate notifications for different jobs.
-   *To use polling*, omit the `callback_url`, `events`, and `user_token` query parameters. You must then use the `GET /v1/recognitions` or `GET /v1/recognitions/{id}` methods to check the status of the job, using the latter to retrieve the results when the job is complete.

In both cases, you can include the `results_ttl` query parameter to specify the number of minutes for which the results are to remain available after the job completes.

In addition to the previous parameters, which are specific to the asynchronous interface, the `POST /v1/recognitions` method supports most of the same parameters as the WebSocket and HTTP REST interfaces. For more information, see [Input features](/docs/services/speech-to-text/input.html) and [Output features](/docs/services/speech-to-text/output.html).

The asynchronous interface does not support interim results, and it imposes a data size limit of 100 MB on the audio that you submit with the request. For more information about audio formats and about using compression to increase the amount of audio that you can send with a request, see [Audio Formats](/docs/services/speech-to-text/audio-formats.html).

### Callback notifications
{: #notifications}

If the job is created with a callback URL, the service uses the HTTP `POST` request method to send a callback notification to the registered URL when one of the events specified in the request occurs. The body of a basic notification consists of a JSON object with the following structure:

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

The `id` field identifies the ID of the job that generated the callback, and the `event` field identifies the event that triggered the callback. The `user_token` field includes the user token for the job if one was specified; otherwise, the field is an empty string. If the event is `recognitions.completed_with_results`, the object includes a `results` field that provides the results of the recognition request. The client should respond to the callback notification with status code 200.

If the callback URL was registered with a user secret, the service also sends the `X-Callback-Signature` header with the callback notification. The header specifies the HMAC-SHA1 signature of the body of the request calculated by using the user secret as a key. The client can calculate the signature of the payload for the callback notification to be sure it matches the signature in the header. For example, the following simple Python code computes the signature on the string `notification_payload`:

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### Callback example
{: #callback}

The following example creates a job associated with the previously white-listed callback URL `http://{user_callback_path}/results`. The user token `job25` is passed to identify the job in callback notifications sent by the service. The call uses the default events, so the user must call the `GET /v1/recognitions/{id}` method to retrieve the results when the service sends a callback notification indicating that the job is complete. The call sets the `timestamps` query parameter of the recognition request to `true`.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

The service returns the status of the request, which is `waiting` to indicate that the service is preparing the job for processing. The response includes the creation time, the job ID, and the URL at which to obtain more information about the job.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Polling example
{: #polling}

The following example creates a job that is not associated with a callback URL. The user must poll the service to learn when the job is complete and then retrieve the results with the `GET /v1/recognitions/{id}` method. Like the previous example, the call sets the `timestamps` parameter of the recognition request to `true`.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/wav"
--data-binary @audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

The service returns a status of `processing` to indicate that it is already processing the job, along with the creation time, the job ID, and the URL for getting information about the job.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Checking the status and retrieving the results of a job
{: #job}

You call the `GET /v1/recognitions/{id}` method to check the status of the job specified with the `id` path parameter. The response always includes the ID and status of the job and its creation and update times. If the status is `completed`, the response also includes the results of the recognition request.

The `GET /v1/recognitions/{id}` method is the only way to retrieve job results if

-   The job was submitted without a callback URL.
-   The job was submitted with a callback URL but without specifying the `recognitions.completed_with_results` event.

However, you can still use the method to retrieve the results for a job that specified a callback URL and the `recognitions.completed_with_results` event. You can retrieve the results for any job as many times as you want as long as they remain available. A job and its results remain available until you delete them with the `DELETE /v1/recognitions/{id}` method or until the job's time to live expires, whichever comes first. By default, results expire after one week unless you specify a different time to live with the `results_ttl` parameter of the `POST /v1/recognitions` method.

### Example of status without results
{: #withoutResults}

The following example checks the status of the job with the specified ID. The job is not yet complete, so the response does not include the results.

```bash
curl -X GET -u {username}:{password}
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

### Example of status with results
{: #withResults}

The following example requests the status of the job with the specified ID. The job is complete, so the response includes the results of the request.

```bash
curl -X GET -u {username}:{password}
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
              "confidence": 0.885
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

## Checking the status of all jobs
{: #jobs}

You call the `GET /v1/recognitions` method to check the status of all outstanding jobs. The method returns the status of all current jobs associated with the service credentials with which it is called. The method returns the ID and status of each job, along with its creation and update times; if a job was created with a callback URL and a user token, the method also returns the user token for the job. The status is one of the following:

-   `waiting` if the service is preparing the job for processing. This is the initial state of all jobs. The job remains in this state until the service has the capacity to begin processing it.
-   `processing` if the service is actively processing the job.
-   `completed` if the service has finished processing the job. If the job specified a callback URL and the event `recognitions.completed_with_results`, the service sent the results with the callback notification. Otherwise, use the `GET /v1/recognitions/{id}` method to obtain the results.
-   `failed` if the job failed for some reason.

A job and its results remain available until you delete them with the `DELETE /v1/recognitions/{id}` method or until the job's time to live expires, whichever comes first.

### Example
{: #statusExample}

The following example requests the status of all current jobs associated with the caller's service credentials. The user has three outstanding jobs in various states; the first job was created with a callback URL and a user token.

```bash
curl -X GET -u {username}:{password}
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

## Deleting a job
{: #delete}

You can use the `DELETE /v1/recognitions/{id}` method to delete the job specified with the `id` path parameter. You typically delete a job after you have obtained its results from the service. Once you delete a job, its results are no longer available. You cannot delete a job that the service is actively processing.

By default, the service maintains the results of each job until the job expires because its time to live has elapsed. The default TTL is one week, but you can use the `results_ttl` parameter of the `POST /v1/recognitions` method to specify the number of minutes that the service is to maintain the results.

### Example
{: #deleteExample}

The following example deletes the job with the specified ID:

```bash
curl -X DELETE -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
