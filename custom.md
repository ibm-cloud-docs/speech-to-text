---

copyright:
  years: 2015, 2021
lastupdated: "2021-08-28"

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

The {{site.data.keyword.speechtotextfull}} service offers a customization interface that you can use to augment its speech recognition capabilities.  You can use customization to improve the accuracy of speech recognition requests by customizing a base model for your domain and audio.
{: shortdesc}

The customization interface supports both custom language models and custom acoustic models. The interfaces for both types of custom model are similar and straightforward to use. Using either type of custom model with a recognition request is also straightforward: You specify the customization ID of the model with the request.

Speech recognition works the same with or without a custom model. When you use a custom model for speech recognition, you can use all of the parameters that are normally available with a recognition request. For more information about all available parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only.** You must have the Plus, Standard, or Premium pricing plan to use language model or acoustic model customization. Users of the Lite plan cannot use the customization interface, but they can upgrade to the Plus plan to gain access to customization. For more information, see the [Pricing FAQs](/docs/speech-to-text?topic=speech-to-text-faq-pricing).
{: note}

## Language model customization
{: #customLanguage-intro}

Language model customization is available for both previous-generation and next-generation models. Customization works differently for previous- and next-generation models.
{: note}

The service was developed with a broad, general audience in mind. The service's base vocabulary contains many words that are used in everyday conversation. Its models provide sufficiently accurate recognition for many applications. But they can lack knowledge of specific terms that are associated with particular domains.

The *language model customization* interface can improve the accuracy of speech recognition for domains such as medicine, law, information technology, and others. By using language model customization, you can expand and tailor the vocabulary of a base model to include domain-specific terminology.

You create a custom language model and add corpora and words specific to your domain. Once you train the custom language model on your enhanced vocabulary, you can use it for customized speech recognition. The service can typically train any custom model in a matter of minutes. The time and effort that are needed to create a custom model depend on the data that you have available for the model.

For more information about getting started with language model customization, see

-   [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate)
-   [Using a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse)

## Acoustic model customization
{: #customAcoustic-intro}

Acoustic model customization is available only for previous-generation models.
{: note}

Similarly, the service was developed with base acoustic models that work well for various audio characteristics. But in cases like the following, adapting a base model to suit your audio can improve speech recognition:

-   Your acoustic channel environment is unique. For example, the environment is noisy, microphone quality or positioning are suboptimal, or the audio suffers from far-field effects.
-   Your speakers' speech patterns are atypical. For example, a speaker talks abnormally fast or the audio includes casual conversations.
-   Your speakers' accents are pronounced. For example, the audio includes speakers who are talking in a non-native or second language.

The *acoustic model customization* interface can adapt a base model to your environment and speakers. You create a custom acoustic model and add audio data (audio resources) that closely match the acoustic signature of the audio that you want to transcribe. Once you train the custom acoustic model with your audio resources, you can use it for customized speech recognition.

The length of time that it takes the service to train the custom model depends on how much audio data the model contains. In general, training takes twice the length of the cumulative audio. The time and effort that are needed to create a custom model depend on the audio data that you have available for the model. They also  on whether you use transcriptions of the audio.

For more information about getting started with acoustic model customization, see

-   [Creating a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic)
-   [Using a custom acoustic model for speech recognition](/docs/speech-to-text?topic=speech-to-text-acousticUse)

## Grammars
{: #grammars-intro}

Grammars are available as beta functionality and only for previous-generation models.
{: beta}

Custom language models allow you to expand the service's base vocabulary. *Grammars* enable you to restrict the words that the service can recognize from that vocabulary. When you use a grammar with a custom language model for speech recognition, the service can recognize only words, phrases, and strings that are recognized by the grammar. Because the grammar defines a limited search space for valid matches, the service can deliver results faster and more accurately.

You add a grammar to a custom language model and train the model just as you do for a corpus. Unlike a corpus, however, you must explicitly specify that a grammar is to be used with a custom model during speech recognition.

For more information about getting started with grammars, see

-   [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars)
-   [Understanding grammars](/docs/speech-to-text?topic=speech-to-text-grammarUnderstand)
-   [Adding a grammar to a custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd)
-   [Using a grammar for speech recognition](/docs/speech-to-text?topic=speech-to-text-grammarUse)

## Using acoustic and language customization together
{: #customization-combined}

Using acoustic and language customization together applies only to previous-generation models. Also, some previous-generation models do not support both language and acoustic customization.
{: note}

Using a custom acoustic model alone can improve the service's recognition capabilities. But if transcriptions or related corpora are available for your example audio, you can use that data to further improve the quality of speech recognition based on the custom acoustic model.

By creating a custom language model that complements your custom acoustic model, you can enhance speech recognition by using the two models together. When you train a custom acoustic model, you can specify a custom language model that includes transcriptions of the audio resources or a vocabulary of domain-specific words from the resources. Similarly, when you transcribe audio, the service accepts a custom language model, a custom acoustic model, or both. And if your custom language model includes a grammar, you can use that model and grammar with a custom acoustic model for speech recognition.

For more information, see [Using custom acoustic and custom language models together](/docs/speech-to-text?topic=speech-to-text-useBoth).

## Upgrading custom models
{: #upgrading-intro}

Upgrading custom models is necessary only for previous-generation models.
{: note}

To improve the quality of speech recognition, the service occasionally updates base models. Updates to individual base models do not affect other models. Base models for different languages are independent of each other, as are previous-generation broadband and narrowband models for a language, and next-generation multimedia and telephony models for a language.

-   *For previous-generation models,* when a new version of a base model is released, you must upgrade any custom language and custom acoustic models that are built on that base model to take advantage of the updates. Once you upgrade a custom model, the service uses the latest version of the custom model by default when you specify that model with a speech recognition request. But you can still direct the service to use the older version of the model.
-   *For next-generation models,* you do not need to upgrade any custom models that are built on that base model. New versions of base models are automatically and seamlessly used by custom models that are built on them.

For more information about upgrading custom models that are based on previous-generation models, see

-   [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade)
-   [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use)
