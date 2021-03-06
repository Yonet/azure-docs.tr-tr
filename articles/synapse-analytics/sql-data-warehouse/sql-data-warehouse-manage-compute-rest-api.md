---
title: Duraklatma, devam etmeyi, REST API 'lerle ölçeklendirme
description: REST API 'Leri aracılığıyla Azure SYNAPSE Analytics 'te adanmış SQL Havuzu (eski adıyla SQL DW) için işlem gücünü yönetin.
services: synapse-analytics
author: antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 03/29/2019
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 41436da5ed9d82b44a9e1e63fb023c163a9761cf
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98934235"
---
# <a name="rest-apis-for-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te adanmış SQL Havuzu (eski adıyla SQL DW) için REST API 'Leri

Azure SYNAPSE Analytics 'te adanmış SQL Havuzu (eski adıyla SQL DW) için işlem yönetimi için REST API 'Leri.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

Veri ambarı birimlerini değiştirmek için [Create veya Update Database](/rest/api/sql/databases/createorupdate?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API kullanın. Aşağıdaki örnek, sunucu sunucum üzerinde barındırılan MySQLDW veritabanı için veri ambarı birimlerini DW1000 olarak ayarlar. Sunucu, ResourceGroup1 adlı bir Azure Kaynak grubunda bulunur.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2020-08-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": "DW1000c"
    }
}
```

## <a name="pause-compute"></a>İşlem Duraklat

Bir veritabanını duraklatmak için [duraklatma veritabanını](/rest/api/sql/databases/pause?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API kullanın. Aşağıdaki örnek, server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanını duraklatır. Sunucu, ResourceGroup1 adlı bir Azure Kaynak grubunda bulunur.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2020-08-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>İşlem işlemine geri dön

Bir veritabanını başlatmak için REST API [özgeçmişi veritabanını](/rest/api/sql/databases/resume?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) kullanın. Aşağıdaki örnek, server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanını başlatır. Sunucu, ResourceGroup1 adlı bir Azure Kaynak grubunda bulunur.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2020-08-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Veritabanı durumunu denetle

> [!NOTE]
> Veritabanı çevrimiçi iş akışını tamamlarken, bağlantı hatalarıyla sonuçlarken veritabanı durumu şu anda ÇEVRIMIÇI olarak dönebilir. Bağlantı girişimlerini tetiklemek için bu API çağrısını kullanıyorsanız, uygulama kodunuzda 2 ila 3 dakika gecikme eklemeniz gerekebilir.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>Bakım zamanlaması al

Adanmış bir SQL Havuzu (eski adıyla SQL DW) için ayarlanmış bakım zamanlamasını denetleyin.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Bakım zamanlamasını ayarla

Mevcut ayrılmış bir SQL havuzunda (eski adıyla SQL DW) bir bakım zamanlaması ayarlamak ve güncelleştirmek için.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": "Saturday",
                                "startTime": "00:00",
                                "duration": "08:00",
                },
                {
                                "dayOfWeek": "Wednesday",
                                "startTime": "00:00",
                                "duration": "08:00",
                }
                ]
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Işlem yönetimi](sql-data-warehouse-manage-compute-overview.md).
