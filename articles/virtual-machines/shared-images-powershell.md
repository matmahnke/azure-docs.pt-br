---
title: Criar uma galeria de imagens compartilhada com Azure PowerShell
description: Saiba como usar Azure PowerShell para criar uma galeria de imagens compartilhadas no Azure
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: b6828571499631ae08b077a4b7e3120f599e5b8b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84673744"
---
# <a name="create-a-shared-image-gallery-with-azure-powershell"></a>Criar uma galeria de imagem compartilhada com o Azure PowerShell 

Uma [Galeria de Imagens Compartilhadas](./windows/shared-image-galleries.md) simplifica o compartilhamento da imagem personalizada em sua organização. Imagens personalizadas são como imagens do marketplace, mas você mesmo as cria. As imagens personalizadas podem ser usadas para inicializar tarefas de implantação, como pré-carregamento de aplicativos, configurações de aplicativos e outras configurações do sistema operacional. 

A galeria de imagens compartilhadas permite compartilhar suas imagens da VM personalizadas com outras pessoas em sua organização, dentro ou entre regiões, em um locatário do AAD. Escolha quais imagens você deseja compartilhar, em quais regiões deseja torná-las disponíveis e com quem deseja compartilhá-las. Você pode criar várias galerias, de modo que pode agrupar logicamente as imagens compartilhadas. 

A galeria é um recurso de nível superior que fornece RBAC (controle de acesso completo baseado em função). As imagens podem ser atualizadas, e você pode optar por replicar cada versão da imagem para um conjunto diferente de regiões do Azure. A galeria funciona apenas com imagens gerenciadas.

O recurso Galeria de Imagens Compartilhadas tem vários tipos de recursos. 

[!INCLUDE [virtual-machines-shared-image-gallery-resources](../../includes/virtual-machines-shared-image-gallery-resources.md)]


[!INCLUDE [virtual-machines-common-shared-images-powershell](../../includes/virtual-machines-common-shared-images-powershell.md)]


## <a name="next-steps"></a>Próximas etapas

Crie uma imagem de uma [VM](image-version-vm-powershell.md), de uma [imagem gerenciada](image-version-managed-image-powershell.md)ou de uma [imagem em outra galeria](image-version-another-gallery-powershell.md).

O [Construtor de imagens do Azure (visualização)](./windows/image-builder-overview.md) pode ajudar a automatizar a criação da versão da imagem, até mesmo usá-la para atualizar e [criar uma nova versão da imagem a partir de uma versão de imagem existente](./windows/image-builder-gallery-update-image-version.md). 

Você também pode criar um recurso de Galeria de imagens compartilhadas usando modelos. Há vários Modelos de Início Rápido do Azure disponíveis: 

- [Criar uma Galeria de Imagens Compartilhadas](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Criar uma Definição de Imagem em uma Galeria de Imagens Compartilhadas](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Criar uma Versão da Imagem em uma Galeria de Imagens Compartilhadas](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Criar uma VM por meio de uma Versão da Imagem](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)


