---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

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

# Notes sur l'édition
{: #release-notes}

Les sections suivantes présentent les nouvelles fonctions et les modifications intégrées dans chaque édition et mise à jour du service {{site.data.keyword.speechtotextfull}}. Les informations comprennent les limitations connues. Sauf indication contraire, toutes les modifications sont compatibles avec les éditions antérieures et sont disponibles automatiquement et en toute transparence pour les applications, nouvelles ou existantes.
{: shortdesc}

## Limitations connues
{: #limitations}

Aucune limitation connue à ce stade.

## 30 juillet 2019
{: #July2019}

Le service propose désormais des modèles de langue à bande large et à bande étroite dans six dialectes espagnols :

-   Espagnol d'Argentine (`es-AR_BroadbandModel` et `es-AR_NarrowbandModel`)
-   Espagnol castillan (`es-ES_BroadbandModel` et `es-ES_NarrowbandModel`)
-   Espagnol du Chili (`es-CL_BroadbandModel` et `es-CL_NarrowbandModel`)
-   Espagnol de Colombie (`es-CO_BroadbandModel` et `es-CO_NarrowbandModel`)
-   Espagnol du Mexique (`es-MX_BroadbandModel` et `es-MX_NarrowbandModel`)
-   Espagnol du Pérou (`es-PE_BroadbandModel` et `es-PE_NarrowbandModel`)

Les modèles en espagnol castillan ne sont pas nouveaux. Ils sont généralement disponibles pour la reconnaissance vocale et la personnalisation des modèles de langue et en version bêta pour la personnalisation des modèles acoustiques.

Les cinq autres dialectes sont nouveaux et en version bêta pour toutes les utilisations. Comme ils sont en version bêta, ces dialectes supplémentaires peuvent ne pas être prêts pour une utilisation en production et sont sujets à modification. Ce sont des offres initiales dont la qualité est prévue pour s'améliorer avec le temps et l'utilisation.

Pour plus d'informations, voir les sections suivantes :

-   [Modèles de langue pris en charge](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList)
-   [Support de langue pour la personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)

## 24 juin 2019
{: #June2019b}

-   Les modèles à bande étroite suivants ont été mis à jour pour une meilleure reconnaissance vocale :
    -   Modèle à bande étroite en anglais américain (`en-US_NarrowbandModel`)
    -   Modèle à bande étroite en portugais brésilien (`pt-BR_NarrowbandModel`)

    Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance vocale. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Ce service vous permet désormais de soumettre simultanément plusieurs demandes pour ajouter différentes ressources audio à un modèle acoustique personnalisé. Auparavant, le service n'autorisait qu'une seule demande à la fois pour l'ajout de contenu audio à un modèle personnalisé.
-   Les sorties des méthodes HTTP `GET` qui répertorient des informations sur les modèles de langue personnalisés et les modèles acoustiques personnalisés comprennent maintenant une zone `updated`. La zone indique la date et l'heure de la dernière modification du modèle personnalisé en temps universel coordonné (UTC).
-   Le schéma a été modifié pour un avertissement qui a été généré par une demande d'entraînement de modèle personnalisé lorsque le paramètre `strict` est défini sur `false`. Les noms des zones sont passés respectivement de `warning_id` et `description` à `code` et `message`. Pour plus d'informations, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

## 10 juin 2019
{: #June2019a}

Les [mesures de traitement](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) sont disponibles uniquement avec les interfaces HTTP asynchrones et WebSocket. Elles ne sont pas prises en charge avec l'interface HTTP synchrone.

## Editions antérieures
{: #older}

-   [17 mai 2019](#May2019b)
-   [10 mai 2019](#May2019)
-   [19 avril 2019](#April2019b)
-   [3 avril 2019](#April2019)
-   [21 mars 2019](#March2019d)
-   [15 mars 2019](#March2019c)
-   [11 mars 2019](#March2019b)
-   [4 mars 2019](#March2019)
-   [28 janvier 2019](#January2019)
-   [20 décembre 2018](#December2018b)
-   [13 décembre 2018](#December2018a)
-   [12 novembre 2018](#November2018b)
-   [7 novembre 2018](#November2018a)
-   [30 octobre 2018](#October2018b)
-   [9 octobre 2018](#October2018a)
-   [10 septembre 2018](#September2018b)
-   [7 septembre 2018](#September2018a)
-   [8 août 2018](#August2018)
-   [13 juillet 2018](#July2018)
-   [12 juin 2018](#June2018)
-   [15 mai 2018](#May2018)
-   [26 mars 2018](#March2018b)
-   [1er mars 2018](#March2018a)
-   [1er février 2018](#February2018)
-   [14 décembre 2017](#December2017)
-   [2 octobre 2017](#October2017)
-   [14 juillet 2017](#July2017b)
-   [1er juillet 2017](#July2017a)
-   [22 mai 2017](#May2017)
-   [10 avril 2017](#April2017)
-   [8 mars 2017](#March2017)
-   [1er décembre 2016](#December2016)
-   [22 septembre 2016](#September2016)
-   [30 juin 2016](#June2016b)
-   [23 juin 2016](#June2016a)
-   [10 mars 2016](#March2016)
-   [19 janvier 2016](#January2016)
-   [17 décembre 2015](#December2015)
-   [21 septembre 2015](#September2015)
-   [1er juillet 2015](#July2015)

### 17 mai 2019
{: #May2019b}

-   Le service offre désormais deux types de mesures facultatives avec des demandes de reconnaissance vocale :
    -   Les [mesures de traitement](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) fournissent des informations de synchronisation détaillées sur l'analyse du service de l'entrée audio. Le service renvoie les mesures à des intervalles spécifiés et avec des événements de transcription, par exemple des résultats temporaires et finaux. Utilisez ces mesures pour évaluer la progression du service de transcription du contenu audio.
    -   Les [mesures audio](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics) fournissent des informations détaillées sur les caractéristiques de signal de l'entrée audio. Les résultats fournissent des mesures globales pour l'ensemble de l'entrée audio à la fin du traitement de la parole. Utilisez ces mesures pour déterminer les caractéristique et la qualité du contenu audio.

    Vous pouvez demander deux types de mesures avec n'importe quelle demande de reconnaissance vocale. Par défaut, le service ne renvoie aucune mesure pour une demande.
-   Le modèle japonais à large bande (`ja-JP_BroadbandModel`) a été mis à jour pour une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement le modèle mis à jour pour toutes les demandes de reconnaissance vocale. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ce modèle, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).

### 10 mai 2019
{: #May2019}

Les modèles de langue espagnols ont été mis à jour pour une meilleure reconnaissance vocale :

-   `es-ES_BroadbandModel`
-   `es-ES_NarrowbandModel`

Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance vocale. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).

### 19 avril 2019
{: #April2019b}

-   Les méthodes d'entraînement de l'interface de personnalisation contiennent désormais un paramètre de requête `strict` qui indique si l'entraînement doit se poursuivre si un modèle personnalisé contient à la fois des ressources valides et non valides. Par défaut, l'entraînement échoue si un modèle personnalisé contient une ou plusieurs ressources non valides. Définissez le paramètre sur `false` pour permettre la poursuite de l'entraînement tant que le modèle contient au moins une ressource valide. Le service exclut les ressources non valides de l'entraînement. 
    -   Pour plus d'informations sur l'utilisation du paramètre `strict` avec la méthode `POST /v1/customizations/{customization_id}/train`, voir [Entraînement du modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language) et [Echec d'entraînement](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
    -   Pour plus d'informations sur l'utilisation du paramètre `strict` avec la méthode `POST /v1/acoustic_customizations/{customization_id}/train`, voir [Entraînement du modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic) et [Echec d'entraînement](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).
-   Vous pouvez désormais ajouter 90 mille mots OOV (Out-of-vocabulary) maximum à la ressource de mots d'un modèle de langue personnalisé. La limite maximale précédente était de 30 mille mots OOV. Ce nombre inclut les mots OOV de toutes les sources (corpus, grammaires et mots personnalisés individuels que vous ajoutez directement).Vous pouvez ajouter un total de 10 millions maximum à un modèle personnalisé à partir de toutes les sources. Pour plus d'informations, voir [De quelle quantité de données ai-je besoin ?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount).

### 3 avril 2019
{: #April2019}

Les modèles acoustiques personnalisés acceptent désormais 200 heures d'audio maximum. La limite maximale précédente était de 100 heures d'audio.

### 21 mars 2019
{: #March2019d}

Désormais, les utilisateurs ne peuvent voir que les informations liées aux données d'identification du service associées au rôle ayant été attribué à leur compte {{site.data.keyword.cloud_notm}}. Par exemple, si vous bénéficiez d'un rôle `reader`, les données d'identification d'un rôle `writer` ou supérieur, ne sont plus visibles.

Cette modification n'affecte pas l'accès à l'API des utilisateurs ou des applications ayant déjà des données d'identification. Elle affecte uniquement l'affichage des données d'identification au sein d'{{site.data.keyword.cloud_notm}}.

Pour plus d'informations sur les clés de service et les rôles utilisateur, voir [Clés d'API de service IAM](/docs/services/watson?topic=watson-api-key-bp#api-key-bp).

### 15 mars 2019
{: #March2019c}

Le service prend désormais en charge le format audio A-law (`audio/alaw`). Pour plus d'informations, voir [format audio/alaw](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#alaw).

### 11 mars 2019
{: #March2019b}

-   Pour le paramètre `max_alternatives`, le service accepte à nouveau la valeur `0`. Si vous indiquez `0`, le service utilise automatiquement la valeur par défaut, `1`. Une modification effectuée à l'occasion de la mise à jour du service publiée le 4 mars provoquait une erreur lorsque la valeur `0` était utilisée. (Le service renvoie une erreur si vous spécifiez une valeur négative.)
-   Pour le paramètre `word_alternatives_threshold`, le service accepte à nouveau la valeur `0` . Une modification effectuée à l'occasion de la mise à jour du service publiée le 4 mars provoquait une erreur lorsque la valeur `0` était utilisée. (Le service renvoie une erreur si vous spécifiez une valeur négative.)
-   Le service renvoie désormais toutes les cotes de confiance avec une précision maximale de deux décimales, ce qui inclut les cotes de confiance pour les fonctions de retranscriptions (transcripts), la confiance des mots (word confidence), les autres propositions de mots (word alternatives), les résultats par mot-clé (keyword results) et les étiquettes de locuteurs (speaker labels).

### 4 mars 2019
{: #March2019}

Les modèles de langue à bande étroite ont été mis à jour pour une meilleure reconnaissance vocale :

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance vocale. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).

### 28 janvier 2019
{: #January2019}

L'interface WebSocket prend désormais en charge l'authentification Identity and Access Management (IAM) à base de jeton à partir d'un code JavaScript exécuté dans le navigateur. La limitation en sens contraire a été supprimée. Afin d'établir une connexion authentifiée avec la méthode de l'interface WebSocket `/v1/recognize` :

-   Si vous utilisez l'authentification IAM, incluez le paramètre de requête `access_token`.
-   Si vous utilisez des données d'identification de service Cloud Foundry, incluez le paramètre de requête `watson-token`.

Pour plus d'informations, voir [Ouverture d'une connexion](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen).

### 20 décembre 2018
{: #December2018b}

L'interface grammaticale est entièrement fonctionnelle partout depuis le 8 janvier 2019.
{: note}

-   Le service fournit désormais des grammaires pour la reconnaissance vocale. Ces grammaires sont disponibles en fonctionnalité bêta pour toutes les langues qui prennent en charge la personnalisation des modèles de langue. Vous pouvez les ajouter à un modèle de langue personnalisé et les utiliser pour limiter la série d'expressions reconnaissables par le service à partir d'une source audio. Vous pouvez définir une grammaire au format ABNF (Augmented Backus-Naur Form) ou XML.

    Les quatre méthodes suivantes sont disponibles pour utiliser des grammaires :
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` ajoute un fichier de grammaire à un modèle de langue personnalisé.
    -   `GET /v1/customizations/{customization_id}/grammars ` répertorie les informations sur toutes les grammaires d'un modèle personnalisé.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` renvoie des informations sur une grammaire spécifiée d'un modèle personnalisé.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` retire une grammaire existante d'un modèle personnalisé.

    Vous pouvez utiliser une grammaire pour la reconnaissance vocale avec les interfaces WebSocket et HTTP. Utilisez les paramètres `language_customization_id` et `grammar_name` pour identifier le modèle personnalisé et la grammaire que vous désirez utiliser. Actuellement, vous ne pouvez utiliser qu'une seule grammaire avec une demande de reconnaissance vocale.

    Pour plus d'informations sur les grammaires, voir la documentation suivante :
    -   [Utilisation de grammaires avec des modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-grammars)
    -   [Description des grammaires](/docs/services/speech-to-text?topic=speech-to-text-grammarUnderstand)
    -   [Ajout d'une grammaire à un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
    -   [Utilisation d'une grammaire pour la reconnaissance vocale](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)
    -   [Gestion des grammaires](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars)
    -   [Exemples de grammaires](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples)

    Pour plus d'informations sur toutes les méthodes de l'interface, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
-   Une nouvelle fonction d'occultation numérique est maintenant disponible pour masquer les numéros à trois chiffres successifs ou plus. L'occultation est conçue pour supprimer des informations personnelles sensibles, comme les numéros de carte de crédit, des retranscriptions. Vous activez cette fonction en définissant le paramètre `redaction` avec la valeur `true` lors d'une demande de reconnaissance. Cette fonction est une fonctionnalité bêta disponible uniquement pour l'anglais américain, le japonais et le coréen. Pour plus d'informations, voir [Occultation numérique](/docs/services/speech-to-text?topic=speech-to-text-output#redaction).
-   Les nouveaux modèles de langue allemand et français suivants sont désormais disponibles avec le service :
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    Ces deux nouveaux modèles prennent en charge la personnalisation des modèles de langue (GA) et la personnalisation des modèles acoustiques (bêta). Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
-   Un nouveau modèle de langue anglais-américain, `en-US_ShortForm_NarrowbandModel`, est actuellement disponible. Le nouveau modèle est conçu pour être utilisé avec les solutions de serveur vocal interactif et d'automatisation du service clients. Le modèle prend en charge la personnalisation des modèles de langue (GA) et la personnalisation des modèles acoustiques (bêta). Pour plus d'informations, voir [Le modèle anglais américain simplifié](/docs/services/speech-to-text?topic=speech-to-text-models#modelsShortform).
-   Les modèles de langue suivants ont été mis à jour pour une meilleure reconnaissance vocale :
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance vocale. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Le service prend en charge actuellement l'audio au format G.729 (`audio/g729`). Il prend en charge uniquement le format G.729 Annex D pour l'audio à bande étroite. Pour plus d'informations sur les formats audio pris en charge, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   La fonction d'étiquettes de locuteur (speaker labels) est maintenant disponible pour le modèle anglais britannique à bande étroite (`en-GB_NarrowbandModel`). Cette fonction est une fonctionnalité bêta pour toutes les langues prises en charge. Pour plus d'informations, voir [Etiquettes de locuteur](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   La durée audio maximale que vous pouvez ajouter à un modèle acoustique personnalisé a augmenté pour passer de 50 à 100 heures.

### 13 décembre 2018
{: #December2018a}

Le service est désormais disponible à l'emplacement {{site.data.keyword.cloud_notm}} correspondant à Londres (**eu-gb**). Comme tous les emplacements, Londres a recours à l'authentification IAM à base de jeton. Toutes les nouvelles instances de service que vous créez à cet emplacement utilisent l'authentification IAM.

### 12 novembre 2018
{: #November2018b}

Le service prend désormais en charge le formatage intelligent pour la reconnaissance vocale en japonais. Auparavant, seuls l'anglais américain et l'espagnol étaient pris en charge pour ce type de formatage. Cette fonction est une fonctionnalité bêta pour toutes les langues prises en charge. Pour plus d'informations, voir [Formatage intelligent](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).

### 7 novembre 2018
{: #November2018a}

Le service est désormais disponible à l'emplacement {{site.data.keyword.cloud_notm}} correspondant à Tokyo (**jp-tok**). Comme tous les emplacements, Tokyo a recours à l'authentification IAM à base de jeton. Toutes les nouvelles instances de service que vous créez à cet emplacement utilisent l'authentification IAM.

### 30 octobre 2018
{: #October2018b}

Le service a migré vers l'authentification IAM à base de jeton pour tous les emplacements. Tous les services {{site.data.keyword.cloud_notm}} utilisent à présent l'authentification IAM. Le service {{site.data.keyword.speechtotextshort}} a migré aux dates suivantes pour chaque emplacement :

-   Dallas (**us-south**) : 30 octobre 2018
-   Francfort (**eu-de**) : 30 octobre 2018
-   Washington, DC (**us-east**) : 12 juin 2018
-   Sydney (**au-syd**) : 15 mai 2018

La migration vers l'authentification IAM affecte différemment les instances de service nouvelles et existantes :

-   *Toutes les nouvelles instances de service que vous créez à un emplacement* utilisent désormais l'authentification IAM pour accéder au service. Vous pouvez transmettre un jeton bearer ou une clé d'API : les jetons prennent en charge les demandes sans intégrer les données d'identification du service dans chaque appel. Les clés d'API utilisent l'authentification de base HTTP. Lorsque vous utilisez l'un des logiciels SDK de {{site.data.keyword.watson}}, vous pouvez transmettre la clé d'API et laisser le SDK gérer le cycle de vie des jetons.
-   *Les instances de service existantes que vous avez créées à un emplacement avant la date de migration indiquée* utilisent toujours le nom d'utilisateur `{username}` et le mot de passe `{password}` de leurs données d'identification de service Cloud Foundry précédentes pour l'authentification jusqu'à leur migration pour utiliser l'authentification IAM. Pour plus d'informations sur la migration vers l'authentification IAM, voir [Migration des instances de service Cloud Foundry vers un groupe de ressources](https://{DomainName}/docs/resources?topic=resources-migrate).

Pour plus d'informations, voir la documentation suivante :

-   Pour connaître le mécanisme d'authentification utilisé par votre instance de service, affichez vos données d'identification de service en cliquant sur l'instance dans le [tableau de bord {{site.data.keyword.cloud_notm}}](https://{DomainName}/dashboard/apps){: external}.
-   Pour plus d'informations sur l'utilisation de jetons IAM avec des services Watson, voir [Authentification avec des jetons IAM](/docs/services/watson?topic=watson-iam).
-   Pour plus d'informations sur l'utilisation de clés d'API IAM avec des services Watson, voir [Clés d'API de service IAM](/docs/services/watson?topic=watson-api-key-bp).
-   Pour consulter des exemples utilisant l'authentification IAM, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

### 9 octobre 2018
{: #October2018a}

-   L'en-tête `Content-Type` est à présent facultatif pour les demandes de reconnaissance vocale. Le service détecte automatiquement le format audio (type MIME) de la plupart des sources audio. Vous devez continuer à indiquer le type de contenu pour les formats suivants :
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Dans les cas indiqués, le type de contenu que vous spécifiez pour ces formats doit inclure la fréquence d'échantillonnage et peut éventuellement inclure le nombre de canaux et l'ordre d'octets de l'audio. Pour tous les autres formats audio, vous pouvez omettre le type de contenu ou indiquer le type de contenu `application/octet-stream` pour que le service détecte automatiquement le format.

    Lorsque vous utilisez la commande `curl` pour effectuer une demande de reconnaissance vocale avec l'interface HTTP, vous devez spécifier le format audio avec l'en-tête `Content-Type`, indiquer `"Content-Type: application/octet-stream"` ou `"Content-Type:"`. Si vous omettez complètement l'en-tête, la commande `curl` utilise la valeur par défaut `application/x-www-form-urlencoded`. La plupart des exemples dans cette documentation continuent à spécifier le format des demandes de reconnaissance vocale même si ce n'est pas obligatoire.
    {: important}

    Cette modification s'applique aux méthodes suivantes :
    -   `/v1/recognize` pour les demandes WebSocket. La zone `content-type` du message texte que vous envoyez pour initier une demande sur une connexion WebSocket ouverte est désormais facultative.
    -   `POST /v1/recognize` pour les demandes HTTP synchrones. L'en-tête `Content-Type` est désormais facultatif. (Pour les demandes multiparties, la zone `part_content_type` des métadonnées JSON est désormais également facultative.)
    -   `POST /v1/recognitions` pour les demandes HTTP asynchrones. L'en-tête `Content-Type` est désormais facultatif.

    Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Le modèle à large bande portugais brésilien, `pt-BR_BroadbandModel`, a été mis à jour pour une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement le modèle mis à jour pour toutes les demandes de reconnaissance. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ce modèle, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Le paramètre `customization_id` des méthodes de reconnaissance vocale est obsolète et sera retiré dans une édition ultérieure. Pour spécifier un modèle de langue personnalisé pour une demande de reconnaissance vocale, utilisez à la place le paramètre `language_customization_id`. Cette modification s'applique aux méthodes suivantes :
    -   `/v1/recognize` pour les demandes WebSocket
    -   `POST /v1/recognize` pour les demandes HTTP synchrones (notamment les demandes multiparties)
    -   `POST /v1/recognitions` pour les demandes HTTP asynchrones
-   A partir du 1er octobre 2018, vous êtes facturé pour tout l'audio que vous transmettez au service pour la reconnaissance vocale. Les mille premières minutes d'audio que vous envoyez chaque mois ne sont plus gratuites. Pour plus d'informations sur les plans de tarification du service, voir le service {{site.data.keyword.speechtotextshort}} dans le [catalogue {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog/services/speech-to-text){: external}.

### 10 septembre 2018
{: #September2018b}

Pour obtenir la liste des erreurs qui ont été corrigées depuis l'édition initiale, voir [Problèmes résolus](#known_issues).
{: important}

-   Le service prend désormais en charge un modèle à large bande allemand, `de-DE_BroadbandModel`. Ce nouveau modèle allemand prend en charge la personnalisation des modèles de langue (GA) et la personnalisation des modèles acoustiques (bêta).
    -   Pour plus d'informations sur le mode d'analyse des corpus pour l'allemand, voir [Analyse de l'anglais, du français, de l'allemand, de l'espagnol et du portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   Pour plus d'informations sur la création de prononciations possibles pour les mots personnalisés en allemand, voir [Instructions pour le français, l'allemand, l'espagnol et le portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).
-   Les modèles portugais brésiliens, `pt-BR_BroadbandModel` et `pt-BR_NarrowbandModel`, prennent désormais en charge la personnalisation des modèles de langue (GA). Les modèles n'ayant pas été mis à jour pour activer cette prise en charge, aucune mise à niveau de modèles acoustiques personnalisés n'est requise.
    -   Pour plus d'informations sur le mode d'analyse des corpus pour le portugais brésilien, voir [Analyse de l'anglais, du français, de l'allemand, de l'espagnol et du portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   Pour plus d'informations sur la création de prononciations possibles pour les mots personnalisés en portugais brésilien, voir [Instructions pour le français, l'allemand, l'espagnol et le portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).
-   Les nouvelles versions des modèles à large bande et à bande étroite anglais américains et japonais sont disponibles :
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    Les nouveaux modèles offrent une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Les fonctions de détection de mots clés et d'autres propositions de mots sont désormais disponibles en version GA et non plus en fonctionnalité bêta pour toutes les langues. Pour plus d'informations, voir 
    -   [Détection de mots clés](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)
    -   [Autres propositions de mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)

### Problèmes résolus
{: #known_issues}

Les problèmes connus suivants qui étaient associés à l'interface de personnalisation ont été résolus. Cette liste est gérée pour les utilisateurs qui ont rencontré ces problèmes auparavant.

-   Si vous ajoutez des données à un modèle de langue personnalisé ou un modèle acoustique personnalisé, vous devez entraîner à nouveau ce modèle avant de l'utiliser pour la reconnaissance vocale. Le problème survient dans le scénario suivant :

    1.  L'utilisateur crée un nouveau modèle personnalisé (de langue ou acoustique) et entraîne le modèle.
    1.  L'utilisateur ajoute des ressources supplémentaires (mots, corpus ou audio) au modèle personnalisé mais n'entraîne pas le modèle.
    1.  L'utilisateur ne peut pas utiliser le modèle personnalisé pour la reconnaissance vocale. Le service renvoie une erreur au format suivant lorsqu'il est utilisé avec une demande de reconnaissance vocale :

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    Pour contourner le problème, l'utilisateur doit à nouveau entraîner le modèle personnalisé sur ses données les plus récentes. Il peut alors utiliser le modèle personnalisé avec la reconnaissance vocale.
-   Avant d'entraîner un modèle de langue personnalisé ou un modèle acoustique personnalisé, vous devez le mettre à niveau à la dernière version du modèle de base correspondant. Le problème survient dans le scénario suivant :

    1.  L'utilisateur dispose d'un modèle personnalisé existant (langue ou acoustique) basé sur un modèle qui a été mis à jour.
    1.  L'utilisateur entraîne le modèle personnalisé existant avec l'ancienne version du modèle de base sans avoir préalablement effectué la migration vers la dernière version du modèle de base.
    1.  L'utilisateur ne peut pas utiliser le modèle personnalisé pour la reconnaissance vocale.

    Pour contourner ce problème, l'utilisateur doit utiliser la méthode `POST /v1/customizations/{customization_id}/upgrade_model` ou `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` pour mettre à niveau le modèle personnalisé à la dernière version du modèle de base associé. Il peut alors utiliser le modèle personnalisé avec la reconnaissance vocale.

Ces deux problèmes ont été corrigés en production.

### 7 septembre 2018
{: #September2018a}

L'interface REST HTTP basée sur une session n'est plus prise en charge. Toutes les informations relatives aux sessions sont retirées de la documentation. Les méthodes suivantes ne sont plus disponibles :
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Si votre application utilise l'interface à base de sessions, vous devez migrer vers l'une des interfaces REST HTTP restantes ou vers l'interface WebSocket. Pour plus d'informations, voir la mise à jour de service du [8 août 2018](#August2018).

### 8 août 2018
{: #August2018}

L'interface REST HTTP basée sur une session est obsolète depuis le **8 août 2018**. Toutes les méthodes d'API des sessions sont retirées du service à compter du **7 septembre 2018**, après cette date vous n'aurez plus la possibilité d'utiliser l'interface basée sur les sessions. Cette notification de dépréciation immédiate et de retrait sous 30 jours s'appliquent aux méthodes suivantes :

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Si votre application utilise l'interface de session, vous devez effectuer la migration vers l'une des interfaces suivantes au plus tard le 7 septembre :
{: important}

-   Pour la reconnaissance vocale basée sur les flux (y compris les cas d'utilisation réels), utilisez l'[interface WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), qui fournit l'accès aux résultats intermédiaires et aux temps d'attente les plus faibles.
-   Pour la reconnaissance vocale à base de fichiers, utilisez l'une des interfaces suivantes :

    -   Pour les fichiers plus courts jusqu'à quelques minutes d'audio, utilisez l'[interface HTTP synchrone](/docs/services/speech-to-text?topic=speech-to-text-http) `(POST /v1/recognize`) ou l'[interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async) (`POST /v1/recognitions`).
    -   Pour les fichiers plus longs de plus de quelques minutes d'audio, utilisez l'interface HTTP asynchrone. Cette interface accepte jusqu'à 1 Go de données audio avec une seule demande.

Les interfaces WebSocket et HTTP produisent les mêmes résultats que l'interface de session (seule l'interface WebSocket fournit des résultats intermédiaires). Vous pouvez également utiliser l'un des logiciels SDK Watson, ce qui simplifie le développement d'application avec l'une de ces interfaces. Pour plus d'informations, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

### 13 juillet 2018
{: #July2018}

Le modèle espagnol à bande étroite, `es-ES_NarrowbandModel`, a été mis à jour pour une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement le modèle mis à jour pour toutes les demandes de reconnaissance. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur ce modèle, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).

Depuis cette mise à jour, les deux versions suivantes du modèle espagnol à bande étroite sont disponibles :

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (actuel)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (précédent)

La version suivante du modèle n'est plus disponible :

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

Une demande de reconnaissance qui tente d'utiliser un modèle personnalisé basé sur le modèle de base qui n'est désormais plus disponible, utilise le dernier modèle de base disponible sans aucune personnalisation. Le service renvoie le message d'avertissement suivant : `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` Pour continuer en utilisant un modèle personnalisé basé sur le modèle qui n'est pas disponible, vous devez d'abord effectuer la migration du modèle en utilisant la méthode `upgrade_model` appropriée décrite précédemment.

### 12 juin 2018
{: #June2018}

Les fonctions suivantes sont activées pour les applications hébergées à Washington, DC (**us-east**) :

-   Le service prend désormais en charge le nouveau processus d'authentification d'API. Pour plus d'informations, voir la [mise à jour de service du 30 octobre 2018](#October2018b).
-   Le service prend désormais en charge l'en-tête `X-Watson-Metadata` et la méthode `DELETE /v1/user_data`. Pour plus d'informations, voir [Sécurité des informations](/docs/services/speech-to-text?topic=speech-to-text-information-security).

### 15 mai 2018
{: #May2018}

Les fonctions suivantes sont activées pour les applications hébergées à Sydney (**au-syd**) :

-   Le service prend désormais en charge le nouveau processus d'authentification d'API. Pour plus d'informations, voir la [mise à jour de service du 30 octobre 2018](#October2018b).
-   Le service prend désormais en charge l'en-tête `X-Watson-Metadata` et la méthode `DELETE /v1/user_data`. Pour plus d'informations, voir [Sécurité des informations](/docs/services/speech-to-text?topic=speech-to-text-information-security).

### 26 mars 2018
{: #March2018b}

-   Le service prend désormais en charge la personnalisation du modèle de langue français, `fr-FR_BroadbandModel`. Le modèle français est disponible en version GA pour une utilisation en production avec la personnalisation de modèle de langue.
    -   Pour plus d'informations sur le mode d'analyse des corpus pour le français, voir [Analyse de l'anglais, du français, de l'allemand, de l'espagnol et du portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   Pour plus d'informations sur la création de prononciations possibles pour les mots personnalisés en français, voir [Instructions pour le français, l'allemand, l'espagnol et le portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR).
-   Les modèles espagnol et coréen à bande étroite, `es-ES_NarrowbandModel` et `ko-KR_NarrowbandModel`, ainsi que le modèle français à large bande, `fr-FR_BroadbandModel`, ont été mis à jour pour une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur l'un de ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Le paramètre `version` des méthodes suivantes est renommé `base_model_version` :

    -   `/v1/recognize` pour les demandes WebSocket
    -   `POST /v1/recognize` pour les demandes HTTP sans session
    -   `POST /v1/sessions` pour les demandes HTTP avec session
    -   `POST /v1/recognitions` pour les demandes HTTP asynchrones

    Le paramètre `base_model_version` indique la version d'un modèle de base à utiliser pour la reconnaissance vocale. Pour plus d'informations, voir [Demandes de reconnaissance effectuées avec des modèles personnalisés mis à niveau](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition) et [Version du modèle de base](/docs/services/speech-to-text?topic=speech-to-text-input#version).
-   Le formatage intelligent est désormais pris en charge pour l'espagnol et l'anglais américain. Pour l'anglais américain, cette fonction convertit désormais les chaînes de mots clés en signes de ponctuation pour les points, les virgules, les points d'interrogation et les points d'exclamation. Pour plus d'informations, voir [Formatage intelligent](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).

### 1er mars 2018
{: #March2018a}

Les modèles espagnol et français à large bande, `es-ES_BroadbandModel` et `fr-FR_BroadbandModel`, ont été mis à jour pour une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur l'un de ces modèles, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Pour plus d'informations, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade). La section présente les règles de mise à niveau des modèles personnalisés, les effets de la mise à niveau ainsi que les approches possibles pour utiliser les modèles mis à niveau.

### 1er février 2018
{: #February2018}

Le service offre désormais des modèles pour la reconnaissance vocale de la langue coréenne : `ko-KR_BroadbandModel`, modèle audio échantillonné avec un minimum de 16 kHz et `ko-KR_NarrowbandModel`, modèle audio échantillonné avec un minimum de 8 kHz. Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).

Pour la personnalisation de modèle de langue, les modèles coréens sont disponibles en version GA pour être utilisés en production, pour la personnalisation de modèle acoustique, ils sont en fonctionnalité bêta. Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).

-   Pour plus d'informations sur le mode d'analyse des corpus pour le coréen, voir [Analyse du coréen](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages-koKR).
-   Pour plus d'informations sur la création de prononciations possibles pour les mots personnalisés en coréen, voir [Instructions pour le coréen](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-koKR).

### 14 décembre 2017
{: #December2017}

-   Les modèles anglais américains, `en-US_BroadbandModel` et `en-US_NarrowbandModel`, ont été mis à jour pour une meilleure reconnaissance vocale. Par défaut, le service utilise automatiquement les modèles mis à jour pour toutes les demandes de reconnaissance. Si vous disposez de modèles de langue personnalisés ou de modèles acoustiques personnalisés basés sur les modèles anglais américains, vous devez mettre à niveau vos modèles personnalisés pour tirer parti de ces mises à jour à l'aide des méthodes suivantes :

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Pour plus d'informations sur cette procédure, voir [Mise à niveau des modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade). La section présente les règles de mise à niveau des modèles personnalisés, les effets de la mise à niveau ainsi que les approches possibles pour utiliser les modèles mis à niveau. Actuellement, les méthodes s'appliquent uniquement aux nouveaux modèles de base anglais américains. Mais les mêmes informations s'appliqueront aux mises à niveau d'autres modèles de base dès qu'elles seront disponibles.
-   Les diverses méthodes de demandes de reconnaissance comprennent désormais un nouveau paramètre `base_model_version` que vous pouvez utiliser pour initier des demandes qui utilisent les versions plus anciennes ou mises à niveau des modèles de base et des modèles personnalisés. Bien qu'il soit principalement conçu pour être utilisé avec des modèles personnalisés mis à niveau, le paramètre `base_model_version` peut également être employé sans modèles personnalisés. Pour plus d'informations, voir [Version du modèle de base](/docs/services/speech-to-text?topic=speech-to-text-input#version).
-   Le service prend désormais en charge la personnalisation de modèle acoustique en tant que fonctionnalité bêta pour toutes les langues disponibles. Vous pouvez créer des modèles acoustiques personnalisés pour des modèles à bande étroite ou à large bande pour toutes les langues. Pour une introduction à la personnalisation, comprenant la personnalisation des modèles acoustiques, voir [L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization).
-   Le service prend désormais en charge la personnalisation de modèle de langue pour les modèles anglais britanniques, `en-GB_BroadbandModel` et `en-GB_NarrowbandModel`. Bien que le service traite les corpus et les mots personnalisés en anglais américain et en anglais britannique de manière généralement similaire, il existe des différences importantes :
    -   Pour plus d'informations sur le mode d'analyse des corpus pour l'anglais britannique, voir [Analyse de l'anglais, du français, de l'allemand, de l'espagnol et du portugais brésilien](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages).
    -   Pour plus d'informations sur la création de prononciations possibles pour les mots personnalisés en anglais britannique, voir [Instructions pour l'anglais (américain et britannique)](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-enUS-enGB). Pour l'anglais britannique, en particulier, vous ne pouvez pas utiliser des points ou des tirets dans les prononciations possibles.
-   La personnalisation de modèle de langue et tous les paramètres associés sont à présent disponibles en version GA pour toutes les langues prises en charge : japonais, espagnol, anglais britannique et anglais américain.

### 2 octobre 2017
{: #October2017}

-   L'interface de personnalisation offre désormais la personnalisation de modèle acoustique. Vous pouvez créer des modèles acoustiques personnalisés qui adaptent les modèles de base du service à votre environnement et à vos locuteurs. Vous alimentez et entraînez un modèle acoustique personnalisé à l'audio qui correspond le plus à la signature acoustique de l'audio que vous voulez transcrire. Vous utilisez ensuite ce modèle acoustique personnalisé avec des demandes de reconnaissance pour augmenter la précision de la reconnaissance vocale.

    Les modèles acoustiques personnalisés sont complémentaires des modèles de langue personnalisés. Vous pouvez entraîner un modèle acoustique personnalisé avec un modèle de langue personnalisé, et vous pouvez utiliser ces deux types de modèle lors de la reconnaissance vocale. La personnalisation de modèle acoustique est une interface bêta disponible uniquement pour l'anglais américain, l'espagnol et le japonais.

    -   Pour plus d'informations sur les langues prises en charge par l'interface de personnalisation et le niveau de support disponible pour chaque langue, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
    -   Pour plus d'informations sur l'interface de personnalisation du service, voir [L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization).
    -   Pour plus d'informations sur la création d'un modèle acoustique personnalisé, voir [Création d'un modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acoustic).
    -   Pour plus d'informations sur l'utilisation d'un modèle acoustique personnalisé, voir [Utilisation d'un modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acousticUse).
    -   Pour plus d'informations sur toutes les méthodes de l'interface de personnalisation, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
-   Pour la personnalisation de modèle de langue, le service comprend désormais une fonction bêta qui définit un poids de personnalisation facultatif pour un modèle de langue personnalisé. Un poids de personnalisation indique le poids relatif à attribuer aux mots d'un modèle de langue personnalisé par rapport aux mots du vocabulaire de base du service. Vous pouvez définir un poids de personnalisation lors de l'entraînement et de la reconnaissance vocale. Pour plus d'informations, voir [Utilisation d'un poids de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight).
-   Le modèle de langue `ja-JP_BroadbandModel` a été mis à jour pour capturer les améliorations apportées au modèle de base. La mise à niveau n'affecte pas les modèles personnalisés existants basés sur le modèle.
-   Le service comprend désormais un paramètre pour indiquer l'ordre d'octets (endianness) de l'audio soumis au format `audio/l16` (PCM (Pulse-Code Modulation) linéaire 16 bits). En plus des paramètres `rate` et `channels` avec ce format, vous pouvez également indiquer `big-endian` ou `little-endian` avec le paramètre `endianness`. Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

### 14 juillet 2017
{: #July2017b}

-   Le service prend désormais en charge la transcription audio au format MP3 ou MPEG (Motion Picture Experts Group). Pour plus d'informations sur les formats audio pris en charge, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   L'interface de personnalisation de modèle de langue prend désormais en charge l'espagnol en tant que fonctionnalité bêta. Vous pouvez créer un modèle personnalisé basé sur l'un des modèles de langue espagnols : `es-ES_BroadbandModel` ou `es-ES_NarrowbandModel`. Pour plus d'informations, voir [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate). La tarification des demandes de reconnaissance utilisant des modèles de langue personnalisés espagnols est identique à celle des demandes utilisant les modèles anglais américain et japonais.
-   L'objet JSON `CreateLanguageModel` que vous transmettez à la méthode `POST /v1/customizations` pour créer un modèle de langue personnalisé comprend désormais une zone `dialect`. Cette zone indique le dialecte de la langue à utiliser avec le modèle personnalisé. Par défaut, le dialecte correspond à la langue du modèle de base. Le paramètre est parlant uniquement pour les modèles espagnols pour lesquels le service peut créer un modèle personnalisé qui convient au discours dans l'un des dialectes suivants :
    -   `es-ES` pour l'espagnol castillan (valeur par défaut)
    -   `es-LA` pour l'espagnol latino-américain
    -   `es-US` pour l'espagnol d'Amérique du Nord (mexicain)

    Les méthodes `GET /v1/customizations` et `GET /v1/customizations/{customization_id}` de l'interface de personnalisation comprennent le dialecte d'un modèle personnalisé dans leur sortie. Pour plus d'informations, voir [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language) et [Affichage de la liste des modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Les noms des modèles de langue `en-UK_BroadbandModel` et `en-UK_NarrowbandModel` sont désormais obsolètes. Ces modèles sont désormais disponibles sous les noms `en-GB_BroadbandModel` et `en-GB_NarrowbandModel`.

    Les noms `en-UK_{model}` obsolètes fonctionnent toujours, mais la méthode `GET /v1/models` ne renvoie plus ces noms dans la liste des modèles disponibles. Vous pouvez toujours interroger les noms directement avec la méthode `GET /v1/models/{model_id}`.

### 1er juillet 2017
{: #July2017a}

-   L'interface de personnalisation de modèle de langue du service est à présent disponible en version GA pour les deux langues prises en charge : l'anglais américain et le japonais. {{site.data.keyword.IBM_notm}} ne facture pas la création, l'hébergement ou la gestion des modèles de langue personnalisés. Comme indiqué dans la section suivante, {{site.data.keyword.IBM_notm}} facture désormais un supplément de 0,03 $ par minute audio pour les demandes de reconnaissance utilisant des modèles personnalisés.
-   {{site.data.keyword.IBM_notm}} a mis à jour la tarification du service ainsi :
    -   Elimination d'un coût supplémentaire pour l'utilisation des modèles à bande étroite
    -   Application d'une tarification différenciée progressive pour les clients importants en termes de volume
    -   Facturation d'un supplément de 0,03 $ par minute audio pour les demandes de reconnaissance qui utilisent des modèles de langue personnalisés anglais américain et japonais.

    Pour plus d'informations sur les mises à jour de tarification, voir
    -   l'article de blogue [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: external}
    -   le service {{site.data.keyword.speechtotextshort}} dans le [catalogue {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog/services/speech-to-text){: external}
    -   la [foire aux questions liées à la tarification](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing)
-   Vous n'avez plus besoin de transmettre un objet de données vide en tant que corps pour les demandes `POST` suivantes :
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    Par exemple, vous pouvez désormais appeler la méthode `POST /v1/sessions` avec `curl` comme suit :

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    Vous n'avez plus besoin de transmettre l'option `curl` suivante avec la demande : `--data "{}"`.

    Si vous rencontrez des problèmes avec l'une de ces demandes `POST`, essayez de transmettre un objet de données vide avec le corps de la demande. Cette opération ne change en aucune manière la nature ou la signification de la demande.
    {: note}

### 22 mai 2017
{: #May2017}

Les dépréciations suivantes annoncées en mars 2017 sont désormais effectives :

-   Le paramètre `continuous` est retiré de toutes les méthodes qui initient des demandes de reconnaissance. Le service transcrit désormais un flux audio complet jusqu'à la fin ou expire, selon ce qui intervient en premier lieu. Ce comportement équivaut à définir l'ancien paramètre `continuous` avec la valeur `true`. Par défaut, le service interrompait la transcription dès la première demi-seconde sans parole (silence) si le paramètre était omis ou défini avec la valeur `false`.

    Les applications existantes qui définissaient ce paramètre avec la valeur `true` ne verront aucun changement de comportement. Les applications qui définissaient ce paramètre avec la valeur `false` ou qui s'appuyaient sur le comportement par défaut verront sans doute un changement. Si ce paramètre est spécifié dans la demande, le service répond désormais en renvoyant un message d'avertissement pour le paramètre inconnu :

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    La demande aboutit malgré cet avertissement et il n'y a aucune incidence sur une session existante ou une connexion WebSocket.

    {{site.data.keyword.IBM_notm}} a supprimé ce paramètre pour répondre au nombre impressionnant de commentaires de la communauté de développeurs selon lesquels indiquer `continuous=false` n'ajoutait pas grand chose et pouvait réduire l'exactitude de la transcription globale.
-   Il n'est plus possible d'empêcher un délai d'attente de session sans envoyer de données audio :
    -   Lorsque vous utilisez l'interface WebSocket, le client ne peut plus conserver une connexion active en envoyant un message texte en JSON avec le paramètre `action` défini avec `no-op`. L'envoi d'un message `no-op` ne génère pas d'erreur, mais est sans effet.
    -   Lorsque vous utilisez des sessions avec l'interface HTTP, le client ne peut plus étendre la session en envoyant une demande `GET /v1/sessions/{session_id}/recognize`. La méthode renvoie toujours le statut d'une session active mais ne permet pas de conserver la session active.
    Vous pouvez désormais effectuer les opérations suivantes pour conserver une session active :
    -   Définir le paramètre `inactivity_timeout` avec la valeur `-1` pour éviter l'expiration du délai au bout de 30 secondes d'inactivité.
    -   Envoyer des données audio, constituées uniquement de silence, au service pour éviter une expiration du délai d'attente de session au bout de 30 secondes d'inactivité. Vous êtes facturé pour toutes les données que vous envoyez au service, notamment le silence que vous envoyez pour étendre une session.

    Pour plus d'informations, voir [Délais d'attente](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts). Dans l'idéal, vous établirez une session juste avant d'obtenir l'audio pour la transcription et conserverez la session par l'envoi audio à un débit proche du temps réel. Veillez également à ce que votre application soit rétablie normalement après une fermeture de session ou de connexion.

    {{site.data.keyword.IBM_notm}} a retiré cette fonctionnalité pour garantir une offre de service de reconnaissance vocale de première qualité et à faible temps d'attente à tous les utilisateurs.

### 10 avril 2017
{: #April2017}

-   Le service prend désormais en charge la fonction d'étiquettes de locuteur (speaker labels) pour les modèles à large bande suivants :
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

Pour plus d'informations, voir [Etiquettes de locuteur](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   Le service prend désormais en charge le format audio WebM (Web Media) avec le codec Opus ou Vorbis. Il prend également en charge le format audio Ogg avec le codec Vorbis en plus du codec Opus. Pour plus d'informations sur les formats audio pris en charge, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Le service prend désormais en charge le partage de ressources d'origine croisée (CORS) pour permettre aux clients utilisant un navigateur d'appeler directement le service. Pour plus d'informations, voir [Support de CORS](/docs/services/speech-to-text?topic=speech-to-text-developerOverview#cors).
-   L'interface HTTP asynchrone offre désormais une méthode `POST /v1/unregister_callback` qui retire l'enregistrement d'une URL de rappel sur liste blanche. Pour plus d'informations, voir [Désenregistrement d'une URL de rappel](/docs/services/speech-to-text?topic=speech-to-text-async#unregister).
-   L'interface WebSocket n'expire plus pour les demandes de reconnaissance, notamment pour les fichiers audio longs. Vous n'avez plus besoin de demander des résultats intermédiaires avec le message JSON `start` pour empêcher l'expiration du délai d'attente. (Ce problème était décrit dans les notes sur l'édition du 10 mars 2016.)
-   La méthode `DELETE /v1/customizations/{customization_id}` renvoie à présent le code de réponse HTTP 401 si vous essayez de supprimer un modèle personnalisé inexistant.
-   La méthode `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` renvoie à présent le code de réponse HTTP 400 si vous essayez de supprimer un corpus inexistant.

### 8 mars 2017
{: #March2017}

L'interface HTTP asynchrone est désormais disponible en version GA. Avant cette date, il s'agissait d'une fonctionnalité bêta.

### 1er décembre 2016
{: #December2016}

-   Le service offre désormais une fonction d'étiquettes de locuteur (speaker labels) bêta pour l'audio à bande étroite en anglais américain, en espagnol ou en japonais. Cette fonction identifie quels sont les mots qui ont été prononcés et par qui dans un échange entre plusieurs personnes. Les méthodes de reconnaissance sans session, avec session, asynchrones et WebSocket comprennent chacune un paramètre `speaker_labels` qui accepte une valeur booléenne pour indiquer s'il faut inclure les étiquettes des locuteurs (speaker labels) dans la réponse. Pour plus d'informations sur cette fonction, voir [Etiquettes de locuteur](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels).
-   L'interface de personnalisation de modèle de langue bêta est désormais prise en charge pour le japonais en plus de l'anglais américain. Toutes les méthodes de l'interface prennent en charge le japonais. Pour plus d'informations, voir les sections suivantes :
    -   Pour plus d'informations, voir [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate) et [Utilisation d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageUse).
    -   Pour des questions d'ordre général et spécifiques au japonais pour l'ajout d'un fichier texte de corpus, voir [Préparation d'un fichier de corpus](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#prepareCorpus) et [Que se passe-t-il lorsque j'ajoute un fichier de corpus ?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)
    -   Pour des questions spécifiques au japonais lorsque vous spécifiez la zone `sounds_like` d'un mot personnalisé, voir [Instructions pour le japonais](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-jaJP).
    -   Pour plus d'informations sur toutes les méthodes de l'interface de personnalisation, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
-   L'interface de personnalisation de modèle de langue comprend désormais une méthode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` qui recense les informations sur un corpus spécifié. Cette méthode est pratique pour la gestion du statut d'une demande d'ajout d'un corpus à un modèle personnalisé. Pour plus d'informations, voir [Affichage de la liste des corpus d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).
-   La sortie JSON qui est renvoyée par les méthodes `GET /v1/customizations/{customization_id}/words` et `GET /v1/customizations/{customization_id}/words/{word_name}` comprend désormais une zone `count` pour chaque mot. Cette zone indique le nombre de fois que le mot est détecté sur l'ensemble des corpus. Si vous ajoutez un mot personnalisé dans un modèle avant son ajout par un corpus, le comptage commence par `1`. Si le mot est d'abord ajouté dans un corpus et modifié par la suite, le comptage répercute uniquement le nombre de fois que le mot est détecté dans les corpus. Pour plus d'informations, voir [Affichage de la liste des mots d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords).

    Pour les modèles personnalisés créés avant la mise en place de la zone `count`, la zone reste toujours à `0`. Pour mettre à jour la zone pour les modèles de ce type, ajoutez à nouveau les corpus du modèle et incluez le paramètre `allow_overwrite` avec la méthode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`.
-   La méthode `GET /v1/customizations/{customization_id}/words` comprend désormais le paramètre de requête `sort` qui contrôle l'ordre d'apparition des mots dans la liste. Le paramètre accepte deux arguments, `alphabetical` ou `count`, pour indiquer comment les mots doivent être triés. Vous pouvez ajouter en option au début, le signe `+` ou `-` dans un argument pour indiquer si les résultats doivent être triés par ordre croissant ou décroissant. Par défaut, la méthode affiche les mots par ordre alphabétique croissant. Pour plus d'informations, voir [Affichage de la liste des mots d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords).

    Pour les modèles personnalisés créés avant l'introduction de la zone `count`, l'utilisation de l'argument `count` avec le paramètre `sort` est sans fondement. Utilisez l'argument par défaut `alphabetical` avec les modèles de ce type.
-   La zone `error` qui peut être renvoyée dans la réponse JSON des méthodes `GET /v1/customizations/{customization_id}/words` et `GET /v1/customizations/{customization_id}/words/{word_name}` est désormais un tableau. Si le service découvre un ou plusieurs problèmes avec la définition d'un mot personnalisé, la zone répertorie chaque élément du problème dans la définition et fournit un message avec la description du problème. Pour plus d'informations, voir [Affichage de la liste des mots d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   Les paramètres `keywords_threshold` et `word_alternatives_threshold` des méthodes de reconnaissance n'acceptent plus de valeur NULL. Pour ignorer des mots clés ou d'autres propositions de mots dans la réponse, omettez ces paramètres. Une valeur spécifiée doit être une variable flottante.

Pour plus d'informations sur l'interface du service, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

### 22 septembre 2016
{: #September2016}

-   Le service offre à présent une nouvelle interface de personnalisation de modèle de langue bêta pour l'anglais américain. Vous pouvez utiliser cette interface pour personnaliser le vocabulaire de base et les modèles de langue du service via la création de modèles de langue personnalisés qui intègrent la terminologie spécifique à un domaine. Vous pouvez ajouter des mots personnalisés individuellement ou les faire extraire des corpus par le service. Pour utiliser vos modèles personnalisés avec les méthodes de reconnaissance vocale proposées par l'une des interfaces du service, transmettez le paramètre de requête `customization_id`. Pour plus d'informations, voir 
    -   [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
    -   [Utilisation d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
    -   [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}
-   La liste des formats audio pris en charge comprend désormais le format `audio/mulaw`, qui fournit un seul canal audio codé avec l'algorithme de données u-law (ou mu-law). Lorsque vous utilisez ce format, vous devez également indiquer la fréquence d'échantillonnage de capture audio. Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Les méthodes `GET /v1/models` et `GET /v1/models/{model_id}` renvoient désormais une zone `supported_features` dans leur sortie pour chaque modèle de langue. Des informations complémentaires indiquent si la personnalisation est prise en charge par le modèle. Pour plus d'informations, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

### 30 juin 2016
{: #June2016b}

L'interface HTTP asynchrone bêta est désormais compatible avec toutes les langues prises en charge par le service. L'interface était disponible uniquement en anglais américain auparavant. Pour plus d'informations, voir [L'interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async) et [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.

### 23 juin 2016
{: #June2016a}

-   Une interface HTTP asynchrone bêta est à présent disponible. Cette interface fournit les fonctions de reconnaissance complètes pour la transcription de l'anglais américain via des appels HTTP non bloquants. Vous pouvez enregistrer des URL de rappel et fournir des chaînes secrètes pour assurer l'authentification et l'intégrité des données avec des signatures numériques. Pour plus d'informations, voir [L'interface HTTP asynchrone](/docs/services/speech-to-text?topic=speech-to-text-async) et [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
-   Une fonction de formatage intelligent bêta convertit désormais les dates, les heures, une série de chiffres et de nombres, des numéros de téléphone, des valeurs de devise et des adresses Internet en représentations plus conventionnelles dans les retranscriptions finales. Cette fonction est activée en définissant le paramètre `smart_formatting` avec la valeur `true` à l'occasion d'une demande de reconnaissance. Il s'agit d'une fonctionnalité bêta disponible uniquement pour l'anglais américain. Pour plus d'informations, voir [Formatage intelligent](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).
-   La liste des modèles pris en charge pour la reconnaissance vocale comprend à présent le modèle audio `fr-FR_BroadbandModel` en langue française échantillonné à 16 kHz minimum. Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).
-   La liste des formats audio pris en charge comprend à présent le format `audio/basic`. Ce format fournit un canal audio unique codé à l'aide de données au format u-law (ou mu-law) sur 8 bits échantillonnées à 8 kHz. Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Les différentes méthodes de reconnaissance peuvent renvoyer une réponse `warnings` contenant des messages sur les paramètres de requête ou les zones JSON non valides inclus dans une demande. Le format de ces avertissements a changé. Par exemple, `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` est devenu `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`
-   Pour les demandes HTTP `POST` qui ne transmettent généralement pas de données au service, vous devez inclure un corps de demande vide sous la forme `{}`. Avec la commande `curl`, utilisez l'option `--data` pour transmettre les données vides.

### 10 mars 2016
{: #March2016}

-   Les deux formes de transmission de données (diffusion ponctuelle et diffusion en continu) imposent désormais une taille limite de 100 Mo pour les données audio, à l'instar de l'interface WebSocket. Auparavant, la taille limite des données pour une diffusion ponctuelle était fixée 4 Mo. Pour plus d'informations, voir [Transmission audio](/docs/services/speech-to-text?topic=speech-to-text-input#transmission) (pour toutes les interfaces) et [Envoi audio et réception des résultats de la reconnaissance](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSaudio) (pour l'interface WebSocket). La section WebSocket évoque également la taille maximale de trame ou de message de 4 Mo imposée par l'interface WebSocket.
-   La réponse JSON d'une demande de reconnaissance peut désormais inclure un tableau de messages d'avertissement pour des paramètres de requête ou des zones JSON non valides inclus dans une demande. Chaque élément du tableau est une chaîne qui décrit la nature de l'avertissement, suivie d'un tableau de chaînes d'arguments non valides. Par exemple, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`. Pour plus d'informations, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
-   Le logiciel *{{site.data.keyword.watson}} Speech Software Development Kit (SDK) pour le système d'exploitation iOS d'Apple&reg;* est obsolète. Utilisez à la place le logiciel *{{site.data.keyword.watson}} SDK pour le système d'exploitation iOS d'Apple&reg;*. Ce nouveau logiciel SDK est disponible dans le [référentiel ios-sdk](https://github.com/watson-developer-cloud/ios-sdk){: external} dans l'espace de nom `watson-developer-cloud` sur GitHub.

L'interface WebSocket renferme les problèmes connus suivants :

-   Le service peut nécessiter plusieurs minutes pour produire les résultats finaux d'une demande de reconnaissance pour un fichier audio particulièrement long. Pour l'interface WebSocket, la connexion TCP sous-jacente reste en veille pendant que le service prépare la réponse. Par conséquent, la connexion peut s'interrompre en raison d'un dépassement du délai d'attente. Pour éviter cela avec l'interface WebSocket, demandez des résultats intermédiaires (`\"interim_results\": \"true\"`) dans le fichier JSON pour le message `start` pour initier la demande. Vous pouvez supprimer ces résultats intermédiaires si vous n'en avez pas besoin. Ce problème sera résolu dans une prochaine mise à jour.

### 19 janvier 2016
{: #January2016}

Le service a été mis à jour pour inclure une nouvelle fonction de filtrage des grossièretés le 19 janvier 2016. Par défaut, le service censure les grossièretés et ne les retranscrit pas dans les résultats pour l'audio anglais américain. Pour plus d'informations, voir [Filtrage des grossièretés](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter).

### 17 décembre 2015
{: #December2015}

-   Le service offre à présent une fonction de détection de mots clés. Vous pouvez spécifier un tableau de chaînes de mots clés à faire correspondre dans l'entrée audio. Vous devez également indiquer le niveau de confiance défini par l'utilisateur qu'un mot doit avoir pour être considéré comme correspondance d'un mot clé. Pour plus d'informations, voir [Détection de mots clés](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting). La fonction de détection de mots clés est une fonctionnalité bêta.
-   Le service offre désormais une fonction permettant d'obtenir d'autres propositions de mots (word alternatives). Cette fonction renvoie des hypothèses de mots possibles dans l'entrée audio en accord avec un niveau de confiance défini par l'utilisateur. Pour plus d'informations, voir [Autres propositions de mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives). La fonction d'autres proposition de mots (word alternatives) est une fonctionnalité bêta.
-   Le service prend en charge plus de langues avec ses modèles de retranscription : `en-UK_BroadbandModel` et `en-UK_NarrowbandModel` pour l'anglais britannique et `ar-AR_BroadbandModel` pour l'arabe standard moderne. Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).
-   Les demandes de reconnaissance HTTP ne sont plus tributaires d'un dépassement de délai de plateforme fixé à 10 minutes. Le service conserve la connexion active en envoyant un caractère espace dans l'objet JSON de réponse toutes les 20 secondes tant que la reconnaissance est en cours. Pour plus d'informations, voir [Délais d'attente](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts). 
-   Le service ne renvoie plus le code de statut HTTP 490 pour les méthodes HTTP avec session `GET /v1/sessions/{session_id}/observe_result` et `POST /v1/sessions/{session_id}/recognize`. Le service répond désormais avec le code de statut HTTP 400 à la place.

    Dans les réponses JSON qu'il renvoie en cas d'erreurs avec des méthodes avec session, le service comprend désormais une nouvelle zone `session_closed`. Cette zone est définie avec la valeur `true` si la session est fermée suite à une erreur. Pour plus d'informations sur les codes retour possibles d'une méthode, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
-   Lorsque vous utilisez la commande `curl` pour transcrire de l'audio avec le service, vous n'avez plus besoin d'utiliser l'option `--limit-rate` pour transférer des données à un débit inférieur à 40 000 octets par seconde.

### 21 septembre 2015
{: #September2015}

-   Les nouveaux logiciels SDK pour applications mobiles bêta sont disponibles pour les services vocaux. Ces logiciels SDK permettent aux applications mobiles d'interagir avec les services {{site.data.keyword.speechtotextshort}} et {{site.data.keyword.texttospeechshort}}.
    -   Le logiciel *{{site.data.keyword.watson}} Speech SDK pour la plateforme Android&trade; de Google* prend en charge la diffusion audio en continu vers le service {{site.data.keyword.speechtotextshort}} en temps réel et reçoit une retranscription des données audio à mesure que vous parlez. Le projet comprend un modèle d'application qui présente l'interaction avec les deux services de reconnaissance vocale. Ce logiciel SDK est disponible dans le [référentiel speech-android-sdk](https://github.com/watson-developer-cloud/speech-android-sdk){: external} dans l'espace de nom `watson-developer-cloud` sur GitHub.
    -   Le logiciel *{{site.data.keyword.watson}} Speech SDK pour le système d'exploitation iOS d'Apple&reg;* prend en charge la diffusion audio en continu vers le service {{site.data.keyword.speechtotextshort}} et reçoit une retranscription des données audio en réponse. Le logiciel SDK est disponible dans le [référentiel speech-ios-sdk](https://github.com/watson-developer-cloud/speech-ios-sdk){: external} dans l'espace de nom `watson-developer-cloud` sur GitHub.

    Ces deux logiciels SDK prennent en charge l'authentification auprès des services de reconnaissance vocale à l'aide de vos données d'identification au service {{site.data.keyword.cloud_notm}} ou d'un jeton d'authentification.

    Comme ces SDK sont en version bêta, ils sont susceptibles de changer par la suite.
    {: note}
-   Le service prend en charge deux nouvelles langues, le portugais brésilien et le chinois mandarin. Les modèles correspondant à ces deux langues sont `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel` et `zh-CN_NarrowbandModel`. Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).
-   Les demandes HTTP `POST` `/v1/sessions/{session_id}/recognize` et `/v1/recognize`, ainsi que la demande WebSocket `/v1/recognize`, prennent en charge la transcription d'un nouveau type de support : `audio/ogg;codecs=opus` pour les fichiers au format Ogg utilisant le codec Opus. Par ailleurs, le format `audio/wav` format pour ces méthodes est désormais compatible avec tout type de codage. La restriction relative à l'utilisation du codage PCM linéaire n'existe plus. Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Le service prend désormais en charge les dépassements de délai d'attente lorsque vous transcrivez des fichiers audio longs avec l'interface HTTP. Lorsque vous utilisez des sessions, vous pouvez employer un masque d'interrogation long en spécifiant des ID de séquence avec les méthodes `GET /v1/sessions/{session_id}/observe_result` et `POST /v1/sessions/{session_id}/recognize` pour les tâches de reconnaissance à exécution longue. En utilisant le nouveau paramètre `sequence_id` de ces méthodes, vous pouvez demander des résultats, avant, pendant ou après la soumission d'une demande de reconnaissance.
-   Pour les modèles anglais américains, `en_US_BroadbandModel` et `en_US_NarrowbandModel`, le service met correctement en majuscules de nombreux noms propres. Par exemple, le service renverra désormais le texte "Barack Obama graduated from Columbia University" au lieu de "barack obama graduated from columbia university". Cette modification vous sera utile si votre application est sensible à la casse des noms propres.
-   La demande HTTP `DELETE /v1/sessions/{session_id}` ne renvoie pas le code de statut 415 "Unsupported Media Type". Ce code de retour est supprimé de la documentation relative à cette méthode.

### 1er juillet 2015
{: #July2015}

Le service est passé de la version bêta à GA (disponibilité générale) le 1er juillet 2015. Il existe les différences suivantes entre les versions bêta et GA des API {{site.data.keyword.speechtotextshort}}. La version GA nécessite que les utilisateurs effectuent une mise à niveau pour passer à la nouvelle version du service.

-   La version GA de l'API HTTP est compatible avec la version bêta. Vous devez modifier votre code d'application uniquement si vous avez spécifié explicitement un nom de modèle. Par exemple, le code exemple disponible pour le service dans GitHub comprend la ligne de code suivante dans le fichier `demo.js` :

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    Cette ligne spécifie le modèle par défaut, `WatsonModel`, utilisé dans la version bêta du service. Si votre application spécifie également ce modèle, vous devez le remplacer pour utiliser l'un des nouveaux modèles pris en charge par la version GA. Pour plus d'informations, voir la section suivante.
-   Le service prend désormais en charge un nouveau modèle de programmation pour l'interaction directe entre un client et le service au moyen d'une connexion WebSocket. En utilisant ce modèle, un client peut obtenir un jeton d'authentification pour communiquer directement avec le service. Le jeton évite la nécessité d'avoir recours à une application proxy côté serveur dans {{site.data.keyword.cloud_notm}} pour appeler le service pour le compte du client. Les jetons sont les moyens privilégiés des clients pour interagir avec le service.

    Le service prend toujours en charge l'ancien modèle de programmation qui s'appuyait sur un proxy côté serveur pour transmettre l'audio et les messages entre le client et le service. Le nouveau modèle est quand même plus efficace et offre une capacité de traitement supérieure.
-   Les méthodes `POST /v1/sessions` et `POST /v1/recognize`, ainsi que la méthode WebSocket `/v1/recognize`, prennent désormais en charge le paramètre de requête `model`. Ce paramètre vous permet d'indiquer des informations sur l'audio :

    -   La langue : *Anglais*, *Japonais* ou *Espagnol*
    -   La fréquence d'échantillonnage minimale : *large bande* (16 kHz) ou *bande étroite* (8 kHz)

Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).
-   L'en-tête `Content-Type` des méthodes `recognize` prend désormais en charge les fichiers `audio/wav` pour le format de fichier Waveform Audio File Format (WAV), en plus de `audio/flac` et `audio/l16`. Pour plus d'informations sur les formats audio pris en charge, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Les méthodes `recognize` prennent désormais en charge un certain nombre de paramètres de requête que vous pouvez utiliser pour personnaliser le service en fonction des besoins de votre application :
    -   Le paramètre `inactivity_timeout` définit, en secondes, la valeur du délai au bout duquel le service ferme la connexion s'il détecte du silence (aucune parole) en mode diffusion en continu. Par défaut, le service met fin à la session au bout de 30 secondes de silence. Pour plus d'informations, voir [Délais d'attente](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts). 
    -   Le paramètre `max_alternatives` demande au service de renvoyer les *n* meilleures hypothèses de substitution pour la transcription audio. Pour plus d'informations, voir [Nombre maximal d'alternatives](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives).
    -   Le paramètre `word_confidence` demande au service de renvoyer une cote de confiance pour chaque mot de la transcription. Pour plus d'informations, voir [Confiance des mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence).
    -   Le paramètre `timestamps` demande au service de renvoyer le moment de début et de fin par rapport au début audio de chaque mot de la transcription. Pour plus d'informations, voir [Horodatage des mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps).
-   La méthode `GET /v1/sessions/{session_id}/observeResult` est désormais nommée `GET /v1/sessions/{session_id}/observe_result`. Le nom `observeResult` est toujours pris en charge pour la compatibilité avec les versions antérieures.
-   Les méthodes `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize` et `POST /v1/recognize` comprennent désormais le paramètre d'en-tête `X-WDC-PL-OPT-OUT` pour contrôler si le service utilise des données audio et de transcription d'une demande afin d'améliorer les futurs résultats. L'interface WebSocket comprend un paramètre de requête équivalent. Indiquez la valeur `1` pour empêcher le service d'utiliser des résultats d'audio et de transcription. Le paramètre s'applique uniquement à la demande en cours. Le nouvel en-tête remplace l'en-tête `X-logging` utilisé dans la version d'API bêta. Voir [Contrôle de la journalisation des demandes pour les services {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-logging-overview).
-   Le service a désormais une limite de 100 Mo de données par session en mode diffusion en continu. Le mode diffusion en continu est spécifié en indiquant la valeur `chunked` avec l'en-tête `Transfer-Encoding`. La diffusion ponctuelle d'un fichier audio impose toujours une taille limite de 4 Mo pour les données envoyées. Pour plus d'informations, voir [Transmission audio](/docs/services/speech-to-text?topic=speech-to-text-input#transmission).
-   Pour les méthodes `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize` et `/v1/recognize`, le code 415 ("Unsupported Media Type") est ajouté.
-   Pour les demandes `POST` et `GET` vers la méthode `/v1/sessions/{session_id}/recognize`, les codes d'erreur suivants ont été modifiés :
    -   Code d'erreur 404 ("Session_id not found") a un message plus descriptif (`POST` et `GET`).
    -   Code d'erreur 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") a un message plus descriptif (`POST` uniquement).
-   Pour les demandes HTTP `POST` vers les méthodes `/v1/sessions` et `/v1/recognize`, le code d'erreur 503 ("Service Unavailable") peut être renvoyé. Ce code d'erreur peut également être renvoyé lors de la création d'une connexion WebSocket avec la méthode `/v1/recognize`.
