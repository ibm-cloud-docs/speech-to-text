---

copyright:
  years: 2019, 2021
lastupdated: "2021-09-22"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Backing up and restoring your data
{: #speech-backup}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

To recover from potential disasters, you must back up and be prepared to restore your data from {{site.data.keyword.speechtotextfull}} and {{site.data.keyword.texttospeechfull}} for {{site.data.keyword.icp4dfull}}. You are responsible for understanding your data and for being prepared to restore it in the event of data loss.
{: shortdesc}

The Speech services rely on two datastores:

-   [MinIO](https://min.io/){: external} is an object storage solution that contains binary data for the Speech services.
-   [PostgreSQL](https://www.postgresql.org/){: external} is an open-source relational database that contains structured data for the Speech services.

The following sections describe what data to back up, how to back it up, and how to restore it for both of these datastores.

## What to back up
{: #speech-backup-what}

You need to back up your data only if you use one or more of the `sttCustomization`, `ttsCustomization`, and `sttAync` installation components. These microservices are stateful. If you do not install or use any of these components, you do not need to create backups. If you install and use only some of the components, back up only the components you use. Otherwise, the datastores respond with errors about nonexistent databases during the procedures.
{: important}

Your disaster recovery plan includes knowing, preserving, and being prepared to restore your data. For the {{site.data.keyword.speechtotextshort}} service, you need to back up data from the following interfaces:

-   *Language model customization* - Data from custom language models and grammars that you create with the `/v1/customizations` interface. For more information, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate) and [Using Grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars).
-   *Acoustic model customization* - Data from custom acoustic models that you create with the `/v1/acoustic_customizations` interface. For more information, see [Creating a custom acoustic model](/docs/speech-to-text?topic=speech-to-text-acoustic).
-   *Asynchronous HTTP* - Data for *allowlisted* URLs and speech recognition jobs that you create with the `/v1/recognitions` interface. For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async).

For the {{site.data.keyword.texttospeechshort}} service, you need to back up data from the following interface:

-   *Customization* - Data from custom models that you create with the `/v1/customizations` interface. For more information, see [Understanding customization](/docs/text-to-speech?topic=text-to-speech-customIntro).

The frequency of the backups depends on factors like the following:

-   How often you modify your {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} custom models and data, and how easy it is to re-create changes that might be lost.
-   How heavily you use the {{site.data.keyword.speechtotextshort}} asynchronous HTTP interface, and how severely the loss of queued jobs and the results of past jobs might impact your applications.

## MinIO backup and restore
{: #speech-backup-minio}

You use the MinIO client tool, `mc`, to connect to MinIO and back up and restore your data. For more information, see the [MinIO Client Complete Guide](https://docs.min.io/docs/minio-client-complete-guide.html){: external}.

You need the access key and secret key that you defined when you installed and configured the Speech services and MinIO. You can find the access and secret in the `minio` Kubernetes secret object within the cluster. The name appears as `minio` in the example override file. For more information, see [Managing your datastores](/docs/speech-to-text?topic=speech-to-text-speech-datastores).

### Backing up data from MinIO
{: #speech-backup-minio-backup}

Use these steps to back up your data from MinIO.

1.  Create a directory in which to store the MinIO data. Use a dedicated directory for the backup files, ensure that you have read and write permissions on the directory, and restrict other users' access to the directory. The following steps refer to the directory as `{minio_backup_directory}`.

    It is a best practice to create the backup directory on a mounted volume or file system that resides on a different host from the datastore. Alternatively, you can copy the finished backup files to a safe collection.
    {: note}

1.  Configure the MinIO client by running the `mc` command from within your cluster:

    ```bash
    mc --insecure -C {minio_backup_directory} config host add {alias} \
    https://{release_name}-ibm-minio-svc:9000 {minio_access_key} {minio_secret_key}
    ```
    {: pre}

    where

    -   `{minio_backup_directory}` is the name of the local directory in which you want to store the MinIO client configuration.
    -   `{alias}` is an alias that refers to the target MinIO host; for example, `icp-minio`.
    -   `{release_name}` is the name of the installed release that you are backing up.
    -   `{minio_access_key}` is the access key to connect to MinIO.
    -   `{minio_secret_key}` is the secret key to connect to MinIO.

1.  Check the output of the previous command to ensure that the host is added successfully. Run the following command to verify that the MinIO client is correctly configured by listing the MinIO buckets.

    ```bash
    mc --insecure -C {minio_backup_directory} ls {alias}
    ```
    {: pre}

    where `{minio_backup_directory}` is the local directory that you created in the first step. The command succeeded if it returns the following buckets. This example output assumes that you installed both the `sttCustomization` and `sttAsync` components.

    ```text
    speech-service-base-models/
    stt-async-icp/
    stt-customization-icp/
    ```
    {: codeblock}

1.  Create a subdirectory of the `{minio_backup_directory}` in which to dump the data; for example, `{minio-backup-directory}/{minio_data_dump}`.

1.  Perform the actual MinIO backup by using the directory names and alias from the previous steps. For example, the following command backs up the data for the `stt-customization-icp` component. The command copies the entire `stt-customization-icp` bucket to the target directory.

    ```bash
    mc --insecure -C {minio_backup_directory} \
    cp -r {alias}/stt-customization-icp {minio_backup_directory}/{minio_data_dump}
    ```
    {: pre}

    You can use a Kubernetes job that calls the `mc` command to dump the data into a directory where a Kubernetes persistent volume (for example, a Portworx persistent volume) is mounted.

1.  Verify that the output of the previous command has no errors, and make sure that the backed-up data is an exact copy of the original.

### Restoring data to MinIO
{: #speech-backup-minio-restore}

Use these steps to restore your data to MinIO.

1.  Configure the MinIO client by running the `mc` command to connect to the target MinIO installation to which the data is to be restored. The target installation is typically a different datastore from the backup.

    ```bash
    mc --insecure -C {minio_backup_directory} config host add {target_alias} \
    https://{release_name}-ibm-minio-svc:9000 {minio_access_key} {minio_secret_key}
    ```
    {: pre}

    where

    -   `{minio_backup_directory}` is the local directory to which you backed up the MinIO client configuration.
    -   `{target_alias}` is the alias that refers to the target MinIO host for the installation; for example, `icp-minio-restore`.
    -   `{release_name}` is the name of the installed release that you are restoring.
    -   `{minio_access_key}` is the access key to connect to MinIO.
    -   `{minio_secret_key}` is the secret key to connect to MinIO.

1.  Once the MinIO client is successfully configured, use the following command to create the `stt-customization` bucket in the target MinIO datastore (`{target_alias}`):

    ```bash
    mc --insecure -C {minio_backup_directory} mb {target_alias}/stt-customization-icp
    ```
    {: pre}

    The expected response is `Bucket created successfully {target_alias}/stt-customization-icp`.

1.  Copy the backed-up data into the bucket by running the following command:

    ```bash
    mc --insecure -C {minio_backup_directory} \
    cp -r {minio_backup_directory}/{minio_data_dump}/stt-customization-icp/customizations \
    {target_alias}/stt-customization-icp
    ```
    {: pre}

1.  Repeat the previous steps as needed to restore the `speech-service-base-models` and `stt-async-icp` components. If you use other components, repeat the steps to restore those components, as well.

## PostgreSQL backup and restore
{: #speech-backup-postgresql}

To dump backup data from PostgreSQL, you use the `pg_dump` tool. For more information, see the [PostgreSQL `pg_dump` documentation](https://www.postgresql.org/docs/9.6/backup-dump.html){: external}.

To restore data to PostgreSQL, you use the `psql` tool. This utility is a terminal-based front-end that connects to the database to perform interactive queries and to restore data to the database from backup files. For more information, see the [PostgreSQL `psql` documentation](https://www.postgresql.org/docs/9.6/app-psql.html){: external}.

You need the user name that you defined when you installed and configured the Speech services and PostgreSQL. You can find the name in the `postgresql` Kubernetes secret object within the cluster. The name appears as `user-provided-postgressql` in the example override file. For more information, see [Managing your datastores](/docs/speech-to-text?topic=speech-to-text-speech-datastores).

### Backing up data from PostgreSQL
{: #speech-backup-postgresql-backup}

Use these steps to back up your data from PostgreSQL.

1.  Create a directory in which to store the PostgreSQL data. Use a dedicated directory for the backup files, ensure that you have read and write permissions on the directory, and restrict other users' access to the directory. The following steps refer to the directory as `{postgres_backup_directory}`.

    It is a best practice to create the backup directory on a mounted volume or file system that resides on a different host from the datastore. Alternatively, you can copy the finished backup files to a safe collection.
    {: note}

1.  You need to back up three databases: `stt-customization`, `tts-customization`, and `stt-async`. Run the following command once per database.

    ```bash
    pg_dump -v -h {release_name}-ibm-postgresql-proxy-svc.{namespace} \
    -p 5432 -d {database_name} -U {username} \
    -f {postgres_backup_directory}/{database_name}.backup.sql
    ```
    {: pre}

    where

    -   `{release_name}` is the name of the installed release that you are backing up.
    -   `{namespace}` is the name of the namespace where IBM Cloud Pak for Data is installed (normally `zen`).
    -   `{database_name}` is the name of the database from which data is to be dumped (`stt-customization`, `tts-customization`, or `stt-async`).
    -   `{postgres_backup_directory}` is the name of the local directory in which you want to store the data.
    -   `{username}` is the name of the user for connecting to PostgreSQL (typically `stolon`).
1.  Repeat the previous step as needed to back up all of the components that you use.

### Restoring data to PostgreSQL
{: #speech-backup-postgresql-restore}

Use these steps to restore your data to PostgreSQL.

1.  Create the databases in the target PostgreSQL server to perform the data restore.

    ```bash
    psql -v -d postgres -U {username} \
    -h {release_name}-ibm-postgresql-proxy-svc.{namespace} -p 5432
    ```
    {: pre}

    where

    -   `{username}` is the name of the user for connecting to PostgreSQL (typically `stolon`).
    -   `{release_name}` is the name of the installed release that you are restoring.
    -   `{namespace}` is the name of the namespace where IBM Cloud Pak for Data is installed (normally `zen`).

1.  Run the following statements to create the necessary databases:

    ```bash
    postgres=# create database "stt-customization";
    CREATE DATABASE
    postgres=# create database "tts-customization";
    CREATE DATABASE
    postgres=# create database "tts-async";
    CREATE DATABASE
    ```

1.  Verify that the databases were created successfully by running the `\l` command:

    ```bash
    postgres=# \l
    ```

1.  To restore your data, run the following command once for each of the three databases (`stt-customization`, `tts-customization`, and `stt-async`):

    ```bash
    psql -v -d {database_name} -U {username} \
    -h {target_release_name}-ibm-postgresql-proxy-svc.{namespace} \
    -p 5432 -f {postgres_backup_directory}/{database_name}.backup.sql
    ```
    {: pre}

    where

    -   `{database_name}` is the name of the database whose data you are restoring (`stt-customization`, `tts-customization`, or `stt-async`).
    -   `{username}` is the name of the user for connecting to PostgreSQL (typically `stolon`).
    -   `{target_release_name}` is the name of the release to which you are restoring the data.
    -   `{namespace}` is the name of the namespace where IBM Cloud Pak for Data is installed (normally `zen`).
    -   `{postgres_backup_directory}` is the name of the local directory in which you stored the backup data.

1.  Verify the command's output to make sure the command ran successfully. Then run a few SQL statements against the databases to make sure they are restored correctly. The commands in the following examples succeed if their output lists the customizations that are available in the database.

    -    For the `stt-customization` database, run the following two commands:

         ```bash
         psql -v -d stt-customization -U stolon \
         -h {target_release-name}-ibm-postgresql-proxy-svc.{namespace} -p 5432
         ```
         {: pre}

         ```bash
         stt-customization=# SELECT count(*) FROM customizations;
         ```

    -   For the `tts-customization` database, run the following two commands:

        ```bash
        psql -v -d tts-customization -U stolon \
        -h {target_release_name}-ibm-postgresql-proxy-svc.{namespace} -p 5432
        ```
        {: pre}

        ```bash
        tts-customization=# SELECT count(*) FROM customizations;
        ```

1.  *If you are restoring the data to a different PostgreSQL instance (a different installation of the datastore)*, you need to change the ownership of some of the records within the databases. This is required because data ownership is based on service instance IDs. These IDs are specific to each installation of the Speech services.

    -   For the `stt-customization` and `tts-customization` databases, run the following command to transfer ownership to the `{target_instance_id}` from the `{source_instance_id}`:

        ```bash
        UPDATE customizations SET owner='{target_instance_id}'::uuid WHERE owner='{source_instance_id}'::uuid;
        ```

    -   For the `stt-async` database, run the following commands:

        ```bash
        UPDATE notification_urls SET username='{target_instance_id}' WHERE username='{source_instance_id}';
        UPDATE notification_urls SET instance_id='{target_instance_id}' WHERE instance_id='{source_instance_id}';
        UPDATE jobs SET username='{target_instance_id}' WHERE username='{source_instance_id}';
        ```
