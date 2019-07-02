---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-02"

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

{{site.data.keyword.IBM}} pioneered speech recognition and audio generation research and systems in the early 1960s. For example, [Bahl, Jelinek, and Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) describes the basic mathematical approach to speech recognition that is employed in essentially all modern speech recognition systems. And [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) describes the creation of the first real-time large vocabulary speech recognition system for dictation. This paper also describes problems that are still unsolved research topics today.
{: shortdesc}

{{site.data.keyword.IBM_notm}} continues this rich tradition of research and development with the {{site.data.keyword.speechtotextfull}} service. Language and acoustic modeling represent two significant aspects of {{site.data.keyword.IBM_notm}}'s progress in this domain:

-   *For language modeling,* {{site.data.keyword.IBM_notm}} uses n-gram models. It leverages a neural network-based language model to generate training text ([Suzuki and others, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *For acoustic modeling,* {{site.data.keyword.IBM_notm}} uses a Convolutional Neural Network (CNN) that is fairly compact to accommodate the resource limitations of the cloud. For training this CNN, the service uses "teacher-student training" and "knowledge distillation." The network is first trained on large neural networks such as Long Short-Term Memory (LSTM), models of the Oxford Visual Geometry Group (VGG), and the Residual Network (ResNet). The output of these networks is used as teacher signals to train a compact CNN for actual deployment ([Fukuda and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)).

With these approaches, {{site.data.keyword.IBM_notm}} has demonstrated industry-record speech recognition accuracy on the public benchmark data sets for Conversational Telephone Speech (CTS) ([Saon and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) and Broadcast News (BN) transcription ([Thomas and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)). For the benchmarks, {{site.data.keyword.IBM_notm}} leveraged LSTM and CNN for language modeling ([Kurata and others, 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a), and [Kurata and others, 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)), in addition to demonstrating the effectiveness of acoustic modeling.

The following announcements summarize {{site.data.keyword.IBM_notm}}'s recent speech recognition accomplishments:

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

{{site.data.keyword.IBM_notm}} continues to focus on end-to-end modeling. For example, it has established a strong modeling pipeline for direct acoustic-to-word models ([Audhkhasi and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017), and [Audhkhasi and others, 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) that it is now further improving ([Saon and others, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)). It is also making efforts to create compact end-to-end models for future deployment on the cloud ([Kurata and Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)).

For more information about the scientific research behind the service, see the documents that are listed in [Research references](/docs/services/speech-to-text?topic=speech-to-text-references).
