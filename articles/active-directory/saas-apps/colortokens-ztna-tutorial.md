---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory com o ColorTokens ZTNA   Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o ColorTokens ZTNA.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/15/2020
ms.author: jeedes
ms.openlocfilehash: 4c823e008251a612866676f1947fb1a0f6c19655
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88525550"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-colortokens-ztna"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao ColorTokens ZTNA

Neste tutorial, você aprenderá a integrar o ColorTokens ZTNA ao Azure AD (Azure Active Directory). Com a integração do ColorTokens ZTNA ao Azure AD, você pode:

* Controlar no Azure AD quem tem acesso ao ColorTokens ZTNA.
* Permitir que os usuários sejam conectados automaticamente ao ColorTokens ZTNA com suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura do ColorTokens ZTNA habilitada para SSO (logon único).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O ColorTokens ZTNA dá suporte ao SSO iniciado por **SP**
* Após configurar o ColorTokens ZTNA, você poderá impor controles de sessão, que protegem contra o vazamento e a infiltração de dados confidenciais de sua organização em tempo real. O controle da sessão é estendido do Acesso Condicional. [Saiba como impor o controle de sessão com o Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-colortokens-ztna-from-the-gallery"></a>Adicionando o ColorTokens ZTNA da galeria

Para configurar a integração do ColorTokens ZTNA ao Azure AD, você precisará adicionar o ColorTokens ZTNA da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria**, digite **ColorTokens ZTNA** na caixa de pesquisa.
1. Selecione **ColorTokens ZTNA** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-colortokens-ztna"></a>Configurar e testar o logon único do Azure AD para o ColorTokens ZTNA

Configure e teste o SSO do Azure AD com o ColorTokens ZTNA usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do ColorTokens ZTNA.

Para configurar e testar o SSO do Azure AD com o ColorTokens ZTNA, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do ColorTokens ZTNA](#configure-colortokens-ztna-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do ColorTokens ZTNA](#create-colortokens-ztna-test-user)** – para ter um equivalente de B.Fernandes no ColorTokens ZTNA que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **ColorTokens ZTNA**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML**, insira os valores para os seguintes campos:

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<tenantname>.spectrum.colortokens.com`


    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com a URL de Resposta, o Identificador e a URL de Logon reais. Contate a [equipe de suporte ao cliente do ColorTokens ZTNA](mailto:support@colortokens.com) para obter esses valores. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. Seu aplicativo ColorTokens ZTNA espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados à sua configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão.

    ![image](common/default-attributes.png)

1. Além do indicado acima, o aplicativo ColorTokens ZTNA espera que mais alguns atributos sejam passados novamente na resposta SAML, que são mostrados abaixo. Esses atributos também são pré-populados, mas você pode examiná-los de acordo com seus requisitos.

    | Nome | Atributo de Origem|
    | ------------- | --------- |
    | department | user.userprincipalname |
    | Agrupar | user.groups |

    > [!NOTE]
    > Clique [aqui](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) para saber como criar funções no Azure AD.

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, localize **XML de Metadados de Federação** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/metadataxml.png)

1. Na seção **Configurar o ColorTokens ZTNA**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure concedendo a ela acesso ao ColorTokens ZTNA.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **ColorTokens ZTNA**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-colortokens-ztna-sso"></a>Configurar o SSO do ColorTokens ZTNA

Para configurar o logon único no lado do **ColorTokens ZTNA**, é necessário enviar o **XML de Metadados de Federação** baixado e as URLs copiadas apropriadas do portal do Azure para a [equipe de suporte do ColorTokens ZTNA](mailto:support@colortokens.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-colortokens-ztna-test-user"></a>Criar usuário de teste do ColorTokens ZTNA

Nesta seção, você criará um usuário chamado Brenda Fernandes no ColorTokens ZTNA. Trabalhe com a  [equipe de suporte do ColorTokens ZTNA](mailto:support@colortokens.com) para adicionar os usuários à plataforma do ColorTokens ZTNA. Os usuários devem ser criados e ativados antes de usar o logon único.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do ColorTokens ZTNA no Painel de Acesso, você deverá ser conectado automaticamente ao ColorTokens ZTNA, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimentar o ColorTokens ZTNA com o Azure AD](https://aad.portal.azure.com/)

- [O que é controle de sessão no Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Como proteger o ColorTokens ZTNA com visibilidade e controles avançados](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
