---

copyright:
  years: 2015, 2022
lastupdated: "2022-08-12"

keywords: known limitations,known issues

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Known limitations
{: #known-limitations}

The {{site.data.keyword.speechtotextshort}} service has the following known limitations.
{: shortdesc}

## Issue: Interim results for previous-generation models
{: #limitation-interim-results}

**27 April 2021:** When you use previous-generation models with the WebSocket interface, if you request both speaker labels and interim results, the speaker labels response includes an extra object. The final results duplicate the object for the last speaker label. Instead of appearing once with `"final": true`, the object appears twice: once with `"final": false` and once with `"final": true`.

For example, a WebSocket request that includes both speaker labels and interim results sends a `start` message like the following:

```javascript
var message = {
  action: 'start',
  speaker_labels: true,
  interim_results: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

To indicate final results for speaker labels, the service is supposed to return a response of the following form:

```json
{
  "speaker_labels": [
    . . .
    {
      "from": 1.01,
      "to": 1.75,
      "speaker": 0,
      "confidence": 0.75,
      "final": false
    },
    {
      "from": 1.76,
      "to": 2.50,
      "speaker": 1,
      "confidence": 0.80,
      "final": true
    }
  ]
}
```
{: codeblock}

Instead, the service returns final results like the following. Note that the object for the last speaker label, which begins at `1.76` and ends at `2.50`, is returned twice. The `"final": true` indication is associated with the second instance of the label.

```json
{
  "speaker_labels": [
    . . .
    {
      "from": 1.01,
      "to": 1.75,
      "speaker": 0,
      "confidence": 0.75,
      "final": false
    },
    {
      "from": 1.76,
      "to": 2.50,
      "speaker": 1,
      "confidence": 0.80,
      "final": false
    }
  ]
}{
  "from": 1.76,
  "to": 2.50,
  "speaker": 1,
  "confidence": 0.80,
  "final": true
}
```
{: codeblock}

For more information about using speaker labels with interim results, see [Requesting interim results for speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels#speaker-labels-interim).

## Issue: Supported features for speaker labels is always `true`
{: #limitation-supported-features}

**6 August 2020:** The `GET /v1/models` and `GET /v1/models/{model_id}` methods list information about language models. Under `supported_features`, the `speaker_labels` field indicates whether you can use the `speaker_labels` parameter with a model. At this time, the field returns `true` for all models.

However, speaker labels are supported as beta functionality only for US English, Australian English, German, Japanese, Korean, and Spanish (both broadband and narrowband models) and UK English (narrowband model only). Speaker labels are not supported for any other models. Do not rely on the field to identify which models support speaker labels.

For more information about speaker labels and supported models, see [Speaker labels](/docs/speech-to-text?topic=speech-to-text-speaker-labels).

## Issue: The `progress` field for custom models is not continuous
{: #limitation-progress-field}

The following methods include a `progress` field in the information that they return for a custom model:

-   `GET /v1/customizations`
-   `GET /v1/customizations/{customization_id}`
-   `GET /v1/acoustic_customizations`
-   `GET /v1/acoustic_customizations/{customization_id}`

The field returns the progress for a training or upgrade operation. Currently, the value of the field is `100` if the status is `available`; otherwise, it is `0`.

When you monitor the training or upgrading of a custom model, poll the value of the `status` field, not the value of the `progress` field. If the operation fails for any reason, the value of the `status` field changes to reflect the failure; the value of the `progress` field remains `0`.
