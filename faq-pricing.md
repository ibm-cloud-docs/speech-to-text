---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-27"

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

# Pricing
{: #pricing}

1.  <span style="color:#003F69">What is the price for using the {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} standard service?</span>

    The {{site.data.keyword.speechtotextshort}} service is priced at $0.02 (USD) per minute. This price applies to use of both broadband and narrowband models. For more information, see the {{site.data.keyword.speechtotextshort}} [service landing page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}.

1.  <span style="color:#003F69">What does "pricing per minute" mean? Do you charge for the number of minutes of audio that I send to the service or the number of minutes that it takes the service to process the audio?</span>

    The price is based on the amount of audio that is sent to the service, not on the time that it takes the service to process the audio.

1.  <span style="color:#003F69">Do you round up to the nearest minute for every call to the API? For example, if I send two audio files that are each 30 seconds in length, am I charged for two minutes or one minute?</span>

    {{site.data.keyword.IBM_notm}} does not round up the length of the audio for every API call that the service receives. Instead, {{site.data.keyword.IBM_notm}} aggregates all usage for the month and rounds to the nearest minute at the end of the month. In this example, if you send two audio files that are each 30 seconds long, {{site.data.keyword.IBM_notm}} sums the duration of the total audio to one minute and charges $0.02 (USD).

1.  <span id="graduated" style="color:#003F69">How does volume-based Graduated Tiered Pricing work?</span>

    The tiered pricing model is intended to give high-volume users additional discounts as they continue to use the service. Per-minute pricing is reduced for additional minutes of audio once certain thresholds for total monthly audio are met. For more information, see the [pricing page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/services/speech-to-text){: new_window} for the service.

1.  <span style="color:#003F69">When using tiered pricing, what would my total charge be if I used the service to transcribe, for example, 275 thousand audio minutes in one month?</span>

    -   Every month, you receive 1000 free minutes of audio usage. So you would be charged only for 274 thousand minutes of audio: 275,000 - 1000 = 274,000.
    -   For the first 250-thousand minutes of audio, you would be charged at $0.02 (USD) / minute: 250,000 * $0.02 = $5000.00 (USD).
    -   For the remaining 24 thousand minutes of audio, you would be charged at the reduced rate of $0.015 (USD) / minute: 24,000 * $0.015 = $360.00 (USD).
    -   In this scenario, therefore, your total charge for the month would be $5360.00 (USD).

1.  <span style="color:#003F69">What is the price for using the service's customization interface? Am I charged for creating and storing a custom model?</span>

    For *language model customization*, {{site.data.keyword.IBM_notm}} does not charge for creating or hosting a custom language model, only for using the model with a recognition request. Using a custom language model for transcription incurs an add-on charge of $0.03 (USD) per minute. This is in addition to the standard usage charge of $0.02 (USD) per minute and applies to all languages supported by the customization interface. So the total charge for using a custom language model for speech recognition is $0.05 (USD) per minute.

    For *acoustic model customization*, which is currently a beta interface for all supported languages, {{site.data.keyword.IBM_notm}} does not currently charge for creating, hosting, or using a custom acoustic model. Free use of custom acoustic models for recognition requests is subject to change in the future.
