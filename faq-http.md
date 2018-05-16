---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-14"

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

# HTTP sessions
{: #sessions}

1.  <span style="color:#003F69">Why would I use sessions?</span>

    The use of sessions is encouraged to send multiple requests to the same server (transcription engine). One advantage of using sessions is reduced latency on subsequent calls because a new connection does not need to be initialized for each call. Sessions further reduce latency if you use custom models. Using the same server can also improve the accuracy of subsequent recognition requests. You can realize these same advantages by using the WebSocket interface.

1.  <span style="color:#003F69">How do I use cookies with sessions?</span>

    When you use session-based methods, the initial `POST /v1/sessions` request to create the session returns a cookie in the `Set-Cookie` response header. You must return this cookie with each call that uses that session. For more information, see [Using cookies with sessions](/docs/services/speech-to-text/http.html#cookies).

1.  <span style="color:#003F69">How do I diagnose request failures that are related to the use of cookies with sessions?</span>

    Two common errors that are associated with the use of cookies are forgetting to set the cookie on a session-based request and passing an invalid cookie.

    -   If you neglect to pass the cookie, the service returns HTTP status code 400 with the message `Cookie must be set.`
    -   If you pass an invalid cookie, the service returns HTTP status code 404 with the message `Session does not exist.`

    To help you diagnose the second type of error, [Using cookies with sessions](/docs/services/speech-to-text/http.html#cookies) describes the session cookie and how to ensure that it matches the session ID.
