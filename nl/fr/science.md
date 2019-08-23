---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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

# Contexte scientifique du service
{: #science}

Comme le décrit [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external}, {{site.data.keyword.IBM}} est à la pointe de la recherche sur la reconnaissance vocale depuis le début des années 60. Par exemple, le document [Bahl, Jelinek et Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) présente l'approche mathématique de base de la reconnaissance vocale employée dans la quasi-totalité des systèmes de reconnaissance vocal modernes. Et le document [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) décrit la création du premier système de reconnaissance vocale en temps réel à vocabulaire étendu pour la dictée. Il décrit également les problèmes qui constituent encore des thèmes de recherche à résoudre à l'heure actuelle.
{: shortdesc}

{{site.data.keyword.IBM_notm}} poursuit cette riche tradition de recherche et de développement avec le service {{site.data.keyword.speechtotextfull}}. {{site.data.keyword.IBM_notm}} a démontré un record de précision de la reconnaissance vocale dans l'industrie sur les ensembles de données de référence publics pour la transcription des conversations téléphoniques ([Saon and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) et des informations audiovisuelles ([Thomas and others, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)). En plus de démontrer l'efficacité de la modélisation acoustique, {{site.data.keyword.IBM_notm}} a exploité des réseaux de neurones pour modéliser la langue ([Kurata and others, 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) et [Kurata and others, 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)).

Les annonces suivantes résument les réalisations récentes d'{{site.data.keyword.IBM_notm}} en matière de reconnaissance vocale :

-   [Atteinte de nouveaux records en matière de reconnaissance vocale](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} établit un record de l'industrie pour la reconnaissance vocale conversationnelle en développant des technologies d'apprentissage en profondeur](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} établit de nouvelles performances en matière de transcription pour le sous-titrage automatique du journal télévisé](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

Ces réalisations contribuent à faire progresser les services vocaux d'{{site.data.keyword.IBM_notm}}. Parmi les idées récentes qui correspondent le mieux au service cloud {{site.data.keyword.speechtotextshort}} :

-   *Pour la modélisation de la langue, * {{site.data.keyword.IBM_notm}} exploite un modèle de langue basé sur un réseau de neurones pour générer un texte d'entraînement ([Suzuki and others, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *Pour la modélisation acoustique, * {{site.data.keyword.IBM_notm}} utilise un modèle assez compact pour s'adapter aux limitations de ressources du cloud. Pour entraîner ce modèle compact, {{site.data.keyword.IBM_notm}} utilise "l'entraînement enseignant-élève / la distillation des connaissances". Les réseaux de neurones les plus importants et puissants tels que LSTM (Long Short-Term Memory), VGG et ResNet (Residual Network) sont les premiers à être entraînés. La sortie de ces réseaux est ensuite utilisée comme signaux d'enseignement pour entraîner un modèle compact pour le déploiement réel ([Fukuda and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)).

Pour repousser encore les limites, {{site.data.keyword.IBM_notm}} se concentre également sur la modélisation de bout en bout. Par exemple, un solide pipeline de modélisation a été mis en place pour les modèles acoustiques-mots directs ([Audhkhasi and others, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) et [Audhkhasi and others, 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) qui s'améliore encore ([Saon and others, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)). Des efforts sont également fait pour créer des modèles de bout en bout compacts pour un déploiement ultérieur sur le cloud ([Kurata and Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)).

Pour plus d'informations sur la recherche scientifique à l'origine de ce service, voir les documents répertoriés dans [Références de recherche](/docs/services/speech-to-text?topic=speech-to-text-references).
