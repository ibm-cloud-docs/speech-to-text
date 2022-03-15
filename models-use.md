---

copyright:
  years: 2022
lastupdated: "2022-03-08"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Using a model for speech recognition
{: #models-use}

You use the `model` parameter of a speech recognition request to indicate the model that is to be used with the request. You can specify a previous- or next-generation model with the parameter. If you omit the `model` parameter from a speech recognition request, the service uses the previous-generation US English broadband model, `en-US_BroadbandModel`, by default.
{: shortdesc}

For more information about the models that are available for speech recognition, see

-   [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
-   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)

## Specify a previous-generation model example
{: #models-pg-specify-example}

The following example HTTP request uses the previous-generation model `en-US_NarrowbandModel` for speech recognition:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```sh
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Specify a next-generation model example
{: #models-ng-specify-example}

The following example HTTP request uses the next-generation `en-US_Telephony` model for speech recognition:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```sh
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}
