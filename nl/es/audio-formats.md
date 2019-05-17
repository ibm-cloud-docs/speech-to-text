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

# Formatos de audio
{: #audio-formats}

El servicio {{site.data.keyword.speechtotextfull}} puede extraer voz del audio en muchos formatos diferentes.

-   Si no está familiarizado con el audio y sobre cómo se describe y se especifica, empiece por [Características y terminología de audio](#terminology) para ayudarle a empezar.
-   Si ya sabe cómo utilizar el audio, salte a [Formatos de audio soportados](#formats) para obtener información detallada acerca de los formatos a los que da soporte el servicio.

Las secciones finales, [Límites de datos y compresión](#limits), [Conversión de audio](#conversion) y [Consejos para mejorar el reconocimiento de voz](#audioTips), pueden ayudarle a sacar el máximo partido del servicio.

## Características y terminología de audio
{: #terminology}

Se utiliza la siguiente terminología para describir las características de los datos de audio en los distintos formatos.

### Frecuencia de muestreo
{: #samplingRate}

La *frecuencia de muestreo* es el número de muestras de audio que se toman por segundo. La frecuencia de muestreo se mide en hercios (Hz) o en kilohercios (kHz). Por ejemplo, una frecuencia de 16.000 muestras por segundo es igual a 16.000 Hz (o 16 kHz). Con el {{site.data.keyword.speechtotextshort}} servicio, se especifica un modelo para indicar la frecuencia de muestreo del audio:

-   Los modelos de *banda ancha* se utilizan para el audio que se muestrea a 16 kHz o más, que {{site.data.keyword.IBM}} recomienda para aplicaciones en tiempo real (por ejemplo, para aplicaciones de voz en directo).
-   Los modelos de *banda estrecha* se utilizan para el audio que se muestrea a 8 kHz o más, que es la frecuencia que se suele utilizar para audio telefónico.

El servicio da soporte a audio de banda ancha y de banda estrecha para la mayoría de idiomas y formatos. Ajusta automáticamente la frecuencia de muestreo de su audio para que se adapte al modelo que especifique antes de realizar el reconocimiento de voz.

-   En el caso de los modelos de banda ancha, el servicio convierte el audio registrado a frecuencias de muestreo más altas a 16 kHz.
-   En el caso de los modelos de banda estrecha, convierte el audio a 8 kHz.

En teoría, puedes enviar un audio de 44 kHz con un modelo de banda ancha o de banda estrecha, pero eso incrementa innecesariamente el tamaño del audio. Para maximizar la cantidad de audio que puede enviar, haga coincidir la frecuencia de muestreo de su audio con el modelo que utiliza. El servicio no acepta el audio que se muestrea a una frecuencia menor que la frecuencia de muestreo intrínseca del modelo.

#### Notas sobre los formatos de audio
{: #samplingRateNotes}

-   Para los formatos `audio/alaw`, `audio/l16` y `audio/mulaw`, debe especificar la frecuencia del audio.
-   Para los formatos `audio/basic` y `audio/g729`, el servicio solo da soporte a audio de banda estrecha.

#### Más información
{: #samplingRateMore}

-   Para obtener más información sobre las frecuencias de muestreo, consulte [en.wikipedia.org/wiki/Sampling (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Sampling){: new_window}. Seleccione *Muestreo en la música*.
-   Para obtener más información acerca de los modelos que ofrece el servicio para cada idioma soportado, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).

### Velocidad en bits
{: #bitRate}

La *velocidad en bits* es el número de bits de datos que se envían por segundo. La velocidad en bits de una secuencia de audio se mide en kilobits por segundo (kbps). La velocidad en bits se calcula a partir de la frecuencia de muestreo y del número de bits almacenados por muestra. Para el reconocimiento de voz, {{site.data.keyword.IBM}} recomienda registrar 16 bits por muestra para el audio.

Por ejemplo, un audio que utilice una frecuencia de muestreo de banda ancha de 16 kHz y 16 bits por muestra tiene una velocidad en bits de 256 kbps: `(16.000 * 16) / 1000`.

#### Más información
{: #bitRateMore}

-   Para obtener más información sobre las velocidades en bits, consulte [en.wikipedia.org/wiki/Bit_rate (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}.
-   Para ver un debate general sobre las frecuencias de muestreo y las velocidades en bits, consulte [¿Qué son las velocidades en bits? ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} y [Selección de velocidades en bits para podcasts ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window}.

### Compresión
{: #compression}

La *compresión* se utiliza en muchos formatos de audio para reducir el tamaño de los datos de audio. La compresión reduce el número de bits almacenados por muestra y, por lo tanto, la velocidad en bits. Algunos formatos no utilizan compresión, pero la mayoría utilizan uno de los tipos básicos de compresión:

-   La compresión *sin pérdida (lossless)* reduce el tamaño del audio sin pérdida de calidad, pero la tasa de compresión suele ser baja.
-   La compresión *con pérdida (lossy)* reduce el tamaño del audio hasta 10 veces, pero algunos datos y cierta calidad se pierden de forma irrecuperable en la compresión.

Con el servicio {{site.data.keyword.speechtotextshort}}, puede utilizar la compresión con pérdida de forma segura para maximizar la cantidad de audio que puede enviar al servicio con una solicitud de reconocimiento. Debido a que el rango dinámico de la voz humana es más limitado que, digamos, el de la música, la voz puede acomodar una velocidad en bits mucho más baja que otros tipos de audio. Para el reconocimiento de voz, {{site.data.keyword.IBM}} recomienda utilizar 16 bits por muestra para el audio y utilizar un formato que comprima los datos de audio.

#### Notas sobre los formatos de audio
{: #compressionNotes}

-   Los formatos `audio/ogg` y `audio/webm` son contenedores cuya compresión se basa en el codec que se utiliza para codificar los datos: Opus o Vorbis.
-   El formato `audio/wav` es un contenedor que puede incluir datos descomprimidos, comprimidos sin pérdida o comprimidos con pérdida.

#### Más información
{: #compressionMore}

-   Para obtener más información sobre la compresión de audio, consulte [en.wikipedia.org/wiki/Data_compression#Audio (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}.
-   Para obtener más información sobre el uso de la compresión de datos para aumentar la cantidad de audio que puede enviar con una solicitud, consulte [Límites de datos y compresión](#limits).

### Canales
{: #channels}

Los *canales* son el número de secuencias del audio grabado:

-   El audio *monoaural* (o mono) solo tiene un canal.
-   El audio *estereofónico* (o estéreo) suele tener dos canales.

El servicio {{site.data.keyword.speechtotextshort}} acepta audio con un máximo de 16 canales. Puesto que solo utiliza un canal para el reconocimiento de voz, el servicio combina el audio con múltiples canales en uno mono de un solo canal durante la transcodificación.

#### Notas sobre los formatos de audio
{: #channelsNotes}

-   Para el formato de `audio/l16`, debe especificar el número de canales si el audio tiene más de un canal.
-   Para el formato `audio/wav`, el servicio acepta audio con un máximo de nueve canales.

#### Más información
{: #channelsMore}

-   Para obtener más información sobre los canales de audio, consulte [en.wikipedia.org/wiki/Audio_signal (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}.

### Endianness
{: #endianness}

*Endianness* indica cómo organiza la arquitectura subyacente del sistema los bytes de datos:

-   *Big-endian* ordena los datos comenzando por el bit más significativo.
-   *Little-endian* ordena los datos comenzando por el bit menos significativo.

El servicio {{site.data.keyword.speechtotextshort}} detecta automáticamente la característica endianness del audio de entrada.

#### Notas sobre los formatos de audio
{: #endiannessNotes}

-   En el caso del formato `audio/l16`, puede especificar la característica endianness para inhabilitar la detección automática si es necesario.

#### Más información
{: #endiannessMore}

-   Para obtener más información sobre la característica endianness, consulte [en.wikipedia.org/wiki/Endianness (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Endianness){: new_window}.

### Frecuencia de audio
{: #frequency}

La *frecuencia de audio* es el rango de frecuencias audibles del audio. La frecuencia audible estándar aceptada para los seres humanos está entre 20 y 20.000 Hz. Puede utilizar análisis espectrográficos para generar un espectrograma que muestre el contenido de la frecuencia del audio.

La frecuencia de muestreo que se aplica al audio suele ser el doble de la frecuencia máxima del audio. Por ejemplo, una frecuencia de muestreo de 16 kHz significa que la frecuencia máxima de la señal de audio muestreada es de 8 kHz. Los modelos del servicio se han creado teniendo esto en cuenta.

-   Los modelos de banda estrecha se crean con audio muestreado a 8 kHz. Los modelos de banda estrecha esperan encontrar información en un rango que sea inferior o igual a 4 kHz.
-   Los modelos de banda ancha se crean con audio muestreado a 16 kHz. Los modelos de banda ancha esperan encontrar información en el rango de 4-8 kHz.

Los datos de entrenamiento de los modelos proceden de diferentes canales (telefonía para los modelos de banda estrecha). Los modelos reflejan las características de los canales en los que se han entrenado.

#### Aumento de la frecuencia de muestreo (Upsampling)
{: #upsampling}

El *aumento de la frecuencia de muestreo o upsampling* aumenta la frecuencia de muestreo del audio, pero no incorpora información nueva en el audio. Genera una aproximación de la señal de audio que se habría obtenido mediante el muestreo del audio a un ritmo más alto. Aumenta el tamaño de los datos de audio.

La información del audio muestreado originalmente a una frecuencia de banda estrecha está limitada a un rango de 0-4 kHz. Si se aumenta la frecuencia de muestreo del audio de banda estrecha por una frecuencia de muestreo superior, es improbable que se mejore la precisión del reconocimiento de voz. Si se aumenta la frecuencia de muestreo del audio de banda estrecha, en el rango falta información que esperan los modelos de banda ancha. Además, la información que se encuentra en el rango esperado de una muestra de banda estrecha es cualitativamente diferente de la información que se encuentra en el mismo rango de una muestra de banda ancha. Por lo tanto, el aumento de la frecuencia de muestreo en realidad da como resultado una degradación de la precisión del reconocimiento.

Para una frecuencia de muestreo de banda ancha de 16 kHz, se espera que la frecuencia máxima presente en la señal de audio muestreada sea de 8 kHz. Por lo tanto, debe filtrar la señal original a 8 kHz antes de muestrearla con una velocidad de 16 kHz. De lo contrario, se produce una degradación debido a un fenómeno denominado *aliasing*. Para comprender por qué sucede, consulte [Frecuencia Nyquist ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}.

Una comparación útil sería imaginarse ver una cinta de VHS en una gran pantalla plana HDTV. La imagen se vería borrosa, porque reproducir una cinta en un dispositivo de alta definición no añade realmente nueva información a la secuencia. Simplemente hace que el formato sea compatible con el dispositivo mejor. Esto es lo que ocurre con el aumento de la frecuencia de muestreo.

#### Reducción de la frecuencia de muestreo (Downsampling)
{: #downsampling}

La *reducción de la frecuencia de muestreo o downsampling* reduce la frecuencia de muestreo del audio. Genera una aproximación de la señal de audio que se habría obtenido mediante el muestreo del audio a un ritmo más bajo. La reducción de la frecuencia de muestreo no elimina información de la señal de audio, pero sí que reduce el tamaño de los datos de audio.

La reducción de la frecuencia de muestreo del puede resultar efectiva en algunos casos. Por ejemplo, si la frecuencia de muestreo de su audio es superior a 8 kHz *y* un examen espectrográfico revela que no hay contenido de frecuencia superior a 4 kHz, considere la posibilidad de reducir la frecuencia de muestreo del audio a 8 kHz.

#### Más información
{: #frequencyMore}

-   Para obtener más información sobre la frecuencia de audio, consulte [https://en.wikipedia.org/wiki/Audio_frequency (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Audio_frequency){: new_window}.
-   Para obtener más información el aumento de la frecuencia de muestreo, consulte [https://en.wikipedia.org/wiki/Upsampling (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Upsampling){: new_window}.
-   Para obtener más información sobre la reducción de la frecuencia de muestreo, consulte [https://en.wikipedia.org/wiki/Downsampling (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Downsampling_%28signal_processing%29){: new_window}.

## Formatos de audio soportados
{: #formats}

La Tabla 1 contiene un resumen de los formatos de audio a los que da soporte el servicio.

-   *Formato de audio y compresión* identifica cada formato e indica su compresión soportada. Puede enviar un máximo de 100 MB de audio al servicio con una sola solicitud HTTP síncrona o WebSocket. Si utiliza un formato que dé soporte a la compresión, puede reducir el tamaño de su audio para maximizar la cantidad de datos que puede pasar al servicio. Para obtener más información, consulte [Límites de datos y compresión](#limits).
-   *Especificación Content-type* indica que debe utilizar la cabecera `Content-Type` o un parámetro equivalente para especificar el formato (tipo de MIME) del audio que envía al servicio. Puede especificar el formato de audio para cualquier solicitud, pero eso no siempre es necesario:
    -   Para la mayoría de los formatos, el tipo de contenido es opcional. Puede omitir el tipo de contenido o especificar `application/octet-stream` para que el servicio detecte el formato automáticamente.
    -   Para otros, el tipo de contenido es obligatorio. Estos formatos no proporcionan la información, como la frecuencia de muestreo, que el servicio necesita para detectar su formato automáticamente.
-   En las columnas finales se identifican *Parámetros obligatorios* y *Parámetros opcionales* adicionales para cada formato. Las siguientes secciones contienen más información sobre estos parámetros.

<table>
  <caption>Tabla 1. Resumen de los formatos de audio soportados</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      Formato de audio<br>y compresión
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Especificación de<br/>Content-type
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Parámetros<br/>obligatorios
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Parámetros<br/>opcionales
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Obligatorio
    </td>
    <td style="text-align:center">
      <code>rate={entero}</code>
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Obligatorio
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>Ninguno
    </td>
    <td style="text-align:center">
      Obligatorio
    </td>
    <td style="text-align:center">
      <code>rate={entero}</code>
    </td>
    <td style="text-align:center">
      <code>channels={entero}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Obligatorio
    </td>
    <td style="text-align:center">
      <code>rate={entero}</code>
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>Ninguno, sin pérdida <br/>o con pérdida
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>Con pérdida
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Ninguno
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

Si utiliza el mandato `curl` para realizar una solicitud de reconocimiento de voz con las interfaces HTTP, debe especificar el formato de audio con la cabecera `Content-Type`, especificar `"Content-Type: application/octet-stream"` o especificar `"Content-Type:"`. Si omite la cabecera por completo, `curl` utiliza el valor predeterminado `application/x-www-form-urlencoded`.
{: important}

### Formato audio/alaw
{: #alaw}

*A-law* (`audio/alaw`) es un formato de audio de un solo canal con pérdida. Utiliza un algoritmo parecido al algoritmo de u-law que aplican los formatos `audio/basic` y `audio/mulaw`, aunque el algoritmo A-law genera distintas características de señal. Si se utiliza este formato, el servicio necesita un parámetro adicional en la especificación de formato.

<table>
  <caption>Tabla 2. Parámetro para el formato `audio/alaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parámetro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descripción
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obligatorio</em>
    </td>
    <td>
      Un número entero que especifica la frecuencia de muestreo con la que se captura el audio. Por ejemplo, especifique el siguiente parámetro para datos de audio que se capturan a 8 kHz:<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

Para obtener más información, consulte [en.wikipedia.org/wiki/A-law_algorithm (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/A-law_algorithm){: new_window}.

### Formato audio/basic
{: #basic}

*Basic audio* (`audio/basic`) es un formato de audio de un solo canal con pérdida que se codifica utilizando datos u-law (o mu-law) de 8 bits que se muestrean a 8 kHz. Este formato proporciona un denominador lowest-common que indica el tipo de soporte del audio. El servicio solo da soporte a la utilización de archivos en formato `audio/basic` con modelos de banda estrecha.

Para obtener más información, consulte Internet Engineering Task Force (IETF) [Request for Comment (RFC) 2046 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://tools.ietf.org/html/rfc2046){: new_window} e [iana.org/assignments/media-types/audio/basic ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}.

### Formato audio/flac
{: #flac}

*Free Lossless Audio Codec (FLAC)* (`audio/flac`) es un formato de audio sin pérdida. Para obtener más información, consulte [en.wikipedia.org/wiki/FLAC (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/FLAC){: new_window}.

### Formato audio/g729
{: #g729}

*G.729* (`audio/g729`) es un formato de audio con pérdida que da soporte a datos codificados a 8 kHz. El servicio solo da soporte a G.729 Anexo D, no al Anexo J. El servicio solo permite el uso de archivos en formato `audio/g729` con modelos de banda estrecha. Para obtener más información, consulte [en.wikipedia.org/wiki/G.729 (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/G.729){: new_window}.

### Formato audio/l16
{: #l16}

*Linear 16-bit Pulse-Code Modulation (PCM)* (`audio/l16`) es un formato de audio no comprimido. Utilice este formato para pasar un archivo PCM sin formato. El audio PCM lineal también se puede transportar dentro de un archivo Waveform Audio File Format (WAV) contenedor. Cuando se utiliza el formato `audio/l16`, el servicio acepta parámetros adicionales obligatorios y opcionales en la especificación del formato.

<table>
  <caption>Tabla 3. Parámetros para el formato `audio/l16`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parámetro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descripción
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obligatorio</em>
    </td>
    <td>
      Un número entero que especifica la frecuencia de muestreo con la que se captura el audio. Por ejemplo, especifique el siguiente parámetro para datos de audio que se capturan a 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>Opcional</em>
    </td>
    <td>
      De forma predeterminada, el servicio trata el audio como si tuviera un solo canal.
      <em>Si el audio tiene más de un canal,</em> debe especificar un número entero que identifique el número de canales. Por ejemplo, especifique el siguiente parámetro para datos de audio de dos canales que se capturan a 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      El servicio acepta un máximo de 16 canales. Combina el audio en un canal durante la transcodificación.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>Opcional</em>
    </td>
    <td>
      De forma predeterminada, el servicio detecta automáticamente el valor endianness del audio de entrada. Pero su detección automática a veces puede fallar y descartar la conexión para audios cortos en formato `audio/l16`. La especificación del valor endianness desactiva la detección automática. Especifique <code>big-endian</code> o <code>little-endian</code>. Por ejemplo, especifique el siguiente parámetro para datos de audio que se capturan a 16 kHz en formato little-endian:<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      La sección 5.1 de <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comment (RFC) 2045 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")</a> especifica el formato big-endian para datos <code>audio/l16</code>, pero mucha gente utiliza el formato little-endian.
    </td>
  </tr>
</table>

Para obtener más información, consulte IETF [Request for Comment (RFC) 2586 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://tools.ietf.org/html/rfc2586){: new_window} y [en.wikipedia.org/wiki/Pulse-code_modulation (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}.

### Formatos audio/mp3 y audio/mpeg
{: #mp3}

*MP3* (`audio/mp3`) o *Motion Picture Experts Group (MPEG)* (`audio/mpeg`) es un formato de audio con pérdida. (MP3 y MPEG son el mismo formato.) Para obtener más información, consulte [en.wikipedia.org/wiki/MP3 (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/MP3){: new_window}.

### Formato audio/mulaw
{: #mulaw}

*Mu-law* (`audio/mulaw`) es un formato de audio de un solo canal con pérdida. Los datos se codifican mediante el algoritmo u-law (o mu-law). El formato `audio/basic` es un formato equivalente que siempre se muestra a 8 kHz. Si se utiliza este formato, el servicio necesita un parámetro adicional en la especificación de formato.

<table>
  <caption>Tabla 4. Parámetro para el formato `audio/mulaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parámetro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descripción
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obligatorio</em>
    </td>
    <td>
      Un número entero que especifica la frecuencia de muestreo con la que se captura el audio. Por ejemplo, especifique el siguiente parámetro para datos de audio que se capturan a 8 kHz:<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

Para obtener más información, consulte [en.wikipedia.org/wiki/M-law_algorithm (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}.

### formato audio/ogg
{: #ogg}

*Ogg* (`audio/ogg`) es un formato de contenedor abierto mantenido por Xiph.org Foundation ([xiph.org/ogg ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.xiph.org/ogg){: new_window}). Puede utilizar secuencias de audio comprimidas con los siguientes codecs con pérdida:

-   *Opus* (`audio/ogg;codecs=opus`). Para obtener más información, consulte [opus-codec.org ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.opus-codec.org/){: new_window} y [en.wikipedia.org/wiki/Opus (audio format) (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Opus){: new_window}. En la página *Opus (formato de audio)*, examine detenidamente la sección sobre *Contenedores*.
-   *Vorbis* (`audio/ogg;codecs=vorbis`). Para obtener más información, consulte [xiph.org/vorbis ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://xiph.org/vorbis/){: new_window} y [en.wikipedia.org/wiki/Vorbis (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

Si omite el codec, el servicio lo detecta automáticamente a partir del audio de entrada. Opus es el codec recomendado; Internet Engineering Task Force (IETF) as [Request for Comment (RFC) 6716 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://tools.ietf.org/html/rfc6716){: new_window} lo indica como formato estándar.

### Formato audio/wav
{: #wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) es un formato de contenedor que se suele utilizar para secuencias de audio sin compresión, aunque también puede contener audio comprimido. El servicio da soporte a audio WAV que utilice cualquier codificación. Acepta audio WAV con un máximo de nueve canales (debido a una limitación de FFmpeg).

Para obtener más información sobre el formato WAV, consulte [en.wikipedia.org/wiki/WAV (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/WAV){: new_window}. Para obtener más información sobre cómo reducir el tamaño del audio WAV convirtiéndolo en el codec Opus, consulte [Conversión a audio/ogg con el codec Opus](#conversionOgg).

### Formato audio/webm
{: #webm}

*Web Media (WebM)* (`audio/webm`) es un formato de contenedor abierto mantenido por WebM project ([webmproject.org ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.webmproject.org/){: new_window}). Puede utilizar secuencias de audio comprimidas con los siguientes codecs con pérdida:

-   *Opus* (`audio/webm;codecs=opus`). Para obtener más información, consulte [opus-codec.org ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.opus-codec.org/){: new_window} y [en.wikipedia.org/wiki/Opus (audio format) (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Opus){: new_window}. En la página *Opus (formato de audio)*, examine detenidamente la sección sobre *Contenedores*.
-   *Vorbis* (`audio/webm;codecs=vorbis`). Para obtener más información, consulte [xiph.org/vorbis ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://xiph.org/vorbis/){: new_window} y [en.wikipedia.org/wiki/Vorbis (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

Si omite el codec, el servicio lo detecta automáticamente a partir del audio de entrada.

Para ver el código JavaScript que muestra cómo capturar audio de un micrófono en un navegador Chrome y codificarlo en una secuencia de datos WebM, consulte [jsbin.com/hedujihuqo/edit?js,console ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window}. El código no envía el audio capturado al servicio.
{: tip}

## Límites de datos y compresión
{: #limits}

El servicio acepta un máximo de 100 MB de datos de audio para la transcripción con una solicitud HTTP síncrona o WebSocket. Cuando reconozca secuencias de audio continuas largas o archivos grandes, siga los pasos siguientes para asegurarse de que el audio no supere el límite de datos de 100 MB:

-   Utilice una frecuencia de muestreo que no supere los 16 kHz (para modelos de banda ancha) o los 8 kHz (para modelos de banda estrecha), y utilice 16 bits por muestra. El servicio convierte el audio grabado con una frecuencia de muestreo mayor que el modelo de destino (16 kHz u 8 kHz) a la frecuencia del modelo. Por lo tanto, las frecuencias mayores no mejoran la precisión del reconocimiento, pero aumentan el tamaño de la secuencia de audio.
-   Codifique el audio en un formato que ofrezca compresión de datos. Al codificar los datos de forma más eficiente, puede enviar mucho más audio sin sobrepasar el límite de 100 MB. Los formatos de audio como `audio/ogg` y `audio/mp3` reducen significativamente el tamaño de la secuencia de audio. Puede utilizar estos formatos para enviar mayor cantidad de audio con una sola solicitud.

Si comprime demasiado el audio con el formato `audio/ogg`, puede perder una cierta precisión en el reconocimiento. Para ir sobre seguro, utilice una velocidad en bits de al menos 24 kbps para el audio.

### Comparación de tamaños de audio aproximados
{: #compareSizes}

Consulte el tamaño aproximado de la secuencia de datos que resulta de 2 horas de transmisión de voz continua que se muestrea a 16 kHz y a 16 bits por muestra:

-   Si los datos se codifican con el formato `audio/wav`, la secuencia de dos horas tiene un tamaño de 230 MB, muy por encima del límite de 100 MB del servicio.
-   Si los datos se codifican con el formato `audio/ogg`, la secuencia de dos horas tiene un tamaño de solo 23 MB, muy por debajo del límite del servicio.

En la tabla siguiente se muestra una aproximación de la duración máxima de audio que se puede enviar para el reconocimiento de voz con una solicitud HTTP síncrona o WebSocket en diferentes formatos. La duración tiene en cuenta el límite de 100 MB del servicio. Los valores reales pueden variar en función de la complejidad del audio y de la tasa de compresión alcanzada.

<table style="width:75%">
  <caption>Tabla 5. Duración máxima del audio en diferentes formatos</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      Formato de audio
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      Duración máxima del audio (aproximada)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 minutos</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 hora 40 minutos</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 horas 20 minutos</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 horas 40 minutos</td>
  </tr>
</table>

Los formatos `audio/ogg;codecs=opus` y `audio/webm;codecs=opus` normalmente son equivalentes, y sus tamaños son casi idénticos. Utilizan el mismo codec de forma interna; solo el formato de contenedor es distinto.

## Conversión de audio
{: #conversion}

Puede utilizar diversas herramientas para convertir el audio a un formato diferente. Las herramientas pueden resultar útiles si el audio está en un formato al que no da soporte el servicio o si está en un formato no comprimido o sin pérdida. En el último caso, puede convertir el audio a un formato con pérdida para reducir su tamaño.

Dispone de las siguientes herramientas gratuitas para convertir el audio de un formato a otro:

-   Sound eXchange (SoX) ([sox.sourceforge.net ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.audacityteam.org/){: new_window})
-   Para el formato Ogg con el codec Opus, **opus-tools** ([opus-codec.org/downloads/ ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://opus-codec.org/downloads/){: new_window})

Estas herramientas ofrecen soporte de varias plataformas para múltiples formatos de audio. Además, puede utilizar muchas de las herramientas para reproducir audio. No utilice las herramientas para infringir leyes de copyright.

### Conversión a audio/ogg con el codec Opus
{: #conversionOgg}

La herramienta [**opus-tools** ![>Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://opus-codec.org/downloads/){: new_window} incluye tres programas de utilidad de línea de mandatos para trabajar con audio Ogg en el codec Opus:

-   El programa de utilidad [**opusenc** ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} codifica audio procedente de formatos WAV, FLAC y otros en Ogg con el codec Opus. La página muestra cómo comprimir las secuencias de audio. La compresión resulta útil para pasar audio en tiempo real al servicio.
-   El programa de utilidad [**opusdec** ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} decodifica audio procedente del codec Opus en archivos PCM WAV sin comprimir.
-   El programa de utilidad [**opusinfo** ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} proporciona información sobre comprobación de calidez de archivos Opus.

Muchos usuarios envían archivos WAV para el reconocimiento de voz. Con el límite de 100 MB de datos del servicio para las solicitudes HTTP síncronas y WebSocket, el formato WAV reduce la cantidad de audio que se puede reconocer con una sola solicitud. El uso del mandato **opusenc** para convertir el audio en el formato `audio/ogg:codecs=opus` preferido puede aumentar en gran medida la cantidad de audio que se puede enviar con una solicitud de reconocimiento.

Por ejemplo, supongamos que tiene un archivo WAV de banda ancha sin comprimir (16 kHz) (**input.wav**) que utiliza 16 bits por muestra para una velocidad en bits de 256 kbps. El mandato siguiente convierte el audio en un archivo (**output.opus**) que utiliza el codec Opus:

```bash
opusenc input.wav output.opus
```
{: pre}

La conversión comprime el audio con un factor de cuatro y genera un archivo de salida con una velocidad en bits de 64 kbps. Sin embargo, de acuerdo con los [Valores recomendados de Opus ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window}, puede reducir de forma segura la velocidad en bits a 24 kbps y seguir manteniendo una banda completa para el audio de voz. Esta reducción comprime el audio con un factor de 10. El mandato siguiente utiliza la opción `--bitrate` para generar un archivo de salida con una velocidad en bits de 24 kbps:

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

La compresión con el programa de utilidad **opusenc** es rápida. La compresión se produce a una velocidad que es aproximadamente 100 veces más rápida que el tiempo real. Cuando termina, el mandato escribe la salida en la consola que proporciona detalles completos sobre su tiempo de ejecución y los datos de audio resultantes.

## Consejos para mejorar el reconocimiento de voz
{: #audioTips}

Los siguientes consejos le pueden ayudar a mejorar la calidad del reconocimiento de voz:

-   El modo en que se graba el audio puede marcar una gran diferencia en los resultados del servicio. El reconocimiento de voz puede ser muy sensible a la calidad del audio de entrada. Para obtener la mejor precisión posible, asegúrese de que la calidad del audio de entrada sea lo mejor posible.
    -   Utilice un micrófono cercano y orientado al habla (por ejemplo, unos auriculares) siempre que sea posible y ajuste la configuración del micrófono si es necesario. El servicio funciona mejor si se utilizan micrófonos profesionales para capturar el audio.
    -   Evite utilizar el micrófono incorporado del sistema. Los micrófonos que normalmente se instalan en dispositivos móviles y tabletas resultan a menudo inadecuados.
    -   Asegúrese de que los oradores estén cerca de los micrófonos. La precisión disminuye a medida que un orador se aleja del micrófono. A una distancia de tres metros, por ejemplo, al servicio le cuesta mucho generar resultados adecuados.
-   El reconocimiento de voz es sensible al ruido de fondo y a los matices de la voz humana.
    -   El ruido de un motor, el sonido de dispositivos en funcionamiento, el ruido de la calle y las conversaciones de fondo pueden reducir significativamente la precisión del reconocimiento.
    -   Los acentos regionales y las pronunciaciones diferentes también pueden reducir la precisión.

    Si el audio tiene estas características, tenga en cuenta la posibilidad de utilizar la personalización del modelo acústico para mejorar la precisión del reconocimiento de voz. Para obtener más información, consulte [La interfaz de personalización](/docs/services/speech-to-text/custom.html).
