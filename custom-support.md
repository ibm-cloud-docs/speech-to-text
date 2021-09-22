---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-22"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Language support and usage notes for customization
{: #custom-support}

The {{site.data.keyword.speechtotextfull}} service supports language model customization, acoustic model customization, and grammars at different levels for different languages and types of models. In addition, customization works differently for previous- and next-generation models. However, topics such as custom model ownership, limits on the number of custom models that you can create, and security are common to all supported languages and model types.
{: shortdesc}

## Language support for customization
{: #custom-language-support}
{: help}
{: support}

The tables in the following sections document the service's level of support for all languages and types of models:

-   *GA* indicates that the feature is generally available for production use for that language.
-   *Beta* indicates that the feature is available as a beta offering for that language.
-   *Not supported* means that the feature is not available for that language.

### Language support for previous-generation models
{: #custom-language-support-pg}

To learn which models are supported for {{site.data.keyword.cloud}}, {{site.data.keyword.icp4dfull}}, or both, see [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported).

| Language<br/>(dialect) | <br/>Models | Language model<br/>customization | <br/>Grammars | Acoustic model<br/>customization |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_BroadbandModel` | Not supported | Not supported | GA |
| Chinese  \n (Mandarin) | `zh-CN_BroadbandModel`  \n   \n `zh-CN_NarrowbandModel` | Not supported | Not supported | GA |
| Dutch  \n (Netherlands) | `nl-NL_BroadbandModel`  \n   \n `nl-NL_NarrowbandModel` | GA | GA | GA |
| English  \n (Australian) | `en-AU_BroadbandModel`  \n   \n `en-AU_NarrowbandModel` | GA | GA | GA |
| English  \n (United Kingdom) | `en-GB_BroadbandModel`  \n   \n `en-GB_NarrowbandModel` | GA | GA | GA |
| English  \n (United States) |  `en-US_BroadbandModel`  \n   \n `en-US_NarrowbandModel`  \n   \n `en-US_ShortForm_NarrowbandModel` | GA | GA | GA |
| French  \n (Canadian) | `fr-CA_BroadbandModel`  \n   \n `fr-CA_NarrowbandModel` | GA | GA | GA |
| French  \n (France) | `fr-FR_BroadbandModel`  \n   \n `fr-FR_NarrowbandModel` | GA | GA | GA |
| German | `de-DE_BroadbandModel`  \n   \n `de-DE_NarrowbandModel` | GA | GA | GA |
| Italian | `it-IT_BroadbandModel`  \n   \n `it-IT_NarrowbandModel` | GA | GA | GA |
| Japanese | `ja-JP_BroadbandModel`  \n   \n `ja-JP_NarrowbandModel` | GA | GA | GA |
| Korean | `ko-KR_BroadbandModel`  \n   \n `ko-KR_NarrowbandModel` | GA | GA | GA |
| Portuguese  \n (Brazilian) | `pt-BR_BroadbandModel`  \n   \n `pt-BR_NarrowbandModel` | GA | GA | GA |
| Spanish  \n (Argentinian) | `es-AR_BroadbandModel`  \n   \n `es-AR_NarrowbandModel` | Beta | Beta | Beta |
| Spanish  \n (Castilian) | `es-ES_BroadbandModel`  \n   \n `es-ES_NarrowbandModel` | GA | GA | GA |
| Spanish  \n (Chilean) | `es-CL_BroadbandModel`  \n   \n `es-CL_NarrowbandModel` | Beta | Beta | Beta |
| Spanish  \n (Colombian) | `es-CO_BroadbandModel`  \n   \n `es-CO_NarrowbandModel` | Beta | Beta | Beta |
| Spanish  \n (Mexican) | `es-MX_BroadbandModel`  \n   \n `es-MX_NarrowbandModel` | Beta | Beta | Beta |
| Spanish  \n (Peruvian) | `es-PE_BroadbandModel`  \n   \n `es-PE_NarrowbandModel` | Beta | Beta | Beta |
{: caption="Table 1. Previous-generation language support for customization"}

### Language support for next-generation models
{: #custom-language-support-ng}

To learn which models are supported for {{site.data.keyword.cloud_notm}}, {{site.data.keyword.icp4dfull_notm}}, or both, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).

| Language<br/>(dialect) | <br/>Models | Language model<br/>customization | <br/>Grammars | Acoustic model<br/>customization |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_Telephony` | GA | Not supported | Not supported |
| Czech | `cz-CZ_Telephony` | Beta | Not supported | Not supported |
| Dutch  \n (Belgian) | `nl-BE_Telephony` | GA | Not supported | Not supported |
| Dutch  \n (Netherlands) | `nl-NL_Telephony` | Beta | Not supported | Not supported |
| English  \n (Australian) | `en-AU_Telephony` | GA | Not supported | Not supported |
| English  \n (Indian) | `en-IN_Telephony` | GA | Not supported | Not supported |
| English  \n (United Kingdom) | `en-GB_Telephony` | GA | Not supported | Not supported |
| English  \n (United States) | `en-US_Multimedia`  \n   \n `en-US_Telephony` | GA | Not supported | Not supported |
| French  \n (Canadian) | `fr-CA_Telephony` | GA | Not supported | Not supported |
| French  \n (France) | `fr-FR_Multimedia`  \n   \n `fr-FR_Telephony` | GA | Not supported | Not supported |
| German | `de-DE_Telephony` | GA | Not supported | Not supported |
| Hindi | `hi-IN_Telephony` | GA | Not supported | Not supported |
| Italian | `it-IT_Telephony` | GA | Not supported | Not supported |
| Japanese | `ja-JP_Multimedia` | GA | Not supported | Not supported |
| Korean | `ko-KR_Multimedia`  \n   \n `ko-KR_Telephony` | GA | Not supported | Not supported |
| Portuguese  \n (Brazilian) | `pt-BR_Telephony` | GA | Not supported | Not supported |
| Spanish  \n (Castilian) | `es-ES_Telephony` | GA | Not supported | Not supported |
{: caption="Table 2. Next-generation language support for customization"}

## Ownership of custom models
{: #custom-owner}

A custom model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use credentials for that instance of the service with methods of the customization interface. Credentials that are created for other instances of the service cannot view or access the custom model.

All credentials that are obtained for the same instance of the {{site.data.keyword.speechtotextshort}} service share access to all custom models created for that service instance. To restrict access to a custom model, create a separate instance of the service and use only the credentials for that service instance to create and work with the model. Credentials for other service instances cannot affect the custom model.

An advantage of sharing ownership across credentials for a service instance is that you can cancel a set of credentials, for example, if they become compromised. You can then create new credentials for the same service instance and still maintain ownership of and access to custom models created with the original credentials.

## Maximum number of custom models
{: #custom-maximum}

You can create no more than 1024 custom language models and no more than 1024 custom acoustic models per owning service credentials. If you try to create more than 1024 custom models of either type, the service returns an error. You do not lose any existing models, but you cannot create any more until your model count is below the limit of 1024 for the type of custom model that you are trying to create.

## Information security
{: #custom-security}

You can associate a customer ID with data that is added or updated for custom language and custom acoustic models. You associate a customer ID with corpora, custom words, grammars, and audio resources by passing the `X-Watson-Metadata` header with the following methods. If necessary, you can then delete the data by using the `DELETE /v1/user_data` method.

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

In addition, if you delete an instance of the {{site.data.keyword.speechtotextshort}} service from the {{site.data.keyword.cloud_notm}} console, all data associated with that service instance is automatically deleted. This includes all custom language models, corpora, grammars, and words, and all custom acoustic models and audio resources. This data is purged automatically and regardless of whether a customer ID is associated with the data.

For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

## Request logging and data privacy
{: #custom-logging}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only**

How the service handles request logging for calls to the customization interface depends on the request:

-   The service *does not* log data that is used to build custom models. For example, when working with corpora and words in a custom language model, you do not need to set the `X-Watson-Learning-Opt-Out` request header. Your training data is never used to improve the service's base models.
-   The service *does* log data when a custom model is used with a recognition request. You must set the `X-Watson-Learning-Opt-Out` request header to `true` to prevent logging for recognition requests.

For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging).
