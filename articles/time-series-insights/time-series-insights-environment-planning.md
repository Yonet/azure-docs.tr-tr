---
title: Gen1 ortamınızı planlayın-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights Gen1 ortamınızı hazırlamak, yapılandırmak ve dağıtmak için en iyi uygulamalar.
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 09/29/2020
ms.custom: seodec18
ms.openlocfilehash: 5e0f1ea42aa2ba888b89dd652d3397a3a2163a3e
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95016216"
---
# <a name="plan-your-azure-time-series-insights-gen1-environment"></a>Azure Time Series Insights Gen1 ortamınızı planlayın

> [!CAUTION]
> Bu bir Gen1 makaledir.

Bu makalede, Azure Time Series Insights Gen1 ortamınızı beklenen giriş oranına ve veri saklama gereksinimlerinize göre nasıl planlayabileceğiniz açıklanır.

## <a name="video"></a>Video

**Azure Time Series Insights veri saklama ve bunun nasıl planlanacağı hakkında daha fazla bilgi edinmek için bu videoyu izleyin**:<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>En iyi uygulamalar

Azure Time Series Insights kullanmaya başlamak için, ne kadar veri göndermeyi beklediğinizi ve verilerinizi ne kadar süreyle depolamanız gerektiğini biliyorsanız bu en iyisidir.  

Azure Time Series Insights SKU 'Larının kapasitesi ve bekletme hakkında daha fazla bilgi için [Azure Time Series Insights fiyatlandırmayı](https://azure.microsoft.com/pricing/details/time-series-insights/)okuyun.

Azure Time Series Insights ortamınızı uzun süreli başarıyı en iyi şekilde planlamak için aşağıdaki öznitelikleri göz önünde bulundurun:

