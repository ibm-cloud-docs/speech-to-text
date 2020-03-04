---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-04"

subcollection: speech-to-text

---

{:faq: .faq}
{:support: data-reuse='support'}
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

# Pricing FAQs
{: #faq-pricing}

## What is the price for using the {{site.data.keyword.speechtotextshort}} Lite plan?
{: #faq-pricing-zero}
{: faq}
{: support}

The Lite plan lets you get started with 500 minutes per month of speech recognition at no cost. The Lite plan does not provide access to the service's customization interfaces. For more information, see the [pricing page](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} for the {{site.data.keyword.speechtotextshort}} service.

## What is the price for using the {{site.data.keyword.speechtotextshort}} Standard plan?
{: #faq-pricing-one}
{: faq}
{: support}

The Standard pricing plan is priced at $0.02 (USD) per minute of speech that you recognize. This price applies to use of both broadband and narrowband models. The plan uses a tiered pricing model, and it allows you to use the customization interfaces at an additional charge. For more information, see the [pricing page](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} for the {{site.data.keyword.speechtotextshort}} service.

## What does "pricing per minute" mean?
{: #faq-pricing-two}
{: faq}
{: support}

The price is based on the amount (number of minutes) of audio that you send to the service. The price does not depend on how long the service takes to process the audio.

## Do you round up to the nearest minute for every call to the API?
{: #faq-pricing-three}
{: faq}
{: support}

{{site.data.keyword.IBM_notm}} does not round up the length of the audio for every API call that the service receives. Instead, {{site.data.keyword.IBM_notm}} aggregates all usage for the month and rounds to the nearest minute at the end of the month. For example, if you send two audio files that are each 30 seconds long, {{site.data.keyword.IBM_notm}} sums the duration of the total audio to one minute and charges $0.02 (USD).

## How does volume-based Graduated Tiered Pricing work?
{: #faq-pricing-four}
{: faq}
{: support}

The tiered pricing model is intended to give high-volume users further discounts as they continue to use the service. Per-minute pricing is reduced for extra minutes of audio once certain thresholds for total monthly audio are met. For more information, see the [pricing page](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} for the service.

## For tiered pricing, what would my total charge be if I used the service to transcribe, for example, 275 thousand audio minutes in one month?
{: #faq-pricing-five}
{: faq}
{: support}

For the first 250 thousand minutes of audio, you would be charged at $0.02 (USD) / minute: 250,000 \* $0.02 = $5000.00 (USD). For the remaining 25 thousand minutes of audio, you would be charged at the reduced rate of $0.015 (USD) / minute: 25,000 \* $0.015 = $375.00 (USD). In this case, your total charge for the month would be $5375.00 (USD).

## What pricing plan do I need to use the service's customization interface?
{: #faq-pricing-six}
{: faq}
{: support}

You must have the Standard pricing plan to use language model or acoustic model customization. Users of the Lite plan cannot use the customization interface. For more information, see the [pricing page](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} for the {{site.data.keyword.speechtotextshort}} service.

## What is the price for using the service's customization interface?
{: #faq-pricing-seven}
{: faq}
{: support}

For *language model customization*, {{site.data.keyword.IBM_notm}} does not charge for creating or hosting a custom language model, only for using the model with a recognition request. Using a custom language model for transcription incurs an add-on charge of $0.03 (USD) per minute. This charge is in addition to the standard usage charge of $0.02 (USD) per minute. It applies to all languages supported by the customization interface. So the total charge for using a custom language model for speech recognition is $0.05 (USD) per minute.

For *acoustic model customization*, which is a beta interface for all supported languages, {{site.data.keyword.IBM_notm}} does not charge for creating, hosting, or recognizing speech with a custom acoustic model. Free use of custom acoustic models for recognition requests is subject to change in the future.
