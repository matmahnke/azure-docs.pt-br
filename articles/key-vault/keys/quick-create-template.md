---
title: Guia de Início Rápido do Azure – Criar um Azure Key Vault e uma chave usando o modelo do Azure Resource Manager   Microsoft Docs
description: Guia de início rápido que mostra como criar cofres de chaves do Azure e adicionar uma chave aos cofres usando o modelo do Azure Resource Manager.
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: mvc,subject-armqs
ms.date: 10/14/2020
ms.author: sebansal
ms.openlocfilehash: 55641dacf8f7efb18b479dd4b4253787df540341
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132441"
---
# <a name="quickstart-create-an-azure-key-vault-and-a-key-by-using-arm-template-preview"></a>Início Rápido: Criar um Azure Key Vault e uma chave usando o modelo do ARM (versão prévia)

[O Azure Key Vault](../general/overview.md) é um serviço de nuvem que fornece um armazenamento seguro de segredos, como chaves, senhas e certificados, entre outros. Este guia de início rápido tem como foco o processo de implantação de um modelo do ARM (Azure Resource Manager) para a criação de um cofre de chaves e uma chave.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, será preciso:

* Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

* Sua ID de objeto de usuário do Azure AD é necessária para o modelo configurar permissões. O procedimento a seguir obtém a ID do objeto (GUID).

    1. Execute o comando do Azure PowerShell ou CLI do Azure a seguir selecionando **Experimentar** e colando o script no painel do shell. Para colar o script, clique com o botão direito do mouse no shell e, em seguida, selecione **Colar** .

        # <a name="cli"></a>[CLI](#tab/CLI)
        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --id $upn --query "objectId" &&
        echo "Press [ENTER] to continue ..."
        ```

        # <a name="powershell"></a>[PowerShell](#tab/PowerShell)
        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        Write-Host "Press [ENTER] to continue..."
        ```

        ---

    2. Anote a ID do objeto. Você precisará dela na próxima seção deste início rápido.

## <a name="review-the-template"></a>Examinar o modelo

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the key vault to be created."
            }
        },
        "skuName": {
            "type": "string",
            "defaultValue": "Standard",
            "allowedValues": [
                "Standard",
                "Premium"
            ],
            "metadata": {
                "description": "The SKU of the vault to be created."
            }
        },
        "keyName": {
            "type": "string",
            "metadata": {
                "description": "The name of the key to be created."
            }
        },
        "keyType": {
            "type": "string",
            "metadata": {
                "description": "The JsonWebKeyType of the key to be created."
            }
        },
        "keyOps": {
            "type": "array",
            "defaultValue": [],
            "metadata": {
                "description": "The permitted JSON web key operations of the key to be created."
            }
        },
        "keySize": {
            "type": "int",
            "defaultValue": -1,
            "metadata": {
                "description": "The size in bits of the key to be created."
            }
        },
        "curveName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The JsonWebKeyCurveName of the key to be created."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('vaultName')]",
            "apiVersion": "2019-09-01",
            "location": "[resourceGroup().location]",
            "properties": {
                "enableRbacAuthorization": false,
                "enableSoftDelete": false,
                "enabledForDeployment": false,
                "enabledForDiskEncryption": false,
                "enabledForTemplateDeployment": false,
                "tenantId": "[subscription().tenantId]",
                "accessPolicies": [],
                "sku": {
                    "name": "[parameters('skuName')]",
                    "family": "A"
                },
                "networkAcls": {
                    "defaultAction": "Allow",
                    "bypass": "AzureServices"
                }
            }
        },
        {
            "type": "Microsoft.KeyVault/vaults/keys",
            "name": "[concat(parameters('vaultName'), '/', parameters('keyName'))]",
            "apiVersion": "2019-09-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[resourceId('Microsoft.KeyVault/vaults', parameters('vaultName'))]"
            ],
            "properties": {
                "kty": "[parameters('keyType')]",
                "keyOps": "[parameters('keyOps')]",
                "keySize": "[if(equals(parameters('keySize'), -1), json('null'), parameters('keySize'))]",
                "curveName": "[parameters('curveName')]"
            }
        }
    ],
    "outputs": {
        "proxyKey": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.KeyVault/vaults/keys', parameters('vaultName'), parameters('keyName')))]"
        }
    }
}

```

Encontre mais amostras de modelo do Azure Key Vault nos [Modelos de início rápido do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault&pageNumber=1&sort=Popular).


## <a name="review-deployed-resources"></a>Examinar os recursos implantados

Você pode usar o portal do Azure para verificar o cofre de chaves e a chave ou o script da CLI do Azure ou do Azure PowerShell a seguir para listar a chave criada.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault key list --vault-name $keyVaultName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultKey -vaultName $keyVaultName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Limpar os recursos

Outros tutoriais e inícios rápidos do Key Vault complementam este início rápido. Se você planeja continuar a trabalhar com os tutoriais e inícios rápidos subsequentes, deixe esses recursos onde estão.
Quando não for mais necessário, exclua o grupo de recursos, que excluirá o Key Vault e os recursos relacionados. Para excluir o grupo de recursos usando a CLI do Azure ou o Azure PowerShell:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Próximas etapas

Neste guia de início rápido, você criou um cofre de chaves e uma chave usando um modelo do ARM e validou a implantação. Para saber mais sobre o Key Vault e o Azure Resource Manager, continue com os artigos abaixo.

- Leia uma [Visão geral do Azure Key Vault](../general/overview.md)
- Saiba mais sobre o [Azure Resource Manager](../../azure-resource-manager/management/overview.md)
- Examine as [Melhores práticas do Azure Key Vault](../general/best-practices.md)
