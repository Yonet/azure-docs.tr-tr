---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: 179ae760f146a5ac3041a54065ae12147f3f9bf0
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97739829"
---
Ayrıca, aynı zamanda bir test kümesi oluşturduğundan, yöntem imzasında yeni parametreyi işlemek için bu testleri güncelleştirmeniz gerekir `msg` `run` .  

_Src/test/Java_ altındaki test kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve altındaki kod satırını `//Invoke` aşağıdaki kodla değiştirin.

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/test/java/com/function/FunctionTest.java" range="48-50":::
