---
title: Azure Machine Learning de monitoramento | Microsoft Docs
description: Saiba como usar Azure Monitor para exibir, analisar e criar alertas sobre as métricas de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 10/01/2020
ms.openlocfilehash: 3470f969034a051b17e762b685a89c0f910e0cbb
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92747112"
---
# <a name="monitor-azure-machine-learning"></a>Monitorar o Azure Machine Learning

Quando você tem aplicativos e processos de negócios críticos que dependem de recursos do Azure, recomendamos monitorar a disponibilidade, o desempenho e a operação desses recursos. Este artigo descreve os dados de monitoramento gerados pelo Azure Machine Learning e como analisar e alertar sobre esses dados com o Azure Monitor.

> [!TIP]
> As informações contidas neste documento são basicamente para administradores, pois descrevem o monitoramento do Azure Machine Learning no nível do *espaço de trabalho* . Se você for um cientista de dados ou desenvolvedor e quiser monitorar informações específicas para suas *execuções de treinamento de modelo* , consulte os seguintes documentos:
>
> * [Iniciar, monitorar e cancelar execuções de treinamento](how-to-manage-runs.md)
> * [Métricas de logs para execuções de treinamento](how-to-track-experiments.md)
> * [Acompanhar experimentos com o MLflow](how-to-use-mlflow.md)
> * [Visualizar execuções com o TensorBoard](how-to-monitor-tensorboard.md)

## <a name="what-is-azure-monitor"></a>O que é o Azure Monitor?

Azure Machine Learning cria dados de monitoramento usando [Azure monitor](/azure/azure-monitor/overview), que é um serviço de monitoramento de pilha completo no Azure. Azure Monitor fornece um conjunto completo de recursos para monitorar os recursos do Azure. Ele também pode monitorar recursos em outras nuvens e locais.

Comece com o artigo [monitorando os recursos do Azure com Azure monitor](/azure/azure-monitor/insights/monitor-azure-resource), que descreve os seguintes conceitos:

- O que é o Azure Monitor?
- Custos associados ao monitoramento
- Monitoramento de dados coletados no Azure
- Configuração de coleta de dados
- Ferramentas padrão no Azure para analisar e alertar sobre dados de monitoramento

As seções a seguir se baseiam neste artigo descrevendo os dados específicos coletados para Azure Machine Learning. Essas seções também fornecem exemplos para configurar a coleta de dados e analisar esses dados com as ferramentas do Azure.

> [!TIP]
> Para entender os custos associados a Azure Monitor, confira [uso e custos estimados](/azure/azure-monitor/platform/usage-estimated-costs). Para entender o tempo que leva para que seus dados apareçam em Azure Monitor, consulte [tempo de ingestão de dados de log](/azure/azure-monitor/platform/data-ingestion-time).

## <a name="monitoring-data-from-azure-machine-learning"></a>Monitorando dados de Azure Machine Learning

Azure Machine Learning coleta os mesmos tipos de dados de monitoramento que outros recursos do Azure descritos em [monitoramento de dados de recursos do Azure](/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data-from-Azure-resources). 

Veja [Azure Machine Learning referência de dados de monitoramento](monitor-resource-reference.md) para obter uma referência detalhada dos logs e métricas criados por Azure Machine Learning.

<a id="configuration"></a>

## <a name="collection-and-routing"></a>Coleta e roteamento

As métricas de plataforma e o log de atividades são coletados e armazenados automaticamente, mas podem ser roteados para outros locais usando uma configuração de diagnóstico.  

Os logs de recursos não são coletados e armazenados até você criar uma configuração de diagnóstico e roteá-los para um ou mais locais.

