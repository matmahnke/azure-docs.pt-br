---
title: 'Início Rápido: Criar um servidor – CLI do Azure – Banco de Dados do Azure para MySQL'
description: Este início rápido descreve como usar a CLI do Azure para criar um servidor de Banco de dados do Azure para MySQL no grupo de recursos do Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 07/15/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 161d1c95507d366ce00f75580a100fd2607675d9
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92544011"
---
# <a name="quickstart-create-an-azure-database-for-mysql-server-using-azure-cli"></a>Início Rápido: Criar um servidor de Banco de Dados do Azure para MySQL usando a CLI do Azure

> [!TIP]
> Considere a possibilidade de usar o comando mais simples da CLI do Azure [az mysql up](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) (atualmente em versão prévia). Experimente o [início rápido](./quickstart-create-server-up-azure-cli.md).

Este início rápido mostra como usar os comandos da [CLI do Azure](/cli/azure/get-started-with-azure-cli) no [Azure Cloud Shell](https://shell.azure.com) para criar um servidor de Banco de Dados do Azure para MySQL em cinco minutos. Se você não tiver uma assinatura do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

> [!NOTE]
> Se você optar por instalar e usar a CLI localmente, este artigo exigirá que seja executada a CLI do Azure versão 2.0 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

## <a name="prerequisites"></a>Pré-requisitos
Este artigo exige que você esteja executando a CLI do Azure versão 2.0 ou posterior localmente. Para ver a versão instalada, execute o comando `az --version`. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

Você precisará fazer logon em sua conta usando o comando [az login](/cli/azure/reference-index#az-login). Observe a propriedade **id** , que se refere à **ID da Assinatura** para sua conta do Azure. 

```azurecli-interactive
az login
```

Selecione a assinatura específica em sua conta usando o comando [az account set](/cli/azure/account). Anote o valor de **id** da saída **az login** para usar como valor para o argumento **subscription** no comando. Se tiver várias assinaturas, escolha a que for adequada para cobrança do recurso. Para obter todas as suas assinaturas, use [az account list](/cli/azure/account#az-account-list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-mysql-server"></a>Criar um Banco de Dados do Azure para o servidor MySQL
Crie um [grupo de recursos do Azure](../azure-resource-manager/management/overview.md) usando o comando [az group create](/cli/azure/group) e crie seu servidor MySQL dentro desse grupo. Você deve fornecer um nome exclusivo. O exemplo a seguir cria um grupo de recursos chamado `myresourcegroup` na localização `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

Crie um servidor de Banco de Dados do Azure para MySQL com o comando [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create). Um servidor pode conter vários bancos de dados.

```azurecli
az mysql server create --resource-group myresourcegroup --name mydemoserver --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 
```

Estes são os detalhes dos argumentos acima: 

**Configuração** | **Valor de exemplo** | **Descrição**
---|---|---
name | mydemoserver | Insira um nome exclusivo que identifique seu servidor do Banco de Dados do Azure para MySQL. O nome do servidor pode conter apenas letras minúsculas, números e o caractere de hífen (-). Ele deve conter de 3 a 63 caracteres.
resource-group | myresourcegroup | Forneça o nome do grupo de recursos do Azure.
local | westus | O local do Azure para o servidor.
admin-user | myadmin | O nome de usuário para o logon de administrador. Não pode ser **azure_superuser** , **admin** , **administrator** , **root** , **guest** nem **public** .
admin-password | *senha de segurança* | A senha do usuário administrador. Ele deve conter entre 8 e 128 caracteres. A senha precisa conter caracteres de três das seguintes categorias: Letras maiúsculas, letras minúsculas, números e caracteres não alfanuméricos.
sku-name|GP_Gen5_2|Insira o nome do tipo de preço e a configuração de computação. Segue a convenção {tipo de preço} _{geração de computação}_ {vCores} em formato abreviado. Confira os [tipos de preço](./concepts-pricing-tiers.md) para obter mais informações.

>[!IMPORTANT] 
>- A versão padrão do MySQL no seu servidor é a 5.7. No momento, as versões 5.6 e 8.0 também estão disponíveis.
>- Para exibir todos os argumentos do comando **az mysql server create** , confira este [documento de referência](/cli/azure/mysql/server#az-mysql-server-create).
>- O SSL é habilitado por padrão no seu servidor. Para obter mais informações sobre o SSL, confira [Configurar a conectividade SSL](howto-configure-ssl.md)

## <a name="configure-a-server-level-firewall-rule"></a>Configurar uma regra de firewall no nível de servidor 
Por padrão, o novo servidor criado é protegido com regras de firewall e não é acessível publicamente. Você pode configurar a regra de firewall no servidor usando o comando [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule). Isso permitirá que você se conecte ao servidor localmente.

O exemplo a seguir cria uma regra de firewall chamada `AllowMyIP`, que permite conexões de um endereço IP específico, 192.168.0.1. Substitua o endereço IP do qual você se conectará. Use um intervalo de endereços IP, se necessário. Se você não sabe como procurar seu endereço IP, acesse [https://whatismyipaddress.com/](https://whatismyipaddress.com/) para obtê-lo.

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> As conexões ao Banco de Dados do Azure para MySQL se comunicam pela porta 3306. Se estiver tentando se conectar em uma rede corporativa, talvez o tráfego de saída pela porta 3306 não seja permitido. Se esse for o caso, você não poderá se conectar ao seu servidor enquanto o departamento de TI não abrir a porta 3306.

## <a name="get-the-connection-information"></a>Obter informações de conexão

Para se conectar ao servidor, é preciso fornecer credenciais de acesso e informações do host.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name mydemoserver
```

O resultado está no formato JSON. Anote o **fullyQualifiedDomainName** e o **administratorLogin** .
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-to-azure-database-for-mysql-server-using-mysql-command-line-client"></a>Conectar-se ao servidor do Banco de Dados do Azure para MySQL usando o cliente de linha de comando mysql
Você pode se conectar ao seu servidor usando uma ferramenta de cliente conhecida, a ferramenta de linha de comando **[mysql.exe](https://dev.mysql.com/downloads/)** com o [Azure Cloud Shell](../cloud-shell/overview.md). Como alternativa, use a linha de comando do mysql em seu ambiente local.
```bash
 mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="clean-up-resources"></a>Limpar os recursos
Se não precisar desses recursos para outro início rápido/tutorial, você poderá excluí-los ao fazer o seguinte comando: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Se você quiser simplesmente excluir o servidor recém-criado, poderá executar o comando [az mysql server delete](/cli/azure/mysql/server#az-mysql-server-delete).

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
>[Criar um aplicativo PHP no Windows com o MySQL](../app-service/tutorial-php-mysql-app.md)
>[Criar um aplicativo PHP no Linux com o MySQL](../app-service/tutorial-php-mysql-app.md?pivots=platform-linux%253fpivots%253dplatform-linux)
>[Criar um aplicativo Spring baseado em Java com o MySQL](/azure/developer/java/spring-framework/spring-app-service-e2e?tabs=bash)