---
title: Azure Application Insights bağımlılık Izleme | Microsoft Docs
description: Şirket içi veya Microsoft Azure Web uygulamanızdan gelen bağımlılık çağrılarını Application Insights ile izleyin.
ms.topic: conceptual
ms.date: 08/26/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: df13042656aa077b30bf144aab0a47d9fc0a0662
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91263938"
---
# <a name="dependency-tracking-in-azure-application-insights"></a>Azure Application Insights 'de bağımlılık Izleme 

*Bağımlılık* , uygulamanız tarafından çağrılan bir bileşendir. Genellikle HTTP kullanılarak çağrılan bir hizmet ya da bir veritabanı veya dosya sistemidir. [Application Insights](./app-insights-overview.md) , bağımlılık çağrılarının süresini ölçer, başarısız olup olmadığı ve bağımlılık adı gibi ek bilgilerle birlikte ölçer. Belirli bağımlılık çağrılarını araştırabilir ve bunları isteklerle ve özel durumlarla ilişkilendirmenize olanak sağlayabilirsiniz.

## <a name="automatically-tracked-dependencies"></a>Otomatik olarak izlenen bağımlılıklar

.NET ve .NET Core için Application Insights SDK 'Ları `DependencyTrackingTelemetryModule` , otomatik olarak bağımlılıkları toplayan bir telemetri modülü olan ile birlikte gelir. Bu bağımlılık koleksiyonu, bağlantılı resmi belgelere göre yapılandırıldığında [ASP.net](./asp-net.md) ve [ASP.NET Core](./asp-net-core.md) uygulamaları için otomatik olarak etkinleştirilir. `DependencyTrackingTelemetryModule` , [Bu](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) NuGet paketi olarak sevk edilir ve NuGet paketlerinden ya da ya da ya da herhangi biri kullanılırken otomatik olarak getirilir `Microsoft.ApplicationInsights.Web` `Microsoft.ApplicationInsights.AspNetCore` .

 `DependencyTrackingTelemetryModule` Şu anda aşağıdaki bağımlılıkları otomatik olarak izler:

