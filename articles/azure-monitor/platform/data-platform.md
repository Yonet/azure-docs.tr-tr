---
title: Azure Izleyici veri platformu | Microsoft Docs
description: Azure Izleyici tarafından toplanan izleme verileri, basit ve gelişmiş analizler için kullanılan neredeyse gerçek zamanlı senaryoları ve günlükleri destekleyebilen, hafif ve desteklenebilecek ölçümlere ayrılmıştır.
documentationcenter: ''
author: bwren
manager: carmonm
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: e87ddd243aa248b896a26e6389ac1a219579a06d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87325585"
---
# <a name="azure-monitor-data-platform"></a>Azure Izleyici veri platformu

Günümüzün bulut ve şirket içi hizmetlere dayalı dağıtılmış uygulamaları çalıştıran karmaşık bilgi işlem ortamlarında Observability etkinleştirme, her katmandan ve dağıtılmış sistemin her bileşenlerinden işlemsel verilerin toplanmasını gerektirir. Bu verilerde derin içgörüler gerçekleştirebiliyor ve bu verileri farklı perspektiflerle tek bir cam bölmeden birleştiren, kuruluşunuzda çok sayıda paydaşya sahip olmanız gerekir.

[Azure izleyici](../overview.md) , çeşitli kaynaklardan veri toplar ve bunları analiz, görselleştirme ve uyarı için kullanılabilecek ortak bir veri platformuna toplar. Birden fazla kaynaktaki verilerin üzerine tutarlı bir deneyim sunar. Bu, tüm izlenen kaynaklarda ve hatta verilerini Azure Izleyici 'de depolayan diğer hizmetlerden gelen verilerle birlikte ayrıntılı öngörüler sağlar.


![Azure İzleyici'ye genel bakış](media/data-platform/overview.png)

## <a name="observability-data-in-azure-monitor"></a>Azure Izleyici 'de verileri Observability
Ölçümler, Günlükler ve dağıtılmış izlemeler, yaygın olarak Observability 'in üç paragraf adı olarak adlandırılır. Bunlar, izlenen bir sistemin yeterli Observability sağlamak için bir izleme aracının toplaması ve analiz edilmesi gereken farklı veri türleridir. Observability birden çok ve daha fazla şekilde verilerle ilişkilendirerek ve izlenen tüm kaynak kümesi genelinde veri toplanarak elde edilebilir. Azure Izleyici birden fazla kaynaktaki verileri birlikte depoladığından, veriler ortak bir araç kümesi kullanılarak bağıntılı ve analiz edilebilir. Ayrıca, diğer hizmetlere yönelik verilerin barındırılmasına ek olarak, verileri birden çok Azure aboneliği ve kiracılar arasında da ilişkilendirir.

Azure kaynakları, önemli miktarda izleme verisi üretir. Azure Izleyici, bu verileri diğer kaynaklardaki izleme verileriyle birlikte ölçüm veya günlük platformu olarak birleştirir. Her biri belirli izleme senaryoları için iyileştirilmiştir ve her biri Azure Izleyici 'de farklı özellikleri destekler. Veri analizi, görselleştirmeler veya uyarı gibi özellikler, gerekli senaryonuzu en verimli ve ekonomik şekilde uygulayabilmeniz için farkları anlamanız gerekir. [Application Insights](../app/app-insights-overview.md) veya [VM'ler için Azure izleyici](../insights/vminsights-overview.md) gibi Azure izleyici içgörüleri, iki veri türü arasındaki farklılıkları anlamak zorunda kalmadan belirli izleme senaryosuna odaklanabilmenize olanak tanıyan analiz araçlarına sahiptir. 


### <a name="metrics"></a>Ölçümler
[Ölçümler](data-platform-metrics.md) , belirli bir noktadaki sistemin bazı yönlerini tanımlayan sayısal değerlerdir. Düzenli aralıklarla toplanır ve bir zaman damgası, ad, değer ve bir veya daha fazla tanımlayıcı etiketle tanımlanır. Diğer ölçümlere kıyasla çeşitli algoritmalar kullanılarak ölçümler toplanabilir ve zaman içindeki eğilimler açısından analiz edilebilir. 

