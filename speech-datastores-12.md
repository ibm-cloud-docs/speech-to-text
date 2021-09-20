---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-09"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Managing your datastores
{: #speech-datastores-12}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

The following sections provide information about administering the MinIO and PostgreSQL datastores.

## Configuring MinIO object storage
{: #speech-datastores-minio-12}

MinIO object storage is used to store persistent data that is needed by Speech services components. You can specify the security secrets, operational mode, and amount of storage.

### Secrets
{: #speech-datastores-minio-secrets-12}

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
kubectl create -f minio
```
{: pre}

The file must contain the following definition of the secrets object:

```yaml
apiVersion: v1
kind: Secret
type: Opaque
metadata:
  name: minio
data:
  accesskey: YWRtaW4=
  secretkey: YWRtaW4xMjM0
```
{: codeblock}

Then, during installation, set the value `global.datastores.minio.authSecretName` in the `speech-override.yaml` file to the name of the object you create. The following example sets the value to `minio`:

```yaml
global:
  datastores:
    minio:
      authSecretName: minio
```
{: codeblock}

### Mode of operation
{: #speech-datastores-minio-mode-12}

MinIO operates in distributed mode, which means that MinIO is scheduled to run multiple instances on every worker node to ensure high availability of storage. To use high availability optimally, you must specify an appropriate number of replicas. You can specify a number of replicas for a development configuration that is different from the number of replicas for a deployment configuration. You set both of these values in the `speech-override.yaml` file.

1.  To specify the deployment type for your configuration, set the value `global.datastores.minio.deploymentType` to either `Development` or `Production`.
2. To specify the number of replicas for each deployment type, set the following values. The number of replicas for both values must be no less than 4 and no more than 32 (`4 <= replicas <= 32`).

    -   `global.datastores.minio.replicasForDev`
    -   `global.datastores.minio.replicasForProd`

For example, specify the following values to use a development configuration with 4 replicas:

```yaml
  datastores:
    minio:
      deploymentType: Development
      replicasForDev: 4
```
{: codeblock}

### Storage size calculation guidelines
{: #speech-datastores-minio-calc-12}

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
{: #speech-datastores-postgre-credentials-12}

To access the PostgreSQL database, the installation reads credentials from a secrets file. By default, the installation creates an object that contains randomly generated passwords when you install the services. For security reasons, you need to change the automatically generated passwords when the deployment is complete.

You can also create credentials prior to installation. If you do, you need to set the values for the following attributes of the object. All of these values must be base64 encoded.

-   `data.PG_USER` is the name of the PostgreSQL user. You must use the value `enterprisedb`.
-   `data.PG_PASSWORD` is the password for the PostgreSQL user. Provide your own password. Do not use the default password.
-   `data.PG_REPLICATION_USER` is the name of the PostgreSQL replication user for PostgreSQL. You can use any name that you want (for example, `replication`).
-   `data.PG_REPLICATION_PASSWORD` is the password for the PostgreSQL replication user. Provide your own password. Do not use the default password.
-   `data.USING_SECRET` must be set to `true`.

The following example shows a secrets object with the necessary values base64 encoded. The `metadata.name` of the secrets object is set to `user-provided-postgressql`.

```yaml
apiVersion: v1
data:
  PG_PASSWORD: ZGxkQ001WHJNOU1sMTZobUo1Ym5qUEtGcUtzPQ==
  PG_REPLICATION_PASSWORD: eTRvRHBlMGIzNCt5dm50dVFtU1BsTVcyZkRzPQ==
  PG_REPLICATION_USER: cmVwbGljYXRpb24=
  PG_USER: ZW50ZXJwcmlzZWRi
  USING_SECRET: dHJ1ZQ==
kind: Secret
metadata:
  name: user-provided-postgressql
type: Opaque
```
{: codeblock}

To create a secrets object, run the following command to create a YAML file:

```bash
kubectl create -f user-provided-postgressql
```
{: pre}

Then, during installation, set the value `global.datastores.postgressql.authSecretName` in the `speech-override.yaml` file to the name of the object you create. The following example sets the value to `"user-provided-prosgressql"`:

```yaml
global:
  datastores:
    postgressql:
      authSecretName: "user-provided-prosgressql"
```
{: codeblock}
