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

# Utilizzo di grammatiche con i modelli di lingua personalizzati
{: #grammars}

Il servizio {{site.data.keyword.speechtotextfull}} supporta l'utilizzo di grammatiche con i modelli di lingua personalizzati. Puoi aggiungere le grammatiche a un modello di lingua personalizzato e utilizzarle per il riconoscimento vocale. Le grammatiche limitano l'insieme di frasi che il servizio può riconoscere dall'audio.
{: shortdesc}

Le grammatiche utilizzano una specifica della lingua formale per definire un insieme di regole di produzione per la trascrizione delle stringhe. Le regole specificano come formare delle stringhe valide dall'alfabeto della lingua. Quando applichi una grammatica al riconoscimento vocale, il servizio può restituire solo una o più delle frasi generate dalla grammatica.

Ad esempio, quando hai bisogno di riconoscere parole o frasi specifiche, come *sì* o *no*, singole lettere o numeri o un elenco di nomi, l'utilizzo delle grammatiche può essere più efficace rispetto all'esame di parole e trascrizioni alternative. Inoltre, limitando lo spazio di ricerca per le stringhe valide, il servizio può fornire i risultati in modo più veloce e più accurato.

Quando utilizzi un modello di lingua personalizzato e una grammatica per il riconoscimento vocale, il servizio può restituire una frase valida dalla grammatica o un risultato vuoto. Se il risultato non è vuoto, il servizio include un punteggio di attendibilità con la trascrizione finale, così come per tutte le richieste di riconoscimento. Per le grammatiche, il punteggio indica la probabilità che la risposta corrisponda alla grammatica. I falsi positivi sono sempre possibili, in particolare per le grammatiche semplici, quindi devi considerare sempre l'attendibilità dei risultati del servizio quando valuti la sua risposta.

La funzione delle grammatiche è una funzionalità beta. Il servizio supporta le grammatiche per tutte le lingue per le quali supporta la personalizzazione del modello di lingua.
{: note}

## Formati di grammatica supportati
{: #grammarFormats}

Il servizio {{site.data.keyword.speechtotextshort}} supporta le grammatiche definite nei seguenti formati standard:

-   *ABNF (Augmented Backus-Naur Form)*, che utilizza una rappresentazione in testo semplice simile alla grammatica BNF tradizionale. Il tipo di supporto per questo formato è `application/srgs`.
-   *Formato XML*, che utilizza elementi XML per rappresentare la grammatica. Il tipo di supporto per questo formato è `application/srgs+xml`.

Entrambi i formati di grammatica hanno la forza espressiva di una grammatica libera dal contesto (Context-Free Grammar o CFG). Tuttavia, il servizio può decodificare solo le grammatiche regolari di tipo 3 nella gerarchia di Chomsky. Tali grammatiche rappresentano automi a stati finiti.

Per informazioni generali sulle grammatiche, consulta le seguenti pagine di Wikipedia:

-   [SRGS (Speech Recognition Grammar Specification) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: new_window}
-   [ABNF (Augmented Backus-Naur Form) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: new_window}
-   [Gerarchia di Chomsky ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikipedia.org/wiki/Chomsky_hierarchy){: new_window}

## La specifica SRGS (Speech Recognition Grammar Specification)
{: #grammarSpecification}

Il servizio {{site.data.keyword.speechtotextshort}} supporta le grammatiche come definito dalla specifica [Speech Recognition Grammar Specification Versione 1.0 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/){: new_window} del W3C. La specifica fornisce informazioni dettagliate sui formati supportati e sulla definizione di una grammatica. Per informazioni sui tipi di elementi multimediali supportati, vedi [Appendix G. Media Types and File Suffix ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#AppG){: new_window} nella specifica.

Attualmente, il servizio *non* supporta tutte le funzioni della specifica SRGS (Speech Recognition Grammar Specification). In particolare, il servizio non supporta le funzioni descritte nelle seguenti sezioni della specifica:

-   [Section 1.4 Semantic Interpretation ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S1.4){: new_window}. {{site.data.keyword.IBM_notm}} sta lavorando per supportare questa funzione in una futura release del servizio.
-   [Section 1.5 Embedded Grammars ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S1.5){: new_window}. {{site.data.keyword.IBM_notm}} sta lavorando per supportare questa funzione in una futura release del servizio.
-   [Section 2.2.2 External Reference by URI ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S2.2.2){: new_window}. Il servizio supporta solo i riferimenti locali, come descritto in [Section 2.2.1 Local References ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S2.2.1){: new_window}. In altre parole, una grammatica deve essere autonoma.
-   [Section 2.2.3 Special Rules ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S2.2.3){: new_window}.
-   [Section 2.2.4 Referencing N-gram Documents (Informative) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S2.2.4){: new_window}.
-   [Section 2.7 Language ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S2.7){: new_window}. Il servizio non supporta il passaggio da una lingua all'altra. Il servizio supporta solo una lingua globale per ogni grammatica.

Le parole nella grammatica devono essere in codifica UTF-8 (ASCII è un sottoinsieme di UTF-8). L'utilizzo di qualsiasi altra codifica può causare problemi durante la compilazione della grammatica o risultati imprevisti nella decodifica. Il servizio ignora una codifica specificata nell'intestazione della grammatica.
{: note}
