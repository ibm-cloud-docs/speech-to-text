---

copyright:
  years: 2020, 2023
lastupdated: "2024-08-14"

keywords: IBM,activity tracker,event,security,speech to text

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Activity Tracker events
{: #at-events}

[IBM Cloud]{: tag-ibm-cloud}

As of 28 March 2024, the {{site.data.keyword.at_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.at_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Activity tracking events are the same for both services. For information about migrating from {{site.data.keyword.at_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: important}

As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with {{site.data.keyword.speechtotextfull}} in {{site.data.keyword.cloud}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard.  For more information, see the tutorial [Getting started with {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

## Locations where activity tracking events are generated
{: #at-locations}

### Locations where activity tracking events are sent to {{site.data.keyword.at_full_notm}} hosted event search
{: #at-legacy-locations}

{{site.data.keyword.speechtotextfull}} in {{site.data.keyword.cloud}} sends activity tracking events to {{site.data.keyword.at_full_notm}} hosted event search in the regions that are indicated in the following table.

| Dallas (us-south)            | Washington (us-east)                         | Toronto (ca-tor)      | Sao Paulo (br-sao) |
|-------------------|-------------------------------------|------------------------------------|------------------------------------|
| [No]{: tag-red}          | [No]{: tag-red}  | [No]{: tag-red} | [No]{: tag-red}|                                                  |
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #at-table-1} 
{: tab-title="Americas"}
{: tab-group="at"}
{: class="simple-tab-table"} 
{: row-headers}

| Tokyo (jp-tok)           | Sydney (au-syd)                    |  Osaka (jp-osa)     | Chennai (in-che) |
|-------------------|-------------------------------------|------------------------------------|------------------------------------|
| [No]{: tag-red}          | [No]{: tag-red}  | [No]{: tag-red} | [No]{: tag-red}|                                                  |
{: caption="Regions where activity tracking events are sent in Asia Pacific locations" caption-side="top"} 
{: #at-table-2} 
{: tab-title="Asia Pacific"}
{: tab-group="at"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (eu-de)        | London (eu-gb)                 |  Madrid (eu-es)    | 
|-------------------|-------------------------------------|------------------------------------|
| [Yes]{: tag-green}       | [No]{: tag-red}  | [No]{: tag-red} |                              |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #at-table-3}  
{: tab-title="Europe"}
{: tab-group="at"} 
{: class="simple-tab-table"} 
{: row-headers}

## Language model customization events
{: #at-lm-events}

The following tables list the {{site.data.keyword.speechtotextshort}} actions for language model customization that generate events.

### Create events
{: #at-lm-events-create}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-language-model.create`           | Create a custom language model (`POST /v1/customizations`).                                                             |
| `speech-to-text.custom-language-model-word-list.create` | Create a word list for a custom language model (`POST /v1/customizations/{customization_id}/words`).                    |
| `speech-to-text.custom-language-model-word.create`      | Create a word for a custom language model (`PUT /v1/customizations/{customization_id}/words/{word_name}`).              |
| `speech-to-text.custom-language-model-corpus.create`    | Create a corpus for a custom language model (`POST /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar.create`   | Create a grammar for a custom language model (`POST /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
{: caption="Language model customization .create actions that generate events"}

### Read events
{: #at-lm-events-read}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-language-model-list.read`         | Read a list of custom language models (`GET /v1/customizations`).                                                   |
| `speech-to-text.custom-language-model.read`              | Read a custom language model (`GET /v1/customizations/{customization_id}`).                                         |
| `speech-to-text.custom-language-model-word-list.read`    | Read a word list of a custom language model (`GET /v1/customizations/{customization_id}/words`).                    |
| `speech-to-text.custom-language-model-word.read`         | Read a word for a custom language model (`GET /v1/customizations/{customization_id}/words/{word_name}`).            |
| `speech-to-text.custom-language-model-corpus-list.read`  | Read a corpus list of a custom language model (`GET /v1/customizations/{customization_id}/corpora`).                |
| `speech-to-text.custom-language-model-corpus.read`       | Read a corpus of a custom language model (`GET /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar-list.read` | Read a grammar list of a custom language model (`GET /v1/customizations/{customization_id}/grammars`).              |
| `speech-to-text.custom-language-model-grammar.read`      | Read a grammar of a custom language model (`GET /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
{: caption="Language model customization .read actions that generate events"}

### Delete events
{: #at-lm-events-delete}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-language-model.delete`         | Delete a custom language model (`DELETE /v1/customizations/{customization_id}`).                                           |
| `speech-to-text.custom-language-model-word.delete`    | Delete a word from a custom language model (`DELETE /v1/customizations/{customization_id}/words/{word_name}`).             |
| `speech-to-text.custom-language-model-corpus.delete`  | Delete a corpus from a custom language model (`DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar.delete` | Delete a grammar from a custom language model (`DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
{: caption="Language model customization .delete actions that generate events"}

### Train event
{: #at-lm-events-train}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-language-model.train` | Train a custom language model (`POST /v1/customizations/{customization_id}/train`).          |
{: caption="Language model customization .train action that generates an event"}

### Reset event
{: #at-lm-events-reset}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-language-model.reset` | Reset a custom language model (`POST /v1/customizations/{customization_id}/reset`).          |
{: caption="Language model customization .reset action that generates an event"}

### Upgrade event
{: #at-lm-events-upgrade}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-language-model.upgrade` | Upgrade a custom language model (`POST /v1/customizations/{customization_id}/upgrade_model`).          |
{: caption="Language model customization .upgrade action that generates an event"}

## Acoustic model customization events
{: #at_am_events}

The following tables list the {{site.data.keyword.speechtotextshort}} actions for acoustic model customization that generate events.

### Create events
{: #at-am-events-create}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-acoustic-model.create`           | Create a custom acoustic model (`POST /v1/acoustic_customizations`).                                                    |
| `speech-to-text.custom-acoustic-model-audio.create`     | Create an audio for a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Acoustic model customization .create actions that generate events"}

### Read events
{: #at-am-events-read}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-acoustic-model-list.read`         | Read a list of custom acoustic models (`GET /v1/acoustic_customizations`).                                          |
| `speech-to-text.custom-acoustic-model.read`              | Read a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}`).                                |
| `speech-to-text.custom-acoustic-model-audio-list.read`   | Read an audio list of a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}/audio`).         |
| `speech-to-text.custom-acoustic-model-audio.read`        | Read an audio of a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Acoustic model customization .read actions that generate events"}

### Delete events
{: #at-am-events-delete}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-acoustic-model.delete`         | Delete a custom acoustic model (`DELETE /v1/acoustic_customizations/{customization_id}`).                                  |
| `speech-to-text.custom-acoustic-model-audio.delete`   | Delete an audio from a custom acoustic model (`DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Acoustic model customization .delete actions that generate events"}

### Train event
{: #at-am-events-train}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-acoustic-model.train` | Train a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/train`). |
{: caption="Acoustic model customization .train action that generates an event"}

### Reset event
{: #at-am-events-reset}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-acoustic-model.reset` | Reset a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/reset`). |
{: caption="Acoustic model customization .reset action that generates an event"}

### Upgrade event
{: #at-am-events-upgrade}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.custom-acoustic-model.upgrade` | Upgrade a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/upgrade_model`). |
{: caption="Acoustic model customization .upgrade action that generates an event"}

## Asynchronous HTTP interface events
{: #at-async-events}

The following tables list the {{site.data.keyword.speechtotextshort}} actions for the asynchronous HTTP interface that generate events.

### Create events
{: #at-async-events-create}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.async-recognition-job.create` | Create an asynchronous recognition job (`POST /v1/recognitions`). |
| `speech-to-text.async-recognition-notification-url.create` | Create an asynchronous recognition notification URL (`POST /v1/register_callback`). |
{: caption="Asynchronous HTTP recognition .create actions that generate events"}

### Read events
{: #at-async-events-read}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.async-recognition-job-list.read` | Read an asynchronous recognition job list (`GET /v1/recognitions`). |
| `speech-to-text.async-recognition-job.read`      | Read an asynchronous recognition job (`GET /v1/recognitions/{id}`). |
{: caption="Asynchronous HTTP recognition .read actions that generate events"}

### Delete events
{: #at-async-events-delete}

| Action            | Description                         |
|-------------------|-------------------------------------|
| `speech-to-text.async-recognition-job.delete` | Delete an asynchronous recognition job (`DELETE /v1/recognitions/{id}`). |
| `speech-to-text.async-recognition-notification-url.delete` | Delete an asynchronous recognition notification URL (`POST /v1/unregister_callback`). |
{: caption="Asynchronous HTTP recognition .delete actions that generate events"}

## GDPR event
{: #at-events-gdpr}

The following table lists the {{site.data.keyword.speechtotextshort}} action for General Data Protection Regulation (GDPR) that generates an event.

| Action | Description |
|--------|-------------|
| `speech-to-text.gdpr-user-data.delete` | Delete information for a user (`DELETE /v1/user_data`). |
{: caption="GDPR .delete action that generates an event"}

## Where to view events
{: #at-ui}

Events that are generated by an instance of the {{site.data.keyword.speechtotextshort}} service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web user interface (UI) of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Navigating to the UI](/docs/activity-tracker?topic=activity-tracker-launch).
