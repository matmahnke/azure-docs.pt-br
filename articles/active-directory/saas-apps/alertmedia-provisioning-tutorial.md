---
title: 'Tutorial: configurar o AlertMedia para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como provisionar e desprovisionar automaticamente as contas de usuário do Azure AD para o AlertMedia.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: a5df0dd7-05a3-4744-9d51-ec33e89a934f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2020
ms.author: Zhchia
ms.openlocfilehash: 98cca99ab0e088bbae047fa64ec52429e531dfed
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096063"
---
# <a name="tutorial-configure-alertmedia-for-automatic-user-provisioning"></a>Tutorial: configurar o AlertMedia para o provisionamento automático de usuário

Este tutorial descreve as etapas que você precisa executar tanto no AlertMedia quanto no Azure Active Directory (Azure AD) para configurar o provisionamento automático de usuário. Quando configurado, o Azure AD provisiona e desprovisiona automaticamente usuários e grupos para [AlertMedia](https://www.alertmedia.com/) usando o serviço de provisionamento do Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades com suporte
> [!div class="checklist"]
> * Criar usuários no AlertMedia
> * Remover usuários no AlertMedia quando eles não exigem mais acesso
> * Manter os atributos de usuário sincronizados entre o Azure AD e o AlertMedia
> * Provisionar grupos e associações de grupo no AlertMedia
> * [Logon único](https://docs.microsoft.com/azure/active-directory/saas-apps/alertmedia-tutorial) no AlertMedia (recomendado)

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um [locatário do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Uma conta de usuário no Azure AD com [permissão](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para configurar o provisionamento (por exemplo, Administrador de Aplicativo, Administrador de aplicativos de nuvem, Proprietário de Aplicativo ou Administrador Global). 
* Um [locatário do AlertMedia](https://dashboard.alertmedia.com/#/login).
* Uma conta de usuário no AlertMedia com permissões de administrador para configurar uma integração de API.

## <a name="step-1-plan-your-provisioning-deployment"></a>Etapa 1. Planeje a implantação do provisionamento
1. Saiba mais sobre [como funciona o serviço de provisionamento](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Determine quem estará no [escopo de provisionamento](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Determine quais dados [mapeados entre o Azure AD e o AlertMedia](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-alertmedia-to-support-provisioning-with-azure-ad"></a>Etapa 2. Configurar o AlertMedia para dar suporte ao provisionamento com o Azure AD

1. Faça logon em sua conta do AlertMedia. Navegue até a **API de > da empresa** .
2. Clique em **Adicionar novo** .
3. Opte por dar um nome à sua **integração de API** para ajudá-lo a reconhecer facilmente onde as chaves estão sendo usadas.
4. Selecione o administrador com o qual você deseja associar a integração.
5. Clique no botão **gerar chaves** e **salvar** .
6. Copie e salve o **token do cliente** de sua integração. Isso é usado como o **token secreto** na guia provisionamento do aplicativo AlertMedia no portal do Azure.


## <a name="step-3-add-alertmedia-from-the-azure-ad-application-gallery"></a>Etapa 3. Adicionar o AlertMedia da Galeria de aplicativos do Azure AD

Adicione o AlertMedia da Galeria de aplicativos do Azure AD para começar a gerenciar o provisionamento no AlertMedia. Se você tiver configurado anteriormente o AlertMedia para SSO, poderá usar o mesmo aplicativo. No entanto, recomendamos que você crie um aplicativo diferente ao testar a integração no início. Saiba mais sobre como adicionar um aplicativo da galeria [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Etapa 4. Defina quem estará no escopo de provisionamento 

No Azure AD, é possível definir quem estará no escopo de provisionamento com base na atribuição ao aplicativo ou nos atributos do usuário/grupo. Se você optar por definir quem estará no escopo de provisionamento com base na atribuição, poderá usar as [etapas](../manage-apps/assign-user-or-group-access-portal.md) a seguir para atribuir usuários e grupos ao aplicativo. Se você optar por definir quem estará no escopo de provisionamento com base somente em atributos do usuário ou do grupo, poderá usar um filtro de escopo, conforme descrito [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Ao atribuir usuários e grupos ao AlertMedia, você deve selecionar uma função diferente de **acesso padrão** . Os usuários com a função Acesso Padrão são excluídos do provisionamento e serão marcados como "Não qualificado efetivamente" nos logs de provisionamento. Se a única função disponível no aplicativo for a de acesso padrão, você poderá [atualizar o manifesto do aplicativo](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) para adicionar outras funções. 

* Comece pequeno. Teste com um pequeno conjunto de usuários e grupos antes de implementar para todos. Quando o escopo de provisionamento é definido para usuários e grupos atribuídos, é possível controlar isso atribuindo um ou dois usuários ou grupos ao aplicativo. Quando o escopo é definido para todos os usuários e grupos, é possível especificar um [atributo com base no filtro de escopo](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-alertmedia"></a>Etapa 5. Configurar o provisionamento automático de usuário para o AlertMedia 

Nesta seção, você verá orientações para seguir as etapas de configuração do serviço de provisionamento do Azure AD para criar, atualizar e desabilitar usuários e/ou grupos no TestApp com base em atribuições de usuário e/ou grupo no Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-alertmedia-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para AlertMedia no Azure AD:

1. Entre no [portal do Azure](https://portal.azure.com). Selecione **Aplicativos Empresariais** e **Todos os Aplicativos** .

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **AlertMedia** .

    ![O link do AlertMedia na lista de aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento** .

    ![Guia Provisionamento](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático** .

    ![Guia de provisionamento automático](common/provisioning-automatic.png)

5. Na seção **credenciais de administrador** , insira a **URL do locatário** do AlertMedia como um dos seguintes.
      * (nenhum domínio personalizado) https://docs.gitlab.com/ee/api/scim.html

      * (domínio personalizado) https://developer.github.com/v3/scim/

      Insira o **token secreto** como recuperado anteriormente na etapa 2. Clique em **testar conexão** para garantir que o Azure ad possa se conectar ao AlertMedia. Se a conexão falhar, verifique se sua conta do AlertMedia tem permissões de administrador e tente novamente.

      ![Token](common/provisioning-testconnection-tenanturltoken.png)

6. No campo **Notificação por Email** , insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e marque a caixa de seleção **Enviar uma notificação por email quando ocorrer uma falha** .

    ![Email de notificação](common/provisioning-notification-email.png)

7. Clique em **Salvar** .

8. Na seção **mapeamentos** , selecione **sincronizar Azure Active Directory usuários para AlertMedia** .

9. Examine os atributos de usuário que são sincronizados do Azure AD para o AlertMedia na seção de **mapeamento de atributo** . Os atributos selecionados como propriedades **correspondentes** são usados para corresponder as contas de usuário no AlertMedia para operações de atualização. Se você optar por alterar o [atributo de destino correspondente](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), será necessário garantir que a API AlertMedia dê suporte à filtragem de usuários com base nesse atributo. Selecione o botão **Salvar** para confirmar as alterações.

   |Atributo|Type|
   |---|---|
   |userName|String|
   |ativo|Boolean|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: first_name|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: last_name|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: email|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: EMAIL2|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: email3|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: usuário: title|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: mobile_phone|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: mobile_phone_post_dial|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: mobile_phone2|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: mobile_phone2_post_dial|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: mobile_phone3|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: mobile_phone3_post_dial|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: home_phone|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: home_phone_post_dial|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: office_phone|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: office_phone_post_dial|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: usuário: endereço|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: Endereço2|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: City|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: State|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: usuário: país|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: ZipCode|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: Notes|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: customer_user_id|String|
   |urn: IETF: params: SCIM: esquemas: extensão: alertmedia: 2.0: CustomAttribute: User: user_type|String|

10. Na seção **mapeamentos** , selecione **sincronizar grupos de Azure Active Directory para AlertMedia** .

11. Examine os atributos de grupo que são sincronizados do Azure AD para o AlertMedia na seção de **mapeamento de atributo** . Os atributos selecionados como propriedades **correspondentes** são usados para corresponder os grupos no AlertMedia para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

      |Atributo|Type|
      |---|---|
      |displayName|String|
      |membros|Referência|

12. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar o serviço de provisionamento do Azure AD para o AlertMedia, altere o **status de provisionamento** para **ativado** na seção **configurações** .

    ![Status do provisionamento ativado](common/provisioning-toggle-on.png)

14. Defina os usuários e/ou grupos que você deseja provisionar para o AlertMediaAlertMedia escolhendo os valores desejados no **escopo** na seção **configurações** .

    ![Escopo de provisionamento](common/provisioning-scope.png)

15. Quando estiver pronto para provisionar, clique em **Salvar** .

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação começa o ciclo de sincronização inicial de todos os usuários e grupos definidos no **Escopo** na seção **Configurações** . O ciclo inicial leva mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Azure AD esteja em execução. 

## <a name="step-6-monitor-your-deployment"></a>Etapa 6. Monitorar a implantação
Depois de configurar o provisionamento, use os seguintes recursos para monitorar a implantação:

1. Use os [logs de provisionamento](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) para determinar quais usuários foram provisionados com êxito ou não
2. Confira a [barra de progresso](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) para ver o status do ciclo de provisionamento e saber como fechá-la para concluir
3. Se a configuração de provisionamento parecer estar em um estado não íntegro, o aplicativo entrará em quarentena. Saiba mais sobre os estados de quarentena [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)
