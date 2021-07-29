---

copyright:
  years: 2015, 2020
lastupdated: "2020-07-29"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
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

# The science behind the service
{: #science}

As [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external} describes, {{site.data.keyword.IBM}} has been at the forefront of speech recognition research since the early 1960s. For example, [Bahl, Jelinek, and Mercer (1983)](/docs/speech-to-text?topic=speech-to-text-references#bahl1983) describes the basic mathematical approach to speech recognition that is employed in essentially all modern speech recognition systems. And [Jelinek (1985)](/docs/speech-to-text?topic=speech-to-text-references#jelinek1985) describes the creation of the first real-time large vocabulary speech recognition system for dictation. This paper also describes problems that are still unsolved research topics today.
{: shortdesc}

{{site.data.keyword.IBM_notm}} continues this rich tradition of research and development with the {{site.data.keyword.speechtotextfull}} service. {{site.data.keyword.IBM_notm}} has demonstrated industry-record speech recognition accuracy on the public benchmark data sets for Conversational Telephone Speech (CTS) ([Saon and others, 2017](/docs/speech-to-text?topic=speech-to-text-references#saon2017)) and Broadcast News (BN) transcription ([Thomas and others, 2019](/docs/speech-to-text?topic=speech-to-text-references#thomas2019)). {{site.data.keyword.IBM_notm}} leveraged neural networks for language modeling ([Kurata and others, 2017a](/docs/speech-to-text?topic=speech-to-text-references#kurata2017a), and [Kurata and others, 2017b](/docs/speech-to-text?topic=speech-to-text-references#kurata2017a)), in addition to demonstrating the effectiveness of acoustic modeling.

The following announcements summarize {{site.data.keyword.IBM_notm}}'s recent speech recognition accomplishments:

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}
-   [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://www.ibm.com/blogs/research/2020/07/speaker-diarization-in-real-use-cases/){: external}
-   [Advancing RNN Transducer Technology for Speech Recognition](https://arxiv.org/abs/2103.09935){: external}

These accomplishments contribute to further advance {{site.data.keyword.IBM_notm}}'s speech services. Recent ideas that best fit the cloud-based {{site.data.keyword.speechtotextshort}} service include

-   *For language modeling,* {{site.data.keyword.IBM_notm}} leverages a neural network-based language model to generate training text ([Suzuki and others, 2019](/docs/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *For acoustic modeling,* {{site.data.keyword.IBM_notm}} uses a fairly compact model to accommodate the resource limitations of the cloud. To train this compact model, {{site.data.keyword.IBM_notm}} uses "teacher-student training / knowledge distillation." Large and strong neural networks such as Long Short-Term Memory (LSTM), VGG, and the Residual Network (ResNet) are first trained. The output of these networks is then used as teacher signals to train a compact model for actual deployment ([Fukuda and others, 2017](/docs/speech-to-text?topic=speech-to-text-references#fukuda2017)).

To further push the envelope, {{site.data.keyword.IBM_notm}} also focuses on end-to-end modeling. For example, it has established a strong modeling pipeline for direct acoustic-to-word models ([Audhkhasi and others, 2017](/docs/speech-to-text?topic=speech-to-text-references#audhkhasi2017), and [Audhkhasi and others, 2018](/docs/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) that it is now further improving ([Saon and others, 2019](/docs/speech-to-text?topic=speech-to-text-references#saon2019)). It is also making efforts to create compact end-to-end models for future deployment on the cloud ([Kurata and Audhkhasi, 2019](/docs/speech-to-text?topic=speech-to-text-references#kurata2019)).

For more information about the scientific research behind the service, see the documents that are listed in [Research references](/docs/speech-to-text?topic=speech-to-text-references).
