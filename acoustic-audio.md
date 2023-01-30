---

copyright:
  years: 2019, 2023
lastupdated: "2023-01-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Managing audio resources
{: #manageAudio}

Acoustic model customization is available only for previous-generation models. It is not available for next-generation models.
{: note}

The customization interface includes the `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method, which is used to add an audio resource to a custom acoustic model. For more information, see [Add audio to the custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#addAudio). The interface also includes the following methods for listing and deleting audio resources for a custom acoustic model.
{: shortdesc}

## Listing audio resources for a custom acoustic model
{: #listAudio}

The customization interface provides two methods for listing information about the audio resources of a custom acoustic model:

-   The `GET /v1/acoustic_customizations/{customization_id}/audio` method lists information about all audio resources that have been added to a custom model.
-   The `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method lists information about a specified audio resource for a custom model.

Both methods return the `name` of the audio resource plus the following additional information:

-   `duration` is the length in seconds of the audio. For an archive file, the figure represents the accumulated duration of all of the audio files that are contained in the archive.
-   `details` identifies the type of the resource: `audio` or `archive`. (The type is `undetermined` if the service cannot validate the resource, possibly because the user mistakenly passed a file that does not contain audio.) For an audio file, the details include the `codec` and `frequency` of the audio. For an archive file, they include its `compression` type.

Additionally, listing all audio resources for a model returns the `total_minutes_of_audio` summed over all of the valid audio resources for the model. You can use this value to determine whether the custom model has enough or too much audio to begin training.

The methods also list the status of the audio data. The status is important for checking the service's analysis of audio files in response to a request to add them to a custom model:

-   `ok` indicates that the service has successfully analyzed the audio data. The data can be used to train the custom model.
-   `being_processed` indicates that the service is still analyzing the audio data. The service cannot accept requests to add new audio or to train the custom model until its analysis is complete.
-   `invalid` indicates that the audio data is not valid for training the model (possibly because it has the wrong format or sampling rate, or because it is corrupted).

### List all audio resources example
{: #listExample-audio}

The following example lists all audio resources for the custom acoustic model with the specified customization ID:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

The acoustic model has three audio resources. The service has successfully analyzed `audio1` and `audio2`; it is still analyzing `audio3`.

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### List an audio-type resource example
{: #getExampleAudio}

The following example returns information about the audio-type resource named `audio1`:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

The resource is 131 seconds long and is encoded with the `pcm_s16le` codec. It was successfully added to the model.

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

### List an archive-type resource example
{: #getExampleArchive}

The following example returns information about the archive-type resource named `audio2`:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

The resource is a `.zip` file that contains more than nine minutes of audio. It too was successfully added to the model. As the example shows, querying information about an archive-type resource also provides information about the files that it contains.

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
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## Deleting an audio resource from a custom acoustic model
{: #deleteAudio}

Use the `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method to remove an existing audio resource from a custom acoustic model. When you delete an archive-type audio resource, the service removes the entire archive of files. The service does not allow deletion of individual files from an archive resource.

Removing an audio resource does not affect the custom model until you train the model on its updated data by using the `POST /v1/acoustic_customizations/{customization_id}/train` method. If you successfully trained the model on the resource, until you retrain the model, the existing audio data continues to be used for speech recognition.

### Delete an audio resource example
{: #deleteExample-audio}

The following method deletes the audio resource that is named `audio3` from the custom model with the specified customization ID:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X DELETE -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X DELETE \
--header "Authorization: Bearer {token}" \
"{url}/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
