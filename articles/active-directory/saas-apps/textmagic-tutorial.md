---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao TextMagic | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o TextMagic.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.openlocfilehash: 81dc3a4f55b214ea6ca7ad0388e57f4b4fdc7124
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88546467"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-textmagic"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao TextMagic

Neste tutorial, você aprenderá a integrar o TextMagic ao Azure AD (Azure Active Directory). Ao integrar o TextMagic ao Azure AD, você pode:

* Controlar no Azure AD quem tem acesso ao TextMagic.
* Permitir que os usuários sejam conectados automaticamente ao TextMagic com as contas do Azure AD deles.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO (logon único) do TextMagic.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O TextMagic é compatível com o SSO iniciado por **IDP**

* O TextMagic é compatível com o provisionamento de usuário **just-in-time**

> [!NOTE]
> O identificador desse aplicativo é um valor de cadeia de caracteres fixo; portanto apenas uma instância pode ser configurada em um locatário.

## <a name="adding-textmagic-from-the-gallery"></a>Adicionando TextMagic da galeria

Para configurar a integração do TextMagic ao Azure AD, você precisa adicionar o TextMagic da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar por meio da galeria**, digite **TextMagic** na caixa de pesquisa.
1. Selecione **TextMagic** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-textmagic"></a>Configurar e testar o logon único do Azure AD para o TextMagic

Configure e teste o SSO do Azure AD com o TextMagic usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do TextMagic.

Para configurar e testar o SSO do Azure AD com o TextMagic, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do TextMagic](#configure-textmagic-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do TextMagic](#create-textmagic-test-user)** – para ter um equivalente de B.Fernandes no TextMagic que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **TextMagic**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML**, insira os valores para os seguintes campos:

    Na caixa de texto **Identificador**, digite uma URL: `https://my.textmagic.com/saml/metadata`

5. O aplicativo TextMagic espera as declarações SAML em um formato específico, o que exige a adição de mapeamentos de atributo personalizado à configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão, em que **nameidentifier** é mapeado com **user.userprincipalname**. O aplicativo TextMagic espera que **nameidentifier** seja mapeado com **user.mail**. Portanto, é necessário editar o mapeamento de atributo clicando no ícone **Editar** e alterar o mapeamento de atributo.

    ![image](common/edit-attribute.png)

1. Além do indicado acima, o aplicativo TextMagic espera que mais alguns atributos sejam passados novamente na resposta SAML, que são mostrados abaixo. Esses atributos também são pré-populados, mas você pode examiná-los de acordo com seu requisito.

    | Nome |   Atributo de Origem| Namespace  |
    | --------------- | --------------- | --------------- |
    | company | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | firstName               | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName            | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | phone               | user.telephonenumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, localize **Certificado (Base64)** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

1. Na seção **Configurar o TextMagic**, copie as URLs apropriadas de acordo com suas necessidades.

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

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure permitindo acesso ao TextMagic.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **TextMagic**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="configure-textmagic-sso"></a>Configurar o SSO do TextMagic

1. Para automatizar a configuração no TextMagic, é necessário instalar a **extensão do navegador Entrada Segura dos Meus Aplicativos** clicando em **Instalar a extensão**.

    ![Extensão Meus Aplicativos](common/install-myappssecure-extension.png)

2. Após a adição da extensão ao navegador, um clique em **Configurar o TextMagic** direcionará você ao aplicativo TextMagic. Nele, forneça as credenciais de administrador para entrar no TextMagic. A extensão do navegador configurará automaticamente o aplicativo e automatizará as etapas de 3 a 5.

    ![Configuração da instalação](common/setup-sso.png)

3. Caso deseje configurar o TextMagic manualmente, abra uma nova janela do navegador da Web, entre em seu site de empresa do TextMagic como administrador e execute as seguintes etapas:

4. Selecione **Configurações da conta** sob o nome de usuário.

    ![Configuração do TextMagic](./media/textmagic-tutorial/config1.png)

5. Clique na guia **SSO (Logon Único)** e preencha os campos a seguir:  

    ![Configuração do TextMagic](./media/textmagic-tutorial/config2.png)

    a. Na caixa de texto **ID da Entidade do Provedor de Identidade:** , cole o valor de **Identificador do Azure AD**, copiado do portal do Azure.

    b. Na caixa de texto **URL de SSO do Provedor de Identidade:** , cole o valor da **URL de Logon** que você copiou do portal do Azure.

    c. Na caixa de texto **URL de SLO do Provedor de Identidade:** , cole o valor da **URL de Logoff** que você copiou do portal do Azure.

    d. Abra seu certificado codificado em **base-64** no bloco de notas baixado do portal do Azure, copie o conteúdo dele para a área de transferência e cole-o na caixa de texto **Certificado Público x509**.

    e. Clique em **Save** (Salvar).

### <a name="create-textmagic-test-user"></a>Criar usuário de teste do TextMagic

O aplicativo dá suporte ao **provisionamento de usuário just-in-time** e, após a autenticação, os usuários serão automaticamente criados no aplicativo. Você precisa preencher as informações uma vez no primeiro logon para ativar a subconta no sistema.
Não há itens de ação para você nesta seção.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do TextMagic no Painel de Acesso, você deverá entrar automaticamente no TextMagic no qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimentar o TextMagic com o Azure AD](https://aad.portal.azure.com/)

