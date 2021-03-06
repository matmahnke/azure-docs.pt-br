---
title: Configurar a criptografia Kerberos do NFSv 4.1 para Azure NetApp Files | Microsoft Docs
description: Descreve como configurar a criptografia Kerberos NFSv 4.1 para Azure NetApp Files e o impacto no desempenho.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/19/2020
ms.author: b-juche
ms.openlocfilehash: edb084a3539f4ab25f328d4cc59ee4ef3279bf07
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92217041"
---
# <a name="configure-nfsv41-kerberos-encryption-for-azure-netapp-files"></a>Configurar a criptografia Kerberos do NFSv 4.1 para Azure NetApp Files

O Azure NetApp Files dá suporte à criptografia de cliente NFS nos modos Kerberos (krb5, krb5i e krb5p) com a criptografia AES-256. Este artigo descreve as configurações necessárias para usar um volume NFSv 4.1 com criptografia Kerberos.

## <a name="requirements"></a>Requisitos

Os seguintes requisitos se aplicam à criptografia de cliente do NFSv 4.1: 

* Conexão Active Directory Domain Services (AD DS) para facilitar a emissão de tíquetes Kerberos 
* Criação de registro de DNS A/PTR para os endereços IP do servidor NFS e do Azure NetApp Files
* Um cliente Linux  
    Este artigo fornece diretrizes para clientes RHEL e Ubuntu.  Outros clientes funcionarão com etapas de configuração semelhantes. 
* Acesso ao servidor NTP  
    Você pode usar um dos controladores de domínio do controlador de Domínio do Active Directory (AD DC) usados com frequência.

## <a name="create-an-nfs-kerberos-volume"></a>Criar um volume Kerberos do NFS

1.  Siga as etapas em [criar um volume de NFS para Azure NetApp files](azure-netapp-files-create-volumes.md) para criar o volume nfsv 4.1.   

    Na página criar um volume, defina a versão do NFS como **nfsv 4.1**e defina Kerberos como **habilitado**.

    > [!IMPORTANT] 
    > Você não pode modificar a seleção de habilitação de Kerberos após a criação do volume.

    ![Criar um volume Kerberos NFSv 4.1](../media/azure-netapp-files/create-kerberos-volume.png)  

