---

copyright:
  years: 2015, 2019
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

# Idiomas e modelos
{: #models}

O serviço {{site.data.keyword.speechtotextfull}} suporta reconhecimento de voz em vários idiomas. Para todas as interfaces, é possível usar o parâmetro `model` para especificar o modelo para uma solicitação de reconhecimento de voz. O modelo indica o idioma no qual o áudio é falado e a taxa em que é amostrado.
{: shortdesc}

## Modelos de idiomas suportados
{: #modelsList}

Para a maioria dos idiomas, o serviço suporta os modelos de banda larga e de banda estreita:

-   *Modelos de banda larga* são para áudio que é amostrado em 16 kHz ou acima. Use modelos de banda larga para aplicativos responsivos, em tempo real. Por exemplo, para aplicativos de fala em tempo real.
-   *Modelos de banda estreita* são para áudio que é amostrado em 8 kHz. Use modelos de banda estreita para decodificação off-line de fala telefônica, que é o uso típico para essa taxa de amostragem.

Escolher o modelo correto para seu aplicativo é importante. Use o modelo que corresponde à taxa de amostragem (e idioma) de seu áudio. O serviço ajusta automaticamente a taxa de amostragem de seu áudio para corresponder ao modelo que você especifica. Para obter mais informações, consulte [Taxa de amostragem](/docs/services/speech-to-text/audio-formats.html#samplingRate).

Para obter a melhor precisão de reconhecimento, você também precisa considerar o conteúdo de frequência de seu áudio. Para obter mais informações, consulte [Frequência de áudio](/docs/services/speech-to-text/audio-formats.html#frequency).
{: tip}

A Tabela 1 lista os modelos suportados para cada idioma. Se você omitir o parâmetro `model` de uma solicitação, o serviço usará o modelo de banda larga de inglês dos EUA, `en-US_BroadbandModel`, por padrão.

<table>
  <caption>Tabela 1. Modelos de idiomas suportados</caption>
  <tr>
    <th style="text-align:left">idioma</th>
    <th style="text-align:center">Modelo de banda larga</th>
    <th style="text-align:center">Modelo de banda estreita</th>
  </tr>
  <tr>
    <td>Português do Brasil</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Francês</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Alemão</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Japonês</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Coreano</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Chinês mandarim</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Árabe padrão moderno</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">Não suportado</td>
  </tr>
  <tr>
    <td>Espanhol</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Inglês do Reino Unido</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Inglês dos EUA</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
</table>

### O modelo de formato curto de inglês dos EUA
{: #modelsShortform}

O modelo de formato curto de inglês dos EUA, `en-US_ShortForm_NarrowbandModel`, pode melhorar o reconhecimento de voz para a resposta de voz interativa (IVR) e as soluções de suporte ao cliente automatizadas. O modelo de formato curto é treinado para reconhecer as elocuções curtas que são frequentemente expressas em configurações de suporte ao cliente, como as centrais de atendimento de suporte automatizadas e humanas. O modelo é ajustado, por exemplo, para elocuções precisas, como dígitos, palavra de caractere único, ortografias de nome e respostas sim/não. Usar uma gramática em combinação com o modelo de formato curto pode melhorar ainda mais os resultados do reconhecimento.

Como acontece com todos os modelos, ambientes com ruído podem impactar negativamente os resultados. Por exemplo, o ruído acústico em segundo plano de aeroportos, veículos em movimento, salas de conferência e múltiplos falantes pode reduzir a precisão da transcrição.  O áudio dos telefones do falante também pode reduzir a precisão devido ao eco comum desses dispositivos. Usar um modelo acústico customizado com o modelo de formato curto pode neutralizar esses efeitos.

-   Para obter mais informações sobre o modelo de idioma e a customização do modelo acústico, consulte [A interface de customização](/docs/services/speech-to-text/custom.html).
-   Para obter mais informações sobre gramáticas, consulte [Usando gramáticas com modelos de idioma customizados](/docs/services/speech-to-text/grammar.html).

### Exemplo de modelo de idioma
{: #modelsExample}

A solicitação de HTTP de exemplo a seguir usa o modelo `en-US-NarrowbandModel` para reconhecimento de voz:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Listando modelos
{: #listModels}

A interface de HTTP fornece dois métodos para listar informações sobre os modelos suportados:

-   O método `GET /v1/models` lista informações sobre todos os modelos disponíveis.
-   O método `GET /v1/models/{model_id}` lista informações sobre um modelo especificado.

Os dois métodos retornam as informações a seguir sobre um modelo:

-   `name` é o nome do modelo que você usa em uma solicitação.
-   `language` é o identificador de idioma do modelo (por exemplo, `en-US`).
-   `rate` identifica a taxa de amostragem (taxa mínima aceitável para áudio) que é usada pelo modelo em Hertz.
-   `url` é o URI para o modelo.
-   `description` fornece uma breve descrição do modelo.
-   `supported_features` descreve os recursos de serviço adicionais que são suportados com o modelo:
    -   `custom_language_model` é um booleano que indica se é possível criar modelos de idioma customizados que são baseados no modelo.
    -   `speaker_labels` indica se é possível usar o parâmetro `speaker_labels` com o modelo.

### Solicitações e respostas de exemplo
{: #listExample}

O exemplo a seguir lista todos os modelos que são suportados pelo serviço:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

O exemplo a seguir mostra informações sobre o modelo de banda larga de inglês dos EUA. O modelo suporta a customização do modelo de idioma e os rótulos de falantes.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
