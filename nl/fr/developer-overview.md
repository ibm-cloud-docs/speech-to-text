---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-13"

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

# Présentation destinée aux développeurs
{: #developerOverview}

Vous pouvez accéder aux fonctions du service {{site.data.keyword.speechtotextfull}} en utilisant une interface WebSocket et des interfaces HTTP REST (Representational State Transfer) synchrones ou asynchrones. Vous pouvez également personnaliser les modèles de langue du service pour les adapter à votre domaine et à votre environnement. Des logiciels SDK sont disponibles pour simplifier le développement d'application dans de nombreux langages de programmation.
{: shortdesc}

## Programmation avec le service
{: #programming}

La rubrique [Effectuer une demande de reconnaissance](/docs/services/speech-to-text?topic=speech-to-text-basic-request) vous montre comment solliciter une transcription de base avec les différentes interfaces de programmation du service :

-   [L'interface WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets) offre une implémentation efficace, à faible temps d'attente et à haut débit via une connexion en duplex intégral.
-   [L'interface HTTP synchrone](/docs/services/speech-to-text?topic=speech-to-text-http) fournit une interface de base pour la transcription audio avec des demandes bloquantes.
-   [L'interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async) fournit une interface non bloquante qui vous laisse enregistrer une URL de rappel permettant de recevoir des notifications ou d'interroger le service pour obtenir le statut et les résultats d'un travail.

Ces interfaces offrent les mêmes fonctions de reconnaissance vocale, mais vous pouvez spécifier le même paramètre en tant qu'en-tête de demande, paramètre de requête ou paramètre d'un objet JSON en fonction de l'interface que vous utilisez. Par ailleurs, les interfaces WebSocket et HTTP synchrone acceptent jusqu'à 100 Mo de données audio maximum avec une seule demande. L'interface HTTP asynchrone accepte jusqu'à 1 Go de données audio maximum.

-   Pour obtenir la description de tous les paramètres de reconnaissance vocale disponibles, voir [Récapitulatif des paramètres](/docs/services/speech-to-text?topic=speech-to-text-summary).
-   Pour obtenir la description de toutes les méthodes et de leurs paramètres, ainsi que des exemples, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

## Avantages de l'interface WebSocket
{: #advantages}

L'interface WebSocket comporte un certain nombre d'avantages par rapport à l'interface HTTP :

-   L'interface WebSocket fournit un canal de communication en duplex intégral sur un seul socket. L'interface laisse le client envoyer des demandes et des données audio au service et recevoir les résultats via une connexion unique de manière asynchrone.
-   Elle offre une expérience de programmation beaucoup plus simple et encore plus efficace. Le service envoie des réponses basées sur des événements dans les messages du client, évitant ainsi au client d'avoir à interroger le serveur.
-   Elle vous permet d'établir et d'utiliser une connexion unique authentifiée indéfiniment. Les interfaces HTTP nécessitent que vous authentifiez chaque appel au service.
-   Elle réduit les temps d'attente. Les résultats de la reconnaissance arrivent plus rapidement car le service les envoie directement au client. L'interface HTTP nécessite quatre demandes et connexions distinctes pour parvenir au même résultat.
-   Elle réduit l'utilisation du réseau. WebSocket est un protocole léger. Il lui suffit d'une seule connexion pour effectuer une reconnaissance vocale en direct.
-   Elle permet la diffusion en continu des données audio au service directement à partir des navigateurs (clients WebSocket HTML5).

## Personnalisation du service
{: #customizing}

[L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization) vous permet de créer des modèles personnalisés pour améliorer les fonctions de reconnaissance vocale du service :

-   [Les modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-languageCreate) vous permettent de définir les mots spécifiques à un domaine pour un modèle de base. Les modèles de langue personnalisés enrichissent le vocabulaire de base du service avec la terminologie spécifique à des domaines, tels que la médecine ou le droit.
-   [Les modèles acoustiques personnalisés](/docs/services/speech-to-text?topic=speech-to-text-acoustic) vous permettent d'adapter un modèle de base aux caractéristiques acoustiques de votre environnement et de vos locuteurs. Les modèles acoustiques personnalisés améliorent la capacité du service à reconnaître la parole en tenant compte de caractéristiques acoustiques spécifiques.
-   [Les grammaires](/docs/services/speech-to-text?topic=speech-to-text-grammars) vous permettent de restreindre les expressions que le service peut reconnaître à celles définies dans les règles de grammaire. En limitant l'espace de recherche aux chaînes valides, le service peut délivrer des résultats plus rapides et plus précis. Les grammaires sont prises en charge avec les modèles de langue personnalisés.

Vous pouvez utiliser un modèle de langue personnalisé, un modèle acoustique personnalisé ou ces deux types de modèle pour la reconnaissance vocale avec n'importe quelle interface du service.

Vous devez disposer du plan de tarification standard pour pouvoir utiliser la personnalisation du modèle de langue ou du modèle acoustique. Les utilisateurs du plan Lite ne peuvent pas utiliser l'interface de personnalisation. Pour plus d'informations, voir la [page de tarification](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} du service {{site.data.keyword.speechtotextshort}}.
{: note}

## Obtention de mesures
{: #overview-metrics}

Le service offre deux types de mesures facultatives pour les demandes de reconnaissance vocale :

-   Les [mesures de traitement](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) fournissent des informations de synchronisation détaillées sur l'analyse du service de l'entrée audio. Le service renvoie les mesures à des intervalles spécifiés et avec des événements de transcription, par exemple des résultats temporaires et finaux. Vous pouvez utiliser les mesures pour évaluer la progression du service de transcription du contenu audio.
-   Les [mesures audio](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics) fournissent des informations détaillées sur les caractéristiques de signal de l'entrée audio. Les résultats fournissent des mesures globales pour l'ensemble de l'entrée audio à la fin du traitement de la parole. Vous pouvez utiliser les mesures pour déterminer les caractéristiques et la qualité du contenu audio.

Vous pouvez demander des mesures de traitement avec les interfaces HTTP asynchrones et WebSocket. Vous pouvez demander des mesures audio avec n'importe quelle interface du service. Par défaut, le service ne renvoie aucune mesure.

## Prise en charge de CORS
{: #cors}

Le service prend en charge le partage de ressources d'origine croisée (CORS). Avec CORS, les pages Web peuvent directement demander des ressources en provenance d'un domaine externe. CORS permet de contourner les règles de sécurité d'origine identique, qui empêcheraient les demandes de ce type. Comme le service prend en charge CORS, une page Web peut communiquer directement avec le service sans avoir à passer par le serveur Web qui héberge la page pour transmettre la demande.

Par exemple, une page Web chargée depuis un serveur dans {{site.data.keyword.cloud}} peut appeler directement l'API de personnalisation, en ignorant le serveur {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [enable-cors.org](https://enable-cors.org/){: external}.

## Utilisation de logiciels SDK
{: #sdks}

Des logiciels SDK sont disponibles pour le service {{site.data.keyword.speechtotextshort}} afin de simplifier le développement des applications vocales. Les logiciels {{site.data.keyword.ibmwatson}} SDK sont disponibles pour de nombreux langages de programmation et plateformes populaires.

-   Pour obtenir une liste complète de logiciels SDK et de liens vers les logiciels SDK sur GitHub, voir [Utilisation des logiciels SDK](/docs/services/watson?topic=watson-using-sdks).
-   Pour obtenir des informations détaillées sur toutes les méthodes de SDK Node, Java&trade;, Python, Ruby, Swift et Go pour le service {{site.data.keyword.speechtotextshort}}, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

## En savoir plus sur le développement d'application
{: #learn}

Pour plus d'informations sur l'utilisation des services {{site.data.keyword.watson}} et {{site.data.keyword.cloud_notm}}, consultez les références suivantes :

-   Pour une introduction à l'utilisation des services {{site.data.keyword.watson}} et {{site.data.keyword.cloud_notm}}, voir [Initiation à {{site.data.keyword.watson}} et {{site.data.keyword.cloud_notm}}](/docs/services/watson?topic=watson-about).
-   Toutes les nouvelles instances de service utilisent l'authentification {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Les instances de service antérieures peuvent continuer à utiliser le nom d'utilisateur `{username}` et le mot de passe `{password}` de leurs données d'identification de service Cloud Foundry pour l'authentification. Pour plus d'informations sur l'authentification auprès du service, voir la [mise à jour du service du 30 octobre 2018](/docs/services/speech-to-text?topic=speech-to-text-release-notes#October2018b) dans les notes sur l'édition.
-   Pour plus d'informations sur le contrôle de la journalisation des demandes par défaut effectuée pour tous les services {{site.data.keyword.watson}}, voir [Contrôle de la journalisation des demandes pour les services {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-logging-overview).
