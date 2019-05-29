---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-03"

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

# A propos de
{: #about}

**Mise à jour du service :** *le service {{site.data.keyword.speechtotextshort}} a été mis à jour le 3 avril 2019. Ce service vous permet désormais d'ajouter 200 heures d'audio à un modèle acoustique personnalisé. Pour plus d'informations, voir la [mise à jour du service datée du 3 avril 2019](/docs/services/speech-to-text/release-notes.html#April2019) dans les notes sur l'édition*.

Le service {{site.data.keyword.speechtotextfull}} fournit des fonctions de transcription vocale pour vos applications. Le service tire parti de l'apprentissage automatique pour combiner les connaissances grammaticales et morphologiques du langage et la composition des signaux audio et vocaux afin d'obtenir une transcription précise de la voix humaine. Il met à jour et affine sa transcription en mode continu à mesure qu'il reçoit les paroles.
{: shortdesc}

Le service fournit différentes interfaces qui lui permettent de s'adapter à toute application avec des paroles en entrée et une retranscription textuelle en sortie. Exemples d'applications :

-   Contrôle vocal des applications, des appareils embarqués et des accessoires automobiles
-   Transcription des réunions et des conférences téléphoniques
-   Dictée de courriers électroniques et de notes

## Interfaces prises en charge
{: #interfaces}

Le service {{site.data.keyword.speechtotextshort}} offre trois interfaces pour la reconnaissance vocale :

-   Une [interface WebSocket](/docs/services/speech-to-text/websockets.html) pour établir des connexions permanentes, en duplex intégral et à faible temps d'attente avec le service. Vous pouvez transmettre une quantité maximale de 100 Mo de données audio au service avec une seule demande.
-   Une [interface HTTP synchrone](/docs/services/speech-to-text/http.html) pour les appels HTTP de base au service. Vous pouvez transmettre une quantité maximale de 100 Mo de données audio avec une demande.
-   Une [interface HTTP asynchrone](/docs/services/speech-to-text/async.html) pour les appels non bloquants au service. Vous pouvez transmettre jusqu'à 1 Go de données audio avec une demande.

Le service propose également une [interface de personnalisation](/docs/services/speech-to-text/custom.html) que vous pouvez utiliser pour optimiser la reconnaissance vocale en fonction de votre langue et en tenant compte de vos exigences acoustiques. Vous pouvez enrichir le vocabulaire d'un modèle avec une terminologie spécifique à un domaine ou adapter un modèle aux caractéristiques acoustiques de vos données audio. Vous pouvez également ajouter des [grammaires](/docs/services/speech-to-text/grammar.html) pour limiter les expressions reconnaissables par le service.

-   Pour obtenir une description détaillée du développement d'application avec le service, voir [Présentation destinée aux développeurs](/docs/services/speech-to-text/developer-overview.html).
-   Pour obtenir des exemples de demandes de reconnaissance vocale de base pour chacune des interfaces du service, voir [Effectuer une demande de reconnaissance](/docs/services/speech-to-text/basic-request.html).

Des logiciels SDK sont disponibles dans de nombreux langages de programmation pour vous faciliter la tâche lorsque vous utilisez le service. Pour plus d'informations, voir [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Fonctions d'entrée
{: #inputFeatures}

Les interfaces du service partagent des fonctions d'entrée communes pour la transcription parole-texte :

-   [Formats audio](/docs/services/speech-to-text/audio-formats.html) - Vous pouvez transcrire des données audio au format Ogg ou WebM (Web Media) avec le codec Opus ou Vorbis, MP3 (ou MPEG), Waveform Audio File Format (WAV), Free Lossless Audio Codec (FLAC), PCM (Pulse-Code Modulation) linéaire 16 bits, G.729, A-Law, mu-law (ou u-law) et basic audio. En utilisant un format prenant en charge la compression, vous pouvez maximiser la quantité de données audio que vous pouvez envoyer avec une demande. 
-   [Langues et modèles](/docs/services/speech-to-text/models.html) - Pour la plupart des langues, vous pouvez transcrire des données audio en utilisant des modèles à large bande ou à bande étroite. Les modèles à large bande conviennent aux données audio échantillonnées à une fréquence minimale de 16 kHz. Utilisez des modèles à bande étroite pour les données audio échantillonnées à une fréquence minimale de 8 kHz.
-   [Transmission audio](/docs/services/speech-to-text/input.html#transmission) - Vous pouvez effectuer une transmission audio en diffusion continue de blocs de données ou en diffusion ponctuelle transmettant la totalité des données en une seule fois. Avec la diffusion en continu, le service impose des [délais d'attente](/docs/services/speech-to-text/input.html#timeouts) d'inactivité et de session.

## Fonctions de sortie
{: #outputFeatures}

Les interfaces prennent également en charge les fonctions de sortie communes suivantes :

-   Les [étiquettes de locuteur (speaker labels)](/docs/services/speech-to-text/output.html#speaker_labels) reconnaissent différents locuteurs pour les données audio en anglais américain, en anglais britannique, en espagnol ou en japonais. La transcription identifie les contributions de chaque locuteur d'une conversation à plusieurs intervenants. (Fonctionnalité bêta.)
-   La [détection de mots clés (keyword spotting)](/docs/services/speech-to-text/output.html#keyword_spotting) identifie des expressions parlées correspondant à des chaînes de mots clés spécifiées selon un niveau de confiance défini par l'utilisateur. La détection de mots clés est particulièrement utile lorsque des expressions individuelles dans les données audio sont plus importantes que la transcription globale. Par exemple, un système de support clientèle peut identifier les mots clés permettant de déterminer comment acheminer les demandes des utilisateurs.
-   Les [résultats intermédiaires (interim results)](/docs/services/speech-to-text/output.html#interim) renvoient des hypothèses évolutives au fur et à mesure des progrès de la transcription. Le service renvoie les résultats finaux lorsque la transcription est terminée. Les résultats intermédiaires sont disponibles uniquement avec l'interface WebSocket.
-   Le [nombre maximal d'alternatives (max alternatives)](/docs/services/speech-to-text/output.html#max_alternatives) fournit d'autres retranscriptions possibles. Le service indique les résultats finaux auxquels il accorde le plus haut niveau de confiance.
-   Les [autres propositions de mots (word alternatives)](/docs/services/speech-to-text/output.html#word_alternatives) demandent d'autres propositions de mots avec des caractéristiques acoustiques similaires aux mots d'une retranscription.
-   Le [confiance des mots (word confidence)](/docs/services/speech-to-text/output.html#word_confidence) renvoie des niveaux de confiance pour chaque mot d'une retranscription.
-   Les [horodatages de mots (word timestamps)](/docs/services/speech-to-text/output.html#word_timestamps) renvoient des horodatages pour le début et la fin de chaque mot d'une retranscription.
-   Le [formatage intelligent (smart formatting)](/docs/services/speech-to-text/output.html#smart_formatting) convertit des dates, des heures, des nombres, des valeurs de devise, des numéros de téléphone et des adresses Internet sous forme plus lisible et plus conventionnelle dans les retranscriptions finales. Pour l'anglais américain, vous pouvez également fournir des groupes de mots clés pour inclure certains signes de ponctuation dans les retranscriptions finales. Le formatage intelligent est pris en charge pour les données audio en anglais américain, en japonais et en espagnol. (Fonctionnalité bêta.)
-   L'[occultation numérique (numeric redaction)](/docs/services/speech-to-text/output.html#redaction) occulte, ou masque, les données numériques dans une retranscription finale. L'occultation est conçue pour supprimer des informations personnelles sensibles, comme les numéros de carte de crédit, dans les retranscriptions. Cette fonction est prise en charge pour les données audio en anglais américain, en japonais et en coréen. (Fonctionnalité bêta.)
-   Le [filtrage des grossièretés (profanity filtering)](/docs/services/speech-to-text/output.html#profanity_filter) censure les grossièretés des retranscriptions en anglais américain.

## Support de langue
{: #languages}

Le service offre des modèles pour les langues suivantes : allemand, anglais britannique, anglais américain, arabe standard moderne, chinois mandarin, coréen, espagnol, français, japonais et portugais brésilien. Le service ne prend pas en charge toutes les fonctions pour toutes les langues. Cependant, il prend en charge certaines fonctions en version GA pour une utilisation en production et d'autres en tant qu'offres en version bêta pour les différentes langues.

-   Les interfaces WebSocket et HTTP sont disponibles en version GA pour toutes les langues.
-   Le service offre des modèles à large bande, bande étroite ou les deux pour les différentes langues. Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text/models.html).
-   Certaines fonctions de reconnaissance vocale sont disponibles uniquement pour certaines langues. Pour plus d'informations, voir [Récapitulatif des paramètres](/docs/services/speech-to-text/summary.html).
-   L'interface de personnalisation de modèle de langue est disponible en version GA pour la plupart des langues. L'interface de personnalisation de modèle acoustique est disponible en fonctionnalité bêta pour toutes les langues. Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text/custom.html#languageSupport).

## Tarification
{: #pricing-index}

Pour plus d'informations sur les plans de tarification du service, voir le service {{site.data.keyword.speechtotextshort}} dans le [Catalogue {{site.data.keyword.Bluemix_short}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/catalog/services/speech-to-text){: new_window}. Pour trouver les réponses aux questions qui ont trait à la tarification et obtenir des exemples de coûts d'utilisation mensuelle, voir [Foire aux questions liées à la tarification](/docs/services/speech-to-text/faq-pricing.html).

## Expérimentation du service
{: #tryOut}

Pour obtenir des exemples de fonctionnement du service {{site.data.keyword.speechtotextshort}}, voir

-   Une [démonstration rapide ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://speech-to-text-demo.ng.bluemix.net/){: new_window} qui transcrit le texte d'une entrée audio en diffusion en continu ou d'un fichier que vous téléchargez.
-   Les applications dans les [Kits de démarrage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} d'{{site.data.keyword.ibmwatson}} qui proposent une démonstration du service.
-   L'article de blogue {{site.data.keyword.watson}} [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window} qui montre comment utiliser l'interface WebSocket du service avec Python pour extraire des paroles d'une séquence audio. Cet article fournit un tutoriel complet qui présente le code et la procédure à suivre.
