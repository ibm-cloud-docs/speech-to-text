---

copyright:
  years: 2015, 2023
<<<<<<< HEAD
lastupdated: "2023-06-19"
=======
lastupdated: "2024-05-10"
>>>>>>> 8531dee8 (Custom models, corpora LSM updates)

subcollection: speech-to-text

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Working with corpora and custom words for large speech models and next-generation models
{: #corporaWords-ng}

This information is specific to custom models that are based on *large speech models and next-generation models*. For information about corpora and custom words for custom models that are based on *previous-generation models*, see [Working with corpora and custom words for previous-generation models](/docs/speech-to-text?topic=speech-to-text-corporaWords).
{: note}

You populate a custom language model with words by adding corpora to the model or by adding custom words directly to the model. You use the same methods and operations for large speech models, previous- and next-generation models. For more information about adding corpora and custom words to a model, see [Working with corpora for large speech models and next-generation models](#workingCorpora-ng) and [Working with custom words for large speech models and next-generation models](#workingWords-ng).

Although language model customization is similar in usage and intent for large speech models, previous- and next-generation models, there are differences between the three types of models at the implementation level. To understand how language model customization works for large speech models and next-generation models, and how you can make the best use of customization, you need a high-level understanding of the differences.

-   When you create and use a custom language model that is based on a *large speech model or previous-generation model*, the service relies on words from the custom model to create transcripts that contain domain-specific terms. In combination with words from its base vocabulary, the service uses these words from the custom model to predict and transcribe speech from audio. You provide the information for a custom language model in the form of corpora, custom words, and grammars. The service stores this information in the words resource for the custom model.

-   When you create a custom language model that is based on a *next-generation model*, the services relies on sequences of characters from the custom model to create transcripts that reflect domain-specific terms. In combination with character sequences from the base model, the service uses these series of characters from the custom model to predict and transcribe speech from audio.

    You provide the information for a custom language model in the form of corpora, custom words, and grammars. But rather than rely on a words resource that contains these words, the service extracts and stores character sequences from the corpora and custom words. The service does not extract and calculate out-of-vocabulary (OOV) words from corpora and custom words. The words resource is simply where it stores custom words that you add directly to the model.

When you develop a custom language model based on a large speech model or next-generation model, you must still provide corpora and custom words to train the model on domain-specific terminology. So the process of creating and training a custom model is largely the same for large speech models, next-generation and previous-generation models.

The following topics describe the rules for providing corpora and custom words for a custom language model that is based on large speech models and next-generation models. The rules are similar to those for working with a custom model based on a previous-generation model, but some important differences do exist.

## The words resource
{: #wordsResource-ng}

The *words resource* includes custom words that you add directly to the custom model. The words resource contains the following information about each custom word:

-   `word` - The spelling of the word as added by you.

    Do not use characters that need to be URL-encoded. For example, do not use spaces, slashes, backslashes, colons, ampersands, double quotes, plus signs, equals signs, question marks, etc. in the name. The service does not prevent the use of these characters, but because they must be URL-encoded wherever they are used, it is strongly discouraged.
    {: important}

-   `sounds_like` - The pronunciation of the word. You can use the `sounds_like` field to add one or more pronunciations for the word. For more information, see [Using the sounds_like field](#sounds-like-ng).
-   `display_as` - The spelling of the word that the service uses in transcripts. Unless you specify an alternative representation, the spelling matches the value of the `word` field. For more information, see [Using the display_as field](#display-as-ng).
-   `source` - How the word was added to the words resource. The field always contains the string `user` to indicate that it was added directly as a custom word.

After adding or modifying a custom word, it is important that you verify the correctness of the word's definition; for more information, see [Validating a words resource for large speech models and next-generation models](#validateModel-ng). You must also train the model for the changes to take effect during transcription; for more information, see [Train the custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

You can add custom words that already exist, for example, to add more sounds-like pronunciations for common words. Otherwise, there is no reason to duplicate common words. Such words remain in the model's words resource, but they are harmless and unnecessary.
{: note}

-   `mapping_only` - Parameter for custom words. 
You can use the 'mapping_only' key in custom words as a form of post processing. This key parameter has a boolean value to determine whether 'sounds_like' (for non-Japanese models) or word (for Japanese) is not used for the model fine-tuning, but for the replacement for 'display_as'. This feature helps you when you use custom words exclusively to map 'sounds_like' (or word) to 'display_as' value. When you use custom words solely for post-processing purposes that does not need fine-tuning.

Use case examples,

Before using 'mapping_only':
Speech to Text machine output is 'hilton honors' as its ASR transcript. However, you want it to be displayed as 'HHonors' as final output. So, you can use the following custom word to map 'hilton honors' to 'HHonors'.

```json
{"word": "HHonors", "sounds_like": ["hilton honors"], "display_as": "HHonors"}
```
{: codeblock}

While this maps any word 'hilton honors' in ASR transcript to 'HHonors', it fine-tunes the model with 'sounds_like' (hilton honors) by default, even if the model has no problem with recognizing the word 'hilton honors'. This is the example of words that do not need to be fine-tuned but need to be mapped to 'display_as'.

After using 'mapping_only':
Since Speech to Text model is recognizing the word 'hilton honors' very well, it does not have to be fine-tuned on that word. Thus, you can use the following custom words to skip training and mapping the 'sounds_like' to 'display_as'.

```json
{"word": "HHonors", "sounds_like": ["hilton honors"], "display_as": "HHonors", "mapping_only": true}
```
{: codeblock}

This parameter is applicable for the next-generation models that support the enhanced customization (English models, ja-Jp models and so on). See [the list of supported models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng).

## How much data do I need?
{: #wordsResourceAmount-ng}

Many factors contribute to the amount of data that you need for an effective custom language model. It is not possible to indicate the exact amount of data that you need to add for any custom model or application. Depending on the use case, even adding a few words directly to a custom model can improve the model's quality. But adding corpora that show the words in the context in which they are used can greatly improve transcription accuracy.

You can add a maximum of 10 million total words to a custom model from all sources. This figure includes all words that are included in corpora and that you add directly. The service uses all of the words from a corpus to learn the context in which sequences of characters can occur, which is why corpora are a more effective means of improving recognition accuracy.

Adding a large number of corpora and words can increase the latency of speech recognition, but the exact effect is difficult to quantify or predict. As with the amount of data that is needed to produce an effective custom model, the performance impact of a large amount of data depends on many factors. Test your custom model with different amounts of data to determine the performance of your models.

### Guidelines for adding words to custom models based on improved next-generation models
{: #wordsResourceAmount-words-ng}

For custom models that are based on next-generation language models that use improved customization, limit the number of custom words that you add directly to the model with the following methods:

-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`

Using these methods to add custom words to a custom model can significantly increase the training time of the model. Training time increases linearly with the total number of words that you add. However, the training time increases only when the model is first trained on the new custom words. The time required for subsequent training requests, without new custom words, returns to normal.

Training a custom model with words added only via corpora with the following method is typically quick:

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`

For more information about improved customization for next-generation models, see [Improved language model customization for next-generation models](/docs/speech-to-text?topic=speech-to-text-customization#customLanguage-intro-ng).

The non-Japanese models use 'sounds_like' for mapping ('sounds_like' -> 'display_as'). 
{: note}

### Guidelines for adding words to Japanese models based on improved next-generation models
{: #wordsResourceAmount-japanese-ng}

Do not add custom words for known words, words that are commonly recognized and have a general correspondence between the word and how it is pronounced. Use custom words to create a mapping between how less common words are spelled and how they are pronounced. Also use custom words to add unknown words, those that lack a correspondence between the word and its pronunciation or that are not commonly recognized as words. Adding such words to a corpus is equally effective.

The service enforces a maximum limit of 25 total characters, not including leading or trailing spaces, for custom words and sounds-likes. If you add a custom word or sounds-like that exceeds this limit, the service automatically treats the word as if it were added by a corpus. The word does not appear as a custom word for the model. For the most effective training, it is recommended that Japanese custom words and sounds-likes contain no more than 20 characters. Add long words such as `ＩＢＭクラウド音声認識サービス` to a corpus.

For example, the pronunciation `アイビーエム` for the word `ＩＢＭ` is recognized without customization, so there is no need to add it as a custom word. Because `ＩＢＭ`, `クラウド`, `音声認識`, and `サービス` are common words, adding them as custom words has no effect.

For a custom word, enter a commonly used notation that reflects the word's usage and pronunciation. This allows for more efficient customization. For instance, the following example does not reliably produce the string `Artificial_Intelligence` in response to the utterance `エーアイ` because `Artificial_Intelligence` and `人工知能` are not generally uttered as `AI`:

```json
{\"word\": \"Artificial_Intelligence\", \"sounds_like\": [\"エーアイ\"], \"display_as\": \"Artificial_Intelligence\"},
{\"word\": \"人工知能\", \"sounds_like\": [\"エーアイ\"], \"display_as\": \"Artificial_Intelligence\"}
```
{: codeblock}

Because the context is typically something like `これからＡＩはますます発展してきます`, `ＡＩ` is the most appropriate notation for the sounds-like `エーアイ`. The following example is therefore likely to yield better results:

```json
{\"word\": \"ＡＩ\", \"sounds_like\": [\"エーアイ\"], \"display_as\": \"Artificial_Intelligence\"}
```
{: codeblock}

Finally, in custom words, half-width alphabetic characters are converted to full-width characters. English uppercase and lowercase characters are treated as different characters.

<<<<<<< HEAD
## Working with corpora for next-generation models
=======
Japanese models use word for mapping ('word' -> 'display_as').
{: note}

## Working with corpora for large speech models and next-generation models
>>>>>>> 8531dee8 (Custom models, corpora LSM updates)
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

Character sequences in words from a custom model are in competition with character sequences from the base model, as well as sequences from other words of the model. (Factors such as audio noise and speaker accents also affect the quality of transcription.)

The accuracy of transcription can depend largely on the data that you add to a model and how speakers say words in audio. To improve the service's accuracy, use corpora to provide as many examples as possible of how words are used in a domain. Repeating the words in corpora can improve the quality of a custom language model. How you duplicate the words in corpora depends on how you expect users to say them in the audio that is to be recognized. The more sentences that you add that represent the context in which speakers use words from the domain, the better the service's recognition accuracy.

For example, accountants adhere to a common set of standards and procedures that are known as Generally Accepted Accounting Principles (GAAP). When you create a custom model for a financial domain, provide sentences that use the term GAAP in context. The sentences help the service distinguish between general phrases such as "the gap between them is small" and domain-centric phrases such as "GAAP provides guidelines for measuring and disclosing financial information."

In general, it is better for corpora to use words in different contexts, which can improve how the service learns the phrases. However, if users speak the words in only a couple of contexts, then showing the words in other contexts does not improve the quality of the custom model: Speakers never use the words in those contexts. If speakers are likely to use the same phrase frequently, repeating that phrase in the corpora can improve the quality of the model. In some cases, even adding a few custom words directly to a custom model can make a positive difference.

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

When you add a corpus file, the service analyzes the file's contents. To distill the most meaning from the content, the service tokenizes and parses the data that it reads from a corpus file. The following topics describe how the service parses a corpus file for each supported language.

Information for the following languages is not yet available: Arabic, Chinese, Czech, Hindi, and Swedish. If you need this information for your custom language model, contact your IBM Support representative.
{: note}

#### Parsing of Dutch, English, French, German, Italian, Portuguese, and Spanish
{: #corpusLanguages-ng}

The following information applies to all supported dialects of Dutch, English, French, German, Italian, Portuguese, and Spanish:

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

The following information applies to Japanese:

-   Converts all characters to full-width characters.
-   Converts numbers to their equivalent words, for example, `500` becomes `五百`, and `0.15` becomes `〇・一五`.
-   Does not convert tokens that include symbols to equivalent strings, for example, `100%` becomes `百％`.
-   Does not automatically remove punctuation. {{site.data.keyword.IBM_notm}} highly recommends that you remove punctuation if your application is transcription-based as opposed to dictation-based.

#### Parsing of Korean
{: #corpusLanguages-koKR-ng}

The following information applies to Korean:

-   Converts numbers to their equivalent words, for example, `10` becomes `십`.
-   Removes the following punctuation and special characters: `- ( ) * : . , ' "`. However, not all punctuation and special characters that are removed for other languages are removed for Korean, for example:
    -   Removes a period (`.`) symbol only when it occurs at the end of a line of input.
    -   Does not remove a tilde (`~`) symbol.
    -   Does not remove or otherwise process Unicode wide-character symbols, for example, `…` (triple dot or ellipsis).

    In general, {{site.data.keyword.IBM_notm}} recommends that you remove punctuation, special characters, and Unicode wide-characters before you process a corpus file.
-   Does not remove or ignore phrases that are enclosed in `( )` (parentheses), `< >` (angle brackets), `[ ]` (square brackets), or `{ }` (curly braces).
-   Converts tokens that include certain symbols to meaningful string representations, for example:
    -   `24%` becomes `이십사퍼센트`.
    -   `$10` becomes `십달러`.

    This list is not exhaustive. The service makes similar adjustments for other characters as needed.
-   For phrases that consist of Latin (English) characters or a mix of Hangul and Latin characters, the service uses the phrases exactly as they appear in the corpus file.

## Working with custom words for large speech models and next-generation models
{: #workingWords-ng}

You can use the `POST /v1/customizations/{customization_id}/words` and `PUT /v1/customizations/{customization_id}/words/{word_name}` methods to add new words to a custom model. You can also use the methods to modify or augment a custom word.

You might, for instance, need to use the methods to correct a typographical error or other mistake that is made when a word is added to a custom model. If you modify an existing word, the new data that you provide overwrites the word's existing definition in the words resource. The rules for adding a word also apply to modifying an existing word.

You are likely to add most custom words from corpora. Make sure that you know the character encoding of your corpus text files. The service preserves the encoding that it finds in the text files. You must use that same encoding when working with custom words in the custom model. For more information, see [Character encoding for custom words](/docs/speech-to-text?topic=speech-to-text-manageWords#charEncoding).
{: important}

### Using the sounds_like field
{: #sounds-like-ng}

The `sounds_like` field specifies how a word is pronounced by speakers in audio. By default, the service does not automatically attempt to generate a sounds-like pronunciation for a word for which you do not provide one. You can add sounds-likes for any words that do not have them. After adding or modifying words, you must validate the words resource to ensure that each word's definition is complete and valid. For more information, see [Validating a words resource for large speech models and next-generation models](#validateModel-ng).

You can provide as many as five alternative pronunciations for a word that is difficult to pronounce or that can be pronounced in different ways. Some possible uses of the field follow:

-   *Provide different pronunciations for acronyms.* For example, the acronym `NCAA` can be pronounced as it is spelled or colloquially as *N C double A* The following example adds both of these sounds-like pronunciations for the word `NCAA`:

    [IBM Cloud]{: tag-ibm-cloud}

    ```bash
    curl -X PUT -u "apikey:{apikey}" \
    --header "Content-Type: application/json" \
    --data "{\"sounds_like\": [\"N C A A\", \"N C double A\"]}" \
    "{url}/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    [IBM Cloud Pak for Data]{: tag-cp4d}

    ```bash
    curl -X PUT \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: application/json" \
    --data "{\"sounds_like\": [\"N C A A\", \"N C double A\"]}" \
    "{url}/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    For more information about how the service recognizes acronyms, see [Additional transcription efforts](#parseWord-additional-ng).

-   *Handle foreign words.* For example, the French word `garçon` contains a character that is not found in the English language. You can specify a sounds-like of `gaarson`, replacing `ç` with `s`, to tell the service how English speakers would pronounce the word.

The following topics provide guidelines for specifying a sounds-like pronunciation. Speech recognition uses statistical algorithms to analyze audio, so adding a word does not guarantee that the service transcodes it with complete accuracy. When you add a word, consider how it might be pronounced. Use the `sounds_like` field to provide various pronunciations that reflect how a word can be spoken.

Information for the following languages is not yet available: Arabic, Chinese, Czech, Hindi, and Swedish. If you need this information for your custom language model, contact your IBM Support representative.
{: note}

#### General guidelines for all languages
{: #sounds-like-general-ng}

Follow these guidelines when specifying a sounds-like for any language:

-   *Do not use punctuation characters in sounds-likes.* For example, do not use periods, dashes, underscores, commas, end of sentence punctuation characters, and special characters such as dollar and euro signs, parentheses, brackets, and braces.
-   *Use alphabetic characters that are valid for your language.* For English, this includes `a-z` and `A-Z`. For other languages, valid characters can include accented letters or language-specific characters.
-   *In English, substitute equivalent English letters for non-English or accented letters.* For example, `s` for `ç`, `ny` for `ñ`, or `e` for `è`.
-   *Use real or made-up words that are pronounceable for words that are difficult to pronounce.* For example, in English you can use the sounds-like `shuchesnie` for the word `Sczcesny`.
-   *Use the spelling of numbers without dashes.* For example, for the number `75`, use `seventy five` in English, `setenta y cinco` in Spanish, and `soixante quinze` in French.
-   *To pronounce a single letter, use the letter followed by a space.* For example, use `N C A A`, *not* `N. C. A. A.`, `N.C.A.A.`, or `NCAA`.
-   *You can include multiple words that are separated by spaces.*
    -   For most languages, the service enforces a maximum of 40 total characters, not including leading or trailing spaces.
    -   For Japanese, the service enforces a maximum limit of 25 total characters, not including leading or trailing spaces. If you add a custom word or sounds-like that exceeds this limit, the service automatically treats the word as if it were added by a corpus. The word does not appear as a custom word for the model. For the most effective training, it is recommended that Japanese custom words and sounds-likes contain no more than 20 characters.

#### Guidelines for Japanese
{: #wordLanguages-jaJP-ng}

Follow these guidelines when specifying a sounds-like for Japanese:

-   Use only full-width Katakana characters by using the `―` lengthen symbol (*chou-on*, or 長音, in Japanese). Do not use half-width characters. (For the `display_as` field, if you enter a half-width character, it is rendered as a half-width character in transcription results.)
-   Use contracted sounds (*yoh-on*, or 拗音, in Japanese) only in the following syllable contexts:

    `イェ`, `ウィ`, `ウェ`, `ウォ`, `キィ`, `キャ`, `キュ`, `キョ`, `ギャ`, `ギュ`, `ギョ`, `クァ`, `クィ`, `クェ`, `クォ`

    `グァ`, `グォ`, `シィ`, `シェ`, `シャ`, `シュ`, `ショ`, `ジィ`, `ジェ`, `ジャ`, `ジュ`, `ジョ`, `スィ`, `ズィ`, `チェ`

    `チャ`, `チュ`, `チョ`, `ヂェ`, `ヂャ`, `ヂュ`, `ヂョ`, `ツァ`, `ツィ`, `ツェ`, `ツォ`, `ティ`, `テュ`, `ディ`, `デャ`

    `デュ`, `デョ`, `トゥ`, `ドゥ`, `ニェ`, `ニャ`, `ニュ`, `ニョ`, `ヒャ`, `ヒュ`, `ヒョ`, `ビャ`, `ビュ`, `ビョ`, `ピィ`

    `ピャ`, `ピュ`, `ピョ`, `ファ`, `フィ`, `フェ`, `フォ`, `フュ`, `ミャ`, `ミュ`, `ミョ`, `リィ`, `リェ`, `リャ`, `リュ`

    `リョ`, `ヴァ`, `ヴィ`, `ヴェ`, `ヴォ`, `ヴュ`

-   Use only the following syllables after an assimilated sound (*soku-on*, or 促音, in Japanese):

    `バ`, `ビ`, `ブ`, `ベ`, `ボ`, `チ`, `チェ`, `チャ`, `チュ`, `チョ`, `ダ`, `デ`, `ディ`, `ド`, `ドゥ`, `フ`

    `ファ`, `フィ`, `フェ`, `フォ`, `ガ`, `ギ`, `グ`, `ゲ`, `ゴ`, `ハ`, `ヒ`, `ヘ`, `ホ`, `ジ`, `ジェ`, `ジャ`

    `ジュ`, `ジョ`, `カ`, `キ`, `ク`, `ケ`, `コ`, `キャ`, `キュ`, `キョ`, `パ`, `ピ`, `プ`, `ペ`, `ポ`, `ピャ`

    `ピュ`, `ピョ`, `サ`, `ス`, `セ`, `ソ`, `シ`, `シェ`, `シャ`, `シュ`, `ショ`, `タ`, `テ`, `ト`, `ツ`, `ザ`

    `ズ`, `ゼ`, `ゾ`

-   Do not use `ン` as the first character of a word. For example, use `ウーント` instead of `ンート`, the latter of which is invalid.
-   The character-sequence `ウー` is ambiguous in some left contexts. Do not use characters (syllables) that end with the phoneme `/o/`, such as `ロ` and `ト`. In such cases, use `ウウ` or just `ウ` instead of `ウー`. For example, use `ロウウマン` or `ロウマン` instead of `ロウーマン`.
-   Many compound words consist of *prefix+noun* or *noun+suffix*. The character sequences of the base model cover most compound words that occur frequently (for example, `長電話` and `古新聞`) but not those compound words that occur infrequently. If your corpus commonly contains compound words, add them as one word as the first step of your customization. For example, `古鉛筆` is not common in general Japanese text; if you use it often, add it to your custom model to improve transcription accuracy.
-   Do not use a trailing assimilated sound.

#### Guidelines for Korean
{: #wordLanguages-koKR-ng}

Follow these guidelines when specifying a sounds-like for Korean:

-   Use Korean Hangul characters, symbols, and syllables.
-   You can also use Latin (English) alphabetic characters: `a-z` and `A-Z`.
-   Do not use any characters or symbols that are not included in the previous sets.

### Using the display_as field
{: #display-as-ng}

The `display_as` field specifies how a word is displayed in a transcript. By default, the service sets the field to match the spelling of the custom word. The field is intended for cases where you want the service to display a string that is different from the word's spelling. For example, you can indicate that the word `hhonors` is to be displayed as `HHonors`.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"display_as\": \"HHonors\"}" \
"{url}/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"display_as\": \"HHonors\"}" \
"{url}/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

As another example, you can indicate that the word `IBM` is to be displayed as `IBM™`.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X PUT -u "apikey:{apikey}" \
--header "Content-Type: application/json" \
--data "{\"display_as\":\"IBM™\"}" \
"{url}/v1/customizations/{customization_id}/words/IBM"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X PUT \
--header "Authorization: Bearer {token}" \
--header "Content-Type: application/json" \
--data "{\"display_as\":\"IBM™\"}" \
"{url}/v1/customizations/{customization_id}/words/IBM"
```
{: pre}

#### Interaction with smart formatting and numeric redaction
{: #display-smart-ng}

If you use the `smart_formatting` or `redaction` parameters with a recognition request, be aware that the service applies smart formatting and redaction to a word before it considers the `display_as` field for the word. You might need to experiment with results to ensure that the features do not interfere with how your custom words are displayed. You might also need to add custom words to accommodate the effects.

For instance, suppose that you add the custom word `one` with a `display_as` field of `one`. Smart formatting changes the word `one` to the number `1`, and the display-as value is not applied. To work around this issue, you could add a custom word for the number `1` and apply the same `display_as` field to that word.

For more information about working with these features, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting) and [Numeric redaction](/docs/speech-to-text?topic=speech-to-text-formatting#numeric-redaction).

### What happens when I add or modify a custom word?
{: #parseWord-ng}

How the service responds to a request to add or modify a custom word depends on the fields and values that you specify. It also depends on whether the character sequences of the word exist in the base model's character sequences.

-   **Omit both the `sounds_like` and `display_as` fields:**
    -   The service sets the `display_as` field to the value of the `word` field. The service does not attempt to set the `sounds_like` field to a pronunciation of the word.

-   **Specify only the `sounds_like` field:**
    -   *If the `sounds_like` field is valid,* the service sets the value of the `sounds_like` field to the specified value. The service also sets the `display_as` field to the value of the `word` field.
    -   *If the `sounds_like` field is invalid:*
        -   The `POST /v1/customizations/{customization_id}/words` method adds an `error` field to the word in the model's words resource.
        -   The `PUT /v1/customizations/{customization_id}/words/{word_name}` method fails with a 400 response code and an error message. The service does not add the word to the words resource.

-   **Specify only the `display_as` field:**
    -   The service sets the `display_as` field to the specified value. The service does not attempt to set the `sounds_like` field to a pronunciation of the word.

-   **Specify both the `sounds_like` and `display_as` fields:**
    -   *If the `sounds_like` field is valid,* the service sets the `sounds_like` and `display_as` fields to the specified values.
    -   *If the `sounds_like` field is invalid,* the service responds as it does in the case where the `sounds_like` field is specified but the `display_as` field is not.

### Additional transcription efforts
{: #parseWord-additional-ng}

For custom language models that are based on large speech models and next-generation models, the service makes additional efforts to ensure the most effective transcription:

-   For sounds-like pronunciations for custom words, the service uses the reverse of the sounds-like as well as its definition in a custom word. For example, given a `sounds_like` field of `I triple E` for the word `IEEE`, the service also effectively uses a reverse sounds-like of `IEEE` for the "word" `I triple E`. This enhances the application of sounds-like pronunciations for speech recognition. (Note that it is not possible for users to create custom words that contain spaces.)
-   For acronyms that are parsed from corpora or defined as custom words, the service makes additional speech recognition efforts. An acronym is any word that consists of two or more consecutive uppercase letters. If the acronym contains one or more vowels, the service attempts to recognize the acronym as a series of individual characters *and* as a pronounced word.

    For example, the acronym `NASA` can be read as four individual letters or pronounced as a word that sounds like `nassa`. The service checks for both during speech recognition. This enhances its ability to represent acronyms correctly in a transcript.

## Validating a words resource for large speech models and next-generation models
{: #validateModel-ng}
{: troubleshoot}
{: support}

Especially when you add a corpus to a custom language model or add multiple custom words at one time, make sure to perform the following validation:

-   *Look for typographical and other errors in corpora.* Especially when you add corpora, which can be large, mistakes are easily made. Make sure to review the corpus closely before adding it to the model.
-   *Look for typographical and other errors in custom words.* Make sure to review closely custom words that you add directly to a model.
-   *Verify the sounds-like pronunciations.* The service tries to generate sounds-like pronunciations for custom words for which none are specified. In most cases, these pronunciations are sufficient. But the service cannot generate a pronunciation for all words, so you must review the word's definition to ensure that it is complete and valid. Reviewing the pronunciations for accuracy is also recommended for words that have unusual spellings or are difficult to pronounce, and for acronyms and technical terms.

Typographical errors have the unintended consequence of modifying a custom model for nonexistent words, as do ill-formed HTML tags that are left in a corpus file.

To validate and, if necessary, correct a custom word for a custom model, use the following methods:

-   List all of the words from a custom model by using the `GET /v1/customizations/{customization_id}/words` method or query an individual word with the `GET /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Listing custom words from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   Modify words in a custom model to correct errors or to add display-as values by using the `POST /v1/customizations/{customization_id}/words` or `PUT /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Working with custom words for large speech models and next-generation models](#workingWords).
-   Delete extraneous words that are introduced in error (for example, by typographical or other mistakes) by using the `DELETE /v1/customizations/{customization_id}/words/{word_name}` method. For more information, see [Deleting a word from a custom language model](/docs/speech-to-text?topic=speech-to-text-manageWords#deleteWord).
