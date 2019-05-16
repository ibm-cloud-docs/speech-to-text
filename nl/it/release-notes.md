---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Note sulla release 
{: #release-notes}

Le seguenti sezioni documentano le nuove funzioni e le modifiche che sono state incluse in ogni release e aggiornamento del servizio {{site.data.keyword.speechtotextfull}}. Le informazioni includono tutte le limitazioni note. Salvo diversa indicazione, tutte le modifiche sono compatibili con le release precedenti e sono automaticamente e trasparentemente disponibili per tutte le applicazioni nuove ed esistenti.
{: shortdesc}

## Limitazioni note
{: #limitations}

Nessuna limitazione nota al momento. 

## 3 aprile 2019
{: #April2019}

I modelli acustici personalizzati accettano ora un massimo di 200 ore di audio. Il limite massimo precedente era di 100 ore di audio.

## 21 marzo 2019
{: #March2019d}

Gli utenti possono ora visualizzare solo le informazioni sulle credenziali del servizio associate al ruolo che è stato assegnato al loro account {{site.data.keyword.cloud_notm}}. Ad esempio, se ti viene assegnato un ruolo `reader`, qualsiasi livello `writer` o superiore delle credenziali del servizio non sarà più visibile. 

Questa modifica non influisce sull'accesso API per gli utenti o le applicazioni con credenziali del servizio esistenti. La modifica influisce solo sulla visualizzazione delle credenziali all'interno di {{site.data.keyword.cloud_notm}}. 

Per ulteriori informazioni sulle chiavi del servizio e sui ruoli utente, vedi [Chiavi API del servizio IAM](/docs/services/watson?topic=watson-api-key-bp#api-key-bp). 

## 15 marzo 2019
{: #March2019c}

Il servizio ora supporta l'audio nel formato A-law (`audio/alaw`). Per ulteriori informazioni, vedi [Formato audio/alaw](/docs/services/speech-to-text/audio-formats.html#alaw).

## Release precedenti 
{: #older}

-   [11 marzo 2019](#March2019b)
-   [4 marzo 2019](#March2019)
-   [28 gennaio 2019](#January2019)
-   [20 dicembre 2018](#December2018b)
-   [13 dicembre 2018](#December2018a)
-   [12 novembre 2018](#November2018b)
-   [7 novembre 2018](#November2018a)
-   [30 ottobre 2018](#October2018b)
-   [9 ottobre 2018](#October2018a)
-   [10 settembre 2018](#September2018b)
-   [7 settembre 2018](#September2018a)
-   [8 agosto 2018](#August2018)
-   [13 luglio 2018](#July2018)
-   [12 giugno 2018](#June2018)
-   [15 maggio 2018](#May2018)
-   [26 marzo 2018](#March2018b)
-   [1 marzo 2018](#March2018a)
-   [1 febbraio 2018](#February2018)
-   [14 dicembre 2017](#December2017)
-   [2 ottobre 2017](#October2017)
-   [14 luglio 2017](#July2017b)
-   [1 luglio 2017](#July2017a)
-   [22 maggio 2017](#May2017)
-   [10 aprile 2017](#April2017)
-   [8 marzo 2017](#March2017)
-   [1 dicembre 2016](#December2016)
-   [22 settembre 2016](#September2016)
-   [30 giugno 2016](#June2016b)
-   [23 giugno 2016](#June2016a)
-   [10 marzo 2016](#March2016)
-   [19 gennaio 2016](#January2016)
-   [17 dicembre 2015](#December2015)
-   [21 settembre 2015](#September2015)
-   [1 luglio 2015](#July2015)

### 11 marzo 2019
{: #March2019b}

-   Per il parametro `max_alternatives`, il servizio accetta ancora un valore di `0`. Se specifichi `0`, il servizio utilizza automaticamente il valore predefinito, `1`. Una modifica apportata durante l'aggiornamento del servizio del 4 marzo ha comportato che il valore `0` restituisca un errore. (Il servizio restituisce un errore se specifichi un valore negativo.)
-   Per il parametro `word_alternatives_threshold`, il servizio accetta ancora un valore di `0`. Una modifica apportata durante l'aggiornamento del servizio del 4 marzo ha comportato che il valore `0` restituisca un errore. (Il servizio restituisce un errore se specifichi un valore negativo.)
-   Il servizio ora restituisce tutti i punteggi di attendibilità con una precisione massima di due posizioni decimali. Sono inclusi i punteggi di attendibilità per trascrizioni, attendibilità della parola, alternative alle parole, risultati di parole chiave ed etichette del parlante.

### 4 marzo 2019
{: #March2019}

I seguenti modelli di lingua a banda stretta sono stati aggiornati per un riconoscimento vocale migliorato:

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

Per impostazione predefinita, il servizio utilizza automaticamente i modelli aggiornati per tutte le richieste di riconoscimento vocale. Se hai dei modelli acustici o di lingua personalizzati basati sui modelli, devi eseguire l'upgrade dei tuoi modelli personalizzati esistenti per sfruttare gli aggiornamenti utilizzando i seguenti metodi:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).

### 28 gennaio 2019
{: #January2019}

Ora l'interfaccia WebSocket supporta l'autenticazione IAM (Identity and Access Management) basata sul token dal codice JavaScript basato sul browser. La limitazione al contrario è stata rimossa. Per stabilire una connessione autenticata con il metodo WebSocket `/v1/recognize`:

-   Se utilizzi l'autenticazione IAM, includi il parametro di query `access_token`. 
-   Se utilizzi le credenziali del servizio Cloud Foundry, includi il parametro di query `watson-token`. 

Per ulteriori informazioni, vedi [Apri una connessione](/docs/services/speech-to-text/websockets.html#WSopen).

### 20 dicembre 2018
{: #December2018b}

L'interfaccia grammaticale è pienamente funzionale in tutte le ubicazioni a partire dall'8 gennaio 2019.
{: note}

-   Il servizio ora supporta le grammatiche per il riconoscimento vocale. Le grammatiche sono disponibili come una funzionalità beta per tutte le lingue che supportano la personalizzazione del modello di lingua. Puoi aggiungere le grammatiche a un modello di lingua personalizzato e utilizzarle per limitare la serie di frasi che il servizio può riconoscere dall'audio. Puoi definire una grammatica in ABNF (Augmented Backus-Naur Form) o nel formato XML.

    I seguenti quattro metodi sono disponibili per utilizzare le grammatiche:
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` aggiunge un file di grammatica a un modello di lingua personalizzato.
    -   `GET /v1/customizations/{customization_id}/grammars ` elenca le informazioni su tutte le grammatiche per un modello personalizzato.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` restituisce le informazioni su una grammatica specifica per un modello personalizzato.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` rimuove una grammatica esistente da un modello personalizzato.

    Puoi utilizzare una grammatica per il riconoscimento vocale con le interfacce WebSocket e HTTP. Utilizza i parametri `language_customization_id` e `grammar_name` per identificare il modello personalizzato e la grammatica che vuoi utilizzare. Al momento, puoi utilizzare una sola grammatica con una richiesta di riconoscimento vocale.

    Per ulteriori informazioni sulle grammatiche, vedi la seguente documentazione:
    -   [Utilizzo di grammatiche con i modelli di lingua personalizzati](/docs/services/speech-to-text/grammar.html)
    -   [Informazioni sulle grammatiche](/docs/services/speech-to-text/grammar-understand.html)
    -   [Aggiunta di una grammatica a un modello di lingua personalizzato](/docs/services/speech-to-text/grammar-add.html)
    -   [Utilizzo di una grammatica per il riconoscimento vocale](/docs/services/speech-to-text/grammar-use.html)
    -   [Gestione delle grammatiche](/docs/services/speech-to-text/grammar-manage.html)
    -   [Grammatiche di esempio](/docs/services/speech-to-text/grammar-examples.html)

    Per informazioni su tutti i metodi dell'interfaccia, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   È ora disponibile una nuova funzione di oscuramento numerico per mascherare i numeri che hanno tre o più cifre consecutive. L'oscuramento ha lo scopo di rimuovere le informazioni personali sensibili, come i numeri delle carte di credito, dalle trascrizioni. Abilita la funzione impostando il parametro `redaction` su `true` in una richiesta di riconoscimento. La funzione è una funzionalità beta disponibile solo per inglese americano, giapponese e coreano. Per ulteriori informazioni, vedi [Oscuramento numerico](/docs/services/speech-to-text/output.html#redaction).
-   I seguenti nuovi modelli di lingua in tedesco e francese sono disponibili con il servizio:
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    Entrambi i nuovi modelli supportano la personalizzazione del modello di lingua (GA) e la personalizzazione del modello acustico (beta). Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).
-   Un nuovo modello di lingua in inglese americano, `en-US_ShortForm_NarrowbandModel`, è ora disponibile. Il nuovo modello è destinato all'uso nelle soluzioni IVR (interactive voice response) e di supporto clienti automatizzato. Il modello supporta la personalizzazione del modello di lingua (GA) e la personalizzazione del modello acustico (beta). 
-   I seguenti modelli di lingua sono stati aggiornati per un riconoscimento vocale migliorato:
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    Per impostazione predefinita, il servizio utilizza automaticamente i modelli aggiornati per tutte le richieste di riconoscimento vocale. Se hai dei modelli acustici o di lingua personalizzati basati sui modelli, devi eseguire l'upgrade dei tuoi modelli personalizzati esistenti per sfruttare gli aggiornamenti utilizzando i seguenti metodi:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).
-   Il servizio ora supporta l'audio nel formato G.729 (`audio/g729`). Il servizio supporta solo G.729 Annex D per l'audio a banda stretta. Per ulteriori informazioni sui formati audio supportati, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   La funzione etichette del parlante è ora disponibile per il modello a banda stretta per l'inglese britannico (`en-GB_NarrowbandModel`). La funzione è una funzionalità beta per tutti i linguaggi supportati. Per ulteriori informazioni, vedi [Etichette del parlante](/docs/services/speech-to-text/output.html#speaker_labels).
-   La quantità massima di audio che puoi aggiungere a un modello acustico personalizzato è stata aumentata da 50 a 100 ore.

### 13 dicembre 2018
{: #December2018a}

Il servizio è ora disponibile nell'ubicazione {{site.data.keyword.cloud_notm}} di Londra (**eu-gb**). Come tutte le altre ubicazioni, Londra utilizza l'autenticazione IAM basata sul token. Tutte le nuove istanze del servizio che crei in questa ubicazione utilizzano l'autenticazione IAM.

### 12 novembre 2018
{: #November2018b}

Il servizio ora supporta la formattazione intelligente per il riconoscimento vocale per il giapponese. In precedenza, il servizio supportava la formattazione intelligente solo per spagnolo e inglese americano. La funzione è una funzionalità beta per tutti i linguaggi supportati. Per ulteriori informazioni, vedi [Formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting).

### 7 novembre 2018
{: #November2018a}

Il servizio è ora disponibile nell'ubicazione {{site.data.keyword.cloud_notm}} di Tokyo (**jp-tok**). Come tutte le altre ubicazioni, Tokyo utilizza l'autenticazione IAM basata sul token. Tutte le nuove istanze del servizio che crei in questa ubicazione utilizzano l'autenticazione IAM.

### 30 ottobre 2018
{: #October2018b}

Il servizio ha eseguito la migrazione all'autenticazione IAM basata sul token per tutte le ubicazioni. Tutti i servizi {{site.data.keyword.cloud_notm}} utilizzano ora l'autenticazione IAM. Il servizio {{site.data.keyword.speechtotextshort}} è stato migrato in ogni ubicazione nelle seguenti date:

-   Dallas (**us-south**): 30 ottobre 2018
-   Francoforte (**eu-de**): 30 ottobre 2018
-   Washington, DC (**us-east**): 12 giugno 2018
-   Sydney (**au-syd**): 15 maggio 2018

La migrazione all'autenticazione IAM riguarda sia le nuove istanze che quelle esistenti indifferentemente:

-   *Tutte le nuove istanze del servizio che crei in ogni ubicazione* utilizzano ora l'autenticazione IAM per accedere al servizio. Puoi passare un token di connessione o una chiave API: i token supportano le richieste autenticate senza credenziali del servizio incorporate in ogni chiamata; le chiavi API utilizzano l'autenticazione di base HTTP. Quando utilizzi uno degli SDK {{site.data.keyword.watson}}, puoi passare la chiave API e lasciare che l'SDK gestisca il ciclo di vita dei token.
-   *Le istanze del servizio esistenti che hai creato in un'ubicazione prima della data di migrazione indicata* continuano ad utilizzare `{username}` e `{password}` dalle loro precedenti credenziali del servizio Cloud Foundry per l'autenticazione finché non le migri in modo che utilizzino l'autenticazione IAM. Per ulteriori informazioni sulla migrazione all'autenticazione IAM, vedi [Migrazione di istanze del servizio Cloud Foundry a un gruppo di risorse](https://{DomainName}/docs/resources/instance_migration.html).

Per ulteriori informazioni, consulta la seguente documentazione:

-   Per ulteriori informazioni su quale meccanismo di autenticazione viene utilizzato dalla tua istanza del servizio, visualizza le tue credenziali del servizio facendo clic sull'istanza sul [Dashboard {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/dashboard/apps){: new_window}.
-   Per ulteriori informazioni sull'utilizzo dei token IAM con i servizi Watson, vedi [Autenticazione con i token IAM](/docs/services/watson/getting-started-iam.html).
-   Per ulteriori informazioni sull'utilizzo delle chiavi API IAM con i servizi Watson, vedi [Chiavi API del servizio IAM](/docs/services/watson/apikey-bp.html).
-   Per degli esempi di utilizzo dell'autenticazione IAM, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 9 ottobre 2018
{: #October2018a}

-   L'intestazione `Content-Type` è ora facoltativa per le richieste di riconoscimento vocale. Il servizio ora rileva automaticamente il formato audio (tipo MIME) della maggior parte dei tipi di audio. Devi continuare a specificare il tipo di contenuto per i seguenti formati:
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Dove indicato, il tipo di contenuto che specifichi per questi formati deve includere la frequenza di campionamento e può facoltativamente includere il numero di canali e endian dell'audio. Per tutti gli altri formati audio, puoi omettere il tipo di contenuto o specificarne uno del tipo `application/octet-stream` in modo che il servizio rilevi automaticamente il formato.

    Quando utilizzi il comando `curl` per effettuare una richiesta di riconoscimento vocale con l'interfaccia HTTP, devi specificare il formato audio con l'intestazione `Content-Type` e specificare `"Content-Type: application/octet-stream"` o `"Content-Type:"`. Se ometti completamente l'intestazione, `curl` utilizza il valore predefinito `application/x-www-form-urlencoded`. La maggior parte degli esempi in questa documentazione continua a specificare il formato per le richieste di riconoscimento vocale indipendentemente dal fatto che sia necessario.
    {: important}

    Questa modifica si applica ai seguenti metodi:
    -   `/v1/recognize` per le richieste WebSocket. Il campo `content-type` del messaggio di testo che invii per avviare una richiesta su una connessione WebSocket aperta è ora facoltativo.
    -   `POST /v1/recognize` per le richieste HTTP sincrone. L'intestazione `Content-Type` è ora facoltativa. (Per le richieste a più parti, anche il campo `part_content_type` dei metadati JSON è ora facoltativo.)
    -   `POST /v1/recognitions` per le richieste HTTP asincrone. L'intestazione `Content-Type` è ora facoltativa. 

    Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   Il modello a banda larga per il portoghese brasiliano, `pt-BR_BroadbandModel`, è stato aggiornato per fornire un riconoscimento vocale migliorato. Per impostazione predefinita, il servizio utilizza automaticamente il modello aggiornato per tutte le richieste di riconoscimento. Se hai dei modelli acustici o di lingua personalizzati basati su questo modello, devi eseguire l'upgrade dei tuoi modelli personalizzati esistenti per sfruttare gli aggiornamenti utilizzando i seguenti metodi:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).
-   Il parametro `customization_id` dei metodi di riconoscimento vocale è obsoleto e sarà rimosso in una futura release. Per specificare un modello di lingua personalizzato per una richiesta di riconoscimento vocale, utilizza invece il parametro `language_customization_id`. Questa modifica si applica ai seguenti metodi:
    -   `/v1/recognize` per le richieste WebSocket
    -   `POST /v1/recognize` per le richieste HTTP sincrone (incluse le richieste a più parti)
    -   `POST /v1/recognitions` per le richieste HTTP asincrone
-   A partire dal primo ottobre 2018, ti viene ora effettuato un addebito per tutto l'audio che passi al servizio per il riconoscimento vocale. I primi mille minuti di audio che invii ogni mese non sono più gratuiti. Per ulteriori informazioni sui piani dei prezzi per il servizio, vedi il servizio {{site.data.keyword.speechtotextshort}} nel [Catalogo {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/speech-to-text){: new_window}.

### 10 settembre 2018
{: #September2018b}

Per un elenco dei problemi che sono stati risolti dalla release iniziale, vedi [Problemi risolti](#known_issues).
{: important}

-   Il servizio ora supporta un modello a banda larga in tedesco, `de-DE_BroadbandModel`. Il nuovo modello in tedesco supporta la personalizzazione del modello di lingua (generalmente disponibile) e la personalizzazione del modello acustico (beta). 
    -   Per informazioni su come il servizio analizza i corpora per il tedesco, vedi [Analisi di inglese, francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Per ulteriori informazioni sulla creazione di pronunce dal suono simile per le parole personalizzate in tedesco, vedi [Linee guida per francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   I modelli di portoghese brasiliano esistenti, `pt-BR_BroadbandModel` e `pt-BR_NarrowbandModel`, supportano ora la personalizzazione del modello di lingua (generalmente disponibile). I modelli sono stati aggiornati per abilitare questo supporto, per cui non è necessario alcun upgrade dei modelli acustici personalizzati esistenti.
    -   Per informazioni su come il servizio analizza i corpora per il portoghese brasiliano, vedi [Analisi di inglese, francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Per ulteriori informazioni sulla creazione di pronunce dal suono simile per le parole personalizzate in portoghese brasiliano, vedi [Linee guida per francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Sono disponibili delle nuove versioni dei modelli a banda stretta e a banda larga per l'inglese americano e il giapponese:
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    I nuovi modelli offrono un riconoscimento vocale migliorato. Per impostazione predefinita, il servizio utilizza automaticamente i modelli aggiornati per tutte le richieste di riconoscimento. Se hai dei modelli acustici o di lingua personalizzati basati su questi modelli, devi eseguire l'upgrade dei tuoi modelli personalizzati esistenti per sfruttare gli aggiornamenti utilizzando i seguenti metodi:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).
-   Le funzioni di individuazione di parole chiave e di alternative alle parole sono ora generalmente disponibili (GA) invece che una funzionalità beta per tutte le lingue. Per ulteriori informazioni, vedi
    -   [Individuazione di parole chiave](/docs/services/speech-to-text/output.html#keyword_spotting)
    -   [Alternative alle parole](/docs/services/speech-to-text/output.html#word_alternatives)

### Problemi risolti
{: #known_issues}

I seguenti problemi noti che erano associati all'interfaccia di personalizzazione sono stati risolti. Questo elenco viene conservato per gli utenti che possono aver riscontrato quei problemi nel passato.

-   Se aggiungi dei dati a un modello di lingua o acustico personalizzato, devi ripetere l'addestramento del modello prima di utilizzarlo per il riconoscimento vocale. Il problema si presenta nel seguente scenario:

    1.  L'utente crea un nuovo modello personalizzato (di lingua o acustico) e addestra il modello.
    1.  L'utente aggiunge ulteriori risorse (parole, corpora o audio) al modello personalizzato ma non ripete l'addestramento del modello.
    1.  L'utente non può utilizzare il modello personalizzato per il riconoscimento vocale. Il servizio restituisce un errore del seguente formato quando utilizzato con una richiesta di riconoscimento vocale:

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    Per evitare questo problema, l'utente deve ripetere l'addestramento del modello personalizzato con i dati più recenti. L'utente può quindi utilizzare il modello personalizzato con il riconoscimento vocale.
-   Prima di addestrare un modello acustico o di lingua personalizzato esistente, devi eseguirne l'upgrade all'ultima versione del suo modello di base. Il problema si presenta nel seguente scenario:

    1.  L'utente ha un modello personalizzato esistente (di lingua o acustico) basato su un modello che è stato aggiornato.
    1.  L'utente addestra il modello personalizzato esistente con una versione precedente del modello di base senza prima eseguire l'upgrade all'ultima versione del modello di base.
    1.  L'utente non può utilizzare il modello personalizzato per il riconoscimento vocale. 

    Per evitare questo problema, l'utente deve utilizzare il metodo `POST /v1/customizations/{customization_id}/upgrade_model` o `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` per eseguire l'upgrade del modello personalizzato all'ultima versione del suo modello di base. L'utente può quindi utilizzare il modello personalizzato con il riconoscimento vocale.


Entrambi questi problemi sono stati risolti in fase di produzione.

### 7 settembre 2018
{: #September2018a}

L'interfaccia REST HTTP basata sulla sessione non è più supportata. Tutte le informazioni relative alle sessioni sono state rimosse dalla documentazione. I seguenti metodi non sono più disponibili:
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Se la tua applicazione utilizza l'interfaccia delle sessioni, devi eseguire la migrazione a una delle interfacce REST HTTP rimanenti o all'interfaccia WebSocket. Per ulteriori informazioni, vedi l'aggiornamento del servizio dell'[8 agosto 2018](#August2018).

### 8 agosto 2018
{: #August2018}

L'interfaccia REST HTTP basata sulla sessione è obsoleta a partire dall'**8 agosto 2018**. Tutti i metodi dell'API delle sessioni saranno rimossi dal servizio a partire dal **7 settembre 2018**, dopodiché non potrai più utilizzare l'interfaccia basata sulla sessione. Questo avviso di deprecazione immediata e di rimozione di 30 giorni si applica ai seguenti metodi:

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Se la tua applicazione utilizza l'interfaccia delle sessioni, devi eseguire la migrazione a una delle seguenti interfacce entro il 7 settembre:
{: important}

-   Per il riconoscimento vocale basato sul flusso (inclusi i casi di utilizzo dal vivo), utilizza l'[interfaccia WebSocket](/docs/services/speech-to-text/websockets.html), che fornisce l'accesso a risultati provvisori e latenza più bassa.
-   Per il riconoscimento vocale basato sul file, utilizza una delle seguenti interfacce:

    -   Per file brevi, fino a cinque minuti di audio, utilizza l'[interfaccia HTTP sincrona](/docs/services/speech-to-text/http.html) `(POST /v1/recognize`) o l'[interfaccia HTTP asincrona](/docs/services/speech-to-text/async.html) (`POST /v1/recognitions`).
    -   Per file più lunghi di pochi minuti di audio, utilizza l'interfaccia HTTP asincrona. L'interfaccia HTTP asincrona accetta fino a 1 GB di dati audio con una sola richiesta.

Le interfacce WebSocket e HTTP forniscono gli stessi risultati dell'interfaccia delle sessioni (solo l'interfaccia WebSocket fornisce dei risultati provvisori). Puoi anche utilizzare uno degli SDK Watson, che semplificano lo sviluppo dell'applicazione con qualsiasi interfaccia. Per ulteriori informazioni, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 13 luglio 2018
{: #July2018}

Il modello a banda larga per lo spagnolo, `es-ES_NarrowbandModel`, è stato aggiornato per fornire un riconoscimento vocale migliorato. Per impostazione predefinita, il servizio utilizza automaticamente il modello aggiornato per tutte le richieste di riconoscimento. Se hai dei modelli acustici o di lingua personalizzati basati su questo modello, devi eseguire l'upgrade dei tuoi modelli personalizzati per sfruttare gli aggiornamenti utilizzando i seguenti metodi:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).

A partire da questo aggiornamento, sono disponibili le seguenti due versioni del modello a banda larga per lo spagnolo:

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (corrente)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (precedente)

La seguente versione del modello non è più disponibile:

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

Una richiesta di riconoscimento che tenta di utilizzare un modello personalizzato basato sul modello di base ora non disponibile, utilizza il modello di base più recente senza alcuna personalizzazione. Il servizio restituisce il seguente messaggio di avvertenza: `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` Per riprendere a utilizzare un modello personalizzato basato sul modello non disponibile, devi prima eseguire l'upgrade del modello utilizzando il metodo `upgrade_model` appropriato descritto precedentemente.

### 12 giugno 2018
{: #June2018}

Le seguenti funzioni sono abilitate per le applicazioni ospitate in Washington, DC (**us-east**): 

-   Ora il servizio supporta un nuovo processo di autenticazione API. Per ulteriori informazioni, vedi l'[aggiornamento del servizio del 30 ottobre 2018](#October2018b).
-   Ora il servizio supporta l'intestazione `X-Watson-Metadata` e il metodo `DELETE /v1/user_data`. Per ulteriori informazioni, vedi [Sicurezza delle informazioni](/docs/services/speech-to-text/information-security.html). 

### 15 maggio 2018
{: #May2018}

Le seguenti funzioni sono abilitate per le applicazioni ospitate in Sydney (**au-syd**): 

-   Ora il servizio supporta un nuovo processo di autenticazione API. Per ulteriori informazioni, vedi l'[aggiornamento del servizio del 30 ottobre 2018](#October2018b).
-   Ora il servizio supporta l'intestazione `X-Watson-Metadata` e il metodo `DELETE /v1/user_data`. Per ulteriori informazioni, vedi [Sicurezza delle informazioni](/docs/services/speech-to-text/information-security.html). 

### 26 marzo 2018
{: #March2018b}

-   Ora il servizio supporta la personalizzazione del modello di lingua per il francese, `fr-FR_BroadbandModel`. Il modello per il francese è generalmente disponibile per l'utilizzo nella produzione con la personalizzazione del modello di lingua.
    -   Per ulteriori informazioni su come il servizio analizza i corpora per il francese, vedi [Analisi di inglese, francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Per ulteriori informazioni sulla creazione di pronunce dal suono simile per le parole personalizzate in francese, vedi [Linee guida per francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   I modelli a banda stretta per lo spagnolo e il coreano, `es-ES_NarrowbandModel` e `ko-KR_NarrowbandModel` e il modello a banda larga per il francese, `fr-FR_BroadbandModel`, sono stati aggiornati per fornire un riconoscimento vocale migliorato. Per impostazione predefinita, il servizio utilizza automaticamente i modelli aggiornati per tutte le richieste di riconoscimento. Se hai dei modelli acustici o di lingua personalizzati basati su entrambi questi modelli, devi eseguire l'upgrade dei tuoi modelli personalizzati per sfruttare gli aggiornamenti utilizzando i seguenti metodi:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).
-   Il parametro `version` dei seguenti metodi è stato ridenominato `base_model_version`:

    -   `/v1/recognize` per le richieste WebSocket
    -   `POST /v1/recognize` per le richieste HTTP senza sessione. 
    -   `POST /v1/sessions` per le richieste HTTP basate sulla sessione. 
    -   `POST /v1/recognitions` per le richieste HTTP asincrone

    Il parametro `base_model_version` specifica la versione di un modello di base che deve essere utilizzato per il riconoscimento vocale. Per ulteriori informazioni, vedi [Effettuazione di richieste di riconoscimento con modelli personalizzati di cui è stato eseguito l'upgrade](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition) e [Versione del modello di base](/docs/services/speech-to-text/input.html#version).
-   La formattazione intelligente è ora supportata per lo spagnolo nonché per l'inglese americano. Per l'inglese americano, la funzione ora converte anche le stringhe di parole chiave in simboli di punteggiatura per i punti, le virgole, i punti di domanda e i punti esclamativi. Per ulteriori informazioni, vedi [Formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting).

### 1 marzo 2018
{: #March2018a}

I modelli a banda larga per lo spagnolo e il francese, `es-ES_BroadbandModel` e `fr-FR_BroadbandModel`, sono stati aggiornati per un riconoscimento vocale migliorato. Per impostazione predefinita, il servizio utilizza automaticamente i modelli aggiornati per tutte le richieste di riconoscimento. Se hai dei modelli acustici o di lingua personalizzati basati su entrambi questi modelli, devi eseguire l'upgrade dei tuoi modelli personalizzati per sfruttare gli aggiornamenti utilizzando i seguenti metodi:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Per ulteriori informazioni, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html). La sezione presenta le regole per l'upgrade dei modelli personalizzati, gli effetti dell'upgrade e gli approcci per l'utilizzo dei modelli aggiornati.

### 1 febbraio 2018
{: #February2018}

Il servizio ora offre dei modelli per la lingua coreana per il riconoscimento vocale: `ko-KR_BroadbandModel` per l'audio campionato a un minimo di 16 kHz e `ko-KR_NarrowbandModel` per l'audio campionato a un minimo di 8 kHz. Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html). 

Per la personalizzazione del modello di lingua, i modelli per il coreano sono generalmente disponibili per l'utilizzo nella produzione; per la personalizzazione del modello acustico sono una funzionalità beta. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).

-   Per ulteriori informazioni su come il servizio analizza i corpora per il coreano, vedi [Analisi in coreano](/docs/services/speech-to-text/language-resource.html#corpusLanguages-koKR).
-   Per ulteriori informazioni sulla creazione di pronunce dal suono simile per le parole personalizzate in coreano, vedi [Linee guida per il coreano](/docs/services/speech-to-text/language-resource.html#wordLanguages-koKR).

### 14 dicembre 2017
{: #December2017}

-   I modelli per l'inglese americano, `en-US_BroadbandModel` e `en-US_NarrowbandModel`, sono stati aggiornati per un riconoscimento vocale migliorato. Per impostazione predefinita, il servizio utilizza automaticamente i modelli aggiornati per tutte le richieste di riconoscimento. Se hai dei modelli acustici o di lingua personalizzati basati sui modelli per l'inglese americano, devi eseguire l'upgrade dei tuoi modelli personalizzati per sfruttare gli aggiornamenti utilizzando i seguenti metodi:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Per ulteriori informazioni sulla procedura, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html). La sezione presenta le regole per l'upgrade dei modelli personalizzati, gli effetti dell'upgrade e gli approcci per l'utilizzo dei modelli aggiornati. Al momento, i metodi si applicano solo ai nuovi modelli di base per l'inglese americano. Ma le stesse informazioni si applicheranno agli upgrade di altri modelli di base come diventano disponibili.
-   I vari metodi per effettuare le richieste di riconoscimento ora includono il parametro `base_model_version` che puoi utilizzare per inizializzare le richieste che utilizzano le versioni precedenti o aggiornate dei modelli personalizzati e di base. Sebbene sia concepito principalmente per l'utilizzo con i modelli personalizzati di cui è stato eseguito l'upgrade, il parametro `base_model_version` può essere utilizzato anche senza i modelli personalizzati. Per ulteriori informazioni, vedi [Versione del modello di base](/docs/services/speech-to-text/input.html#version). 
-   Il servizio ora supporta la personalizzazione del modello acustico come una funzionalità beta per tutte le lingue disponibili. Puoi creare dei modelli acustici personalizzati per modelli a banda larga e stretta per tutte le lingue. Per un'introduzione alla personalizzazione, inclusa quella del modello acustico, vedi [L'interfaccia di personalizzazione](/docs/services/speech-to-text/custom.html).
-   Ora il servizio supporta la personalizzazione del modello di lingua per i modelli per l'inglese britannico, `en-GB_BroadbandModel` e `en-GB_NarrowbandModel`. Sebbene il servizio gestisca le parole personalizzate e i corpora in inglese americano e britannico in modo generalmente simile, esistono alcune importanti differenze:
    -   Per ulteriori informazioni su come il servizio analizza i corpora per l'inglese britannico, vedi [Analisi di inglese, francese, tedesco, spagnolo e portoghese (Brasile)](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Per ulteriori informazioni sulla creazione di pronunce dal suono simile per le parole personalizzate in inglese britannico, vedi [Linee guida per l'inglese (Stati Uniti e Regno Unito)](/docs/services/speech-to-text/language-resource.html#wordLanguages-enUS-enGB). Nello specifico, per l'inglese britannico, non puoi utilizzare punti o trattini in pronunce dal suono simile.
-   La personalizzazione del modello di lingua e tutti i parametri associati sono ora generalmente disponibili (GA) per tutte le lingue supportate: giapponese, spagnolo, inglese britannico e inglese americano.

### 2 ottobre 2017
{: #October2017}

-   L'interfaccia di personalizzazione ora offre la personalizzazione del modello acustico. Puoi creare dei modelli acustici personalizzati che adattano i modelli di base del servizio al tuo ambiente e ai parlanti. Popola e addestra un modello acustico personalizzato sull'audio che più si avvicina alla firma acustica dell'audio che vuoi trascrivere. Utilizza quindi il modello acustico personalizzato con le richieste di riconoscimento per aumentare l'accuratezza del riconoscimento vocale.

    I modelli acustici personalizzati integrano i modelli di lingua personalizzati. Puoi addestrare un modello acustico personalizzato con un modello di lingua personalizzato e puoi utilizzarli entrambi durante il riconoscimento vocale. La personalizzazione del modello acustico è un'interfaccia beta disponibile solo per l'inglese americano, lo spagnolo e il giapponese.

    -   Per ulteriori informazioni sulle lingue supportate dall'interfaccia di personalizzazione e il livello di supporto disponibile per ogni lingua, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).
    -   Per ulteriori informazioni sull'interfaccia di personalizzazione del sevizio, vedi [L'interfaccia di personalizzazione](/docs/services/speech-to-text/custom.html).
    -   Per ulteriori informazioni sulla creazione di un modello acustico personalizzato, vedi [Creazione di un modello acustico personalizzato](/docs/services/speech-to-text/acoustic-create.html).
    -   Per ulteriori informazioni sull'utilizzo di un modello acustico personalizzato, vedi [Utilizzo di un modello acustico personalizzato](/docs/services/speech-to-text/acoustic-use.html).
    -   Per ulteriori informazioni su tutti i metodi dell'interfaccia di personalizzazione, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Per la personalizzazione del modello di lingua, il servizio ora include una funzione beta che imposta un peso di personalizzazione facoltativo per un modello di lingua personalizzato. Un peso di personalizzazione specifica il peso relativo da dare alle parole da un modello di lingua personalizzato rispetto alle parole dal vocabolario di base del servizio. Puoi impostare un peso di personalizzazione durante il riconoscimento vocale e l'addestramento. Per ulteriori informazioni, vedi [Utilizzo del peso di personalizzazione](/docs/services/speech-to-text/language-use.html#weight). 
-   Il modello di lingua `ja-JP_BroadbandModel` è stato aggiornato per acquisire i miglioramenti nel modello di base. L'upgrade non influisce sui modelli personalizzati esistenti basati sul modello.
-   Il servizio ora include un parametro per specificare l'endian dell'audio inviato nel formato `audio/l16` PCM (Pulse-Code Modulation) lineare a 16 bit. Oltre a specificare i parametri `rate` e `channels` con il formato, puoi ora specificare anche `big-endian` o `little-endian` con il parametro `endianness`. Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).

### 14 luglio 2017
{: #July2017b}

-   Il servizio ora supporta la trascrizione dell'audio nel formato MP3 o MPEG (Motion Picture Experts Group). Per ulteriori informazioni sui formati audio supportati, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   L'interfaccia di personalizzazione del modello di lingua ora supporta lo spagnolo come una funzionalità beta. Puoi creare un modello personalizzato basato su entrambi i modelli di lingua per lo spagnolo di base: `es-ES_BroadbandModel` o `es-ES_NarrowbandModel`; per ulteriori informazioni, vedi [Creazione di un modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html). I prezzi per le richieste di riconoscimento che utilizzano i modelli di lingua personalizzati per lo spagnolo sono gli stessi di quelli per le richieste che utilizzano i modelli per l'inglese americano e il giapponese.
-   L'oggetto `CreateLanguageModel` JSON che passi al metodo `POST /v1/customizations` per creare un nuovo modello di lingua personalizzato ora include il campo `dialect`. Il campo specifica il dialetto della lingua che deve essere utilizzato con il modello personalizzato. Per impostazione predefinita, il dialetto corrisponde alla lingua del modello di base. Il parametro è significativo solo per i modelli per lo spagnolo, per cui il servizio può creare un modello personalizzato adatto per un discorso in uno dei seguenti dialetti:
    -   `es-ES` per lo spagnolo castigliano (valore predefinito)
    -   `es-LA` per lo spagnolo latino americano
    -   `es-US` per lo spagnolo nord americano (messicano)

    I metodi `GET /v1/customizations` e `GET /v1/customizations/{customization_id}` dell'interfaccia di personalizzazione includono il dialetto di un modello personalizzato nei propri input. Per ulteriori informazioni, vedi [Creazione di un modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html#createModel-language) e [Elenco dei modelli di lingua personalizzati](/docs/services/speech-to-text/language-models.html#listModels-language).
-   I nomi dei modelli di lingua `en-UK_BroadbandModel` e `en-UK_NarrowbandModel` sono diventati obsoleti. I modelli sono ora disponibili con i nomi `en-GB_BroadbandModel` e `en-GB_NarrowbandModel`.

    I nomi `en-UK_{model}` obsoleti continuano a funzionare, ma il metodo `GET /v1/models` non restituisce più i nomi nell'elenco di modelli disponibili. Puoi ancora eseguire la query dei nomi direttamente con il metodo `GET /v1/models/{model_id}`.

### 1 luglio 2017
{: #July2017a}

-   L'interfaccia di personalizzazione del modello di lingua del servizio è ora generalmente disponibile (GA) per entrambe le sue lingue supportate, inglese americano e giapponese. {{site.data.keyword.IBM_notm}} non addebita alcun costo per la creazione, l'hosting o la gestione dei modelli di lingua personalizzati. Come descritto al prossimo punto, {{site.data.keyword.IBM_notm}} ora addebita ulteriori $0.03 (USD) per ogni minuto di audio per le richieste di riconoscimento che utilizzano i modelli personalizzati.
-   {{site.data.keyword.IBM_notm}} ha aggiornato i prezzi per il servizio 
    -   Eliminando il prezzo del componente aggiuntivo per l'utilizzo dei modelli a banda stretta
    -   Fornendo un prezzo a più livelli graduati per gli utenti con grandi volumi
    -   Addebitando ulteriori $0.03 (USD) per ogni minuto di audio per le richieste di riconoscimento che utilizzano i modelli di lingua personalizzati per l'inglese americano e il giapponese.

    Per ulteriori informazioni sugli aggiornamenti dei prezzi, vedi
    -   Il post di blog [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   Il servizio {{site.data.keyword.speechtotextshort}} nel [Catalogo {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/speech-to-text){: new_window}
    -   Le [Domande frequenti sui prezzi](/docs/services/speech-to-text/faq-pricing.html)
-   Non hai più bisogno di passare un oggetto dati vuoto come corpo per le seguenti richieste `POST`:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    Ad esempio, puoi ora richiamare il metodo `POST /v1/sessions` con `curl` nel seguente modo:

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    Non hai più bisogno di passare la seguente opzione `curl` con la richiesta: `--data "{}"`.

    Se stai riscontrando dei problemi con una di queste richieste `POST`, prova a passare un oggetto dati vuoto con il corpo della richiesta. Il passaggio di un oggetto vuoto non modifica la natura o il significato della richiesta in alcun modo.
    {: note}

### 22 maggio 2017
{: #May2017}

Le seguenti deprecazioni annunciate per la prima volta a marzo 2017 sono ora in vigore:

-   Il parametro `continuous` è stato rimosso da tutti i metodi che avviano le richieste di riconoscimento. Il servizio ora trascrive un intero flusso audio finché non termina o va in timeout, a seconda di cosa si verifica prima. Questo comportamento è equivalente all'impostazione del precedente parametro `continuous` su `true`. Per impostazione predefinita, il servizio precedentemente arrestava la trascrizione al primo mezzo secondo senza alcun discorso (normalmente del silenzio) se il parametro veniva omesso o impostato su `false`.

    Le applicazioni esistenti che impostano il parametro su `true` non vedranno alcuna modifica nel comportamento. Le applicazioni che impostano il parametro su `false` o che si basano sul comportamento predefinito verosimilmente vedranno una modifica. Se una richiesta specifica il parametro, il servizio ora risponde restituendo un messaggio di avvertenza per il parametro sconosciuto:

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    La richiesta ha esito positivo nonostante l'avvertenza e una connessione WebSocket o una sessione esistente non ne vengono influenzate.

    {{site.data.keyword.IBM_notm}} ha rimosso il parametro per rispondere all'enorme quantità di feedback dalla community di sviluppatori che specificando `continuous=false` viene aggiunto poco valore e si potrebbe ridurre l'accuratezza della trascrizione generale.
-   Non è più possibile evitare un timeout di sessione senza inviare dell'audio:
    -   Quando utilizzi l'interfaccia WebSocket, il client non può più mantenere una connessione attiva inviando un messaggio di testo JSON con il parametro `action` impostato su `no-op`. L'invio del messaggio `no-op` non genera un errore, ma non ha alcun effetto.
    -   Quando utilizzi le sessioni con l'interfaccia HTTP, il client non può più estendere la sessione inviando una richiesta `GET /v1/sessions/{session_id}/recognize`. Il metodo restituisce ancora lo stato di una sessione attiva, ma non mantiene la sessione attiva.
    Puoi ora effettuare le seguenti operazioni per mantenere una sessione attiva:
    -   Imposta il parametro `inactivity_timeout` su `-1` per evitare il timeout di inattività di 30 secondi.
    -   Invia tutti i dati audio, incluso il silenzio, al servizio per evitare il timeout della sessione di 30 secondi. Ti viene effettuato un addebito per la durata di tutti i dati che invii al servizio, incluso il silenzio che invii per estendere la sessione.

    Per ulteriori informazioni, vedi [Timeout](/docs/services/speech-to-text/input.html#timeouts).Idealmente, vuoi stabilire una sessione appena prima di ottenere l'audio per la trascrizione e mantenere la sessione attiva inviando l'audio a una velocità quanto più possibile in tempo reale. Inoltre, assicurati che la tua applicazione ripristini normalmente le sessioni o le connessioni chiuse. 

    {{site.data.keyword.IBM_notm}} ha rimosso questa funzionalità per garantire di continuare ad offrire a tutti gli utenti un servizio di riconoscimento vocale a bassa latenza e all'avanguardia.

### 10 aprile 2017
{: #April2017}

-   Il servizio ora supporta la funzione di etichette del parlante per i seguenti modelli a banda larga:
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    Per ulteriori informazioni, vedi [Etichette del parlante](/docs/services/speech-to-text/output.html#speaker_labels).
-   Ora il servizio supporta il formato audio Web Media (WebM) con il codec Opus o Vorbis. Al momento il servizio supporta anche il formato audio Ogg con il codec Vorbis in aggiunta al codec Opus. Per ulteriori informazioni sui formati audio supportati, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   Ora il servizio supporta CORS (Cross-Origin Resource Sharing) per consentire ai client basati sul browser di richiamare direttamente il servizio. Per ulteriori informazioni, vedi [Supporto CORS](/docs/services/speech-to-text/developer-overview.html#cors).
-   L'interfaccia HTTP asincrona ora offre un metodo `POST /v1/unregister_callback` che rimuove la registrazione per un URL di callback inserito in whitelist. Per ulteriori informazioni, vedi [Annullamento della registrazione di un URL di callback](/docs/services/speech-to-text/async.html#unregister).
-   L'interfaccia WebSocket non va più in timeout per le richieste di riconoscimento per file audio particolarmente lunghi. Non devi più richiedere dei risultati provvisori con il messaggio `start` JSON per evitare il timeout. (Questo problema è stato descritto nelle note sulla release del 10 marzo 2016.)
-   Il metodo `DELETE /v1/customizations/{customization_id}` ora restituisce il codice di risposta HTTP 401 se tenti di eliminare un modello personalizzato non esistente.
-   Il metodo `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` ora restituisce il codice di risposta HTTP 400 se tenti di eliminare un corpus non esistente.

### 8 marzo 2017
{: #March2017}

L'interfaccia HTTP asincrona è ora generalmente disponibile. Prima di questa data, era una funzionalità beta.

### 1 dicembre 2016
{: #December2016}

-   Il servizio ora offre una funzione di etichette del parlante beta per l'audio a banda stretta per inglese americano, spagnolo o giapponese. La funzione identifica quali parole sono state pronunciate da quale parlante in uno scambio tra più persone. I metodi di riconoscimento senza sessione, basati sulla sessione, asincroni e WebSocket includono tutti un parametro `speaker_labels` che accetta un valore booleano per indicare se le etichette del parlante devono essere incluse nella risposta. Per ulteriori informazioni sulla funzione, vedi [Etichette del parlante](/docs/services/speech-to-text/output.html#speaker_labels).
-   L'interfaccia di personalizzazione del modello di lingua beta è ora supportata per il giapponese in aggiunta all'inglese americano. Tutti i metodi dell'interfaccia supportano il giapponese. Per ulteriori informazioni, fai riferimento alle seguenti sezioni:
    -   Per ulteriori informazioni, vedi [Creazione di un modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html) e [Utilizzo di un modello di lingua personalizzato](/docs/services/speech-to-text/language-use.html).
    -   Per considerazioni generali e specifiche per il giapponese sull'aggiunta di un file di testo di corpus, vedi [Preparazione di un file di corpus](/docs/services/speech-to-text/language-resource.html#prepareCorpus) e [Cosa succede quando aggiungi un file di corpus?](/docs/services/speech-to-text/language-resource.html#parseCorpus)
    -   Per considerazioni specifiche per il giapponese quando specifichi il campo `sounds_like` per una parola personalizzata, vedi [Linee guida per il giapponese](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP).
    -   Per ulteriori informazioni su tutti i metodi dell'interfaccia di personalizzazione, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   L'interfaccia di personalizzazione del modello di lingua ora include un metodo `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` che elenca le informazioni su un corpus specificato. Il metodo è utile per il monitoraggio dello stato di una richiesta per aggiungere un corpus a un modello personalizzato. Per ulteriori informazioni, vedi [Elenco dei corpora per un modello di lingua personalizzato](/docs/services/speech-to-text/language-corpora.html#listCorpora).
-   L'output JSON che viene restituito dai metodi `GET /v1/customizations/{customization_id}/words` e `GET /v1/customizations/{customization_id}/words/{word_name}` ora include un campo `count` per ogni parola. Il campo indica il numero di volte in cui viene trovata la parola tra tutti i corpora. Se aggiungi una parola personalizzata a un modello prima che venga aggiunta da eventuali corpora, il conteggio inizia da `1`. Se la parola viene prima aggiunta da un corpus e viene poi successivamente modificata, il conteggio riflette solo il numero di volte per cui viene trovata nei corpora. Per ulteriori informazioni, vedi [Elenco delle parole da un modello di lingua personalizzato](/docs/services/speech-to-text/language-words.html#listWords).

    Per i modelli personalizzati che sono stati creati prima dell'esistenza del campo `count`, esso rimane sempre a `0`. Per aggiornare il campo per tali modelli, aggiungi nuovamente i corpora del modello in modo che includano il parametro `allow_overwrite` con il metodo `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`.
-   Il metodo `GET /v1/customizations/{customization_id}/words` ora include un parametro di query `sort` che controlla l'ordine in cui le parole devono essere elencate. Il parametro accetta due argomenti, `alphabetical` o `count`, per indicare come le parole devono essere ordinate. Puoi anteporre un `+` o un `-` facoltativo a un argomento per indicare se i risultati devono essere ordinati in ordine crescente o decrescente. Per impostazione predefinita, il metodo visualizza le parole in ordine alfabetico crescente. Per ulteriori informazioni, vedi [Elenco delle parole da un modello di lingua personalizzato](/docs/services/speech-to-text/language-words.html#listWords).

    Per i modelli personalizzati creati prima dell'introduzione del campo `count`, l'utilizzo dell'argomento `count` con il parametro `sort` non ha senso. Utilizza l'argomento `alphabetical` predefinito con tali modelli.
-   Il campo `error` che può essere restituito come parte della risposta JSON dai metodi `GET /v1/customizations/{customization_id}/words` e `GET /v1/customizations/{customization_id}/words/{word_name}` è ora un array. Se il servizio ha rilevato uno o più problemi con la definizione di una parola personalizzata, il campo elenca ogni elemento del problema dalla definizione e fornisce un messaggio che descrive il problema. Per ulteriori informazioni, vedi [Elenco delle parole da un modello di lingua personalizzato](/docs/services/speech-to-text/language-words.html#listWords).
-   I parametri `keywords_threshold` e `word_alternatives_threshold` dei metodi di riconoscimento non accettano più un valore null. Per omettere delle parole chiave o delle alternative alle parole dalla risposta, ometti i parametri. Un valore specificato deve essere un valore mobile.

Per ulteriori informazioni sull'interfaccia del servizio, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 22 settembre 2016
{: #September2016}

-   Il servizio ora offre una nuova interfaccia di personalizzazione del modello di lingua beta per l'inglese americano. Puoi utilizzare l'interfaccia per personalizzare i modelli di lingua e vocabolario di base del servizio tramite la creazione di modelli di lingua personalizzati che includono la terminologia specifica per il dominio. Puoi aggiungere le parole personalizzate individualmente o lasciare che il servizio le estragga dai corpora. Per utilizzare i tuoi modelli personalizzati con i metodi di riconoscimento vocale offerti da tutte le interfacce del servizio, passa il parametro di query `customization_id`. Per ulteriori informazioni, vedi
    -   [Creazione di un modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html)
    -   [Utilizzo di un modello di lingua personalizzato](/docs/services/speech-to-text/language-use.html)
    -   [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}
-   L'elenco di formati audio supportati ora include `audio/mulaw`, che fornisce un audio a canale singolo codificato utilizzando l'algoritmo di dati u-law (o mu-law). Quando utilizzi questo formato, devi specificare anche la frequenza di campionamento con cui viene acquisito l'audio. Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   I metodi `GET /v1/models` e `GET /v1/models/{model_id}` ora restituiscono un campo `supported_features` come parte del proprio output per ogni modello di lingua. Le informazioni aggiuntive descrivono se il modello supporta la personalizzazione. Per ulteriori informazioni, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 30 giugno 2016
{: #June2016b}

L'interfaccia HTTP asincrona beta ora supporta tutte le lingue supportate dal servizio. L'interfaccia era precedentemente disponibile solo per l'inglese americano. Per ulteriori informazioni, vedi [L'interfaccia HTTP asincrona](/docs/services/speech-to-text/async.html) e la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 23 giugno 2016
{: #June2016a}

-   Un'interfaccia HTTP asincrona beta è ora disponibile. L'interfaccia fornisce tutte le funzionalità di riconoscimento per la trascrizione in inglese americano tramite chiamate HTTP non bloccanti. Puoi registrare gli URL di callback e fornire le stringhe segrete specificate dall'utente per fornire l'integrità dei dati e l'autenticazione con le firme digitali. Per ulteriori informazioni, vedi [L'interfaccia HTTP asincrona](/docs/services/speech-to-text/async.html) e la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Una funzionalità di formattazione intelligente beta che converte date, ore, serie di cifre e numeri, numeri di telefono, valori valutari e indirizzi internet in rappresentazioni più convenzionali nelle trascrizioni finali. Abilita la funzione impostando il parametro `smart_formatting` su `true` in una richiesta di riconoscimento. La funzione è una funzionalità beta disponibile solo per l'inglese americano. Per ulteriori informazioni, vedi [Formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting).
-   L'elenco di modelli supportati per il riconoscimento vocale ora include `fr-FR_BroadbandModel` per l'audio nella lingua francese campionato a un minimo di 16 kHz. Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html). 
-   L'elenco di formati audio supportati ora include `audio/basic`. Il formato fornisce un audio a canale singolo codificato utilizzando i dati u-law (o mu-law) a 8-bit campionati a 8 hKz. Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   I vari metodi di riconoscimento possono restituire una risposta `warnings` che include i messaggi sui parametri di query o i campi JSON non validi che vengono inclusi in una richiesta. Il formato delle avvertenze è stato modificato. Ad esempio, `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` è ora `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`
-   Per le richieste `POST` HTTP che non riescono altrimenti a passare i dati al servizio, devi includere un corpo della richiesta vuoto del formato `{}`. Con il comando `curl`, utilizza l'opzione `--data` per passare i dati vuoti.

### 10 marzo 2016
{: #March2016}

-   Entrambe le forme di trasmissione dei dati (fornitura unica e invio in streaming) ora impongono un limite di 100 MB sui dati audio, come per l'interfaccia WebSocket. Precedentemente, l'approccio di fornitura unica aveva un limite massimo di 4 MB di dati. Per ulteriori informazioni, vedi [Trasmissione audio](/docs/services/speech-to-text/input.html#transmission) (per tutte le interfacce) e [Invia l'audio e ricevi i risultati del riconoscimento](/docs/services/speech-to-text/websockets.html#WSaudio) (per l'interfaccia WebSocket). La sezione WebSocket parla anche della velocità massima o della dimensione dei messaggi di 4 MB imposta dall'interfaccia WebSocket.
-   La risposta JSON per una richiesta di riconoscimento può ora includere un array di messaggi di avvertenza per parametri di query o campi JSON non validi inclusi con una richiesta. Ogni elemento dell'array è una stringa che descrive la natura dell'avvertenza seguita da un array di stringhe di argomenti non valide. Ad esempio, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`. Per ulteriori informazioni, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   La versione beta di *{{site.data.keyword.watson}} Speech Software Development Kit (SDK) per il sistema operativo iOS di Apple&reg;* è obsoleta. Utilizza invece l'SDK *{{site.data.keyword.watson}} per il sistema operativo iOS di Apple&reg;*. Il nuovo SDK è disponibile dal [repository ios-sdk ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} nello spazio dei nomi `watson-developer-cloud` su GitHub.

L'interfaccia WebSocket al momento presenta i seguenti problemi noti:

-   Il servizio può impiegare diversi minuti per produrre i risultati finali per una richiesta di riconoscimento per un file audio particolarmente lungo. Per l'interfaccia WebSocket, la connessione TCP sottostante rimane inattiva mentre il servizio prepara la risposta. Pertanto, la connessione può venire chiusa a causa del timeout. Per evitare il timeout con l'interfaccia WebSocket, richiedi i risultati provvisori (`\"interim_results\": \"true\"`) nel JSON per il messaggio `start` per avviare la richiesta. Puoi eliminare i risultati provvisori se non ne hai bisogno. Questo problema sarà risolto in un aggiornamento futuro.

### 19 gennaio 2016
{: #January2016}

Il servizio è stato aggiornato per includere una nuova funzione di filtro di contenuto volgare il 19 gennaio 2016. Per impostazione predefinita, il servizio censura il contenuto volgare dai propri risultati di trascrizione per l'audio in inglese americano. Per ulteriori informazioni, vedi [Filtro di contenuto volgare](/docs/services/speech-to-text/output.html#profanity_filter).

### 17 dicembre 2015
{: #December2015}

-   Il servizio ora offre una funzione di individuazione di parole chiave. Puoi specificare un array di stringhe di parole chiave che devono essere soddisfatte nell'audio di input. Devi inoltre specificare un livello di attendibilità definito dall'utente che una parola deve soddisfare per essere considerata una corrispondenza per una parola chiave. Per ulteriori informazioni, vedi [Individuazione di parole chiave](/docs/services/speech-to-text/output.html#keyword_spotting).

    La funzione di individuazione di parole chiave è una funzionalità beta.
    {: note}
-   Il servizio ora offre una funzione di alternative alle parole. La funzione restituisce delle ipotesi alternative alle parole nell'audio di input che soddisfano un livello di attendibilità definito dall'utente. Per ulteriori informazioni, vedi [Alternative alle parole](/docs/services/speech-to-text/output.html#word_alternatives).

    La funzione di alternative alle parole è una funzionalità beta.
    {: note}
-   Il servizio supporta più lingue con i propri modelli di trascrizione: `en-UK_BroadbandModel` e `en-UK_NarrowbandModel` per l'inglese britannico e `ar-AR_BroadbandModel` per l'arabo standard moderno. Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html). 
-   Le richieste di riconoscimento HTTP non sono più soggette a un timeout della piattaforma di 10 minuti. Il servizio ora mantiene la connessione attiva inviando un carattere spazio nell'oggetto JSON di risposta ogni 20 secondi finché il riconoscimento è in corso. Per ulteriori informazioni, vedi [Timeout](/docs/services/speech-to-text/input.html#timeouts).
-   Il servizio non restituisce più un codice di stato HTTP 490 per i metodi HTTP basati sulla sessione `GET /v1/sessions/{session_id}/observe_result` e `POST /v1/sessions/{session_id}/recognize`. Il servizio ora risponde invece con il codice di stato HTTP 400.

    Nelle risposte JSON che restituiscono degli errori con i metodi basati sulla sessione, il servizio ora include anche un nuovo campo `session_closed`. Il campo è impostato su `true` se la sessione viene chiusa a causa dell'errore. Per ulteriori informazioni sui codici di ritorno possibili per ogni metodo, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Quando utilizzi il comando `curl` per trascrivere dell'audio con il servizio, non devi più utilizzare l'opzione `--limit-rate` per trasferire i dati a una velocità inferiore a 40.000 byte al secondo.

### 21 settembre 2015
{: #September2015}

-   Sono disponibili due nuovi SDK mobili beta per i servizi vocali. Gli SDK consentono alle applicazioni mobili di interagire con i servizi {{site.data.keyword.speechtotextshort}} e {{site.data.keyword.texttospeechshort}}.
    -   L'SDK *{{site.data.keyword.watson}} Speech per la piattaforma Google Android&trade;* supporta lo streaming dell'audio al servizio {{site.data.keyword.speechtotextshort}} in tempo reale e la ricezione di una trascrizione dell'audio quando parli. Il progetto include un'applicazione di esempio che illustra l'interazione con entrambi i servizi vocali. L'SDK è disponibile dal [repository speech-android-sdk ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window} nello spazio dei nomi `watson-developer-cloud` su GitHub.
    -   L'SDK *{{site.data.keyword.watson}} Speech per il sistema operativo Apple&reg; iOS* supporta lo streaming dell'audio al servizio {{site.data.keyword.speechtotextshort}} e la ricezione di una trascrizione dell'audio in risposta. L'SDK è disponibile dal [repository speech-ios-sdk ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window} nello spazio dei nomi `watson-developer-cloud` su GitHub.

    Entrambi gli SDK supportano l'autenticazione con i servizi vocali utilizzando le credenziali del servizio {{site.data.keyword.cloud_notm}} o un token di autenticazione.

    Poiché gli SDK sono nella beta, sono soggetti a modifiche future.
    {: note}
-   Il servizio supporta due nuove lingue, portoghese brasiliano e cinese mandarino. I modelli per queste nuove lingue sono `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel` e `zh-CN_NarrowbandModel`. Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html). 
-   Le richieste `POST` HTTP `/v1/sessions/{session_id}/recognize` e `/v1/recognize`, nonché la richiesta `/v1/recognize` WebSocket, supportano la trascrizione di un nuovo tipo di supporto: `audio/ogg;codecs=opus` per i file di formato Ogg che utilizzano il codec Opus. Inoltre, il formato `audio/wav` per i metodi ora supporta qualsiasi codifica. La limitazione sull'utilizzo della codifica PCM lineare è stata rimossa. Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   Il servizio ora supporta il superamento dei timeout quando trascrivi file audio lunghi con l'interfaccia HTTP. Quando utilizzi le sessioni, puoi utilizzare un modello di polling lungo specificando gli ID di sequenza con i metodi `GET /v1/sessions/{session_id}/observe_result` e `POST /v1/sessions/{session_id}/recognize` per attività di riconoscimento a lunga esecuzione. Utilizzando il nuovo parametro `sequence_id` di questi metodi, puoi richiedere i risultati prima, durante o dopo aver inviato una richiesta di riconoscimento.
-   Per i modelli di lingua per l'inglese americano, `en_US_BroadbandModel` e `en_US_NarrowbandModel`, il servizio ora converte in maiuscolo molti nomi propri. Ad esempio, il servizio restituirà del nuovo testo che legge "Barack Obama graduated from Columbia University" invece di "barack obama graduated from columbia university." Questa modifica potrebbe essere interessante se la tua applicazione è sensibile per un qualsiasi motivo al maiuscolo/minuscolo dei nomi propri.
-   La richiesta `DELETE /v1/sessions/{session_id}` HTTP non restituisce il codice di stato 415 "Unsupported Media Type." Questo codice di ritorno è stato rimosso dalla documentazione per il metodo.

### 1 luglio 2015
{: #July2015}

Il servizio è passato dalla beta alla disponibilità generale (GA) il primo luglio 2015. Esistono le seguenti differenze tra le versioni beta e GA delle API {{site.data.keyword.speechtotextshort}}. La release GA richiede che gli utenti eseguano l'upgrade alla nuova versione del servizio.

-   La versione GA dell'API HTTP è compatibile con la versione beta. Devi modificare il tuo codice applicazione esistente solo se hai specificato esplicitamente un nome di modello. Ad esempio, il codice di esempio disponibile per il servizio da GitHub includeva la seguente riga di codice nel file `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    Questa riga specificava il modello predefinito, `WatsonModel`, per la versione beta del servizio. Se anche la tua applicazione specificava questo modello, devi modificarla in modo che utilizzi uno dei nuovi modello supportati dalla versione GA. Per ulteriori informazioni, vedi il prossimo punto.
-   Il servizio ora supporta un nuovo modello di programmazione per l'interazione diretta tra un client e il servizio su una connessione WebSocket. Utilizzando questo modello, un client può ottenere un token di autenticazione per comunicare direttamente con il servizio. Il token elude la necessità di un'applicazione proxy lato server in {{site.data.keyword.cloud_notm}} di richiamare il servizio al posto del client. I token sono i mezzi preferiti per i client per interagire con il servizio.

    Il servizio continua a supportare il vecchio modello di programmazione basato su un proxy lato server per trasmettere l'audio e i messaggi tra il client e il servizio. Tuttavia, il nuovo modello è più efficiente e offre una velocità effettiva più elevata. Per ulteriori informazioni sul nuovo modello di programmazione, vedi [Modelli di programmazione per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).
-   I metodi `POST /v1/sessions` e `POST /v1/recognize`, insieme al metodo `/v1/recognize` WebSocket, ora supportano un parametro di query `model`. Utilizza il parametro per specificare le informazioni sull'audio:

    -   La lingua: *inglese*, *giapponese* o *spagnolo*
    -   La frequenza di campionamento minima: *banda larga* (16 kHz) o *banda stretta* (8 kHz)

    Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html).
-   L'intestazione `Content-Type` dei metodi `recognize` ora supporta `audio/wav` per i file WAV (Waveform Audio File Format), in aggiunta a `audio/flac` e `audio/l16`. Per ulteriori informazioni sui formati audio supportati, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).
-   I metodi `recognize` ora supportano diversi nuovi parametri di query che puoi utilizzare per personalizzare il servizio in modo che soddisfi i bisogni della tua applicazione:
    -   Il parametro `inactivity_timeout` imposta il valore di timeout in secondi dopo il quale il servizio chiude la connessione se rileva del silenzio (nessun discorso) nella modalità di streaming. Per impostazione predefinita, il servizio termina la sessione dopo 30 secondi di silenzio. Vedi [Timeout](/docs/services/speech-to-text/input.html#timeouts).
    -   Il parametro `max_alternatives` indica al servizio di restituire le migliori *n* ipotesi alternative per la trascrizione audio. Vedi [Numero massimo di alternative](/docs/services/speech-to-text/output.html#max_alternatives).
    -   Il parametro `word_confidence` indica al servizio di restituire un punteggio di attendibilità per ogni parola della trascrizione. Vedi [Attendibilità delle parole](/docs/services/speech-to-text/output.html#word_confidence).
    -   Il parametro `timestamps` indica al servizio di restituire l'ora di inizio e di fine per ciascuna parola relativamente all'inizio dell'audio. Vedi [Date/ore delle parole](/docs/services/speech-to-text/output.html#word_timestamps).
-   Il metodo `GET /v1/sessions/{session_id}/observeResult` è ora denominato `GET /v1/sessions/{session_id}/observe_result`. Il nome `observeResult` è ancora supportato per la retrocompatibilità.
-   I metodi `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize` e `POST /v1/recognize` ora includono il parametro di intestazione `X-WDC-PL-OPT-OUT` per controllare se il servizio utilizza i dati di trascrizione e audio da una richiesta per migliorare i risultati futuri. L'interfaccia WebSocket include un parametro di query equivalente. Specifica un valore di `1` per evitare che il servizio utilizzi i risultati di trascrizione e audio. Il parametro si applica solo alla richiesta corrente. La nuova intestazione sostituisce l'intestazione `X-logging` dall'API beta. Vedi [Controllo della registrazione delle richieste per i servizi {{site.data.keyword.watson}}](../common/getting-started-logging.html).
-   Il servizio ora ha un limite di 100 MB di dati per sessione nella modalità di streaming. Specifica la modalità di streaming mediante il valore `chunked` con l'intestazione `Transfer-Encoding`. La fornitura unica di un file audio impone ancora un limite di dimensione di 4 MB sui dati che vengono inviati. Vedi [Trasmissione audio](/docs/services/speech-to-text/input.html#transmission).
-   Per i metodi `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize` e `/v1/recognize`, è stato aggiunto il codice di errore 415 ("Unsupported Media Type").
-   Per le richieste `POST` e `GET` al metodo `/v1/sessions/{session_id}/recognize`, i seguenti codici di errore sono stati modificati:
    -   Il codice di errore 404 ("Session_id not found") ha un messaggio più descrittivo (`POST` e `GET`).
    -   Il codice di errore 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") ha un messaggio più descrittivo (solo `POST`).
-   Per le richieste `POST` HTTP ai metodi `/v1/sessions` e `/v1/recognize`, può essere restituito il codice di errore 503 ("Service Unavailable"). Il codice di errore può essere restituito anche durante la creazione di una connessione WebSocket con il metodo `/v1/recognize`.
