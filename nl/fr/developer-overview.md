---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# Présentation destinée aux développeurs
{: #developerOverview}

Vous pouvez accéder aux fonctions du service {{site.data.keyword.speechtotextfull}} en utilisant une interface WebSocket et des interfaces HTTP REST (Representational State Transfer) synchrones ou asynchrones. Vous pouvez également personnaliser les modèles de langue du service pour les adapter à votre domaine et à votre environnement. Des logiciels SDK sont disponibles pour simplifier le développement d'application dans de nombreux langages de programmation.
{: shortdesc}

## Programmation avec le service
{: #programming}

La rubrique [Effectuer une demande de reconnaissance](/docs/services/speech-to-text/basic-request.html) vous montre comment solliciter une transcription de base avec les différentes interfaces de programmation du service :

-   [L'interface WebSocket](/docs/services/speech-to-text/websockets.html) offre une implémentation efficace, à faible temps d'attente et à haut débit via une connexion en duplex intégral.
-   [L'interface HTTP synchrone](/docs/services/speech-to-text/http.html) fournit une interface de base pour la transcription audio avec des demandes bloquantes.
-   [L'interface HTTP asynchrone](/docs/services/speech-to-text/async.html) fournit une interface non bloquante qui vous laisse enregistrer une URL de rappel permettant de recevoir des notifications ou d'interroger le service pour obtenir le statut et les résultats d'un travail.

Ces interfaces offrent les mêmes fonctions de reconnaissance vocale, mais vous pouvez spécifier le même paramètre en tant qu'en-tête de demande, paramètre de requête ou paramètre d'un objet JSON en fonction de l'interface que vous utilisez. Par ailleurs, les interfaces WebSocket et HTTP synchrone acceptent jusqu'à 100 Mo de données audio maximum avec une seule demande. L'interface HTTP asynchrone accepte jusqu'à 1 Go de données audio maximum.

-   Pour obtenir la description de tous les paramètres de reconnaissance vocale disponibles, voir [Récapitulatif des paramètres](/docs/services/speech-to-text/summary.html).
-   Pour obtenir la description de toutes les méthodes et de leurs paramètres, ainsi que des exemples, voir [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

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

[L'interface de personnalisation](/docs/services/speech-to-text/custom.html) vous permet de créer des modèles personnalisés pour améliorer les fonctions de reconnaissance vocale du service :

-   [Les modèles de langue personnalisés](/docs/services/speech-to-text/language-create.html) vous permettent de définir les mots spécifiques à un domaine pour un modèle de base. Les modèles de langue personnalisés enrichissent le vocabulaire de base du service avec la terminologie spécifique à des domaines, tels que la médecine ou le droit.
-   [Les modèles acoustiques personnalisés](/docs/services/speech-to-text/acoustic-create.html) vous permettent d'adapter un modèle de base aux caractéristiques acoustiques de votre environnement et de vos locuteurs. Les modèles acoustiques personnalisés améliorent la capacité du service à reconnaître la parole en tenant compte de caractéristiques acoustiques spécifiques.
-   [Les grammaires](/docs/services/speech-to-text/grammar.html) vous permettent de restreindre les expressions que le service peut reconnaître à celles définies dans les règles de grammaire. En limitant l'espace de recherche aux chaînes valides, le service peut délivrer des résultats plus rapides et plus précis. Les grammaires sont prises en charge avec les modèles de langue personnalisés.

Vous pouvez utiliser un modèle de langue personnalisé, un modèle acoustique personnalisé ou ces deux types de modèle pour la reconnaissance vocale avec n'importe quelle interface du service.

## Prise en charge de CORS
{: #cors}

Le service prend en charge le partage de ressources d'origine croisée (CORS). Avec CORS, les pages Web peuvent directement demander des ressources en provenance d'un domaine externe. CORS permet de contourner les règles de sécurité d'origine identique, qui empêcheraient les demandes de ce type. Comme le service prend en charge CORS, une page Web peut communiquer directement avec le service sans avoir à passer par le serveur Web qui héberge la page pour transmettre la demande.

Par exemple, une page Web chargée depuis un serveur dans {{site.data.keyword.cloud}} peut appeler directement l'API de personnalisation, en ignorant le serveur {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [enable-cors.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://enable-cors.org/){: new_window}.

## Utilisation de logiciels SDK
{: #sdks}

Des logiciels SDK sont disponibles pour le service {{site.data.keyword.speechtotextshort}} afin de simplifier le développement des applications vocales. Les logiciels {{site.data.keyword.ibmwatson}} SDK sont disponibles pour de nombreux langages de programmation et plateformes populaires.

-   Pour obtenir une liste complète de logiciels SDK et de liens vers les logiciels SDK sur GitHub, voir [Utilisation des logiciels SDK](/docs/services/watson/getting-started-sdks.html).
-   Pour obtenir des informations détaillées sur toutes les méthodes de Node, Java&trade;, Python, Ruby, Swift et Go SDKs pour le service {{site.data.keyword.speechtotextshort}}, voir [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## En savoir plus sur le développement d'application
{: #learn}

Pour plus d'informations sur l'utilisation des services {{site.data.keyword.watson}} et {{site.data.keyword.cloud_notm}}, consultez les références suivantes :

-   Pour une introduction à l'utilisation des services {{site.data.keyword.watson}} et {{site.data.keyword.cloud_notm}}, voir [Initiation à {{site.data.keyword.watson}} et {{site.data.keyword.cloud_notm}}](/docs/services/watson/index.html).
-   Toutes les nouvelles instances de service utilisent l'authentification {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Les instances de service antérieures peuvent continuer à utiliser le nom d'utilisateur `{username}` et le mot de passe `{password}` de leurs données d'identification de service Cloud Foundry pour l'authentification. Pour plus d'informations sur l'authentification auprès du service, voir la [mise à jour du service du 30 octobre 2018](/docs/services/speech-to-text/release-notes.html#October2018b) dans les notes sur l'édition.
-   Pour plus d'informations sur le contrôle de la journalisation des demandes par défaut effectuée pour tous les services {{site.data.keyword.watson}}, voir [Contrôle de la journalisation des demandes pour les services {{site.data.keyword.watson}}](/docs/services/watson/getting-started-logging.html).
