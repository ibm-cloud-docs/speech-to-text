---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# Recursos de saída
{: #output}

O serviço {{site.data.keyword.speechtotextfull}} oferece os recursos a seguir para indicar as informações que o serviço deve incluir em seus resultados de transcrição para uma solicitação de reconhecimento de voz. Todos os parâmetros de saída são opcionais.
{: shortdesc}

-   Para obter exemplos de solicitações de reconhecimento de voz simples para cada uma das interfaces do serviço, consulte [Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Para obter exemplos e descrições de respostas de reconhecimento de voz, consulte [Entendendo os resultados de reconhecimento](/docs/services/speech-to-text?topic=speech-to-text-basic-response). O serviço retorna todo o conteúdo de resposta JSON no conjunto de caracteres UTF-8.
-   Para obter uma lista em ordem alfabética de todos os parâmetros de reconhecimento de voz disponíveis, incluindo seu status (geralmente disponível ou beta) e idiomas suportados, consulte o [Resumo de parâmetro](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Rótulos do falante
{: #speaker_labels}

O recurso de rótulos do falante é a funcionalidade beta que está disponível para inglês dos EUA, japonês e espanhol (modelos de banda larga e banda estreita) e inglês do Reino Unido (somente modelo de banda estreita).
{: note}

Os rótulos do falante identificam quais palavras cada indivíduo falou em uma interação com múltiplos participantes. (Às vezes, a rotulagem de quem falou e quando é conhecida como *diarização do falante*.) É possível usar as informações para desenvolver uma transcrição pessoa por pessoa de um fluxo de áudio, como um contato para uma central de atendimento. Ou é possível usá-las para animar uma interação com um robô ou avatar de conversação. Para obter melhor desempenho, use áudio que tenha pelo menos um minuto.

Os rótulos do falante são otimizados para cenários de dois falantes. Eles funcionam melhor para conversas telefônicas que envolvem duas pessoas em uma interação prolongada. Eles podem lidar com até seis falantes, mas mais de dois falantes pode resultar em desempenho variável. Interações de duas pessoas são geralmente realizadas por meio de mídia de banda estreita, mas é possível usar rótulos de falante com modelos de banda larga e banda estreita suportados.

Para usar o recurso, configure o parâmetro `speaker_labels` como `true` para uma solicitação de reconhecimento. O parâmetro é `false` por padrão. O serviço identifica os falantes por palavras individuais do áudio. Ele depende do horário de início e de encerramento da palavra para identificar seu falante. Portanto, a ativação dos rótulos do falante também força o parâmetro `timestamps` a ser `true` (consulte [Registros de data e hora](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)).

### Exemplo de rótulos do falante
{: #speakerLabelsExample}

A solicitação de exemplo a seguir mostra uma resposta que inclui rótulos do falante. Os valores numéricos que estão associados a cada elemento da matriz `timestamps` são os horários de início e de encerramento da palavra no áudio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

O campo `transcript` mostra a transcrição final do áudio, que lista as palavras à medida que foram faladas por todos os participantes. Ao comparar e sincronizar os rótulos do falante com os registros de data e hora, é possível remontar a conversa como ela ocorreu.

<table style="width:50%">
  <caption>Tabela 1. Exemplo de rótulos do falante</caption>
  <tr>
    <th style="text-align:left">Registro de data e hora</th>
    <th style="text-align:left">Rótulo do falante</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

Os resultados indicam claramente como essa interação de duas pessoas começou:

-   **Falante 2** - "Olá?"
-   **Falante 1** - "Sim?"
-   **Falante 2** - "Sim, como está o Billy?"

No exemplo, os campos `confidence` indicam a confiança do serviço em sua identificação dos falantes. O campo `final` tem um valor de `false` para cada palavra. Esse valor indica que o serviço ainda está processando o áudio. O serviço pode mudar sua identificação do falante ou sua confiança para palavras individuais antes de concluir.

### IDs do falante para rótulos do falante
{: #speakerLabelsIDs}

O exemplo também ilustra um aspecto interessante dos IDs do falante. Nos campos `speaker`, o primeiro falante tem um ID `2` e o segundo tem um ID `1`. O serviço desenvolve um melhor entendimento dos padrões do falante à medida que ele processa o áudio. Portanto, ele pode mudar os IDs do falante para palavras individuais e também pode incluir e remover falantes, até que ele gere seus resultados finais.

Como resultado, os IDs do falante podem não ser sequenciais, contíguos ou ordenados. Por exemplo, os IDs geralmente iniciam em `0`, mas no exemplo anterior, o ID mais antigo é `1`. O serviço provavelmente omitiu uma designação de palavra anterior para o falante `0` com base na análise adicional do áudio. Omissões também podem acontecer para falantes posteriores. As omissões podem resultar em diferenças na numeração e produzir resultados para dois falantes que são rotulados, por exemplo, `0` e `2`. Outra consideração é que os falantes podem deixar uma conversa. Portanto, um participante que contribui somente para os estágios iniciais de uma conversa pode não aparecer em resultados posteriores.

### Solicitando resultados provisórios para rótulos do falante
{: #speakerLabelsInterim}

Com a interface do WebSocket, é possível solicitar resultados provisórios, assim como os rótulos do falante (consulte [Resultados provisórios](/docs/services/speech-to-text?topic=speech-to-text-output#interim)). Os resultados finais são geralmente melhores do que os resultados provisórios. Mas os resultados provisórios podem ajudar a identificar a evolução de uma transcrição e a designação de rótulos do falante. Os resultados provisórios podem indicar onde os falantes e os IDs transitórios apareceram ou desapareceram. No entanto, o serviço pode reutilizar os IDs de falantes que ele identifica inicialmente e, posteriormente, reconsidera e omite. Portanto, um ID pode se referir a dois falantes diferentes nos resultados provisório e final.

Quando você solicita resultados provisórios e rótulos do falante, os resultados finais para fluxos de áudio longos podem chegar bem depois dos resultados provisórios iniciais serem retornados. Também é possível para alguns resultados provisórios incluir apenas um campo `speaker_labels` sem os campos `results` e `result_index`. Se você não solicitar resultados provisórios, o serviço retornará resultados finais que incluem os campos `results` e `result_index` e um único campo `speaker_labels`.

O serviço envia os resultados finais quando o fluxo de áudio é concluído ou em resposta a um tempo limite, o que ocorrer primeiro. O serviço configura o campo `final` como `true` somente para a última palavra dos rótulos do falante que ele retorna em qualquer um dos casos.

### Considerações sobre desempenho para rótulos do falante
{: #speakerLabelsPerformance}

Conforme observado anteriormente, o recurso de rotulagem do falante é otimizado para conversas com duas pessoas, como as comunicações com uma central de atendimento. Portanto, é necessário considerar os seguintes problemas de desempenho em potencial:

-   O desempenho para áudio com um único falante pode ser pobre. As variações na qualidade de áudio ou na voz do falante podem fazer com que o serviço identifique falantes extras que não estão presentes. Esses falantes são conhecidos como alucinações.
-   Da mesma forma, o desempenho para áudio com um falante dominante, como um podcast, pode ser insuficiente. O serviço tende a perder os falantes que falam por uma menor quantidade de tempo e isso também pode produzir alucinações.
-   O desempenho para áudio com mais de seis falantes é indefinido. O recurso pode manipular um máximo de seis falantes.
-   O desempenho para elocuções curtas pode ser menos preciso do que para elocuções longas. O serviço produz melhores resultados quando os participantes falam por períodos mais longos de tempo, pelo menos 30 segundos por falante. A quantia relativa de áudio que está disponível para cada falante também pode afetar o desempenho.
-   O desempenho pode se degradar para os primeiros 30 segundos da fala. Ele geralmente melhora para um nível razoável após 1 minuto de áudio.

Como com toda transcrição, o desempenho também pode ser afetado pela má qualidade do áudio, o ruído de plano de fundo, a maneira de falar da pessoa, a sobreposição de falantes e outros aspectos do áudio.

## Marcação de palavra-chave
{: #keyword_spotting}

O recurso de marcação da palavra-chave detecta sequências especificadas em uma transcrição. O serviço pode marcar a mesma palavra-chave múltiplas vezes e relatar cada ocorrência. O serviço marca as palavras-chave somente nos resultados finais, não nos resultados provisórios. Por padrão, o serviço não faz nenhuma marcação de palavra-chave.

Para usar a marcação de palavra-chave, deve-se especificar ambos os parâmetros a seguir:

-   Use o parâmetro `keywords` para especificar uma matriz de sequências a serem marcadas. O serviço não marcará nenhuma palavra-chave se você omitir o parâmetro ou especificar uma matriz vazia. Uma sequência de palavra-chave pode incluir mais de um token. Por exemplo, a palavra-chave `Speech to Text` tem três tokens.

    Para inglês dos EUA, o serviço normaliza cada palavra-chave para corresponder às sequências faladas versus por escrito. Por exemplo, ele normaliza números para corresponder ao modo como elas são faladas em oposição a como são escritas. Para outros idiomas, as palavras-chave devem ser especificadas à medida que são ditas.

    É possível identificar um máximo de 1.000 palavras-chave com uma única solicitação. As palavras-chave não fazem distinção entre maiúsculas e minúsculas.
-   Use o parâmetro `keywords_threshold` para especificar uma probabilidade entre 0,0 e 1,0 para uma correspondência de palavra-chave. O limite indica o limite inferior para o nível de confiança que o serviço deve ter para uma palavra que corresponda à palavra-chave. Uma palavra-chave é marcada na transcrição somente se sua confiança for maior ou igual ao limite especificado.

    Especificar um pequeno limite pode potencialmente produzir muitas correspondências. Se você especificar um limite, também deverá especificar uma ou mais palavras-chave. Omita o parâmetro para não retornar nenhuma correspondência.

A marcação de palavra-chave é necessária para identificar palavras-chave em um fluxo de áudio. Não é possível identificar palavras-chave processando uma transcrição final porque a transcrição representa os melhores resultados decodificadores do serviço para o áudio de entrada. Ela não inclui tokens com pontuações de confiança mais baixas que podem representar uma palavra de interesse. Então, aplicar as ferramentas de processamento de texto a uma transcrição no lado do cliente pode não identificar palavras-chave. Uma representação mais rica dos resultados decodificadores é necessária e essa representação está disponível somente no servidor.

### Resultados de marcação de palavra-chave
{: #keywordSpottingResults}

O serviço retorna os resultados em um campo `keywords_result` que é um elemento da matriz `results`. O campo `keywords_result` é um dicionário, ou matriz associativa, de propriedades enumeráveis. Cada propriedade é identificada por uma palavra-chave especificada e inclui uma matriz de objetos. O serviço retorna um elemento da matriz para cada correspondência que ele localiza para a palavra-chave. O objeto para cada correspondência inclui os campos a seguir:

-   `normalized_text` é a palavra-chave especificada que é normalizada para a frase falada que correspondeu ao áudio de entrada.
-   `start_time` é o horário de início em segundos da correspondência.
-   `end_time` é o horário de encerramento em segundos da correspondência.
-   `confidence` é a confiança do serviço de que a correspondência representa a palavra-chave especificada. A confiança deve ser pelo menos tão grande quanto o limite especificado a ser incluído nos resultados.

Uma palavra-chave para a qual o serviço não localiza nenhuma correspondência é omitida da matriz. Uma palavra-chave pode não ser localizada se

-   O áudio simplesmente não inclui a palavra-chave. A ausência da palavra-chave é a explicação mais óbvia.
-   O limite está configurado muito alto. O serviço pode identificar a palavra-chave, mas com um nível inferior de confiança, caso em que ele omite a correspondência dos resultados.
-   Uma sequência de palavra-chave que contém múltiplos tokens (por exemplo, `Speech to Text`) é falada com muito silêncio entre os seus tokens. Quando o serviço transcreve o áudio, ele divide o fluxo em uma série de blocos. Cada bloco representa um chunk contínuo de áudio que não tem um intervalo de silêncio acima de meio segundo. Ele constrói uma matriz de objetos de resultado que consiste nesses blocos.

    O serviço corresponde a uma palavra-chave com múltiplos tokens somente se

    -   Os tokens da palavra-chave estão no mesmo bloco.
    -   Os tokens são adjacentes ou separados por uma diferença de até 0,1 segundo.

    O último caso poderá ocorrer se um breve preenchimento ou elocução não lexical, como "uhm" ou "well", estiver entre dois tokens da palavra-chave. Para obter mais informações, consulte [Marcadores de hesitação](/docs/services/speech-to-text?topic=speech-to-text-basic-response#hesitation).

### Exemplo de marcação de palavra-chave
{: #keywordSpottingExample}

A solicitação de exemplo a seguir configura o parâmetro `keywords` para uma matriz codificada por URL de três sequências (`colorado`, `tornado` e `tornados`) e o parâmetro `keywords_threshold` como `0.5`. O serviço localiza as ocorrências de `colorado` e `tornadoes`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
        {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.96,
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

## Alternativas máximas
{: #max_alternatives}

O parâmetro `max_alternatives` aceita um valor de número inteiro que informa ao serviço para retornar as *n* melhores hipóteses alternativas para os resultados. Por padrão, o serviço retorna apenas um único resultado de transcrição, que é o equivalente a configurar o parâmetro como `1`. Ao configurar `max_alternatives` para um número maior que 1, você pede ao serviço para retornar esse número das melhores transcrições alternativas. (Se você especificar um valor de `0`, o serviço usará o valor padrão de `1`.)

O serviço relata uma pontuação de confiança somente para a melhor alternativa que ele retorna. Na maioria dos casos, essa é a alternativa a escolher.

### Exemplo de alternativas máximas
{: #maximumAlternativesExample}

A solicitação de exemplo a seguir configura o parâmetro `max_alternatives` como `3`. O serviço relata uma confiança para a mais provável das três alternativas.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado and Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Resultados provisórios
{: #interim}

O recurso de resultados provisórios está disponível somente com a interface do WebSocket.
{: note}

Os resultados provisórios são hipóteses intermediárias de uma transcrição que provavelmente mudará antes que o serviço retorne seus resultados finais. O serviço retorna os resultados provisórios assim que ele os gera. Os resultados provisórios são úteis para

-   Aplicativos interativos e transcrição em tempo real
-   Fluxos de áudio longos, que podem levar um tempo para serem transcritos

Os resultados provisórios chegam com mais frequência e mais rapidamente do que os resultados finais. É possível usá-los para permitir que seu aplicativo responda mais rapidamente ou para avaliar o progresso da transcrição.

Para receber resultados provisórios, configure o parâmetro JSON `interim_results` como `true` na mensagem `start` para uma solicitação de reconhecimento do WebSocket. Se você omitir o parâmetro `interim_results` ou configurá-lo como `false`, o serviço retornará somente uma única transcrição JSON no final do áudio. Siga estas diretrizes para usar o parâmetro:

-   Omita o parâmetro ou configure-o como `false` se você estiver fazendo uma transcrição off-line ou em lote, não precisar de latência mínima e desejar um único resultado JSON para todo o áudio.
-   Configure o parâmetro como `true` se você desejar que os resultados cheguem progressivamente à medida que o serviço processar o áudio ou se deseja os resultados com a latência mínima. Tenha em mente que o serviço pode atualizar resultados provisórios à medida que ele processa mais áudio.

Os resultados provisórios são indicados na resposta JSON com o campo `final` configurado como `false`. O serviço pode atualizar esses resultados com transcrições mais precisas à medida que ele processa áudio adicional. Os resultados finais são identificados com o campo `final` configurado como `true`. O serviço não faz atualizações adicionais nos resultados finais.

### Exemplo de resultados provisórios
{: #interimResultsExample}

O exemplo abreviado a seguir solicita resultados provisórios para uma solicitação do WebSocket. Em sua resposta, o serviço configura o atributo `final` como `true` somente para o resultado final.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
        {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
        {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.96,
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

## Alternativas de palavra
{: #word_alternatives}

O recurso de alternativas de palavra (também conhecido como redes de confusão) relata hipóteses para alternativas acusticamente semelhantes para palavras do áudio de entrada. Por exemplo, a palavra `Austin` pode ser a melhor hipótese para uma palavra do áudio. Mas a palavra `Boston` é outra hipótese possível no mesmo intervalo de tempo. As hipóteses compartilham um horário comum inicial e de encerramento, mas têm diferentes ortografias e, geralmente, diferentes pontuações de confiança.

Por padrão, o serviço não relata alternativas de palavra. Para indicar que você deseja receber hipóteses alternativas, use o parâmetro `word_alternatives_threshold` para especificar uma probabilidade entre 0,0 e 1,0. O limite indica o limite inferior para o nível de confiança que o serviço deve ter em uma hipótese para retorná-la como uma alternativa de palavra. Uma hipótese é retornada apenas se sua confiança for maior ou igual ao limite especificado.

É possível pensar em alternativas de palavra como a linha de tempo para uma transcrição que é dividida em intervalos ou compartimentos menores. Cada compartimento pode ter uma ou mais hipóteses com diferentes ortografias e confiança. O parâmetro `word_alternatives_threshold` controla a densidade dos resultados que o serviço retorna. Especificar um pequeno limite pode produzir potencialmente muitas hipóteses.

### Resultados de alternativas de palavra
{: #wordAlternativesResults}

O serviço retorna os resultados em um campo `word_alternatives` que é um elemento da matriz `results`. O campo `word_alternatives` é uma matriz de objetos, cada um dos quais fornece os campos a seguir para uma palavra alternativa:

-   `start_time` indica o tempo em segundos em que a palavra para a qual as hipóteses são retornadas inicia no áudio de entrada.
-   `end_time` indica o tempo em segundos no qual a palavra para a qual as hipóteses são retornadas termina no áudio de entrada.
-   `alternatives` é uma matriz de objetos de hipótese. Cada objeto inclui uma `confidence` que indica a pontuação de confiança do serviço para a hipótese e uma `word` que identifica a hipótese. A confiança deve ser pelo menos tão grande quanto o limite especificado a ser incluído nos resultados. O serviço ordena as alternativas por confiança.

### Exemplo de alternativas de palavra
{: #wordAlternativesExample}

A solicitação de exemplo a seguir especifica um `word_alternatives_threshold` de `0.9`. A saída inclui hipóteses em potencial e pontuações de confiança para várias palavras, juntamente com os seus horários de início e de encerramento.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
        {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.96,
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

## Confiança de palavra
{: #word_confidence}

O parâmetro `word_confidence` informa ao serviço se medidas de confiança para as palavras da transcrição devem ser fornecidas. Por padrão, o serviço relata uma medida de confiança somente para a transcrição final como um todo. Configurar `word_confidence` como `true` direciona o serviço para relatar uma medida de confiança para cada palavra individual da transcrição.

Uma medida de confiança indica a estimativa do serviço de que a palavra transcrita está correta com base na evidência acústica. As pontuações de confiança variam de 0,0 a 1,0.

-   Uma pontuação de 1,0 indica que a transcrição atual da palavra reflete o resultado mais provável.
-   Uma pontuação de 0,5 significa que a palavra tem uma chance de 50% de estar correta.

### Exemplo de confiança de palavra
{: #wordConfidenceExample}

O exemplo a seguir solicita pontuações de confiança de palavra para as palavras da transcrição.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Registros de data e hora da palavra
{: #word_timestamps}

O parâmetro `timestamps` informa ao serviço se registros de data e hora para as palavras transcritas devem ser produzidos. Por padrão, o serviço não relata nenhum registro de data e hora. Configurar `timestamps` como `true` direciona o serviço para relatar o tempo de início e de encerramento em segundos para cada palavra relativa ao início do áudio.

### Exemplo de registros de data e hora de palavra
{: #wordTimestampsExample}

O exemplo a seguir solicita registros de data e hora para as palavras da transcrição.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.96,
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

## Formatação Smart
{: #smart_formatting}

O recurso de formatação inteligente é a funcionalidade beta que está disponível para inglês dos EUA, japonês e espanhol.
{: note}

O parâmetro `smart_formatting` direciona o serviço para converter as sequências a seguir em representações mais convencionais:

-   Datas
-   Horários
-   Série de algarismos e números
-   Números de telefone
-   Valores de moeda
-   E-mail da Internet e endereços da web

Você configura o parâmetro `smart_formatting` como `true` para ativar a formatação inteligente. Por padrão, o serviço não executa formatação inteligente.

O serviço aplica a formatação inteligente somente à transcrição final de uma solicitação de reconhecimento. Ele aplica a formatação inteligente pouco antes de retornar os resultados para o cliente, quando a normalização do texto está concluída. A conversão torna a transcrição mais legível e permite um melhor pós-processamento dos resultados da transcrição.

### Pontuação (Inglês dos EUA)
{: #smartFormattingPunctuation}

Para inglês dos EUA, o recurso também direciona o serviço para substituir símbolos de pontuação para as seguintes sequências de palavras-chave faladas no áudio.

<table style="width:50%">
  <caption>Tabela 2. Palavras-chave de pontuação da formatação inteligente</caption>
  <tr>
    <th style="width:45%; text-align:left">Sequência de palavras-chave</th>
    <th style="text-align:center">Pontuação resultante</th>
  </tr>
  <tr>
    <td>
      Vírgula
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      Ponto
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      Ponto de interrogação
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      Ponto de exclamação
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

O serviço converte essas sequências de palavras-chave em símbolos somente em locais apropriados. Por exemplo, o serviço converte a frase falada

```
the warranty period is short period
```
{: codeblock}

para o texto a seguir em uma transcrição

```
the warranty period is short.
```
{: codeblock}

### Diferenças de idioma
{: #smartFormattingDifferences}

A formatação inteligente é baseada na presença de palavras-chave óbvias na transcrição. Os idiomas suportados manipulam a formatação inteligente de forma um pouco diferente.

#### Inglês dos EUA e espanhol
{: #smartFormattingEnglishSpanish}

-   Os horários são identificados por palavras-chave como `AM`, `PM`ou `EST`.
-   Os horários militares serão convertidos se forem identificados pela palavra-chave `hours`.
-   Os números de telefone devem ser `911` ou um número com 10 ou 11 dígitos que inicia com o número `1`.

#### Japonês
{: #smartFormattingJapanese}

-   O e-mail da Internet e os endereços da web não são convertidos.
-   Os números de telefone devem ter 10 ou 11 dígitos e começar com prefixos válidos para números de telefone no Japão. Por exemplo, prefixos válidos incluem `03` e `090`.
-   Palavras inglesas são convertidas em caracteres ASCII (*hankaku*). Por exemplo, <code>& #65321; & #65314; & #65325;</code> é convertido em `IBM`.
-   Os termos ambíguos podem não ser convertidos se não houver contexto suficiente disponível. Por exemplo, não está claro se <code>& #19968; & #26178;</code> e <code>& #21313; & #20998;</code> referem-se a horários.
-   A pontuação é manipulada da mesma maneira com ou sem formatação inteligente. Por exemplo, com base em cálculos de probabilidade, um de <code>& #12459; & #12531; & #12510;</code> ou `,` é selecionado.

### Exemplo de formatação inteligente
{: #smartFormattingExample}

O exemplo a seguir solicita formatação inteligente com uma solicitação de reconhecimento, configurando o parâmetro `smart_formatting` como `true`. As seções a seguir mostram os efeitos da formatação inteligente sobre os resultados de uma solicitação.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### Resultados da formatação inteligente
{: #smartFormattingResults}

A tabela a seguir mostra exemplos de transcrições finais com e sem formatação inteligente. As transcrições são baseadas em áudio em inglês dos EUA.

<table summary="Cada linha de título é seguida por múltiplas linhas de exemplos que mostram o efeito da formatação inteligente para o elemento que é identificado no título.">
  <caption>Tabela 3. Transcrições de exemplo de formatação inteligente</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">Sem formatação inteligente</th>
    <th id="with_formatting" style="text-align:left">Com formatação inteligente</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>Datas</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>Horários</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>Números</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>Números de telefone</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>Valores de moeda</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>E-mail da Internet e endereços da web</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>Combinações</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### Exemplo de transcrições com pausas longas
{: #smartFormattingLongPauses}

Nos casos em que uma elocução contém silêncios longos o suficiente, o serviço pode dividir a transcrição em dois ou mais chunks finais. Isso afeta os resultados da formatação, conforme mostrado nos exemplos a seguir.

<table>
  <caption>Tabela 4. Transcrições de exemplo de formatação inteligente para pausas longas</caption>
  <tr>
    <th style="width:45%; text-align:left">Fala de áudio</th>
    <th style="text-align:left">Resultados de transcrição formatados</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## Edição de dados numéricos
{: #redaction}

O recurso de edição de dados numéricos é a funcionalidade beta que está disponível para inglês dos EUA, japonês e coreano.
{: note}

O parâmetro `redaction` direciona o serviço para editar, ou mascarar, dados numéricos de transcrições finais. O recurso edita qualquer número que tenha três ou mais dígitos consecutivos, substituindo cada dígito por um caractere `X`. Ele é destinado a editar dados numéricos sensíveis, como números de cartão de crédito.

Por padrão, o serviço não edita dados numéricos. Configure o parâmetro `redaction` como `true` para ativar a edição de dados numéricos. Quando você ativa a edição de dados, o serviço ativa automaticamente a formatação inteligente, independentemente de você desativar explicitamente esse recurso. Para assegurar a segurança máxima, o serviço também impõe os seguintes valores de parâmetro ao ativar a edição de dados:

-   O serviço desativa a marcação de palavra-chave, independentemente de você especificar valores para os parâmetros `keywords` e `keywords_threshold`.
-   O serviço desativa os resultados provisórios, independentemente de você configurar o parâmetro `interim_results` da interface do WebSocket como `true`.
-   O serviço retorna apenas uma única transcrição final, independentemente de você especificar um valor para o parâmetro `maximum_alternatives`.

O design do recurso é semelhante ao recurso de formatação inteligente existente. O serviço aplica a edição de dados somente à transcrição final de uma solicitação de reconhecimento, pouco antes de retornar os resultados para o cliente e após a normalização do texto ser concluída.

Versões futuras do recurso podem expandir a edição de dados para mascarar todos os números de telefone, endereços de rua, contas bancárias, números de seguridade social e outros dados numéricos sensíveis.
{: note}

### Diferenças de idioma
{: #redactionDifferences}

O recurso funciona exatamente conforme descrito para os modelos em inglês dos EUA, mas tem as diferenças a seguir para modelos japoneses e coreanos.

#### Japonês
{: #redactionJapanese}

A edição de dados japonesa tem as diferenças a seguir:

-   Além de mascarar sequências de três ou mais dígitos consecutivos, a edição de dados também mascara endereços e números de rua, independentemente de se eles contêm menos de três dígitos.
-   Da mesma forma, a edição de dados também mascara informações de data em datas de nascimento no estilo japonês. Em japonês, as informações de data são geralmente apresentadas no formato *Anno Domini* latino, mas às vezes segue o estilo japonês, especificamente para datas de nascimento. Nesse caso, o ano e o mês são mascarados, mesmo que contenham apenas um ou dois dígitos. Por exemplo, a edição de dados numéricos muda a sequência a seguir conforme mostrado.

    <table style="width:50%">
      <caption>Tabela 5. Exemplo de edição de dados da data de nascimento em estilo japonês</caption>
      <tr>
        <th style="text-align:left">Sem edição de dados</th>
        <th style="text-align:left">Com edição de dados</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### Coreano
{: #redactionKorean}

A edição de dados coreana tem as diferenças a seguir:

-   O recurso de formatação inteligente não é suportado. O serviço ainda executa a edição de dados numéricos para coreano, mas não executa nenhuma outra formatação inteligente.
-   Os caracteres de dígito isolado são reduzidos, mas os possíveis caracteres de dígito que estão incluídos como parte de frases em coreano não são. Por exemplo, o caractere <code>& #51060;</code> na frase a seguir não é substituído por um `X` porque ele é adjacente ao caractere seguinte:

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    Se o caractere <code>& #51060;</code> fosse separado do caractere seguinte por um espaço, ele seria substituído por um `X`, conforme descrito em [Resultados da edição de dados numéricos](#redactionResults).

### Exemplo de edição de dados numéricos
{: #redactionExample}

O exemplo a seguir solicita a edição numérica com uma solicitação de reconhecimento, configurando o parâmetro `redaction` como `true`. Como a solicitação ativa a edição de dados, o serviço ativa implicitamente a formatação inteligente com a solicitação. O serviço desativa efetivamente os outros parâmetros da solicitação para que eles não tenham efeito. O serviço retorna uma única transcrição final e não reconhece palavras-chave. A seção a seguir mostra os efeitos da edição de dados.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### Resultados de edição de dados numéricos
{: #redactionResults}

A tabela a seguir mostra exemplos de transcrições finais com e sem edição de dados numéricos em cada idioma suportado.

<table style="width:90%" summary="Cada linha de título identifica um idioma e é seguida por uma única linha que mostra a mesma transcrição com e sem a edição de dados numéricos ativada.">
  <caption>Tabela 6. Transcrições de exemplo de edição de dados numéricos</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">Sem edição de dados</th>
    <th id="with_redaction" style="text-align:left">Com edição de dados</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>Inglês dos EUA</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>Japonês</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>Coreano</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## Filtragem de profanidade
{: #profanity_filter}

O recurso de filtragem de profanidade está geralmente disponível somente para inglês dos EUA.
{: note}

O parâmetro `profanity_filter` indica se o serviço deve censurar a profanidade de seus resultados. Por padrão, o serviço obscurece toda a profanidade, substituindo-a por uma série de asteriscos na transcrição. Configurar o parâmetro como `false` exibe as palavras na saída exatamente como transcritas.

O serviço censura a profanidade de todas as transcrições finais e de qualquer transcrição alternativa. Ele também censura a profanidade de resultados que estão associados com alternativas de palavras, confiança de palavra e registros de data e hora de palavra. A única exceção é a marcação de palavra-chave, para a qual o serviço retorna todas as palavras, conforme especificado pelo usuário, independentemente de `profanity_filter` ser `true`.

### Exemplo de filtragem de profanidade
{: #profanityFilteringExample}

O exemplo a seguir mostra os resultados para um arquivo de áudio breve que é transcrito com o valor `true` padrão para o parâmetro `profanity_filter`. A solicitação também configura o parâmetro `word_alternatives_threshold` para um valor relativamente alto de `0.99` e os parâmetros `word_confidence` e `timestamps` como `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
        {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
