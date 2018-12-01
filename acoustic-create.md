---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-15"

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

# Creating a custom acoustic model
{: #acoustic}

Follow these steps to create a custom acoustic model for the {{site.data.keyword.speechtotextshort}} service:
{: shortdesc}

1.  [Create a custom acoustic model](#createModel). You can create multiple custom models for the same or different domains or environments. The process is the same for any model that you create. However, you can specify only a single custom acoustic model at a time with a recognition request.
1.  [Add audio to the custom acoustic model](#addAudio). The service accepts the same audio file formats for acoustic modeling that it accepts for recognition requests. It also accepts archive files that contain multiple audio files. Archive files are the preferred means of adding audio resources. You can repeat the method to add more audio or archive files to a custom model.
1.  [Train the custom acoustic model](#trainModel). Once you add audio resources to the custom model, you must train the model. Training prepares the custom acoustic model for use in speech recognition. Training can take a significant amount of time. The length of the training depends on the amount of audio data that the model contains.

    You can specify a helper custom language model during training of your custom acoustic model. A custom language model that includes transcriptions of your audio files or OOV words from the domain of your audio files can improve the quality of the custom acoustic model. For more information, see [Training a custom acoustic model with a custom language model](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).
1.  After you train your custom model, you can use it with recognition requests. If the audio passed for transcription has acoustic qualities that are similar to the audio of the custom model, the results reflect the service's enhanced understanding. For more information, see [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html).

    You can pass both a custom acoustic model and a custom language model in the same recognition request to further improve recognition accuracy. For more information, see [Using custom language and custom acoustic models during speech recognition](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

The steps for creating a custom acoustic model are iterative. You can add or delete audio and train or retrain a model as often as needed. You must retrain a model for any changes to its audio to take effect. When you retrain a model, all audio data is used in the training (not just the new data). So the training time is commensurate with the total amount of audio that is contained in the model.

Acoustic model customization is available as beta functionality for all languages. For more information, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Create a custom acoustic model
{: #createModel}

You use the `POST /v1/acoustic_customizations` method to create a new custom acoustic model. You can create any number of custom acoustic models, but you can use only one custom acoustic model at a time with a speech recognition request. The method accepts a JSON object that defines the attributes of the new custom model as the body of the request.

<table>
  <caption>Table 1. Attributes of a new custom acoustic model</caption>
  <tr>
    <th style="width:20%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      A user-defined name for the new custom acoustic model. Use a name
      that describes the acoustic environment of the custom model, such
      as <code>Mobile custom model</code> or <code>Noisy car custom
      model</code>. Use a name that is unique among all custom acoustic
      models that you own. Use a localized name that matches the language
      of the custom model.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The name of the base model that is to be customized by the new
      model. You must use the name of a model that is returned by the
      <code>GET /v1/models</code> method. The new custom model can be
      used only with the base model that it customizes.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      A description of the new model. Use a localized description that
      matches the language of the custom model.
    </td>
  </tr>
</table>

The following example creates a new custom acoustic model named `Example acoustic model`. The model is created for the base model `en-US_BroadbandModel` and has the description `Example custom acoustic model`. The `Content-Type` header specifies that JSON data is being passed to the method.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

The example returns the customization ID of the new model. Each custom model is identified by a unique customization ID, which is a Globally Unique Identifier (GUID). You specify a custom model's GUID with the `customization_id` parameter of calls that are associated with the model.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

The new custom model is owned by the service instance whose credentials are used to create it. For more information, see [Ownership of custom models](/docs/services/speech-to-text/custom.html#customOwner).

## Add audio to the custom acoustic model
{: #addAudio}

Once you create your custom acoustic model, the next step is to add audio resources to it. You can add

-   An individual audio file in any format that is supported for speech recognition (see [Audio formats](/docs/services/speech-to-text/audio-formats.html)). For more information about using audio-type resources, see [Working with audio files](/docs/services/speech-to-text/acoustic-resource.html#workingAudio).
-   An archive file (a **.zip** or **.tar.gz** file) that includes multiple audio files. All audio files added with the same archive file must have the same audio format. When you load many audio files, gathering the files into a single archive file and loading that file is significantly more efficient than adding the files individually. For more information about using archive-type resources, see [Working with archive files](/docs/services/speech-to-text/acoustic-resource.html#workingArchive).

You use the `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method to add an audio resource of either type to a custom acoustic model. When you add an audio resource, you assign it an `audio_name`. Use a localized name that matches the language of the custom model and reflects the contents of the resource.

-   Include a maximum of 128 characters in the name.
-   Do not include spaces, `/` (slashes), or `\` (backslashes) in the name.
-   Do not use the name of an audio resource that has already been added to the custom model.

The following examples show the addition of both audio- and archive-type resources. In both cases, the audio resource is passed as the body of the request.

-   The following example adds an audio-type resource to the acoustic custom model with the specified `customization_id`. The `Content-Type` header identifies the type of the audio as `audio/wav`. The audio file, **audio1.wav**, is passed as the body of the request, and the resource is given the named `audio1`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   The following example adds an archive-type resource to the specified custom acoustic model. The `Content-Type` header identifies the type of the archive as `application/zip`. The `Contained-Contented-Type` header indicates that all files that are contained in the archive have the format `audio/l16` and are sampled at a rate of 16 kHz. The archive file, **audio2.zip**, is passed as the body of the request, and the resource is given the name `audio2`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

The method also accepts an optional `allow_overwrite` query parameter overwrites an existing audio resource for a custom model. Use the parameter if you need to update an audio resource after you add it to a model.

The method is asynchronous. It can take several seconds to complete depending on the duration of the audio. For an archive file, the length of the operation depends on the duration of the audio files that are being processed. For more information about checking the status of a request to add an audio resource, see [Monitoring the add audio request](#monitorAudio).

You can add any number of audio resources to a custom model by calling the method once for each audio or archive file. The addition of one audio resource must be fully complete before you can add another. You must add a minimum of 10 minutes and a maximum of 50 hours of audio that includes speech, not silence, to a custom model before you can train it. No audio- or archive-type resource can be larger than 100 MB. For more information about adding audio to a custom model, see [Guidelines for adding audio resources](/docs/services/speech-to-text/acoustic-resource.html#audioGuidelines).

### Monitoring the add audio request
{: #monitorAudio}

The service returns a 201 response code if the audio is valid. It then asynchronously analyzes the contents of the audio file or files and automatically extracts information about the audio such as length, sampling rate, and encoding. You cannot submit requests to add more audio to a custom acoustic model, or to train the model, until the service's analysis of all audio files for the current request completes.

To determine the status of the request, use the `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method to poll the status
of the audio. The method accepts the GUID of the custom model and the name of the audio resource. Its response includes the `status` of the resource, which has one of the following values:

-   `ok` indicates that the audio is acceptable and analysis is complete.
-   `being_processed` indicates that the service is still analyzing the audio.
-   `invalid` indicates that the audio file is not acceptable for processing. It might have the wrong format, the wrong sampling rate, or not be an audio file. For an archive file, if any of the audio files that it contains are invalid, the entire archive is invalid.

The content of the response and location of the `status` field depend on the type of the resource, audio or archive.

-   *For an audio-type resource,* the `status` field is located in the top-level (`AudioListing`) object.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *For an archive-type resource,* the `status` field is located in the second-level (`AudioResource`) object that is nested in the `container` field.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    ```
    {: codeblock}

Use a loop to check the status of the audio resource every few seconds until it becomes `ok`. For more information about other fields that are returned by the method, see [Listing audio resources for a custom acoustic model](/docs/services/speech-to-text/acoustic-audio.html#listAudio).

## Train the custom acoustic model
{: #trainModel}

Once you populate a custom acoustic model with audio resources, you must train the model on the new data. Training prepares a custom model for use in speech recognition. A model cannot be used for recognition requests until you train it on the new data. Also, updates to a custom model in the form of new or changed audio resources are not reflected by the model until you train it with the changes.

You use the `POST /v1/acoustic_customizations/{customization_id}/train` method to train a custom model. You pass the method the customization ID of the model that you want to train, as in the following example.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

The method also accepts an optional `custom_language_model_id` query parameter to specify a separately created custom language model that is to be used during training. Create a custom language model that contains transcriptions of your audio files, corpora (text files), or a list of words that are relevant to the contents of the audio files. You can also use an existing custom language model that contains related OOV words. For more information, see [Training a custom acoustic model with a custom language model](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).

The training method is asynchronous. Training can take on the order of minutes or hours to complete, depending on the amount of audio data that the custom acoustic model contains and the current load on the service. A general guideline is that training a custom acoustic model takes approximately two to four times the length of its audio data. The range of time depends on the model that is being trained and the nature of the audio, such as whether the audio is clean or noisy. For example, it can take between 4 and 8 hours to train a model that contains 2 hours of audio. For more information about checking the status of a training operation, see [Monitoring the train model request](#monitorTraining).

### Monitoring the train model request
{: #monitorTraining}

The service returns a 200 response code if the training process is successfully initiated. The service cannot accept subsequent training requests, or requests to add more audio resources, until the existing request completes.

To determine the status of a training request, use the `GET /v1/acoustic_customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the acoustic model and returns its status, as in the following example:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

The response includes `status` and `progress` fields that report the current state of the model. The meaning of the `progress` field depends on the model's status. The `status` field can have one of the following values:

-   `pending` indicates that the model was created but is waiting either for training data to be added or for the service to finish analyzing data that was added. The `progress` field is `0`.
-   `ready` indicates that the model is ready to be trained. The `progress` field is `0`.
-   `training` indicates that the model is being trained. The `progress` field changes from `0` to `100` when training is complete. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indicates that the model is trained and ready to use. The `progress` field is `100`.
-   `upgrading` indicates that the model is being upgraded. The `progress` field is `0`.
-   `failed` indicates that training of the model failed. The `progress` field is `0`.

Use a loop to check the status of the training once a minute until the model becomes `available`. For more information about other fields that are returned by the method, see [Listing custom acoustic models](/docs/services/speech-to-text/acoustic-models.html#listModels).

### Training failures
{: #failedTraining}

Training of a custom acoustic model fails to start if the service is handling another request for the custom model. A conflicting request could be another training request or a request to add audio resources to the model. Training can also fail to start for the following reasons:

-   The custom model contains less than 10 minutes of audio data.
-   The custom model contains more than 50 hours of audio data.
-   One or more of the custom model's audio resources is invalid.

If the status of a custom model's training is `failed`, use the `GET /v1/acoustic_customizations/{customization_id}/audio` and `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` methods to examine the model's audio resources and address any problems that you find. For more information, see [Listing audio resources for a custom acoustic model](/docs/services/speech-to-text/acoustic-audio.html#listAudio).
