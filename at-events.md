---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-10"

keywords: IBM,activity tracker,event,security,speech to text

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:important: .important}
{:note: .note}

# Activity Tracker events
{: #atEvents}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud}} only**

As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with {{site.data.keyword.speechtotextfull}} in {{site.data.keyword.cloud}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the tutorial [Getting started with {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

## Language model customization events
{: #at_lm_events}

The following tables list the {{site.data.keyword.speechtotextshort}} actions for language model customization that generate events.

### Create events
{: #at_lm_events_create}

| Action                                                  | Description                                                                                                             |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.create`           | Create a custom language model (`POST /v1/customizations`).                                                             |
| `speech-to-text.custom-language-model-word-list.create` | Create a word list for a custom language model (`POST /v1/customizations/{customization_id}/words`).                    |
| `speech-to-text.custom-language-model-word.create`      | Create a word for a custom language model (`PUT /v1/customizations/{customization_id}/words/{word_name}`).              |
| `speech-to-text.custom-language-model-corpus.create`    | Create a corpus for a custom language model (`POST /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar.create`   | Create a grammar for a custom language model (`POST /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
{: caption="Table 1. Language model customization .create actions that generate events"}

### Read events
{: #at_lm_events_read}

| Action                                                   | Description                                                                                                         |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model-list.read`         | Read a list of custom language models (`GET /v1/customizations`).                                                   |
| `speech-to-text.custom-language-model.read`              | Read a custom language model (`GET /v1/customizations/{customization_id}`).                                         |
| `speech-to-text.custom-language-model-word-list.read`    | Read a word list of a custom language model (`GET /v1/customizations/{customization_id}/words`).                    |
| `speech-to-text.custom-language-model-word.read`         | Read a word for a custom language model (`GET /v1/customizations/{customization_id}/words/{word_name}`).            |
| `speech-to-text.custom-language-model-corpus-list.read`  | Read a corpus list of a custom language model (`GET /v1/customizations/{customization_id}/corpora`).                |
| `speech-to-text.custom-language-model-corpus.read`       | Read a corpus of a custom language model (`GET /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar-list.read` | Read a grammar list of a custom language model (`GET /v1/customizations/{customization_id}/grammars`).              |
| `speech-to-text.custom-language-model-grammar.read`      | Read a grammar of a custom language model (`GET /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
{: caption="Table 2. Language model customization .read actions that generate events"}

### Delete events
{: #at_lm_events_delete}

| Action                                                | Description                                                                                                                |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.delete`         | Delete a custom language model (`DELETE /v1/customizations/{customization_id}`).                                           |
| `speech-to-text.custom-language-model-word.delete`    | Delete a word from a custom language model (`DELETE /v1/customizations/{customization_id}/words/{word_name}`).             |
| `speech-to-text.custom-language-model-corpus.delete`  | Delete a corpus from a custom language model (`DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar.delete` | Delete a grammar from a custom language model (`DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
{: caption="Table 3. Language model customization .delete actions that generate events"}

### Train event
{: #at_lm_events_train}

| Action                                       | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.train` | Train a custom language model (`POST /v1/customizations/{customization_id}/train`).          |
{: caption="Table 4. Language model customization .train action that generates an event"}

### Reset event
{: #at_lm_events_reset}

| Action                                       | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.reset` | Reset a custom language model (`POST /v1/customizations/{customization_id}/reset`).          |
{: caption="Table 5. Language model customization .reset action that generates an event"}

### Upgrade event
{: #at_lm_events_upgrade}

| Action                                         | Description                                                                                            |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.upgrade` | Upgrade a custom language model (`POST /v1/customizations/{customization_id}/upgrade_model`).          |
{: caption="Table 6. Language model customization .upgrade action that generates an event"}

## Acoustic model customization events
{: #at_am_events}

The following tables list the {{site.data.keyword.speechtotextshort}} actions for acoustic model customization that generate events.

### Create events
{: #at_am_events_create}

| Action                                                  | Description                                                                                                             |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-acoustic-model.create`           | Create a custom acoustic model (`POST /v1/acoustic_customizations`).                                                    |
| `speech-to-text.custom-acoustic-model-audio.create`     | Create an audio for a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Table 7. Acoustic model customization .create actions that generate events"}

### Read events
{: #at_am_events_read}

| Action                                                   | Description                                                                                                         |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-acoustic-model-list.read`         | Read a list of custom acoustic models (`GET /v1/acoustic_customizations`).                                          |
| `speech-to-text.custom-acoustic-model.read`              | Read a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}`).                                |
| `speech-to-text.custom-acoustic-model-audio-list.read`   | Read an audio list of a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}/audio`).         |
| `speech-to-text.custom-acoustic-model-audio.read`        | Read an audio of a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Table 8. Acoustic model customization .read actions that generate events"}

### Delete events
{: #at_am_events_delete}

| Action                                                | Description                                                                                                                |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-acoustic-model.delete`         | Delete a custom acoustic model (`DELETE /v1/acoustic_customizations/{customization_id}`).                                  |
| `speech-to-text.custom-acoustic-model-audio.delete`   | Delete an audio from a custom acoustic model (`DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Table 9. Acoustic model customization .delete actions that generate events"}

### Train event
{: #at_am_events_train}

| Action                                       | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `speech-to-text.custom-acoustic-model.train` | Train a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/train`). |
{: caption="Table 10. Acoustic model customization .train action that generates an event"}

### Reset event
{: #at_am_events_reset}

| Action                                       | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `speech-to-text.custom-acoustic-model.reset` | Reset a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/reset`). |
{: caption="Table 11. Acoustic model customization .reset action that generates an event"}

### Upgrade event
{: #at_am_events_upgrade}

| Action                                         | Description                                                                                            |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-acoustic-model.upgrade` | Upgrade a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/upgrade_model`). |
{: caption="Table 12. Acoustic model customization .upgrade action that generates an event"}

## Asynchronous HTTP interface events
{: #at_async_events}

The following tables list the {{site.data.keyword.speechtotextshort}} actions for the asynchronous HTTP interface that generate events.

### Create events
{: #at_async_events_create}

| Action                                                     | Description                                                                         |
|------------------------------------------------------------|-------------------------------------------------------------------------------------|
| `speech-to-text.async-recognition-job.create`              | Create an asynchronous recognition job (`POST /v1/recognitions`).                   |
| `speech-to-text.async-recognition-notification-url.create` | Create an asynchronous recognition notification URL (`POST /v1/register_callback`). |
{: caption="Table 13. Asynchronous HTTP recognition .create actions that generate events"}

### Read events
{: #at_async_events_read}

| Action                                           | Description                                                         |
|--------------------------------------------------|---------------------------------------------------------------------|
| `speech-to-text.async-recognition-job-list.read` | Read an asynchronous recognition job list (`GET /v1/recognitions`). |
| `speech-to-text.async-recognition-job.read`      | Read an asynchronous recognition job (`GET /v1/recognitions/{id}`). |
{: caption="Table 14. Asynchronous HTTP recognition .read actions that generate events"}

### Delete events
{: #at_async_events_delete}

| Action                                                     | Description                                                                           |
|------------------------------------------------------------|---------------------------------------------------------------------------------------|
| `speech-to-text.async-recognition-job.delete`              | Delete an asynchronous recognition job (`DELETE /v1/recognitions/{id}`).              |
| `speech-to-text.async-recognition-notification-url.delete` | Delete an asynchronous recognition notification URL (`POST /v1/unregister_callback`). |
{: caption="Table 15. Asynchronous HTTP recognition .delete actions that generate events"}

## Where to view events
{: #at_ui}

Events that are generated by an instance of the {{site.data.keyword.speechtotextshort}} service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web user interface (UI) of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Navigating to the UI](/docs/activity-tracker?topic=activity-tracker-launch).
