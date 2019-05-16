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

# Panoramica per gli sviluppatori
{: #developerOverview}

Puoi accedere alle funzionalità del servizio {{site.data.keyword.speechtotextfull}} utilizzando un'interfaccia WebSocket e usando le interfacce REST (Representational State Transfer) HTTP sincrone o asincrone. Puoi anche personalizzare i modelli di lingua del servizio per adattarli al tuo dominio e al tuo ambiente. Sono disponibili degli SDK per semplificare lo sviluppo delle applicazioni in molti linguaggi di programmazione.
{: shortdesc}

## Programmazione con il servizio
{: #programming}

[Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text/basic-request.html) ti mostra come richiedere la trascrizione di base con ciascuna delle interfacce di programmazione del servizio:

-   [L'interfaccia WebSocket](/docs/services/speech-to-text/websockets.html) offre un'efficiente implementazione a bassa latenza e velocità effettiva elevata su una connessione full duplex.
-   [L'interfaccia HTTP sincrona](/docs/services/speech-to-text/http.html) fornisce un'interfaccia di base per trascrivere l'audio con richieste bloccanti.
-   [L'interfaccia HTTP asincrona](/docs/services/speech-to-text/async.html) fornisce un'interfaccia non bloccante che ti consente di registrare un URL di callback per ricevere le notifiche o eseguire il polling del servizio per ottenere lo stato del lavoro e i risultati.

Le interfacce forniscono le stesse funzionalità di riconoscimento vocale, ma potresti specificare lo stesso parametro come intestazione di richiesta, parametro di query o parametro di un oggetto JSON in base all'interfaccia che utilizzi. Inoltre, le interfacce WebSocket e HTTP sincrona accettano un massimo di 100 MB di dati audio con una singola richiesta. L'interfaccia HTTP asincrona accetta un massimo di 1 GB di dati audio.

-   Per le descrizioni di tutti i parametri di riconoscimento vocale disponibili, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text/summary.html).
-   Per le descrizioni di tutti i metodi e dei relativi parametri, insieme ad esempi, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Vantaggi dell'interfaccia WebSocket
{: #advantages}

L'interfaccia WebSocket presenta numerosi vantaggi rispetto all'interfaccia HTTP:

-   L'interfaccia WebSocket fornisce un canale di comunicazione full duplex a singolo socket. L'interfaccia consente al client di inviare richieste e audio al servizio e di ricevere i risultati su una singola connessione in modo asincrono.
-   Fornisce un'esperienza di programmazione molto più semplice e più potente. Il servizio invia risposte basate su eventi ai messaggi del client, eliminando la necessità per il client di eseguire il polling del server.
-   Ti consente di stabilire e utilizzare una singola connessione autenticata per un tempo illimitato. Le interfacce HTTP richiedono di autenticare ogni chiamata al servizio.
-   Riduce la latenza. I risultati del riconoscimento arrivano più velocemente perché il servizio li invia direttamente al client. L'interfaccia HTTP richiede quattro richieste e connessioni distinte per ottenere gli stessi risultati.
-   Riduce l'utilizzo della rete. Il protocollo WebSocket è leggero. Richiede solo una singola connessione per eseguire il riconoscimento vocale dinamico.
-   Consente di inviare in streaming l'audio direttamente dai browser (client WebSocket HTML5) al servizio.

## Personalizzazione del servizio
{: #customizing}

[L'interfaccia di personalizzazione](/docs/services/speech-to-text/custom.html) ti permette di creare modelli personalizzati per migliorare le funzionalità di riconoscimento vocale del servizio:

-   I [modelli di lingua personalizzati](/docs/services/speech-to-text/language-create.html) ti consentono di definire parole specifiche per il dominio per un modello di base. I modelli di lingua personalizzati espandono il vocabolario di base del servizio con una terminologia specifica per domini come medicina e legge.
-   I [modelli acustici personalizzati](/docs/services/speech-to-text/acoustic-create.html) ti consentono di adattare un modello di base per le caratteristiche acustiche del tuo ambiente e dei tuoi parlanti. I modelli acustici personalizzati migliorano la capacità del servizio di riconoscere il discorso per specifiche caratteristiche acustiche.
-   Le [grammatiche](/docs/services/speech-to-text/grammar.html) ti consentono di limitare le frasi che il servizio può riconoscere a quelle definite nelle regole della grammatica. Limitando lo spazio di ricerca per le stringhe valide, il servizio può fornire i risultati in modo più veloce e più accurato. Le grammatiche sono supportate con i modelli di lingua personalizzati.

Puoi utilizzare un modello di lingua personalizzato, un modello acustico personalizzato o entrambi per il riconoscimento vocale con qualsiasi interfaccia del servizio.

## Supporto CORS
{: #cors}

Il servizio supporta CORS (Cross-Origin Resource Sharing). Utilizzando CORS, le pagine web possono richiedere le risorse direttamente da un dominio esterno. CORS aggira la politica di sicurezza della stessa origine, che altrimenti impedisce tali richieste. Poiché il servizio supporta CORS, una pagina web può comunicare direttamente con il servizio senza passare la richiesta attraverso il server web che ospita la pagina.

Ad esempio, una pagina web caricata da un server in {{site.data.keyword.cloud}} può chiamare direttamente l'API di personalizzazione, ignorando il server {{site.data.keyword.cloud_notm}}. Per ulteriori informazioni, vedi [enable-cors.org ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://enable-cors.org/){: new_window}.

## Utilizzo di SDK (Software Development Kit)
{: #sdks}

Gli SDK sono disponibili per il servizio {{site.data.keyword.speechtotextshort}} per semplificare lo sviluppo di applicazioni di riconoscimento vocale. Gli SDK {{site.data.keyword.ibmwatson}} sono disponibili per molti linguaggi di programmazione e piattaforme comuni.

-   Per un elenco completo di SDK e link agli SDK su GitHub, vedi [Utilizzo di SDK](/docs/services/watson/getting-started-sdks.html).
-   Per informazioni dettagliate su tutti i metodi degli SDK Node, Java&trade;, Python, Ruby, Swift e Go per il servizio {{site.data.keyword.speechtotextshort}}, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Ulteriori informazioni sullo sviluppo dell'applicazione
{: #learn}

Per ulteriori informazioni sull'utilizzo dei servizi {{site.data.keyword.watson}} e {{site.data.keyword.cloud_notm}}, consulta quanto segue:

-   Per un'introduzione all'utilizzo dei servizi {{site.data.keyword.watson}} e {{site.data.keyword.cloud_notm}}, vedi [Introduzione a {{site.data.keyword.watson}} e {{site.data.keyword.cloud_notm}}](/docs/services/watson/index.html).
-   Tutte le nuove istanze del servizio utilizzano {{site.data.keyword.cloud_notm}} IAM (Identity and Access Management) per l'autenticazione. Le istanze del servizio meno recenti potrebbero continuare a utilizzare `{username}` e `{password}` delle loro credenziali esistenti del servizio Cloud Foundry per eseguire l'autenticazione. Per ulteriori informazioni sull'autenticazione presso il servizio, vedi l'[aggiornamento del servizio del 30 ottobre 2018](/docs/services/speech-to-text/release-notes.html#October2018b) nelle note sulla release.
-   Per informazioni sul controllo della registrazione delle richieste predefinita eseguita per tutti i servizi {{site.data.keyword.watson}}, vedi [Controllo della registrazione delle richieste per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-logging.html).
