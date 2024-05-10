---

copyright:
  years: 2015, 2023
lastupdated: "2024-05-10"

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Creating a custom language model
{: #languageCreate}

Follow these steps to create, add contents to, and train a custom language model for the {{site.data.keyword.speechtotextfull}} service:
{: shortdesc}

1.  [Create a custom language model](#createModel-language). You can create multiple custom models for the same or different domains. The process is the same for any model that you create. Language model customization is available for all large speech models, most previous-generation models and for all next-generation models. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).
1.  [Add a corpus to the custom language model](#addCorpus). A corpus is a plain text document that uses terminology from the domain in context. You can add multiple corpora serially, one at a time, to a custom model. *For custom models that are based on previous-generation models,* the service builds a vocabulary for a custom model by extracting terms from corpora that do not exist in its base vocabulary. *For custom models that are based on next-generation models,* the service extracts character sequences rather than words from corpora.
1.  [Add words to the custom language model](#addWords). You can also add custom words to a model individually. You can specify how the words from a custom model are to be displayed in a speech transcript and how they are pronounced in audio. *For custom models that are based on previous-generation models,* you can also modify custom words that are extracted from corpora.
1.  [Train the custom language model](#trainModel-language). After you add corpora and words to the custom model, you must train the model. Training prepares the custom model for use in speech recognition. The model does not use new or modified corpora or words until you train it.
1.  [Use a custom language model for speech recognition](/docs/speech-to-text?topic=speech-to-text-languageUse). After you train your custom model, you can use it with speech recognition requests. If the audio that is passed for transcription contains domain-specific words that are defined in the custom model's corpora and custom words, the results of the request reflect the model's enhanced vocabulary. You can use only one model at a time with a speech recognition request.

The steps for creating a custom language model are iterative. You can add corpora, add words, and train or retrain a model as often as needed. You can also add grammars to most custom language model. Grammars restrict the service's response to only those words that are recognized by a grammar.

-   For more information about using grammars, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).
-   For more information about the languages and models that support grammars, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).

## Create a custom language model
{: #createModel-language}

You use the `POST /v1/customizations` method to create a new custom language model. The method accepts a JSON object that defines the attributes of the new custom model as the body of the request. The new custom model is owned by the instance of the service whose credentials are used to create it. For more information, see [Ownership of custom models](/docs/speech-to-text?topic=speech-to-text-custom-usage#custom-owner).

You can create a maximum of 1024 custom language models per owning credentials. For more information, see [Maximum number of custom models](/docs/speech-to-text?topic=speech-to-text-custom-usage#custom-maximum).

A new custom language model has the following attributes:

`name` (*required* string)
:   A user-defined name for the new custom model. Use a localized name that matches the language of the custom model and describes the domain of the model, such as `Medical custom model` or `Legal custom model`.
    -   Include a maximum of 256 characters in the name.
    -   Do not use backslashes, slashes, colons, equal signs, ampersands, or question marks in the name.
    -   Use a name that is unique among all custom language models that you own.

`base_model_name` (*required* string)
:   The name of the base language model that is to be customized by the new custom model. You must use the name of a model that is returned by the `GET /v1/models` method. The new custom model can be used only with the base model that it customizes.

`dialect` (*optional* string)
:   The dialect of the specified language that is to be used with the new custom model. *For all languages, it is always safe to omit this field.* The service automatically uses the language identifier from the name of the base model. For example, the service automatically uses `en-US` for all US English models.

    If you specify the `dialect` for a new custom model, follow these guidelines:
    -   For non-Spanish previous-generation models and for next-generation models, you must specify a value that matches the five-character language identifier from the name of the base model.
    -   For Spanish previous-generation models, you must specify one of the following values:
        -   `es-ES` for Castilian Spanish (`es-ES` models)
        -   `es-LA` for Latin American Spanish (`es-AR`, `es-CL`, `es-CO`, and `es-PE` models)
        -   `es-US` for Mexican (North American) Spanish (`es-MX` models)

    All values that you pass for the `dialect` field are case-insensitive.

`description` (*optional* string)
:   A recommended description of the new custom model.
    -   Use a localized description that matches the language of the custom model.
    -   Include a maximum of 128 characters in the description.

The following example creates a new custom language model named `Example model`. The model is created for the base model `en-US-BroadbandModel` and has the description `Example custom language model`. The required `Content-Type` header specifies that JSON data is being passed to the method.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"name\": \"Example model\", \
  \"base_model_name\": \"en-US_BroadbandModel\", \
  \"description\": \"Example custom language model\"}" \
"{url}/v1/customizations"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"name\": \"Example model\", \
  \"base_model_name\": \"en-US_BroadbandModel\", \
  \"description\": \"Example custom language model\"}" \
"{url}/v1/customizations"
```
{: pre}

The example returns the customization ID of the new model. Each custom model is identified by a unique customization ID, which is a Globally Unique Identifier (GUID). You specify a custom model's GUID with the `customization_id` parameter of calls that are associated with the model.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

## Add a corpus to the custom language model
{: #addCorpus}

Once you create your custom language model, the next step is to add domain-specific data to the model. The recommended means of populating a custom model is to add one or more corpora. A corpus is a plain text file that ideally contains sample sentences from your domain.

-   *For custom models that are based on large speech models,* the service parses and extracts word sequences from one or multiple corpora files. The characters help the service learn and predict character sequences from audio. For more information about using corpora with custom models that are based on large speech models, see [Working with corpora for large speech models and next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#workingCorpora-ng).

-   *For custom models that are based on previous-generation models,* the service parses a corpus file and extracts any words that are not in its base vocabulary. Such words are referred to out-of-vocabulary (OOV) words. For more information about using corpora with custom models that are based on previous-generation models, see [Working with corpora for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords#workingCorpora).
-   *For custom models that are based on next-generation models,* the service parses and extracts character sequences from a corpus file. The characters help the service learn and predict character sequences from audio. For more information about using corpora with custom models that are based on next-generation models, see [Working with corpora for large speech models and next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#workingCorpora-ng).

By providing sentences that include domain-specific words, corpora allow the service to learn the words and character sequences in context. You can also augment and modify a model's words individually. Training a model only on individual words as opposed to words added from corpora is more time-consuming and can produce less effective results.
{: tip}

You use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add a corpus to a custom model:

`customization_id` (*required* string)
:   Specify the customization ID of the custom model to which the corpus is to be added.

`corpus_name` (*required* string)
:   Specify a name for the corpus. Use a localized name that matches the language of the custom model and reflects the contents of the corpus.
    -   Include a maximum of 128 characters in the name.
    -   Do not use characters that need to be URL-encoded. For example, do not use spaces, slashes, backslashes, colons, ampersands, double quotes, plus signs, equals signs, questions marks, and so on in the name. (The service does not prevent the use of these characters. But because they must be URL-encoded wherever used, their use is strongly discouraged.)
    -   Do not use the name of a corpus or grammar that has already been added to the custom model.
    -   Do not use the name `user`, which is reserved by the service to denote custom words that are added or modified by the user.
    -   Do not use the name `base_lm` or `default_lm`. Both names are reserved for future use by the service.

Pass the corpus text file as the required body of the request. The following example adds the corpus text file `healthcare.txt` to the custom model with the specified ID. The example names the corpus `healthcare`.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--data-binary @healthcare.txt \
"{url}/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--data-binary @healthcare.txt \
"{url}/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

The method also accepts an optional `allow_overwrite` query parameter that overwrites an existing corpus for a custom model. Use the parameter if you need to update a corpus file after you add it to a model.

The method is asynchronous. It can take on the order of minutes to complete. The duration of the operation depends on the total number of words in the corpus and the current load on the service. *For custom models that are based on previous-generation models,* the duration also depends on the number of new words that the service finds in the corpus. For more information about checking the status of a corpus, see [Monitoring the add corpus request](#monitorCorpus).

You can add any number of corpora to a custom model by calling the method once for each corpus text file. The addition of one corpus must be fully complete before you can add another. A corpus has the status `being_processed` when you first add it to a model. Its status becomes `analyzed` when the service finishes processing it.

*For custom models that are based on previous-generation models,* after the addition of a corpus is complete, examine the new custom words that were extracted from it to check for typographical and other errors. For more information, see [Validating a words resource for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

### Monitoring the add corpus request
{: #monitorCorpus}

The service returns a 201 response code if the corpus is valid. It then asynchronously processes the contents of the corpus. You cannot submit requests to add data to the custom model or to train the model until the service's analysis of the corpus for the current request completes.

To determine the status of the analysis, use the `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method to poll the status of the corpus. The method accepts the ID of the model and the name of the corpus, as shown in the following example:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

The response includes the status of the corpus. Because the custom model is based on a previous-generation model, the response shows the number of OOV words.

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

The `status` field has one of the following values:

-   `analyzed` indicates that the service has successfully analyzed the corpus.
-   `being_processed` indicates that the service is still analyzing the corpus.
-   `undetermined` indicates that the service encountered an error while processing the corpus.

Use a loop to check the status of the corpus every 10 seconds until it becomes `analyzed`. For more information about checking the status of a model's corpora, see [Listing corpora for a custom language model](/docs/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).

## Add words to the custom language model
{: #addWords}

Although adding corpora is the recommended means of adding words to a custom language model, you can also add individual custom words to the model directly. The service parses custom words for the custom model just as it does the contents of words from corpora.

If you have only one or a few words to add to a model, using corpora to add the words might not be practical or even viable. The simplest approach is to add a word with only its spelling. But you can also indicate how the word is to be displayed and one or more pronunciations for the word.

-   For more information about adding words to a custom model that is based on a *previous-generation model*, see [Working with custom words for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords#workingWords).
-   For more information about adding words to a custom model that is based on * large speech models and next-generation models*, see [Working with custom words for large speech models and next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#workingWords-ng).

After you add words to a custom model, examine the new custom words to check for typographical and other errors. This check is especially important when you add multiple words at one time.

-   *For custom models that are based on previous-generation models,* see [Validating a words resource for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
-   *For custom models that are based on large speech models and next-generation models,* see [Validating a words resource for large speech models and next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#validateModel-ng).

### Add words with the POST method
{: #add-words-push}

The `POST /v1/customizations/{customization_id}/words` method adds one or more words at one time. You pass the words to be added as JSON data via the body of the request or from a file. In either case, the required `Content-Type` header specifies that JSON data is being passed to the method.

The following examples add two custom words, `HHonors` and `IEEE`, to the custom model with the specified ID:

-   The first example passes the information about each word via the body of the request:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data "{\"words\": [ \
       {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}, \
       {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}" \
    "{url}/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: application/json" \
    --data "{\"words\": [ \
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}, \
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}" \
    "{url}/v1/customizations/{customization_id}/words"
    ```
    {: pre}

-   The second example adds the same words from a file named `words.json`:

    ```javascript
    {
      "words": [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    The following request adds the words from the file:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data-binary @words.json \
    "{url}/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: application/json" \
    --data-binary @words.json \
    "{url}/v1/customizations/{customization_id}/words"
    ```
    {: pre}

The `POST` method is asynchronous. It can take on the order of minutes to complete. The time that it takes to complete depends on the number of words that you add and the current load on the service. For more information about checking the status of the operation, see [Monitoring the add words request](#monitorWords).

### Add a word with the PUT method
{: #add-words-pull}

The `PUT /v1/customizations/{customization_id}/words/{word_name}` method adds individual words. You pass a JSON object that provides information about the word as the body of the request.

The following example adds the word `NCAA` to the model with the specified ID. The required `Content-Type` header again indicates that JSON data is being passed to the method.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}" \
"{url}/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}" \
"{url}/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

The `PUT` method is synchronous. The service returns a response code that reports the success or failure of the request immediately.

### Monitoring the add words request
{: #monitorWords}

When you use the `POST /v1/customizations/{customization_id}/words` method, the service returns a 201 response code if the input data is valid. It then asynchronously processes the words to add them to the model. You cannot submit requests to add data to the custom model or to train the model until the service completes the request to add new words.

To determine the status of the request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model, as in the following example:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}"
```
{: pre}

The request includes information about the status of the model:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

The `status` field reports the current state of the model. While the service is processing new words, the status remains `pending`. Use a loop to check the status every 10 seconds until it becomes `ready` to indicate that the operation is complete. For more information about possible `status` values, see [Monitoring the train model request](#monitorTraining-language).

### Modifying words in a custom model
{: #modifyWord}

You can also use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to modify or augment a word in a custom model. You might need to use the methods to correct a typographical error or other mistake that was made when a word was added to the model. You might also need to add sounds-like definitions for an existing word.

You use the methods to modify the definition of an existing word exactly as you do to add a word. The new data that you provide for the word overwrites the word's existing definition. *For custom models that are based on previous-generation models,* you can also modify words that were added from corpora.

## Train the custom language model
{: #trainModel-language}

Once you populate a custom language model with new words (by adding corpora, by adding words directly, or by adding grammars), you must train the model on the new data. Training prepares the custom model to use the data in speech recognition. The model does not use words that you add via any means until you train it on the data.

You use the `POST /v1/customizations/{customization_id}/train` method to train a custom model. You pass the method the customization ID of the model that you want to train, as in the following example:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/train"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/train"
```
{: pre}

The method is asynchronous. Training can take on the order of minutes to complete depending on the number of new words on which the model is being trained and the current load on the service. For more information about checking the status of a training operation, see [Monitoring the train model request](#monitorTraining-language).

The method includes the following optional query parameters:

-   The `word_type_to_add` parameter specifies the words on which the custom model is to be trained:
    -   Specify `all` or omit the parameter to train the model on all of its words, regardless of their origin.
    -   Specify `user` to train the model only on words that were added or modified by the user, ignoring words that were extracted only from corpora or grammars.

    *For custom models that are based on previous-generation models,* this option is useful if you add corpora with noisy data, such as words that contain typographical errors. Before training the model on such data, you can use the `word_type` query parameter of the `GET /v1/customizations/{customization_id}/words` method to review words that are extracted from corpora and grammars. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).

    *For custom models that are based on next-generation models,*  the service ignores the `word_type_to_add` parameter. The words resource contains only custom words that the user adds or modifies directly, so the parameter is unnecessary.
-   The `customization_weight` parameter specifies the relative weight that is given to words from the custom model as opposed to words from the base vocabulary when the custom model is used for speech recognition. You can also specify a customization weight with any recognition request that uses the custom model. For more information, see [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight).
-   The `strict` parameter indicates whether training is to proceed if the custom model contains a mix of valid and invalid resources (corpora, words, and grammars). By default, training fails if the model contains one or more invalid resources. Set the parameter to `false` to allow training to proceed as long as the model contains at least one valid resource. The service excludes invalid resources from the training. For more information, see [Training failures for custom language models](#failedTraining-language).

### Monitoring the train model request
{: #monitorTraining-language}

The service returns a 200 response code if it initiates the training process successfully. The service cannot accept subsequent training requests, or requests to add new corpora, words, or grammars, until the existing request completes.

Adding custom words directly to a custom model that is based on a large speech model or next-generation model, as described in [Add words to the custom language model](#addWords), causes training of a model to take a few minutes longer than it otherwise would. If you are training a model with custom words that you added by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method, allow for some minutes of extra training time for the model.
{: note}

To determine the status of a training request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model:

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}"
```
{: pre}

The response includes information about the model's status:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

The response includes `status` and `progress` fields that report the state of the custom model. The meaning of the `progress` field depends on the model's status. The `status` field can have one of the following values:

-   `pending` indicates that the model was created but is waiting either for valid training data to be added or for the service to finish analyzing data that was added. The `progress` field is `0`.
-   `ready` indicates that the model contains valid data and is ready to be trained. The `progress` field is `0`.

    If the model contains a mix of valid and invalid resources (for example, both valid and invalid custom words), training of the model fails unless you set the `strict` query parameter to `false`. For more information, see [Training failures for custom language models](#failedTraining-language).
-   `training` indicates that the model is being trained. The `progress` field is `0`. The field changes from `0` to `100` when training is complete.
-   `available` indicates that the model is trained and ready to use. The `progress` field is `100`.
-   `upgrading` indicates that the model is being upgraded. The `progress` field is `0`.
-   `failed` indicates that training of the model failed. The `progress` field is `0`. For more information, see [Training failures for custom language models](#failedTraining-language).

Use a loop to check the status every 10 seconds until it becomes `available`. For more information about checking the status of a custom model, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).

### Training failures for custom language models
{: #failedTraining-language}
{: troubleshoot}
{: support}

Training fails to start if the service is handling another request for the custom language model. For instance, a training request fails to start with a status code of 409 if the service is

-   Processing a corpus or grammar to generate a list of OOV words or to extract character sequences
-   Processing custom words to validate or auto-generate sounds-like pronunciations
-   Handling another training request

Training also fails to start with a status code of 400 if the custom model

-   Contains no new valid training data (corpora, words, or grammars) since it was created or last trained
-   Contains one or more invalid corpora, words, or grammars (for example, a custom word has an invalid sounds-like pronunciation)

If the training request fails with a status code of 400, the service sets the custom model's status to `failed`. Take one of the following actions:

-   Use methods of the customization interface to examine the model's resources and fix any errors that you find:
    -   For an invalid corpus, you can correct the corpus text file and use the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add the corrected file to the model. For more information, see [Add a corpus to the custom language model](#addCorpus).
    -   For an invalid grammar, you can correct the grammar file and use the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method to add the corrected file to the model. For more information, see [Add a grammar to the custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar).
    -   For an invalid custom word, you can use the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method to modify the word directly in the model's words resource. For more information, see [Modifying words in a custom model](#modifyWord).

    For more information about validating the words in a custom language model, see
    -   [Validating a words resource for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel)
    -   [Validating a words resource for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng#validateModel-ng).

-   Set the `strict` parameter of the `POST /v1/customizations/{customization_id}/train` method to `false` to exclude invalid resources from the training. The model must contain at least one valid resource (corpus, word, or grammar) for training to succeed. The `strict` parameter is useful for training a custom model that contains a mix of valid and invalid resources.
