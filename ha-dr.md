---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-04"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
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

# High availability and disaster recovery
{: #ha-dr}

The {{site.data.keyword.speechtotextfull}} service is highly available within any {{site.data.keyword.cloud_notm}} location (for example, Dallas or Washington, DC). However, recovering from potential disasters that affect an entire location requires planning and preparation.
{: shortdesc}

You are responsible for understanding your configuration, customization, and usage of the service. You are also responsible for being ready to re-create an instance of the service in a new location and to restore your data in any location.

## High availability
{: #high-availability}

The {{site.data.keyword.speechtotextshort}} service supports high availability with no single point of failure. The service achieves high availability automatically and transparently by means of the multi-zone region (MZR) feature provided by {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.cloud_notm}} enables multiple zones that do not share a single point of failure within a single location. It also provides automatic load balancing across the zones within a region.

## Disaster recovery
{: #disaster-recovery}

Disaster recovery can become an issue if an {{site.data.keyword.cloud_notm}} location experiences a significant failure that includes the potential loss of data. Because MZR is not available across locations, you must wait for {{site.data.keyword.IBM_notm}} to bring a location back online if it becomes unavailable. If underlying data services are compromised by the failure, you must also wait for {{site.data.keyword.IBM_notm}} to restore those data services.

In the event of a catastrophic failure, {{site.data.keyword.IBM_notm}} might not be able to recover data from database backups. In this case, you need to restore your data to return your service instance to its most recent state. You can restore the data to the same or to a different location.

Your disaster recovery plan includes knowing, preserving, and being prepared to restore all data that is maintained on {{site.data.keyword.cloud_notm}}. This includes data from custom language models, custom acoustic models, and asynchronous speech recognition requests.

Re-creating custom models, especially custom acoustic models, from saved data can take a significant amount of time. Maintaining parallel service configurations in multiple locations can eliminate the turnaround time associated with disaster recovery.
{: note}

### Disaster recovery for custom language models
{: #disaster-recovery-language}

For custom language models, you need to maintain and be prepared to re-create and restore your custom language models and their corpora, grammars, and custom words. You also need to be prepared to retrain your custom language models on their data and words resources.

#### Backing up custom language models
{: #disaster-recovery-language-backup}

Preserve the following information about your custom language models:

-   A list of all of your custom language models and their definitions. To list information about your custom models:
    -   Use the `GET /v1/customizations` method to list information about all custom models.
    -   Use the `GET /v1/customizations/{customization_id}` method to list information about a specified custom model.

    For more information, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Copies of all corpus text files that you add to your custom language models. To list information about the corpora for your custom models:
    -   Use the `GET /v1/customizations/{customization_id}/corpora` method to list all corpora for a custom model.
    -   Use the `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method to list information about a specified corpus for a custom model.

    For more information, see [Listing corpora for a custom language model](/docs/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).
-   Copies of all grammar files that you add to your custom language models. To list information about the grammars for your custom models:
    -   Use the `GET /v1/customizations/{customization_id}/grammars` method to list information about all grammars for a custom model.
    -   Use the `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` method to list information about a specified grammar for a custom model.

    For more information, see [Listing grammars for a custom language model](/docs/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars).
-   Information about all custom words, including their sounds-like and display-as definitions, that you add directly to your custom language models. To list information about the out-of-vocabulary (OOV) words for your custom models:
    -   Use the `GET /v1/customizations/{customization_id}/words` method to list information about the words from a custom model. You can use the `word_type` parameter to list `all` words from a model, words added directly by the `user`, words extracted from `corpora`, or words recognized by `grammars`.
    -   Use the `GET /v1/customizations/{customization_id}/words/{word_name}` method to list information about a specified word from a custom model.

    For more information, see [Listing words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

It is a best practice to preserve this information in a format that you can use to re-create your custom language models in the event of a failure. Actively maintaining the information about your custom models and their data, and preparing the calls listed in the following section ahead of time, can enable you to recover as quickly as possible.

#### Restoring custom language models
{: #disaster-recovery-language-restore}

If you need to recover from a disaster, you can use the backup information to re-create your custom language models and their data:

1.  To re-create your custom language models, use the `POST /v1/customizations` method. For more information, see [Create a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).
1.  To add the corpus text files to your custom models, use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method. For more information, see [Add a corpus to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addCorpus).
1.  To add the grammar files to your custom models, use the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method. For more information, see [Add a grammar to the custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar).
1.  To add multiple words to your custom models, use the `POST /v1/customizations/{customization_id}/words` method. To add single words to your custom models, use the `PUT /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Add words to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addWords).
1.  To train your custom models once you restore your corpora, grammars, and custom words, use the `POST /v1/customizations/{customization_id}/train` method. For more information, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

The methods that you use to add corpora, grammars, and words and to train a custom language model are asynchronous. You need to monitor the requests until they complete.

You can incrementally add data and train your custom language models rather than adding all data before training the models. For example, you can add your corpora and then train your models before you add grammars and individual words. You can also incrementally add and train your models on individual corpus text files, grammar files, and groups of custom words.

### Disaster recovery for custom acoustic models
{: #disaster-recovery-acoustic}

For custom acoustic models, you need to maintain and be prepared to re-create and restore your custom acoustic models and their audio resources. You also need to be prepared to retrain your custom acoustic models on their audio resources.

#### Backing up custom acoustic models
{: #disaster-recovery-acoustic-backup}

Preserve the following information about your custom acoustic models:

-   A list of all of your custom acoustic models and their definitions. To list information about your custom models:
    -   Use the `GET /v1/acoustic_customizations` method to list information about all custom models.
    -   Use the `GET /v1/acoustic_customizations/{customization_id}` method to list information about a specified custom model.

    For more information, see [Listing custom acoustic models](/docs/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).
-   Copies of all audio resources, both individual audio files and archive files, that you add to your custom acoustic models. To list information about the audio resources for your custom models:
    -   Use the `GET /v1/acoustic_customizations/{customization_id}/audio` method to list information about all audio resources for a custom model.
    -   Use the `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method to list information about a specified audio resource for a custom model.

    For more information, see [Listing audio resources for a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-manageAudio#listAudio).

It is a best practice to preserve this information in a format that you can use to re-create your custom acoustic models in the event of a failure. Actively maintaining the information about your custom models and their audio resources, and preparing the calls listed in the following section ahead of time, can enable you to recover as quickly as possible.

#### Restoring custom acoustic models
{: #disaster-recovery-acoustic-restore}

If you need to recover from a disaster, you can use the backup information to re-create your custom acoustic models and their data:

1.  To re-create your custom acoustic models, use the `POST /v1/acoustic_customizations` method. For more information, see [Create a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic).
1.  To add the audio resources to your custom models, use the `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` method. For more information, see [Add audio to the custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#addAudio).
1.  To train your custom models once you restore your audio resources, use the `POST /v1/acoustic_customizations/{customization_id}/train` method. For more information, see [Train the custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic).

The methods that you use to add audio resources and to train a custom acoustic model are asynchronous. You need to monitor the requests until they complete.

You can incrementally add audio resources and train your custom acoustic models rather than adding all data before training the models. For example, you can add your audio resources one at a time or in groups, training your models on subsets of the audio resources rather than on all of the audio at one time.

### Disaster recovery for asynchronous speech recognition jobs
{: #disaster-recovery-async}

For speech recognition with the asynchronous HTTP interface, you need to maintain the following information:

-   All callback URLs that you whitelist for use with the asynchronous interface. If a failure occurs, you might need to use the `POST /v1/register_callback` method to re-register the URLs. The method returns an appropriate response if a URL is already whitelisted.
-   Copies of the audio files that you submit to the asynchronous interface for speech recognition. If a failure occurs before you receive or retrieve the results of a completed asynchronous job, you need to use the `POST /v1/recognitions` method to resubmit the audio files when the service is restored. Once you have the results of a completed asynchronous job, you no longer need to maintain the audio files.

For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async). As with the backup data for custom models, you can actively preserve this information and be prepared to reissue the necessary requests ahead of time.
