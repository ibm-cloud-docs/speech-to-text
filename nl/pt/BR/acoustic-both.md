---

copyright:
  years: 2019
lastupdated: "2019-06-24"

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

# Usando modelos acústicos e de idioma customizados juntos
{: #useBoth}

É possível melhorar a precisão do reconhecimento de voz usando modelos complementares acústicos e de idioma customizados. É possível usar os dois tipos de modelo durante o treinamento de seu modelo acústico e durante o reconhecimento de voz. Os dois modelos devem ser de propriedade da mesma instância de serviço e devem ser baseados no mesmo modelo de idioma de base.
{: shortdesc}

Usar um modelo acústico customizado sozinho ou com um modelo de idioma customizado no qual ele não foi treinado ainda pode ser útil. Se o modelo acústico customizado foi treinado em características acústicas que correspondem ao áudio que está sendo transcrito, ele ainda poderá melhorar a qualidade da transcrição.

## Treinando um modelo acústico customizado com um modelo de idioma customizado
{: #useBothTrain}

Treinar um modelo acústico customizado com dados de áudio sozinho é conhecido como *treinamento não supervisionado*. Usar um modelo de idioma customizado durante o treinamento é conhecido como *treinamento levemente supervisionado*. O treinamento levemente supervisionado pode melhorar a eficácia do seu modelo acústico customizado.

Use o treinamento levemente supervisionado para treinar um modelo acústico customizado com um modelo de idioma customizado nos casos a seguir:

-   O modelo de idioma customizado é baseado em transcrições (literalmente texto) dos arquivos de áudio incluídos no modelo acústico customizado.

    Como as transcrições contêm os conteúdos exatos do áudio, o treinamento com um modelo de idioma customizado que é baseado em transcrições pode produzir os melhores resultados. O serviço pode analisar os conteúdos das transcrições no contexto e extrair palavras OOV e n-grams que podem ajudá-lo a fazer o uso mais efetivo de seu áudio. Isso é especialmente verdadeiro se os seus dados de áudio têm menos de uma hora de duração.

    Transcrever os dados de áudio não é estritamente necessário. Mas se você tiver transcrições do áudio, elas poderão melhorar a qualidade do modelo acústico customizado. As transcrições serão especialmente valiosas se o áudio contiver muitas palavras OOV.
-   O modelo de idioma customizado é baseado em corpora (arquivos de texto) ou em uma lista de palavras que são relevantes para os conteúdos dos arquivos de áudio.

    Se seu áudio contiver palavras específicas do domínio que não estão localizadas no vocabulário base do serviço, a customização do modelo acústico sozinha não produzirá essas palavras durante a transcrição. A customização do modelo de idioma é a única maneira de expandir o vocabulário base do serviço. Se você não tiver transcrições, treine com um modelo de idioma customizado que inclua palavras OOV do mesmo domínio que os seus dados de áudio. Mesmo o treinamento com um modelo de idioma customizado que inclui uma lista das palavras OOV que são usadas no áudio pode ser útil.

    Por exemplo, suponha que você esteja criando um modelo acústico customizado que se baseie no áudio da central de atendimento para um produto específico. É possível treinar o modelo acústico customizado com um modelo de idioma customizado que é baseado em transcrições de chamadas relacionadas ou que inclui nomes de produtos específicos que são manipulados pela central de atendimento.

Para usar uma transcrição ou uma lista de palavras, primeiro crie um modelo de idioma customizado que contém esses dados textuais. Para treinar um modelo acústico customizado com um modelo de idioma customizado, ambos os modelos customizados devem ser baseados na mesma versão do mesmo modelo base. Se uma nova versão do modelo base for disponibilizada, você deverá fazer upgrade de ambos os modelos para a mesma versão do modelo base para que o treinamento seja bem-sucedido.

Use o parâmetro de consulta opcional `custom_language_model_id` do método `POST /v1/acoustic_customizations/{customization_id}/train` para treinar seu modelo acústico customizado com um modelo de idioma customizado. Passe o GUID do modelo acústico com o parâmetro `customization_id` e o GUID do modelo de idioma customizado com o parâmetro `custom_language_model_id`. Ambos os modelos devem ser de propriedade das credenciais que são transmitidas com a solicitação.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Usando os modelos acústico e de idioma customizados durante o reconhecimento de voz
{: #useBothRecognize}

É possível especificar um modelo de idioma customizado e um modelo acústico customizado com qualquer solicitação de reconhecimento. Se um modelo de idioma customizado contiver palavras OOV do domínio do áudio que está sendo reconhecido, será possível usá-lo com um modelo acústico customizado durante o reconhecimento de voz para melhorar a precisão da transcrição.

Usar um modelo de idioma customizado pode melhorar a precisão da transcrição, independentemente de você ter treinado o modelo acústico customizado com o modelo de idioma customizado:

-   Usar os modelos acústico e de idioma customizados durante o treinamento melhora a qualidade do modelo acústico customizado.
-   Usar os dois tipos de modelo durante o reconhecimento de voz melhora a qualidade da transcrição.

Se um modelo de idioma customizado incluir gramáticas, também será possível usar o modelo de idioma customizado e uma de suas gramáticas com um modelo acústico customizado durante o reconhecimento de voz.

O exemplo a seguir transmite os dois tipos de modelo para o método HTTP `POST /v1/recognize`. Passe o GUID do modelo acústico customizado com o parâmetro `acoustic_customization_id` e o GUID do modelo de idioma customizado com o parâmetro `language_customization_id`. Ambos os modelos devem ser de propriedade das credenciais que são transmitidas com a solicitação e devem ser baseados no mesmo modelo base (por exemplo, `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

Para uma solicitação de HTTP assíncrona, especifique os parâmetros quando criar a tarefa assíncrona. Para o WebSockets, passe os parâmetros ao estabelecer a conexão.
