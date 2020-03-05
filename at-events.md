---

copyright:
  years: 2020
lastupdated: "2020-02-27"

keywords: IBM,activity tracker,LogDNA,event,security,speech to text

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

As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with {{site.data.keyword.speechtotextfull}} in {{site.data.keyword.cloud}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the getting started tutorial for [{{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

## List of events
{: #at_actions}

The following table lists the {{site.data.keyword.speechtotextshort}} `.create` customization actions that generate an event.

| Action                                                  | Description                                                                                                             |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.create`           | Create a custom language model (`POST /v1/customizations`).                                                             |
| `speech-to-text.custom-acoustic-model.create`           | Create a custom acoustic model (`POST /v1/acoustic_customizations`).                                                    |
| `speech-to-text.custom-language-model-word-list.create` | Create a word list for a custom language model (`POST /v1/customizations/{customization_id}/words`).                    |
| `speech-to-text.custom-language-model-word.create`      | Create a word for a custom language model (`PUT /v1/customizations/{customization_id}/words/{word_name}`).              |
| `speech-to-text.custom-language-model-corpus.create`    | Create a corpus for a custom language model (`POST /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar.create`   | Create a grammar for a custom language model (`POST /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
| `speech-to-text.custom-acoustic-model-audio.create`     | Create an audio for a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Table 1. Customization .create actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.read` customization actions that generate an event.

| Action                                                   | Description                                                                                                         |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model-list.read`         | Read a list of custom language models (`GET /v1/customizations`).                                                   |
| `speech-to-text.custom-acoustic-model-list.read`         | Read a list of custom acoustic models (`GET /v1/acoustic_customizations`).                                          |
| `speech-to-text.custom-language-model.read`              | Read a custom language model (`GET /v1/customizations/{customization_id}`).                                         |
| `speech-to-text.custom-acoustic-model.read`              | Read a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}`).                                |
| `speech-to-text.custom-language-model-word-list.read`    | Read a word list of a custom language model (`GET /v1/customizations/{customization_id}/words`).                    |
| `speech-to-text.custom-language-model-word.read`         | Read a word for a custom language model (`GET /v1/customizations/{customization_id}/words/{word_name}`).            |
| `speech-to-text.custom-language-model-corpus-list.read`  | Read a corpus list of a custom language model (`GET /v1/customizations/{customization_id}/corpora`).                |
| `speech-to-text.custom-language-model-corpus.read`       | Read a corpus of a custom language model (`GET /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar-list.read` | Read a grammar list of a custom language model (`GET /v1/customizations/{customization_id}/grammars`).              |
| `speech-to-text.custom-language-model-grammar.read`      | Read a grammar of a custom language model (`GET /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
| `speech-to-text.custom-acoustic-model-audio-list.read`   | Read an audio list of a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}/audio`).         |
| `speech-to-text.custom-acoustic-model-audio.read`        | Read an audio of a custom acoustic model (`GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Table 2. Customization .read actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.reset` customization actions that generate an event.

| Action                                       | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.reset` | Reset a custom language model (`POST /v1/customizations/{customization_id}/reset`).          |
| `speech-to-text.custom-acoustic-model.reset` | Reset a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/reset`). |
{: caption="Table 3. Customization .reset actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.delete` customization actions that generate an event.

| Action                                                | Description                                                                                                                |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.delete`         | Delete a custom language model (`DELETE /v1/customizations/{customization_id}`).                                           |
| `speech-to-text.custom-acoustic-model.delete`         | Delete a custom acoustic model (`DELETE /v1/acoustic_customizations/{customization_id}`).                                  |
| `speech-to-text.custom-language-model-word.delete`    | Delete a word from a custom language model (`DELETE /v1/customizations/{customization_id}/words/{word_name}`).             |
| `speech-to-text.custom-language-model-corpus.delete`  | Delete a corpus from a custom language model (`DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}`).       |
| `speech-to-text.custom-language-model-grammar.delete` | Delete a grammar from a custom language model (`DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}`).    |
| `speech-to-text.custom-acoustic-model-audio.delete`   | Delete an audio from a custom acoustic model (`DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`). |
{: caption="Table 4. Customization .delete actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.train` customization actions that generate an event.

| Action                                       | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.train` | Train a custom language model (`POST /v1/customizations/{customization_id}/train`).          |
| `speech-to-text.custom-acoustic-model.train` | Train a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/train`). |
{: caption="Table 5. Customization .train actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.upgrade` customization actions that generate an event.

| Action                                         | Description                                                                                            |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `speech-to-text.custom-language-model.upgrade` | Upgrade a custom language model (`POST /v1/customizations/{customization_id}/upgrade_model`).          |
| `speech-to-text.custom-acoustic-model.upgrade` | Upgrade a custom acoustic model (`POST /v1/acoustic_customizations/{customization_id}/upgrade_model`). |
{: caption="Table 6. Customization .upgrade actions that generate events" caption-side="top"}

<!-- Not available with 20.03
The following table lists the {{site.data.keyword.speechtotextshort}} `.create` asynchronous recognition actions that generate an event.

| Action                                                     | Description                                                                         |
|------------------------------------------------------------|-------------------------------------------------------------------------------------|
| `speech-to-text.async-recognition-job.create`              | Create an asynchronous recognition job (`POST /v1/recognitions`).                   |
| `speech-to-text.async-recognition-notification-url.create` | Create an asynchronous recognition notification URL (`POST /v1/register_callback`). |
{: caption="Table 7. Asynchronous recognition .create actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.read` asynchronous recognition actions that generate an event.

| Action                                           | Description                                                         |
|--------------------------------------------------|---------------------------------------------------------------------|
| `speech-to-text.async-recognition-job-list.read` | Read an asynchronous recognition job list (`GET /v1/recognitions`). |
| `speech-to-text.async-recognition-job.read`      | Read an asynchronous recognition job (`GET /v1/recognitions/{id}`). |
{: caption="Table 8. Asynchronous recognition .read actions that generate events" caption-side="top"}

The following table lists the {{site.data.keyword.speechtotextshort}} `.delete` asynchronous recognition actions that generate an event.

| Action                                                     | Description                                                                           |
|------------------------------------------------------------|---------------------------------------------------------------------------------------|
| `speech-to-text.async-recognition-job.delete`              | Delete an asynchronous recognition job (`DELETE /v1/recognitions/{id}`).              |
| `speech-to-text.async-recognition-notification-url.delete` | Delete an asynchronous recognition notification URL (`POST /v1/unregister_callback`). |
{: caption="Table 9. Asynchronous recognition .delete actions that generate events" caption-side="top"}
-->

## Where to view events
{: #at_ui}

Events that are generated by an instance of the {{site.data.keyword.speechtotextshort}} service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web user interface (UI) of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).
