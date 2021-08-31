---

copyright:
  years: 2015, 2021
lastupdated: "2021-08-26"

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

# Managing corpora
{: #manageCorpora}

The customization interface includes the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method for adding a corpus to a custom language model. For more information, see [Add a corpus to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addCorpus). The interface also includes the following methods for listing and deleting corpora for a custom language model.
{: shortdesc}

## Listing corpora for a custom language model
{: #listCorpora}

The customization interface provides two methods for listing information about the corpora for a custom language model:

-   The `GET /v1/customizations/{customization_id}/corpora` method lists information about all corpora for a custom model.
-   The `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method lists information about a specified corpus for a custom model.

Both methods return the `name` of the corpus, the `total_words` read from the corpus, and, *for custom models based on previous-generation models*, the number of `out-of-vocabulary_words` extracted from the corpus. The methods also list the `status` of the corpus. The status is important for checking the service's analysis of a corpus in response to a request to add it to a custom model.

-   `analyzed` indicates that the service successfully analyzed the corpus. You can train the custom model with data from the corpus, or you can add additional corpora or words to the model.
-   `being_processed` indicates that the service is still analyzing the corpus. The service cannot accept requests to add new corpora or words, or to train the custom model, until its analysis is complete.
-   `undetermined` indicates that the service encountered an error while processing the corpus. The information that is returned for the corpus includes an error message that offers guidance for correcting the error.

    For example, the corpus might be invalid, or you might have tried to add a corpus with the same name as an existing corpus. You can try to add the corpus again and include the `allow_overwrite` parameter with the request. You can also delete the corpus and then try adding it again.

### List all corpora example
{: #listExample-corpora-all}

The following example lists all corpora for the custom model with the specified customization ID:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/corpora"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/corpora"
```
{: pre}

Three corpora were added to the custom model. The service successfully analyzed `corpus1`. It is still analyzing `corpus2`, and its analysis of `corpus3` failed. Because the corpora were added to a custom model that is based on a previous-generation model, the `out_of_vocabulary_words` field shows the number of OOV words that the service extracted from the first corpus.  The field will show the number of OOV words that are extracted from `corpus2` when it is successfully analyzed. Analysis of `corpus3` failed, so no OOV words were extracted.

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

### List a specific corpus example
{: #listExample-corpora-specific}

The following example returns information about the corpus that is named `corpus1` for the custom model with the specified customization ID:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X GET -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

The corpus is fully analyzed and contains more than 400 OOV words:

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## Deleting a corpus from a custom language model
{: #deleteCorpus}

Use the `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` method to remove an existing corpus from a custom language model.

-   *If the custom model is based on a next-generation model,* the service deletes the corpus from the model.
-   *If the custom model is based on a previous-generation model,* the service deletes the corpus from the model *and* removes OOV words that are associated with the corpus from the custom model's words resource. The service removes an OOV word from a custom model unless
    -   The word was also added by another corpus or by a grammar.
    -   The word was modified in some way with the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method.

Removing a corpus does not affect the custom model until you train the model on its updated data by using the `POST /v1/customizations/{customization_id}/train` method. If you previously trained the model on the corpus successfully, information extracted from the corpus remains in the model and applies to speech recognition until you retrain the model.

### Delete a corpus example
{: #deleteExample-corpus}

The following example deletes the corpus that is named `corpus3` from the custom model with the specified customization ID:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X DELETE -u "apikey:{apikey}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X DELETE \
--header "Authorization: Bearer {token}" \
"{url}/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
