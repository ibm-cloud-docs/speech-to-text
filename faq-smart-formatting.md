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

# Smart formatting
{: #smartFormatting}

1.  <span style="color:#003F69">How can I get numbers to appear as digits instead of being spelled out as words in a transcription?</span>

    Set the `smart_formatting` parameter to `true`. This feature converts dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses into more conventional representations in the final transcript of a recognition request. See [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting). Note that smart formatting is currently available only for US English.

1.  <span style="color:#003F69">How can I add punctuation to a transcript? And why does the demo have punctuation but the service does not?</span>

    The service does not indicate punctuation in a transcription. The `smart_formatting` parameter improves the default formatting of the transcription but does not provide punctuation. The most effective approach is to rely on the JSON attribute `final`; when the attribute is `true` to indicate the end of a phrase, add a period to the end of the phrase and capitalize the next word of the transcript. The service demo performs such parsing of the transcription.
