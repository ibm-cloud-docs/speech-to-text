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

# Fonctions d'entrée
{: #input}

Le service {{site.data.keyword.speechtotextfull}} offre les fonctions suivantes pour spécifier comment le service doit effectuer une demande de reconnaissance vocale. Tous les paramètres d'entrée décrits dans les sections suivantes sont facultatifs. Seule l'entrée audio est obligatoire.
{: shortdesc}

-   Pour obtenir des exemples de demandes de reconnaissance vocale simples pour chacune des interfaces du service, voir [Effectuer une demande de reconnaissance](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Pour obtenir une liste alphabétique de tous les paramètres de reconnaissance vocale disponibles, y compris leur statut (GA ou bêta) et les langues prises en charge, voir [Récapitulatif des paramètres](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Modèles personnalisés
{: #custom-input}

La personnalisation de modèle de langue et de modèle acoustique est disponible à différents niveaux de support (GA ou bêta) pour les différentes langues. Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

Toutes les interfaces acceptent l'utilisation d'un modèle personnalisé dans une demande de reconnaissance :

-   Les *modèles de langue personnalisés* développent le vocabulaire de base du service avec la terminologie propre à des domaines spécifiques. Utilisez le paramètre `language_customization_id` pour inclure un modèle de langue personnalisé avec une demande. Vous pouvez également spécifier le paramètre facultatif `customization_weight`. Ce paramètre indique le poids relatif attribué aux mots du modèle personnalisé par opposition aux mots du vocabulaire de base.

Pour plus d'informations, voir [Utilisation d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageUse).
-   Les *modèles acoustiques personnalisés* adaptent le modèle acoustique de base du service aux caractéristiques acoustiques de votre environnement et de vos locuteurs. Utilisez le paramètre `acoustic_customization_id` pour inclure un modèle acoustique personnalisé avec une demande. Vous pouvez spécifier un modèle de langue personnalisé et un modèle acoustique personnalisé avec une demande.

    Pour plus d'informations, voir [Utilisation d'un modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acousticUse).

Les modèles personnalisés sont basés sur les modèles de langue présentés à la rubrique [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models). Un modèle personnalisé ne peut être utilisé qu'avec le modèle de base pour lequel il est créé. Si votre modèle personnalisé n'est pas basé sur le modèle par défaut `en-US_BroadbandModel`, vous devez également spécifier le nom du modèle avec la demande. Pour utiliser un modèle personnalisé, vous devez émettre la demande avec les données d'identification de l'instance du service propriétaire de ce modèle.

Pour obtenir une introduction à la personnalisation, voir [L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization).

### Exemples de modèles personnalisés
{: #customExample}

L'exemple de demande suivant comprend le paramètre `language_customization_id` permettant d'utiliser le modèle de langue personnalisé avec l'ID spécifié. Il inclut le paramètre `customization_weight` pour indiquer les mots du modèle personnalisés auxquels attribuer un poids relatif correspondant à `0.5`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

L'exemple de demande suivant utilise à la fois un modèle de langue personnalisé et un modèle acoustique personnalisé. Le premier modèle est identifié par le paramètre `language_customization_id`, le second par le paramètre `acoustic_customization_id`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

Pour obtenir des exemples utilisant des modèles personnalisés avec chacune des interfaces du service, voir

-   [Utilisation d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
-   [Utilisation d'un modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## Grammaires
{: #grammars-input}

La fonction correspondant aux grammaires est une fonctionnalité bêta. Le service prend en charge les grammaires pour toutes les langues compatibles avec la personnalisation de modèle de langue.
{: note}

Vous pouvez ajouter des grammaires à un modèle de langue personnalisé et les utiliser pour la reconnaissance vocale. Les grammaires utilisent une spécification linguistique formelle pour définir un ensemble de règles de production pour transcrire les chaînes. Ces règles spécifient comment former des chaînes valides à partir de l'alphabet de la langue concernée.

Lorsque vous utilisez une grammaire pour la reconnaissance vocale, le service reconnaît uniquement les expressions reconnues par la grammaire. En limitant l'espace de recherche aux chaînes valides, le service peut délivrer des résultats plus rapides et plus précis.

Toutes les interfaces acceptent les paramètres suivants dans le cadre d'une demande de reconnaissance :

-   Le paramètre `language_customization_id` identifie le modèle de langue personnalisé pour lequel est définie la grammaire. Vous devez émettre la demande avec les données d'identification de l'instance du service propriétaire du modèle.
-   Le paramètre `grammar_name` spécifie la grammaire que vous souhaitez utiliser. Vous ne pouvez spécifier qu'une seule grammaire avec la demande.

Pour plus d'informations, voir [Utilisation de grammaires avec des modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-grammars).

### Exemples de grammaires
{: #grammarsExample}

L'exemple de demande suivante comprend les paramètres `language_customization_id` et `grammar_name` pour limiter la réponse du service aux chaînes définies dans la grammaire nommée `list-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

Pour obtenir des exemples utilisant des grammaires avec chacune des interfaces du service, voir [Utilisation d'une grammaire pour la reconnaissance vocale](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

## Version du modèle de base
{: #version}

Afin d'améliorer la qualité de la reconnaissance vocale, le service met occasionnellement à jour les modèles de langue de base décrits dans la rubrique [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models). Les modèles de base des différentes langues sont indépendants les uns des autres, tout comme les modèles à large bande et à bande étroite correspondant à une langue. Par conséquent, les mises à jour des modèles de base se produisent indépendamment les unes des autres et n'ont aucun effet sur d'autres modèles.

Lorsqu'il existe plusieurs versions d'un modèle de base, le paramètre facultatif `base_model_version` spécifie la version du modèle à utiliser avec une demande de reconnaissance. Ce paramètre est principalement conçu pour être utilisé avec des modèles personnalisés mis à jour pour un nouveau modèle de base mais il peut être utilisé sans modèle personnalisé. La version d'un modèle de base utilisé pour une demande dépend de la transmission du paramètre `base_model_version`. Elle dépend également de la spécification d'un modèle personnalisé (modèle de langue et/ou modèle acoustique) avec la demande.

-   *Si vous n'indiquez pas de modèle personnalisé avec la demande*

    -   Omettez le paramètre `base_model_version` pour utiliser la dernière version du modèle de base.
    -   Spécifiez le paramètre `base_model_version` pour utiliser une version spécifique du modèle de base.

-   *Si vous spécifiez un modèle personnalisé avec la demande*

    -   Omettez le paramètre `base_model_version` pour utiliser la dernière version du modèle de base vers lequel le modèle personnalisé est mis à niveau. Par exemple, si le modèle personnalisé est mis à niveau à la dernière version du modèle de base, le service utilise les versions les plus récentes du modèle de base et du modèle personnalisé.
    -   Spécifiez le paramètre `base_model_version` pour utiliser les versions spécifiées du modèle de base et des modèles personnalisés.

Ce paramètre est conçu pour être utilisé avec les modèles personnalisés. Par conséquent, vous pouvez découvrir les versions disponibles d'un modèle de base uniquement en répertoriant les informations sur un modèle personnalisé basé sur ce modèle de base.

-   Pour plus d'informations sur la mise à niveau des modèles personnalisés, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Pour plus d'informations sur l'utilisation de différentes versions de modèle de base et de modèles personnalisés, voir [Demandes de reconnaissance effectuées avec des modèles personnalisés mis à niveau](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition).

## Transmission audio
{: #transmission}

*Avec l'interface WebSocket,* les données audio sont diffusées au service via la connexion. Vous pouvez transmettre toutes les données en même temps ou transmettre les données en direct au fur et à mesure de leur disponibilité. Le service renvoie les résultats dès qu'ils sont disponibles.

*Avec les interfaces HTTP,* vous pouvez transmettre des données audio au service des deux manières suivantes :

-   *Diffusion ponctuelle* - Vous omettez l'en-tête `Transfer-Encoding` et transmettez toutes les données audio au service en une seule fois sous forme de diffusion unique.
-   *Diffusion en continu* - Vous définissez l'en-tête de la demande `Transfer-Encoding` avec la valeur `chunked` et diffusez les données en continu via une connexion permanente. Les données n'ont pas besoin d'exister en totalité lorsque vous les diffusez en continu vers le service. Vous pouvez transmettre les données au fur et à mesure de leur disponibilité. Le service n'envoie les résultats qu'une fois qu'il a reçu le bloc final, ce que vous pouvez indiquer en envoyant un bloc vide.

    Pour plus d'informations sur la diffusion en continu de données audio mémorisées en blocs avec l'en-tête `Transfer-Encoding`, voir
    -   [Codage de transfert segmenté](https://wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   [Transfer Codings](https://tools.ietf.org/html/rfc7230#section-4){: external} dans le document *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*

Avec les interfaces HTTP, le service transcrit toujours le flux audio complet avant d'envoyer les résultats. Les résultats peuvent comprendre plusieurs éléments `transcript` pour indiquer que les expressions sont séparées par des pauses. Concaténez ces éléments `transcript` pour assembler la retranscription complète.

Le service impose des délais d'attente pour une session de diffusion en continu. Il peut mettre fin à une session de ce type s'il détecte une période de silence prolongée ou s'il ne reçoit aucune donnée audio pendant une durée de 30 secondes. Pour plus d'informations sur les délais d'attente et savoir comment les éviter, voir [Délais d'attente](#timeouts).

### Exemple de transmission audio
{: #transmissionExample}

L'exemple de demande suivant indique `chunked` pour que l'en-tête `Transfer-Encoding` utilise le mode de diffusion en continu. La connexion reste ouverte pour accepter d'autres blocs audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Délais d'attente
{: #timeouts}

Lorsque vous initiez une session de diffusion en continu avec les méthodes de reconnaissance vocale HTTP ou WebSocket, le service met en place des délais d'attente d'inactivité et des délais d'attente de session. Lorsqu'un délai d'attente expire lors d'une session de diffusion en continu, le service ferme la connexion. Votre application doit être restaurée normalement après des fermetures de connexion éventuelles.

Lorsque vous diffusez des données audio en continu via HTTP, le service envoie un caractère espace dans sa réponse toutes les 20 secondes. Le service agit ainsi pour faciliter le fonctionnement en évitant l'application du délai d'attente d'inactivité de l'API REST HTTP fixé à 30 secondes. Pour conserver la connexion active pendant que la reconnaissance est en cours, le service continue à envoyer ce caractère espace jusqu'à la fin de sa transcription. Le caractère espace n'a aucun effet sur les données de réponse codées en JSON.

Le délai d'attente d'inactivité HTTP est différent du délai d'attente d'inactivité du service. L'interface WebSocket n'est pas soumise à ce délai d'attente HTTP.
{: note}

### Délai d'attente d'inactivité
{: #timeouts-inactivity}

L'expiration du *délai d'attente d'inactivité* (code de statut HTTP 400) se produit lorsque le service reçoit des données audio mais ne détecte qu'un silence prolongé ou des activités non vocales (sans parole) pendant une durée de 30 secondes. Le service envoie le message d'erreur `No speech detected for 30s`. Le délai d'attente d'inactivité s'avère utile, par exemple, pour mettre fin à une session lorsqu'un utilisateur s'éloigne d'un micro.

Le délai d'attente d'inactivité par défaut est fixé à 30 secondes. Vous pouvez remplacer cette valeur en utilisant le paramètre `inactivity_timeout`. Indiquez une valeur plus élevée pour augmenter le délai d'attente d'inactivité. Indiquez la valeur `-1` pour définir un délai d'attente d'inactivité illimité. Vous êtes facturé pour toutes les données audio que vous envoyez au service, y compris le silence, par conséquent augmenter le délai d'attente d'inactivité peut induire des coûts supplémentaires pour une session de diffusion en continu qui n'enverrait que du silence.

L'exemple de demande suivant définit le délai d'attente d'inactivité à 60 secondes. La demande envoie un fichier initial pour commencer la session de diffusion en continu.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### Délai d'attente de session
{: #timeouts-session}

Une *expiration du délai d'attente de session* (code de statut HTTP 408) se produit lorsque vous n'envoyez pas suffisamment de données audio pour garder une session de diffusion en continu active. Le service peut considérer une session comme étant inactive et déclencher une expiration du délai d'attente de session pour les raisons suivantes :

-   Vous n'envoyez pas au moins 15 secondes de données audio au service dans un créneau de 30 secondes.

    Tant que vous n'avez pas envoyé le dernier tronçon pour indiquer la fin du flux, vous devez envoyer au moins 15 secondes de données audio toutes les 30 secondes. Ces données audio peuvent comporter du silence si vous avez défini le paramètre `inactivity_timeout` avec une valeur supérieure ou avec `-1`. Vous êtes facturé pour la durée des données audio que vous envoyez au service, y compris le silence.
-   Vous transmettez des données audio à une fréquence beaucoup plus lente qu'en temps réel.

    Idéalement, vous allez initier une demande pour établir une session juste avant d'obtenir les données audio à transcrire. Vous conservez alors la session en envoyant des données audio à une fréquence proche du temps réel.

Vous n'avez pas besoin de vous soucier du délai d'attente de session après avoir envoyé le dernier tronçon pour indiquer la fin du flux audio. Le service continue à traiter les données audio jusqu'à ce qu'il renvoie les résultats de la transcription finale.

Lorsque vous transcrivez un flux audio long, le service peut prendre plus de 30 secondes pour traiter les données audio et générer une réponse. Il ne commence à calculer le délai d'attente de session qu'après avoir terminé le traitement de toutes les données audio qu'il a reçues. Le temps de traitement du service ne peut pas entraîner le dépassement du délai d'attente de session fixé à 30 secondes.

Par exemple, si vous effectuez une heure de transmission audio dans les 10 premières secondes d'une session, le service peut nécessiter 300 secondes pour traiter les données audio.  Pour conserver cette session active, il vous faudra rajouter au moins 15 secondes d'audio supplémentaires, y compris du silence, au plus tard 340 secondes dans la session.

Dans cet exemple, si vous envoyez 15 secondes d'audio supplémentaires à la 100e seconde de la session, le service peut passer deux secondes de plus à traiter ces données audio. Dans ce cas, vous devrez transmettre 15 secondes d'audio supplémentaires au plus tard 342 secondes dans la session.

Ne vous fiez pas au temps de traitement ou à la réception des résultats pour déterminer si une session de diffusion en continu est inactive. Considérez que le service peut traiter toutes les données audio instantanément, et envoyez les données au service en conséquence. Si vous diffusez les données audio en temps réel, n'oubliez pas d'envoyer les données audio à la moitié du temps réel (15 secondes d'audio) par créneau de 30 secondes. Cette fréquence est en général suffisante pour tenir compte des temps d'attente de réseau et des retards.
{: important}

## Journalisation des demandes
{: #logging}

Par défaut, {{site.data.keyword.IBM_notm}} consigne toutes les demandes adressées aux services {{site.data.keyword.watson}} et leurs résultats. La journalisation a pour unique but d'améliorer les services pour les futurs utilisateurs. Les données de journal ne sont ni partagées ni rendues publiques.

Si vous tenez à la confidentialité des informations personnelles des utilisateurs ou si vous ne souhaitez pas que vos demandes soient consignées par IBM, vous pouvez refuser la journalisation des données par IBM (en la désactivant). Vous pouvez choisir de désactiver la journalisation au niveau du compte ou au niveau de la demande d'API. Pour plus d'informations, voir [Contrôle de la journalisation des demandes pour les services {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-logging-overview).

## Sécurité des informations
{: #security-input}

La sécurité des informations comprend des fonctions permettant d'associer un ID client aux données transmises au service dans le cadre d'une demande. Vous associez un ID client aux données en transmettant l'en-tête `X-Watson-Metadata` avec la demande. Si nécessaire, vous pouvez ensuite supprimer les données en utilisant la méthode `DELETE /v1/user_data`. Pour plus d'informations, voir [Sécurité des informations](/docs/services/speech-to-text?topic=speech-to-text-information-security).
