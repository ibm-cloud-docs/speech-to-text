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

# Gestione delle grammatiche
{: #manageGrammars}

L'interfaccia di personalizzazione include il metodo `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` per l'aggiunta di una grammatica a un modello di lingua personalizzato. Per ulteriori informazioni, vedi [Aggiungi una grammatica al modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). L'interfaccia include anche i seguenti metodi per elencare ed eliminare le grammatiche per un modello di lingua personalizzato.
{: shortdesc}

## Elenco delle grammatiche per un modello di lingua personalizzato
{: #listGrammars}

L'interfaccia di personalizzazione fornisce due metodi per elencare le informazioni relative alle grammatiche per un modello di lingua personalizzato:

-   Il metodo `GET /v1/customizations/{customization_id}/grammars` elenca le informazioni su tutte le grammatiche per un modello personalizzato.
-   Il metodo `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` elenca le informazioni su una grammatica specificata per un modello personalizzato.

Entrambi i metodi restituiscono le stesse informazioni su una grammatica. Le informazioni includono il nome (`name`) della grammatica e il numero di parole OOV (`out-of_vocabulary_words`) riconosciute dalla grammatica. La risposta include anche lo `status` della grammatica, che è importante per controllare l'analisi della grammatica da parte del servizio quando la aggiungi a un modello personalizzato:

-   `being_processed` indica che il servizio sta ancora elaborando la grammatica in risposta a una richiesta `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`.
-   `analyzed` indica che il servizio ha elaborato correttamente la grammatica e l'ha aggiunta al modello personalizzato.
-   `undetermined` significa che il servizio ha rilevato un errore durante l'elaborazione della grammatica. Le informazioni restituite per la grammatica includono un messaggio di errore che offre indicazioni per correggere l'errore.

    Ad esempio, la grammatica potrebbe non essere valida oppure potresti aver tentato di aggiungere una grammatica con lo stesso nome di una grammatica esistente. Puoi provare ad aggiungere nuovamente la grammatica e includere il parametro `allow_overwrite` con la richiesta. Puoi anche eliminare la grammatica e quindi provare ad aggiungerla di nuovo.

### Richieste e risposte di esempio
{: #listExample-grammars}

Il seguente esempio elenca le informazioni su tutte le grammatiche che sono state aggiunte al modello personalizzato con l'ID di personalizzazione specificato.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

Tre grammatiche sono state aggiunte correttamente al modello personalizzato: `confirm-xml`, `confirm-abnf` e `list-abnf`.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

Il seguente esempio mostra le informazioni sulla grammatica specificata, `list-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## Eliminazione di una grammatica da un modello di lingua personalizzato
{: #deleteGrammar}

Utilizza il metodo `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` per rimuovere una grammatica esistente da un modello di lingua personalizzato. Quando elimina la grammatica, il servizio rimuove tutte le parole OOV associate alla grammatica dalla risorsa di parole del modello personalizzato, tranne nei seguenti casi

-   La parola era stata aggiunta anche da un'altra grammatica o da un corpus.
-   La parola era stata modificata in qualche modo con il metodo `POST /v1/customizations/{customization_id}/words` o `PUT /v1/customizations/{customization_id}/words/{word_name}`.

La rimozione di una grammatica non influisce sul modello personalizzato finché non addestri il modello sui relativi dati aggiornati utilizzando il metodo `POST /v1/customizations/{customization_id}/train`. Se hai addestrato correttamente il modello sulla grammatica, la grammatica continua ad essere disponibile per il riconoscimento vocale finché non ripeti l'addestramento del modello.

### Richiesta di esempio
{: #deleteExample-grammar}

Il seguente esempio elimina la grammatica denominata `list-abnf` dal modello personalizzato con l'ID di personalizzazione specificato.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
