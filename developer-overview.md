---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-02"

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

# Overview for developers
{: #developerOverview}

You can access the capabilities of the {{site.data.keyword.speechtotextshort}} service via a WebSocket interface, an HTTP Representational State Transfer (REST) interface, or an asynchronous HTTP interface. You can also customize the service's language models to suit your domain and environment. Several Software Development Kits (SDKs) are also available to simplify application development in various languages and environments. The following sections provide an overview of application development with the service.
{: shortdesc}

## Programming with the service
{: #programming}

The {{site.data.keyword.speechtotextshort}} service offers three programming interfaces for transcribing speech to text:

-   [The WebSocket interface](/docs/services/speech-to-text/websockets.html) provides a single version of the `/v1/recognize` method for transcribing audio. The interface offers efficient implementation, low latency, and high throughput over a full-duplex connection.
-   [The HTTP REST interface](/docs/services/speech-to-text/http.html) provides HTTP `POST` versions of the `/v1/recognize` method that transcribe audio with or without establishing a session with the service. The methods let you send audio via the body of the request or as multipart form data that consists of one or more audio files. Additional methods of the interface let you establish and maintain sessions with the service and obtain information about supported languages and models.
-   [The asynchronous HTTP interface](/docs/services/speech-to-text/async.html) provides a non-blocking `POST /v1/recognitions` method for transcribing audio. Additional methods of the interface enable you to register a callback URL to which the service sends job status and optional results or to check the status of jobs and retrieve results manually. The interface uses HMAC-SHA1 signatures based on a user-specified secret to provide authentication and data integrity for callback notifications sent over the HTTP protocol.

While the various recognition methods share many common capabilities, you might specify the same parameter as a request header, a query parameter, or a parameter of a JSON object depending on the interface and method you are using. For more information about the service's features, see [Input features](/docs/services/speech-to-text/input.html) and [Output features](/docs/services/speech-to-text/output.html).

## Customizing the service
{: #custom}

The {{site.data.keyword.speechtotextshort}} service provides a customization interface that lets you create custom models for use in speech recognition requests:

-   *Custom language models.* The service's base vocabulary contains many words that are used in everyday conversation, but it can lack knowledge of terms that are associated with specific domains. The customization interface enables you to improve the accuracy of speech recognition for particular domains. You can create custom language models that expand the service's base vocabulary with terminology specific to domains such as medicine and law.
-   *Custom acoustic models.* The service was developed with a base acoustic model that works well with a variety of audio characteristics. But in some cases, adapting the base model for the acoustic characteristics of your environment and speakers can improve speech recognition. The customization interface allows you to provide example audio that matches the acoustic signature of the audio that you want to transcribe.

You can use a custom language model, a custom acoustic model, or both for speech recognition with any of the interfaces described in the previous section. For more information, see [The customization interface](/docs/services/speech-to-text/custom.html).

## Using Software Development Kits
{: #sdks}

The {{site.data.keyword.speechtotextshort}} service supports a number of SDKs to simplify the development of speech applications. The SDKs are available for many popular programming languages and platforms, including Node.js, Java, Python, and Apple&reg; iOS. For a complete list of SDKs and information about using them, see [{{site.data.keyword.watson}} SDKs](/docs/services/watson/getting-started-sdks.html). All SDKs are available from the [watson-developer-cloud namespace ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud){: new_window} on GitHub.

The following sample applications can help you get started:

-   You can access an application that uses the Node.js SDK at the [speech-to-text-nodejs repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/speech-to-text-nodejs){: new_window}.
-   You can access a Python client that interacts with the service through its WebSocket interface at the [speech-to-text-websockets-python repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/speech-to-text-websockets-python){: new_window}.

For mobile development, see the [{{site.data.keyword.watson}} Developer Cloud SDK for Apple&reg; iOS ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} and the [{{site.data.keyword.watson}} Speech SDK for the Google Android&trade; platform ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window}.

## Learning more about application development
{: #learn}

Like all {{site.data.keyword.watson}} services, the {{site.data.keyword.speechtotextshort}} service supports two typical programming models: *Direct interaction*, in which the client (for example, a web browser or an Android or iOS native app) streams audio to the service directly; and *relaying via a proxy*, in which the client and service exchange all data (requests, audio, and results) through a proxy application that resides in {{site.data.keyword.Bluemix_short}}. With the HTTP interface, you can use either programming model; with the WebSocket interface, you must use direct communication.

For more information about working with {{site.data.keyword.watson}} Developer Cloud services and {{site.data.keyword.Bluemix_notm}}, see the following:

-   For an introduction to working with {{site.data.keyword.watson}} services and {{site.data.keyword.Bluemix_notm}}, see [Getting started with {{site.data.keyword.watson}} and {{site.data.keyword.Bluemix_notm}}](/docs/services/watson/index.html).
-   For a language-independent introduction to developing {{site.data.keyword.watson}} services applications in {{site.data.keyword.Bluemix_notm}}, see [{{site.data.keyword.Bluemix_notm}} development approaches](/docs/services/watson/getting-started-bluemix.html).
-   For information about the two programming models available for developing {{site.data.keyword.watson}} applications, see [Programming models for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-develop.html):
    -   With relaying via a proxy, the client relies on a proxy server that resides in {{site.data.keyword.Bluemix_notm}} to communicate with the service; it passes all requests through the proxy application. Relaying requests via a proxy relies only on service credentials to authenticate with the service; see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html).
    -   With direct interaction, the client uses the proxy application in {{site.data.keyword.Bluemix_notm}} only to obtain an authentication token for the service, after which it communicates directly with the service. Direct interaction uses service credentials only to obtain a token; see [Tokens for authentication](/docs/services/watson/getting-started-tokens.html).
-   For information about controlling the default request logging that is performed for all {{site.data.keyword.watson}} services, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-logging.html).

## Considerations for application development
{: #consider}

Converting speech to text is a difficult problem. Some general things to consider when using the {{site.data.keyword.speechtotextshort}} service in your applications follow:

-   *Speech recognition can be very sensitive to input audio quality.* When you experiment with the demo application or build an application of your own that uses the service, please try to ensure that the input audio quality is as good as possible. To obtain the best possible accuracy, use a close, speech-oriented microphone (such as a headset) whenever possible and adjust the microphone settings if necessary. Try to avoid using a laptop's built-in microphone.
-   *Choosing the correct model is important.* For most supported languages, the service supports two models: broadband and narrowband. {{site.data.keyword.IBM}} recommends that you use the broadband model for responsive, real-time applications and the narrowband model for offline decoding of telephone speech. For more information about the models and the sampling rates they support, see [Languages and models](/docs/services/speech-to-text/input.html#models).
-   *Conversion of speech to text may not be perfect.* Tremendous progress has been made over the last several years. Today, speech recognition technology is successfully used in many domains and applications. However, in addition to audio quality, speech recognition systems are sensitive to nuances of human speech, such as regional accents and differences in pronunciation, and may not always successfully transcribe audio input.
