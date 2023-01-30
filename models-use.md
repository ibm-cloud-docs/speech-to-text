---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Using a model for speech recognition
{: #models-use}

You use the `model` parameter of a speech recognition request to indicate the model that is to be used with the request. You can specify a previous- or next-generation model with the parameter.
{: shortdesc}

For more information about the models that are available for speech recognition, see

-   [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
-   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)

## Specify a previous-generation model example
{: #models-pg-specify-example}

The following example HTTP request uses the previous-generation model `en-US_NarrowbandModel` for speech recognition:

[IBM Cloud]{: tag-ibm-cloud}

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

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

[IBM Cloud]{: tag-ibm-cloud}

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```sh
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?model=en-US_Telephony"
```
{: pre}

## Using the default model
{: #models-use-default}

If you omit the `model` parameter from a speech recognition request, the service uses the US English `en-US_BroadbandModel` by default. This default applies to all speech recognition requests.

[IBM Cloud Pak for Data]{: tag-cp4d} If you do not install the `en-US_BroadbandModel`, it cannot serve as the default model. In this case, you must either

-   Use the `model` parameter to pass the model that is to be used with each request.
-   Specify a new default model for your installation of {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} by using the `defaultSTTModel` property in the Speech services custom resource. For more information, see  [Installing {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=text-installing-watson-speech){: external}.
