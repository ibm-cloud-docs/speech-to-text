---

copyright:
  years: 2025
lastupdated: "2025-10-07"

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

-   date - Format input as a date.
-   alphanum – Treat input as an alphanumeric string.
-   number – Format input as a numeric value.
-   email – Interpret input as an email address.

By specifying a hint, developers can enhance the formatter’s ability to deliver results that more closely match user expectations. For more information on smart formatter, see [Smart Formatter](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).

## Enable hints in API calls
{: #hints-enable-in-api}

To enable `hints` in API calls, add `hints=date` or `hints=alphanum` or `hints=number` or `hints=email` parameter to the recognition request.

Example request:

```curl
curl -X POST -u "apikey:<apikey>" \
  -H "Content-Type: audio/wav" \
  --data-binary @{path}New_Recording.wav \
  "{url}/v1/recognize?model=en-US&hints=date"

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

You can also add the `strict` attribute along with any hint to remove unwanted context from the output.

Example request:
```curl
curl -X POST -u "apikey:<apikey>" \
  -H "Content-Type: audio/wav" \
  --data-binary @{path}New_Recording.wav \
  "{url}/v1/recognize?model=en-US&hints=date,strict"

```
{: codeblock}

For example, if the input is `My Birthday is on June twenty seven two thousand fourteen`, the output with `date` attribute is `My Birthday is on 06/27/2014`. The output with both `date` and `strict` attributes is `06/27/2014`.

## Limitations
{: #hints-limitations}

The `hints` parameter currently supports only a single language, English (US).
