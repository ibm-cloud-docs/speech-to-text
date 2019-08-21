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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Guía de aprendizaje de iniciación
{: #gettingStarted}

El servicio {{site.data.keyword.speechtotextfull}} transcribe audio a texto para habilitar las prestaciones de transcripción de voz para las aplicaciones. Esta guía de aprendizaje basada en curl puede ayudarle a comenzar a utilizar rápidamente el servicio. En los ejemplos se muestra cómo llamar al método `POST /v1/recognize` del servicio para solicitar una transcripción.
{: shortdesc}

## Antes de empezar
{: #before-you-begin}

- {: hide-dashboard}  Cree una instancia del servicio:
    1.  {: hide-dashboard} Vaya a la página [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} en el catálogo de {{site.data.keyword.cloud_notm}}.
    1.  {: hide-dashboard} Regístrese para obtener una cuenta gratuita de {{site.data.keyword.cloud_notm}} o inicie una sesión.
    1.  {: hide-dashboard} Pulse **Crear**.
-   Copie las credenciales para autenticarse con la instancia de servicio:
    1.  {: hide-dashboard} Desde la [Lista de recursos de {{site.data.keyword.cloud_notm}}](https://{DomainName}/resources){: external}, pulse la instancia del servicio {{site.data.keyword.speechtotextshort}} para ir a la página del panel de control del servicio {{site.data.keyword.speechtotextshort}}.
    1.  En la página **Gestionar**, pulse **Mostrar** para visualizar las credenciales.
    1.  Copie los valores de `Clave de API` y `URL`.

### Utilización de los ejemplos de curl
{: #getting-started-curl}

En esta guía de aprendizaje se utiliza el mandato `curl` para llamar a métodos de la interfaz HTTP de servicio. Asegúrese de tener instalado el mandato `curl` en el sistema.

1.  Para comprobar si está instalado el mandato `curl`, ejecute el mandato siguiente en la línea de mandatos. Si en la salida se muestra que la versión de `curl` admite la capa de sockets seguros (SSL - SSL Secure Sockets Layer), estará listo para empezar a utilizar esta guía de aprendizaje.

    ```bash
    curl -V
    ```
    {: pre}

1.  Si es necesario, instale la versión de `curl` con la opción SSL habilitada para su sistema operativo desde [curl.haxx.se](https://curl.haxx.se/){: external}.

Omita las llaves de los ejemplos porque indican valores de variables.
{: tip}

## Paso 1: Transcribir audio sin opciones
{: #transcribe}

Llame al método `POST /v1/recognize` para solicitar una transcripción básica de un archivo de audio FLAC sin parámetros de solicitud adicionales.

1.  Descargue el archivo de audio de ejemplo <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
1.  Emita el mandato siguiente para llamar al método `/v1/recognize` del servicio para la transcripción básica sin parámetros. En el ejemplo se utiliza la cabecera `Content-Type` para indicar el tipo de audio, `audio/flac`. En el ejemplo se utiliza el modelo de lenguaje predeterminado, `en-US_BroadbandModel`, para la transcripción.
    -   {: hide-dashboard} Sustituya `{apikey}` y `{url}` por su clave de API y su URL.
    -   Modifique `{path_to_file}` para especificar la ubicación del archivo `audio-file.flac`.

    *Usuario de Windows:* sustituya la barra inclinada invertida (``\`) al final de cada línea por un acento circunflejo (``^`). Asegúrese de que no haya espacios finales.
    {: tip}

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    El servicio devuelve los siguientes resultados de la transcripción:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Paso 2: Transcribir audio con opciones
{: #transcribeOptions}

Llame al método `POST /v1/recognize` para transcribir el mismo archivo de audio FLAC, pero especifique dos parámetros de transcripción.

1.  Si es necesario, descargue el archivo de audio de ejemplo <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
1.  Emita el mandato siguiente para llamar al método `/v1/recognize` del servicio con dos parámetros adicionales. Establezca el parámetro `timestamps` en `true` para indicar el principio y el final de cada palabra en la secuencia de audio. Establezca el parámetro `max_alternatives` en `3` para recibir las tres alternativas más probables para la transcripción. En el ejemplo se utiliza la cabecera `Content-Type` para indicar el tipo de audio, `audio/flac`, y la solicitud utiliza el modelo predeterminado, `en-US_BroadbandModel`.
    -   {: hide-dashboard} Sustituya `{apikey}` y `{url}` por su clave de API y su URL.
    -   Modifique `{path_to_file}` para especificar la ubicación del archivo `audio-file.flac`.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    El servicio devuelve los siguientes resultados, que incluyen indicaciones de fecha y hora y tres transcripciones alternativas:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                ["tornadoes":, 1.51, 2.15],
                ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado and Sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Siguientes pasos

-   Obtenga más información sobre las interfaces y los SDK que están disponibles para realizar solicitudes de reconocimiento de voz en el apartado [Visión general para los desarrolladores](/docs/services/speech-to-text?topic=speech-to-text-developerOverview).
-   Consulte las solicitudes básicas de reconocimiento de voz para cada una de las interfaces del servicio en [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Busque la información detallada sobre todos los métodos de las interfaces del servicio en la [Referencia de API](https://{DomainName}/apidocs/speech-to-text){: external}.
