---
title: 'Tutorial: Introdução à análise de dados com pools de SQL dedicados'
description: Neste tutorial, você usará os dados de exemplo de táxi de NYC para explorar as funcionalidades analíticas do pool de SQL.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: c46adf9e9f5c1b2e74c1098ebf137c4556bfc58d
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92147560"
---
# <a name="analyze-data-with-dedicated-sql-pools"></a>Analisar dados com pools de SQL dedicados

O Azure Synapse Analytics fornece a capacidade de analisar dados com o pool de SQL dedicado. Neste tutorial, você usará os dados de táxis de Nova York para explorar as funcionalidades do pool de SQL dedicado.

## <a name="load-the-nyc-taxi-data-into-sqldb1"></a>Carregar os Dados de Táxi de Nova York no SQLDB1

1. No Synapse Studio, navegue até o hub **Desenvolver** e crie um script de SQL
1. Selecione o pool 'SQLDB1' (pool criado na [ETAPA 1](https://docs.microsoft.com/azure/synapse-analytics/get-started-create-workspace#create-a-sql-pool) deste tutorial) na seção 'Conectar-se a' do script.
1. Insira o seguinte código:
    ```
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );

    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/QID6392_20171107_05910_0.txt.gz'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```
1. Esse script levará cerca de um minuto para ser executado. Ele carrega 2 milhões linhas de dados de Táxi de Nova York em uma tabela chamada **dbo.Trip**

## <a name="explore-the-nyc-taxi-data-in-the-dedicated-sql-pool"></a>Explorar os dados de táxis de Nova York no pool de SQL dedicado

1. No Synapse Studio, acesse o hub **Dados** .
1. Acesse **SQLDB1** > **Tabelas** . Você verá várias tabelas carregadas.
1. Clique com o botão direito do mouse na tabela **dbo.Trip** e selecione **Novo Script de SQL** > **Selecionar as Primeiras 100 Linhas** .
1. Aguarde enquanto um novo script SQL é criado e executado.
1. Observe na parte superior do script de SQL que **Conectar ao** é automaticamente definido como o pool de SQL chamado **SQLDB1** .
1. Substitua o texto do script de SQL por esse código e execute-o.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    Essa consulta mostra como as distâncias totais de viagem e a distância média da viagem estão relacionadas ao número de passageiros.
1. Na janela de resultados do script de SQL, altere a opção **Exibição** para **Gráfico** para uma visualização dos resultados como um gráfico de linhas.



## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Analisar usando o Spark](get-started-analyze-spark.md)

