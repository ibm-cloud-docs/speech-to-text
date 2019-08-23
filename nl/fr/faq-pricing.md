---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

subcollection: speech-to-text

---

{:faq: .faq}
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

# Foire aux questions liées à la tarification
{: #faq-pricing}

## Quel est le tarif d'utilisation pour le plan Lite {{site.data.keyword.speechtotextshort}} ?
{: #faq-pricing-zero}
{: faq}

Le plan Lite vous permet de commencer avec 500 minutes de reconnaissance vocale par mois sans frais. Le plan Lite ne fournit pas d'accès aux interfaces de personnalisation du service. Pour plus d'informations, voir la [page de tarification](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} du service {{site.data.keyword.speechtotextshort}}.


## Quel est le tarif d'utilisation pour le plan Standard {{site.data.keyword.speechtotextshort}} ?
{: #faq-pricing-one}
{: faq}

Le tarif du plan Standard est de 0,02 USD par minute de reconnaissance vocale. Ce tarif s'applique à la fois aux modèles à large bande et à bande étroite. Le plan utilise un modèle de tarification différenciée qui vous permet d'utiliser les interfaces de personnalisation moyennant des frais supplémentaires. Pour plus d'informations, voir la [page de tarification](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} du service {{site.data.keyword.speechtotextshort}}.


## Que signifie "tarification à la minute" ?
{: #faq-pricing-two}
{: faq}

Le prix est basé sur la quantité (nombre de minutes) d'appels envoyés au service. Le prix ne dépend pas de la durée nécessaire au service pour traiter l'appel.

## Arrondissez-vous chaque appel d'API à la minute supérieure ?
{: #faq-pricing-three}
{: faq}

{{site.data.keyword.IBM_notm}} n'arrondit pas la durée audio pour chaque appel d'API reçu par le service. {{site.data.keyword.IBM_notm}} regroupe toutes les utilisations effectuées dans le mois et arrondit le résultat à la minute supérieure à la fin du mois. Par exemple, si vous envoyez deux fichiers audio de 30 secondes chacun, {{site.data.keyword.IBM_notm}} additionne la durée des fichiers audio, soit une minute, et facture 0,02 USD.

## Comment fonctionne la tarification différenciée progressive basée sur le volume ?
{: #faq-pricing-four}
{: faq}

Le modèle de tarification différenciée est conçu pour permettre des remises supplémentaires pour les clients utilisant de gros volumes, au fur et à mesure de leur utilisation du service. La tarification par minute est réduite pour les minutes d'audio supplémentaires, dès que certains seuils correspondant à un total audio mensuel sont atteints. Pour plus d'informations, voir la [page de tarification](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} du service.

## Pour la tarification différenciée, quel sera mon coût total si j'ai utilisé le service pour transcrire, par exemple, 275 mille minutes d'audio dans le mois ?
{: #faq-pricing-five}
{: faq}

Pour les 250 mille premières minutes d'audio, vous serez facturé 0,02 USD/minute : 250 000 * 0,02 = 5000,00 USD. Pour les 25 mille minutes d'audio restantes, vous serez facturé au taux réduit de 0,015 USD/minute : 25 000 * 0,015 = 375,00 USD. Dans ce cas, votre coût total mensuel s'élève à 5375,00 USD.

## De quel plan de tarification ai-je besoin pour utiliser l'interface de personnalisation du service ?
{: #faq-pricing-six}
{: faq}

Vous devez disposer du plan de tarification standard pour pouvoir utiliser la personnalisation du modèle de langue ou du modèle acoustique. Les utilisateurs du plan Lite ne peuvent pas utiliser l'interface de personnalisation. Pour plus d'informations, voir la [page de tarification](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} du service {{site.data.keyword.speechtotextshort}}.


## Quel est le tarif d'utilisation pour l'interface de personnalisation du service ?
{: #faq-pricing-seven}
{: faq}

Pour la *personnalisation de modèle de langue*, {{site.data.keyword.IBM_notm}} ne facture pas la création et l'hébergement d'un modèle de langue personnalisé, mais l'utilisation du modèle avec une demande de reconnaissance. L'utilisation d'un modèle de langue personnalisé dans le cadre d'une transcription induit un coût supplémentaire de 0,03 $ (USD) par minute. Ce coût s'ajoute aux frais d'utilisation standard de 0,02 $ (USD) par minute. Il s'applique à toutes les langues prises en charge par l'interface de personnalisation. Par conséquent, le coût total d'utilisation d'un modèle de langue personnalisé pour la reconnaissance vocale s'élève à 0,05 $ (USD) par minute.

Pour la *personnalisation de modèle acoustique*, qui est une interface bêta pour toutes les langues prises en charge, {{site.data.keyword.IBM_notm}} ne facture pas la création et l'hébergement d'un modèle acoustique personnalisé ou son utilisation dans le cadre de la reconnaissance vocale. L'utilisation gratuite des modèles acoustiques personnalisés pour les demandes de reconnaissance est susceptible d'évoluer ultérieurement.
