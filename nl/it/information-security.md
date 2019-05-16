---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# Sicurezza delle informazioni
{: #information-security}

{{site.data.keyword.IBM}} si impegna a fornire ai nostri clienti e partner soluzioni innovative per la privacy, la sicurezza e la governance dei dati.
{: shortdesc}

I clienti sono responsabili di garantire la propria conformità alle varie leggi e normative, incluso il Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea. È responsabilità esclusiva dei clienti richiedere una consulenza legale qualificata per identificare e interpretare normative e regolamenti che potrebbero influenzare le attività di business dei clienti ed eventuali azioni che i clienti potrebbero dover intraprendere per rispettare la conformità a tali normative e regolamenti.
{: important}

I prodotti, i servizi e le altre funzionalità qui descritti non sono adatti per tutte le situazioni dei clienti e potrebbero avere una disponibilità limitata. {{site.data.keyword.IBM_notm}} non fornisce consulenza legale, contabile o di controllo né dichiara o garantisce che i propri servizi o prodotti assicureranno che i clienti siano conformi ad eventuali norme di legge o regolamentari.

Se hai bisogno di richiedere il supporto GDPR per le risorse {{site.data.keyword.cloud}} {{site.data.keyword.watson}} che vengono create

-   Nell'Unione Europea, vedi [Richiesta di supporto per le risorse {{site.data.keyword.cloud_notm}} Watson create nell'Unione Europea](/docs/services/watson/getting-started-gdpr-sar.html#request-EU).
-   Al di fuori dell'Unione Europea (EU), vedi [Richiesta di supporto per le risorse fuori dall'Unione Europea](/docs/services/watson/getting-started-gdpr-sar.html#request-non-EU).

## Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea
{: #gdpr}

{{site.data.keyword.IBM_notm}} si impegna a offrire ai nostri clienti e partner soluzioni innovative per la privacy, la sicurezza e la governance dei dati per assisterli nel loro percorso verso la conformità GDPR.

È possibile trovare ulteriori informazioni sul percorso di conformità al GDPR di {{site.data.keyword.IBM_notm}} e sulle nostre funzionalità e offerte GDPR per supportare il tuo percorso di conformità [qui![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/gdpr){: new_window}.

## Etichettatura ed eliminazione dei dati nel servizio Speech to Text
{: #gdpr-speech-to-text}

Il servizio {{site.data.keyword.speechtotextfull}} ti consente di eliminare tutti i dati associati alle richieste di riconoscimento, ai modelli di lingua personalizzati e ai modelli acustici personalizzati. Per eliminare i dati, devi effettuare le seguenti operazioni:

1.  Utilizza l'intestazione `X-Watson-Metadata` per associare un ID cliente ai dati passati tramite una richiesta al servizio; vedi [Specifica di un ID cliente](#specify-customer-id).
1.  Utilizza il metodo `DELETE /v1/user_data` per eliminare tutti i dati associati a un ID cliente specificato; vedi [Eliminazione dei dati](#delete-pi).

Per impostazione predefinita, nessun ID cliente è associato ai dati.

Le funzioni sperimentali e beta non sono pensate per l'utilizzo in un ambiente di produzione e pertanto non è garantito che funzionino come previsto durante l'etichettatura e l'eliminazione dei dati. Le funzioni sperimentali e beta non dovrebbero essere utilizzate durante l'implementazione di una soluzione che richiede l'etichettatura e l'eliminazione dei dati.
{: note}

### Specifica di un ID cliente
{: #specify-customer-id}

Per associare un ID cliente ai dati, includi l'intestazione `X-Watson-Metadata` con la richiesta che passa le informazioni. Passi la stringa `customer_id={id}` come argomento dell'intestazione. Il seguente esempio associa l'ID cliente `my_customer_ID` ai dati passati con una richiesta `POST /v1/recognize`:

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Un ID cliente può includere qualsiasi carattere ad eccezione di `;` (punto e virgola) e `=` (segno di uguale). Specifica una stringa casuale o generica per l'ID cliente; non specificare una stringa con informazioni personali, come un indirizzo email o un ID Twitter. Puoi specificare diversi ID cliente con richieste diverse. Un ID cliente che specifichi viene associato all'istanza del servizio le cui credenziali vengono utilizzate con la richiesta; solo le credenziali per quell'istanza del servizio possono eliminare i dati associati all'ID.

Utilizza l'intestazione `X-Watson-Metadata` con i seguenti metodi:

-   Con le richieste WebSocket:
    -   `/v1/recognize`

    Specifica l'ID cliente con il parametro di query `x-watson-metadata` della richiesta per aprire la connessione. Devi codificare in URL l'argomento nel parametro di query, ad esempio `customer_id%3dmy_customer_ID`. L'ID cliente viene associato a tutti i dati passati con le richieste di riconoscimento inviate tramite la connessione.
-   Con le richieste HTTP sincrone:
    -   `POST /v1/recognize`

    L'ID cliente viene associato ai dati inviati con la singola richiesta.
-   Con le richieste HTTP asincrone:
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    L'ID cliente viene associato all'URL di callback inserito in whitelist o ai dati inviati con la singola richiesta di riconoscimento.
-   Con le richieste di aggiunta di corpora, parole personalizzate o grammatiche ai modelli di lingua personalizzati:
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    L'ID cliente viene associato ai corpora, alle parole personalizzate o alle grammatiche che vengono aggiunti o aggiornati dalla richiesta.
-   Con le richieste di aggiunta di risorse audio ai modelli acustici personalizzati:
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    L'ID cliente viene associato alla risorsa audio che viene aggiunta o aggiornata dalla richiesta.

### Eliminazione dei dati
{: #delete-pi}

Per eliminare tutti i dati associati a un ID cliente, utilizza il metodo `DELETE /v1/user_data`. Passi la stringa `customer_id={id}` come parametro di query con la richiesta. Il seguente esempio elimina tutti i dati per l'ID cliente `my_customer_ID`:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

Il metodo `/v1/user_data` elimina tutti i dati associati all'ID cliente specificato, indipendentemente dal metodo con cui sono state aggiunte le informazioni. Il metodo non ha alcun effetto se non ci sono dati associati all'ID cliente. Devi immettere la richiesta con le credenziali per la stessa istanza del servizio utilizzata per associare l'ID cliente ai dati.
