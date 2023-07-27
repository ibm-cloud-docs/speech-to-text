---

copyright:
  years: 2015, 2023
lastupdated: "2023-07-27"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Language support for customization
{: #custom-support}

The {{site.data.keyword.speechtotextfull}} service supports language model customization, acoustic model customization, and grammars at different levels for different languages and types of models. The tables in the following sections document the service's level of support for all languages and models:
{: shortdesc}

-   *GA* indicates that the feature is generally available for production use for that model.
-   *Beta* indicates that the feature is available as a beta offering for that model.
-   *Not supported* means that the feature is not available for that model.

Where applicable, customization features are also identified as specific to {{site.data.keyword.cloud}} or {{site.data.keyword.icp4dfull}}. Unless otherwise indicated, a feature is available for both versions of the service.

## Customization support for previous-generation models
{: #custom-language-support-pg}

Starting **August 1, 2023**, all previous-generation models are now **discontinued** from the service. New clients must now only use the next-generation models. All existing clients must now migrate to the equivalent next-generation model. For more information, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
{: attention}

Table 1 lists the previous-generation models that are supported for language model customization, grammars, and acoustic model customization. To learn which models are supported for speech recognition for {{site.data.keyword.cloud}}, {{site.data.keyword.icp4dfull}}, or both, see [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported).

| Language (dialect) | Models | Language model customization | Grammars | Acoustic model customization |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_BroadbandModel`  \n Discontinued | Not supported | Not supported | GA |
| Chinese  \n (Mandarin) | `zh-CN_NarrowbandModel`  \n Discontinued | Not supported | Not supported | GA |
|  | `zh-CN_BroadbandModel`  \n Discontinued | Not supported | Not supported | GA |
| Dutch  \n (Netherlands) | `nl-NL_NarrowbandModel`  \n Discontinued | GA | GA | GA |
|  | `nl-NL_BroadbandModel`  \n Discontinued | GA | GA | GA |
| English  \n (Australian) | `en-AU_NarrowbandModel`  \n Discontinued | GA | GA | GA |
|  | `en-AU_BroadbandModel`  \n Discontinued | GA | GA | GA |
| English  \n (United Kingdom) | `en-GB_NarrowbandModel`  \n Discontinued | GA | GA | GA |
|  | `en-GB_BroadbandModel`  \n Discontinued | GA | GA | GA |
| English  \n (United States) | `en-US_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `en-US_BroadbandModel`  \n Discontinued | GA | GA | GA |
| | `en-US_ShortForm_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| French  \n (Canadian) | `fr-CA_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `fr-CA_BroadbandModel`  \n Discontinued | GA | GA | GA |
| French  \n (France) | `fr-FR_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `fr-FR_BroadbandModel`  \n Discontinued | GA | GA | GA |
| German | `de-DE_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `de-DE_BroadbandModel`  \n Discontinued | GA | GA | GA |
| Italian | `it-IT_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `it-IT_BroadbandModel`  \n Discontinued | GA | GA | GA |
| Japanese | `ja-JP_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `ja-JP_BroadbandModel`  \n Discontinued | GA | GA | GA |
| Korean | `ko-KR_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `ko-KR_BroadbandModel`  \n Discontinued | GA | GA | GA |
| Portuguese  \n (Brazilian) | `pt-BR_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `pt-BR_BroadbandModel`  \n Discontinued | GA | GA | GA |
| Spanish  \n (Argentinian) | `es-AR_NarrowbandModel`  \n Discontinued | Beta | Beta | Beta |
| | `es-AR_BroadbandModel`  \n Discontinued | Beta | Beta | Beta |
| Spanish  \n (Castilian) | `es-ES_NarrowbandModel`  \n Discontinued | GA | GA | GA |
| | `es-ES_BroadbandModel`  \n Discontinued | GA | GA | GA |
| Spanish  \n (Chilean) | `es-CL_NarrowbandModel`  \n Discontinued | Beta | Beta | Beta |
| | `es-CL_BroadbandModel`  \n Discontinued | Beta | Beta | Beta |
| Spanish  \n (Colombian) | `es-CO_NarrowbandModel`  \n Discontinued | Beta | Beta | Beta |
| | `es-CO_BroadbandModel`  \n Discontinued | Beta | Beta | Beta |
| Spanish  \n (Mexican) | `es-MX_NarrowbandModel`  \n Discontinued | Beta | Beta | Beta |
| | `es-MX_BroadbandModel`  \n Discontinued | Beta | Beta | Beta |
| Spanish  \n (Peruvian) | `es-PE_NarrowbandModel`  \n Discontinued | Beta | Beta | Beta |
| | `es-PE_BroadbandModel`  \n Discontinued | Beta | Beta | Beta |
{: caption="Table 1. Previous-generation language support for customization"}

## Customization support for next-generation models
{: #custom-language-support-ng}

Table 2 lists the next-generation models that are supported for language model customization and grammars. Next-generation language models do not support acoustic model customization. To learn which models are supported for speech recognition for {{site.data.keyword.cloud}}, {{site.data.keyword.icp4dfull}}, or both, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).

In the *Language model customization* column, an *(improved)* date indicates when a model was migrated to the new language model customization technology. Make sure to check for a date for the version of the service that you use, *{{site.data.keyword.cloud_notm}}* or *{{site.data.keyword.icp4dfull_notm}}*. Models that do not include an *(improved)* date are not yet migrated to the new technology. For more information about the improved technology, see [Improved language model customization for next-generation models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng).
{: note}

| Language (dialect) |  Models | Language model customization (improved) | Grammars |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_Telephony` | GA | GA |
| Chinese  \n (Mandarin) | `zh-CN_Telephony` | GA | GA |
| Czech | `cs-CZ_Telephony` | GA | GA |
| Dutch  \n (Belgian) | `nl-BE_Telephony` | GA | GA |
| Dutch  \n (Netherlands) | `nl-NL_Telephony` | GA | GA |
| | `nl-NL_Multimedia` | GA | GA |
| English  \n (Australian) | `en-AU_Telephony` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA |
| | `en-AU_Multimedia` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| English  \n (Indian) | `en-IN_Telephony` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| English  \n (United Kingdom) | `en-GB_Telephony` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| | `en-GB_Multimedia` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| English  \n (United States) | `en-US_Telephony` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| | `en-US_Multimedia` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| English  \n (all supported dialects) | `en-WW_Medical_Telephony` | Beta | Beta |
| French  \n (Canadian) | `fr-CA_Telephony` | GA | GA |
| | `fr-CA_Multimedia` | GA | GA |
| French  \n (France) | `fr-FR_Telephony` | GA | GA |
| | `fr-FR_Multimedia` | GA | GA |
| German | `de-DE_Telephony` | GA | GA |
| | `de-DE_Multimedia` | GA | GA |
| Hindi | `hi-IN_Telephony` | GA | GA |
| Italian | `it-IT_Telephony` | GA | GA |
|  | `it-IT_Multimedia` | GA | GA |
| Japanese | `ja-JP_Telephony` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| | `ja-JP_Multimedia` | GA  \n [IBM Cloud]{: tag-ibm-cloud} 27 February 2023  \n  [IBM Cloud Pak for Data]{: tag-cp4d} 1 May 2023 | GA | GA |
| Korean | `ko-KR_Telephony` | GA | GA |
| | `ko-KR_Multimedia` | GA | GA |
| Portuguese  \n (Brazilian) | `pt-BR_Telephony` | GA | GA |
| | `pt-BR_Multimedia` | GA | GA |
| Spanish  \n (Castilian) | `es-ES_Telephony` | GA | GA |
| | `es-ES_Multimedia` | GA | GA |
| Spanish  \n (Argentinian, Chilean,  \n Colombian, Mexican,  \n and Peruvian) | `es-LA_Telephony` | GA | GA |
| Swedish | `sv-SE_Telephony` | GA | GA |
{: caption="Table 2. Next-generation language support for customization"}
