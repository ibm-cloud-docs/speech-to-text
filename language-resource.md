---

copyright:
  years: 2015, 2021
lastupdated: "2021-10-04"

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Working with corpora and custom words for previous-generation models
{: #corporaWords}

This information is specific to custom models that are based on *previous-generation models*. For information about corpora and custom words for custom models that are based on *next-generation models*, see [Working with corpora and custom words for next-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords-ng).
{: note}

You can populate a custom language model with words by adding corpora or grammars to the model, or by adding custom words directly:
{: shortdesc}

-   **Corpora** - The recommended means of populating a custom language model with words is to add one or more corpora to the model. When you add a corpus, the service analyzes the file and automatically adds any new words that it finds to the custom model. Adding a corpus to a custom model allows the service to extract domain-specific words in context, which helps ensure better transcription results. For more information, see [Working with Corpora](#workingCorpora).
-   **Grammars** - You can add grammars to a custom model to limit speech recognition to the words or phrases that are recognized by a grammar. When you add a grammar to a model, the service automatically adds any new words that it finds to the model, just as it does with corpora. For more information, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).
-   **Individual words** - You can also add individual custom words to a model directly. The service adds the words to the model just as it does words that it discovers from corpora or grammars. When you add a word directly, you can specify multiple pronunciations and indicate how the word is to be displayed. You can also update existing words to modify or augment the definitions that were extracted from corpora or grammars. For more information, see [Working with custom words](#workingWords).

Regardless of how you add them, the service stores all words that you add to a custom language model in the model's words resource.

## The words resource
{: #wordsResource}

The *words resource* includes all words that you add from corpora, from grammars, or directly. Its purpose is to define the pronunciation and spelling of words that are not already present in the service's base vocabulary. The definitions tell the service how to transcribe these *out-of-vocabulary (OOV) words*.

The words resource contains the following information about each OOV word. The service creates the definitions for words that are extracted from corpora and grammars. You specify the characteristics for words that you add or modify directly.

-   `word` - The spelling of the word as found in a corpus or grammar or as added by you.
-   `sounds_like` - The pronunciation of the word. For words extracted from corpora and grammars, the value represents how the service believes that the word is pronounced based on its language rules. In many cases, the pronunciation reflects the spelling of the `word` field.

    You can use the `sounds_like` field to modify the word's pronunciation. You can also use the field to specify multiple pronunciations for a word. For more information, see [Using the sounds_like field](#soundsLike).
-   `display_as` - The spelling of the word that the service uses in transcripts. The field indicates how the word is to be displayed. In most cases, the spelling matches the value of the `word` field.

    You can use the `display_as` field to specify a different spelling for the word. For more information, see [Using the display_as field](#displayAs).
-   `source` - How the word was added to the words resource. If the service extracted the word from a corpus or grammar, the field lists the name of that resource. Because the service can encounter the same word in multiple resources, the field can list multiple corpus or grammar names. The field includes the string `user` if you add or modify the word directly.

After adding or modifying a word in a model's words resource, it is important that you verify the correctness of the word's definition; for more information, see [Validating a words resource for previous-generation models](#validateModel). You must also train the model for the changes to take effect during transcription; for more information, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

## How much data do I need?
{: #wordsResourceAmount}

Many factors contribute to the amount of data that you need for an effective custom language model. It is not possible to indicate the exact number of words that you need to add for any custom model or application. Depending on the use case, even adding a few words directly to a custom model can improve the model's quality. But adding OOV words from a corpus that shows the words in the context in which they are used in audio can greatly improve transcription accuracy.

The service limits the number of words that you can add to a custom language model:

-   You can add a maximum of 90 thousand OOV words to the words resource of a custom model. This figure includes OOV words from all sources (corpora, grammars, and individual custom words that you add directly).
-   You can add a maximum of 10 million total words to a custom model from all sources. This figure includes all words, both OOV words and words that are already part of the service's base vocabulary, that are included in corpora or grammars. For corpora, the service uses these additional words to learn the context in which OOV words can appear, which is why corpora are a more effective means of improving recognition accuracy.

A large words resource can increase the latency of speech recognition, but the exact effect is difficult to quantify or predict. As with the amount of data that is needed to produce an effective custom model, the performance impact of a large words resource depends on many factors. Test your custom model with different amounts of data to determine the performance of your models and data.

## Working with corpora for previous-generation models
{: #workingCorpora}

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

Speech recognition relies on statistical algorithms to analyze audio. Words from a custom model are in competition with words from the service's base vocabulary as well as other words of the model. (Factors such as audio noise and speaker accents also affect the quality of transcription.)

The accuracy of transcription can depend largely on how words are defined in a model and how speakers say them. To improve the service's accuracy, use corpora to provide as many examples as possible of how OOV words are used in the domain. Repeating the OOV words in corpora can improve the quality of the custom language model. How you duplicate the words in corpora depends on how you expect users to say them in the audio that is to be recognized. The more sentences that you add that represent the context in which speakers use words from the domain, the better the service's recognition accuracy.

The service does not apply a simple word-matching algorithm. Its transcription depends on the context in which words are used. When it parses a corpus, the service includes information about n-grams (bi-grams, tri-grams, and so on) from the sentences of the corpus in the custom model. This information helps the service transcribe audio with greater accuracy, and it explains why training a custom model on corpora is more valuable than training it on custom words alone.

For example, accountants adhere to a common set of standards and procedures that are known as Generally Accepted Accounting Principles (GAAP). When you create a custom model for a financial domain, provide sentences that use the term GAAP in context. The sentences help the service distinguish between general phrases such as "the gap between them is small" and domain-centric phrases such as "GAAP provides guidelines for measuring and disclosing financial information."

In general, it is better for corpora to use words in different contexts, which can improve how the service learns the words. However, if users speak the words in only a couple of contexts, then showing the words in other contexts does not improve the quality of the custom model. Speakers never use the words in those contexts. If speakers are likely to use the same phrase frequently, then repeating that phrase in the corpora can improve the quality of the model. (In some cases, even adding a few custom words directly to a custom model can make a positive difference.)

### Preparing a corpus text file
{: #prepareCorpus}

Follow these guidelines to prepare a corpus text file:

-   Provide a plain text file that is encoded in UTF-8 if it contains non-ASCII characters. The service assumes UTF-8 encoding if it encounters such characters.

    Make sure that you know the character encoding of your corpus text files. The service preserves the encoding that it finds in the text files. You must use that same encoding when working with custom words in the custom model. For more information, see [Character encoding for custom words](/docs/speech-to-text?topic=speech-to-text-manageWords#charEncoding).
    {: important}

-   Use consistent capitalization for words in the corpus. The words resource is case-sensitive. Mix upper- and lowercase letters and use capitalization only when intended.
-   Include each sentence of the corpus on its own line, and terminate each line with a carriage return. Including multiple sentences on the same line can degrade accuracy.
-   Add personal names as discrete units on separate lines. Do not add the individual elements of a name on separate lines or as individual custom words, and do not include multiple names on the same line of a corpus. The following example shows the correct way to improve recognition accuracy for three names:

    ```text
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Include additional contextual information where appropriate, for example, `Doctor Sebastian Leifson` or `President Malcolm Ingersol`. As with all words, duplicating the names multiple times and, if possible, in different contexts can improve recognition accuracy.
-   Beware of typographical errors. The service assumes that typographical errors are new words. Unless you correct them before you train the model, the service adds them to the model's vocabulary. Remember the adage *Garbage in, garbage out!*
-   More sentences result in better accuracy. But the service does limit a model to a maximum of 10 million total words and 90 thousand OOV words from all sources combined.

The service cannot generate a pronunciation for all words. After adding a corpus, you must validate the words resource to ensure that each OOV word's definition is complete and valid. For more information, see [Validating a words resource for previous-generation models](#validateModel).

### What happens when I add a corpus file?
{: #parseCorpus}

When you add a corpus file, the service analyzes the file's contents. It extracts any new (OOV) words that it finds and adds each OOV word to the custom model's words resource. To distill the most meaning from the content, the service tokenizes and parses the data that it reads from a corpus file. The following sections describe how the service parses a corpus file for each supported language.

#### Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish
{: #corpusLanguages}

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

-   Converts tokens that include certain symbols to meaningful string representations. These examples are not exhaustive. The service makes similar adjustments for other characters as needed. (For Spanish, if the dialect is `es-LA`, `$100` and `100$` become `cien pesos`.)

    | Language | A dollar sign and a number | A euro sign and a number | A percent sign and a number |
    |---------------|:-------------------------------:|:---------------------------:|:-------------------------------:|
    | Dutch      | `$100` becomes `honderd dollar` | `€100` becomes `honderd euro` | `100%` becomes `honderd procent` |
    | English    | `$100` becomes `one hundred dollars` | `€100` becomes `one hundred euros` | `100%` becomes `one hundred percent` |
    | French     | `$100` becomes `cent dollars` | `€100` becomes `cent euros` | `100%` becomes `cent pour cent` |
    | German     | `$100` and `100$` become `einhundert dollar` | `€100` and `100€` become `einhundert euro` | `100%` becomes `einhundert prozent` |
    | Italian    | `$100` becomes `cento dollari` | `€100` becomes `cento euro` | `100%` becomes `cento per cento` |
    | Portuguese | `$100` and `100$` become `cem dólares` | `€100` and `100€` become `cem euros` | `100%` becomes `cem por cento` |
    | Spanish    | `$100` and `100$` become `cien dólares` | `€100` and `100€` become `cien euros` | `100%` becomes `cien por ciento` |
    {: caption="Table 2. Examples of symbol conversion"}

-   Processes non-alphanumeric, punctuation, and special characters depending on their context. For example, the service removes a `$` (dollar sign) or `€` (euro symbol) unless it is followed by a number. Processing is context-dependent and consistent across the supported languages.
-   Ignores phrases that are enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), or `{ }` (curly braces).

#### Parsing of Japanese
{: #corpusLanguages-jaJP}

-   Converts all characters to full-width characters.
-   Converts numbers to their equivalent words, for example, `500` becomes <code>&#20116;&#30334;</code>, and `0.15` becomes <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Does not convert tokens that include symbols to equivalent strings, for example, `100%` becomes <code>&#30334;&#65285;</code>.
-   Does not automatically remove punctuation. {{site.data.keyword.IBM_notm}} highly recommends that you remove punctuation if your application is transcription-based as opposed to dictation-based.

#### Parsing of Korean
{: #corpusLanguages-koKR}

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
-   For phrases that consist of Latin (English) characters or a mix of Hangul and Latin characters, the service creates OOV words for the phrases exactly as they appear in the corpus file. And it creates sounds-like pronunciations for the words based on Hangul transcriptions.
    -   It gives the OOV word `London` a sounds-like of <code>&#47088;&#45912;</code>.
    -   It gives the OOV word <code>IBM&#54856;&#54168;&#51060;&#51648;</code> a sounds-like of <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>.

## Working with custom words for previous-generation models
{: #workingWords}

You can use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to add new words to a custom model's words resource. You can also use the methods to modify or augment a word in a words resource.

You might, for instance, need to use the methods to correct a typographical error or other mistake that is made when a word is added from a corpus. You might also need to add sounds-like definitions for an existing word. If you modify an existing word, the new data that you provide overwrites the word's existing definition in the words resource. The rules for adding a word also apply to modifying an existing word.

You are likely to add most custom words from corpora. Make sure that you know the character encoding of your corpus text files. The service preserves the encoding that it finds in the text files. You must use that same encoding when working with custom words in the custom model. For more information, see [Character encoding for custom words](/docs/speech-to-text?topic=speech-to-text-manageWords#charEncoding).
{: important}

### Using the sounds_like field
{: #soundsLike}

The `sounds_like` field specifies how a word is pronounced by speakers. By default, the service automatically attempts to complete the field with the word's spelling. But the service cannot generate a pronunciation for all words. After adding or modifying words, you must validate the words resource to ensure that each word's definition is complete and valid. For more information, see [Validating a words resource for previous-generation models](#validateModel).

You can provide as many as five alternative pronunciations for a word that is difficult to pronounce or that can be pronounced in different ways. Consider using the field to

-   *Provide different pronunciations for acronyms.* For example, the acronym `NCAA` can be pronounced as it is spelled or colloquially as *N. C. double A.* The following example adds both of these sounds-like pronunciations for the word `NCAA`:

    ![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

    ```bash
    curl -X PUT -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}" \
    "{url}/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    ![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

    ```bash
    curl -X PUT \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: application/json" \
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}" \
    "{url}/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *Handle foreign words.* For example, the French word `garçon` contains a character that is not found in the English language. You can specify a sounds-like of `gaarson`, replacing `ç` with `s`, to tell the service how English speakers would pronounce the word.

Speech recognition uses statistical algorithms to analyze audio, so adding a word does not guarantee that the service transcodes it with complete accuracy. When you add a word, consider how it might be pronounced. Use the `sounds_like` field to provide various pronunciations that reflect how a word can be spoken. The following sections provide language-specific guidelines for specifying a sounds-like pronunciation.

#### Guidelines for English
{: #wordLanguages-english}

*Guidelines for Australian, United Kingdom, and United States English:*

-   Use English alphabetic characters: `a-z` and `A-Z`.
-   Use real or made-up words that are pronounceable in English for words that are difficult to pronounce, for example, `shuchesnie` for the word `Sczcesny`.
-   Substitute equivalent English letters for non-English letters, for example, `s` for `ç` or `ny` for `ñ`.
-   Substitute non-accented letters for accented letters, for example, `a` for `à` or `e` for `è`.
-   You can include multiple words that are separated by spaces, but the service enforces a maximum of 40 total characters not including spaces.

*Guidelines for Australian and United States English only:*

-   To pronounce a single letter, use the letter followed by a period. If the period is followed by another character, be sure to use a space between the period and the next character. For example, use `N. C. A. A.`, *not* `N.C.A.A.`
-   Use the spelling of numbers, for example, `seventy-five` for `75`.

*Guidelines for United Kingdom English only:*

-   You **cannot** use periods or dashes in sounds-like pronunciations for UK English.
-   To pronounce a single letter, use the letter followed by a space. For example, use `N C A A`, *not* `N. C. A. A.`, `N.C.A.A.`, or `NCAA`.
-   Use the spelling of numbers without dashes, for example, `seventy five` for `75`.

#### Guidelines for Dutch, French, German, Italian, Portuguese, and Spanish
{: #wordLanguages-esES-frFR}

*Guidelines for all supported dialects of Dutch, French, German, Italian, Portuguese, and Spanish:*

-   You **cannot** use dashes in sounds-like pronunciations.
-   Use alphabetic characters that are valid for the language: `a-z` and `A-Z` including valid accented letters.
-   To pronounce a single letter, use the letter followed by a period. If the period is followed by another character, be sure to use a space between the period and the next character. For example, use `N. C. A. A.`, *not* `N.C.A.A.`
-   Use real or made-up words that are pronounceable in the language for words that are difficult to pronounce.
-   Use the spelling of numbers without dashes, for example, for `75` use
    -   *Dutch (Netherlands):* `vijfenzeventig`
    -   *French:* `soixante quinze`
    -   *German:* `fünfundsiebzig`
    -   *Italian:* `settantacinque`
    -   *Portuguese (Brazilian):* `setenta e cinco`
    -   *Spanish:* `setenta y cinco`
-   You can include multiple words that are separated by spaces, but the service enforces a maximum of 40 total characters not including spaces.

#### Guidelines for Japanese
{: #wordLanguages-jaJP}

-   Use only full-width Katakana characters by using the <code>&#8213;</code> lengthen symbol (*chou-on*, or &#38263;&#38899;, in Japanese). Do not use half-width characters.
-   Use contracted sounds (*yoh-on*, or &#25303;&#38899;, in Japanese) only in the following syllable contexts:

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>

    <code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>

    <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>

    <code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>

    <code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>

    <code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Use only the following syllables after an assimilated sound (*soku-on*, or &#20419;&#38899;, in Japanese):

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>

    <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>

    <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>

    <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>>

    <code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   Do not use <code>&#12531;</code> as the first character of a word. For example, use <code>&#12454;&#12540;&#12531;&#12488;</code> instead of <code>&#12531;&#12540;&#12488;</code>, the latter of which is invalid.
-   Many compound words consist of *prefix+noun* or *noun+suffix*. The service's base vocabulary covers most compound words that occur frequently (for example, <code>&#x9577;&#x96FB;&#x8A71;</code> and <code>&#x53E4;&#x65B0;&#x805E;</code>) but not those compound words that occur infrequently. If your corpus commonly contains compound words, add them as one word as the first step of your customization. For example, <code>&#x53E4;&#x925B;&#x7B46;</code> is not common in general Japanese text; if you use it often, add it to your custom model to improve transcription accuracy.
-   Do not use a trailing assimilated sound.

#### Guidelines for Korean
{: #wordLanguages-koKR}

-   Use Korean Hangul characters, symbols, and syllables.
-   You can also use Latin (English) alphabetic characters: `a-z` and `A-Z`.
-   Do not use any characters or symbols that are not included in the previous sets.

### Using the display_as field
{: #displayAs}

The `display_as` field specifies how a word is displayed in a transcript. It is intended for cases where you want the service to display a string that is different from the word's spelling. For example, you can indicate that the word `hhonors` is to be displayed as `HHonors` regardless of whether it sounds like `hilton honors` or `h honors`.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}" \
"{url}/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}" \
"{url}/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

As another example, you can indicate that the word `IBM` is to be displayed as `IBM™`.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM™\"}" \
"{url}/v1/customizations/{customization_id}/words/IBM"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM™\"}" \
"{url}/v1/customizations/{customization_id}/words/IBM"
```
{: pre}

#### Interaction with smart formatting and numeric redaction
{: #displaySmart}

If you use the `smart_formatting` or `redaction` parameters with a recognition request, be aware that the service applies smart formatting and redaction to a word before it considers the `display_as` field for the word. You might need to experiment with results to ensure that the features do not interfere with how your custom words are displayed. You might also need to add custom words to accommodate the effects.

For instance, suppose that you add the custom word `one` with a `display_as` field of `one`. Smart formatting changes the word `one` to the number `1`, and the display-as value is not applied. To work around this issue, you could add a custom word for the number `1` and apply the same `display_as` field to that word.

For more information about working with these features, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting) and [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction).

### What happens when I add or modify a custom word?
{: #parseWord}

How the service responds to a request to add or modify a custom word depends on the fields and values that you specify. It also depends on whether the word exists in the service's base vocabulary.

-   **Omit both the `sounds_like` and `display_as` fields:**
    -   *If the word does not exist in the service's base vocabulary,* the service attempts to set the `sounds_like` field to its pronunciation of the word. It cannot generate a pronunciation for all words, so you must review the word's definition to ensure that it is complete and valid. The service sets the `display_as` field to the value of the `word` field.
    -   *If the word exists in the service's base vocabulary,* the service leaves the `sounds_like` and `display_as` fields empty. These fields are empty only if the word exists in the service's base vocabulary. The word's presence in the model's words resource is harmless but unnecessary.

-   **Specify only the `sounds_like` field:**
    -   *If the `sounds_like` field is valid,* the service sets the `display_as` field to the value of the `word` field.
    -   *If the `sounds_like` field is invalid:*
        -   The `POST /v1/customizations/{customization_id}/words` method adds an `error` field to the word in the model's words resource.
        -   The `PUT /v1/customizations/{customization_id}/words/{word_name}` method fails with a 400 response code and an error message. The service does not add the word to the words resource.

-   **Specify only the `display_as` field:**
    -   *If the word does not exist in the service's base vocabulary,* the service attempts to set the `sounds_like` field to its pronunciation of the word. It cannot generate a pronunciation for all words, so you must review the word's definition to ensure that it is complete and valid. The service leaves the `display_as` field as specified.
    -   *If the word exists in the service's base vocabulary,* the service leaves the `sounds_like` empty and leaves the `display_as` field as specified.

-   **Specify both the `sounds_like` and `display_as` fields:**
    -   *If the `sounds_like` field is valid,* the service sets the `sounds_like` and `display_as` fields to the specified values.
    -   *If the `sounds_like` field is invalid,* the service responds as it does in the case where the `sounds_like` field is specified but the `display_as` field is not.

## Validating a words resource for previous-generation models
{: #validateModel}
{: troubleshoot}
{: support}

Especially when you add a corpus to a custom language model or add multiple custom words at one time, examine the OOV words in the model's words resource.

-   *Look for typographical and other errors.* Especially when you add corpora, which can be large, mistakes are easily made. Typographical errors in a corpus (or grammar) file have the unintended consequence of adding new words to a model's words resource, as do ill-formed HTML tags that are left in a corpus file.
-   *Verify the sounds-like pronunciations.* The service tries to generate sounds-like pronunciations for OOV words automatically. In most cases, these pronunciations are sufficient. But the service cannot generate a pronunciation for all words, so you must review the word's definition to ensure that it is complete and valid. Reviewing the pronunciations for accuracy is also recommended for words that have unusual spellings or are difficult to pronounce, and for acronyms and technical terms.

To validate and, if necessary, correct a word for a custom model, regardless of how it was added to the words resource, use the following methods:

-   List all of the words from a custom model by using the `GET /v1/customizations/{customization_id}/words` method or query an individual word with the `GET /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   Modify words in a custom model to correct errors or to add sounds-like or display-as values by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Working with custom words for previous-generation models](#workingWords).
-   Delete extraneous words that are introduced in error (for example, by typographical or other mistakes in a corpus) by using the `DELETE /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Deleting a word from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#deleteWord).
    -   If the word was extracted from a corpus, you can instead update the corpus text file to correct the error and then reload the file by using the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method. For more information, see [Add a corpus to the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#addCorpus).
    -   If the word was extracted from a grammar, you can update the grammar file to correct the error and then reload the file by using the `allow_overwrite` parameter of the `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` method. For more information, see [Add a grammar to the custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar).
