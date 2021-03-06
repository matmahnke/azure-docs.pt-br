---
title: Funções de data e hora na linguagem de consulta Azure Cosmos DB
description: Saiba mais sobre as funções do sistema SQL de data e hora no Azure Cosmos DB para executar as operações DateTime e timestamp.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a7f19a7c3cc7337f0e1c53cf387abb35b96d1e38
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93100313"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Funções de data e hora (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

As funções de data e hora permitem que você execute as operações DateTime e timestamp em Azure Cosmos DB.

## <a name="functions-to-obtain-the-date-and-time"></a>Funções para obter a data e a hora

As seguintes funções escalares permitem que você obtenha a data e a hora UTC atuais em três formas: uma cadeia de caracteres que está de acordo com o formato ISO 8601, um carimbo de hora numérico cujo valor é o número de milissegundos decorridos desde a época do UNIX, ou tiques numéricos cujo valor é o número de tiques de 100 nanossegundos decorridos desde a época :

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)
* [GetCurrentTicks](sql-query-getcurrentticks.md)

## <a name="functions-to-work-with-datetime-values"></a>Funções para trabalhar com valores DateTime

As funções a seguir permitem que você manipule facilmente os valores DateTime, timestamp e Tick:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [DateTimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)
* [DateTimePart](sql-query-datetimepart.md)
* [DateTimeToTicks](sql-query-datetimetoticks.md)
* [DateTimeToTimestamp](sql-query-datetimetotimestamp.md)
* [TicksToDateTime](sql-query-tickstodatetime.md)
* [TimestampToDateTime](sql-query-timestamptodatetime.md)

## <a name="next-steps"></a>Próximas etapas

- [Funções de sistema do Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)
- [Funções definidas pelo usuário](sql-query-udfs.md)
- [Agregações](sql-query-aggregates.md)
