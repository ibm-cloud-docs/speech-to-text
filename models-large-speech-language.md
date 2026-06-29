---

copyright:
  years: 2024, 2026
lastupdated: "2026-06-29"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

#  Large speech languages and models
{: #models-large-speech-languages}

The {{site.data.keyword.speechtotextfull}} service supports a growing collection of Large Speech Models (LSMs) that improve the speech recognition capabilities of the service's previous-generation models. The model name is the locale, which consists of the language code and the region or country code that is separated by a dash. For example, `en-US` is for English that is spoken in the United states. LSMs are large models. They have a large number of trainable parameters and are trained on large amounts of audio. Because of their large size, the large amounts of training material they are built on, and the state-of-the-art architecture and training recipe that is used to build them, these models deliver more transcription accuracy compared to the previous models available.

You can use these models for both Telephony use cases and Broadband use cases. 
{: shortdesc}

## Supported large speech model languages
{: #models-supported-languages}

The table lists the large speech models that are available for each language. Unless otherwise labeled as
[IBM Cloud]{: tag-ibm-cloud}, [IBM Cloud Pak for Data]{: tag-cp4d} or [IBM Software Hub]{: tag-teal}, a model is supported for all versions of the service.

| Language |  Model name | Status | 
|------------------------|:-----------:|:----------------------------------------|
| Dutch (Netherlands) | `nl-NL` | [IBM Cloud]{: tag-ibm-cloud} 10 April 2026 \n [IBM Software Hub]{: tag-teal} 10 April 2026 | 
| English (Australian) | `en-AU` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English (Indian) | `en-IN` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English (United Kingdom) | `en-GB` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| English (United States) | `en-US` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| French (Canadian) | `fr-CA` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| French (France) | `fr-FR` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| German | `de-DE` | [IBM Cloud]{: tag-ibm-cloud} 19 November 2024 | 
| Italian (Italy) | `it-IT` | [IBM Cloud]{: tag-ibm-cloud} 15 May 2026 \n [IBM Software Hub]{: tag-teal} 15 May 2026 | 
| Japanese | `ja-JP` | [IBM Cloud]{: tag-ibm-cloud} 20 May 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 12 June 2024 | 
| Portuguese (Brazilian) | `pt-BR` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024  | 
| Portuguese (Portugal) | `pt-PT` | [IBM Cloud]{: tag-ibm-cloud} 23 August 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 |
| Spanish (Castilian) | `es-ES` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Argentinian) | `es-AR` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Chilean) | `es-CL` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Colombian) | `es-CO` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Mexican) | `es-MX` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
| Spanish (Peruvian) | `es-PE` | [IBM Cloud]{: tag-ibm-cloud} 18 June 2024 \n [IBM Cloud Pak for Data]{: tag-cp4d} 23 August 2024 | 
{: caption="Large speech models"}

