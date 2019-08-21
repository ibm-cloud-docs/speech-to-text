---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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

# La tecnología subyacente al servicio
{: #science}

Tal como describe [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external}, {{site.data.keyword.IBM}} ha estado en la vanguardia de la investigación sobre el reconocimiento de voz desde principios de los años sesenta. Por ejemplo, en [Bahl, Jelinek y Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) se describe el enfoque matemático básico del reconocimiento de voz que se emplea esencialmente en todos los sistemas de reconocimiento de voz modernos. Y en [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) se describe la creación del primer sistema de reconocimiento de voz con un vocabulario amplio en tiempo real. En este documento también se describen los problemas que actualmente siguen siendo temas de investigación sin resolver.
{: shortdesc}

{{site.data.keyword.IBM_notm}} continúa esta rica tradición de investigación y desarrollo con el servicio {{site.data.keyword.speechtotextfull}}. {{site.data.keyword.IBM_notm}} ha demostrado la precisión del reconocimiento de voz del sector en conjuntos de datos de referencia pública para la transcripción CTS (Conversational Telephone Speech) ([Saon et al., 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) y la BN (Broadcast News) ([Thomas et al., 2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)). {{site.data.keyword.IBM_notm}} ha optimizado las redes neuronales para el modelado de idiomas ([Kurata et al., 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a), y [Kurata et al., 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)), además de demostrar la eficacia del modelado acústico.

En los siguientes anuncios se resumen los logros recientes en cuanto al reconocimiento de voz de {{site.data.keyword.IBM_notm}}:

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

Estos logros contribuyen a la consecución de los servicios de habla de {{site.data.keyword.IBM_notm}}. Las ideas recientes que mejor se adaptan al servicio {{site.data.keyword.speechtotextshort}} basado en la nube son:

-   *Para el modelado de idiomas,* {{site.data.keyword.IBM_notm}} aprovecha un modelo de lenguaje neuronal basado en la red para generar el texto de entrenamiento ([Suzuki et al., 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *Para el modelado acústico,* {{site.data.keyword.IBM_notm}} utiliza un modelo bastante compacto para dar cabida a las limitaciones de recursos de la nube. Para entrenar el modelo compacto, {{site.data.keyword.IBM_notm}} utiliza el "entrenamiento profesor-alumno y la extracción de conocimientos". Las redes neuronales grandes y potentes como, por ejemplo, Long Short-Term Memory (LSTM), VGG y Residual Network (ResNet) se entrenan por primera vez. Las salidas de estas redes se utilizan luego como señales de maestro para entrenar un modelo compacto para el despliegue real ([Fukuda et al., 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)).

Para ir más allá, {{site.data.keyword.IBM_notm}} también se centra en el modelado de extremo a extremo. Por ejemplo, se ha establecido un conducto de modelado potente para los modelos directos de acústica a palabra ([Audhkhasi et al., 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017), y [Audhkhasi et al., 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) que ahora se está mejorando ([Saon et al., 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)). También se están haciendo esfuerzo en crear modelos compactos de extremo a extremo para un futuro despliegue en la nube ([Kurata y Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)).

Para obtener más información sobre la investigación científica detrás del servicio, consulte los documentos que aparecen en [Referencias de investigación](/docs/services/speech-to-text?topic=speech-to-text-references).
