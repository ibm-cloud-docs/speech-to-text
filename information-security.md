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

# Informationssicherheit
{: #information-security}

{{site.data.keyword.IBM}} hat sich dazu verpflichtet, seinen Kunden und Partnern innovative Lösungen für Datenschutz, Sicherheit und Governance zu bieten.
{: shortdesc}

Jeder Kunde ist für die Einhaltung der geltenden Gesetze und Vorschriften<!--geprüft-->, einschließlich der Datenschutz-Grundverordnung der Europäischen Union (DSGVO), selbst verantwortlich. <!--geprüft--> Es obliegt allein den Kunden, sich von kompetenter juristischer Stelle zu Inhalt und Auslegung aller relevanten Gesetze und Vorschriften<!--geprüft--> beraten zu lassen, die ihre Geschäftstätigkeit und die von ihnen eventuell einzuleitenden Maßnahmen zur Einhaltung dieser Gesetze und Vorschriften betreffen. <!--geprüft-->
{: important}

Die hierin beschriebenen Produkte, Services und sonstigen Funktionen eignen sich nicht für alle Kundensituationen und sind möglicherweise nur eingeschränkt verfügbar.<!--geprüft--> {{site.data.keyword.IBM_notm}} erteilt keine Rechts- oder Steuerberatung und gibt keine Garantie bezüglich der Konformität von IBM Produkten oder Services mit den geltenden Gesetzen und Vorschriften. <!--geprüft-->

Falls Sie für die erstellten {{site.data.keyword.cloud}} {{site.data.keyword.watson}}-Ressourcen Unterstützung hinsichtlich der DSGVO benötigen, ziehen Sie Folgendes hinzu:

-   Lesen Sie für Ressourcen, die in der Europäischen Union (EU) erstellt werden, den Abschnitt [Unterstützung für {{site.data.keyword.cloud_notm}}-Ressourcen anfordern, die in der Europäischen Union erstellt werden](/docs/services/watson?topic=watson-gdpr-sar#request-EU).
-   Lesen Sie für Ressourcen, die außerhalb der EU erstellt werden, den Abschnitt [Unterstützung für Ressourcen anfordern, die außerhalb der Europäischen Union erstellt werden](/docs/services/watson?topic=watson-gdpr-sar#request-non-EU).

## Datenschutz-Grundverordnung der Europäischen Union (DSGVO)
{: #gdpr}

{{site.data.keyword.IBM_notm}} hat sich dazu verpflichtet, seinen Kunden und Partnern innovative Lösungen für Datenschutz, Sicherheit und Governance zu bieten und sie bei der Umsetzung der DSGVO zu unterstützen.

Weitere Informationen zu den eigenen Maßnahmen von {{site.data.keyword.IBM_notm}}für die Umsetzung der DSGVO sowie zu den Leistungsmerkmalen und Angeboten, die Ihnen dabei helfen, die Umsetzung der DSGVO vorzubereiten, finden Sie [hier](http://www.ibm.com/gdpr){: external}.

## Daten im Speech to Text-Service kennzeichnen und löschen
{: #gdpr-speech-to-text}

Beim {{site.data.keyword.speechtotextfull}}-Service können Sie alle Daten löschen, die mit Erkennungsanforderungen, angepassten Sprachmodellen und angepassten Akustikmodellen verbunden sind. Zum Löschen der Daten müssen Sie Folgendes ausführen:

1.  Ordnen Sie mit dem Header `X-Watson-Metadata` eine Kunden-ID zu Daten zu, die durch eine Anforderung an den Service übergeben werden; entsprechende Informationen finden Sie unter [Kunden-ID angeben](#specify-customer-id).
1.  Verwenden Sie die Methode `DELETE /v1/user_data`, um alle Daten zu löschen, die einer bestimmten Kunden-ID zugeordnet sind; Näheres hierzu enthält der Abschnitt [Daten löschen](#delete-pi).

Standardmäßig wird den Daten keine Kunden-ID zugeordnet.

Experimentelle und Beta-Funktionen sind nicht für die Verwendung in einer Produktionsumgebung gedacht; beim Kennzeichnen und Löschen von Daten kann ihre erwartungsgemäße Funktionsweise daher nicht garantiert werden. Experimentelle und Beta-Funktionen sollten nicht bei der Implementierung einer Lösung verwendet werden, die das Kennzeichnen und Löschen von Daten erforderlich macht.
{: note}

### Kunden-ID angeben
{: #specify-customer-id}

Um Daten eine Kunden-ID zuzuordnen, nehmen Sie den Header `X-Watson-Metadata` in die Anforderung auf, mit der die Informationen übergeben werden. Als Argument des Headers übergeben Sie die Zeichenfolge `customer_id={id}`. Im folgenden Beispiel wird die Kunden-ID `my_customer_ID` den Daten zugeordnet, die mit einer Methode `POST /v1/recognize` übergeben werden:

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Mit Ausnahme des Semikolons (`;`) und des Gleichheitszeichens (`=`) können Sie in einer Kunden-ID jedes beliebige Zeichen verwenden. Geben Sie für die Kunden-ID eine zufällige oder allgemeine Zeichenfolge an; verwenden Sie keine erkennbar persönlichen Angaben wie beispielsweise eine E-Mail-Adresse oder eine Twitter-ID. Sie können bei unterschiedlichen Anforderungen verschiedene Kunden-IDs angeben. Eine Kunden-ID, die Sie angeben, ist der Instanz des Service zugeordnet, dessen Berechtigungsnachweise mit der Anforderung verwendet werden. Nur Berechtigungsnachweise für diese Instanz des Service können Daten löschen, die der ID zugeordnet sind.

Verwenden Sie den Header `X-Watson-Metadata` bei den folgenden Methoden:

-   WebSocket-Anforderungen:
    -   `/v1/recognize`

    Sie geben die Kunden-ID mit dem Abfrageparameter `x-watson-metadata` der Anforderung an, um die Verbindung zu öffnen. Sie müssen das Argument für den Abfrageparameter als URL codieren, z. B. `customer_id%3dmy_customer_ID`. Die Kunden-ID wird allen Daten zugeordnet, die zusammen mit den über die Verbindung gesendeten Erkennungsanforderungen übergeben werden.
-   Synchrone HTTP-Anforderungen:
    -   `POST /v1/recognize`

    Die Kunden-ID wird allen Daten zugeordnet, die zusammen mit der einzelnen Anforderung gesendet werden.
-   Asynchrone HTTP-Anforderungen:
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    Die Kunden-ID wird der in der Whitelist aufgeführten Callback-URL oder den Daten zugeordnet, die zusammen mit der einzelnen Erkennungsanforderung gesendet werden.
-   Bei Anforderungen zum Hinzufügen von Korpora, angepassten Wörtern oder Grammatiken zu angepassten Sprachmodellen:
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    Die Kunden-ID wird den Korpora, angepassten Wörtern oder Grammatiken zugeordnet, die durch die Anforderung hinzugefügt oder aktualisiert werden.
-   Bei Anforderungen zum Hinzufügen von Audioressourcen zu angepassten Akustikmodellen:
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    Die Kunden-ID wird der Audioressource zugeordnet, die durch die Anforderung hinzugefügt oder aktualisiert wird.

### Daten löschen
{: #delete-pi}

Mit der Methode `DELETE /v1/user_data` können Sie alle Daten löschen, die einer Kunden-ID zugeordnet sind. Hierzu übergeben Sie mit der Anforderung die Zeichenfolge `customer_id={id}` als Abfrageparameter. Im folgenden Beispiel werden alle Daten für die Kunden-ID `my_customer_ID`gelöscht:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

Die Methode `/v1/user_data` löscht alle Daten, die der angegebenen Kunden-ID zugeordnet sind. Hierbei ist es unerheblich, mit welcher Methode die Informationen hinzugefügt wurden. Falls der Kunden-ID keine Daten zugeordnet sind, ist die Methode wirkungslos. Sie müssen die Anforderung mit Berechtigungsnachweisen für dieselbe Instanz des Service ausgeben, die auch verwendet wurde, um den Daten die Kunden-ID zuzuordnen.
