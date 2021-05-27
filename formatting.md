---

copyright:
  years: 2015, 2021
lastupdated: "2021-05-26"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Response formatting and filtering
{: #formatting}

The {{site.data.keyword.speechtotextfull}} service provides three features that you can use to parse transcription results. You can format a final transcript to include more conventional representations of certain strings and to include punctuation. You can redact sensitive numeric information from a final transcript, and you can filter profanity from most transcription results. All of these features are beta functionality and are restricted to certain languages.
{: shortdesc}

## Smart formatting
{: #smart-formatting}

The smart formatting feature is beta functionality that is available for US English, Japanese, and Spanish.
{: beta}

The `smart_formatting` parameter directs the service to convert the following strings into more conventional representations:

-   Dates
-   Times
-   Series of digits and numbers
-   Phone numbers
-   Currency values (for US English and Spanish)
-   Internet email and web addresses (for US English and Spanish)

You set the `smart_formatting` parameter to `true` to enable smart formatting. By default, the service does not perform smart formatting. The service applies smart formatting just before it returns the final results to the client, when text normalization is complete. The conversion makes the transcript more readable and promotes better post-processing of transcription results by representing these artifacts as they would normally be written.

### What results does smart formatting affect?
{: #smart-formatting-effects}

Smart formatting impacts some transcription results and not others:

-   Smart formatting affects only words in the `transcript` field of final results, those results for which the `final` field is `true`. It does not affect interim results, for which `final` is `false`.
-   Smart formatting does not affect words in other fields of the response. For example, smart formatting is not applied to response data in the `timestamps` or `alternatives` fields.
-   Hesitation markers and disfluencies, such as "uhm" and "uh", can adversely impact the conversion of phrases and strings by smart formatting for some languages.
    -   *For US English,* smart formatting suppresses hesitation markers from the `transcript` field for final results.
    -   *For Japanese,* hesitation markers continue to appear in final results.
    -   *For both US English and Japanese,* hesitation markers continue to appear in interim results.
    -   *For Spanish,* the service does not produce hesitation markers for any results.

    For more information, see [Hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

### Language differences
{: #smart-formatting-differences}

Smart formatting is based on the presence of obvious keywords in the transcript. Because of differences among the supported languages, smart formatting works slightly differently for each language. The following sections describe the strings and content that trigger smart formatting changes for [US English and Spanish](#smart-formatting-english-spanish) and for [Japanese](#smart-formatting-japanese).

#### US English and Spanish
{: #smart-formatting-english-spanish}

-   Times are identified by keywords such as `AM`, `PM`, or `EST`.
-   Military times are converted if they are identified by the keyword `hours` (*US English*) or `horas` (*Spanish*).
-   Phone numbers must be either `911` or a number that contains 10 or 11 digits and starts with the number `1`.
-   Currency symbols are substituted for the following strings in appropriate contexts:
    -   *For US English,* dollar, cent, and euro.
    -   *For Spanish,* dolar, peso, peseta, libras esterlinas, libra, and euro.
-   Internet email addresses are converted in some cases. Specifically, the service converts email addresses if the input audio uses the phrasing `email address ... {address}`. The following examples show a correct conversion of spoken phrases:
    -   `My email address is j dot d o e at i b m dot com` becomes `My email address is j.doe@ibm.com`.
    -   `Mi correo electronico es j punto d o e arroba i b m punto com` becomes `Mi correo electronico es j.doe@ibm.com`.
-   Internet web addresses are converted in their short forms. Fully qualified web addresses are not converted. The following examples show complete conversions:
    -   `I saw the story on yahoo dot com` becomes `I saw the story on yahoo.com`.
    -   `Vi la historia en yahoo punto com` becomes `Vi la historia en yahoo.com`.

    The following examples show incomplete conversions:
    -   `I saw the story on w w w dot yahoo dot com` becomes `I saw the story on w w w .yahoo.com`.
    -   `Vi la historia en w w w punto yahoo punto com` becomes `Vi la historia en w w w .yahoo.com`.
-   Conversion of large numbers and currency values can be challenging. The service converts digits and many numbers well. But larger and more complex numbers and currency values work best with more precise phrasing. For example, the service correctly converts the following transcripts because of their precise wording:
    -   `sixty nine thousand five hundred sixty dollars and twenty five cents` becomes `$69560.25`,
    -   `sixty nine thousand five hundred sixty dollars point twenty five` becomes `$69560.25`.

    But the service cannot correctly convert the following transcripts due to their looser phrasing:
    -   `sixty nine thousand five sixty dollars and twenty five cents` becomes `60 9000 $560.25`.
    -   `sixty nine thousand five sixty dollars point twenty five` becomes `60 9000 $560.25`.

    To correctly convert a greater possible variety of complex numbers, you need to experiment with the results of smart formatting and customize your own post-processing utilities.
-   *For US English,* certain punctuation symbols are added for special keywords that occur in appropriate places. The service substitutes punctuation symbols for the following keyword strings based on where it finds them in a transcript.

    <table style="width:50%">
      <caption>Table 2. Smart formatting punctuation keywords for US English</caption>
      <tr>
        <th style="text-align:left">Keyword string</th>
        <th style="text-align:center">Resulting punctuation</th>
      </tr>
      <tr>
        <td>
          `Comma`
        </td>
        <td style="text-align:center">
          `,`
        </td>
      </tr>
      <tr>
        <td>
          `Period`
        </td>
        <td style="text-align:center">
          `.`
        </td>
      </tr>
      <tr>
        <td>
          `Question mark`
        </td>
        <td style="text-align:center">
          `?`
        </td>
      </tr>
      <tr>
        <td>
          `Exclamation point`
        </td>
        <td style="text-align:center">
          `!`
        </td>
      </tr>
    </table>

    The service converts these keyword strings to symbols only in appropriate positions of a transcript. In the following example, the speaker says the word `period` at the end of the sentence. The service correctly differentiates between the noun that appears earlier in the sentence and the concluding punctuation.
    -   `the warranty period is short period` becomes `the warranty period is short.`

#### Japanese
{: #smart-formatting-japanese}

-   Phone numbers must be 10 or 11 digits and begin with valid prefixes for telephone numbers in Japan. For example, valid prefixes include `03` and `090`.
-   English words are converted to ASCII (*hankaku*) characters. For example, <code>&#65321;&#65314;&#65325;</code> is converted to `IBM`.
-   Ambiguous terms might not be converted if sufficient context is unavailable. For example, it is unclear whether <code>&#19968;&#26178;</code> and <code>&#21313;&#20998;</code> refer to times.
-   Punctuation is handled in the same way with or without smart formatting. For example, based on probability calculations, one of <code>&#12459;&#12531;&#12510;</code> or `,` is selected.
-   Strings that describe yen values are not replaced with the yen currency symbol.
-   Internet email and web addresses in any form are not converted.
-   The Japanese narrowband model (`ja-JP_NarrowbandModel`) includes some multigram word units for digits and decimal fractions. The service returns these multigram units regardless of whether you enable smart formatting. The following examples show the units that the service returns. The numbers in parentheses show the equivalent Arabic numeric expression for each unit.
    -   *Digits:* <code>&#12295;&#19968;</code> (01), ..., <code>&#12295;&#20061;</code> (09), <code>&#19968;&#12295;</code> (10), ..., <code>&#20061;&#12295;</code> (90)
    -   *Decimal fractions:* <code>&#12295;&#12539;</code> (0.), <code>&#19968;&#12539;</code> (1.), ..., <code>&#21313;&#12539;</code> (10.)

    The smart formatting feature understands and returns the multigram units that the model generates. If you apply your own post-processing to transcription results, you need to handle these units appropriately.

### Smart formatting results
{: #smart-formatting-results}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Information | Without smart formatting | With smart formatting |
|-------------|--------------------------|-----------------------|
| Dates | I was born on ten oh six nineteen seventy | I was born on 10/6/1970 |
|       | I was born on the ninth of December nineteen hundred | I was born on 12/9/1900 |
|       | Today is June sixth | Today is June 6 |
| Times | The meeting starts at nine thirty AM | The meeting starts at 9:30 AM |
|       | I am available at seven EST | I am available at 7:00 EST |
|       | We meet at oh seven hundred hours | We meet at 0700 hours |
| Numbers | The quantity is one million one hundred and one | The quantity is 1000101 |
|         | One point five is between one and two | 1.5 is between 1 and 2 |
| Phone numbers | Call me at nine one four two three seven one thousand | Call me at 914-237-1000 |
|               | Call me at one nine one four nine oh nine twenty six forty five | Call me at 1-914-909-2645 |
| Currency values | You owe me three thousand two hundred two dollars and sixty six | You owe me $3202.66 |
|                 | The dollar rose to one hundred and nine point seven nine yen from one hundred and nine point seven two yen | The dollar rose to 109.79 yen from 109.72 yen |
| Internet email and web addresses | My email address is john dot doe at foo dot com | My email address is john.doe@foo.com |
|                                  | I saw the story on yahoo dot com | I saw the story on yahoo.com |
| Combinations | The code is zero two four eight one and the date of service is May fifth two thousand and one | The code is 02481 and the date of service is 5/5/2001 |
|              | There are forty seven links on Yahoo dot com now | There are 47 links on Yahoo.com now |
{: caption="Table 3. Smart formatting example transcripts"}

### Smart formatting results for long pauses
{: #smart-formatting-long-pauses}

In cases where an utterance contains long enough pauses of silence, the service can split the transcript into two or more final results. This affects the contents of the response, as shown in the following examples.

| Audio speech | Formatted transcription results |
|--------------|---------------------------------|
| My phone number is nine one four five five seven three three nine two | "My phone number is 914-557-3392" |
| My phone number is nine one four *&lt;pause&gt;* five five seven three three nine two | "My phone number is 914"<br/>"5573392" |
{: caption="Table 4. Smart formatting example transcripts for long pauses"}

For more information about specifying a pause interval that affects the service's response, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).

### Smart formatting example
{: #smart-formatting-example}

The following example requests smart formatting with a recognition request by setting the `smart_formatting` parameter to `true`. The following sections show the effects of smart formatting on the results of a request.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?smart_formatting=true"
```
{: pre}

## Numeric redaction
{: #numeric-redaction}

The numeric redaction feature is beta functionality that is available for US English, Japanese, and Korean.
{: beta}

The `redaction` parameter directs the service to redact, or mask, numeric data from final transcripts. The feature redacts any number that has three or more consecutive digits by replacing each digit with an `X` character. It is intended to redact sensitive numeric data, such as credit card numbers.

By default, the service does not redact numeric data. Set the `redaction` parameter to `true` to enable numeric redaction. When you enable redaction, the service automatically enables smart formatting by setting the `smart_formatting` parameter to `true`, regardless of whether you explicitly disable that feature. To ensure maximum security, the service also disables the following parameters when you enable redaction:

-   The service disables keyword spotting, regardless of whether you specify values for the `keywords` and `keywords_threshold` parameters.
-   The service disables maximum alternatives, regardless of whether you specify a value greater than 1 for the `max_alternatives` parameter. The service returns only a single final transcript.
-   The service disables interim results for the WebSocket interface, regardless of whether you set the `interim_results` parameter to `true`.

The design of the feature parallels the existing smart formatting feature. The service applies redaction only to the final transcript of a recognition request, just before it returns the results to the client and after text normalization is complete.

### Language differences
{: #numeric-redaction-differences}

The feature works exactly as described for US English models but has the following differences for Japanese and Korean models.

#### Japanese
{: #numeric-redaction-japanese}

Japanese redaction has the following differences:

-   In addition to masking strings of three or more consecutive digits, redaction also masks street addresses and numbers, regardless of whether they contain fewer than three digits.
-   Similarly, redaction also masks date information in Japanese-style birth dates. In Japanese, date information is usually presented in Common Era format but sometimes follows Japanese style, particularly for birth dates. In this case, the year and month are masked even though they contain just one or two digits. For example, numeric redaction changes the following string as shown.

    <table style="width:50%">
      <caption>Table 5. Example redaction of Japanese-style birth date</caption>
      <tr>
        <th style="text-align:left">Without redaction</th>
        <th style="text-align:left">With redaction</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### Korean
{: #numeric-redaction-korean}

Korean redaction has the following differences:

-   The smart formatting feature is not supported. The service still performs numeric redaction for Korean, but it performs no other smart formatting.
-   Isolated digit characters are reduced, but possible digit characters that are included as part of Korean phrases are not. For example, the character <code>&#51060;</code> in the following phrase is not replaced by an `X` because it is adjacent to the following character:

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    If the <code>&#51060;</code> character were separated from the following character by a space, it would be replaced by an `X`, as described in [Numeric redaction results](#numeric-redaction-results).

### Numeric redaction results
{: #numeric-redaction-results}

The following table shows examples of final transcripts both with and without numeric redaction in each supported language.

| Language | Without redaction | With redaction |
|----------|-------------------|----------------|
| US English | my credit card number is four one four seven two | my credit card number is XXXXX |
| Japanese | &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377; | &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377; |
| Korean | &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796; | &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796; |
{: caption="Table 6. Numeric redaction example transcripts"}

### Numeric redaction example
{: #numeric-redaction-example}

The following example requests numeric redaction with a recognition request by setting the `redaction` parameter to `true`. Because the request enables redaction, the service implicitly enables smart formatting with the request. The service effectively disables the other parameters of the request so that they have no effect: The service returns a single final transcript and recognizes no keywords.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?&redaction=true&max_alternatives=3&keywords=birth%2Cbirthday&keywords_threshold=0.5"
```
{: pre}

## Profanity filtering
{: #profanity-filtering}

The profanity filtering feature is generally available for US English and Japanese only.
{: note}

The `profanity_filter` parameter indicates whether the service is to censor profanity from its results. By default, the service obscures all profanity by replacing it with a series of asterisks in the transcript. Setting the parameter to `false` displays words in the output exactly as transcribed.

The service censors profanity from all final transcripts and from any alternative transcripts. It also censors profanity from results that are associated with word alternatives, word confidence, and word timestamps. The sole exception is keyword spotting, for which the service returns all words as specified by the user, regardless of whether `profanity_filter` is `true`.

### Profanity filtering example
{: #profanity-filtering-example}

The following example shows the results for a brief audio file that is transcribed with the default `true` value for the `profanity_filter` parameter. The request also sets the `word_alternatives_threshold` parameter to a relatively high value of `0.99` and the `word_confidence` and `timestamps` parameters to `true`.

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
