---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-12"

subcollection: speech-to-text

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

# Managing grammars
{: #manageGrammars}

The customization interface includes the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method for adding a grammar to a custom language model. For more information, see [Add a grammar to the custom language model](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). The interface also includes the following methods for listing and deleting grammars for a custom language model.
{: shortdesc}

## Listing grammars for a custom language model
{: #listGrammars}

The customization interface provides two methods for listing information about the grammars for a custom language model:

-   The `GET /v1/customizations/{customization_id}/grammars` method lists information about all grammars for a custom model.
-   The `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` method lists information about a specified grammar for a custom model.

Both methods return the same information about a grammar. The information includes the `name` of the grammar and the number of `out-of_vocabulary_words` that are recognized by the grammar. The response also includes the `status` of the grammar, which is important for checking the service's analysis of the grammar when you add it to a custom model:

-   `being_processed` indicates that the service is still processing the grammar in response to a `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` request.
-   `analyzed` indicates that the service has successfully processed the grammar and added it to the custom model.
-   `undetermined` means that the service encountered an error while processing the grammar. The information that is returned for the grammar includes an error message that offers guidance for correcting the error.

    For example, the grammar might be invalid, or you might have tried to add a grammar with the same name as an existing grammar. You can try to add the grammar again and include the `allow_overwrite` parameter with the request. You can also delete the grammar and then try adding it again.

### Example requests and responses
{: #listExample-grammars}

The following example lists information about all grammars that have been added to the custom model with the specified customization ID.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

Three grammars were successfully added to the custom model: `confirm-xml`, `confirm-abnf`, and `list-abnf`.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

The following example shows information about the specified grammar, `list-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## Deleting a grammar from a custom language model
{: #deleteGrammar}

Use the `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` method to remove an existing grammar from a custom language model. When it deletes the grammar, the service removes any OOV words that are associated with the grammar from the custom model's words resource unless

-   The word was also added by another grammar or by a corpus.
-   The word was modified in some way with the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method.

Removing a grammar does not affect the custom model until you train the model on its updated data by using the `POST /v1/customizations/{customization_id}/train` method. If you successfully trained the model on the grammar, the grammar continues to be available for speech recognition until you retrain the model.

### Example request
{: #deleteExample-grammar}

The following example deletes the grammar named `list-abnf` from the custom model with the specified customization ID.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
