---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-17"

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

    Set the `smart_formatting` parameter to `true`. This feature converts dates, times, series of digits and numbers, phone numbers, currency values, and internet addresses into more conventional representations in the final transcript of a recognition request. See [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting). Smart formatting is available only for US English.

1.  <span style="color:#003F69">How can I add punctuation to a transcript? And why does the demo have punctuation but the service does not?</span>

    The service demo parses the transcript to add punctuation. The service substitutes punctuation symbols only for certain keyword strings in the spoken audio: comma, period, question mark, and exclamation point. For more information, see [Punctuation and capitalization](/docs/services/speech-to-text/output.html#smartFormattingPunctuation). You are responsible for capitalizing the first word of each sentence.
