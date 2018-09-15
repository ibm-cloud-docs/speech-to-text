---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-15"

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

The {{site.data.keyword.speechtotextshort}} service offers the following features to specify how the service is to perform the recognition request. All of the input parameters that are described in the following sections are optional. Only the input audio and its format are required.
{: shortdesc}

-   For more information about the supported audio formats, see [Audio formats](/docs/services/speech-to-text/audio-formats.html).
-   For examples of simple requests and responses for each of the service's interfaces, see [Making a recognition request](/docs/services/speech-to-text/basic-request.html).
-   For an alphabetized list of all available parameters, including their status (generally available or beta) and supported languages, see the [Parameter summary](/docs/services/speech-to-text/summary.html).

## Authentication tokens
{: #tokens}

*Authentication tokens* are an alternative to service credentials. By using tokens, you can make authenticated requests to {{site.data.keyword.watson}} services without embedding your service credentials in every call. You use your service credentials to obtain a token for the service. You then call the service directly, without relying on an intermediate server-side application for handling communications to and from the service. You must use authentication tokens with the WebSocket interface. For more information, see [Tokens for authentication](/docs/services/watson/getting-started-tokens.html).

## Request logging
{: #logging}

*Request logging* is used by all {{site.data.keyword.watson}} services to log each request to a service and its results. When you agree (opt in) to request logging, {{site.data.keyword.IBM_notm}} reserves the right to store and use the data to improve the service's base language models. {{site.data.keyword.IBM_notm}} accesses the data only to improve the service for future users. It never shares or makes public the logged data. Once you opt in, {{site.data.keyword.IBM_notm}} offers no mechanism to delete the stored audio or transcripts.

To prevent {{site.data.keyword.IBM_notm}} from accessing your data for general service improvements, set the `X-Watson-Learning-Opt-Out` request header to `true` for all requests. When you use the header to opt out of request logging, {{site.data.keyword.IBM_notm}} does not access your data for service improvements. For more information about opting out of request logging, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-logging.html).

## Information security
{: #security}

*Information security* includes features to associate a customer ID with data that is passed to the service with a request. You associate a customer ID with the data by passing the `X-Watson-Metadata` header with the request. If necessary, you can then delete the data by using the `DELETE /v1/user_data` method. For more information, see [Information security](/docs/services/speech-to-text/information-security.html).

## Languages and models
{: #models}

You can use the `model` parameter to specify a language model for the request. The model indicates the language in which the audio is spoken and the rate at which it is sampled. For most languages, the service supports two models:

-   A *broadband model* for audio that is sampled at greater than or equal to 16 kHz. {{site.data.keyword.IBM}} recommends that you use the broadband model for responsive, real-time applications (for example, for live-speech applications).
-   A *narrowband model* for audio that is sampled at 8 kHz. This rate is typically used for telephonic audio.

The service automatically adjusts the sampling rate of your audio to match the model that you specify. For more information, see [Sampling rate](/docs/services/speech-to-text/audio-formats.html#samplingRate).

### Available language models
{: #modelsList}

To list all available models for all languages, use the `GET /v1/models` method. To see detailed information about a specific model, use the `GET /v1/models/{model_id}` method.

The following table lists the supported models for each language. If you omit the `model` parameter from a request, the service uses the US English broadband model, `en-US_BroadbandModel`, by default.

<table style="width:80%">
  <caption>Table 1. Supported language models</caption>
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
    <td>German</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
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

All interfaces accept a custom model for use in a recognition request:

-   *Custom language models* expand the service's base vocabulary with terminology from specific domains. Use the `customization_id` parameter to include a custom language model with a request. You can also specify an optional `customization_weight` parameter. The parameter indicates the relative weight that is given to words from the custom model as opposed to words from the base vocabulary. For more information, see [Using a custom language model](/docs/services/speech-to-text/language-use.html).
-   *Custom acoustic models* adapt the service's base acoustic model for the acoustic characteristics of your environment and speakers. Use the `acoustic_customization_id` parameter to include a custom acoustic model with a request. You can specify both a custom language model and a custom acoustic model with a request. For more information, see [Using a custom acoustic model](/docs/services/speech-to-text/acoustic-use.html).

Custom models are based on one of the language models that are described in [Languages and models](#models). A custom model can be used only with the base model for which it is created. If your custom model is based on a model other than the `en-US_BroadbandModel`, the default, you must also specify the name of the model with the request. To use a custom model, you must issue the request with service credentials that are created for the instance of the service that owns the custom model. For an introduction to customization, see [The customization interface](/docs/services/speech-to-text/custom.html).

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

To improve the quality of speech recognition, the service occasionally updates the base language models described in [Languages and models](#models). The base models for languages are independent of each other, as are the broadband and narrowband models for a language. Therefore, updates to base models occur independently of each other and have no effect on other models.

When multiple versions of a base model exist, the optional `base_model_version` parameter specifies the version of the model to be used with a recognition request. The parameter is intended primarily for use with custom models that are updated for a new base model, but it can be used without a custom model. The version of a base model that is used for a request depends on whether you pass the `base_model_version` parameter. It also depends on whether you specify a custom model (language, acoustic, or both) with the request.

-   *If you do not specify a custom model with the request*

    -   Omit the `base_model_version` parameter to use the latest version of the base model.
    -   Specify the `base_model_version` parameter to use a specific version of the base model.

-   *If you specify a custom model with the request*

    -   Omit the `base_model_version` parameter to use the latest version of the base model to which the custom model is upgraded. For example, if the custom model is upgraded to the latest version of the base model, the service uses the latest versions of the base and custom models.
    -   Specify the `base_model_version` parameter to use the specified versions of both the base and custom models.

The parameter is intended for use with custom models. Therefore, you can learn about the available versions of a base model only by listing information about a custom model that is based on it. For more information about upgrading custom models and about using different versions of base and custom models for speech recognition, see [Upgrading custom models](/docs/services/speech-to-text/custom-upgrade.html). See especially [Making recognition requests with upgraded custom models](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition).

## Audio transmission
{: #transmission}

*With the WebSocket interface,* audio data is always streamed to the service over the connection. You can pass data through the socket all at once, or you can pass data for the live-use case as it becomes available. The service returns results as they become available.

*With the HTTP interfaces,* you can transmit audio to the service in either of the following ways:

-   *One-shot delivery* - You omit the `Transfer-Encoding` header and pass all of the audio data to the service at one time as a single delivery.
-   *Streaming* - You set the `Transfer-Encoding` request header to the value `chunked` and stream the data over a persistent connection. The data does not need to exist fully before you stream it to the service. You can stream the data as it becomes available. The service sends results only when it receives the final chunk, which you indicate by sending an empty chunk.

    For more information about streaming chunked audio with the `Transfer-Encoding` header, see
    -   [en.wikipedia.org/wiki/Chunked_transfer_encoding ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}
    -   [Transfer Codings ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc7230#section-4){: new_window} in *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*

With the HTTP interfaces, the service always transcribes the entire audio stream before sending any results. The results can include multiple `transcript` elements to indicate phrases that are separated by pauses. Concatenate the `transcript` elements to assemble the complete transcript.

To preserve system resources when you stream audio data, the service enforces timeouts. The service can terminate a request if

-   The request is initiated but the service receives no audio.
-   The service detects an extended period of silence in the audio.

For more information about timeouts and how to work around them, see [Timeouts](#timeouts).

### Audio transmission example
{: #transmissionExample}

The following example request specifies `chunked` for the `Transfer-Encoding` header to use streaming mode. The connection remains open to accept additional chunks of audio.

```bash
curl -X POST -u {username}:{password}
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Timeouts
{: #timeouts}

To preserve resources when you stream audio data, the service enforces timeouts. If a timeout lapses during a streaming session, the service closes the connection. Your application must recover gracefully from possible closed connections.

When you initiate a streaming session with the HTTP `/v1/recognize` or WebSocket `/v1/recognize` method, the service enforces the following timeouts:

-   A *session timeout* (HTTP status code 408) occurs when the client opens a streaming session but

    -   Sends no audio to the service for 30 seconds
    -   Stops sending audio for 30 seconds before it sends the last chunk to the service
    -   Streams audio at a rate that is much slower than real-time

    Ideally, you would initiate a request to establish a session just before you obtain audio for transcription and maintain it by sending audio at a rate that is close to real time. If the client has sent all data, the service can take more than 30 seconds to generate a response for long audio. In this case, the request does not time out.

    You can keep a session active by sending any audio data, including just silence, before the 30-second session timeout occurs. (You must also set the `inactivity_timeout` parameter to `-1`, as described in the next bullet.) You are charged for the duration of any audio data that you send to the service, including the silence that you send to extend a session.

-   An *inactivity timeout* (HTTP status code 400) occurs when the service is receiving audio from the client but it detects silence (no speech) for 30 seconds. The inactivity timeout is useful, for example, for terminating a session when a user simply walks away from a live microphone. The service uses the timeout to ensure that a session remains active.

    You can override this timeout by specifying a different value for the `inactivity_timeout` parameter. Specify a value of `-1` to set the inactivity timeout to infinity.

To improve usability for long audio, the service avoids HTTP REST inactivity timeouts by sending a space character every 20 seconds in the response JSON object until it completes the transcription. Sending the character keeps the connection alive while recognition is ongoing. (The WebSocket interface is not subject to this type of timeout.)

### Inactivity timeout example
{: #timeoutsExample}

The following example request sets the inactivity timeout to 60 seconds. The client sends an initial file to begin the streaming session.

```bash
curl -X POST -u {username}:{password}
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}
