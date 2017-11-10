---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-04"

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
{:download: .download}

# Getting started tutorial
{: #gettingStarted}

The {{site.data.keyword.speechtotextfull}} service transcribes audio to text to enable speech transcription capabilities for applications. This cURL-based tutorial can help you get started quickly with the service. The examples show you how to call the service's sessionless `POST /v1/recognize` method to request a transcription.
{: shortdesc}

## Before you begin
{: #before-you-begin}

- Create an instance of the service:
    - {: download} If you're seeing this, you created your service instance. Now get your credentials.
    - Create a project from a service:
        1.  Go to the {{site.data.keyword.watson}} Developer Console [Services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/developer/watson/services){: new_window} page.
        1.  Select {{site.data.keyword.speechtotextshort}}, click **Add Services**, and either sign up for a free {{site.data.keyword.Bluemix_notm}} account or log in.
        1.  Type `speech-tutorial` as the project name and click **Create Project**.
- Copy the credentials to authenticate to your service instance:
    - {: download} From the service dashboard (what you're looking at):
        1.  Click the **Service credentials** tab.
        1.  Click **View credentials** under **Actions**.
        1.  Copy the `username`, `password`, and `url` values.
        {: download}
    - From your **speech-tutorial** project in the Developer Console, copy the `username`,  `password`, and `url` values for `"speech_to_text"` from the  **Credentials** section.
- Make sure you have cURL:
    - The examples use cURL to call methods of the HTTP interface. Install the version for your operating system from [curl.haxx.se ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://curl.haxx.se/){: new_window}. Install the version that supports the Secure Sockets Layer (SSL) protocol. Make sure to include the installed binary file on your `PATH` environment variable.

<!-- Remove this text after dedicated instances have the Developer Console: begin -->

If you use {{site.data.keyword.Bluemix_dedicated_notm}}, you create a service instance from the [{{site.data.keyword.speechtotextshort}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/catalog/services/speech-to-text/){: new_window} page in the Catalog. For details about how to find your service credentials, see [Service credentials for Watson services ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually){: new_window}.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Step 1: Transcribe audio with no options
{: #transcribe}

Call the `POST /v1/recognize` method to request a basic transcription of a FLAC audio file with no additional request parameters.

1.  Download the sample audio file <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.
1.  Issue the following command to call the service's `/v1/recognize` method for basic transcription with no parameters. The example uses the `Content-Type` header to indicate the type of the audio, `audio/flac`. The example uses the default language model, `en-US_BroadbandModel`, for transcription.
    -   Replace `{username}` and `{password}` with your service credentials from the previous step.
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u {username}:{password} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
    ```
    {: pre}

    The service returns the following transcription results:

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

## Step 2: Transcribe audio with additional options
{: #transcribeOptions}

Call the `POST /v1/recognize` method to transcribe the same FLAC audio file, but specify two of the method's many additional transcription features.

1.  If necessary, download the sample audio file <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.
1.  Issue the following command to call the service's `/v1/recognize` method with two additional parameters. The `timestamps` parameter is set to `true` to indicate the beginning and end of each word in the audio stream, and the `max_alternatives` parameter is set to `3` to receive the three most likely alternatives for the transcription. The example uses the `Content-Type` header to indicate the type of the audio, `audio/flac`, and the request uses the default model, `en-US_BroadbandModel`.
    -   Replace `{username}` and `{password}` with your service credentials.
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u {username}:{password} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true&max_alternatives=3"
    ```
    {: pre}

    The service returns the following results, which includes timestamps and three alternative transcriptions:

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

-   Learn more about the interfaces and SDKs that are available for making speech recognition requests in the [Overview for developers](/docs/services/speech-to-text/developer-overview.html).
-   See basic recognition requests for each of the service's interfaces in [Making a recognition request](/docs/services/speech-to-text/basic-request.html).
-   Find detailed information about all methods of the service's interfaces in the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   Interact with the API in the [API explorer ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://watson-api-explorer.mybluemix.net/apis/speech-to-text-v1){: new_window}.
