---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-27"

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

# Using custom acoustic and custom language models together
{: #useBoth}

You can improve speech recognition accuracy by using complementary custom language and custom acoustic models. You can use both types of model during training of your acoustic model, during speech recognition, or both. The custom language and custom acoustic models must be owned by the same service instance, and both must customize the same base language model.
{: shortdesc}

## Training a custom acoustic model with a custom language model
{: #useBothTrain}

The following terms differentiate how a custom acoustic model is trained, alone or with a custom language model:

-   *Unsupervised training* refers to training a custom acoustic model with audio data alone. Training a custom acoustic model on audio alone can improve transcription quality if the characteristics of the custom model's audio match those of the audio that is being transcribed.
-   *Lightly supervised training* refers to training a custom acoustic model with a complementary custom language model. Lightly supervised training is more effective than unsupervised training in improving the quality of speech recognition.

Use lightly supervised training in the following cases:

-   When you have a custom language model that contains transcripts of your audio files.

    Transcribing your audio data is not strictly necessary. But training with a custom language model whose corpora are based on transcripts of your audio files can improve speech recognition. This is especially true if the audio data contains out-of-vocabulary (OOV) words that are not found in the service's base vocabulary. The service can parse the contents of the transcriptions in context, extracting OOV words and n-grams that can help it make the most effective use of your audio data.

-   When you have a custom language model that is based on corpora or words that are relevant to the contents of your audio files.

    If you don't have transcripts of your audio files, you can train with a custom language model that includes OOV words from the same domain as your audio data. Training with a custom language model that includes OOV words that are used in the audio can improve speech recognition.

For example, suppose you are creating a custom acoustic model that is based on call-center audio for specific products. You can train the custom acoustic model with a custom language model that contains transcripts of related calls, or you can train with a custom language model that includes just names of specific products that are handled by the call center.

### Guidelines for using lightly supervised training
{: #useBothTrainGuidelines}

Follow these guidelines when you have transcripts of the audio files from the custom acoustic acoustic model:

-   Add a transcript to a custom language model as a corpus. The custom language model can include other corpora and words, including transcripts of other audio files. It does not need to be dedicated to a single transcript or to transcripts alone. It is not strictly necessary for the corpus to include only one sentence per line. But as with all corpora, the service can make better use of a corpus that includes each sentence on its own line.
-   It is neither necessary nor possible to associate a specific corpora with a specific audio file. A custom language model can contain multiple corpora, just as a custom acoustic model can include multiple audio files. All contents of a custom language model can help improve the internal transcript of the audio that the service generates during the training process.
-   A transcript does not need to be a verbatim match of all sentences and words from the audio. It can include only sentences and words of the audio that are relevant to the domain. For both language model and acoustic model customization, the training data needs to reflect the actual use-case of the speech that you want to recognize. If only 20 percent of the data (transcript or audio) is specific to the domain of the use-case, use just that 20 percent of the data for training.

    However, if you are using acoustic model customization to achieve better results for accented speech (for example, for speech by non-native speakers), keep as much audio as possible, even if it's not relevant to the domain. The same is true if you are using acoustic model customization to improve speech recognition accuracy under difficult acoustic conditions, such as a noisy background.
-   A verbatim transcript does not need to contain the disfluencies, verbal tics, and filler statements that are common to human speech. You can remove these elements from the transcript, the audio, or both. (You can also remove audio in which people are talking over each other, since that does not contribute to the training.)

    For example, suppose a verbatim transcript includes the following sentence:

    `So that's, uhm, you know, that, as I say, is the is the predominant form today.`

    You can edit these peculiarities from the transcript, the audio, or both to create the following sentence:

    `So that as I say is the predominant form today.`

### Performing lightly supervised training
{: #useBothTrainPerform}

To train a custom acoustic model with a custom language model, you must

-   Ensure that the custom language model is fully trained and in the `available` state. Training fails if the custom language model is not `available`.
-   Ensure that both custom models are based on the same version of the same base model. If a new version of the base model is made available, you must upgrade both models to the same version of the base model for training to succeed. For more information, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-customUpgrade).

Use the optional `custom_language_model_id` query parameter of the `POST /v1/acoustic_customizations/{customization_id}/train` method to train your custom acoustic model with a custom language model. Pass the GUID of the acoustic model with the `customization_id` parameter and the GUID of the custom language model with the `custom_language_model_id` parameter. Both models must be owned by the credentials that are passed with the request.

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Using custom language and custom acoustic models during speech recognition
{: #useBothRecognize}

You can specify both a custom language model and a custom acoustic model with any speech recognition request. If your audio contains domain-specific OOV words, acoustic model customization alone cannot reliably produce those words during speech recognition. Using a custom language model that contains OOV words from the domain of the audio is the only way to expand the service's base vocabulary.

Using a custom language model can improve transcription accuracy regardless of whether you trained the custom acoustic model with the custom language model:

-   Using both custom language and custom acoustic models during training improves the quality of the custom acoustic model.
-   Using both types of model during speech recognition improves transcription quality.

If a custom language model includes grammars, you can also use the custom language model and one of its grammars with a custom acoustic model during speech recognition.

The following example passes both types of model to the HTTP `POST /v1/recognize` method. Pass the GUID of the custom acoustic model with the `acoustic_customization_id` parameter and the GUID of the custom language model with the `language_customization_id` parameter. Both models must be owned by the credentials that are passed with the request, and both must be based on the same base model (for example, `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @audio-file1.flac \
"{url}/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

For an asynchronous HTTP request, you specify the parameters when you create the asynchronous job. For a WebSocket request, you pass the parameters when you establish a connection. For more information, see [Using a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acousticUse).