|Bağımlılıklar |Ayrıntılar|
|---------------|-------|
|Http/https | Yerel veya uzak http/https çağrıları |
|WCF çağrıları| Yalnızca HTTP tabanlı bağlamalar kullanılıyorsa otomatik olarak izlenir.|
|SQL | İle yapılan çağrılar `SqlClient` . SQL sorgusunu yakalamak için [bunu](#advanced-sql-tracking-to-get-full-sql-query) inceleyin.  |
|[Azure depolama (blob, tablo, kuyruk)](https://www.nuget.org/packages/WindowsAzure.Storage/) | Azure Storage Istemcisi ile yapılan çağrılar. |
|[EventHub Istemci SDK 'Sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Sürüm 1.1.0 ve üstü. |
|[ServiceBus Istemci SDK 'Sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Sürüm 3.0.0 ve üstü. |
|Azure Cosmos DB | Yalnızca HTTP/HTTPS kullanılıyorsa otomatik olarak izlenir. TCP modu Application Insights tarafından yakalanmayacaktır. |

Bir bağımlılığı eksik ise veya farklı bir SDK kullanıyorsanız, [otomatik olarak toplanan bağımlılıklar](./auto-collect-dependencies.md)listesinde olduğundan emin olun. Bağımlılık otomatik olarak toplanmazsa, bir [izleme bağımlılığı çağrısıyla](./api-custom-events-metrics.md#trackdependency)el ile izleyebilirsiniz.

## <a name="setup-automatic-dependency-tracking-in-console-apps"></a>Konsol uygulamalarında otomatik bağımlılık izlemeyi ayarla

.NET konsol uygulamalarından bağımlılıkları otomatik olarak izlemek için, NuGet paketini yükledikten `Microsoft.ApplicationInsights.DependencyCollector` sonra `DependencyTrackingTelemetryModule` aşağıdaki gibi başlatın:

```csharp
    DependencyTrackingTelemetryModule depModule = new DependencyTrackingTelemetryModule();
    depModule.Initialize(TelemetryConfiguration.Active);
```

.NET Core konsol uygulamaları için TelemetryConfiguration. Active artık kullanılmıyor. [Çalışan hizmeti belgelerindeki](./worker-service.md) kılavuza ve [ASP.NET Core izleme belgelerine](./asp-net-core.md) bakın

### <a name="how-automatic-dependency-monitoring-works"></a>Otomatik bağımlılık izleme nasıl çalışıyor?

Bağımlılıklar, aşağıdaki tekniklerden biri kullanılarak otomatik olarak toplanır:

* Select metotları etrafında Byte Code izleme kullanma. (StatusMonitor ya da Azure Web App uzantısından ınstrumentationengine)
* EventSource geri çağırmaları
* DiagnosticSource geri çağırmaları (en son .NET/.NET Core SDK 'lerinde)

## <a name="manually-tracking-dependencies"></a>Bağımlılıkları el ile izleme

Aşağıda, otomatik olarak toplanmayan ve bu nedenle el ile izleme gerektiren bazı bağımlılıklar örnekleri verilmiştir.

* Azure Cosmos DB yalnızca [http/https](../../cosmos-db/performance-tips.md#networking) kullanılıyorsa otomatik olarak izlenir. TCP modu Application Insights tarafından yakalanmayacaktır.
* Redis

SDK tarafından otomatik olarak toplanmayan bağımlılıklar için, standart otomatik toplama modülleri tarafından kullanılan [Trackdependency API](api-custom-events-metrics.md#trackdependency) 'sini kullanarak bunları el ile izleyebilirsiniz.

Örneğin, kodunuzun kendinize yazmadığınız bir derlemeyle derlenmesi durumunda, yanıt süreleriniz için hangi katkıyı yaptığını öğrenmek için tüm çağrıları zaman içinde oluşturabilirsiniz. Bu verilerin Application Insights içindeki bağımlılık grafiklerde görüntülenmesini sağlamak için, kullanarak gönderin `TrackDependency` .

```csharp

    var startTime = DateTime.UtcNow;
    var timer = System.Diagnostics.Stopwatch.StartNew();
    try
    {
        // making dependency call
        success = dependency.Call();
    }
    finally
    {
        timer.Stop();
        telemetryClient.TrackDependency("myDependencyType", "myDependencyCall", "myDependencyData",  startTime, timer.Elapsed, success);
    }
```

Alternatif olarak, `TelemetryClient` `StartOperation` `StopOperation` [burada](custom-operations-tracking.md#outgoing-dependencies-tracking) gösterildiği gibi, bağımlılıkları el ile izlemek için kullanılabilecek uzantı yöntemlerini ve bu yöntemleri sağlar

Standart bağımlılık izleme modülünü devre dışı bırakmak istiyorsanız, ASP.NET uygulamaları için [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) içindeki DependencyTrackingTelemetryModule başvurusunu kaldırın. ASP.NET Core uygulamalar için [buradaki](asp-net-core.md#configuring-or-removing-default-telemetrymodules)yönergeleri izleyin.

## <a name="tracking-ajax-calls-from-web-pages"></a>Web sayfalarından AJAX çağrılarını izleme

Web sayfaları için Application Insights JavaScript SDK 'Sı otomatik olarak AJAX çağrılarını bağımlılık olarak toplar.

## <a name="advanced-sql-tracking-to-get-full-sql-query"></a>Tam SQL sorgusu almak için gelişmiş SQL izleme

SQL çağrıları için sunucu ve veritabanının adı her zaman toplanır ve toplanan adı olarak depolanır `DependencyTelemetry` . Tam SQL sorgu metnini içerebilen ' Data ' adlı ek bir alan var.

ASP.NET Core uygulamalar için, artık kullanarak SQL metin toplamayı tercih etmek gerekir.
```csharp
services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) => { module. EnableSqlCommandTextInstrumentation = true; });
```

ASP.NET uygulamalar için, tam SQL sorgu metni, izleme altyapısını kullanmayı veya System. Data. SqlClient kitaplığı yerine [Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient) NuGet paketini kullanmayı gerektiren bayt kodu izleme yardımıyla birlikte toplanır. Tam SQL sorgu toplamayı etkinleştirmek için platforma özgü adımlar aşağıda açıklanmıştır:

| Platform | Tam SQL sorgusu almak için gereken adımlar |
| --- | --- |
| Azure Web App |Web uygulaması denetim masasında [Application Insights dikey penceresini açın](../../azure-monitor/app/azure-web-apps.md) ve .net altında SQL komutlarını etkinleştirin |
| IIS sunucusu (Azure VM, şirket içi vb.) | [Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient) NuGet paketini kullanın veya durum İzleyicisi PowerShell modülünü kullanarak [Izleme altyapısını yükleyip](../../azure-monitor/app/status-monitor-v2-api-reference.md#enable-instrumentationengine) IIS 'yi yeniden başlatın. |
| Azure Cloud Service | [StatusMonitor 'ı yüklemek için başlangıç görevi](../../azure-monitor/app/cloudservices.md#set-up-status-monitor-to-collect-full-sql-queries-optional) ekleme <br> [ASP.net](./asp-net.md) veya [ASP.NET Core uygulamalarına](./asp-net-core.md) yönelik NuGet paketlerini yükleyerek uygulamanızın derleme zamanında eklendi to ApplicationInsights SDK 'sı olması gerekir |
| IIS Express | [Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient) NuGet paketini kullanın.
| Azure Web Işleri | [Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient) NuGet paketini kullanın.

Yukarıdaki platforma özgü adımlara ek olarak, applicationInsights.config dosyasını aşağıdaki ile değiştirerek **SQL komut toplamayı etkinleştirmek için de açıkça tercih etmeniz gerekir** :

```xml
<Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
<EnableSqlCommandTextInstrumentation>true</EnableSqlCommandTextInstrumentation>
</Add>
```

Yukarıdaki durumlarda, izleme altyapısının doğru şekilde doğrulandığının doğru şekilde doğrulanması, toplanan SDK sürümünün `DependencyTelemetry` ' rddp ' olduğunu doğrulamakdır. ' rdddsd ' veya ' rddf ', bağımlılıkların DiagnosticSource veya EventSource Callbacks aracılığıyla toplanacağını ve bu nedenle tam SQL sorgusunun yakalanmayacağını gösterir.

## <a name="where-to-find-dependency-data"></a>Bağımlılık verilerinin nerede bulunacağı

* [Uygulama Haritası](app-map.md) , uygulamanız ve komşu bileşenler arasındaki bağımlılıkları görselleştirir.
* [Işlem tanılama](transaction-diagnostics.md) Birleşik, bağıntılı sunucu verilerini gösterir.
* [Tarayıcılar sekmesi](javascript.md) kullanıcılarınızın TARAYıCıLARıNDAKI Ajax çağrılarını gösterir.
* Bağımlılık çağrılarını denetlemek için yavaş veya başarısız olan isteklerden öğesine tıklayın.
* [Analiz](#logs-analytics) , bağımlılık verilerini sorgulamak için kullanılabilir.

## <a name="diagnose-slow-requests"></a><a name="diagnosis"></a> Yavaş istekleri tanılama

Her istek olayı, uygulamanız isteği işlerken bağımlılık çağrıları, özel durumlar ve izlenen diğer olaylarla ilişkilendirilir. Bu nedenle, bazı istekler hatalı çalışıyorsa, bir bağımlılıktan yavaş yanıt olup olmadığını öğrenebilirsiniz.

### <a name="tracing-from-requests-to-dependencies"></a>İsteklerden bağımlılıklara kadar izleme

**Performans** sekmesini açın ve işlemler yanındaki üst kısımdaki **Bağımlılıklar** sekmesine gidin.

Genel altında bir **bağımlılık adına** tıklayın. Bir bağımlılığı seçtikten sonra, bağımlılık sürelerinin süreleri için bir grafik sağda görünür.

![Performans sekmesinde, üstteki bağımlılık sekmesine tıklayın ve ardından grafikte bir bağımlılık adı](./media/asp-net-dependencies/2-perf-dependencies.png)

Uçtan uca işlem ayrıntılarını görmek için sağ alt köşedeki mavi **örnekler** düğmesine ve ardından bir örneğe tıklayın.

![Uçtan uca işlem ayrıntılarını görmek için bir örneğe tıklayın](./media/asp-net-dependencies/3-end-to-end.png)

### <a name="profile-your-live-site"></a>Canlı sitenizin profilini yapın

Saatin nerede gitcei? [Application Insights Profiler](../../azure-monitor/app/profiler.md) , canlı sitenize http çağrılarını izler ve kodunuzda en uzun süreyi geçen işlevleri gösterir.

## <a name="failed-requests"></a>Başarısız istekler

Başarısız olan istekler, bağımlılıklara yönelik başarısız çağrılar ile de ilişkilendirilebilir.

Soldaki **sorunlar** sekmesine gidebilir ve sonra üstteki **Bağımlılıklar** sekmesine tıklayabilirsiniz.

![Başarısız istekler grafiğine tıklayın](./media/asp-net-dependencies/4-fail.png)

Burada, başarısız bağımlılık sayısını görebileceksiniz. Alt tablodaki bir bağımlılık adına tıklatmaya çalışırken başarısız bir oluşum hakkında daha fazla bilgi almak için. Uçtan uca işlem ayrıntılarını almak için sağ alt köşedeki mavi **Bağımlılıklar** düğmesine tıklayabilirsiniz.

## <a name="logs-analytics"></a>Günlükler (Analiz)

[Kusto sorgu dilinde](/azure/kusto/query/)bağımlılıkları izleyebilirsiniz. Aşağıda bazı örnekler verilmiştir.

* Başarısız bağımlılık çağrılarını bulun:

``` Kusto

    dependencies | where success != "True" | take 10
```

* AJAX çağrılarını bul:

``` Kusto

    dependencies | where client_Type == "Browser" | take 10
```

* İsteklerle ilişkili bağımlılık çağrılarını bul:

``` Kusto

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Sayfa görünümleriyle ilişkili AJAX çağrılarını bulun:

``` Kusto 

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-does-automatic-dependency-collector-report-failed-calls-to-dependencies"></a>*Otomatik bağımlılık toplayıcı raporu bağımlılıklara nasıl çağrı başarısız oldu?*

* Başarısız bağımlılık çağrılarında ' success ' alanı false olarak ayarlanmalıdır. `DependencyTrackingTelemetryModule` rapor etmez `ExceptionTelemetry` . Bağımlılık için tam veri modeli [burada](data-model-dependency-telemetry.md)açıklanmıştır.

### <a name="how-do-i-calculate-ingestion-latency-for-my-dependency-telemetry"></a>*Bağımlılık telemetrimin alma gecikmesini hesaplamak Nasıl yaparım? mi?*

```kusto
dependencies
| extend E2EIngestionLatency = ingestion_time() - timestamp 
| extend TimeIngested = ingestion_time()
```

### <a name="how-do-i-determine-the-time-the-dependency-call-was-initiated"></a>*Nasıl yaparım? bağımlılık çağrısının başlatıldığı saati belirlemektir mi?*

Log Analytics sorgu görünümü ' nde, `timestamp` bağımlılık çağrısı yanıtı alındıktan hemen sonra gerçekleşen trackdependency () çağrısının başlatıldığı süre temsil eder. Bağımlılık çağrısının başladığı saati hesaplamak için, `timestamp` bağımlılık çağrısının kaydedilmesini alıp çıkarabilirsiniz `duration` .

## <a name="open-source-sdk"></a>Açık kaynaklı SDK
Her Application Insights SDK gibi bağımlılık koleksiyonu modülü de açık kaynaktır. [Resmi GitHub](https://github.com/Microsoft/ApplicationInsights-dotnet-server)deposunda kodu okuyun ve katkıda bulunun veya sorunları raporla.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Durumlar](./asp-net-exceptions.md)
* [Kullanıcı & sayfası verileri](./javascript.md)
* [Kullanılabilirlik](./monitor-web-app-availability.md)

