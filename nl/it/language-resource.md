---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Attività con corpora e parole personalizzate
{: #corporaWords}

Puoi popolare un modello di lingua personalizzato con le parole aggiungendo corpora o grammatiche al modello oppure aggiungendo parole personalizzate direttamente:
{: shortdesc}

-   **Corpora:** il metodo consigliato per popolare un modello di lingua personalizzato con delle parole consiste nell'aggiungere uno o più corpora al modello. Quando aggiungi un corpus, il servizio analizza i file e aggiunge automaticamente le eventuali parole nuove che trova al modello personalizzato. L'aggiunta di un corpus a un modello personalizzato consente al servizio di estrarre parole specifiche per il dominio in contesto e questo aiuta a garantire dei migliori risultati della trascrizione. Per ulteriori informazioni, vedi [Utilizzo dei corpora](#workingCorpora).
-   **Grammatiche:** puoi aggiungere grammatiche a un modello personalizzato per limitare il riconoscimento vocale alle parole o alle frasi riconosciute da una grammatica. Quando aggiungi una grammatica a un modello, il servizio aggiunge automaticamente le nuove parole che trova al modello, proprio come fa con i corpora. Per ulteriori informazioni, vedi [Utilizzo di grammatiche con i modelli di lingua personalizzati](/docs/services/speech-to-text/grammar.html).
-   **Singole parole:** puoi anche aggiungere singole parole personalizzate a un modello direttamente. Il servizio aggiunge le parole al modello proprio come fa con le parole che rileva dai corpora o dalle grammatiche. Quando aggiungi una parola direttamente, puoi specificare più pronunce e indicare in che modo deve essere visualizzata la parola. Puoi anche aggiornare le parole esistenti per modificare o aumentare le definizioni che erano state estratte da corpora o grammatiche. Per ulteriori informazioni, vedi [Utilizzo delle parole personalizzate](#workingWords).

Indipendentemente da come le aggiungi, il servizio archivia tutte le parole da te aggiunte a un modello di lingua personalizzato nella risorsa di parole del modello.

## La risorsa di parole
{: #wordsResource}

La *risorsa di parole* include tutte le parole che aggiungi dai corpora, dalle grammatiche o direttamente. Il suo scopo è quello di definire la pronuncia e l'ortografia di parole che non sono già presenti nel vocabolario di base del servizio. Le definizioni indicano al servizio come trascrivere queste *parole OOV (out-of-vocabulary)*.

La risorsa di parole contiene le seguenti informazioni su ciascuna parola OOV. Il servizio crea le definizioni per le parole estratte da corpora e grammatiche. Specifichi le caratteristiche per le parole che aggiungi o modifichi direttamente.

-   `word`: la pronuncia della parola come trovata in un corpus o una grammatica oppure come aggiunta da te.
-   `sounds_like`: la pronuncia della parola. Per le parole estratte da corpora e grammatiche, il valore rappresenta in che modo il servizio ritiene che la parola sia pronunciata in base alle sue regole linguistiche. In molti casi, la pronuncia riflette l'ortografia del campo `word`.

    Puoi utilizzare il campo `sounds_like` per modificare la pronuncia della parola. Puoi anche utilizzare il campo per specificare più pronunce per una parola. Per ulteriori informazioni, vedi [Utilizzo del campo sounds_like](#soundsLike).
-   `display_as`: l'ortografia della parola che il servizio utilizza nelle trascrizioni. Il campo indica il modo in cui deve essere visualizzata la parola. Nella maggior parte dei casi, l'ortografia corrisponde al valore del campo `word`.

    Puoi utilizzare il campo `display_as` per specificare un'ortografia differente per la parola. Per ulteriori informazioni, vedi [Utilizzo del campo display_as](#displayAs).
-   `source`: il modo in cui la parola è stata aggiunta alla risorsa di parole. Se il servizio ha estratto la parola da un corpus o da una grammatica, il campo elenca il nome di tale risorsa. Poiché il servizio può rilevare la stessa parola in più risorse, il campo può elencare più nomi di corpus o grammatica. Il campo include la stringa `user` se aggiungi o modifichi la parola direttamente.

Quando aggiorni la risorsa di parole di un modello in qualsiasi modo, devi addestrare il modello perché le modifiche diventino effettive durante la trascrizione. Per ulteriori informazioni, vedi [Addestra il modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html#trainModel-language).

## Di quanti dati ho bisogno?
{: #wordsResourceAmount}

Molti fattori contribuiscono alla quantità di dati di cui hai bisogno per un modello di lingua personalizzato efficace. Non è possibile fornire il numero esatto di parole che devi aggiungere per qualsiasi applicazione o modello personalizzati.

A seconda del caso d'uso, anche l'aggiunta di qualche parola direttamente a un modello personalizzato può migliorarne la qualità. L'aggiunta però di parole OOV da un corpus che mostra le parole nel contesto in cui vengono utilizzate nell'audio può migliorare notevolmente l'accuratezza della trascrizione. Per ulteriori informazioni, vedi [Utilizzo dei corpora](#workingCorpora).

Il servizio limita il numero di parole che puoi aggiungere a un modello di lingua personalizzato:

-   Puoi aggiungere un massimo di 90.000 parole OOV alla risorsa di parole di un modello personalizzato. Ciò include le parole OOV da tutte le origini (corpora, grammatiche e singole parole personalizzate che puoi aggiungere direttamente).
-   Puoi aggiungere un massimo di 10 milioni di parole a un modello personalizzato da tutte le origini. Questa cifra include tutte le parole, sia quelle OOV che quelle che fanno già parte del vocabolario di base del servizio, che sono incluse in corpora o grammatiche.Per i corpora, il servizio utilizza queste parole aggiuntive per imparare il contesto in cui possono presentarsi le parole OOV, motivo per cui i corpora sono un metodo più efficace per migliorare l'accuratezza del riconoscimento.

Un'ampia risorsa di parole può aumentare la latenza del riconoscimento vocale ma l'effetto esatto è difficile da quantificare o prevedere. Come con la quantità di dati necessaria per produrre un modello personalizzato efficace, l'impatto sulle prestazioni di un'ampia risorsa di parole dipende da molti fattori. Verifica il tuo modello personalizzato con diverse quantità di dati per determinare le prestazioni dei tuoi modelli e dei tuoi dati.

## Utilizzo dei corpora
{: #workingCorpora}

Utilizzi il metodo `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` per aggiungere un corpus a un modello personalizzato. Un corpus è un file di testo semplice che contiene frasi di esempio dal tuo dominio. Il seguente esempio mostra un corpus abbreviato per il dominio dell'assistenza sanitaria. Un file di corpus è di norma molto più lungo.

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

Il riconoscimento vocale si basa sugli algoritmi statistici per analizzare l'audio. Le parole da un modello personalizzato sono in competizione con le parole dal vocabolario di base del servizio e con altre parole del modello. (Anche fattori quali il rumore audio e gli accenti dei parlanti influenzano la qualità della trascrizione).

L'accuratezza della trascrizione può dipendere in larga misura dal modo in cui le parole sono definite in un modello e dal modo in cui i parlanti le pronunciano. Per migliorare l'accuratezza del servizio, utilizza i corpora per fornire il maggior numero di esempi possibile di come sono utilizzate le parole OOV nel dominio. Ripetere le parole OOV nei corpora può migliorare la qualità del modello di lingua personalizzato. Il modo in cui duplichi le parole nei corpora dipende da come prevedi che le pronuncino gli utenti nell'audio che deve essere riconosciuto. Maggiore è il numero di frasi da te aggiunte che rappresentano il contesto in cui i parlanti utilizzano le parole dal dominio e maggiore sarà l'accuratezza del riconoscimento del servizio.

Il servizio non applica un semplice algoritmo di messa in corrispondenza delle parole. La sua trascrizione dipende dal contesto in cui vengono utilizzate le parole. Quando analizza un corpus, il servizio include informazioni su n-grammi (bigrammi, trigrammi e così via) dalle frasi del corpus nel modello personalizzato. Queste informazioni aiutano il servizio a trascrivere l'audio con maggiore accuratezza e spiega perché addestrare un modello personalizzato sui corpora è più importante che addestrarlo solo sulle parole personalizzate.

Ad esempio, i contabili rispettano un set comune di standard e procedure noti come GAAP (Generally Accepted Accounting Principles). Quando crei un modello personalizzato per un dominio finanziario, fornisci le frasi che utilizzano il termine GAAP in contesto. Le frasi aiutano il servizio a distinguere tra frasi generiche quali "the gap between them is small" e le frasi incentrate sul dominio come ad esempio "GAAP provides guidelines for measuring and disclosing financial information."

In generale, è meglio che i corpora utilizzino le parole in contesti differenti poiché questo può migliorare il modo in cui il servizio impara le parole. Tuttavia, se gli utenti pronunciano le parole solo in un paio di contesti, mostrarle in altri contesti non migliora la qualità del modello personalizzato. I parlanti non utilizzano mai le parole in tali contesti. Se i parlanti probabilmente utilizzeranno spesso la stessa frase, ripetere tale frase nei corpora può migliorare la qualità del modello. (In alcuni casi, anche aggiungere qualche parola personalizzata direttamente a un modello personalizzato può fare una differenza positiva).

### Preparazione di un file di testo di corpus
{: #prepareCorpus}

Attieniti alle seguenti linee guida per preparare un file di testo di corpus:

-   Fornisci un file di testo semplice codificato in UTF-8 se contiene caratteri non ASCII. Il servizio presume la codifica UTF-8 se riscontra tali caratteri.

    Assicurati di conoscere la codifica dei caratteri dei tuoi file di testo di corpus. Il servizio preserva la codifica che trova nei file di testo. Devi utilizzare questa codifica quando utilizzi le parole nel modello di lingua personalizzato. Per ulteriori informazioni, vedi [Codifica dei caratteri](#charEncoding).
    {: important}
-   Utilizza le maiuscole in modo congruente per le parole nel corpus. La risorsa di parole è sensibile a maiuscole/minuscole. Combina lettere maiuscole e minuscole e utilizza le maiuscole solo quando previsto.
-   Includi ogni frase del corpus su una sua riga e termina ogni riga con un ritorno a capo. L'inclusione di più frasi sulla stessa riga può ridurre l'accuratezza.
-   Aggiungi i nomi personali come unità discrete su righe separate. Non aggiungere le parole di un nome su righe separate o come singole parole personalizzate e non includere più nomi sulla stessa riga del corpus. Il seguente esempio mostra il modo corretto di migliorare l'accuratezza del riconoscimento per tre nomi:

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Includi ulteriori informazioni contestuali laddove disponibili, ad esempio `Doctor Sebastian Leifson` o `President Malcolm Ingersol`. Come con tutte le parole, la duplicazione dei nomi più volte e, se possibile, in contesti differenti, può migliorare l'accuratezza del riconoscimento.
-   Attenzione agli errori di battitura. Il servizio presume che gli errori di battitura siano parole nuove. Se non li correggi prima di addestrare il modello, il servizio li aggiunge al vocabolario del modello. Ricordati l'adagio *Spazzatura dentro, spazzatura fuori,*

Un maggior numero di frasi produce una migliore accuratezza. Il servizio però limita un modello a un massimo di 10 milioni di parole in totale e a 90.000 parole OOV per tutte le origini combinate.

### Cosa succede quando aggiungo un file di corpus?
{: #parseCorpus}

Quando aggiungi un file di corpus, il servizio ne analizza il contenuto. Estrae tutte le parole (OOV) nuove che trova e aggiunge ciascuna di esse alla risorsa di parole del modello personalizzato. Per ricavare il massimo significato dal contenuto, il servizio tokenizza e analizza i dati che legge da un file di corpus. Le seguenti sezioni descrivono in che modo il servizio analizza un file di corpus per ciascuna lingua supportata.

#### Analisi di inglese, francese, tedesco, spagnolo e portoghese (Brasile)
{: #corpusLanguages}

Le seguenti descrizioni si applicano a inglese (Stati Uniti) e a inglese (Regno Unito), francese, tedesco, spagnolo e portoghese (Brasile).

-   Converte i numeri nelle loro parole equivalenti, ad esempio:
    -   *Per l'inglese, * `500` diventa `five hundred` e `0.15` diventa `zero point fifteen`.
    -   *Per il francese, * `500` diventa `cinq cents` e `0,15` diventa <code>z&eacute;ro quinze</code>.
    -   *Per il tedesco, * `500` diventa <code>f&uuml;nfhundert</code> e `0,15` diventa <code>null punkt f&uuml;nfzehn</code>.
    -   *Per lo spagnolo, * `500` diventa `quinientos` e `0,15` diventa `cero coma quince`.
    -   *Per il portoghese (Brasile), * `500` diventa `quinhentos` e `0,15` diventa `zero ponto quinze`.
-   Converte i token che includono specifici simboli in rappresentazioni stringa significative, ad esempio:
    -   Converte un `$` (simbolo del dollaro) e un numero:
        -   *Per l'inglese, * `$100` diventa `one hundred dollars`.
        -   *Per il francese, * `$100` diventa `cent dollar`.
        -   *Per il tedesco, * `$100` e `100$` diventano `einhundert dollar`.
        -   *Per lo spagnolo, * `$100` e `100$` diventano <code>cien d&oacute;lares</code> (o `cien pesos` se il dialetto è `es-LA`).
        -   *Per il portoghese (Brasile), * `$100` e `100$` diventano <code>cem d&oacute;lares</code>.
    -   Converte un <code>&euro;</code> (simbolo dell'euro) e un numero:
        -   *Per l'inglese, * <code>&euro;100</code> diventa `one hundred euros`.
        -   *Per il francese, * <code>&euro;100</code> diventa `cent euros`.
        -   *Per il tedesco, * <code>&euro;100</code> e <code>100&euro;</code> diventano `einhundert euro`.
        -   *Per lo spagnolo, * <code>&euro;100</code> e <code>100&euro;</code> diventano `cien euros`.
        -   *Per il portoghese (Brasile), * <code>&euro;100</code> e <code>100&euro;</code> diventano `cem euros`.
    -   Converte un `%` (segno percentuale) preceduto da un numero:
        -   *Per l'inglese, * `100%` diventa `one hundred percent`.
        -   *Per il francese, * `100%` diventa `cent pourcent`.
        -   *Per il tedesco, * `100%` diventa `einhundert prozent`.
        -   *Per lo spagnolo, * `100%` diventa `cien por ciento`.
        -   *Per il portoghese (Brasile), * `100%` diventa `cem por cento`.

    Questo elenco non è esaustivo. Il servizio apporta delle regolazioni simili per altri caratteri come necessario.
-   Elabora i caratteri non alfanumerici, di punteggiatura e speciali, a seconda del loro contesto. Ad esempio il servizio rimuove un `$` (simbolo del dollaro) o <code>&euro;</code> (simbolo dell'euro) a meno che non sia seguito da un numero. L'elaborazione dipende dal contesto ed è congruente tra le lingue supportate.
-   Ignora le frasi racchiuse tra parentesi (`( )`), parentesi uncinate (`< >`), parentesi quadre (`[ ]`) o parentesi graffe (`{ }`).

#### Analisi del giapponese
{: #corpusLanguages-jaJP}

-   Converte tutti i caratteri in caratteri a larghezza intera.
-   Converte i numeri nelle loro parole equivalenti, ad esempio `500` diventa <code>&#20116;&#30334;</code> e `0.15` diventa <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Non converte i token che includono simboli in stringhe equivalenti, ad esempio `100%` diventa <code>&#30334;&#65285;</code>.
-   Non rimuove automaticamente la punteggiatura. {{site.data.keyword.IBM_notm}} ti consiglia vivamente di rimuovere la punteggiatura se la tua applicazione è basata sulla trascrizione invece che sulla dettatura.

#### Analisi in coreano
{: #corpusLanguages-koKR}

-   Converte i numeri nelle loro parole equivalenti, ad esempio <code>10</code> diventa <code>&#49901;</code>.
-   Rimuove la seguente punteggiatura e i seguenti caratteri speciali: `- ( ) * : . , ' "`. Tuttavia, non tutta la punteggiatura e non tutti i caratteri speciali che vengono rimossi dalle altre lingue vengono rimossi per il coreano, ad esempio:
    -   Rimuove un simbolo di punto (`.`) solo quando ricorre alla fine di una riga di input.
    -   Non rimuove un simbolo di tilde (`~`).
    -   Non rimuove o in altro modo elabora i simboli di caratteri estesi Unicode, ad esempio <code>&#8230;</code> (triplo punto o puntini di sospensione).

    In generale, {{site.data.keyword.IBM_notm}} ti consiglia di rimuovere punteggiatura, caratteri speciali e caratteri estesi Unicode prima di elaborare un file di corpus.
-   Non rimuove o ignora le frasi racchiuse tra parentesi (`( )`), parentesi uncinate (`< >`), parentesi quadre (`[ ]`) o parentesi graffe (`{ }`).
-   Converte i token che includono specifici simboli in rappresentazioni stringa significative, ad esempio:
    -   `24%` diventa <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>.
    -   `$10` diventa <code>&#49901;&#45804;&#47084;</code>.

    Questo elenco non è esaustivo. Il servizio apporta delle regolazioni simili per altri caratteri come necessario.
-   Per le frasi formate da caratteri latini (inglese) o da una combinazione di caratteri Hangul e latini, il servizio crea delle parole OOV per le frasi esattamente come si presentano nel file di corpus.Crea inoltre delle pronunce dal suono simile (sounds-like) per le parole basate su trascrizioni Hangul.
   - Dà alla parola OOV `London` un suono simile (sounds-like) di <code>&#47088;&#45912;</code>.
   - Dà alla parola OOV <code>IBM&#54856;&#54168;&#51060;&#51648;</code> un suono simile (sounds-like) di <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>.

## Utilizzo delle parole personalizzate
{: #workingWords}

Puoi utilizzare i metodi `POST /v1/customizations/{customization_id}/words` e `PUT /v1/customizations/{customization_id}/words/{word_name}` per aggiungere nuove parole alla risorsa di parole di un modello personalizzato. Puoi anche utilizzare i metodi per modificare o aumentare una parola in una risorsa di parole.

Potesti, ad esempio, dover utilizzare i metodi per correggere un errore di battitura o un altro errore che è stato fatto quando una parola è stata aggiunta da un corpus. Potresti anche dover aggiungere delle definizioni di suono simile (sounds-like) per una parola esistente. Se modifichi una parola esistente, i nuovi dati che fornisci sovrascrivono la definizione esistente della parola nella risorsa di parole. Le regole per aggiungere una parola si applicano anche alla modifica di una parola esistente.

### Codifica dei caratteri
{: #charEncoding}

In generale, è probabile che aggiungi la maggior parte delle parole personalizzate dai corpora. Assicurati di conoscere la codifica di caratteri utilizzata nei file di testo per i tuoi corpora. Il servizio preserva la codifica che trova nei file di testo. 

Devi utilizzare questa codifica quando utilizzi le singole parole nel modello di lingua personalizzato. Quando specifichi una parola con il metodo `GET`, `PUT` o `DELETE /v1/customizations/{customization_id}/words/{word_name}`, devi codificare in URL il `word_name` che passi nell'URL se la parola include caratteri non ASCII.

Ad esempio, la seguente tabella mostra come si presenta la stessa lettera in due codifiche differenti, ASCII e UTF-8. Puoi passare il carattere ASCII in un URL come `z`. Puoi passare il carattere UTF-8 come `%EF%BD%9A`. 
<table>
  <caption>Tabella 1. Esempi di codifica dei caratteri</caption>
  <tr>
    <th style="text-align:left">Lettera</th>
    <th style="text-align:center">Codifica</th>
    <th style="text-align:center">Valore</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
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
    <td style="text-align:left; width:30%">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 esadecimale
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### Utilizzo del campo sounds_like
{: #soundsLike}

Il campo `sounds_like` specifica in che modo una parola viene pronunciata dai parlanti. Per impostazione predefinita, il servizio completa automaticamente il campo con l'ortografia della parola. Puoi fornire fino a cinque pronunce alternative per una parola che è difficile da pronunciare o che può essere pronunciata in diversi modi. Considera l'utilizzo del campo per

-   *Fornire pronunce differenti per gli acronimi.* Ad esempio, l'acronimo `NCAA` può essere pronunciato come è scritto oppure colloquialmente come *N. C. doppia A.* il seguente esempio aggiunge entrambe queste pronunce dal suono simile (sounds-like) per la parola `NCAA`:

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *Gestire le parole straniere.* Ad esempio, la parola francese <code>gar&ccedil;on</code> contiene un carattere che non è presente nella lingua inglese. Puoi specificare un suono simile (sounds-like) di `gaarson`, sostituendo <code>&ccedil;</code> con `s`, per indicare al servizio in che modo i parlanti di lingua inglese pronuncerebbero la parola.

Il riconoscimento vocale utilizza gli algoritmi statistici per analizzare l'audio; l'aggiunta di una parola, quindi, non garantisce che il servizio ne esegua la transcodifica con un'accuratezza completa. Quando aggiungi una parola, considera in che modo potrebbe essere pronunciata. Utilizza il campo `sounds_like` per fornire diverse pronunce che riflettono in che modo può essere pronunciata una parola. Le seguenti sezioni forniscono linee guida specifiche per la lingua per specificare una pronuncia dal suono simile (sounds-like).

#### Linee guida per l'inglese (Stati Uniti e Regno Unito)
{: #wordLanguages-enUS-enGB}

*Linee guida sia per l'inglese (Stati Uniti) che per l'inglese (Regno Unito):*

-   Utilizza i caratteri alfabetici inglesi: `a-z` e `A-Z`.
-   Utilizza parole reali o inventate che sono pronunciabili in inglese per le parole che sono difficili da pronunciare, ad esempio `shuchesnie` per la parola `Sczcesny`.
-   Sostituisci le lettere dell'inglese equivalenti alle lettere non dell'inglese, ad esempio `s` per <code>&ccedil;</code> o `ny` per <code>&ntilde;</code>.
-   Sostituisci le lettere non accentate alle lettere accentate, ad esempio `a` per <code>&agrave;</code> o `e` per <code>&egrave;</code>.
-   Puoi includere più parole separate da spazi ma il servizio applica un massimo di 40 caratteri in totale, non includendo gli spazi.

*Linee guida solo per l'inglese (Stati Uniti)*

-   Per pronunciare una singola lettera, utilizza la lettera seguita da un punto. Se il punto è seguito da un altro carattere, assicurati di utilizzare uno spazio tra il punto e il carattere successivo. Ad esempio, utilizza `N. C. A. A.`, *non* `N.C.A.A.`
-   Utilizza l'ortografia dei numeri, ad esempio `seventy-five` per `75`.

*Linee guida solo per l'inglese (Regno Unito)*

-   **Non puoi** utilizzare i punti o i trattini nelle pronunce dal suono simile (sounds-like) per l'inglese (Regno Unito).
-   Per pronunciare una singola lettera, utilizza la lettera seguita da uno spazio. Ad esempio, utilizza `N C A A`, *non* `N. C. A. A.`, `N.C.A.A.` o `NCAA`.
-   Utilizza l'ortografia dei numeri senza trattini, ad esempio `seventy five` per `75`.

#### Linee guida per francese, tedesco, spagnolo e portoghese (Brasile)
{: #wordLanguages-esES-frFR}

-   **Non puoi** utilizzare i trattini nelle pronunce dal suono simile (sounds-like).
-   Utilizza i caratteri alfabetici che sono validi per la lingua: `a - z` e `A-Z` incluse le lettere accentate valide.
-   Per pronunciare una singola lettera, utilizza la lettera seguita da un punto. Se il punto è seguito da un altro carattere, assicurati di utilizzare uno spazio tra il punto e il carattere successivo. Ad esempio, utilizza `N. C. A. A.`, *non* `N.C.A.A.`
-   Utilizza parole reali o inventate che sono pronunciabili nella lingua per le parole che sono difficili da pronunciare.
-   Utilizza l'ortografia dei numeri senza trattini, ad esempio per `75` utilizza
    -   *Francese:* `soixante quinze`
    -   *Tedesco:* <code>f&uuml;nfundsiebzig</code>
    -   *Spagnolo:* `setenta y cinco`
    -   *Portoghese (Brasile):* `setenta e cinco`
-   Puoi includere più parole separate da spazi ma il servizio applica un massimo di 40 caratteri in totale, non includendo gli spazi.

#### Linee guida per il giapponese
{: #wordLanguages-jaJP}

-   Utilizza solo caratteri Katana a larghezza intera utilizzando il simbolo di allungamento <code>&#8213;</code> (*chou-on* o &#38263;&#38899;, in giapponese). Non utilizzare i caratteri a metà larghezza.
-   Utilizza i suoni contratti (*yoh-on* o &#25303;&#38899;, in giapponese) solo nei seguenti contesti sillabici:

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Utilizza solo le seguenti sillabe dopo un suono assimilato (*soku-on* o &#20419;&#38899;, in giapponese):

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   Non utilizzare <code>&#12531;</code> come primo carattere di una parola. Ad esempio, utilizza <code>&#12454;&#12540;&#12531;&#12488;</code> invece di <code>&#12531;&#12540;&#12488;</code>, che non è valido.
-   Molte parole composte sono formate da *prefisso+nome* o *nome+suffisso*. Il vocabolario di base del servizio copre la maggior parte delle parole composte che ricorrono frequentemente (ad esempio <code>&#x9577;&#x96FB;&#x8A71;</code> e <code>&#x53E4;&#x65B0;&#x805E;</code>) ma non le parole composte che ricorrono con scarsa frequenza. Se il tuo corpus contiene comunemente parole composte, aggiungile come una singola parola come primo passo della tua personalizzazione. Ad esempio, <code>&#x53E4;&#x925B;&#x7B46;</code> non è comune in testo generico in giapponese; se la utilizzi spesso, aggiungila al tuo modello personalizzato per migliorare l'accuratezza della trascrizione.
-   Non utilizzare un suono assimilato in coda.

#### Linee guida per il coreano
{: #wordLanguages-koKR}

-   Utilizza caratteri, simboli e sillabe in coreano Hangul.
-   Puoi anche utilizzare i caratteri alfabetici latini (inglese): `a-z` e `A-Z`.
-   Non utilizzare caratteri o simboli non inclusi nei set precedenti.

### Utilizzo del campo display_as
{: #displayAs}

Il campo `display_as` specifica il modo in cui una parola viene visualizzata in una trascrizione. Il suo uso previsto è per i casi in cui desideri che il servizio visualizzi una stringa diversa dall'ortografia della parola. Ad esempio, puoi indicare che la parola `hhonors` debba essere visualizzata come `HHonors` indipendentemente dal fatto che suoni come `hilton honors` o `h honors`.

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

Come altro esempio, puoi indicare che la parola `IBM` deve essere visualizzata come <code>IBM&trade;</code>.

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### Interazione con la formattazione intelligente e l'oscuramento numerico
{: #displaySmart}

Se utilizzi i parametri `smart_formatting` o `redaction` con una richiesta di riconoscimento, tieni conto del fatto che il servizio applica la formattazione intelligente e l'oscuramento a una parola prima di prendere in considerazione il campo `display_as` per la parola. Potresti dover fare delle prove con i risultati per assicurarti che le funzioni non interferiscano con il modo in cui vengono visualizzate le tue parole personalizzate. Potresti anche dover aggiungere delle parole personalizzate per tener conto degli effetti.

Supponi ad esempio di aggiungere la parola personalizzata `one` con un campo `display_as` di `one`. La formattazione intelligente modifica la parola `one` nel numero `1`, e il valore di modalità di visualizzazione (display-as) non viene applicato. Per evitare questi problema, puoi aggiungere una parola personalizzata per il numero `1` e applicare lo stesso campo `display_as` a tale parola.

Per ulteriori informazioni sull'utilizzo di queste funzioni, vedi [Formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting) e [Oscuramento numerico](/docs/services/speech-to-text/output.html#redaction).

### Cosa succede quando aggiungo o modifico una parola personalizzata?
{: #parseWord}

Il modo in cui il servizio risponde a una richiesta di aggiunta o modifica di una parola personalizzata dipende da quali valori fornisci e dal fatto che la parola esista o meno nel vocabolario di base del servizio.

<table>
  <caption>Tabella 1. Aggiunta di parole personalizzate con campi differenti</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom">Il campo <code>sounds_like</code> è...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom">Il campo <code>display_as</code> è...</th>
    <th style="text-align:left; vertical-align:bottom">Risposta</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Non specificato
    </td>
    <td style="text-align:center; vertical-align:top">
      Non specificato
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se la parola non esiste nel vocabolario di base del
          servizio,</em> il servizio imposta il campo <code>sounds_like</code>
          sulla sua pronuncia della parola. Imposta il campo
          <code>display_as</code> sul valore del campo
          <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se la parola esiste nel vocabolario di base del servizio,</em>
          il servizio lascia i campi <code>sounds_like</code> e
          <code>display_as</code> vuoti. Questi campi sono vuoti solo se
          la parola esiste nel vocabolario di base del servizio. La presenza
          della parola nella risorsa di parole del modello è innocua ma non
          necessaria.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Specificato
    </td>
    <td style="text-align:center; vertical-align:top">
      Non specificato
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se il campo <code>sounds_like</code> è valido, </em> il
          servizio imposta il campo <code>display_as</code> sul valore
          del campo <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se il campo <code>sounds_like</code> non è valido:</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              Il metodo <code>POST
                /v1/customizations/{customization_id}/words</code>
              aggiunge un campo <code>error</code> alla parola nella
              risorsa di parole del modello.
            </li>
            <li style="margin:10px 0px; line-height:120%">
              Il metodo <code>PUT
                /v1/customizations/{customization_id}/words/{word_name}</code>
              non riesce con un codice di risposta 400 e un messaggio di errore. Il
              servizio non aggiunge la parola alla risorsa di parole.
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Non specificato
    </td>
    <td style="text-align:center; vertical-align:top">
      Specificato
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se la parola non esiste nel vocabolario di base
          del servizio, </em> il servizio imposta il campo <code>sounds_like</code>
          sulla sua pronuncia della parola e lascia il campo
          <code>display_as</code> come specificato.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se la parola esiste nel vocabolario di base del servizio, </em>
          il servizio lascia <code>sounds_like</code> vuoto e lascia il
          campo <code>display_as</code> come specificato.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Specificato
    </td>
    <td style="text-align:center; vertical-align:top">
      Specificato
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se il campo <code>sounds_like</code> è valido, </em> il
          servizio imposta i campi <code>sounds_like</code> e <code>display_as</code>
          sui valori specificati.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se il campo <code>sounds_like</code> non è valido, </em>
          il servizio risponde come fa nel caso in cui il campo
          <code>sounds_like</code> sia specificato mentre il campo
          <code>display_as</code> invece no.
        </li>
      </ul>
    </td>
  </tr>
</table>

## Convalida di una risorsa di parole
{: #validateModel}

Soprattutto quando aggiungi un corpus a un modello di lingua personalizzato o aggiungi più parole personalizzate contemporaneamente, esamina le parole OOV nella risorsa di parole del modello.

-   *Cerca gli errori di battitura e di altro tipo.* Soprattutto quando aggiungi dei corpora, che possono essere di ampie dimensioni, è facile commettere degli errori. Gli errori di battitura in un file di corpus (o grammatica) hanno la conseguenza imprevista di aggiungere nuove parole alla risorse di parole di un modello, come succede con le tag HTML dal formato errato che vengono lasciate in un file di corpus.
-   *Verifica le pronunce dal suono simile (sounds-like).* Il servizio genera le pronunce dal suono simile (sound-like) dalle parole OOV automaticamente. Nella maggior parte dei casi, queste pronunce sono sufficienti. Per le parole però che hanno delle ortografie insolite o che sono difficili da pronunciare, e per gli acronimi e i termini tecnici, si consiglia di riesaminare l'accuratezza delle pronunce.

Per convalidare e, se necessario, correggere una parola per un modello personalizzato, indipendentemente da come è stata aggiunta alla risorsa di parole, utilizza i seguenti metodi:

-   Elenca tutte le parole da un modello personalizzato utilizzando il metodo `GET /v1/customizations/{customization_id}/words` o interroga una singola parola con il metodo `GET /v1/customizations/{customization_id}/words/{word_name}`. Per ulteriori informazioni, vedi [Elenco delle parole da un modello di lingua personalizzato](/docs/services/speech-to-text/language-words.html#listWords).
-   Modifica le parole in un modello personalizzato per correggere gli errori o per aggiungere valori di suono simile (sounds-like) e di modalità di visualizzazione (display-as) utilizzando il metodo `POST /v1/customizations/{customization_id}/words` o quello `PUT /v1/customizations/{customization_id}/words/{word_name}`. Per ulteriori informazioni, vedi [Utilizzo delle parole personalizzate](#workingWords).
-   Elimina le parole estranee introdotte erroneamente (ad esempio da errori di battitura o di altro tipo in un corpus) utilizzando il metodo `DELETE /v1/customizations/{customization_id}/words/{word_name}`. Per ulteriori informazioni, vedi [Eliminazione di una parola da un modello di lingua personalizzato](/docs/services/speech-to-text/language-words.html#deleteWord).
    -   Se la parola era stata estratta da un corpus, puoi invece aggiornare il file di testo di corpus per correggere l'errore e ricaricare quindi il file utilizzando il parametro `allow_overwrite` del metodo `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`. Per ulteriori informazioni, vedi [Aggiungi un corpus al modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html#addCorpus). 
    -   Se la parola era stata estratta da una grammatica, puoi aggiornare il file di grammatica per correggere l'errore e ricaricare quindi il file utilizzando il parametro `allow_overwrite` del metodo `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`. Per ulteriori informazioni, vedi [Aggiungi una grammatica al modello di lingua personalizzato](/docs/services/speech-to-text/grammar-add.html#addGrammar). 
