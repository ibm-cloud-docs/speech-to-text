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

# Gerenciando palavras customizadas
{: #manageWords}

A interface de customização inclui os métodos `POST /v1/customizations/{customization_id}/words` e `PUT /v1/customizations/{customization_id}/words/{word_name}`, que são usados para incluir ou modificar palavras para um modelo customizado. Para obter mais informações, consulte [Incluir palavras no modelo de idioma customizado](/docs/services/speech-to-text/language-create.html#addWords). A interface também inclui os métodos a seguir para listar e excluir palavras para um modelo de idioma customizado.
{: shortdesc}

É provável que você inclua a maioria das palavras customizadas dos corpora. Certifique-se de que você conheça a codificação de caracteres usada nos arquivos de texto para seus corpora. O serviço preserva a codificação que ele localiza nos arquivos de texto. Deve-se usar essa codificação ao trabalhar com as palavras individuais no modelo de idioma customizado. Ao especificar uma palavra com o método `GET`, `PUT` ou `DELETE /v1/customizations/{customization_id}/words/{word_name}`, deve-se codificar a URL do `word_name` que você transmite na URL se a palavra inclui caracteres não ASCII. Para obter mais informações, consulte [Codificação de caracteres](/docs/services/speech-to-text/language-resource.html#charEncoding).
{: important}

## Listando palavras de um modelo de idioma customizado
{: #listWords}

A interface de customização oferece dois métodos para listar palavras de um modelo de idioma customizado:

-   O método `GET /v1/customizations/{customization_id}/words` lista informações sobre as palavras do recurso de palavras do modelo customizado. O método inclui dois parâmetros de consulta opcionais:
    -   O parâmetro `word_type` especifica quais palavras devem ser listadas:

        -   `all` (o padrão) mostra todas as palavras.
        -   `user` mostra somente palavras customizadas que foram incluídas ou modificadas pelo usuário.
        -   `corpora` mostra apenas as palavras OOV que foram extraídas dos corpora.
        -   `grammars` mostra somente as palavras OOV que foram extraídas de gramáticas.
    -   O parâmetro `sort` indica como as palavras devem ser ordenadas. O parâmetro aceita dois argumentos para indicar como as palavras devem ser classificadas: `alphabetical` e `count`. É possível incluir um `+` ou `-` opcional à frente de um argumento para indicar se os resultados devem ser classificados em ordem crescente ou decrescente. Por padrão, o método exibe as palavras em ordem alfabética crescente.
-   O método `GET /v1/customizations/{customization_id}/words/{word_name}` lista informações sobre uma única palavra especificada do recurso de palavras do modelo.

Além de um campo `word` que identifica a palavra, ambos os métodos retornam as informações a seguir sobre cada palavra:

-   Um campo `sounds_like` que apresenta uma matriz de pronúncias para a palavra. A matriz poderá incluir a pronúncia gerada automaticamente pelo serviço se nenhum valor de pronúncia for fornecido para a palavra. Para obter mais informações, consulte [Usando o campo sounds_like](/docs/services/speech-to-text/language-resource.html#soundsLike).
-   Um campo `display_as` que mostra a ortografia da palavra customizada que o serviço exibe em transcrições. O campo conterá uma cadeia vazia se nenhum valor de exibição for fornecido para a palavra. Nesse caso, a palavra é exibida como é escrita. Para obter mais informações, consulte [Usando o campo display_as](/docs/services/speech-to-text/language-resource.html#displayAs).
-   Um campo `source` que indica como a palavra foi incluída no recurso de palavras do modelo customizado. O campo inclui o nome de cada corpus e gramática do qual o serviço extraiu a palavra. Se você modificou ou incluiu a palavra diretamente, o campo incluirá a sequência `user`.
-   Um campo `count` que indica o número de vezes que a palavra é localizada em todos os corpora e gramáticas. Por exemplo, se a palavra ocorrer cinco vezes em um corpus e sete vezes em outro, sua contagem será `12`.

    Se você incluir uma palavra customizada em um modelo antes que ela seja incluída por qualquer corpora ou gramática, a contagem iniciará em `1`. Se a palavra for incluída de um corpus ou gramática primeiro e modificada posteriormente, a contagem refletirá somente o número de vezes que ela é localizada em corpora e gramáticas.

    Para modelos customizados que foram criados antes da introdução do campo `count`, o campo sempre permanece em `0`. Para atualizar o campo para esses modelos, inclua os corpora e gramáticas do modelo novamente e inclua o parâmetro `allow_overwrite` com a solicitação.
    {: note}

Se o serviço descobrir um ou mais problemas com uma definição de palavra customizada, a saída incluirá um campo `error`. O campo fornece uma matriz que lista cada elemento do problema por meio da definição e uma mensagem que descreve o problema.

Por exemplo, um erro poderá ocorrer se você incluir uma palavra customizada com um campo `sounds_like` inválido, um que viola uma das regras para incluir uma pronúncia. Não é possível treinar um modelo customizado cujo recurso de palavras inclui uma palavra com um erro. Deve-se corrigir ou excluir a palavra antes de poder treinar o modelo.

### Solicitações e respostas de exemplo
{: #listExample-words}

O exemplo a seguir lista todas as palavras, independentemente do tipo, do modelo customizado com o ID de customização especificado. As palavras são exibidas na ordem de classificação alfabética crescente padrão.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

O recurso de palavras para o modelo contém quatro palavras. A primeira palavra foi incluída diretamente pelo usuário, mas seu campo `sounds_like` contém um erro: o campo não pode conter números. As outras palavras foram incluídas pelo usuário ou pelos corpora e o usuário.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

O exemplo a seguir mostra informações sobre a palavra `NCAA` do recurso de palavras do modelo especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

O usuário incluiu a palavra inicialmente. O serviço então localizou a palavra duas vezes em `corpus3`.

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## Excluindo uma palavra de um modelo de idioma customizado
{: #deleteWord}

Use o método `DELETE /v1/customizations/{customization_id}/words/{word_name}` para excluir uma palavra de um modelo de idioma customizado. Use o método para remover palavras que foram incluídas com erro, por exemplo, de um corpus com dados com falha.

É possível remover qualquer palavra que você incluiu no recurso de palavras do modelo customizado via qualquer meio. No entanto, não é possível excluir uma palavra do vocabulário base do serviço. A exclusão de uma palavra de um modelo customizado exclui apenas a pronúncia customizada para a palavra. A palavra permanece no vocabulário base.

A remoção de uma palavra de um modelo customizado não afeta o modelo até que você o retreine usando o método `POST /v1/customizations/{customization_id}/train`. Se o modelo foi treinado anteriormente na palavra, o modelo continuará a aplicar a palavra ao reconhecimento de voz mesmo depois de excluir a palavra de seu recurso de palavras. Deve-se retreinar o modelo para refletir a exclusão.

### Solicitação de exemplo
{: #deleteExample-word}

O exemplo a seguir exclui a palavra `IEEE` do modelo customizado com o ID de customização especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
