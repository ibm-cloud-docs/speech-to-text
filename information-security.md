---

copyright:
  years: 2015, 2021
lastupdated: "2021-06-22"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Information security
{: #information-security}

{{site.data.keyword.IBM}} is committed to providing our clients and partners with innovative data privacy, security, and governance solutions.
{: shortdesc}

Clients are responsible for ensuring their own compliance with various laws and regulations, including the European Union General Data Protection Regulation. Clients are solely responsible for obtaining advice of competent legal counsel as to the identification and interpretation of any relevant laws and regulations that might affect the clients’ business and any actions the clients might need to take to comply with such laws and regulations.
{: important}

The products, services, and other capabilities described herein are not suitable for all client situations and might have restricted availability. {{site.data.keyword.IBM_notm}} does not provide legal, accounting or auditing advice or represent or warrant that its services or products will ensure that clients are in compliance with any law or regulation.

If you need to request GDPR support for {{site.data.keyword.cloud}} {{site.data.keyword.watson}} resources that are created

-   In the European Union (EU), see [Requesting support for {{site.data.keyword.cloud_notm}} Watson resources created in the European Union](/docs/watson?topic=watson-gdpr-sar#request-EU).
-   Outside of the EU, see [Requesting support for resources outside the European Union](/docs/watson?topic=watson-gdpr-sar#request-non-EU).

## European Union General Data Protection Regulation (GDPR)
{: #gdpr}

{{site.data.keyword.IBM_notm}} is committed to providing our clients and partners with innovative data privacy, security and governance solutions to assist them on their journey to GDPR compliance.

Learn more about {{site.data.keyword.IBM_notm}}'s own GDPR readiness journey and our GDPR capabilities and offerings to support your compliance journey [here](http://www.ibm.com/gdpr){: external}.

## Health Insurance Portability and Accountability Act (HIPAA)
{: #hipaa}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only**

US Health Insurance Portability and Accountability Act (HIPAA) support is available for Premium plans that are hosted in the Washington, DC, location and are created on or after 1 April 2019. For more information, see [Enabling EU and HIPAA supported settings](https://cloud.ibm.com/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

Do not include personal health information (PHI) in data that is to be added to custom models. Specifically, be sure to remove any PHI from data that you use for custom language models or custom acoustic models.

## Labeling and deleting data in the {{site.data.keyword.speechtotextshort}} service
{: #gdpr-speech-to-text}

The {{site.data.keyword.speechtotextfull}} service enables you to delete all data that is associated with recognition requests, custom language models, and custom acoustic models. To delete data, you must do the following:

1.  Use the `X-Watson-Metadata` header to associate a customer ID with data that is passed by a request to the service; see [Specifying a customer ID](#specify-customer-id).
1.  Use the `DELETE /v1/user_data` method to delete all data that is associated with a specified customer ID; see [Deleting data](#delete-pi).

By default, no customer ID is associated with data.

Experimental and beta features are not intended for use with a production environment and therefore are not guaranteed to function as expected when labeling and deleting data. Experimental and beta features should not be used when implementing a solution that requires the labeling and deletion of data.
{: note}

### Specifying a customer ID
{: #specify-customer-id}

To associate a customer ID with data, include the `X-Watson-Metadata` header with the request that passes the information. You pass the string `customer_id={id}` as the argument of the header.

A customer ID can include any characters except for the `;` (semicolon) and `=` (equals sign). Specify a random or generic string for the customer ID; do not specify a personally identifiable string, such as an email address or Twitter ID. You can specify different customer IDs with different requests. A customer ID that you specify is associated with the instance of the service whose credentials are used with the request; only credentials for that instance of the service can delete data associated with the ID.

#### Supported methods
{: #specify-customer-id-methods}

You can use the `X-Watson-Metadata` header with the following methods:

-   With WebSocket requests:
    -   `/v1/recognize`

    You specify the customer ID with the `x-watson-metadata` query parameter of the request to open the connection. You must URL-encode the argument to the query parameter, for example, `customer_id%3dmy_customer_ID`. The customer ID is associated with all data that is passed with recognition requests sent over the connection.
-   With synchronous HTTP requests:
    -   `POST /v1/recognize`

    The customer ID is associated with the data that is sent with the individual request.
-   With asynchronous HTTP requests:
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    The customer ID is associated with the allowlisted callback URL or with the data that is sent with the individual recognition request.
-   With requests to add corpora, custom words, or grammars to custom language models:
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    The customer ID is associated with the corpora, custom words, or grammars that are added or updated by the request.
-   With requests to add audio resources to custom acoustic models:
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    The customer ID is associated with the audio resource that is added or updated by the request.

#### Specify a customer ID example
{: #specify-customer-id-example}

The following example associates the customer ID `my_customer_ID` with the data passed with a `POST /v1/recognize` request:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X POST -u "apikey:{apikey}" \
--header "X-Watson-Metadata: customer_id=my_customer_ID" \
--header "Content-Type: audio/wav" \
--data-binary @audio.wav \
"{url}/v1/recognize"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "X-Watson-Metadata: customer_id=my_customer_ID" \
--header "Content-Type: audio/wav" \
--data-binary @audio.wav \
"{url}/v1/recognize"
```
{: pre}

### Deleting customer data
{: #delete-pi}

To delete all data that is associated with a customer ID, use the `DELETE /v1/user_data` method. You pass the string `customer_id={id}` as a query parameter with the request.

The `/v1/user_data` method deletes all data that is associated with the specified customer ID, regardless of the method by which the information was added. The method has no effect if no data is associated with the customer ID. You must issue the request with credentials for the same instance of the service that was used to associate the customer ID with the data.

#### Delete customer data example
{: #delete-pi-example}

The following example deletes all data for the customer ID `my_customer_ID`:

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}}**

```bash
curl -X DELETE -u "apikey:{apikey}" \
"{url}/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}}**

```bash
curl -X DELETE \
--header "Authorization: Bearer {token}" \
"{url}/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

## Deletion of all data for a {{site.data.keyword.speechtotextshort}} service instance
{: #gdpr-speech-to-text-instance}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only**

If you delete an instance of the {{site.data.keyword.speechtotextshort}} service from the {{site.data.keyword.cloud_notm}} console, all data associated with that service instance is automatically deleted. This includes all custom language models, corpora, grammars, and words; all custom acoustic models and audio resources; all registered endpoints for the asynchronous HTTP interface; and all data related to speech recognition requests.

This data is purged automatically and regardless of whether a customer ID is associated with the data. Once you delete a service instance, you can no longer restore any of the deleted data.
