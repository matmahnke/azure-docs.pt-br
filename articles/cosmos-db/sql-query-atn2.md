---
title: ATN2 na linguagem de consulta Azure Cosmos DB
description: Saiba mais sobre como a função do sistema SQL ATN2 no Azure Cosmos DB retorna o valor principal da tangente de arco de y/x, expressa em radianos
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f8fa4738cdd165d068f05cead09e25b382a83730
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93081698"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Retorna o valor principal do arco tangente de y/x, expresso em radianos.  
  
## <a name="syntax"></a>Sintaxe
  
```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   É uma expressão numérica.  
  
## <a name="return-types"></a>Tipos de retorno
  
  Retorna uma expressão numérica.  
  
## <a name="examples"></a>Exemplos
  
  O exemplo a seguir calcula o ATN2 dos componentes x e y especificados.  
  
```sql
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Este é o conjunto de resultados.  
  
```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="remarks"></a>Comentários

Essa função do sistema não usará o índice.

## <a name="next-steps"></a>Próximas etapas

- [Funções matemáticas Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Funções de sistema do Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)
