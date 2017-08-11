---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Additional customization methods
{: #custom-methods}

[Using customization](/docs/services/speech-to-text/custom.html) describes the basic usage pattern for creating and using a custom language model with the {{site.data.keyword.speechtotextshort}} service. The customization interface includes additional methods for managing custom models, corpora, and custom words.
{: shortdesc}

For detailed information about all methods of the customization interface, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}. You must use service credentials from the service instance that owns a custom language model to use customization methods with the model; see [Ownership of custom language models](/docs/services/speech-to-text/custom.html#customOwner).

> **Note:** The language model customization interface is available only for US English and Japanese (GA) and for Spanish (beta).

## Managing custom language models
{: #manageModels}

The customization interface offers the `POST /v1/customizations` method for creating a custom language model (see [Creating a custom language model](/docs/services/speech-to-text/custom.html#createModel)) and the `POST /v1/customizations/train` method for training a custom model on the latest data from its words resource (see [Training a custom language model](/docs/services/speech-to-text/custom.html#trainModel)). In addition, the interface includes methods for listing information about custom models, for resetting a custom model to its initial state, and for deleting a custom model.

### Listing custom language models
{: #listModels}

The customization interface provides two methods for listing information about custom language models that are owned by the caller:

-   The `GET /v1/customizations` method lists information about all custom language models or about all custom models for a specified language.
-   The `GET /v1/customizations/{customization_id}` method lists information about a specified custom language model. Use this method to poll the service about the status of a training request.

Both methods return the following information about a custom model:

-   `customization_id`: The custom model's Globally Unique Identifier (GUID), which is used to identify the model in methods of the interface.
-   `created`: The date and time in Coordinated Universal Time (UTC) at which the custom model was created.
-   `language`: The language of the custom model, `en-US`, `es-ES`, or `ja-JP`.
-   `dialect`: The dialect of the language for the custom model. For US English and Japanese models, the field is always `en-US` or `ja-JP`. For Spanish models, the field indicates the dialect for which the model was created: `es-ES` for Castilian Spanish, `es-LA` for Latin-American Spanish, or `es-US` for North-American (Mexican) Spanish.
-   `owner`: The credentials of the service instance that owns the custom model.
-   `name`: The name of the custom model.
-   `description`: The description of the custom model, if one was provided at its creation.
-   `base_model`: The name of the base language model for which the custom model was created, one of the `en-US`, `es-ES`, or `ja-JP` language models.

The methods also returns a `status` field that indicates the current status of the custom model:

-   `pending` indicates that the model was created but is waiting either for training data to be added or for the service to finish analyzing data that has been added.
-   `ready` indicates that the model contains data and is ready to be trained.
-   `training` indicates that the model is currently being trained on data.
-   `available` indicates that the model is trained and ready to use with a recognition request.
-   `failed` indicates that training of the model failed. Examine the words in the model's words resource to determine the errors that prevented the model from being trained.

Additionally, the output includes a `progress` field that indicates the current progress of the custom model's training. If you used the `POST /v1/customizations/{customization_id}/train` method to initiate training of the model, this field indicates the current progress of that request as a percentage complete. At this time, the value of the field is `0` if the status is `pending`, `ready`, or `failed`; the value is `100` if the status is `available`.

The following example includes the `language` query parameter to list all US English custom language models owned by the calling user:

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

The caller owns two such models. The first is awaiting data or is currently being processed by the service; the second is fully trained and ready for use.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "dialect": "en-US",
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model",
      "description": "Example custom language model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "language": "en-US",
      "dialect": "en-US",
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_NarrowbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

The following example returns information about the custom model that has the specified ID:

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
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

### Resetting a custom language model
{: #resetModel}

Use the `POST /v1/customizations/{customization_id}/reset` method to reset a custom model. Resetting a model removes all of the corpora and words from the model, initializing the model to its state at creation. The method does not delete the model itself or metadata such as its name and language. However, once you reset a model, its words resource is empty and can be re-created only by again adding corpora and words.

The following example resets the custom model with the specified ID:

```bash
curl -X POST -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

### Deleting a custom language model
{: #deleteModel}

Use the `DELETE /v1/customizations/{customization_id}` method to delete a custom language model that you no longer need. The method deletes all corpora and words associated with the custom model as well as the model itself. Use this method with caution: a custom model and the data associated with it cannot be reclaimed once you delete the model.

The following example deletes the custom model with the specified ID:

```bash
curl -X DELETE -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

## Managing corpora
{: #manageCorpora}

In addition the the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method, which is used to add a corpus to a custom language model (see [Adding corpora to a custom language model](/docs/services/speech-to-text/custom.html#addCorpora)), the customization interface includes methods for listing and deleting corpora for a custom model.

### Listing corpora for a custom language model
{: #listCorpora}

The customization interface provides two methods for listing information about the corpora for a custom language model that is owned by the caller:

-   The `GET /v1/customizations/{customization_id}/corpora` method lists information about all corpora that have been added to a custom model.
-   The `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method lists information about a specified corpus for a custom model.

Both methods return the name of the corpus, the total number of words read from the corpus, and the number of out-of-vocabulary (OOV) words extracted from the corpus. The methods also list the status of the corpus, which is important for checking the service's analysis of a corpus in response a request to add it to a custom model:

-   `analyzed` indicates that the service has successfully analyzed the corpus. The custom model can be trained with data from the corpus.
-   `being_processed` indicates that the service is still analyzing the corpus. The service cannot accept requests to add new corpora or words, or to train the custom model, until its analysis is complete.
-   `undetermined` indicates that the service encountered an error while processing the corpus. The information returned for the corpus includes an error message that offers guidance for correcting the error, for example, if you tried to add a corpus with the same name without including the `allow_overwrite` option. In response to an error, you can also delete the corpus and try adding it again.

The following example lists all corpora for the custom model with the specified ID:

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

Three corpora have been added to the custom model. The service has successfully analyzed `corpus1`; you can train the model on words found in the corpus. The service is currently analyzing `corpus2`, and its analysis of `corpus3` failed.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

The following example returns information about the corpus named `corpus1` for the custom model with the specified ID:

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

### Deleting a corpus from a custom language model
{: #deleteCorpus}

Use the `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` method to remove an existing corpus from a custom language model. When it deletes the corpus, the service removes any OOV words associated with the corpus from the custom model's words resource unless

-   The word was also added by another corpus or
-   The word has been modified in some way with the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method.

Removing a corpus does not affect the custom model until you train the model on its updated data by using the `POST /v1/customizations/{customization_id}/train` method. Assuming you successfully trained the model on the corpus, until you retrain the model, words from the corpus remain in the model's vocabulary and apply to speech recognition.

The following method deletes the corpus named `corpus3` from the custom model with the specified ID:

```bash
curl -X DELETE -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}

## Managing custom words
{: #manageWords}

Along with the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods, which are used to add or modify words for a custom model (see [Adding words to a custom model](/docs/services/speech-to-text/custom.html#addWords)), the customization interface provides methods for listing the words of a custom language model and for deleting a word from a custom model.

### Listing words from a custom language model
{: #listWords}

The customization interface offers two methods for listing words from a custom language model:

-   The `GET /v1/customizations/{customization_id}/words` method lists information about the words from the custom model's words resource. The method includes two optional query parameters:
    -   The `word_type` parameter lets you specify which words are to be listed: `all` (the default) shows all words, `user` shows only custom words that were added or modified by the user, and `corpora` shows only OOV words that were extracted from corpora.
    -   The `sort` parameter lets you indicate how the words are to be ordered. The parameter accepts one of two arguments, `alphabetical` or `count`, to indicate how the words are to be sorted. You can prepend an optional `+` or `-` to an argument to indicate whether the results are to be sorted in ascending or descending order. By default, the method displays the words in ascending alphabetical order.
-   The `GET /v1/customizations/{customization_id}/words/{word_name}` methods lists information about a single specified word from the model's words resource.

In addition to a `word` field that identifies the word, both methods return the following information about a word:

-   A `sounds_like` field that presents an array of pronunciations for the word. The array can include the sounds-like pronunciation automatically generated by the service if no sounds-like value is provided for the word. For more information, see [Using the sounds_like field](/docs/services/speech-to-text/custom.html#soundsLike).
-   A `display_as` field that shows the spelling of the custom word that the service displays in a transcription. The field contains an empty string if no display-as value is provided for the word, in which case the word is displayed as it is spelled. For more information, see [Using the display_as field](/docs/services/speech-to-text/custom.html#soundsLike).
-   A `count` field that indicates the number of times the word is found across all corpora. For example, if the word occurs five times in one corpus and seven times in another, its count is `12`. If you add a custom word to a model before it is added by any corpora, the count begins at `1`; if the word is added from a corpus first and later modified, the count reflects only the number of times it is found in corpora.

    > **Note:** For custom models created prior to the introduction of the `count` field, the field always remains at `0`. To update the field for such models, add the model's corpora again and include the `allow_overwrite` parameter with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method.
-   A `source` field that indicates how the word was added to the custom model's words resource. For OOV words, the field includes the name of each corpus from which the service extracted the word. If you modified or added the word directly, the field includes the string `user`.

If the service discovered one or more problems with a custom word's definition, the output includes an `error` field that provides an array that lists each problem element from the definition and a message that describes the problem. An error can occur, for example, if you add a custom word with an invalid `sounds_like` field, one that violates one of the rules for adding a pronunciation. You cannot train a model whose words resource includes a word with an error; you must correct or delete the word before you can train the model.

The following example lists all of the words, regardless of type, from the custom model with the specified ID. The words are displayed in the default sort order, ascending alphabetical.

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

The words resource for the model contains four words. The first word was added directly by the user, but its `sounds_like` field contains an error: the field cannot contain numbers. The next two words were added from the listed corpora; the final word was added directly by the user.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": ["hilton honors","h honors"],
      "display_as": "HHonors",
      "count": 1,
      "source": ["corpus1"]
    },
    {
      "word": "IEEE",
      "sounds_like": ["i triple e"],
      "display_as": "IEEE",
      "count": 3,
      "source": ["corpus1","corpus2"]
    },
    {
      "word": "tomato",
      "sounds_like": ["tomatoh","tomayto"],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

The following example shows information about the word `NCAA` from the words resource of the specified model:

```bash
curl -X GET -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

The service added the word from `corpus3`, and the word was also modified by the user:

```javascript
{
  "word": "NCAA",
  "sounds_like": ["N. C. A. A.","N. C. double A."],
  "display_as": "NCAA",
  "count": 1,
  "source": ["corpus3","user"]
}
```
{: codeblock}

### Deleting a word from a custom language model
{: #deleteWord}

Use the `DELETE /v1/customizations/{customization_id}/words/{word_name}` method to delete a word from a custom language model. Use the method to remove words that were added in error, for example, from a corpus with faulty data.

You can remove any word that you added to the custom model's words resource via any means. However, you cannot delete a word from the service's base vocabulary. Deleting a word from a custom model deletes only the custom pronunciation for the word; the word remains in the base vocabulary.

Removing a word from a custom model does not affect the model until you retrain it by using the `POST /v1/customizations/{customization_id}/train` method. If the model was previously trained on the word, until you retrain it, the model continues to apply the word to speech recognition even after you delete the word from the its words resource.

The following example deletes the word `IEEE` from the custom model with the specified ID:

```bash
curl -X DELETE -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
