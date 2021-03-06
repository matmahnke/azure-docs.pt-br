---
title: Atualizar destinos de armazenamento do cache HPC do Azure
description: Como editar destinos de armazenamento do cache HPC do Azure
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/30/2020
ms.author: v-erkel
ms.openlocfilehash: 70f350204796099e02f7afe829a6e2e1fdf653c8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613077"
---
# <a name="edit-storage-targets"></a>Editar destinos de armazenamento

Você pode remover ou modificar destinos de armazenamento com o portal do Azure ou usando o CLI do Azure.

Dependendo do tipo de armazenamento, você pode modificar esses valores de destino de armazenamento:

* Para destinos de armazenamento de BLOBs, você pode alterar o caminho do namespace.

* Para destinos de armazenamento NFS, você pode alterar esses valores:

  * Caminhos de namespace
  * O subdiretório de exportação de armazenamento ou exportação associado a um caminho de namespace
  * Modelo de uso

Você não pode editar o nome, tipo ou sistema de armazenamento de back-end de um destino de armazenamento (contêiner de BLOB ou endereço IP/nome de host do NFS). Se você precisar alterar essas propriedades, exclua o destino de armazenamento e crie uma substituição com o novo valor.

> [!TIP]
> O [vídeo Gerenciando o cache HPC do Azure](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) mostra como editar um destino de armazenamento no portal do Azure.

## <a name="remove-a-storage-target"></a>Remover um destino de armazenamento

### <a name="portal"></a>[Portal](#tab/azure-portal)

Para remover um destino de armazenamento, abra a página **destinos de armazenamento** . Selecione o destino de armazenamento na lista e clique no botão **excluir** .

### <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Use [AZ HPC-cache Storage-Target remove](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-remove) para excluir um destino de armazenamento do cache.

```azurecli
$ az hpc-cache storage-target remove --resource-group cache-rg --cache-name doc-cache0629 --name blob1

{- Finished ..
  "endTime": "2020-07-09T21:45:06.1631571+00:00",
  "name": "2f95eac1-aded-4860-b19c-3f089531a7ec",
  "startTime": "2020-07-09T21:43:38.5461495+00:00",
  "status": "Succeeded"
}
```

---

A exclusão de um destino de armazenamento remove a associação do sistema de armazenamento com esse sistema de cache HPC do Azure, mas não altera o sistema de armazenamento de back-end. Por exemplo, se você usou um contêiner de armazenamento de BLOBs do Azure, o contêiner e seu conteúdo ainda existem depois de você excluí-lo do cache. Você pode adicionar o contêiner a um cache do HPC do Azure diferente, adicioná-lo novamente a esse cache ou excluí-lo com o portal do Azure.

Todas as alterações de arquivo armazenadas no cache são gravadas no sistema de armazenamento de back-end antes de o destino de armazenamento ser removido. Esse processo pode levar uma hora ou mais se muitos dados alterados estiverem no cache.

## <a name="change-a-blob-storage-targets-namespace-path"></a>Alterar o caminho do namespace de um destino de armazenamento de BLOBs

Os caminhos de namespace são os caminhos que os clientes usam para montar esse destino de armazenamento. (Para saber mais, leia [planejar o namespace agregado](hpc-cache-namespace.md) e [Configurar o namespace agregado](add-namespace-paths.md)).

O caminho do namespace é a única atualização que você pode fazer em um destino de armazenamento de BLOBs do Azure. Use o portal do Azure ou o CLI do Azure para alterá-lo.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Use a página de **namespace** para o cache do Azure HPC. A página namespace é descrita mais detalhadamente no artigo [Configurar o namespace agregado](add-namespace-paths.md).

Clique no nome do caminho que você deseja alterar e crie o novo caminho na janela de edição que aparece.

![Captura de tela da página de namespace depois de clicar em um caminho de namespace de blob-os campos de edição aparecem em um painel à direita](media/edit-namespace-blob.png)

Depois de fazer alterações, clique em **OK** para atualizar o destino de armazenamento ou clique em **Cancelar** para descartar as alterações.

### <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Para alterar o namespace de um destino de armazenamento de BLOBs com o CLI do Azure, use o comando [AZ HPC-cache BLOB-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-update). Somente o `--virtual-namespace-path` valor pode ser alterado.

  ```azurecli
  az hpc-cache blob-storage-target update --cache-name cache-name --name target-name \
    --resource-group rg --virtual-namespace-path "/new-path"
  ```

---

## <a name="update-an-nfs-storage-target"></a>Atualizar um destino de armazenamento NFS

