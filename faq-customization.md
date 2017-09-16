---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-16"

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

# Customization
{: #customization}

1.  <span style="color:#003F69">Can I add my own specialized vocabulary to the service?</span>

    Yes. The service's base vocabulary contains many words that are used in everyday conversation by a broad, general audience. Its models provide sufficiently accurate recognition for a variety of applications but can lack knowledge of specific terms that are associated with particular domains; these are referred to as *out-of-vocabulary (OOV) words*.

    The language model customization interface lets you improve the accuracy of speech recognition for specialized domains. You can use customization to expand and tailor the vocabulary of a base model to include domain-specific data and terminology. See [Using language model customization](/docs/services/speech-to-text/custom.html). Note that customization is currently available only for US English and Japanese (GA) and for Spanish (beta).

1.  <span style="color:#003F69">How secure is the data that I add to a custom model?</span>

    A custom language model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use service credentials created for that instance of the service. Credentials created for other instances of the service cannot view or access the custom model. In addition, the data associated with a custom model is encrypted both at rest (when it is stored) and in motion (when it is used). For more information, see [Ownership of custom language models](/docs/services/speech-to-text/custom.html#customOwner).

1.  <span style="color:#003F69">A number of customization methods are asynchronous. How do I check their results to know when they are finished?</span>

    Three customization operations are asynchronous. The following sections describe how to check the results of the operations:
    -   To check the status of a request to add a corpus to a model with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method, see [Monitoring the add corpus request](/docs/services/speech-to-text/custom.html#monitorCorpus).
    -   To check the status of a request to add words to a model with the `POST /v1/customizations/{customization_ID}/words` method, see [Monitoring the add words request](/docs/services/speech-to-text/custom.html#monitorWords).
    -   To check the status of a request to train a model with the `POST /v1/customizations/{customization_id}/train` method, see [Monitoring the train model request](/docs/services/speech-to-text/custom.html#monitorTraining).

    See [Example scripts](/docs/services/speech-to-text/custom.html#exampleScripts) for sample code that monitors the operations in a Python or shell script.

1.  <span style="color:#003F69">I created a custom model, but the service does not appear to be using any of the new words that it contains during recognition?</span>

    Make sure that you are correctly passing the customization ID to the recognition request; for more information, see [Using a custom language model](/docs/services/speech-to-text/custom.html#useModel). Also check the pronunciations that were generated for the new words to make sure that they are correct; see [Validating a custom model's words resource](/docs/services/speech-to-text/custom.html#validateModel).
