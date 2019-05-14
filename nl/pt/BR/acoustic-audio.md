---

copyright:
  years: 2019
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

# Gerenciando recursos de áudio
{: #manageAudio}

A interface de customização inclui o método `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`, que é usado para incluir um recurso de áudio em um modelo acústico customizado. Para obter mais informações, consulte [Incluir áudio no modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html#addAudio)). A interface também inclui os métodos a seguir para listar e excluir recursos de áudio para um modelo acústico customizado.
{: shortdesc}

## Listando recursos de áudio para um modelo acústico customizado
{: #listAudio}

A interface de customização fornece dois métodos para listar informações sobre os recursos de áudio de um modelo acústico customizado:

-   O método `GET /v1/acoustic_customizations/{customization_id}/audio` lista informações sobre todos os recursos de áudio que foram incluídos em um modelo customizado.
-   O método `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` lista informações sobre um recurso de áudio especificado para um modelo customizado.

Os dois métodos retornam o `name` do recurso de áudio mais as informações adicionais a seguir:

-   `duration` é o comprimento, em segundos, do áudio. Para um archive, a figura representa a duração acumulada de todos os arquivos de áudio que estão contidos no archive.
-   `details` identifica o tipo do recurso: `audio` ou `archive`. (O tipo será `undetermined` se o serviço não puder validar o recurso, possivelmente porque o usuário passou erroneamente um arquivo que não contém áudio.) Para um arquivo de áudio, os detalhes incluem o `codec` e a `frequency` do áudio. Para um archive, eles incluem seu tipo de `compression`.

Além disso, listar todos os recursos de áudio para um modelo retorna o `total_minutes_of_audio` somado sobre todos os recursos de áudio válidos para o modelo. É possível usar esse valor para determinar se o modelo customizado tem áudio suficiente ou em excesso para iniciar o treinamento.

Os métodos também listam o status dos dados de áudio. O status é importante para verificar a análise do serviço de arquivos de áudio em resposta a uma solicitação para incluí-los em um modelo customizado:

-   `ok` indica que o serviço analisou com êxito os dados de áudio. Os dados podem ser usados para treinar o modelo customizado.
-   `being_processed` indica que o serviço ainda está analisando os dados de áudio. O serviço não pode aceitar solicitações para incluir um novo áudio ou para treinar o modelo customizado até que sua análise seja concluída.
-   `invalid` indica que os dados de áudio não são válidos para o treinamento do modelo (possivelmente porque o formato ou a taxa de amostragem está errada ou porque está corrompido).

### Solicitação de exemplo: Listar todos os recursos de áudio
{: #listExample-audio}

O exemplo a seguir lista todos os recursos de áudio para o modelo acústico customizado com o ID de customização especificado. O modelo acústico tem três recursos de áudio. O serviço analisou `audio1` e `audio2` com êxito. Ele ainda está analisando `audio3`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### Solicitação de exemplo: Obter informações para um recurso de tipo áudio
{: #getExampleAudio}

O exemplo a seguir retorna informações sobre o recurso de tipo de áudio denominado `audio1`. O recurso tem 131 segundos de comprimento e é codificado com o codec `pcm_s16le`. Ele foi incluído com êxito no modelo.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

```javascript
{
  "duration": 131,
  "name": "audio1",
  "details": {
    "codec": "pcm_s16le",
    "type": "audio",
    "frequency": 22050
  }
  "status": "ok"
}
```
{: codeblock}

### Solicitação de exemplo: Obter informações para um recurso de tipo archive
{: #getExampleArchive}

O exemplo a seguir retorna as informações sobre o recurso de tipo archive denominado `audio2`. O recurso é um arquivo **.zip** que contém mais de 9 minutos de áudio. Ele também foi incluído com êxito no modelo. Como o exemplo mostra, consultar informações sobre um recurso de tipo archive também fornece informações sobre os arquivos que ele contém.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

```javascript
{
  "container": {
    "duration": 556,
    "name": "audio2",
    "details": {
      "type": "archive",
      "compression": "zip"
    },
    "status": "ok"
  },
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## Excluindo um recurso de áudio de um modelo acústico customizado
{: #deleteAudio}

Use o método `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para remover um recurso de áudio existente de um modelo acústico customizado. Quando você exclui um recurso de áudio do tipo archive, o serviço remove o archive inteiro de arquivos. A interface atual não permite a exclusão de arquivos individuais por meio de um recurso de archive.

A remoção de um recurso de áudio não afeta o modelo customizado até que você treine o modelo em seus dados atualizados usando o método `POST /v1/acoustic_customizations/{customization_id}/train`. Se você treinou com êxito o modelo no recurso, os dados de áudio existentes continuarão a ser usados para reconhecimento de voz até você retreinar o modelo.

### Solicitação de exemplo
{: #deleteExample-audio}

O método a seguir exclui o recurso de áudio que é denominado `audio3` do modelo customizado com o ID de customização especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
