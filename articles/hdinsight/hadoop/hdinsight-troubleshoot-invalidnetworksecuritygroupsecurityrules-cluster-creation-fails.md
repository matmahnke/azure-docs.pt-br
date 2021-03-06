---
title: Erro InvalidNetworkSecurityGroupSecurityRules-Azure HDInsight
description: A criação do cluster falha com o ErrorCode InvalidNetworkSecurityGroupSecurityRules
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 1662be3bedb9ec0f2dc21e98ffbd57d8f8ed5777
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540611"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Cenário: InvalidNetworkSecurityGroupSecurityRules-a criação de cluster falha no Azure HDInsight

Este artigo descreve as etapas de solução de problemas e as possíveis resoluções para problemas ao interagir com clusters do Azure HDInsight.

## <a name="issue"></a>Problema

Você recebe o código de erro `InvalidNetworkSecurityGroupSecurityRules` com uma descrição semelhante a "as regras de segurança no grupo de segurança de rede configuradas com a sub-rede não permitem a conectividade de entrada e/ou saída necessária".

## <a name="cause"></a>Causa

Provavelmente um problema com as regras de [grupo de segurança de rede](../../virtual-network/virtual-network-vnet-plan-design-arm.md) de entrada configuradas para o cluster.

## <a name="resolution"></a>Resolução

Vá para a portal do Azure e identifique o NSG associado à sub-rede em que o cluster está sendo implantado. Na seção **regras de segurança de entrada** , verifique se as regras permitem o acesso de entrada à porta 443 para os endereços IP mencionados [aqui](../control-network-traffic.md).

## <a name="next-steps"></a>Próximas etapas

Se você não encontrou seu problema ou não conseguiu resolver seu problema, visite um dos seguintes canais para obter mais suporte:

* Obtenha respostas de especialistas do Azure por meio do [Suporte da Comunidade do Azure](https://azure.microsoft.com/support/community/).

* Conecte-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente conectando-se à comunidade do Azure para os recursos certos: respostas, suporte e especialistas.

* Se precisar de mais ajuda, poderá enviar uma solicitação de suporte do [portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecione **Suporte** na barra de menus ou abra o hub **Ajuda + suporte** . Para obter informações mais detalhadas, confira [Como criar uma solicitação de suporte do Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). O acesso ao Gerenciamento de assinaturas e ao suporte de cobrança está incluído na sua assinatura do Microsoft Azure, e o suporte técnico é fornecido por meio de um dos [Planos de suporte do Azure](https://azure.microsoft.com/support/plans/).