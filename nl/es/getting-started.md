---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

En la guía de aprendizaje se utilizan claves de API de {{site.data.keyword.cloud}} Identity and Access Management (IAM) para la autenticación. Las instancias antiguas del servicio pueden seguir utilizando los valores `{username}` y `{password}` de sus credenciales de servicio de Cloud Foundry existentes para la autenticación. Utilice la autenticación que resulte adecuada para su instancia de servicio. Para obtener más información sobre el uso que hace el servicio de la autenticación de IAM, consulte la [actualización del servicio del 30 de octubre de 2018](/docs/services/speech-to-text/release-notes.html#October2018b) en las notas del release.
{: important}

## Antes de empezar
{: #before-you-begin}

- {: hide-dashboard}  Cree una instancia del servicio:
    1.  {: hide-dashboard} Vaya a la página [{{site.data.keyword.speechtotextshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window} en el catálogo de {{site.data.keyword.cloud_notm}}.
    1.  {: hide-dashboard} Regístrese para obtener una cuenta gratuita de {{site.data.keyword.cloud_notm}} o inicie una sesión.
    1.  {: hide-dashboard} Pulse **Crear**.
-   Copie las credenciales para autenticarse con la instancia de servicio:
    1.  {: hide-dashboard} En el [panel de control de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/dashboard/apps){: new_window}, pulse su instancia de servicio de {{site.data.keyword.speechtotextshort}} y vaya a la página del panel de control del servicio {{site.data.keyword.speechtotextshort}}.
    1.  En la página **Gestionar**, pulse **Mostrar** para visualizar las credenciales.
    1.  Copie los valores de `Clave de API` y `URL`.
-   Asegúrese de que tiene el mandato `curl`.
    -   En los ejemplos se utiliza el mandato `curl` para llamar a los métodos de la interfaz HTTP. Instale la versión correspondiente a su sistema operativo desde [curl.haxx.se ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://curl.haxx.se/){: new_window}. Instale la versión que da soporte al protocolo SSL (Secure Sockets Layer). Asegúrese de incluir el archivo binario instalado en la variable de entorno `PATH`.

Cuando especifique un mandato, sustituya `{apikey}` y `{url}` por su clave de API y su URL reales. Omita las llaves, que indican un valor variable, en el mandato. Un valor real se asemeja al del ejemplo siguiente:
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## Paso 1: Transcribir audio sin opciones
{: #transcribe}

Llame al método `POST /v1/recognize` para solicitar una transcripción básica de un archivo de audio FLAC sin parámetros de solicitud adicionales.

1.  Descargue el archivo de audio de ejemplo <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
1.  Emita el mandato siguiente para llamar al método `/v1/recognize` del servicio para la transcripción básica sin parámetros. En el ejemplo se utiliza la cabecera `Content-Type` para indicar el tipo de audio, `audio/flac`. En el ejemplo se utiliza el modelo de lenguaje predeterminado, `en-US_BroadbandModel`, para la transcripción.
    -   {: hide-dashboard} Sustituya `{apikey}` y `{url}` por su clave de API y su URL.
    -   Modifique `{path_to_file}` para especificar la ubicación del archivo `audio-file.flac`.

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
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through colorado on sunday "
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
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touch down is a line
of severe thunderstorms swept through colorado on sunday "
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

-   Obtenga más información sobre las interfaces y los SDK que están disponibles para realizar solicitudes de reconocimiento de voz en el apartado [Visión general para los desarrolladores](/docs/services/speech-to-text/developer-overview.html).
-   Consulte las solicitudes básicas de reconocimiento de voz para cada una de las interfaces del servicio en [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text/basic-request.html).
-   Consulte información detallada sobre todos los métodos de las interfaces del servicio en la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
