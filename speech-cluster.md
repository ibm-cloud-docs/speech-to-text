---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-09"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Managing your cluster
{: #speech-cluster}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

After you install and configure the Speech services on {{site.data.keyword.icp4dfull}}, you can manage the service instance and the cluster. The following sections document some basic cluster and service management operations.
{: shortdesc}

## Managing user access
{: #speech-cluster-user-access}

After you provision an instance, you can share the URL for the service with other users. However, those users can log in to the service only if you give them access.

If you plan to use Security Assertion Markup Language (SAML) for single sign-on (SSO), complete the procedure in [Configuring single sign-on](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/cpd/install/saml-sso.html){: external} before you add users. If you add users before you configure SSO, you need to re-add the users with their SAML IDs to enable them to use SSO.

1.  From the web client menu, click **Administer > Manage user**.
1.  Click **Add user**, then specify the user's full name, user name, and email address. Set the user's permissions, and then click **Add**.
1.  From the web client menu, select **My Instances**.
1.  Find your {{site.data.keyword.speechtotextshort}} instance, click the more (**...**) menu, and then choose **Manage Access**.
1.  Click **Add user**.
1.  Click the user name field to see a list of the people you can add.
1.  The users that are returned by the previous steps are listed. Select a name, choose **User** or **Admin** as their access role, and then click **Add**.

If you are not connected to an existing user registry and have not enabled single sign-on, then temporary passwords are created for the users that you add. The temporary passwords are sent to users by way of the email addresses you specify.

## Performing management tasks
{: #speech-cluster-tasks}

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
{: #speech-cluster-rhos-security-reqs}

The Speech services installation has no Red Hat OpenShift SecurityContextConstraints requirements. Follow the generic SecurityContextConstraints requirements.
{: note}

Installation requires a SecurityContextConstraints resource to be bound to the target namespace prior to installation. To meet this requirement, you need to perform cluster- and namespace-scoped pre and post actions.

If you do not specify a security context, the OpenShift [`restricted`](https://ibm.biz/cpkspec-scc){: external} security context constraint is applied by default. The predefined SecurityContextConstraints `restricted` has been verified for this installation.

If your target namespace is bound to this SecurityContextConstraints resource, you can skip the rest of this section and proceed with installation. Otherwise, run the following command to bind the `restricted` SecurityContextConstraints to your namespace:

```bash
oc adm policy add-scc-to-group restricted system:serviceaccounts:{namespace_name}
```
{: pre}

where `{namespace_name}` is the namespace into which IBM Cloud Pak for Data is installed, normally `zen`.

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
{: #speech-cluster-customer-data}

The {{site.data.keyword.speechtotextshort}} runtime and {{site.data.keyword.speechtotextshort}} customization AM patcher temporarily store payload data in the running container by default. The data includes audio files, recognition hypotheses, and annotations.

You can disable this behavior by checking the following option:

`STT Runtime | Disable storage of customer data`

Checking this option also removes sensitive information from container logs. For information about anonymizing data, see [Anonymizing logs and audio data](/docs/speech-to-text?topic=speech-to-text-speech-override#speech-override-anonymize).

## Installing ad hoc models and voices
{: #speech-cluster-model-install-adhoc}

It is possible to install ad hoc models and voices, which are models and voices that are not included with this version of the service. To do so, you need to download a special package that contains data for the models and voices. You then upload the package into the cluster as you did the main package and specify the following values in the `speech-override.yaml` file during installation.

| Value | Description |
|-------|-------------|
| `global.sttModels.$modelName.catalogName` | Model name as it is found in the catalog |
| `global.sttModels.$modelName.size` | Memory footprint that is used to calculate memory requirements for the model |
| `global.ttsVoices.$voiceName.catalogName` | Voice name as it is found in the catalog |
| `global.ttsVoices.$voiceName.size` | Memory footprint that is used to calculate memory requirements for the voice |
{: caption="Table 1. Values for ad hoc models and voices"}

For example, suppose a new broadband model for the Czech language is made available after the current release. To enable it as a service update, specify the following values during installation: `$modelName` is `csCSBroadbandModel`, and `$catalogName` is `cs-CS_BroadbandModel`. For more information, see

-   [Installing {{site.data.keyword.speechtotextshort}} models](/docs/speech-to-text?topic=speech-to-text-speech-override#speech-override-stt-models)
-   [Installing {{site.data.keyword.texttospeechshort}} voices](/docs/speech-to-text?topic=speech-to-text-speech-override#speech-override-tts-voices)
