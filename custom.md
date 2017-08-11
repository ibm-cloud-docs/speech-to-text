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

# Using customization
{: #custom}

The {{site.data.keyword.speechtotextshort}} service was developed with a broad, general audience in mind. The service's base vocabulary contains many words that are used in everyday conversation. This general model provides sufficiently accurate recognition for a variety of applications, but it can lack knowledge of specific terms that are associated with particular domains.
{: shortdesc}

The language model customization interface lets you improve the accuracy of speech recognition for domains such as medicine, law, information technology, and so on. Customization lets you expand and tailor the vocabulary of a base model to include domain-specific data and terminology. Once you provide data for your domain and build a custom language model that reflects that data, you can use the model with your applications to provide customized speech recognition.

The following sections describe the steps you follow to create and use a custom language model. The sections introduce basic customization terminology. The final section provides [Example scripts](#exampleScripts) that create a custom language model and populate it with words.

The customization interface offers a rich set of methods for managing custom models and the words they contain; for more information about using these additional methods, see [Additional customization methods](/docs/services/speech-to-text/custom-methods.html). For detailed information about all customization methods, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

> **Note:** The language model customization interface is available only for US English and Japanese (GA) and for Spanish (beta).

## Usage model
{: #usage}

The typical usage model for working with {{site.data.keyword.speechtotextshort}} customization includes the following steps for creating and using a custom language model:

1.  [Create a custom language model.](#createModel) You can create multiple custom models for the same or different domains. The process is the same for any model you create. However, you can specify only a single custom model at a time with a recognition request.
1.  [Add data from corpora to the custom language model.](#addCorpora) A corpus is a plain text document that uses terminology from the domain in context. The service builds a vocabulary for a custom model by extracting terms from corpora that do not exist in its base vocabulary. You can add multiple corpora to a custom model.
1.  [Add words to the custom language model.](#addWords) You can add custom words to a model individually as well as via corpora. In addition, you can use the same methods to modify custom words extracted for the model from corpora. The methods let you specify the pronunciation of words and how they are displayed in a speech transcript.
1.  [Train the custom language model.](#trainModel) Once you add words to the custom model (from corpora, individually, or both), you must train the model on the custom words in your domain-specific vocabulary. Training prepares the custom model for use in speech recognition. The model does not use new words you add until you train it on the new data.
1.  [Use the custom language model in a recognition request.](#useModel) Once you train your custom model, you can use it with a recognition request. The results of the request reflect the enhanced vocabulary available with the custom model.

The steps involved in preparing a custom model for use in speech recognition are iterative. You can add corpora, add words, and train or retrain a model as often as needed. The recommended approach is to add corpora to the model, since providing sentences allows the model to learn the words in context. You can then augment or modify the model's words individually. Training a model only on individual words as opposed to words added from corpora is more time-consuming and can produce less effective results.

## Usage notes
{: #customGuidelines}

Keep the following in mind when working with the customization interface.

### Ownership of custom language models
{: #customOwner}

A custom language model is owned by the instance of the {{site.data.keyword.speechtotextshort}} service whose credentials are used to create it. To work with the custom model in any way, you must use service credentials created for that instance of the service with methods of the customization interface. Credentials created for other instances of the service cannot view or access the custom model. In addition, the data associated with a custom model is encrypted both at rest (when it is stored) and in motion (when it is used).

All service credentials obtained for the same instance of the {{site.data.keyword.speechtotextshort}} service share access to all custom models created for that service instance. If you want to restrict access to a custom model, create a separate instance of the service and use only the credentials for that service instance to create and work with the model. Credentials for other service instances cannot affect the custom model.

An advantage of sharing ownership across service credentials is that you can cancel a set of credentials, for example, if they become compromised. You can then create new credentials for the same service instance and still maintain ownership of and access to custom models created with the original credentials.

### Request logging and data privacy
{: #customLogging}

How the service handles request logging for calls to the customization interface depends on the request:

-   The service *does not* log data (corpora and words) that are used to build custom language models. You do not need to set the `X-Watson-Learning-Opt-Out` request header when using the customization interface to manage the corpora and words in a custom model. Your training data is never used to improve the service's base models.
-   The service *does* log data when a custom model is used with a recognition request. You must set the `X-Watson-Learning-Opt-Out` request header to prevent logging for recognition requests.

For more information about request logging, see [Authentication tokens and request logging](/docs/services/speech-to-text/input.html#common).

### Using the examples
{: #customCurl}

The examples that follow use cURL to demonstrate the methods of the customization interface. To run the examples, create an instance of the {{site.data.keyword.speechtotextshort}} service in {{site.data.keyword.Bluemix_notm}}. Then replace `{username}:{password}` in each example with the values of your *username* and *password* from the credentials for the service instance. Concatenate the two values with an embedded colon to create a single string of the form *username*:*password*.

Note that you must use your service credentials, *not* your {{site.data.keyword.Bluemix_notm}} ID and password. For more information, see [Service credentials for {{site.data.keyword.watson}} services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/common/getting-started-credentials.html){: new_window}.

## Creating a custom language model
{: #createModel}

You use the the `POST /v1/customizations` method to create a new custom language model. You can create any number of custom language models, but you can use only one model at a time with a speech recognition request. The method accepts the following parameters to specify the attributes of the new custom model. You pass the parameters as a JSON `CustomModel` object via the body of the request.

<table>
  <caption>Table 1. Parameters of the <code>customizations</code>
    method</caption>
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
      name that is unique among all custom models that you own. Use a
      localized name that matches the language of the custom model.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The name of the language model that is to be customized by the
      new custom model. The interface currently supports only US English
      and Japanese (GA) and Spanish (beta), so you must specify the name
      of one of the <code>en-US</code>, <code>es-ES</code>, or
      <code>ja-JP</code> language models returned by the <code>GET
        /v1/models</code> method. The new model can be used only with
      the base language model that it customizes.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      The dialect of the specified language that is to be used with the
      custom model. By default, the dialect matches the language of the
      base language model; for example, the dialect is <code>en-US</code>
      for either of the US English language models,
      <code>en-US_BroadbandModel</code> or
      <code>en-US_NarrowbandModel</code>.<br/></br>
      The parameter is meaningful only for Spanish models, for which
      the service creates a custom model that is suited for speech in
      the indicated dialect. You can specify one of the following
      dialects:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>es-ES</code> for Castilian Spanish (the default)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>es-LA</code> for Latin-American Spanish
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>es-US</code> for North-American (Mexican) Spanish
        </li>
      </ul>
      If you specify a dialect, it must be valid for the base language
      model.
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

The following example creates a new custom language model named `Example model`. The model is created for the base language model `en-US-BroadbandModel` and has the description `Example custom language model`. The `Content-Type` header specifies that JSON data is being passed to the method.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

The example returns the customization ID of the new model. Each custom model is identified by a unique customization ID, which is a Globally Unique Identifier (GUID). You specify a custom model's GUID with the `customization_id` parameter of calls associated with the model.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

The new custom model is owned by the service instance whose credentials are used to create it. For more information, see [Ownership of custom language models](#customOwner).

## Adding corpora to a custom language model
{: #addCorpora}

Once you create your custom language model, the next step is to add data (domain-specific words) to it. The recommended means of adding data to a custom model is by adding one or more corpora to the model. A corpus is a plain text file that ideally contains sample sentences from your domain. For more information about creating a corpus text file, see [Preparing a corpus file](#prepareCorpus).

The following simple example file shows an abbreviated corpus for the healthcare domain. A corpus file is typically much longer.

```
Am I at risk for health problems during travel?
Some people are more likely to have health problems when traveling outside the United States.
How Is Coronary Microvascular Disease Treated?
If you're diagnosed with coronary MVD and also have anemia, you may benefit from treatment for that condition.
Anemia is thought to slow the growth of cells needed to repair damaged blood vessels.
What causes autoimmune hepatitis?
A combination of autoimmunity, environmental triggers, and a genetic predisposition can lead to autoimmune hepatitis.
What research is being done for Spinal Cord Injury?
The National Institute of Neurological Disorders and Stroke NINDS conducts spinal cord research in its laboratories at the National Institutes of Health NIH.
NINDS also supports additional research through grants to major research institutions across the country.
Advances in research are giving doctors and patients hope that repairing injured spinal cords is a reachable goal.
Advances in basic research are also being matched by progress in clinical research, especially in understanding the kinds of physical rehabilitation that work best to restore function.
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
Osteogenesis imperfecta OI is a rare genetic disorder that, like juvenile osteoporosis, is characterized by bones that break easily, often from little or no apparent cause.
```
{: screen}

You use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add a corpus to a custom model. Specify the GUID of the custom model for the `customization_id` path parameter and a name for the corpus for the `corpus_name` parameter. The corpus name cannot contain spaces and cannot be the string `user`, which is reserved by the service to denote custom words added or modified by the user. Use a localized name that matches the language of the custom model.

Specify the text file for the corpus via the body of the request. The following example adds the corpus text file `healthcare.txt` to the custom model with the specified ID:

```bash
curl -X POST -u {username}:{password}
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

The method also accepts an optional `allow_overwrite` query parameter that lets you overwrite an existing corpus for a custom model. Use this option if you need to update a corpus file after you have added it to a model.

The method is asynchronous. It can take on the order of a minute or two to complete depending on the total number of words in the corpus, the number of new words that the service finds in the corpus, and the current load on the service. For information about checking the status of the corpus, see [Monitoring the add corpus request](#monitorCorpus).

You can use this method to add any number of corpora to a custom model by calling the method once for each corpus text file. But the addition of one corpus must be fully complete before you can add another.

### Monitoring the add corpus request
{: #monitorCorpus}

The service returns a 201 response code if the corpus is valid. It then asynchronously pre-processes the contents of the corpus and automatically extracts new words; for more information, see [What happens when you add a corpus file](#parseCorpus). You cannot submit requests to add additional corpora or words to a custom model, or to train the model, until the service's analysis of the corpus for the current request completes.

To determine the status of the analysis, use the `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method to poll the status of the corpus. The method accepts the ID of the model and the name of the corpus, and it returns the status of the corpus, as in the following example:

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

The response includes a `status` field that has one of the following values:

-   `analyzed` indicates that the service has successfully analyzed the corpus.
-   `being_processed` indicates that the service is still analyzing the corpus.
-   `undetermined` indicates that the service encountered an error while processing the corpus.

Use a loop to check the status of the corpus every 10 seconds until it becomes `analyzed`. For more information about checking the status of a model's corpora, see [Listing corpora for a custom language model](/docs/services/speech-to-text/custom-methods.html#listCorpora).

### Preparing a corpus file
{: #prepareCorpus}

A corpus text file contains sample sentences from your domain. Adding a corpus to a custom language model allows the service to extract domain-specific words from the corpus in context. Speech recognition relies on statistical algorithms to analyze audio. Words from a custom model are in competition with words from the service's base vocabulary as well as other words of the model. (Factors such as audio noise and speaker accents also affect the quality of transcription.)

The accuracy of transcription can depend largely on how words are defined in a model and how speakers say them. To improve the service's accuracy, provide as many examples as possible of how custom words are used in the domain. The more sentences you add that represent the context in which speakers use words from the domain, the better the service's recognition accuracy. The service does not apply a simple word-matching algorithm; its transcription depends on the context in which words are used.

For example, accountants adhere to a common set of standards and procedures known as Generally Accepted Accounting Principles (GAAP). When creating a custom model for a financial domain, the more sentences you provide that use the term GAAP in context, the better the service can distinguish between general sentences such as "the gap between them is small" and domain-centric sentences such as "GAAP provides guidelines for measuring and disclosing financial information."

Follow these guidelines to prepare a corpus text file:

-   Provide a plain text file that is encoded in UTF-8 if it contains non-ASCII characters. The service assumes UTF-8 encoding if it encounters such characters. (If your domain data is in a format other than ASCII or UTF-8, consider using the [{{site.data.keyword.watson}} {{site.data.keyword.documentconversionshort}} service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/document-conversion/index.html) to convert it to a text file.
-   Include each sentence of the corpus on its own line, terminating each line with a carriage return. Including multiple sentences on the same line can degrade accuracy.
-   Use consistent capitalization for words in the corpus. The words resource is case-sensitive; mix upper- and lowercase letters and use capitalization only when intended.
-   Beware of typographical errors. The service assumes that typos are new words; unless you correct them before training the model, the service adds them to the model's vocabulary. Remember the adage *Garbage in, garbage out!*

While more sentences result in better accuracy, the service does limit the overall amount of data that you can add to a custom model. You can add a maximum of 10 million total words from all corpora combined, and no more than 30 thousand new (out-of-vocabulary) words to a model, including words that the service extracts from corpora and words that you add directly.

#### Language-specific considerations
{: #corpusLanguages}

To distill the most meaning from the content, the service tokenizes and parses the data that it reads from a corpus file. The service does the following for each supported language.

**For US English:**

-   Converts numbers to their equivalent words. For example, `500` becomes `five hundred`, and `0.15` becomes `zero point fifteen`.
- Removes the following punctuation and special characters: `! @ # $ % ^ & * - + = ~ _ . , ; : ( ) < > [ ] { }`
-   Ignores phrases enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), and `{ }` (curly braces).
-   Converts tokens that include certain symbols to meaningful strings. For example, the service
    -   Converts a `$` (dollar sign) followed by a number to its string representation: `$100` becomes `one hundred dollars`.
    -   Converts a `%` (percent sign) preceded by a number to its string representation: `100%` becomes `one hundred percent`.

    This list is not exhaustive; the service makes similar adjustments for other characters as needed.

**For Spanish:**

-   Converts numbers to their equivalent words. For example, `500M` becomes `quinientos`, and `0,15` becomes `cero coma quince`.
-   Removes the following punctuation and special characters: `! ^ . , ; : ( ) < > [ ] { }`
-   Ignores phrases enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), and `{ }` (curly braces).
-   Converts tokens that include certain symbols to meaningful strings. For example, the service
    -   Converts a `$` (dollar sign) followed by a number to its string representation: `$100` becomes <code>cien d&oacute;lares</code> (or `cien pesos` if the dialect is `es-LA`).
    -   Converts a `%` (percent sign) preceded by a number to its string representation: `100%` becomes `cien por ciento`.

    This list is not exhaustive; the service makes similar adjustments for other characters as needed.

**For Japanese:**

-   Converts all characters to full-width characters.
-   Converts numbers to their equivalent words. For example, `500` becomes <code>&#20116;&#30334;</code>, and `0.15` becomes <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Does not convert tokens that include symbols to equivalent strings. For example, `100%` becomes <code>&#30334;&#65285;</code>.
-   Does not automatically remove punctuation. {{site.data.keyword.IBM_notm}} highly recommends that you remove punctuation if your application is transcription-based as opposed to dictation-based.

### What happens when you add a corpus file
{: #parseCorpus}

When you add a corpus file, the service analyzes its contents and automatically extracts any new words that it finds. New words are defined as those that are not already in the service's base vocabulary. Such words are referred to as out-of-vocabulary (OOV) words. The service adds each OOV word to the custom model's words resource.

The words resource contains the following information about each OOV word:

-   `word`: The spelling of the word found in the corpus.
-   `sounds_like`: How the service believes the word is pronounced based on its language rules. In many cases, it matches the spelling of the word, but you can modify the value of the field to change or augment the word's pronunciation; see [Modifying words in a model's words resource](#modifyWord).
-   `display_as`: The spelling of the word that the service uses in transcriptions. In most cases, it matches the spelling in the `word` field, but you can use the field to specify a different spelling for the word.
-   `source`: Where the word came from. If the service extracted the word from a corpus, the field lists the name of the corpus. Because the service can encounter the same word in multiple corpora, the field can list multiple corpus names. The field includes the string `user` if you modify the word in any way.

### Validating a custom model's words resource
{: #validateModel}

When you add a corpus to a custom language model, it is good practice to examine the OOV words in the model's words resource:

-   *Look for typographical and other errors.* Corpora can be large, and mistakes are easily made. Typos in a corpus file have the unintended consequence of adding new words to a model's words resource, as do ill-formed HTML tags left in the corpus.
-   *Verify the sounds-like pronunciations.* The service generates sounds-like pronunciations for OOV words automatically. Most of these pronunciations are sufficient, but for words that have unusual spellings or are difficult to pronounce, and for acronyms and technical terms, reviewing the pronunciations for accuracy is recommended.

To validate and, if necessary, correct the words for a custom model, use the following methods:

-   List all of the words from a custom model by using the `GET /v1/customizations/{customization_id}/words` method or query an individual word with the `GET /v1/customizations/{customization_id}/words/{word_name}` method; see [Listing words from a custom language model](/docs/services/speech-to-text/custom-methods.html#listWords). You can also use these methods to examine new custom words that you add directly to a custom model.
-   Modify words in a custom model to correct errors or to add data for a word by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method; see [Modifying words in a model's words resource](#modifyWord).
-   Delete extraneous words introduced in error (for example, by typographical or other mistakes in a corpus) by using the `DELETE /v1/customizations/{customization_id}/words/{word_name}` method; see [Deleting a word from a custom language model](/docs/services/speech-to-text/custom-methods.html#deleteWord). You can also update the corpus text file to correct the mistakes that caused the errors and then reload the corpus by using the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method; see [Adding corpora to a custom language model](#addCorpora).

## Adding words to a custom language model
{: #addWords}

Although adding corpora is the recommended means of adding words to a custom language model, you can also add custom words to the model directly. The service adds the custom words to the model's words resource, just as it does OOV words that it discovers from corpora. Add words individually when adding them via corpora is not practical or viable.

The simplest approach is to add a word with only its spelling, but you can also provide multiple pronunciations for the word and indicate how the word is to be displayed; see [Using the sounds_like field](#soundsLike) and [Using the display_as field](#displayAs). For information about how the service adds custom words to a model, see [What happens when you add a custom word](#parseWord).

You can use the following methods to add words to a custom model:

-   The `POST /v1/customizations/{customization_id}/words` method lets you add multiple words at one time. You pass a JSON object that provides information about each word via the body of the request. The following example adds two words, `HHonors` and `IEEE`, to the custom model with the specified ID. The `Content-Type` header specifies that JSON data is being passed to the method.

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"h honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"i triple e\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    This method is asynchronous. The time that it takes to complete depends on the number of words that you add and the current load on the service. For information about checking the status of the operation, see [Monitoring the add words request](#monitorWords).
-   The `PUT /v1/customizations/{customization_id}/words/{word_name}` method lets you add individual words. You pass a JSON object that provides information about the word. The following example adds the word `NCAA` to the model with the specified ID. The `Content-Type` header again indicates that JSON data is being passed to the method.

    ```bash
    curl -X PUT -u {username}:{password}
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    This method is synchronous. The service returns a response code that reports the success or failure of the request immediately.

As with adding corpora, it is a good idea to examine the new custom words in a model's words resource to check for typographical and other errors, especially when adding multiple words at once. For more information, see [Validating a custom model's words resource](#validateModel).

> **Note:** You can also use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to correct, modify, or augment words in a model's words resource; see [Modifying words in a model's words resource](#modifyWord).

### Monitoring the add words request
{: #monitorWords}

The service returns a 201 response code if the input data is valid. It then asynchronously pre-processes the words to add them to the model's words resource. The operation is generally faster than adding a corpus or training a model, but it can still take some time depending on the number of new words and the current load on the service. You cannot submit requests to add additional corpora or words to the custom model, or to train the model, until the service completes the request to add new words.

To determine the status of the request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns its current status, as in the following example:

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

The `status` field reports the current state of the model. While the service is processing new words, the status remains `pending`. Use a loop to check the status every 10 seconds until it becomes `ready` to indicate that the operation is complete.

You also use the `GET /v1/customizations/{customization_id}` method to check the status of a training operation. For more information, see [Monitoring the train model request](#monitorTraining).

### Using the sounds_like field
{: #soundsLike}

The `sounds_like` field specifies how a word is pronounced by speakers. By default, the service automatically completes the field with the word's spelling. You can use this field to provide as many as five alternative pronunciations for a word that is difficult to pronounce or that can be pronounced in different ways. For instance, you can do the following:

-   Provide different pronunciations for acronyms. For example, the acronym `NCAA` can be pronounced as it is spelled or colloquially as *N. C. double A.* A previous example adds both of these sounds-like pronunciations for the word.
-   Handle foreign words. For example, the French word <code>gar&ccedil;on</code> contains a character that is not found in the English language. You can specify a sounds-like of `gaarson`, replacing <code>&ccedil;</code> with `s`, to tell the service how English speakers would pronounce the word.

Speech recognition uses statistical algorithms to analyze audio, so adding a word does not guarantee that the service will transcode it with complete accuracy. When adding a word, consider how it might be pronounced; use the `sounds_like` field to provide a variety of pronunciations that reflect how a word can be spoken.

#### Language-specific considerations
{: #wordLanguages}

Follow these guidelines when specifying a sounds-like for a word in one of the supported languages.

**For US English:**

-   Use English alphabetic characters: `a-z` and `A-Z`.
-   To pronounce a single letter, use the letter followed by a period, for example, `N. C. A. A.` for the word `NCAA`. If the period is followed by another character, be sure to use a space between the period and the next character: use `N. C. A. A.`, *not* `N.C.A.A.`
-   Use real or made-up words that are pronounceable in the native language, for example, `shuchensnie` for the word `Sczcesny`.
-   Substitute equivalent English letters for non-English letters, for example, `s` for <code>&ccedil;</code> or `ny` for <code>&ntilde;</code>.
-   Substitute non-accented letters for accented letters, for example, `a` for <code>&agrave;</code> or `e` for <code>&egrave;</code>.
-   Use the spelling of numbers, for example, `seventy-five` for `75`.
-   You can include multiple words separated by spaces, but the service enforces a maximum of 40 total characters not including spaces.

**For Spanish:**

-   Use Spanish alphabetic characters: `a-z` and `A-Z` including valid Spanish accented letters.
-   To pronounce a single letter, use the letter followed by a period, for example, `N. C. A. A.` for the word `NCAA`. If the period is followed by another character, be sure to use a space between the period and the next character: use `N. C. A. A.`, *not* `N.C.A.A.`
-   Use real or made-up words that are pronounceable in the native language, for example, `shuchensni` for the word `Sczcesny`.
-   Use the spelling of numbers, for example, `setenta y cinco` for `75`.
-   You can include multiple words separated by spaces, but the service enforces a maximum of 40 total characters not including spaces.

**For Japanese:**

-   Use only full-width Katakana characters by using the <code>&#8213;</code> lengthen symbol (*chou-on*, or &#38263;&#38899;, in Japanese). Do not use half-width characters.
-   Use contracted sounds (*yoh-on*, or &#25303;&#38899;, in Japanese) only in the following syllable contexts:

    <code>&#12452;&#12455;, &#12454;&#12451;, &#12454;&#12455;, &#12454;&#12457;, &#12461;&#12451;, &#12461;&#12515;, &#12461;&#12517;, &#12461;&#12519;, &#12462;&#12515;, &#12462;&#12517;, &#12462;&#12519;, &#12463;&#12449;, &#12463;&#12451;, &#12463;&#12455;, &#12463;&#12457;, &#12464;&#12449;, &#12464;&#12457;, &#12471;&#12451;, &#12471;&#12455;, &#12471;&#12515;, &#12471;&#12517;, &#12471;&#12519;, &#12472;&#12451;, &#12472;&#12455;, &#12472;&#12515;, &#12472;&#12517;, &#12472;&#12519;, &#12473;&#12451;, &#12474;&#12451;, &#12481;&#12455;, &#12481;&#12515;, &#12481;&#12517;, &#12481;&#12519;, &#12482;&#12455;, &#12482;&#12515;, &#12482;&#12517;, &#12482;&#12519;, &#12484;&#12449;, &#12484;&#12451;, &#12484;&#12455;, &#12484;&#12457;, &#12486;&#12451;, &#12486;&#12517;, &#12487;&#12451;, &#12487;&#12515;, &#12487;&#12517;, &#12487;&#12519;, &#12488;&#12453;, &#12489;&#12453;, &#12491;&#12455;, &#12491;&#12515;, &#12491;&#12517;, &#12491;&#12519;, &#12498;&#12515;, &#12498;&#12517;, &#12498;&#12519;, &#12499;&#12515;, &#12499;&#12517;, &#12499;&#12519;, &#12500;&#12451;, &#12500;&#12515;, &#12500;&#12517;, &#12500;&#12519;, &#12501;&#12449;, &#12501;&#12451;, &#12501;&#12455;, &#12501;&#12457;, &#12501;&#12517;, &#12511;&#12515;, &#12511;&#12517;, &#12511;&#12519;, &#12522;&#12451;, &#12522;&#12455;, &#12522;&#12515;, &#12522;&#12517;, &#12522;&#12519;, &#12532;&#12449;, &#12532;&#12451;, &#12532;&#12455;, &#12532;&#12457;, &#12532;&#12517;</code>

-   Do not use <code>&#12531;</code> as the first character of a word. For example, use <code>&#12454;&#12540;&#12531;&#12488;</code> instead of <code>&#12531;&#12540;&#12488;</code>, the latter of which is invalid.
-   Use only the following syllables after an assimilated sound (*soku-on*, or &#20419;&#38899;, in Japanese):

    <code>&#12496;, &#12499;, &#12502;, &#12505;, &#12508;, &#12481;, &#12481;&#12455;, &#12481;&#12515;, &#12481;&#12517;, &#12481;&#12519;, &#12480;, &#12487;, &#12487;&#12451;, &#12489;, &#12489;&#12453;, &#12501;, &#12501;&#12449;, &#12501;&#12451;, &#12501;&#12455;, &#12501;&#12457;, &#12460;, &#12462;, &#12464;, &#12466;, &#12468;, &#12495;, &#12498;, &#12504;, &#12507;, &#12472;, &#12472;&#12455;, &#12472;&#12515;, &#12472;&#12517;, &#12472;&#12519;, &#12459;, &#12461;, &#12463;, &#12465;, &#12467;, &#12461;&#12515;, &#12461;&#12517;, &#12461;&#12519;, &#12497;, &#12500;, &#12503;, &#12506;, &#12509;, &#12500;&#12515;, &#12500;&#12517;, &#12500;&#12519;, &#12469;, &#12473;, &#12475;, &#12477;, &#12471;, &#12471;&#12455;, &#12471;&#12515;, &#12471;&#12517;, &#12471;&#12519;, &#12479;, &#12486;, &#12488;, &#12484;, &#12470;, &#12474;, &#12476;, &#12478;</code>

-   Many compound words consist of *prefix+noun* or *noun+suffix*. The service's base vocabulary covers most compound words that occur frequently (for example, <code>&#x9577;&#x96FB;&#x8A71;</code> and <code>&#x53E4;&#x65B0;&#x805E;</code>) but not those that occur infrequently. If your corpus commonly contains compound words, add them as one word as the first step of your customization. For example, <code>&#x53E4;&#x925B;&#x7B46;</code> is not common in general Japanese text; if you use it often, add it to your custom model to improve transcription accuracy.
-   Do not use a trailing assimilated sound.

### Using the display_as field
{: #displayAs}

The `display_as` field specifies how a word is displayed in a transcription. It is intended for cases where you want the service to display a string that is different from the word's spelling. For example, you can indicate that the word `hhonors` is to be displayed as `HHonors` regardless of whether it sounds like `hilton honors` or `h honors`.

```bash
curl -X PUT -u {username}:{password}
--header "Content-Type: application/json"
--data "{\"sounds_like": [\"hilton honors\", \"h honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

As another example, you can indicate that the word `IBM` is to be displayed as <code>IBM&trade;</code>.

<pre><code class="language-bash">curl -X PUT -u {username}:{password}
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

> **Note:** If you use the `smart_formatting` parameter (see [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting)) with a recognition request, be aware that the service applies smart formatting to a word before it considers the `display_as` field for the word. You might need to experiment with results to ensure that smart formatting does not interfere with how your custom words are displayed, and you might need to add custom words to accommodate the effects.

To illustrate, suppose you add the custom word `one` with a `display_as` field of `*one*`. Smart formatting changes the word `one` to the number `1`, and the display-as value is not applied. You could add a custom word for the number `1` and apply the same `display-as` field to that word.

### What happens when you add a custom word
{: #parseWord}

How the service handles the addition of a custom word depends on what values you provide and whether the word already exists in the service's base vocabulary.

<table>
  <caption>Table 2. Adding custom words with different fields</caption>
  <tr>
    <th style="width:20%; text-align:center">The <code>sounds_like</code> field is...</th>
    <th style="width:20%; text-align:center">The <code>display_as</code> field is...</th>
    <th style="vertical-align:bottom; text-align:left">Response</th>
  </tr>
  <tr>
    <td style="text-align:center">
      Not specified
    </td>
    <td style="text-align:center">
      Not specified
    </td>
    <td style="text-align:left">
      <em>If the word does not exist in the service's base vocabulary,</em>
      the service sets the <code>sound_like</code> field to its
      pronunciation of the word and sets the <code>display_as</code>
      field to the value of the <code>word</code> field.<br/><br/>
      <em>If the word exists in the service's base vocabulary,</em> the
      service leaves the <code>sounds_like</code> and
      <code>display_as</code> fields empty. These fields are empty only
      if the word already exists in the service's base vocabulary; the
      word's presence in the model's words resource is harmless but
      unnecessary.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">
      Specified
    </td>
    <td style="text-align:center">
      Not specified
    </td>
    <td style="text-align:left">
      <em>If the <code>sounds_like</code> field is valid,</em> the
      service sets the <code>display_as</code> field to the value of
      the <code>word</code> field.<br/><br/>
      <em>If the <code>sounds_like</code> field is invalid:</em>
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          The <code>POST
            /v1/customizations/{customization_id}/words</code>
          method adds an <code>error</code> field to the word in the
          model's words resource.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          The <code>PUT
            /v1/customizations/{customization_id}/words/{word_name}</code>
          method fails with a 400 response code and an error message;
          it does not add the word to the words resource.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">
      Not specified
    </td>
    <td style="text-align:center">
      Specified
    </td>
    <td style="text-align:left">
      <em>If the word does not exist in the service's base vocabulary,</em>
      the service sets the <code>sound_like</code> field to its
      pronunciation of the word and leaves the <code>display_as</code>
      field as specified.<br/><br/>
      <em>If the word exists in the service's base vocabulary,</em> the
      service leaves the <code>sounds_like</code> empty and leaves the
      <code>display_as</code> field as specified.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">
      Specified
    </td>
    <td style="text-align:center">
      Specified
    </td>
    <td style="text-align:left">
      <em>If the <code>sounds_like</code> field is valid,</em> the
      service sets the <code>sounds_like</code> and <code>display_as</code>
      fields to the specified values.<br/><br/>
      <em>If the <code>sounds_like</code> field is invalid,</em> the
      service responds as it does in the case where the
      <code>sounds_like</code> field is specified but the
      <code>display_as</code> field is not.
    </td>
  </tr>
</table>

You can list all of the words from a custom model or query individual words with methods of the customization API; see [Listing words from a custom language model](/docs/services/speech-to-text/custom-methods.html#listWords).

### Modifying words in a model's words resource
{: #modifyWord}

You can also use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to correct, modify, or augment a word in a model's words resource. You might need to use the methods to correct a typographical error or other mistake made when a word was added from a corpus or directly. You might also need to add sounds-like definitions for an existing word.

You use the methods to modify the definition of an existing word exactly as you do to add a new word. The new data that you provide for the word overrides the word's existing definition in a model's words resource. All of the rules for adding a new word apply to modifying an existing word.

## Training a custom language model
{: #trainModel}

Once you populate a custom language model with new words, either by adding corpora or by adding new words directly, you must train the model on the new data from its words resource. Training prepares the custom model to use the data in speech recognition. The model does not use words that you add via either means until you train it on the new data.

You train a custom model by using the `POST /v1/customizations/{customization_id}/train` method. You pass the method the customization ID of the model that you want to train, as in the following example:

```bash
curl -X POST -u {username}:{password}
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

You can use the optional `word_type_to_add` query parameter to specify the words from the words resource on which the custom model is to be trained:

-   Specify `all` or omit the parameter to train the model on all words from its words resource, regardless of their origin.
-   Specify `user` to train the model only on words that were added or modified by the user, ignoring OOV words that were extracted only from corpora. This is useful if you add corpora with noisy data, such as words that contain typographical errors. Before training the model on such data, you can use the `word_type` query parameter of the `GET /v1/customizations/{customization_id}/words` method to review words extracted from corpora, as described in [Listing words from a custom language model](/docs/services/speech-to-text/custom-methods.html#listWords).

The method is asynchronous. Training can take on the order of minutes to complete depending on the number of new words on which the model is being trained and the current load on the service. For information about checking the status of a training operation, see [Monitoring the train model request](#monitorTraining).

### Monitoring the train model request
{: #monitorTraining}

The service returns a 200 response code if the training process is successfully initiated. The service cannot accept subsequent training requests, or requests to add new corpora or words, until the existing request completes.

To determine the status of a training request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns its current status, as in the following example:

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
  "status": "training",
  "progress": 0
}
```
{: codeblock}

The response includes `status` and `progress` fields that report the current state of the custom model; the meaning of the `progress` field depends on the model's status. The `status` field can have one of the following values:

-   `pending` indicates that the model was created but is waiting either for training data to be added or for the service to finish analyzing data that was added. The `progress` field is `0`.
-   `ready` indicates that the model is ready to be trained. The `progress` field is `0`.
-   `training` indicates that the model is currently being trained. The `progress` field indicates the progress of the training as a percentage complete.

    > **Note:** The `progress` field does not currently reflect the progress of the training. The field changes from `0` to `100` when training is complete.
-   `available` indicates that the model is trained and ready to use. The `progress` field is `100`.
-   `failed` indicates that training of the model failed. The `progress` field is `0`.

Use a loop to check the status every 10 seconds until it becomes `available`. For more information about checking the status of a custom model, see [Listing custom language models](/docs/services/speech-to-text/custom-methods.html#listModels).

### Training failures
{: #failedTraining}

If the status of a custom model's training is `failed`, use methods of the customization API to examine the words in the model's words resource and fix any errors that you find; see [Listing words from a custom language model](/docs/services/speech-to-text/custom-methods.html#listWords). Training can fail to start for the following reasons:

-   No training data (corpora or words) have been added to the custom model since it was created or last trained.
-   Pre-processing of corpora to generate a list of OOV words is not complete.
-   Pre-processing of words to validate or auto-generate sounds-like pronunciations is not complete.
-   One or more words that were added to the custom model have invalid sounds-like pronunciations that you must fix.

Training can also fail if the service is currently handling another request for the custom model, such as a training request or a request to add a corpus text file or words to the model.

## Using a custom language model
{: #useModel}

Once you have created and trained your custom language model, you can use it in speech recognition requests with the service. You specify the customization ID (GUID) of the custom model that is to be used with a request via the `customization_id` query parameter, as shown in the following examples. You must issue the request with the service credentials of the owning service instance.

-   For the WebSocket interface, with the `recognize` method:

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize?watson-token=' + token
      + '&model=es-ES_BroadbandModel&customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    For more information, see [Using the WebSocket interface](/docs/services/speech-to-text/websockets.html).
-   For a sessionless request with the HTTP REST interface, with the `POST /v1/recognize` method:

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?customization_id={customization_id}"
    ```
    {: pre}

    For more information about sessionless requests, see [Making sessionless requests](/docs/services/speech-to-text/http.html#HTTP-sessionless).
-   For a session-based request with the HTTP REST interface, with the `POST /v1/sessions` method:

    ```bash
    curl -X POST -u {username}:{password}
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions?customization_id={customization_id}"
    ```
    {: pre}

    For more information about session-based requests, see [Making session-based requests](/docs/services/speech-to-text/http.html#HTTP-sessions).
-   For the HTTP asynchronous interface, with the `POST /v1/recognitions` method:

    ```bash
    curl -X POST -u {username}:{password}
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?customization_id={customization_id}"
    ```
    {: pre}

    For more information, see [Using the asynchronous HTTP interface](/docs/services/speech-to-text/async.html).

You can omit the base language model from the request if the custom model is based on the default language model, `en-US_BroadbandModel`; otherwise, you must use the `model` parameter to specify the base model, as shown for the WebSocket example. A custom model can be used only with the base language model for which it is created.

Speech recognition works the same either with or without a custom model. You can use all of the input and output parameters that are normally available with a recognition request. For more information, see [Input features and parameters](/docs/services/speech-to-text/input.html) and [Output features and parameters](/docs/services/speech-to-text/output.html).

## Example scripts
{: #exampleScripts}

You can use either of the following scripts to experiment with the steps for creating and working with a custom language model:

-   A Python script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>; see [Example Python script](#pythonScript) for more information.
-   A Bash shell script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>; see [Example shell script](#shellScript) for more information.

The two scripts provide identical functionality. Each script creates a custom model, adds words from a corpus text file, and adds both single and multiple words to the model directly. The script queries the words resource for the model to extract words added from a corpus and directly by the user. It also trains the model and demonstrates the polling that is recommended for monitoring the results of asynchronous operations. Both scripts show the results of the methods that they call.

You can use either of two provided corpus text files with the scripts, or you can test with your own files:

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a> is an abbreviated 6 KB corpus that adds six healthcare-related terms to the words resource of a model. This file produces a small amount of output when used with the script. The script uses this file by default.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a> is a richer 164 KB corpus that adds many healthcare-related terms to the words resource. This file produces much more output when used with the script.

By default, a new custom model that you create with either script is available for use with recognition requests. The scripts include an optional step for deleting a new custom model, which can be helpful when experimenting with the process; follow the comments in the scripts to enable the deletion step.

### Example Python script
{: #pythonScript}

Follow these steps to use the Python script:

1.  Download the Python script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.
1.  Download the example corpus text files to use with the script. You are free to test with either of the corpus text files or with a file of your own choosing. By default, all corpus text files must reside in the same directory as the script.
1.  The script uses the Python `requests` library for HTTP requests to the service. Use `pip` or `easy_install` to install the library for use by the script, for example:

    ```bash
    pip install requests
    ```
    {: pre}

    For more information about the library, see [pypi.python.org/pypi/requests ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://pypi.python.org/pypi/requests){: new_window}.
1.  Edit the script to replace the following two variables with the username and password from your {{site.data.keyword.speechtotextshort}} service credentials:

    ```
    username = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    password = "zzzzzzzzzzzz"
    ```
    {: screen}

    For more information, see [Service credentials for {{site.data.keyword.watson}} services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/common/getting-started-credentials.html){: new_window}. Note that you must use your service credentials, *not* your {{site.data.keyword.Bluemix_notm}} ID and password.
1.  Run the script by entering the following command:

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

### Example shell script
{: #shellScript}

Follow these steps to use the Bash shell script:

1.  Download the shell script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.
1.  Download the example corpus text files to use with the script. You are free to test with either of the corpus text files or with a file of your own choosing. By default, all corpus text files must reside in the same directory as the script.
1.  The script uses cURL for HTTP requests to the service. If you have not already downloaded the `curl` executable, you can install the version for your operating system from [curl.haxx.se ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://curl.haxx.se){: new_window}. Install the version that supports the Secure Sockets Layer (SSL) protocol. Make sure to include the installed binary file on your `PATH` environment variable.
1.  Edit the script to replace the following two variables with the username and password from your {{site.data.keyword.speechtotextshort}} service credentials:

    ```
    USERNAME="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    PASSWORD="zzzzzzzzzzzz"
    ```
    {: screen}

    For more information, see [Service credentials for {{site.data.keyword.watson}} services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/common/getting-started-credentials.html){: new_window}. Note that you must use your service credentials, *not* your {{site.data.keyword.Bluemix_notm}} ID and password.
1.  Run the script by entering the following command. Make sure the script has executable permissions before running it.

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
