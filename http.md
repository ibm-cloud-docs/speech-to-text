---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# The synchronous HTTP interface
{: #http}

The synchronous HTTP interface of the {{site.data.keyword.speechtotextfull}} service provides a single `POST /v1/recognize` method for requesting speech recognition with the service. This method is the simplest means of obtaining a transcript. It offers two ways of submitting a speech recognition request:
{: shortdesc}

-   The first sends all of the audio in a single stream via the body of the request. You specify the parameters of the operation as request headers and query parameters. For more information, see [Making a basic HTTP request](#HTTP-basic).
-   The second sends the audio as a multipart request. You specify the parameters of the request as a combination of request headers, query parameters, and JSON metadata. For more information, see [Making a multipart HTTP request](#HTTP-multi).

Submit a maximum of 100 MB and a minimum of 100 bytes of audio data with a single request. For information about audio formats and about using compression to maximize the amount of audio that you can send with a request, see [Audio formats](/docs/services/speech-to-text?topic=speech-to-text-audio-formats). For information about all methods of the HTTP interface, see the [API reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## Making a basic HTTP request
{: #HTTP-basic}

The HTTP `POST /v1/recognize` method provides a simple means of transcribing audio. You pass all audio via the body of the request and specify the parameters as request headers and query parameters.

The following `curl` example sends a recognition request for a single FLAC file named `audio-file.flac`. The request omits the `model` query parameter to use the default language model, `en-US_BroadbandModel`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

The example returns the following transcript for the audio:

```javascript
{
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
  ],
  "result_index": 0
}
```
{: codeblock}

The `POST /v1/recognize` method returns results only after it processes all of the audio for a request. The method is appropriate for batch processing but not for live speech recognition. Use the WebSocket interface to transcribe live audio.

If your data consists of multiple audio files, the recommended means of submitting the audio is by sending multiple requests, one for each audio file. You can submit the requests in a loop, optionally with parallelism to improve performance. You can also use multipart speech recognition to pass multiple audio files with a single request.

## Making a multipart HTTP request
{: #HTTP-multi}

The `POST /v1/recognize` method also supports multipart requests. You pass all audio data as multipart form data. You specify some parameters as request headers and query parameters, but you pass JSON metadata as form data to control most aspects of the transcription.

Multipart speech recognition is intended for the following use cases:

-   To pass multiple audio files with a single speech recognition request.
-   With browsers for which JavaScript is disabled. Multipart requests based on form data do not require the use of JavaScript.
-   When the parameters of a recognition request are greater than 8 KB, which is the limit imposed by most HTTP servers and proxies. For example, spotting a very large number of keywords can increase the size of a request beyond this limit. Multipart requests use form data to avoid this constraint.

The following sections describe the parameters that you use for multipart requests and show an example request.

### Parameters for multipart requests
{: #multipartParameters}

You specify the following parameters of multipart speech recognition as request headers, query parameters, and form data.

<table summary="Each row of the table describes the use of one possible parameter for a multipart recognition request.">
  <caption>Table 1. Parameters for multipart requests</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">Parameter</th>
    <th id="description" style="text-align:center; width:80%">Description</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>Form data</em>
      <br/><em>Object</em>
    </td>
    <td>
      <em>Required.</em> A JSON object that provides the transcription
      parameters for the request. The object must be the first part of
      the form data. The information describes the audio in the subsequent
      parts of the form data. See
      [JSON metadata for multipart requests](#multipartJSON).
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>Form data</em>
      <br/><em>File</em>
    </td>
    <td>
      <em>Required.</em> One or more audio files as the remainder of the
      form data for the request. All audio files must have the same format.
      With the `curl` command, include a separate <code>--form</code> option
      for each file of the request.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Header</em>
      <br/><em>String</em>
    </td>
    <td>
      <em>Required.</em> Specify `multipart/form-data` to indicate how
      data is passed to the method. You specify the content type of the
      audio with the JSON `part_content_type` parameter.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Header</em>
      <br/><em>String</em>
    </td>
    <td>
      <em>Optional.</em> Specify `chunked` to stream the audio data to the
      service. Omit the parameter if you send all audio with a single request.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Query</em>
      <br/><em>String</em>
    </td>
    <td>
      <em>Optional.</em> The identifier of the model that is to be used with
      the request. The default is `en-US_BroadbandModel`.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Query</em>
      <br/><em>String</em>
    </td>
    <td>
      <em>Optional.</em> The GUID of a custom language model that is to be
      used with the request.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Query</em>
      <br/><em>String</em>
    </td>
    <td>
      <em>Optional.</em> The GUID of a custom acoustic model that is
      to be used with the request.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Query</em>
      <br/><em>String</em>
    </td>
    <td>
      <em>Optional.</em> The version of the specified base model that
      is to be used with the request.
    </td>
  </tr>
</table>

For more information about the query parameters, see the [Parameter summary](/docs/services/speech-to-text?topic=speech-to-text-summary).

### JSON metadata for multipart requests
{: #multipartJSON}

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

Only the following two parameters are specific to multipart requests:

-   The `part_content_type` field is *optional* for most audio formats. It is required for the `audio/alaw`, `audio/basic`, `audio/l16`, and `audio/mulaw` formats. It specifies the format of the audio in the following parts of the request. All audio files must be in the same format.
-   The `data_parts_count` field is *optional* for all requests. It specifies the number of audio files that are sent with the request. The service applies end-of-stream detection to the last (and possibly the only) data part. If you omit the parameter, the service determines the number of parts from the request.

All other parameters of the metadata are optional. For descriptions of all available parameters, see the [Parameter summary](/docs/services/speech-to-text?topic=speech-to-text-summary).

### Example multipart request

The following `curl` example shows how to pass a multipart recognition request with the `POST /v1/recognize` method. The request passes two audio files, **audio-file1.flac** and **audio-file2.flac**. The `metadata` parameter provides most parameters of the request; the `upload` parameters provide the audio files.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

The example returns the following transcript for the audio files. The service returns the results for the two files in the order in which they are sent. (The example output abbreviates the results for the second file.)

```javascript
{
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
   ],
   "result_index": 0
}
```
{: codeblock}
