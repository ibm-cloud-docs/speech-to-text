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

# Seguridad de la información
{: #information-security}

En {{site.data.keyword.IBM}} nos comprometemos a proporcionar a nuestros clientes y socios soluciones innovadoras de privacidad de datos, seguridad y control.
{: shortdesc}

Los clientes son responsables de garantizar el cumplimiento de las leyes y normativas aplicables, incluido el Reglamento general de protección de datos de la Unión Europea. Los clientes son los únicos responsables de obtener asesoramiento de un asesor jurídico competente en cuanto a la identificación e interpretación de las leyes y los reglamentos pertinentes que puedan afectar al negocio de los clientes y a cualquier medida que los clientes puedan emprender para cumplir con esas leyes y reglamentos.
{: important}

Los productos, los servicios y otras prestaciones que se describen en el presente documento no son adecuados para todos los casos de cliente y pueden tener una disponibilidad restringida. {{site.data.keyword.IBM_notm}} no proporciona asesoramiento legal, contable ni de auditoría ni representa ni garantiza que sus servicios o productos garanticen que los clientes vayan a cumplir con cualquier legislación o regulación.

Si tiene que solicitar soporte de GDPR para los recursos de {{site.data.keyword.cloud}} {{site.data.keyword.watson}} que se crean

-   En la Unión Europea (UE), consulte [Solicitud de soporte para los recursos de {{site.data.keyword.cloud_notm}} Watson creados en la Unión Europea](/docs/services/watson?topic=watson-gdpr-sar#request-EU).
-   Fuera de la Unión Europea, consulte [Solicitud de soporte para recursos fuera de la Unión Europea](/docs/services/watson?topic=watson-gdpr-sar#request-non-EU).

## Reglamento General de Protección de Datos de la Unión Europea (GDPR)
{: #gdpr}

En {{site.data.keyword.IBM_notm}} nos comprometemos a proporcionar a nuestros clientes y socios soluciones innovadoras de privacidad de datos, seguridad y control para ayudarles en el cumplimiento con GDPR.

Obtenga más información sobre el camino de {{site.data.keyword.IBM_notm}} hacia la conformidad con GDPR y sobre nuestras prestaciones y ofertas relacionadas con GDPR para darle soporte en su camino hacia esta conformidad [aquí](http://www.ibm.com/gdpr){: external}.

## Etiquetado y supresión de datos en el servicio Speech to Text
{: #gdpr-speech-to-text}

El servicio {{site.data.keyword.speechtotextfull}} le permite suprimir todos los datos asociados a solicitudes de reconocimiento, modelos de lenguaje personalizado y modelos acústicos personalizados. Para suprimir datos, debe hacer lo siguiente:

1.  Utilice la cabecera `X-Watson-Metadata` para asociar un ID de cliente a los datos que una solicitud pasa al servicio; consulte [Especificación de un ID de cliente](#specify-customer-id).
1.  Utilice el método `DELETE /v1/user_data` para suprimir todos los datos asociados a un ID de cliente especificado; consulte [Supresión de datos](#delete-pi).

De forma predeterminada, no se asocia ningún ID de cliente a los datos.

Las características experimentales y beta no están pensadas para su uso en un entorno de producción y, por lo tanto, no se garantiza que funcionen tal como se esperaba cuando se etiquetan y se suprimen datos. Las características experimentales y beta no deben utilizarse al implementar una solución que requiera el etiquetado y supresión de datos.
{: note}

### Especificación de un ID de cliente
{: #specify-customer-id}

Para asociar un ID de cliente con datos, incluya la cabecera `X-Watson-Metadata` con la solicitud que pasa la información. La serie `customer_id={id}` se pasa como argumento de la cabecera. En el ejemplo siguiente se asocia el ID de cliente `my_customer_ID` con los datos que se pasan con una solicitud `POST /v1/recognize`:

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Un ID de cliente puede incluir cualquier carácter excepto `;` (punto y coma) y `=` (signo igual). Especifique una serie aleatoria o genérica para el ID de cliente; no especifique una serie de identificación personal, como por ejemplo una dirección de correo electrónico o un ID de Twitter. Puede especificar distintos ID de cliente con las diferentes solicitudes. Un ID de cliente que especifique se asocia a la instancia del servicio cuyas credenciales se utilizan con la solicitud; solo las credenciales correspondientes a dicha instancia del servicio pueden suprimir los datos asociados con el ID.

Utilice la cabecera `X-Watson-Metadata` con los métodos siguientes:

-   Con solicitudes WebSocket:
    -   `/v1/recognize`

    El ID de cliente se especifica con el parámetro de consulta `x-watson-metadata` de la solicitud para abrir la conexión. Debe codificar en URL el argumento para el parámetro de consulta, por ejemplo `customer_id%3dmy_customer_ID`. El ID de cliente se asocia a todos los datos que se pasan con las solicitudes de reconocimiento enviadas a través de la conexión.
-   Con solicitudes HTTP síncronas:
    -   `POST /v1/recognize`

    El ID de cliente se asocia con los datos que se envían con la solicitud individual.
-   Con solicitudes HTTP asíncronas:
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    El ID de cliente se asocia con el URL de devolución de llamada de la lista blanca o con los datos que se envían con la solicitud de reconocimiento individual.
-   Con solicitudes para añadir corpus, palabras personalizadas o gramáticas a modelos de lenguaje personalizado:
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    El ID de cliente se asocia con el corpus, las palabras personalizadas o las gramáticas que se añaden o se actualizan mediante la solicitud.
-   Con solicitudes para añadir recursos de audio a modelos acústicos personalizados:
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    El ID de cliente se asocia con el recurso de audio que la solicitud añade o actualiza.

### Supresión de datos
{: #delete-pi}

Para suprimir todos los datos que están asociados a un ID de cliente, utilice el método `DELETE /v1/user_data`. Pase la serie `customer_id={id}` como parámetro de consulta con la solicitud. En el ejemplo siguiente se suprimen todos los datos correspondientes al ID de cliente `my_customer_ID`:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

El método `/v1/user_data` suprime todos los datos asociados con el ID de cliente especificado, independientemente del método por el que se ha añadido la información. El método no tiene ningún efecto si no hay datos asociados con el ID de cliente. Debe enviar la solicitud con credenciales correspondientes a la misma instancia del servicio que se ha utilizado para asociar el ID de cliente con los datos.
