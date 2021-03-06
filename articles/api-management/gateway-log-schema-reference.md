---
title: Başvuru-Azure API Management kaynak günlüğü
description: Azure API Management GatewayLogs kaynak günlüğü için şema başvurusu
services: api-management
author: dlepow
ms.service: api-management
ms.custom: mvc
ms.topic: tutorial
ms.date: 10/14/2020
ms.author: apimpm
ms.openlocfilehash: 064ac21e01239f7be3ddca9e48089ce880c6af58
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93380485"
---
# <a name="reference-api-management-resource-log-schema"></a>Başvuru: API Management kaynak günlüğü şeması

Bu makalede Azure API Management GatewayLogs kaynak günlüğü için bir şema başvurusu sunulmaktadır. Günlük girişleri, [üst düzey ortak şemadaki](../azure-monitor/platform/resource-logs-schema.md#top-level-common-schema)alanları da içerir.

API Management kaynak günlüğü toplamayı etkinleştirmek için bkz. [yayımlanan API 'Leri izleme](api-management-howto-use-azure-monitor.md#resource-logs).

## <a name="gatewaylogs-schema"></a>GatewayLogs şeması

Her API isteği için aşağıdaki özellikler günlüğe kaydedilir.

| Özellik  | Tür | Açıklama |
| ------------- | ------------- | ------------- |
| method | string | Gelen isteğin HTTP yöntemi |
| url | string | Gelen isteğin URL’si |
| responseCode | tamsayı | Bir istemciye gönderilen HTTP yanıtının durum kodu |
| responseSize | tamsayı | İstek işlenirken bir istemciye gönderilen bayt sayısı | 
| cache | string | İstek işlemede API Management önbelleği katılımı durumu (isabet, isabetsizlik, yok) | 
| apiId | string | Geçerli istek için API varlığı tanımlayıcısı | 
| operationId | string | Geçerli istek için işlem varlığı tanımlayıcısı | 
| clientProtocol | string | Gelen isteğin HTTP protokolü sürümü |
| clientTime | tamsayı | Genel istemci g/ç (bağlantı, gönderme ve alma bayt) için harcanan milisaniye sayısı | 
| apiRevision | string | Geçerli istek için API düzeltmesi | 
| clientTlsVersion| string | İstemci gönderme isteği tarafından kullanılan TLS sürümü |
| lastError | object | Başarısız bir istek için, son istek işleme hatası hakkındaki ayrıntılar | 
| backendMethod | string | Arka uca gönderilen isteğin HTTP yöntemi |
| backendUrl | string | Arka uca gönderilen isteğin URL’si |
| backendResponseCode | tamsayı | Arka uçtan alınan HTTP yanıtının kodu |
| backedProtocol | string | Arka uca gönderilen isteğin HTTP protokolü sürümü |
| backendTime | tamsayı | Genel arka uç GÇ (bağlanma, gönderme ve alma bayt) için harcanan milisaniye sayısı | 


## <a name="next-steps"></a>Sonraki adımlar

* API Management API 'Leri izleme hakkında daha fazla bilgi için bkz. [yayımlanan API 'Leri izleme](api-management-howto-use-azure-monitor.md)
* [Azure Kaynak günlükleri Için ortak ve hizmete özgü şema](../azure-monitor/platform/resource-logs-schema.md) hakkında daha fazla bilgi edinin

