---

copyright:
  years: 2015, 2022
lastupdated: "2022-03-16"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Managing grammars
{: #manageGrammars}

The customization interface includes the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method for adding a grammar to a custom language model. For more information, see [Add a grammar to the custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). The interface also includes the following methods for listing and deleting grammars for a custom language model.
{: shortdesc}

For more information about the languages and models that support grammars and their level of support (generally available or beta), see [Language support for customization](/docs/speech-to-text?topic=speech-to-text-custom-support).
{: note}

## Listing grammars for a custom language model
{: #listGrammars}

The customization interface provides two methods for listing information about the grammars for a custom language model:

-   The `GET /v1/customizations/{customization_id}/grammars` method lists information about all grammars for a custom model.
-   The `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` method lists information about a specified grammar for a custom model.

Both methods return the same information about a grammar. The information includes the `name` of the grammar and, *for custom models based on previous-generation models*, the number of `out-of_vocabulary_words` that are recognized by the grammar. The response also includes the `status` of the grammar, which is important for checking the service's analysis of the grammar when you add it to a custom model:

-   `being_processed` indicates that the service is still processing the grammar in response to a `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` request.
-   `analyzed` indicates that the service has successfully processed the grammar and added it to the custom model.
-   `undetermined` means that the service encountered an error while processing the grammar. The information that is returned for the grammar includes an error message that offers guidance for correcting the error.

    For example, the grammar might be invalid, or you might have tried to add a grammar with the same name as an existing grammar. You can try to add the grammar again and include the `allow_overwrite` parameter with the request. You can also delete the grammar and then try adding it again.

### List all grammars example
{: #listExample-grammars-all}

The following example lists information about all grammars that have been added to the custom model with the specified customization ID.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/grammars"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/grammars"
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

### List a specific grammar example
{: #listExample-grammars-specific}

The following example shows information about the specified grammar, `list-abnf`:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

The grammar contains eight OOV words and is fully analyzed:

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

### Delete a grammar example
{: #deleteExample-grammar}

The following example deletes the grammar named `list-abnf` from the custom model with the specified customization ID.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X DELETE -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X DELETE \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}
