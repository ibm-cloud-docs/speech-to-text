---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Gerenciando gramáticas
{: #manageGrammars}

A interface de customização inclui o método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` para incluir uma gramática em um modelo de idioma customizado. Para obter mais informações, consulte [Incluir uma gramática no modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). A interface também inclui os métodos a seguir para listar e excluir gramáticas para um modelo de idioma customizado.
{: shortdesc}

## Listando gramáticas para um modelo de idioma customizado
{: #listGrammars}

A interface de customização fornece dois métodos para listar informações sobre gramáticas para um modelo de idioma customizado:

-   O método `GET /v1/customizations/{customization_id}/grammars` lista informações sobre todas as gramáticas para um modelo customizado.
-   O método `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` lista informações sobre uma gramática especificada para um modelo customizado.

Os dois métodos retornam as mesmas informações sobre uma gramática. As informações incluem o `name` da gramática e o número de `out-of_vocabulary_words` que são reconhecidas pela gramática. A resposta também inclui o `status` da gramática, que é importante para verificar a análise da gramática do serviço ao inclui-la em um modelo customizado:

-   `being_processed` indica que o serviço ainda está processando a gramática em resposta a uma solicitação `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`.
-   `analyzed` indica que o serviço processou a gramática com êxito e a incluiu no modelo customizado.
-   `undetermined` significa que o serviço encontrou um erro ao processar a gramática. As informações que são retornadas para a gramática incluem uma mensagem de erro que oferece orientação para corrigir o erro.

    Por exemplo, a gramática pode ser inválida ou você pode ter tentado inclui-la com o mesmo nome de uma existente. É possível tentar incluir a gramática novamente e incluir o parâmetro `allow_overwrite` com a solicitação. Também é possível excluir a gramática e, em seguida, tentar inclui-la novamente.

### Solicitações e respostas de exemplo
{: #listExample-grammars}

O exemplo a seguir lista as informações sobre todas as gramáticas que foram incluídas no modelo customizado com o ID de customização especificado.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

Três gramáticas foram incluídas com êxito no modelo customizado: `confirm-xml`, `confirm-abnf` e `list-abnf`.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

O exemplo a seguir mostra informações sobre a gramática especificada, `list-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## Excluindo uma gramática de um modelo de idioma customizado
{: #deleteGrammar}

Use o método `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` para remover uma gramática existente de um modelo de idioma customizado. Quando ele exclui a gramática, o serviço remove todas as palavras OOV que estão associadas à gramática do recurso de palavras do modelo customizado, a menos que

-   A palavra também foi incluída por outra gramática ou por um corpus.
-   A palavra foi modificada de alguma maneira com o método `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}`.

A remoção de uma gramática não afeta o modelo customizado até que você treine o modelo em seus dados atualizados usando o método `POST /v1/customizations/{customization_id}/train`. Se você treinou com êxito o modelo na gramática, a gramática continuará disponível para reconhecimento de voz até que você retreine o modelo.

### Solicitação de exemplo
{: #deleteExample-grammar}

O exemplo a seguir exclui a gramática denominada `list-abnf` do modelo customizado com o ID de customização especificado.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
