---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/15/2020
ms.author: glenga
ms.openlocfilehash: 906413d0a6702e6146779f79d628b5cebf383af1
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92165783"
---
| | |
|--|--|
|**`<DESTINATION>`**| Günlüklerin gönderildiği hedef. Geçerli değerler `AppInsights` ve ' dir `Blob` .<br/>Kullandığınızda `AppInsights` , [işlev uygulamanızda Application Insights etkinleştirildiğinden](../articles/azure-functions/configure-monitoring.md#enable-application-insights-integration)emin olun.<br/>Hedefi olarak ayarladığınızda `Blob` , Günlükler `azure-functions-scale-controller` uygulama ayarında ayarlanan varsayılan depolama hesabı içinde adlı bir blob kapsayıcısında oluşturulur `AzureWebJobsStorage` . |
|**`<VERBOSITY>`** | Günlüğe kaydetme düzeyini belirtir. Desteklenen değerler `None` , `Warning` , ve `Verbose` .<br/>Olarak ayarlandığında `Verbose` , ölçek denetleyicisi çalışan sayındaki her değişiklik için bir neden ve bu kararlara yönelik Tetikleyiciler hakkındaki bilgileri günlüğe kaydeder. Ayrıntılı Günlükler, tetikleyici uyarıları ve ölçek denetleyicisi çalıştırılmadan önce ve sonra Tetikleyiciler tarafından kullanılan karmaları içerir. |

> [!TIP]
> Ölçek denetleyicisi günlük kaydını etkin bıraktığınızda, [işlev uygulamanızı izlemenin olası maliyetlerini](../articles/azure-functions/functions-monitoring.md#application-insights-pricing-and-limits)etkilediğini aklınızda bulundurun. Ölçek denetleyicisinin nasıl davrandığını anlamak ve sonra devre dışı bırakmak için yeterli veri toplanana kadar günlüğe kaydetmeyi etkinleştirmeyi düşünün.