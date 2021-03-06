---
title: O que é treinamento distribuído?
titleSuffix: Azure Machine Learning
description: Saiba mais sobre o treinamento distribuído e como Azure Machine Learning dá suporte a ele.
services: machine-learning
ms.service: machine-learning
author: nibaccam
ms.author: nibaccam
ms.subservice: core
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 62edee6a882191551ce2409646ea8b617576c059
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89651161"
---
# <a name="distributed-training-with-azure-machine-learning"></a>Treinamento distribuído com o Azure Machine Learning

Neste artigo, você aprenderá sobre o treinamento distribuído e como Azure Machine Learning dá suporte a ele para modelos de aprendizado profundo. 

No treinamento distribuído, a carga de trabalho para treinar um modelo é dividida e compartilhada entre vários mini processadores, chamados de nós de trabalho. Esses nós de trabalho funcionam em paralelo para acelerar o treinamento de modelo. O treinamento distribuído pode ser usado para modelos de ML tradicionais, mas é mais adequado para tarefas intensivas de computação e tempo, como [aprendizado profundo](concept-deep-learning-vs-machine-learning.md) para o treinamento de redes neurais profundas. 

## <a name="deep-learning-and-distributed-training"></a>Aprendizado avançado e treinamento distribuído 

Há dois tipos principais de treinamento distribuído: [paralelismo de dados](#data-parallelism) e [paralelismo de modelo](#model-parallelism). Para treinamento distribuído sobre modelos de aprendizado profundo, o [SDK do Azure Machine Learning no Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) dá suporte a integrações com estruturas populares, PyTorch e TensorFlow. Ambas as estruturas empregam o paralelismo de dados para treinamento distribuído e podem aproveitar o [horovod](https://horovod.readthedocs.io/en/latest/summary_include.html) para otimizar as velocidades de computação. 

* [Treinamento distribuído com o PyTorch](how-to-train-pytorch.md#distributed-training)

* [Treinamento distribuído com o TensorFlow](how-to-train-tensorflow.md#distributed-training)

Para modelos de ML que não exigem treinamento distribuído, consulte [treinar modelos com Azure Machine Learning](concept-train-machine-learning-model.md#python-sdk) para diferentes maneiras de treinar modelos usando o SDK do Python.

## <a name="data-parallelism"></a>Paralelismo de dados

O paralelismo de dados é o mais fácil de implementar as duas abordagens de treinamento distribuídas e é suficiente para a maioria dos casos de uso.

Nessa abordagem, os dados são divididos em partições, em que o número de partições é igual ao número total de nós disponíveis, no cluster de computação. O modelo é copiado em cada um desses nós de trabalho, e cada operador opera em seu próprio subconjunto dos dados. Tenha em mente que cada nó precisa ter a capacidade de dar suporte ao modelo que está sendo treinado, ou seja, o modelo precisa se ajustar totalmente a cada nó. O diagrama a seguir fornece uma demonstração visual dessa abordagem.

![Paralelismo de dados-conceito-diagrama](./media/concept-distributed-training/distributed-training.svg)

Cada nó computa de forma independente os erros entre suas previsões para seus exemplos de treinamento e as saídas rotuladas. Por sua vez, cada nó atualiza seu modelo com base nos erros e deve comunicar todas as suas alterações aos outros nós para atualizar seus modelos correspondentes. Isso significa que os nós de trabalho precisam sincronizar os parâmetros do modelo, ou gradientes, no final da computação do lote para garantir que eles estejam treinando um modelo consistente. 

## <a name="model-parallelism"></a>Paralelismo de modelo

No paralelismo de modelo, também conhecido como paralelismo de rede, o modelo é segmentado em diferentes partes que podem ser executadas simultaneamente em nós diferentes, e cada uma delas será executada nos mesmos dados. A escalabilidade desse método depende do grau de paralelização de tarefas do algoritmo e é mais complexa de implementar do que o paralelismo de dados. 

No paralelismo de modelo, os nós de trabalho precisam apenas sincronizar os parâmetros compartilhados, geralmente uma vez para cada etapa de propagação direta ou retroativa. Além disso, os modelos maiores não são uma preocupação, pois cada nó opera em uma subseção do modelo nos mesmos dados de treinamento.

## <a name="next-steps"></a>Próximas etapas

* Saiba como [usar destinos de computação para treinamento de modelo](how-to-set-up-training-targets.md) com o SDK do Python.
* Para obter um exemplo técnico, consulte o [cenário de arquitetura de referência](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/training-deep-learning).
* [Treinar modelos de ml com TensorFlow](how-to-train-tensorflow.md).
* [Treinar modelos de ml com PyTorch](how-to-train-pytorch.md). 