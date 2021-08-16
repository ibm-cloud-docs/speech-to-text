---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-12"

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

# Using the override file
{: #speech-override}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull}} only**

You can use the `speech-override.yaml` file to customize many aspects of your {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} installation. The installation procedure in the IBM Documentation shows an abbreviated example of the `speech-override.yaml` file. But an override file can include many more installation and configuration values.

## The speech-override.yaml file
{: #speech-override-file}

The following example duplicates the `speech-override.yaml` example from [Override values for {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} installation](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/cpd/svc/watson/speech-to-text-override.html){: external} and [Override values for {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}} installation](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/cpd/svc/watson/text-to-speech-override.html){: external}. The sections that follow describe the values you can use to modify and augment the file. You can download a complete copy of the [speech-override.yaml](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/speech-override.yaml){: external} file.

```yaml
tags:
  sttAsync: true
  sttCustomization: true
  ttsCustomization: true
  sttRuntime: true
  ttsRuntime: true

affinity: {}

global:
  dockerRegistryPrefix: "{registry_from_cluster}/{project_namespace}"
  zenControlPlaneNamespace: "zen"
  image:
    pullSecret: {Docker_pull_secret}
    pullPolicy: "IfNotPresent"

  datastores:
    minio:
      secretName: "minio"
    postgressql:
      auth:
        authSecretName: "user-provided-postgressql"

  sttModels:
    enUsBroadbandModel:
      enabled: true
    enUsNarrowbandModel:
      enabled: true
    enUsShortFormNarrowbandModel:
      enabled: true

  ttsVoices:
    enUSMichaelV3Voice:
      enabled: true
    enUSAllisonV3Voice:
      enabled: true
    enUSLisaV3Voice:
      enabled: true
```
{: codeblock}

You replace the following variable values in the file:

-   `{registry_from_cluster}/{project_namespace}` is the address of the internal Red Hat&reg; OpenShift&reg; Docker registry for the mandatory `{project_namespace}` value, which is typically `zen`. The `{registry_from_cluster}` is one of the following:
    -   `image-registry.openshift-image-registry.svc:5000` for OpenShift 4.x.
    -   `docker-registry.default.svc:5000` for OpenShift 3.x.
-   `{Docker_pull_secret}` is the pull secret for the internal Docker registry, which you can learn by running the following command:

    ```bash
    oc get secrets | grep default-dockercfg
    ```
    {: pre}

## Override values for both Speech services
{: #speech-override-both}

The following sections describe override values that apply to both Speech services.

### Installing Speech services components
{: #speech-override-components}

The following five main components provide the base {{site.data.keyword.speechtotextshort}} and  {{site.data.keyword.texttospeechshort}} functionality. You can set these tags in the `speech-override.yaml` file to enable or disable the installation of each of the components.

By default, all of the components are enabled, but you can enable or disable them separately.

-   To install {{site.data.keyword.speechtotextshort}} only, set `ttsRuntime` and `ttsCustomization` to `false`.
-   To install  {{site.data.keyword.texttospeechshort}} only, set `sttRuntime`, `sttCustomization`, and `sttAsync` to `false`.
-   To install both {{site.data.keyword.speechtotextshort}} and  {{site.data.keyword.texttospeechshort}} without enabling customization, set `sttCustomization` and `ttsCustomization` to `false`.

Table 1 describes the top-level Speech components that you can install.

| Value | Installs ... | Default |
|-------|--------------|:-------:|
| `tags.sttRuntime` | {{site.data.keyword.speechtotextshort}} runtime component, the base component for speech recognition. This value enables the `/v1/recognize` interfaces (synchronous HTTP and WebSocket). Enabling any other {{site.data.keyword.speechtotextshort}} component automatically enables the {{site.data.keyword.speechtotextshort}} runtime component. For more information, see [The synchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-http) and [The WebSocket interface](/docs/speech-to-text?topic=speech-to-text-websockets). | `True` |
| `tags.sttAsync` | {{site.data.keyword.speechtotextshort}} asynchronous HTTP component. This value enables the `/v1/recognitions` interface. For more information, see [The asynchronous HTTP interface](/docs/speech-to-text?topic=speech-to-text-async). | `True` |
| `tags.sttCustomization` | {{site.data.keyword.speechtotextshort}} customization component. This value enables the `/v1/customizations` and `/v1/acoustic_customizations` interfaces for language model and acoustic model customization. Enabling it also enables the {{site.data.keyword.speechtotextshort}} runtime component if `tags.sttRuntime=false`. For more information, see [The customization interface](/docs/speech-to-text?topic=speech-to-text-customization). | `True` |
| `tags.ttsRuntime` | {{site.data.keyword.texttospeechshort}} runtime component, the base component for speech synthesis. This value enables the `/v1/synthesize` interfaces (HTTP and WebSocket). Enabling any other {{site.data.keyword.texttospeechshort}} component automatically enables the {{site.data.keyword.texttospeechshort}} runtime component. For more information, see [The HTTP interface](/docs/text-to-speech?topic=text-to-speech-usingHTTP) and [The WebSocket interface](/docs/text-to-speech?topic=text-to-speech-usingWebSocket). | `True` |
| `tags.ttsCustomization` | {{site.data.keyword.texttospeechshort}} customization component. This value enables the `/v1/customizations` interface for customization. Enabling it also enables the {{site.data.keyword.texttospeechshort}} runtime component if `tags.sttRuntime=false`. For more information, see [Understanding customization](/docs/text-to-speech?topic=text-to-speech-customIntro). | `True` |
{: caption="Table 1. Installation of Speech services components"}

### Configuring datastores
{: #speech-override-datastores}

Table 2 describes the values you can specify to configure the datastores for your installation. For more information about the MinIO and PostgreSQL datastores, see [Managing datastores](/docs/speech-to-text?topic=speech-to-text-speech-datastores) and [Scaling up your installation](/docs/speech-to-text?topic=speech-to-text-speech-scaling).

| Value | Specifies ... | Default |
|-------|---------------|:-------:|
| `external.minio.mode` | MinIO server mode (`standalone` or `distributed`). | `distributed` |
| `external.minio.persistence.size` | Size of persistent volume claim (PVC) for MinIO. | `100Gi` |
| `external.minio.replicas` | Number of nodes (applicable only for MinIO distributed mode). Must be `4 <= x <= 32`. | `4` |
| `external.minio.minioAccessSecret` | A secrets object that contains base64-encoded `accesskey` (5 - 20 characters) and `secretkey` (8 - 40 characters) values. The keys are used to access the MinIO Object Server securely. You need to create the secrets object in the same namespace in which you deploy the services. | `minio` |
| `global.datastores.minio.secretName` | Name of the secrets object that contains the MinIO secrets that are needed to access the datastore securely. | `minio` |
| `global.datastores.postgressql.auth.authSecretName` | Name of the secrets object that contains the PostgresSQL credentials that are needed to access the datastore securely. | `user-provided-postgressql` |
| `postgressql.auth.authSecretName` | Name of the secrets object that contains the PostgresSQL credentials that are needed to access the datastore securely. | `user-provided-postgressql` |
{: caption="Table 2. Configuration of datastores"}

### Specifying dynamic resource calculation
{: #speech-override-dynamic}

The {{site.data.keyword.speechtotextshort}} runtime, {{site.data.keyword.texttospeechshort}} runtime, and {{site.data.keyword.speechtotextshort}} customization AM patcher support dynamic memory resource calculation. The calculation is performed automatically based on the selected number of CPUs and the selected models and voices.

Dynamic resource calculation is enabled by default. You can modify this behavior by setting the values in Table 3 to `true` or `false` in the root of the override file. If you set any of these values to `false`, you must specify the required memory yourself.

Disabling dynamic resource allocation is not recommended and can cause undesired service behavior.
{: important}

| Value | Enables ... | Default |
|-------|-------------|:-------:|
| `sttRuntime.groups.sttRuntimeDefault.resources.dynamicMemory` | Dynamic resource calculation for the {{site.data.keyword.speechtotextshort}} runtime component. | `True` |
| `ttsRuntime.groups.ttsRuntimeDefault.resources.dynamicMemory` | Dynamic resource calculation for the {{site.data.keyword.texttospeechshort}} runtime component. | `True` |
| `sttAMPatcher.groups.sttAMPatcher.resources.dynamicMemory` | Dynamic resource calculation for the {{site.data.keyword.speechtotextshort}} customization AM patcher component. | `True` |
{: caption="Table 3. Configuration of dynamic resource calculation"}

### Specifying node affinity
{: #speech-override-affinity}

Node affinity for the Speech services pods enables you to constrain the nodes on which a pod can run based on the nodes' labels. The default affinity allows any pod to run on any amd64 node.

You can override the default specification by defining the `affinity` value in the `speech-override.yaml` file. For example, the following specification replaces the `affinity: {}` definition to enable affinity for the designated nodes.

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/e2e-az-name
          operator: In
          values:
          - e2e-az1
          - e2e-az2
```
{: codeblock}

### Anonymizing logs and audio data
{: #speech-override-anonymize}

You can anonymize your log and audio data for increased user privacy. Use the values in Table 4 to anonymize data that is stored by the different components. You can also disable the temporary storing of payload data in the running container. For more information, see [Disabling storage of user data](/docs/speech-to-text?topic=speech-to-text-speech-cluster#speech-cluster-customer-data).

| Value | Anonymizes ... | Default |
|-------|----------------|:-------:|
| `sttRuntime.anonymizeLogs` | {{site.data.keyword.speechtotextshort}} runtime logs and audio data. | `False` |
| `ttsRuntime.anonymizeLogs` | {{site.data.keyword.texttospeechshort}} runtime logs and audio data. | `False` |
| `sttAMPatcher.anonymizeLogs` | {{site.data.keyword.speechtotextshort}} customization acoustic model (AM) patcher runtime logs and audio data. | `False` |
{: caption="Table 4. Anonymization of logs and audio data"}

## Override values for the {{site.data.keyword.speechtotextshort}} service
{: #speech-override-stt}

The following sections describe override values that are specific to the {{site.data.keyword.speechtotextshort}} service.

### Configuring the {{site.data.keyword.speechtotextshort}} runtime
{: #speech-override-stt-runtime}

Table 5 describes the configuration values for the {{site.data.keyword.speechtotextshort}} runtime.

| Value | Specifies ... | Default |
|-------|---------------|:-------:|
| `sttRuntime.groups.sttRuntimeDefault.resources.dynamicMemory` | Automatic calculation of the memory requirements for the {{site.data.keyword.speechtotextshort}} runtime according to the selected models. | `True` |
| `sttRuntime.groups.sttRuntimeDefault.resources.requestsCpu` | Requested CPUs for the {{site.data.keyword.speechtotextshort}} runtime. The minimum value is 4. Each runtime session needs at least 4 CPUs. | `8` |
| `sttRuntime.groups.sttRuntimeDefault.resources.requestsMemory` | Memory requirements for the {{site.data.keyword.speechtotextshort}} runtime. When dynamic memory is enabled, this value has no effect. | `22Gi` |
{: caption="Table 5. Configuration of {{site.data.keyword.speechtotextshort}} runtime"}

### Configuring the {{site.data.keyword.speechtotextshort}} customization AM patcher
{: #speech-override-stt-AMpatcher}

Table 6 describes the configuration values for the {{site.data.keyword.speechtotextshort}} customization AM patcher.

| Value | Specifies ... | Default |
|-------|---------------|:-------:|
| `sttAMPatcher.groups.sttAMPatcher.resources.dynamicMemory` | Automatic calculation of the memory requirements for the {{site.data.keyword.speechtotextshort}} customization AM patcher according to the selected models. | `True` |
| `sttAMPatcher.groups.sttAMPatcher.resources.requestsCpu` | Requested CPUs for the {{site.data.keyword.speechtotextshort}} customization AM patcher. The minimum value is 4. Each customization session needs at least 4 CPUs. | `8` |
| `sttAMPatcher.groups.sttAMPatcher.resources.requestsMemory` | Memory requirements for the {{site.data.keyword.speechtotextshort}} customization AM patcher. The amount of memory depends on the number of CPUs. The size can be calculated as the number of CPUs * 3 GB. | `22Gi` |
| `sttAMPatcher.groups.sttAMPatcher.resources.threads` | Number of parallel-processing threads for the {{site.data.keyword.speechtotextshort}} customization AM patcher. Note that fewer threads means longer training time. | `4` |
{: caption="Table 6. Configuration of {{site.data.keyword.speechtotextshort}} customization AM patcher"}

### Installing {{site.data.keyword.speechtotextshort}} models
{: #speech-override-stt-models}

You can use the values in Table 7 to specify the language models to install. Installing all models substantially increases the memory requirements. You are therefore strongly encouraged to install only those models and voices that you intend to use. By default, the dynamic resource calculation feature automatically computes the exact amount of memory that is required for the selected models.

You specify the models to be installed by setting the values in the `speech-override.yaml` file to `true` or `false`. For example, to install the Japanese, Korean, and Chinese language models (both broadband and narrowband) instead of the US English models, modify the `global.sttModels` values as follows:

```yaml
global:
  sttModels:
    enUsBroadbandModel:
      enabled: false
    enUsNarrowbandModel:
      enabled: false
    enUsShortFormNarrowbandModel:
      enabled: false
    jaJPBroadbandModel:
      enabled: true
    jaJPNarrowbandModel:
      enabled: true
    koKRBroadbandModel:
      enabled: true
    koKRNarrowbandModel:
      enabled: true
    zhCNBroadbandModel:
      enabled: true
    zhCNNarrowbandModel:
      enabled: true
```
{: codeblock}

For more information about all available models, see [Previous-generation languages and models](/docs/speech-to-text?topic=speech-to-text-models). For information about updating the override file to install ad hoc models, see [Installing ad hoc models and voices](/docs/speech-to-text?topic=speech-to-text-speech-cluster#speech-cluster-model-install-adhoc).

| Value | Installs ... | Default |
|-------|--------------|:-------:|
| `global.sttModels.enUsBroadbandModel.enabled` | US English (`en-US`) Broadband model | `True` |
| `global.sttModels.enUsNarrowbandModel.enabled` | US English (`en-US`) Narrowband model | `True` |
| `global.sttModels.enUsShortFormNarrowbandModel.enabled` | US English (`en-US`) Short-Form Narrowband model | `True` |
| `global.sttModels.arArBroadbandModel.enabled` | Modern Standard Arabic (`ar-AR`) Broadband model | `False` |
| `global.sttModels.enGbBroadbandModel.enabled` | UK English (`en-GB`) Broadband model | `False` |
| `global.sttModels.enGbNarrowbandModel.enabled` | UK English (`en-GB`) Narrowband model | `False` |
| `global.sttModels.esEsBroadbandModel.enabled` | Spanish (`es-ES`, `es-AR`, `es-CL`, `es-CO`, `es-MX`, and `es-PE`) Broadband models | `False` |
| `global.sttModels.esEsNarrowbandModel.enabled` | Spanish (`es-ES`, `es-AR`, `es-CL`, `es-CO`, `es-MX`, and `es-PE`) Narrowband models | `False` |
| `global.sttModels.frFrBroadbandModel.enabled` | French (`fr-FR`) Broadband model | `False` |
| `global.sttModels.frFrNarrowbandModel.enabled` | French (`fr-FR`) Narrowband model | `False` |
| `global.sttModels.deDeBroadbandModel.enabled` | German (`de-DE`) Broadband model | `False` |
| `global.sttModels.deDeNarrowbandModel.enabled` | German (`de-DE`) Narrowband model | `False` |
| `global.sttModels.itItBroadbandModel.enabled` | Italian (`it-IT`) Broadband model | `False` |
| `global.sttModels.itItNarrowbandModel.enabled` | Italian (`it-IT`) Narrowband model | `False` |
| `global.sttModels.jaJpBroadbandModel.enabled` | Japanese (`ja-JP`) Broadband model | `False` |
| `global.sttModels.jaJpNarrowbandModel.enabled` | Japanese (`ja-JP`) Narrowband model | `False` |
| `global.sttModels.koKrBroadbandModel.enabled` | Korean (`ko-KR`) Broadband model | `False` |
| `global.sttModels.koKrNarrowbandModel.enabled` | Korean (`ko-KR`) Narrowband model | `False` |
| `global.sttModels.nlNlBroadbandModel.enabled` | Dutch (`nl-NL`) Broadband model | `False` |
| `global.sttModels.nlNlNarrowbandModel.enabled` | Dutch (`nl-NL`) Narrowband model | `False` |
| `global.sttModels.ptBrBroadbandModel.enabled` | Brazilian Portuguese (`pt-BR`) Broadband model | `False` |
| `global.sttModels.ptBrNarrowbandModel.enabled` | Brazilian Portuguese (`pt-BR`) Narrowband model | `False` |
| `global.sttModels.zhCnBroadbandModel.enabled` | Mandarin Chinese (`zh-CN`) Broadband model | `False` |
| `global.sttModels.zhCnNarrowbandModel.enabled` | Mandarin Chinese (`zh-CN`) Narrowband model | `False` |
{: caption="Table 7. Installation of {{site.data.keyword.speechtotextshort}} models"}

## Override values for the {{site.data.keyword.texttospeechshort}} service
{: #speech-override-tts}

The following sections describe override values that are specific to the {{site.data.keyword.texttospeechshort}} service.

### Configuring the {{site.data.keyword.texttospeechshort}} runtime
{: #speech-override-tts-runtime}

Table 8 describes the configuration values for the {{site.data.keyword.texttospeechshort}} runtime.

| Value | Specifies ... | Default |
|-------|---------------|:-------:|
| `ttsRuntime.groups.ttsRuntimeDefault.resources.dynamicMemory` | Automatic calculation of memory requirements for the {{site.data.keyword.texttospeechshort}} runtime according to the selected voices. | `True` |
| `ttsRuntime.groups.ttsRuntimeDefault.resources.requestsCpu` | Requested CPUs for the {{site.data.keyword.texttospeechshort}} runtime. The minimum value is 4. Each runtime session needs at least 4 CPUs. | `8` |
| `ttsRuntime.groups.ttsRuntimeDefault.resources.requestsMemory` | Memory requirements for the {{site.data.keyword.texttospeechshort}} runtime. When dynamic memory is enabled, this value has no effect. | `22Gi` |
| `global.ttsVoiceMarginalCPU` | {{site.data.keyword.texttospeechshort}} voice marginal CPU used for synthesis. The value is in milli-CPUs. | `400` |
{: caption="Table 8. Configuration of {{site.data.keyword.texttospeechshort}} runtime"}

### Installing {{site.data.keyword.texttospeechshort}} voices
{: #speech-override-tts-voices}

You can use the values in Table 9 to specify the voices to install. Installing all voices increases the memory requirements. You are therefore encouraged to install only those voices that you intend to use. By default, the dynamic resource calculation feature automatically computes the exact amount of memory that is required for the selected voices.

You specify the voices to be installed by setting the values in the `speech-override.yaml` file to `true` or `false`. For example, to install the German, French, and Italian voices instead of the US English voices, modify the `global.ttsVoices` values as follows:

```yaml
global:
  ttsVoices:
    enUSMichaelV3Voice:
      enabled: false
    enUSAllisonV3Voice:
      enabled: false
    enUSLisaV3Voice:
      enabled: false
    deDEBirgitV3Voice:
      enabled: true
    deDEDieterV3Voice:
      enabled: true
    deDEErikaV3Voice:
      enabled: true
    frFRReneeV3Voice:
      enabled: true
    itITFrancescaV3Voice:
      enabled: true
```
{: codeblock}

For more information about all available voices, see [Using languages and voices](/docs/text-to-speech?topic=text-to-speech-voices). For information about updating the override file to install ad hoc voices, see [Installing ad hoc models and voices](/docs/speech-to-text?topic=speech-to-text-speech-cluster#speech-cluster-model-install-adhoc).

| Value | Installs ... | Default |
|-------|--------------|:-------:|
| `global.ttsVoices.enUSAllisonV3Voice.enabled` | US English (`en-US`) Allison neural voice | `True` |
| `global.ttsVoices.enUSLisaV3Voice.enabled` | US English (`en-US`) Lisa neural voice | `True` |
| `global.ttsVoices.enUSMichaelV3Voice.enabled` | US English (`en-US`) Michael neural voice | `True` |
| `global.ttsVoices.enUSEmilyV3Voice.enabled` | US English (`en-US`) Emily neural voice | `False` |
| `global.ttsVoices.enUSHenryV3Voice.enabled` | US English (`en-US`) Henry neural voice | `False` |
| `global.ttsVoices.enUSKevinV3Voice.enabled` | US English (`en-US`) Kevin neural voice | `False` |
| `global.ttsVoices.enUSOliviaV3Voice.enabled` | US English (`en-US`) Olivia neural voice | `False` |
| `global.ttsVoices.deDEBirgitV3Voice.enabled` | German (`de-DE`) Birgit neural voice | `False` |
| `global.ttsVoices.deDEDieterV3Voice.enabled` | German (`de-DE`) Dieter neural voice | `False` |
| `global.ttsVoices.deDeErikaV3Voice.enabled` | German (`de-DE`) Erika neural voice | `False` |
| `global.ttsVoices.enGBKateV3Voice.enabled` | UK English (`en-GB`) Kate neural voice | `False` |
| `global.ttsVoices.esESEnriqueV3Voice.enabled` | Spanish (`es-ES`) Enrique neural voice | `False` |
| `global.ttsVoices.esESLauraV3Voice.enabled` | Spanish (`es-ES`) Laura neural voice | `False` |
| `global.ttsVoices.esLASofiaV3Voice.enabled` | Latin American Spanish (`es-LA`) Sofia neural voice | `False` |
| `global.ttsVoices.esUSSofiaV3Voice.enabled` | North American Spanish (`es-US`) Sofia neural voice | `False` |
| `global.ttsVoices.frFRReneeV3Voice.enabled` | French (`fr-FR`) Renee neural voice | `False` |
| `global.ttsVoices.itITFrancescaV3Voice.enabled` | Italian (`it-IT`) Francesca neural voice | `False` |
| `global.ttsVoices.jaJPEmiV3Voice.enabled` | Japanese (`ja-JP`) Emi neural voice | `False` |
| `global.ttsVoices.ptBRIsabelaV3Voice.enabled` | Brazilian Portuguese (`pt-BR`) Isabela neural voice | `False` |
{: caption="Table 9. Installation of {{site.data.keyword.texttospeechshort}} voices"}
