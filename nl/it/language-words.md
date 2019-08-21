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

# Gestione delle parole personalizzate
{: #manageWords}

L'interfaccia di personalizzazione include i metodi `POST /v1/customizations/{customization_id}/words` e `PUT /v1/customizations/{customization_id}/words/{word_name}`, che vengono utilizzati per aggiungere o modificare le parole per un modello personalizzato. Per ulteriori informazioni, vedi [Aggiungi parole al modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addWords). L'interfaccia include anche i seguenti metodi per elencare ed eliminare le parole per un modello di lingua personalizzato.
{: shortdesc}

È probabile che aggiungi la maggior parte delle parole personalizzate dai corpora. Assicurati di conoscere la codifica di caratteri utilizzata nei file di testo per i tuoi corpora. Il servizio preserva la codifica che trova nei file di testo. Devi utilizzare questa codifica quando utilizzi le singole parole nel modello di lingua personalizzato. Quando specifichi una parola con il metodo `GET`, `PUT` o `DELETE /v1/customizations/{customization_id}/words/{word_name}`, devi codificare in URL il `word_name` che passi nell'URL se la parola include caratteri non ASCII. Per ulteriori informazioni, vedi [Codifica dei caratteri](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#charEncoding).
{: important}

## Elenco delle parole da un modello di lingua personalizzato
{: #listWords}

L'interfaccia di personalizzazione offre due metodi per elencare le parole da un modello di lingua personalizzato:

-   Il metodo `GET /v1/customizations/{customization_id}/words` elenca le informazioni sulle parole dalla risorsa di parole del modello personalizzato. Il metodo include due parametri di query facoltativi:
    -   Il parametro `word_type` specifica quali parole devono essere elencate:

        -   `all` (il valore predefinito) mostra tutte le parole.
        -   `user` mostra solo parole personalizzate che erano state aggiunte o modificate dall'utente.
        -   `corpora` mostra solo le parole OOV che erano state estratte dai corpora.
        -   `grammars` mostra solo le parole OOV che erano state estratte dalle grammatiche.
    -   Il parametro `sort` indica in che modo le parole devono essere ordinate. Il parametro accetta due argomenti per indicare come devono essere ordinate le parole: `alphabetical` e `count`. Puoi aggiungere un `+` o `-` facoltativo davanti a un argomento per indicare se i risultati devono essere ordinati in modo crescente o decrescente. Per impostazione predefinita, il metodo visualizza le parole in ordine alfabetico crescente.
-   Il metodo `GET /v1/customizations/{customization_id}/words/{word_name}` elenca le informazioni su una singola parola specificata dalla risorsa di parole del modello.

Oltre a un campo `word` che identifica la parola, entrambi i metodi restituiscono le seguenti informazioni su ciascuna parola:

-   Un campo `sounds_like` che presenta un array di pronunce per la parola. L'array può includere la pronuncia dal suono simile (sounds-like) che viene generata automaticamente dal servizio se non viene fornito alcun valore di suono simile (sounds-like) per la parola. Per ulteriori informazioni, vedi [Utilizzo del campo sounds_like](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#soundsLike).
-   Un campo `display_as` che mostra l'ortografia della parola personalizzata che il servizio visualizza nelle trascrizioni. Il campo contiene una stringa vuota se non viene fornito alcun valore di modalità di visualizzazione (display-as) per la parola, nel qual caso la parola viene visualizzata così come è scritta. Per ulteriori informazioni, vedi [Utilizzo del campo display_as](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#displayAs).
-   Un campo `source` che indica in che modo la parola è stata aggiunta alla risorsa di parole del modello personalizzato. Il campo include il nome di ogni corpus e grammatica da cui il servizio ha estratto la parola. Se hai modificato o aggiunto la parola direttamente, il campo include la stringa `user`.
-   Un campo `count` che indica il numero di volte per cui la parola viene rilevata in tutti i corpora e in tutte le grammatiche. Ad esempio, se la parola ricorre cinque volte in un corpus e sette volte in un altro, il suo conteggio è `12`.

    Se aggiungi una parola personalizzata a un modello prima che venga aggiunta da eventuali corpora o grammatiche, il conteggio inizia da `1`. Se la parola viene prima aggiunta da un corpus o una grammatica e viene successivamente modificata, il conteggio riflette solo il numero di volte per cui viene rilevata in corpora e grammatiche.

    Per i modelli personalizzati che erano stati creati prima dell'introduzione del campo `count`, il campo rimane sempre a `0`. Per aggiornare il campo per tali modelli, aggiungi nuovamente corpora e grammatiche del modello e includi il parametro `allow_overwrite` con la richiesta.
    {: note}

Se il servizio rileva uno o più problemi con la definizione della parola personalizzata, l'output include un campo `error`. Il campo fornisce un array che elenca ciascun elemento del problema dalla definizione e un messaggio che descrive il problema.

Un errore si può verificare, ad esempio, se aggiungi una parola personalizzata con un campo `sounds_like` non valido, uno che non rispetta una delle regole per l'aggiunta di una pronuncia. Non puoi addestrare un modello personalizzato la cui risorsa di parole include una parola con un errore. Devi correggere o eliminare la parola prima di poter addestrare il modello.

### Richieste e risposte di esempio
{: #listExample-words}

Il seguente esempio elenca tutte le parole, indipendentemente dal tipo, dal modello personalizzato con l'ID di personalizzazione specificato. Le parole vengono visualizzate nell'ordine predefinito, alfabetico crescente.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

La risorsa di parole per il modello contiene quattro parole. La prima parola era stata aggiunta direttamente dall'utente ma il suo campo `sounds_like` contiene un errore. Il campo non può contenere numeri. Le altre parole erano state aggiunte dall'utente o sia dall'utente che dai corpora.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

Il seguente esempio mostra le informazioni sulla parola `NCAA` dalla risorsa di parole del modello specificato:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

L'utente ha inizialmente aggiunto la parola. Il servizio ha quindi rilevato la parola due volte in `corpus3`.

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## Eliminazione di una parola da un modello di lingua personalizzato
{: #deleteWord}

Utilizza il metodo `DELETE /v1/customizations/{customization_id}/words/{word_name}` per eliminare una parola da un modello di lingua personalizzato. Utilizza il metodo per rimuovere le parole che erano state aggiunte erroneamente, ad esempio da un corpus con dati errati.

Puoi rimuovere qualsiasi parola che avevi aggiunto, con qualsiasi mezzo, alla risorsa di parole del modello personalizzato. Tuttavia, non puoi eliminare una parola dal vocabolario di base del servizio. L'eliminazione di una parola da un modello personalizzato elimina solo la pronuncia personalizzata per la parola; la parola rimane nel vocabolario di base.

La rimozione di una parola da un modello personalizzato non influisce sul modello finché non ne riesegui l'addestramento utilizzando il metodo `POST /v1/customizations/{customization_id}/train`. Se il modello era stato precedentemente addestrato sulla parola, il modello continua ad applicare la parola al riconoscimento vocale anche dopo che avrai eliminato la parola dalla sua risorsa di parole. Devi rieseguire l'addestramento del modello per riflettere l'eliminazione.

### Richiesta di esempio
{: #deleteExample-word}

Il seguente esempio elimina la parola `IEEE` dal modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
