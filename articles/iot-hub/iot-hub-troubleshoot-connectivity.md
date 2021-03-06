---
title: Monitorar e solucionar problemas de desconexões com o Hub IoT
description: Aprenda a monitorar e solucionar erros comuns com a conectividade de dispositivos do Hub IoT
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
- 'Role: Technical Support'
ms.openlocfilehash: b194812ef68820a0c310d0bac3b055360c5b5e4a
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92538418"
---
# <a name="monitor-diagnose-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Monitorar, diagnosticar e solucionar problemas de desconexões com o Hub IoT

Problemas de conectividade para dispositivos IoT podem ser difíceis de solucionar porque há muitos possíveis pontos de falha. A lógica do aplicativo, as redes físicas, os protocolos, o hardware, o Hub IoT e outros serviços de nuvem podem causar problemas. Poder detectar e identificar a origem de um problema é crucial. No entanto, uma solução de IoT em escala pode ter milhares de dispositivos, portanto, não é viável verificar dispositivos individuais manualmente. Para ajudar a detectar, diagnosticar e solucionar esses problemas em escala, use os recursos de monitoramento que o Hub IoT oferece pelo Azure Monitor. Esses recursos se limitam ao que o Hub IoT pode observar. Por isso, recomendamos também seguir as melhores práticas de monitoramento para seus dispositivos e outros serviços do Azure.

## <a name="get-alerts-and-error-logs"></a>Obtenha alertas e logs de erros

Use o Azure Monitor para receber alertas e gravar logs quando os dispositivos desconectarem.

### <a name="turn-on-logs"></a>Ativar logs

Para registrar eventos e erros de conexão do dispositivo, crie uma configuração de diagnóstico para [os logs de recursos de conexões do Hub IOT](monitor-iot-hub-reference.md#connections). É recomendável criar essa configuração o mais cedo possível, porque esses logs não são coletados por padrão e, sem eles, você não terá nenhuma informação para solucionar problemas de desconexão de dispositivo com quando eles ocorrerem.

1. Entre no [portal do Azure](https://portal.azure.com).

1. Navegue até seu hub IoT.

1. Selecionar **configurações de Diagnóstico** .

1. Selecione **Adicionar configuração de diagnóstico** .

1. Selecione logs de **conexões** .

1. Para análise mais fácil, selecione **Enviar para log Analytics** ( [consulte preços](https://azure.microsoft.com/pricing/details/log-analytics/)). Consulte o exemplo em [resolver erros de conectividade](#resolve-connectivity-errors).

   ![Configurações recomendadas](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

Para saber mais, consulte [monitorar o Hub IOT](monitor-iot-hub.md).

### <a name="set-up-alerts-for-device-disconnect-at-scale"></a>Configurar alertas para desconexões de dispositivo em escala

Para receber alertas quando os dispositivos são desconectados, configure alertas na métrica **Dispositivos conectados (versão prévia)** .

1. Entre no [portal do Azure](https://portal.azure.com).

2. Navegue até seu hub IoT.

3. Selecione **Alertas** .

4. Selecione **Nova regra de alerta** .

5. Selecione **Adicionar condição** e, em seguida, selecione “Dispositivos conectados (versão prévia)”.

6. Siga as solicitações para configurar o limite e o alerta.

Para saber mais, consulte [O que são alertas no Microsoft Azure?](../azure-monitor/platform/alerts-overview.md).

#### <a name="detecting-individual-device-disconnects"></a>Como detectar desconexões de dispositivo individuais

Para detectar desconexões *por dispositivo* , como quando é necessário saber quando um alocador ficou offline, [configure eventos de desconexão de dispositivo com a Grade de Eventos](iot-hub-event-grid.md).

## <a name="resolve-connectivity-errors"></a>Resolver problemas de conectividade

Quando você ativa logs e alertas para dispositivos conectados, recebe alertas quando ocorrem erros. Esta seção descreve como procurar problemas comuns quando você recebe um alerta. As etapas a seguir pressupõem que você já criou uma configuração de diagnóstico para enviar logs de conexões do Hub IoT a um espaço de trabalho Log Analytics.

1. Entre no [portal do Azure](https://portal.azure.com).

1. Navegue até seu hub IoT.

1. Selecione **Logs** .

1. Para isolar logs de erros de conectividade do Hub IoT, insira a consulta a seguir e selecione **Executar** :

    ```kusto
    AzureDiagnostics
    | where ( ResourceType == "IOTHUBS" and Category == "Connections" and Level == "Error")
    ```

1. Se houver resultados, procure `OperationName`, `ResultType` (código de erro), e `ResultDescription` (mensagem de erro) para obter mais detalhes sobre o erro.

   ![Exemplo de log de erros](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

1. Para os erros mais comuns, siga os guias de resolução do problema:

    - **[404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)**
    - **[401003 IoTHubUnauthorized](iot-hub-troubleshoot-error-401003-iothubunauthorized.md)**
    - **[409002 LinkCreationConflict](iot-hub-troubleshoot-error-409002-linkcreationconflict.md)**
    - **[500001 ServerError](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**
    - **[500008 GenericTimeout](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**

## <a name="i-tried-the-steps-but-they-didnt-work"></a>Tentei as etapas, mas elas não funcionaram

Se as etapas anteriores não ajudarem, tente:

* Se você tiver acesso aos dispositivos problemáticos, seja física ou remotamente (como o SSH), siga o [guia de solução de problemas do lado do dispositivo](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) para continuar com a solução de problemas.

* Verifique se os seus dispositivos estão **Habilitados** no portal do Azure> seu hub IoT> dispositivos IoT.

* Se o dispositivo usar o protocolo MQTT, verifique se a porta 8883 está aberta. Para saber mais, veja [Conectar-se ao Hub IoT (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

* Obtenha ajuda na [Página de P e R da Microsoft para o Hub IoT](/answers/topics/azure-iot-hub.html), no [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub) ou no [Suporte do Azure](https://azure.microsoft.com/support/options/).

Para ajudar a melhorar a documentação para todos, deixe um comentário na seção de comentários abaixo, caso este guia não tenha ajudado.

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre como resolver problemas transitórios, consulte [Tratamento de falhas transitórias](/azure/architecture/best-practices/transient-faults).

* Para saber mais sobre o SDK do IoT do Azure e gerenciar repetições, consulte [Como gerenciar a conectividade e mensagens confiáveis usando SDKs de dispositivo do Hub IoT do Azure](iot-hub-reliability-features-in-sdks.md#connection-and-retry).