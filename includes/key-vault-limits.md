---
author: rothja
ms.service: key-vault
ms.topic: include
ms.date: 04/21/2020
ms.author: jroth
ms.openlocfilehash: 01b3c9584f3ecddbcdcc6938f5eb469510a47a4e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85838996"
---
### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Principais transações (máximo de transações permitidas a cada 10 segundos por cofre e por região<sup>1</sup>):

|Tipo de chave|Chave HSM<br>Chave CREATE|Chave HSM<br>Todas as outras transações|Chave de software<br>Chave CREATE|Chave de software<br>Todas as outras transações|
|:---|---:|---:|---:|---:|
|RSA de 2.048 bits|5|1,000|10|2\.000|
|RSA de 3.072 bits|5|250|10|500|
|RSA de 4.096 bits|5|125|10|250|
|ECC P-256|5|1,000|10|2\.000|
|ECC P-384|5|1,000|10|2\.000|
|ECC P-521|5|1,000|10|2\.000|
|ECC SECP256K1|5|1,000|10|2\.000|

> [!NOTE]
> Na tabela anterior, vimos que, para chaves de software do tipo RSA de 2.048 bits, são permitidas 2 mil transações GET a cada 10 segundos. Para chaves HSM do tipo RSA de 2.048 bits, são permitidas 1 mil transações GET a cada 10 segundos.
>
> Os limites da limitação acima são ponderados, e a imposição está na soma. Por exemplo, conforme mostrado na tabela anterior, quando você executa operações GET em chaves HSM do tipo RSA, o uso de chaves de 4.096 bits é oito vezes mais caro em comparação com as chaves de 2.048 bits. Isso ocorre porque 1.000/125 = 8.
>
> Em um determinado intervalo de 10 segundos, um cliente do Azure Key Vault poderá fazer *somente uma* das seguintes operações antes de encontrar um código de status HTTP de limitação de `429`:
> - 2 mil transações GET de chave de software do tipo RSA de 2.048 bits
> - 1 mil transações GET de chave HSM do tipo RSA de 2.048 bits
> - 125 transações GET de chave HSM do tipo RSA de 4.096 bits
> - 124 transações GET de chave HSM do tipo RSA de 4.096 bits e 8 transações GET de chave HSM do tipo RSA de 2.048 bits

### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Segredos, chaves de conta de armazenamento gerenciado e transações de cofre:

| Tipo de transação | Máximo de transações permitidas a cada 10 segundos por cofre e por região<sup>1</sup> |
| --- | --- |
| Todas as transações |2\.000 |

Para obter informações sobre como lidar com a limitação quando esses limites forem excedidos, confira [Diretrizes de limitação do Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> O limite de assinaturas para todos os tipos de transações será cinco vezes o limite do cofre de chaves. Por exemplo, outras transações HSM por assinatura são limitadas a 5.000 transações a cada 10 segundos por assinatura.

### <a name="azure-private-link-integration"></a>Integração de Link Privado do Azure

> [!NOTE]
> O número de cofres de chaves com pontos de extremidade privados habilitados por assinatura é um limite ajustável. Esse limite mostrado abaixo é o limite padrão. Se você quiser solicitar um aumento de limite para o serviço, envie um email para akv-privatelink@microsoft.com. Aprovaremos essas solicitações dependendo do caso.

| Recurso | Limite |
| -------- | ----- |
| Pontos de extremidade privados por cofre de chaves | 64 |
| Cofres de chaves com pontos de extremidade privados por assinatura | 400 |
