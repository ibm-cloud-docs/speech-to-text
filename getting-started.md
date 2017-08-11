---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-09"

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

# Getting started tutorial
{: #gettingStarted}

The {{site.data.keyword.speechtotextfull}} service transcribes audio to text to enable speech transcription capabilities for applications. This cURL-based tutorial can help you get started quickly with the service. The examples show you how to call the service's `POST /v1/recognize` method to request a transcription.
{: shortdesc}

> **Note:** The examples use cURL to call methods of the HTTP interface. You can install the version of cURL for your operating system from [curl.haxx.se](https://curl.haxx.se/){: new_window}. You must install the version that supports the Secure Sockets Layer (SSL) protocol. Make sure to include the installed binary file on your `PATH` environment variable.

## Step 1: Log in, create the service, and get your credentials

If you already know the credentials for your {{site.data.keyword.speechtotextshort}} service instance, skip this step.
{: tip}

1.  Go to the [{{site.data.keyword.speechtotextshort}} service](https://console.bluemix.net/catalog/services/speech-to-text/){: new_window} and either sign up for a free Bluemix account or log in.
1.  After you log in, enter `speech-to-text-tutorial` in the **Service name field** of the {{site.data.keyword.speechtotextshort}} page. Click **Create**.
1.  Copy your credentials:
    1.  Click **Service credentials**.
    1.  Click **View credentials** under **Actions**.
    1.  Copy the `username` and `password` values.

## Step 2. Transcribe audio with no options
{: #transcribe}

Call the `POST /v1/recognize` method to request a basic transcription of an audio file named `audio-file.flac` with no additional request parameters.

1.  Download the sample audio file <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.
1.  Issue the following command to call the service's `/v1/recognize` method for basic transcription with no parameters. The example uses the default language model, `en-US_BroadbandModel`, for transcription.
    -   Replace `{username}` and `{password}` with your service credentials from the previous step.
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u {username}:{password} \
    --header "Content-Type: audio/flac" \
    --header "Transfer-Encoding: chunked" \
    --data-binary @{path_to_file}audio-file.flac \
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
    ```
    {: pre}

The service returns the following JSON transcription:

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.8691191673278809,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms
swept through colorado on sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Step 3. Transcribe audio with additional options
{: #transcribeOptions}

Call the `POST /v1/recognize` method to transcribe the same audio file, `audio-file.flac`, but specify two of the method's many additional transcription parameters.

1.  Issue the following command to call the service's `/v1/recognize` method with two additional parameters. The `timestamps` parameter is set to `true` to indicate the beginning and end of each word in the audio stream, and the `max_alternatives` parameter is set to `3` to receive the three most likely alternatives for the transcription. The request uses the default model, `en-US_BroadbandModel`.
    -   Replace `{username}` and `{password}` with your service credentials.
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u {username}:{password} \
    --header "Content-Type: audio/flac" \
    --header "Transfer-Encoding: chunked" \
    --data-binary @{path_to_file}audio-file.flac \
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true&max_alternatives=3"
    ```
    {: pre}

The service returns the following output, which includes timestamps and alternative transcriptions:

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
          "confidence": 0.8691191673278809,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms
swept through colorado on sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of severe thunderstorms
swept through colorado on sunday "
        },
        {
          "transcript": "several tornadoes touch down is a line of severe thunderstorms
swept through colorado on sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Next steps

-   For more information about the parameters of the `/v1/recognize` method, see [Input features and parameters](/docs/services/speech-to-text/input.html) and [Output features and parameters](/docs/services/speech-to-text/output.html).
-   For more information about using the service's HTTP interface, see [Using the HTTP REST interface](/docs/services/speech-to-text/http.html).
-   For detailed information about all methods of the service's interfaces, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   Interact with the API in the [API explorer ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://watson-api-explorer.mybluemix.net/apis/speech-to-text-v1){: new_window}.
