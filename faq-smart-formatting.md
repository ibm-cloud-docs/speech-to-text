---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-121"

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

    Set the `smart_formatting` parameter to `true`. This feature converts the following strings into more conventional representations in the final transcript of a recognition request:

    -   Dates
    -   Times
    -   Series of digits and numbers
    -   Phone numbers
    -   Currency values
    -   Internet email and web addresses

    Smart formatting is available only for US English and Spanish. For more information, see [Smart formatting](/docs/services/speech-to-text/output.html#smart_formatting).

1.  <span style="color:#003F69">How can I add punctuation to a transcript? And why does the demo have punctuation but the service does not?</span>

    The service demo parses the transcript to add punctuation. With smart formatting, the service substitutes punctuation symbols only for certain keyword strings in US English transcripts: comma, period, question mark, and exclamation point. You are responsible for capitalizing the first word of each sentence.

For more information, see [Punctuation and capitalization](/docs/services/speech-to-text/output.html#smartFormattingPunctuation).
