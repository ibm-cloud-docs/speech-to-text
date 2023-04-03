---

copyright:
  years: 2015, 2023
lastupdated: "2023-04-03"

subcollection: speech-to-text

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Pricing FAQs
{: #faq-pricing}

[IBM Cloud]{: tag-ibm-cloud}

The {{site.data.keyword.speechtotextfull}} service is available at three pricing plans: Lite, Plus, and Premium. The following FAQs provide an overview of the pricing plans. For more information, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/speech-to-text){: external}.

The Standard plan is no longer available for purchase by users. The Standard plan continues to be available to its existing users indefinitely. For more information, see [Can I continue to use the {{site.data.keyword.speechtotextshort}} Standard plan?](#faq-pricing-standard) For new users, read about the new Plus and Premium plans below.
{: note}

## What is the price for using the {{site.data.keyword.speechtotextshort}} Lite plan?
{: #faq-pricing-lite}
{: faq}

The Lite plan lets you get started with 500 minutes per month of speech recognition at no cost. You can use any available model for speech recognition. The Lite plan does not provide access to customization. You must use a paid plan to use customization.

The Lite plan is intended for any user who wants to try out the service before committing to a purchase. For more information, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/speech-to-text){: external}.
Services that are created with the Lite plan are deleted after 30 days of inactivity.

## What is the price for using the {{site.data.keyword.speechtotextshort}} Plus plan?
{: #faq-pricing-plus}
{: faq}

The Plus plan provides access to all of the service's features:

-   Use of all available models (same as the Lite plan).
-   Unlimited creation and use of custom language and custom acoustic models at no extra charge.
-   A maximum of one hundred concurrent transcription requests from all interfaces, WebSocket and HTTP, combined.

The plan uses a simple tiered pricing model to give high-volume users further discounts as they use the service more heavily. Pricing is based on the aggregate number of minutes of audio that you recognize per month:

-   Users who recognize 1 to 999,999 minutes of audio in a given month pay $0.02 (USD) per minute of audio for that month.
-   Users who recognize at least 1,000,000 minutes of audio in a given month pay $0.01 (USD) per minute of audio for that month.

The Plus plan is intended for small businesses. It is also a good choice for large enterprises that want to develop and test larger applications before considering moving to a Premium plan. For more information, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/speech-to-text){: external}.

## Can I continue to use the {{site.data.keyword.speechtotextshort}} Standard plan?
{: #faq-pricing-standard}
{: faq}

The Standard plan is no longer available for purchase by new users. But existing users of the Standard plan can continue to use the plan indefinitely with no change in their pricing. Their API settings and custom models remain unaffected.

Existing users can also choose to upgrade to the new Plus plan by visiting the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/speech-to-text){: external}. They will continue to have access to all of their settings and custom models after upgrading. And, if they find that the Plus plan does not meet their needs for any reason, they can always downgrade back to their Standard plan.

## What pricing plan do I need to use the service's customization interface?
{: #faq-pricing-customization}
{: faq}

You must have a paid plan (Plus, Standard, or Premium) to use language model or acoustic model customization. Users of the Lite plan cannot use customization. To use customization, users of the Lite plan must upgrade to a paid plan such as the Plus plan.

## How do I upgrade from the Lite plan to the Plus plan?
{: #faq-pricing-upgrade}
{: faq}

You can upgrade from the Lite plan to the Plus plan, for example, to gain access to customization. To upgrade from the Lite plan to the Plus plan, use the **Upgrade** button in the resource catalog page for your service instance:

-   From the [resource list](https://cloud.ibm.com/resources){: external}, click on your {{site.data.keyword.speechtotextshort}} instance to go to the {{site.data.keyword.speechtotextshort}} dashboard page.
-   From the **Manage** page, click **Upgrade** to move to the Plus plan.
-   Follow the steps on the **Plan** page to complete your upgrade.

## What does "pricing per minute" mean?
{: #faq-pricing-per-minute}
{: faq}
{: support}

For the Plus plan, pricing is based on the cumulative amount (number of minutes) of audio that you send to the service in any one month. The per-minute price of all audio that you recognize in a month is reduced once you reach the threshold of one million minutes of audio for that month. The price does not depend on how long the service takes to process the audio. (Per-minute pricing is different for the Standard plan.)

For information about pricing for the Plus and Standard plans, see the {{site.data.keyword.speechtotextshort}} service in the [{{site.data.keyword.cloud}} Catalog](https://{DomainName}/catalog/speech-to-text){: external}.

## Do you round up to the nearest minute for every call to the API?
{: #faq-pricing-rounding}
{: faq}

{{site.data.keyword.IBM_notm}} does not round up the length of the audio for every API call that the service receives. Instead, {{site.data.keyword.IBM_notm}} aggregates all usage for the month and rounds to the nearest minute at the end of the month. For example, if you send two audio files that are each 30 seconds long, {{site.data.keyword.IBM_notm}} sums the duration of the total audio for that month to one minute.

## Am I charged for all audio that I send, even audio that does not include speech?
{: #faq-pricing-audio}
{: faq}

Yes, all audio that you send to the service contributes to your cumulative minutes of audio. This includes silence and noisy audio that does not contain or otherwise contribute to speech recognition. Because the service must process all audio that it receives, it does not distinguish between the type or quality of audio that you send. For pricing purposes, three seconds of silence is equivalent to three seconds of actual speech.

## What advantages do I get by using a {{site.data.keyword.speechtotextshort}} Premium plan?
{: #faq-pricing-premium}
{: faq}

The Premium plan offers developers and organizations all of the capabilities and features of the Plus plan. The plan also offers these additional features:

-   The ability to use {{site.data.keyword.ibmwatson}} services in the {{site.data.keyword.cloud}} with data isolation and added security features such as {{site.data.keyword.keymanagementservicefull}} (also referred to as *Bring your own keys* (BYOK)), Service Endpoints, Mutual Authentication, and US Health Insurance Portability and Accountability Act (HIPAA) readiness.
-   Significantly greater capacity of up to 500 concurrent transcriptions with the option to add more, far above the limit of 100 concurrent transcriptions for the Plus plan.
-   Your first 150,000 minutes of speech recognition each month at no charge.

To learn more or to make a purchase, [contact an {{site.data.keyword.IBM_notm}} representative](https://ibm.biz/contact-wdc-premium){: external}.

## Can I provision a premium instance right now?
{: #faq-pricing-premium-provision}
{: faq}

No, you must first contact your IBM sales representative to purchase an entitlement and share your requirements. To make a purchase, [contact an {{site.data.keyword.IBM_notm}} representative](https://ibm.biz/contact-wdc-premium){: external}.
