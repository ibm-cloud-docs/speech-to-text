---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Sécurité des informations
{: #information-security}

{{site.data.keyword.IBM}} s'engage à fournir à nos clients et partenaires des solutions innovantes en termes de confidentialité, de sécurité et de gouvernance des données.
{: shortdesc}

Il incombe aux clients de veiller à leur propre conformité aux différentes lois et réglementations, y compris au Règlement général sur la protection des données (RGPD) de l'Union européenne. Il est de la responsabilité des clients de faire appel à un conseiller juridique compétent pour identifier et interpréter les lois et réglementations applicables qui pourraient affecter leurs opérations et toutes les actions qu'ils pourraient être amenés à entreprendre pour se conformer à ces lois et réglementations.
{: important}

Les produits, services et autres fonctionnalités décrits dans le présent document ne conviennent pas à toutes les situations client et leur disponibilité peut être limitée. {{site.data.keyword.IBM_notm}} ne donne aucun avis juridique, comptable ou d'audit et ne garantit pas que ses produits ou services assurent la conformité de ses clients par rapport aux lois et réglementations applicables.

Si vous avez besoin d'une assistance RGPD pour les ressources {{site.data.keyword.cloud}} {{site.data.keyword.watson}} créées

-   dans l'Union européenne (UE), voir [Demande de support pour les ressources {{site.data.keyword.cloud_notm}} Watson créées dans l'Union européenne](/docs/services/watson?topic=watson-gdpr-sar#request-EU).
-   hors UE, voir [Demande de support pour les ressources situées en dehors de l'Union européenne](/docs/services/watson?topic=watson-gdpr-sar#request-non-EU).

## Règlement général sur la protection des données (RGPD) de l'Union Européenne
{: #gdpr}

{{site.data.keyword.IBM_notm}} s'engage à fournir à nos clients et partenaires des solutions innovantes en termes de confidentialité, de sécurité et de gouvernance des données pour les aider dans leur parcours de conformité avec le RGPD.

Pour en savoir plus sur le propre parcours de mise en conformité avec le RGPD d'{{site.data.keyword.IBM_notm}}, ainsi que sur nos fonctions et offres liées au GDPR pour soutenir votre parcours de conformité, [cliquez ici](http://www.ibm.com/gdpr){: external}.

## Etiquetage et suppression de données dans le service Speech to Text
{: #gdpr-speech-to-text}

Le service {{site.data.keyword.speechtotextfull}} vous permet de supprimer toutes les données associées aux demandes de reconnaissance, aux modèles de langue personnalisés et aux modèles acoustiques personnalisés. Pour supprimer les données, vous devez effectuer les opérations suivantes :

1.  Utilisez l'en-tête `X-Watson-Metadata` pour associer un ID client (customer_id) aux données transmises par une demande au service, voir [Spécification d'un ID client](#specify-customer-id).
1.  Utilisez la méthode `DELETE /v1/user_data` pour supprimer toutes les données associées à un ID client spécifié, voir [Suppression de données](#delete-pi).

Par défaut, aucun ID client n'est associé aux données.

Les fonctions expérimentales et les fonctions bêta ne sont pas conçues pour être utilisées dans un environnement de production et par conséquent ces fonctions ne fonctionneront pas forcément comme prévu lors de l'étiquetage ou de la suppression des données. Les fonctions expérimentales et bêta ne doivent pas être utilisées lors de l'implémentation d'une solution nécessitant l'étiquetage et la suppression de données.
{: note}

### Spécification d'un ID client
{: #specify-customer-id}

Pour associer un ID client aux données, incluez l'en-tête `X-Watson-Metadata` avec la demande qui transmet les informations. Transmettez la chaîne `customer_id={id}` en tant qu'argument de l'en-tête. L'exemple suivant associe l'ID client `my_customer_ID` aux données transmises avec une demande `POST /v1/recognize` :

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Un ID client peut inclure tous les caractères à l'exception du point-virgule (`;`) et du signe égal (`=`). Indiquez une chaîne aléatoire ou générique pour l'ID client. N'indiquez pas une chaîne contenant des renseignements personnels identifiables, comme par exemple une adresse e-mail ou un ID Twitter. Vous pouvez indiquer différents ID client avec différentes demandes. Un ID client que vous indiquez est associé à l'instance du service dont les données d'identification sont utilisées avec la demande ; seules les données d'identification de cette instance du service peuvent supprimer les données associées à l'ID.

Utilisez l'en-tête `X-Watson-Metadata` avec les méthodes suivantes :

-   Avec des demandes WebSocket :
    -   `/v1/recognize`

    Vous spécifiez l'ID client avec le paramètre de requête `x-watson-metadata` de la demande pour ouvrir la connexion. Vous devez coder l'argument dans le paramètre de requête en codage URL, par exemple, `customer_id%3dmy_customer_ID`. L'ID client est associé à toutes les données transmises avec les demandes de reconnaissance envoyées via la connexion.
-   Avec des demandes HTTP synchrones :
    -   `POST /v1/recognize`

    L'ID client est associé aux données envoyées avec la demande individuelle.
-   Avec des demandes HTTP asynchrones :
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    L'ID client est associé à une URL de rappel en liste blanche ou aux données envoyées avec la demande de reconnaissance individuelle.
-   Avec des demandes d'ajout de corpus, de mots personnalisés ou de grammaires à des modèles de langue personnalisés :
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    L'ID client est associé aux corpus, aux mots personnalisés ou aux grammaires qui sont ajoutés ou mis à jour par la demande.
-   Avec des demandes d'ajout de ressources audio à des modèles acoustiques personnalisés :
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    L'ID client est associé à la ressource audio qui est ajoutée ou mise à jour par la demande.

### Suppression de données
{: #delete-pi}

Pour supprimer toutes les données associées à un ID client, utilisez la méthode `DELETE /v1/user_data`. Vous transmettez la chaîne `customer_id={id}` en tant que paramètre de requête avec la demande. L'exemple suivant supprime toutes les données de l'ID client `my_customer_ID` :

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

La méthode `/v1/user_data` supprime toutes les données associées à l'ID client spécifié, quelle que soit la méthode employée pour ajouter les informations. La méthode n'a aucun effet si aucune donnée n'est associée à l'ID client. Vous devez émettre la demande avec les données d'identification de la même instance de service ayant été utilisée pour associer l'ID client aux données.
