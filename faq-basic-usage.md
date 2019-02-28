---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Basic usage
{: #faq-basic}

1.  <span style="color:#003F69">I passed a one-hour long audio file to the service but did not see results until the full transcription completed. How can I see my results sooner?</span>

    Use the WebSocket interface, and set the `interim_results` parameter to `true`. By default, the service returns results only when it detects long silences in the audio. See [Interim results](/docs/services/speech-to-text/output.html#interim).

1.  <span style="color:#003F69">My audio includes pauses of varying lengths. How does the service respond to them?</span>

    The service transcribes an entire audio stream until either the stream ends or a timeout occurs, whichever comes first. If your input includes pauses, transcription results can include multiple `transcript` elements to indicate phrases that are separated by the pauses. Concatenate the `transcript` elements to assemble the complete transcription of the audio stream.

    All methods that initiate recognition requests previously included a `continuous` parameter that you could set to `true` to avoid having the service stop transcription at the first end of speech incident, typically silence. The service no longer stops transcription at pauses, so the parameter is removed from all methods.

1.  <span style="color:#003F69">Can I change the time or pause interval that determines when the end of a phrase occurs?</span>

    No. This capability is not currently available with the service.

1.  <span style="color:#003F69">My output includes multiple transcriptions of my content. Which one should I use?</span>

    Use the transcription for which the JSON `final` attribute is `true`. Ignore the intermediate results for which the attribute is `false`.
