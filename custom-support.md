---

copyright:
  years: 2015, 2022
lastupdated: "2022-01-14"

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

## Language support for previous-generation models
{: #custom-language-support-pg}

Table 1 lists the previous-generation models that are supported for language model customization, grammars, and acoustic model customization. To learn which models are supported for speech recognition for {{site.data.keyword.cloud}}, {{site.data.keyword.icp4dfull}}, or both, see [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported).

| Language (dialect) | Models | Language model customization | Grammars | Acoustic model customization |
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

## Language support for next-generation models
{: #custom-language-support-ng}

Table 2 lists the next-generation models that are supported for language model customization, grammars, and acoustic model customization. To learn which models are supported for speech recognition for {{site.data.keyword.cloud_notm}}, {{site.data.keyword.icp4dfull_notm}}, or both, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).

*As of 3 December 2021 for {{site.data.keyword.cloud_notm}}* and *20 December 2021 for {{site.data.keyword.icp4dfull_notm}}*, custom language models based on certain next-generation models must be re-created. If you created custom language models based on certain versions of the `en-AU_Telephony`, `en-GB_Telephony`, `en-US_Telephony`, or `en-US_Multimedia` models, you must re-create the custom models. Until you do, speech recognition requests that attempt to use the custom models fail with HTTP error code 400. For more information, see the [3 December 2021](/docs/speech-to-text?topic=speech-to-text-release-notes#speech-to-text-3december2021) update in the release notes for {{site.data.keyword.cloud_notm}} and the [20 December 2021 (Version 4.0.4)](/docs/speech-to-text?topic=speech-to-text-release-notes-data#speech-to-text-data-20december2021) update in the release notes {{site.data.keyword.icp4dfull_notm}}.
{: important}

| Language (dialect) |  Models | Language model customization | Grammars | Acoustic model customization |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_Telephony` | GA | Beta | Not supported |
| Chinese  \n (Mandarin) | `zh-CN_Telephony` | GA | Beta | Not supported |
| Czech | `cz-CZ_Telephony` | GA | Beta | Not supported |
| Dutch  \n (Belgian) | `nl-BE_Telephony` | GA | Beta | Not supported |
| Dutch  \n (Netherlands) | `nl-NL_Telephony` | GA | Beta | Not supported |
| English  \n (Australian) | `en-AU_Multimedia` | GA | Beta | Not supported |
| | `en-AU_Telephony` | GA | Beta | Not supported |
| English  \n (Indian) | `en-IN_Telephony` | GA | Beta | Not supported |
| English  \n (United Kingdom) | `en-GB_Multimedia` | GA | Beta | Not supported |
| | `en-GB_Telephony` | GA | Beta  | Not supported |
| English  \n (United States) | `en-US_Multimedia` | GA | Beta | Not supported |
| | `en-US_Telephony` | GA | Beta | Not supported |
| French  \n (Canadian) | `fr-CA_Telephony` | GA | Beta | Not supported |
| French  \n (France) | `fr-FR_Multimedia` | GA | Beta | Not supported |
| | `fr-FR_Telephony` | GA | Beta | Not supported |
| German | `de-DE_Telephony` | GA | Beta | Not supported |
| Hindi | `hi-IN_Telephony` | GA | Beta | Not supported |
| Italian | `it-IT_Telephony` | GA | Beta | Not supported |
| Japanese | `ja-JP_Multimedia` | GA | Beta | Not supported |
| Korean | `ko-KR_Multimedia` | GA | Beta | Not supported |
| | `ko-KR_Telephony` | GA | Beta | Not supported |
| Portuguese  \n (Brazilian) | `pt-BR_Telephony` | GA | Beta | Not supported |
| Spanish  \n (Castilian) | `es-ES_Telephony` | GA | Beta | Not supported |
| Spanish  \n (Latin American) | `es-LA_Telephony` | GA | Beta | Not supported |
{: caption="Table 2. Next-generation language support for customization"}
