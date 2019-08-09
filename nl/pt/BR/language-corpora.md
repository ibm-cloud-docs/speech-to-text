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

# Gerenciando os corpora
{: #manageCorpora}

A interface de customização inclui o método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para incluir um corpus em um modelo de idioma customizado. Para obter mais informações, consulte [Incluir um corpus no modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus). A interface também inclui os métodos a seguir para listar e excluir corpora para um modelo de idioma customizado.
{: shortdesc}

## Listando os corpora para um modelo de idioma customizado
{: #listCorpora}

A interface de customização fornece dois métodos para listar informações sobre os corpora para um modelo de idioma customizado:

-   O método `GET /v1/customizations/{customization_id}/corpora` lista informações sobre todos os corpora para um modelo customizado.
-   O método `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` lista informações sobre um corpus especificado para um modelo customizado.

Os dois métodos retornam o `name` do corpus, o `total_words` lido do corpus e o número de `out-of-of-vocabulary_words` extraído do corpus. Os métodos também listam o `status` do corpus. O status é importante para verificar a análise do serviço de um corpus em resposta a uma solicitação para incluí-lo em um modelo customizado:

-   `analyzed` indica que o serviço analisou com êxito o corpus. O modelo customizado pode ser treinado com dados do corpus.
-   `being_processed` indica que o serviço ainda está analisando o corpus. O serviço não pode aceitar solicitações para incluir novos corpora ou palavras, ou para treinar o modelo customizado, até que sua análise esteja concluída.
-   `undetermined` indica que o serviço encontrou um erro ao processar o corpus. As informações que são retornadas para o corpus incluem uma mensagem de erro que oferece orientação para corrigir o erro.

    Por exemplo, o corpus pode ser inválido ou você pode ter tentado inclui-lo com o mesmo nome de um existente. É possível tentar incluir o corpus novamente e incluir o parâmetro `allow_overwrite` com a solicitação. Também é possível excluir o corpus e, em seguida, tentar incluí-lo novamente.

### Solicitações e respostas de exemplo
{: #listExample-corpora}

O exemplo a seguir lista todos os corpora para o modelo customizado com o ID de customização especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

Três corpora foram incluídos no modelo customizado. O serviço analisou `corpus1` com êxito. Ele ainda está analisando `corpus2` e sua análise de `corpus3` falhou.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

O exemplo a seguir retorna informações sobre o corpus que é denominado `corpus1` para o modelo customizado com o ID de customização especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## Excluindo um corpus de um modelo de idioma customizado
{: #deleteCorpus}

Use o método `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` para remover um corpus existente de um modelo de idioma customizado. Ao excluir o corpus, o serviço remove todas as palavras OOV que estão associadas ao corpus do recurso de palavras do modelo customizado, a menos que

-   A palavra também foi incluída por outro corpus ou por uma gramática.
-   A palavra foi modificada de alguma maneira com o método `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}`.

A remoção de um corpus não afeta o modelo customizado até que você treine o modelo em seus dados atualizados usando o método `POST /v1/customizations/{customization_id}/train`. Se você treinou com êxito o modelo no corpus, as palavras do corpus permanecerão no vocabulário do modelo e se aplicarão ao reconhecimento de voz até que você retreine o modelo.

### Solicitação de exemplo
{: #deleteExample-corpus}

O exemplo a seguir exclui o corpus que é denominado `corpus3` do modelo customizado com o ID de customização especificado.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
