---
title: Arquitetura SQL do Synapse
description: Saiba como o Azure Synapse SQL combina recursos de processamento de consultas distribuídas com o armazenamento do Azure para atingir alto desempenho e escalabilidade.
services: synapse-analytics
author: mlee3gsd
manager: rothja
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: ae3b54ca72c92722dffa370b0b8be1ca2c490f97
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92476001"
---
# <a name="azure-synapse-sql-architecture"></a>Arquitetura do SQL do Azure Synapse 

Este artigo descreve os componentes de arquitetura do SQL do Synapse.

[!INCLUDE [preview](../includes/note-preview.md)]

## <a name="synapse-sql-architecture-components"></a>Componentes de arquitetura do SQL do Synapse

O SQL do Synapse usa uma arquitetura de expansão o processamento computacional dos dados em vários nós de expansão. A computação é separada do armazenamento, o que permite que você dimensione a computação independentemente dos dados em seu sistema. 

Para o pool de SQL, a unidade de escala é uma abstração de poder de computação que é conhecida como [ unidade de data warehouse](resource-consumption-models.md). 

Para o SQL sob demanda, sem servidor, o dimensionamento é feito automaticamente para acomodar os requisitos de recursos de consulta. À medida que a topologia muda ao longo do tempo, adicionando, removendo nós ou failovers, ela se adapta às alterações e garante que sua consulta tenha recursos suficientes e seja concluída com sucesso. Por exemplo, a imagem abaixo mostra o SQL sob demanda utilizando 4 nós de computação para executar uma consulta.

