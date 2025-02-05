---

copyright:
  years: 2015, 2025
lastupdated: "2025-02-05"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# The science behind the service
{: #science}

{{site.data.keyword.IBM}} has been at the forefront of speech recognition research since the early 1960s and continues this rich tradition of research and development with the {{site.data.keyword.speechtotextfull}} service.
{: shortdesc}

{{site.data.keyword.IBM_notm}} has demonstrated industry-record speech recognition accuracy on the public benchmark data sets for Conversational Telephone Speech (CTS) and Broadcast News (BN) transcription. {{site.data.keyword.IBM_notm}} leveraged neural networks for language modeling in addition to demonstrating the effectiveness of acoustic modeling.

The following announcements summarize {{site.data.keyword.IBM_notm}}'s recent speech recognition accomplishments:

-   [Reaching new records in speech recognition](https://www.ibm.com/products/speech-to-text){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://newsroom.ibm.com/){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://research.ibm.com/blog){: external}
-   [IBM Research AI Advances Speaker Diarization in Real Use Cases](https://research.ibm.com/blog){: external}
-   [Advancing RNN Transducer Technology for Speech Recognition](https://arxiv.org/abs/2103.09935){: external}

These accomplishments contribute to further advance {{site.data.keyword.IBM_notm}}'s speech services. Recent ideas that best fit the cloud-based {{site.data.keyword.speechtotextshort}} service include

-   *For language modeling,* {{site.data.keyword.IBM_notm}} leverages a neural network-based language model to generate training text.
-   *For acoustic modeling,* {{site.data.keyword.IBM_notm}} uses a fairly compact model to accommodate the resource limitations of the cloud. To train this compact model, {{site.data.keyword.IBM_notm}} uses "teacher-student training / knowledge distillation." Large and strong neural networks such as Long Short-Term Memory (LSTM), VGG, and the Residual Network (ResNet) are first trained. The output of these networks is then used as teacher signals to train a compact model for actual deployment.

To further push the envelope, {{site.data.keyword.IBM_notm}} also focuses on end-to-end modeling. For example, it has established a strong modeling pipeline for direct acoustic-to-word models that it is now further improving. It is also making efforts to create compact end-to-end models for future deployment on the cloud.
