---

copyright:
  years: 2019
lastupdated: "2019-10-04"

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

You can improve speech recognition accuracy by using complementary custom language and custom acoustic models. You can use both types of model during training of your acoustic model and during speech recognition. Both models must be owned by the same service instance, and both must be based on the same base language model.
{: shortdesc}

Using a custom acoustic model alone or with a custom language model on which it was not trained can still be useful. If the custom acoustic model was trained on acoustic characteristics that match the audio that is being transcribed, it can still improve transcription quality.

## Training a custom acoustic model with a custom language model
{: #useBothTrain}

Training a custom acoustic model with audio data alone is referred to as *unsupervised training*. Using a custom language model during training is referred to as *lightly supervised training*. Lightly supervised training can improve the effectiveness of your custom acoustic model.

Use lightly supervised training to train a custom acoustic model with a custom language model in the following cases:

-   The custom language model is based on transcriptions (verbatim text) from the audio files that you added to the custom acoustic model.

    Because transcriptions contain the exact contents of the audio, training with a custom language model that is based on transcriptions can produce the best results. The service can parse the contents of the transcriptions in context and extract OOV words and n-grams that can help it make the most effective use of your audio. This is especially true if your audio data is less than an hour long.

    Transcribing the audio data is not strictly necessary. But if you have transcriptions of the audio, they can improve the quality of the custom acoustic model. Transcriptions are especially valuable if the audio contains many OOV words.
-   The custom language model is based on corpora (text files) or a list of words that are relevant to the contents of the audio files.

    If your audio contains domain-specific words that are not found in the service's base vocabulary, acoustic model customization alone does not produce those words during transcription. Language model customization is the only way to expand the service's base vocabulary. If you don't have transcriptions, train with a custom language model that includes OOV words from the same domain as your audio data. Even training with a custom language model that includes a list of the OOV words that are used in the audio can prove helpful.

    For example, suppose that you are creating a custom acoustic model that is based on call-center audio for a specific product. You can train the custom acoustic model with a custom language model that is based on transcriptions of related calls or that includes names of specific products that are handled by the call center.

To use a transcription or list of words, you first create a custom language model that contains this textual data. To train a custom acoustic model with a custom language model, both custom models must be based on the same version of the same base model. If a new version of the base model is made available, you must upgrade both models to the same version of the base model for training to succeed.

Use the optional `custom_language_model_id` query parameter of the `POST /v1/acoustic_customizations/{customization_id}/train` method to train your custom acoustic model with a custom language model. Pass the GUID of the acoustic model with the `customization_id` parameter and the GUID of the custom language model with the `custom_language_model_id` parameter. Both models must be owned by the credentials that are passed with the request.

```bash
curl -X POST -u "apikey:{apikey}"
"{url}/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Using custom language and custom acoustic models during speech recognition
{: #useBothRecognize}

You can specify both a custom language model and a custom acoustic model with any recognition request. If a custom language model contains OOV words from the domain of the audio that is being recognized, you can use it with a custom acoustic model during speech recognition to improve transcription accuracy.

Using a custom language model can improve transcription accuracy regardless of whether you trained the custom acoustic model with the custom language model:

-   Using both custom language and custom acoustic models during training improves the quality of the custom acoustic model.
-   Using both types of model during speech recognition improves the quality of transcription.

If a custom language model includes grammars, you can also use the custom language model and one of its grammars with a custom acoustic model during speech recognition.

The following example passes both types of model to the HTTP `POST /v1/recognize` method. Pass the GUID of the custom acoustic model with the `acoustic_customization_id` parameter and the GUID of the custom language model with the `language_customization_id` parameter. Both models must be owned by the credentials that are passed with the request, and both must be based on the same base model (for example, `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"{url}/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

For an asynchronous HTTP request, you specify the parameters when you create the asynchronous job. For WebSockets, you pass the parameters when you establish the connection.
