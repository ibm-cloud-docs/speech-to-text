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

# Segurança de informações
{: #information-security}

A {{site.data.keyword.IBM}} está comprometida em fornecer aos nossos clientes e parceiros as soluções inovadoras de privacidade de dados, segurança e governança.
{: shortdesc}

Os clientes são responsáveis por assegurar sua própria conformidade com várias leis e regulamentos, incluindo o Regulamento Geral de Proteção de Dados da União Europeia. Os clientes são os únicos
responsáveis por obter consultoria jurídica competente quanto à identificação e interpretação de
quaisquer leis e regulamentações relevantes que possam afetar os negócios dos clientes e quaisquer
ações que os clientes precisem realizar para cumprir tais leis e regulamentações.
{: important}

Os produtos, serviços e outros recursos descritos aqui não são adequados para todas as situações do cliente e podem ter disponibilidade restrita. A {{site.data.keyword.IBM_notm}} não fornece conselho legal, contábil ou de auditoria nem declara ou garante que os seus serviços ou produtos assegurarão que os clientes estejam em conformidade com qualquer lei ou regulamento.

Se você precisar solicitar suporte do GDPR para os recursos do {{site.data.keyword.cloud}} {{site.data.keyword.watson}} que forem criados

-   Na União Europeia, (UE), veja [Solicitando suporte para recursos do {{site.data.keyword.cloud_notm}} Watson criados na União Europeia](/docs/services/watson/getting-started-gdpr-sar.html#request-EU).
-   Fora da UE, veja [Solicitando suporte para recursos fora da União Europeia](/docs/services/watson/getting-started-gdpr-sar.html#request-non-EU).

## Regulamento Geral sobre a Proteção de Dados (GDPR) da União Europeia
{: #gdpr}

O{{site.data.keyword.IBM_notm}}está comprometido em fornecer aos nossos clientes e parceiros com soluções inovadoras de privacidade de dados, segurança e controle para auxiliá-los em sua jornada para a conformidade com GDPR.

Saiba mais sobre a própria jornada de prontidão do GDPR da {{site.data.keyword.IBM_notm}} e os nossos recursos e ofertas do GDPR para suportar a sua jornada de conformidade [aqui ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/gdpr){: new_window}.

## Rotulando e excluindo dados no serviço Speech to Text
{: #gdpr-speech-to-text}

O serviço {{site.data.keyword.speechtotextfull}} permite excluir todos os dados que estão associados a solicitações de reconhecimento, modelos de idioma customizados e modelos acústicos customizados. Para excluir dados, deve-se fazer o seguinte:

1.  Use o cabeçalho `X-Watson-Metadata` para associar um ID do cliente aos dados que são passados por uma solicitação para o serviço. Consulte [Especificando um ID do cliente](#specify-customer-id).
1.  Use o método `DELETE /v1/user_data` para excluir todos os dados que estão associados a um ID do cliente especificado. Consulte [Excluindo dados](#delete-pi).

Por padrão, nenhum ID do cliente está associado a dados.

Os recursos experimentais e beta não são destinados para uso em um ambiente de produção e, portanto, não têm garantia de funcionamento conforme o esperado ao rotular e excluir dados. Os recursos experimentais e beta não devem ser usados ao implementar uma solução que requer a rotulagem e a exclusão de dados.
{: note}

### Especificando um ID do cliente
{: #specify-customer-id}

Para associar um ID do cliente aos dados, inclua o cabeçalho `X-Watson-Metadata` com a solicitação que transmite as informações. Passe a sequência `customer_id={id}` como o argumento do cabeçalho. O exemplo a seguir associa o ID do cliente `my_customer_ID` com os dados passados com uma solicitação `POST /v1/recognize`:

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

Um ID do cliente pode incluir qualquer caractere, exceto o `;` (ponto e vírgula) e o `=` (sinal de igual). Especifique uma sequência aleatória ou genérica para o ID do cliente. Não especifique uma sequência pessoalmente identificável, como um endereço de e-mail ou um ID do Twitter. É possível especificar diferentes IDs do cliente com diferentes solicitações. Um ID do cliente que você especifica está associado à instância do serviço cujas credenciais são usadas com a solicitação. Apenas as credenciais para essa instância do serviço podem excluir dados associados ao ID.

Use o cabeçalho `X-Watson-Metadata` com os métodos a seguir:

-   Com as solicitações do WebSocket:
    -   `/v1/recognize`

    Especifique o ID do cliente com o parâmetro de consulta `x-watson-metadata` da solicitação para abrir a conexão. Deve-se codificar em URL o argumento para o parâmetro de consulta, por exemplo, `customer_id%3dmy_customer_ID`. O ID do cliente está associado a todos os dados que são transmitidos com as solicitações de reconhecimento enviadas pela conexão.
-   Com solicitações de HTTP síncronas:
    -   `POST /v1/recognize`

    O ID do cliente está associado aos dados que são enviados com a solicitação individual.
-   Com solicitações de HTTP assíncronas:
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    O ID do cliente está associado à URL de retorno de chamada inserida na lista de desbloqueio ou com os dados que são enviados com a solicitação de reconhecimento individual.
-   Com as solicitações para incluir corpora, palavras customizadas ou gramáticas em modelos de idioma customizados:
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    O ID do cliente está associado aos corpora, palavras customizadas ou gramáticas que são incluídas ou atualizadas pela solicitação.
-   Com solicitações para incluir recursos de áudio em modelos acústicos customizados:
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    O ID do cliente está associado com o recurso de áudio que é incluído ou atualizado pela solicitação.

### Excluindo dados
{: #delete-pi}

Para excluir todos os dados que estão associados a um ID do cliente, use o método `DELETE /v1/user_data`. Você passa a sequência `customer_id={id}` como um parâmetro de consulta com a solicitação. O exemplo a seguir exclui todos os dados para o ID do cliente `my_customer_ID`:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

O método `/v1/user_data` exclui todos os dados que estão associados ao ID do cliente especificado, independentemente do método pelo qual as informações foram incluídas. O método não terá efeito se nenhum dado estiver associado ao ID de cliente. Deve-se emitir a solicitação com credenciais para a mesma instância do serviço que foi usada para associar o ID do cliente aos dados.
