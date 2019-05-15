---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Trabalhando com corpora e palavras customizadas
{: #corporaWords}

É possível preencher um modelo de idioma customizado com palavras incluindo corpora ou gramáticas no modelo ou incluindo palavras customizadas diretamente:
{: shortdesc}

-   **Corpora:** os meios recomendados de preencher um modelo de idioma customizado com palavras é incluir um ou mais corpora no modelo. Quando você inclui um corpus, o serviço analisa o arquivo e inclui automaticamente quaisquer novas palavras que ele localiza para o modelo customizado. A inclusão de um corpus em um modelo customizado permite que o serviço extraia palavras específicas do domínio no contexto, o que ajuda a assegurar melhores resultados de transcrição. Para obter mais informações, consulte [Trabalhando com os corpora](#workingCorpora).
-   **Gramáticas:** é possível incluir gramáticas em um modelo customizado para limitar o reconhecimento de voz para as palavras ou frases que são reconhecidas por uma gramática. Quando você inclui uma gramática em um modelo, o serviço inclui automaticamente quaisquer novas palavras que ele localiza para o modelo, assim como faz com os corpora. Para obter mais informações, consulte [Usando gramáticas com modelos de idioma customizados](/docs/services/speech-to-text/grammar.html).
-   **Palavras individuais:** também é possível incluir diretamente palavras customizadas individuais em um modelo. O serviço inclui as palavras no modelo assim como faz com as palavras que descobre por meio dos corpora ou gramáticas. Quando você inclui uma palavra diretamente, é possível especificar múltiplas pronúncias e indicar como a palavra deve ser exibida. Também é possível atualizar palavras existentes para modificar ou aumentar as definições que foram extraídas dos corpora ou gramáticas. Para obter mais informações, consulte [Trabalhando com palavras customizadas](#workingWords).

Independentemente de como você as inclui, o serviço armazena todas as palavras que você inclui em um modelo de idioma customizado no recurso de palavras do modelo.

## O recurso de palavras
{: #wordsResource}

O *recurso de palavras* inclui todas as palavras que você inclui dos corpora, das gramáticas ou diretamente. O seu propósito é definir a pronúncia e a ortografia de palavras que ainda não estão presentes no vocabulário base do serviço. As definições dizem ao serviço como transcrever essas *palavras fora do vocabulário (OOV)*.

O recurso de palavras contém as informações a seguir sobre cada palavra OOV. O serviço cria as definições para palavras que são extraídas dos corpora e das gramáticas. Você especifica as características para palavras que você inclui ou modifica diretamente.

-   `word`: a ortografia da palavra conforme localizada em um corpus ou gramática ou conforme incluída por você.
-   `sounds_like`: a pronúncia da palavra. Para palavras extraídas dos corpora e gramáticas, o valor representa como o serviço acredita que a palavra é pronunciada com base em suas regras de idioma. Em muitos casos, a pronúncia reflete a ortografia do campo `word`.

    É possível usar o campo `sounds_like` para modificar a pronúncia da palavra. Também é possível usar o campo para especificar múltiplas pronúncias para uma palavra. Para obter mais informações, consulte [Usando o campo sounds_like](#soundsLike).
-   `display_as`: a ortografia da palavra que o serviço usa em transcrições. O campo indica como a palavra deve ser exibida. Na maioria dos casos, a ortografia corresponde ao valor do campo `word`.

    É possível usar o campo `display_as` para especificar uma ortografia diferente para a palavra. Para obter mais informações, consulte [Usando o campo display_as](#displayAs).
-   `source`: como a palavra foi incluída no recurso de palavras. Se o serviço extraiu a palavra de um corpus ou gramática, o campo listará o nome desse recurso. Como o serviço pode encontrar a mesma palavra em múltiplos recursos, o campo pode listar múltiplos nomes de corpus ou gramática. O campo inclui a sequência `user` se você incluir ou modificar a palavra diretamente.

Quando você atualiza o recurso de palavras de um modelo de qualquer forma, deve-se treinar o modelo para que as mudanças sejam efetivadas durante a transcrição. Para obter mais informações, consulte [Treinar o modelo de idioma customizado](/docs/services/speech-to-text/language-create.html#trainModel-language).

## Qual a quantia de dados que eu preciso?
{: #wordsResourceAmount}

Muitos fatores contribuem para a quantia de dados que você precisa para um modelo de idioma customizado efetivo. Não é possível fornecer o número exato de palavras que devem ser incluídas para qualquer modelo ou aplicativo customizado.

Dependendo do caso de uso, mesmo a inclusão de algumas poucas palavras diretamente em um modelo customizado pode melhorar a qualidade do modelo. Mas incluir palavras OOV de um corpus que mostra as palavras no contexto no qual elas são usadas em áudio pode melhorar muito a precisão da transcrição. Para obter mais informações, consulte [Trabalhando com os corpora](#workingCorpora).

O serviço limita o número de palavras que você pode incluir em um modelo de idioma customizado:

-   É possível incluir um máximo de 90 mil palavras OOV para o recurso de palavras de um modelo customizado. Isso inclui as palavras OOV de todas as origens (corpora, gramáticas e palavras customizadas individuais que você inclui diretamente).
-   É possível incluir um máximo de 10 milhões de palavras em um modelo customizado de todas as origens. Essa figura inclui todas as palavras, tanto as palavras OOV quanto as palavras que já fazem parte do vocabulário base do serviço, que estão incluídas em corpora ou gramáticas. Para corpora, o serviço usa essas palavras adicionais para aprender o contexto no qual as palavras OOV podem aparecer, motivo pelo qual os corpora são um meio mais eficaz de melhorar a precisão do reconhecimento.

Um recurso de palavras grande pode aumentar a latência do reconhecimento de voz, mas o efeito exato é difícil de quantificar ou prever. Como acontece com a quantia de dados que é necessária para produzir um modelo customizado efetivo, o impacto no desempenho de um recurso de palavras grande depende de muitos fatores. Teste seu modelo customizado com diferentes quantias de dados para determinar o desempenho de seus modelos e dados.

## Trabalhando com os corpora
{: #workingCorpora}

Use o método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para incluir um corpus em um modelo customizado. Um corpus é um arquivo de texto simples que contém sentenças de amostra de seu domínio. O exemplo a seguir mostra um corpus abreviado para o domínio de assistência médica. Um arquivo de corpus é geralmente muito mais longo.

```
Corro o risco de ter problemas de saúde durante a viagem?
Algumas pessoas têm mais chances de ter problemas de saúde quando viajam para fora dos Estados Unidos.
Como a doença coronária microvascular é tratada?
Se você for diagnosticado com MVD coronário e também tiver anemia, poderá se beneficiar do tratamento para essa condição.
A anemia desacelera o crescimento das células necessárias para reparar os vasos sanguíneos danificados.
O que causa hepatite autoimune?
Uma combinação de autoimunidade, acionadores ambientais e uma predisposição genética pode levar à hepatite autoimune.
Que pesquisa está sendo feita para a lesão medular?
O National Institute of Neurological Disorders and Stroke NINDS conduz a pesquisa de medula em seus laboratórios nos National Institutes of Health NIH.
O NINDS também apoia a pesquisa adicional por meio de subsídios para as principais instituições de pesquisa no país.
Algumas das mais promissoras técnicas de reabilitação estão ajudando os pacientes com lesão medular a ter mais mobilidade.
O que é osteogênese imperfeita (OI)?
. . .
```
{: codeblock}

O reconhecimento de voz depende de algoritmos estatísticos para analisar o áudio. Palavras de um modelo customizado estão em competição com palavras do vocabulário base do serviço, bem como outras palavras do modelo. (Fatores como o ruído de áudio e os sotaques do falante também afetam a qualidade da transcrição.)

A precisão da transcrição pode depender em grande parte de como as palavras são definidas em um modelo e como os falantes as pronunciam. Para aprimorar a precisão do serviço, use os corpora para fornecer o máximo possível de exemplos de como as palavras OOV são usadas no domínio. Repetir as palavras OOV nos corpora pode melhorar a qualidade do modelo de idioma customizado. Como você duplica as palavras nos corpora depende de como você espera que os usuários as pronunciem no áudio que deve ser reconhecido. Quanto mais sentenças você incluir que representem o contexto no qual os falantes usam as palavras do domínio, melhor a precisão do reconhecimento do serviço.

O serviço não aplica um algoritmo de correspondência de palavra simples. A sua transcrição depende do contexto em que as palavras são usadas. Quando ele analisa um corpus, o serviço inclui informações sobre n-gramas (bigramas, trigramas e assim por diante) das sentenças do corpus no modelo customizado. Essas informações ajudam o serviço a transcrever o áudio com maior precisão e explica por que o treinamento de um modelo customizado em corpora é mais valioso do que o treinamento em palavras customizadas sozinho.

Por exemplo, os contadores aderem a um conjunto comum de padrões e procedimentos que são conhecidos como Princípios Contábeis Geralmente Aceitos (GAAP). Quando você cria um modelo customizado para um domínio financeiro, fornece sentenças que usam o termo GAAP no contexto. As sentenças ajudam o serviço a distinguir entre frases gerais como "a diferença entre eles é pequena" e frases centradas no domínio, como "o GAAP fornece diretrizes para medir e discernir informações financeiras".

Em geral, é melhor para os corpora usarem palavras em contextos diferentes, o que pode melhorar como o serviço aprende as palavras. No entanto, se os usuários falarem as palavras em apenas alguns contextos, a exibição das palavras em outros contextos não melhorará a qualidade do modelo customizado. Os falantes nunca usam as palavras nesses contextos. Se há a probabilidade de os falantes usarem a mesma frase frequentemente, então, repetir essa frase nos corpora poderá melhorar a qualidade do modelo. (Em alguns casos, mesmo incluir algumas palavras customizadas diretamente em um modelo customizado pode fazer uma diferença positiva.)

### Preparando um arquivo de texto do corpus
{: #prepareCorpus}

Siga estas diretrizes para preparar um arquivo de texto do corpus:

-   Forneça um arquivo de texto simples que seja codificado em UTF-8 se ele contiver caracteres não ASCII. O serviço assumirá a codificação UTF-8 se encontrar tais caracteres.

    Certifique-se de que você conheça a codificação de caracteres dos arquivos de texto do corpus. O serviço preserva a codificação que ele localiza nos arquivos de texto. Deve-se usar essa codificação ao trabalhar com as palavras no modelo de idioma customizado. Para obter mais informações, consulte [Codificação de caracteres](#charEncoding).
    {: important}
-   Use letras maiúsculas e minúsculas consistente para palavras no corpus. O recurso de palavras faz distinção entre maiúsculas e minúsculas. Misture letras maiúsculas e minúsculas e use capitalização somente quando desejado.
-   Inclua cada sentença do corpus em sua própria linha e finalize cada linha com um retorno de linha. A inclusão de múltiplas sentenças na mesma linha pode degradar a precisão.
-   Inclua nomes pessoais como unidades discretas em linhas separadas. Não inclua as palavras de um nome em linhas separadas ou como palavras customizadas individuais e não inclua múltiplos nomes na mesma linha do corpus. O exemplo a seguir mostra a maneira correta de melhorar a precisão de reconhecimento para três nomes:

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Inclua informações contextuais adicionais onde disponíveis, por exemplo, `Doctor Sebastian Leifson` ou `President Malcolm Ingersol`. Como ocorre com todas as palavras, duplicar os nomes várias vezes e, se possível, em contextos diferentes, pode melhorar a precisão do reconhecimento.
-   Cuidado com os erros tipográficos. O serviço assume que os erros tipográficos são novas palavras. A menos que você os corrija antes de treinar o modelo, o serviço as incluirá no vocabulário do modelo. Lembre-se do provérbio *Entra lixo, sai lixo!*

Mais sentenças resultam em melhor precisão. Mas o serviço limita um modelo a um máximo de 10 milhões de palavras no total e 90 mil palavras OOV de todas as fontes combinadas.

### O que acontece ao incluir um arquivo de corpus?
{: #parseCorpus}

Quando você inclui um arquivo de corpus, o serviço analisa o conteúdo do arquivo. Ele extrai qualquer nova palavra (OOV) que ele localiza e inclui cada palavra OOV no recurso de palavras do modelo customizado. Para extrair o máximo de significado do conteúdo, o serviço tokeniza e analisa os dados que lê de um arquivo de corpus. As seções a seguir descrevem como o serviço analisa um arquivo de corpus para cada idioma suportado.

#### Análise de inglês, francês, alemão, espanhol e português do Brasil
{: #corpusLanguages}

As descrições a seguir se aplicam ao inglês dos EUA e do Reino Unido, francês, alemão, espanhol e português do Brasil.

-   Converte os números em suas palavras equivalentes, por exemplo:
    -   *Para inglês,*`500` se torna `five hundred` e `0.15` se torna `zero point fifteen`.
    -   *Para francês,* `500` se torna `cinq cents` e `0,15` se torna <code>z&eacute;ro quinze</code>.
    -   *Para alemão,* `500`se torna <code>f&uuml;nfhundert</code> e `0,15` se torna <code>null punkt f&uuml;nfzehn</code>.
    -   *Para espanhol,* `500` se torna `quinientos` e `0,15` se torna `cero coma quince`.
    -   *Para português do Brasil,*`500` se torna `quinhentos` e `0,15` se torna `zero ponto quinze`.
-   Converte tokens que incluem determinados símbolos para representações de sequência significativas, por exemplo:
    -   Converte um `$` (sinal de dólar) e um número:
        -   *Para inglês,*`$100` se torna `one hundred dollars`.
        -   *Para Francês,*`$100` se torna `cent dollar`.
        -   *Para alemão,*`$100` e `100$` se tornam `einhundert dollar`.
        -   *Para espanhol,* `$100` e `100$` se tornam <code>cien d&oacute;lares</code> (ou `cien pesos` se o dialeto for `es-LA`).
        -   *Para português do Brasil,* `$100` e `100$` se tornam <code>cem d&oacute;lares</code>.
    -   Converte um <code>&euro;</code> (sinal do euro) e um número:
        -   *Para inglês,* <code>&euro;100</code> se torna `one hundred euros`.
        -   *Para francês,* <code>&euro;100</code> se torna `cent euros`.
        -   *Para alemão,* <code>&euro;100</code> e <code>100&euro;</code> se tornam `einhundert euro`.
        -   *Para espanhol,* <code>&euro;100</code> e <code>100&euro;</code> se tornam `cien euros`.
        -   *Para português do Brasil,* <code>&euro;100</code> e <code>100&euro;</code> se tornam `cem euros`.
    -   Converte um `%` (sinal de percentual) precedido por um número:
        -   *Para inglês,*`100%` se torna `one hundred percent`.
        -   *Para francês,*`100%` se torna `cent pourcent`.
        -   *Para alemão,*`100%` se torna `einhundert prozent`.
        -   *Para espanhol,*`100%` se torna `cien por ciento`.
        -   *Para português do Brasil,*`100%` se torna `cem por cento`.

    Essa lista não está completa. O serviço faz ajustes semelhantes para outros caracteres, conforme necessário.
-   Processa caracteres não alfanuméricos, pontuação e caracteres especiais, dependendo de seu contexto. Por exemplo, o serviço removerá um `$` (sinal de dólar) ou <code>&euro;</code> (símbolo do euro) a menos que ele seja seguido por um número. O processamento é dependente do contexto e consistente entre os idiomas suportados.
-   Ignora frases entre parênteses `( )`, `< >` (sinais de maior e menor), `[ ]` (colchetes) ou `{ }` (chaves).

#### Análise do japonês
{: #corpusLanguages-jaJP}

-   Converte todos os caracteres em caracteres de largura total.
-   Converte números para suas palavras equivalentes, por exemplo, `500` se torna <code>&#20116;&#30334;</code> e `0.15` se torna <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Não converte tokens que incluem símbolos para sequências equivalentes, por exemplo, `100%` se torna <code>&#30334;&#65285;</code>.
-   Não remove automaticamente a pontuação. A {{site.data.keyword.IBM_notm}} recomenda altamente que você remova a pontuação se seu aplicativo for baseado em transcrição, contrário ao ditado.

#### Análise de coreano
{: #corpusLanguages-koKR}

-   Converte números em suas palavras equivalentes, por exemplo, <code>10</code> se torna <code>&#49901;</code>.
-   Remove a pontuação a seguir e os caracteres especiais: `- ( ) * : . , ' "`. No entanto, nem todas as pontuações e caracteres especiais que são removidos para outros idiomas são removidos para coreano, por exemplo:
    -   Remove um símbolo de ponto (`.`) somente quando ele ocorre no final de uma linha de entrada.
    -   Não remove um símbolo de til (`~`).
    -   Não remove ou, de outra forma, processa símbolos de caracteres largos Unicode, por exemplo, <code>&#8230;</code> (ponto triplo ou reticências).

    Em geral, a {{site.data.keyword.IBM_notm}} recomenda que você remova pontuação, caracteres especiais e caracteres largos Unicode antes de processar um arquivo de corpus.
-   Não remove ou ignora frases entre parênteses `( )`, `< >` (sinais de maior e menor), `[ ]` (colchetes) ou `{ }` (chaves).
-   Converte tokens que incluem determinados símbolos para representações de sequência significativas, por exemplo:
    -   `24%` se torna <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>.
    -   `$10` se torna <code>&#49901;&#45804;&#47084;</code>.

    Essa lista não está completa. O serviço faz ajustes semelhantes para outros caracteres, conforme necessário.
-   Para frases que consistem em caracteres em latim (inglês) ou uma mistura de caracteres em Hangul e latim, o serviço cria palavras OOV para as frases exatamente como elas aparecem no arquivo de corpus. E isso cria pronúncias para as palavras com base nas transcrições de Hangul.
   - Ele dá à palavra OOV `London` uma pronúncia de <code>&#47088;&#45912;</code>.
   - Ele dá à palavra OOV <code>IBM&#54856;&#54168;&#51060;&#51648;</code> uma pronúncia de <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>.

## Trabalhando com palavras customizadas
{: #workingWords}

É possível usar os métodos `POST /v1/customizations/{customization_id}/words` e `PUT /v1/customizations/{customization_id}/words/{word_name}` para incluir novas palavras em um recurso de palavras de um modelo customizado. Também é possível usar os métodos para modificar ou aprimorar uma palavra em um recurso de palavras.

Por exemplo, pode ser necessário usar os métodos para corrigir um erro tipográfico ou outro erro que foi cometido ao incluir uma palavra de um corpus. Também pode ser necessário incluir definições de pronúncia para uma palavra existente. Se você modificar uma palavra existente, os novos dados que você fornecer sobrescreverão a definição existente da palavra no recurso de palavras. As regras para incluir uma palavra também se aplicam à modificação de uma palavra existente.

### Codificação de caracteres
{: #charEncoding}

Em geral, é provável que você inclua a maioria das palavras customizadas dos corpora. Certifique-se de que você conheça a codificação de caracteres usada nos arquivos de texto para seus corpora. O serviço preserva a codificação que ele localiza nos arquivos de texto.

Deve-se usar essa codificação ao trabalhar com as palavras individuais no modelo de idioma customizado. Ao especificar uma palavra com o método `GET`, `PUT` ou `DELETE /v1/customizations/{customization_id}/words/{word_name}`, deve-se codificar a URL do `word_name` que você transmite na URL se a palavra inclui caracteres não ASCII.

Por exemplo, a tabela a seguir mostra o que parece ser a mesma letra em duas codificações diferentes, ASCII e UTF-8. É possível passar o caractere ASCII em uma URL como `z`. Deve-se passar o caractere UTF-8 como `%EF%BD%9A`.

<table>
  <caption>Tabela 1. Exemplos de codificação de caracteres</caption>
  <tr>
    <th style="text-align:left">Letra</th>
    <th style="text-align:center">Codificação</th>
    <th style="text-align:center">Valor</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a` (`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      Hexadecimal UTF-8
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### Usando o campo sounds_like
{: #soundsLike}

O campo `sounds_like` especifica como uma palavra é pronunciada por falantes. Por padrão, o serviço conclui automaticamente o campo com a ortografia da palavra. É possível fornecer até cinco pronúncias alternativas para uma palavra que seja difícil de pronunciar ou que possa ser pronunciada de diferentes maneiras. Considere usar o campo para

-   *Fornecer pronúncias diferentes para acrônimos.* Por exemplo, o acrônimo `NCAA` pode ser pronunciado como é escrito ou coloquialmente como *N. C. dois As*. O exemplo a seguir inclui ambas as pronúncias para a palavra `NCAA`:

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *Manipular palavras estrangeiras.* Por exemplo, a palavra francesa <code>gar&ccedil;on</code> contém um caractere que não está localizado no idioma inglês. É possível especificar uma pronúncia de `gaarson`, substituindo <code>&ccedil;</code> por `s`, para dizer ao serviço como os falantes do inglês pronunciarão a palavra.

O reconhecimento de voz usa algoritmos estatísticos para analisar o áudio, portanto, incluir uma palavra não garante que o serviço a transcodifique com precisão completa. Ao incluir uma palavra, considere como ela pode ser pronunciada. Use o campo `sounds_like` para fornecer várias pronúncias que refletem como uma palavra pode ser falada. As seções a seguir fornecem diretrizes específicas do idioma para especificar uma pronúncia.

#### Diretrizes para inglês (EUA e Reino Unido)
{: #wordLanguages-enUS-enGB}

*Diretrizes para inglês dos EUA e do Reino Unido:*

-   Use caracteres alfabéticos em inglês: `a-z` e `A-Z`.
-   Use palavras reais ou inventadas que sejam pronunciáveis em inglês para palavras difíceis de pronunciar, por exemplo, `shuchesnie` para a palavra `Sczcesny`.
-   Substitua letras em inglês equivalentes por letras não em inglês, por exemplo, `s` para <code>&ccedil;</code> ou `ny` para <code>&ntilde;</code>.
-   Substitua letras não acentuadas para letras acentuadas, por exemplo, `a` para <code>&agrave;</code> ou `e` para <code>&egrave;</code>.
-   É possível incluir múltiplas palavras que são separadas por espaços, mas o serviço impõe um máximo de 40 caracteres totais, sem incluir os espaços.

*Diretrizes para inglês dos EUA apenas:*

-   Para pronunciar uma única letra, use a letra seguida por um ponto. Se o ponto for seguido por outro caractere, certifique-se de usar um espaço entre o ponto e o próximo caractere. Por exemplo, use `N. C. A. A.`, *não*`N.C.A.A.`
-   Use a ortografia de números, por exemplo, `setenta e cinco` para `75`.

*Diretrizes para inglês do Reino Unido somente:*

-   **Não é possível** usar pontos ou traços em pronúncias para o inglês do Reino Unido.
-   Para pronunciar uma única letra, use a letra seguida por um espaço. Por exemplo, use `N C A A`, *não*`N. C. A. A.`, `N.C.A.A.` ou `NCAA`.
-   Use a ortografia de números sem traços, por exemplo, `seventy five` para `75`.

#### Diretrizes para francês, alemão, espanhol e português do Brasil
{: #wordLanguages-esES-frFR}

-   **Não é possível** usar traços em pronúncias.
-   Use caracteres alfabéticos que são válidos para o idioma: `a-z` e `A-Z`, incluindo letras acentuadas válidas.
-   Para pronunciar uma única letra, use a letra seguida por um ponto. Se o ponto for seguido por outro caractere, certifique-se de usar um espaço entre o ponto e o próximo caractere. Por exemplo, use `N. C. A. A.`, *não*`N.C.A.A.`
-   Use palavras reais ou inventadas que sejam pronunciáveis no idioma para palavras que são difíceis de pronunciar.
-   Use a ortografia de números sem traços, por exemplo, para `75`, use
    -   *Francês:* `soixante quinze`
    -   *Alemão:* <code>f&uuml;nfundsiebzig</code>
    -   *Espanhol* `setenta y cinco`
    -   *Português do Brasil* `setenta e cinco`
-   É possível incluir múltiplas palavras que são separadas por espaços, mas o serviço impõe um máximo de 40 caracteres totais, sem incluir os espaços.

#### Diretrizes para japonês
{: #wordLanguages-jaJP}

-   Use somente caracteres katakana da largura total usando o símbolo de comprimento <code>&#8213;</code> (*chou-on* ou &#38263;&#38899;, em japonês). Não use caracteres de meia largura.
-   Use sons contraídos (*yoh-on* ou &#25303;&#38899;, em japonês) somente nos seguintes contextos silábicos:

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Use somente as sílabas a seguir após um som assimilado (*soku-on* ou &#20419;&#38899;, em japonês):

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   Não use <code>&#12531;</code> como o primeiro caractere de uma palavra. Por exemplo, use <code>&#12454;&#12540;&#12531;&#12488;</code> em vez de <code>&#12531;&#12540;&#12488;</code>, o último do qual é inválido.
-   Muitas palavras compostas consistem em *prefixo+substantivo* ou *substantivo+prefixo*. O vocabulário base do serviço abrange a maioria das palavras compostas que ocorrem frequentemente (por exemplo, <code>&#x9577;&#x96FB;&#x8A71;</code> e <code>&#x53E4;&#x65B0;&#x805E;</code>), mas não aquelas palavras compostas que ocorrem esporadicamente. Se comumente seu corpus contém palavras compostas, inclua-as como uma palavra, como a primeira etapa da customização. Por exemplo, <code>&#x53E4;&#x925B;&#x7B46;</code> não é comum em texto geral japonês. Se você o usar com frequência, inclua-o em seu modelo customizado para melhorar a precisão da transcrição.
-   Não use um som assimilado final.

#### Diretrizes para coreano
{: #wordLanguages-koKR}

-   Use caracteres Hangul coreano, símbolos e sílabas.
-   Também é possível usar caracteres alfabéticos em latim (inglês): `a-z` e `A-Z`.
-   Não use nenhum caractere ou símbolo que não esteja incluído nos conjuntos anteriores.

### Usando o campo display_as
{: #displayAs}

O campo `display_as` especifica como uma palavra é exibida em uma transcrição. Ele é destinado a casos em que você deseja que o serviço exiba uma sequência que seja diferente da ortografia da palavra. Por exemplo, é possível indicar que a palavra `hhonors` deve ser exibida como `HHonors`, independentemente de soar como `hilton honors` ou `h honors`.

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

Como outro exemplo, é possível indicar que a palavra `IBM` deve ser exibida como <code>IBM&trade;</code>.

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### Interação com formatação inteligente e edição de dados numéricos
{: #displaySmart}

Se você usar os parâmetros `smart_formatting` ou `redaction` com uma solicitação de reconhecimento, esteja ciente de que o serviço aplicará formatação inteligente e edição de dados a uma palavra antes de considerar o campo `display_as` para a palavra. Pode ser necessário experimentar com resultados para assegurar que os recursos não interfiram com a forma de exibição de suas palavras customizadas. Também pode ser necessário incluir palavras customizadas para acomodar os efeitos.

Por exemplo, suponha que você inclua a palavra customizada `one` com um campo `display_as` de `one`. A formatação inteligente muda a palavra `one` para o número `1` e o valor de exibição não é aplicado. Para uma solução alternativa desse problema, é possível incluir uma palavra customizada para o número `1` e aplicar o mesmo campo `display_as` a essa palavra.

Para obter mais informações sobre como trabalhar com esses recursos, consulte [Formatação inteligente](/docs/services/speech-to-text/output.html#smart_formatting) e [Edição de dados numéricos](/docs/services/speech-to-text/output.html#redaction).

### O que acontece ao incluir ou modificar uma palavra customizada?
{: #parseWord}

Como o serviço responde a uma solicitação para incluir ou modificar uma palavra customizada depende dos valores que você fornece e se a palavra existe no vocabulário base do serviço.

<table>
  <caption>Tabela 1. Incluindo palavras customizadas com campos diferentes</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom">O campo <code>sounds_like</code> é...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom">O campo <code>display_as</code> é...</th>
    <th style="text-align:left; vertical-align:bottom">Resposta</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Não especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      Não especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se a palavra não existir no vocabulário base do serviço,</em> o serviço configurará o campo <code>sounds_like</code> para a pronúncia dele da palavra. Ele configura o campo <code>display_as</code> para o valor do campo <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se a palavra existir no vocabulário base do serviço,</em> o serviço deixará os campos <code>sounds_like</code> e <code>display_as</code> vazios. Esses campos estarão vazios somente se a palavra existir no vocabulário base do serviço. A presença da palavra no recurso de palavras do modelo é inofensiva, mas desnecessária.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      Não especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se o campo <code>sounds_like</code> for válido,</em> o serviço configurará o campo <code>display_as</code> para o valor do campo <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se o campo <code>sounds_like</code> for inválido:</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              O método <code>POST
                /v1/customizations/{customization_id}/words</code> incluirá um campo <code>error</code> na palavra no recurso de palavras do modelo.
            </li>
            <li style="margin:10px 0px; line-height:120%">
              O método <code>PUT
                /v1/customizations/{customization_id}/words/{word_name}</code> falha com um código de resposta 400 e uma mensagem de erro. O serviço não inclui a palavra no recurso de palavras.
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Não especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se a palavra não existir no vocabulário base do serviço,</em> o serviço configurará o campo <code>sounds_like</code> para sua pronúncia da palavra e deixará o campo <code>display_as</code> conforme especificado.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se a palavra existir no vocabulário base do serviço,</em> o serviço deixará <code>sounds_like</code> vazio e o campo <code>display_as</code> conforme especificado.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Se o campo <code>sounds_like</code> for válido,</em> o serviço configurará os campos <code>sounds_like</code> e <code>display_as</code> para os valores especificados.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Se o campo <code>sounds_like</code> for inválido,</em> o serviço responderá como ele faz no caso em que o campo <code>sounds_like</code> é especificado, mas o campo <code>display_as</code> não.
        </li>
      </ul>
    </td>
  </tr>
</table>

## Validando um recurso de palavras
{: #validateModel}

Especialmente quando você inclui um corpus em um modelo de idioma customizado ou inclui múltiplas palavras customizadas de uma vez, examine as palavras OOV no recurso de palavras do modelo.

-   *Procure por erros tipográficos e outros.* Especialmente quando você inclui os corpora, que podem ser grandes, erros são facilmente cometidos. Os erros tipográficos em um arquivo de corpus (ou gramática) têm a consequência indesejada de incluir novas palavras em um recurso de palavras de um modelo, como fazem as tags HTML incorretamente formadas que são deixadas em um arquivo de corpus.
-   *Verifique as pronúncias.* O serviço gera pronúncias para palavras OOV automaticamente. Na maioria dos casos, essas pronúncias são suficientes. Mas para palavras que têm ortografia incomum ou são difíceis de pronunciar, e para acrônimos e termos técnicos, é recomendado revisar as pronúncias para precisão.

Para validar e, se necessário, corrigir uma palavra para um modelo customizado, independentemente de como ela foi incluída no recurso de palavras, use os métodos a seguir:

-   Liste todas as palavras de um modelo customizado usando o método `GET /v1/customizations/{customization_id}/words` ou consulte uma palavra individual com o método `GET /v1/customizations/{customization_id}/words/{word_name}`. Para obter mais informações, consulte [Listando palavras de um modelo de idioma customizado](/docs/services/speech-to-text/language-words.html#listWords).
-   Modifique as palavras em um modelo customizado para corrigir erros ou para incluir valores de pronúncia ou exibição usando o método `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}`. Para obter mais informações, consulte [Trabalhando com palavras customizadas](#workingWords).
-   Exclua palavras estranhas introduzidas com erro (por exemplo, erros tipográficos ou outros em um corpus) usando o método `DELETE /v1/customizations/{customization_id}/words/{word_name}`. Para obter mais informações, consulte [Excluindo uma palavra de um modelo de idioma customizado](/docs/services/speech-to-text/language-words.html#deleteWord).
    -   Se a palavra foi extraída de um corpus, será possível, em vez disso, atualizar o arquivo de texto do corpus para corrigir o erro e, em seguida, recarregar o arquivo usando o parâmetro `allow_overwrite` do método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`. Para obter mais informações, consulte [Incluir um corpus no modelo de idioma customizado](/docs/services/speech-to-text/language-create.html#addCorpus).
    -   Se a palavra foi extraída de uma gramática, será possível atualizar o arquivo de gramática para corrigir o erro e, em seguida, recarregar o arquivo usando o parâmetro `allow_overwrite` do método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`. Para obter mais informações, consulte [Incluir uma gramática no modelo de idioma customizado](/docs/services/speech-to-text/grammar-add.html#addGrammar).
