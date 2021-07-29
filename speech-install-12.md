---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-16"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}

# Installing {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.speechtotextshort}} version 1.2
{: #speech-install-12}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

This and the following pages provide information about installing and managing {{site.data.keyword.speechtotextfull}} and {{site.data.keyword.texttospeechfull}} for {{site.data.keyword.icp4dfull}}. The two services share a common installation and common datastores to allow for more efficient resource consumption and simplified support. Most of the installation information therefore applies to both services.
{: shortdesc}

The following pages often refer to the combination of {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} as *Speech services.* Information specific to just one of the two services is clearly identified.

These instructions apply to versions 1.2 and 1.2.1 of the Speech services.
{: note}

## Where to begin
{: #speech-install-begin-12}

See the following topics in the IBM Documentation for the information that you need to get started with the {{site.data.keyword.watson}} Speech services.

-   *To install a new deployment of the service:*

    -   [Setting up the cluster for {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-install-adm.html){: external}
    -   [Creating an override file for {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} installation](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-override.html){: external}
    -   [Installing the {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-install.html){: external}

<!--
-   *To upgrade an existing deployment of the service:*

    -   [Preparing to upgrade {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-upgrade-adm.html){: external}
    -   [Upgrading {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-upgrade-svc.html){: external}
-->

-   *To uninstall the service:*

    -   [Uninstalling {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-speech/stt-svc-uninstall.html){: external}

## Where to continue
{: #speech-install-continue-12}

See the following pages for more information about installation and configuration parameters, and post-deployment management activities.

| Page | Contents |
|------|----------|
| [Using the override file](/docs/speech-to-text?topic=speech-to-text-speech-override-12) | Using values in the `speech-override.yaml` file to specify aspects of the installation and configuration that are common to both Speech services, including datastores, or are specific to {{site.data.keyword.speechtotextshort}} or {{site.data.keyword.texttospeechshort}}. |
| [Managing your cluster](/docs/speech-to-text?topic=speech-to-text-speech-cluster-12) | A collection of basic cluster and service management procedures and information. Topics include modifying the installed models and voices, managing user access, using common cluster management commands, setting Red Hat OpenShift SecurityContextConstraints, and disabling storage of user data. |
| [Managing your datastores](/docs/speech-to-text?topic=speech-to-text-speech-datastores-12) | Administering the MinIO and PostgreSQL datastores. For MinIO, topics include security secrets, which must be named `minio`, operation mode, and storage. For PostgreSQL, the page describes security secrets, which must be named `user-provided-postgressql`. |
| [Scaling up your installation](/docs/speech-to-text?topic=speech-to-text-speech-scaling-12) | Scaling from a development configuration to a production configuration. Topics include scaling up PostgreSQL, RabbitMQ, and MinIO. Topics also include scaling up other aspects of the installation and setting the session-to-CPU ratio for the {{site.data.keyword.speechtotextshort}} runtime and customization AM patcher, and for the {{site.data.keyword.texttospeechshort}} runtime. |
| [Backing up and restoring your data](/docs/speech-to-text?topic=speech-to-text-speech-backup-12) | Backup and restore procedures for both disaster recovery and upgrade of your data. The procedures cover the MinIO and PostgreSQL datastores. |
{: caption="Table 1. Documentation overview and links"}
