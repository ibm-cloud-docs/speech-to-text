---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-04"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Overview for developers
{: #developerOverview}

You can access the capabilities of the {{site.data.keyword.speechtotextshort}} service by using a WebSocket interface and by using synchronous or asynchronous HTTP Representational State Transfer (REST) interfaces. You can also customize the service's language models to suit your domain and environment. Several Software Development Kits (SDKs) are available to simplify application development in various programming languages. The service returns all JSON response content in the UTF-8 character set.
{: shortdesc}

## Programming with the service
{: #programming}

[Making a recognition request](/docs/services/speech-to-text/basic-request.html) shows you how to request basic transcription with each of the service's programming interfaces:

-   [The WebSocket interface](/docs/services/speech-to-text/websockets.html) offers an efficient, low-latency, and high-throughput implementation over a full-duplex connection.
-   [The HTTP interface](/docs/services/speech-to-text/http.html) provides a basic interface to transcribe audio with synchronous requests.
-   [The asynchronous HTTP interface](/docs/services/speech-to-text/async.html) provides a non-blocking interface that lets you register a callback URL to receive notifications or to poll the service for job status and results.

The interfaces generally provide the same speech recognition capabilities, but you might specify the same parameter as a request header, a query parameter, or a parameter of a JSON object depending on the interface and method that you use.

-   For descriptions of all available speech recognition parameters, see the [Parameter summary](/docs/services/speech-to-text/summary.html).
-   For descriptions of all methods and their parameters, along with examples, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

## Advantages of the WebSocket interface
{: #advantages}

The WebSocket interface has a number of advantages over the HTTP interface:

-   The WebSocket interface provides a single-socket, full-duplex communication channel. The interface lets the client send requests and audio to the service and receive results over a single connection in an asynchronous fashion.
-   It provides a much simpler and more powerful programming experience. The service sends event-driven responses to the client's messages, eliminating the need for the client to poll the server.
-   It allows you to establish and use a single authenticated connection indefinitely. The HTTP interfaces require you to authenticate each call to the service.
-   It reduces latency. Recognition results arrive faster because the service sends them directly to the client. The HTTP interface requires four distinct requests and connections to achieve the same results.
-   It reduces network utilization. The WebSocket protocol is lightweight. It requires only a single connection to perform live-speech recognition.
-   It enables audio to be streamed directly from browsers (HTML5 WebSocket clients) to the service.

## Customizing the service
{: #custom}

[The customization interface](/docs/services/speech-to-text/custom.html) lets you create custom models to improve the service's speech recognition capabilities:

-   *Custom language models* let you define domain-specific words for a base model. Custom language models expand the service's base vocabulary with terminology specific to domains such as medicine and law.
-   *Custom acoustic models* let you adapt a base model for the acoustic characteristics of your environment and speakers. Custom acoustic models improve the service's ability to recognize speech for specific acoustic characteristics.

You can use a custom language model, a custom acoustic model, or both for speech recognition with any of the service's interfaces.

## CORS support
{: #cors}

The service supports Cross-Origin Resource Sharing (CORS). By using CORS, web pages can request resources directly from a foreign domain. CORS circumvents the same-origin security policy, which otherwise prevents such requests. Because the service supports CORS, a web page can communicate directly with the service without passing the request through the web server that hosts the page.

For instance, a web page that is loaded from a server in {{site.data.keyword.Bluemix_notm}} can call the customization API directly, bypassing the {{site.data.keyword.Bluemix_notm}} server. For more information, see [enable-cors.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://enable-cors.org/){: new_window}.

## Using Software Development Kits
{: #sdks}

SDKs are available for the {{site.data.keyword.speechtotextshort}} service to simplify the development of speech applications. {{site.data.keyword.watson}} SDKs are available for many popular programming languages and platforms.

-   For a complete list of SDKs and links to the SDKs on GitHub, see [Using SDKs](/docs/services/watson/getting-started-sdks.html).
-   For detailed information about all methods of the Node, Java, Python, and Ruby SDKs for the {{site.data.keyword.speechtotextshort}} service, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/){: new_window}.

## Learning more about application development
{: #learn}

For more information about working with {{site.data.keyword.watson}} Developer Cloud services and {{site.data.keyword.Bluemix_notm}}, see the following:

-   For an introduction to working with {{site.data.keyword.watson}} services and {{site.data.keyword.Bluemix_notm}}, see [Getting started with {{site.data.keyword.watson}} and {{site.data.keyword.Bluemix_notm}}](/docs/services/watson/index.html).
-   All new service instances use {{site.data.keyword.Bluemix}} Identity and Access Management (IAM) for authentication. Older service instances might continue to use the `{username}` and `{password}` from their existing Cloud Foundry service credentials for authentication. For more information about authenticating to the service, see the [30 October 2018 service update](/docs/services/speech-to-text/release-notes.html#October2018b) in the release notes.
-   For information about controlling the default request logging that is performed for all {{site.data.keyword.watson}} services, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-logging.html).

## Considerations for application development
{: #consider}

Converting speech to text is a difficult problem. Consider the following guidance when using the {{site.data.keyword.speechtotextshort}} service in your applications:

-   *Speech recognition can be very sensitive to input audio quality.* When you experiment with a demo application or build an application of your own that uses the service, try to ensure that the input audio quality is as good as possible. To obtain the best possible accuracy, use a close, speech-oriented microphone (such as a headset) whenever possible and adjust the microphone settings if necessary. Try to avoid using a laptop's built-in microphone.
-   *Choosing the correct model is important.* For most supported languages, the service supports two models: broadband and narrowband. {{site.data.keyword.IBM}} recommends that you use the broadband model for responsive, real-time applications and the narrowband model for offline decoding of telephone speech.
-   *Conversion of speech to text might not be perfect.* Tremendous progress has been made over the last several years. Today, speech recognition technology is successfully used in many domains and applications. However, in addition to audio quality, speech recognition systems are sensitive to nuances of human speech, such as regional accents and differences in pronunciation, and might not always transcribe audio input correctly.
