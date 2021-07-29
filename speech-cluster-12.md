---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-09"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Managing your cluster
{: #speech-cluster-12}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

After you install and configure the Speech services on {{site.data.keyword.icp4dfull}}, you can manage the service instance and the cluster. The following sections document some basic cluster and service management operations.
{: shortdesc}

## Modifying the installed models and voices
{: #speech-cluster-models-voices-12}

You can add or remove installed models and voices from an existing installation. You must perform a `helm upgrade` operation to modify the installation. Complete the following steps to modify your installed models or voices.

1.  Log in to your Red Hat OpenShift cluster as a project administrator:

    ```bash
    oc login {OpenShift_URL}:{port}
    ```
    {: pre}

1.  Exec to the `cpd-install-operator` pod. The Helm chart is installed in this location.

    -   Get the name of the operator pod:

        ```bash
        oc get pods | grep cpd-install-operator
        ```
        {: pre}

    -   Exec to the named pod:

        ```bash
        oc exec -ti {operator_podname} -- /bin/bash
        ```
        {: pre}

1.  If you do not already know it, find the name of the Speech release. If you already know the release name, continue to the next step.

    ```bash
    helm list --tls
    ```
    {: pre}

1. Save the `values.yaml` file from the existing installation. This command copies the file to `/tmp/values.yaml`. In the command, replace `{speech_release_name}` with the name of your release.

    ```bash
    helm get values {speech_release_name} --tls > /tmp/values.yaml
    ```
    {: pre}

    Make a backup copy of this file in case you need to retry or correct the upgrade procedure.

1.  Edit the `/tmp/values.yaml` file. To modify the currently installed models or voices, locate the `sttModels` or `ttsVoices` section of the file. Set the `enabled` flag to `true` for any models or voices that you want to install. Similarly, set the flag to `false` for any models or voices that you want to remove from the installation. Removing unnecessary models and voices decreases the memory that your deployment uses. Save the file to preserve your changes.

1.  Validate the path to the Helm chart for the Speech services. The Helm chart exists at one of the following locations depending on the version of the Speech services you have installed:

    -   *Version 1.2:* `/mnt/installer/modules/watson-speech-base/x86_64/1.2/ibm-watson-speech-prod-1.2.1.tgz`
    -   *Version 1.2.1:* `/mnt/installer/modules/watson-speech-base/x86_64/1.2.1/ibm-watson-speech-prod-1.2.2.tgz`

1.  Run the `helm upgrade` upgrade command to add or remove the models or voices you updated in the Helm chart. In the command, `{speech_release_name}` is the name of your release, and `{path_to_helm_chart}` is the path from the previous step.

    ```bash
    helm upgrade {speech_release_name} {path_to_helm_chart} -f /tmp/values.yaml --tls
    ```
    {: pre}

    The amount of time required for the upgrade depends on the size of your installation and on the models or voices that you are adding or removing. You can monitor the status of the pod to ensure that the upgrade is progressing.

    The size of your cluster might lack the resources for your old and new deployment runtimes to run simultaneously. If the new runtime pods report insufficient CPU or memory, you can delete the old pods to free up resources.

    Deleting old pods causes the service to be unavailable until the new pods start running.
    {: important}

Once all of the new pods are in the `Running` state and healthy, the upgrade is complete. You can begin to use any new models or voices you enabled.

## Managing user access
{: #speech-cluster-user-access-12}

After you provision an instance, you can share the URL for the service with other users. However, those users can log in to the service only if you give them access.

If you plan to use Security Assertion Markup Language (SAML) for single sign-on (SSO), complete the procedure in [Configuring single sign-on](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/cpd/install/saml-sso.html){: external} before you add users. If you add users before you configure SSO, you need to re-add the users with their SAML IDs to enable them to use SSO.

1.  From the web client menu, click **Administer > Manage user**.
1.  Click **Add user**, then specify the user's full name, user name, and email address. Set the user's permissions, and then click **Add**.
1.  From the web client menu, select **My Instances**.
1.  Find your {{site.data.keyword.speechtotextshort}} instance, click the more (**...**) menu, and then choose **Manage Access**.
1.  Click **Add user**.
1.  Click the user name field to see a list of the people you can add.
1.  The users that are returned by the previous steps are listed. Select a name, choose **User** or **Admin** as their access role, and then click **Add**.

If you are not connected to an existing user registry and have not enabled single sign-on, then temporary passwords are created for the users that you add. The temporary passwords are sent to users by way of the email addresses you specify.

## Performing management tasks
{: #speech-cluster-tasks-12}

The following are some of the tasks you can perform to monitor and maintain a service instance:

-   Identify the cluster nodes to which the product is deployed:

    ```bash
    kubectl get pods -o wide | grep -v Completed
    ```
    {: pre}

-   List the number of replicas of each microservice in a given `{namespace}`:

    ```bash
    kubectl get deploy -n {namespace}
    ```
    {: pre}

    or

    ```bash
    kubectl get statefulset -n {namespace}
    ```
    {: pre}

-   Change (increase or decrease) the number of replicas:

    ```bash
    kubectl scale deploy {pod_name} --replicas={number}
    ```
    {: pre}

    or

    ```bash
    kubectl scale statefulsets {set_name} --replicas={number}
    ```
    {: pre}

## Meeting SecurityContextConstraints requirements
{: #speech-cluster-rhos-security-reqs-12}

The Speech services installation has no Red Hat OpenShift SecurityContextConstraints requirements. Follow the generic SecurityContextConstraints requirements.
{: note}

Installation requires a SecurityContextConstraints resource to be bound to the target namespace prior to installation. To meet this requirement, you need to perform cluster- and namespace-scoped pre and post actions.

If you do not specify a security context, the OpenShift [`restricted`](https://ibm.biz/cpkspec-scc){: external} security context constraint is applied by default. The predefined SecurityContextConstraints `restricted` has been verified for this installation.

If your target namespace is bound to this SecurityContextConstraints resource, you can skip the rest of this section and proceed with installation. Otherwise, run the following command to bind the `restricted` SecurityContextConstraints to your namespace:

```bash
oc adm policy add-scc-to-group restricted system:serviceaccounts:{namespace_name}
```
{: pre}

where `{namespace_name}` is the namespace into which {{site.data.keyword.icp4dfull_notm}} is installed, normally `zen`.

The standard definition of the `restricted` SecurityContextConstraints follows. Use this content to create a custom definition.

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities: null
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: MustRunAs
groups:
- system:authenticated
- system:serviceaccounts:zen
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: restricted denies access to all host features and
      requires pods to be run with a UID, and SELinux context that are allocated
      to the namespace. This is the most restrictive SCC and it is used by
      default for authenticated users.
  creationTimestamp: 2020-05-26T20:06:29Z
  name: restricted
  resourceVersion: "610956"
  selfLink: /apis/security.openshift.io/v1/securitycontextconstraints/restricted
  uid: 62cf9eba-9f8c-11ea-bf07-00163e01f134
priority: null
readOnlyRootFilesystem: false
requiredDropCapabilities:
- KILL
- MKNOD
- SETUID
- SETGID
runAsUser:
  type: MustRunAsRange
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
users: []
volumes:
- configMap
- downwardAPI
- emptyDir
- persistentVolumeClaim
- projected
- secret
```
{: codeblock}

## Disabling storage of user data
{: #speech-cluster-customer-data-12}

The {{site.data.keyword.speechtotextshort}} runtime and {{site.data.keyword.speechtotextshort}} customization AM patcher temporarily store payload data in the running container by default. The data includes audio files, recognition hypotheses, and annotations.

You can disable this behavior by checking the following option:

`STT Runtime | Disable storage of customer data`

Checking this option also removes sensitive information from container logs. For information about anonymizing data, see [Anonymizing logs and audio data](/docs/speech-to-text?topic=speech-to-text-speech-override-12#speech-override-anonymize-12).

<!-- COMMENT OUT FOR APRIL 2021 1.2.1 ADDITION OF MODIFYING MODELS/VOICES.
## Installing ad hoc models and voices
{: #speech-cluster-model-install-adhoc-12}

It is possible to install ad hoc models and voices, which are models and voices that are not included with this version of the service. To do so, you need to download a special package that contains data for the models and voices. You then upload the package into the cluster as you did the main package and specify the following values in the `speech-override.yaml` file during installation.

| Value | Description |
|-------|-------------|
| `global.sttModels.$modelName.catalogName` | Model name as it is found in the catalog |
| `global.sttModels.$modelName.size` | Memory footprint that is used to calculate memory requirements for the model |
| `global.ttsVoices.$voiceName.catalogName` | Voice name as it is found in the catalog |
| `global.ttsVoices.$voiceName.size` | Memory footprint that is used to calculate memory requirements for the voice |
{: caption="Table 1. Values for ad hoc models and voices"}

For example, suppose a new broadband model for the Czech language is made available after the current release. To enable it as a service update, specify the following values during installation: `$modelName` is `csCSBroadbandModel`, and `$catalogName` is `cs-CS_BroadbandModel`. For more information, see

-   [Installing {{site.data.keyword.speechtotextshort}} models](/docs/speech-to-text?topic=speech-to-text-speech-override-12#speech-override-stt-models-12)
-   [Installing {{site.data.keyword.texttospeechshort}} voices](/docs/speech-to-text?topic=speech-to-text-speech-override-12#speech-override-tts-voices-12)
-->
