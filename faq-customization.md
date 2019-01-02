---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Customization
{: #customization}

1.  <span style="color:#003F69">What languages does the customization interface support?</span>

    Language model customization and acoustic model customization are available for different languages and at different levels of support (generally available or beta). For more information about the supported languages, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

1.  <span style="color:#003F69">How secure is the data that I add to a custom model?</span>

    A custom model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use service credentials that are created for that instance of the service. Credentials that are created for other instances of the service cannot view or access the custom model. For more information, see [Ownership of custom language models](/docs/services/speech-to-text/custom.html#customOwner).

1.  <span style="color:#003F69">What is the price for using the service's customization interface? Am I charged for creating and storing a custom model?</span>

    For *language model customization*, {{site.data.keyword.IBM_notm}} does not charge for creating or hosting a custom language model, only for using the model with a recognition request. Using a custom language model for transcription incurs an add-on charge of $0.03 (USD) per minute. This charge is in addition to the standard usage charge of $0.02 (USD) per minute. It applies to all languages supported by the customization interface. So the total charge for using a custom language model for speech recognition is $0.05 (USD) per minute.

    For *acoustic model customization*, which is a beta interface for all supported languages, {{site.data.keyword.IBM_notm}} does not charge for creating, hosting, or recognizing audio with a custom acoustic model. Free use of custom acoustic models for recognition requests is subject to change in the future.

1.  <span style="color:#003F69">How much effort is required to create and use a custom model?</span>

    Creating a *custom language model* requires you to add corpora, words, or grammars to the custom model. The service can typically train any custom model in a matter of minutes. The level of effort that it takes to create a model depends on the data that you have available for the model.

    Creating a *custom acoustic model* requires you to add audio resources (audio data) to the custom model. The length of time that it takes the service to train the custom model depends on how much audio data the model contains. In general, training takes twice the length of the cumulative audio. The level of effort that it takes to create a model depends on the audio data that you have available for the model. It also depends on whether you want to use transcriptions of the audio.

    The customization interfaces for both types of model are similar and straightforward to use. Using either type of custom model with a recognition request is also straightforward: You specify the customization ID of the model with the request.

## Language model customization
{: #lmCustomization}

1.  <span style="color:#003F69">Can I add my own specialized vocabulary to the service?</span>

    Yes. The service's base vocabulary contains many words that are used in everyday conversation by a broad, general audience. Its models provide sufficiently accurate recognition for many applications but can lack knowledge of specific terms that are associated with particular domains. Such words are referred to as *out-of-vocabulary (OOV) words*.

    The language model customization interface can improve the accuracy of speech recognition for specialized domains. You can use customization to expand and tailor the vocabulary of a base model to include domain-specific terminology. For more information, see [Creating a custom language model](/docs/services/speech-to-text/language-create.html) and [Using a custom language model](/docs/services/speech-to-text/language-use.html).

1.  <span style="color:#003F69">How much data is needed to build a custom language model?</span>

    It is not possible to provide an exact figure, since many factors contribute to the answer. Adding OOV words in context via a corpus and adding custom words directly can both improve the quality of a custom language model. When you add OOV words from corpora, a corpus that uses the words in the context in which they are used in audio can improve transcription accuracy. But depending on the use case, even adding a few custom words directly to a model can make a positive difference.

1.  <span style="color:#003F69">Can corpora that I add to a custom language model repeat words and sentences?</span>

    Yes. Repeating the OOV words in corpora can improve the quality of the custom language model. How you duplicate the words in corpora depends on how you expect users to say them in the audio that is to be recognized.

    In general, it is better for the corpora to use the OOV words in different contexts and phrases, which improves how the service learns the words. However, if users speak the words in only a couple of contexts, then showing the words in other contexts does not improve the quality of the custom model. Speakers never use the words in those contexts. If speakers are likely to use the same phrase frequently, then repeating that phrase in the corpora can improve the quality of the model.

1.  <span style="color:#003F69">A number of methods for language model customization are asynchronous. How do I check their results to know when they are finished?</span>

    The following sections describe how to check the results of asynchronous operations for language model customization:
    -   To check the status of a request to add a corpus to a custom language model with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method, see [Monitoring the add corpus request](/docs/services/speech-to-text/language-create.html#monitorCorpus).
    -   To check the status of a request to add words to a custom language model with the `POST /v1/customizations/{customization_id}/words` method, see [Monitoring the add words request](/docs/services/speech-to-text/language-create.html#monitorWords).
    -   To check the status of a request to add a grammar to a custom language model with the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method, see [Monitoring the add grammar request](/docs/services/speech-to-text/grammar-add.html#monitorGrammar).
    -   To check the status of a request to train a custom language model with the `POST /v1/customizations/{customization_id}/train` method, see [Monitoring the train model request](/docs/services/speech-to-text/language-create.html#monitorTraining).

1.  <span style="color:#003F69">I created a custom language model, but the service does not appear to be using any of the new words that it contains during recognition?</span>

    Try the following steps:

    -   Make sure that you are correctly passing the customization ID to the recognition request. For more information, see [Using a custom language model](/docs/services/speech-to-text/language-use.html).
    -   Make sure that the status of the custom model is `available`, meaning that it is fully trained and ready to use. For more information, see [Listing custom language models](/docs/services/speech-to-text/language-models.html#listModels).
    -   Check the pronunciations that were generated for the new words to make sure that they are correct. For more information, see [Validating a words resource](/docs/services/speech-to-text/language-resource.html#validateModel).

## Acoustic model customization
{: #amCustomization}

1.  <span style="color:#003F69">Can I adapt the service for the acoustic characteristics of my environment and speakers?</span>

    Yes. The service was developed with a base acoustic model that works well with various audio characteristics. But adapting a base model can improve speech recognition in some cases, such as a unique or noisy environment, atypical speech patterns, or speaker accents.

    The acoustic model customization interface can improve the accuracy of speech recognition for different environments and speakers. For more information, see [Creating a custom acoustic model](/docs/services/speech-to-text/acoustic-create.html) and [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html).

1.  <span style="color:#003F69">How much audio data is needed to build a custom acoustic model?</span>

    You must add at least 10 minutes and no more than 100 hours of audio to a custom acoustic model. The quality of the audio makes a difference when you are determining how much to add. The better the model's audio reflects the characteristics of the audio that is to be recognized, the better the quality of the custom model. If the audio is of good quality, adding more audio can improve transcription accuracy. As a rough guideline, adding five to ten hours of audio can make a positive difference.

1.  <span style="color:#003F69">Do I need to transcribe the audio data that I add to a custom acoustic model?</span>

    Transcribing the audio data is not strictly necessary. But if you have transcriptions of the audio, or even a list of the OOV words that are used in the audio, they can improve the quality of the custom acoustic model. Transcriptions are especially valuable if the audio contains many OOV words.

    To use a transcription or list of words, you first create a custom language model with the textual data. You can then use both the custom language model and the custom acoustic model together:

    -   You can train the custom acoustic model with the custom language model. For more information, see [Training a custom acoustic model with a custom language model](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).
    -   You can use the custom language model with the custom acoustic model in speech recognition requests. For more information, see [Using custom language and custom acoustic models during speech recognition](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

   Both of these approaches can improve the accuracy of speech recognition with a custom acoustic model.

1.  <span style="color:#003F69">What kind of improvement in recognition accuracy can I expect from using a custom acoustic model?</span>

    The answer depends on a number of factors, such as how much audio data the custom acoustic model contains and how similar that data is to the audio that is being transcribed. The answer also depends on whether the custom acoustic model is trained with a corresponding custom language model. Using audio data alone is referred to as *unsupervised training*. Using a corresponding custom language model during training is referred to as *lightly supervised training*.

    Using a custom language model to train a custom acoustic model is effective only if

    -   The custom language model was built with direct transcriptions of the audio data.
    -   The custom language model contains words from the same domain as the audio data.

    If the audio contains many OOV words, it is better to use a custom language model during training, even if the custom language model merely adds a list of custom words. In general, a good custom acoustic model can improve the accuracy of transcription by as much as 40 percent.

1.  <span style="color:#003F69">A number of methods for acoustic model customization are asynchronous. How do I check their results to know when they are finished?</span>

    The following sections describe how to check the results of asynchronous operations for acoustic model customization:
    -   To check the status of a request to add audio resources to a custom acoustic model with the `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method, see [Monitoring the add audio request](/docs/services/speech-to-text/acoustic-create.html#monitorAudio).
    -   To check the status of a request to train a custom acoustic model with the `POST /v1/acoustic_customizations/{customization_id}/train` method, see [Monitoring the train model request](/docs/services/speech-to-text/acoustic-create.html#monitorTraining).
