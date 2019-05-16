---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# Riepilogo dei parametri
{: #summary}

Viene qui di seguito riportato un riepilogo di tutti i parametri disponibili per il riconoscimento vocale. Per ulteriori informazioni su tutti i metodi del servizio {{site.data.keyword.speechtotextshort}}, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
{: shortdesc}

Quando effettui una richiesta di riconoscimento vocale, tieni conto dei seguenti requisiti di base:

-   I nomi di metodo sono sensibili a maiuscole/minuscole.
-   Le intestazioni di richiesta HTTP non sono sensibili a maiuscole/minuscole.
-   I parametri di query HTTP e WebSocket sono sensibili a maiuscole/minuscole.
-   I nomi di campo JSON sono sensibili a maiuscole/minuscole.
-   Tutto il contenuto di risposta JSON è nel set di caratteri UTF-8.

Tieni inoltre conto dei seguenti requisiti specifici per il servizio:

-   Devi specificare solo l'audio di input. Tutti gli altri parametri sono facoltativi.
-   Se specifichi un campo JSON o un parametro di query non valido come parte dell'input, la risposta include un campo `warnings` che descrive l'argomento non valido. La richiesta ha esito positivo nonostante le eventuali avvertenze.

## access_token
{: #summary-access-token}

Se utilizzi l'autenticazione IAM (Identity and Access Management), un token di accesso IAM facoltativo che utilizzi per stabilire una connessione autenticata con l'interfaccia WebSocket. Per ulteriori informazioni, vedi [Apri una connessione](/docs/services/speech-to-text/websockets.html#WSopen).

<table>
  <caption>Tabella 1. Il parametro access_token</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Non supportato
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Non supportato
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

Un ID di personalizzazione facoltativo per un modello acustico personalizzato che viene adattato per le caratteristiche acustiche del tuo ambiente e dei tuoi parlanti. Per impostazione predefinita, non viene utilizzato alcun modello personalizzato. Per ulteriori informazioni, vedi [Modelli personalizzati](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabella 2. Il parametro acoustic_customization_id</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Beta per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

Una versione facoltativa di un modello di base. Il parametro è concepito principalmente per essere utilizzato con i modelli personalizzati aggiornati per un nuovo modello di base ma può essere utilizzato senza un modello personalizzato. Il valore predefinito dipende dal fatto che il parametro sia utilizzato con o senza un modello personalizzato. Per ulteriori informazioni, vedi [Versione del modello di base](/docs/services/speech-to-text/input.html#version).

<table>
  <caption>Tabella 3. Il parametro base_model_version</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

Un formato audio facoltativo (tipo MIME) che specifica il formato dei dati audio che passi al servizio. Il servizio può rilevare automaticamente il formato della maggior parte degli elementi audio, quindi il parametro è facoltativo per la maggior parte dei formati. È obbligatorio per i formati `audio/alaw`, `audio/basic`, `audio/l16` e `audio/mulaw`. Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html).

<table>
  <caption>Tabella 4. Il parametro Content-Type</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro <code>content-type</code> del messaggio <code>start</code>
      JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

Un valore double facoltativo compreso tra 0.0 e 1.0 che indica il peso relativo dato dal servizio alle parole da un modello di lingua personalizzato rispetto alle parole dal vocabolario di base. Il valore predefinito è 0.3 a meno che non sia stato specificato un peso diverso quando è stato addestrato il modello di lingua personalizzato. Per ulteriori informazioni, vedi [Modelli personalizzati](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabella 5. Il parametro customization_weight</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per inglese (Stati Uniti), inglese (Regno Unito), portoghese (Brasile), francese, tedesco, giapponese, coreano e spagnolo
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

Una stringa facoltativa che identifica una grammatica che deve essere utilizzata per il riconoscimento vocale. Il servizio riconosce solo le stringhe definite dalla grammatica. Devi specificare sia il nome della grammatica che l'ID di personalizzazione del modello di lingua personalizzato per il quale è definita la grammatica. Per ulteriori informazioni, vedi [Grammatiche](/docs/services/speech-to-text/input.html#grammars-input).

<table>
  <caption>Tabella 6. Il parametro grammar_name</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Beta per inglese (Stati Uniti), inglese (Regno Unito), portoghese (Brasile), francese, tedesco, giapponese, coreano e spagnolo
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

Un numero intero facoltativo che specifica il numero di secondi per il timeout di inattività del servizio. Inattività significa che il servizio non rileva alcun discorso nell'audio del flusso. Il valore predefinito è 30 secondi. Utilizza `-1` per indicare un tempo indefinito. Per ulteriori informazioni, vedi [Timeout di inattività](/docs/services/speech-to-text/input.html#timeouts-inactivity).

<table>
  <caption>Tabella 7. Il parametro inactivity_timeout</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

Un valore booleano facoltativo che indica al servizio di restituire le ipotesi intermedie che probabilmente varieranno prima della trascrizione finale. Per impostazione predefinita (`false`), i risultati provvisori non vengono restituiti. Per ulteriori informazioni, vedi [Risultati provvisori](/docs/services/speech-to-text/output.html#interim).

<table>
  <caption>Tabella 8. Il parametro interim_results</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Non supportato
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Non supportato
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

Un array facoltativo di stringhe di parole chiave che il servizio individua nell'audio di input. Per impostazione predefinita, l'individuazione di parole chiave non viene eseguita. Per ulteriori informazioni, vedi [Individuazione di parole chiave](/docs/services/speech-to-text/output.html#keyword_spotting).

<table>
  <caption>Tabella 9. Il parametro keywords</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

Un valore double facoltativo compreso tra 0.0 e 1.0 che indica la soglia minima per una corrispondenza di parola chiave positiva. Per impostazione predefinita, l'individuazione di parole chiave non viene eseguita. Per ulteriori informazioni, vedi [Individuazione di parole chiave](/docs/services/speech-to-text/output.html#keyword_spotting).

<table>
  <caption>Tabella 10. Il parametro keywords_threshold</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

Un ID di personalizzazione facoltativo per un modello di lingua personalizzato che include la terminologia dal tuo dominio. Per impostazione predefinita, non viene utilizzato alcun modello personalizzato. Per ulteriori informazioni, vedi [Modelli personalizzati](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabella 11. Il parametro language_customization_id</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per inglese (Stati Uniti), inglese (Regno Unito), portoghese (Brasile), francese, tedesco, giapponese, coreano e spagnolo
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

Un numero intero facoltativo che specifica il numero massimo di ipotesi alternative restituito dal servizio. Per impostazione predefinita, il servizio restituisce una singola ipotesi finale. Per ulteriori informazioni, vedi [Numero massimo di alternative](/docs/services/speech-to-text/output.html#max_alternatives).

<table>
  <caption>Tabella 12. Il parametro max_alternatives</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## model
{: #summary-model}

Un modello facoltativo che specifica la lingua in cui viene pronunciato l'audio e la frequenza con la quale viene campionato: banda larga (broadband) o banda stretta (narrowband). Per impostazione predefinita, viene utilizzato `en-US_BroadbandModel`. Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html).

<table>
  <caption>Tabella 13. Il parametro model</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

Un valore booleano facoltativo che indica se il servizio censura il contenuto volgare in una trascrizione. Per impostazione predefinita (`true`), il contenuto volgare viene filtrato dalla trascrizione. Per ulteriori informazioni, vedi [Filtro di contenuto volgare](/docs/services/speech-to-text/output.html#profanity_filter).

<table>
  <caption>Tabella 14. Il parametro profanity_filter</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per l'inglese (Stati Uniti)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

Un valore booleano facoltativo che indica se il servizio oscura i dati numerici con tre o più cifre consecutive da una trascrizione. Se imposti il parametro `redaction` su `true`, il servizio forza automaticamente il parametro `smart_formatting` su `true`. Per impostazione predefinita (`false`), i dati numerici non sono oscurati. Per ulteriori informazioni, vedi [Oscuramento numerico](/docs/services/speech-to-text/output.html#redaction).

<table>
  <caption>Tabella 15. Il parametro redaction</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Beta per inglese (Stati Uniti), giapponese e coreano
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

Un valore booleano facoltativo che indica se il servizio converte le date, le ore, i numeri, la valuta e valori simili in rappresentazioni più convenzionali nella trascrizione finale. Per l'inglese (Stati Uniti), la funzione converte anche delle specifiche frasi di parole chiave in simboli di punteggiatura. Per impostazione predefinita (`false`), la formattazione intelligente non viene eseguita. Per ulteriori informazioni, vedi [Formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting).

<table>
  <caption>Tabella 16. Il parametro smart_formatting</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Beta per inglese (Stati Uniti), giapponese e spagnolo
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

Un valore booleano facoltativo che indica se il servizio identifica gli individui e le parole da essi espresse in uno scambio tra più partecipanti. Se imposti il parametro `speaker_labels` su `true`, il servizio forza automaticamente il parametro `timestamps` su `true`. Per impostazione predefinita (`false`), le etichette del parlante non vengono restituite. Per ulteriori informazioni, vedi [Etichette del parlante](/docs/services/speech-to-text/output.html#speaker_labels).

<table>
  <caption>Tabella 17. Il parametro speaker_labels</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Beta per inglese (Stati Uniti), giapponese e spagnolo (modelli a banda larga e a banda stretta) e inglese (Regno Unito) (solo modello a banda stretta).
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

Un valore booleano facoltativo che indica se il servizio produce delle date/ore per le parole della trascrizione. Per impostazione predefinita (`false`), le date/ore non vengono restituite. Per ulteriori informazioni, vedi [Date/ore delle parole](/docs/services/speech-to-text/output.html#word_timestamps).

<table>
  <caption>Tabella 18. Il parametro timestamps</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

Un valore facoltativo di `chunked` che causa l'invio in streaming dell'audio al servizio. Per impostazione predefinita, l'audio viene inviato tutto insieme come una singola fornitura unica. Per ulteriori informazioni, vedi [Trasmissione audio](/docs/services/speech-to-text/input.html#transmission).

<table>
  <caption>Tabella 19. Il parametro Transfer-Encoding</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Non applicabile, sempre in streaming
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

Se utilizzi le credenziali del servizio Cloud Foundry, un token di autenticazione {{site.data.keyword.watson}} facoltativo che utilizzi per stabilire una connessione autenticata con l'interfaccia WebSocket. Per ulteriori informazioni, vedi [Apri una connessione](/docs/services/speech-to-text/websockets.html#WSopen).

<table>
  <caption>Tabella 20. Il parametro watson-token</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Non supportato
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Non supportato
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

Un valore double facoltativo compreso tra 0.0 e 1.0 che specifica la soglia a cui il servizio notifica le alternative acusticamente simili per le parole dell'audio di input. Per impostazione predefinita, le alternative alle parole non vengono restituite. Per ulteriori informazioni, vedi [Alternative alle parole](/docs/services/speech-to-text/output.html#word_alternatives).

<table>
  <caption> Tabella 21. Il parametro word_alternatives_threshold</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

Un valore booleano facoltativo che indica se il servizio fornisce le misure dell'attendibilità per le parole della trascrizione. Per impostazione predefinita (`false`), le misure dell'attendibilità delle parole non vengono restituite. Per ulteriori informazioni, vedi [Attendibilità delle parole](/docs/services/speech-to-text/output.html#word_confidence).

<table>
  <caption>Tabella 22. Il parametro word_confidence</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro del messaggio <code>start</code> JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Parametro di query del metodo <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

Un token di autenticazione facoltativo che effettua richieste autenticate al servizio senza integrare le tue credenziali del servizio in ogni chiamata. Per impostazione predefinita, le credenziali del servizio devono essere passate con ogni richiesta. I token di autenticazione Watson sono basati sulle credenziali del servizio Cloud Foundry che utilizzano uno `{username}` e una `{password}` per l'autenticazione.

L'intestazione `X-Watson-Authorization-Token` non accetta i token IAM o le chiavi API.
{: note}

<table>
  <caption>Tabella 23. Il parametro X-Watson-Authorization-Token</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Non supportato: utilizza il parametro di query <code>watson-token</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta di ciascuna richiesta
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta di ciascuna richiesta
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

Un valore booleano facoltativo che indica se rifiuti esplicitamente la registrazione delle richieste predefinita eseguita da {{site.data.keyword.IBM_notm}} per migliorare il servizio per gli utenti futuri. Per impedire che IBM acceda ai tuoi dati per miglioramenti del servizio generali, specifica <code>true</code> per il parametro. Per ulteriori informazioni, vedi [Registrazione delle richieste](/docs/services/speech-to-text/input.html#logging).

<table>
  <caption>Tabella 24. Il parametro X-Watson-Learning-Opt-Out</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametro di query <code>x-watson-learning-opt-out</code>
      della richiesta di connessione <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta di ciascuna richiesta
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta di ciascuna richiesta
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

Una stringa facoltativa che associa un ID cliente ai dati passati per le richieste di riconoscimento. Il parametro accetta l'argomento `customer_id={id}`. Per impostazione predefinita, nessun ID cliente è associato ai dati. Per ulteriori informazioni, vedi [Sicurezza delle informazioni](/docs/services/speech-to-text/information-security.html).

<table>
  <caption>Tabella 25.  Il parametro X-Watson-Metadata</caption>
  <tr>
    <th>Disponibilità e utilizzo</th>
    <th style="vertical-align:bottom">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilità**
    </td>
    <td style="text-align:left">
      Generalmente disponibile per tutte le lingue
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parametri di query <code>x-watson-metadata</code> della
      richiesta di connessione <code>/v1/recognize</code> (devi codificare in URL
      l'argomento, ad esempio `customer_id%3dmy_customer_ID`.)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP sincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta relativa alla richiesta POST <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asincrono**
    </td>
    <td style="text-align:left">
      Intestazione della richiesta relativa alle richieste <code>POST /v1/register_callback</code> e
      <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>