Azure Izleyici ölçümleri, zaman damgalı verileri çözümlemek için en iyi duruma getirilmiş bir zaman serisi veritabanında depolanır. Bu, ölçümleri uyarı ve hızlı bir şekilde algılama için özellikle uygun hale getirir. Bunlar sisteminizin nasıl çalıştığını söyleyebilir, ancak genellikle sorunların temel nedenini belirlemek için günlüklerle birleştirilmesi gerekir.

Ölçümler [Azure Ölçüm Gezgini](./metrics-getting-started.md)ile Azure Portal etkileşimli analiz için kullanılabilir. Bunlar, başka verilerle birlikte görselleştirme için bir [Azure panosuna](../learn/tutorial-app-dashboards.md) eklenebilir ve neredeyse gerçek zamanlı [Uyarı](alerts-metric.md)için kullanılabilir.

Azure izleyici [ölçümlerinde](data-platform-metrics.md)bulunan veri kaynakları da dahil olmak üzere Azure izleyici ölçümleri hakkında daha fazla bilgi edinin.

### <a name="logs"></a>Günlükler
[Günlükler](data-platform-logs.md) , sistem içinde gerçekleşen olaylardır. Farklı türlerde veriler içerebilir ve zaman damgasıyla yapılandırılmış veya serbest biçimli metinler olabilir. Ortamdaki olaylar, günlük girişleri oluşturdukça gelişigüzel oluşturulabilir ve ağır yük altındaki bir sistem genellikle daha fazla günlük hacmi oluşturur.

Azure Izleyici 'deki Günlükler, güçlü bir analiz altyapısı ve [zengin sorgu dili](/azure/kusto/query/)sağlayan [Azure Veri Gezgini](/azure/data-explorer/) temel alan Log Analytics çalışma alanında depolanır. Günlükler genellikle tanımlanmakta olan sorunun tamamen bağlamını sağlamak için yeterli bilgi sağlar ve bu sorunların kök durumunu tanımlamak için değerlidir.

> [!NOTE]
> Azure Izleyici günlüklerini ve Azure 'da günlük verilerinin kaynaklarını ayırt etmek önemlidir. Örneğin, Azure 'daki abonelik düzeyi olayları, Azure Izleyici menüsünden görüntüleyebileceğiniz bir [etkinlik günlüğüne](platform-logs-overview.md) yazılır. Çoğu kaynak, işletimsel bilgileri farklı konumlara iletebilmeniz için bir [kaynak günlüğüne](platform-logs-overview.md) yazar. Azure Izleyici günlükleri, tüm kaynak kümesinde derin analiz sağlamak üzere diğer izleme verileriyle birlikte etkinlik günlüklerini ve kaynak günlüklerini toplayan bir günlük veri platformudur.


 Azure portal [Log Analytics](../log-query/log-query-overview.md) ile [günlük sorgularıyla](../log-query/log-query-overview.md) etkileşimli olarak çalışabilir veya sonuçları diğer verilerle birlikte görselleştirme için bir [Azure panosuna](../learn/tutorial-app-dashboards.md) ekleyebilirsiniz. Ayrıca, bir zamanlama sorgusunun sonuçlarına dayalı olarak bir uyarı tetikleyecek [günlük uyarıları](alerts-log.md) da oluşturabilirsiniz.

