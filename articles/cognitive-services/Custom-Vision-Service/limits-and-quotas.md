---
title: Limites e cotas – Serviço de Visão Personalizada
titleSuffix: Azure Cognitive Services
description: Este artigo explica os diferentes tipos de chaves de licenciamento e sobre os limites e cotas para o Serviço de Visão Personalizada.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: pafarley
ms.openlocfilehash: b79bf5e8ead16bbdf9c69e8d7faae43fa778ab3e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316572"
---
# <a name="limits-and-quotas"></a>Limites e cotas

Há duas camadas de chaves para o serviço de Visão Personalizada. Você pode se inscrever para uma assinatura F0 (gratuita) ou S0 (padrão) por meio do portal do Azure. Consulte a [página de preços de serviços cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) correspondente para obter detalhes sobre preços e transações.

Espera-se que o número de imagens de treinamento por projeto e tags por projeto aumente com o tempo para projetos S0.

|Fator|**F0**|**S0**|
|-----|-----|-----|
|Projetos|2|100|
|Imagens de treinamento por projeto |5\.000|100.000|
|Previsões/mês|10.000 |Ilimitado|
|Marcas/projeto|50|500|
|Iterações |10|10|
|Mínimo de imagens rotuladas por marca, classificação (50 + recomendado) |5|5|
|Mínimo de imagens rotuladas por marca, detecção de objeto (mais de 50 + recomendado)|15|15|
|Por quanto tempo as imagens de previsão são armazenadas|30 dias|30 dias|
|Operações de [Previsão](https://go.microsoft.com/fwlink/?linkid=865445) com armazenamento (Transações por Segundo)|2|10|
|Operações de [Previsão](https://go.microsoft.com/fwlink/?linkid=865445) sem armazenamento (Transações por Segundo)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (chamadas à API por Segundo)|2|10|
|[Outras chamadas à API](https://go.microsoft.com/fwlink/?linkid=865446) (Transações por Segundo)|10|10|
|Tipos de imagem aceitos|jpg, png, BMP, gif|jpg, png, BMP, gif|
|Altura/largura da imagem mínima em pixels|256 (consulte a observação)|256 (consulte a observação)|
|Altura/largura máxima da imagem em pixels|10.240|10.240|
|Tamanho máximo da imagem (upload da imagem de treinamento) |6 MB|6 MB|
|Tamanho máximo da imagem (previsão)|4 MB|4 MB|
|Imagem de treinamento da detecção de regiões máximas por objeto|300|300|
|Máximo de marcas por imagem de classificação|100|100|

> [!NOTE]
> Imagens com menos de 256 pixels serão aceitas, mas em escala.
> A taxa de proporção da imagem não deve ser maior que 25