## Supported features for large speech models
{: #models-lsm-supported-features}

The large speech models are supported for use with a large subset of the service's speech recognition features. In cases where a supported feature is restricted to certain languages, the same language restrictions usually apply to large speech models, previous-generation, and next-generation models.

-   For more information about the parameters that you can use with large speech models, including their language support and whether the parameters are GA or beta, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
-   For more information about large speech models' support for customization, see [Customization support for large speech models](/docs/speech-to-text?topic=speech-to-text-custom-support#custom-language-support-lsm).

Large speech models support all speech recognition parameters and headers *except*:

-   `acoustic_customization_id` (Large speech models do not support acoustic model customization.)
-   `keywords` and `keywords_threshold`
-   `word_alternatives_threshold`
-   `grammar_name` (Large speech models do not support grammar customization.)
-   `character_insertion_bias`

`en-US` large speech model now supports `low_latency`.

Large speech models also differ from previous-generation models with respect to the following additional feature:

-   Large speech models do not produce hesitation markers. They instead include the actual hesitations in transcription results. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

## The parameter_set request parameter
{: #models-lsm-parameter-set}

Large speech models support a `parameter_set` request parameter that applies a curated set of tuned values in a single call. You can pass `parameter_set=enhanced` to opt in to improved recognition quality without setting individual parameters manually.

When `parameter_set` is omitted or set to `default`, the service behaves exactly as it did before this parameter existed. No values are overridden.

### Supported values
{: #models-lsm-parameter-set-values}

The table lists the supported values that can be used with the `parameter_set` request parameter.

| Value | Behavior |
|---|---|
| (Omitted) | Default behavior. No tuned values are applied. |
| `default` | Same as omitted. |
| `enhanced` | Applies the recommended tuning per language. |
{: caption="Supported values for the parameter_set request parameter"}

### Combining parameter_set with other parameters
{: #models-lsm-parameter-set-behavior}

When you specify `parameter_set=enhanced`, the preset values take precedence over any values you set individually for the parameters that the preset tunes. To set the parameters individually, omit `parameter_set` (or set `parameter_set=default`) and configure them directly. 

The `enhanced` preset tunes the following parameters:

- `sad_module`
- `speech_detector_sensitivity`
- `background_audio_suppression`
- `character_insertion_bias`
- `end_of_phrase_silence_time`

Use `parameter_set=enhanced` to apply recommendations for the specified language in a single setting rather than configuring each parameter individually. For more information on recommended parameter configurations and general parameter tuning guidance, see [Recommended parameter configurations](#models-lsm-recommended-configs) and [Recommended query parameters for large speech models](#models-lsm-recommended-parameters).

### Availability and usage
{: #models-lsm-parameter-set-availability}

The table lists the availability based on model and Websocket usage.

| Description | Support |
|---|---|
| Previous-generation models | Not available |
| Next-generation models | Not available |
| WebSocket | Parameter of JSON `start` message |
{: caption="Availability and usage support for the parameter_set request parameter"}

#### Example WebSocket `start` message
{: #models-lsm-parameter-set-example}

```json
{
  "action": "start",
  "content-type": "audio/wav",
  "parameter_set": "enhanced"
}
```

### Notes
{: #models-lsm-parameter-set-notes}

Consider the following when you use `parameter_set` request parameter:

- Only large speech models support `parameter_set`. Sending it to a non-LSM model is silently ignored.
- Five base models cover thirteen locale codes through Watson language alias chain:
   - English: `en-AU`, `en-GB`, `en-IN`
   - Spanish: `es-AR`, `es-CL`, `es-CO`, `es-ES`, `es-MX`, `es-PE`
   - French: `fr-CA`, `fr-FR`
   - Portuguese: `pt-BR`, `pt-PT`
   - German: `de-DE`

### Recommended parameter configurations
{: #models-lsm-recommended-configs}

The following configurations provide recommended baseline settings across supported languages.

- **English (en-*)**

```json
{
  "sad_module": 2,
  "speech_detector_sensitivity": 0.9,
  "background_audio_suppression": 0.1,
  "character_insertion_bias": -1.0,
  "end_of_phrase_silence_time": 1.0
}
```

- **French (fr-FR)**

```json
{
  "sad_module": 2,
  "speech_detector_sensitivity": 0.8,
  "background_audio_suppression": 0.0,
  "character_insertion_bias": 0.0,
  "end_of_phrase_silence_time": 1.0
}
```

- **Spanish (es-*)**

```json
{
  "sad_module": 1,
  "speech_detector_sensitivity": 0.2,
  "background_audio_suppression": 0.0,
  "character_insertion_bias": 0.0,
  "end_of_phrase_silence_time": 1.0
}
```

- **Brazilian Portuguese (pt-BR)**

```json
{
  "sad_module": 2,
  "speech_detector_sensitivity": 0.8,
  "background_audio_suppression": 0.0,
  "character_insertion_bias": -0.3,
  "end_of_phrase_silence_time": 1.0
}
```

- **German (de-DE)**

```json
{
  "sad_module": 1,
  "speech_detector_sensitivity": 0.5,
  "background_audio_suppression": 0.0,
  "character_insertion_bias": -0.2,
  "end_of_phrase_silence_time": 1.0
}
```

### Parameter guidelines
{: #models-lsm-parameter-guidelines}

The following guidelines provide recommended parameter values and tuning strategies to help you achieve better speech recognition accuracy, reduce timeout issues, and improve overall model performance.

1. **sad_module**

    - Recommended: `2` (default is `1`)
    - Provides improved speech/silence segmentation compared to the default CNN-based SAD
    - Helps reduce timeout issues caused by misclassification of noise as speech

2. **speech_detector_sensitivity**

    - Higher values lead to more aggressive speech detection
    - Large speech models generally benefit from higher sensitivity
    - Recommended range: `0.6` to `0.8` depending on language and noise conditions

3. **end_of_phrase_silence_time**

    - Recommended: `0.7` to `0.8` (default is `0.0`)
    - Improves segmentation for conversational applications
    - Particularly beneficial for short structured utterances

4. **background_audio_suppression**

    - Helps improve recognition accuracy for speech in background noise
    - Should be tuned conservatively to avoid suppressing speech energy
    - Example: `0.1` for moderate noise environments

5. **character_insertion_bias**

    - Default: `-0.22`
    - Slight increase (for example, `-0.1` to `0.0`) can help reduce deletion errors
    - Use caution: Positive values may introduce insertion errors

## Recommended query parameters for large speech models
{: #models-lsm-recommended-parameters}

Large speech models support a wide range of configurable query parameters that can be tuned to optimize performance across different languages and use cases. While default parameter values provide strong baseline performance, adjusting specific parameters can significantly improve:

- transcription stability
- speech segmentation and endpointing behavior
- recognition accuracy, especially in noisy or variable environments

Use this approach if you want to customize the parameters individually instead of using the preset values set by `parameter_set=enhanced`.

### Target use cases
{: #models-lsm-target-use-cases}

The following recommendations are designed for conversational and interactive applications, including:

- Virtual assistants
- Healthcare voice interfaces
- Banking and insurance systems
- Customer support and contact center automation

These use cases typically involve short to medium-length utterances, frequent turn-taking interactions, and variable background noise conditions.

### Supported parameters
{: #models-lsm-supported-tuning-parameters}

The following parameters are commonly tuned with large speech models:

- `sad_module` - Selects the Speech Activity Detection (SAD) module.
- `speech_detector_sensitivity` - Controls how aggressively speech is detected. Higher values increase sensitivity to speech onset.
- `background_audio_suppression` - Controls noise suppression.
- `character_insertion_bias` - Controls the balance between insertion and deletion errors.
- `end_of_phrase_silence_time` - Defines the duration of silence (in seconds) required to finalize an utterance.

For more information about these parameters, see the [Parameter summary](/docs/speech-to-text?topic=speech-to-text-summary).
