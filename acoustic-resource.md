---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-29"

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

# Working with audio resources
{: #audioResources}

You can add individual audio files or archive files that contain multiple audio files to a custom acoustic model. The recommended means of adding audio resources is by adding archive files. Creating and adding a single archive file is considerably more efficient than adding files individually.
{: shortdesc}

You use the `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method to add either type of audio resource to a custom model; see [Add audio to the custom acoustic model](/docs/services/speech-to-text/acoustic-create.html#addAudio). You pass the audio resource as the body of the request and include the following parameters:

-   The `customization_id` path parameter to specify the customization ID of the model.
-   The `audio_name` path parameter to specify a name for the audio resource. The name cannot contain spaces. Use a localized name that matches the language of the model.

The following sections describe required request headers for audio- and archive-type resources and provide guidelines for adding audio resources.

## Working with audio files
{: #workingAudio}

To add an individual audio file to a custom acoustic model, you specify the format (MIME type) of the audio with the `Content-Type` header. You can add audio with any format that is supported for use with recognition requests. Include `rate`, `channels`, and `endianness` parameters with the specification of formats that require them. For a complete list of supported formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).

The following example from [Add audio to the custom acoustic model](/docs/services/speech-to-text/acoustic-create.html#addAudio) adds an `audio/wav` file:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## Working with archive files
{: #workingArchive}

The preferred means of adding audio to a custom acoustic model is to add an archive file that includes multiple audio files. You can add the following types of archive files by specifying the type of the archive with the `Content-Type` request header:

-   A **.zip** file by specifying `application/zip`
-   A **.tar.gz** file by specifying `application/gzip`

All audio files added with a single archive file must have the same audio format. By default, the method accepts an archive of WAV files. If your archive includes any other type of audio, you must include the `Contained-Content-Type` header with the request to specify the format of the audio. The header accepts all of the audio formats that are supported for use with recognition requests, including the `rate`, `channels`, and `endianness` parameters that are used with some formats. For more information about the supported formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).

For an audio file that is embedded within an archive-type resource, the name of the audio file must adhere to the following restrictions:

-   Include a maximum of 128 characters in the file name, including the file extension.
-   Do not include spaces, `/` (slashes), or `\` (backslashes) in the file name.
-   Do not use the name of an audio file that has already been added to the custom model as part of an archive-type resource.

The following example from [Add audio to the custom acoustic model](/docs/services/speech-to-text/acoustic-create.html#addAudio) adds an `application/zip` file that contains audio files in `audio/l16` format that are sampled at 16 kHz:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## Guidelines for adding audio resources
{: #audioGuidelines}

Follow these guidelines when you add audio resources to a custom acoustic model:

-   The custom model must contain at least 10 minutes and no more than 50 hours of audio that includes speech, not silence.
-   Add audio resources that are no larger than 100 MB. All audio- and archive-type resources are limited to a maximum size of 100 MB.
-   Add audio content that reflects the acoustic channel conditions of the audio that you plan to transcribe. For example, if your application deals with audio that has background noise from a car, use the same type of data to build the custom model.
-   The sampling rate of an audio file must match the sampling rate of the base model for the custom acoustic model:
    -   For broadband models, the sampling rate must be at least 16 kHz (16,000 samples per second).
    -   For narrowband models, the sampling rate must be at least 8 kHz (8000 samples per second).

    If the sampling rate of the audio is higher than the minimum required sampling rate, the service down-samples the audio to the appropriate rate. If the sampling rate of the audio is lower than the minimum required rate, the service labels the audio file as `invalid`. If any audio file that is contained in an archive file is invalid, the service considers the entire archive invalid.
-    If your audio data is less than an hour long, {{site.data.keyword.IBM_notm}} recommends that you create a custom language model based on transcriptions of the audio to achieve the best results. For more information, see [Using custom acoustic and custom language models together](/docs/services/speech-to-text/acoustic-both.html).
-    If your audio is domain-specific and contains unique words that are not found in the service's base vocabulary, acoustic model customization alone does not produce those words during transcription. You must use language model customization to expand the service's base vocabulary. For more information, see [Using custom acoustic and custom language models together](/docs/services/speech-to-text/acoustic-both.html).
