---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# Entendendo os resultados de reconhecimento
{: #basic-response}

Independentemente da interface que você usa, o serviço {{site.data.keyword.speechtotextfull}} retorna resultados de transcrição que refletem os parâmetros de entrada e saída que você especifica. O serviço retorna todo o conteúdo de resposta JSON no conjunto de caracteres UTF-8.
{: shortdesc}

## Resposta de transcrição básica
{: #response}

O serviço retorna a resposta a seguir para os exemplos em [Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request). Os exemplos passam apenas um arquivo de áudio e seu tipo de conteúdo. O áudio fala uma única sentença sem pausas perceptíveis entre as palavras.

```javascript
{
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

O serviço retorna um objeto `SpeechRecognitionResults`, que é o objeto de resposta de nível superior. Para essas solicitações simples, o objeto inclui um campo `results` e um campo `result_index`:

-   O campo `results` fornece uma matriz de informações sobre os resultados da transcrição. Para este exemplo, o campo `alternatives` inclui a `transcript` e a `confidence` do serviço nos resultados. O campo `final` tem um valor igual a `true` para indicar que esses resultados não mudarão.
-   O campo `result_index` fornece um identificador exclusivo para os resultados. O exemplo mostra os resultados finais para uma solicitação com um único arquivo de áudio que não tem pausas e a solicitação não inclui parâmetros adicionais. Portanto, o serviço retorna um único campo `result_index` com um valor de `0`, que é sempre o índice inicial.

Se o áudio de entrada for mais complexo ou a solicitação incluir parâmetros adicionais, os resultados poderão conter muito mais informações.

### O campo alternatives
{: #responseAlternatives}

O campo `alternatives` fornece uma matriz de resultados da transcrição. Para essa solicitação, a matriz inclui um único elemento.

-   O campo `confidence` é uma pontuação que indica a confiança do serviço na transcrição, que para este exemplo se aproxima de 90%.
-   O campo `transcript` fornece os resultados da transcrição.

Os campos `final` e `result_index` qualificam o significado desses campos.

### O campo final
{: #responseFinal}

O campo `final` indica se a transcrição mostra os resultados da transcrição final:

-   O campo é `true` para obter os resultados finais, que têm a garantia de não mudar. O serviço não envia atualizações adicionais para transcrições que ele retorna como resultados finais.
-   O campo é `false` para resultados provisórios, que estão sujeitos à mudança. Se você usar o parâmetro `interim_results` com a interface do WebSocket, o serviço retornará hipóteses provisórias em desenvolvimento na forma de múltiplos campos `results` à medida que transcrever o áudio. O campo `final` é sempre `false` para resultados provisórios. O serviço configura o campo como `true` para obter os resultados finais para o áudio. O serviço não envia atualizações adicionais para a transcrição desse áudio.

Para obter resultados provisórios, use a interface do WebSocket e configure o parâmetro `interim_results` como `true`. Para obter mais informações, consulte [Resultados provisórios](/docs/services/speech-to-text?topic=speech-to-text-output#interim).

### O campo result_index
{: #responseResultIndex}

O campo `result_index` fornece um identificador para os resultados que são exclusivos para essa solicitação. Se você solicitar resultados provisórios, o serviço enviará múltiplos campos `results` para o desenvolvimento de hipóteses do áudio de entrada. Os índices para resultados provisórios para o mesmo áudio sempre têm o mesmo valor, assim como os resultados finais para o mesmo áudio.

Depois de receber os resultados finais para qualquer áudio, o serviço não envia mais resultados com esse índice para o restante da solicitação. O índice para qualquer resultado adicional é incrementado por um.

Independentemente de você solicitar resultados provisórios, o serviço poderá retornar vários resultados finais com índices diferentes se seu áudio incluir pausas ou períodos de silêncio estendidos. Para obter mais informações, consulte [Pausas e silêncio](#pauses-silence).

Se o áudio produzir múltiplos resultados finais, concatene os elementos `transcript` dos resultados finais para montar a transcrição completa do áudio. Montar os resultados em ordem por `result_index`. É possível ignorar resultados provisórios para os quais o campo `final` é `false`.

### Conteúdo de resposta adicional
{: #responseAdditional}

Muitos dos parâmetros de saída que estão disponíveis para reconhecimento de voz afetam os conteúdos da resposta do serviço. Por exemplo, os parâmetros a seguir fazem com que o serviço retorne informações adicionais sobre o áudio:

-   O parâmetro `max_alternatives` solicita múltiplas transcrições alternativas. A matriz `alternatives` inclui um elemento separado para cada alternativa. Cada elemento da matriz inclui um campo `transcript`. Somente a alternativa mais provável inclui um campo de `confidence`.
-   Os parâmetros `word_confidence` e `timestamps` solicitam informações adicionais sobre palavras individuais da transcrição. A matriz `alternatives` inclui campos adicionais com esses nomes.
-   Os parâmetros `keywords` e `keywords_threshold` solicitam a marcação de palavra-chave. A matriz `results` inclui um campo `keywords_result`.
-   O parâmetro `word_alternatives_threshold` solicita resultados acústicos semelhantes para palavras individuais. A matriz `results` inclui um campo `word_alternatives`.
-   O parâmetro `interim_results` da interface do WebSocket solicita hipóteses de transcrição provisórias. Conforme mencionado anteriormente, o serviço envia múltiplas respostas à medida que transcreve o áudio.
-   O parâmetro `speaker_labels` identifica os falantes individuais de uma interação com múltiplos participantes. A resposta inclui um campo `speaker_labels` no mesmo nível que os campos `results` e `results_index`.

Para obter mais informações sobre esses e outros parâmetros que podem afetar a resposta do serviço, consulte [Recursos de saída](/docs/services/speech-to-text?topic=speech-to-text-output). Para obter mais informações sobre todos os parâmetros disponíveis, consulte o [Resumo de parâmetro](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Pausas e silêncio
{: #pauses-silence}

O serviço transcreve um fluxo de áudio inteiro até que o fluxo termine ou um tempo limite ocorra. No entanto, se o áudio incluir pausas ou silêncio estendido entre palavras ou frases faladas, os resultados do reconhecimento poderão incluir múltiplos resultados finais.

O intervalo de pausa padrão que o serviço usa para determinar os resultados finais separados é aproximadamente um segundo. Para a maioria dos idiomas, o intervalo de pausa é precisamente 0,8 segundo. Para o chinês, o intervalo é 0,6 segundo. Não é possível mudar a duração do intervalo de pausa.

Como o serviço retorna os resultados depende da interface que você usa. Os exemplos a seguir mostram respostas com dois resultados finais das interfaces HTTP e WebSocket. O mesmo áudio de entrada é usado em ambos os casos. O áudio fala a frase "um dois três quatro cinco seis", com uma pausa de um segundo entre as palavras "três" e "quatro".

-   *Para as interfaces HTTP,* o serviço envia um único objeto `SpeechRecognitionResults`. A matriz `alternatives` tem um elemento separado para cada resultado final. A resposta tem um único campo `result_index` com um valor de `0`.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *Para a interface do WebSocket,* o serviço envia duas respostas separadas com dois objetos `SpeechRecognitionResults` diferentes. Cada objeto de resposta tem um campo `result_index` diferente, que tem um valor de `0` para a primeira resposta e `1` para a segunda resposta.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

Se seus resultados incluírem múltiplos resultados finais, concatene os elementos `transcript` dos resultados finais para montar a transcrição completa do áudio.

O silencio de 30 segundos no áudio transmitido pode resultar em um [tempo limite de inatividade](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
{: note}

## Marcadores de hesitação
{: #hesitation}

O serviço pode incluir marcadores de hesitação em uma transcrição ao descobrir breves preenchimentos ou pausas na fala. Também referido como disfluências, essas pausas podem incluir preenchimentos como "uhm", "uh", "hmm" e elocuções não lexicais relacionadas. A menos que seja necessário usá-los para seu aplicativo, é possível filtrar com segurança os marcadores de hesitação de uma transcrição.

Em inglês, o serviço usa o token de hesitação `%HESITATION`, conforme mostrado no exemplo a seguir. Outros idiomas podem usar marcadores diferentes; por exemplo, o japonês usa o token `D_`.

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Os marcadores de hesitação também podem aparecer em outros campos de uma transcrição. Por exemplo, se você solicitar [Registros de data e hora da palavra](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) para as palavras individuais de uma transcrição, o serviço relatará o horário de início e de encerramento de cada marcador de hesitação.

```javascript
{
  "results": [
        {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Letras maiúsculas e minúsculas
{: #capitalization}

Para a maioria dos idiomas, o serviço não usa letras maiúsculas e minúsculas em transcrições de resposta. Se letras maiúsculas e minúsculas forem importantes para o seu aplicativo, você deverá mudar para letra maiúscula a primeira palavra de cada sentença e qualquer outro termo para o qual letras maiúsculas e minúsculas sejam apropriadas.

*Somente para inglês dos EUA,* o serviço insere letra maiúscula em muitos nomes próprios. Por exemplo, o serviço retorna o texto a seguir para a frase especificada:

```
Barack Obama graduated from Columbia University
```
{: codeblock}

Para outros idiomas, o serviço retorna o texto a seguir:

```
barack obama graduated from columbia university
```
{: codeblock}

O serviço sempre aplica letras maiúsculas e minúsculas ao inglês dos EUA, independentemente de se você usa a formatação inteligente.

## Pontuação
{: #punctuation}

Por padrão, o serviço não insere a pontuação em transcrições de resposta. Deve-se incluir qualquer pontuação que você precisa para os resultados do serviço.

Para alguns idiomas, é possível usar formatação inteligente para direcionar o serviço a substituir símbolos de pontuação, como vírgulas, pontos, pontos de interrogação e pontos de exclamação, para determinadas sequências de palavras-chave. Para obter mais informações, consulte [formatação inteligente](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).
