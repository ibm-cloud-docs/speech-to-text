---

copyright:
  years: 2015, 2025
lastupdated: "2025-06-05"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# The asynchronous HTTP interface
{: #async}

The asynchronous HTTP interface of the {{site.data.keyword.speechtotextfull}} service provides methods for transcribing audio via non-blocking calls to the service. The interface employs user-specified secret strings and digital signatures to provide a level of security for requests that are made over the HTTP protocol. To use the asynchronous interface, you can
{: shortdesc}

-   Register a callback URL to be notified by the service of the job status and the results automatically.
-   Poll the service to obtain the job status and the results manually.

The two approaches are not mutually exclusive. You can elect to receive callback notifications but still poll the service for the latest status or contact the service to retrieve results manually. The following sections describe how to use the asynchronous HTTP interface with either approach.

Submit a maximum of 1 GB and a minimum of 100 bytes of audio data with a single request. For information about audio formats and about using compression to maximize the amount of audio that you can send with a request, see [Supported audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats). For more information about the individual methods of the interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

The asynchronous HTTP interface does not support multipart speech recognition. You can use only the synchronous HTTP interface for multipart requests. For more information, see [Making a multipart HTTP speech recognition request](/docs/speech-to-text?topic=speech-to-text-http#HTTP-multi).
{: note}

## Usage models
{: #usage}

When you work with the service's asynchronous HTTP interface, you can elect to learn about job status and receive results in the following ways:

-   By using callback notifications:
    1.  Call the `POST /v1/register_callback` method to register a callback URL with the service. You can provide an optional user-specified secret string to enable authentication and data integrity for callbacks that are sent to the URL.
    1.  Call the `POST /v1/recognitions` method with an already registered callback URL to which the service sends notifications when the status of the job changes. You specify a list of events of which to be notified. By default, the service sends notifications when a job is started, when it is complete, and if an error occurs. You can also request the results of the request in the completion notification. Otherwise, you need to use the `GET /v1/recognitions/{id}` method to retrieve the results.
-   By polling the service:
    1.  Call the `POST /v1/recognitions` method without a callback URL, events, or a user token.
    1.  Periodically call the `GET /v1/recognitions` method to check the status of the most recent jobs or the `GET /v1/recognitions/{id}` method to check the status of a specific job.
    1.  If you check job status with the `GET /v1/recognitions` method, call the `GET /v1/recognitions/{id}` method to retrieve the results of the job once it is complete.

The two approaches can be used together. You can still poll the service to obtain the latest status for a job that is created with a callback URL. For example, you might want to obtain the status of a job if notifications are taking a while to arrive. You might also check the status if you suspect that you missed one or more notifications due to a service or network error.

## Registering a callback URL
{: #register}

You register a callback URL by calling the `POST /v1/register_callback` method. Once you register a callback URL, you can use it to receive notifications for an indefinite number of jobs. The registration process comprises four steps:

1.  You call the `POST /v1/register_callback` method and pass a callback URL. Optionally, you can also specify a user-specified secret. The service uses the secret to compute keyed-hash message authentication code (HMAC) Secure Hash Algorithm (SHA-256) signatures for authentication and data integrity. The following example registers a user callback that responds at the URL `http://{user_callback_path}/results`. The call includes a user secret of `ThisIsMySecret`.

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    "{url}/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    "{url}/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  The service attempts to validate, or allowlist, the callback URL if it is not already registered by sending a `GET` request to the callback URL. The service passes a random alphanumeric challenge string via the `challenge_string` query parameter of the request. The request includes an `Accept` header that specifies `text/plain` as the required response type.

    If the call to the `register_callback` method included a user secret, the `GET` request from the service also includes an `X-Callback-Signature` header that specifies the HMAC-SHA256 signature of the challenge string. The service calculates the signature by using the user secret as the key.

    ```text
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA256_signature}
    ```
    {: codeblock}

1.  You respond to the `GET` request from the service with status code 200. Include the challenge string that was sent by the service in the response. Include the string in plain text in the body of the response, and set the `Content-Type` response header to `text/plain`.

    If the initial `POST` request included a user secret, you can calculate an HMAC-SHA256 signature of the challenge string by using the secret as the key. If the `GET` request was sent by the service, the signature matches the value specified by the `X-Callback-Signature` header.

    ```text
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  The service checks whether the challenge string is returned in the body of the response to its `GET` request. If it is, the service allowlists the callback URL and responds to your original `POST` request with status code 201. The body of the response includes a JSON object with a `status` field that has the value `created` and a `url` field that has the value of your callback URL.

    ```text
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

The service sends only a single `GET` request to a callback URL during the registration process. The service must receive a reply with response code 200 that includes the challenge string in its body within five seconds. Otherwise, it does not allowlist the URL. Instead, it sends status code 400 in response to the `POST /v1/register_callback` request. If the callback URL was already successfully allowlisted, the service sends status code 200 in response to the initial `POST` request.

### Security considerations
{: #security-async}

When you successfully use the `POST /v1/register_callback` method to register a callback URL, the service allowlists the URL to indicate that it is verified for use with callback notifications. If you specify a user secret with the registration call, allowlisting also means that the URL has been validated for added security. Specifying a user secret provides authentication and data integrity for requests that use the callback URL with the asynchronous HTTP interface.

The service uses the user secret to compute an HMAC-SHA256 signature over the payload of every callback notification that it sends to the URL. The service sends the signature via the `X-Callback-Signature` header with each notification. The client can use the secret to compute its own signature of each notification payload. If its signature matches the value of the `X-Callback-Signature` header, the client knows that the notification was sent by the service and that its contents have not been altered during transmission. This knowledge guarantees that the client is not the victim of a man-in-middle-attack.

HTTPS is ideal for production applications. But during application development and prototyping, the HTTP-based callback notifications that are supported by the service can simplify and accelerate the development process by avoiding the expense of HTTPS.

### Unregistering a callback URL
{: #unregister}

You can unregister an allowlisted callback URL at any time by calling the `POST /v1/unregister_callback` method. Unregistering a callback URL can be useful for testing your application with the service. Once you unregister a callback URL, you can no longer use it with asynchronous recognition requests.

#### Unregister a callback URL example
{: #unregisterExample-async}

The following example unregisters a previously registered callback URL:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/unregister_callback?callback_url=http://{user_callback_path}/results"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
"{url}/v1/unregister_callback?callback_url=http://{user_callback_path}/results"
```
{: pre}

## Creating a job
{: #create}

You create a recognition job by calling the `POST /v1/recognitions` method. How you learn the status and results of the job depends on the approach you use and the parameters you pass:

-   *To use callback notifications*, include the `callback_url` query parameter to specify a URL to which the service is to send callback notifications when the status of the job changes. You can also specify the following optional query parameters:
    -   `events` to subscribe to a list of notification events. By default, the service sends callback notifications when the job is started (the `recognitions.started` event), when the job is complete (the `recognitions.completed` event), and if an error occurs (the `recognitions.failed` event). You can specify a subset of the events or use the `recognitions.completed_with_results` event instead of the `recognitions.completed` event to include the results with the job-completed notification.
    -   `user_token` to specify a string that is to be included with each notification for the job. Because you can use the same callback URL with an indefinite number of jobs, you can leverage user tokens to differentiate notifications for different jobs.
-   *To use polling*, omit the `callback_url`, `events`, and `user_token` query parameters. You must then use the `GET /v1/recognitions` or `GET /v1/recognitions/{id}` methods to check the status of the job, using the latter to retrieve the results when the job is complete.

In both cases, you can include the `results_ttl` query parameter to specify the number of minutes for which the results are to remain available after the job completes. The new job is owned by the instance of the service whose credentials are used to create it.

In addition to the previous parameters, which are specific to the asynchronous interface, the `POST /v1/recognitions` method supports most of the same parameters as the WebSocket and synchronous HTTP interfaces. For more information, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

### Callback notifications
{: #notifications}

If the job is created with a callback URL, the service sends an HTTP `POST` callback notification to the registered URL when a specified event occurs. The body of a basic notification consists of a JSON object with the following structure:

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

The `id` field identifies the ID of the job that generated the callback, and the `event` field identifies the event that triggered the callback. The `user_token` field includes the user token for the job if one was specified. Otherwise, the field is an empty string. If the event is `recognitions.completed_with_results`, the object includes a `results` field that provides the results of the recognition request. The client can respond to the callback notification with status code 200.

If the callback URL was registered with a user secret, the service also sends the `X-Callback-Signature` header with the callback notification. The header specifies the HMAC-SHA256 signature of the body of the request. The service calculates the signature by using the user secret as a key. The client can calculate the signature of the payload for the callback notification to be sure that it matches the signature in the header. For example, the following simple Python code computes the signature on the string `notification_payload`:

```python
import hmac
import base64
from hashlib import sha256
# 'user_secret' and 'notification_payload' must be bytes
hashed = hmac.new(user_secret, notification_payload, sha256)
signature = base64.b64encode(hashed.digest()).decode().rstrip('\n')
```
{: codeblock}

### Create a job with a callback URL example
{: #callback}

The following example creates a job that is associated with the previously allowlisted callback URL `http://{user_callback_path}/results`. The example passes the user token `job25` to identify the job in callback notifications that are sent by the service. The call uses the default events, so the user must call the `GET /v1/recognitions/{id}` method to retrieve the results when the service sends a callback notification to indicate that the job is complete. The call sets the `timestamps` query parameter of the recognition request to `true`.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

The service returns the status of the request, which is `waiting` to indicate that the service is preparing the job for processing. The response includes the creation time, the job ID, and the URL at which to obtain more information about the job.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "{url}/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Create a job with polling example
{: #polling}

The following example creates a job that is not associated with a callback URL. The user must poll the service to learn when the job is complete and then retrieve the results with the `GET /v1/recognitions/{id}` method. Like the previous example, the call sets the `timestamps` parameter of the recognition request to `true`.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognitions?timestamps=true"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognitions?timestamps=true"
```
{: pre}

The service returns a status of `processing` to indicate that it is already processing the job, along with the creation time, the job ID, and the URL for getting information about the job.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "{url}/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Checking the status and retrieving the results of a job
{: #job}

You call the `GET /v1/recognitions/{id}` method to check the status of the job that is specified with the `id` path parameter. The response always includes the ID and status of the job and its creation and update times. If the status is `completed`, the response also includes the results of the recognition request.

The `GET /v1/recognitions/{id}` method is the only way to retrieve job results if

-   The job was submitted without a callback URL.
-   The job was submitted with a callback URL but without specifying the `recognitions.completed_with_results` event.
-   The job is not one of the 100 latest outstanding jobs. When you omit the `id` path parameter, only the 100 latest jobs are returned.

However, you can still use the method to retrieve the results for a job that specified a callback URL and the `recognitions.completed_with_results` event. You can retrieve the results for any job as many times as you want while they remain available. A job and its results remain available until you delete them with the `DELETE /v1/recognitions/{id}` method or until the job's time to live expires, whichever comes first. By default, results expire after one week unless you specify a different time to live with the `results_ttl` parameter of the `POST /v1/recognitions` method.

### Check job status without results example
{: #withoutResults}

The following example checks the status of the job with the specified ID:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/recognitions/{job_id}"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/recognitions/{job_id}"
```
{: pre}

The job is not yet complete, so the response does not include the results.

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### Check job status with results example
{: #withResults}

The following example requests the status of the job with the specified ID:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/recognitions/{job_id}"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/recognitions/{job_id}"
```
{: pre}

The job is complete, so the response includes the results of the request.

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
              "confidence": 0.96
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

## Checking the status of the latest jobs
{: #jobs}

You call the `GET /v1/recognitions` method to check the status of the latest jobs. The method returns the status of the most recent 100 outstanding jobs that are associated with the credentials with which it is called. The method returns the ID and status of each job, along with its creation and update times. If a job was created with a callback URL and a user token, the method also returns the user token for the job.

The response includes one of the following states:

-   `waiting` if the service is preparing the job for processing. This status is the initial state of all jobs. The job remains in this state until the service has the capacity to begin processing it.
-   `processing` if the service is actively processing the job.
-   `completed` if the service has finished processing the job. If the job specified a callback URL and the event `recognitions.completed_with_results`, the service sent the results with the callback notification. Otherwise, use the `GET /v1/recognitions/{id}` method to obtain the results.
-   `failed` if the job failed for some reason.

A job and its results remain available until you delete them with the `DELETE /v1/recognitions/{id}` method or until the job's time to live expires, whichever comes first.

### Check the status of the latest jobs example
{: #statusExample}

The following example requests the status of the latest current jobs that are associated with the caller's credentials:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/recognitions"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/recognitions"
```
{: pre}

The user has three outstanding jobs in various states. The first job was created with a callback URL and a user token.

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
{: #delete-async}

You can use the `DELETE /v1/recognitions/{id}` method to delete the job that is specified with the `id` path parameter. You typically delete a job after you obtain its results from the service. Once you delete a job, its results are no longer available. You cannot delete a job that the service is actively processing.

By default, the service maintains the results of each job until the job's time to live expires. The default time to live is one week, but you can use the `results_ttl` parameter of the `POST /v1/recognitions` method to specify the number of minutes that the service is to maintain the results.

### Delete a job example
{: #deleteExample-async}

The following example deletes the job with the specified ID:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X DELETE -u "apikey:{apikey}" \
"{url}/v1/recognitions/{job_id}"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

```bash
curl -X DELETE \
--header "Authorization: Bearer {token}" \
"{url}/v1/recognitions/{job_id}"
```
{: pre}
