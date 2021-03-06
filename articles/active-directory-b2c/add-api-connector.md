---
title: Adicionar conectores de API a fluxos de usuário (visualização)
description: Configure um conector de API para ser usado em um fluxo de usuário.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.openlocfilehash: 857429ab5fd2e2ea9a0cb0173015ceba4bb0bacb
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92504104"
---
# <a name="add-an-api-connector-to-a-sign-up-user-flow-preview"></a>Adicionar um conector de API a um fluxo de usuário de inscrição (versão prévia)

> [!IMPORTANT]
> Os conectores de API para inscrição são um recurso de visualização pública do Azure AD B2C. Para saber mais sobre versões prévias, consulte os [Termos de Uso Complementares para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para usar um [conector de API](api-connectors-overview.md), primeiro crie o conector de API e habilite-o em um fluxo de usuário.

## <a name="create-an-api-connector"></a>Criar um conector de API

1. Entre no [portal do Azure](https://portal.azure.com/).
2. Em **Serviços do Azure**, selecione **Azure ad B2C**.
4. Selecione **conectores de API (versão prévia)** e, em seguida, selecione **novo conector de API**.

   ![Adicionar um novo conector de API](./media/add-api-connector/api-connector-new.png)

5. Forneça um nome de exibição para a chamada. Por exemplo, **valide as informações do usuário**.
6. Forneça a **URL do ponto de extremidade** para a chamada à API.
7. Forneça as informações de autenticação para a API.

   - Somente a autenticação básica tem suporte no momento. Se você quiser usar uma API sem autenticação básica para fins de desenvolvimento, basta inserir um **nome de usuário** e **senha** ' fictícios ' que sua API pode ignorar. Para usar com uma função do Azure com uma chave de API, você pode incluir o código como um parâmetro de consulta na **URL do ponto de extremidade** (por exemplo, https []() ://contoso.azurewebsites.NET/API/Endpoint<b>? Code = 0123456789</b>).

   ![Configurar um novo conector de API](./media/add-api-connector/api-connector-config.png)
8. Selecione **Salvar**.

## <a name="the-request-sent-to-your-api"></a>A solicitação enviada à sua API
Um conector de API se materializa como uma solicitação **http post** , enviando atributos de usuário (' declarações ') como pares de chave-valor em um corpo JSON. Os atributos são serializados da mesma forma para [Microsoft Graph](https://docs.microsoft.com/graph/api/resources/user#properties) Propriedades de usuário. 

**Solicitação de exemplo**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

Somente as propriedades do usuário e os atributos personalizados listados na **Azure ad B2C**  >  experiência de**atributos de usuário** estão disponíveis para serem enviados na solicitação.

Existem atributos personalizados no formato **de \<extensions-app-id> _CustomAttribute extension_**  no diretório. Sua API deve esperar receber declarações nesse mesmo formato serializado. Para obter mais informações sobre atributos personalizados, consulte [definir atributos personalizados em Azure Active Directory B2C](user-flow-custom-attributes.md).

Além disso, a Declaração **localidades da interface do usuário (' ui_locales ')** é enviada por padrão em todas as solicitações. Ele fornece a localidade de um usuário, conforme configurado em seu dispositivo, que pode ser usado pela API para retornar respostas internacionalizadas.

> [!IMPORTANT]
> Se uma declaração não tiver um valor no momento em que o ponto de extremidade de API for chamado, a declaração não será enviada para a API. Sua API deve ser projetada para verificar explicitamente e manipular o caso em que uma declaração não está na solicitação.

> [!TIP] 
> [**identidades (' identidades ')**](https://docs.microsoft.com/graph/api/resources/objectidentity) e as declarações de **endereço de email (' email ')** podem ser usadas pela sua API para identificar um usuário antes que eles tenham uma conta em seu locatário. 

## <a name="enable-the-api-connector-in-a-user-flow"></a>Habilitar o conector de API em um fluxo de usuário

Siga estas etapas para adicionar um conector de API a um fluxo de usuário de inscrição.

1. Entre no [portal do Azure](https://portal.azure.com/).
2. Em **Serviços do Azure**, selecione **Azure ad B2C**.
4. Selecione **fluxos de usuário**e, em seguida, selecione o fluxo de usuário ao qual você deseja adicionar o conector de API.
5. Selecione **conectores de API**e, em seguida, selecione os pontos de extremidade de API que você deseja invocar nas etapas a seguir no fluxo do usuário:

   - **Depois de entrar com um provedor de identidade**
   - **Antes de criar o usuário**

   ![Adicionar APIs ao fluxo do usuário](./media/add-api-connector/api-connectors-user-flow-select.png)

6. Selecione **Salvar**.

## <a name="after-signing-in-with-an-identity-provider"></a>Depois de entrar com um provedor de identidade

Um conector de API nesta etapa no processo de inscrição é invocado imediatamente depois que o usuário é autenticado com um provedor de identidade (como o Google, Facebook, & Azure AD). Esta etapa precede a página de*_coleção de atributos_**, que é o formulário apresentado ao usuário para coletar atributos de usuário. Esta etapa não será invocada se um usuário estiver se registrando com uma conta local.

### <a name="example-request-sent-to-the-api-at-this-step"></a>Exemplo de solicitação enviada para a API nesta etapa
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ 
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "lastName":"Smith",
 "ui_locales":"en-US"
}
```

As declarações exatas enviadas para a API dependem de quais informações são fornecidas pelo provedor de identidade. ' email ' é sempre enviado.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Tipos de resposta esperados da API Web nesta etapa

Quando a API Web recebe uma solicitação HTTP do Azure AD durante um fluxo de usuário, ela pode retornar essas respostas:

- Resposta de continuação
- Resposta de bloqueio

#### <a name="continuation-response"></a>Resposta de continuação

Uma resposta de continuação indica que o fluxo do usuário deve continuar para a próxima etapa: a página coleção de atributos.

Em uma resposta de continuação, a API pode retornar declarações. Se uma declaração for retornada pela API, a declaração fará o seguinte:

- Preenche previamente o campo de entrada na página coleção de atributos.

Consulte um exemplo de uma [resposta de continuação](#example-of-a-continuation-response).

#### <a name="blocking-response"></a>Resposta de bloqueio

Uma resposta de bloqueio sai do fluxo do usuário. Ele pode ser emitido intencionalmente pela API para interromper a continuação do fluxo do usuário, exibindo uma página de bloco para o usuário. A página bloco exibe o `userMessage` fornecido pela API.

Consulte um exemplo de uma [resposta de bloqueio](#example-of-a-blocking-response).

## <a name="before-creating-the-user"></a>Antes de criar o usuário

Um conector de API nesta etapa no processo de inscrição é invocado após a página de coleção de atributos, se um for incluído. Essa etapa é sempre invocada antes de uma conta de usuário ser criada.

<!-- The following are examples of scenarios you might enable at this point during sign-up: -->
<!-- 
- Validate user input data and ask a user to resubmit data.
- Block a user sign-up based on data entered by the user.
- Perform identity verification.
- Query external systems for existing data about the user and overwrite the user-provided value. -->

### <a name="example-request-sent-to-the-api-at-this-step"></a>Exemplo de solicitação enviada para a API nesta etapa

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```
As declarações exatas enviadas para a API dependem de quais informações são coletadas do usuário ou são fornecidas pelo provedor de identidade.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Tipos de resposta esperados da API Web nesta etapa

Quando a API Web recebe uma solicitação HTTP do Azure AD durante um fluxo de usuário, ela pode retornar essas respostas:

- Resposta de continuação
- Resposta de bloqueio
- Resposta de validação

#### <a name="continuation-response"></a>Resposta de continuação

Uma resposta de continuação indica que o fluxo do usuário deve continuar para a próxima etapa: criar o usuário no diretório.

Em uma resposta de continuação, a API pode retornar declarações. Se uma declaração for retornada pela API, a declaração fará o seguinte:

- Substitui qualquer valor que já tenha sido atribuído à declaração da página de coleção de atributos.

Consulte um exemplo de uma [resposta de continuação](#example-of-a-continuation-response).

#### <a name="blocking-response"></a>Resposta de bloqueio
Uma resposta de bloqueio sai do fluxo do usuário. Ele pode ser emitido intencionalmente pela API para interromper a continuação do fluxo do usuário, exibindo uma página de bloco para o usuário. A página bloco exibe o `userMessage` fornecido pela API.

Consulte um exemplo de uma [resposta de bloqueio](#example-of-a-blocking-response).

### <a name="validation-error-response"></a>Validação-resposta de erro
 Quando a API responde com uma resposta de erro de validação, o fluxo do usuário permanece na página coleção de atributos e um `userMessage` é exibido para o usuário. O usuário pode editar e reenviar o formulário. Esse tipo de resposta pode ser usado para validação de entrada.

Consulte um exemplo de uma [resposta de erro de validação](#example-of-a-validation-error-response).

## <a name="example-responses"></a>Respostas de exemplo

### <a name="example-of-a-continuation-response"></a>Exemplo de uma resposta de continuação

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue",
    "postalCode": "12349", // return claim
    "extension_<extensions-app-id>_CustomAttribute": "value" // return claim
}
```

| Parâmetro                                          | Type              | Obrigatório | Descrição                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version                                            | String            | Sim      | A versão da API.                                                                                                                                                                                                                                                                |
| ação                                             | String            | Sim      | O valor precisa ser `Continue`.                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Não       | Os valores retornados podem substituir os valores coletados de um usuário. Eles também podem ser retornados no token, se selecionado como uma declaração de aplicativo _ * * *.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Não       | A declaração não precisa conter `_<extensions-app-id>_` . Os valores retornados podem substituir os valores coletados de um usuário. Eles também podem ser retornados no token, se selecionado como uma **declaração de aplicativo**.  |

### <a name="example-of-a-blocking-response"></a>Exemplo de uma resposta de bloqueio

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
}

```

| Parâmetro   | Type   | Obrigatório | Descrição                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| version     | String | Sim      | A versão da API.                                                    |
| ação      | String | Sim      | O valor deve ser `ShowBlockPage`                                              |
| userMessage | String | Sim      | Mensagem a ser exibida ao usuário.                                            |

**Experiência do usuário final com uma resposta de bloqueio**

![Exemplo de página de bloco](./media/add-api-connector/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>Exemplo de uma resposta de erro de validação



```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code."
}
```

| Parâmetro   | Type    | Obrigatório | Descrição                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| version     | String  | Sim      | A versão da API.                                                    |
| ação      | String  | Sim      | O valor precisa ser `ValidationError`.                                           |
| status      | Integer | Sim      | Deve ser `400` um valor para uma resposta do ValidationError.                        |
| userMessage | String  | Sim      | Mensagem a ser exibida ao usuário.                                            |

> [!NOTE]
> O código de status HTTP deve ser "400", além do valor de "status" no corpo da resposta.

**Experiência do usuário final com uma resposta de erro de validação**

![Página de validação de exemplo](./media/add-api-connector/validation-error-postal-code.png)


## <a name="best-practices-and-how-to-troubleshoot"></a>Práticas recomendadas e como solucionar problemas

### <a name="using-serverless-cloud-functions"></a>Usando funções de nuvem sem servidor
Funções sem servidor, como gatilhos HTTP no Azure Functions, fornecem uma maneira simples de criar pontos de extremidade de API para usar com o conector de API. Você pode usar a função de nuvem sem servidor para, [por exemplo](code-samples.md#api-connectors), executar a lógica de validação e limitar as entradas a domínios de email específicos. A função de nuvem sem servidor também pode chamar e invocar outras APIs da Web, lojas de usuários e outros serviços de nuvem para cenários mais complexos.

### <a name="best-practices"></a>Práticas recomendadas
Verifique se:
* Sua API está seguindo os contratos de solicitação e resposta da API, conforme descrito acima. 
* A **URL do ponto de extremidade** do conector de API aponta para o ponto de extremidade de API correto.
* Sua API verifica explicitamente se há valores nulos de declarações recebidas.
* Sua API responde o mais rápido possível para garantir uma experiência de usuário fluida.
    * Se estiver usando uma função sem servidor ou um serviço Web escalonável, use um plano de hospedagem que mantenha a API "ativo" ou "quente". em produção. Por Azure Functions, é recomendável usar o [plano Premium](../azure-functions/functions-scale.md)


### <a name="use-logging"></a>Usar registro em log
Em geral, é útil usar as ferramentas de log habilitadas pelo serviço de API Web, como o [Application insights](../azure-functions/functions-monitoring.md), para monitorar sua API para códigos de erro inesperados, exceções e baixo desempenho.
* Monitore os códigos de status HTTP que não são HTTP 200 ou 400.
* Um código de status HTTP 401 ou 403 normalmente indica que há um problema com sua autenticação. Verifique a camada de autenticação da API e a configuração correspondente no conector da API.
* Use níveis mais agressivos de registro em log (por exemplo, "rastreamento" ou "depuração") no desenvolvimento, se necessário.
* Monitore sua API para tempos de resposta longos.

## <a name="next-steps"></a>Próximas etapas
<!-- - Learn how to [add a custom approval workflow to sign-up](add-approvals.md) -->
- Comece com nossos [exemplos](code-samples.md#api-connectors).
