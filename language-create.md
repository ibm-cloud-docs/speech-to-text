---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-17"

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

# Creating a custom language model
{: #languageCreate}

Follow these steps to create a custom language model for the {{site.data.keyword.speechtotextshort}} service:
{: shortdesc}

1.  [Create a custom language model](#createModel). You can create multiple custom models for the same or different domains. The process is the same for any model that you create. However, you can specify only a single custom model at a time with a recognition request.
1.  [Add corpora to the custom language model](#addCorpora). A corpus is a plain text document that uses terminology from the domain in context. The service builds a vocabulary for a custom model by extracting terms from corpora that do not exist in its base vocabulary. You can add multiple corpora to a custom model.
1.  [Add words to the custom language model](#addWords). You can also add custom words to a model individually. In addition, you can use the same methods to modify custom words that are extracted from corpora. The methods let you specify the pronunciation of words and how the words are displayed in a speech transcript.
1.  [Train the custom language model](#trainModel). Once you add words to the custom model (from corpora, individually, or both), you must train the model on the custom words. Training prepares the custom model for use in speech recognition. The model does not use new or modified words until you train it.
1.  After you train your custom model, you can use it with recognition requests. If the audio passed for transcription contains domain-specific words that are defined in the custom model, the results of the request reflect the enhanced vocabulary available with the custom model. For more information, see [Using a custom language model](/docs/services/speech-to-text/language-use.html).

The steps for creating a custom language model are iterative. You can add corpora, add words, and train or retrain a model as often as needed. The recommended approach is to add corpora to the model, since providing sentences allows the service to learn the words in context. You can then augment or modify the model's words individually. Training a model only on individual words as opposed to words added from corpora is more time-consuming and can produce less effective results.

> **Note:** Language model customization is available for only some languages and at different levels of support; see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

## Create a custom language model
{: #createModel}

You use the `POST /v1/customizations` method to create a new custom language model. You can create any number of custom language models, but you can use only one model at a time with a speech recognition request. The method accepts a JSON object that defines the attributes of the new custom model as the body of the request.

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
      custom model. By default, the dialect matches the language of the
      base model; for example, the dialect is <code>en-US</code>
      for either of the US English language models,
      <code>en-US_BroadbandModel</code> or
      <code>en-US_NarrowbandModel</code>.<br/></br>
      The parameter is meaningful only for Spanish models, for which
      the service creates a custom model that is suited for speech in
      the indicated dialect:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-ES</code> for Castilian Spanish (the default)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-LA</code> for Latin-American Spanish
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-US</code> for North-American (Mexican) Spanish
        </li>
      </ul>
      If you specify a dialect, it must be valid for the base model.
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
curl -X POST -u "{username}:{password}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

The example returns the customization ID of the new model. Each custom model is identified by a unique customization ID, which is a Globally Unique Identifier (GUID). You specify a custom model's GUID with the `customization_id` parameter of calls that are associated with the model.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

The new custom model is owned by the service instance whose credentials are used to create it. For more information, see [Ownership of custom models](/docs/services/speech-to-text/custom.html#customOwner).

## Add corpora to the custom language model
{: #addCorpora}

Once you create your custom language model, the next step is to add data (domain-specific words) to it. The recommended means of adding data to a custom model is to add one or more corpora to the model. A corpus is a plain text file that ideally contains sample sentences from your domain. The service parses the file's contents and extracts any words that are not in its base vocabulary. Such words are referred to out-of-vocabulary (OOV) words. For more information, see [Working with corpora](/docs/services/speech-to-text/language-resource.html#workingCorpora). For more information about how the service adds corpora to a model, see [What happens when you add a corpus file](/docs/services/speech-to-text/language-resource.html#parseCorpus).

You use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add a corpus to a custom model:

-   Specify the customization ID of the custom model with the `customization_id` path parameter.
-   Specify a name for the corpus with the `corpus_name` path parameter. Use a localized name that matches the language of the custom model and reflects the contents of the corpus.
    -   Include a maximum of 128 characters in the name.
    -   Do not include spaces, `/` (slashes), or `\` (backslashes) in the name.
    -   Do not use the name of a corpus that has already been added to the custom model.
    -   Do not use the name `user`, which is reserved by the service to denote custom words that are added or modified by the user.
-   Pass the corpus text file as the body of the request.

The following example adds the corpus text file `healthcare.txt` to the custom model with the specified ID. The example names the corpus `healthcare`.

```bash
curl -X POST -u "{username}:{password}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

The method also accepts an optional `allow_overwrite` query parameter that overwrites an existing corpus for a custom model. Use the parameter if you need to update a corpus file after you add it to a model.

The method is asynchronous. It can take on the order of a minute or two to complete. The duration of the operation depends on the total number of words in the corpus, the number of new words that the service finds in the corpus, and the current load on the service. For more information about checking the status of a corpus, see [Monitoring the add corpus request](#monitorCorpus).

You can add any number of corpora to a custom model by calling the method once for each corpus text file. The addition of one corpus must be fully complete before you can add another. After you add a corpus to a custom model, examine the new custom words to check for typographical and other errors. For more information, see [Validating a words resource](/docs/services/speech-to-text/language-resource.html#validateModel).

### Monitoring the add corpus request
{: #monitorCorpus}

The service returns a 201 response code if the corpus is valid. It then asynchronously processes the contents of the corpus and automatically extracts new words. You cannot submit requests to add more corpora or words to a custom model, or to train the model, until the service's analysis of the corpus for the current request completes.

To determine the status of the analysis, use the `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method to poll the status of the corpus. The method accepts the ID of the model and the name of the corpus, as shown in the following example:

```bash
curl -X GET -u "{username}:{password}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
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

Use a loop to check the status of the corpus every 10 seconds until it becomes `analyzed`. For more information about checking the status of a model's corpora, see [Listing corpora for a custom language model](/docs/services/speech-to-text/language-corpora.html#listCorpora).

## Add words to the custom language model
{: #addWords}

Although adding corpora is the recommended means of adding words to a custom language model, you can also add individual custom words to the model directly. The service adds the custom words to the custom model just as it does OOV words that it discovers from corpora. If you have only one or a few words to add to a model, using corpora to add the words is not practical or viable. For more information, see [Working with custom words](/docs/services/speech-to-text/language-resource.html#workingWords).

The simplest approach is to add a word with only its spelling. But you can also provide multiple pronunciations for the word and indicate how the word is to be displayed. For more information, see [Using the sounds_like field](/docs/services/speech-to-text/language-resource.html#soundsLike) and [Using the display_as field](/docs/services/speech-to-text/language-resource.html#displayAs). For more information about how the service adds custom words to a model, see [What happens when you add a custom word](/docs/services/speech-to-text/language-resource.html#parseWord).

You can use the following methods to add words to a custom model:

-   The `POST /v1/customizations/{customization_id}/words` method adds multiple words at one time. You pass a JSON object that provides information about each word via the body of the request. The following example adds two custom words, `HHonors` and `IEEE`, to the custom model with the specified ID. The `Content-Type` header specifies that JSON data is being passed to the method.

    ```bash
    curl -X POST -u "{username}:{password}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    You can also add the words from a file. For example, the file `words.json` defines the same two custom words.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    The following command adds the words from the file:

    ```bash
    curl -X POST -u "{username}:{password}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    This method is asynchronous. The time that it takes to complete depends on the number of words that you add and the current load on the service. For more information about checking the status of the operation, see [Monitoring the add words request](#monitorWords).
-   The `PUT /v1/customizations/{customization_id}/words/{word_name}` method adds individual words. You pass a JSON object that provides information about the word. The following example adds the word `NCAA` to the model with the specified ID. The `Content-Type` header again indicates that JSON data is being passed to the method.

    ```bash
    curl -X PUT -u "{username}:{password}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    This method is synchronous. The service returns a response code that reports the success or failure of the request immediately.

As with adding corpora, examine the new custom words to check for typographical and other errors. This check is especially important when you add multiple words at once. For more information, see [Validating a words resource](/docs/services/speech-to-text/language-resource.html#validateModel).

### Monitoring the add words request
{: #monitorWords}

When you use the `POST /v1/customizations/{customization_id}/words` method, the service returns a 201 response code if the input data is valid. It then asynchronously processes the words to add them to the model. The operation is generally faster than adding a corpus or training a model, but it can still take some time. The duration of the operation depends on the number of new words and the current load on the service. You cannot submit requests to add more corpora or words to the custom model, or to train the model, until the service completes the request to add new words.

To determine the status of the request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns information that includes the model's status, as in the following example:

```bash
curl -X GET -u "{username}:{password}"
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

The `status` field reports the current state of the model. While the service is processing new words, the status remains `pending`. Use a loop to check the status every 10 seconds until it becomes `ready` to indicate that the operation is complete. For more information about possible `status` values, see [Monitoring the train model request](#monitorTraining).

### Modifying words in a custom model
{: #modifyWord}

You can also use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to modify or augment a word in a custom model. You might need to use the methods to correct a typographical error or other mistake that was made when a word was added to the model. You might also need to add sounds-like definitions for an existing word.

You use the methods to modify the definition of an existing word exactly as you do to add a word. The new data that you provide for the word overwrites the word's existing definition.

## Train the custom language model
{: #trainModel}

Once you populate a custom language model with new words, either by adding corpora or by adding the words directly, you must train the model on the new data. Training prepares the custom model to use the data in speech recognition. The model does not use words that you add via either means until you train it on the new data.

You use the `POST /v1/customizations/{customization_id}/train` method to train a custom model. You pass the method the customization ID of the model that you want to train, as in the following example:

```bash
curl -X POST -u "{username}:{password}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

You can use the optional `word_type_to_add` query parameter to specify the words on which the custom model is to be trained:

-   Specify `all` or omit the parameter to train the model on all of its words, regardless of their origin.
-   Specify `user` to train the model only on words that were added or modified by the user, ignoring words that were extracted only from corpora. This option is useful if you add corpora with noisy data, such as words that contain typographical errors. Before training the model on such data, you can use the `word_type` query parameter of the `GET /v1/customizations/{customization_id}/words` method to review words that are extracted from corpora. For more information, see [Listing words from a custom language model](/docs/services/speech-to-text/language-words.html#listWords).

In addition, you can use the optional `customization_weight` query parameter. The parameter specifies the relative weight that is given to words from the custom model as opposed to words from the base vocabulary when the custom model is used for speech recognition. You can also specify a customization weight with any recognition request that uses the custom model. For more information, see [Using customization weight](/docs/services/speech-to-text/language-use.html#weight).

The method is asynchronous. Training can take on the order of minutes to complete depending on the number of new words on which the model is being trained and the current load on the service. For more information about checking the status of a training operation, see [Monitoring the train model request](#monitorTraining).

### Monitoring the train model request
{: #monitorTraining}

The service returns a 200 response code if the training process is successfully initiated. The service cannot accept subsequent training requests, or requests to add new corpora or words, until the existing request completes.

To determine the status of a training request, use the `GET /v1/customizations/{customization_id}` method to poll the model's status. The method accepts the customization ID of the model and returns information about the model.

```bash
curl -X GET -u "{username}:{password}"
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

The response includes `status` and `progress` fields that report the state of the custom model. The meaning of the `progress` field depends on the model's status. The `status` field can have one of the following values:

-   `pending` indicates that the model was created but is waiting either for training data to be added or for the service to finish analyzing data that was added. The `progress` field is `0`.
-   `ready` indicates that the model is ready to be trained. The `progress` field is `0`.
-   `training` indicates that the model is being trained. The `progress` field changes from `0` to `100` when training is complete. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indicates that the model is trained and ready to use. The `progress` field is `100`.
-   `upgrading` indicates that the model is being upgraded. The `progress` field is `0`.
-   `failed` indicates that training of the model failed. The `progress` field is `0`.

Use a loop to check the status every 10 seconds until it becomes `available`. For more information about checking the status of a custom model, see [Listing custom language models](/docs/services/speech-to-text/language-models.html#listModels).

### Training failures
{: #failedTraining}

Training of a custom language model fails to start if the service is handling another request for the custom model. Such requests can include another training request or a request to add a corpus or words to the model. For instance, a training request fails to start if the service is

-   Processing a corpus to generate a list of OOV words
-   Processing words to validate or auto-generate sounds-like pronunciations

Training can also fail to start for the following reasons:

-   No training data (corpora or words) was added to the custom model since it was created or last trained.
-   One or more words that were added to the custom model have invalid sounds-like pronunciations that you must fix.

If the status of a custom model's training is `failed`, use methods of the customization API to examine the model's words and fix any errors that you find. For more information, see [Validating a words resource](/docs/services/speech-to-text/language-resource.html#validateModel).

## Example scripts
{: #exampleScripts}

You can use the following scripts to experiment with the steps for creating a custom language model:

-   A Python script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>. For more information, see [Example Python script](#pythonScript).
-   A Bash shell script named <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>. For more information, see [Example shell script](#shellScript).

The two scripts provide identical functionality. Each script creates a custom model, adds words from a corpus text file, and adds both single and multiple words to the model directly. The script queries the model to list words added from a corpus and directly by the user. It also trains the model and demonstrates the polling that is recommended for monitoring the results of asynchronous operations.

You can use either of two provided corpus text files with the scripts, or you can test with your own corpus files:

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a> is an abbreviated 6 KB corpus that adds six healthcare-related terms to a model. This file produces a small amount of output when used with the scripts. The scripts use this file by default.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a> is a richer 164 KB corpus that adds many healthcare-related terms to a model. This file produces much more output when used with the scripts.

By default, the new custom model that you create with either script is available for use with recognition requests. The scripts include an optional step for deleting the new custom model, which can be helpful when you are experimenting with the process. Follow the comments in the scripts to enable the deletion step.

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
    {: codeblock}

    For more information, see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html).
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
1.  The script uses the `curl` command for HTTP requests to the service. If you have not already downloaded `curl`, you can install the version for your operating system from [curl.haxx.se ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://curl.haxx.se){: new_window}. Install the version that supports the Secure Sockets Layer (SSL) protocol, and make sure to include the installed binary file on your `PATH` environment variable.
1.  Edit the script to replace the following two variables with the username and password from your {{site.data.keyword.speechtotextshort}} service credentials:

    ```
    USERNAME="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    PASSWORD="zzzzzzzzzzzz"
    ```
    {: codeblock}

    For more information, see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html).
1.  Make sure that the script has executable permissions, and then run the script by entering the following command:

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
