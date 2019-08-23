---

copyright:
  years: 2017, 2019
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

# Utilisation d'un modèle acoustique personnalisé
{: #acousticUse}

Après avoir créé et entraîné votre modèle acoustique personnalisé, vous pouvez l'utiliser dans des demandes de reconnaissance vocale. Pour spécifier le modèle acoustique personnalisé dans le cadre d'une demande, utilisez le paramètre de requête `acoustic_customization_id`, comme illustré dans les exemples suivants. Vous devez émettre la demande avec les données d'identification de l'instance du service propriétaire du modèle.
{: shortdesc}

Vous pouvez également spécifier un modèle de langue personnalisé à utiliser avec la demande, ce qui peut optimiser la précision de la transcription. Pour plus d'informations, voir [Utilisation d'un modèle de langue personnalisé et d'un modèle acoustique personnalisé lors de la reconnaissance vocale](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize).

Vous pouvez créer plusieurs modèles acoustiques personnalisés pour des domaines ou environnements identiques ou différents. Cependant, vous ne pouvez spécifier qu'un seul modèle acoustique personnalisé à la fois avec le paramètre `acoustic_customization_id`.

-   Pour l'[interface WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), utilisez la méthode `/v1/recognize`. Le modèle personnalisé spécifié est utilisé pour toutes les demandes envoyées via la connexion.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Pour l'[interface HTTP synchrone](/docs/services/speech-to-text?topic=speech-to-text-http), utilisez la méthode `POST /v1/recognize`. Le modèle personnalisé spécifié est utilisé pour cette demande.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   Pour l'[interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async), utilisez la méthode `POST /v1/recognitions`. Le modèle personnalisé spécifié est utilisé pour cette demande.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

Vous pouvez omettre le modèle de langue dans la demande si le modèle personnalisé est basé sur le modèle par défaut, `en-US_BroadbandModel`. Autrement, vous devez utiliser le paramètre `model` pour spécifier le modèle de base, comme indiqué dans l'exemple correspondant à WebSocket. Un modèle personnalisé ne peut être utilisé qu'avec le modèle de base pour lequel il est créé.
