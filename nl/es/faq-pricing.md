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

# Preguntas frecuentes sobre precios
{: #faq-pricing}

1.  <span style="color:#003F69">¿Cuánto cuesta utilizar el servicio estándar {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}?</span>

    El servicio {{site.data.keyword.speechtotextshort}} se cobra a 0,02 $ (dólares) por minuto. Este precio se aplica al uso de los modelos de banda ancha y de banda estrecha. Para obtener más información, consulte la {{site.data.keyword.speechtotextshort}}[página de inicio del servicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}.

1.  <span style="color:#003F69">¿Qué significa "precio por minuto"? ¿Se me factura por el número de minutos de audio que envío al servicio o por el número de minutos que tarda el servicio en procesar el audio?</span>

    El precio se basa en la cantidad de audio que se envía al servicio, no en el tiempo que tarda el servicio en procesar el audio.

1.  <span style="color:#003F69">¿Se redondea al minuto más cercano para cada llamada a la API? Por ejemplo, si envío dos archivos de audio de 30 segundos de duración cada uno, ¿se me facturan dos minutos o un minuto?</span>

    {{site.data.keyword.IBM_notm}} no redondee la duración del audio de cada llamada de API que recibe el servicio. {{site.data.keyword.IBM_notm}} suma el uso del mes y lo redondea al minuto más cercano al final de mes. En este ejemplo, si envía dos archivos de audio de 30 segundos cada uno, {{site.data.keyword.IBM_notm}} suma la duración total del audio, un minuto, y factura 0,02 $ (dólares).

1.  <span id="graduated" style="color:#003F69">¿Cómo funciona el Precio gradual por niveles?</span>

    El objetivo del modelo de precio por niveles es ofrecer a los usuarios con un gran volumen de uso mayores descuentos a medida que utilizan el servicio. El precio por minuto desciende para los minutos adicionales de audio cuando se alcanzan determinados umbrales de audio mensual total. Para obtener más información, consulte la [página de precios ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window} del servicio.

1.  <span style="color:#003F69">En el caso de precios por niveles, ¿cuál será el importe total de mi factura si he utilizado el servicio para transcribir, por ejemplo, 275.000 minutos de audio en un mes?</span>

    -   Para los primeros 250.000 minutos de audio, se le cobrará 0,02 $ (dólares) / minuto: 250.000 * 0,02 $ = 5000 $ (dólares).
    -   Para los 25.000 minutos de audio restantes, se le cobrará una tasa reducida de 0,015 $ (dólares) / minuto: 25.000 * 0,015 $ = 375 $ (dólares).
    -   En este caso, el importe total facturado del mes sería 5375 $ (dólares).

1.  <span style="color:#003F69">¿Cuánto cuesta utilizar la interfaz de personalización del servicio? ¿Se me factura por crear y almacenar un modelo personalizado?</span>

    En el caso de la *personalización del modelo de lenguaje*, {{site.data.keyword.IBM_notm}} no cobra por crear o alojar un modelo de lenguaje personalizado, solo por utilizar el modelo con una solicitud de reconocimiento. El uso de un modelo de lenguaje personalizado para una transcripción supone un cargo adicional de 0,03 $ (dólares) por minuto. Este cargo se suma al cargo de uso estándar de 0,02 $ (dólares) por minuto. Se aplica a todos los idiomas admitidos por la interfaz de personalización. De modo que el coste total de utilizar un modelo de lenguaje personalizado para el reconocimiento de voz es de 0,05 $ (dólares) por minuto.

    En el caso de la *personalización del modelo acústico*, que es una interfaz beta para todos los idiomas admitidos, {{site.data.keyword.IBM_notm}} no cobra por crear, alojar o reconocer voz con un modelo acústico personalizado. El uso gratuito de modelos acústicos personalizados para solicitudes de reconocimiento está sujeto a cambios en el futuro.
