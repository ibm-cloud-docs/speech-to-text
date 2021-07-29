---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-09"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:note: .note}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}

# Managing your datastores
{: #speech-datastores}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

The following sections provide information about administering the MinIO and PostgreSQL datastores.

## Configuring MinIO object storage
{: #speech-datastores-minio}

MinIO object storage is used to store persistent data that is needed by Speech services components. You can specify the security secrets, operational mode, and amount of storage.

### Secrets
{: #speech-datastores-minio-secrets}

Before you install {{site.data.keyword.speechtotextshort}} or {{site.data.keyword.texttospeechshort}}, you need to provide a secrets object that is used by MinIO itself and by other service components that interact with MinIO. This object contains the security keys to access the MinIO Object Server.

The secret must contain the items `accesskey` (5 - 20 characters) and `secretkey` (8 - 40 characters) in base64 encoding. Before you create the secrets, you need to perform the base64 encoding. The following commands encode the `accesskey` and `secretkey` in base64.

For security reasons, you are strongly encouraged to create an `accesskey` and a `secretkey` that are different from the sample keys (`admin` and `admin1234`) that are shown in the following examples.
{: important}

```bash
echo -n "admin" | base64
YWRtaW4=
```
{: pre}

```bash
echo -n "admin1234" | base64
YWRtaW4xMjM0
```
{: pre}

To create a secrets object, run the following command to create a YAML file:

```bash
kubectl create -f {secrets_file}
```
{: pre}

The file must contain the following definition of the secrets object:

```yaml
apiVersion: v1
kind: Secret
type: Opaque
metadata:
  name: minio # This name can be anything you choose.
data:
  accesskey: YWRtaW4=
  secretkey: YWRtaW4xMjM0
```
{: codeblock}

Then, during installation, set the following value in the `speech-override.yaml` file to the name of the object you create (for example, `minio`):

-   `global.datastores.minio.secretName`

### Mode of operation
{: #speech-datastores-minio-mode}

By default, MinIO operates in `distributed` mode, which means that MinIO is scheduled to run multiple instances on every worker node to ensure high availability of storage. To use high availability optimally, you must specify an appropriate number of replicas. Specify the number of replicas for distributed mode by setting the `external.minio.replicas={number_of_cluster_nodes}` value, where `{number_of_cluster_nodes}` is no less than 4 and no more than 32.

MinIO can also operate in `standalone` mode, which means that only one instance of MinIO runs on an arbitrary worker node. If that node fails, the services become unavailable until a new instance is running and healthy. This value is sufficient for development or testing purposes but not for production.

If you want to run MinIO in `standalone` mode, set the value `external.minio.mode=standalone`. In this case, you do not have to set the `external.minio.replicas` value.

### Storage size calculation guidelines
{: #speech-datastores-minio-calc}

Object storage is used for storing binary data from the following sources:

-   *Base models (for example, `en-US_NarrowbandModel`).* On average, base models are each 1.5 GB. Because models are updated regularly, you need to multiply the size of the models that you install by three to make room for at least that many concurrent versions of each model.
-   *Base voices (for example, `en-US_MichaelV3Voice`).* On average, base voices are each 300 MB. Only a single version of any voice is ever installed.
-   *Customization data (audio files and training snapshots).* The storage that is required for customization data depends on how many hours of audio you use to train your custom acoustic models. On average, one hour of audio data requires 0.5 GB of storage space. You can have multiple custom models, so you must factor in additional space per custom model.
-   *Audio files from asynchronous recognition jobs that need to be queued.* The storage that is required for asynchronous jobs depends on the use case. If you plan to submit large batches of audio files, expect the service to queue some jobs temporarily. In this case, the service temporarily holds some audio files in binary storage. The amount of storage that is required for this purpose does not exceed the size of the largest batch of jobs that you plan to submit in parallel.

A few examples of how to calculate storage size (in gigabytes) for {{site.data.keyword.speechtotextshort}} follow:

-   6 models, 3 versions, 50 hours audio = `(6 * (1.5 * 3)) + (50 * 0.5) = 52`
-   2 models, 3 versions, 20 hours audio = `(2 * (1.5 * 3)) + (20 * 0.5) = 19`

The default storage size, 100 GB, is a minimum starting point and is typically enough for operations with two to six models and about 50 hours of audio for the purpose of training custom acoustic models. But it is always better to be generous in anticipation of future storage needs.

## Setting access credentials for PostgreSQL
{: #speech-datastores-postgre-credentials}

To access the PostgreSQL database, the installation reads credentials from a secrets file. By default, the installation creates an object that contains randomly generated passwords when you install the services. For security reasons, you need to change the automatically generated passwords when the deployment is complete.

You can also create credentials prior to installation. If you do, you need to set the attribute `data.pg_su_password` to the PostgreSQL password that you want (base64 encoded). You also need to set the attribute `pg_repl_password`, which is the replication password and is also base64 encoded, to that value.

```yaml
apiVersion: v1
kind: Secret
type: Opaque
metadata:
  name: user-provided-postgressql # This name can be anything you choose.
data:
  pg_repl_password: cmVwbHVzZXI=
  pg_su_password: c3RvbG9u

```
{: codeblock}

To create a secrets object, run the following command to create a YAML file:

```bash
kubectl create -f {secrets_file}
```
{: pre}

Then, during installation, set the following two values in the `speech-override.yaml` file to the name of the object you create (for example, `user-provided-postgressql`):

-   `global.datastores.postgressql.auth.authSecretName`
-   `postgressql.auth.authSecretName`
