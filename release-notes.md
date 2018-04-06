---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-06"

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

# Release notes
{: #release-notes}

The following sections document the new features and changes that were included for each release and update of the {{site.data.keyword.speechtotextshort}} service. Unless otherwise noted, all changes are backward-compatible and are automatically and transparently available to all new and existing applications.
{: shortdesc}

## 26 March 2018
{: #March2018b}

-   The service now supports language model customization for the French language model, `fr-FR_BroadbandModel`. The French model is generally available for production use with language model customization.
-   The Spanish and Korean narrowband models, `es-ES_NarrowbandModel` and `ko-KR_NarrowbandModel`, and the French broadband model `fr-FR_BroadbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. However, if you have custom language or custom acoustic models based on either of these models, you must upgrade your custom models to take advantage of the updated base models by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    [Upgrading custom models](/docs/services/speech-to-text/custom-upgrade.html) provides complete details about the upgrade procedure. It presents rules for upgrading custom models, the effects of upgrading, and approaches for using the models following upgrade.
-   The `version` parameter of the following methods has been renamed `base_model_version`:

    -   `/v1/recognize` for WebSocket requests
    -   `POST /v1/recognize` for sessionless HTTP requests
    -   `POST /v1/sessions` for session-based HTTP requests
    -   `POST /v1/recognitions` for asynchronous HTTP requests

    The `base_model_version` parameter lets you specify the version of a base model that is to be used for speech recognition. For more information, see [Making recognition requests with upgraded custom models](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition) and [Base model version](/docs/services/speech-to-text/input.html#version).

## 1 March 2018
{: #March2018a}

The Spanish and French broadband models, `es-ES_BroadbandModel` and `fr-FR_BroadbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. However, if you have custom language or custom acoustic models based on either of these models, you must upgrade your custom models to take advantage of the updated base models by using the following methods:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

[Upgrading custom models](/docs/services/speech-to-text/custom-upgrade.html) provides complete details about the upgrade procedure. It presents rules for upgrading custom models, the effects of upgrading, and approaches for using the models following upgrade.

## 1 February 2018
{: #February2018}

The service now offers models for the Korean language for speech recognition: `ko-KR_BroadbandModel` for audio that is sampled at a minimum of 16 kHz, and `ko-KR_NarrowbandModel` for audio that is sampled at a minimum of 8 kHz. For more information, see [Languages and models](/docs/services/speech-to-text/input.html#models).

For language model customization, the Korean models are generally available for production use; for acoustic model customization, they are beta functionality. For more information, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

## Older releases
{: #older}

-   [14 December 2017](#December2017)
-   [2 October 2017](#October2017)
-   [14 July 2017](#July2017b)
-   [1 July 2017](#July2017a)
-   [22 May 2017](#May2017)
-   [10 April 2017](#April2017)
-   [8 March 2017](#March2017)
-   [1 December 2016](#December2016)
-   [22 September 2016](#September2016)
-   [30 June 2016](#June2016b)
-   [23 June 2016](#June2016a)
-   [10 March 2016](#March2016)
-   [19 January 2016](#January2016)
-   [17 December 2015](#December2015)
-   [21 September 2015](#September2015)
-   [1 July 2015](#July2015)

### 14 December 2017
{: #December2017}

-   The US English models, `en-US_BroadbandModel` and `en-US_NarrowbandModel`, have been updated for improved speech recognition. By default, the service automatically uses the updated models for all recognition requests. However, if you have custom language or custom acoustic models based on the US English models, you must upgrade your custom models to take advantage of the updated base models by using the following methods:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    [Upgrading custom models](/docs/services/speech-to-text/custom-upgrade.html) provides complete details about the upgrade procedure. It presents rules for upgrading custom models, the effects of upgrading, and approaches for using the models following upgrade. At this time, the methods apply only to the new US English base models, but the same information will apply to upgrades of other base models that become available in the future.

-   The various methods for making recognition requests now include a new `base_model_version` parameter that lets you initiate speech recognition requests that use either the older or upgraded versions of base and custom models. Although it is intended primarily for use with custom models that have been upgraded, the `base_model_version` parameter can also be used without custom models. For more information, see [Base model version](/docs/services/speech-to-text/input.html#version).
-   The service now supports acoustic model customization as beta functionality for all available languages. You can create custom acoustic models for broadband or narrowband models for all languages. For an introduction to customization, including acoustic model customization, see [The customization interface](/docs/services/speech-to-text/custom.html).
-   The service now supports language model customization for the UK English models, `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`. Although the service handles UK and US English corpora and custom words in a generally similar fashion, some important differences exist:
    -   For information about how the service parses corpora for UK English, see [Parsing of English (US and UK)](/docs/services/speech-to-text/language-resource.html#corpusLanguages-enUS).
    -   For information about creating sounds-like pronunciations for custom words in UK English, see [Guidelines for UK English](/docs/services/speech-to-text/language-resource.html#wordLanguages-enGB). Specifically, for UK English, you cannot use periods or dashes in sounds-like pronunciations.
-   Language model customization is now generally available (GA) for all supported languages: Japanese, Spanish, UK English, and US English.

### 2 October 2017
{: #October2017}

-   The customization interface now offers acoustic model customization. You can create custom acoustic models that adapt the service's base models to your environment and speakers. You populate and train a custom acoustic model on audio that more closely matches the acoustic signature of the audio that you want to transcribe. You then use the custom acoustic model with recognition requests to increase the accuracy of speech recognition.

    Custom acoustic models complement custom language models. You can train a custom acoustic model with a custom language model, and you can use both types of model during speech recognition.

    Acoustic model customization is a beta interface that is available only for US English, Spanish, and Japanese. For information about the languages supported by the customization interface and the level of support that is available for each language, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

    -   For information about the service's customization interface, see [The customization interface](/docs/services/speech-to-text/custom.html).
    -   For information about creating a custom acoustic model, see [Creating a custom acoustic model](/docs/services/speech-to-text/acoustic-create.html).
    -   For information about using a custom acoustic model, see [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html).
    -   For complete details about all methods of the customization interface, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   For language model customization, the service now includes a beta feature that lets you set an optional customization weight for a custom language model. A customization weight specifies the relative weight to be given to words from a custom language model as opposed to those from the service's base vocabulary. You can set a customization weight during both training and speech recognition. For more information, see [Using customization weight](/docs/services/speech-to-text/language-use.html#weight).
-   The `ja-JP_BroadbandModel` language model was upgraded to capture improvements in the base model. The upgrade does not affect existing custom models based on the model.
-   The service now lets you specify the endianness of audio submitted in `audio/l16` (Linear 16-bit Pulse-Code Modulation (PCM)) format. In addition to specifying `rate` and `channels` parameters with the format, you can now also specify `big-endian` or `little-endian` with the `endianness` parameter. For more information, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).

### 14 July 2017
{: #July2017b}

-   The service now supports the transcription of audio in the MP3 or Motion Picture Experts Group (MPEG) format. For more information about supported audio formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   The language model customization interface now supports Spanish as beta functionality. You can create a custom model based on either of the base Spanish language models: `es-ES_BroadbandModel` or `es-ES_NarrowbandModel`; for more information, see [Creating a custom language model](/docs/services/speech-to-text/language-create.html). Pricing for recognition requests that use Spanish custom language models is the same as for requests that use US English and Japanese models.
-   The JSON `CreateLanguageModel` object that you pass to the `POST /v1/customizations` method to create a new custom language model now includes a `dialect` field. The field specifies the dialect of the language that is to be used with the custom model. By default, the dialect matches the language of the base model. The parameter is meaningful only for Spanish models, for which the service lets you create a custom model that is suited for speech in one of the following dialects:
    -   `es-ES` for Castilian Spanish (the default)
    -   `es-LA` for Latin-American Spanish
    -   `es-US` for North-American (Mexican) Spanish

    The `GET /v1/customizations` and `GET /v1/customizations/{customization_id}` methods of the customization interface include the dialect of a custom model in their output. For more information, see [Creating a custom language model](/docs/services/speech-to-text/language-create.html#createModel) and [Listing custom language models](/docs/services/speech-to-text/language-models.html#listModels).
-   The names of the language models `en-UK_BroadbandModel` and `en-UK_NarrowbandModel` have been deprecated. The models are now available with the names `en-GB_BroadbandModel` and `en-GB_NarrowbandModel`.

    The deprecated `en-UK_{model}` names will continue to function for backward compatibility, but they will no longer be returned among the list of available models returned by the `GET /v1/models` method. You can still query them directly with the `GET /v1/models/{model_id}` method.

### 1 July 2017
{: #July2017a}

-   The language model customization interface of the {{site.data.keyword.speechtotextshort}} service is now generally available (GA) for both of its supported languages, US English and Japanese. {{site.data.keyword.IBM_notm}} does not charge for creating, hosting, or managing custom language models. As described in the next bullet, {{site.data.keyword.IBM_notm}} now charges an additional $0.03 (USD) per minute of audio for recognition requests that use custom models.
-   {{site.data.keyword.IBM_notm}} has updated the pricing for the {{site.data.keyword.speechtotextshort}} service by
    -   Eliminating the add-on price for using {{site.data.keyword.speechtotextshort}} narrowband models
    -   Providing Graduated Tiered Pricing for high-volume customers
    -   Charging an additional $0.03 (USD) per minute of audio for recognition requests that use US English or Japanese custom language models

    For more information about the pricing updates, see the following:
    -   The blog post [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   The [{{site.data.keyword.speechtotextshort}} service in the {{site.data.keyword.Bluemix_short}} Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/services/speech-to-text){: new_window}
    -   The [Pricing FAQs](/docs/services/speech-to-text/faq-pricing.html)
-   You no longer need to pass an empty data object as the body for the following `POST` requests:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    For example, you now call the `POST /v1/sessions` method with cURL as follows:

    ```bash
    curl -X POST -u "{username}":"{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    You no longer need to pass the following cURL option with the request: `--data "{}"`.

    > **Note:** If you experience any problems with one of these `POST` requests, try passing an empty data object with the body of the request. Passing an empty object does not change the nature or meaning of the request in any way.

### 22 May 2017
{: #May2017}

The following deprecations first announced in March 2017 are now in effect:

-   The `continuous` parameter has been removed from all methods that initiate recognition requests. The service now transcribes an entire audio stream until it ends or times out, whichever occurs first; this is equivalent to setting the former `continuous` parameter to `true`. By default, the service previously stopped transcription at the first half-second of non-speech (typically silence) if the parameter was omitted or set to `false`.

    Existing applications that set the parameter to `true` will see no change in behavior. Applications that set the parameter to `false` or that relied on the default behavior will likely see a change. If a request specifies the parameter, the service now responds as it does to any unknown parameter: by returning a warning message with its response:

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    The request succeeds despite the warning, and an existing session or WebSocket connection is not affected.

    {{site.data.keyword.IBM_notm}} made this change in response to overwhelming feedback from the developer community that specifying `continuous=false` added little value and could reduce overall transcription accuracy.
-   It is no longer possible to avoid a session timeout without sending audio:
    -   When using the WebSocket interface, the client can no longer keep a connection alive by sending a JSON text message with the `action` parameter set to `no-op`. Sending a `no-op` message does not generate an error, but it has no effect.
    -   When using sessions with the HTTP interface, the client can no longer extend the session by sending a `GET /v1/sessions/{session_id}/recognize` request. The method still returns the status of an active session, but it does not keep the session active.
    You can now do the following to keep a session alive:
    -   Set the `inactivity_timeout` parameter to `-1` to avoid the 30-second inactivity timeout.
    -   Send any audio data, including just silence, to the service to avoid the 30-second session timeout. You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

    For more information, see [Timeouts](/docs/services/speech-to-text/input.html#timeouts). Ideally, you would establish a session just before you obtain audio for transcription and maintain the session by sending audio at a rate that is close to real time. Your application should also recover gracefully from closed sessions or connections.

    {{site.data.keyword.IBM_notm}} made this change to ensure that it continues to offer all users a best-in-class, low-latency speech recognition service.

### 10 April 2017
{: #April2017}

-   The speaker labels feature is now supported for the following additional broadband models:
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    For more information, see [Speaker labels](/docs/services/speech-to-text/output.html#speaker_labels).
-   The service now supports the Web Media (WebM) audio format with the Opus or Vorbis codec. The service now also supports the Ogg audio format with the Vorbis codec in addition to the Opus codec. For more information about supported audio formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   The asynchronous HTTP interface now offers a `POST /v1/unregister_callback` method that allows you to remove the registration for a white-listed callback URL. For more information, see [Unregistering a callback URL](/docs/services/speech-to-text/async.html#unregister).
-   The WebSocket interface no longer times out when handling recognition requests for especially long audio files. You no longer need to request interim results with the JSON `start` message to avoid the timeout. (This issue was described in the release notes for March 10, 2016.)
-   The `DELETE /v1/customizations/{customization_id}` method now returns HTTP response code 401 if you attempt to delete a nonexistent custom model.
-   The `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` method now returns HTTP response code 400 if you attempt to delete a nonexistent corpus.

### 8 March 2017
{: #March2017}

The asynchronous HTTP interface is now generally available. Prior to this date, it was beta functionality.

### 1 December 2016
{: #December2016}

-   The service now offers a beta speaker labels feature for narrowband audio in US English, Spanish, or Japanese. The feature identifies which words were spoken by which speakers in a multi-person exchange. The sessionless, session-based, asynchronous, and WebSocket recognition methods each include a `speaker_labels` parameter that accepts a boolean value to indicate whether speaker labels are to be included in the response. For more information about the feature, see [Speaker labels](/docs/services/speech-to-text/output.html#speaker_labels).
-   The beta language model customization interface is now supported for Japanese in addition to US English. All methods of the interface support Japanese. For more information, see the following sections:
    -   For more information, see [Creating a custom language model](/docs/services/speech-to-text/language-create.html) and [Using a custom language model](/docs/services/speech-to-text/language-use.html).
    -   For general and Japanese-specific considerations when adding a corpus text file, see [Preparing a corpus file](/docs/services/speech-to-text/language-resource.html#prepareCorpus) and [What happens when you add a corpus file](/docs/services/speech-to-text/language-resource.html#parseCorpus).
    -   For Japanese-specific considerations when specifying the `sounds_like` field for a custom word, see [Guidelines for Japanese](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP).
    -   For detailed information about all methods of the customization interface, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   The language model customization interface now includes a `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` method that lists information about a specified corpus. The method is useful for monitoring the status of a request to add a corpus to a custom model. For more information, see [Listing corpora for a custom language model](/docs/services/speech-to-text/language-corpora.html#listCorpora).
-   The JSON output that is returned by the `GET /v1/customizations/{customization_id}/words` and `GET /v1/customizations/{customization_id}/words/{word_name}` methods now includes a `count` field for each word. The field indicates the number of times the word is found across all corpora. If you add a custom word to a model before it is added by any corpora, the count begins at `1`; if the word is added from a corpus first and later modified, the count reflects only the number of times it is found in corpora. For more information, see [Listing words from a custom language model](/docs/services/speech-to-text/language-words.html#listWords).

    For custom models created prior to the existence of the `count` field, the field always remains at `0`. To update the field for such models, add the model's corpora again and include the `allow_overwrite` parameter with the `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` method.
-   The `GET /v1/customizations/{customization_id}/words` method now includes a `sort` query parameter that lets you control the order in which the words are to be listed. The parameter accepts one of two arguments, `alphabetical` or `count`, to indicate how the words are to be sorted. You can prepend an optional `+` or `-` to an argument to indicate whether the results are to be sorted in ascending or descending order. By default, the method displays the words in ascending alphabetical order. For more information, see [Listing words from a custom language model](/docs/services/speech-to-text/language-words.html#listWords).

    For custom models created prior to the introduction of the `count` field, use of the `count` argument with the `sort` parameter is meaningless. Use the default `alphabetical` argument with such models.
-   The `error` field that can be returned as part of the JSON response from the `GET /v1/customizations/{customization_id}/words` and `GET /v1/customizations/{customization_id}/words/{word_name}` methods is now an array. If the service discovered one or more problems with a custom word's definition, the field lists each problem element from the definition and provides a message that describes the problem. For more information, see [Listing words from a custom language model](/docs/services/speech-to-text/language-words.html#listWords).
-   The `keywords_threshold` and `word_alternatives_threshold` parameters of the recognition methods no longer accept a null value. To omit keywords and word alternatives from the response, omit the parameters. A specified value must be a float.

For detailed documentation about the service's interface, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

### 22 September 2016
{: #September2016}

-   The service now offers a new beta language model customization interface for US English. The interface lets you tailor the service's base vocabulary and language models via the creation of custom language models that include domain-specific terminology. You can add custom words individually or have the service extract them from corpora. You can use your custom models with the speech recognition methods offered by any of the service's interfaces by passing the `customization_id` query parameter. For more information, see
    -   [Creating a custom language model](/docs/services/speech-to-text/language-create.html)
    -   [Using a custom language model](/docs/services/speech-to-text/language-use.html)
    -   [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}
-   The list of supported audio formats now includes `audio/mulaw`, which provides single-channel audio encoded using the u-law (or mu-law) data algorithm. When you use this format, you must also specify the sampling rate at which the audio is captured. For more information, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   The `GET /v1/models` and `GET /v1/models/{model_id}` methods now return an additional `supported_features` field as part of their output for each language model. The additional information describes whether the model supports customization. For more information, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

### 30 June 2016
{: #June2016b}

The beta asynchronous HTTP interface now provides full support for all languages supported by the service. The interface was previously available for US English only. For more information, see [The asynchronous HTTP interface](/docs/services/speech-to-text/async.html) and the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

### 23 June 2016
{: #June2016a}

-   A beta asynchronous HTTP interface is now available. The interface provides full recognition capabilities for US English transcription via non-blocking HTTP calls. You can register callback URLs and provide user-specified secret strings to achieve authentication and data integrity with digital signatures. For more information, see [The asynchronous HTTP interface](/docs/services/speech-to-text/async.html) and the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   A beta smart formatting feature that converts dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses into more conventional representations in final transcripts. The feature is enabled by setting the `smart_formatting` parameter to `true` on a recognition request. It is currently beta functionality that is available for US English only. For more information, see [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting).
-   The list of supported models for speech recognition now includes `fr-FR_BroadbandModel` for audio in the French language that is sampled at a minimum of 16 kHz. For more information, see [Languages and models](/docs/services/speech-to-text/input.html#models).
-   The list of supported audio formats now includes `audio/basic`, which provides single-channel audio encoded using 8-bit u-law (or mu-law) data sampled at 8 kHz. For more information, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   The various recognition methods can return a `warnings` response that includes messages about invalid query parameters or JSON fields included with a request. The format of the warnings has changed. For example, `"warnings": "Unknown arguments: [u'invalid_arg_1', u'invalid_arg_2']."` is now `"warnings": "Unknown arguments: invalid_arg_1, invalid_arg_2."`
-   For HTTP `POST` requests that do not otherwise pass data to the service, you must include an empty request body of the form `{}`. With cURL, you use the `--data` option to pass the empty data.

### 10 March 2016
{: #March2016}

-   Both forms of data transmission (one-shot delivery and streaming) now impose a size limit of 100 MB on the audio data, as does the WebSocket interface. Formerly, the one-shot approach had a maximum limit of 4MB of data. For more information, see [Audio transmission](/docs/services/speech-to-text/input.html#transmission) (for all interfaces) and [Send audio and receive recognition results](/docs/services/speech-to-text/websockets.html#WSaudio) (for the WebSocket interface). The WebSocket section also discusses the maximum frame or message size of 4 MB enforced by the WebSocket interface.
-   The JSON response returned by the `POST /v1/sessions/{session_id}/recognize` and `POST /v1/recognize` methods of the HTTP interface, as well as the `/v1/recognize` method of the WebSocket interface, can now include an array of warning messages for invalid query parameters or JSON fields included with a request. Each element of the array is a string that describes the nature of the warning followed by an array of invalid argument strings; for example, `"warnings": [ "Unknown arguments: [u'invalid_arg_1', u'invalid_arg_2']." ]`. For more information, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   The beta *{{site.data.keyword.watson}} Speech Software Development Kit (SDK) for the Apple&reg; iOS operating system* has been deprecated and replaced by the *{{site.data.keyword.watson}} Developer Cloud SDK for the Apple&reg; iOS operating system*. The new SDK is available from the [ios-sdk repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} in the `watson-developer-cloud` namespace on GitHub.

Note that the WebSocket interface has the following known issue at this time:

-   The service can take minutes to produce final results for a recognition request for an especially long audio file. For the WebSocket interface, the underlying TCP connection remains idle while the service is preparing the response and, as a result, can close due to a timeout. To avoid the timeout with the WebSocket interface, request interim results (`\"interim_results\": \"true\"`) in the JSON for the `start` message to initiate the request. You can discard the interim results if you do not need them. This issue will be resolved in a future update.

### 19 January 2016
{: #January2016}

The {{site.data.keyword.speechtotextshort}} service was updated to include a new profanity filtering feature on January 19, 2016. By default, the service censors profanity from its transcription results for US English audio. For more information, see [Profanity filtering](/docs/services/speech-to-text/output.html#profanity_filter).

### 17 December 2015
{: #December2015}

-   The service now offers a keyword spotting feature. The feature lets you specify an array of keyword strings to be matched in the input audio and a user-defined confidence level that a word must meet to be considered a match for a keyword. For more information, see [Keyword spotting](/docs/services/speech-to-text/output.html#keyword_spotting).

    > **Note:** The keyword spotting feature is beta functionality at this time.
-   The service now offers a word alternatives feature. The feature lets you obtain word alternative hypotheses for words in the input audio that meet a user-defined confidence level. For more information, see [Word alternatives](/docs/services/speech-to-text/output.html#word_alternatives).

    > **Note:** The word alternatives feature is beta functionality at this time.
-   The service supports additional languages with its transcription models: `en-UK_BroadbandModel` and `en-UK_NarrowbandModel` for UK English, and `ar-AR_BroadbandModel` for Modern Standard Arabic. For more information, see [Languages and models](/docs/services/speech-to-text/input.html#models).
-   HTTP recognition requests are no longer subject to a 10-minute platform timeout. The service now keeps the connection alive by sending a space character in the response JSON object every 20 seconds as long as recognition is ongoing. For more information, see [Timeouts](/docs/services/speech-to-text/input.html#timeouts).
-   The service no longer returns HTTP status code 490 for the session-based HTTP methods `GET /v1/sessions/{session_id}/observe_result` and `POST /v1/sessions/{session_id}/recognize`. The service now responds with HTTP status code 400 instead.

    In the JSON responses that it returns for errors with session-based methods, the service now also includes a new `session_closed` field. The field is set to `true` if the session is closed as a result of the error. For more information about possible return codes for any method, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.
-   When using cURL commands to transcribe audio with the service, you no longer need to use the `--limit-rate` option to transfer data at a rate no faster than 40,000 bytes per second.

### 21 September 2015
{: #September2015}

-   Two new beta mobile SDKs are available for the speech services. The SDKs enable mobile applications to interact with both the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} services.
    -   The *{{site.data.keyword.watson}} Speech SDK for the Google Android&trade; platform* provides support for streaming audio to the {{site.data.keyword.speechtotextshort}} service in real-time and receiving a transcript of the audio as you speak. The project includes an example application that showcases interaction with both of the speech services. The SDK is available from the [speech-android-sdk repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window} in the `watson-developer-cloud` namespace on GitHub.
    -   The *{{site.data.keyword.watson}} Speech SDK for the Apple&reg; iOS operating system* provides support for streaming audio to the {{site.data.keyword.speechtotextshort}} service and receiving a transcript of the audio in response. The SDK is available from the [speech-ios-sdk repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window} in the `watson-developer-cloud` namespace on GitHub.

    Both SDKs provide support for authenticating with the speech services by using either your {{site.data.keyword.Bluemix_short}} service credentials or an authentication token.

    > **Note:** Because the SDKs are beta at this time, they are subject to change in the future.
-   The service supports two new languages, Brazilian Portuguese and Mandarin Chinese. The models for these new languages are `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel`, and `zh-CN_NarrowbandModel`. For more information, see [Languages and models](/docs/services/speech-to-text/input.html#models).
-   The HTTP `POST` requests `/v1/sessions/{session_id}/recognize` and `/v1/recognize`, as well as the WebSocket `/v1/recognize` request, support transcription of an additional media type: `audio/ogg;codecs=opus` for Ogg format files that use the Opus codec. In addition, the `audio/wav` format for the methods now supports any encoding; the restriction regarding the use of linear PCM encoding has been removed. For more information, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   The service provides support for overcoming timeouts when you transcribe long audio files with the HTTP interface. When you use sessions, you can employ a long polling pattern by specifying sequence IDs with the `GET /v1/sessions/{session_id}/observe_result` and `POST /v1/sessions/{session_id}/recognize` methods for long-running recognition tasks. The new `sequence_id` parameter of these methods lets you request results for a recognition task before, during, or after you submit the recognition request. For more information, see [Transcribing long audio files with sessions](/docs/services/speech-to-text/http.html#HTTP-long).
-   For the US English language models, `en_US_BroadbandModel` and `en_US_NarrowbandModel`, the service now correctly capitalizes many proper nouns. For example, the service would new return text that reads "Barack Obama graduated from Columbia University" instead of "barack obama graduated from columbia university." This change might be of interest to you if your application is sensitive in any way to the case of proper nouns.
-   The HTTP `DELETE /v1/sessions/{session_id}` request does not return status code 415 "Unsupported Media Type." This return code has been removed from the documentation for the method.

### 1 July 2015
{: #July2015}

The {{site.data.keyword.speechtotextshort}} service moved from beta to general availability (GA) on July 1, 2015. The following differences existed between the beta and GA versions of the {{site.data.keyword.speechtotextshort}} APIs. The GA release required that users upgrade to the new version of the service.

-   The GA version of the HTTP API is backward-compatible with the beta version. You need to change your existing application code only if you explicitly specified a model name. For example, the sample code available for the {{site.data.keyword.speechtotextshort}} service from GitHub included the following line of code in the file `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    This line specified the default model, `WatsonModel`, for the beta version of the service. If your application also specified this model, you need to change it to use one of the new models supported with the GA version. See the next bullet for more information.
-   The service now supports a new programming model for direct interaction between a client and the service over a WebSocket. This model allows a client to obtain an authentication token for communicating directly with the service, bypassing the need for a server-side proxy application in {{site.data.keyword.Bluemix_notm}} to call the service on the client's behalf. This is the preferred means for clients to interact with the service.

    The service continues to support the old programming model that relied on a server-side proxy to relay audio and messages between the client and the service. But the new model is more efficient and provides higher throughput. For more information about the new programming model, see [Programming models for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-develop.html).
-   The `POST /v1/sessions` and `POST /v1/recognize` methods, along with the WebSocket `/v1/recognize` method, now support a `model` query parameter. You use this parameter to specify the language, currently *English*, *Japanese*, or *Spanish*, and the minimum sampling rate, *broadband* (16 kHz) or *narrowband* (8 kHz), of the audio data in a single specification. For more information, see [Languages and models](/docs/services/speech-to-text/input.html#models).
-   The `Content-Type` header of the `recognize` methods now supports `audio/wav` for Waveform Audio File Format (WAV) files, in addition to `audio/flac` and `audio/l16`. For more information about the supported audio formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   The `recognize` methods now support a number of additional query parameters that let you tailor the service to suit your application needs:
    -   The `inactivity_timeout` parameter sets the timeout value in seconds after which the service closes the connection if it detects silence (no speech) in streaming mode. By default, the service terminates the session after 30 seconds of silence. See [Timeouts](/docs/services/speech-to-text/input.html#timeouts).
    -   The `max_alternatives` parameter instructs the service to return the *n*-best alternative hypotheses for the audio transcription. See [Maximum alternatives](/docs/services/speech-to-text/output.html#max_alternatives).
    -   The `word_confidence` parameter instructs the service to return a confidence score for each word of the transcription. See [Word confidence](/docs/services/speech-to-text/output.html#word_confidence).
    -   The `timestamps` parameter instructs the service to return the beginning and ending time relative to the start of the audio for each word of the transcription. See [Word timestamps](/docs/services/speech-to-text/output.html#word_timestamps).
-   The `GET /v1/sessions/{session_id}/observeResult` method is now named `GET /v1/sessions/{session_id}/observe_result`. The name `observeResult` is still supported for backward compatibility.
-   The `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize`, and `POST /v1/recognize` methods now include the header parameter `X-WDC-PL-OPT-OUT` to control whether the service uses the audio and transcription data from a request to improve future results. The WebSocket interface includes an equivalent query parameter. Specify a value of `1` to prevent the service from using the audio and transcription results. The parameter applies only to the current request. The new header replaces the `X-logging` header from the beta API. See [Controlling request logging for {{site.data.keyword.watson}} services](../common/getting-started-logging.html).
-   The service now has a limit of 100 MB of data per session in streaming mode. You specify streaming mode by specifying the value `chunked` with the header `Transfer-Encoding`. One-shot delivery of an audio file still imposes a size limit of 4 MB on the data that is sent. See [Audio transmission](/docs/services/speech-to-text/input.html#transmission).
-   For the `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize`, and `/v1/recognize` methods, error code 415 ("Unsupported Media Type") has been added.
-   For `POST` and `GET` requests to the `/v1/sessions/{session_id}/recognize` method, the following error codes have been modified:
    -   Error code 404 ("Session_id not found") has a more descriptive message (`POST` and `GET`).
    -   Error code 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") has a more descriptive message (`POST` only).
-   For HTTP `POST` requests to the `/v1/sessions` and `/v1/recognize` methods, error code 503 ("Service Unavailable") can be returned. The error code can also be returned when creating a WebSocket connection with the `/v1/recognize` method.
