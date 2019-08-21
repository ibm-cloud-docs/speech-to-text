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

# Gestione dei corpora
{: #manageCorpora}

L'interfaccia di personalizzazione include il metodo `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` per aggiungere un corpus a un modello di lingua personalizzato. Per ulteriori informazioni, vedi [Aggiungi un corpus al modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus). L'interfaccia include anche i seguenti metodi per elencare ed eliminare i corpora per un modello di lingua personalizzato.
{: shortdesc}

## Elenco dei corpora per un modello di lingua personalizzato
{: #listCorpora}

L'interfaccia di personalizzazione fornisce due metodi per elencare le informazioni relative ai corpora per un modello di lingua personalizzato:

-   Il metodo `GET /v1/customizations/{customization_id}/corpora` elenca le informazioni su tutti i corpora per un modello personalizzato.
-   Il metodo `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` elenca le informazioni per uno specifico corpus per un modello personalizzato.

Entrambi i metodi restituiscono il nome (`name`) del corpus, il numero totale di parole (`total_words`) letto dal corpus e il numero di parole OOV (`out-of-vocabulary_words`) estratte dal corpus. I metodi elencano anche lo stato (`status`) del corpus. Lo stato è importante per il controllo dell'analisi del servizio di un corpus in risposta a una richiesta di eseguirne l'aggiunta a un modello personalizzato:

-   `analyzed` indica che il servizio ha eseguito correttamente l'analisi del corpus. Il modello personalizzato può essere addestrato con i dati del corpus.
-   `being_processed` indica che il servizio sta ancora analizzando il corpus. Il servizio non può accettare richieste di aggiunta di nuovi corpora o parole, o di addestrare il modello personalizzato, fino a quando la sua analisi non è completa.
-   `undetermined` indica che il servizio ha riscontrato un errore durante l'elaborazione del corpus. Le informazioni restituite per il corpus includono un messaggio di errore che offre una guida per la correzione dell'errore.

    Ad esempio, il corpus potrebbe non essere valido oppure potresti aver provato ad aggiungere un corpus con lo stesso nome di un corpus esistente. Puoi provare nuovamente ad aggiungere il corpus e includere il parametro `allow_overwrite` con la richiesta. Puoi anche eliminare il corpus e quindi provare ad aggiungerlo nuovamente.

### Richieste e risposte di esempio
{: #listExample-corpora}

Il seguente esempio elenca tutti i corpora per il modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

Tre corpora sono stati aggiunti al modello personalizzato. Il servizio ha analizzato correttamente `corpus1`. Si sta ancora analizzando `corpus2` e la sua analisi di `corpus3` non è riuscita.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

Il seguente esempio restituisce le informazioni relative al corpus denominato `corpus1` per il modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## Eliminazione di un corpus da un modello di lingua personalizzato
{: #deleteCorpus}

Utilizza il metodo `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` per rimuovere un corpus esistente da un modello di lingua personalizzato. Quando elimina il corpus, il servizio rimuove le parole OOV a esso associate dalle risorse di parole del modello personalizzato tranne che nei seguenti casi

-   La parola era stata aggiunta anche da un altro corpus o da una grammatica
-   La parola era stata modificata in qualche modo con il metodo `POST /v1/customizations/{customization_id}/words` o `PUT /v1/customizations/{customization_id}/words/{word_name}`.

La rimozione di un corpus non influisce sul modello personalizzato fino a quando non addestri il modello sui suoi dati aggiornati utilizzando il metodo `POST /v1/customizations/{customization_id}/train`. Se hai addestrato con esito positivo il modello sul corpus, le parole dal corpus rimangono nel vocabolario del modello e si applicano al riconoscimento vocale finché non riesegui l'addestramento del modello.

### Richiesta di esempio
{: #deleteExample-corpus}

Il seguente esempio elimina il corpus denominato `corpus3` dal modello personalizzato con l'ID di personalizzazione specificato.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
