---

copyright:
  years: 2021
lastupdated: "2021-04-02"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Data security
{: #data-security}

The {{site.data.keyword.speechtotextshort}} service provides the following security features to help you protect your user data.

## Basic data security
{: #data-security-basic}

The service provides security for all user data both in motion and at rest:

-   Transport Layer Security (TLS) 1.2 is used to secure data in transit.
-   Advanced Encryption Standard (AES)-256 with Secure Hash Algorithm (SHA)-256 is used to secure data at rest.

For more information about data security for cloud applications, see [Security architecture for cloud applications](https://www.ibm.com/cloud/architecture/architectures/securityArchitecture/security-for-data){: external}.

## Information security
{: #data-security-info-security}

The service supports the European Union General Data Protection Regulation (GDPR) to manage user data. You can pass the `X-Watson-Metadata` header with a request to associate a customer ID with data that the request passes to the service. If necessary, you can then delete the data by using the `DELETE /v1/user_data` method.

For Premium plans, the service also offers US Health Insurance Portability and Accountability Act (HIPAA) readiness. For more information about both features, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

## Service plans and encryption
{: #data-security-plan-encryption}

The service's Plus and Premium pricing plans offer different levels of data separation and encryption for users:

-   Plus plans are multi-tenant solutions that provide logical separation of data by using common encryption keys.
-   Premium plans are single-tenant solutions that provide physical separation of data. Premium plans provide dedicated data storage accounts that use unique encryption keys.

Users of Premium plans can also integrate with {{site.data.keyword.keymanagementservicefull}} to create, import, and manage their encryption keys. This process is commonly referred to as *Bring your own keys* (BYOK). For more information about using {{site.data.keyword.keymanagementservicefull}}, see [Protecting sensitive information in your Watson service](/docs/speech-to-text?topic=watson-keyservice).

## Network endpoints
{: #data-security-network-endpoints}

{{site.data.keyword.cloud}} supports both public and private network endpoints for certain plans. Connections to private network endpoints do not require public internet access. Private network endpoints support routing services over the {{site.data.keyword.cloud_notm}} private network instead of the public network. A private network endpoint provides a unique IP address that is accessible to you without a VPN connection.

Private network endpoints are supported for paid plans. Check the plan information for your service to learn about the plans that support private network endpoints. For more information, see [Public and private network endpoints](/docs/speech-to-text?topic=watson-public-private-endpoints).

## Request logging
{: #data-security-request-logging}

The service lets you control the default request logging that is performed for all {{site.data.keyword.watson}} services. The service logs request and response data only to improve the service for future users. The logged data is never shared or made public.

If you are concerned about the privacy of users' personal information or otherwise do not want your requests to be logged by {{site.data.keyword.IBM_notm}}, you can opt out of the default logging to prevent the service from logging your request and response data. If you opt out, the service logs *no* user data from your requests, saving no audio or text to disk. You can choose to opt out of logging at either the account level or the API request level. For more information, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/watson?topic=watson-gs-logging-overview).

## Asynchronous job retention
{: #data-security-async}

You use the `POST /v1/recognitions` method of the asynchronous HTTP interface to initiate an asynchronous speech recognition job. The service retains the results of each job until its time to live expires. The default time to live for a job is one week, though you can specify a shorter retention period.

You can also use the `DELETE /v1/recognitions/{id}` method to delete the results of a job. You typically delete a job after you obtain its results from the service. Once you delete a job, its results are no longer available. For more information, see [Deleting a job](/docs/speech-to-text?topic=speech-to-text-async#delete-async).

## CORS support
{: #data-security-cors}

The service supports Cross-Origin Resource Sharing (CORS). By using CORS, web pages can request resources directly from a foreign domain. CORS circumvents the same-origin security policy, which otherwise prevents such requests. Because the service supports CORS, a web page can communicate directly with the service without passing the request through the web server that hosts the page.

For instance, a web page that is loaded from a server in {{site.data.keyword.cloud}} can call the customization API directly, bypassing the {{site.data.keyword.cloud_notm}} server. For more information, see [enable-cors.org](https://enable-cors.org/){: external}.
