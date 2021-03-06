---
author: baanders
description: Azure dijital TWINS için dosya ekleme-örneği yönetme yolları
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 887d185249f96b5d3be4aab6a96aa3c6c4a85690
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96231421"
---
Bu makalede, [Azure Digital TWINS .net (C#) **SDK 'sını**](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true)kullanarak farklı yönetim işlemlerinin nasıl tamamlanacağı vurgulanmıştır. Ayrıca, [*nasıl yapılır: Azure Digital TWINS API 'leri ve SDK 'Ları kullanma*](../articles/digital-twins/how-to-use-apis-sdks.md)bölümünde açıklanan diğer dil SDK 'larını kullanarak aynı yönetim çağrılarını da oluşturabilirsiniz.

> [!TIP] 
> Tüm SDK yöntemlerinin zaman uyumlu ve zaman uyumsuz sürümlerde yer aldığını unutmayın. Disk belleği çağrılarında zaman uyumsuz yöntemler, `AsyncPageable<T>` zaman uyumlu sürümler geri dönirken döndürülür `Pageable<T>` .

Diğer bir yönetim seçeneği, bu konu için Azure Digital TWINS [**REST API 'lerini**](/rest/api/azure-digitaltwins/) doğrudan Postman gıbı bir rest istemcisi aracılığıyla çağırmmdır. Bunun nasıl yapılacağı hakkında yönergeler için bkz. [*nasıl yapılır: Istekleri Postman Ile oluşturma*](../articles/digital-twins/how-to-use-postman.md).

Son olarak, Azure Digital TWINS **CLI** kullanarak aynı yönetim işlemlerini tamamlayabilirsiniz. CLı kullanma hakkında daha fazla bilgi edinmek için bkz. [*nasıl yapılır: Azure dijital TWINS CLI 'Yi kullanma*](../articles/digital-twins/how-to-use-cli.md).