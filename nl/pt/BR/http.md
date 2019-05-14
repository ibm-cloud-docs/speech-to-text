---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# A interface de HTTP síncrona
{: #http}

A interface de HTTP síncrona do serviço {{site.data.keyword.speechtotextfull}} fornece um único método `POST /v1/recognize` para solicitar reconhecimento de voz com o serviço. Esse método é o meio mais simples de obter uma transcrição. Ele oferece duas maneiras de enviar uma solicitação de reconhecimento de voz:
{: shortdesc}

-   A primeira envia todo o áudio em um único fluxo por meio do corpo da solicitação. Você especifica os parâmetros da operação como cabeçalhos da solicitação e parâmetros de consulta. Para obter mais informações, consulte [Fazendo uma solicitação de HTTP básica](#HTTP-basic).
-   A segunda envia o áudio como uma solicitação com múltiplas partes. Você especifica os parâmetros da solicitação como uma combinação de cabeçalhos da solicitação, parâmetros de consulta e metadados JSON. Para obter mais informações, consulte [Fazendo uma solicitação de HTTP com múltiplas partes](#HTTP-multi).

Envie um máximo de 100 MB e um mínimo de 100 bytes de dados de áudio com uma única solicitação. Para obter informações sobre formatos de áudio e sobre como usar compactação para maximizar a quantia de áudio que você pode enviar com uma solicitação, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html). Para obter informações sobre todos os métodos da interface de HTTP, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Fazendo uma solicitação de HTTP básica
{: #HTTP-basic}

O método HTTP `POST /v1/recognize` fornece um meio simples de transcrever áudio. Você passa todo o áudio por meio do corpo da solicitação e especifica os parâmetros como cabeçalhos da solicitação e parâmetros de consulta.

O exemplo a seguir `curl` envia uma solicitação de reconhecimento para um único arquivo FLAC denominado `audio-file.flac`. A solicitação omite o parâmetro de consulta `model` para usar o modelo de idioma padrão, `en-US_BroadbandModel`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

O exemplo retorna a transcrição a seguir para o áudio:

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

O método `POST /v1/recognize` retorna resultados somente depois que ele processa todo o áudio para uma solicitação. O método é apropriado para processamento em lote, mas não para reconhecimento de voz em tempo real. Use a interface do WebSocket para transcrever áudio em tempo real.

Se os seus dados consistem em múltiplos arquivos de áudio, o meio recomendado para enviar o áudio é enviando múltiplas solicitações, uma para cada arquivo de áudio. É possível enviar as solicitações em um loop, opcionalmente com paralelismo para melhorar o desempenho. Também é possível usar o reconhecimento de voz com múltiplas partes para passar múltiplos arquivos de áudio com uma única solicitação.

## Fazendo uma solicitação de HTTP com múltiplas partes
{: #HTTP-multi}

O método `POST /v1/recognize` também suporta solicitações com múltiplas partes. Você passa todos os dados de áudio como dados de formulário com múltiplas partes. Você especifica alguns parâmetros como cabeçalhos da solicitação e parâmetros de consulta, mas passa os metadados JSON como dados de formulário para controlar a maioria dos aspectos da transcrição.

O reconhecimento de voz com múltiplas partes é destinado para os casos de uso a seguir:

-   Para passar múltiplos arquivos de áudio com uma solicitação de reconhecimento de voz única.
-   Com navegadores para os quais o JavaScript está desativado. Solicitações com múltiplas partes baseadas nos dados de formulário não requerem o uso do JavaScript.
-   Quando os parâmetros de uma solicitação de reconhecimento são maiores que 8 KB, que é o limite imposto pela maioria dos servidores HTTP e proxies. Por exemplo, a marcação de um número muito grande de palavras-chave pode aumentar o tamanho de uma solicitação além desse limite. Solicitações com múltiplas partes usam dados de formulário para evitar essa restrição.

As seções a seguir descrevem os parâmetros que você usa para solicitações com múltiplas partes e mostram uma solicitação de exemplo.

### Parâmetros para solicitações com múltiplas partes
{: #multipartParameters}

Você especifica os seguintes parâmetros de reconhecimento de voz com múltiplas partes como cabeçalhos da solicitação, parâmetros de consulta e dados de formulário.

<table summary="Cada linha da tabela descreve o uso de um possível parâmetro para uma solicitação de reconhecimento com múltiplas partes.">
  <caption>Tabela 1. Parâmetros para solicitações com múltiplas partes</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">Parâmetros</th>
    <th id="description" style="text-align:center; width:80%">Descrição</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>Objeto</em>
      <br/><em>de dados de formulário</em>
    </td>
    <td>
      <em>Necessário.</em> Um objeto JSON que fornece os parâmetros de transcrição para a solicitação. O objeto deve ser a primeira parte dos dados de formulário. As informações descrevem o áudio nas partes subsequentes dos dados de formulário. Consulte [Metadados JSON para solicitações com múltiplas partes](#multipartJSON).
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>Arquivo dos</em>
      <br/><em>dados de formulário</em>
    </td>
    <td>
      <em>Necessário.</em> Um ou mais arquivos de áudio como o restante dos dados de formulário para a solicitação. Todos os arquivos de áudio devem ter o mesmo formato.
      Com o comando `curl`, inclua uma opção <code>--form</code> separada para
cada arquivo da solicitação.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Sequência do</em>
      <br/><em>cabeçalho</em>
    </td>
    <td>
      <em>Necessário.</em> Especifique `multipart/form-data` para indicar como os dados são passados para o método. Especifique o tipo de conteúdo do áudio com o parâmetro JSON `part_content_type`.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Sequência do</em>
      <br/><em>cabeçalho</em>
    </td>
    <td>
      <em>Opcional.</em> Especifique `chunked` para transmitir os dados de áudio para o serviço. Omita o parâmetro se você enviar todo o áudio com uma única solicitação.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Sequência de</em>
      <br/><em>consulta</em>
    </td>
    <td>
      <em>Opcional.</em> O identificador do modelo que deve ser usado com a solicitação. O padrão é `en-US_BroadbandModel`.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Sequência de</em>
      <br/><em>consulta</em>
    </td>
    <td>
      <em>Opcional.</em> O GUID de um modelo de idioma customizado que deve ser usado com a solicitação.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Sequência de</em>
      <br/><em>consulta</em>
    </td>
    <td>
      <em>Opcional.</em> O GUID de um modelo acústico customizado que deve ser usado com a solicitação.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Sequência de</em>
      <br/><em>consulta</em>
    </td>
    <td>
      <em>Opcional.</em> A versão do modelo base especificado que deve ser usada com a solicitação.
    </td>
  </tr>
</table>

Para obter mais informações sobre os parâmetros de consulta, consulte o [Resumo de parâmetro](/docs/services/speech-to-text/summary.html).

### Metadados JSON para solicitações com múltiplas partes
{: #multipartJSON}

Os metadados do JSON que você transmite com uma solicitação com múltiplas partes podem incluir os campos a seguir:

-   `part_content_type` (sequência)
-   `data_parts_count` (número inteiro)
-   `customization_weight` (número)
-   `inactivity_timeout` (número inteiro)
-   `keywords` (sequência[])
-   `keywords_threshold` (número)
-   `max_alternatives` (número inteiro)
-   `word_alternatives_threshold` (número)
-   `word_confidence` (booleano)
-   `timestamps` (booleano)
-   `profanity_filter` (booleano)
-   `smart_formatting` (booleano)
-   `speaker_labels` (booleano)
-   `grammar_name` (sequência)
-   `redaction` (booleano)

Somente os dois parâmetros a seguir são específicos para solicitações com múltiplas partes:

-   O campo `part_content_type` é *opcional* para a maioria dos formatos de áudio. Ele é necessário para os formatos `audio/alaw`, `audio/basic`, `audio/l16` e `audio/mulaw`. Ele especifica o formato do áudio nas partes a seguir da solicitação. Todos os arquivos de áudio devem estar no mesmo formato.
-   O campo `data_parts_count` é *opcional* para todas as solicitações. Ele especifica o número de arquivos de áudio enviados com a solicitação. O serviço aplica a detecção de final de fluxo para a última (e possivelmente a única) parte dos dados. Se você omitir o parâmetro, o serviço determinará o número de partes por meio da solicitação.

Todos os outros parâmetros dos metadados são opcionais. Para obter descrições de todos os parâmetros disponíveis, consulte o [Resumo de parâmetro](/docs/services/speech-to-text/summary.html).

### Exemplo de solicitação com múltiplas partes

O exemplo de `curl` a seguir mostra como passar uma solicitação de reconhecimento com múltiplas partes com o método `POST /v1/recognize`. A solicitação passa dois arquivos de áudio, **audio-file1.flac** e **audio-file2.flac**. O parâmetro `metadata` fornece a maioria dos parâmetros da solicitação. Os parâmetros `upload` fornecem os arquivos de áudio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

O exemplo retorna a transcrição a seguir para os arquivos de áudio. O serviço retorna os resultados para os dois arquivos na ordem em que eles são enviados. (A saída de exemplo abreviará os resultados para o segundo arquivo.)

```javascript
{
   "results": [
        {
         "word_alternatives": [
            {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
               "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
            {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
               "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
               "end_time": 6.04
            }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
