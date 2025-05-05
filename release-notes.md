---

copyright:
  years: 2015, 2025
lastupdated: "2025-05-05"

keywords: speech to text release notes,speech to text for IBM cloud release notes

subcollection: speech-to-text

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.cloud_notm}}
{: #release-notes}

[IBM Cloud]{: tag-ibm-cloud}

The following features and changes were included for each release and update of managed instances of {{site.data.keyword.speechtotextfull}} that are hosted on {{site.data.keyword.cloud_notm}} or for instances that are hosted on [{{site.data.keyword.icp4dfull_notm}} as a Service](https://dataplatform.cloud.ibm.com/docs/content/svc-welcome/wstt.html){: external}. Unless otherwise noted, all changes are compatible with earlier releases and are automatically and transparently available to all new and existing applications.
{: shortdesc}

For information about known limitations of the service, see [Known limitations](/docs/speech-to-text?topic=speech-to-text-known-limitations).

For information about releases and updates of the service for {{site.data.keyword.icp4dfull_notm}}, see [Release notes for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}}](/docs/speech-to-text?topic=speech-to-text-release-notes-data).
{: note}

## 19 November 2024
{: #speech-to-text-19nov2024}
{: release-note}

New large speech model for German is now generally available
:   The large speech model for German is now generally available.

    - For more information about large speech models, see [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages).
    - For more information about the features that are supported for large speech models, see [Supported features for large speech models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages#models-lsm-supported-features).

## 23 August 2024
{: #speech-to-text-23aug2024}
{: release-note}

All Large Speech Models are now generally available
:   The large speech models for all languages are now generally available (GA). They are supported for use in production environments and applications.

    - For more information about large speech models, see [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages).
    - For more information about the features that are supported for large speech models, see [Supported features for large speech models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages#models-lsm-supported-features).

## 18 June 2024
{: #speech-to-text-18jun2024}
{: release-note}

New large speech models for Brazilian Portuguese and Spanish are now in open beta
:   The large speech models for Brazilian Portuguese and Spanish are now in open beta. Spanish includes the Castilian, Argentinian, Chilean, Colombian, Mexican, and Peruvian dialects.

    - For more information about large speech models, see [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages).
    - For more information about the features that are supported for large speech models, see [Supported features for large speech models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages#models-lsm-supported-features).

## 15 May 2024
{: #speech-to-text-20may2024}
{: release-note}

Large Speech Model for English is now generally available
:   The large speech model for English, which includes the United States, Australian, Indian, and United Kingdom dialects, is now generally available (GA). It is supported for use in production environments and applications. 

    - For more information about large speech models, see [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages).
    - For more information about the features that are supported for large speech models, see [Supported features for large speech models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages#models-lsm-supported-features).

## 07 March 2024
{: #speech-to-text-07mar2024}
{: release-note}

Large Speech Model for US English in Open Beta
:   The new Large speech model for US English is in open beta. See [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages) for more details with supported features (beta).

## 30 November 2023
{: #speech-to-text-30nov2023}
{: release-note}

Speech to Text parameter speech_begin_event
:   This parameter would enable the client application to know that some words or speech is detected and Speech to Text is in the process of decoding. For more details, see [Using speech recognition parameters](/docs/speech-to-text?topic=speech-to-text-service-features#features-parameters). 

Parameter 'mapping_only' for custom words 
:   By using the 'mapping_only' parameter, you can use custom words directly to map 'sounds_like' (or word) to 'display_as' value as post-processing instead of training. For more information, see [The words resource](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordsResource-ng). 

:   See the guidelines for [Non-Japanese](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordsResourceAmount-words-ng) and [Japanese](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordsResourceAmount-japanese-ng). 

Support for Brazilian-Portuguese and French-Canadian on new improved next-generation language model customization 
:   Language model customization for Brazilian-Portuguese and French-Canadian next-generation models is recently added. This service update includes further internal improvements. 

New Smart Formatting Feature
:   A new smart formatting feature for next-generation models is supported in US English, Brazilian Portuguese, French and German languages. See [Smart formatting Version](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-version) for details.

Support for Castilian Spanish and LATAM Spanish on new improved next-generation language model customization 
:   The language model customization for Castilian Spanish and LATAM Spanish next-generation models are added. This service update includes further internal improvements.

Large Speech Models for English, Japanese, and French - for early access 
:   For early access feature, Large Speech Models are available for English, Japanese and French languages for you in IBM Watson Speech-to-Text and IBM watsonx Assistant. The feature set for these Large Speech Models is limited, but more accurate than Next-Generation models and are faster and cheaper to run due to smaller size and better streaming mode capability.

If you are interested in testing these base models, and sharing results and feedback, contact our Product Management team by filling out this [form](https://form.asana.com/?k=ElxhxG66qgc1CJDsehfKYQ&d=8612789739828).

## 28 July 2023
{: #speech-to-text-28july2023}
{: release-note}

Important: All previous-generation models are discontinued starting August 1, 2023
:   **Important:** All previous-generation models are now discontinued from the service. New clients must now only use the next-generation models. All existing clients must now migrate to the equivalent next-generation model. For more information about all next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng). For more information on how to migrate to the next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).

## 9 June 2023
{: #speech-to-text-09june2023}
{: release-note}

Defect fix: Creating and training a custom Language Model is now optimal for both standard and low-latency Next-Generation models
:   **Defect fix:** When creating and training a custom Language Model with corpora text files and / or custom words using a Next-generation low-latency model, it is now performing the same way as with a standard model. Previously, it was not optimal only when using a Next-Generation low-latency model.

Defect fix: STT Websockets sessions no longer fail due to tensor error message
:   **Defect fix:** When using STT websockets, sessions no longer fail due to an error message “STT returns the error: Sizes of tensors must match except in dimension 0”. 

## 18 May 2023
{: #speech-to-text-18may2023}
{: release-note}

Updates to English next-generation Medical telephony model
:   The English next-generation Medical telephony model has been updated for improved speech recognition:

    -   `en-WW_Medical_Telephony`

Added support for French and German on new improved next-generation language model customization
:   Language model customization for French and German next-generation models was recently added. This service update includes further internal improvements.

    For more information about improved next-generation customization, see

    -   [February 27, 2023 service update](/docs/speech-to-text?topic=speech-to-text-release-notes#speech-to-text-27february2023)
    -   [Improved language model customization for next-generation models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng)

Defect fix: Custom words containing half-width Katakana characters now return a clear error message with Japanese Telephony model
:   **Defect fix:** Per the [documentation](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordLanguages-jaJP-ng), only full-width Katakana characters are accepted in custom words and the next generation models now show an error message to explain that it's not supported. Previously, when creating custom words containing half-width Katakana characters, no error message was provided.

Defect fix: Japanese Telephony language model no longer fails due to long training time
:   **Defect fix:** When training a custom language model with Japanese Telephony, the service now effectively handles large numbers of custom words without failing.

## 2 May 2023
{: #speech-to-text-2may2023}
{: release-note}

New procedure for upgrading a custom model that is based on an improved next-generation model
:   Two approaches are now available to upgrade a custom language model to an improved next-generation base model. You can still modify and then retrain the custom model, as already documented. But now, you can also upgrade the custom model by including the query parameter `force=true` with the `POST /v1/customizations/{customization_id}/train` request. The `force` parameter upgrades the custom model regardless of whether it contains changes (is in the `ready` or `available` state).

    For more information, see [Upgrading a custom language model based on an improved next-generation model](/docs/speech-to-text?topic=speech-to-text-custom-upgrade#custom-upgrade-language-ng).

Guidance for adding words to custom models that are based on improved next-generation models
:   The documentation now offers more guidance about adding words to custom models that are based on improved next-generation models. For performance reasons during training, the guidance encourages the use of corpora rather than the direct addition of custom words whenever possible.

    For more information, see [Guidelines for adding words to custom models based on improved next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordsResourceAmount-words-ng).

Japanese custom words for custom models that are based on improved next-generation models are handled differently
:   For Japanese custom models that are based on next-generation models, custom words are handled differently from other languages. For Japanese, you can add a custom word or sounds-like that does not exceed 25 characters in length. If your custom word or sounds-like exceeds that limit, the service adds the word to the custom model as if it were added by a corpus. The word does not appear as a custom word for the model.

    For more information, see [Guidelines for adding words to Japanese models based on improved next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordsResourceAmount-japanese-ng).

## 12 April 2023
{: #speech-to-text-12april2023}
{: release-note}

Defect fix: The WebSocket interface now times out as expected when using next-generation models
:   **Defect fix:** When used for speech recognition with next-generation models, the WebSocket interface now times out as expected after long periods of silence. Previously, when used for speech recognition of short audio files, the WebSocket session could fail to time out. When the session failed to time out, the service did not return a final hypothesis to the waiting client application, and the client instead timed while waiting for the results.

## 6 April 2023
{: #speech-to-text-6april2023}
{: release-note}

Defect fix: Limits to allow completion of training for next-generation Japanese custom models
:   **Defect fix:** Successful training of a next-generation Japanese custom language model requires that custom words and sounds-likes added to the model each contain no more than 25 characters. For the most effective training, it is recommended that custom words and sounds-likes contain no more than 20 characters. Training of Japanese custom models with longer custom words and sounds-likes fails to complete after multiple hours of training.

   If you need to add the equivalent of a long word or sounds-like to a next-generation Japanese custom model, take these steps:
   1.  Add a shorter word or sounds-like that captures the essence of the longer word or sounds-like to the custom model.
   1.  Add one or more sentences that use the longer word or sounds-like to a corpus.
   1.  Consider adding sentences to the corpus that provide more context for the word or sounds-like. Greater context gives the service more information with which to recognize the word and apply the correct sounds-like.
   1.  Add the corpus to the custom model.
   1.  Retrain the custom model on the combination of the shorter word or sounds-like and the corpus that contains the longer string.

   The limits and steps just described allow next-generation Japanese custom models to complete training. Keep in mind that adding large numbers of new custom words to a custom language model increases the training time of the model. But the increased training time occurs only when the custom model is initially trained on the new words. Once the custom model has been trained on the new words, training time returns to normal.

    For more information, see
    -   [Add a corpus to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)
    -   [Add words to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addWords)
    -   [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)
    -   [Working with corpora and custom words for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng)

Further improvements to updated next-generation language model customization
:   Language model customization for English and Japanese next-generation models was recently improved. This service update includes further internal improvements. For more information about improved next-generation customization, see
    -   [27 February 2023 service update](#speech-to-text-27february2023) and
    -   [Improved language model customization for next-generation models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng)

## 13 March 2023
{: #speech-to-text-13march2023}
{: release-note}

Defect fix: Smart formatting for US English dates is now correct
:   **Defect fix:** Smart formatting now correctly includes days of the week and dates when both are present in the spoken audio, for example, `Tuesday February 28`. Previously, in some cases the day of the week was omitted and the date was presented incorrectly. Note that smart formatting is beta functionality.

Defect fix: Update documentation for speech hesitation words for next-generation models
:   **Defect fix:** Documentation for speech hesitation words for next-generation models has been updated. More details are provided about US English and Japanese hesitation words. Next-generation models include the actual hesitation words in transcription results, unlike previous-generation models, which include only hesitation markers. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

## 27 February 2023
{: #speech-to-text-27february2023}
{: release-note}

New Japanese next-generation telephony model
:   The service now offers a next-generation telephony model for Japanese: `ja-JP_Telephony`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see

    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Improved language model customization for next-generation English and Japanese models
:   The service now provides improved language model customization for next-generation English and Japanese models:
    -   `en-AU_Multimedia`
    -   `en-AU_Telephony`
    -   `en-IN_Telephony`
    -   `en-GB_Multimedia`
    -   `en-GB_Telephony`
    -   `en-US_Multimedia`
    -   `en-US_Telephony`
    -   `ja-JP_Multimedia`
    -   `ja-JP_Telephony`

    **Visible improvements to the models:** The new technology improves the default behavior of the new English and Japanese models. Among other changes, the new technology optimizes the default behavior for the following parameters:
    -   The default `customization_weight` for custom models that are based on the new versions of these models changes from `0.2` to `0.1`.
    -   The default `character_insertion_bias` for custom models that are based on the new versions of these models remains `0.0`, but the models have changed in a manner that makes use of the parameter for speech recognition less necessary.

    **Upgrading to the new models:** To take advantage of the improved technology, you must upgrade any custom language models that are based on the new models. To upgrade to the new version of one of these base models, do the following:
    1.  Change your custom model by adding or modifying a custom word, corpus, or grammar that the model contains. Any change that you make moves the model to the `ready` state.
    1.  Use the `POST /v1/customizations/{customization_id}/train` method to retrain the model. Retraining upgrades the custom model to the new technology and moves the model to the `available` state.

        **Known issue:** At this time, you cannot use the `POST /v1/customizations/{customization_id}/upgrade_model` method to upgrade a custom model to one of the new base models. This issue will be addressed in a future release.

    **Using the new models:** Following the upgrade to the new base model, you are advised to evaluate the performance of the upgraded custom model by paying special attention to the `customization_weight` and `character_insertion_bias` parameters for speech recognition. When you retrain your custom model:
    -   The custom model uses the new default `customization_weight` of `0.1` for your custom model. A non-default `customization_weight` that you had associated with your custom model is removed.
    -   The custom model might no longer require use of the `character_insertion_bias` parameter for optimal speech recognition.

    Improvements to language model customization render these parameters less important for high-quality speech recognition:
    -   If you use the default values for these parameters, continue to do so after the upgrade. The default values will likely continue to offer the best results for speech recognition.
    -   If you specify non-default values for these parameters, experiment with the default values following upgrade. Your custom model might work well for speech recognition with the default values.

    If you feel that using different values for these parameters might improve speech recognition with your custom model, experiment with incremental changes to determine whether the parameters are needed to improve speech recognition.

    **Note:** At this time, the improvements to language model customization apply only to custom models that are based on the next-generation English or Japanese base language models listed earlier. Over time, the improvements will be made available for other next-generation language models.

    **More information:** For more information about upgrading and about speech recognition with these parameters, see
    -   [Improved language model customization for next-generation models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng)
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight)
    -   [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias)

Defect fix: Grammar files now handle strings of digits correctly
:   **Defect fix:** When grammars are used, the service now handles longer strings of digits correctly. Previously, it was failing to complete recognition or returning incorrect results.

## 15 February 2023
{: #speech-to-text-15february2023}
{: release-note}

Important: All previous-generation models are deprecated and will reach end of service on 31 July 2023
:   **Important:** All previous-generation models are deprecated and will reach end of service effective **31 July 2023**. On that date, all previous-generation models will be removed from the service and the documentation. The previous deprecation date was 3 March 2023. The new date allows users more time to migrate to the appropriate next-generation models. But users must migrate to the equivalent next-generation model by 31 July 2023.

    Most previous-generation models were deprecated on 15 March 2022. Previously, the Arabic and Japanese models were not deprecated. Deprecation now applies to *all* previous-generation models.
    -   For more information about the next-generation models to which you can migrate from each of the deprecated models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
    -   For more information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
    -   For more information about all next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)

    **Note:** When the previous-generation `en-US_BroadbandModel` is removed from service, the next-generation `en-US_Multimedia` model will become the default model for speech recognition requests.

Defect fix: Improved training time for next-generation custom language models
:   **Defect fix:** Training time for next-generation custom language models is now significantly improved. Previously, training time took much longer than necessary, as reported for training of Japanese custom language models. The problem was corrected by an internal fix.

Defect fix: Dynamically generated grammar files now work properly
:   **Defect fix:** Dynamically generated grammar files now work properly. Previously, dynamic grammar files could cause internal failures, as reported for integration of {{site.data.keyword.speechtotextshort}} with {{site.data.keyword.conversationfull}}. The problem was corrected by an internal fix.

## 20 January 2023
{: #speech-to-text-20january2023}
{: release-note}

Deprecated Arabic and United Kingdom model names are no longer available
:   The following Arabic and United Kingdom model names are no longer accepted by the service:
    -   `ar-AR_BroadbandModel` - Use `ar-MS_BroadbandModel` instead.
    -   `en-UK_NarrowbandModel` - Use `en-GB_NarrowbandModel` instead.
    -   `en-UK_BroadbandModel` - Use `en-GB_BroadbandModel` instead.

    The Arabic model name was deprecated on 2 December 2020. The UK English model names were deprecated on 14 July 2017.

Cloud Foundry deprecation and migration to resource groups
:   {{site.data.keyword.IBM_notm}} announced the deprecation of IBM Cloud Foundry on 31 May 2022. As of 30 November 2022, new {{site.data.keyword.IBM_notm}} Cloud Foundry applications cannot be created and only existing users are able to deploy applications. {{site.data.keyword.IBM_notm}} Cloud Foundry reaches end of support on 1 June 2023. At that time, any {{site.data.keyword.IBM_notm}} Cloud Foundry application runtime instances running {{site.data.keyword.IBM_notm}} Cloud Foundry applications will be permanently disabled, deprovisioned, and deleted. 

    To continue to use your {{site.data.keyword.cloud_notm}} applications beyond 1 June 2023, you must migrate to resource groups before that date. Resource groups are conceptually similar to Cloud Foundry spaces. They include several extra benefits, such as finer-grained access control by using IBM Cloud Identity and Access Management (IAM), the ability to connect service instances to apps and service across different regions, and an easy way to view usage per group. 

The `max_alternatives` parameter is now available for use with next-generation models
:   The `max_alternatives` parameter is now available for use with all next-generation models. The parameter is generally available for all next-generation models. For more information, see [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-metadata#max-alternatives).

Defect fix: Allow use of both `max_alternatives` and `end_of_phrase_silence_time` parameters with next-generation models

:   **Defect fix:** When you use both the `max_alternatives` and `end_of_phrase_silence_time` parameters in the same request with next-generation models, the service now returns multiple alternative transcripts while also respecting the indicated pause interval. Previously, use of the two parameters in a single request generated a failure. (Use of the `max_alternatives` parameter with next-generation models was previously available as an experimental feature to a limited number of customers.)

Defect fix: Update French Canadian next-generation telephony model (upgrade required)
:   **Defect fix:** The French Canadian next-generation telephony model, `fr-CA_Telephony`, was updated to address an internal inconsistency that could cause an error during speech recognition. *You need to upgrade any custom models that are based on the `fr-CA_Telephony` model.* For more information about upgrading custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

Defect fix: Add documentation guidelines for creating Japanese sounds-likes based on next-generation models
:   **Defect fix:** In sounds-likes for Japanese custom language models that are based on next-generation models, the character-sequence `ウー` is ambiguous in some left contexts. Do not use characters (syllables) that end with the phoneme `/o/`, such as `ロ` and `ト`. In such cases, use `ウウ` or just `ウ` instead of `ウー`. For example, use `ロウウマン` or `ロウマン` instead of `ロウーマン`. For more information, see [Guidelines for Japanese](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#wordLanguages-jaJP-ng).

Adding words directly to custom models that are based on next-generation models increases the training time
:   Adding custom words directly to a custom model that is based on a next-generation model causes training of a model to take a few minutes longer than it otherwise would. If you are training a model with custom words that you added by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method, allow for some minutes of extra training time for the model. For more information, see
    -   [Add words to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addWords)
    -   [Monitoring the train model request](/docs/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language)

Maximum hours of audio resources for custom acoustic models in the Tokyo location has been increased
:   The maximum hours of audio resources that you can add to custom acoustic models in the Tokyo location is again 200 hours. Previously, the maximum was reduced to 50 hours for the Tokyo region. That reduction has been rescinded and postponed until next year. For more more information, see [Maximum hours of audio](/docs/speech-to-text?topic=speech-to-text-audioResources#audioMaximum).

## 5 December 2022
{: #speech-to-text-5december2022}
{: release-note}

New Netherlands Dutch next-generation multimedia model
:   The service now offers a next-generation multimedia model for Netherlands Dutch: `nl-NL_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Defect fix: Correct custom word recognition in transcription results for next-generation models
:   **Defect fix:** For language model customization with next-generation models, custom words are now recognized and used in all transcripts. Previously, custom words sometimes failed to be recognized and used in transcription results.

Defect fix: Correct use of `display_as` field in transcription results for next-generation models
:   **Defect fix:** For language model customization with next-generation models, the value of the `display_as` field for a custom word now appears in all transcripts. Previously, the value of the `word` field sometimes appeared in transcription results.

Defect fix: Update custom model naming documentation
:   **Defect fix:** The documentation now provides detailed rules for naming custom language models and custom acoustic models. For more information, see
    -   [Create a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)
    -   [Create a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
    -   [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}

## 20 October 2022
{: #speech-to-text-20october2022}
{: release-note}

Updates to English next-generation telephony models
:   The English next-generation telephony models have been updated for improved speech recognition:
    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`

    All of these models continue to support low latency. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Defect fix: Update Japanese next-generation multimedia model (upgrade required)
:   **Defect fix:** The Japanese next-generation multimedia model, `ja-JP_Multimedia`, was updated to address an internal inconsistency that could cause an error during speech recognition with low latency. *You need to upgrade any custom models that are based on the `ja-JP_Multimedia` model.* For more information about upgrading custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

## 7 October 2022
{: #speech-to-text-7october2022}
{: release-note}

New Swedish next-generation telephony model
:   The service now offers a next-generation telephony model for Swedish: `sv-SE_Telephony`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Updates to English next-generation telephony models
:   The English next-generation telephony models have been updated for improved speech recognition:
    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`

    All of these models continue to support low latency. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

## 21 September 2022
{: #speech-to-text-21september2022}
{: release-note}

New Activity Tracker event for GDPR deletion of user information
:   The service now returns an Activity Tracker event when you use the `DELETE /v1/user_data` method to delete all information about a user. The event is named `speech-to-text.gdpr-user-data.delete`. For more information, see [Activity Tracker events](/docs/speech-to-text?topic=speech-to-text-at_events).

Defect fix: Update some next-generation models to improve low-latency response time
:   **Defect fix:** The following next-generation models were updated to improve their response time when the `low_latency` parameter is used:
    -   `en-IN_Telephony`
    -   `hi-IN_Telephony`
    -   `it-IT_Multimedia`
    -   `nl-NL_Telephony`

    Previously, these models did not return recognition results as quickly as expected when the `low_latency` parameter was used. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

## 19 August 2022
{: #speech-to-text-19august2022}
{: release-note}

Important: Deprecation date for most previous-generation models is now 3 March 2023
:   **Superseded:** This deprecation notice is superseded by the [15 February 2023 service update](#speech-to-text-15february2023). The end of service date for *all* previous-generation models is now **31 July 2023**.

    On 15 March 2022, the previous-generation models for all languages other than Arabic and Japanese were deprecated. At that time, the deprecated models were to remain available until 15 September 2022. To allow users more time to migrate to the appropriate next-generation models, the deprecated models will now remain available until 3 March 2023. As with the initial deprecation notice, the Arabic and Japanese previous-generation models are *not* deprecated. For a complete list of all deprecated models, see the [15 March 2022 service update](#speech-to-text-15march2022).

    On 3 March 2023, the deprecated models will be removed from the service and the documentation. If you use any of the deprecated models, you must migrate to the equivalent next-generation model by the 3 March 2023.
    -   For more information about the next-generation models to which you can migrate from each of the deprecated models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
    -   For more information about the next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   For more information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).

    **Note:** When the previous-generation `en-US_BroadbandModel` is removed from service, the next-generation `en-US_Multimedia` model will become the default model for speech recognition requests.

## 15 August 2022
{: #speech-to-text-15august2022}
{: release-note}

New French Canadian next-generation multimedia model
:   The service now offers a next-generation multimedia model for French Canadian: `fr-CA_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Updates to English next-generation telephony models
:   The English next-generation telephony models have been updated for improved speech recognition:
    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`

    All of these models continue to support low latency. You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Italian next-generation multimedia model now supports low latency
:   The Italian next-generation multimedia model, `it-IT_Multimedia`, now supports low latency. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Important: Maximum hours of audio data being reduced for custom acoustic models
:   **Important:** The maximum amount of audio data that you can add to a custom acoustic model is being reduced from 200 hours to 50 hours. This change is being phased into different locations from August to September 2022. For information about the schedule for the limit reduction and what it means for existing custom acoustic models that contain more than 50 hours of audio, see [Maximum hours of audio](/docs/speech-to-text?topic=speech-to-text-audioResources#audioMaximum).

## 3 August 2022
{: #speech-to-text-3august2022}
{: release-note}

Defect fix: Update speech hesitations and hesitation markers documentation
:   **Defect fix:** Documentation for speech hesitations and hesitation markers has been updated. Previous-generation models include hesitation markers in place of speech hesitations in transcription results for most languages; smart formatting removes hesitation markers from US English final transcripts. Next-generation models include the actual speech hesitations in transcription results; smart formatting has no effect on their inclusion in final transcription results.

    For more information, see:
    -   [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation)
    -   [What results does smart formatting affect?](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-effects)

## 1 June 2022
{: #speech-to-text-1june2022}
{: release-note}

Updates to multiple next-generation telephony models
:   The following next-generation telephony models have been updated for improved speech recognition:

    -   `en-AU_Telephony`
    -   `en-GB_Telephony`
    -   `en-IN_Telephony`
    -   `en-US_Telephony`
    -   `ko-KR_Telephony`

    You do not need to upgrade custom models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

## 25 May 2022
{: #speech-to-text-25may2022}
{: release-note}

New beta `character_insertion_bias` parameter for next-generation models
:   All next-generation models now support a new beta parameter, `character_insertion_bias`, which is available with all speech recognition interfaces. By default, the service is optimized for each individual model to balance its recognition of candidate strings of different lengths. The model-specific bias is equivalent to 0.0. Each model's default bias is sufficient for most speech recognition requests.

    However, certain use cases might benefit from favoring hypotheses with shorter or longer strings of characters. The parameter accepts values between -1.0 and 1.0 that represent a change from a model's default. Negative values instruct the service to favor shorter strings of characters. Positive values direct the service to favor longer strings of characters. For more information, see [Character insertion bias](/docs/speech-to-text?topic=speech-to-text-parsing#insertion-bias).

## 19 May 2022
{: #speech-to-text-19may2022}
{: release-note}

New Italian `it-IT_Multimedia` next-generation model
:   The service now offers a next-generation multimedia model for Italian: `it-IT_Multimedia`. The new model is generally available. It does not support low latency, but it does support language model customization and grammars. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Updated Korean telephony and multimedia next-generation models
:   The existing Korean next-generation models have been updated:
    -   The `ko-KR_Telephony` model has been updated for improved low-latency support for speech recognition.
    -   The `ko-KR_Multimedia` model has been updated for improved speech recognition. The model now also supports low latency.

    Both models are generally available, and both support language model customization and grammars. You do not need to upgrade custom language models that are based on these models. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Defect fix: Confidence scores are now reported for all transcription results
:   **Defect fix:** Confidence scores are now reported for all transcription results. Previously, when the service returned multiple transcripts for a single speech recognition request, confidence scores might not be returned for all transcripts.

## 11 April 2022
{: #speech-to-text-11april2022}
{: release-note}

New Brazilian Portuguese `pt-BR_Multimedia` next-generation model
:   The service now offers a next-generation multimedia model for Brazilian Portuguese: `pt-BR_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization and grammars. For more information about next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Update to German `de-DE_Multimedia` next-generation model to support low latency
:   The next-generation German model, `de-DE_Multimedia`, now supports low latency. You do not need to upgrade custom models that are based on the updated German base model. For more information about the next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

Support for sounds-like is now documented for custom models based on next-generation models
:   For custom language models that are based on next-generation models, support is now documented for sounds-like specifications for custom words. Support for sounds-likes has been available since late 2021.

    Differences exist between the use of the `sounds_like` field for custom models that are based on next-generation and previous-generation models. For more information about using the `sounds_like` field with custom models that are based on next-generation models, see [Working with custom words for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#workingWords-ng).

Important: Deprecated `customization_id` parameter removed from the documentation
:   **Important:** On [9 October 2018](#speech-to-text-9october2018), the `customization_id` parameter of all speech recognition requests was deprecated and replaced by the `language_customization_id` parameter. The `customization_id` parameter has now been removed from the documentation for the speech recognition methods:
    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for synchronous HTTP requests (including multipart requests)
    -   `POST /v1/recognitions` for asynchronous HTTP requests

    **Note:** If you use the {{site.data.keyword.watson}} SDKs, make sure that you have updated any application code to use the `language_customization_id` parameter instead of the `customization_id` parameter. The `customization_id` parameter will no longer be available from the equivalent methods of the SDKs as of their next major release. For more information about the speech recognition methods, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text#service-endpoint){: external}.

## 17 March 2022
{: #speech-to-text-17march2022}
{: release-note}

Grammar support for next-generation models is now generally available
:   Grammar support is now generally available (GA) for next-general models that meet the following conditions:
    -   The models are generally available.
    -   The models support language model customization.

    For more information, see the following topics:
    -   For more information about the status of grammar support for next-generation models, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).
    -   For more information about grammars, see [Grammars](/docs/speech-to-text?topic=speech-to-text-customization#grammars-intro).

New German next-generation multimedia model
:   The service now offers a next-generation multimedia model for German: `de-DE_Multimedia`. The new model is generally available. It does not support low latency. It does support language model customization (generally available) and grammars (beta).

    For more information about all available next-generation models and their customization support, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)

Beta next-generation `en-WW_Medical_Telephony` model now supports low latency
:   The beta next-generation `en-WW_Medical_Telephony` model now supports low latency. For more information about all next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

## 15 March 2022
{: #speech-to-text-15march2022}
{: release-note}

Important: Deprecation of most previous-generation models
:   **Superseded:** This deprecation notice is superseded by the [15 February 2023 service update](#speech-to-text-15february2023). The end of service date for *all* previous-generation models is now **31 July 2023**.

    Effective 15 March 2022, previous-generation models for all languages other than Arabic and Japanese are deprecated. The deprecated models remain available until 15 September 2022, when they will be removed from the service and the documentation. The Arabic and Japanese previous-generation models are *not* deprecated.

    The following previous-generation models are now deprecated:
    -   Chinese (Mandarin): `zh-CN_NarrowbandModel` and `zh-CN_BroadbandModel`
    -   Dutch (Netherlands): `nl-NL_NarrowbandModel` and `nl-NL_BroadbandModel`
    -   English (Australian): `en-AU_NarrowbandModel` and `en-AU_BroadbandModel`
    -   English (United Kingdom): `en-GB_NarrowbandModel` and `en-GB_BroadbandModel`
    -   English (United States): `en-US_NarrowbandModel`, `en-US_BroadbandModel`, and `en-US_ShortForm_NarrowbandModel`
    -   French (Canadian): `fr-CA_NarrowbandModel` and `fr-CA_BroadbandModel`
    -   French (France): `fr-FR_NarrowbandModel` and `fr-FR_BroadbandModel`
    -   German: `de-DE_NarrowbandModel` and `de-DE_BroadbandModel`
    -   Italian: `it-IT_NarrowbandModel` and `it_IT_BroadbandModel`
    -   Korean: `ko-KR_NarrowbandModel` and `ko-KR_BroadbandModel`
    -   Portuguese (Brazilian): `pt-BR_NarrowbandModel` and `pt-BR_BroadbandModel`
    -   Spanish (Argentinian): `es-AR_NarrowbandModel` and `es-AR_BroadbandModel`
    -   Spanish (Castilian): `es-ES_NarrowbandModel` and `es-ES_BroadbandModel`
    -   Spanish (Chilean): `es-CL_NarrowbandModel` and `es-CL_BroadbandModel`
    -   Spanish (Colombian): `es-CO_NarrowbandModel` and `es-CO_BroadbandModel`
    -   Spanish (Mexican): `es-MX_NarrowbandModel` and `es-MX_BroadbandModel`
    -   Spanish (Peruvian): `es-PE_NarrowbandModel` and `es-PE_BroadbandModel`

    If you use any of these deprecated models, you must migrate to the equivalent next-generation model by the end of service date.
    -   For more information about the next-generation models to which you can migrate from each of the deprecated models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
    -   For more information about the next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   For more information about migrating from previous-generation to next-generation models, see [Migrating to next-generation models](/docs/speech-to-text?topic=speech-to-text-models-migrate).

    **Note:** When the previous-generation `en-US_BroadbandModel` is removed from service on 15 September, the next-generation `en-US_Multimedia` model will become the default model for speech recognition requests.

Next-generation models now support audio-parsing parameters
:   All next-generation models now support the following audio-parsing parameters as generally available features:

    -   `end_of_phrase_silence_time` specifies the duration of the pause interval at which the service splits a transcript into multiple final results. For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).
    -   `split_transcript_at_phrase_end` directs the service to split the transcript into multiple final results based on semantic features of the input. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

Defect fix: Correct speaker labels documentation
:   **Defect fix:** Documentation of speaker labels included the following erroneous statement in multiple places: *For next-generation models, speaker labels are not supported for use with interim results or low latency.* Speaker labels are supported for use with interim results and low latency for next-generation models. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

## 28 February 2022
{: #speech-to-text-28february2022}
{: release-note}

Updates to English and French next-generation multimedia models to support low latency
:   The following multimedia models have been updated to support low latency:
    -   Australian English: `en-AU_Multimedia`
    -   UK English: `en-GB_Multimedia`
    -   US English: `en-US_Multimedia`
    -   French: `fr-FR_Multimedia`

    You do not need to upgrade custom language models that are built on these base models. For more information about the next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

New Castilian Spanish next-generation multimedia model
:   The service now offers a next-generation multimedia model for Castilian Spanish: `es-ES_Multimedia`. The new model supports low latency and is generally available. It also supports language model customization (generally available) and grammars (beta).

    For more information about all available next-generation models and their customization support, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng)

## 11 February 2022
{: #speech-to-text-11february2022}
{: release-note}

Defect fix: Correct custom model upgrade and base model version documentation
:   **Defect fix:** The documentation that describes the upgrade of custom models and the version strings that are used for different versions of base models has been updated. The documentation now states that upgrade for language model customization also applies to next-generation models. Also, the version strings that represent different versions of base models have been updated. And the `base_model_version` parameter can also be used with upgraded next-generation models.

    For more information about custom model upgrade, when upgrade is necessary, and how to use older versions of custom models, see
    -   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
    -   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)

Defect fix: Update capitalization documentation
:   **Defect fix:** The documentation that describes the service's automatic capitalization of transcripts has been updated. The service capitalizes appropriate nouns only for the following languages and models:
    -   All previous-generation US English models
    -   The next-generation German model

    For more information, see [Capitalization](/docs/speech-to-text?topic=speech-to-text-basic-response#response-capitalization).

## 2 February 2022
{: #speech-to-text-2february2022}
{: release-note}

New beta `en-WW_Medical_Telephony` model is now available
:   A new beta next-generation `en-WW_Medical_Telephony` is now available. The new model understands terms from the medical and pharmacological domains. Use the model in situations where you need to transcribe common medical terminology such as medicine names, product brands, medical procedures, illnesses, types of doctor, or COVID-19-related terminology. Common use cases include conversations between a patient and a medical provider (for example, a doctor, nurse, or pharmacist).

    The new model is available for all supported English dialects: Australian, Indian, UK, and US. The new model supports language model customization and grammars as beta functionality. It supports most of the same parameters as the `en-US_Telephony` model, including `smart_formatting` for US English audio. It does not support the following parameters: `low_latency`, `profanity_filter`, `redaction`, and `speaker_labels`.

    For more information, see [The English medical telephony model](/docs/speech-to-text?topic=speech-to-text-models-ng#models-medical).

Update to Chinese `zh-CN_Telephony` model
:   The next-generation Chinese model `zh-CN_Telephony` has been updated for improved speech recognition. The model continues to support low latency. By default, the service automatically uses the updated model for all speech recognition requests. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

    If you have custom language models that are based on the updated model, you must upgrade your existing custom models to take advantage of the updates by using the `POST /v1/customizations/{customization_id}/upgrade_model` method. For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

Update to Japanese `ja-JP_Multimedia` next-generation model to support low latency
:   The next-generation Japanese model `ja-JP_Multimedia` now supports low latency. You can use the `low_latency` parameter with speech recognition requests that use the model. You do not need to upgrade custom models that are based on the updated Japanese base model. For more information about the next-generation models and low latency, see
    -   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
    -   [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency)

## 3 December 2021
{: #speech-to-text-3december2021}
{: release-note}

New Latin American Spanish next-generation telephony model
:   The service now offers a next-generation telephony model for Latin American Spanish: `es-LA_Telephony`. The new model supports low latency and is generally available.

    The `es-LA_Telephony` model applies to all Latin American dialects. It is the equivalent of the previous-generation models that are available for the Argentinian, Chilean, Colombian, Mexican, and Peruvian dialects. If you used a previous-generation model for any of these specific dialects, use the `es-LA_Telephony` model to migrate to the equivalent next-generation model.

    For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Important: Custom language models based on certain next-generation models must be re-created
:   **Important:** If you created custom language models based on certain next-generation models, you must re-create the custom models. Until you re-create the custom language models, speech recognition requests that attempt to use the custom models fail with HTTP error code 400.

    You need to re-create custom language models that you created based on the following versions of next-generation models:
    -   For the `en-AU_Telephony` model, custom models that you created from `en-AU_Telephony.v2021-03-03` to `en-AU_Telephony.v2021-10-04`.
    -   For the `en-GB_Telephony` model, custom models that you created from `en-GB_Telephony.v2021-03-03` to `en-GB_Telephony.v2021-10-04`.
    -   For the `en-US_Telephony` model, custom models that you created from `en-US_Telephony.v2021-06-17` to `en-US_Telephony.v2021-10-04`.
    -   For the `en-US_Multimedia` model, custom models that you created from `en-US_Multimedia.v2021-03-03` to `en-US_Multimedia.v2021-10-04`.

    **To identify the version of a model on which a custom language model is based,** use the `GET /v1/customizations` method to list all of your custom language models or the `GET /v1/customizations/{customization_id}` method to list a specific custom language model. The `versions` field of the output shows the base model for a custom language model. For more information, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).

    **To re-create a custom language model,** first create a new custom model. Then add all of the previous custom model's corpora and custom words to the new model. You can then delete the previous custom model. For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate).

## 28 October 2021
{: #speech-to-text-28october2021}
{: release-note}

New Chinese next-generation telephony model
:   The service now offers a next-generation telephony model for Mandarin Chinese: `zh-CN_Telephony`. The new model supports low latency and is generally available. For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

New Australian English and UK English next-generation multimedia models
:   The service now offers the following next-generation multimedia models. The new models are generally available, and neither model supports low latency.
    -   Australian English: `en-AU_Multimedia`
    -   UK English: `en-GB_Multimedia`

    For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Updates to multiple next-generation models for improved speech recognition
:   The following next-generation models have been updated for improved speech recognition:
    -   Australian English telephony model (`en-AU_Telephony`)
    -   UK English telephony model (`en-GB_Telephony`)
    -   US English multimedia model (`en-US_Multimedia`)
    -   US English telephony model (`en-US_Telephony`)
    -   Castilian Spanish telephony model (`es-ES_Telephony`)

    For more information about all available next-generation models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Grammar support for previous-generation models is now generally available
:   Grammar support is now generally available (GA) for previous-general models that meet the following conditions:
    -   The models are generally available.
    -   The models support language model customization.

    For more information, see the following topics:
    -   For more information about the status of grammar support for previous-generation models, see [Customization support for previous-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-pg).
    -   For more information about grammars, see [Grammars](/docs/speech-to-text?topic=speech-to-text-customization#grammars-intro).

New beta grammar support for next-generation models
:   Grammar support is now available as beta functionality for all next-generation models. All next-generation models are generally available (GA) and support language model customization. For more information, see the following topics:
    -   For more information about the status of grammar support for next-generation models, see [Customization support for next-generation models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-ng).
    -   For more information about grammars, see [Grammars](/docs/speech-to-text?topic=speech-to-text-customization#grammars-intro).

    **Note:** Beta support for grammars by next-generation models is available for the {{site.data.keyword.speechtotextshort}} service on {{site.data.keyword.cloud_notm}} only. Grammars are not yet supported for next-generation models on {{site.data.keyword.icp4dfull_notm}}.

New `custom_acoustic_model` field for supported features
:   The `GET /v1/models` and `GET /v1/models/{model_id}` methods now report whether a model supports acoustic model customization. The `SupportedFeatures` object now includes an additional field, `custom_acoustic_model`, a boolean that is `true` for a model that supports acoustic model customization and `false` otherwise. Currently, the field is `true` for all previous-generation models and `false` for all next-generation models.
    -   For more information about these methods, see [Listing information about models](/docs/speech-to-text?topic=speech-to-text-models-list).
    -   For more information about support for acoustic model customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

## 22 October 2021
{: #speech-to-text-22october2021}
{: release-note}

Defect fix: Address asynchronous HTTP failures
:   **Defect fix:** The asynchronous HTTP interface failed to transcribe some audio. In addition, the callback for the request returned status `recognitions.completed_with_results` instead of `recognitions.failed`. This error has been resolved.

## 6 October 2021
{: #speech-to-text-6october2021}
{: release-note}

Updates to Czech and Dutch next-generation models
:   The following next-generation language models have changed as indicated:
    -   The Czech telephony model, `cs-CZ_Telephony`, is now generally available (GA). The model continues to support low latency.
    -   The Belgian Dutch telephony model, `nl-BE_Telephony`, has been updated for improved speech recognition. The model continues to support low latency.
    -   The Netherlands Dutch telephony model, `nl-NL_Telephony`, is now GA. In addition, the model now supports low latency.

    For more information about all available next-generation language models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

New US HIPAA support for Premium plans in Dallas location
:   US Health Insurance Portability and Accountability Act (HIPAA) support is now available for Premium plans that are hosted in the Dallas (`us-south`) location.  For more information, see [Health Insurance Portability and Accountability Act (HIPAA)](/docs/speech-to-text?topic=speech-to-text-information-security#hipaa).

## 16 September 2021
{: #speech-to-text-16september2021}
{: release-note}

New beta Czech and Netherlands Dutch next-generation models
:   The service now supports the following new next-generation language models. Both new models are beta functionality.
    -   Czech: `cs-CZ_Telephony`. The new model supports low latency.
    -   Netherlands Dutch: `nl-NL_Telephony`. The new model does not support low latency.

    For more information about all available next-generation language models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Updates to Korean and Brazilian Portuguese next-generation models
:   The following next-generation models have been updated:
    -   The Korean model `ko-KR_Telephony` now supports low latency.
    -   The Brazilian Portuguese model `pt-BR_Telephony` has been updated for improved speech recognition.

Defect fix: Correct interim results and low-latency documentation
:   **Defect fix:** Documentation that describes the interim results and low-latency features with next-generation models has been rewritten for clarity and correctness. For more information, see the following topics:
    -   [Interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim), especially [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency)
    -   [How the service sends recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#ws-results)

Defect fix: Improve speakers labels results
:   **Defect fix:** When you use speakers labels with next-generation models, the service now identifies the speaker for all words of the input audio, including very short words that have the same start and end timestamps.

## 31 August 2021
{: #speech-to-text-31august2021}
{: release-note}

All next-generation models are now generally available
:   All existing next-generation language models are now generally available (GA). They are supported for use in production environments and applications.
    -   For more information about all available next-generation language models, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
    -   For more information about the features that are supported for each next-generation model, see [Supported features for next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features).

Language model customization for next-generation models is now generally available
:   Language model customization is now generally available (GA) for all available next-generation languages and models. Language model customization for next-generation models is supported for use in production environments and applications.

    You use the same commands to create, manage, and use custom language models, corpora, and custom words for next-generation models as you do for previous-generation models. But customization for next-generation models works differently from customization for previous-generation models. For custom models that are based on next-generation models:
    -   The custom models have no concept of out-of-vocabulary (OOV) words.
    -   Words from corpora are not added to the words resource.
    -   You cannot currently use the sounds-like feature for custom words.
    -   You do not need to upgrade custom models when base language models are updated.
    -   Grammars are not currently supported.

    For more information about using language model customization for next-generation models, see
    -   [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)
    -   [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate)
    -   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)
    -   [Working with corpora and custom words for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng)

    Additional topics describe managing custom language models, corpora, and custom words. These operations are the same for custom models based on previous- and next-generation models.

## 16 August 2021
{: #speech-to-text-16august2021}
{: release-note}

New beta Indian English, Indian Hindi, Japanese, and Korean next-generation models
:   The service now supports the following new next-generation language models. All of the new models are beta functionality.

    -   Indian English: `en-IN_Telephony`. The model supports low latency.
    -   Indian Hindi: `hi-IN_Telephony`. The model supports low latency.
    -   Japanese: `ja-JP_Multimedia`. The model does not support low latency.
    -   Korean: `ko-KR_Multimedia` and `ko-KR_Telephony`. The models do not support low latency.

    For more information about the next-generation models and low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

## 16 July 2021
{: #speech-to-text-16july2021}
{: release-note}

New beta French next-generation model
:   The French next-generation language model `fr-FR_Multimedia` is now available. The new model does not support low latency. The model is beta functionality.

Updates to beta US English next-generation model for improved speech recognition
:   The next-generation US English `en-US_Telephony` model has been updated for improved speech recognition. The updated model continues to be beta functionality.

Defect fix: Update documentation for hesitation markers
:   **Defect fix:** The documentation failed to state that next-generation models do not produce hesitation markers. The documentation has been updated to note that only previous-generation models produce hesitation markers. Next-generation models include the actual hesitations in transcription results. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

## 15 June 2021
{: #speech-to-text-15june2021}
{: release-note}

New beta Belgian Dutch next-generation model
:   The Belgian Dutch (Flemish) next-generation language model `nl-BE_Telephony` is now available. The new model supports low latency. The model is beta functionality. For more information about the next-generation models and about low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

New beta low-latency support for Arabic, Canadian French, and Italian next-generation models
:   The following existing beta next-generation language models now support low latency:
    -   Arabic `ar-MS_Telephony` model
    -   Canadian French `fr-CA_Telephony` model
    -   Italian `it-IT_Telephony` model

    For more information about the next-generation models and about low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

Updates to beta Arabic and Brazilian Portuguese next-generation models for improved speech recognition
:   The following existing beta next-generation language models have been updated for improved speech recognition:
    -   Arabic `ar-MS_Telephony` model
    -   Brazilian Portuguese `pt-BR_Telephony` model

    For more information about the next-generation models and about low latency, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng) and [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

## 26 May 2021
{: #speech-to-text-26may2021}
{: release-note}

New beta support for `audio_metrics` parameter for next-generation models
:   The `audio_metrics` parameter is now supported as beta functionality for use with all next-generation languages and models. For more information, see [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio-metrics).

New beta support for `word_confidence` parameter for next-generation models
:   The `word_confidence` parameter is now supported as beta functionality for use with all next-generation languages and models. For more information, see [Word confidence](/docs/speech-to-text?topic=speech-to-text-metadata#word-confidence).

Defect fix: Update documentation for next-generation models
:   **Defect fix:** The documentation has been updated to correct the following information:
    -   When you use a next-generation model for speech recognition, final transcription results now include the `confidence` field. The field was always included in final transcription results when you use a previous-generation model. This fix addresses a limitation that was reported for the 12 April 2021 release of the next-generation models.
    -   The documentation incorrectly stated that using the `smart_formatting` parameter causes the service to remove hesitation markers from final transcription results for Japanese. Smart formatting does not remove hesitation markers from final results for Japanese, only for US English. For more information, see [What results does smart formatting affect?](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-effects)

## 27 April 2021
{: #speech-to-text-27april2021}
{: release-note}

New beta Arabic and Brazilian Portuguese next-generation models
:   The service supports two new beta next-generation models:
    -   The Brazilian Portuguese `pt-BR_Telephony` model, which supports low latency.
    -   The Arabic (Modern Standard ) `ar-MS_Telephony` model, which does not support low latency.

    For more information, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).

Updates to beta Castilian Spanish next-generation model for improved speech recognition
:   The beta next-generation Castilian Spanish `es-ES_Telephony` model now supports the `low_latency` parameter. For more information, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).

New beta support for speaker labels with next-generation models
:   The `speaker_labels` parameter is now supported as beta functionality for use with the following next-generation models:
    -   Australian English `en-AU_Telephony` model
    -   UK English `en-GB_Telephony` model
    -   US English `en-US_Multimedia` and `en-US_Telephony` models
    -   German `de-DE_Telephony` model
    -   Castilian Spanish `es-ES_Telephony` model

    With the next generation models, the `speaker_labels` parameter is not supported for use with the `interim_results` or `low_latency` parameters at this time. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

New HTTP error code for use of `word_confidence` with next-generation models
:   The `word_confidence` parameter is not supported for use with next-generation models. The service now returns the following 400 error code if you use the `word_confidence` parameter with a next-generation model for speech recognition:

    ```json
    {
      "error": "word_confidence is not a supported feature for model {model}",
      "code": 400,
      "code_description": "Bad Request"
    }
    ```
    {: codeblock}

## 12 April 2021
{: #speech-to-text-12april2021}
{: release-note}

New beta next-generation language models and `low_latency` parameter
:   The service now supports a growing number of next-generation language models. The next-generation *multimedia* and *telephony* models improve upon the speech recognition capabilities of the service's previous generation of broadband and narrowband models. The new models leverage deep neural networks and bidirectional analysis to achieve both higher throughput and greater transcription accuracy. At this time, the next-generation models support only a limited number of languages and speech recognition features. The supported languages, models, and features will increase with future releases. The next-generation models are beta functionality.

    Many of the next-generation models also support a new `low_latency` parameter that lets you request faster results at the possible expense of reduced transcription quality. When low latency is enabled, the service curtails its analysis of the audio, which can reduce the accuracy of the transcription. This trade-off might be acceptable if your application requires lower response time more than it does the highest possible accuracy.The `low_latency` parameter is beta functionality.

    The `low_latency` parameter impacts your use of the `interim_results` parameter with the WebSocket interface. Interim results are available only for those next-generation models that support low latency, and only if both the `interim_results` and `low_latency` parameters are set to `true`.

    -   For more information about the next-generation models and their capabilities, see [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng).
    -   For more information about language support for next-generation models and about which next-generation models support low latency, see [Supported next-generation language models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-supported).
    -   For more information about feature support for next-generation models, see [Supported features for next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features).
    -   For more information about the `low_latency` parameter, see [Low latency](/docs/speech-to-text?topic=speech-to-text-interim#low-latency).
    -   For more information about the interaction between the `low_latency` and `interim_results` parameters for next-generation models, see [Requesting interim results and low latency](/docs/speech-to-text?topic=speech-to-text-interim#interim-low-latency).

## 17 March 2021
{: #speech-to-text-17march2021}
{: release-note}

Defect fix: Fix limitation for asynchronous HTTP interface
:   **Defect fix:** The limitation that was reported with the asynchronous HTTP interface in the Dallas (`us-south`) location on 16 December 2020 has been addressed. Previously, a small percentage of jobs were entering infinite loops that prevented their execution. Asynchronous HTTP requests in the Dallas data center no longer experience this limitation.

## 2 December 2020
{: #speech-to-text-2december2020}
{: release-note}

Arabic model renamed to  `ar-MS_BroadbandModel`
:   The Arabic language broadband model is now named `ar-MS_BroadbandModel`. The former name, `ar-AR_BroadbandModel`, is deprecated. It will continue to function for at least one year but might be removed at a future date. You are encouraged to migrate to the new name at your earliest convenience.

## 2 November 2020
{: #speech-to-text-2november2020}
{: release-note}

Canadian French models now generally available
:   The Canadian French models, `fr-CA_BroadbandModel` and `fr-CA_NarrowbandModel`, are now generally available (GA). They were previously beta. They also now support language model and acoustic model customization.

    -   For more information about supported languages and models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).
    -   For more information about language support for customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

## 22 October 2020
{: #speech-to-text-22october2020}
{: release-note}

Australian English models now generally available
:   The Australian English models, `en-AU_BroadbandModel` and `en-AU_NarrowbandModel`, are now generally available (GA). They were previously beta. They also now support language model and acoustic model customization.
    -   For more information about supported languages and models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).
    -   For more information about language support for customization, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

Updates to Brazilian Portuguese models for improved speech recognition
:   The Brazilian Portuguese models, `pt-BR_BroadbandModel` and `pt-BR_NarrowbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

The `split_transcript_at_phrase_end` parameter now generally available for all languages
:   The speech recognition parameter `split_transcript_at_phrase_end` is now generally available (GA) for all languages. Previously, it was generally available only for US and UK English. For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

## 7 October 2020
{: #speech-to-text-7october2020}
{: release-note}

Updates to Japanese broadband model for improved speech recognition
:   The `ja-JP_BroadbandModel` model has been updated for improved speech recognition. By default, the service automatically uses the updated model for all speech recognition requests. If you have custom language or custom acoustic models that are based on this model, you must upgrade your existing custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

## 30 September 2020
{: #speech-to-text-30september2020}
{: release-note}

Updates to pricing plans for the service
:   The pricing plans for the service have changed:

    -   The service continues to offer a Lite plan that provides basic no-charge access to limited minutes of speech recognition per month.
    -   The service offers a new Plus plan that provides a simple tiered pricing model and access to the service's customization capabilities.
    -   The service offers a new Premium plan that provides significantly greater capacity and enhanced features.

    The Plus plan replaces the Standard plan. The Standard plan continues to be available for purchase for a short time. It also continues to be available indefinitely to existing users of the plan with no change in their pricing. Existing users can upgrade to the Plus plan at any time.

    For more information about the available pricing plans, see the following resources:

    -   For general information about the pricing plans and answers to common questions, see the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).
    -   For more information about the pricing plans or to purchase a plan, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/services/speech-to-text){: external}.

## 20 August 2020
{: #speech-to-text-20august2020}
{: release-note}

New Canadian French models
:   The service now offers beta broadband and narrowband models for Canadian French:

    -   `fr-CA_BroadbandModel`
    -   `fr-CA_NarrowbandModel`

    The new models do not support language model or acoustic model customization, speaker labels, or smart formatting. For more information about these and all supported models, see [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported).

## 5 August 2020
{: #speech-to-text-5august2020}
{: release-note}

New Australian English models
:   The service now offers beta broadband and narrowband models for Australian English:
    -   `en-AU_BroadbandModel`
    -   `en-AU_NarrowbandModel`

    The new models do not support language model or acoustic model customization, or smart formatting. The new models do support speakers labels. For more information, see
    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported)
    -   [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels)

Updates to multiple models for improved speech recognition
:   The following models have been updated for improved speech recognition:
    -   French broadband model (`fr-FR_BroadbandModel`)
    -   German broadband (`de-DE_BroadbandModel`) and narrowband (`de-DE_NarrowbandModel`) models
    -   UK English broadband (`en-GB_BroadbandModel`) and narrowband (`en-GB_NarrowbandModel`) models
    -   US English short-form narrowband (`en-US_ShortForm_NarrowbandModel`) model

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

Hesitation marker for German changed
:   The hesitation marker that is used for the updated German broadband and narrowband models has changed from `[hesitation]` to `%HESITATION`. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

## 4 June 2020
{: #speech-to-text-4june2020}
{: release-note}

Defect fix: Improve latency for custom language models with many grammars
:   **Defect fix:** The latency issue for custom language models that contain a large number of grammars has been resolved. When initially used for speech recognition, such custom models could take multiple seconds to load. The custom models now load much faster, greatly reducing latency when they are used for recognition.

## 28 April 2020
{: #speech-to-text-28april2020}
{: release-note}

Updates to Italian models for improved speech recognition
:   The Italian broadband (`it-IT_BroadbandModel`) and narrowband (`it-IT_NarrowbandModel`) models have been updated for improved speech recognition. By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

Dutch and Italian models now generally available
:   The Dutch and Italian language models are now generally available (GA) for speech recognition and for language model and acoustic model customization:
    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)

    For more information about all available language models, see
    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)

## 1 April 2020
{: #speech-to-text-1april2020}
{: release-note}

Acoustic model customization now generally available
:   Acoustic model customization is now generally available (GA) for all supported languages. As with custom language models, {{site.data.keyword.IBM_notm}} does not charge for creating or hosting a custom acoustic model. You are charged only for using a custom model with a speech recognition request.

    Using a custom language model, a custom acoustic model, or both types of model for transcription incurs an add-on charge of $0.03 (USD) per minute. This charge is in addition to the standard usage charge of $0.02 (USD) per minute, and it applies to all languages supported by the customization interface. So the total charge for using one or more custom models for speech recognition is $0.05 (USD) per minute.

    -   For more information about support for individual language models, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).
    -   For more information about pricing, see the [pricing page](https://www.ibm.com/products/speech-to-text){: external} for the {{site.data.keyword.speechtotextshort}} service or the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).

## 16 March 2020
{: #speech-to-text-16march2020}
{: release-note}

Speaker labels now supported for German and Korean
:   The service now supports speaker labels (the `speaker_labels` parameter) for German and Korean language models. Speaker labels identify which individuals spoke which words in a multi-participant exchange. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

Activity Tracker now supported for asynchronous HTTP interface
:   The service now supports the use of Activity Tracker events for all operations of the asynchronous HTTP interface. {{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud}}. For more information, see [Activity Tracker events](/docs/speech-to-text?topic=speech-to-text-at_events).

## 24 February 2020
{: #speech-to-text-25february2020}
{: release-note}

Updates to multiple models for improved speech recognition
:   The following models have been updated for improved speech recognition:
    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)
    -   Japanese narrowband model (`ja-JP_NarrowbandModel`)
    -   US English broadband model (`en-US_BroadbandModel`)

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

Language model customization now available for Dutch and Italian
:   Language model customization is now supported for Dutch and Italian with the new versions of the following models:

    -   Dutch broadband model (`nl-NL_BroadbandModel`)
    -   Dutch narrowband model (`nl-NL_NarrowbandModel`)
    -   Italian broadband model (`it-IT_BroadbandModel`)
    -   Italian narrowband model (`it-IT_NarrowbandModel`)

    For more information, see
    -   [Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)
    -   [Guidelines for Dutch, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)

    Because the Dutch and Italian models are beta, their support for language model customization is also beta.

Japanese narrowband model now includes some multigram word units
:   The Japanese narrowband model (`ja-JP_NarrowbandModel`) now includes some multigram word units for digits and decimal fractions. The service returns these multigram units regardless of whether you enable smart formatting. The smart formatting feature understands and returns the multigram units that the model generates. If you apply your own post-processing to transcription results, you need to handle these units appropriately. For more information, see [Japanese](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting-japanese) in the smart formatting documentation.

New speech activity detection and background audio suppression parameters for speech recognition
:   The service now offers two new optional parameters for controlling the level of speech activity detection. The parameters can help ensure that only relevant audio is processed for speech recognition.
    -   The `speech_detector_sensitivity` parameter adjusts the sensitivity of speech activity detection. You can use the parameter to suppress word insertions from music, coughing, and other non-speech events.
    -   The `background_audio_suppression` parameter suppresses background audio based on its volume to prevent it from being transcribed or otherwise interfering with speech recognition. You can use the parameter to suppress side conversations or background noise.

    You can use the parameters individually or together. They are available for all interfaces and for most language models. For more information about the parameters, their allowable values, and their effect on the quality and latency of speech recognition, see [Speech activity detection](/docs/speech-to-text?topic=speech-to-text-detection).

Activity Tracker now supported for customization interfaces
:   The service now supports the use of Activity Tracker events for all customization operations. {{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. For more information, see [Activity Tracker events](/docs/speech-to-text?topic=speech-to-text-at_events).

Defect fix: Correct generation of processing metrics with WebSocket interface
:   **Defect fix:** The WebSocket interface now works seamlessly when generating processing metrics. Previously, processing metrics could continue to be delivered after the client sent a `stop` message to the service.

## 18 December 2019
{: #speech-to-text-18december2019}
{: release-note}

New beta Italian models available
:   The service now offers beta broadband and narrowband models for the Italian language:

    -   `it-IT_BroadbandModel`
    -   `it-IT_NarrowbandModel`

    These language models support acoustic model customization. They do not support language model customization. Because they are beta, these models might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

    For more information, see the following sections:

    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)

New `end_of_phrase_silence_time` parameter for speech recognition
:   For speech recognition, the service now supports the `end_of_phrase_silence_time` parameter. The parameter specifies the duration of the pause interval at which the service splits a transcript into multiple final results. Each final result indicates a pause or extended silence that exceeds the pause interval. For most languages, the default pause interval is 0.8 seconds; for Chinese the default interval is 0.6 seconds.

    You can use the parameter to effect a trade-off between how often a final result is produced and the accuracy of the transcription. Increase the interval when accuracy is more important than latency. Decrease the interval when the speaker is expected to say short phrases or single words.

    For more information, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).

New `split_transcript_at_phrase_end` parameter for speech recognition
:   For speech recognition, the service now supports the `split_transcript_at_phrase_end` parameter. The parameter directs the service to split the transcript into multiple final results based on semantic features of the input, such as at the conclusion of sentences. The service bases its understanding of semantic features on the base language model that you use with a request. Custom language models and grammars can also influence how and where the service splits a transcript.

    The parameter causes the service to add an `end_of_utterance` field to each final result to indicate the motivation for the split: `full_stop`, `silence`, `end_of_data`, or `reset`.

    For more information, see [Split transcript at phrase end](/docs/speech-to-text?topic=speech-to-text-parsing#split-transcript).

## 12 December 2019
{: #speech-to-text-12december2019}
{: release-note}

Full support for IBM Cloud IAM
:   The {{site.data.keyword.speechtotextshort}} service now supports the full implementation of {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). API keys for {{site.data.keyword.ibmwatson}} services are no longer limited to a single service instance. You can create access policies and API keys that apply to more than one service, and you can grant access between services. For more information about IAM, see [Authenticating to {{site.data.keyword.watson}} services](/docs/watson?topic=watson-iam).

    To support this change, the API service endpoints use a different domain and include the service instance ID. The pattern is `api.{location}.speech-to-text.watson.cloud.ibm.com/instances/{instance_id}`.

    -   Example HTTP URL for an instance hosted in the Dallas location:

        `https://api.us-south.speech-to-text.watson.cloud.ibm.com/instances/6bbda3b3-d572-45e1-8c54-22d6ed9e52c2`

    -   Example WebSocket URL for an instance hosted in the Dallas location:

        `wss://api.us-south.speech-to-text.watson.cloud.ibm.com/instances/6bbda3b3-d572-45e1-8c54-22d6ed9e52c2`

    For more information about the URLs, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text#service-endpoint){: external}.

    These URLs do not constitute a breaking change. The new URLs work for both your existing service instances and for new instances. The original URLs continue to work on your existing service instances for at least one year, until December 2020.

New network and data security features available
:   Support for the following new network and data security feature is now available:
    -   *Support for private network endpoints*

        Users of Premium plans can create private network endpoints to connect to the {{site.data.keyword.speechtotextshort}} service over a private network. Connections to private network endpoints do not require public internet access. For more information, see [Public and private network endpoints](/docs/speech-to-text?topic=speech-to-text-public-private-endpoints).

## 10 December 2019
{: #speech-to-text-10december2019}
{: release-note}

New beta Netherlands Dutch models available
:   The service now offers beta broadband and narrowband models for the Netherlands Dutch language:

    -   `nl-NL_BroadbandModel`
    -   `nl-NL_NarrowbandModel`

    These language models support acoustic model customization. They do not support language model customization. Because they are beta, these models might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

    For more information, see the following sections:

    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)

## 25 November 2019
{: #speech-to-text-25november2019}
{: release-note}

Updates to speaker labels for improved identification of individual speakers
:   Speaker labels are updated to improve the identification of individual speakers for further analysis of your audio sample. For more information about the speaker labels feature, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels). For more information about the improvements to the feature, see [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://research.ibm.com/blog){: external}.

## 12 November 2019
{: #speech-to-text-12november2019}
{: release-note}

New Seoul location now available
:   The {{site.data.keyword.speechtotextshort}} service is now available in the {{site.data.keyword.cloud_notm}} Seoul location (**kr-seo**). As with other locations, the {{site.data.keyword.cloud_notm}} location uses token-based IAM authentication. All new services instances that you create in this location use IAM authentication.

## 1 November 2019
{: #speech-to-text-1november2019}
{: release-note}

New limits on maximum number of custom models
:   You can create no more than 1024 custom language models and no more than 1024 custom acoustic models per owning credentials. For more information, see [Maximum number of custom models](/docs/speech-to-text?topic=speech-to-text-custom-usage#custom-maximum).

## 1 October 2019
{: #speech-to-text-1october2019}
{: release-note}

New US HIPAA support for Premium plans in Washington, DC, location
:   US HIPAA support is available for Premium plans that are hosted in the Washington, DC (**us-east**), location and are created on or after 1 April 2019. For more information, see [US Health Insurance Portability and Accountability Act (HIPAA)](/docs/speech-to-text?topic=speech-to-text-information-security#hipaa).

## 22 August 2019
{: #speech-to-text-22august2019}
{: release-note}

Defect fix: Multiple small improvements
:   The service was updated for small defect fixes and improvements.

## 30 July 2019
{: #speech-to-text-30july2019}
{: release-note}

New models for Spanish dialects now available
:   The service now offers broadband and narrowband language models in six Spanish dialects:

    -   Argentinian Spanish (`es-AR_BroadbandModel` and `es-AR_NarrowbandModel`)
    -   Castilian Spanish (`es-ES_BroadbandModel` and `es-ES_NarrowbandModel`)
    -   Chilean Spanish (`es-CL_BroadbandModel` and `es-CL_NarrowbandModel`)
    -   Colombian Spanish (`es-CO_BroadbandModel` and `es-CO_NarrowbandModel`)
    -   Mexican Spanish (`es-MX_BroadbandModel` and `es-MX_NarrowbandModel`)
    -   Peruvian Spanish (`es-PE_BroadbandModel` and `es-PE_NarrowbandModel`)

    The Castilian Spanish models are not new. They are generally available (GA) for speech recognition and language model customization, and beta for acoustic model customization.

    The other five dialects are new and are beta for all uses. Because they are beta, these additional dialects might not be ready for production use and are subject to change. They are initial offerings that are expected to improve in quality with time and usage.

    For more information, see the following sections:

    -   [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported)
    -   [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support)

## 24 June 2019
{: #speech-to-text-24june2019}
{: release-note}

Updates to Brazilian Portuguese and US English models for improved speech recognition
:   The following narrowband models have been updated for improved speech recognition:
    -   Brazilian Portuguese narrowband model (`pt-BR_NarrowbandModel`)
    -   US English narrowband model (`en-US_NarrowbandModel`)

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

New support for concurrent requests to update different custom acoustic models
:   The service now allows you to submit multiple simultaneous requests to add different audio resources to a custom acoustic model. Previously, the service allowed only one request at a time to add audio to a custom model.

New `updated` field for methods that list custom models
:   The output of the HTTP `GET` methods that list information about custom language and custom acoustic models now includes an `updated` field. The field indicates the date and time in Coordinated Universal Time (UTC) at which the custom model was last modified.

Change to schema for warnings associated with custom model training
:   The schema changed for a warning that is generated by a custom model training request when the `strict` parameter is set to `false`. The names of the fields changed from `warning_id` and `description` to `code` and `message`, respectively. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## 10 June 2019
{: #speech-to-text-10june2019}
{: release-note}

Processing metrics not available with synchronous HTTP interface
:    Processing metrics are available only with the WebSocket and asynchronous HTTP interfaces. They are not supported with the synchronous HTTP interface. For more information, see [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing-metrics).

## 17 May 2019
{: #speech-to-text-17may2019}
{: release-note}

New processing metrics and audio metrics features for speech recognition
:   The service now offers two types of optional metrics with speech recognition requests:
    -   [Processing metrics](/docs/speech-to-text?topic=speech-to-text-metrics#processing-metrics) provide detailed timing information about the service's analysis of the input audio. The service returns the metrics at specified intervals and with transcription events, such as interim and final results. Use the metrics to gauge the service's progress in transcribing the audio.
    -   [Audio metrics](/docs/speech-to-text?topic=speech-to-text-metrics#audio-metrics) provide detailed information about the signal characteristics of the input audio. The results provide aggregated metrics for the entire input audio at the conclusion of speech processing. Use the metrics to determine the characteristics and quality of the audio.

    You can request both types of metrics with any speech recognition request. By default, the service returns no metrics for a request.

Updates to Japanese broadband model for improved speech recognition
:   The Japanese broadband model (`ja-JP_BroadbandModel`) has been updated for improved speech recognition. By default, the service automatically uses the updated model for all speech recognition requests. If you have custom language or custom acoustic models that are based on the model, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

## 10 May 2019
{: #speech-to-text-10may2019}
{: release-note}

Updates to Spanish models for improved speech recognition
:   The Spanish language models have been updated for improved speech recognition:

    -   `es-ES_BroadbandModel`
    -   `es-ES_NarrowbandModel`

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

## 19 April 2019
{: #speech-to-text-19april2019}
{: release-note}

New `strict` parameter for custom model training now available
:   The training methods of the customization interface now include a `strict` query parameter that indicates whether training is to proceed if a custom model contains a mix of valid and invalid resources. By default, training fails if a custom model contains one or more invalid resources. Set the parameter to `false` to allow training to proceed as long as the model contains at least one valid resource. The service excludes invalid resources from the training.
    -   For more information about using the `strict` parameter with the `POST /v1/customizations/{customization_id}/train` method, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language) and [Training failures](/docs/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
    -   For more information about using the `strict` parameter with the `POST /v1/acoustic_customizations/{customization_id}/train` method, see [Train the custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic) and [Training failures](/docs/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).

New limits on maximum number of out-of-vocabulary words for custom language models
:   You can now add a maximum of 90 thousand out-of-vocabulary (OOV) words to the words resource of a custom language model. The previous maximum was 30 thousand OOV words. This figure includes OOV words from all sources (corpora, grammars, and individual custom words that you add directly). You can add a maximum of 10 million total words to a custom model from all sources. For more information, see [How much data do I need?](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount).

## 3 April 2019
{: #speech-to-text-3april2019}
{: release-note}

New limits on maximum amount of audio for custom acoustic models
:   Custom acoustic models now accept a maximum of 200 hours of audio. The previous maximum limit was 100 hours of audio.

## 21 March 2019
{: #speech-to-text-21march2019}
{: release-note}

Visibility of service credentials now restricted by role
:   Users can now see only service credential information that is associated with the role that has been assigned to their {{site.data.keyword.cloud_notm}} account. For example, if you are assigned a `reader` role, any `writer` or higher levels of service credentials are no longer visible.

    This change does not affect API access for users or applications with existing service credentials. The change affects only the viewing of credentials within {{site.data.keyword.cloud_notm}}.

## 15 March 2019
{: #speech-to-text-15march2019}
{: release-note}

New support for A-law audio format
:   The service now supports audio in the A-law (`audio/alaw`) format. For more information, see [audio/alaw format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-alaw).

## 11 March 2019
{: #speech-to-text-11march2019}
{: release-note}

Change to passing value of `0` for `max_alternatives` parameter
:   For the `max_alternatives` parameter, the service again accepts a value of `0`. If you specify `0`. the service automatically uses the default value, `1`. A change made for the March 4 service update caused a value of `0` to return an error. (The service returns an error if you specify a negative value.)

Change to passing value of `0` for `word_alternatives_threshold` parameter
:   For the `word_alternatives_threshold` parameter, the service again accepts a value of `0` . A change made for the March 4 service update caused a value of `0` to return an error. (The service returns an error if you specify a negative value.)

New limit on maximum precision for confidence scores
:   The service now returns all confidence scores with a maximum precision of two decimal places. This change includes confidence scores for transcripts, word confidence, word alternatives, keyword results, and speaker labels.

## 4 March 2019
{: #speech-to-text-4march2019}
{: release-note}

Updates to Brazilian Portuguese, French, and Spanish narrowband models for improved speech recognition
:   The following narrowband language models have been updated for improved speech recognition:
    -   Brazilian Portuguese narrowband model (`pt-BR_NarrowbandModel`)
    -   French French model (`fr-FR_NarrowbandModel`)
    -   Spanish narrowband model (`es-ES_NarrowbandModel`)

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

## 28 January 2019
{: #speech-to-text-28january2019}
{: release-note}

New support for IBM Cloud IAM by WebSocket interface
:   The WebSocket interface now supports token-based Identity and Access Management (IAM) authentication from browser-based JavaScript code. The limitation to the contrary has been removed. To establish an authenticated connection with the WebSocket `/v1/recognize` method:
    -   If you use IAM authentication, include the `access_token` query parameter.
    -   If you use Cloud Foundry service credentials, include the `watson-token` query parameter.

    For more information, see [Open a connection](/docs/speech-to-text?topic=speech-to-text-websockets#ws-open).

## 20 December 2018
{: #speech-to-text-20december2018}
{: release-note}

New beta grammars feature for custom language models now available
:   The service now supports grammars for speech recognition. Grammars are available as beta functionality for all languages that support language model customization. You can add grammars to a custom language model and use them to restrict the set of phrases that the service can recognize from audio. You can define a grammar in Augmented Backus-Naur Form (ABNF) or XML Form.

    The following four methods are available for working with grammars:
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` adds a grammar file to a custom language model.
    -   `GET /v1/customizations/{customization_id}/grammars` lists information about all grammars for a custom model.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` returns information about a specified grammar for a custom model.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` removes an existing grammar from a custom model.

    You can use a grammar for speech recognition with the WebSocket and HTTP interfaces. Use the `language_customization_id` and `grammar_name` parameters to identify the custom model and the grammar that you want to use. Currently, you can use only a single grammar with a speech recognition request.

    For more information about grammars, see the following documentation:
    -   [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars)
    -   [Understanding grammars](/docs/speech-to-text?topic=speech-to-text-grammarUnderstand)
    -   [Adding a grammar to a custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd)
    -   [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse)
    -   [Managing grammars](/docs/speech-to-text?topic=speech-to-text-manageGrammars)
    -   [Example grammars](/docs/speech-to-text?topic=speech-to-text-grammarExamples)

    For information about all methods of the interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

New numeric redaction feature for US English, Japanese, and Korean now available
:   A new numeric redaction feature is now available to mask numbers that have three or more consecutive digits. Redaction is intended to remove sensitive personal information, such as credit card numbers, from transcripts. You enable the feature by setting the `redaction` parameter to `true` on a recognition request. The feature is beta functionality that is available for US English, Japanese, and Korean only. For more information, see [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction).

New French and German narrowband models now available
:   The following new German and French language models are now available with the service:
    -   French narrowband model (`fr-FR_NarrowbandModel`)
    -   German narrowband model (`de-DE_NarrowbandModel`)

    Both new models support language model customization (GA) and acoustic model customization (beta). For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

New US English `en-US_ShortForm_NarrowbandModel` now available
:   A new US English language model, `en-US_ShortForm_NarrowbandModel`, is now available. The new model is intended for use in Interactive Voice Response and Automated Customer Support solutions. The model supports language model customization (GA) and acoustic model customization (beta). For more information, see [The US English short-form model](/docs/speech-to-text?topic=speech-to-text-models#modelsShortform).

Updates to UK English and Spanish narrowband models for improved speech recognition
:   The following language models have been updated for improved speech recognition:
    -   UK English narrowband model (`en-GB_NarrowbandModel`)
    -   Spanish narrowband model (`es-ES_NarrowbandModel`)

    By default, the service automatically uses the updated models for all speech recognition requests. If you have custom language or custom acoustic models that are based on the models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

New support for G.279 audio format
:   The service now supports audio in the G.729 (`audio/g729`) format. The service supports only G.729 Annex D for narrowband audio. For more information, see [audio/g729 format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-g729).

Speakers labels feature now available for UK English narrowband model
:   The speaker labels feature is now available for the narrowband model for UK English (`en-GB_NarrowbandModel`). The feature is beta functionality for all supported languages. For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

New limits on maximum amount of audio for custom acoustic models
:   The maximum amount of audio that you can add to a custom acoustic model has increased from 50 hours to 100 hours.

## 13 December 2018
{: #speech-to-text-13december2018}
{: release-note}

New London location now available
:   The {{site.data.keyword.speechtotextshort}} service is now available in the {{site.data.keyword.cloud_notm}} London location (**eu-gb**). Like all locations, London uses token-based IAM authentication. All new services instances that you create in this location use IAM authentication.

## 12 November 2018
{: #speech-to-text-12november2018}
{: release-note}

New support for smart formatting for Japanese speech recognition
:   The service now supports smart formatting for Japanese speech recognition. Previously, the service supported smart formatting for US English and Spanish only. The feature is beta functionality for all supported languages. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).

## 7 November 2018
{: #speech-to-text-7november2018}
{: release-note}

New Tokyo location now available
:   The {{site.data.keyword.speechtotextshort}} service is now available in the {{site.data.keyword.cloud_notm}} Tokyo location (**jp-tok**). Like all locations, Tokyo uses token-based IAM authentication. All new services instances that you create in this location use IAM authentication.

## 30 October 2018
{: #speech-to-text-30october2018}
{: release-note}

New support for token-based IBM Cloud IAM
:   The {{site.data.keyword.speechtotextshort}} service has migrated to token-based IAM authentication for all locations. All {{site.data.keyword.cloud_notm}} services now use IAM authentication. The {{site.data.keyword.speechtotextshort}} service migrated in each location on the following dates:

    -   Dallas (**us-south**): October 30, 2018
    -   Frankfurt (**eu-de**): October 30, 2018
    -   Washington, DC (**us-east**): June 12, 2018
    -   Sydney (**au-syd**): May 15, 2018

    The migration to IAM authentication affects new and existing service instances differently:

    -   *All new service instances that you create in any location* now use IAM authentication to access the service. You can pass either a bearer token or an API key: Tokens support authenticated requests without embedding service credentials in every call; API keys use HTTP basic authentication. When you use any of the {{site.data.keyword.watson}} SDKs, you can pass the API key and let the SDK manage the lifecycle of the tokens.
    -   *Existing service instances that you created in a location before the indicated migration date* continue to use the `{username}` and `{password}` from their previous Cloud Foundry service credentials for authentication until you migrate them to use IAM authentication. 

    For more information, see the following documentation:

    -   To learn which authentication mechanism your service instance uses, view your service credentials by clicking the instance on the [{{site.data.keyword.cloud_notm}} dashboard](https://{DomainName}/dashboard/apps){: external}.
    -   For more information about using IAM tokens with {{site.data.keyword.watson}} services, see [Authenticating to {{site.data.keyword.watson}} services](/docs/watson?topic=watson-iam).
    -   For examples that use IAM authentication, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## 9 October 2018
{: #speech-to-text-9october2018}
{: release-note}

Important updates to pricing charges for speech recognition requests
:   As of 1 October 2018, you are now charged for all audio that you pass to the service for speech recognition. The first one thousand minutes of audio that you send each month are no longer free. For more information about the pricing plans for the service, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud_notm}} Catalog](https://{DomainName}/catalog/services/speech-to-text){: external}.

The `Content-Type` header now optional for most speech recognition requests
:   The `Content-Type` header is now optional for most speech recognition requests. The service now automatically detects the audio format (MIME type) of most audio. You must continue to specify the content type for the following formats:
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Where indicated, the content type that you specify for these formats must include the sampling rate and can optionally include the number of channels and the endianness of the audio. For all other audio formats, you can omit the content type or specify a content type of `application/octet-stream` to have the service auto-detect the format.

    When you use the `curl` command to make a speech recognition request with the HTTP interface, you must specify the audio format with the `Content-Type` header, specify `"Content-Type: application/octet-stream"`, or specify `"Content-Type:"`. If you omit the header entirely, `curl` uses a default value of `application/x-www-form-urlencoded`. Most of the examples in this documentation continue to specify the format for speech recognition requests regardless of whether it's required.

    This change applies to the following methods:
    -   `/v1/recognize` for WebSocket requests. The `content-type` field of the text message that you send to initiate a request over an open WebSocket connection is now optional.
    -   `POST /v1/recognize` for synchronous HTTP requests. The `Content-Type` header is now optional. (For multipart requests, the `part_content_type` field of the JSON metadata is also now optional.)
    -   `POST /v1/recognitions` for asynchronous HTTP requests. The `Content-Type` header is now optional.

    For more information, see [Audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-list).

Updates to Brazilian Portuguese broadband model for improved speech recognition
:   The Brazilian Portuguese broadband model, `pt-BR_BroadbandModel`, was updated for improved speech recognition. By default, the service automatically uses the updated model for all recognition requests. If you have custom language or custom acoustic models that are based on this model, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

The `customization_id` parameter renamed to `language_customization_id`
:   The `customization_id` parameter of the speech recognition methods is deprecated and will be removed in a future release. To specify a custom language model for a speech recognition request, use the `language_customization_id` parameter instead. This change applies to the following methods:
    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for synchronous HTTP requests (including multipart requests)
    -   `POST /v1/recognitions` for asynchronous HTTP requests

## 10 September 2018
{: #speech-to-text-10september2018}
{: release-note}

New German broadband model
:   The service now supports a German broadband model, `de-DE_BroadbandModel`. The new German model supports language model customization (generally available) and acoustic model customization (beta).
    -   For information about how the service parses corpora for German, see [Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in German, see [Guidelines for Dutch, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).

Language model customization now available for Brazilian Portuguese
:   The existing Brazilian Portuguese models, `pt-BR_BroadbandModel` and `pt-BR_NarrowbandModel`, now support language model customization (generally available). The models were not updated to enable this support, so no upgrade of existing custom acoustic models is required.
    -   For information about how the service parses corpora for Brazilian Portuguese, see [Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in Brazilian Portuguese, see [Guidelines for Dutch, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).

Updates to US English and Japanese models for improved speech recognition
:   New versions of the US English and Japanese broadband and narrowband models are available:
    -   US English broadband model (`en-US_BroadbandModel`)
    -   US English narrowband model (`en-US_NarrowbandModel`)
    -   Japanese broadband model (`ja-JP_BroadbandModel`)
    -   Japanese narrowband model (`ja-JP_NarrowbandModel`)

    The new models offer improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on these models, you must upgrade your existing custom models to take advantage of the updates by using the following methods:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

Keyword spotting and word alternatives features now generally available
:   The keyword spotting and word alternatives features are now generally available (GA) rather than beta functionality for all languages. For more information, see
    -   [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-spotting#keyword-spotting)
    -   [Word alternatives](/docs/speech-to-text?topic=speech-to-text-spotting#word-alternatives)

Defect fix: Improve documentation for customization interface
:   **Defect fix:** The following known issues that were associated with the customization interface have been resolved and are fixed in production. The following information is preserved for users who may have encountered the problems in the past.

    -   If you add data to a custom language or custom acoustic model, you must retrain the model before using it for speech recognition. The problem shows up in the following scenario:

        1.  The user creates a new custom model (language or acoustic) and trains the model.
        1.  The user adds additional resources (words, corpora, or audio) to the custom model but does not retrain the model.
        1.  The user cannot use the custom model for speech recognition. The service returns an error of the following form when used with a speech recognition request:

            ```json
            {
              "code_description": "Bad Request",
              "code": 400,
              "error": "Requested custom language model is not available.
                        Please make sure the custom model is trained."
            }
            ```
            {: codeblock}

        To work around this issue, the user must retrain the custom model on its latest data. The user can then use the custom model with speech recognition.
    -   Before training an existing custom language or custom acoustic model, you must upgrade it to the latest version of its base model. The problem shows up in the following scenario:

        1.  The user has an existing custom model (language or acoustic) that is based on a model that has been updated.
        1.  The user trains the existing custom model against the old version of the base model without first upgrading to the latest version of the base model.
        1.  The user cannot use the custom model for speech recognition.

        To work around this issue, the user must use the `POST /v1/customizations/{customization_id}/upgrade_model` or `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` method to upgrade the custom model to the latest version of its base model. The user can then use the custom model with speech recognition.

## 7 September 2018
{: #speech-to-text-7september2018}
{: release-note}

Session-based interface no longer available
:   The session-based HTTP REST interface is no longer supported. All information related to sessions is removed from the documentation. The following methods are no longer available:

    -   `POST /v1/sessions`
    -   `POST /v1/sessions/{session_id}/recognize`
    -   `GET /v1/sessions/{session_id}/recognize`
    -   `GET /v1/sessions/{session_id}/observe_result`
    -   `DELETE /v1/sessions/{session_id}`

    If your application uses the sessions interface, you must migrate to one of the remaining HTTP REST interfaces or to the WebSocket interface. For more information, see the service update for [8 August 2018](#speech-to-text-8august2018).

## 8 August 2018
{: #speech-to-text-8august2018}
{: release-note}

Deprecation notice for session-based speech recognition interface
:   The session-based HTTP REST interface is deprecated as of **August 8, 2018**. All methods of the sessions API will be removed from service as of **September 7, 2018**, after which you will no longer be able to use the session-based interface. This notice of immediate deprecation and 30-day removal applies to the following methods:

    -   `POST /v1/sessions`
    -   `POST /v1/sessions/{session_id}/recognize`
    -   `GET /v1/sessions/{session_id}/recognize`
    -   `GET /v1/sessions/{session_id}/observe_result`
    -   `DELETE /v1/sessions/{session_id}`

    If your application uses the sessions interface, you must migrate to one of the following interfaces by September 7:

    -   For stream-based speech recognition (including live-use cases), use the [WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets), which provides access to interim results and the lowest latency.
    -   For file-based speech recognition, use one of the following interfaces:
        -   For shorter files of up to a few minutes of audio, use either the [synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http) `(POST /v1/recognize`) or the [asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) (`POST /v1/recognitions`).
        -   For longer files of more than a few minutes of audio, use the asynchronous HTTP interface. The asynchronous HTTP interface accepts as much as 1 GB of audio data with a single request.

    The WebSocket and HTTP interfaces provide the same results as the sessions interface (only the WebSocket interface provides interim results). You can also use one of the {{site.data.keyword.watson}} SDKs, which simplify application development with any of the interfaces. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## 13 July 2018
{: #speech-to-text-13july2018}
{: release-note}

Updates to Spanish narrowband model for improved speech recognition
:   The Spanish narrowband model, `es-ES_NarrowbandModel`, was updated for improved speech recognition. By default, the service automatically uses the updated model for all recognition requests. If you have custom language or custom acoustic models that are based on this model, you must upgrade your custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

    As of this update, the following two versions of the Spanish narrowband model are available:

    -   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (current)
    -   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (previous)

    The following version of the model is no longer available:

    -   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

    A recognition request that attempts to use a custom model that is based on the now unavailable base model uses the latest base model without any customization. The service returns the following warning message: `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` To resume using a custom model that is based on the unavailable model, you must first upgrade the model by using the appropriate `upgrade_model` method described previously.

## 12 June 2018
{: #speech-to-text-12june2018}
{: release-note}

New features for applications hosted in Washington, DC, location
:   The following features are enabled for applications that are hosted in Washington, DC (**us-east**):

    -   The service now supports a new API authentication process. For more information, see the [30 October 2018 service update](#speech-to-text-30october2018).
    -   The service now supports the `X-Watson-Metadata` header and the `DELETE /v1/user_data` method. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

## 15 May 2018
{: #speech-to-text-15may2018}
{: release-note}

New features for applications hosted in Sydney location
:   The following features are enabled for applications that are hosted in Sydney (**au-syd**):

    -   The service now supports a new API authentication process. For more information, see the [30 October 2018 service update](#speech-to-text-30october2018).
    -   The service now supports the `X-Watson-Metadata` header and the `DELETE /v1/user_data` method. For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

## 26 March 2018
{: #speech-to-text-26march2018}
{: release-note}

Language model customization now available for French broadband model
:   The service now supports language model customization for the French broadband language model, `fr-FR_BroadbandModel`. The French model is generally available (GA) for production use with language model customization.
    -   For more information about how the service parses corpora for French, see [Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in French, see [Guidelines for Dutch, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).

Updates to French, Korean, and Spanish models for improved speech recognition
:   The following models were updated for improved speech recognition:

    -   Korean narrowband model (`ko-KR_NarrowbandModel`)
    -   Spanish narrowband model (`es-ES_NarrowbandModel`)
    -   French broadband model (`fr-FR_BroadbandModel`)

    By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on either of these models, you must upgrade your custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).

The `version` parameter renamed to `base_model_version`
:   The `version` parameter of the following methods is now named `base_model_version`:

    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for sessionless HTTP requests
    -   `POST /v1/sessions` for session-based HTTP requests
    -   `POST /v1/recognitions` for asynchronous HTTP requests

    The `base_model_version` parameter specifies the version of a base model that is to be used for speech recognition. For more information, see [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use) and [Making speech recognition requests with upgraded custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-recognition).

New support for smart formatting for Spanish speech recognition
:   Smart formatting is now supported for Spanish as well as US English. For US English, the feature also now converts keyword strings into punctuation symbols for periods, commas, question marks, and exclamation points. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).

## 1 March 2018
{: #speech-to-text-1march2018}
{: release-note}

Updates to French and Spanish broadband models for improved speech recognition
:   The French and Spanish broadband models, `fr-FR_BroadbandModel` and `es-ES_BroadbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on either of these models, you must upgrade your custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade). The section presents rules for upgrading custom models, the effects of upgrading, and approaches for using upgraded models.

## 1 February 2018
{: #speech-to-text-1february2018}
{: release-note}

New Korean models
:   The service now offers language models for Korean: `ko-KR_BroadbandModel` for audio that is sampled at a minimum of 16 kHz, and `ko-KR_NarrowbandModel` for audio that is sampled at a minimum of 8 kHz. For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).

    For language model customization, the Korean models are generally available (GA) for production use; for acoustic model customization, they are beta functionality. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

    -   For more information about how the service parses corpora for Korean, see [Parsing of Korean](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages-koKR).
    -   For more information about creating sounds-like pronunciations for custom words in Korean, see [Guidelines for Korean](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-koKR).

## 14 December 2017
{: #speech-to-text-14december2017}
{: release-note}

Language model customization now generally available
:   Language model customization and all associated parameters are now generally available (GA) for all supported languages: Japanese, Spanish, UK English, and US English.

Beta acoustic model customization now available for all languages
:   The service now supports acoustic model customization as beta functionality for all available languages. You can create custom acoustic models for broadband or narrowband models for all languages. For an introduction to customization, including acoustic model customization, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).

New `version` parameter for speech recognition
:   The various methods for making recognition requests now include a new `version` parameter that you can use to initiate requests that use either the older or upgraded versions of base and custom models. Although it is intended primarily for use with custom models that have been upgraded, the `version` parameter can also be used without custom models. For more information, see [Making speech recognition requests with upgraded custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use#custom-upgrade-use-recognition).

Updates to US English models for improved speech recognition
:   The US English models, `en-US_BroadbandModel` and `en-US_NarrowbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. If you have custom language or custom acoustic models that are based on the US English models, you must upgrade your custom models to take advantage of the updates by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    For more information about the procedure, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade). The section presents rules for upgrading custom models, the effects of upgrading, and approaches for using upgraded models. Currently, the methods apply only to the new US English base models. But the same information will apply to upgrades of other base models as they become available.

Language model customization now available for UK English
:   The service now supports language model customization for the UK English models, `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`. Although the service handles UK and US English corpora and custom words in a generally similar fashion, some important differences exist:
    -   For more information about how the service parses corpora for UK English, see [Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish](/docs/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   For more information about creating sounds-like pronunciations for custom words in UK English, see [Guidelines for English](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-english). Specifically, for UK English, you cannot use periods or dashes in sounds-like pronunciations.

## 2 October 2017
{: #speech-to-text-2october2017}
{: release-note}

New beta acoustic model customization interface for US English, Japanese, and Spanish
:   The customization interface now offers acoustic model customization. You can create custom acoustic models that adapt the service's base models to your environment and speakers. You populate and train a custom acoustic model on audio that more closely matches the acoustic signature of the audio that you want to transcribe. You then use the custom acoustic model with recognition requests to increase the accuracy of speech recognition.

    Custom acoustic models complement custom language models. You can train a custom acoustic model with a custom language model, and you can use both types of model during speech recognition. Acoustic model customization is a beta interface that is available only for US English, Japanese, and Spanish.

    -   For more information about the languages that are supported by the customization interface and the level of support that is available for each language, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).
    -   For more information about the service's customization interface, see [Understanding customization](/docs/speech-to-text?topic=speech-to-text-customization).
    -   For more information about creating a custom acoustic model, see [Creating a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic).
    -   For more information about using a custom acoustic model, see [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse).
    -   For more information about all methods of the customization interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

New beta `customization_weight` parameter for custom language models
:   For language model customization, the service now includes a beta feature that sets an optional customization weight for a custom language model. A customization weight specifies the relative weight to be given to words from a custom language model versus words from the service's base vocabulary. You can set a customization weight during both training and speech recognition. For more information, see [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight).

Updates to Japanese broadband model for improved speech recognition
:   The `ja-JP_BroadbandModel` language model was upgraded to capture improvements in the base model. The upgrade does not affect existing custom models that are based on the model.

New `endianness` parameter for `audio/l16` audio format
:   The service now includes a parameter to specify the endianness of audio that is submitted in `audio/l16` (Linear 16-bit Pulse-Code Modulation (PCM)) format. In addition to specifying `rate` and `channels` parameters with the format, you can now also specify `big-endian` or `little-endian` with the `endianness` parameter. For more information, see [audio/l16 format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-l16).

## 14 July 2017
{: #speech-to-text-14july2017}
{: release-note}

New support for MP3 (MPEG) audio format
:   The service now supports the transcription of audio in the MP3 or Motion Picture Experts Group (MPEG) format. For more information, see [audio/mp3 and audio/mpeg formats](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-mp3).

Beta language model customization now available for Spanish
:   The language model customization interface now supports Spanish as beta functionality. You can create a custom model based on either of the base Spanish language models: `es-ES_BroadbandModel` or `es-ES_NarrowbandModel`; for more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate). Pricing for recognition requests that use Spanish custom language models is the same as for requests that use US English and Japanese models.

New `dialect` field for method that creates a custom language model
:   The JSON `CreateLanguageModel` object that you pass to the `POST /v1/customizations` method to create a new custom language model now includes a `dialect` field. The field specifies the dialect of the language that is to be used with the custom model. By default, the dialect matches the language of the base model. The parameter is meaningful only for Spanish models, for which the service can create a custom model that is suited for speech in one of the following dialects:
    -   `es-ES` for Castilian Spanish (the default)
    -   `es-LA` for Latin-American Spanish
    -   `es-US` for North-American (Mexican) Spanish

    The `GET /v1/customizations` and `GET /v1/customizations/{customization_id}` methods of the customization interface include the dialect of a custom model in their output. For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#createModel-language) and [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).

New names for UK English models
:   The names of the language models `en-UK_BroadbandModel` and `en-UK_NarrowbandModel` have been deprecated. The models are now available with the names `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`.

    The deprecated `en-UK_{model}` names continue to function, but the `GET /v1/models` method no longer returns the names in the list of available models. You can still query the names directly with the `GET /v1/models/{model_id}` method.

## 1 July 2017
{: #speech-to-text-1july2017}
{: release-note}

Language model custom now generally available for US English and Japanese
:   The language model customization interface of the service is now generally available (GA) for both of its supported languages, US English and Japanese. {{site.data.keyword.IBM_notm}} does not charge for creating, hosting, or managing custom language models. As described in the next bullet, {{site.data.keyword.IBM_notm}} now charges an extra $0.03 (USD) per minute of audio for recognition requests that use custom models.

Updates to pricing plans for the service
:   {{site.data.keyword.IBM_notm}} updated the pricing for the service by
    -   Eliminating the add-on price for using narrowband models
    -   Providing Graduated Tiered Pricing for high-volume customers
    -   Charging an additional $0.03 (USD) per minute of audio for recognition requests that use US English or Japanese custom language models

    For more information about the pricing updates, see
    -   The {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud_notm}} Catalog](https://{DomainName}/catalog/services/speech-to-text){: external}
    -   The [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing)

Empty body no longer required for HTTP POST requests
:   You no longer need to pass an empty data object as the body for the following `POST` requests:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    For example, you now call the `POST /v1/sessions` method with `curl` as follows:

    ```sh
    curl -X POST -u "{username}:{password}" \
    --cookie-jar cookies.txt \
    "{url}/v1/sessions"
    ```
    {: pre}

    You no longer need to pass the following `curl` option with the request: `--data "{}"`. If you experience any problems with one of these `POST` requests, try passing an empty data object with the body of the request. Passing an empty object does not change the nature or meaning of the request in any way.

## 22 May 2017
{: #speech-to-text-22may2017}
{: release-note}

The `continuous` parameter removed from all methods
:   The `continuous` parameter is removed from all methods that initiate recognition requests. The service now transcribes an entire audio stream until it ends or times out, whichever occurs first. This behavior is equivalent to setting the former `continuous` parameter to `true`. By default, the service previously stopped transcription at the first half-second of non-speech (typically silence) if the parameter was omitted or set to `false`.

    Existing applications that set the parameter to `true` will see no change in behavior. Applications that set the parameter to `false` or that relied on the default behavior will likely see a change. If a request specifies the parameter, the service now responds by returning a warning message for the unknown parameter:

    ```json
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    The request succeeds despite the warning, and an existing session or WebSocket connection is not affected.

    {{site.data.keyword.IBM_notm}} removed the parameter to respond to overwhelming feedback from the developer community that specifying `continuous=false` added little value and could reduce overall transcription accuracy.

Sending audio required to avoid session timeout
:   It is no longer possible to avoid a session timeout without sending audio:
    -   When you use the WebSocket interface, the client can no longer keep a connection alive by sending a JSON text message with the `action` parameter set to `no-op`. Sending a `no-op` message does not generate an error, but it has no effect.
    -   When you use sessions with the HTTP interface, the client can no longer extend the session by sending a `GET /v1/sessions/{session_id}/recognize` request. The method still returns the status of an active session, but it does not keep the session active.

    You can now do the following to keep a session alive:
    -   Set the `inactivity_timeout` parameter to `-1` to avoid the 30-second inactivity timeout.
    -   Send any audio data, including just silence, to the service to avoid the 30-second session timeout. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

    For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts). Ideally, you would establish a session just before you obtain audio for transcription and maintain the session by sending audio at a rate that is close to real time. Also, make sure your application recovers gracefully from closed sessions or connections.

    {{site.data.keyword.IBM_notm}} removed this functionality to ensure that it continues to offer all users a best-in-class, low-latency speech recognition service.

## 10 April 2017
{: #speech-to-text-10april2017}
{: release-note}

Speaker labels now supported for US English, Spanish, and Japanese
:   The service now supports the speaker labels feature for the following broadband models:
    -   US English broadband model (`en-US-BroadbandModel`)
    -   Spanish broadband model (`es-ES-BroadbandModel`)
    -   Japanese broadband model (`ja-JP_BroadbandModel`)

    For more information, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

New support for Web Media (WebM) audio format
:   The service now supports the Web Media (WebM) audio format with the Opus or Vorbis codec. The service now also supports the Ogg audio format with the Vorbis codec in addition to the Opus codec. For more information about supported audio formats, see [audio/webm format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-webm).

New support for Cross-Origin Resource Sharing
:   The service now supports Cross-Origin Resource Sharing (CORS) to allow browser-based clients to call the service directly. For more information, see [CORS support](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-cors).

New method to unregister a callback URL with asynchronous HTTP interface
:   The asynchronous HTTP interface now offers a `POST /v1/unregister_callback` method that removes the registration for an allowlisted callback URL. For more information, see [Unregistering a callback URL](/docs/speech-to-text?topic=speech-to-text-async#unregister).

Defect fix: Eliminate timeouts for long audio with WebSocket interface
:   **Defect fix:** The WebSocket interface no longer times out for recognition requests for especially long audio files. You no longer need to request interim results with the JSON `start` message to avoid the timeout. (This issue was described in the [update for 10 March 2016](#speech-to-text-10march2016).)

New HTTP error codes
:   The following language model customization methods can now return the following HTTP error codes:
    -   The `DELETE /v1/customizations/{customization_id}` method now returns HTTP response code 401 if you attempt to delete a nonexistent custom model.
    -   The `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` method now returns HTTP response code 400 if you attempt to delete a nonexistent corpus.

## 8 March 2017
{: #speech-to-text-8march2017}
{: release-note}

Asynchronous HTTP interface is now generally available
:   The asynchronous HTTP interface is now generally available (GA). Prior to this date, it was beta functionality.

## 1 December 2016
{: #speech-to-text-1december2016}
{: release-note}

New beta speaker labels feature
:   The service now offers a beta speaker labels feature for narrowband audio in US English, Spanish, or Japanese. The feature identifies which words were spoken by which speakers in a multi-person exchange. The sessionless, session-based, asynchronous, and WebSocket recognition methods each include a `speaker_labels` parameter that accepts a boolean value to indicate whether speaker labels are to be included in the response. For more information about the feature, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

Beta language model customization now available for Japanese
:   The beta language model customization interface is now supported for Japanese in addition to US English. All methods of the interface support Japanese. For more information, see the following sections:
    -   For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate) and [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse).
    -   For general and Japanese-specific considerations for adding a corpus text file, see [Preparing a corpus text file](/docs/speech-to-text?topic=speech-to-text-corporaWords#prepareCorpus) and [What happens when I add a corpus file?](/docs/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)
    -   For Japanese-specific considerations when specifying the `sounds_like` field for a custom word, see [Guidelines for Japanese](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-jaJP).
    -   For more information about all methods of the customization interface, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

New method for listing information about a corpus
:   The language model customization interface now includes a `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method that lists information about a specified corpus. The method is useful for monitoring the status of a request to add a corpus to a custom model. For more information, see [Listing corpora for a custom language model](/docs/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).

New `count` field for methods that list words for custom language models
:   The JSON response that is returned by the `GET /v1/customizations/{customization_id}/words` and `GET /v1/customizations/{customization_id}/words/{word_name}` methods now includes a `count` field for each word. The field indicates the number of times the word is found across all corpora. If you add a custom word to a model before it is added by any corpora, the count begins at `1`. If the word is added from a corpus first and later modified, the count reflects only the number of times it is found in corpora. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

    For custom models that were created prior to the existence of the `count` field, the field always remains at `0`. To update the field for such models, add the model's corpora again and include the `allow_overwrite` parameter with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method.

New `sort` parameter for methods that list words for custom language models
:   The `GET /v1/customizations/{customization_id}/words` method now includes a `sort` query parameter that controls the order in which the words are to be listed. The parameter accepts two arguments, `alphabetical` or `count`, to indicate how the words are to be sorted. You can prepend an optional `+` or `-` to an argument to indicate whether the results are to be sorted in ascending or descending order. By default, the method displays the words in ascending alphabetical order. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

    For custom models created prior to the introduction of the `count` field, use of the `count` argument with the `sort` parameter is meaningless. Use the default `alphabetical` argument with such models.

New `error` field format for methods that list words for custom language models
:   The `error` field that can be returned as part of the JSON response from the `GET /v1/customizations/{customization_id}/words` and `GET /v1/customizations/{customization_id}/words/{word_name}` methods is now an array. If the service discovered one or more problems with a custom word's definition, the field lists each problem element from the definition and provides a message that describes the problem. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

The `keywords_threshold` and `word_alternatives_threshold` parameters no longer accept a null value
:   The `keywords_threshold` and `word_alternatives_threshold` parameters of the recognition methods no longer accept a null value. To omit keywords and word alternatives from the response, omit the parameters. A specified value must be a float.

## 22 September 2016
{: #speech-to-text-22september2016}
{: release-note}

New beta language model customization interface
:   The service now offers a new beta language model customization interface for US English. You can use the interface to tailor the service's base vocabulary and language models via the creation of custom language models that include domain-specific terminology. You can add custom words individually or have the service extract them from corpora. To use your custom models with the speech recognition methods that are offered by any of the service's interfaces, pass the `customization_id` query parameter. For more information, see
    -   [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate)
    -   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)
    -   [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}

New support for `audio/mulaw` audio format
:   The list of supported audio formats now includes `audio/mulaw`, which provides single-channel audio encoded using the u-law (or mu-law) data algorithm. When you use this format, you must also specify the sampling rate at which the audio is captured. For more information, see [audio/mulaw format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-mulaw).

New `supported_features` identified when listing models
:   The `GET /v1/models` and `GET /v1/models/{model_id}` methods now return a `supported_features` field as part of their output for each language model. The additional information describes whether the model supports customization. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## 30 June 2016
{: #speech-to-text-30june2016}
{: release-note}

Beta asynchronous HTTP interface now supports all available languages
:   The beta asynchronous HTTP interface now supports all languages that are supported by the service. The interface was previously available for US English only. For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) and the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

## 23 June 2016
{: #speech-to-text-23june2016}
{: release-note}

New beta asynchronous HTTP interface now available
:   A beta asynchronous HTTP interface is now available. The interface provides full recognition capabilities for US English transcription via non-blocking HTTP calls. You can register callback URLs and provide user-specified secret strings to achieve authentication and data integrity with digital signatures. For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async) and the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

New beta `smart_formatting` parameter for speech recognition
:   A beta smart formatting feature that converts dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses into more conventional representations in final transcripts. You enable the feature by setting the `smart_formatting` parameter to `true` on a recognition request. The feature is beta functionality that is available for US English only. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).

New French broadband model
:   The list of supported models for speech recognition now includes `fr-FR_BroadbandModel` for audio in the French language that is sampled at a minimum of 16 kHz. For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).

New support for `audio/basic` audio format
:   The list of supported audio formats now includes `audio/basic`. The format provides single-channel audio that is encoded by using 8-bit u-law (or mu-law) data that is sampled at 8 kHz. For more information, see [audio/basic format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-basic).

Speech recognition methods now return warnings for invalid parameters
:   The various recognition methods can return a `warnings` response that includes messages about invalid query parameters or JSON fields that are included with a request. The format of the warnings changed. For example, `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` is now `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`

Empty body required for HTTP `POST` methods that pass no data
:   For HTTP `POST` requests that do not otherwise pass data to the service, you must include an empty request body of the form `{}`. With the `curl` command, you use the `--data` option to pass the empty data.

## 10 March 2016
{: #speech-to-text-10march2016}
{: release-note}

New maximum limits on audio transmitted for speech recognition
:   Both forms of data transmission (one-shot delivery and streaming) now impose a size limit of 100 MB on the audio data, as does the WebSocket interface. Formerly, the one-shot approach had a maximum limit of 4 MB of data. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission) (for all interfaces) and [Send audio and receive recognition results](/docs/speech-to-text?topic=speech-to-text-websockets#ws-audio) (for the WebSocket interface). The WebSocket section also discusses the maximum frame or message size of 4 MB enforced by the WebSocket interface.

HTTP and WebSocket interfaces can now return warnings
:   The JSON response for a recognition request can now include an array of warning messages for invalid query parameters or JSON fields that are included with a request. Each element of the array is a string that describes the nature of the warning followed by an array of invalid argument strings. For example, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`. For more information, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

Beta Apple iOS SDK is deprecated
:   The beta *{{site.data.keyword.watson}} Speech Software Development Kit (SDK) for the Apple® iOS operating system* is deprecated. Use the *{{site.data.keyword.watson}} SDK for the Apple® iOS operating system* instead. The new SDK is available from the [ios-sdk repository](https://github.com/watson-developer-cloud/swift-sdk){: external} in the `watson-developer-cloud` namespace on GitHub.

WebSocket interface can produce delayed results
:   The WebSocket interface can take minutes to produce final results for a recognition request for an especially long audio file. For the WebSocket interface, the underlying TCP connection remains idle while the service prepares the response. Therefore, the connection can close due to a timeout. To avoid the timeout with the WebSocket interface, request interim results (`\"interim_results\": \"true\"`) in the JSON for the `start` message to initiate the request. You can discard the interim results if you do not need them. This issue will be resolved in a future update.

## 19 January 2016
{: #speech-to-text-19january2016}
{: release-note}

New profanity filtering feature
:   The service was updated to include a new profanity filtering feature on January 19, 2016. By default, the service censors profanity from its transcription results for US English audio. For more information, see [Profanity filtering](/docs/speech-to-text?topic=speech-to-text-formatting#profanity-filtering).

## 17 December 2015
{: #speech-to-text-17december2015}
{: release-note}

New keyword spotting feature
:   The service now offers a keyword spotting feature. You can specify an array of keyword strings that are to be matched in the input audio. You must also specify a user-defined confidence level that a word must meet to be considered a match for a keyword. For more information, see [Keyword spotting](/docs/speech-to-text?topic=speech-to-text-spotting#keyword-spotting). The keyword spotting feature is beta functionality.

New word alternatives feature
:   The service now offers a word alternatives feature. The feature returns alternative hypotheses for words in the input audio that meet a user-defined confidence level. For more information, see [Word alternatives](/docs/speech-to-text?topic=speech-to-text-spotting#word-alternatives). The word alternatives feature is beta functionality.

New UK English and Arabic models
:   The service supports more languages with its transcription models: `en-UK_BroadbandModel` and `en-UK_NarrowbandModel` for UK English, and `ar-AR_BroadbandModel` for Modern Standard Arabic. For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).

New `session_closed` field for session-based methods
:   In the JSON responses that it returns for errors with session-based methods, the service now also includes a new `session_closed` field. The field is set to `true` if the session is closed as a result of the error. For more information about possible return codes for any method, see the [API & SDK reference](https://{DomainName}/apidocs/speech-to-text){: external}.

HTTP platform timeout no longer applies
:   HTTP recognition requests are no longer subject to a 10-minute platform timeout. The service now keeps the connection alive by sending a space character in the response JSON object every 20 seconds while recognition is ongoing. For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts).

Rate limiting with curl command is no longer needed
:   When you use the `curl` command to transcribe audio with the service, you no longer need to use the `--limit-rate` option to transfer data at a rate no faster than 40,000 bytes per second.

Changes to HTTP error codes
:   The service no longer returns HTTP status code 490 for the session-based HTTP methods `GET /v1/sessions/{session_id}/observe_result` and `POST /v1/sessions/{session_id}/recognize`. The service now responds with HTTP status code 400 instead.

## 21 September 2015
{: #speech-to-text-21september2015}
{: release-note}

New mobile SDKs available
:   Two new beta mobile SDKs are available for the speech services. The SDKs enable mobile applications to interact with both the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} services.
    -   The *{{site.data.keyword.watson}} Speech SDK for the Google Android™ platform* supports streaming audio to the {{site.data.keyword.speechtotextshort}} service in real time and receiving a transcript of the audio as you speak. The project includes an example application that showcases interaction with both of the speech services. The SDK is available from the [speech-android-sdk repository](https://github.com/watson-developer-cloud/speech-android-sdk){: external} in the `watson-developer-cloud` namespace on GitHub.
    -   The *{{site.data.keyword.watson}} Speech SDK for the Apple® iOS operating system* supports streaming audio to the {{site.data.keyword.speechtotextshort}} service and receiving a transcript of the audio in response. The SDK is available from the [speech-ios-sdk repository](https://github.com/germanattanasio/speech-ios-sdk){: external} in the `watson-developer-cloud` namespace on GitHub.

    Both SDKs support authenticating with the speech services by using either your {{site.data.keyword.cloud_notm}} service credentials or an authentication token. Because the SDKs are beta, they are subject to change in the future.

New Brazilian Portuguese and Mandarin Chinese models
:   The service supports two new languages, Brazilian Portuguese and Mandarin Chinese, with the following models:

    -   Brazilian Portuguese broadband model (`pt-BR_BroadbandModel`)
    -   Brazilian Portuguese narrowband model (`pt-BR_NarrowbandModel`)
    -   Mandarin Chinese broadband model (`zh-CN_BroadbandModel`)
    -   Mandarin Chinese narrowband model (`zh-CN_NarrowbandModel`)

    For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).

New support for `audio/ogg;codecs=opus` audio format
:   The HTTP `POST` requests `/v1/sessions/{session_id}/recognize` and `/v1/recognize`, as well as the WebSocket `/v1/recognize` request, support transcription of a new media type: `audio/ogg;codecs=opus` for Ogg format files that use the Opus codec. In addition, the `audio/wav` format for the methods now supports any encoding. The restriction about the use of linear PCM encoding is removed. For more information, see [audio/ogg format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-ogg).

New `sequence_id` parameter for long polling of sessions
:   The service now supports overcoming timeouts when you transcribe long audio files with the HTTP interface. When you use sessions, you can employ a long polling pattern by specifying sequence IDs with the `GET /v1/sessions/{session_id}/observe_result` and `POST /v1/sessions/{session_id}/recognize` methods for long-running recognition tasks. By using the new `sequence_id` parameter of these methods, you can request results before, during, or after you submit a recognition request.

New capitalization feature for US English transcription
:   For the US English language models, `en_US_BroadbandModel` and `en_US_NarrowbandModel`, the service now correctly capitalizes many proper nouns. For example, the service would return new text that reads "Barack Obama graduated from Columbia University" instead of "barack obama graduated from columbia university". This change might be of interest to you if your application is sensitive in any way to the case of proper nouns.

New HTTP error code
:   The HTTP `DELETE /v1/sessions/{session_id}` request does not return status code 415 "Unsupported Media Type". This return code is removed from the documentation for the method.

## 1 July 2015
{: #speech-to-text-1july2015}
{: release-note}

The {{site.data.keyword.speechtotextshort}} service is now generally available
:   The service moved from beta to general availability (GA) on July 1, 2015. The following differences exist between the beta and GA versions of the {{site.data.keyword.speechtotextshort}} APIs. The GA release requires that users upgrade to the new version of the service.

    The GA version of the HTTP API is compatible with the beta version. You need to change your existing application code only if you explicitly specified a model name. For example, the sample code available for the service from GitHub included the following line of code in the file `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    This line specified the default model `WatsonModel`, for the beta version of the service. If your application also specified this model, you need to change it to use one of the new models that are supported by the GA version. For more information, see the next bullet.

New token-based programming model
:   The service now supports a new programming model for direct interaction between a client and the service over a WebSocket connection. By using this model, a client can obtain an authentication token for communicating directly with the service. The token bypasses the need for a server-side proxy application in {{site.data.keyword.cloud_notm}} to call the service on the client's behalf. Tokens are the preferred means for clients to interact with the service.

    The service continues to support the old programming model that relied on a server-side proxy to relay audio and messages between the client and the service. But the new model is more efficient and provides higher throughput.

New `model` parameter for speech recognition
:   The `POST /v1/sessions` and `POST /v1/recognize` methods, along with the WebSocket `/v1/recognize` method, now support a `model` query parameter. You use the parameter to specify information about the audio:

    -   The language: *English*, *Japanese*, or *Spanish*
    -   The minimum sampling rate: *broadband* (16 kHz) or *narrowband* (8 kHz)

    For more information, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models).

New `inactivity_timeout` parameter for speech recognition
:   The `inactivity_timeout` parameter sets the timeout value in seconds after which the service closes the connection if it detects silence (no speech) in streaming mode. By default, the service terminates the session after 30 seconds of silence. The `POST /v1/recognize` and WebSocket `/v1/recognize` methods support the parameter. For more information, see [Timeouts](/docs/speech-to-text?topic=speech-to-text-input#timeouts).

New `max_alternatives` parameter for speech recognition
:   The `max_alternatives` parameter instructs the service to return the *n*-best alternative hypotheses for the audio transcription. The `POST /v1/recognize` and WebSocket `/v1/recognize` methods support the parameter. For more information, see [Maximum alternatives](/docs/speech-to-text?topic=speech-to-text-metadata#max-alternatives).

New `word_confidence` parameter for speech recognition
:   The `word_confidence` parameter instructs the service to return a confidence score for each word of the transcription. The `POST /v1/recognize` and WebSocket `/v1/recognize` methods support the parameter. For more information, see [Word confidence](/docs/speech-to-text?topic=speech-to-text-metadata#word-confidence).

New `timestamps` parameter for speech recognition
:   The `timestamps` parameter instructs the service to return the beginning and ending time relative to the start of the audio for each word of the transcription. The `POST /v1/recognize` and WebSocket `/v1/recognize` methods support the parameter. For more information, see [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps).

Renamed sessions method for observing results
:   The `GET /v1/sessions/{session_id}/observeResult` method is now named `GET /v1/sessions/{session_id}/observe_result`. The name `observeResult` is still supported for backward compatibility.

New support for Waveform Audio File (WAV) audio format
:   The `Content-Type` header of the `recognize` methods now supports `audio/wav` for Waveform Audio File (WAV) files, in addition to `audio/flac` and `audio/l16`. For more information, see [audio/wav format](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-wav).

Limits on maximum amount of audio for speech recognition
:   The service now has a limit of 100 MB of data per session in streaming mode. You can specify streaming mode by specifying the value `chunked` with the header `Transfer-Encoding`. One-shot delivery of an audio file still imposes a size limit of 4 MB on the data that is sent. For more information, see [Audio transmission](/docs/speech-to-text?topic=speech-to-text-input#transmission).

New header to opt out of contributing to service improvements
:   The `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize`, and `POST /v1/recognize` methods now include the header parameter `X-WDC-PL-OPT-OUT` to control whether the service uses the audio and transcription data from a request to improve future results. The WebSocket interface includes an equivalent query parameter. Specify a value of `1` to prevent the service from using the audio and transcription results. The parameter applies only to the current request. The new header replaces the `X-logging` header from the beta API. See [Controlling request logging for {{site.data.keyword.watson}} services](/docs/watson?topic=watson-gs-logging-overview).

Changes to HTTP error codes
:   The service can now respond with the following HTTP error codes:

    -   For the `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize`, and `/v1/recognize` methods, error code 415 ("Unsupported Media Type") is added.
    -   For `POST` and `GET` requests to the `/v1/sessions/{session_id}/recognize` method, the following error codes are modified:
        -   Error code 404 ("Session_id not found") has a more descriptive message (`POST` and `GET`).
        -   Error code 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") has a more descriptive message (`POST` only).
        -   For HTTP `POST` requests to the `/v1/sessions` and `/v1/recognize` methods, error code 503 ("Service Unavailable") can be returned. The error code can also be returned when you create a WebSocket connection with the `/v1/recognize` method.
