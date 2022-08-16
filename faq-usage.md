---

copyright:
  years: 2015, 2022
lastupdated: "2022-08-15"

keywords: faqs,frequently asked questions,question,Speech to Text

subcollection: speech-to-text

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Usage FAQs
{: #faq-usage}

FAQs for {{site.data.keyword.speechtotextfull}} include questions about speech recognition, audio transmission, customization, and other topics. To find all FAQs for {{site.data.keyword.cloud}}, see the [FAQ library](/docs/faqs){: external}.

## How do I access my service credentials?
{: #faq-credentials}
{: faq}
{: support}

How you access your service credentials depends on whether you are using {{site.data.keyword.speechtotextshort}} with {{site.data.keyword.cloud}} or {{site.data.keyword.icp4dfull}}. For more information about obtaining your credentials for both versions, see [Before you begin](/docs/speech-to-text?topic=speech-to-text-gettingStarted#getting-started-before-you-begin) in the getting started tutorial.

Once you have your service credentials, see the following topics for information about authenticating to the service:

-   [Authenticating to {{site.data.keyword.cloud_notm}}](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-authentication-cloud)
-   [Authenticating to {{site.data.keyword.icp4dfull_notm}}](/docs/speech-to-text?topic=speech-to-text-data-security#data-security-authentication-icpd)

## What languages does the service support?
{: #faq-language-support}
{: faq}
{: support}

The {{site.data.keyword.speechtotextshort}} service supports both previous-generation and next-generation languages and models. Most languages support both broadband/multimedia and narrowband/telephony models, which have minimum sampling rates of 16 kHz and 8 kHz, respectively. For more information about the available models and the features they support for all languages, see the following topics:

-   [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models)
-   [Next-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models-ng)

## What are the input audio formats?
{: #faq-audio-types}
{: faq}
{: support}

The service supports many audio formats (MIME types). Different formats support different sampling rates and other characteristics. By using a format that supports compression, you can maximize the amount of audio data that you can send with a request. For more information about the supported audio formats, see the following topics:

-   [Audio terminology and characteristics](/docs/speech-to-text?topic=speech-to-text-audio-terminology)
-   [Supported audio formats](/docs/speech-to-text?topic=speech-to-text-audio-formats)

## How much audio data can I submit to the service?
{: #faq-usage-two}
{: faq}
{: support}

The amount of audio that you can submit with a single speech recognition request depends on the interface that you are using:

-   The WebSocket and synchronous HTTP interfaces accept a maximum of 100 MB of audio data.
-   The asynchronous HTTP interface accepts a maximum of 1 GB of audio data.

For more information, see [Recognizing speech with the service](/docs/speech-to-text?topic=speech-to-text-service-features#features-recognition).

## Can I transcribe speech from video files?
{: #faq-usage-video}
{: faq}
{: support}

You cannot transcribe speech from a multimedia file that contains both audio and video. To transcribe speech from a video file, you must separate the audio data from the video data. For more information, see [Transcribing speech from video files](/docs/speech-to-text?topic=speech-to-text-audio-formats#audio-formats-video).

## How can I improve transcription accuracy?
{: #faq-usage-three}
{: faq}
{: support}

The {{site.data.keyword.speechtotextshort}} service offers a customization interface that provides many features and options to improve the speech recognition capabilities of the supported base language models:

-   If you are transcribing audio for a specific domain, you can create custom language models to expand and tailor a base model's vocabulary to include domain-specific terminology. When you use custom language models, you can also create and incorporate custom grammars to restrict the words that the service can recognize from your model's vocabulary. Language model customization is supported for both previous- and next-generation models. For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate) and [Adding a grammar to a custom language model](/docs/speech-to-text?topic=speech-to-text-grammarAdd).
-   If you are transcribing audio with unique characteristics (such as speaker accents, telephone conversations, or background noise), you can create a custom acoustic model to adapt a base model for your environment and speakers. Acoustic model customization is supported only for previous-generation models. For more information, see [Creating a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic).
-   You can also use custom acoustic and custom language models together. If transcriptions or related corpora are available for your audio, you can use that data to create a complementary custom language model to further improve the quality of speech recognition based on your custom acoustic model. Grammars are supported only for previous-generation models. For more information, see [Using custom acoustic and custom language models together](/docs/speech-to-text?topic=speech-to-text-useBoth).

## How many words can I add to a custom language model?
{: #faq-usage-four}
{: faq}
{: support}

You can add a maximum of 90 thousand out-of-vocabulary (OOV) words to a custom language model from all sources. You can add a maximum of 10 million total words to a custom language model from all sources. But many factors contribute to the amount of data that you need for an effective custom language model. Although it is not possible to provide the exact number of words that you need to add for any custom model or application, even adding a few words to a custom model can improve speech recognition. For more information about limits on the number of words that you can add and for other factors that affect the amount of data that you need, see [How much data do I need?](/docs/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount).

## How does custom model upgrading work?
{: #faq-usage-five}
{: faq}
{: support}

When a new version of a previous-generation base model is released to improve the quality of speech recognition, you must upgrade any custom language and custom acoustic models that are based on that model to take advantage of the updates. When you upgrade a custom model, you do not need to upgrade its resources individually. The service upgrades the resources automatically. Custom model upgrading applies only to previous-generation models.

-   For more information about upgrading a custom model, see [Upgrading custom models](/docs/speech-to-text?topic=speech-to-text-custom-upgrade).
-   For more information about using an upgraded custom model for a speech recognition request, see [Using upgraded custom models for speech recognition](/docs/speech-to-text?topic=speech-to-text-custom-upgrade-use).

## Can the {{site.data.keyword.speechtotextshort}} service transcribe numbers as digits instead of strings?
{: #faq-usage-six}
{: faq}
{: support}

For US English, Japanese, and Spanish audio, you can use smart formatting to convert certain strings, such as digits and numbers, to more conventional representations. Smart formatting is beta functionality. For more information, see [Smart formatting](/docs/speech-to-text?topic=speech-to-text-formatting#smart-formatting).
