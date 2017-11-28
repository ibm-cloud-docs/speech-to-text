---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-10"

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

# The customization interface
{: #customization}

The {{site.data.keyword.speechtotextshort}} service offers a customization interface that you can use to augment its speech recognition capabilities. You can use customization to improve the accuracy of speech recognition requests by customizing a base model for your domain and audio. Customization is available for only some languages and at different levels of support for different languages; see [Language support for customization](#languageSupport).
{: shortdesc}

Speech recognition works the same either with or without a custom model. When using a custom model, you can use all of the input and output parameters that are normally available with a recognition request. For more information, see [Input features](/docs/services/speech-to-text/input.html), [Output features](/docs/services/speech-to-text/output.html), and the [Parameter summary](/docs/services/speech-to-text/summary.html).

## Language model customization
{: #customLanguage}

The service was developed with a broad, general audience in mind. The service's base vocabulary contains many words that are used in everyday conversation. The general model provides sufficiently accurate recognition for a variety of applications, but it can lack knowledge of specific terms that are associated with particular domains.

The *language model customization* interface lets you improve the accuracy of speech recognition for domains such as medicine, law, information technology, and others. Language model customization lets you expand and tailor the vocabulary of a base model to include domain-specific terminology. You create a custom language model and provide corpora and words specific to your domain. Once you train the custom language model on your enhanced vocabulary, you can use it for customized speech recognition. For more information, see

-   [Creating a custom language model](/docs/services/speech-to-text/language-create.html)
-   [Using a custom language model](/docs/services/speech-to-text/language-use.html)

## Acoustic model customization
{: #customAcoustic}

Similarly, the service was developed with a base acoustic model that works well with a variety of audio characteristics. But in cases like the following, adapting a base model to suit your audio can improve speech recognition:

-   Your acoustic channel environment is unique. For example, the environment is noisy, microphone quality or positioning are suboptimal, or the audio suffers from far-field effects.
-   Your speakers' speech patterns are atypical. For example, a speaker talks abnormally fast or the audio includes casual conversations.
-   Your speakers' accents are pronounced. For example, the audio includes speakers talking in a non-native or second language.

The *acoustic model customization* interface lets you adapt a base model to your environment and speakers. You create a custom acoustic model and provide example audio that closely matches the acoustic signature of the audio that you want to transcribe. Once you train the custom acoustic model with your audio resources, you can use it for customized speech recognition. For more information, see

-   [Creating a custom acoustic model](/docs/services/speech-to-text/acoustic-create.html)
-   [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html)

## Using acoustic and language customization together
{: #combined}

Using a custom acoustic model alone can improve the service's recognition capabilities. But if transcriptions or related corpora are available for your example audio, you can use that data to further improve the quality of speech recognition based on the custom acoustic model.

By creating a custom language model that complements your custom acoustic model, you can enhance speech recognition by using the two models together. When training a custom acoustic model, you can specify a custom language model that includes transcriptions of the audio resources or a vocabulary of domain-specific words found in the resources. Similarly, when you transcribe audio, the service accepts a custom language model, a custom acoustic model, or both. For more information, see [Using custom acoustic and custom language models together](/docs/services/speech-to-text/acoustic-both.html).

## Language support for customization
{: #languageSupport}

Language and acoustic model customization are available for only some languages. The following table documents the level at which the service supports customization for each language. *GA* indicates that the interface is generally available for production use; *Beta* indicates that the interface is available as a beta offering; and *Not supported* means that the interface is not available for that language. You can use both broadband and narrowband models with any supported language.

<table>
  <caption>Table 1. Language support for customization</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      Language
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Support for<br/>language model customization
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Support for <br/>acoustic model customization
    </th>
  </tr>
  <tr>
    <td>Brazilian Portuguese</td>
    <td style="text-align:center">Not supported</td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>French</td>
    <td style="text-align:center">Not supported</td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>Japanese</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Mandarin Chinese</td>
    <td style="text-align:center">Not supported</td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>Modern Standard Arabic</td>
    <td style="text-align:center">Not supported</td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>Spanish</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>UK English</td>
    <td style="text-align:center">Not supported</td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>US English</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
</table>

You can also use the `GET /v1/models` and `GET /v1/models/{model_id}` methods to check whether a language model supports language model customization. If the base model supports language model customization, the `supported_features` field of the methods' output for the model sets the `custom_language_model` field to `true`.

## Usage notes for customization
{: #customUsage}

The following usage notes apply to both language model customization and acoustic model customization.

### Ownership of custom models
{: #customOwner}

A custom model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use service credentials created for that instance of the service with methods of the customization interface. Credentials created for other instances of the service cannot view or access the custom model. The data associated with a custom model is encrypted both at rest (when it is stored) and in motion (when it is traveling over the network).

All service credentials obtained for the same instance of the {{site.data.keyword.speechtotextshort}} service share access to all custom models created for that service instance. To restrict access to a custom model, create a separate instance of the service and use only the credentials for that service instance to create and work with the model. Credentials for other service instances cannot affect the custom model.

An advantage of sharing ownership across credentials for a service instance is that you can cancel a set of credentials, for example, if they become compromised. You can then create new credentials for the same service instance and still maintain ownership of and access to custom models created with the original credentials.

### Request logging and data privacy
{: #customLogging}

How the service handles request logging for calls to the customization interface depends on the request:

-   The service *does not* log data that are used to build custom models. For example, when working with corpora and words in a custom language model, you do not need to set the `X-Watson-Learning-Opt-Out` request header. Your training data is never used to improve the service's base models.
-   The service *does* log data when a custom model is used with a recognition request. You must set the `X-Watson-Learning-Opt-Out` request header to `true` to prevent logging for recognition requests.

For more information about request logging, see [Authentication tokens and request logging](/docs/services/speech-to-text/input.html#common).

### Using the examples
{: #customCurl}

The documentation includes examples that use cURL to demonstrate the methods of the customization interface. To run the examples, create an instance of the {{site.data.keyword.speechtotextshort}} service in {{site.data.keyword.Bluemix_notm}}. Then replace `{username}:{password}` in each example with the values of your *username* and *password* from the credentials for the service instance. Concatenate the two values with an embedded colon to create a single string as shown.

Note that you must use your service credentials, *not* your {{site.data.keyword.Bluemix_notm}} ID and password. For more information, see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html).
