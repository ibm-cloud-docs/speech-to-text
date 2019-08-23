---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-24"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Utilisation d'une grammaire pour la reconnaissance vocale
{: #grammarUse}

Après avoir créé et entraîné votre modèle de langue personnalisé avec votre grammaire, vous pouvez utiliser cette grammaire dans les demandes de reconnaissance vocale avec les interfaces WebSocket et HTTP du service.
{: shortdesc}

-   Utilisez le paramètre `language_customization_id` pour spécifier l'ID de personnalisation (GUID) du modèle de langue personnalisé pour lequel est définie la grammaire. Vous devez émettre la demande avec les données d'identification de l'instance du service propriétaire du modèle.
-   Utilisez le paramètre `grammar_name` pour spécifier le nom de la grammaire. Vous ne pouvez spécifier qu'une seule grammaire avec une demande.

Lorsque vous utilisez une grammaire, le service reconnaît uniquement les mots de la grammaire spécifiée. Il n'utilise pas les mots personnalisés ajoutés à partir de corpus, ajoutés ou modifiés individuellement ou reconnus par d'autres grammaires.

-   Pour l'[interface WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), vous spécifiez d'abord l'ID de personnalisation avec le paramètre `language_customization_id` de la méthode `/v1/recognize`. Vous utilisez cette méthode pour établir une connexion WebSocket avec le service.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    Vous indiquez ensuite le nom de la grammaire avec le paramètre `grammar_name` dans le message JSON `start` pour la connexion active. La transmission de cette valeur avec le message `start`, vous permet de modifier la grammaire de manière dynamique pour chaque demande que vous envoyez via la connexion.

    ```javascript
    function onOpen(evt) {
      var message = {
        action: 'start',
        content-type: 'audio/l16;rate=22050',
        grammar_name: '{grammar_name}'
      };
      websocket.send(JSON.stringify(message));
      websocket.send(blob);
    }
    ```
    {: codeblock}
-   Pour l'[interface HTTP synchrone](/docs/services/speech-to-text?topic=speech-to-text-http), transmettez les deux paramètres avec la méthode `POST /v1/recognize`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   Pour l'[interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async), transmettez les deux paramètres avec la méthode `POST /v1/recognitions`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
