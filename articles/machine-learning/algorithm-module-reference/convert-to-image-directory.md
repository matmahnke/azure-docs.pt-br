---
title: Converter em Diretório de Imagem
titleSuffix: Azure Machine Learning
description: Saiba como usar o módulo converter em diretório de imagem para converter o conjunto de informações no formato de diretório de imagem.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/09/2020
ms.openlocfilehash: 2e597299c9b157d79a5317c97550fc30820636d6
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940337"
---
# <a name="convert-to-image-directory"></a>Converter em Diretório de Imagem

Este artigo descreve como usar o módulo converter em diretório de imagem para ajudar a converter o conjunto de dados de imagem em tipo de dado de *diretório de imagem* , que é um formato de dados padronizado em tarefas relacionadas à imagem, como classificação de imagem no designer de Azure Machine Learning.

## <a name="how-to-use-convert-to-image-directory"></a>Como usar converter em diretório de imagem  

1. Prepare primeiro o conjunto de seus conjuntos de imagem. 

    Para aprendizado supervisionado, você precisa especificar o rótulo do conjunto de informações de treinamento. O arquivo de conjunto de dados de imagem deve estar na seguinte estrutura:
    
    ```
    Your_image_folder_name/Category_1/xxx.png
    Your_image_folder_name/Category_1/xxy.jpg
    Your_image_folder_name/Category_1/xxz.jpeg
    
    Your_image_folder_name/Category_2/123.png
    Your_image_folder_name/Category_2/nsdf3.png
    Your_image_folder_name/Category_2/asd932_.png
    ```
    
    Na pasta do conjunto de imagens de imagem, há várias subpastas. Cada subpasta contém imagens de uma categoria, respectivamente. Os nomes das subpastas são considerados como rótulos para tarefas como classificação de imagem. Consulte conjuntos de dados [torchvision](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder) para obter mais informações.

    > [!WARNING]
    > Atualmente, os conjuntos de dados com rótulos exportados de rótulos não têm suporte no designer.

    Há suporte para imagens com essas extensões (em minúsculas): '. jpg ', '. jpeg ', '. png ', '. ppm ', '. bmp ', '. PGM ', '. tif ', '. TIFF ', '. webp '. Você também pode ter vários tipos de imagens em uma pasta. Não é necessário conter a mesma contagem de imagens em cada pasta de categorias.

    Você pode usar a pasta ou o arquivo compactado com a extensão '. zip ', '. tar ', '. gz ' e '. bz2 '. **Arquivos compactados são recomendados para melhor desempenho.** 
    
    ![Conjunto de exemplo de imagem](./media/module/image-sample-dataset.png)

    Para pontuação, a pasta do conjunto de imagens de imagem só precisa conter imagens não classificadas.

1. [Registre o conjunto de dados de imagem como um conjunto](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets) de dado de arquivo em seu espaço de trabalho, já que a entrada do módulo converter em diretório de imagem deve ser um conjunto de dados de **arquivo**.

1. Adicione o conjunto de imagens de imagem registrada à tela. Você pode encontrar o conjunto de seus DataSets na categoria **DataSets** na lista de módulos à esquerda de Canvas. Atualmente, o designer não oferece suporte a visualização do conjunto de imagem.

    > [!WARNING]
    > **Não é possível** usar o módulo **importar dados** para importar DataSet de imagem, pois o tipo de saída do módulo **importar dados** é o diretório dataframe, que contém apenas uma cadeia de caracteres de caminho de arquivo.

1. Adicione o módulo **converter no diretório de imagens** à tela. Você pode encontrar esse módulo na categoria ' Pesquisa Visual Computacional/transformação de dados de imagem ' na lista de módulos. Conecte-o ao conjunto de imagens de imagem.
    
3.  Envie o pipeline. Esse módulo pode ser executado em uma GPU ou CPU.

## <a name="results"></a>Resultados

A saída do módulo **converter para o diretório de imagens** está no formato **do diretório de imagens** e pode ser conectada a outros módulos relacionados à imagem dos quais o formato da porta de entrada também é o diretório de imagens.

![Converter em saída do diretório de imagem](./media/module/convert-to-image-directory-output.png)

## <a name="technical-notes"></a>Observações técnicas 

###  <a name="expected-inputs"></a>Entradas esperadas  

| Nome          | Tipo                  | Descrição   |
| ------------- | --------------------- | ------------- |
| Conjunto de dados de entrada | AnyDirectory, ZipFile | Conjunto de dados de entrada |

###  <a name="output"></a>Saída  

| Nome                   | Tipo           | Descrição            |
| ---------------------- | -------------- | ---------------------- |
| Diretório de imagens de saída | ImageDirectory | Diretório de imagens de saída |

## <a name="next-steps"></a>Próximas etapas

Confira o [conjunto de módulos disponíveis](module-reference.md) no Azure Machine Learning. 
