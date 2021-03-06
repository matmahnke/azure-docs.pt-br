---
title: Alterações do certificado TLS do Azure
description: Alterações do certificado TLS do Azure
services: security
author: msmbaldwin
tags: azure-resource-manager
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.date: 10/01/2020
ms.author: mbaldwin
ms.openlocfilehash: 93c2f847678292a1d38eff540a813b591c255ef0
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043208"
---
# <a name="azure-tls-certificate-changes"></a>Alterações no certificado TLS do Azure  

A Microsoft está atualizando os serviços do Azure para que eles usem certificados TLS de outro conjunto de ACs (autoridades de certificação) raiz. Essa alteração está sendo feita porque os Certificados de Autoridade de Certificação atuais não estão em conformidade com um dos requisitos de linha de base do fórum do navegador/da AC.

## <a name="when-will-this-change-happen"></a>Quando ocorrerá essa alteração?

- Os serviços do [Azure AD (Active Directory)](/azure/active-directory) iniciaram essa transição em 7 de julho de 2020.
- Todos os pontos de extremidade do TLS/SSL do Azure recém-criados contêm o encadeamento de certificados atualizados até as novas ACs raiz.
- Os pontos de extremidade existentes do Azure estão fazendo a transição em fases desde 13 de agosto de 2020.
- O [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub) e o [DPS](/azure/iot-dps/) permanecerão na AC Baltimore CyberTrust Root, mas as ACs intermediárias deles serão alteradas. [Clique aqui para obter detalhes](https://techcommunity.microsoft.com/t5/internet-of-things/azure-iot-tls-changes-are-coming-and-why-you-should-care/ba-p/1658456).
- O [Armazenamento do Azure](/azure/storage) permanecerá na AC Raiz Baltimore CyberTrust, mas as ACs intermediárias deles serão alteradas. [Clique aqui para obter detalhes](https://techcommunity.microsoft.com/t5/azure-storage/azure-storage-tls-changes-are-coming-and-why-you-care/ba-p/1705518).
- O [cache do Azure para Redis](/azure/azure-cache-for-redis) permanecerá na AC raiz Baltimore Cybertrust, mas suas CAs intermediárias serão alteradas. [Clique aqui para obter detalhes](../../azure-cache-for-redis/cache-whats-new.md).
> [!IMPORTANT]
> É possível que os clientes precisem atualizar os respectivos aplicativos após essa alteração para evitar falhas de conectividade durante a tentativa de se conectarem aos serviços do Azure.

## <a name="what-is-changing"></a>O que está mudando?

Hoje, a maioria dos certificados TLS usados pelos serviços do Azure são encadeados à seguinte AC raiz:

| Nome comum da AC | Impressão digital (SHA1) |
|--|--|
| [Baltimore CyberTrust Root](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |

Os certificados TLS usados pelos serviços do Azure serão encadeados a uma das seguintes ACs raiz:

| Nome comum da AC | Impressão digital (SHA1) |
|--|--|
| [DigiCert Global Root G2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt) | df3c24f9bfd666761b268073fe06d1cc8d4f82a4 |
| [DigiCert Global Root CA](https://cacerts.digicert.com/DigiCertGlobalRootCA.crt) | a8985d3a65e5e5c4b2d7d66d40c6dd2fb19c5436 |
| [Baltimore CyberTrust Root](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |
| [D-TRUST Root Class 3 CA 2 2009](https://www.d-trust.net/cgi-bin/D-TRUST_Root_Class_3_CA_2_2009.crt) | 58e8abb0361533fb80f79b1b6d29d3ff8d5f00f0 |
| [Microsoft RSA Root Certificate Authority 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20RSA%20Root%20Certificate%20Authority%202017.crt) | 73a5e64a3bff8316ff0edccc618a906e4eae4d74 | 
| [Microsoft ECC Root Certificate Authority 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20ECC%20Root%20Certificate%20Authority%202017.crt) | 999a64c37ff47d9fab95f14769891460eec4c3c5 |

## <a name="when-can-i-retire-the-old-intermediate-thumbprint"></a>Quando posso desativar a impressão digital intermediária antiga?

Os Certificados de AC atuais *não* serão revogados até 15 de fevereiro de 2021. Após essa data, você poderá remover as impressões digitais antigas do código.

Se essa data for alterada, você será notificado sobre a nova data de revogação.

## <a name="will-this-change-affect-me"></a>Esta alteração vai me afetar? 

Esperamos que **a maioria dos clientes do Azure não** seja afetada.  No entanto, seu aplicativo poderá ser afetado se ele especificar explicitamente uma lista de ACs aceitáveis. Essa prática é conhecida como anexação de certificado.

Estas são algumas maneiras de detectar se o seu aplicativo foi afetado:

- Pesquise o código-fonte para obter a impressão digital, o nome comum e outras propriedades de certificado de uma das ACs do TLS de TI da Microsoft encontradas [aqui](https://www.microsoft.com/pki/mscorp/cps/default.htm). Se houver uma correspondência, seu aplicativo será afetado. Para resolver esse problema, atualize o código-fonte, incluindo as novas ACs. Como melhor prática, verifique se as ACs podem ser adicionadas ou editadas em curto prazo. Os regulamentos do setor exigem que os Certificados de AC sejam substituídos no prazo de até sete dias e, portanto, os clientes que dependem da anexação precisam tomar uma medida rapidamente.

- Se você tiver um aplicativo que se integra às APIs do Azure ou a outros serviços do Azure e não tiver certeza se ele usa a anexação de certificado, verifique com o fornecedor do aplicativo.

- Diferentes sistemas operacionais e runtimes de linguagem que se comunicam com os serviços do Azure podem exigir etapas adicionais para a criação correta da cadeia de certificados com estas novas raízes:
    - **Linux** : muitas distribuições exigem a adição das ACs a /etc/ssl/certs. Para obter instruções específicas, veja a documentação da distribuição.
    - **Java** : verifique se o repositório de chaves Java contém as ACs listadas acima.
    - **Windows em execução em ambientes desconectados** : os sistemas em execução em ambientes desconectados precisarão ter raízes adicionadas ao repositório de Autoridades de Certificação Raiz Confiáveis e os intermediários adicionados ao repositório de Autoridades de Certificação Intermediárias.
    - **Android** : verifique a documentação do dispositivo e a versão do Android.
    - **Outros dispositivos de hardware, especialmente IoT** : entre em contato com o fabricante do dispositivo.

- Caso você tenha um ambiente em que as regras de firewall estejam definidas para permitir chamadas de saída somente a localizações específicas de download da CRL (lista de certificados revogados) e/ou de verificação do protocolo OCSP. Você precisará permitir as seguintes URLs da CRL e do OCSP:

    - http://crl3&#46;digicert&#46;com
    - http://crl4&#46;digicert&#46;com
    - http://ocsp&#46;digicert&#46;com
    - http://www&#46;d-trust&#46;net
    - http://root-c3-ca2-2009&#46;ocsp&#46;d-trust&#46;net
    - http://crl&#46;microsoft&#46;com
    - http://oneocsp&#46;microsoft&#46;com
    - http://ocsp&#46;msocsp&#46;com
    - http://www&#46; Microsoft&#46;com/pkiops

## <a name="next-steps"></a>Próximas etapas

Se você tiver outras dúvidas, entre em contato conosco por meio de [suporte](https://azure.microsoft.com/support/options/).
