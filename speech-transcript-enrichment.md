---

copyright:
  years: 2025
lastupdated: "2025-10-14"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Speech transcript enrichment
{: #speech-transcript-enrichment}

Use the speech transcript enrichment feature to improve the readability and usability of raw Automatic Speech Recognition (ASR) transcripts. This post-processing service automatically adds punctuation and applies intelligent capitalization to enhance the structure and clarity of spoken content.

The service inserts punctuation marks such as periods, commas, question marks, and exclamation points. It also capitalizes sentence beginnings, proper nouns, acronyms, and brand names based on context. Confidence scoring help ensure accurate and reliable enrichment.

By using speech transcript enrichment, you can produce clean, professional transcripts that are ready for review, publication, or integration into downstream applications.

## Integrate Speech transcript enrichment through API
{: #ste-api-integration}

To enable enrichment in API calls:

1. Add `enrichments=punctuation` parameter to the recognition request. For more details, see [Update the recognition request](#ste-update-request).
2. Process the enhanced response from `enriched_results` object. For more details, see [Process enriched response](#ste-handle-enriched-response).
3. Access the confidence metrics for quality assessment.

### Update the recognition request
{: #ste-update-request}

To enable enrichment, add the `enrichments=punctuation` parameter to your recognition request.

Example request:

```curl
curl -X POST -u "apikey:<API-KEY>" \
  -H "Content-Type: audio/wav" \
  --data-binary @data/<your_wav_audio_file>.wav \
  "https://api.jp-tok.speech-to-text.watson.cloud.ibm.com/instances/<INSTANCE-ID>/v1/recognize?model=fr-FR_Telephony&enrichments=punctuation"
```
{: codeblock}

### Process the enriched response
{: #ste-handle-enriched-response}

When enrichment is enabled, the response includes an `enriched_results` object. This object contains the post-processed transcript with punctuation and capitalization that is applied, along with metadata for quality and traceability.

Example response:

```JSON
{
   "speaker_labels": [
      {
         "from": 52.52,
         "to": 52.7,
         "speaker": 0,
         "confidence": 1.0,
         "final": false
      }
   ],
   "enriched_results": {
      "transcript": {
         "text": "Oh là, et bon, il faudrait me payer ma complation, là. Et vous avez ajouté à le fournir, me payer ma complatigne.",
         "timestamp": {
            "from": 0.22,
            "to": 59.68
         }
      },
      "status": "success"
   }
}
```
{: codeblock}

For more information about speaker labels and timestamps, see [Speaker Labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels) and [Word timestamps](/docs/speech-to-text?topic=speech-to-text-metadata#word-timestamps).

## Supported languages
{: #ste-supported languages}

Speech transcript enrichment currently supports the following languages:

-   English (US, UK, Australia, India)
-   French (France, Canada)
-   German
-   Italian
-   Portuguese (Brazil, Portugal)
-   Spanish (Spain, Latin America, Argentina, Chile, Colombia, Mexico, Peru)
-   Japanese
