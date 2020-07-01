---

copyright:
  years: 2015, 2020
lastupdated: "2020-07-01"

subcollection: speech-to-text

---

{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}
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

# Creating a custom language model
{: #languageCreate}

Follow these steps to create a custom language model for the {{site.data.keyword.speechtotextshort}} service:
{: shortdesc}

1.  [Create a custom language model](#createModel-language). You can create multiple custom models for the same or different domains. The process is the same for any model that you create. However, you can specify only a single custom model at a time with a recognition request.
1.  [Add a corpus to the custom language model](#addCorpus). A corpus is a plain text document that uses terminology from the domain in context. The service builds a vocabulary for a custom model by extracting terms from corpora that do not exist in its base vocabulary. You can add multiple corpora serially, one at a time, to a custom model.
1.  [Add words to the custom language model](#addWords). You can also add custom words to a model individually. In addition, you can use the same methods to modify custom words that are extracted from corpora. The methods let you specify the pronunciation of words and how the words are displayed in a speech transcript.
1.  [Train the custom language model](#trainModel-language). Once you add words to the custom model, you must train the model on the words. Training prepares the custom model for use in speech recognition. The model does not use new or modified words until you train it.
1.  After you train your custom model, you can use it with recognition requests. If the audio that is passed for transcription contains domain-specific words that are defined in the custom model, the results of the request reflect the model's enhanced vocabulary. You can use only one model at a time with a speech recognition request. For more information, see [Using a custom language model](/docs/speech-to-text?topic=speech-to-text-languageUse).

The steps for creating a custom language model are iterative. You can add corpora, add words, and train or retrain a model as often as needed.

You can also add grammars to a custom language model. Grammars restrict the service's response to only those words that are recognized by a grammar. For more information, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).

Language model customization is available for most languages. For more information, see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Create a custom language model
{: #createModel-language}

You use the `POST /v1/customizations` method to create a new custom language model. The method accepts a JSON object that defines the attributes of the new custom model as the body of the request. The new custom model is owned by the instance of the service whose credentials are used to create it. For more information, see [Ownership of custom models](/docs/speech-to-text?topic=speech-to-text-customization#customOwner).

You can create a maximum of 1024 custom language models per owning credentials. The service returns an error if you attempt to create more than 1024 models. You do not lose any models, but you cannot create any more until your model count is below the limit.

<table>
  <caption>Table 1. Attributes of a new custom language model</caption>
  <tr>
    <th style="width:20%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      A user-defined name for the new custom model. Use a name that
      describes the domain of the custom model, such as <code>Medical
        custom model</code> or <code>Legal custom model</code>. Use a
      name that is unique among all custom language models that you own.
      Use a localized name that matches the language of the custom model.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The name of the language model that is to be customized by the
      new custom model. Specify one of the supported language models
      that is returned by the <code>GET /v1/models</code> method. The
      new model can be used only with the base model that it customizes.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      The dialect of the specified language that is to be used with the
      new custom model. For most languages, the dialect matches the
      language of the base model by default. For example, `en-US` is used
      for either of the US English language models.<br><br>
      For a Spanish language, the service creates a custom model that is
      suited for speech in one of the following dialects:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          `es-ES` for Castilian Spanish (`es-ES` models)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-LA` for Latin American Spanish (`es-AR`, `es-CL`, `es-CO`,
          and `es-PE` models)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-US` for Mexican (North American) Spanish (`es-MX` models)
        </li>
      </ul>
      The parameter is meaningful only for Spanish models, for which
      you can always safely omit the parameter to have the service create
      the correct mapping.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          If you specify the `dialect` parameter for non-Spanish language
          models, its value must match the language of the base model.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          If you specify the `dialect` for Spanish language models, its
          value must match one of the defined mappings as indicated
          (`es-ES`, `es-LA`, or `es-MX`).
        </li>
      </ul>
      All dialect values are case-insensitive.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      A description of the new model. Use a localized description that
      matches the language of the custom model.
    </td>
  </tr>
</table>

The following example creates a new custom language model named `Example model`. The model is created for the base model `en-US-BroadbandModel` and has the description `Example custom language model`. The `Content-Type` header specifies that JSON data is being passed to the method.

```bash
curl -X POST -u "apikey:{apikey}" \
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

Once you create your custom language model, the next step is to add data (domain-specific words) to the model. The recommended means of populating a custom model with new words is to add one or more corpora.

A corpus is a plain text file that ideally contains sample sentences from your domain. The service parses a corpus file's contents and extracts any words that are not in its base vocabulary. Such words are referred to out-of-vocabulary (OOV) words.

-   For more information about using corpora, see [Working with corpora](/docs/speech-to-text?topic=speech-to-text-corporaWords#workingCorpora).
-   For more information about how the service adds corpora to a model, see [What happens when I add a corpus file?](/docs/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)

By providing sentences that include new words, corpora allow the service to learn the words in context. You can then augment or modify the model's words individually. Training a model only on individual words as opposed to words added from corpora is more time-consuming and can produce less effective results.
{: tip}

You use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add a corpus to a custom model:

-   Specify the customization ID of the custom model with the `customization_id` path parameter.
-   Specify a name for the corpus with the `corpus_name` path parameter. Use a localized name that matches the language of the custom model and reflects the contents of the corpus.
    -   Include a maximum of 128 characters in the name.
    -   Do not use characters that need to be URL-encoded. For example, do not use spaces, slashes, backslashes, colons, ampersands, double quotes, plus signs, equals signs, questions marks, and so on in the name. (The service does not prevent the use of these characters. But because they must be URL-encoded wherever used, their use is strongly discouraged.)
    -   Do not use the name of a corpus or grammar that has already been added to the custom model.
    -   Do not use the name `user`, which is reserved by the service to denote custom words that are added or modified by the user.
    -   Do not use the name `base_lm` or `default_lm`. Both names are reserved for future use by the service.
-   Pass the corpus text file as the body of the request.

You can add a maximum of 90 thousand OOV words and 10 million total words from all sources combined. This includes words from corpora and grammars, and words that you add directly. For more information, see [How much data do I need?](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)
{: note}

The following example adds the corpus text file `healthcare.txt` to the custom model with the specified ID. The example names the corpus `healthcare`.

```bash
curl -X POST -u "apikey:{apikey}" \
--data-binary @healthcare.txt \
"{url}/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

The method also accepts an optional `allow_overwrite` query parameter that overwrites an existing corpus for a custom model. Use the parameter if you need to update a corpus file after you add it to a model.

The method is asynchronous. It can take on the order of minutes to complete. The duration of the operation depends on the total number of words in the corpus, the number of new words that the service finds in the corpus, and the current load on the service. For more information about checking the status of a corpus, see [Monitoring the add corpus request](#monitorCorpus).

You can add any number of corpora to a custom model by calling the method once for each corpus text file. The addition of one corpus must be fully complete before you can add another. A corpus has the status `being_processed` when you first add it to a model. Its status becomes `analyzed` when the service finishes processing it.

Once the addition of a corpus is complete, examine the new custom words that were extracted from it to check for typographical and other errors. For more information, see [Validating a words resource](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

### Monitoring the add corpus request
{: #monitorCorpus}

The service returns a 201 response code if the corpus is valid. It then asynchronously processes the contents of the corpus and automatically extracts new words. You cannot submit requests to add data to the custom model or to train the model until the service's analysis of the corpus for the current request completes.

To determine the status of the analysis, use the `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method to poll the status of the corpus. The method accepts the ID of the model and the name of the corpus, as shown in the following example:

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

The response includes the status of the corpus:

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

Although adding corpora is the recommended means of adding words to a custom language model, you can also add individual custom words to the model directly. The service adds the custom words to the custom model just as it does OOV words that it discovers from corpora.

If you have only one or a few words to add to a model, using corpora to add the words might not be practical or even viable. The simplest approach is to add a word with only its spelling. But you can also provide multiple pronunciations for the word and indicate how the word is to be displayed.

-   For more information about adding words directly, see [Working with custom words](/docs/speech-to-text?topic=speech-to-text-corporaWords#workingWords).
-   For more information about how the service adds custom words to a model, see [What happens when I add or modify a custom word?](/docs/speech-to-text?topic=speech-to-text-corporaWords#parseWord)

You can use the following methods to add words to a custom model:

-   The `POST /v1/customizations/{customization_id}/words` method adds multiple words at one time. You pass a JSON object that provides information about each word via the body of the request. The following example adds two custom words, `HHonors` and `IEEE`, to the custom model with the specified ID. The `Content-Type` header specifies that JSON data is being passed to the method.

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data "{\"words\": [ \
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}, \
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}" \
    "{url}/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    You can also add the words from a file. For example, the file `words.json` defines the same two custom words.

    ```javascript
    {
      "words": [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    The following command adds the words from the file:

    ```bash
    curl -X POST -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data-binary @words.json \
    "{url}/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    This method is asynchronous. It can take on the order of minutes to complete. The time that it takes to complete depends on the number of words that you add and the current load on the service. For more information about checking the status of the operation, see [Monitoring the add words request](#monitorWords).
-   The `PUT /v1/customizations/{customization_id}/words/{word_name}` method adds individual words. You pass a JSON object that provides information about the word. The following example adds the word `NCAA` to the model with the specified ID. The `Content-Type` header again indicates that JSON data is being passed to the method.

    ```bash
    curl -X PUT -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}" \
    "{url}/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    This method is synchronous. The service returns a response code that reports the success or failure of the request immediately.

As with adding corpora, examine the new custom words to check for typographical and other errors. This check is especially important when you add multiple words at once. For more information, see [Validating a words resource](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

### Monitoring the add words request
{: #monitorWords}

When you use the `POST /v1/customizations/{customization_id}/words` method, the service returns a 201 response code if the input data is valid. It then asynchronously processes the words to add them to the model. You cannot submit requests to add data to the custom model or to train the model until the service completes the request to add new words.

To determine the status of the request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns information that includes the model's status, as in the following example:

```bash
curl -X GET -u "apikey:{apikey}"\
"{url}/v1/customizations/{customization_id}"
```
{: pre}

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

You use the methods to modify the definition of an existing word exactly as you do to add a word. The new data that you provide for the word overwrites the word's existing definition.

## Train the custom language model
{: #trainModel-language}

Once you populate a custom language model with new words (by adding corpora, by adding grammars, or by adding the words directly), you must train the model on the new data. Training prepares the custom model to use the data in speech recognition. The model does not use words that you add via any means until you train it on the data.

You use the `POST /v1/customizations/{customization_id}/train` method to train a custom model. You pass the method the customization ID of the model that you want to train, as in the following example:

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/train"
```
{: pre}

The method is asynchronous. Training can take on the order of minutes to complete depending on the number of new words on which the model is being trained and the current load on the service. For more information about checking the status of a training operation, see [Monitoring the train model request](#monitorTraining-language).

The method includes the following optional query parameters:

-   The `word_type_to_add` parameter specifies the words on which the custom model is to be trained:
    -   Specify `all` or omit the parameter to train the model on all of its words, regardless of their origin.
    -   Specify `user` to train the model only on words that were added or modified by the user, ignoring words that were extracted only from corpora or grammars.

    This option is useful if you add corpora with noisy data, such as words that contain typographical errors. Before training the model on such data, you can use the `word_type` query parameter of the `GET /v1/customizations/{customization_id}/words` method to review words that are extracted from corpora and grammars. For more information, see [Listing words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   The `customization_weight` parameter specifies the relative weight that is given to words from the custom model as opposed to words from the base vocabulary when the custom model is used for speech recognition. You can also specify a customization weight with any recognition request that uses the custom model. For more information, see [Using customization weight](/docs/speech-to-text?topic=speech-to-text-languageUse#weight).
-   The `strict` parameter indicates whether training is to proceed if the custom model contains a mix of valid and invalid resources (corpora, grammars, and words). By default, training fails if the model contains one or more invalid resources. Set the parameter to `false` to allow training to proceed as long as the model contains at least one valid resource. The service excludes invalid resources from the training. For more information, see [Training failures for custom language models](#failedTraining-language).

### Monitoring the train model request
{: #monitorTraining-language}

The service returns a 200 response code if it initiates the training process successfully. The service cannot accept subsequent training requests, or requests to add new corpora, grammars, or words, until the existing request completes.

To determine the status of a training request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns information about the model.

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}"
```
{: pre}

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
-   `training` indicates that the model is being trained. The `progress` field changes from `0` to `100` when training is complete. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indicates that the model is trained and ready to use. The `progress` field is `100`.
-   `upgrading` indicates that the model is being upgraded. The `progress` field is `0`.
-   `failed` indicates that training of the model failed. The `progress` field is `0`. For more information, see [Training failures for custom language models](#failedTraining-language).

Use a loop to check the status every 10 seconds until it becomes `available`. For more information about checking the status of a custom model, see [Listing custom language models](/docs/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).

### Training failures for custom language models
{: #failedTraining-language}
{: troubleshoot}
{: support}

Training fails to start if the service is handling another request for the custom language model. For instance, a training request fails to start with a status code of 409 if the service is

-   Processing a corpus or grammar to generate a list of OOV words
-   Processing custom words to validate or auto-generate sounds-like pronunciations
-   Handling another training request

Training also fails to start with a status code of 400 if the custom model

-   Contains no new valid training data (corpora, grammars, or words) since it was created or last trained
-   Contains one or more invalid corpora, grammars, or words (for example, a custom word has an invalid sounds-like pronunciation)

If the training request fails with a status code of 400, the service sets the custom model's status to `failed`. Take one of the following actions:

-   Use methods of the customization interface to examine the model's resources and fix any errors that you find:
    -   For an invalid corpus, you can correct the corpus text file and use the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add the corrected file to the model. For more information, see [Add a corpus to the custom language model](#addCorpus).
    -   For an invalid grammar, you can correct the grammar file and use the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method to add the corrected file to the model. For more information, see [Add a grammar to the custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar).
    -   For an invalid custom word, you can use the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method to modify the word directly in the model's words resource. For more information, see [Modifying words in a custom model](#modifyWord).

    For more information about validating the words in a custom language model, see [Validating a words resource](/docs/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
-   Set the `strict` parameter of the `POST /v1/customizations/{customization_id}/train` method to `false` to exclude invalid resources from the training. The model must contain at least one valid resource (corpus, grammar, or word) for training to succeed. The `strict` parameter is useful for training a custom model that contains a mix of valid and invalid resources.

## Example scripts
{: #exampleScripts}

You can use the following scripts to experiment with the steps for creating a custom language model:

-   A Python script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a>. For more information, see [Example Python script](#pythonScript).
-   A Bash shell script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a>. For more information, see [Example shell script](#shellScript).

The two scripts provide identical functionality. Each script creates a custom model, adds words from a corpus text file, and adds both single and multiple words to the model directly. The script queries the model to list words added from a corpus and directly by the user. It also trains the model and demonstrates the polling that is recommended for monitoring the results of asynchronous operations.

You can use either of two provided corpus text files with the scripts, or you can test with your own corpus files:

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a> is an abbreviated 6 KB corpus that adds six healthcare-related terms to a model. This file produces a small amount of output when used with the scripts. The scripts use this file by default.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a> is a richer 164 KB corpus that adds many healthcare-related terms to a model. This file produces much more output when used with the scripts.

By default, the new custom model that you create with either script is available for use with recognition requests. The scripts include an optional step for deleting the new custom model, which can be helpful when you are experimenting with the process. Follow the comments in the scripts to enable the deletion step.

### Example Python script
{: #pythonScript}

Follow these steps to use the Python script:

1.  Download the Python script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a>.
1.  Download the example corpus text files to use with the script. You are free to test with either of the corpus text files or with a file of your own choosing. By default, all corpus text files must reside in the same directory as the script.
1.  The script uses the Python `requests` library for HTTP requests to the service. Use `pip` or `easy_install` to install the library for use by the script, for example:

    ```bash
    pip install requests
    ```
    {: pre}

    For more information about the library, see [pypi.python.org/pypi/requests](https://pypi.python.org/pypi/requests){: external}.
1.  Edit the script to replace the `password` string `YOUR_IAM_APIKEY` with the API key from your {{site.data.keyword.speechtotextshort}} credentials:

    ```
    password = "YOUR_API_KEY"
    ```
    {: codeblock}

1.  Edit the script to replace the `url` string `YOUR_URL` with the URL for your service instance as provided in your service credentials:

    ```
    url = "YOUR_URL"
    ```
    {: codeblock}

1.  Run the script by entering the following command:

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

### Example shell script
{: #shellScript}

Follow these steps to use the Bash shell script:

1.  Download the shell script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a>.
1.  Download the example corpus text files to use with the script. You are free to test with either of the corpus text files or with a file of your own choosing. By default, all corpus text files must reside in the same directory as the script.
1.  The script uses the `curl` command for HTTP requests to the service. If you have not already downloaded `curl`, you can install the version for your operating system from [curl.haxx.se](http://curl.haxx.se){: external}. Install the version that supports the Secure Sockets Layer (SSL) protocol, and make sure to include the installed binary file on your `PATH` environment variable.
1.  Edit the script to replace the `PASSWORD` string `YOUR_IAM_APIKEY` with the API key from your {{site.data.keyword.speechtotextshort}} credentials:

    ```
    PASSWORD="YOUR_IAM_APIKEY"
    ```
    {: codeblock}

1.  Edit the script to replace the `URL` string `YOUR_URL` with the URL for your service instance as provided in your service credentials:

    ```
    URL="YOUR_URL"
    ```
    {: codeblock}

1.  Make sure that the script has executable permissions, and then run the script by entering the following command:

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
