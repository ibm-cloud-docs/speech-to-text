---

copyright:
  years: 2022, 2023
lastupdated: "2024-05-02"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Migrating to large speech models
{: #models-migrate}

Starting **August 1, 2023**, all previous-generation models are now **discontinued** from the service. New clients must now only use the large speech models or next-generation models. All existing clients must now migrate to the equivalent large speech model or next-generation model. For more information, see [Migrating to large speech models](/docs/speech-to-text?topic=speech-to-text-models-migrate).
{: attention}

You must migrate your use of any deprecated previous-generation models to the equivalent large speech models or next-generation models by the 31 July 2023 end of service date. Next-generation models provide appreciably better transcription accuracy and throughput. But they currently provide slightly fewer features than previous-generation models.

This topic provides an overview of the steps that you need to take to migrate from previous- to next-generation models. For more information about migrating, you can also see [Watson Speech to Text: How to Plan Your Migration to the Next-Generation Models](https://medium.com/ibm-data-ai/watson-speech-to-text-how-to-plan-your-migration-to-the-next-generation-models-6b10605b3bc5){: external}.

## Step 1: Identify the large speech model or next-generation model to which to migrate
{: #models-migrate-step1}

The following topics describe all large speech models, previous- and next-generation models:
-   [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
-   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)
-   [Large speech languages and models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages)

The tables in [Supported previous-generation language models](/docs/speech-to-text?topic=speech-to-text-models#model-pg-supported) list the recommended large speech model or next-generation model to which to migrate from a previous generation model. Use the indicated large speech model or next-generation model in your speech recognition requests.

The service continues to make new large speech models available. All new models are identified in the release notes and in the tables that describe the available models.

Optimally, you migrate from a narrowband model / telephony model to a large speech model, and from a broadband model / multimedia model to a large speech model. However, not all broadband models and multimedia models have equivalent large speech models. In such cases, you can migrate from a narrowband model to a telephony model, or from a broadband model to a multimedia model. The service downsamples the audio that you send to the rate of the model that you use. So sending broadband audio to a telephony model might prove a sufficient alternative in cases where no equivalent multimedia model is currently available.

For example, the following speech recognition request uses the previous-generation `en-US_NarrowbandModel`:

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

To use the equivalent large speech model `en-US` , you simply change the value that you pass with the `model` query parameter:

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US"
```
{: pre}

## Step 2: Identify the features that are available with large speech models
{: #models-migrate-step2}

Large speech models support slightly fewer features and parameters than previous- and next-generation models. However, although they lack full parity, most features are available with both types of models. And where a feature is limited to a subset of languages, the limitations apply equally to all types of models.

For information about the features that are supported with the different model types, see
-   [Supported features for previous-generation models](/docs/speech-to-text?topic=speech-to-text-models#models-features)
-   [Supported features for next-generation models](/docs/speech-to-text?topic=speech-to-text-models-ng#models-ng-features)
-   [Supported features for large speech models](/docs/speech-to-text?topic=speech-to-text-models-large-speech-languages#models-lsm-supported-features)
-   [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary)

The service continues to make new features available with next-generation models. All updates to feature support are documented in the release notes and in the documentation for the model types.

To migrate to next-generation models, you must remove features that aren't supported by next-generation models from your speech recognition requests. You can also consider using features such as character insertion bias that are available only with large speech models and next-generation models.

For example, the following speech recognition request uses the `profanity_filter`, `redaction`, and `word_alternatives_threshold` parameters with the previous-generation `en-US_NarrowbandModel`:

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path_to_file}audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel&profanity_filter=true&redaction=true&word_alternatives_threshold=0.50"
```
{: pre}

Only the `word_alternatives_threshold` parameter is not supported by large speech models. To use the equivalent large speech model `en-US_Telephony` model, you simply change the value that you pass with the `model` query parameter and eliminate the `word_alternatives_threshold` parameter:

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path_to_file}audio-file.flac \
"{url}/v1/recognize?model=en-US&profanity_filter=true&redaction=true"
```
{: pre}

## Step 3: Re-create any custom language models that you use
{: #models-migrate-step3}

You must re-create any custom language models that are based on previous-generation or next-generation models by basing them on the equivalent large speech models. This requires that you create a new custom language model and add your corpora, and custom words from the old model to the new model.

In general, large speech models do not rely as heavily on custom language models. They use a different approach to transcription that minimizes the need for language model customization.

Large speech models do not support custom acoustic models. Because of how the models transcribe audio, acoustic model customization is not necessary.
{: note}

For example, the following speech recognition request uses a custom language model that is based on the `en-US_NarrowbandModel`. In this example, the custom model has the identifier `8acf31fa-0aa2-4ecc-a805-1f527f342dba`.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @audio-file.flac \
"{url}/v1/recognize?model=en-US_NarrowbandModel&language_customization_id=8acf31fa-0aa2-4ecc-a805-1f527f342dba"
```
{: pre}

After you re-create the custom language model with the equivalent `en-US` model, simply update the model name to `en-US` and the `language_customization_ID` parameter to use the identifier of the new custom model, `636d8494-7e53-436a-8557-30d6b2a63cd7`:

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @audio-file.flac \
"{url}/v1/recognize?model=en-US&language_customization_id=636d8494-7e53-436a-8557-30d6b2a63cd7"
```
{: pre}

## Step 4: Evaluate the results of the large speech model
{: #models-migrate-step4}

Once you have updated your speech recognition requests to use large speech models, eliminated unsupported parameters, and re-created any custom language models, you can experiment with speech recognition based on previous- and next-generation models. Compare the resulting transcripts to determine whether the large speech model produces equivalent or better results. Also consider the performance of requests that use large speech model to determine how much faster you receive results.

You can also compare the word error rate of the large speech models, previous- and next-generation results. The open-source [Word Error Rate (WER) utility](https://github.com/IBM/watson-stt-wer-python){: external}, which is available in Python, can help you measure and compare the accuracy of your results.
