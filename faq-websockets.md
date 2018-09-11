---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-21"

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

# WebSocket API
{: #websockets}

1.  <span style="color:#003F69">What are the advantages of using the WebSocket API?</span>

    The WebSocket interface provides a single-socket, full-duplex communication channel. The client sends requests to the service and receives results over a single connection in an asynchronous fashion. In addition, the WebSocket protocol is lightweight, which further reduces network latency. For more information, see [The WebSocket interface](/docs/services/speech-to-text/websockets.html).

1.  <span style="color:#003F69">My WebSocket connection times out before I receive the final hypothesis. What can I do?</span>

    Make sure that you signal the end of the audio by sending either a `stop` message or an empty binary message to the service. For more information, see [End a recognition request](/docs/services/speech-to-text/websockets.html#WSstop).
