---
title: Solucionar problemas de atualização do provedor de Site Recovery de Microsoft Azure
description: Resolva problemas comuns que ocorrem ao atualizar o provedor de Site Recovery de Microsoft Azure.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/10/2019
ms.author: raynew
ms.openlocfilehash: 5a6e4b415a9fe8ea80a84e415879df9d2f359478
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84434380"
---
# <a name="troubleshoot-microsoft-azure-site-recovery-provider-upgrade-failures"></a>Solucionar problemas de falhas de atualização do provedor do Microsoft Azure Site Recovery

Este artigo ajuda a resolver problemas que podem causar falhas durante uma atualização do provedor do Microsoft Azure Site Recovery.

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>A atualização falha e informa que o provedor do Azure Site Recovery mais recente já está instalado

Ao atualizar o provedor do Microsoft Azure Site Recovery (DRA), a atualização da Instalação Unificada falha e emite a seguinte mensagem de erro:

Não há suporte para atualização porque uma versão superior do software já está instalada.

Para atualizar, execute as etapas a seguir:

1. Baixe a Instalação Unificada do Microsoft Azure Site Recovery:
   1. Na seção "Links para pacotes cumulativos de atualizações atualmente com suporte" do artigo [Atualizações de serviço no Azure Site Recovery](service-updates-how-to.md#links-to-currently-supported-update-rollups), selecione o provedor que você está atualizando.
   2. Na página de rollup, localize a seção **Atualizar informações** e baixe o Pacote Cumulativo de Atualizações para a Instalação Unificada do Microsoft Azure Site Recovery.

2. Abra um prompt de comando e navegue até a pasta na qual você baixou o arquivo de Instalação Unificada. Extraia os arquivos de configuração do download usando o seguinte comando, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;caminho da pasta para os arquivos extraídos&gt;.
    
    Exemplo de comando:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

3. No prompt de comando, navegue até a pasta na qual você extraiu os arquivos e execute os seguintes comandos de instalação:
   
    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART  UCX_SERVER_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /UPGRADE

1. Retorne à pasta na qual você baixou a Instalação Unificada e execute MicrosoftAzureSiteRecoveryUnifiedSetup.exe para concluir a atualização. 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>Falha de atualização devido à pasta de terceiros sendo renomeada

Para que a atualização seja realizada com sucesso, a pasta de terceiros não deve ser renomeada.

Para resolver o problema.

1. Inicie o Editor do Registro (regedit.exe) e abra a ramificação HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\Installed Products\10.
1. Inspecione o valor da chave `Build_Version`. Se estiver definida para a última versão, reduza o número da versão. Por exemplo, se a última versão for 9.22.\* e a chave `Build_Version` estiver definida para esse valor, reduza-a para 9.21.\*.
1. Baixe a última Instalação Unificada do Microsoft Azure Site Recovery:
   1. Na seção "Links para pacotes cumulativos de atualizações atualmente com suporte" do artigo [Atualizações de serviço no Azure Site Recovery](service-updates-how-to.md#links-to-currently-supported-update-rollups), selecione o provedor que você está atualizando.
   2. Na página de rollup, localize a seção **Atualizar informações** e baixe o Pacote Cumulativo de Atualizações para a Instalação Unificada do Microsoft Azure Site Recovery.
1. Abra um prompt de comando e navegue até a pasta na qual você baixou o arquivo de Instalação Unificada e extraia os arquivos de instalação do download usando o seguinte comando, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;caminho da pasta para os arquivos extraídos&gt;.

    Exemplo de comando:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

1. No prompt de comando, navegue até a pasta na qual você extraiu os arquivos e execute os seguintes comandos de instalação:
   
    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART

1. Use o gerenciador de tarefas para monitorar o progresso da instalação. Quando o processo para CX_THIRDPARTY_SETUP.EXE não estiver mais visível no gerenciador de tarefas, prossiga para a próxima etapa.
1. Verifique se C:\thirdparty existe e se a pasta contém as bibliotecas RRD.
1. Retorne à pasta na qual você baixou a Instalação Unificada e execute MicrosoftAzureSiteRecoveryUnifiedSetup.exe para concluir a atualização.

## <a name="upgrade-failure-due-to-master-target-installation-failure"></a>Falha de atualização devido à falha na instalação do destino mestre

Ao atualizar Microsoft Azure provedor de Site Recovery (DRA), a instalação do destino mestre falha com o erro ' o local de instalação não existe e/ou não tem 1 GB de espaço livre e/ou não existe em uma unidade fixa. '.

Isso pode ser devido a um valor nulo para um parâmetro na chave do registro. Para resolver o problema-

1. Inicie o editor do registro (regedit.exe) e abra a ramificação HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\4.
1. Inspecione o valor da chave ' Diretóriodeinstalação '.Se for NULL, adicione o valor do diretório de instalação atual.
1. Da mesma forma, abra a ramificação do HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5 no editor do registro.
1. Inspecione o valor da chave ' Diretóriodeinstalação ' e adicione o valor do diretório de instalação atual.
1. Execute novamente o instalador de instalação unificada.