- [Depolama kapasitesi](#storage-capacity)
- [Veri saklama süresi](#data-retention)
- [Giriş kapasitesi](#ingress-capacity)
- [Olaylarınızı şekillendirme](#shape-your-events)
- [Başvuru verilerinin yerinde olduğundan emin olma](#ensure-that-you-have-reference-data)

## <a name="storage-capacity"></a>Depolama kapasitesi

Varsayılan olarak, Azure Time Series Insights sağladığınız depolama miktarına (birim başına depolama miktarı &#215;) ve giriş verilerine göre verileri korur.

## <a name="data-retention"></a>Veri saklama

Azure Time Series Insights ortamınızda **veri saklama süresi** ayarını değiştirebilirsiniz. 400 güne kadar bekletme sağlayabilirsiniz.

Azure Time Series Insights iki mod vardır:

- En güncel veriler için bir mod en iyi duruma getirir. Örnek ile kullanılabilir son verileri bırakarak **eski verileri temizlemeye** yönelik bir ilke uygular. Bu mod, varsayılan olarak açık olur.
- Diğer, yapılandırılan bekletme sınırlarının altında kalacak şekilde verileri iyileştirir. Giriş **duraklatma** , **depolama sınırı aşıldı davranışı** olarak seçildiğinde yeni verilerin görüntülenmesini önler.

Tutma durumunu ayarlayabilir ve Azure portal ortamın yapılandırma sayfasındaki iki mod arasında geçiş yapabilirsiniz.

> [!IMPORTANT]
> Azure Time Series Insights Gen1 ortamınızda en fazla 400 günlük veri saklama alanı yapılandırabilirsiniz.

### <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. [Azure Portal](https://portal.azure.com), Time Series Insights ortamınızı seçin.

1. **Time Series Insights ortamı** bölmesinde, **Ayarlar** altında **depolama yapılandırması**' nı seçin.

1. **Veri saklama süresi (gün cinsinden)** kutusuna 1 ile 400 arasında bir değer girin.

   [![Bekletmeyi yapılandırma](media/data-retention/configure-data-retention.png)](media/data-retention/configure-data-retention.png#lightbox)

> [!TIP]
> Uygun bir veri saklama ilkesinin nasıl uygulanacağı hakkında daha fazla bilgi edinmek için, [bekletmeyi yapılandırma](./time-series-insights-how-to-configure-retention.md)makalesini okuyun.

## <a name="ingress-capacity"></a>Giriş kapasitesi

[!INCLUDE [Azure Time Series Insights Gen1 limits](../../includes/time-series-insights-ga-limits.md)]

### <a name="environment-planning"></a>Ortam planlama

Azure Time Series Insights ortamınızın planlanmasına odaklanmanız için ikinci alan, giriş kapasitesidir. Günlük giriş depolama ve olay kapasitesi, 1 KB 'lik bloklar halinde dakika başına ölçülür. İzin verilen en büyük paket boyutu 32 KB 'tır. 32 KB 'den büyük veri paketleri kesilir.

S1 veya S2 SKU 'sunun kapasitesini tek bir ortamda 10 birim olarak artırabilirsiniz. S1 ortamından S2 'e geçiş yapamazsınız. S2 ortamından S1 'e geçiş yapamazsınız.

Giriş kapasitesi için, önce gereksinim duyduğunuz toplam girişi ayda bir esasına göre saptayın. Sonra, dakika başına ihtiyaçlarınızı belirlemek için.

Kısıtlama ve gecikme süresi, dakika başına kapasite içinde bir rol oynar. Veri giriş ortamınızda 24 saatten daha az bir zaman kazandıysanız, yukarıdaki tabloda listelenen hızların giriş oranından "yakalayabilmeniz" Azure Time Series Insights.

Örneğin, tek bir S1 SKU 'SU varsa, verileri dakikada 720 olay hızında ve veri hızının 1.440 olay veya daha kısa bir süre içinde bir saatten kısa bir süre boyunca artışlar durumunda ortamınızda belirgin bir gecikme yoktur. Ancak, bir saatten uzun bir süre boyunca 1.440 olayı aşarsanız, görselde görselleştirilen ve sorgu için kullanılabilen verilerdeki gecikme süresi büyük olasılıkla yaşanacaktır.

Ne kadar veri göndermeyi beklediğinizi önceden bilmiyor olabilirsiniz. Bu durumda, [azure IoT Hub](../iot-hub/monitor-iot-hub.md) ve [Azure Event Hubs](/archive/blogs/cloud_solution_architect/using-the-azure-rest-apis-to-retrieve-event-hub-metrics) için veri telemetrisini Azure Portal aboneliğinizde bulabilirsiniz. Telemetri, ortamınızı nasıl sağlayacağınızı belirlemenize yardımcı olabilir. Telemetrisini görüntülemek için ilgili olay kaynağı için Azure portal **ölçümler** bölmesini kullanın. Olay kaynak ölçümlerinizi anladıysanız, Azure Time Series Insights ortamınızı daha verimli bir şekilde planlamak ve temin edebilirsiniz.

### <a name="calculate-ingress-requirements"></a>Giriş gereksinimlerini hesapla

Giriş gereksinimlerinizi hesaplamak için:

- Giriş kapasitenizin, ortalama dakikalık oranınızdan yüksek olduğunu ve ortamınızın, bir saatten kısa bir süre boyunca en fazla iki kata kadar olan tahmini giriş değerinizi işleyecek kadar büyük olduğunu doğrulayın.

- En son 1 saatten uzun süre boyunca giriş ani artışlar gerçekleşirse, ortalama olarak ani artış oranını kullanın. Depo oranını işlemek için kapasiteye sahip bir ortam sağlayın.

### <a name="mitigate-throttling-and-latency"></a>Azaltma ve gecikme süresini azaltma

Azaltma ve gecikme süresinin nasıl önleneceği hakkında daha fazla bilgi için, azaltma [gecikmesini ve azaltmayı](time-series-insights-environment-mitigate-latency.md)okuyun.

## <a name="shape-your-events"></a>Olaylarınızı şekillendirin

Azure Time Series Insights olayları yollamanın, sağladığınız ortamın boyutunu desteklediğinden emin olmak önemlidir. (Buna karşılık, ortamın boyutunu, kaç olay Azure Time Series Insights okuduğunu ve her olayın boyutunu) eşleyebilirsiniz.) Verilerinizi sorguladığınızda dilimlemek ve filtrelemek için kullanmak isteyebileceğiniz öznitelikleri düşünmek de önemlidir.

> [!TIP]
> [Olayları GÖNDERMEDE](time-series-insights-send-events.md)JSON şekillendirme belgelerini gözden geçirin.

## <a name="ensure-that-you-have-reference-data"></a>Başvuru verilerine sahip olduğunuzdan emin olun

*Başvuru veri kümesi* , olay kaynağınızdan olayları geliştiren öğelerin bir koleksiyonudur. Azure Time Series Insights giriş altyapısı, olay kaynağınızdaki her bir olayı, başvuru veri kümenizdeki karşılık gelen veri satırıyla birleştirir. Genişletilmiş olay daha sonra sorgu için kullanılabilir. JOIN, başvuru veri kümenizde tanımlanan **birincil anahtar** sütunlarını temel alır.

> [!NOTE]
> Başvuru verileri geriye dönük olarak katılmadı. Yapılandırma ve karşıya yükleme sonrasında yalnızca geçerli ve gelecekteki giriş verileri eşleştirilir ve başvuru veri kümesine birleştirilir. Azure Time Series Insights için büyük miktarda geçmiş verisi gönderilmesini ve Azure Time Series Insights önce karşıya yüklemeden veya başvuru verilerini oluşturmazsanız, çalışmanızı yinelemek zorunda kalabilirsiniz (İpucu: eğlenceli değil).  

Azure Time Series Insights ' de başvuru verilerinizi oluşturma, karşıya yükleme ve yönetme hakkında daha fazla bilgi edinmek için, [başvuru veri kümesi belgelerimizi](time-series-insights-add-reference-data-set.md)okuyun.

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal yeni bir Azure Time Series Insights ortamı](time-series-insights-get-started.md)oluşturarak başlayın.

- Azure Time Series Insights [bir Event Hubs olay kaynağı eklemeyi](./how-to-ingest-data-event-hub.md) öğrenin.

- [IoT Hub olay kaynağını yapılandırma](./how-to-ingest-data-iot-hub.md)hakkında bilgi edinin.