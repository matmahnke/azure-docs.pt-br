---
title: Como selecionar o tamanho certo da VM para o cluster HDInsight do Azure
description: Saiba como selecionar o tamanho correto da VM para seu cluster HDInsight.
keywords: tamanhos de VM, tamanhos de cluster, configuração de cluster
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/09/2019
ms.openlocfilehash: d347707b0f48314dd872bc3ad34ac624817d2937
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535307"
---
# <a name="selecting-the-right-vm-size-for-your-azure-hdinsight-cluster"></a>Selecionando o tamanho correto da VM para o cluster HDInsight do Azure

Este artigo discute como selecionar o tamanho correto da VM para os vários nós em seu cluster HDInsight. 

Comece compreendendo como as propriedades de uma máquina virtual, como o processamento da CPU, o tamanho da RAM e a latência da rede, afetarão o processamento de suas cargas de trabalho. Em seguida, pense em seu aplicativo e como ele corresponde ao que as diferentes famílias de VM são otimizadas para o. Certifique-se de que a família de VMs que você deseja usar seja compatível com o tipo de cluster que você planeja implantar. Para obter uma lista de todos os tamanhos de VM compatíveis e recomendados para cada tipo de cluster, consulte [configurações de nó com suporte do Azure HDInsight](hdinsight-supported-node-configuration.md). Por fim, você pode usar um processo de benchmark para testar algumas cargas de trabalho de exemplo e verificar qual SKU dentro dessa família é ideal para você.

Para obter mais informações sobre como planejar outros aspectos do cluster, como selecionar um tipo de armazenamento ou tamanho de cluster, consulte [planejamento de capacidade para clusters HDInsight](hdinsight-capacity-planning.md).

## <a name="vm-properties-and-big-data-workloads"></a>Propriedades da VM e cargas de trabalho de Big Data

O tamanho e o tipo da VM são determinados pelo poder de processamento da CPU, pelo tamanho da RAM e pela latência da rede:

- CPU: o tamanho da VM determina o número de núcleos. Quanto mais núcleos, maior o grau de computação paralela que cada nó pode alcançar. Além disso, alguns tipos de VM tem núcleos mais rápidos.

- RAM: o tamanho da VM também determina a quantidade de RAM disponível na VM. Para cargas de trabalho que armazenam dados na memória para processamento em vez de ler no disco, verifique se os nós de trabalho têm memória suficiente que se ajuste aos dados.

- Rede: para a maioria dos tipos de cluster, os dados processados pelo cluster não estão no disco local, mas sim em um serviço de armazenamento externo, como o Data Lake Storage ou o armazenamento do Azure. Considere a largura de banda e taxa de transferência da rede entre a VM do nó e o serviço de armazenamento. Geralmente, a largura de banda da rede disponível para uma VM aumenta com tamanhos maiores. Para mais detalhes, consulte [Visão geral sobre tamanhos de VM](../virtual-machines/sizes.md).

## <a name="understanding-vm-optimization"></a>Noções básicas sobre a otimização da VM

As famílias de máquinas virtuais no Azure são otimizadas para atender a diferentes casos de uso. Na tabela a seguir, você pode encontrar alguns dos casos de uso mais populares e as famílias de VMs que correspondem a eles.

| Type                     | Tamanhos           |    Descrição       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Nível de entrada](../virtual-machines/sizes-general.md)          | A, Av2  | Ter configurações de desempenho e memória da CPU mais adequadas para cargas de trabalho de nível de entrada, como desenvolvimento e teste. Eles são econômicos e fornecem uma opção de baixo custo para começar a usar o Azure. |
| [Propósito geral](../virtual-machines/sizes-general.md)          | D, DSv2, Dv2  | Relação equilibrada de CPU/memória. Ideal para teste e desenvolvimento, bancos de dados pequenos a médios e servidores Web de tráfego baixo a médio. |
| [Computação otimizada](../virtual-machines/sizes-compute.md)        | F           | Alta relação de CPU/memória. Boa para servidores web de tráfego médio, dispositivos de rede, processos de lote e servidores de aplicativo.        |
| [Memória otimizada](../virtual-machines/sizes-memory.md)         | Esv3, Ev3  | Alta relação de memória/CPU. Ótima para servidores de banco de dados relacionais, caches médios a grandes e análises na memória.                 |

- Para obter informações sobre os preços das instâncias de VM disponíveis em regiões com suporte do HDInsight, consulte [preços do hdinsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="cost-saving-vm-types-for-light-workloads"></a>Tipos de VM de economia para cargas de trabalho leves

Se você tiver requisitos leves de processamento, a [série F](https://azure.microsoft.com/blog/f-series-vm-size/) pode ser uma boa opção para começar a usar o HDInsight. A um preço de lista inferior por hora, a série F é o melhor valor de preço/desempenho no portfólio do Azure com base na ACU (Unidade de Computação do Azure) por vCPU.

A tabela a seguir descreve os tipos de cluster e tipos de nó, que podem ser criados com as VMs da série Fsv2.

| Tipo de cluster | Versão | Nó de trabalho | Nó de cabeçalho | Nó Zookeeper |
|---|---|---|---|---|
| Spark | Todos | F4 e acima | no | no |
| O Hadoop | Todos | F4 e acima | no | no |
| Kafka | Todos | F4 e acima | no | no |
| HBase | Todos | F4 e acima | no | no |
| LLAP | desabilitado | no | no | no |
| Storm | desabilitado | no | no | no |
| Serviço do Machine Learning | SOMENTE HDI 3,6 | F4 e acima | no | no |

Para ver as especificações de cada SKU da série F, consulte [tamanhos de VM da série f](https://azure.microsoft.com/blog/f-series-vm-size/).

## <a name="benchmarking"></a>Parâmetros de comparação

O benchmark é o processo de execução de cargas de trabalho simuladas em diferentes VMs para medir o quão bem eles serão executados para suas cargas de trabalho de produção. 

Para obter mais informações sobre o benchmarking para SKUs de VM e tamanhos de cluster, consulte [planejamento de capacidade de cluster no Azure HDInsight ](hdinsight-capacity-planning.md#choose-the-vm-size-and-type).

## <a name="next-steps"></a>Próximas etapas

- [Configurações de nó com suporte do Azure HDInsight](hdinsight-supported-node-configuration.md)
- [Tamanhos das máquinas virtuais do Linux no Azure](../virtual-machines/sizes.md)