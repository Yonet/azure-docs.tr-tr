---
title: Azure Izleyici uyarıları için geçişi anlama
description: Uyarı geçişinin nasıl çalıştığını anlayın ve sorunları giderin.
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: yalavi
author: yalavi
ms.subservice: alerts
ms.openlocfilehash: e57b3dd31455db245103469874c517fe54479110
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526916"
---
# <a name="understand-migration-options-to-newer-alerts"></a>Geçiş seçeneklerini daha yeni uyarılara anlayın

Klasik uyarılar, genel bulut kullanıcıları için [devre](./monitoring-classic-retirement.md) dışı bırakılsa da, henüz yeni uyarıları desteklemeyen kaynaklar için sınırlı kullanımda olabilir. Kalan uyarılar geçişi, [Azure Kamu Bulutu](../../azure-government/documentation-government-welcome.md)ve [Azure Çin 21Vianet](https://docs.azure.cn/)için yakında yeni bir tarih duyurulacaktır.

Bu makalede, el ile geçiş ve gönüllü geçiş aracının nasıl çalıştığı ve kalan uyarı kurallarının geçirilmesi için kullanılacağı açıklanmaktadır. Ayrıca, bazı yaygın sorunlara yönelik düzeltmeler de açıklanmaktadır.

> [!IMPORTANT]
> Etkinlik günlüğü uyarıları (hizmet durumu uyarıları dahil) ve günlük uyarıları geçişten etkilenmez. Geçiş yalnızca [burada](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform)açıklanan klasik uyarı kuralları için geçerlidir.

> [!NOTE]
> Klasik uyarı kurallarınız geçersiz ise, bunlar [kullanım dışı ölçümlerde](#classic-alert-rules-on-deprecated-metrics) veya silinmiş kaynaklardır ve hizmet devre dışı bırakıldıktan sonra kullanılamaz olur.

## <a name="manually-migrating-classic-alerts-to-newer-alerts"></a>Klasik uyarıları daha yeni uyarılara el ile geçirme

Kalan uyarılarını el ile geçirmeye ilgilenen müşteriler, aşağıdaki bölümleri kullanarak zaten bu işlemleri yapabilir. Bu bölümler, kaynak sağlayıcısı tarafından kullanımdan kaldırılan ölçümleri de tanımlar ve şu anda doğrudan geçirilemez.

### <a name="guest-metrics-on-virtual-machines"></a>Sanal makinelerde Konuk ölçümleri

Konuk ölçümlerinde yeni ölçüm uyarıları oluşturabilmeniz için, Konuk ölçümlerinin Azure Izleyici özel ölçüm deposuna gönderilmesi gerekir. Tanılama ayarları 'nda Azure Izleyici havuzunu etkinleştirmek için şu yönergeleri izleyin:

- [Windows VM 'Leri için konuk ölçümleri etkinleştiriliyor](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Linux sanal makineleri için konuk ölçümlerini etkinleştirme](collect-custom-metrics-linux-telegraf.md)

Bu adımlar yapıldıktan sonra, Konuk ölçümlerinde yeni ölçüm uyarıları oluşturabilirsiniz. Yeni ölçüm uyarıları oluşturduktan sonra klasik uyarıları silebilirsiniz.

### <a name="storage-account-metrics"></a>Depolama hesabı ölçümleri

Depolama hesaplarında bulunan tüm klasik uyarılar, bu ölçümlerde uyarılar dışında geçirilebilir:

- Yüztauthorizationerror
- Percentclienentothererror
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- Anonymouskısıtlar Lingerror
- Saskısıtlar Lingerror
- ThrottlingError

Yüzde ölçümlerinde klasik uyarı kurallarının [, eski ve yeni depolama ölçümleri arasındaki eşlemeye](../../storage/common/storage-metrics-migration.md#metrics-mapping-between-old-metrics-and-new-metrics)göre geçirilmesi gerekir. Kullanılabilir yeni ölçüm mutlak bir değer olduğundan eşiklerin uygun şekilde değiştirilmesi gerekir.

Aynı işlevselliği sağlayan bir birleştirilmiş ölçüm olmadığından, Anonymouskısıtlar Lingerror 'da klasik uyarı kuralları, Saskısıtlar Lingerror ve kısıtlar Lingerror iki yeni uyarıya bölünmelidir. Eşiklerin uygun şekilde uyarlanması gerekir.

### <a name="cosmos-db-metrics"></a>Cosmos DB ölçümleri

Cosmos DB ölçümlerinde bulunan tüm klasik uyarılar, bu ölçümlerde uyarılar dışında geçirilebilir:

- Saniye başına ortalama Istek
- Tutarlılık Düzeyi
- Http 2xx
- Http 3xx
- Http 400
- Http 401
- İç sunucu hatası
- Dakikada tüketilen maksimum RUPD
- Saniyede en fazla ru
- Mongo sayısı başarısız Istekler
- Mongo silme başarısız Istekleri
- Mongo ekleme başarısız Istekleri
- Diğer başarısız Istekleri Mongo
- Mongo diğer Istek ücreti
- Mongo diğer Istek hızı
- Mongo sorgu başarısız Istekleri
- Mongo güncelleştirme başarısız Istekleri
- Gözlemlenen okuma gecikmesi
- Gözlemlenen yazma gecikmesi
- Hizmet kullanılabilirliği
- Depolama Kapasitesi
- Kısıtlanmış Istekler
- Toplam İstek Sayısı

Saniyede ortalama Istek sayısı, tutarlılık düzeyi, dakikada tüketilen maksimum RUPISI, saniyede maksimum ru, gözlemlenen okuma gecikmesi, gözlemlenen yazma gecikmesi, depolama kapasitesi Şu anda [Yeni sistemde](metrics-supported.md#microsoftdocumentdbdatabaseaccounts)kullanılamıyor.

Http 2xx, http 3xx, HTTP 400, http 401, Iç sunucu hatası, hizmet kullanılabilirliği, kısıtlanmış Istekler ve toplam Istekler gibi istek ölçümlerine ilişkin uyarılar, bu sayede istekler, klasik ölçümler ve yeni ölçümler arasında farklılık fark ettiğinden geçirilmez. Bu uyarılarla ilgili uyarıların ayarlandığı eşiklerle el ile yeniden oluşturulması gerekir.

Aynı işlevselliği sağlayan birleştirilmiş bir ölçüm olmadığından, Mongo başarısız olan Isteklerin ölçümleri birden çok uyarılara bölünmelidir. Eşiklerin uygun şekilde uyarlanması gerekir.

### <a name="classic-compute-metrics"></a>Klasik işlem ölçümleri

Klasik işlem kaynakları henüz yeni uyarılarla desteklenmediğinden, klasik işlem ölçümlerine ilişkin tüm uyarılar geçiş aracı kullanılarak geçirilmez. Bu kaynak türlerinde yeni uyarılar için destek şu anda genel önizlemededir ve müşteriler, klasik uyarı kurallarına göre yeni eşdeğer uyarı kuralları yeniden oluşturabilir.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Kullanım dışı ölçümler üzerinde klasik uyarı kuralları

Bunlar, daha önce desteklenen ancak sonunda kullanım dışı olan ölçümler üzerinde klasik uyarı kurallarıdır. Müşterilerin küçük bir yüzdesi, bu tür ölçümler üzerinde geçersiz klasik uyarı kurallarına sahip olabilir. Bu uyarı kuralları geçersiz olduğundan, geçirilmez.

| Kaynak türü| Kullanım dışı olan metriler |
|-------------|----------------- |
| Microsoft. Dbformyısql/sunucuları | compute_consumption_percent, compute_limit |
| Microsoft. DBforPostgreSQL/sunucuları | compute_consumption_percent, compute_limit |
| Microsoft. Network/Publicıpaddresses | defaultddoçabaggerrate |
| Microsoft. SQL/Servers/veritabanları | service_level_objective, storage_limit, storage_used, azaltma, dtu_consumption_percent, storage_used |
| Microsoft. Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft. Web/hostingEnvironments/workerpools | BytesReceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Eşit yeni uyarı kuralları ve eylem grupları oluşturma

Geçiş Aracı, klasik uyarı kurallarınızı eşdeğer yeni uyarı kurallarına ve eylem gruplarına dönüştürür. Çoğu klasik uyarı kuralları için, eşdeğer yeni uyarı kuralları, ve gibi aynı özelliklerle aynı ölçümdedir `windowSize` `aggregationType` . Ancak bazı klasik uyarı kuralları, yeni sistemde farklı, eşdeğer bir ölçüme sahip olan ölçümlerde bulunur. Aşağıdaki ilkeler aşağıdaki bölümde belirtilmedikçe klasik uyarıların geçirilmesi için geçerlidir:

- **Sıklık**: klasik veya yeni bir uyarı kuralının koşulu ne sıklıkta denetleyeceğini tanımlar. `frequency`Klasik uyarı kuralları Kullanıcı tarafından yapılandırılamaz ve 1 dak Application Insights bileşenleri hariç tüm kaynak türleri için her zaman 5 dakikadır. Bu nedenle denk kuralların sıklığı sırasıyla 5 dk ve 1 dk olarak ayarlanmıştır.
- **Toplama türü**: ölçümün ilgilendiğiniz pencere üzerinden nasıl toplandığını tanımlar. `aggregationType`Ayrıca, klasik uyarılar ve çoğu ölçüm için yeni uyarılar arasında de aynıdır. Bazı durumlarda, ölçüm klasik uyarılar ve yeni uyarılar arasında farklılık gösterdiğinden, eşdeğer `aggregationType` veya `primary Aggregation Type` ölçüm için tanımlanan değer kullanılır.
- **Units**: uyarının oluşturulduğu ölçümün özelliği. Bazı eşdeğer ölçümler farklı birimlere sahiptir. Eşik gerektiği şekilde uygun şekilde ayarlanır. Örneğin, özgün ölçüm, birim olarak saniyeler içeriyorsa, ancak eşdeğer yeni ölçüm, birim olarak milisaniyeye sahipse, aynı davranışı sağlamak için özgün eşik 1000 ile çarpılır.
- **Pencere boyutu**: eşiğe göre Karşılaştırılacak ölçüm verilerinin üzerine toplanan pencereyi tanımlar. `windowSize`5 dakika, 15 dakika, 30 dakika, 1saat, 3saat, 6 saat, 12 saat, 1 gün gibi standart değerler için, denk yeni uyarı kuralı için değişiklik yapılmaz. Diğer değerler için en yakın `windowSize` kullanım için seçilir. Çoğu müşteri için, bu değişikliğe göre hiçbir etkisi yoktur. Müşterilerin küçük bir yüzdesi için, tam olarak aynı davranışı almak üzere eşiğin ince ayar bir olması gerekebilir.

Aşağıdaki bölümlerde, yeni sistemde farklı, eşdeğer bir ölçüme sahip olan ölçümleri ayrıntılandırıyoruz. Klasik ve yeni uyarı kuralları için aynı kalan her türlü ölçüm listelenmez. [Burada](metrics-supported.md)yeni sistemde desteklenen ölçümlerin listesini bulabilirsiniz.

### <a name="microsoftstorageaccountsservices"></a>Microsoft. StorageAccounts/Services

Blob, tablo, dosya ve kuyruk gibi depolama hesabı Hizmetleri için aşağıdaki ölçümler aşağıda gösterildiği gibi eşdeğer ölçümlere eşlenir:

| Klasik uyarılarda ölçüm | Yeni uyarılardaki eşdeğer ölçüm | Yorumlar|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| "ResponseType" = "AuthorizationError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü| |
| Anonymousclienentothererror | "ResponseType" = "Clienentothererror" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousClientTimeOutError| "ResponseType" = "ClientTimeOutError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousNetworkError | "ResponseType" = "NetworkError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousServerOtherError | "ResponseType" = "Sunucudiğerhatası" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousServerTimeOutError | "ResponseType" = "ServerTimeOutError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousSuccess | "ResponseType" = "Success" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AuthorizationError | "ResponseType" = "AuthorizationError" boyutlarıyla işlem ölçümü | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | Başarılı Sunucugecikmesi | |
| Kapasite | BlobCapacity | `aggregationType`' Last ' yerine ' Average ' kullanın. Ölçüm yalnızca blob Hizmetleri için geçerlidir |
| Clienentothererror | "ResponseType" = "Clienentothererror" boyutlarıyla işlem ölçümü  | |
| ClientTimeoutError | "ResponseType" = "ClientTimeOutError" boyutlarıyla işlem ölçümü | |
| ContainerCount | ContainerCount | `aggregationType`' Last ' yerine ' Average ' kullanın. Ölçüm yalnızca blob Hizmetleri için geçerlidir |
| NetworkError | "ResponseType" = "NetworkError" boyutlarıyla işlem ölçümü | |
| ObjectCount | BlobCount| `aggregationType`' Last ' yerine ' Average ' kullanın. Ölçüm yalnızca blob Hizmetleri için geçerlidir |
| SASAuthorizationError | "ResponseType" = "AuthorizationError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASClientOtherError | "ResponseType" = "Clienentothererror" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASClientTimeOutError | "ResponseType" = "ClientTimeOutError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASNetworkError | "ResponseType" = "NetworkError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASServerOtherError | "ResponseType" = "Sunucudiğerhatası" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASServerTimeOutError | "ResponseType" = "ServerTimeOutError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASSuccess | "ResponseType" = "Success" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| ServerOtherError | Boyutlarla birlikte işlem ölçümü "ResponseType" = "Sunucudiğerhatası" | |
| ServerTimeOutError | "ResponseType" = "ServerTimeOutError" boyutlarıyla işlem ölçümü  | |
| Başarılı | Boyutlarla birlikte işlem ölçümü "ResponseType" = "Success" | |
| TotalBillableRequests| İşlemler | |
| TotalEgress | Çıkış | |
| TotalIngress | Giriş | |
| TotalRequests | İşlemler | |

### <a name="microsoftinsightscomponents"></a>Microsoft. Insights/bileşenler

Application Insights için, eşdeğer ölçümler aşağıda gösterildiği gibidir:

| Klasik uyarılarda ölçüm | Yeni uyarılardaki eşdeğer ölçüm | Yorumlar|
|--------------------------|---------------------------------|---------|
| kullanılabilirlik. Kullanılabilirbilitymetric. değer | Kullanılabilirlik sonuçları/kullanılabilirliği yüzdesi|   |
| AVAILABILITY. durationMetric. Value | Kullanılabilirlik sonuçları/süresi| Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın.  |
| basicExceptionBrowser. Count | özel durumlar/tarayıcı|  ' `aggregationType` Sum ' yerine ' Count ' kullanın. |
| basicExceptionServer. Count | özel durumlar/sunucu| ' `aggregationType` Sum ' yerine ' Count ' kullanın.  |
| clientPerformance. clientProcess. Value | Browserzamanlamalar/processingDuration| Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın.  |
| clientPerformance. networkConnection. Value | Browserzamanlamalar/networkDuration|  Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın. |
| clientPerformance. receiveRequest. Value | Browserzamanlamalar/receiveDuration| Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın.  |
| clientPerformance. sendRequest. Value | Browserzamanlamalar/sendDuration| Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın.  |
| clientPerformance. Total. Value | Browserzamanlamalar/totalDuration| Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın.  |
| performanceCounter.available_bytes. değer | performanceCounters/memoryAvailableBytes|   |
| performanceCounter.io_data_bytes_per_sec. değer | performanceCounters/Processıobitespersecond|   |
| performanceCounter.number_of_exceps_thrown_per_sec. değer | performanceCounters/exceptionsPerSecond|   |
| performanceCounter.percentage_processor_time_normalized. değer | performanceCounters/processCpuPercentage|   |
| performanceCounter.percentage_processor_time. değer | performanceCounters/processCpuPercentage| Özgün ölçüm tüm çekirdekler genelinde olduğundan ve yeni ölçüm bir çekirdeğe normalleştirildikleri için eşiğin uygun şekilde değiştirilmesi gerekir. Geçiş Aracı eşikleri değiştirmez.  |
| performanceCounter.percentage_processor_total. değer | performanceCounters/Processorcpuyüzdesi|   |
| performanceCounter.process_private_bytes. değer | performanceCounters/processPrivateBytes|   |
| performanceCounter.request_execution_time. değer | performanceCounters/requestExecutionTime|   |
| performanceCounter.requests_in_application_queue. değer | performanceCounters/Requestsınqueue|   |
| performanceCounter.requests_per_sec. değer | performanceCounters/requestsPerSecond|   |
| istek. Duration | istek/süre| Klasik ölçüm 'in birimleri Saniyeler içinde ve yeni bir tane için milisaniye cinsinden olan özgün eşiği 1000 ile çarpın.  |
| istek. oran | istek/hız|   |
| requestFailed. Count | istek/başarısız| ' `aggregationType` Sum ' yerine ' Count ' kullanın.   |
| View. Count | pageViews/Count| ' `aggregationType` Sum ' yerine ' Count ' kullanın.   |

### <a name="microsoftdocumentdbdatabaseaccounts"></a>UmentDB/databaseAccounts Microsoft.Doc

Cosmos DB için, eşdeğer ölçümler aşağıda gösterildiği gibidir:

| Klasik uyarılarda ölçüm | Yeni uyarılardaki eşdeğer ölçüm | Yorumlar|
|--------------------------|---------------------------------|---------|
| AvailableStorage     |AvailableStorage|   |
| Veri Boyutu | Veri kullanımı| |
| Belge sayısı | DocumentCount||
| Dizin boyutu | Indexusage||
| Mongo sayımı Istek ücreti| "CommandName" = "Count" boyutuyla Mongorequestücret||
| Mongo sayımı Istek hızı | Dimension "CommandName" = "Count" olan MongoRequestsCount||
| Mongo silme Isteği ücreti | Dimension "CommandName" = "Sil" olacak mongorequestücret||
| Mongo silme Isteği hızı | Dimension ile MongoRequestsCount "CommandName" = "Delete"||
| Mongo ekleme Isteği ücreti | "CommandName" = "INSERT" boyutuyla Mongorequestücretle||
| Mongo ekleme Isteği oranı | Dimension ile MongoRequestsCount "CommandName" = "INSERT"||
| Mongo sorgu Isteği ücreti | "CommandName" = "Find" boyutuyla Mongorequestücretle||
| Mongo sorgu Isteği hızı | Dimension ile MongoRequestsCount "CommandName" = "Find"||
| Mongo güncelleştirme Isteği ücreti | Dimension "CommandName" = "Update" ile mongorequestücret||
| Hizmet kullanılamıyor| ServiceAvailability||
| TotalRequestUnits | TotalRequestUnits||

### <a name="how-equivalent-action-groups-are-created"></a>Denk eylem grupları oluşturma

Klasik uyarı kurallarında, uyarı kuralına bağlı e-posta, Web kancası, mantıksal uygulama ve Runbook eylemleri vardır. Yeni uyarı kuralları, birden fazla uyarı kuralı arasında yeniden kullanılabilen eylem gruplarını kullanır. Geçiş Aracı, eylemi kaç tane uyarı kuralı kullandığını bağımsız olarak aynı eylemler için tek bir eylem grubu oluşturur. Geçiş Aracı tarafından oluşturulan eylem grupları ' Migrated_AG * ' adlandırma biçimini kullanır.

> [!NOTE]
> Klasik uyarılar, klasik yönetici rollerini bilgilendirmek için kullanıldığında klasik yöneticinin yerel ayarlarına bağlı olarak yerelleştirilmiş e-postalar gönderdi. Yeni uyarı e-postaları eylem grupları aracılığıyla gönderilir ve yalnızca Ingilizce olarak gösterilir.

## <a name="rollout-phases"></a>Dağıtım aşamaları

Geçiş Aracı, klasik uyarı kuralları kullanan müşterilere yönelik aşamalar halinde kullanıma alınıyor. Abonelik, Aracı kullanılarak geçirilmesi için hazırsa, abonelik sahipleri bir e-posta alır.

> [!NOTE]
> Araç aşamalarda kullanıma alındığından, aboneliklerinizin bazılarının erken aşamalar sırasında henüz geçirilmeye hazır olmadığını görebilirsiniz.

Aboneliklerin çoğu şu anda geçiş için hazırlanın olarak işaretlendi. Yalnızca aşağıdaki kaynak türlerinde klasik uyarıları olan abonelikler hala geçişe hazırlanmaz.

- Microsoft. classicCompute/domainNames/yuvalar/roller
- Microsoft. Insights/bileşenler

## <a name="who-can-trigger-the-migration"></a>Geçişi kimlerin tetikleyebilen?

Abonelik düzeyinde katkıda bulunan Izleme katılımcısı rolü olan herhangi bir Kullanıcı, geçişi tetikleyebilir. Aşağıdaki izinlerle özel bir rolü olan kullanıcılar geçişi de tetikleyebilir:

- */Read
- Microsoft. Insights/actiongroups/*
- Microsoft. Insights/AlertRules/*
- Microsoft. Insights/metricAlerts/*
- Microsoft. AlertsManagement/smartDetectorAlertRules/*

> [!NOTE]
> İzinlerinizin yukarısında olmasının yanı sıra, aboneliğinizin da Microsoft. AlertsManagement kaynak sağlayıcısına kayıtlı olması gerekir. Application Insights hata anomali uyarılarını başarıyla geçirmek için bu gereklidir. 

## <a name="common-problems-and-remedies"></a>Yaygın sorunlar ve düzeltmeler

[Geçişi tetikledikten](alerts-using-migration-tool.md)sonra, geçiş işleminin tamamlandığını veya sizin için herhangi bir eylemin gerekli olduğunu bildirmek için verdiğiniz adreste e-posta alacaksınız. Bu bölümde bazı yaygın sorunlar ve bunlarla ilgilenme işlemleri açıklanmaktadır.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Aboneliğinizdeki klasik uyarı kurallarında yapılan bazı son değişiklikler nedeniyle abonelik geçirilemez. Bu sorun geçicidir. Geçiş durumu, geçiş **için** birkaç gün geri alındıktan sonra geçişi yeniden başlatabilirsiniz.

### <a name="scope-lock-preventing-us-from-migrating-your-rules"></a>Kapsam kilidi kurallarınızı geçirmemizi önler

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grupları oluşturulur ve ardından klasik uyarı kuralları silinir. Ancak, bir kapsam kilidi, kaynak oluşturmamızı veya silmenizi önleyebilir. Kapsam kilidine bağlı olarak, bazı veya tüm kurallar geçirilemez. [Geçiş aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)listelenen abonelik, kaynak grubu veya kaynak için kapsam kilidini kaldırarak ve geçişi yeniden tetikleyerek bu sorunu çözebilirsiniz. Kapsam kilidi devre dışı bırakılamaz ve geçiş işlemi süresince kaldırılması gerekir. [Kapsam kilitlerini yönetme hakkında daha fazla bilgi edinin](../../azure-resource-manager/management/lock-resources.md#portal).

### <a name="policy-with-deny-effect-preventing-us-from-migrating-your-rules"></a>Kurallarınızı geçirmemizi engelleyen ' Reddet ' efektli ilke

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grupları oluşturulur ve ardından klasik uyarı kuralları silinir. Ancak, bir [Azure ilke](../../governance/policy/index.yml) ataması, kaynak oluşturmamızı önleyebilir. İlke atamaya bağlı olarak, bazı veya tüm kurallar geçirilemez. İşlemi engelleyen ilke atamaları [geçiş aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)listelenir. Bu sorunu aşağıdakilerden birini yaparak çözün:

- İlke atamasından geçiş işlemi süresince abonelikler, kaynak grupları veya tek tek kaynaklar hariç tutulur. [İlke dışlama kapsamlarını yönetme hakkında daha fazla bilgi edinin](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion).
- ' Zorlama modu ' nu ilke atamasında **devre dışı** olarak ayarlayın. [İlke atamasının enforcementMode özelliği hakkında daha fazla bilgi edinin](../../governance/policy/concepts/assignment-structure.md#enforcement-mode).
- Abonelikler, kaynak grupları veya tek kaynaklarda ilke atamasına bir Azure Ilke muafiyeti (Önizleme) ayarlayın. [Azure ilke muafiyeti yapısı hakkında daha fazla bilgi edinin](../../governance/policy/concepts/exemption-structure.md).
- ' DISABLED ', ' Audit ', ' append ' veya ' Modify ' gibi etkileri kaldırma veya değiştirme (örneğin, eksik etiketlerle ilgili sorunları çözebilen). [İlke efektlerini yönetme hakkında daha fazla bilgi edinin](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş için hazırlama](alerts-prepare-migration.md)
