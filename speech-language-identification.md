---

copyright:
  years: 2025
lastupdated: "2025-11-03"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Spoken language identification
{: #speech-language-identification}

Language identification (LID) automatically detects the spoken language in audio streams. The model continuously processes incoming audio and returns the identified language when it reaches a confidence level higher than the specified threshold (0.99 by default). 

The model continuously detects and updates the spoken language throughout the session when audio is streaming. You can end the session after you receive the initial response. 

This feature quickly detects the spoken language and helps client applications switch languages for downstream processing. It is especially useful in environments such as call centers or other applications that support multiple languages.

## Requesting language identification through API 
{: #lid-api-integration}

To request language identification through API:

1. Send a request to the endpoint `/v1/detect_language`. For more details, see [Call API request](#lid-update-request).
2. Optionally, include the `lid_confidence` parameter to set a custom confidence threshold for detection. The value ranges from 0 to 1 (default is 0.99).
   - A higher value (closer to 1) means that the model returns a language only when it is confident, reducing the chance of incorrect detection. 

   - A smaller value allows the model to return results even with less confidence, which can be useful for faster or more responsive applications. 

   For example, setting `lid_confidence=0.9` help ensure that only highly confident results are returned, while `lid_confidence=0.5` might return a language more quickly but with less certainty. 
2. Process the results by accessing the `language_info` object. For more details, see [Process the response](#lid-handle-response).
3. Check the confidence scores to assess the quality of the detection. 

### Call API request
{: #lid-update-request}

Example request:

```curl
curl -X POST -u "apikey:<apikey>" \
  -H "Content-Type: audio/wav" \
  --data-binary @{path}New_Recording.wav \
  "{url}/v1/detect_language"
```
{: codeblock}

```curl
curl -X POST -u "apikey:<apikey>" \
  -H "Content-Type: audio/wav" \
  --data-binary @{path}New_Recording.wav \
  "{url}/v1/detect_language?lid_confidence=0.9" 
```
{: codeblock}

### Process the response
{: #lid-handle-response}

When language identification is requested, the response includes a `language_info` object within the `results` object. 

Example response:

```JSON
{
  "results": [
      {
         "language_info": [
            {
               "confidence": 0.96,
               "language": "fr"
            }
         ]
      },
      {
         "language_info": [
            {
               "confidence": 1.0,
               "language": "fr"
            }
         ]
      },
      {
         "language_info": [
            {
               "confidence": 0.97,
               "language": "fr"
            }
         ]
      },
  ]
}

```
{: codeblock}

## Supported languages
{: #lid-supported-languages}

Language identification currently supports the following languages:
-   English
-   French
-   German
-   Japanese
-   Portuguese
-   Spanish
-   Chinese
-   Italian
-   Korean
-   Swedish
-   Hindi
-   Dutch
-   Arabic
-   Czech
