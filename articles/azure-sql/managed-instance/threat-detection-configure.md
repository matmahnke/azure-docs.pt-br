---
title: Configurar a Proteção Avançada contra Ameaças
titleSuffix: Azure SQL Managed Instance
description: A proteção avançada contra ameaças detecta atividades anormais de banco de dados que indicam possíveis ameaças de segurança ao banco de dados no Azure SQL Instância Gerenciada.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 09/21/2020
ms.openlocfilehash: 31a47b9032ac014fa4eedde343d03d4fd343ff02
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92790654"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-managed-instance"></a>Configurar a proteção avançada contra ameaças no Azure SQL Instância Gerenciada
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

A [proteção avançada contra ameaças](../database/threat-detection-overview.md) para um [Azure SQL instância gerenciada](sql-managed-instance-paas-overview.md) detecta atividades anormais que indicam tentativas incomuns e potencialmente prejudiciais de acessar ou explorar bancos de dados. A proteção avançada contra ameaças pode identificar **possíveis injeção de SQL** , **acesso de localização incomum ou Data Center** , **acesso a partir de um aplicativo não familiar ou potencialmente prejudicial** e **credenciais SQL de força bruta** -consulte mais detalhes em [alertas de proteção avançada contra ameaças](../database/threat-detection-overview.md#alerts).

Você pode receber notificações sobre as ameaças detectadas por meio de [notificações por email](../database/threat-detection-overview.md#explore-detection-of-a-suspicious-event) ou [portal do Azure](../database/threat-detection-overview.md#explore-alerts-in-the-azure-portal)

A [proteção avançada contra ameaças](../database/threat-detection-overview.md) faz parte da oferta do [Azure defender para SQL](../database/azure-defender-for-sql.md) , que é um pacote unificado para recursos avançados de segurança do SQL. A proteção avançada contra ameaças pode ser acessada e gerenciada por meio do Azure defender central para o portal do SQL.

##  <a name="azure-portal"></a>Portal do Azure

1. Entre no  [portal do Azure](https://portal.azure.com). 
2. Navegue até a página de configuração da instância do SQL Instância Gerenciada que você deseja proteger. Em **segurança** , selecione **central de segurança** .
3. Na página de configuração do Azure defender para SQL
   - Ative **o** Azure defender para SQL.
   - Configure a **lista de emails** para receber alertas de segurança após a detecção de atividades anormais do banco de dados.
   - Selecione a **Conta de Armazenamento do Azure** onde os registros de auditoria de ameaças anormais são salvos.
   - Selecione os **tipos de proteção avançada contra ameaças** que você gostaria de configurar. Saiba mais sobre [alertas de proteção avançada contra ameaças](../database/threat-detection-overview.md).
4. Clique em **salvar** para salvar a política nova ou atualizada do Azure defender para SQL.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre a [proteção avançada contra ameaças](../database/threat-detection-overview.md).
- Saiba mais sobre instâncias gerenciadas, consulte [o que é um Azure SQL instância gerenciada](sql-managed-instance-paas-overview.md).
- Saiba mais sobre [a proteção avançada contra ameaças para o banco de dados SQL do Azure](../database/threat-detection-configure.md).
- Saiba mais sobre a [auditoria do SQL instância gerenciada](./auditing-configure.md).
- Saiba mais sobre a [central de segurança do Azure](../../security-center/security-center-introduction.md).