2. Selecione **Exportar política** para corresponder ao nível desejado de acesso e à opção de segurança (Kerberos 5, Kerberos 5i ou Kerberos 5P) para o volume.   

    Para o impacto no desempenho do Kerberos, consulte [impacto no desempenho do Kerberos no nfsv 4.1](#kerberos_performance).  

    Você também pode modificar os métodos de segurança do Kerberos para o volume clicando em exportar política no painel de navegação Azure NetApp Files.

3.  Clique em **examinar + criar** para criar o volume nfsv 4.1.

## <a name="configure-the-azure-portal"></a>Configurar o portal do Azure 

1.  Siga as instruções em [criar uma conexão de Active Directory](azure-netapp-files-create-volumes-smb.md#create-an-active-directory-connection).  

    O Kerberos requer que você crie pelo menos uma conta de computador no Active Directory. As informações de conta que você fornece são usadas para criar as contas para volumes de Kerberos SMB *e* nfsv 4.1. Esta máquina é criada automaticamente durante a criação do volume.

2.  Em **realm do Kerberos**, insira o **nome do servidor do AD** e o endereço IP do **KDC** .

    O servidor AD e o IP KDC podem ser o mesmo servidor. Essas informações são usadas para criar a conta da máquina SPN usada pelo Azure NetApp Files. Depois que a conta da máquina for criada, Azure NetApp Files usará os registros do servidor DNS para localizar servidores KDC adicionais, conforme necessário. 

    ![Realm Kerberos](../media/azure-netapp-files/kerberos-realm.png)
 
3.  Clique em **ingressar** para salvar a configuração.

## <a name="configure-active-directory-connection"></a>Configurar conexão Active Directory 

A configuração do NFSv 4.1 Kerberos cria duas contas de computador no Active Directory:
* Uma conta de computador para compartilhamentos SMB
* Uma conta de computador para NFSv 4.1 – você pode identificar essa conta por meio do prefixo `NFS-` . 

Depois de criar o primeiro volume NFSv 4.1 Kerberos, defina o tipo de criptografia para a conta de computador usando o seguinte comando do PowerShell:

`Set-ADComputer $NFSCOMPUTERACCOUNT -KerberosEncryptionType AES256`

## <a name="configure-the-nfs-client"></a>Configurar o cliente NFS 

Siga as instruções em [configurar um cliente NFS para Azure NetApp files](configure-nfs-clients.md) para configurar o cliente NFS.  

## <a name="mount-the-nfs-kerberos-volume"></a><a name="kerberos_mount"></a>Montar o volume Kerberos do NFS

1. Na página **volumes** , selecione o volume do NFS que você deseja montar.

2. Selecione **instruções de montagem** do volume para exibir as instruções.

    Por exemplo: 

    ![Instruções de montagem para volumes Kerberos](../media/azure-netapp-files/mount-instructions-kerberos-volume.png)  

3. Crie o diretório (ponto de montagem) para o novo volume.  

4. Defina o tipo de criptografia padrão como AES 256 para a conta de computador:  
    `Set-ADComputer $NFSCOMPUTERACCOUNT -KerberosEncryptionType AES256 -Credential $ANFSERVICEACCOUNT`

    * Você precisa executar esse comando apenas uma vez para cada conta de computador.
    * Você pode executar esse comando de um controlador de domínio ou de um PC com o [rsat](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems) instalado. 
    * A `$NFSCOMPUTERACCOUNT` variável é a conta de computador criada no Active Directory quando você implanta o volume Kerberos. Essa é a conta com o prefixo `NFS-` . 
    * A `$ANFSERVICEACCOUNT` variável é uma conta de usuário Active Directory sem privilégios com controles delegados na unidade organizacional em que a conta de computador foi criada. 

5. Monte o volume no host: 

    `sudo mount -t nfs -o sec=krb5p,rw,hard,rsize=1048576,wsize=1048576,vers=4.1,tcp $ANFEXPORT $ANFMOUNTPOINT`

    * A `$ANFEXPORT` variável é o `host:/export` caminho encontrado nas instruções de montagem.
    * A `$ANFMOUNTPOINT` variável é a pasta criada pelo usuário no host do Linux.

## <a name="performance-impact-of-kerberos-on-nfsv41"></a><a name="kerberos_performance"></a>Impacto no desempenho do Kerberos no NFSv 4.1 

Esta seção ajuda você a entender o impacto no desempenho do Kerberos no NFSv 4.1.

### <a name="available-security-options"></a>Opções de segurança disponíveis 

As opções de segurança disponíveis atualmente para os volumes NFSv 4.1 são as seguintes: 

* **SEC = sys** usa UIDs UNIX locais e GIDs usando AUTH_SYS para autenticar operações de NFS.
* **sec = krb5** usa Kerberos v5 em vez de UIDs UNIX locais e GIDs para autenticar usuários.
* **SEC = krb5i** usa Kerberos V5 para autenticação de usuário e executa a verificação de integridade de operações de NFS usando somas de verificação seguras para evitar a violação de dados.
* **SEC = krb5p** usa Kerberos V5 para autenticação de usuário e verificação de integridade. Ele criptografa o tráfego NFS para evitar a detecção de tráfego. Essa opção é a configuração mais segura, mas também envolve a maior sobrecarga de desempenho.

### <a name="performance-vectors-tested"></a>Vetores de desempenho testados

Esta seção descreve o impacto de desempenho único no lado do cliente das várias `sec=*` opções.

* O impacto no desempenho foi testado em dois níveis: baixa simultaneidade (baixa carga) e alta simultaneidade (limites superiores de e/s e taxa de transferência).  
* Três tipos de cargas de trabalho foram testadas:  
    * Leitura/gravação aleatória de operação pequena (usando FIO)
    * Leitura/gravação sequencial de operação grande (usando FIO)
    * Carga de trabalho pesada de metadados, conforme gerado por aplicativos como git

### <a name="expected-performance-impact"></a>Impacto esperado no desempenho 

Há duas áreas de foco: carga leve e limite superior. As listas a seguir descrevem a configuração de segurança de impacto de desempenho por configuração de segurança e cenário por cenário. Todas as comparações são feitas em relação ao `sec=sys` parâmetro de segurança. O teste foi feito em um único volume, usando um único cliente. 

Impacto no desempenho do krb5:

* Baixa simultaneidade (r/w):
    * A latência sequencial aumentou 0,3 ms.
    * Latência de e/s aleatória aumentada em 0,2 ms.
    * A latência de e/s de metadados aumentou 0,2 ms.
* Alta simultaneidade (r/w): 
    * A taxa de transferência máxima sequencial não foi afetada por krb5.
    * A e/s aleatória máxima diminuiu em 30% para cargas de trabalho de leitura puras com o impacto geral descartando para zero, uma vez que a carga de trabalho muda para a gravação pura. 
    * A carga de trabalho de metadados máxima diminuiu em 30%.

Impacto no desempenho do krb5i: 

* Baixa simultaneidade (r/w):
    * A latência sequencial aumentou 0,5 ms.
    * Latência de e/s aleatória aumentada em 0,2 ms.
    * A latência de e/s de metadados aumentou 0,2 ms.
* Alta simultaneidade (r/w): 
    * A taxa de transferência sequencial máxima diminuiu em 70% geral, independentemente da mistura da carga de trabalho.
    * A e/s aleatória máxima diminuiu em 50% para cargas de trabalho de leitura puras com o impacto geral que diminui para 25% à medida que a carga de trabalho muda para a gravação pura. 
    * A carga de trabalho de metadados máxima diminuiu em 30%.

Impacto no desempenho do krb5p:

* Baixa simultaneidade (r/w):
    * A latência sequencial aumentou 0,8 ms.
    * Latência de e/s aleatória aumentada em 0,2 ms.
    * A latência de e/s de metadados aumentou 0,2 ms.
* Alta simultaneidade (r/w): 
    * A taxa de transferência sequencial máxima diminuiu em 85% geral, independentemente da mistura da carga de trabalho. 
    * A e/s aleatória máxima diminuiu em 65% para cargas de trabalho de leitura pura com o impacto geral que diminui para 43%, pois a carga de trabalho muda para a gravação pura. 
    * A carga de trabalho de metadados máxima diminuiu em 30%.

## <a name="next-steps"></a>Próximas etapas  

* [Perguntas frequentes sobre Azure NetApp Files](azure-netapp-files-faqs.md)
* [Criar um volume NFS para o Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Criar uma conexão do Active Directory](azure-netapp-files-create-volumes-smb.md#create-an-active-directory-connection)
* [Configurar um cliente NFS para o Azure NetApp Files](configure-nfs-clients.md) 
