---
title: Consultas parametrizadas no Azure Cosmos DB
description: Saiba como as consultas parametrizadas do SQL fornecem manipulação e saída robustas de entrada do usuário e evitam a exposição acidental de dados por meio de injeção de SQL.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/29/2020
ms.author: tisande
ms.openlocfilehash: a0f354a0420521ede45740a4eb9c492089e36bda
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93089212"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Consultas parametrizadas no Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB dá suporte a consultas com parâmetros expressos pela conhecida @ Notation. O SQL com parâmetros fornece manipulação e saída robustas de entrada do usuário e impede a exposição acidental de dados por meio de injeção de SQL.

## <a name="examples"></a>Exemplos

Por exemplo, você pode escrever uma consulta que aceite `lastName` e `address.state` como parâmetros e execute-a para vários valores de `lastName` e `address.state` com base na entrada do usuário.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Em seguida, você pode enviar essa solicitação para Azure Cosmos DB como uma consulta JSON parametrizada como a seguinte:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

O exemplo a seguir define o argumento superior com uma consulta parametrizada:

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Os valores de parâmetro podem ser qualquer JSON: cadeias de caracteres, números, Boolianos, nulos, pares ou JSON aninhados válidos. Como Azure Cosmos DB não tem Esquema, os parâmetros não são validados em relação a nenhum tipo.

Aqui estão exemplos de consultas parametrizadas em cada SDK Azure Cosmos DB:

- [SDK .NET](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L195)
- [Java](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/src/main/java/com/azure/cosmos/examples/queries/sync/QueriesQuickstart.java#L392-L421)
- [Node.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L58-L79)
- [Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L66-L78)

## <a name="next-steps"></a>Próximas etapas

- [Amostras do .NET no Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Dados de documento modelo](modeling-data.md)
