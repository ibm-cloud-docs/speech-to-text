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

# Incluindo uma gramática em um modelo de idioma customizado
{: #grammarAdd}

Antes de poder usar uma gramática para reconhecimento de voz, deve-se primeiro usar a interface de customização para incluir a gramática em um modelo de idioma customizado. As etapas para incluir uma gramática em um modelo de idioma customizado são semelhantes àquelas usadas para incluir palavras customizadas ou dos corpora:
{: shortdesc}

1.  [Criar um modelo de idioma customizado](#createModel-grammar). É possível criar um novo modelo customizado ou usar um modelo existente.
1.  [Incluir uma gramática no modelo de idioma customizado](#addGrammar). O serviço valida a gramática para assegurar sua exatidão.
1.  [Validar as palavras da gramática](#validateGrammar). Você verifica a exatidão das pronúncias para qualquer palavra fora do vocabulário (OOV) que são reconhecidas pela gramática.
1.  [Treinar o modelo de idioma customizado](#trainGrammar). O serviço prepara o modelo customizado e a gramática para uso no reconhecimento de voz.
1.  Agora é possível usar o modelo customizado e a gramática em solicitações de reconhecimento de voz. Para obter mais informações, consulte [Usando uma gramática para reconhecimento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

Essas etapas são iterativas. É possível incluir gramáticas, corpora e palavras customizadas em um modelo de idioma customizado, quantas forem necessárias. Deve-se treinar o modelo customizado em qualquer novo recurso de dados (gramáticas, corpora ou palavras customizadas) que você inclui. Ao usá-lo para reconhecimento de voz, um modelo customizado reflete os dados nos quais ele foi treinado pela última vez.

O recurso de gramáticas é funcionalidade beta. É possível usar as gramáticas com qualquer idioma que suporte a customização do modelo de idioma. A customização do modelo de idioma está disponível para a maioria dos idiomas. Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Criar um modelo de idioma customizado
{: #createModel-grammar}

Para usar uma gramática com reconhecimento de voz, deve-se inclui-la em um modelo de idioma customizado. É possível usar um modelo de idioma customizado existente ou criar um novo modelo customizado usando o método `POST /v1/customizations`. Para obter informações sobre como criar um novo modelo customizado, consulte [Criar um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).

Um modelo de idioma customizado pode conter corpora e palavras customizadas, bem como gramáticas. Durante o reconhecimento de voz, é possível usar o modelo customizado com ou sem suas gramáticas. No entanto, quando você usa uma gramática, o serviço reconhece apenas palavras da gramática especificada.

## Incluir uma gramática no modelo de idioma customizado
{: #addGrammar}

Use o método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` para incluir um arquivo de gramática em um modelo customizado.

-   Especifique o ID de customização do modelo de idioma customizado com o parâmetro de caminho `customization_id`.
-   Especifique um nome para a gramática com o parâmetro de caminho `grammar_name`. Use um nome localizado que corresponda ao idioma do modelo customizado e reflita os conteúdos da gramática.
    -   Inclua um máximo de 128 caracteres no nome.
    -   Não utilize caracteres que precisem ser codificados pela URL. Por exemplo, não use espaços, barras, barras invertidas, dois-pontos, e comercial, aspas duplas, sinais de mais, sinais de igual, pontos de interrogação e assim por diante no nome. (O serviço não impede o uso desses caracteres. Mas como eles devem ser codificados pela URL onde quer que sejam usados, seu uso não é recomendado.)
    -   Não use o nome de uma gramática ou corpus que já tenha sido incluído no modelo customizado.
    -   Não use o nome `user`, que é reservado pelo serviço para indicar palavras customizadas que são incluídas ou modificadas pelo usuário.
    -   Não use o nome `base_lm` ou `default_lm`. Ambos os nomes são reservados para uso futuro pelo serviço.
-   Use o cabeçalho da solicitação `Content-Type` para especificar o formato da gramática:
    -   `application/srgs` para uma gramática ABNF
    -   `application/srgs + xml` para uma gramática XML
-   Passe o arquivo que contém a gramática como o corpo da solicitação.

O exemplo a seguir inclui o arquivo de gramática denominado `confirm.abnf` no modelo customizado com o ID especificado O exemplo nomeia a gramática `confirm-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

O método também aceita um parâmetro de consulta opcional `allow_overwrite` que você pode incluir para sobrescrever uma gramática existente com o mesmo nome. Use o parâmetro se for necessário atualizar uma gramática depois de inclui-la em um modelo.

O método é assíncrono. Pode levar alguns segundos para que o serviço analise a gramática, dependendo do tamanho da gramática e do carregamento atual no serviço. Para obter informações sobre como verificar o status de uma gramática, consulte [Monitorando a solicitação de inclusão de gramática](#monitorGrammar).

É possível incluir qualquer número de gramáticas em um modelo customizado chamando o método uma vez para cada arquivo de gramática. A inclusão de uma gramática deve ser totalmente concluída antes que outra possa ser incluída.

### Monitorando a solicitação de inclusão de gramática
{: #monitorGrammar}

O serviço retornará um código de resposta 201 se a gramática for válida. Em seguida, ele processa de forma assíncrona os conteúdos da gramática e extrai automaticamente novas palavras que a gramática pode reconhecer. Não é possível enviar solicitações para incluir mais recursos de dados em um modelo customizado ou treinar o modelo até que a análise da gramática do serviço para a solicitação atual seja concluída.

Para determinar o status da análise, use o método `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` para pesquisar o status da gramática. O método aceita o ID do modelo customizado e o nome da gramática. O exemplo a seguir verifica o status da gramática chamada `confirm-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

A resposta inclui o status da gramática:

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

O campo `status` tem um dos valores a seguir:

-   `analyzed` indica que o serviço analisou com êxito a gramática.
-   `being_processed` indica que o serviço ainda está analisando a gramática.
-   `undetermined` indica que o serviço encontrou um erro ao processar a gramática.

Use um loop para verificar o status da gramática a cada 10 segundos até que se torne `analyzed`. Para obter mais informações sobre como verificar o status de uma gramática, consulte [Listando gramáticas para um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars).

### Manipulando falhas de inclusão de gramática
{: #handleFailures}

Se a sua análise de uma gramática falhar, o serviço configurará o status da gramática como `undetermined` e incluirá um campo `error` que descreva a falha com o status da gramática. Também é possível usar o método `GET /v1/customizations/{customization_id}` para verificar o status do modelo customizado. Se a inclusão de uma gramática falhar, a saída incluirá uma mensagem de erro como a seguinte:

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

O status do modelo customizado não é afetado pelo erro, mas a gramática não pode ser usada com o modelo. É possível resolver o problema corrigindo o arquivo de gramática e repetindo a solicitação para inclui-la no modelo customizado. Configure o parâmetro `allow_overwrite` como `true` para sobrescrever a versão do arquivo de gramática que falhou.

Também é possível excluir a gramática do modelo customizado para o momento e, em seguida, corrigir e incluir o arquivo de gramática novamente no futuro. Para obter mais informações sobre como excluir uma gramática, consulte [Excluindo uma gramática de um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#deleteGrammar).

## Validar as palavras da gramática
{: #validateGrammar}

Quando você inclui um arquivo de gramática em um modelo de idioma customizado, o serviço analisa a gramática para determinar se a gramática reconhece quaisquer palavras que ainda não fazem parte do vocabulário base do serviço. Tais palavras são conhecidas como palavras fora do vocabulário (OOV). O serviço inclui palavras OOV para o recurso de palavras do modelo customizado. O propósito do recurso de palavras é definir palavras que ainda não estão presentes no vocabulário do serviço.

As definições no recurso de palavras informam ao serviço como transcrever as palavras OOV. As informações incluem um campo `sounds_like` que informa ao serviço como a palavra é pronunciada, um campo `display_as` que informa ao serviço como exibir a palavra e um campo `source` que indica como a palavra foi incluída no modelo customizado. Para obter mais informações sobre o recurso de palavras e as palavras OOV, consulte [O recurso de palavras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResource).

Depois de incluir uma gramática em um modelo customizado, é uma boa prática examinar as palavras OOV no recurso de palavras do modelo para verificar suas pronúncias. Nem todas as gramáticas têm palavras OOV, mas a verificação do recurso de palavras geralmente é uma boa ideia. Para verificar as palavras do modelo customizado depois de incluir uma gramática, use os métodos a seguir:

-   Use o método `GET /v1/customizations/{customization_id}/words` para listar todas as palavras por meio de um modelo customizado. Passe o valor `grammars` com o parâmetro `word_type` do método para listar apenas palavras que foram incluídas por meio de gramáticas.
-   Use o método `GET /v1/customizations/{customization_id}/words/{word_name}` para visualizar uma palavra individual por meio de um modelo.

Verifique se as pronúncias das palavras são precisas e corretas. Consulte também os erros tipográficos e outros nas próprias palavras. Para obter mais informações sobre como validar e corrigir as palavras em um modelo customizado, consulte [Validando um recurso de palavras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

## Treinar o modelo de idioma customizado
{: #trainGrammar}

A etapa final antes que seja possível usar uma gramática com um modelo de idioma customizado é treinar o modelo. Você usa o mesmo processo usado para treinar um modelo customizado em novos corpora ou novas palavras customizadas. O treinamento do modelo compila a gramática para uso durante o reconhecimento de voz. O serviço processa a gramática de seu formato baseado em texto original para um formato de tempo de execução binário para reconhecimento de voz. Não é possível usar a gramática até que você treine o modelo.

Use o método `POST /v1/customizations/{customization_id}/train` para treinar um modelo customizado. Passe ao método o ID de customização do modelo que você deseja treinar, como no exemplo a seguir.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

O método de treinamento é assíncrono. O treinamento geralmente leva apenas segundos para ser concluído. Mas pode levar mais tempo dependendo do tamanho e da complexidade da gramática e da carga atual no serviço. Para obter informações sobre como verificar o status de uma operação de treinamento, consulte [Monitorando a solicitação de treinamento](#monitorTraining-grammar).

### Monitorando a solicitação de treinamento
{: #monitorTraining-grammar}

O serviço retornará um código de resposta 200 se o processo de treinamento for iniciado com êxito. O serviço não pode aceitar solicitações de treinamento subsequentes ou solicitações para incluir novas gramáticas, corpora ou palavras para o modelo customizado até que a solicitação de treinamento atual seja concluída.

Para determinar o status de uma solicitação de treinamento, use o método `GET /v1/customizations/{customization_id}` para pesquisar o status do modelo. O método aceita o ID de customização do modelo e retorna informações como as seguintes sobre o modelo:

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

O campo `status` tem o valor `training` enquanto o modelo está sendo treinado. Use um loop para verificar o status do modelo a cada 10 segundos até que se torne `available`. Para obter mais informações sobre como monitorar uma solicitação de treinamento e outros valores de status possíveis, consulte [Monitorando a solicitação de treinamento do modelo](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language).
