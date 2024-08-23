---

copyright:
  years: 2021, 2024
lastupdated: "2024-08-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

#  Large speech languages and models
{: #models-large-speech-languages}

The {{site.data.keyword.speechtotextfull}} service supports a growing collection of Large Speech Models (LSMs) that improve upon the speech recognition capabilities of the service's previous-generation models. The model name is the locale, which consists of the language code and the region/country code separated by dash. For example `en-US` is for English spoken in the United states. LSMs are large models, they have a large number of trainable parameters and are trained on very large amounts of audio. Because of their large size, the large amounts of training material they are built on, and the state of the art architecture and training recipe used to build them, these models deliver exceptional transcription accuracy, far superior to previous models available.

These models can be used both for Telephony use cases and Broadband use cases. 
{: shortdesc}

## Supported large speech model languages
{: #models-supported-languages}

The following sections list the large speech models that are available for each language. The tables in the sections provide the following information:

| Language |  Model name | Status | 
|------------------------|:-----------:|:----------------------------------------|
| English  \n (Australian) | `en-AU` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English  \n (Indian) | `en-IN` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English \n (United Kingdom) | `en-GB` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English \n (United States) | `en-US` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| French \n (Canadian) | `fr-CA` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| French \n (France) | `fr-FR` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| Japanese | `ja-JP` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| Portuguese (Brazilian) | `pt-BR` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024  | 
| Portuguese (Portugal) | `pt-PT` | [IBM Cloud]{: tag-ibm-cloud} 23 August 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 |
| Spanish (Castilian) | `es-ES` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Argentinian) | `es-AR` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Chilean) | `es-CL` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Colombian) | `es-CO` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Mexican) | `es-MX` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Peruvian) | `es-PE` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
{: caption="Table 1. Large speech models"}

## Supported features for large speech models
{: #models-lsm-supported-features}

The large speech models are supported for use with a large subset of the service's speech recognition features. In cases where a supported feature is restricted to certain languages, the same language restrictions usually apply to large speech models, previous- and next-generation models.

-   For more information about the parameters that you can use with large speech models, including their language support and whether the parameters are GA or beta, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
-   For more information about large speech models' support for customization, see [Customization support for large speech models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-lsm).

Large speech models support all speech recognition parameters and headers *except* for the following:

-   `acoustic_customization_id` (Large speech models do not support acoustic model customization.)
-   `keywords` and `keywords_threshold`
-   `word_alternatives_threshold`
-   `grammar_name` (Large speech models do not support grammar customization.)
-   `low_latency` (Large speech models natively support low latency out of the box.)
-   `character_insertion_bias`

Large speech models also differ from previous-generation models with respect to the following additional features:

-   Large speech models do not produce hesitation markers. They instead include the actual hesitations in transcription results. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).
