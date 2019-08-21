---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

subcollection: speech-to-text

---

{:faq: .faq}
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

# Preguntas frecuentes sobre precios
{: #faq-pricing}

## ¿Cuánto cuesta utilizar el plan Lite de {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-zero}
{: faq}

El plan Lite le permite empezar con 500 minutos de reconocimiento de voz al mes sin coste alguno. El plan Lite no proporciona acceso a las interfaces de personalización del servicio. Para obtener más información, consulte el servicio {{site.data.keyword.speechtotextshort}} en la [página de precios](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}.

## ¿Cuánto cuesta utilizar el plan Standard de {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-one}
{: faq}

El plan de precios Standard se cobra a 0,02$ (dólares americanos) por minuto de habla que se reconozca. Este precio se aplica al uso de los modelos de banda ancha y de banda estrecha. El plan utiliza un modelo de precio por niveles y le permite utilizar las interfaces de personalización con un cargo adicional. Para obtener más información, consulte el servicio {{site.data.keyword.speechtotextshort}} en la [página de precios](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}.

## ¿Qué significa "precio por minuto"?
{: #faq-pricing-two}
{: faq}

El precio se basa en la cantidad (número de minutos) de audio que se envía al servicio. El precio no depende de lo que tarde el servicio en procesar el audio.

## ¿Se redondea al minuto más cercano para cada llamada a la API?
{: #faq-pricing-three}
{: faq}

{{site.data.keyword.IBM_notm}} no redondee la duración del audio de cada llamada de API que recibe el servicio. {{site.data.keyword.IBM_notm}} suma el uso del mes y lo redondea al minuto más cercano al final de mes. Por ejemplo, si envía dos archivos de audio de 30 segundos cada uno, {{site.data.keyword.IBM_notm}} suma la duración total del audio, un minuto, y factura 0,02 $ (dólares americanos).

## ¿Cómo funciona el Precio gradual por niveles?
{: #faq-pricing-four}
{: faq}

El objetivo del modelo de precio por niveles es ofrecer a los usuarios con un gran volumen de uso mayores descuentos a medida que utilizan el servicio. El precio por minuto desciende para los minutos adicionales de audio cuando se alcanzan determinados umbrales de audio mensual total. Para obtener más información, consulte el servicio [página de precios](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}.

## En el caso de precios por niveles, ¿cuál será el importe total de mi factura si he utilizado el servicio para transcribir, por ejemplo, 275.000 minutos de audio en un mes?
{: #faq-pricing-five}
{: faq}

Para los primeros 250.000 minutos de audio, se le cobrará 0,02$ (dólares americanos) / minuto: 250.000 \* 0,02 = 5000,00$ (dólares americanos). Para los 25.000 minutos de audio restantes, se le cobrará una tasa reducida de 0,015 $ (dólares americanos) / minuto: 25.000 \* 0,015 $ = 375 $ (dólares americanos). En este caso, el importe total facturado del mes sería 5375 $ (dólares americanos).

## ¿Qué plan de precios necesito para utilizar la interfaz de personalización del servicio?
{: #faq-pricing-six}
{: faq}

Debe tener el plan de fijación de precios estándar para utilizar el modelo de lenguaje o la personalización del modelo acústico. Los usuarios del plan Lite no pueden utilizar la interfaz de personalización. Para obtener más información, consulte el servicio {{site.data.keyword.speechtotextshort}} en la [página de precios](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}.

## ¿Cuánto cuesta utilizar la interfaz de personalización del servicio?
{: #faq-pricing-seven}
{: faq}

En el caso de la *personalización del modelo de lenguaje*, {{site.data.keyword.IBM_notm}} no cobra por crear o alojar un modelo de lenguaje personalizado, solo por utilizar el modelo con una solicitud de reconocimiento. El uso de un modelo de lenguaje personalizado para una transcripción supone un cargo adicional de 0,03 $ (dólares americanos) por minuto. Este cargo se suma al cargo de uso estándar de 0,02 $ (dólares americanos) por minuto. Se aplica a todos los idiomas admitidos por la interfaz de personalización. De modo que el coste total de utilizar un modelo de lenguaje personalizado para el reconocimiento de voz es de 0,05 $ (dólares americanos) por minuto.

En el caso de la *personalización del modelo acústico*, que es una interfaz beta para todos los idiomas admitidos, {{site.data.keyword.IBM_notm}} no cobra por crear, alojar o reconocer voz con un modelo acústico personalizado. El uso gratuito de modelos acústicos personalizados para solicitudes de reconocimiento está sujeto a cambios en el futuro.
