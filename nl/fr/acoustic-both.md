---

copyright:
  years: 2019
lastupdated: "2019-03-10"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Utilisation d'un modèle acoustique personnalisé avec un modèle de langue personnalisé
{: #useBoth}

Vous pouvez améliorer la précision de la reconnaissance vocale en combinant deux modèles complémentaires : un modèle de langue personnalisé et un modèle acoustique personnalisé. Vous pouvez utiliser ces deux types de modèle lors de l'entraînement de votre modèle acoustique et lors de la reconnaissance vocale. Les deux modèles doivent appartenir à la même instance de service et ils doivent tous les deux être basés sur le même modèle de langue de base.
{: shortdesc}

L'utilisation d'un modèle acoustique personnalisé seul ou avec un modèle de langue personnalisé sur lequel il n'a pas été entraîné peut toujours être utile. Si le modèle acoustique personnalisé a été entraîné avec des caractéristiques acoustiques correspondant à la séquence audio en cours de transcription, il peut encore améliorer la qualité de la transcription.

## Entraînement d'un modèle acoustique personnalisé avec un modèle de langue personnalisé
{: #useBothTrain}

L'entraînement d'un modèle acoustique personnalisé avec uniquement des données audio est appelé *entraînement non supervisé*. L'utilisation d'un modèle de langue personnalisé lors de l'entraînement est appelé *entraînement légèrement supervisé*. L'entraînement légèrement supervisé peut contribuer à améliorer l'efficacité de votre modèle acoustique personnalisé.

Utilisez l'entraînement légèrement supervisé pour entraîner un modèle acoustique personnalisé avec un modèle de langue personnalisé dans les cas suivants :

-   Le modèle de langue personnalisé est basé sur des transcriptions (compte-rendu textuel) des fichiers audio que vous avez ajoutés au modèle acoustique personnalisé.

    Comme les transcriptions contiennent le contenu exact des données audio, l'entraînement avec un modèle de langue personnalisé basé sur ces transcriptions peut donner les meilleurs résultats. Le service peut analyser le contenu des transcriptions dans leur contexte et extraire des mots ne figurant pas dans le vocabulaire de base du service (mots OOV) et des n-grammes pouvant l'aider à utiliser vos données audio de la manière la plus efficace possible. C'est notamment le cas si vos données audio ont une durée de moins d'une heure.

    La transcription des données audio n'est pas strictement nécessaire. Mais si vous disposez de transcriptions de ce type, elles permettent d'améliorer la qualité du modèle acoustique personnalisé. Les transcriptions sont particulièrement appréciables si les données audio comportent un grand nombre de mots OOV.
-   Le modèle de langue personnalisé est basé sur des corpus (fichiers texte) ou sur une liste de mots caractéristiques du contenu des fichiers audio.

    Si vos données audio contiennent des mots spécifiques à un domaine qui ne figurent pas dans le vocabulaire de base du service, la personnalisation du modèle acoustique seul ne génère pas ces mots lors de la transcription. La personnalisation de modèle de langue personnalisé est le seul moyen d'enrichir le vocabulaire de base du service. Si vous ne disposez pas de transcriptions, effectuez l'entraînement avec un modèle de langue personnalisé incluant des mots OOV du même domaine que vos données audio. Même l'entraînement avec un modèle de langue personnalisé incluant une liste de mots OOV utilisés dans les données audio peut s'avérer utile.

    Par exemple, supposons que vous créez un modèle acoustique personnalisé basé sur les données audio d'un centre d'appels pour un produit spécifique. Vous pouvez entraîner le modèle acoustique personnalisé avec un modèle de langue personnalisé basé sur des transcriptions d'appels associés ou incluant les noms de produits spécifiques traités par le centre d'appels.

Pour utiliser une transcription ou une liste de mots, vous devez d'abord créer un modèle de langue personnalisé contenant ces données textuelles. Pour entraîner un modèle acoustique personnalisé avec un modèle de langue personnalisé, ces deux modèles personnalisés doivent être basés sur la même version du même modèle de base. Si une nouvelle version du modèle de base est mise à disposition, vous devez effectuer la mise niveau des deux modèles à la même version du modèle de base pour que l'entraînement réussisse.

Utilisez le paramètre de requête facultatif `custom_language_model_id` de la méthode `POST /v1/acoustic_customizations/{customization_id}/train` pour entraîner votre modèle acoustique personnalisé avec un modèle de langue personnalisé. Transmettez l'identificateur global unique (GUID) du modèle acoustique avec le paramètre `customization_id` et le GUID du modèle de langue personnalisé avec le paramètre `custom_language_model_id`. Les deux modèles doivent appartenir aux données d'identification de service transmises avec la demande.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Utilisation d'un modèle de langue personnalisé et d'un modèle acoustique personnalisé lors de la reconnaissance vocale
{: #useBothRecognize}

Vous pouvez spécifier à la fois un modèle de langue personnalisé et un modèle acoustique personnalisé avec une demande de reconnaissance. Si un modèle de langue personnalisé contient des mots OOV appartenant au domaine des données audio qui font l'objet de la reconnaissance, vous pouvez l'utiliser avec un modèle acoustique personnalisé lors de la reconnaissance vocale afin d'obtenir une transcription plus précise.

L'utilisation d'un modèle de langue personnalisé peut améliorer la précision de la transcription que vous ayez entraîné ou non le modèle acoustique personnalisé avec le modèle de langue personnalisé :

-   L'utilisation d'un modèle de langue personnalisé avec un modèle acoustique personnalisé lors de l'entraînement améliore la qualité du modèle acoustique personnalisé.
-   L'utilisation de ces deux types de modèle lors de la reconnaissance vocale améliore la qualité de la transcription.

Si un modèle de langue personnalisé comprend des grammaires, vous pouvez également utiliser le modèle de langue personnalisé et l'une de ses grammaires avec un modèle acoustique personnalisé lors de la reconnaissance vocale.

L'exemple suivant transmet les deux types de modèles à la méthode HTTP `POST /v1/recognize`. Spécifiez le GUID du modèle acoustique personnalisé avec le paramètre `acoustic_customization_id` et le GUID du modèle de langue personnalisé avec le paramètre `language_customization_id`. Les deux modèles doivent appartenir aux données d'identification de service transmises avec la demande et ils doivent tous deux être basés sur le même modèle de base (par exemple, `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

Pour une demande HTTP asynchrone, vous spécifiez les paramètres lors de la création du travail asynchrone. Pour WebSockets, vous transmettez les paramètres lorsque vous établissez la connexion.
