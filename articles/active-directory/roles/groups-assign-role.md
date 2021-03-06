---
title: Atribuir uma função a um grupo de nuvem no Azure Active Directory | Microsoft Docs
description: Atribua uma função do Azure AD a um grupo de funções atribuídas no portal do Azure, no PowerShell ou no API do Graph.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28df0e420183239eae21aa18c807bb82b4ec7649
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92374469"
---
# <a name="assign-a-role-to-a-cloud-group-in-azure-active-directory"></a>Atribuir uma função a um grupo de nuvem no Azure Active Directory

Esta seção descreve como um administrador de ti pode atribuir a função Azure Active Directory (Azure AD) a um grupo do Azure AD.

## <a name="using-azure-ad-admin-center"></a>Usando o centro de administração do Azure AD

A atribuição de um grupo a uma função do Azure AD é semelhante à atribuição de usuários e entidades de serviço, exceto que somente os grupos que podem ser atribuídas por função poderão ser usados. No portal do Azure, somente os grupos que podem ser atribuído por função são exibidos.

1. Entre no centro de [Administração do Azure ad](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) com permissões de administrador de função privilegiada ou de administrador global na organização do Azure AD.

1. Selecione **Azure Active Directory**  >  **funções e administradores**e selecione a função que você deseja atribuir.

1. Na página ** _nome da função_*_, selecione > _* adicionar atribuição**.

   ![Adicionar a nova atribuição de função](./media/groups-assign-role/add-assignment.png)

1. Selecione o grupo. Somente os grupos que podem ser atribuídos às funções do Azure AD são exibidos.

    [![Somente os grupos que são atribuíveis são mostrados para uma nova atribuição de função.](./media/groups-assign-role/eligible-groups.png "Somente os grupos que são atribuíveis são mostrados para uma nova atribuição de função.")](./media/groups-assign-role/eligible-groups.png#lightbox)

1. Selecione **Adicionar**.

Para obter mais informações sobre a atribuição de permissões de função, consulte [atribuir funções de administrador e não administrador aos usuários](../fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="using-powershell"></a>Usando o PowerShell

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Criar um grupo que possa ser atribuído à função

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true 
```

### <a name="get-the-role-definition-for-the-role-you-want-to-assign"></a>Obter a definição de função para a função que você deseja atribuir

```powershell
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Helpdesk Administrator'" 
```

### <a name="create-a-role-assignment"></a>Criar uma atribuição de função

```powershell
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.Id 
```

## <a name="using-microsoft-graph-api"></a>Usando a API Microsoft Graph

### <a name="create-a-group-that-can-be-assigned-azure-ad-role"></a>Criar um grupo que pode ser atribuído à função do Azure AD

```
POST https://graph.microsoft.com/beta/groups
{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD.",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [
"Unified"
],
"mailEnabled": true,
"securityEnabled": true
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true,
}
```

### <a name="get-the-role-definition"></a>Obter a definição de função

```
GET https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions?$filter = displayName eq ‘Helpdesk Administrator’
```

### <a name="create-the-role-assignment"></a>Criar a atribuição de função

```
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
{
"principalId":"<Object Id of Group>",
"roleDefinitionId":"<ID of role definition>",
"directoryScopeId":"/"
}
```
## <a name="next-steps"></a>Próximas etapas

- [Usar grupos de nuvem para gerenciar atribuições de função](groups-concept.md)
- [Solução de problemas de funções atribuídas a grupos de nuvem](groups-faq-troubleshooting.md)
