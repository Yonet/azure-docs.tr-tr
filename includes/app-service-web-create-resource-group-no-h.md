---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b59b45de04ebb717dfe55eb17c9dbd92f7523976
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95998067"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Cloud Shell, komutuyla bir kaynak grubu oluşturun [`az group create`](/cli/azure/group?view=azure-cli-latest) . Aşağıdaki örnek *Batı Avrupa* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. **Ücretsiz** katmanda App Service için desteklenen tüm konumları görüntülemek için [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest) komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 

Komut tamamlandığında, bir JSON çıkışı size kaynak grubu özelliklerini gösterir.