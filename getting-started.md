---

copyright:
  years: 2015, 2025
lastupdated: "2025-02-14"

keywords: speech to text,IBM cloud,getting started,tutorial,transcribe audio,speech recognition

subcollection: speech-to-text

content-type: tutorial
account-plan: lite
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.speechtotextshort}}
{: #gettingStarted}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

The {{site.data.keyword.speechtotextfull}} service transcribes audio to text to enable speech transcription capabilities for applications. This `curl`-based tutorial can help you get started quickly with the service. The examples show you how to call the service's `POST /v1/recognize` method to request a transcript.
{: shortdesc}

The tutorial uses the `curl` command-line utility to demonstrate REST API calls. For more information about `curl`, see [Using curl with Watson examples](/docs/watson?topic=watson-using-curl).
{: note}

[IBM Cloud]{: tag-ibm-cloud} Watch the following video for a visual summary of getting started with the {{site.data.keyword.speechtotextshort}} service.

![Getting started with the {{site.data.keyword.speechtotextshort}} service](https://video.ibm.com/embed/recorded/129758027){: video output="iframe" data-script="none" id="watsonmediaplayer" width="560" height="315" scrolling="no" allowfullscreen webkitallowfullscreen mozAllowFullScreen frameborder="0" style="border: 0 none transparent;"}

## Before you begin
{: #getting-started-before-you-begin}

### {{site.data.keyword.cloud_notm}}
{: #getting-started-before-you-begin-cloud}

[IBM Cloud]{: tag-ibm-cloud}

-   Create an instance of the service: {: hide-dashboard}

    1.  Go to the [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} page in the {{site.data.keyword.cloud_notm}} catalog.
    1.  Sign up for a free {{site.data.keyword.cloud_notm}} account or log in.
    1.  Read and agree to the terms of the license agreement.
    1.  Click **Create**.

-   Copy the credentials to authenticate to your service instance:

    1.  View the **Manage** page for the service instance:

        -   If you are on the **Getting started** page for your service instance, click the **Manage** entry in the list of topics.
        -   If you are on the **Resource list** page, expand the **AI / Machine Learning** grouping in the **Name** column, and click the name of your service instance.

    1.  On the **Manage** page, click **Show Credentials** in the **Credentials** box.
    1.  Copy the `API Key` and `URL` values for the service instance.

This tutorial uses an API key to authenticate. In production, use an IAM token. For more information, see [Authenticating to IBM Cloud](/docs/watson?topic=watson-iam#gs-credential-cloud).
{: tip}

### {{site.data.keyword.icp4dfull_notm}}
{: #getting-started-before-you-begin-icpd}

[IBM Cloud Pak for Data]{: tag-cp4d}

The {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} must be installed and configured before beginning this tutorial. For more information, see [Watson Speech services on Cloud Pak for Data](https://www.ibm.com/docs/en/cloud-paks/cp-data/5.0.x?topic=services-watson-speech){: external}.

1.  Create an instance of the service by using the web client, the API, or the command-line interface. For more information about creating a service instance, see [Creating a service instance for Watson Speech services](https://www.ibm.com/docs/en/cloud-paks/cp-data/5.0.x?topic=setup-creating-service-instance){: external}.
1.  Follow the instructions in *Creating a Watson Speech services instance* to obtain a Bearer token for the instance. This tutorial uses a Bearer token to authenticate to the service.

### {{site.data.keyword.IBM_notm}} Software Hub
{: #getting-started-before-you-begin-swh}

[IBM Software Hub]{: tag-teal}

The {{site.data.keyword.speechtotextshort}} service must be installed and configured before beginning this tutorial. For more information, see [Watson Speech services](https://www.ibm.com/docs/en/software-hub/5.1.x?topic=services-watson-speech){: external}.

1.  Create an instance of the service by using the web client, the API, or the command-line interface. For more information about creating a service instance, see [Creating a service instance for Watson Speech services](https://www.ibm.com/docs/en/software-hub/5.1.x?topic=setup-creating-service-instance){: external}.
1.  Follow the instructions in *Creating a Watson Speech services instance* to obtain a Bearer token for the instance. This tutorial uses a Bearer token to authenticate to the service.

## Transcribe audio with no options
{: #getting-started-transcribe}
{: step}

Call the `POST /v1/recognize` method to request a basic transcript of a FLAC audio file with no additional request parameters.

1.  Download the sample audio file [audio-file.flac](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac){: external}.
1.  Issue the following command to call the service's `/v1/recognize` method for basic transcription with no parameters. The example uses the `Content-Type` header to indicate the type of the audio, `audio/flac`. The example uses the default language model, `en-US_BroadbandModel`, for transcription.

    [IBM Cloud]{: tag-ibm-cloud}

    -   Replace `{apikey}` and `{url}` with your API key and URL. {: hide-dashboard}
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

    -   Replace `{token}` and `{url}` with the access token and URL for your service instance.
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"
    ```
    {: pre}

The service returns the following transcription results:

```javascript
{
  "result_index": 0,
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Transcribe audio with options
{: #getting-started-transcribe-options}
{: step}

Call the `POST /v1/recognize` method to transcribe the same FLAC audio file, but specify two transcription parameters.

1.  If necessary, download the sample audio file [audio-file.flac](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac){: external}.
1.  Issue the following command to call the service's `/v1/recognize` method with two extra parameters. Set the `timestamps` parameter to `true` to indicate the beginning and end of each word in the audio stream. Set the `max_alternatives` parameter to `3` to receive the three most likely alternatives for the transcription. The example uses the `Content-Type` header to indicate the type of the audio, `audio/flac`, and the request uses the default model, `en-US_BroadbandModel`.

    [IBM Cloud]{: tag-ibm-cloud}

    -   Replace `{apikey}` and `{url}` with your API key and URL. {: hide-dashboard}
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d} [IBM Software Hub]{: tag-teal}

    -   Replace `{token}` and `{url}` with the access token and URL for your service instance.
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"
    ```
    {: pre}

The service returns the following results, which include timestamps and three alternative transcriptions:

```javascript
{
  "result_index": 0,
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
          "confidence": 0.96
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado and Sunday "
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}

## Next steps
{: #getting-started-next-steps}

-   To try an example application that transcribes text from streaming audio input or from a file that you upload, see the [{{site.data.keyword.speechtotextshort}} demo](https://www.ibm.com/products){: external}.
-   For more information about the service's interfaces and features, see [Service features](/docs/speech-to-text?topic=speech-to-text-service-features).
-   For more information about all methods of the service's interfaces, see the [API & SDK reference](/apidocs/speech-to-text){: external}.
