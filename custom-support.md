---

copyright:
  years: 2015, 2022
lastupdated: "2022-04-01"

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

Effective 15 March 2022, previous-generation models for all languages other than Arabic and Japanese are deprecated. The deprecated models remain available until 15 September 2022, when they will be removed from the service and the documentation. You must migrate to the equivalent next-generation model by the end of service date. For more information, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
{: deprecated}

Table 1 lists the previous-generation models that are supported for language model customization, grammars, and acoustic model customization. To learn which models are supported for speech recognition for {{site.data.keyword.cloud}}, {{site.data.keyword.icp4dfull}}, or both, see [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported).

| Language (dialect) | Models | Language model customization | Grammars | Acoustic model customization |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_BroadbandModel` | Not supported | Not supported | GA |
| Chinese  \n (Mandarin) | `zh-CN_NarrowbandModel`  \n Deprecated | Not supported | Not supported | GA |
|  | `zh-CN_BroadbandModel`  \n Deprecated | Not supported | Not supported | GA |
| Dutch  \n (Netherlands) | `nl-NL_NarrowbandModel`  \n Deprecated | GA | GA | GA |
|  | `nl-NL_BroadbandModel`  \n Deprecated | GA | GA | GA |
| English  \n (Australian) | `en-AU_NarrowbandModel`  \n Deprecated | GA | GA | GA |
|  | `en-AU_BroadbandModel`  \n Deprecated | GA | GA | GA |
| English  \n (United Kingdom) | `en-GB_NarrowbandModel`  \n Deprecated | GA | GA | GA |
|  | `en-GB_BroadbandModel`  \n Deprecated | GA | GA | GA |
| English  \n (United States) | `en-US_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `en-US_BroadbandModel`  \n Deprecated | GA | GA | GA |
| | `en-US_ShortForm_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| French  \n (Canadian) | `fr-CA_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `fr-CA_BroadbandModel`  \n Deprecated | GA | GA | GA |
| French  \n (France) | `fr-FR_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `fr-FR_BroadbandModel`  \n Deprecated | GA | GA | GA |
| German | `de-DE_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `de-DE_BroadbandModel`  \n Deprecated | GA | GA | GA |
| Italian | `it-IT_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `it-IT_BroadbandModel`  \n Deprecated | GA | GA | GA |
| Japanese | `ja-JP_NarrowbandModel` | GA | GA | GA |
| | `ja-JP_BroadbandModel` | GA | GA | GA |
| Korean | `ko-KR_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `ko-KR_BroadbandModel`  \n Deprecated | GA | GA | GA |
| Portuguese  \n (Brazilian) | `pt-BR_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `pt-BR_BroadbandModel`  \n Deprecated | GA | GA | GA |
| Spanish  \n (Argentinian) | `es-AR_NarrowbandModel`  \n Deprecated | Beta | Beta | Beta |
| | `es-AR_BroadbandModel`  \n Deprecated | Beta | Beta | Beta |
| Spanish  \n (Castilian) | `es-ES_NarrowbandModel`  \n Deprecated | GA | GA | GA |
| | `es-ES_BroadbandModel`  \n Deprecated | GA | GA | GA |
| Spanish  \n (Chilean) | `es-CL_NarrowbandModel`  \n Deprecated | Beta | Beta | Beta |
| | `es-CL_BroadbandModel`  \n Deprecated | Beta | Beta | Beta |
| Spanish  \n (Colombian) | `es-CO_NarrowbandModel`  \n Deprecated | Beta | Beta | Beta |
| | `es-CO_BroadbandModel`  \n Deprecated | Beta | Beta | Beta |
| Spanish  \n (Mexican) | `es-MX_NarrowbandModel`  \n Deprecated | Beta | Beta | Beta |
| | `es-MX_BroadbandModel`  \n Deprecated | Beta | Beta | Beta |
| Spanish  \n (Peruvian) | `es-PE_NarrowbandModel`  \n Deprecated | Beta | Beta | Beta |
| | `es-PE_BroadbandModel`  \n Deprecated | Beta | Beta | Beta |
{: caption="Table 1. Previous-generation language support for customization"}

## Language support for next-generation models
{: #custom-language-support-ng}

Table 2 lists the next-generation models that are supported for language model customization, grammars, and acoustic model customization. To learn which models are supported for speech recognition for {{site.data.keyword.cloud}}, {{site.data.keyword.icp4dfull}}, or both, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).

| Language (dialect) |  Models | Language model customization | Grammars | Acoustic model customization |
|------------------------|:-----------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|
| Arabic  \n (Modern Standard) | `ar-MS_Telephony` | GA | GA | Not supported |
| Chinese  \n (Mandarin) | `zh-CN_Telephony` | GA | GA | Not supported |
| Czech | `cz-CZ_Telephony` | GA | GA | Not supported |
| Dutch  \n (Belgian) | `nl-BE_Telephony` | GA | GA | Not supported |
| Dutch  \n (Netherlands) | `nl-NL_Telephony` | GA | GA | Not supported |
| English  \n (Australian) | `en-AU_Telephony` | GA | GA | Not supported |
| | `en-AU_Multimedia` | GA | GA | Not supported |
| English  \n (Indian) | `en-IN_Telephony` | GA | GA | Not supported |
| English  \n (United Kingdom) | `en-GB_Telephony` | GA | GA | Not supported |
| | `en-GB_Multimedia` | GA | GA  | Not supported |
| English  \n (United States) | `en-US_Telephony` | GA | GA | Not supported |
| | `en-US_Multimedia` | GA | GA | Not supported |
| English  \n (all supported dialects) | `en-WW_Medical_Telephony` | Beta | Beta | Not supported |
| French  \n (Canadian) | `fr-CA_Telephony` | GA | GA | Not supported |
| French  \n (France) | `fr-FR_Telephony` | GA | GA | Not supported |
| | `fr-FR_Multimedia` | GA | GA | Not supported |
| German | `de-DE_Telephony` | GA | GA | Not supported |
| | `de-DE_Multimedia` | GA | GA | Not supported |
| Hindi | `hi-IN_Telephony` | GA | GA | Not supported |
| Italian | `it-IT_Telephony` | GA | GA | Not supported |
| Japanese | `ja-JP_Multimedia` | GA | GA | Not supported |
| Korean | `ko-KR_Telephony` | GA | GA | Not supported |
| | `ko-KR_Multimedia` | GA | GA | Not supported |
| Portuguese  \n (Brazilian) | `pt-BR_Telephony` | GA | GA | Not supported |
| | `pt-BR_Multimedia` | GA | GA | Not supported |
| Spanish  \n (Castilian) | `es-ES_Telephony` | GA | GA | Not supported |
| | `es-ES_Multimedia` | GA | GA | Not supported |
| Spanish  \n (Argentinian, Chilean,  \n Colombian, Mexican,  \n and Peruvian) | `es-LA_Telephony` | GA | GA | Not supported |
{: caption="Table 2. Next-generation language support for customization"}
