---

copyright:
  years: 2015, 2021
lastupdated: "2021-06-09"

subcollection: speech-to-text

---

{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
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

# Understanding customization
{: #customization}

The {{site.data.keyword.speechtotextfull}} service offers a customization interface that you can use to augment its speech recognition capabilities.  You can use customization to improve the accuracy of speech recognition requests by customizing a base model for your domain and audio. Customization is available for only some languages and at different levels of support for different languages; see [Language support for customization](#languageSupport).
{: shortdesc}

The customization interface supports both custom language models and custom acoustic models. The interfaces for both types of custom model are similar and straightforward to use. Using either type of custom model with a recognition request is also straightforward: You specify the customization ID of the model with the request.

Speech recognition works the same with or without a custom model. When you use a custom model for speech recognition, you can use all of the parameters that are normally available with a recognition request. For more information about all available parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** You must have the Plus, Standard, or Premium pricing plan to use language model or acoustic model customization. Users of the Lite plan cannot use the customization interface, but they can upgrade to the Plus plan to gain access to customization. For more information, see the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).
{: note}

## Language model customization
{: #customLanguage-intro}

The service was developed with a broad, general audience in mind. The service's base vocabulary contains many words that are used in everyday conversation. Its models provide sufficiently accurate recognition for many applications. But they can lack knowledge of specific terms that are associated with particular domains.

The *language model customization* interface can improve the accuracy of speech recognition for domains such as medicine, law, information technology, and others. By using language model customization, you can expand and tailor the vocabulary of a base model to include domain-specific terminology.

You create a custom language model and add corpora and words specific to your domain. Once you train the custom language model on your enhanced vocabulary, you can use it for customized speech recognition. The service can typically train any custom model in a matter of minutes. The level of effort that it takes to create a model depends on the data that you have available for the model.

For more information, see

-   [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate)
-   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)

## Acoustic model customization
{: #customAcoustic-intro}

Similarly, the service was developed with base acoustic models that work well for various audio characteristics. But in cases like the following, adapting a base model to suit your audio can improve speech recognition:

-   Your acoustic channel environment is unique. For example, the environment is noisy, microphone quality or positioning are suboptimal, or the audio suffers from far-field effects.
-   Your speakers' speech patterns are atypical. For example, a speaker talks abnormally fast or the audio includes casual conversations.
-   Your speakers' accents are pronounced. For example, the audio includes speakers who are talking in a non-native or second language.

The *acoustic model customization* interface can adapt a base model to your environment and speakers. You create a custom acoustic model and add audio data (audio resources) that closely match the acoustic signature of the audio that you want to transcribe. Once you train the custom acoustic model with your audio resources, you can use it for customized speech recognition.

The length of time that it takes the service to train the custom model depends on how much audio data the model contains. In general, training takes twice the length of the cumulative audio. The level of effort that it takes to create a model depends on the audio data that you have available for the model. It also depends on whether you use transcriptions of the audio.

For more information, see

-   [Creating a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic)
-   [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse)

## Grammars
{: #grammars-intro}

Custom language models allow you to expand the service's base vocabulary. *Grammars* enable you to restrict the words that the service can recognize from that vocabulary. When you use a grammar with a custom language model for speech recognition, the service can recognize only words, phrases, and strings that are recognized by the grammar. Because the grammar defines a limited search space for valid matches, the service can deliver results faster and more accurately.

You add a grammar to a custom language model and train the model just as you do for a corpus. Unlike a corpus, however, you must explicitly specify that a grammar is to be used with a custom model during speech recognition.

For more information, see

-   [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars)
-   [Adding a grammar to a custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd)
-   [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse)

The grammars feature is beta functionality. The service supports grammars for all languages for which it supports language model customization.
{: beta}

## Using acoustic and language customization together
{: #combined}

Using a custom acoustic model alone can improve the service's recognition capabilities. But if transcriptions or related corpora are available for your example audio, you can use that data to further improve the quality of speech recognition based on the custom acoustic model.

By creating a custom language model that complements your custom acoustic model, you can enhance speech recognition by using the two models together. When you train a custom acoustic model, you can specify a custom language model that includes transcriptions of the audio resources or a vocabulary of domain-specific words from the resources. Similarly, when you transcribe audio, the service accepts a custom language model, a custom acoustic model, or both. And if your custom language model includes a grammar, you can use that model and grammar with a custom acoustic model for speech recognition.

For more information, see [Using custom acoustic and custom language models together](/docs/speech-to-text?topic=speech-to-text-useBoth).

Some languages do not support both language and acoustic customization.
{: note}

## Language support for customization
{: #languageSupport}
{: help}
{: support}

Language and acoustic model customization are available only for some languages. The following table documents the level at which the service supports customization for each language.

-   *GA* indicates that the interface is generally available for production use for that language.
-   *Beta* indicates that the interface is available as a beta offering for that language.
-   *Not supported* means that the interface is not available for that language.

You can use both broadband and narrowband models with any supported language for which they are available. If a language supports language model customization, it also supports grammars. For a list of all available models, see [Supported language models](/docs/speech-to-text?topic=speech-to-text-models#models-supported).

The beta next-generation models do *not* support customization at this time.
{: note}

| Language | Support for language model customization | Support for acoustic model customization |
|----------|:----------------------------------------:|:----------------------------------------:|
| Arabic (Modern Standard) | Not supported | GA |
| Chinese (Mandarin) | Not supported | GA |
| Dutch (Netherlands) | GA | GA |
| English (Australian) | GA | GA |
| English (United Kingdom) | GA | GA |
| English (United States) | GA | GA |
| French (Canadian) | GA | GA |
| French (France) | GA | GA |
| German | GA | GA |
| Italian | GA | GA |
| Japanese | GA | GA |
| Korean | GA | GA |
| Portuguese (Brazilian) | GA | GA |
| Spanish (Argentinian) | Beta | Beta |
| Spanish (Castilian) | GA | GA |
| Spanish (Chilean) | Beta | Beta |
| Spanish (Colombian) | Beta | Beta |
| Spanish (Mexican) | Beta | Beta |
| Spanish (Peruvian) | Beta | Beta |
{: caption="Table 1. Language support for customization"}

You can use the `GET /v1/models` and `GET /v1/models/{model_id}` methods to check whether a language model supports language model customization. If the base model supports language model customization, the `supported_features` field of the methods' output for the model sets the `custom_language_model` field to `true`.

## Usage notes for customization
{: #customUsage}

The following usage notes apply to both language model customization and acoustic model customization.

### Ownership of custom models
{: #customOwner}

A custom model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use credentials for that instance of the service with methods of the customization interface. Credentials that are created for other instances of the service cannot view or access the custom model.

All credentials that are obtained for the same instance of the {{site.data.keyword.speechtotextshort}} service share access to all custom models created for that service instance. To restrict access to a custom model, create a separate instance of the service and use only the credentials for that service instance to create and work with the model. Credentials for other service instances cannot affect the custom model.

An advantage of sharing ownership across credentials for a service instance is that you can cancel a set of credentials, for example, if they become compromised. You can then create new credentials for the same service instance and still maintain ownership of and access to custom models created with the original credentials.

### Maximum number of custom models
{: #customMaximum}

You can create no more than 1024 custom acoustic models and no more than 1024 custom language models per owning credentials. If you try to create more than 1024 custom models of either type, the service returns an error. You do not lose any existing models, but you cannot create any more until your model count is below the limit of 1024.

### Information security
{: #customSecurity}

You can associate a customer ID with data that is added or updated for custom language and custom acoustic models. You associate a customer ID with corpora, custom words, grammars, and audio resources by passing the `X-Watson-Metadata` header with the following methods. If necessary, you can then delete the data by using the `DELETE /v1/user_data` method.

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

In addition, if you delete an instance of the {{site.data.keyword.speechtotextshort}} service from the {{site.data.keyword.cloud_notm}} console, all data associated with that service instance is automatically deleted. This includes all custom language models, corpora, grammars, and words, and all custom acoustic models and audio resources. This data is purged automatically and regardless of whether a customer ID is associated with the data.

For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

### Request logging and data privacy
{: #customLogging}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only**

How the service handles request logging for calls to the customization interface depends on the request:

-   The service *does not* log data that is used to build custom models. For example, when working with corpora and words in a custom language model, you do not need to set the `X-Watson-Learning-Opt-Out` request header. Your training data is never used to improve the service's base models.
-   The service *does* log data when a custom model is used with a recognition request. You must set the `X-Watson-Learning-Opt-Out` request header to `true` to prevent logging for recognition requests.

For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging).
