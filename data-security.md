---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Data security
{: #data-security}

The {{site.data.keyword.speechtotextshort}} service provides the following security features to help you protect your user data.

## Authenticating to {{site.data.keyword.cloud_notm}}
{: #data-security-authentication-cloud}

[IBM Cloud]{: tag-ibm-cloud}

You authenticate to the service by using {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). You can pass either an API key or a bearer token in an `Authorization` header. For more information, see [Authenticating to {{site.data.keyword.cloud_notm}}](/docs/watson?topic=watson-iam#gs-credential-cloud).

## Authenticating to {{site.data.keyword.icp4dfull_notm}}
{: #data-security-authentication-icpd}

[IBM Cloud Pak for Data]{: tag-cp4d}

You authenticate to the service by passing an access token with each request. You pass a bearer token in an `Authorization` header to authenticate. Several methods exist to generate the token, including by using an API key or by username. For more information, see [Authenticating to {{site.data.keyword.icp4dfull_notm}}](/docs/watson?topic=watson-iam#gs-credential-cpd).

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} is a multi-tenant cloud solution. Your credentials provide access to your data only, and your data is isolated from other users.

## Basic data security
{: #data-security-basic}

The service provides security for all user data both in motion and at rest:

-   Transport Layer Security (TLS) 1.2 is used to secure data in transit.
-   Advanced Encryption Standard (AES)-256 with Secure Hash Algorithm (SHA)-256 is used to secure data at rest.

For more information about data security for cloud applications, see [Security architecture for cloud applications](https://www.ibm.com/topics/data-security){: external}.

## Information security
{: #data-security-info-security}

The service supports the European Union General Data Protection Regulation (GDPR) to manage user data. You can pass the `X-Watson-Metadata` header with a request to associate a customer ID with data that the request passes to the service. If necessary, you can then delete the data by using the `DELETE /v1/user_data` method. For more information about these features and their use, see [Information security](/docs/speech-to-text?topic=speech-to-text-information-security).

[IBM Cloud]{: tag-ibm-cloud} For Premium plans, the service also offers US Health Insurance Portability and Accountability Act (HIPAA) readiness in the Washington, DC, (`us-east`) and Dallas (`us-south`) regions.

## Request logging
{: #data-security-request-logging}

[IBM Cloud]{: tag-ibm-cloud}

The service lets you control the default request logging that is performed for all {{site.data.keyword.watson}} services. The service logs request and response data only to improve the service for future users. The logged data is never shared or made public. No data can be exported from the service. For example, users must retain the data that they use for training custom models because it cannot be retrieved from the service.

If you are concerned about the privacy of users' personal information or otherwise do not want your requests to be logged by {{site.data.keyword.IBM_notm}}, you can opt out of the default logging to prevent the service from logging your request and response data. If you opt out, the service logs *no* user data from your requests:

-   Results from synchronous HTTP and WebSocket requests are never stored to disk.
-   Results from asynchronous HTTP requests are deleted as soon as their time to live expires. For more information, see [Asynchronous job retention](#data-security-async).

You can choose to opt out of logging at either the account level or the API request level. For more information, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/watson?topic=watson-gs-logging-overview).

## Asynchronous job retention
{: #data-security-async}

You use the `POST /v1/recognitions` method of the asynchronous HTTP interface to initiate an asynchronous speech recognition job. The service retains the results of each job until its time to live expires. The default time to live for a job is one week, though you can specify a shorter retention period.

You can also use the `DELETE /v1/recognitions/{id}` method to delete the results of a job. You typically delete a job after you obtain its results from the service. Once you delete a job, its results are no longer available. For more information, see [Deleting a job](/docs/speech-to-text?topic=speech-to-text-async#delete-async).

## Data separation and encryption
{: #data-security-encryption}

[IBM Cloud]{: tag-ibm-cloud}

The service's Plus and Premium pricing plans offer different levels of data separation and encryption for users:

-   Plus plans are multi-tenant solutions that provide logical separation of data by using common encryption keys.
-   Premium plans are single-tenant solutions that provide physical separation of data. Premium plans provide dedicated data storage accounts that use unique encryption keys.

Users of Premium plans can also integrate with {{site.data.keyword.keymanagementservicefull}} to create, import, and manage their encryption keys. This process is commonly referred to as *Bring your own keys* (BYOK). For more information about using {{site.data.keyword.keymanagementservicefull}}, see [Protecting sensitive information in your Watson service](/docs/watson-knowledge-studio?topic=watson-keyservice).

## Network endpoints
{: #data-security-network-endpoints}

[IBM Cloud]{: tag-ibm-cloud}

{{site.data.keyword.cloud_notm}} supports both public and private network endpoints with certain plans. Connections to private network endpoints do not require public internet access. Private network endpoints support routing services over the {{site.data.keyword.cloud_notm}} private network instead of the public network. A private network endpoint provides a unique IP address that is accessible to you without a VPN connection.

Private network endpoints are supported only for paid plans. Check the plan information for your service to learn about the plans that support private network endpoints. For more information, see [Public and private network endpoints](/docs/watson?topic=watson-public-private-endpoints).

## Virtual private endpoints
{: #data-security-virtual-endpoints}

[IBM Cloud]{: tag-ibm-cloud}

{{site.data.keyword.cloud}} Virtual Private Endpoints for Virtual Private Cloud (VPC) are available with certain plans. Virtual private endpoints enable you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC. Virtual private endpoints are an evolution of the private connectivity to {{site.data.keyword.cloud_notm}} services. They are virtual IP interfaces that are bound to an endpoint gateway created on a per service or service instance basis.

Virtual private endpoints are supported only for paid plans. Check the plan information for your service to learn about the plans that support virtual private endpoints. For more information, see [Virtual Private Endpoints](/docs/watson?topic=watson-virtual-private-endpoints).

## CORS support
{: #data-security-cors}

The service supports Cross-Origin Resource Sharing (CORS). By using CORS, web pages can request resources directly from a foreign domain. CORS circumvents the same-origin security policy, which otherwise prevents such requests. Because the service supports CORS, a web page can communicate directly with the service without passing the request through the web server that hosts the page.

For instance, a web page that is loaded from a server in {{site.data.keyword.cloud_notm}} can call the customization API directly, bypassing the {{site.data.keyword.cloud_notm}} server. For more information, see [enable-cors.org](https://enable-cors.org/){: external}.

## Signed URLs
{: #data-security-signed-urls}

Signed URLs provide authentication information as a query string. Although signed URLs provide access for a limited time and with limited permissions, they allow any user with such a URL to access the service, regardless of whether that user has an account. The {{site.data.keyword.speechtotextshort}} service does not support signed URLs.

## FISMA support
{: #data-security-fisma}

[IBM Cloud Pak for Data]{: tag-cp4d}

Federal Information Security Management Act (FISMA) support is available for {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} offerings as of version 1.0.1. {{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} is FISMA High Ready.

## FIPS support
{: #data-security-fips}

[IBM Cloud Pak for Data]{: tag-cp4d}

{{site.data.keyword.speechtotextshort}} for {{site.data.keyword.icp4dfull_notm}} supports running on Federal Information Processing Standard (FIPS)-enabled clusters as of version 4.5.1.
