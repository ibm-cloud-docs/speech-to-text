---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-11"

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

# Adding a grammar to a custom language model
{: #grammarAdd}

Before you can use a grammar for speech recognition, you must first use the customization interface to add the grammar to a custom language model. The steps to add a grammar to a custom language model parallel those used to add corpora or custom words:
{: shortdesc}

1.  [Create a custom language model](#createModel). You can create a new custom model or use an existing model.
1.  [Add a grammar to the custom language model](#addGrammar). The service validates the grammar to ensure its correctness.
1.  [Validate the words from the grammar](#validateGrammar). You verify the correctness of the sounds-like pronunciations for any out-of-vocabulary (OOV) words that are recognized by the grammar.
1.  [Train the custom language model](#trainGrammar). The service prepares the custom model and grammar for use in speech recognition.
1.  You can now use the custom model and grammar in speech recognition requests. For more information, see [Using a grammar for speech recognition](/docs/services/speech-to-text/grammar-use.html).

These steps are iterative. You can add grammars, as well as corpora and custom words, to a custom language model as often as needed. You must train the custom model on any new data resources (grammars, corpora, or custom words) that you add. When you use it for speech recognition, a custom model reflects the data on which it was last trained.

You can use grammars with any language that supports language model customization. Language model customization is available for most languages. For more information, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Create a custom language model
{: #createModel}

To use a grammar with speech recognition, you must add it to a custom language model. You can use an existing custom language model, or you can create a new custom model by using the `POST /v1/customizations` method. For information about creating a new custom model, see [Create a custom language model](/docs/services/speech-to-text/language-create.html#createModel).

A custom language model can contain corpora and custom words as well as grammars. During speech recognition, you can use the custom model with or without its grammars. However, when you use a grammar, the service recognizes only words from the specified grammar.

## Add a grammar to the custom language model
{: #addGrammar}

You use the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method to add a grammar file to a custom model.

-   Specify the customization ID of the custom language model with the `customization_id` path parameter.
-   Specify a name for the grammar with the `grammar_name` path parameter. Use a localized name that matches the language of the custom model and reflects the contents of the grammar.
    -   Include a maximum of 128 characters in the name.
    -   Do not include spaces, slashes, or backslashes in the name.
    -   Do not use the name of a grammar or corpus that has already been added to the custom model.
    -   Do not use the name `user`, which is reserved by the service to denote custom words that are added or modified by the user.

-   Use the `Content-Type` request header to specify the format of the grammar:
    -   `application/srgs` for an ABNF grammar
    -   `application/srgs+xml` for an XML grammar

-   Pass the file that contains the grammar as the body of the request.

The following example adds the grammar file named `confirm.abnf` to the custom model with the specified ID. The example names the grammar `confirm-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

The method also accepts an optional `allow_overwrite` query parameter that you can include to overwrite an existing grammar with the same name. Use the parameter if you need to update a grammar after you add it to a model.

The method is asynchronous. It can take a few seconds for the service to analyze the grammar, depending on the size of the grammar and the current load on the service. For information about checking the status of a grammar, see [Monitoring the add grammar request](#monitorGrammar).

You can add any number of grammars to a custom model by calling the method once for each grammar file. The addition of one grammar must be fully complete before you can add another.

### Monitoring the add grammar request
{: #monitorGrammar}

The service returns a 201 response code if the grammar is valid. It then asynchronously processes the contents of the grammar and automatically extracts new words that the grammar can recognize. You cannot submit requests to add more data resources to a custom model, or to train the model, until the service's analysis of the grammar for the current request completes.

To determine the status of the analysis, use the `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` method to poll the status of the grammar. The method accepts the ID of the custom model and the name of the grammar. The following example checks the status of the grammar named `confirm-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

The response includes the status of the grammar:

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

The `status` field has one of the following values:

-   `analyzed` indicates that the service has successfully analyzed the grammar.
-   `being_processed` indicates that the service is still analyzing the grammar.
-   `undetermined` indicates that the service encountered an error while processing the grammar.

Use a loop to check the status of the grammar every 10 seconds until it becomes `analyzed`. For more information about checking the status of a grammar, see [Listing grammars for a custom language model](/docs/services/speech-to-text/grammar-manage.html#listGrammars).

### Handling add grammar failures
{: #handleFailures}

If its analysis of a grammar fails, the service sets the grammar's status to `undetermined` and includes an `error` field that describes the failure with the grammar's status. You can also use the `GET /v1/customizations/{customization_id}` method to check the status of the custom model. If addition of a grammar fails, the output includes an error message like the following:

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

The status of the custom model is not affected by the error, but the grammar cannot be used with the model. You can address the problem by correcting the grammar file and repeating the request to add it to the custom model. Set the `allow_overwrite` parameter to `true` to overwrite the version of the grammar file that failed.

You can also delete the grammar from the custom model for the time being, and then correct and add the grammar file again in the future. For more information about deleting a grammar, see [Deleting a grammar from a custom language model](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar).

## Validate the words from the grammar
{: #validateGrammar}

When you add a grammar file to a custom language model, the service parses the grammar to determine whether the grammar recognizes any words that are not already part of the service's base vocabulary. Such words are referred to as out-of-vocabulary (OOV) words. The service adds OOV words to the custom model's words resource. The purpose of the words resource is to define words that are not already present in the service's vocabulary.

Definitions in the words resource tell the service how to transcribe the OOV words. The information includes a `sounds_like` field that tells the service how the word is pronounced, a `display_as` field that tells the service how to display the word, and a `source` field that indicates how the word was added to the custom model. For more information about the words resource and OOV words, see [The words resource](/docs/services/speech-to-text/language-resource.html#wordsResource).

After you add a grammar to a custom model, it is good practice to examine the OOV words in the model's words resource to verify their sounds-like pronunciations. Not all grammars have OOV words, but verifying the words resource is generally a good idea. To check the words of the custom model after you add a grammar, use the following methods:

-   Use the `GET /v1/customizations/{customization_id}/words` method to list all of the words from a custom model. Pass the value `grammars` with the method's `word_type` parameter to list only words that were added from grammars.
-   Use the `GET /v1/customizations/{customization_id}/words/{word_name}` method to view an individual word from a model.

Verify that the sounds-like pronunciations of the words are accurate and correct. Also look for typographical and other errors in the words themselves. For more information about validating and correcting the words in a custom model, see [Validating a words resource](/docs/services/speech-to-text/language-resource.html#validateModel).

## Train the custom language model
{: #trainGrammar}

The final step before you can use a grammar with a custom language model is to train the model. You use the same process that you use for training a custom model on new corpora or new custom words. Training the model compiles the grammar for use during speech recognition. The service processes the grammar from its original text-based format to a binary runtime format for speech recognition. You cannot use the grammar until you train the model.

You use the `POST /v1/customizations/{customization_id}/train` method to train a custom model. You pass the method the customization ID of the model that you want to train, as in the following example.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

The training method is asynchronous. Training typically takes only seconds to complete. But it can take longer depending on the size and complexity of the grammar and the current load on the service. For information about checking the status of a training operation, see [Monitoring the training request](#monitorTraining).

### Monitoring the training request
{: #monitorTraining}

The service returns a 200 response code if the training process is successfully initiated. The service cannot accept subsequent training requests, or requests to add new grammars, corpora, or words to the custom model, until the current training request completes.

To determine the status of a training request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns information like the following about the model:

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
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

The `status` field has the value `training` while the model is being trained. Use a loop to check the status of the model every 10 seconds until it becomes `available`. For more information about monitoring a training request and other possible status values, see [Monitoring the train model request](/docs/services/speech-to-text/language-create.html#monitorTraining).
