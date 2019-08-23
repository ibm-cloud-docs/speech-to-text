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

# Utilisation d'un modèle de langue personnalisé
{: #languageUse}

Après avoir créé et entraîné votre modèle de langue personnalisé, vous pouvez l'utiliser dans des demandes de reconnaissance vocale. Vous utilisez le paramètre de requête `language_customization_id` pour spécifier le modèle de langue personnalisé d'une demande, comme illustré dans les exemples suivants. Vous indiquez également au service le poids à attribuer aux mots du modèle personnalisé. Pour plus d'informations, voir [Utilisation d'un poids de personnalisation](#weight). Vous devez émettre la demande avec les données d'identification de l'instance du service propriétaire du modèle.
{: shortdesc}

Vous pouvez créer plusieurs modèles de langue personnalisés pour un même domaine ou pour des domaines différents. Cependant, vous ne pouvez spécifier qu'un seul modèle de langue personnalisé à la fois avec le paramètre `language_customization_id`. Pour obtenir des exemples d'utilisation d'une grammaire avec un modèle de langue personnalisé, voir [Utilisation d'une grammaire pour la reconnaissance vocale](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

-   Pour l'[interface WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), utilisez la méthode `/v1/recognize`. Le modèle personnalisé spécifié est utilisé pour toutes les demandes envoyées via la connexion.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Pour l'[interface HTTP synchrone](/docs/services/speech-to-text?topic=speech-to-text-http), utilisez la méthode `POST /v1/recognize`. Le modèle personnalisé spécifié est utilisé pour cette demande.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   Pour l'[interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async), utilisez la méthode `POST /v1/recognitions`. Le modèle personnalisé spécifié est utilisé pour cette demande.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

Vous pouvez omettre le modèle de langue dans la demande si le modèle personnalisé est basé sur le modèle de langue par défaut, `en-US_BroadbandModel`. Autrement, vous devez utiliser le paramètre `model` pour spécifier le modèle de base, comme indiqué dans l'exemple correspondant à WebSocket. Un modèle personnalisé ne peut être utilisé qu'avec le modèle de base pour lequel il est créé.

## Utilisation d'un poids de personnalisation
{: #weight}

Un modèle de langue personnalisé est une combinaison constituée du modèle personnalisé et du modèle de base qu'il personnalise. Vous pouvez indiquer au service le poids à attribuer aux mots du modèle de langue personnalisé par rapport aux mots du modèle de base pour la reconnaissance vocale. Le poids attribué à un modèle personnalisé est appelé *poids de personnalisation*.

Vous spécifiez le poids relatif à attribuer à un modèle de langue personnalisé en tant que valeur à deux chiffres comprise entre 0.0 et 1.0. Par défaut, chaque modèle de langue personnalisé a un poids avec la valeur 0.3. En règle générale, le poids par défaut produit les meilleures performances. Il permet de reconnaître les mots ne faisant pas partie du vocabulaire de base (mots OOV) du modèle personnalisé et les mots du vocabulaire de base.

Cependant, dans le cas de figure où les données audio à transcrire font appel à l'utilisation fréquente de mots OOV du modèle personnalisé, augmenter le poids de personnalisation peut contribuer à améliorer la précision des résultats de transcription. Soyez prudent lorsque vous définissez le poids de personnalisation. Alors qu'un poids plus élevé peut améliorer la précision des expressions du domaine du modèle personnalisé, il peut également avoir une incidence négative sur des expressions qui n'appartiennent pas au domaine concerné. (Même si vous définissez le poids avec la valeur 0.0, il y a peu de probabilité que la transcription puisse inclure les mots personnalisés en raison de l'implémentation de la personnalisation du modèle de langue.)

Vous spécifiez un poids de personnalisation en utilisant le paramètre `customization_weight`. Vous pouvez indiquer ce paramètre lorsque vous entraînez un modèle de langue personnalisé ou lorsque vous l'utilisez avec une demande de reconnaissance vocale.

-   Dans le cadre d'une demande d'entraînement, l'exemple suivant indique un poids de personnalisation égal à `0.5` avec la méthode `POST /v1/customizations/{customization_id}/train` :

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    La définition d'un poids de personnalisation lors de l'entraînement sauvegarde le poids avec le modèle de langue personnalisé. Vous n'avez pas à transmettre le poids avec chaque demande de reconnaissance qui utilise le modèle personnalisé.

-   Dans le cadre d'une demande de reconnaissance, l'exemple suivant indique un poids de personnalisation égal à `0.7` avec la méthode `POST /v1/recognize` :

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    La définition d'un poids de personnalisation lors la reconnaissance vocale remplace un poids qui avait été sauvegardé avec le modèle lors de l'entraînement.

## Traitement des incidents liés à l'utilisation des modèles de langue personnalisés
{: #languageTroubleshoot}

Si vous appliquez un modèle de langue personnalisé à la reconnaissance vocale mais que vous constatez que le service ne semble pas utiliser les mots inclus dans le modèle, recherchez les problèmes éventuels suivants :

-   Vérifiez que vous avez correctement transmis l'ID de personnalisation dans la demande de reconnaissance, comme indiqué dans les exemples précédents.
-   Vérifiez que le statut du modèle personnalisé est `available`, ce qui signifie que son entraînement est terminé et qu'il est prêt à l'emploi. Pour plus d'informations, voir [Affichage de la liste des modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Vérifiez les prononciations générées pour les mots nouveaux pour vous assurer qu'elles sont correctes. Pour plus d'informations, voir [Validation d'une ressource de mots](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