Consulte [criar configuração de diagnóstico para coletar logs e métricas de plataforma no Azure](/azure/azure-monitor/platform/diagnostic-settings) para o processo detalhado para criar uma configuração de diagnóstico usando o portal do Azure, a CLI ou o PowerShell. Ao criar uma configuração de diagnóstico, você especifica quais categorias de logs coletar. As categorias de Azure Machine Learning são listadas em [referência de dados de monitoramento de Azure Machine Learning](monitor-resource-reference.md#resource-logs).

> [!IMPORTANT]
> A habilitação dessas configurações requer serviços adicionais do Azure (conta de armazenamento, Hub de eventos ou Log Analytics), o que pode aumentar seu custo. Para calcular um custo estimado, visite a [calculadora de preços do Azure](https://azure.microsoft.com/pricing/calculator).

Você pode configurar os seguintes logs para Azure Machine Learning:

| Categoria | Descrição |
|:---|:---|
| AmlComputeClusterEvent | Eventos de clusters de computação Azure Machine Learning. |
| AmlComputeClusterNodeEvent | Eventos de nós em um cluster de computação Azure Machine Learning. |
| AmlComputeJobEvent | Eventos de trabalhos em execução no Azure Machine Learning computação. |

> [!NOTE]
> Quando você habilita as métricas em uma configuração de diagnóstico, as informações de dimensão não são incluídas atualmente como parte das informações enviadas para uma conta de armazenamento, Hub de eventos ou log Analytics.

As métricas e os logs que você pode coletar são discutidos nas seções a seguir.

## <a name="analyzing-metrics"></a>Analisando métricas

Você pode analisar as métricas para Azure Machine Learning, juntamente com as métricas de outros serviços do Azure, abrindo as **métricas** no menu **Azure monitor** . Consulte [Introdução ao Azure Metrics Explorer](/azure/azure-monitor/platform/metrics-getting-started) para obter detalhes sobre como usar essa ferramenta.

Para obter uma lista das métricas de plataforma coletadas, consulte [monitoramento Azure Machine Learning métricas de referência de dados](monitor-resource-reference.md#metrics).

Todas as métricas para Azure Machine Learning estão no namespace **Machine Learning espaço de trabalho do serviço** .

![Metrics Explorer com espaço de trabalho de serviço do Machine Learning selecionado](./media/monitor-azure-machine-learning/metrics.png)

Para referência, você pode ver uma lista de [todas as métricas de recurso com suporte no Azure monitor](/azure/azure-monitor/platform/metrics-supported).

### <a name="filtering-and-splitting"></a>Filtragem e divisão

Para métricas que dão suporte a dimensões, você pode aplicar filtros usando um valor de dimensão. Por exemplo, a filtragem de **núcleos ativos** para um **nome de cluster** do `cpu-cluster` . 

Você também pode dividir uma métrica por dimensão para visualizar como os segmentos diferentes da métrica se comparam entre si. Por exemplo, dividindo o **tipo de etapa de pipeline** para ver uma contagem dos tipos de etapas usados no pipeline.

Para obter mais informações sobre filtragem e divisão, consulte [recursos avançados do Azure monitor](/azure/azure-monitor/platform/metrics-charts).

<a id="analyzing-log-data"></a>
## <a name="analyzing-logs"></a>Análise de logs

Usando Azure Monitor Log Analytics exige que você crie uma configuração de diagnóstico e habilite __as informações de envio para log Analytics__ . Para obter mais informações, consulte a seção [coleção e roteamento](#collection-and-routing) .

Os dados em logs de Azure Monitor são armazenados em tabelas, sendo que cada tabela tem seu próprio conjunto de propriedades exclusivas. Azure Machine Learning armazena dados nas tabelas a seguir:

| Tabela | Descrição |
|:---|:---|
| AmlComputeClusterEvent | Eventos de clusters de computação Azure Machine Learning. |
| AmlComputeClusterNodeEvent | Eventos de nós em um cluster de computação Azure Machine Learning. |
| AmlComputeJobEvent | Eventos de trabalhos em execução no Azure Machine Learning computação. |

> [!IMPORTANT]
> Quando você seleciona **logs** no menu Azure Machine Learning, log Analytics é aberto com o escopo de consulta definido como o espaço de trabalho atual. Isso significa que as consultas de log incluirão apenas os dados desse recurso. Se você quiser executar uma consulta que inclua dados de outros bancos de dados ou de outros serviços do Azure, selecione **Logs** no menu **Azure Monitor** . Confira [Escopo da consulta de log e intervalo de tempo no Log Analytics do Azure Monitor](/azure/azure-monitor/log-query/scope/) para obter detalhes.

Para obter uma referência detalhada dos logs e métricas, consulte [Azure Machine Learning referência de dados de monitoramento](monitor-resource-reference.md).

### <a name="sample-kusto-queries"></a>Consultas de exemplo do Kusto

> [!IMPORTANT]
> Quando você seleciona **logs** no menu [Service-Name], log Analytics é aberto com o escopo de consulta definido para o espaço de trabalho Azure Machine Learning atual. Isso significa que as consultas de log incluirão apenas os dados desse recurso. Se você quiser executar uma consulta que inclui dados de outros espaços de trabalho ou dados de outros serviços do Azure, selecione **logs** no menu **Azure monitor** . Confira [Escopo da consulta de log e intervalo de tempo no Log Analytics do Azure Monitor](/azure/azure-monitor/log-query/scope/) para obter detalhes.

Veja a seguir as consultas que você pode usar para ajudá-lo a monitorar seus Azure Machine Learning recursos: 

+ Obter trabalhos com falha nos últimos cinco dias:

    ```Kusto
    AmlComputeJobEvent
    | where TimeGenerated > ago(5d) and EventType == "JobFailed"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Obter registros para um nome de trabalho específico:

    ```Kusto
    AmlComputeJobEvent
    | where JobName == "automl_a9940991-dedb-4262-9763-2fd08b79d8fb_setup"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Obter eventos de cluster nos últimos cinco dias para clusters em que o tamanho da VM é Standard_D1_V2:

    ```Kusto
    AmlComputeClusterEvent
    | where TimeGenerated > ago(4d) and VmSize == "STANDARD_D1_V2"
    | project  ClusterName , InitialNodeCount , MaximumNodeCount , QuotaAllocated , QuotaUtilized
    ```

+ Obter nós alocados nos últimos oito dias:

    ```Kusto
    AmlComputeClusterNodeEvent
    | where TimeGenerated > ago(8d) and NodeAllocationTime  > ago(8d)
    | distinct NodeId
    ```

## <a name="alerts"></a>Alertas

Você pode acessar alertas para Azure Machine Learning abrindo **alertas** no menu **Azure monitor** . Consulte [criar, exibir e gerenciar alertas de métricas usando Azure monitor](/azure/azure-monitor/platform/alerts-metric) para obter detalhes sobre como criar alertas.

A tabela a seguir lista as regras de alerta de métrica comuns e recomendadas para Azure Machine Learning:

| Tipo de alerta | Condição | Descrição |
|:---|:---|:---|
| Implantação de Modelo com Falha | Tipo de agregação: total, operador: maior que, valor do limite: 0 | Quando uma ou mais implantações de modelo falharam |
| Percentual de Utilização de Cota | Tipo de agregação: Average, Operator: maior que, valor de limite: 90| Quando a porcentagem de utilização de cota é maior que 90% |
| Nós Inutilizáveis | Tipo de agregação: total, operador: maior que, valor do limite: 0 | Quando há um ou mais nós inutilizáveis |

## <a name="next-steps"></a>Próximas etapas

- Para obter uma referência dos logs e métricas, consulte [monitoramento Azure Machine Learning referência de dados](monitor-resource-reference.md).
- Para obter informações sobre como trabalhar com cotas relacionadas a Azure Machine Learning, consulte [gerenciar e solicitar cotas para recursos do Azure](how-to-manage-quotas.md).
- Para obter detalhes sobre como monitorar os recursos do Azure, consulte [monitorando recursos do Azure com o Azure monitor](/azure/azure-monitor/insights/monitor-azure-resource).
