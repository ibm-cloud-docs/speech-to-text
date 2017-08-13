---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-13"

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

1.  <span style="color:#003F69">If I use the WebSocket API, do I need to use sessions?</span>

    No. When you use the WebSocket interface, the WebSocket is the connection to the service, and the connection is the session. A WebSocket connection provides all of the advantages of sessions and more. For example, it provides a single-socket, full-duplex communication channel; the client sends requests to the service and receives results over a single connection in an asynchronous fashion. In addition, the WebSocket protocol is very lightweight, which further reduces network latency. See [Using the WebSocket interface](/docs/services/speech-to-text/websockets.html).

1.  <span style="color:#003F69">My WebSocket connection times out before I receive the final hypothesis. What can I do?</span>

    Make sure that you signal the end of the audio by sending either a `stop` message or an empty binary message to the service. See [Ending a recognition request](/docs/services/speech-to-text/websockets.html#WSstop).
