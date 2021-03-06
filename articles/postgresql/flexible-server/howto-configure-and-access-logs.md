---
title: Günlükleri yapılandırma ve erişme-esnek sunucu-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'na yönelik veritabanı günlüklerine erişme-esnek sunucu
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 3b54fe1ffd49c12db82a038ad449190a0049133f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96492361"
---
# <a name="configure-and-access-logs-in-azure-database-for-postgresql---flexible-server"></a>PostgreSQL için Azure veritabanı 'nda günlükleri yapılandırma ve erişme-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

PostgreSQL günlükleri, esnek bir sunucunun her düğümünde kullanılabilir. Günlükleri bir depolama sunucusuna veya bir analiz hizmetine gönderebilirsiniz. Günlükler, yapılandırma hatalarını belirlemek, sorunlarını gidermek ve onarmak ve performans performansını düzeltmek için kullanılabilir.

## <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma

Azure portal, CLı, REST API ve PowerShell 'i kullanarak Postgres sunucunuz için tanılama ayarlarını etkinleştirebilirsiniz. Seçilecek günlük kategorisi **Postgressqllogs**' dır.

Azure portal kullanarak kaynak günlüklerini etkinleştirmek için:

1. Portalda, Postgres sunucunuzun gezinti menüsünde *Tanılama ayarları* ' na gidin.
   
2. *Tanılama ayarı Ekle*' yi seçin.
   :::image type="content" source="media/howto-logging/diagnostic-settings.png" alt-text="Tanılama Ayarları Ekle düğmesi":::

3. Bu ayarı adlandırın. 

4. Tercih ettiğiniz uç noktayı (depolama hesabı, Olay Hub 'ı, Log Analytics) seçin. 

5. **Postgressqllogs** günlük türünü seçin.
   :::image type="content" source="media/howto-logging/diagnostic-create-setting.png" alt-text="PostgreSQL günlüklerini seçin":::

7. Ayarınızı kaydedin.

PowerShell, CLı veya REST API kullanarak kaynak günlüklerini etkinleştirmek için [Tanılama ayarları](../../azure-monitor/platform/diagnostic-settings.md) makalesini ziyaret edin.

### <a name="access-resource-logs"></a>Kaynak günlüklerine erişin

Günlüklere erişmenin yolu, seçtiğiniz uç noktaya bağlıdır. Azure depolama için [günlük depolama hesabı](../../azure-monitor/platform/resource-logs.md#send-to-azure-storage) makalesine bakın. Event Hubs için bkz. [Azure günlükleri akışı](../../azure-monitor/platform/resource-logs.md#send-to-azure-event-hubs) makalesi.

Azure Izleyici günlükleri için Günlükler seçtiğiniz çalışma alanına gönderilir. Postgres günlükleri **AzureDiagnostics** Collection modunu kullanır, bu nedenle AzureDiagnostics tablosundan sorgulanırlar. Tablodaki alanlar aşağıda açıklanmıştır. [Azure Izleyici günlükleri sorgusuna](../../azure-monitor/log-query/log-query-overview.md) genel bakış bölümünde sorgulama ve uyarı alma hakkında daha fazla bilgi edinin.

Kullanmaya başlamak için kullanabileceğiniz sorgular aşağıda verilmiştir. Sorgular temelinde uyarıları yapılandırabilirsiniz.

Son gün içinde belirli bir sunucu için tüm Postgres günlüklerini arayın

```kusto
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where Category == "PostgreSQLLogs"
| where TimeGenerated > ago(1d) 
```

Localhost olmayan tüm bağlantı girişimlerini ara

```kusto
AzureDiagnostics
| where Message contains "connection received" and Message !contains "host=127.0.0.1"
| where Category == "PostgreSQLLogs" and TimeGenerated > ago(6h)
```

Yukarıdaki sorguda, bu çalışma alanındaki tüm Postgres sunucusu günlüğü için son 6 saat içindeki sonuçlar gösterilecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics sorgularını kullanmaya başlama](../../azure-monitor/log-query/log-analytics-tutorial.md)
- [Azure Olay Hub 'ları](../../event-hubs/event-hubs-about.md) hakkında bilgi edinin