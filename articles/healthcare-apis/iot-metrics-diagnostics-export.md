---
title: Exportar as métricas do Conector IoT do Azure para FHIR (versão prévia) por meio das configurações de diagnóstico
description: Este artigo explica como exportar o conector IoT do Azure para métricas FHIR (versão prévia) por meio das configurações de diagnóstico
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/16/2020
ms.author: jasteppe
ms.openlocfilehash: d7779c74a562e1237db863d7759b2adcffa2bddf
ms.sourcegitcommit: d3c3f2ded72bfcf2f552e635dc4eb4010491eb75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92558543"
---
# <a name="export-azure-iot-connector-for-fhir-preview-metrics-through-diagnostic-settings"></a>Exportar as métricas do Conector IoT do Azure para FHIR (versão prévia) por meio das configurações de diagnóstico

Neste artigo, você aprenderá a exportar o conector do IoT do Azure para logs de métricas do FHIR *. O recurso que habilita o log de métricas são as [**configurações de diagnóstico**](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) no portal do Azure. 

> [!TIP]
> Siga as orientações em [habilitar o log de diagnóstico na API do Azure para FHIR e o conector do Azure IOT para FHIR](enable-diagnostic-logging.md#enable-diagnostic-logging-in-azure-api-for-fhir) para configurar o log de auditoria.

## <a name="enable-metric-logging-for-the-azure-iot-connector-for-fhir-preview"></a>Habilitar o log de métrica para o conector do Azure IoT para FHIR (versão prévia)
1. Para habilitar o log de métrica para o conector do Azure IoT para FHIR, selecione sua API do Azure para o serviço FHIR no portal do Azure 

2. Navegue até **configurações de diagnóstico** 

3. Selecione **+ Adicionar configuração de diagnóstico**

   :::image type="content" source="media/iot-metrics-export/diagnostic-settings-main.png" alt-text="Connector1 IoT" lightbox="media/iot-metrics-export/diagnostic-settings-main.png"::: 

4. Insira um nome na caixa de diálogo **nome da configuração de diagnóstico** .

5. Selecione o método que você deseja usar para acessar seus logs de diagnóstico:

    1. **Arquivar em uma conta de armazenamento** para auditoria ou inspeção manual. A conta de armazenamento que você deseja usar precisa já estar criada.
    2. **Transmita para o Hub de eventos** para ingestão por um serviço de terceiros ou uma solução analítica personalizada. Você precisará criar um namespace do hub de eventos e uma política do hub de eventos antes de configurar esta etapa.
    3. **Transmitir para o** espaço de trabalho Log Analytics no Azure monitor. Você precisará criar seu espaço de trabalho do log Analytics antes de poder selecionar essa opção.

6. Selecione **erros, tráfego e latência** para o conector IOT do Azure para FHIR e quaisquer categorias de métrica adicionais que você deseja capturar para a API do Azure para FHIR.

7. Selecione **Salvar**

   :::image type="content" source="media/iot-metrics-export/diagnostic-setting-add.png" alt-text="Connector1 IoT" lightbox="media/iot-metrics-export/diagnostic-setting-add.png":::

> [!Note] 
> Pode levar até 15 minutos para que os primeiros logs de métricas sejam exibidos no repositório de sua escolha.  
 
Para obter mais informações sobre como trabalhar com logs de diagnóstico, consulte a [documentação do log de recursos do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview)

## <a name="conclusion"></a>Conclusão 
Ter acesso aos logs de métrica é essencial para o monitoramento e a solução de problemas.  O conector do Azure IoT para FHIR permite que você execute essas ações por meio de logs de métrica. 

## <a name="next-steps"></a>Próximas etapas

Confira as perguntas frequentes sobre o conector do Azure IoT para FHIR.

>[!div class="nextstepaction"]
>[Azure IoT Connector para FHIR FAQs](fhir-faq.md)

*No portal do Azure, o Conector IoT do Azure para FHIR é chamado de Conector IoT (versão prévia).

FHIR é uma marca registrada da HL7, usada com permissão da HL7.
