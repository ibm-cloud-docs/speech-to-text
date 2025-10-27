---

copyright:
  years: 2025
lastupdated: "2025-10-27"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Hints parameter
{: #hints-parameter}

To improve post-processing accuracy and better align with user intent, the Smart Formatter now supports a new `hints` parameter.
The `hints` parameter helps guide formatting by indicating the expected attribute type. This parameter allows the formatter to interpret and structure data more effectively, especially when user input is ambiguous or loosely structured.

## Supported values
{: #hints-supported-values}

The `hints` parameter supports the following values:

| Hint |  Description | Resolves Ambiguity In | Example |
|------|--------------|------------------------|---------|
| `date` | Formats input as a date. | Differentiates between numeric expressions and date references. For example, spoken input like `six twenty seven twenty fourteen` might be interpreted either as a number or a date. When the date hint is specified, it is correctly formatted as a date. | `six twenty seven twenty fourteen` is formatted as `06/27/2014`. |
| `alphanum` | Formats input as an alphanumeric string (letters and numbers combined). | In an audio input, when a speaker says `o`, it might refer to either the letter `o` or the digit `0`. By specifying the alphanum hint, the Smart Formatter preserves `o` as a letter instead of being converted to zero.  | `A o 5 B` is formatted as `Ao5B`. |
{: caption="Supported values for hints parameter"}

In addition to `date` and `alphanum`, `hints` parameter also supports the following values:

-   `number` – Interprets numeric sequences correctly in ambiguous contexts. 
-   `email` – Resolves ambiguities in spoken email addresses. 
-   `phonenumber` - Distinguishes phone numbers from other numeric expressions. 

By specifying a hint, developers can enhance the formatter’s ability to deliver results that more closely match user expectations. For more information on smart formatter, see [Smart Formatter](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).

## Using the hints parameter
{: #using-the-hints-parameter}

The `hints` parameter works only when Smart Formatting is enabled with version 2.0. To use it, include the following parameters in your request: 

- `smart_formatting=true` 
- `smart_formatting_version=2.0`

Currently, `date` and `alphanum` are the supported hints. 

Example request:

```curl
curl -X POST -u "apikey:"  
-H "Content-Type: audio/wav"  
--data-binary @ {path} New_Recording.wav  
"{URL}/v1/recognize?model=en-US&hints=date&smart_formatting=true&smart_formatting_version=2.0" 
```
{: codeblock}

Example response:

```JSON
text msg received %s { 
   "result_index": 0, 
   "results": [ 
      { 
         "final": true, 
         "alternatives": [ 
            { 
               "transcript": "9/9/88 ", 
               "confidence": 0.93 
            } 
         ] 
      } 
   ] 
} 
```
{: codeblock}

### Strict attribute
{: #strict-attribute}

The `strict` attribute can be used along with any hint to remove unwanted or extra context from the output. It helps produce a more concise and precise result when the user input contains extra words that are not needed in the formatted output. 

Example request:
```curl
curl -X POST -u "apikey:"  
-H "Content-Type: audio/wav"  
--data-binary @ {path}New_Recording.wav  
"{URL}/v1/recognize?model=en-US&hints=date,strict&smart_formatting=true&smart_formatting_version=2.0" 
```
{: codeblock}

For example, if the input is `My Birthday is on June twenty seven two thousand fourteen`, the output with `date` attribute is `My Birthday is on 06/27/2014`. The output with both `date` and `strict` attributes is `06/27/2014`.

The `strict` attribute help ensure that only the essential formatted value is returned, removing surrounding context if present. 

## Supported languages
{: #hints-supported-languages}

The `hints` parameter currently supports English (US) (`en-US`).
