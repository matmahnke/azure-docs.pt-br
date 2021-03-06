---
title: incluir arquivo
description: incluir arquivo
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 10/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 47eae616ffc62e42448da23fb02152dae17aa548
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92116803"
---
| Recurso | Destino | Limite rígido |
|----------|--------------|------------|
| Serviços de Sincronização de Armazenamento por região | 100 Serviços de Sincronização de Armazenamento | Sim |
| Grupos de sincronização por Serviço de Sincronização de Armazenamento | 200 grupos de sincronização | Sim |
| Servidores registrados por Serviço de Sincronização de Armazenamento | 99 servidores | Sim |
| Pontos de extremidade na nuvem por grupo de sincronização | 1 ponto de extremidade de nuvem | Sim |
| Pontos de extremidade no servidor por grupo de sincronização | 50 pontos de extremidade de servidor | Não |
| Pontos de extremidade de servidor por servidor | 30 pontos de extremidade de servidor | Sim |
| Objetos do sistema de arquivos (diretórios e arquivos) por grupo de sincronização | 100 milhões de objetos | Não |
| Número máximo de objetos do sistema de arquivos (diretórios e arquivos) em um diretório | 5 milhões de objetos | Sim |
| Tamanho máximo do descritor de segurança (diretórios e arquivos) do objeto | 64 KiB | Sim |
| Tamanho do arquivo | 100 GiB | Não |
| Tamanho mínimo do arquivo para que um arquivo seja colocado em camadas | V9 e mais recente: com base no tamanho do cluster do sistema de arquivos (tamanho duplo do cluster do sistema de arquivos). Por exemplo, caso o tamanho do cluster do sistema de arquivos seja 4 KB, o tamanho mínimo do arquivo será 8 KB.<br> V8 e uma versão mais antiga: 64 KiB  | Sim |

> [!Note]  
> Um ponto de extremidade de Sincronização de Arquivos do Azure poderá aumentar o tamanho de um compartilhamento de arquivo do Azure. Caso o limite de tamanho do compartilhamento de arquivo do Azure seja atingido, a sincronização não será capaz de funcionar.
