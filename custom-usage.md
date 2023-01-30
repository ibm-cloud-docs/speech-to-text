---

copyright:
  years: 2015, 2023
lastupdated: "2023-01-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Usage notes for customization
{: #custom-usage}

To use customization with the {{site.data.keyword.speechtotextfull}} service, you need to be aware of topics such as custom model ownership, limits on the number of custom models that you can create, and information security and request logging. These topics are common to all supported languages and model types.
{: shortdesc}

## Ownership of custom models
{: #custom-owner}

A custom model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use credentials for that instance of the service with methods of the customization interface. Credentials that are created for other instances of the service cannot view or access the custom model.

All credentials that are obtained for the same instance of the {{site.data.keyword.speechtotextshort}} service share access to all custom models created for that service instance. To restrict access to a custom model, create a separate instance of the service and use only the credentials for that service instance to create and work with the model. Credentials for other service instances cannot affect the custom model.

An advantage of sharing ownership across credentials for a service instance is that you can cancel a set of credentials, for example, if they become compromised. You can then create new credentials for the same service instance and still maintain ownership of and access to custom models created with the original credentials.

## Maximum number of custom models
{: #custom-maximum}

You can create no more than 1024 custom language models and no more than 1024 custom acoustic models per owning service credentials. If you try to create more than 1024 custom models of either type, the service returns an error. You do not lose any existing models, but you cannot create any more until your model count is below the limit of 1024 for the type of custom model that you are trying to create.

## Information security
{: #custom-security}

You can associate a customer ID with data that is added or updated for custom language and custom acoustic models. You associate a customer ID with corpora, custom words, grammars, and audio resources by passing the `X-Watson-Metadata` header with the following methods. If necessary, you can then delete the data by using the `DELETE /v1/user_data` method.

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

In addition, if you delete an instance of the {{site.data.keyword.speechtotextshort}} service from the {{site.data.keyword.cloud_notm}} console, all data associated with that service instance is automatically deleted. This includes all custom language models, corpora, grammars, and words, and all custom acoustic models and audio resources. This data is purged automatically and regardless of whether a customer ID is associated with the data.

For more information, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

## Request logging and data privacy
{: #custom-logging}

[IBM Cloud]{: tag-ibm-cloud}

How the service handles request logging for calls to the customization interface depends on the request:

-   The service *does not* log data that is used to build custom models. For example, when working with corpora and words in a custom language model, you do not need to set the `X-Watson-Learning-Opt-Out` request header. Your training data is never used to improve the service's base models.
-   The service *does* log data when a custom model is used with a recognition request. You can opt out of request logging at the account level or by setting the `X-Watson-Learning-Opt-Out` request header to `true`.

For more information, see [Request logging](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-request-logging).
