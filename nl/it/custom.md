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

# L'interfaccia di personalizzazione
{: #customization}

Il servizio {{site.data.keyword.speechtotextfull}} offre un'interfaccia di personalizzazione che puoi utilizzare per aumentare le sue funzionalità di riconoscimento vocale. Puoi utilizzare la personalizzazione per migliorare l'accuratezza delle richieste di riconoscimento vocale personalizzando un modello di base per il tuo dominio e il tuo audio. La personalizzazione è disponibile solo per alcune lingue e a diversi livelli di supporto per le diverse lingue; vedi [Supporto delle lingue per la personalizzazione](#languageSupport).
{: shortdesc}

L'interfaccia di personalizzazione supporta sia i modelli di lingua personalizzati che i modelli acustici personalizzati. Le interfacce per entrambi i tipi di modello personalizzato sono simili e semplici da utilizzare. Anche l'uso di entrambi i tipi di modello personalizzato con una richiesta di riconoscimento è semplice: specifichi l'ID di personalizzazione del modello con la richiesta.

Il riconoscimento vocale funziona allo stesso modo con o senza un modello personalizzato. Quando utilizzi un modello personalizzato per il riconoscimento vocale, puoi utilizzare tutti i parametri di input e output normalmente disponibili con una richiesta di riconoscimento. Per ulteriori informazioni su tutti i parametri disponibili, vedi [Riepilogo dei parametri](/docs/services/speech-to-text/summary.html).

## Personalizzazione del modello di lingua
{: #customLanguage-intro}

Il servizio è stato sviluppato tenendo conto di un vasto pubblico generale. Il vocabolario di base del servizio contiene molte parole usate nelle conversazioni di tutti i giorni. I suoi modelli forniscono un riconoscimento sufficientemente accurato per molte applicazioni. Ma possono avere una scarsa conoscenza di termini specifici associati a particolari domini.

L'interfaccia di *personalizzazione del modello di lingua* può migliorare l'accuratezza del riconoscimento vocale per domini come medicina, legge, tecnologie dell'informazione (IT) e altri. Utilizzando la personalizzazione del modello di lingua, puoi espandere e adattare il vocabolario di un modello di base per includere la terminologia specifica per il dominio.

Crea un modello di lingua personalizzato e aggiungi corpora e parole specifici per il tuo dominio. Una volta addestrato il modello di lingua personalizzato sul tuo vocabolario migliorato, puoi utilizzarlo per il riconoscimento vocale personalizzato. In genere, il servizio può addestrare qualsiasi modello personalizzato in pochi minuti. Il livello di impegno necessario per creare un modello dipende dai dati che hai a disposizione per il modello.

Per ulteriori informazioni, vedi

-   [Creazione di un modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html)
-   [Utilizzo di un modello di lingua personalizzato](/docs/services/speech-to-text/language-use.html)

## Personalizzazione del modello acustico
{: #customAcoustic-intro}

In modo analogo, il servizio è stato sviluppato con modelli acustici di base che funzionano correttamente per varie caratteristiche audio. Tuttavia, in casi come quelli che seguono, l'adattamento di un modello di base per adeguarlo al tuo audio può migliorare il riconoscimento vocale:

-   Il tuo ambiente del canale acustico è univoco. Ad esempio, l'ambiente è rumoroso, la qualità o il posizionamento del microfono non sono ottimali oppure l'audio risente di effetti di campo lontano.
-   I discorsi dei parlanti sono atipici. Ad esempio, un parlante parla in modo eccessivamente veloce o l'audio include conversazioni casuali.
-   Gli accenti dei parlanti sono pronunciati. Ad esempio, l'audio include parlanti che parlano in una lingua non nativa o seconda lingua.

L'interfaccia di *personalizzazione del modello acustico* può adattare un modello di base al tuo ambiente e ai tuoi parlanti. Crea un modello acustico personalizzato e aggiungi dati audio (risorse audio) che corrispondano il più possibile alla firma acustica dell'audio che vuoi trascrivere. Una volta addestrato il modello acustico personalizzato con le tue risorse audio, puoi utilizzarlo per il riconoscimento vocale personalizzato.

Il tempo che impiega il servizio per addestrare il modello personalizzato dipende dalla quantità di dati audio contenuti nel modello. In generale, l'addestramento richiede il doppio della durata dell'audio cumulativo. Il livello di impegno necessario per creare un modello dipende dai dati audio che hai a disposizione per il modello. Dipende anche da se utilizzi o meno le trascrizioni dell'audio.

Per ulteriori informazioni, vedi

-   [Creazione di un modello acustico personalizzato](/docs/services/speech-to-text/acoustic-create.html)
-   [Utilizzo di un modello acustico personalizzato](/docs/services/speech-to-text/acoustic-use.html)

## Grammatiche
{: #grammars-intro}

I modelli di lingua personalizzati ti consentono di espandere il vocabolario di base del servizio.Le *grammatiche* ti consentono di limitare le parole che il servizio può riconoscere da quel vocabolario. Quando utilizzi una grammatica con un modello di lingua personalizzato per il riconoscimento vocale, il servizio può riconoscere solo le parole, frasi e stringhe che vengono riconosciute dalla grammatica. Poiché la grammatica definisce uno spazio di ricerca limitato per le corrispondenze valide, il servizio può fornire i risultati in modo più veloce e più accurato.

Aggiungi una grammatica a un modello di lingua personalizzato e addestra il modello esattamente come fai per un corpus. Tuttavia, a differenza di un corpus, devi specificare esplicitamente che una grammatica deve essere utilizzata con un modello personalizzato durante il riconoscimento vocale.

Per ulteriori informazioni, vedi

-   [Utilizzo di grammatiche con i modelli di lingua personalizzati](/docs/services/speech-to-text/grammar.html)
-   [Aggiunta di una grammatica a un modello di lingua personalizzato](/docs/services/speech-to-text/grammar-add.html)
-   [Utilizzo di una grammatica per il riconoscimento vocale](/docs/services/speech-to-text/grammar-use.html)

## Utilizzo combinato della personalizzazione acustica e della lingua
{: #combined}

L'utilizzo di un modello acustico personalizzato da solo può migliorare le funzionalità di riconoscimento del servizio. Ma se per il tuo audio di esempio sono disponibili trascrizioni o corpora correlati, puoi utilizzare questi dati per migliorare ulteriormente la qualità del riconoscimento vocale sulla base del modello acustico personalizzato.

Creando un modello di lingua personalizzato che complementa il tuo modello acustico personalizzato, puoi migliorare il riconoscimento vocale utilizzando insieme i due modelli. Quando addestri un modello acustico personalizzato, puoi specificare un modello di lingua personalizzato che include le trascrizioni delle risorse audio o un vocabolario di parole specifiche per il dominio dalle risorse. Allo stesso modo, quando trascrivi l'audio, il servizio accetta un modello di lingua personalizzato, un modello acustico personalizzato o entrambi. Inoltre, se il tuo modello di lingua personalizzato include una grammatica, puoi utilizzare quel modello e la grammatica con un modello acustico personalizzato per il riconoscimento vocale.

Per ulteriori informazioni, vedi

-   [Utilizzo combinato dei modelli acustici e di lingua personalizzati](/docs/services/speech-to-text/acoustic-both.html)

Alcune lingue non supportano sia la personalizzazione della lingua che quella acustica. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](#languageSupport).
{: note}

## Supporto delle lingue per la personalizzazione
{: #languageSupport}

La personalizzazione del modello di lingua e del modello acustico è disponibile solo per alcune lingue. La seguente tabella documenta il livello al quale il servizio supporta la personalizzazione per ciascuna lingua.

-   *GA* indica che l'interfaccia è generalmente disponibile per l'uso in produzione.
-   *Beta* indica che l'interfaccia è disponibile come offerta beta.
-   *Non supportato* significa che l'interfaccia non è disponibile per quella lingua.

Puoi utilizzare sia modelli a banda larga che a banda stretta con qualsiasi lingua supportata per la quale sono disponibili. Se una lingua supporta la personalizzazione del modello di lingua, supporta anche le grammatiche. Per un elenco di tutti i modelli disponibili, vedi [Modelli di lingua supportati](/docs/services/speech-to-text/models.html#modelsList).

<table>
  <caption>Tabella 1. Supporto delle lingue per la personalizzazione</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      Lingua
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Supporto per la personalizzazione del modello di lingua
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Supporto per la personalizzazione del modello acustico
    </th>
  </tr>
  <tr>
    <td>Portoghese (Brasile)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Francese</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Tedesco</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Giapponese</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Coreano</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Cinese mandarino</td>
    <td style="text-align:center">Non supportato</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Arabo standard moderno</td>
    <td style="text-align:center">Non supportato</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Spagnolo</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Inglese (Regno Unito)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Inglese (Stati Uniti)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
</table>

Puoi utilizzare i metodi `GET /v1/models` e `GET /v1/models/{model_id}` per verificare se un modello di lingua supporta la personalizzazione del modello di lingua. Se il modello di base supporta la personalizzazione del modello di lingua, il campo `supported_features` dell'output dei metodi per il modello imposta il campo `custom_language_model` su `true`.

## Note sull'utilizzo della personalizzazione
{: #customUsage}

Le seguenti note sull'utilizzo si applicano sia alla personalizzazione del modello di lingua che alla personalizzazione del modello acustico.

### Proprietà di modelli personalizzati
{: #customOwner}

Un modello personalizzato è di proprietà dell'istanza del servizio {{site.data.keyword.speechtotextshort}} di cui vengono utilizzate le credenziali per crearlo. Per lavorare in qualsiasi modo con il modello personalizzato, devi utilizzare le credenziali create per quell'istanza del servizio con i metodi dell'interfaccia di personalizzazione. Le credenziali create per altre istanze del servizio non possono visualizzare o accedere al modello personalizzato.

Tutte le credenziali di servizio ottenute per la stessa istanza del servizio {{site.data.keyword.speechtotextshort}} condividono l'accesso a tutti i modelli personalizzati creati per tale istanza del servizio. Per limitare l'accesso a un modello personalizzato, crea un'istanza separata del servizio e utilizza solo le credenziali per tale istanza del servizio per creare e utilizzare il modello. Le credenziali per altre istanze del servizio non possono influire sul modello personalizzato.

Un vantaggio della condivisione della proprietà tra le credenziali di un'istanza del servizio è che puoi annullare un insieme di credenziali, ad esempio, se vengono compromesse. Puoi quindi creare nuove credenziali per la stessa istanza del servizio e mantenere comunque la proprietà e l'accesso ai modelli personalizzati creati con le credenziali originali.

### Registrazione delle richieste e privacy dei dati
{: #customLogging}

Il modo in cui il servizio gestisce la registrazione delle richieste per le chiamate all'interfaccia di personalizzazione dipende dalla richiesta:

-   Il servizio *non* registra i dati utilizzati per creare i modelli personalizzati. Ad esempio, quando lavori con i corpora e le parole in un modello di lingua personalizzato, non devi impostare l'intestazione della richiesta `X-Watson-Learning-Opt-Out`. I tuoi dati di addestramento non vengono mai utilizzati per migliorare i modelli di base del servizio.
-   Il servizio *registra* i dati quando viene utilizzato un modello personalizzato con una richiesta di riconoscimento. Devi impostare l'intestazione della richiesta `X-Watson-Learning-Opt-Out` su `true` per impedire la registrazione delle richieste di riconoscimento.

Per ulteriori informazioni, vedi [Registrazione delle richieste](/docs/services/speech-to-text/input.html#logging).

### Sicurezza delle informazioni
{: #customSecurity}

Puoi associare un ID cliente ai dati aggiunti o aggiornati per i modelli di lingua personalizzati e acustici personalizzati. Puoi associare un ID cliente a corpora, parole personalizzate, grammatiche e risorse audio passando l'intestazione `X-Watson-Metadata` con i seguenti metodi:

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

Se necessario, puoi quindi eliminare i dati utilizzando il metodo `DELETE /v1/user_data`. Per ulteriori informazioni, vedi [Sicurezza delle informazioni](/docs/services/speech-to-text/information-security.html).
