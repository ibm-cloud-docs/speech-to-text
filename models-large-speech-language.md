---

copyright:
  years: 2021, 2024
lastupdated: "2024-04-29"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

#  Large speech languages and models
{: #models-large-speech-languages}

Draft: The {{site.data.keyword.speechtotextfull}} service supports a growing collection of next-generation models that improve upon the speech recognition capabilities of the service's previous-generation models. The model indicates the language in which the audio is spoken and the rate at which it is sampled. Next-generation models have higher throughput than the previous-generation models, so the service can return transcriptions more quickly.  Next-generation models also provide noticeably better transcription accuracy.
{: shortdesc}

## Supported large speech model languages
{: #models-supported-languages}

Draft: The following sections list the large speech models that are available for each language. The tables in the sections provide the following information:

| Language |  Model name | Status | 
|------------------------|:-----------:|:----------------------------------------|
| English  \n (Australian) | `en-AU` | GA \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English  \n (Indian) | `en-IN` | GA \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English \n (United Kingdom) | `en-GB` | GA \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English \n (United States) | `en-US` | GA \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| French \n (Canadian) | `fr-CA` | Beta \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| French \n (France) | `fr-FR` | Beta \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| Japanese | `ja-JP` | Beta \n [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
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

Large speech models also support the following parameters, which are not available with previous-generation models:

-   `character_insertion_bias`, which is supported by all large speech models. For more information, see [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias).

Large speech models also differ from previous-generation models with respect to the following additional features:

-   Large speech models do not produce hesitation markers. They instead include the actual hesitations in transcription results. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).
-   Large speech models support automatic capitalization and punctuation only for the English model. For more information, see [Capitalization](/docs/speech-to-text?topic=speech-to-text-basic-response#response-capitalization).
