---

copyright:
  years: 2015, 2022
lastupdated: "2022-03-29"

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Working with corpora and custom words for next-generation models
{: #corporaWords-ng}

This information is specific to custom models that are based on *next-generation models*. For information about corpora and custom words for custom models that are based on *previous-generation models*, see [Working with corpora and custom words for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords).
{: note}

You populate a custom language model with words by adding corpora to the model or by adding custom words directly to the model. You use the same methods and operations for both previous- and next-generation models. For more information about adding corpora and custom words to a model, see [Working with corpora for next-generation models](#workingCorpora-ng) and [Working with custom words for next-generation models](#workingWords-ng).

Although language model customization is similar in usage and intent for previous- and next-generation models, there are differences between the two types of models at the implementation level. To understand how language model customization works for next-generation models, and how you can make the best use of customization, you need a high-level understanding of the differences.

-   When you create and use a custom language model that is based on a *previous-generation model*, the service relies on words from the custom model to create transcripts that contain domain-specific terms. In combination with words from its base vocabulary, the service uses these words from the custom model to predict and transcribe speech from audio. You provide the information for a custom language model in the form of corpora, custom words, and grammars. The service stores this information in the words resource for the custom model.

-   When you create a custom language model that is based on a *next-generation model*, the services relies on sequences of characters from the custom model to create transcripts that reflect domain-specific terms. In combination with character sequences from its base vocabulary, the service uses these extended series of characters from the custom model to predict and transcribe speech from audio.

    You provide the information for a custom language model in the form of corpora and custom words. But rather than rely on a words resource that contains these words, the service extracts and stores character sequences from the corpora and custom words. The service does not extract words from corpora, and it does not calculate out-of-vocabulary (OOV) words from corpora and custom words. The words resource is simply where it stores custom words that you add directly to the model.

When you develop a custom language model based on a next-generation model, you must still provide corpora and custom words to train the model on domain-specific terminology. So the process of creating and training a custom model is largely the same for next-generation and previous-generation models.

The following sections describe the rules for providing corpora and custom words for a custom language model that is based on a next-generation model. The rules are similar to those for working with a custom model based on a previous-generation model. But some differences do exist, and some features are not available.

## The words resource
{: #wordsResource-ng}

The *words resource* includes custom words that you add directly to the custom model. The words resource contains the following information about each custom word:

-   `word` - The spelling of the word as added by you.
-   `display_as` - The spelling of the word that the service uses in transcripts. The field indicates how the word is to be displayed. Unless you specify an alternative representation, the spelling matches the value of the `word` field.
-   `source` - How the word was added to the words resource. The field always contains the string `user` to indicate that it was added directly as a custom word.
-   `sounds_like` - The pronunciation of the word. The value represents how the service believes that the word is pronounced based on its language rules. In many cases, the pronunciation reflects the spelling of the `word` field. You can use the `sounds_like` field to modify the word's pronunciation. You can also use the field to specify multiple pronunciations for a word.

<!--
For more information, see [Using the sounds_like field](#soundsLike).
-->

After adding or modifying a custom word, it is important that you verify the correctness of the word's definition; for more information, see [Validating a words resource for next-generation models](#validateModel-ng). You must also train the model for the changes to take effect during transcription; for more information, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

## How much data do I need?
{: #wordsResourceAmount-ng}

Many factors contribute to the amount of data that you need for an effective custom language model. It is not possible to indicate the exact amount of data that you need to add for any custom model or application. Depending on the use case, even adding a few words directly to a custom model can improve the model's quality. But adding corpora that show the words in the context in which they are used can greatly improve transcription accuracy.

You can add a maximum of 10 million total words to a custom model from all sources. This figure includes all words that are included in corpora and that you add directly. The service uses all of the words from a corpus to learn the context in which sequences of characters can occur, which is why corpora are a more effective means of improving recognition accuracy.

Adding a large number of corpora and words can increase the latency of speech recognition, but the exact effect is difficult to quantify or predict. As with the amount of data that is needed to produce an effective custom model, the performance impact of a large amount of data depends on many factors. Test your custom model with different amounts of data to determine the performance of your models.

## Working with corpora for next-generation models
{: #workingCorpora-ng}

You use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add a corpus to a custom model. A corpus is a plain text file that contains sample sentences from your domain. The following example shows an abbreviated corpus for the healthcare domain. A corpus file is typically much longer.

```text
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
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

Words from a custom model are in competition with words in the service's base vocabulary as well as other words of the model. (Factors such as audio noise and speaker accents also affect the quality of transcription.)

The accuracy of transcription can depend largely on the data that you add to a model and how speakers say words in audio. To improve the service's accuracy, use corpora to provide as many examples as possible of how words are used in a domain. Repeating the words in corpora can improve the quality of a custom language model. How you duplicate the words in corpora depends on how you expect users to say them in the audio that is to be recognized. The more sentences that you add that represent the context in which speakers use words from the domain, the better the service's recognition accuracy.

For example, accountants adhere to a common set of standards and procedures that are known as Generally Accepted Accounting Principles (GAAP). When you create a custom model for a financial domain, provide sentences that use the term GAAP in context. The sentences help the service distinguish between general phrases such as "the gap between them is small" and domain-centric phrases such as "GAAP provides guidelines for measuring and disclosing financial information."

In general, it is better for corpora to use words in different contexts, which can improve how the service learns the phrases. However, if users speak the words in only a couple of contexts, then showing the words in other contexts does not improve the quality of the custom model. Speakers never use the words in those contexts. If speakers are likely to use the same phrase frequently, repeating that phrase in the corpora can improve the quality of the model. In some cases, even adding a few custom words directly to a custom model can make a positive difference.

### Preparing a corpus text file
{: #prepareCorpus-ng}

Follow these guidelines to prepare a corpus text file:

-   Provide a plain text file that is encoded in UTF-8 if it contains non-ASCII characters. The service assumes UTF-8 encoding if it encounters such characters.

    Make sure that you know the character encoding of your corpus text files. The service preserves the encoding that it finds in the text files. You must use that same encoding when working with custom words in the custom model. For more information, see [Character encoding for custom words](/docs/speech-to-text?topic=speech-to-text-manageWords#charEncoding).
    {: important}

-   Use consistent capitalization for words in the corpus. Mix upper- and lowercase letters and use capitalization only when intended.
-   Include each sentence of the corpus on its own line, and terminate each line with a carriage return. Including multiple sentences on the same line can degrade accuracy.
-   Add personal names as discrete units on separate lines. Do not add the individual elements of a name on separate lines or as individual custom words, and do not include multiple names on the same line of a corpus. The following example shows the correct way to improve recognition accuracy for three names:

    ```text
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Include additional contextual information where appropriate, for example, `Doctor Sebastian Leifson` or `President Malcolm Ingersol`. As with all words, duplicating the names multiple times can improve recognition accuracy.
-   Beware of typographical errors. The service assumes that typographical errors are new words. Remember the adage *Garbage in, garbage out!*
-   More sentences result in better accuracy. But the service does limit a model to a maximum of 10 million total words from all sources combined.

### What happens when I add a corpus file?
{: #parseCorpus-ng}

When you add a corpus file, the service analyzes the file's contents. To distill the most meaning from the content, the service tokenizes and parses the data that it reads from a corpus file. The following sections describe how the service parses a corpus file for each supported language.

Information for Arabic, Chinese, Czech, and Hindi is not yet available. If you need this information for your custom language model, contact your IBM Support representative.
{: note}

#### Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish
{: #corpusLanguages-ng}

The following descriptions apply to all supported dialects of Dutch, English, French, German, Italian, Portuguese, and Spanish.

-   Converts numbers to their equivalent words.

    | Language   | Whole number | Decimal number |
    |------------|:------------:|:--------------:|
    | Dutch      | `500` becomes `vijfhonderd` | `0,15` becomes `nul komma vijftien` |
    | English    | `500` becomes `five hundred` | `0.15` becomes `zero point fifteen` |
    | French     | `500` becomes `cinq cents` | `0,15` becomes `zéro virgule quinze` |
    | German     | `500` becomes `fünfhundert` | `0,15` becomes `null punkt fünfzehn` |
    | Italian    | `500` becomes `cinquecento` | `0,15` becomes `zero virgola quindici` |
    | Portuguese | `500` becomes `quinhentos` | `0,15` becomes `zero ponto quinze` |
    | Spanish    | `500` becomes `quinientos` | `0,15` becomes `cero coma quince` |
    {: caption="Table 1. Examples of number conversion"}

-   Converts tokens that include certain symbols to meaningful string representations. These examples are not exhaustive. The service makes similar adjustments for other characters as needed.

    | Language | A dollar sign and a number | A euro sign and a number | A percent sign and a number |
    |---------------|:-------------------------------:|:---------------------------:|:-------------------------------:|
    | Dutch      | `$100` becomes `honderd dollar` | `€100` becomes `honderd euro` | `100%` becomes `honderd procent` |
    | English    | `$100` becomes `one hundred dollars` | `€100` becomes `one hundred euros` | `100%` becomes `one hundred percent` |
    | French     | `$100` becomes `cent dollars` | `€100` becomes `cent euros`   | `100%` becomes `cent pour cent` |
    | German     | `$100` and `100$` become `einhundert dollar` | `€100` and `100€` become `einhundert euro` | `100%` becomes `einhundert prozent` |
    | Italian    | `$100` becomes `cento dollari` | `€100` becomes `cento euro` | `100%` becomes `cento per cento` |
    | Portuguese | `$100` and `100$` become `cem dólares` | `€100` and `100€` become `cem euros` | `100%` becomes `cem por cento` |
    | Spanish    | `$100` and `100$` become `cien dólares` | `€100` and `100€` become `cien euros` | `100%` becomes `cien por ciento` |
    {: caption="Table 2. Examples of symbol conversion"}

-   Processes non-alphanumeric, punctuation, and special characters depending on their context. For example, the service removes a `$` (dollar sign) or `€` (euro symbol) unless it is followed by a number. Processing is context-dependent and consistent across the supported languages.
-   Ignores phrases that are enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), or `{ }` (curly braces).

#### Parsing of Japanese
{: #corpusLanguages-jaJP-ng}

-   Converts all characters to full-width characters.
-   Converts numbers to their equivalent words, for example, `500` becomes <code>&#20116;&#30334;</code>, and `0.15` becomes <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Does not convert tokens that include symbols to equivalent strings, for example, `100%` becomes <code>&#30334;&#65285;</code>.
-   Does not automatically remove punctuation. {{site.data.keyword.IBM_notm}} highly recommends that you remove punctuation if your application is transcription-based as opposed to dictation-based.

#### Parsing of Korean
{: #corpusLanguages-koKR-ng}

-   Converts numbers to their equivalent words, for example, `10` becomes <code>&#49901;</code>.
-   Removes the following punctuation and special characters: `- ( ) * : . , ' "`. However, not all punctuation and special characters that are removed for other languages are removed for Korean, for example:
    -   Removes a period (`.`) symbol only when it occurs at the end of a line of input.
    -   Does not remove a tilde (`~`) symbol.
    -   Does not remove or otherwise process Unicode wide-character symbols, for example, <code>&#8230;</code> (triple dot or ellipsis).

    In general, {{site.data.keyword.IBM_notm}} recommends that you remove punctuation, special characters, and Unicode wide-characters before you process a corpus file.
-   Does not remove or ignore phrases that are enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), or `{ }` (curly braces).
-   Converts tokens that include certain symbols to meaningful string representations, for example:
    -   `24%` becomes <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>.
    -   `$10` becomes <code>&#49901;&#45804;&#47084;</code>.

    This list is not exhaustive. The service makes similar adjustments for other characters as needed.
-   For phrases that consist of Latin (English) characters or a mix of Hangul and Latin characters, the service uses the phrases exactly as they appear in the corpus file.

## Working with custom words for next-generation models
{: #workingWords-ng}

You can use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to add new words to a custom model. You can also use the methods to modify or augment a custom word.

You might, for instance, need to use the methods to correct a typographical error or other mistake that is made when a word is added to a custom model. If you modify an existing word, the new data that you provide overwrites the word's existing definition in the words resource. The rules for adding a word also apply to modifying an existing word.

You are likely to add most custom words from corpora. Make sure that you know the character encoding of your corpus text files. The service preserves the encoding that it finds in the text files. You must use that same encoding when working with custom words in the custom model. For more information, see [Character encoding for custom words](/docs/speech-to-text?topic=speech-to-text-manageWords#charEncoding).
{: important}

### Using the display_as field
{: #displayAs-ng}

The `display_as` field specifies how a word is displayed in a transcript. It is intended for cases where you want the service to display a string that is different from the word's spelling. For example, you can indicate that the word `hhonors` is to be displayed as `HHonors`.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"display_as\": \"HHonors\"}" \
"{url}/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"display_as\": \"HHonors\"}" \
"{url}/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

As another example, you can indicate that the word `IBM` is to be displayed as `IBM™`.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"display_as\":\"IBM™\"}" \
"{url}/v1/customizations/{customization_id}/words/IBM"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"display_as\":\"IBM™\"}" \
"{url}/v1/customizations/{customization_id}/words/IBM"
```
{: pre}

#### Interaction with smart formatting and numeric redaction
{: #displaySmart-ng}

If you use the `smart_formatting` or `redaction` parameters with a recognition request, be aware that the service applies smart formatting and redaction to a word before it considers the `display_as` field for the word. You might need to experiment with results to ensure that the features do not interfere with how your custom words are displayed. You might also need to add custom words to accommodate the effects.

For instance, suppose that you add the custom word `one` with a `display_as` field of `one`. Smart formatting changes the word `one` to the number `1`, and the display-as value is not applied. To work around this issue, you could add a custom word for the number `1` and apply the same `display_as` field to that word.

For more information about working with these features, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting) and [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction).

### What happens when I add or modify a custom word?
{: #parseWord-ng}

How the service responds to a request to add or modify a custom word depends on whether you specify the `display_as` field and on whether the word exists in the service's base vocabulary:

-   When you omit the `display_as` field:
    -   *If the word does not exist in the service's base vocabulary,* the service sets the `display_as` field to the value of the `word` field.
    -   *If the word exists in the service's base vocabulary,* the service leaves the `display_as` field empty. The field is empty only if the word exists in the service's base vocabulary. The word's presence in the model's words resource is harmless but unnecessary.
-   When you specify the `display_as` field:
    -   *Regardless of whether the word exists in the service's base vocabulary,* the service sets the `display_as` field as specified in the request.

## Validating a words resource for next-generation models
{: #validateModel-ng}
{: troubleshoot}
{: support}

Especially when you add a corpus to a custom language model or add multiple custom words at one time, make sure to perform the following validation:

-   *Look for typographical and other errors in corpora.* Especially when you add corpora, which can be large, mistakes are easily made. Make sure to review the corpus closely before adding it to the model.
-   *Look for typographical and other errors in custom words.* Make sure to review closely custom words that you add directly to a model.

Typographical errors have the unintended consequence of modifying a custom model for nonexistent words, as do ill-formed HTML tags that are left in a corpus file.

To validate and, if necessary, correct a custom word for a custom model, use the following methods:

-   List all of the words from a custom model by using the `GET /v1/customizations/{customization_id}/words` method or query an individual word with the `GET /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   Modify words in a custom model to correct errors or to add display-as values by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Working with custom words for next-generation models](#workingWords).
-   Delete extraneous words that are introduced in error (for example, by typographical or other mistakes) by using the `DELETE /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Deleting a word from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#deleteWord).
