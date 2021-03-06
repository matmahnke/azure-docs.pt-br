---
title: Habilitar extensão de VM usando Azure PowerShell
description: Este artigo descreve como implantar extensões de máquina virtual para servidores habilitados para Arc do Azure em execução em ambientes de nuvem híbrida usando o Azure PowerShell.
ms.date: 10/23/2020
ms.topic: conceptual
ms.openlocfilehash: d2408f75c7b6d81ba297de6dcdb85a712cd8908f
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92495451"
---
# <a name="enable-azure-vm-extensions-using-azure-powershell"></a>Habilitar extensões de VM do Azure usando Azure PowerShell

Este artigo mostra como implantar e desinstalar extensões de VM do Azure, com suporte dos servidores habilitados para Arc do Azure, para uma máquina híbrida do Linux ou Windows usando o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

- Um computador com o PowerShell do Azure. Para obter instruções, consulte [Instalar e configurar o PowerShell do Azure](/powershell/azure/).

Antes de usar Azure PowerShell para gerenciar extensões de VM em seu servidor híbrido gerenciado por servidores habilitados para Arc, você precisa instalar o `Az.ConnectedMachine` módulo. Execute o seguinte comando em seu servidor habilitado para Arc:

`Install-Module -Name Az.ConnectedMachine`.

Quando a instalação for concluída, a seguinte mensagem será retornada:

`The installed extension `AZ. ConnectedMachine` is experimental and not covered by customer support. Please use with discretion.`

## <a name="enable-extension"></a>Habilitar extensão

Para habilitar uma extensão de VM em seu servidor habilitado para Arc, use [New-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/new-azconnectedmachineextension) com os `-Name` `-ResourceGroupName` parâmetros,, `-MachineName` ,,-e `-Location` `-Publisher` `ExtensionType` `-Settings` .

O exemplo a seguir habilita a extensão de VM Log Analytics em um servidor Linux habilitado para Arc:

```powershell
PS C:\> $Setting = @{ "workspaceId" = "workspaceId" }
PS C:\> $protectedSetting = @{ "workspaceKey" = "workspaceKey" }
PS C:\> New-AzConnectedMachineExtension -Name OMSLinuxAgent -ResourceGroupName "myResourceGroup" -MachineName "myMachine" -Location "eastus" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -TypeHandlerVersion "1.10" -Settings $Setting -ProtectedSetting $protectedSetting -ExtensionType OmsAgentforLinux"
```

O exemplo a seguir habilita a extensão de script personalizado em um servidor habilitado para Arc:

```powershell
PS C:\> $Setting = @{ "commandToExecute" = "powershell.exe -c Get-Process" }
PS C:\> New-AzConnectedMachineExtension -Name custom -ResourceGroupName myResourceGroup -MachineName myMachineName -Location eastus -Publisher "Microsoft.Compute" -TypeHandlerVersion 1.10 -Settings $Setting -ExtensionType CustomScriptExtension
```

## <a name="list-extensions-installed"></a>Listar extensões instaladas

Para obter uma lista das extensões de VM em seu servidor habilitado para Arc, use [Get-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) com `-MachineName` os `-ResourceGroupName` parâmetros e.

Exemplo:

```powershell
Get-AzConnectedMachineExtension -ResourceGroupName myResourceGroup -MachineName myMachineName

Name    Location  PropertiesType        ProvisioningState
----    --------  --------------        -----------------
custom  westus2   CustomScriptExtension Succeeded
```

## <a name="remove-an-installed-extension"></a>Remover uma extensão instalada

Para remover uma extensão de VM instalada em seu servidor habilitado para Arc, use [Remove-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/remove-azconnectedmachineextension) com os `-Name` `-MachineName` `-ResourceGroupName` parâmetros e.

Por exemplo, para remover a extensão de VM Log Analytics para Linux, execute o seguinte comando:

```powershell
Remove-AzConnectedMachineExtension -MachineName myMachineName -ResourceGroupName myResourceGroup -Name OmsAgentforLinux
```

## <a name="next-steps"></a>Próximas etapas

- Você pode implantar, gerenciar e remover extensões de VM usando o [CLI do Azure](manage-vm-extensions-cli.md), dos modelos [portal do Azure](manage-vm-extensions-portal.md)ou [Azure Resource Manager](manage-vm-extensions-template.md).

- As informações de solução de problemas podem ser encontradas no [guia solucionar problemas de extensões de VM](troubleshoot-vm-extensions.md).
