---

copyright:
  years: 2015, 2019
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

# Foire aux questions liées à la tarification
{: #faq-pricing}

1.  <span style="color:#003F69">Combien coûte l'utilisation du service standard {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} ?</span>

    Le tarif du service {{site.data.keyword.speechtotextshort}} est fixé à 0,02 $ (USD) par minute. Ce tarif s'applique à la fois aux modèles à large bande et à bande étroite. Pour plus d'informations, voir la [page d'arrivée du service {{site.data.keyword.speechtotextshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}.

1.  <span style="color:#003F69">Que signifie une "tarification par minute" ? Est-ce que le nombre de minutes audio que j'envoie au service ou que le service nécessite pour traiter l'audio est facturé ?</span>

    Le tarif est basé sur la quantité de données audio envoyées au service et non pas sur la durée du traitement de ces données par le service.

1.  <span style="color:#003F69">Arrondissez-vous chaque appel d'API à la minute supérieure ? Par exemple, si j'envoie deux fichiers audio de 30 secondes chacun, suis-je facturé pour deux minutes ou pour une minute ?</span>

    {{site.data.keyword.IBM_notm}} n'arrondit pas la durée audio pour chaque appel d'API reçu par le service. {{site.data.keyword.IBM_notm}} regroupe toutes les utilisations effectuées dans le mois et arrondit le résultat à la minute supérieure à la fin du mois. Dans cet exemple, si vous envoyez deux fichiers audio de 30 secondes chacun, {{site.data.keyword.IBM_notm}} additionne la durée des fichiers audio, soit une minute et facture 0,02 $ (USD).

1.  <span id="graduated" style="color:#003F69">Comment fonctionne la tarification différenciée progressive basée sur le volume ?</span>

    Le modèle de tarification différenciée est conçu pour permettre des remises supplémentaires pour les clients utilisant de gros volumes, au fur et à mesure de leur utilisation du service. La tarification par minute est réduite pour les minutes d'audio supplémentaires, dès que certains seuils correspondant à un total audio mensuel sont atteints. Pour plus d'informations, voir la [page de tarification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/catalog/services/speech-to-text){: new_window} du service.

1.  <span style="color:#003F69">Pour la tarification différenciée, quel sera mon coût total si j'ai utilisé le service pour transcrire, par exemple, 275 mille minutes d'audio dans le mois ?</span>

    -   Pour les 250 mille premières minutes d'audio, vous serez facturé 0,02 $ (USD) / minute : 250 000 * 0,02 $ = 5000,00 $ (USD).
    -   Pour les 25 mille minutes d'audio supplémentaires, vous serez facturé au taux réduit de 0,015 $ (USD) / minute : 25 000 * 0,015 $ = 375,00 $ (USD).
    -   Dans ce scénario, votre coût total mensuel s'élève à 5375,00 $ (USD).

1.  <span style="color:#003F69">Quel est le tarif d'utilisation pour l'interface de personnalisation du service ? Suis-je facturé pour la création et l'hébergement d'un modèle personnalisé ?</span>

    Pour la *personnalisation de modèle de langue*, {{site.data.keyword.IBM_notm}} ne facture pas la création et l'hébergement d'un modèle de langue personnalisé, mais l'utilisation du modèle avec une demande de reconnaissance. L'utilisation d'un modèle de langue personnalisé dans le cadre d'une transcription induit un coût supplémentaire de 0,03 $ (USD) par minute. Ce coût s'ajoute aux frais d'utilisation standard de 0,02 $ (USD) par minute. Il s'applique à toutes les langues prises en charge par l'interface de personnalisation. Par conséquent, le coût total d'utilisation d'un modèle de langue personnalisé pour la reconnaissance vocale s'élève à 0,05 $ (USD) par minute.

    Pour la *personnalisation de modèle acoustique*, qui est une interface bêta pour toutes les langues prises en charge, {{site.data.keyword.IBM_notm}} ne facture pas la création et l'hébergement d'un modèle acoustique personnalisé ou son utilisation dans le cadre de la reconnaissance vocale. L'utilisation gratuite des modèles acoustiques personnalisés pour les demandes de reconnaissance est susceptible d'évoluer ultérieurement.
