---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-19"

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

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** Watch the following video for a visual summary of getting started with the {{site.data.keyword.speechtotextshort}} service.

![Getting started with the {{site.data.keyword.speechtotextshort}} service](https://video.ibm.com/embed/channel/23952663/video/speech-to-text-get-started){: video output="iframe" data-script="none" id="watsonmediaplayer" width="560" height="315" scrolling="no" allowfullscreen webkitallowfullscreen mozAllowFullScreen frameborder="0" style="border: 0 none transparent;"}

## Before you begin
{: #getting-started-before-you-begin}

### {{site.data.keyword.cloud_notm}}
{: #getting-started-before-you-begin-cloud}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only**

-   Create an instance of the service: {: hide-dashboard}

    1.  Go to the [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} page in the {{site.data.keyword.cloud_notm}} Catalog. {: hide-dashboard}
    1.  Sign up for a free {{site.data.keyword.cloud_notm}} account or log in. {: hide-dashboard}
    1.  Click **Create**. {: hide-dashboard}

-   Copy the credentials to authenticate to your service instance:

    1.  From the [{{site.data.keyword.cloud_notm}} Resource list](https://{DomainName}/resources){: external}, click on your {{site.data.keyword.speechtotextshort}} service instance to go to the {{site.data.keyword.speechtotextshort}} service dashboard page. {: hide-dashboard}
    1.  On the **Manage** page, click **Show Credentials** to view your credentials.
    1.  Copy the `API Key` and `URL` values.

### {{site.data.keyword.icp4dfull_notm}}
{: #getting-started-before-you-begin-icpd}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

1.  Provision an instance of {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}. For more information about provisioning, see [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} version 1.2](/docs/speech-to-text?topic=speech-to-text-speech-install-12).
1.  From the {{site.data.keyword.icp4dfull_notm}} web client menu, choose **My Instances**.
1.  Click the {{site.data.keyword.speechtotextshort}} instance to open the overview page.
1.  Copy the `{token}` and `{URL}` values.

### Using the curl examples
{: #getting-started-curl}

This tutorial uses the `curl` command to call methods of the service's HTTP interface. Make sure that you have the `curl` command installed on your system.

1.  To test whether `curl` is installed, run the following command on the command line. If the output lists the `curl` version that supports Secure Sockets Layer (SSL), you are set for the tutorial.

    ```bash
    curl -V
    ```
    {: pre}

1.  If necessary, install the version of `curl` with SSL enabled for your operating system from [curl.haxx.se](https://curl.haxx.se/){: external}.

### Tips
{: #getting-started-tips}

-   Omit the braces (`{ }`) from the examples. They indicate variable values.
-   *Windows users,* replace the backslash (`\`) at the end of each line with a caret (`^`). Make sure there are no trailing spaces.

## Transcribe audio with no options
{: #getting-started-transcribe}
{: step}

Call the `POST /v1/recognize` method to request a basic transcript of a FLAC audio file with no additional request parameters.

1.  Download the sample audio file [audio-file.flac](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac){: external}.
1.  Issue the following command to call the service's `/v1/recognize` method for basic transcription with no parameters. The example uses the `Content-Type` header to indicate the type of the audio, `audio/flac`. The example uses the default language model, `en-US_BroadbandModel`, for transcription.

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    -   Replace `{apikey}` and `{url}` with your API key and URL. {: hide-dashboard}
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"
    ```
    {: pre}
    {: apikey}
    {: url}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    -   Replace `{token}` with the access token for your service instance.
    -   Replace `{url}` with the URL for your service instance.
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

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    -   Replace `{apikey}` and `{url}` with your API key and URL. {: hide-dashboard}
    -   Modify `{path_to_file}` to specify the location of the `audio-file.flac` file.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"
    ```
    {: pre}
    {: apikey}
    {: url}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    -   Replace `{token}` with the access token for your service instance.
    -   Replace `{url}` with the URL for your service instance.
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
{: getting-started-next-steps}

-   To try an example application that transcribes text from streaming audio input or from a file that you upload, see the [{{site.data.keyword.speechtotextshort}} demo (all languages)](https://speech-to-text-demo.ng.bluemix.net/){: external} or the [{{site.data.keyword.speechtotextshort}} demo (US English)](https://www.ibm.com/demos/live/speech-to-text/self-service/home){: external}.
-   For more information about the service's interfaces and features, see [Service features](/docs/speech-to-text?topic=speech-to-text-service-features).
-   For more information about all methods of the service's interfaces, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.