Para destinos de armazenamento NFS, você pode alterar ou adicionar caminhos de namespace virtual, alterar os valores de exportação ou subdiretório NFS aos quais um caminho de namespace aponta e alterar o modelo de uso.

Os detalhes estão abaixo:

* [Alterar valores de namespace agregados](#change-aggregated-namespace-values) (caminho do namespace virtual, exportar e exportar subdiretório)
* [Alterar o modelo de uso](#change-the-usage-model)

### <a name="change-aggregated-namespace-values"></a>Alterar valores de namespace agregados

Você pode usar a portal do Azure ou a CLI do Azure para alterar o caminho do namespace voltado para o cliente, a exportação de armazenamento e o subdiretório de exportação (se usado).

Leia as diretrizes em [adicionar caminhos de namespace NFS](add-namespace-paths.md#nfs-namespace-paths) se você precisar de um lembrete sobre como criar vários caminhos válidos em um destino de armazenamento.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Use a página de **namespace** para o cache do Azure HPC para atualizar valores de namespace. Esta página é descrita mais detalhadamente no artigo [Configurar o namespace agregado](add-namespace-paths.md).

![captura de tela da página namespace do portal com a página de atualização do NFS aberta à direita](media/update-namespace-nfs.png)

1. Clique no nome do caminho que você deseja alterar.
1. Use a janela Editar para digitar o novo caminho virtual, exportar ou valores de subdiretório.
1. Depois de fazer alterações, clique em **OK** para atualizar o destino de armazenamento ou em **Cancelar** para descartar as alterações.

### <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Use a ``--junction`` opção no comando [AZ HPC-cache NFS-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target) para alterar o caminho do namespace, a exportação de NFS ou o subdiretório de exportação.

O ``--junction`` parâmetro usa estes valores:

* ``namespace-path`` -O caminho do arquivo virtual voltado para o cliente
* ``nfs-export`` -A exportação do sistema de armazenamento para associar ao caminho voltado para o cliente
* ``target-path`` (opcional)-um subdiretório da exportação, se necessário

Exemplo: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

Você deve fornecer todos os três valores para cada caminho na ``--junction`` instrução. Use os valores existentes para quaisquer valores que você não deseja alterar.

O nome do cache, o nome do destino de armazenamento e o grupo de recursos também são necessários em todos os comandos de atualização.

Exemplo de comando:

```azurecli
az hpc-cache nfs-storage-target update --cache-name mycache \
  --name st-name --resource-group doc-rg0619 \
  --junction namespace-path="/new-path" nfs-export="/my-export" target-path="my-subdirectory"
```

---

### <a name="change-the-usage-model"></a>Alterar o modelo de uso

O modelo de uso influencia como o cache retém os dados. Leia [escolher um modelo de uso](hpc-cache-add-storage.md#choose-a-usage-model) para saber mais.

Para alterar o modelo de uso de um destino de armazenamento NFS, use um desses métodos.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Altere o modelo de uso da página **destinos de armazenamento** no portal do Azure. Clique no nome do destino de armazenamento a ser alterado.

![captura de tela da página de edição de um destino de armazenamento NFS](media/edit-storage-nfs.png)

Use o seletor suspenso para escolher um novo modelo de uso. Clique em **OK** para atualizar o destino de armazenamento ou clique em **Cancelar** para descartar as alterações.

### <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Use o comando [AZ HPC-cache NFS-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target?view=azure-cli-latest#ext-hpc-cache-az-hpc-cache-nfs-storage-target-update) .

O comando Update é quase idêntico ao comando que você usa para adicionar um destino de armazenamento NFS. Consulte [criar um destino de armazenamento NFS](hpc-cache-add-storage.md#create-an-nfs-storage-target) para obter detalhes e exemplos.

Para alterar o modelo de uso, atualize a ``--nfs3-usage-model`` opção. Exemplo: ``--nfs3-usage-model WRITE_WORKLOAD_15``

O nome do cache, o nome do destino de armazenamento e os valores do grupo de recursos também são necessários.

Se você quiser verificar os nomes dos modelos de uso, use o comando [AZ HPC-cache Usage-Model List](/cli/azure/ext/hpc-cache/hpc-cache/usage-model#ext-hpc-cache-az-hpc-cache-usage-model-list).

Se o cache for interrompido ou não estiver em um estado íntegro, a atualização será aplicada depois que o cache estiver íntegro.

---

## <a name="next-steps"></a>Próximas etapas

* Leia [Adicionar destinos de armazenamento](hpc-cache-add-storage.md) para saber mais sobre essas opções.
* Leia [planejar o namespace agregado](hpc-cache-namespace.md) para obter mais dicas sobre como usar caminhos virtuais.
