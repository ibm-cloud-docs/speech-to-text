---

copyright:
  years: 2015, 2021
lastupdated: "2021-08-17"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Listing information about models
{: #models-list}

The {{site.data.keyword.speechtotextfull}} service provides methods for listing information about all of its available models or about a specific model.
{: shortdesc}

## Model information
{: #models-list-info}

Regardless of whether you list information about all available models or about a specific model, the service returns the same output for a model:

-   `name` is the name of the model that you use in a request.
-   `language` is the language identifier of the model (for example, `en-US`).
-   `rate` identifies the minimum acceptable sampling rate in Hertz for audio that is used with the model.
-   `url` is the URI for the model.
-   `description` provides a brief description of the model.
-   `supported_features` describes the additional service features that are supported with the model:
    -   `custom_language_model` is a boolean that indicates whether you can create custom language models that are based on the model.
    -   `low_latency` is a boolean that indicates whether you can use the `low_latency` parameter with a next-generation model. The service includes this field only for next-generation models. Previous-generation models do not support the `low_latency` parameter.
    -   `speaker_labels` indicates whether you can use the `speaker_labels` parameter with the model.

    The `speaker_labels` field returns `true` for all models. However, speaker labels are supported as beta functionality for only a limited number of languages. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).
    {: note}

## Listing all models
{: #models-list-all}

You use the HTTP `GET /v1/models` method to list information about all available models. The service returns an array of JSON objects that provides information about all of the models.

The order in which the service returns models can change from call to call. So, do not rely on an alphabetized or static list of models. Because the models are returned as an array of JSON objects, the order has no bearing on programmatic uses of the response.

### List all models example
{: #models-list-all-example}

The following example lists all models that are supported by the service:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/models"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/models"
```
{: pre}

The response is abbreviated to show only the first few models.

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "{url}/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "{url}/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "{url}/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

## Listing a specific model
{: #models-list-specific}

You use the HTTP `GET /v1/models/{model_id}` method to list information about a specified model. The service returns information only for that model.

### List a specific model examples
{: #models-list-specific-examples}

The following example shows information about the US English broadband model:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/models/en-US_BroadbandModel"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/models/en-US_BroadbandModel"
```
{: pre}

The model supports both language model customization and speakers labels.

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "{url}/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}

The following example shows information about the next-generation US English telephony model:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/models/en-US_Telephony"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/models/en-US_Telephony"
```
{: pre}

The model supports low latency and speakers labels. It does not support language model customization.

```javascript
{
   "name": "en-US_Telephony",
   "rate": 8000,
   "language": "en-US",
   "description": "US English telephony model for narrowband audio (8kHz)",
   "supported_features": {
      "custom_language_model": false,
      "low_latency": true,
      "speaker_labels": true
   },
   "url": "{url}/v1/models/en-US_Telephony"
}
```
{: codeblock}
