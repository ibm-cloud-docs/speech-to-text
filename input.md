---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-25"

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

# Input features
{: #input}

The {{site.data.keyword.speechtotextshort}} service offers the following features to specify how the service is to perform the recognition request. All of the input parameters described on this page are optional; only the input audio and its format are required.
{: shortdesc}

-   For examples of simple requests and responses for each of the service's interfaces, see [Making a recognition request](/docs/services/speech-to-text/basic-request.html).
-   For information about the supported audio formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   For an alphabetized list of all available parameters, including their status (generally available or beta) and supported languages, see the [Parameter summary](/docs/services/speech-to-text/summary.html).

## Authentication tokens and request logging
{: #common}

The {{site.data.keyword.speechtotextshort}} service leverages the following two {{site.data.keyword.watson}} service-independent features:

-   *Authentication tokens* are an alternative to service credentials that allow you to make authenticated requests to {{site.data.keyword.watson}} services without embedding your service credentials in every call. You use your service credentials to obtain a token for the service and then call the service directly, without relying on an intermediate server-side application for handling communications to and from the service. Note that you must use authentication tokens when working with the WebSocket interface. For more information, see [Tokens for authentication](/docs/services/watson/getting-started-tokens.html).

-   *Request logging* is used by all {{site.data.keyword.watson}} services to log each request to a service and its results. When you agree (opt in) to have your data logged, {{site.data.keyword.IBM_notm}} reserves the right to store and use the data to improve the service's base language models. {{site.data.keyword.IBM_notm}} stores the data only to improve the service for future users; the logged data is never shared or made public. Once you opt in, {{site.data.keyword.IBM_notm}} offers no mechanism to delete the stored audio or transcripts.

    To prevent {{site.data.keyword.IBM_notm}} from accessing your data for general service improvements, set the `X-Watson-Learning-Opt-Out` request header to `true` for all requests. When you use the header to opt out of request logging, {{site.data.keyword.IBM_notm}} stores none of your data. Your data exists within {{site.data.keyword.watson}} only while it is in transit (in memory while the service processes your request).

    In either case, your data is always encrypted both in motion and at rest. For more information about opting out of request logging, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-logging.html).

## Languages and models
{: #models}

You can use the `model` parameter to specify a language model for the request. The model indicates the language in which the audio is spoken and the rate at which it is sampled. For most languages, the service supports two models:

-   A *broadband model* for audio that is sampled at greater than or equal to 16 kHz. {{site.data.keyword.IBM}} recommends that you use the broadband model for responsive, real-time applications (for example, for live-speech applications).
-   A *narrowband model* for audio that is sampled at 8 kHz. This rate is used typically for telephonic audio.

The service automatically adjusts the sampling rate of your audio to match the model that you specify; for more information, see [Sampling rate](/docs/services/speech-to-text/audio-formats.html#samplingRate).

The following table lists the supported models for each language. If you omit the model from a request, the service uses the US English broadband model, `en-US_BroadbandModel`, by default.

<table style="width:80%">
  <caption>Table 2. Supported language models</caption>
  <tr>
    <th style="text-align:left">Language</th>
    <th style="text-align:center">Broadband model</th>
    <th style="text-align:center">Narrowband model</th>
  </tr>
  <tr>
    <td>Brazilian Portuguese</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>French</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>Japanese</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Korean</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Mandarin Chinese</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Modern Standard Arabic</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">Not supported</td>
  </tr>
  <tr>
    <td>Spanish</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>UK English</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>US English</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></td>
  </tr>
</table>

To see the complete list of supported models, use the `GET /v1/models` method. To retrieve detailed information about a specific model, use the `GET /v1/models/{model_id}` method.

### Language model example
{: #modelsExample}

The following example request uses the model `en-US-NarrowbandModel`:

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Custom models
{: #custom}

> **Note:** Language and acoustic model customization are available at different levels of support (generally available or beta) for different languages. For more information, see [Language support for customization](/docs/services/speech-to-text/custom.html#languageSupport).

All interfaces allow you to pass a custom model to the service for use in a recognition request:

-   *Custom language models* let you expand the service's base vocabulary with terminology from specific domains. Use the `customization_id` parameter to include a custom language model with a request. You can also specify an optional `customization_weight` parameter to indicate the relative weight given to words from the custom model compared to those from the base vocabulary. For more information, see [Using a custom language model](/docs/services/speech-to-text/language-use.html).
-   *Custom acoustic models* let you adapt the service's base acoustic model for the acoustic characteristics of your environment and speakers. Use the `acoustic_customization_id` parameter to include a custom acoustic model with a request. You can specify both a custom language model and a custom acoustic model with a request. For more information, see [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html).

Custom models are based on one of the language models described in [Languages and models](#models) and can be used only with the base model for which they are created. If your custom model is based on a model other than the `en-US_BroadbandModel`, the default, you must also specify the name of the model with the request. To use a custom model, you must issue the request with service credentials created for the instance of the service that owns the custom model. For an introduction to customization, see [The customization interface](/docs/services/speech-to-text/custom.html).

### Custom model examples
{: #customExample}

The following example request includes the `customization_id` parameter to use the custom language model with the specified ID. It includes the `customization_weight` parameter to indicate that words from the custom model are to be given a relative weight of `0.5`.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

The following example request uses both a custom language model and a custom acoustic model. The former is identified with the `customization_id` parameter, the latter with the `acoustic_customization_id` parameter.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

For examples that use custom models with each of the service's interfaces, see [Using a custom language model](/docs/services/speech-to-text/language-use.html) and [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html).

## Base model version
{: #version}

To improve the quality of speech recognition, the service occasionally updates the base language models described in [Languages and models](#models). The base models for languages are independent of each other, as are the broadband and narrowband models for a given language. Updates to base models therefore occur independently of each other and have no effect on other models.

When multiple versions of a base model exist, the optional `version` parameter lets you specify the version of the model to be used with a recognition request. The parameter is intended primarily for use with custom models that have been updated for a new base model, but it can be used without a custom model. The version of a base model that is used for a request depends on whether you pass the `version` parameter and whether you specify a custom model (language, acoustic, or both) with the request:

-   *If you do not specify a custom model with the request:*

    -   Omit the `version` parameter to use the latest version of the base model.
    -   Specify the `version` parameter to use a specific version of the base model.

-   *If you specify a custom model with the request:*

    -   Omit the `version` parameter to use the latest version of the base model to which the custom model has been upgraded. For example, if the custom model has been upgraded to the latest version of the base model, the service uses the latest versions of the base and custom models.
    -   Specify the `version` parameter to use the specified versions of both the base and custom models.

Because the parameter is intended for use with custom models, you can learn about the available versions of a base model only by listing information about a custom model that is based on it. For complete information about upgrading custom models and about using different versions of base and custom models for speech recognition, see [Upgrading custom models](/docs/services/speech-to-text/custom-upgrade.html), especially [Making recognition requests with upgraded custom models](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition).

## Audio transmission
{: #transmission}

*With the WebSocket interface,* audio data is always streamed to the service over the connection. You can pass data through the socket all at once or, for the live-use case, as it becomes available.

*With the HTTP interface,* you can transmit audio to the service in one of two ways:

-   *One-shot delivery:* You omit the `Transfer-Encoding` header and pass all of the audio data to the service at one time as a single delivery.
-   *Streaming:* You set the `Transfer-Encoding` request header to the value `chunked` and stream the data over a persistent connection. The data does not need to exist fully before being streamed to the service; you can stream the data as it becomes available. You also need to set the header to `chunked` for a request that passes more than one audio file. For more information about the header, see [en.wikipedia.org/wiki/Chunked_transfer_encoding ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}.

With either interface, the service always transcribes the entire audio stream until it terminates. Transcription results can include multiple `transcript` elements to indicate phrases separated by pauses. Concatenate the `transcript` elements to assemble the complete transcription of the audio stream.

To preserve system resources when you stream audio data, the service enforces timeouts, terminating a request if the operation is started but the service receives no audio or if the service detects an extended period of silence in the audio. For information about the different timeouts associated with audio transmission, see [Timeouts](#timeouts).

### Audio transmission example
{: #transmissionExample}

The following example request specifies `chunked` for the `Transfer-Encoding` header to use streaming mode. The header is necessary because the request passes two audio files.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
--data-binary @{path}audio-file2.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Timeouts
{: #timeouts}

To preserve resources when you stream audio data, the service enforces various timeouts. If one of these timeouts lapses, the service closes the connection.

-   A *session timeout* (HTTP status code 408) occurs when a client starts a session but the service receives no audio for 30 seconds. It also occurs when a session is active but no request is received from the client for 30 seconds. The latter condition occurs only if the service receives no data from the client for 30 seconds and it has not yet received the last chunk of data. If the client has sent all data, the service can take more than 30 seconds to generate a response; in this case, the request does not time out.

    For both WebSocket connections and HTTP sessions, you can keep a session active by sending any audio data, including just silence, before the 30-second session timeout occurs. (You must also set the `inactivity_timeout` parameter to `-1`, as described in the next bullet.) You are charged for the duration of any data that you send to the service, including the silence that you send to extend a session.

    Ideally, you would establish a session just before you obtain audio for transcription and maintain it by sending audio at a rate that is close to real time. Your application should also recover gracefully from closed connections.
-   An *inactivity timeout* (HTTP status code 400) occurs when the service is receiving audio from the client but it detects silence (no speech) for 30 seconds. The service uses the inactivity timeout to ensure that a session remains active. The timeout is useful, for example, for terminating a session when a user simply walks away from a live microphone.

    You can override this timeout by specifying a different value for the `inactivity_timeout` parameter. Specify a value of `-1` to set the inactivity timeout to infinity.

To improve usability for long audio files, the service avoids HTTP REST inactivity timeouts by sending a space character every 20 seconds in the response JSON object to keep the connection alive as long as recognition is ongoing. The WebSocket interface is not subject to such platform timeouts.

### Inactivity timeout example
{: #timeoutsExample}

The following example request sets the inactivity timeout to 60 seconds for recognition of the specified files:

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
--data-binary @{path}audio-file2.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}
