---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-20"

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

# Working with corpora and custom words
{: #corporaWords}

The recommended means of populating a custom language model with words is to add one or more corpora to the model. When you add a corpus, the service analyzes the file and automatically adds any new words that it finds to the custom model. Adding a corpus to a custom model allows the service to extract domain-specific words in context, which helps ensure better transcription results. See [Working with Corpora](#workingCorpora).
{: shortdesc}

You can also add individual custom words to a model directly. The service adds words to the model just as it does words that it discovers from corpora. When you add a word directly, you can specify multiple pronunciations and indicate how the word is to be displayed. You can also update existing words to modify or augment the definitions extracted from a corpus. See [Working with custom words](#workingWords).

## The words resource
{: #wordsResource}

Each word that you add to a custom language model, either from a corpus or directly, is stored in the model's *words resource*. The purpose of the words resource is to define words that are not already present in the service's base vocabulary. The definitions tell the service how to transcribe the words. Such words are referred to as *out-of-vocabulary (OOV) words*. You can add a maximum of 30 thousand OOV words, including words that the service extracts from corpora and words that you add directly.

The words resource contains the following information about each OOV word. The service creates the definitions for words extracted from corpora. You specify the characteristics for words that you add directly.

-   `word`: The spelling of the word as found in a corpus or as added by you.
-   `sounds_like`: How the word is pronounced. For words extracted from corpora, the value represents how the service believes the word is pronounced based on its language rules. In many cases, the pronunciation reflects the spelling of the `word` field, but you can modify the value of the field to change the word's pronunciation. You can use the field to specify multiple pronunciations for a word. For more information, see [Using the sounds_like field](#soundsLike).
-   `display_as`: The spelling of the word that the service uses in transcriptions. The field indicates how the word is to be displayed. In most cases, the spelling matches the value of the `word` field, but you can use the `display_as` field to specify a different spelling for the word. For more information, see [Using the display_as field](#displayAs).
-   `source`: Where the word came from. If the service extracted the word from a corpus, the field lists the name of the corpus. Because the service can encounter the same word in multiple corpora, the field can list multiple corpus names. The field includes the string `user` if you add or modify the word in any way.

> **Note:** When you update a model's words resource, you must train the model for the changes to take effect during transcription. See [Train the custom language model](/docs/services/speech-to-text/custom.md#trainModel).

## Working with corpora
{: #workingCorpora}

You use the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method to add a corpus to a custom model. A corpus is a plain text file that contains sample sentences from your domain. The following example shows an abbreviated corpus for the healthcare domain; a corpus file is typically much longer.

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
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

Speech recognition relies on statistical algorithms to analyze audio. Words from a custom model are in competition with words from the service's base vocabulary as well as other words of the model. (Factors such as audio noise and speaker accents also affect the quality of transcription.)

The accuracy of transcription can depend largely on how words are defined in a model and how speakers say them. To improve the service's accuracy, use corpora to provide as many examples as possible of how OOV words are used in the domain. The more sentences you add that represent the context in which speakers use words from the domain, the better the service's recognition accuracy.

The service does not apply a simple word-matching algorithm. Its transcription depends on the context in which words are used. When it parses a corpus, the service includes information about n-grams (bi-grams, tri-grams, and so on) from the sentences of the corpus in the custom model. This information helps the service transcribe audio with greater accuracy, and it explains why training a custom model on corpora is more valuable than training it on custom words alone.

For example, accountants adhere to a common set of standards and procedures known as Generally Accepted Accounting Principles (GAAP). When creating a custom model for a financial domain, the more sentences you provide that use the term GAAP in context, the better the service can distinguish between general sentences such as "the gap between them is small" and domain-centric sentences such as "GAAP provides guidelines for measuring and disclosing financial information."

In general, it is better for corpora to use words in different contexts. However, if users speak the words in only one or two contexts, then showing the words in other contexts does not improve the quality of the custom model, since speakers never use the words in those contexts. If speakers are likely to use the same phrase frequently, then repeating that phrase in the corpora can improve the quality of the model. (In some cases, even adding a few custom words directly to a custom model can make a positive difference.)

### Preparing a corpus text file
{: #prepareCorpus}

Follow these guidelines to prepare a corpus text file:

-   Provide a plain text file that is encoded in UTF-8 if it contains non-ASCII characters. The service assumes UTF-8 encoding if it encounters such characters.
-   Include each sentence of the corpus on its own line, terminating each line with a carriage return. Including multiple sentences on the same line can degrade accuracy.
-   Use consistent capitalization for words in the corpus. The words resource is case-sensitive; mix upper- and lowercase letters and use capitalization only when intended.
-   Beware of typographical errors. The service assumes that typos are new words. Unless you correct them before training the model, the service adds them to the model's vocabulary. Remember the adage *Garbage in, garbage out!*

More sentences result in better accuracy, but the service does limit a model to a maximum of 10 million total words from all corpora combined, and no more than 30 thousand new (OOV) words, including words that the service extracts from corpora and words that you add directly.

### What happens when you add a corpus file
{: #parseCorpus}

When you add a corpus file, the service analyzes the file's contents, extracts any new (OOV) words that it finds, and adds each OOV word to the custom model's words resource. To distill the most meaning from the content, the service tokenizes and parses the data that it reads from a corpus file. The following sections describe how the service parses a corpus file for each supported language.

#### Parsing of US English
{: #corpusLanguages-enUS}

-   Converts numbers to their equivalent words. For example, `500` becomes `five hundred`, and `0.15` becomes `zero point fifteen`.
- Removes the following punctuation and special characters: `! ? @ # $ % ^ & * - + = ~ _ . , ; : ( ) < > [ ] { }`
-   Ignores phrases enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), or `{ }` (curly braces).
-   Converts tokens that include certain symbols to meaningful strings. For example, the service
    -   Converts a `$` (dollar sign) followed by a number to its string representation: `$100` becomes `one hundred dollars`.
    -   Converts a `%` (percent sign) preceded by a number to its string representation: `100%` becomes `one hundred percent`.

    This list is not exhaustive; the service makes similar adjustments for other characters as needed.

#### Parsing of Spanish
{: #corpusLanguages-esES}

-   Converts numbers to their equivalent words. For example, `500` becomes `quinientos`, and `0,15` becomes `cero coma quince`.
-   Removes the following punctuation and special characters: <code>! &iexcl; ? &iquest; ^ . , ; : ( ) < > [ ] { }</code>
-   Ignores phrases enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), or `{ }` (curly braces).
-   Converts tokens that include certain symbols to meaningful strings. For example, the service
    -   Converts a `$` (dollar sign) followed by a number to its string representation: `$100` becomes <code>cien d&oacute;lares</code> (or `cien pesos` if the dialect is `es-LA`).
    -   Converts a `%` (percent sign) preceded by a number to its string representation: `100%` becomes `cien por ciento`.

    This list is not exhaustive; the service makes similar adjustments for other characters as needed.

#### Parsing of Japanese
{: #corpusLanguages-jaJP}

-   Converts all characters to full-width characters.
-   Converts numbers to their equivalent words. For example, `500` becomes <code>&#20116;&#30334;</code>, and `0.15` becomes <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Does not convert tokens that include symbols to equivalent strings. For example, `100%` becomes <code>&#30334;&#65285;</code>.
-   Does not automatically remove punctuation. {{site.data.keyword.IBM_notm}} highly recommends that you remove punctuation if your application is transcription-based as opposed to dictation-based.

## Working with custom words
{: #workingWords}

You can use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to add new words to a model's words resource. You can also use the methods to modify or augment a word in a words resource.

You might, for instance, need to use the methods to correct a typographical error or other mistake made when a word was added from a corpus. You might also need to add sounds-like definitions for an existing word. If you modify an existing word, the new data that you provide overwrites the word's existing definition in the words resource. The rules for adding a new word also apply to modifying an existing word.

### Using the sounds_like field
{: #soundsLike}

The `sounds_like` field specifies how a word is pronounced by speakers. By default, the service automatically completes the field with the word's spelling. You can provide as many as five alternative pronunciations for a word that is difficult to pronounce or that can be pronounced in different ways. For instance, you can do the following:

-   Provide different pronunciations for acronyms. For example, the acronym `NCAA` can be pronounced as it is spelled or colloquially as *N. C. double A.* The following example adds both of these sounds-like pronunciations for the word `NCAA`:

    ```bash
    curl -X PUT -u {username}:{password}
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   Handle foreign words. For example, the French word <code>gar&ccedil;on</code> contains a character that is not found in the English language. You can specify a sounds-like of `gaarson`, replacing <code>&ccedil;</code> with `s`, to tell the service how English speakers would pronounce the word.

Speech recognition uses statistical algorithms to analyze audio, so adding a word does not guarantee that the service will transcode it with complete accuracy. When adding a word, consider how it might be pronounced; use the `sounds_like` field to provide a variety of pronunciations that reflect how a word can be spoken. The following sections provide language-specific guidelines for specifying a sounds-like.

#### Guidelines for US English
{: #wordLanguages-enUS}

-   Use English alphabetic characters: `a-z` and `A-Z`.
-   To pronounce a single letter, use the letter followed by a period. If the period is followed by another character, be sure to use a space between the period and the next character. For example, use `N. C. A. A.`, *not* `N.C.A.A.`
-   Use real or made-up words that are pronounceable in English, for example, `shuchensnie` for the word `Sczcesny`.
-   Substitute equivalent English letters for non-English letters, for example, `s` for <code>&ccedil;</code> or `ny` for <code>&ntilde;</code>.
-   Substitute non-accented letters for accented letters, for example, `a` for <code>&agrave;</code> or `e` for <code>&egrave;</code>.
-   Use the spelling of numbers, for example, `seventy-five` for `75`.
-   You can include multiple words separated by spaces, but the service enforces a maximum of 40 total characters not including spaces.

#### Guidelines for Spanish
{: #wordLanguages-esES}

-   Use Spanish alphabetic characters: `a-z` and `A-Z` including valid Spanish accented letters.
-   To pronounce a single letter, use the letter followed by a period. If the period is followed by another character, be sure to use a space between the period and the next character. For example, use `N. C. A. A.`, *not* `N.C.A.A.`
-   Use real or made-up words that are pronounceable in Spanish, for example, `shuchensni` for the word `Sczcesny`.
-   Use the spelling of numbers, for example, `setenta y cinco` for `75`.
-   You can include multiple words separated by spaces, but the service enforces a maximum of 40 total characters not including spaces.

#### Guidelines for Japanese
{: #wordLanguages-jaJP}

-   Use only full-width Katakana characters by using the <code>&#8213;</code> lengthen symbol (*chou-on*, or &#38263;&#38899;, in Japanese). Do not use half-width characters.
-   Use contracted sounds (*yoh-on*, or &#25303;&#38899;, in Japanese) only in the following syllable contexts:

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Use only the following syllables after an assimilated sound (*soku-on*, or &#20419;&#38899;, in Japanese):

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   Do not use <code>&#12531;</code> as the first character of a word. For example, use <code>&#12454;&#12540;&#12531;&#12488;</code> instead of <code>&#12531;&#12540;&#12488;</code>, the latter of which is invalid.
-   Many compound words consist of *prefix+noun* or *noun+suffix*. The service's base vocabulary covers most compound words that occur frequently (for example, <code>&#x9577;&#x96FB;&#x8A71;</code> and <code>&#x53E4;&#x65B0;&#x805E;</code>) but not those that occur infrequently. If your corpus commonly contains compound words, add them as one word as the first step of your customization. For example, <code>&#x53E4;&#x925B;&#x7B46;</code> is not common in general Japanese text; if you use it often, add it to your custom model to improve transcription accuracy.
-   Do not use a trailing assimilated sound.

### Using the display_as field
{: #displayAs}

The `display_as` field specifies how a word is displayed in a transcription. It is intended for cases where you want the service to display a string that is different from the word's spelling. For example, you can indicate that the word `hhonors` is to be displayed as `HHonors` regardless of whether it sounds like `hilton honors` or `h honors`.

```bash
curl -X PUT -u {username}:{password}
--header "Content-Type: application/json"
--data "{\"sounds_like": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

As another example, you can indicate that the word `IBM` is to be displayed as <code>IBM&trade;</code>.

<pre><code class="language-bash">curl -X PUT -u {username}:{password}
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### Interaction with smart formatting
{: #displaySmart}

If you use the `smart_formatting` parameter (see [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting)) with a recognition request, be aware that the service applies smart formatting to a word before it considers the `display_as` field for the word. You might need to experiment with results to ensure that smart formatting does not interfere with how your custom words are displayed, and you might need to add custom words to accommodate the effects.

To illustrate, suppose you add the custom word `one` with a `display_as` field of `one`. Smart formatting changes the word `one` to the number `1`, and the display-as value is not applied. To work around this issue, you could add a custom word for the number `1` and apply the same `display-as` field to that word.

### What happens when you add or modify a custom word
{: #parseWord}

How the service responds to a request to add or modify a custom word depends on what values you provide and whether the word already exists in the service's base vocabulary.

<table>
  <caption>Table 1. Adding custom words with different fields</caption>
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
      the service sets the <code>sounds_like</code> field to its
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
        <li style="margin:10px 0px; line-height:120%;">
          The <code>POST
            /v1/customizations/{customization_id}/words</code>
          method adds an <code>error</code> field to the word in the
          model's words resource.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
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
      the service sets the <code>sounds_like</code> field to its
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

## Validating a words resource
{: #validateModel}

Especially when you add a corpus to a custom language model or add multiple custom words at once, it is good practice to examine the OOV words in the model's words resource:

-   *Look for typographical and other errors.* Especially when adding corpora, which can be large, mistakes are easily made. Typos in a corpus file have the unintended consequence of adding new words to a model's words resource, as do ill-formed HTML tags left in the corpus.
-   *Verify the sounds-like pronunciations.* The service generates sounds-like pronunciations for OOV words automatically. Most of these pronunciations are sufficient, but for words that have unusual spellings or are difficult to pronounce, and for acronyms and technical terms, reviewing the pronunciations for accuracy is recommended.

To validate and, if necessary, correct a word for a custom model, regardless of how it was added to the words resource, use the following methods:

-   List all of the words from a custom model by using the `GET /v1/customizations/{customization_id}/words` method or query an individual word with the `GET /v1/customizations/{customization_id}/words/{word_name}` method. See [Listing words from a custom language model](/docs/services/speech-to-text/language-words.html#listWords).
-   Modify words in a custom model to correct errors or to add data for a word by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method. See [Working with custom words](#workingWords).
-   Delete extraneous words introduced in error (for example, by typographical or other mistakes in a corpus) by using the `DELETE /v1/customizations/{customization_id}/words/{word_name}` method. See [Deleting a word from a custom language model](/docs/services/speech-to-text/language-words.html#deleteWord).

    If the word was extracted from a corpus, you can also update the corpus text file to correct the error and then reload the corpus by using the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method. See [Add corpora to the custom language model](/docs/services/speech-to-text/language-create.html#addCorpora).
