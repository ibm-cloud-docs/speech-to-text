---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-06"

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

# Utilisation des corpus et des mots personnalisés
{: #corporaWords}

Vous pouvez alimenter un modèle de langue personnalisé avec des mots en ajoutant des corpus ou des grammaires au modèle, ou en ajoutant directement des mots personnalisés :
{: shortdesc}

-   **Corpus :** le moyen recommandé pour alimenter un modèle de langue personnalisé avec des mots est d'ajouter un ou plusieurs corpus au modèle. Lorsque vous ajoutez un corpus, le service analyse le fichier et ajoute automatiquement les mots nouveaux qu'il détecte au modèle personnalisé. L'ajout d'un corpus à un modèle personnalisé permet au service d'extraire des mots spécifiques à un domaine en contexte, ce qui permet de garantir de meilleurs résultats de transcription. Pour plus d'informations, voir [Utilisation des corpus](#workingCorpora).
-   **Grammaires :** vous pouvez ajouter des grammaires à un modèle personnalisé pour limiter la reconnaissance vocale aux mots ou expressions qui sont reconnus par une grammaire. Lorsque vous ajoutez une grammaire à un modèle, le service ajoute automatiquement les mots nouveaux qu'il détecte au modèle, de la même manière que pour les corpus. Pour plus d'informations, voir [Utilisation de grammaires avec des modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-grammars).
-   **Mots individuels :** vous pouvez également ajouter directement des mots personnalisés individuels à un modèle. Le service ajoute les mots au modèle de la même manière que lorsqu'il les détecte dans les corpus et les grammaires. Lorsque vous ajoutez un mot directement, vous pouvez spécifier plusieurs prononciations et indiquer comment le mot doit s'afficher. Vous pouvez également mettre à jour des mots existants pour modifier ou compléter les définitions qui étaient extraites des corpus ou des grammaires. Pour plus d'informations, voir [Utilisation des mots personnalisés](#workingWords).

Quel que soit le mode d'ajout employé, le service stocke tous les mots que vous ajoutez dans un modèle de langue personnalisé dans la ressource de mots du modèle.

## Ressource de mots
{: #wordsResource}

La ressource de mots (*words resource*) comprend tous les mots que vous ajoutez à partir des corpus, des grammaires ou directement. Son but est de définir la prononciation et l'orthographe des mots qui ne sont pas déjà présents dans le vocabulaire de base du service. Les définitions indiquent au service comment transcrire ces *mots OOV (Out-Of-Vocabulary)*.

La ressource de mots contient les informations suivantes sur chacun de ces mots OOV. Le service crée les définitions des mots extraits des corpus et des grammaires. Vous spécifiez les caractéristiques des mots que vous ajoutez ou modifiez directement.

-   `word` : orthographe du mot telle qu'elle apparaît dans un corpus ou une grammaire ou telle que vous l'avez ajoutée.
-   `sounds_like` : prononciation du mot. Pour les mots extraits de corpus et de grammaires, cette valeur représente la prononciation du mot déterminée par le service en fonction de ses règles linguistiques. Dans de nombreux cas, la prononciation correspond à l'orthographe de la zone `word`.

    Vous pouvez utiliser la zone `sounds_like` pour modifier la prononciation du mot. Vous pouvez également utiliser cette zone pour spécifier plusieurs prononciations d'un mot. Pour plus d'informations, voir [Utilisation de la zone sounds_like](#soundsLike).
-   `display_as` : orthographe du mot utilisée par le service dans les retranscriptions. Cette zone indique comment le mot doit être affiché. Dans la plupart des cas, l'orthographe correspond à la valeur de la zone `word`.

    Vous pouvez utiliser la zone `display_as` pour indiquer une autre orthographe du mot. Pour plus d'informations, voir [Utilisation de la zone display_as](#displayAs).
-   `source` : indique comment le mot a été ajouté à la ressource de mots. Si le service a extrait le mot à partir d'un corpus ou d'une grammaire, la zone indique le nom de cette ressource. Comme le service peut rencontrer le même mot dans plusieurs ressources, cette zone peut répertorier plusieurs noms de corpus ou de grammaire. Cette zone comprend la chaîne `user` si vous ajoutez ou modifiez le mot directement.

Lorsque vous mettez à jour la ressource de mots d'un modèle avec la méthode de votre choix, vous devez entraîner le modèle pour que les modifications soient appliquées lors de la transcription. Pour plus d'informations, voir [Entraînement du modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

## De quelle quantité de données ai-je besoin ?
{: #wordsResourceAmount}

De nombreux facteurs contribuent à déterminer la quantité de données dont vous avez besoin pour un modèle de langue personnalisé efficace. Il n'est pas possible de fournir le nombre exact de mots que vous devez ajouter pour une application ou un modèle de langue personnalisé.

En fonction du cas d'utilisation, même l'ajout direct de quelques mots à un modèle personnalisé suffit à améliorer la qualité du modèle. Mais l'ajout de mots ne faisant pas partie du vocabulaire de base (mots OOV) à partir d'un corpus qui présente les mots dans leur contexte d'utilisation en mode audio peut améliorer considérablement l'exactitude de la transcription. Pour plus d'informations, voir [Utilisation des corpus](#workingCorpora).

Le service limite le nombre de mots que vous pouvez ajouter à un modèle de langue personnalisé :

-   Vous pouvez ajouter 90 mille mots OOV maximum à la ressource de mots d'un modèle personnalisé. Ce nombre inclut les mots OOV de toutes les sources (corpus, grammaires et mots personnalisés individuels que vous ajoutez directement).
-   Vous pouvez ajouter un total de 10 millions maximum à un modèle personnalisé à partir de toutes les sources. Ce nombre inclut tous les mots, OOV et les mots faisant déjà partie du vocabulaire de base du service, qui figurent dans les corpus et les grammaires. Pour les corpus, le service utilise ces mots supplémentaires pour connaître le contexte dans lequel peuvent apparaître des mots OOV, c'est la raison pour laquelle les corpus constituent un moyen plus efficace d'améliorer la précision de la reconnaissance.

Une ressource de mots volumineuse peut augmenter le temps d'attente de la reconnaissance vocale mais il est difficile de quantifier ou de prévoir exactement l'effet produit. Comme pour la quantité de données nécessaire pour obtenir un modèle personnalisé efficace, l'impact sur les performances d'une ressource de mots volumineuse dépend de plusieurs facteurs. Testez votre modèle personnalisé avec différentes quantités de données pour déterminer les performances de vos modèles et de vos données.

## Utilisation des corpus
{: #workingCorpora}

Vous utilisez la méthode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` pour ajouter un corpus à un modèle personnalisé. Un corpus est un fichier en texte brut qui contient des exemples de phrases tirés de votre domaine. L'exemple suivant présente un corpus abrégé dans le domaine de la santé. Un fichier de corpus est en principe beaucoup plus long.

```
Am I at risk for health problems during travel?
Some people are more likely to have health problems when traveling outside the United States.
How Is Coronary Microvascular Disease Treated?
If you're diagnosed with coronary MVD and also have anemia, you may benefit from treatment for that condition.
Anemia is thought to slow the growth of cells needed to repair damaged blood vessels.
What causes autoimmune hepatitis?
A combination of autoimmunity, environmental triggers, and a genetic predisposition can lead to autoimmune hepatitis.
What research is being done for Spinal Cord Injury?
The National Institute of Neurological Disorders and Stroke NINDS conducts spinal cord research in its laboratories at the National Institutes of Health NIH.
NINDS also supports additional research through grants to major research institutions across the country.
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

La reconnaissance vocale dépend des algorithmes statistiques utilisés pour l'analyse audio. Les mots d'un modèle personnalisé sont en concurrence avec les mots du vocabulaire de base du service, ainsi que d'autres mots du modèle. (Des facteurs, tels que des bruits audio et les accents des locuteurs peuvent aussi affecter la qualité de transcription.)

L'exactitude de la transcription peut dépendre largement du mode de définition des mots dans un modèle et de la façon dont les locuteurs les prononcent. Pour améliorer la précision du service, utilisez des corpus pour fournir autant d'exemples que possible de l'utilisation des mots OOV dans le domaine. La répétition des mots OOV dans les corpus peut améliorer la qualité du modèle de langue personnalisé. Votre manière de dupliquer les mots dans les corpus dépend de comment les utilisateurs sont censés les prononcer dans l'audio qui doit faire l'objet d'une reconnaissance. Plus vous ajoutez de phrases qui représentent le contexte d'utilisation des mots du domaine par les locuteurs, plus précise sera la reconnaissance du service.

Le service n'applique pas un simple algorithme de correspondance de mots. Sa transcription dépend du contexte d'utilisation des mots. Lorsque le service analyse un corpus, il inclut des informations sur les n-grammes (bigrammes, trigrammes, etc.) des phrases du corpus dans le modèle personnalisé. Ces informations aident le service à transcrire de l'audio avec une plus grande précision et explique pourquoi l'entraînement d'un modèle personnalisé sur les corpus présente plus d'intérêt que l'entraînement réalisé uniquement sur des mots personnalisés.

Par exemple, aux Etats-Unis, les comptables adhèrent à un ensemble de normes et de procédures dénommé Generally Accepted Accounting Principles (GAAP). Lorsque vous créez un modèle personnalisé dans le domaine financier, fournissez les phrases utilisant le terme GAAP en contexte. Ces phrases aident le service à faire la distinction entre des phrases générales de type "the gap between them is small" et des phrases propres au domaine, telles que "GAAP provides guidelines for measuring and disclosing financial information".

En général, il est préférable pour les corpus d'utiliser des mots dans des contextes différents, ce qui peut améliorer le mode d'apprentissage des mots par le service. Cependant, si les utilisateurs prononcent les mots uniquement dans un ou deux contextes, l'apparition des mots dans d'autres contextes ne contribue pas à améliorer la qualité du modèle personnalisé. Les locuteurs n'utilisent jamais les mots dans ces contextes. Si les locuteurs sont susceptibles d'utiliser la même expression fréquemment, la répétition de cette expression dans les corpus peut améliorer la qualité du modèle. (Dans certains cas, le simple ajout de quelques mots personnalisés directement dans le modèle personnalisé peut faire la différence.)

### Préparation d'un fichier texte de corpus
{: #prepareCorpus}

Suivez ces instructions pour préparer un fichier texte de corpus :

-   Fournissez un fichier de texte brut codé en UTF-8 s'il contient des caractères non ASCII. Le service adopte le codage UTF-8 s'il rencontre des caractères de ce type.

    Vérifiez que vous connaissez le codage de caractères de vos fichiers texte de corpus. Le service conserve le codage qu'il trouve dans les fichiers texte. Vous devez utiliser ce codage lorsque vous utilisez des mots dans le modèle de langue personnalisé. Pour plus d'informations, voir [Codage de caractères](#charEncoding).
    {: important}
-   Utilisez les majuscules à bon escient pour les mots du corpus. La ressource de mots est sensible à la casse. Utilisez une combinaison de majuscules et de minuscules ou la capitalisation uniquement si c'est prévu.
-   Incluez chaque phrase du corpus sur une même ligne et terminez chaque ligne par un retour chariot. Mettre plusieurs phrases sur la même ligne peut nuire à la précision.
-   Ajoutez des noms personnels sous forme d'unités distinctes sur des lignes séparées. N'ajoutez pas les mots d'un nom sur des lignes distinctes ou en tant que mot personnalisé individuel et ne mettez pas plusieurs noms sur la même ligne du corpus. L'exemple suivant présente le bon moyen d'améliorer la précision de la reconnaissance pour trois noms :

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Ajoutez des informations contextuelles, le cas échéant, par exemple, `Doctor Sebastian Leifson` ou `President Malcolm Ingersol`. Comme pour tous les mots, la duplication des noms à plusieurs reprises et, si possible, dans différents contextes, peut améliorer la précision de la reconnaissance.
-   Faites attention aux erreurs typographiques. Le service considère ces erreurs comme des mots nouveaux. Si vous ne les corrigez pas avant d'entraîner le modèle, le service les ajoutera dans le vocabulaire du modèle. N'oubliez pas l'adage GIGO (*Garbage in, garbage out!*) que l'on peut traduire par "à données inexactes, résultats erronés".

Plus il y a de phrases, plus on obtient un résultat précis. Mais le service limite le modèle à un total de 10 millions de mots maximum et de 90 mille mots OOV issus de toutes les sources confondues.

### Que se passe-t-il lorsque j'ajoute un fichier de corpus ?
{: #parseCorpus}

Lorsque vous ajoutez un fichier de corpus, le service analyse le contenu du fichier. Il en extrait les éventuels nouveaux mots (OOV) qu'il détecte et ajoute chaque mot OOV à la ressource de mots du modèle personnalisé. Pour distiller le plus de signification à partir de ce contenu, le service tokénise et analyse les données qu'il lit dans un fichier de corpus. Les sections suivantes expliquent comment le service analyse un fichier de corpus pour chaque langue prise en charge.

#### Analyse de l'anglais, du français, de l'allemand, de l'espagnol et du portugais brésilien
{: #corpusLanguages}

Les descriptions suivantes s'appliquent à l'anglais américain et britannique, au français, à l'allemand, à l'espagnol et au portugais brésilien.

-   Conversion des nombres en mots équivalents, par exemple :
    -   *En anglais,* `500` devient `five hundred` et `0.15` devient `zero point fifteen`.
    -   *En français, * `500` devient `cinq cents` et `0,15` devient <code>z&eacute;ro virgule quinze</code>.
    -   *En allemand,* `500` devient <code>f&uuml;nfhundert</code> et `0,15` devient <code>null punkt f&uuml;nfzehn</code>.
    -   *En espagnol,* `500` devient `quinientos` et `0,15` devient `cero coma quince`.
    -   *En portugais brésilien, * `500` devient `quinhentos` et `0,15` devient `zero ponto quinze`.
-   Conversion des sèmes incluant certains symboles en représentations de chaîne significatives, par exemple :
    -   Conversion de `$` (symbole du dollar) et d'un nombre :
        -   *En anglais,* `$100` devient `one hundred dollars`.
        -   *En français, * `$100` devient `cent dollars`.
        -   *En allemand,* `$100` et `100$` deviennent `einhundert dollar`.
        -   *En espagnol,* `$100` et `100$` deviennent <code>cien d&oacute;lares</code> (ou `cien pesos` si le dialecte est `es-LA`).
        -   *En portugais brésilien,* `$100` et `100$` deviennent <code>cem d&oacute;lares</code>.
    -   Conversion de <code>&euro;</code> (symbole de l'euro) et d'un nombre :
        -   *En anglais,* <code>&euro;100</code> devient `one hundred euros`.
        -   *En français,* <code>&euro;100</code> devient `cent euros`.
        -   *En allemand,* <code>&euro;100</code> et <code>100&euro;</code> deviennent `einhundert euro`.
        -   *En espagnol,* <code>&euro;100</code> et <code>100&euro;</code> deviennent `cien euros`.
        -   *En portugais brésilien,* <code>&euro;100</code> et <code>100&euro;</code> deviennent `cem euros`.
    -   Conversion de `%` (signe pourcentage) précédé d'un nombre :
        -   *En anglais,* `100%` devient `one hundred percent`.
        -   *En français, * `100%` devient `cent pour cent`.
        -   *En allemand,* `100%` devient `einhundert prozent`.
        -   *En espagnol,* `100%` devient `cien por ciento`.
        -   *En portugais brésilien,* `100%` devient `cem por cento`.

    Cette liste n'est pas exhaustive. Le service effectue des ajustements similaires pour d'autres caractères selon les besoins.
-   Traitement des caractères non alphanumériques, de la ponctuation et des caractères spéciaux en fonction de leur contexte. Par exemple, le service supprime un caractère `$` (symbole du dollar) ou <code>&euro;</code> (symbole de l'euro) sauf s'il est suivi d'un nombre. Le traitement est cohérent et dépend du contexte pour toutes les langues prises en charge.
-   Les expressions entre parenthèses `( )`, entre chevrons `< >`, entre crochets `[ ]` ou entre accolades `{ }` sont ignorées.

#### Analyse du japonais
{: #corpusLanguages-jaJP}

-   Conversion de tous les caractères en caractères à pleine chasse.
-   Conversion des nombres en mots équivalents, par exemple, `500` devient <code>&#20116;&#30334;</code> et `0.15` devient <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Les sèmes incluant des symboles ne sont pas convertis en chaînes équivalentes, par exemple, `100%` devient <code>&#30334;&#65285;</code>.
-   La ponctuation n'est pas automatiquement supprimée. {{site.data.keyword.IBM_notm}} recommande vivement de supprimer la ponctuation si votre application est basée sur une transcription et non pas sur une dictée.

#### Analyse du coréen
{: #corpusLanguages-koKR}

-   Conversion des nombres en mots équivalents, par exemple, <code>10</code> devient <code>&#49901;</code>.
-   Suppression de la ponctuation et des caractères spéciaux suivants : `- ( ) * : . , ' "`. Cependant, toute la ponctuation et les caractères spéciaux supprimés pour d'autres langues ne le sont pas forcément pour le coréen, par exemple :
    -   Le point (`.`) est supprimé uniquement lorsqu'il se trouve à la fin d'une ligne d'entrée.
    -   Le tilde (`~`) n'est pas supprimé.
    -   Les symboles à caractères larges (Unicode) ne sont pas supprimés ou traités autrement, par exemple, <code>&#8230;</code> (triple point ou ellipse).

    En général, {{site.data.keyword.IBM_notm}} vous recommande de supprimer la ponctuation, les caractères spéciaux et les caractères larges (Unicode) avant de traiter un fichier de corpus.
-   Les expressions entre parenthèses `( )`, entre chevrons `< >`, entre crochets `[ ]` ou entre accolades `{ }` ne sont pas supprimées ou ignorées.
-   Conversion des sèmes incluant certains symboles en représentations de chaîne significatives, par exemple :
    -   `24%` devient <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>.
    -   `$10` devient <code>&#49901;&#45804;&#47084;</code>.

    Cette liste n'est pas exhaustive. Le service effectue des ajustements similaires pour d'autres caractères selon les besoins.
-   Pour les expressions constituées de caractères latins (anglais) ou d'une combinaison de caractères hangul et latins, le service crée des mots OOV pour ces expressions, telles qu'elles apparaissent dans le fichier de corpus. Et il crée des prononciations possibles pour les mots reposant sur des transcriptions Hangul.
   - Il attribue au mot OOV `London` la prononciation possible (sounds-like) <code>&#47088;&#45912;</code>.
   - Il attribue au mot OOV <code>IBM&#54856;&#54168;&#51060;&#51648;</code> la prononciation possible (sounds-like) <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>.

## Utilisation des mots personnalisés
{: #workingWords}

Vous pouvez utiliser les méthodes `POST /v1/customizations/{customization_id}/words` et `PUT /v1/customizations/{customization_id}/words/{word_name}` pour ajouter de nouveaux mots à la ressource de mots d'un modèle de langue personnalisé. Vous pouvez également utiliser ces méthodes pour modifier ou compléter un mot dans une ressource de mots.

Vous serez, par exemple, peut-être amené à utiliser ces méthodes pour corriger une erreur typographique ou une autre faute qui s'est glissée lorsque le mot a été ajouté à partir d'un corpus. Il vous faudra peut-être également ajouter des définitions de prononciations possibles pour un mot existant. Si vous modifiez un mot existant, les nouvelles données que vous fournissez remplacent la définition existante du mot dans la ressource de mots. Les règles utilisées pour ajouter un mot s'appliquent également à la modification d'un mot existant.

### Codage de caractères
{: #charEncoding}

En général, vous serez amené à ajouter la plupart des mots personnalisés à partir de corpus. Vérifiez que vous connaissez le codage de caractères utilisé dans les fichiers texte de vos corpus. Le service conserve le codage qu'il trouve dans les fichiers texte.

Vous devez utiliser ce codage lorsque vous utilisez des mots individuels dans le modèle de langue personnalisé. Lorsque vous spécifiez un mot avec la méthode `GET`, `PUT` ou `DELETE /v1/customizations/{customization_id}/words/{word_name}`, vous devez coder le paramètre `word_name` que vous transmettez dans l'URL en codage URL si le mot comprend des caractères non ASCII.

Par exemple, le tableau suivant présente à quoi ressemble la même lettre dans deux codages différents, ASCII et UTF-8. Vous pouvez transmettre le caractère ASCII dans une URL sous la forme `z`. Vous devez transmettre le caractère UTF-8 sous la forme `%EF%BD%9A`.

<table style="width:75%">
  <caption>Tableau 1. Exemples de codage de caractères</caption>
  <tr>
    <th style="width:15%; text-align:center">Lettre</th>
    <th style="width:40%; text-align:center">Codage</th>
    <th style="width:45%; text-align:center">Valeur</th>
  </tr>
  <tr>
    <td style="text-align:center">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a` (`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:center">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 en hexadécimal
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### Utilisation de la zone sounds_like
{: #soundsLike}

La zone `sounds_like` indique comment est prononcé un mot par les locuteurs. Par défaut, le service remplit automatiquement cette zone avec l'orthographe du mot. Vous pouvez fournir jusqu'à cinq prononciations différentes pour un mot difficile à prononcer ou qui peut se prononcer de différentes manières. Envisagez d'utiliser cette zone pour

-   *fournir différentes prononciations pour les acronymes.* Par exemple, l'acronyme `NCAA` peut se prononcer comme il s'épelle ou souvent ainsi : *N. C. double A.* L'exemple suivant ajoute ces deux prononciations possibles (sounds-like) pour le mot `NCAA` :

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *traiter les mots étrangers.* Par exemple, le mot français <code>gar&ccedil;on</code> contient un caractère introuvable en anglais. Vous pouvez spécifier un paramètre sounds-like `gaarson`, en remplaçant le caractère <code>&ccedil;</code> par un `s`, pour indiquer au service comment les locuteurs anglais doivent prononcer le mot.

La reconnaissance vocale utilise des algorithmes statistiques pour l'analyse audio, par conséquent l'ajout d'un mot ne garantit pas que le service le transcode avec une parfaite exactitude. Lorsque vous ajoutez un mot, tenez compte de ses différentes prononciations possibles. Utilisez la zone `sounds_like` pour fournir différentes prononciations indiquant comment un mot peut être énoncé. Les sections suivantes fournissent les instructions spécifiques à chaque langue pour la spécification des prononciations possibles.

#### Instructions pour l'anglais (américain et britannique)
{: #wordLanguages-enUS-enGB}

*Instructions pour l'anglais américain et l'anglais britannique :*

-   Utilisez des caractères alphabétiques anglais : `a-z` et `A-Z`.
-   Utilisez des mots réels ou des mots créés pour pouvoir être prononcés en anglais en cas de mots difficiles à prononcer, par exemple, `shuchesnie` pour le mot `Sczcesny`.
-   Remplacez par des lettres anglaises équivalentes des lettres qui n'existent pas en anglais, par exemple, `s` pour <code>&ccedil;</code> ou `ny` pour <code>&ntilde;</code>.
-   Remplacez les lettres accentuées par des lettres non accentuées, par exemple, `a` pour <code>&agrave;</code> ou `e` pour <code>&egrave;</code>.
-   Vous pouvez inclure plusieurs mots séparés par des espaces, mais le service impose une limite maximale de 40 caractères au total, espaces non compris.

*Instructions pour l'anglais américain uniquement :*

-   Pour prononcer une seule lettre, utilisez cette lettre suivie d'un point. Si le point est suivi d'un autre caractère, assurez-vous de mettre un espace entre le point et le caractère qui suit. Par exemple, utilisez `N. C. A. A.`, *et non* `N.C.A.A.`
-   Utilisez l'orthographe des nombres, par exemple, `seventy-five` pour `75`.

*Instructions pour l'anglais britannique uniquement :*

-   Vous **ne pouvez pas** utiliser des points ou des tirets dans les prononciations possibles pour l'anglais britannique.
-   Pour prononcer une seule lettre, utilisez cette lettre suivie d'un espace. Par exemple, utilisez `N C A A`, *et non pas* `N. C. A. A.`, `N.C.A.A.` ou `NCAA`.
-   Utilisez l'orthographe des nombres sans tirets, par exemple `seventy five` pour `75`.

#### Instructions pour le français, l'allemand, l'espagnol et le portugais brésilien
{: #wordLanguages-esES-frFR}

-   Vous **ne pouvez pas** utiliser de tirets dans les prononciations possibles.
-   Utilisez des caractères alphabétiques valides dans la langue concernée : `a-z` et `A-Z`, y compris les caractères accentués valides.
-   Pour prononcer une seule lettre, utilisez cette lettre suivie d'un point. Si le point est suivi d'un autre caractère, assurez-vous de mettre un espace entre le point et le caractère qui suit. Par exemple, utilisez `N. C. A. A.`, *et non* `N.C.A.A.`
-   Utilisez des mots réels ou des mots créés pour pouvoir être prononcés dans la langue concernée en cas de mots difficiles à prononcer.
-   Utilisez l'orthographe des nombres sans tiret, par exemple, pour `75`, utilisez
    -   *en français :* `soixante quinze`
    -   *en allemand :* <code>f&uuml;nfundsiebzig</code>
    -   *en espagnol :* `setenta y cinco`
    -   *en portugais brésilien :* `setenta e cinco`
-   Vous pouvez inclure plusieurs mots séparés par des espaces, mais le service impose une limite maximale de 40 caractères au total, espaces non compris.

#### Instructions pour le japonais
{: #wordLanguages-jaJP}

-   Utilisez uniquement des caractères Katakana à pleine chasse en utilisant le symbole de prolongation <code>&#8213;</code> (*chou-on* ou &#38263;&#38899;, en japonais). N'utilisez pas des caractères à demi-chasse.
-   Utilisez des sons contractés (*yoh-on* ou &#25303;&#38899;, en japonais) uniquement dans les contextes syllabiques suivants :

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Utilisez uniquement les syllabes suivantes après un son assimilé (*soku-on* ou &#20419;&#38899;, en japonais) :

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   N'utilisez pas <code>&#12531;</code> comme premier caractère d'un mot. Par exemple, utilisez <code>&#12454;&#12540;&#12531;&#12488;</code> au lieu de <code>&#12531;&#12540;&#12488;</code> (non valide).
-   De nombreux mots composés sont constitués d'une combinaison *préfixe+nom* ou *nom+suffixe*. Le vocabulaire de base du service couvre la plupart des mots composés qui reviennent fréquemment (par exemple, <code>&#x9577;&#x96FB;&#x8A71;</code> et <code>&#x53E4;&#x65B0;&#x805E;</code>) mais pas les mots composés qui apparaissent rarement. Si votre corpus contient souvent des mots composés, ajoutez-les comme un seul mot dans la première étape de votre personnalisation. Par exemple, <code>&#x53E4;&#x925B;&#x7B46;</code> n'est pas courant dans les textes en japonais général ; si vous l'utilisez souvent, ajoutez-le dans votre modèle personnalisé pour améliorer la précision de la transcription.
-   N'utilisez pas de son assimilé en fin de mot.

#### Instructions pour le coréen
{: #wordLanguages-koKR}

-   Utilisez des symboles, des syllabes et des caractères Hangul coréens.
-   Vous pouvez également utiliser des caractères alphabétiques latins (anglais) : `a-z` et `A-Z`.
-   N'utilisez aucun caractère ou symbole qui n'appartient pas aux jeux de caractères précédents.

### Utilisation de la zone display_as
{: #displayAs}

La zone `display_as` spécifie comment s'affiche un mot dans une retranscription. Elle est conçue pour les cas où vous souhaitez que le service affiche une chaîne qui ne correspond pas à l'orthographe du mot. Par exemple, vous pouvez indiquer que le mot `hhonors` doit être affiché sous la forme `HHonors` même si ça ressemble à `hilton honors` ou `h honors`.

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

Autre exemple, vous pouvez indiquer que le mot `IBM` doit être affiché sous la forme <code>IBM&trade;</code>.

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### Interaction avec le formatage intelligent et l'occultation numérique
{: #displaySmart}

Si vous utilisez les paramètres `smart_formatting` ou `redaction` avec une demande de reconnaissance, n'oubliez pas que le service applique le formatage intelligent et l'occultation à un mot avant de prendre en compte la zone `display_as` correspondant au mot. Vous devrez peut-être expérimenter les résultats pour vous assurer que les fonctions n'interfèrent pas avec le mode d'affichage de vos mots personnalisés. Vous devrez peut-être également ajouter des mots personnalisés pour permettre l'adaptation à ces contraintes.

Par exemple, supposons que vous ajoutez le mot personnalisé `one` avec une zone `display_as` définie avec `one`. Le formatage intelligent remplace le mot `one` par le nombre `1`, et la valeur de la zone display-as n'est pas appliquée. Pour contourner le problème, vous pouvez ajouter un mot personnalisé pour le nombre `1` et appliquer à ce mot la même valeur dans la zone `display_as`.

Pour plus d'informations sur l'utilisation de ces fonctions, voir [Formatage intelligent](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting) et [Occultation numérique](/docs/services/speech-to-text?topic=speech-to-text-output#redaction).

### Que se passe-t-il lorsque j'ajoute ou modifie un mot personnalisé ?
{: #parseWord}

Le mode de réponse du service à une demande d'ajout ou de modification d'un mot personnalisé dépend des valeurs que vous fournissez et de la présence de ce mot dans le vocabulaire de base du service.

<table>
  <caption>Tableau 1. Ajout de mots personnalisés avec différentes zones</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom">La zone <code>sounds_like</code> est...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom">La zone <code>display_as</code> est...</th>
    <th style="text-align:left; vertical-align:bottom">Réponse</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Non spécifiée
    </td>
    <td style="text-align:center; vertical-align:top">
      Non spécifiée
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si le mot ne figure pas dans le vocabulaire de
          base du service,</em> le service définit la zone <code>sounds_like</code>
          avec la prononciation qu'il attribue au mot. Il définit la zone
          <code>display_as</code> avec la valeur de la zone
          <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si le mot figure dans le vocabulaire de base du service,</em>
          le service laisse les zones <code>sounds_like</code> et
          <code>display_as</code> vides. Ces zones sont vides
          uniquement si le mot se trouve dans le vocabulaire de base du service. La
          présence de ce mot dans la ressource de mots du modèle ne pose pas de
          problème mais est inutile.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Spécifiée
    </td>
    <td style="text-align:center; vertical-align:top">
      Non spécifiée
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la zone <code>sounds_like</code> est valide,</em> le
          service définit la zone <code>display_as</code> avec la
          valeur de la zone <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la zone <code>sounds_like</code> n'est pas valide :</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              La méthode <code>POST
                /v1/customizations/{customization_id}/words</code>
              ajoute une zone <code>error</code> pour ce mot dans la
              dans la ressource de mots du modèle.
            </li>
            <li style="margin:10px 0px; line-height:120%">
              La méthode <code>PUT
                /v1/customizations/{customization_id}/words/{word_name}</code>
              échoue avec le code de réponse 400 et un message d'erreur. Le
              service n'ajoute pas le mot dans la ressource de mots.
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Non spécifiée
    </td>
    <td style="text-align:center; vertical-align:top">
      Spécifiée
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si le mot ne figure pas dans le vocabulaire de
          base du service,</em> le service définit la zone <code>sounds_like</code>
          avec la prononciation qu'il attribue au mot et laisse la zone
          <code>display_as</code> telle qu'elle est spécifiée.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si le mot figure dans le vocabulaire de base du service,</em>
          le service laisse la zone <code>sounds_like</code> vide et la
          zone <code>display_as</code> telle qu'elle est spécifiée.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Spécifiée
    </td>
    <td style="text-align:center; vertical-align:top">
      Spécifiée
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la zone <code>sounds_like</code> est valide,</em> le
          service définit les zones <code>sounds_like</code> et <code>display_as</code>
          avec les valeurs spécifiées.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la zone <code>sounds_like</code> n'est pas valide,</em>
          le service répond de la même manière que dans le cas où la
          zone <code>sounds_like</code> est spécifiée mais que la zone
          <code>display_as</code> ne l'est pas.
        </li>
      </ul>
    </td>
  </tr>
</table>

## Validation d'une ressource de mots
{: #validateModel}

Particulièrement quand vous ajoutez un corpus à un modèle de langue personnalisé ou quand vous ajoutez plusieurs mots personnalisés en même temps, examinez les mots OOV dans la ressource de mots du modèle.

-   *Recherchez les éventuelles erreurs typographiques ou autres.* En particulier lorsque vous ajoutez des corpus, qui peuvent être volumineux, il est facile de faire des fautes. Les erreurs typographiques dans un fichier de corpus (ou de grammaire) sont la conséquence involontaire de l'ajout de mots nouveaux dans la ressource de mots d'un modèle, tout comme les balises HTML incorrectes laissées dans un fichier de corpus.
-   *Vérifiez les prononciations possibles.* Le service génère automatiquement ces prononciations pour les mots OOV. Dans la plupart des cas, ces prononciations sont suffisantes. Mais pour les mots avec des orthographes particulières ou difficiles à prononcer, ainsi que pour les acronymes et les termes techniques, il est recommandé de réviser les prononciations pour s'assurer qu'elles sont correctes.

Pour valider, et, le cas échéant, corriger un mot pour un modèle personnalisé, quelle que soit la façon dont il a été ajouté dans la ressource de mots, utilisez les méthodes suivantes :

-   Répertoriez tous les mots d'un modèle personnalisé en utilisant la méthode `GET /v1/customizations/{customization_id}/words` ou effectuez une requête sur un mot individuel avec la méthode `GET /v1/customizations/{customization_id}/words/{word_name}`. Pour plus d'informations, voir [Affichage de la liste des mots d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   Modifiez les mots d'un modèle personnalisé pour corriger les erreurs ou pour ajouter des valeurs sounds-like ou display-as en utilisant la méthode `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}`. Pour plus d'informations, voir [Utilisation des mots personnalisés](#workingWords).
-   Supprimez les mots superflus introduits par erreur (par exemple via des erreurs typographiques ou d'autres erreurs dans un corpus) en utilisant la méthode `DELETE /v1/customizations/{customization_id}/words/{word_name}`. Pour plus d'informations, voir [Suppression d'un mot d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-manageWords#deleteWord).
    -   Si le mot a été extrait d'un corpus, vous pouvez plutôt mettre à jour le fichier texte du corpus pour corriger l'erreur, puis recharger le fichier en utilisant le paramètre `allow_overwrite` de la méthode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`. Pour plus d'informations, voir [Ajout d'un corpus au modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus).
    -   Si le mot a été extrait d'une grammaire, vous pouvez mettre à jour le fichier de grammaire pour corriger l'erreur, puis recharger le fichier en utilisant le paramètre `allow_overwrite` de la méthode `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`. Pour plus d'informations, voir [Ajout d'une grammaire au modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar).
