---

copyright:
  years: 2015, 2022
lastupdated: "2022-05-24"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# The synchronous HTTP interface
{: #http}

The synchronous HTTP interface of the {{site.data.keyword.speechtotextfull}} service provides a single `POST /v1/recognize` method for requesting speech recognition with the service. This method is the simplest means of obtaining a transcript. It offers two ways of submitting a speech recognition request:
{: shortdesc}

-   The first sends all of the audio in a single stream via the body of the request. You specify the parameters of the operation as request headers and query parameters. For more information, see [Making a basic HTTP request](#HTTP-basic).
-   The second sends the audio as a multipart request. You specify the parameters of the request as a combination of request headers, query parameters, and JSON metadata. For more information, see [Making a multipart HTTP request](#HTTP-multi).

Submit a maximum of 100 MB and a minimum of 100 bytes of audio data with a single request. For information about audio formats and about using compression to maximize the amount of audio that you can send with a request, see [Supported audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats). For information about all methods of the HTTP interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## Making a basic HTTP speech recognition request
{: #HTTP-basic}

The HTTP `POST /v1/recognize` method provides a simple means of transcribing audio. You pass all audio via the body of the request and specify the parameters as request headers and query parameters.

The method returns results only after it processes all of the audio for a request. The method is appropriate for batch processing but not for live speech recognition. Use the WebSocket interface to transcribe live audio.

If your data consists of multiple audio files, the recommended means of submitting the audio is by sending multiple requests, one for each audio file. You can submit the requests in a loop, optionally with parallelism to improve performance. You can also use multipart speech recognition to pass multiple audio files with a single request.

### Basic request example
{: #HTTP-basic-example}

The following example sends a recognition request for a single FLAC file named `audio-file.flac`. The request omits the `model` query parameter to use the default language model, `en-US_BroadbandModel`. For more information, see [The default model](/docs/speech-to-text?topic=speech-to-text-models-use#models-use-default).

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```sh
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize"
```
{: pre}

The example returns the following transcript for the audio:

```json
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Making a multipart HTTP speech recognition request
{: #HTTP-multi}

The asynchronous HTTP interface, WebSocket interface, and Watson SDKs do not support multipart speech recognition.
{: note}

The `POST /v1/recognize` method also supports multipart requests for speech recognition. You pass all audio data as multipart form data. You specify some parameters as request headers and query parameters, but you pass JSON metadata as form data to control most aspects of the transcription.

Multipart speech recognition is intended for the following use cases:

-   To pass multiple audio files with a single speech recognition request.
-   With browsers for which JavaScript is disabled. Multipart requests based on form data do not require the use of JavaScript.
-   When the parameters of a recognition request are greater than 8 KB, which is the limit imposed by most HTTP servers and proxies. For example, spotting a very large number of keywords can increase the size of a request beyond this limit. Multipart requests use form data to avoid this constraint.

The following sections describe the parameters that you use for multipart requests and show an example request.

### Parameters for multipart requests
{: #multipart-parameters}

You specify a number of parameters as form data, request headers, or query parameters. For more information about request headers and query parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

#### Form data
{: #multipart-parameters-form-data}

You specify the following parameters of multipart speech recognition requests as form data:

`metadata` (*required* object)
:   A JSON object that provides the transcription parameters for the request. The object must be the first part of the form data. The information describes the audio in the subsequent parts of the form data. See [JSON metadata for multipart requests](#multipart-json).

`upload` (*required* file)
:   One or more audio files as the remainder of the form data for the request. All audio files must have the same format. With the `curl` command, include a separate `--form` option for each file of the request.

#### Request headers
{: #multipart-parameters-request-headers}

You specify the following parameters as request headers:

`Content-Type` (*required* string)
:   Specify `multipart/form-data` to indicate how data is passed to the method. You specify the content type of the audio with the JSON `part_content_type` parameter.

`Transfer-Encoding` (*optional* string)
:   Specify `chunked` to stream the audio data to the service. Omit the parameter if you send all audio with a single request.

#### Query parameters
{: #multipart-parameters-query-parameters}

You specify the following parameters as query parameters:

`model` (*optional* string)
:   The identifier of the model that is to be used with the request. The default is `en-US_BroadbandModel`. For more information, see [The default model](/docs/speech-to-text?topic=speech-to-text-models-use#models-use-default).

`language_customization_id` (*optional* string)
:   The GUID of a custom language model that is to be used with the request.

`acoustic_customization_id` (*optional* string)
:   The GUID of a custom acoustic model that is to be used with the request.

`base_model_version` (*optional* string)
:   The version of the specified base model that is to be used with the request.

### JSON metadata for multipart requests
{: #multipart-json}

The JSON metadata that you pass with a multipart request can include the following fields:

-   `part_content_type` (string)
-   `data_parts_count` (integer)
-   `customization_weight` (number)
-   `inactivity_timeout` (integer)
-   `keywords` (string[])
-   `keywords_threshold` (number)
-   `max_alternatives` (integer)
-   `word_alternatives_threshold` (number)
-   `word_confidence` (boolean)
-   `timestamps` (boolean)
-   `profanity_filter` (boolean)
-   `smart_formatting` (boolean)
-   `speaker_labels` (boolean)
-   `grammar_name` (string)
-   `redaction` (boolean)
-   `end_of_phrase_silence_time` (double)
-   `split_transcript_at_phrase_end` (boolean)
-   `speech_detector_sensitivity` (number)
-   `background_audio_suppression` (number)
-   `low_latency` (boolean)
-   `character_insertion_bias` (float)

Only the following two parameters are specific to multipart requests:

-   The `part_content_type` field is *optional* for most audio formats. It is required for the `audio/alaw`, `audio/basic`, `audio/l16`, and `audio/mulaw` formats. It specifies the format of the audio in the following parts of the request. All audio files must be in the same format.
-   The `data_parts_count` field is *optional* for all requests. It specifies the number of audio files that are sent with the request. The service applies end-of-stream detection to the last (and possibly the only) data part. If you omit the parameter, the service determines the number of parts from the request.

All other parameters of the metadata are optional. For descriptions of all available parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

### Multipart request example
{: #multipart-request-example}

The following example shows how to pass a multipart recognition request with the `POST /v1/recognize` method. The request passes two audio files, **audio-file1.flac** and **audio-file2.flac**. The `metadata` parameter provides most parameters of the request; the `upload` parameters provide the audio files.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: multipart/form-data" \
--form metadata="{\"part_content_type\":\"application/octet-stream\", \
  \"data_parts_count\":2, \
  \"timestamps\":true, \
  \"word_alternatives_threshold\":0.9, \
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"], \
  \"keywords_threshold\":0.5}" \
--form upload="@{path}audio-file1.flac" \
--form upload="@{path}audio-file2.flac" \
"{url}/v1/recognize"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```sh
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: multipart/form-data" \
--form metadata="{\"part_content_type\":\"application/octet-stream\", \
  \"data_parts_count\":2, \
  \"timestamps\":true, \
  \"word_alternatives_threshold\":0.9, \
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"], \
  \"keywords_threshold\":0.5}" \
--form upload="@{path}audio-file1.flac" \
--form upload="@{path}audio-file2.flac" \
"{url}/v1/recognize"
```
{: pre}

The example returns the following transcript for the audio files. The service returns the results for the two files in the order in which they are sent. (The example output abbreviates the results for the second file.)

```json
{
  "result_index": 0,
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "the"
            }
          ],
          "end_time": 0.09
        },
        {
          "start_time": 0.09,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "latest"
            }
          ],
          "end_time": 0.62
        },
        {
          "start_time": 0.62,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "weather"
            }
          ],
          "end_time": 0.87
        },
        {
          "start_time": 0.87,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "report"
            }
          ],
          "end_time": 1.5
        }
      ],
      "keywords_result": {},
      "alternatives": [
        {
          "timestamps": [
            [
              "the",
              0.03,
              0.09
            ],
            [
              "latest",
              0.09,
              0.62
            ],
            [
              "weather",
              0.62,
              0.87
            ],
            [
              "report",
              0.87,
              1.5
            ]
          ],
          "confidence": 0.99,
          "transcript": "the latest weather report "
        }
      ],
      "final": true
    },
    {
      "word_alternatives": [
        {
          "start_time": 0.15,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "a"
            }
          ],
          "end_time": 0.3
        },
        {
          "start_time": 0.3,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "line"
            }
          ],
          "end_time": 0.64
        },
        . . .
        {
          "start_time": 4.58,
          "alternatives": [
            {
              "confidence": 0.98,
              "word": "Colorado"
            }
          ],
          "end_time": 5.16
        },
        {
          "start_time": 5.16,
          "alternatives": [
            {
              "confidence": 0.98,
              "word": "on"
            }
          ],
          "end_time": 5.32
        },
        {
          "start_time": 5.32,
          "alternatives": [
            {
              "confidence": 0.98,
              "word": "Sunday"
            }
          ],
          "end_time": 6.04
        }
      ],
      "keywords_result": {
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 3.03,
            "confidence": 0.98,
            "end_time": 3.84
          }
        ],
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.58,
            "confidence": 0.98,
            "end_time": 5.16
          }
        ]
      },
      "alternatives": [
        {
          "timestamps": [
            [
              "a",
              0.15,
              0.3
            ],
            [
              "line",
              0.3,
              0.64
            ],
            . . .
            [
              "Colorado",
              4.58,
              5.16
            ],
            [
              "on",
              5.16,
              5.32
            ],
            [
              "Sunday",
              5.32,
              6.04
            ]
          ],
          "confidence": 0.99,
          "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}
