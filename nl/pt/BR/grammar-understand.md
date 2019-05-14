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
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Entendendo as gramáticas
{: #grammarUnderstand}

Os exemplos a seguir introduzem o suporte do serviço {{site.data.keyword.speechtotextfull}} para gramáticas. Os exemplos criam duas gramáticas ABNF simples e mostram possíveis resultados quando elas são usadas para reconhecimento de voz. Os exemplos ilustram a importância de examinar a pontuação de confiança que o serviço inclui com uma transcrição.
{: shortdesc}

Os exemplos fornecem apenas os resultados das solicitações de reconhecimento de voz. Para obter exemplos que mostram como passar uma gramática para reconhecimento de voz, consulte [Usando uma gramática para reconhecimento de voz](/docs/services/speech-to-text/grammar-use.html). Os exemplos também são muito básicos. Para obter exemplos de gramáticas mais complexas, consulte [Gramáticas de exemplo](/docs/services/speech-to-text/grammar-examples.html).

## Correspondências de frase única: a gramática yesno
{: #yesnoGrammar}

O primeiro exemplo define uma gramática `yesno` muito simples que aceita duas respostas de palavra única válidas, `yes` e `no`. A gramática é útil em casos em que o usuário deve responder com apenas uma das duas frases.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

Quando você aplica essa gramática a uma solicitação de reconhecimento de voz, o serviço pode retornar uma transcrição com uma pontuação que indica sua confiança na correspondência. Ela também poderá retornar sem resultado se a entrada claramente não corresponder a uma das duas frases.

Por exemplo, se o usuário responder `yes`, o serviço provavelmente retornará uma resposta que é muito semelhante ao resultado a seguir. A pontuação no campo `confidence` indica uma correspondência perfeitamente confiável.

```
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Mas suponha, por exemplo, que o usuário responda `nope`. O serviço pode retornar um resultado com uma pontuação de confiança muito baixa ou nenhum resultado. Um resultado vazio é a indicação mais clara de que a resposta não corresponde à gramática. É mais provável que uma resposta vazia ocorra com gramáticas complexas, em que uma resposta válida deve corresponder a uma sequência específica de várias frases.

## Correspondências de múltiplas frases: a gramática dos nomes
{: #namesGrammar}

Com uma gramática com múltiplas frases, a resposta do usuário deve ser concluída para ser reconhecida. O usuário não pode omitir uma palavra ou parar no meio da resposta. A ausência de uma única palavra pode fazer com que o serviço retorne um resultado vazio.

Além disso, o serviço poderá retornar múltiplas transcrições se o usuário falar frases que são separadas por silêncio suficiente para indicar que elas são elocuções independentes. Por exemplo, considere a gramática `names` simples, que pode corresponder a uma de três nomes com múltiplas palavras.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

Suponha que o usuário fale um dos nomes das regras da gramática, `Yon See`. O serviço retorna uma resposta que indica um nível muito alto de confiança na correspondência.

```
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Agora, suponha que o usuário fale dois nomes separados por silêncio suficiente, pelo menos 0,8 segundo, para indicar que elas são elocuções separadas: `Yon See` [1,0 segundo de silêncio] `Yi Wen Tan`. Nesse caso, o serviço envia duas respostas separadas com uma pontuação de confiança diferente para cada transcrição.

```
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
        {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

Finalmente, considere o caso em que o usuário diz algo como `Yon See` [1,0 segundo de silêncio] `Young Says He`. Para a primeira frase, `Yon See`, o serviço envia uma correspondência positiva com uma pontuação de confiança, semelhante aos exemplos anteriores. Para a segunda frase, `Young Says He`, o serviço pode ter uma de duas respostas possíveis:

-   Ele pode não enviar resposta, indicando que a frase não corresponde a uma das regras da gramática.
-   Ele pode enviar uma resposta com uma pontuação de baixa confiança, indicando que a frase é acusticamente semelhante a uma das regras, mas que não é uma correspondência provável.

## Pontuações de confiança e resultados vazios
{: #confidenceScores}

Para gramáticas relativamente simples como aquelas nos exemplos anteriores, o serviço pode retornar um resultado com uma pontuação de baixa confiança mesmo quando a resposta não parece corresponder à gramática. Isso pode parecer surpreendente, mas uma resposta com uma pontuação de confiança baixa pode indicar a melhor correspondência que o serviço pode encontrar para o áudio fornecido, mesmo que seja improvável a resposta corresponder à gramática. Se a resposta não corresponder à gramática, o valor de confiança dos resultados será geralmente baixo o suficiente para indicar a improbabilidade de que a gramática possa gerar a resposta.

Considere sempre a pontuação de confiança ao avaliar se uma resposta satisfaz uma gramática. Se você não souber qual limite configurar para rejeição de um resultado com base em sua confiança, use um valor inicial de 0,5. Se você tiver uma taxa de rejeição inesperadamente grande de resultados corretos, diminua o limite de confiança; por exemplo, 0,1 pode ser uma boa opção para alguns aplicativos. Se você tiver um grande número de reconhecimentos incorretos, aumente o limite de confiança para seu aplicativo.

Em muitos casos, um resultado vazio ou um resultado com uma pontuação de confiança muito baixa é uma resposta válida. Ele pode indicar que o usuário não entendeu a pergunta ou as opções disponíveis. Sempre é possível reconhecer a resposta de áudio do usuário sem a gramática, mas você corre o risco de obter um resultado que não é válido para a gramática. As gramáticas fornecem resultados mais confiáveis do que as n-grams, que sempre retornam um resultado para qualquer coisa diferente do completo silêncio.

Saber que o resultado deve ser uma das frases válidas definidas por uma gramática é um recurso poderoso que pode simplificar a manipulação de resposta de um aplicativo. Em geral, o serviço pode retornar resultados com alta confiança apenas para elocuções que são geradas por uma gramática. Para a gramática `yesno` no primeiro exemplo para reconhecer com confiança a frase `nope` como uma resposta válida, deve-se incluir essa frase na gramática.