Azure izleyici ['Deki günlüklerde](data-platform-logs.md)bulunan veri kaynaklarını Içeren Azure izleyici günlükleri hakkında daha fazla bilgi edinin.

### <a name="distributed-traces"></a>Dağıtılmış izlemeler
İzlemeler, dağıtılmış bir sistem aracılığıyla bir Kullanıcı isteğini izleyen ilgili olay serisidir. Uygulama kodu davranışlarını ve farklı işlemlerin performansını belirlemede kullanılabilir. Günlükler genellikle dağıtılmış bir sistemin ayrı bileşenleri tarafından oluşturulsa da, bir izleme uygulamanızın tüm bileşen kümesi genelinde işlem ve performansını ölçer.

Azure Izleyici 'de dağıtılmış izleme [APPLICATION INSIGHTS SDK](../app/distributed-tracing.md)ile etkinleştirilir ve izleme verileri, Application Insights tarafından toplanan diğer uygulama günlüğü verileriyle birlikte depolanır. Bu, günlük sorguları, panolar ve uyarılar dahil olmak üzere diğer günlük verileriyle aynı analiz araçlarının kullanılabilmesini sağlar.

Dağıtılmış izleme hakkında daha fazla bilgi için [bkz. dağıtılmış izleme nedir?](../app/distributed-tracing.md).


## <a name="compare-azure-monitor-metrics-and-logs"></a>Azure Izleyici ölçümlerini ve günlüklerini karşılaştırın

Aşağıdaki tabloda Azure Izleyici 'de ölçümler ve Günlükler karşılaştırılmaktadır.

| Öznitelik  | Ölçümler | Günlükler |
|:---|:---|:---|
| Yararları | Uyarı gibi neredeyse gerçek zamanlı senaryolara sahip hafif ve yetenekli senaryolar. Sorunların hızlı algılanması için idealdir. | Zengin sorgu diliyle çözümlendi. Derin analiz ve temel nedeni tanımlama için idealdir. |
| Veriler | Yalnızca sayısal değerler | Metin veya sayısal veriler |
| Yapı | Örnek saat, izlenen kaynak ve sayısal bir değer dahil olmak üzere standart özellikler kümesi. Bazı ölçümler, daha fazla tanım için birden çok boyut içerir. | Günlük türüne göre benzersiz özellik kümesi. |
| Koleksiyon | Düzenli aralıklarla toplanır. | , Olayların oluşturulması için bir kayıt tetiklemesi olarak toplanabilir. |
| Azure portalında görüntüleme | Ölçüm Gezgini | Log Analytics |
| Veri kaynakları şunlardır | Azure kaynaklarından toplanan platform ölçümleri.<br>Application Insights tarafından izlenen uygulamalar.<br>Uygulama veya API tarafından tanımlanan özel. | Uygulama ve kaynak günlükleri.<br>Çözümleri izleme.<br>Aracılar ve VM uzantıları.<br>Uygulama istekleri ve özel durumlar.<br>Azure Güvenlik Merkezi.<br>Veri Toplayıcı API 'SI. |

## <a name="collect-monitoring-data"></a>İzleme verilerini topla
[Azure izleyici için farklı veri kaynakları](data-sources.md) , Log Analytics çalışma alanına (Günlükler) ya da Azure izleyici ölçümleri veritabanına (ölçümler) veya her ikisine de yazılır. Bazı kaynaklar bu veri depolarına doğrudan yazılır, diğerleri ise Azure depolama gibi başka bir konuma yazabilir ve günlükleri veya ölçümleri doldurmak için bazı yapılandırmalar gerektirir. 

Her türü dolduran farklı veri kaynaklarının listesi için bkz. Azure izleyici 'de Azure Izleyici ve [günlüklerde](data-platform-logs.md) [bulunan ölçümler](data-platform-metrics.md) .


## <a name="stream-data-to-external-systems"></a>Dış sistemlere veri akışı
İzleme verilerini çözümlemek için Azure 'daki araçların kullanılmasına ek olarak, onu güvenlik bilgileri ve olay yönetimi (SıEM) ürünü gibi bir dış araca iletme gereksinimine sahip olabilirsiniz. Bu iletme, genellikle [Azure Event Hubs](../../event-hubs/index.yml)aracılığıyla izlenen kaynaklardan doğrudan yapılır. Bazı kaynaklar, gerekli verileri almak için mantıksal uygulama gibi başka bir işlem de kullanabilmeniz sırasında, verileri doğrudan bir olay hub 'ına gönderecek şekilde yapılandırılabilir. Ayrıntılar için bkz. [bir dış araçla tüketim Için Azure izleme verilerini bir olay hub 'ına akış](stream-monitoring-data-event-hubs.md) .



## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici 'de ölçümler](data-platform-metrics.md)hakkında daha fazla bilgi edinin.
- [Azure izleyici 'de Günlükler](data-platform-logs.md)hakkında daha fazla bilgi edinin.
- Azure 'daki farklı kaynaklar için [kullanılabilen izleme verileri](data-sources.md) hakkında bilgi edinin.