![Arquitetura SQL do Synapse](./media//overview-architecture/sql-architecture.png)

O SQL do Synapse usa uma arquitetura baseada em nó. Aplicativos conectam e emitem comandos T-SQL para um nó de Controle, que é o ponto único de entrada para SQL do Synapse. 

O nó de controle do SQL Synapse do Azure utiliza um mecanismo de consulta distribuído para otimizar consultas para processamento paralelo e, em seguida, passa as operações para nós de computação para fazer seu trabalho em paralelo. 

O nó de controle do SQL sob demanda utiliza o mecanismo DQP (processamento de consultas distribuídas) para otimizar e orquestrar a execução distribuída da consulta do usuário, dividindo-a em consultas menores que serão executadas nos nós de computação. Cada consulta pequena é chamada de tarefa e representa a unidade de execução distribuída. Ela lê arquivo(s) do armazenamento, une resultados de outras tarefas, agrupa ou solicita os dados recuperados de outras tarefas. 

Os nós de Computação armazenam todos os dados de usuário no Armazenamento do Azure e executam as consultas paralelas. O serviço de movimentação de dados (DMS) é um serviço de nível de sistema interno que move dados entre os nós, conforme necessário para executar consultas em paralelo e retornar resultados precisos. 

Com o armazenamento e a computação separados, ao usar o SQL do Synapse, é possível se beneficiar de um dimensionamento independente da potência de computação, independentemente das suas necessidades de armazenamento. Para o SQL sob demanda, o dimensionamento é feito automaticamente, enquanto para o pool de SQL é possível:

* Aumentar ou reduzir a potência de computação em um pool de SQL (data warehouse), sem mover os dados.
* Pause a capacidade de computação ao deixar dados intactos para que você só pague pelo armazenamento.
* Retomar a capacidade de computação durante horas operacionais.

## <a name="azure-storage"></a>Armazenamento do Azure

O SQL do Synapse aproveita o armazenamento do Azure para manter os dados do usuário protegidos. Como os dados são armazenados e gerenciados pelo armazenamento do Azure, é gerada uma cobrança separada para o consumo de armazenamento. 

O SQL sob demanda permite consultar arquivos em seu data lake em modo somente leitura, enquanto o pool do SQL também permite você ingerir dados. Quando os dados são ingeridos no pool de SQL, eles são divididos em **distribuições** para otimizar o desempenho do sistema. Você pode escolher qual padrão de fragmentação usar para distribuir os dados quando você define a tabela. Esses padrões de fragmentação são compatíveis com:

* Hash
* Round Robin
* Replicar

## <a name="control-node"></a>Nó de controle

O nó de controle é o cérebro da arquitetura. É o front-end que interage com todos os aplicativos e conexões. 

No Synapse SQL, o mecanismo de consulta distribuída é executado no nó de controle para otimizar e coordenar consultas paralelas. Quando você envia uma consulta T-SQL ao pool de SQL, o nó de Controle a transforma em consultas que são executadas em cada distribuição paralelamente.

No SQL sob demanda, o mecanismo DQP é executado no nó de controle para otimizar e coordenar a execução distribuída da consulta do usuário, dividindo-a em consultas menores que serão executadas em nós de computação. Também atribui conjuntos de arquivos a serem processados por cada nó.

## <a name="compute-nodes"></a>Nós de computação

Os nós de computação fornecem capacidade de computação. 

No pool de SQL, distribuições são mapeadas para nós de computação para processamento. À medida que você paga por mais recursos de computação, o pool remapeia as distribuições para os nós de computação disponíveis. O número de intervalos de nós de 1 a 60 de computação e é determinado pelo nível de serviço para o pool de SQL. Cada nó de computação tem uma ID de nó que está visível nas exibições do sistema. Você pode ver a ID do nó de Computação olhando para a coluna node_id nas exibições do sistema cujos nomes começam com sys.pdw_nodes. Para obter uma lista dessas exibições do sistema, consulte [exibições do sistema SQL do Synapse](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=azure-sqldw-latest).

No SQL sob demanda, a cada nó de computação é atribuída uma tarefa e um conjunto de arquivos para executar a tarefa. A tarefa é uma unidade de execução de consulta distribuída, que na verdade faz parte do usuário de consulta enviada. O dimensionamento automático está em vigor para garantir que nós de computação suficientes sejam utilizados para executar a consulta do usuário.

## <a name="data-movement-service"></a>Serviço de movimentação de dados

O Serviço de movimentação de dados (DMS) é a tecnologia de transporte de dados no pool de SQL que coordena a movimentação de dados entre os nós de computação. Algumas consultas exigem a movimentação de dados para garantir que as consultas paralelas retornem resultados precisos. Quando a movimentação de dados é necessária, DMS garante que os dados corretos cheguem ao local correto.

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="distributions"></a>Distribuições

Uma distribuição é a unidade básica de armazenamento e processamento para consultas paralelas que são executadas em dados distribuídos no pool de SQL. Quando o pool de SQL executa uma consulta, o trabalho é dividido em 60 consultas menores que são executadas em paralelo. 

Cada uma das 60 consultas menores é executada em uma das distribuições de dados. Cada nó de computação gerencia um ou mais de 60 distribuições. Um pool de SQL com recursos de computação máximos tem uma distribuição por nó de computação. Um pool de SQL com recursos de computação máximos tem todas as distribuições em um nó de computação. 

## <a name="hash-distributed-tables"></a>Tabelas distribuídas em hash
Uma tabela de hash distribuída pode fornecer o melhor desempenho de consulta para junções e agregações em tabelas grandes. 

Para fragmentar dados em uma tabela distribuída por hash, o pool de SQL usa uma função de hash para atribuir de forma determinada cada linha a uma distribuição. Na definição de tabela, uma das colunas é designada como a coluna de distribuição. A função de hash usa valores na coluna de distribuição para atribuir cada linha a uma distribuição.

O diagrama a seguir ilustra como uma (tabela não distribuída) completa é armazenada como uma tabela distribuída em hash. 

![Tabela distribuída](media//overview-architecture/hash-distributed-table.png "Tabela distribuída") 

* Cada linha pertence a uma distribuição. 
* Um algoritmo de hash determinístico atribui cada linha a uma distribuição. 
* O número de linhas de tabela por distribuição varia conforme mostrado pelos diferentes tamanhos de tabelas.

Há considerações de desempenho para a seleção de uma coluna de distribuição, como distinção, distorção de dados e tipos de consultas executadas no sistema.

## <a name="round-robin-distributed-tables"></a>Tabelas distribuídas round robin

Uma tabela de round-robin é a tabela mais simples para criar e oferece um desempenho rápido quando usada como uma tabela de preparo para cargas.

Uma tabela distribuída round-robin distribui dados uniformemente entre a tabela, mas sem qualquer otimização adicional. Uma distribuição é escolhida primeiramente de forma aleatória e, em seguida, buffers de linhas são atribuídos a distribuições em sequência. É rápido carregar dados em uma tabela de round-robin, mas o desempenho da consulta geralmente pode ser melhor com tabelas de hash distribuídas. Junções de tabelas de round-robin exigem nova recombinação de dados, e isso leva tempo adicional.

## <a name="replicated-tables"></a>Tabelas replicadas
Uma tabela replicada fornece o melhor desempenho de consulta para tabelas pequenas.

Uma tabela replicada faz cache de uma cópia completa da tabela em cada nó de computação. Consequentemente, replicar uma tabela elimina a necessidade de transferir dados entre nós de Computação antes de uma junção ou agregação. Tabelas replicadas são melhor usadas com tabelas pequenas. Armazenamento adicional é necessário e há custos adicionais ao gravar dados que tornam grandes tabelas inexequíveis. 

O diagrama a seguir mostra uma tabela replicada armazenada em cache na primeira distribuição em cada nó de computação. 

![Tabela replicada](media/overview-architecture/replicated-table.png "Tabela replicada") 

## <a name="next-steps"></a>Próximas etapas

Agora que você sabe um pouco sobre o SQL do Synapse, saiba como [criar um pool de SQL](../quickstart-create-sql-pool-portal.md) rapidamente e [carregar dados de amostra](../sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) (./sql-data-warehouse-load-sample-databases.md). Ou você inicia [usando o SQL sob demanda](../quickstart-sql-on-demand.md). Se você for novo no Azure, você pode encontrar o [Glossário do Azure](../../azure-glossary-cloud-terminology.md) úteis à medida que encontrar nova terminologia. 
