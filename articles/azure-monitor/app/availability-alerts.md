---
title: Azure Application Insights kullanılabilirlik uyarılarını ayarlama | Microsoft Docs
description: Application Insights Web testlerini ayarlamayı öğrenin. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: a0e8540d9488e9cc3756eb09c69349ac90d20b83
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043382"
---
# <a name="availability-alerts"></a>Kullanılabilirlik uyarıları

[Azure Application Insights](./app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermiyorsa veya çok yavaş yanıt verirse sizi uyarır.

## <a name="enable-alerts"></a>Uyarıları etkinleştirme

Uyarılar artık varsayılan olarak otomatik olarak etkinleştirilir, ancak uyarıyı tamamen yapılandırmak için ilk olarak kullanılabilirlik testinizi oluşturmanız gerekir.

![Deneyim oluştur](./media/availability-alerts/create-test.png)

> [!NOTE]
>  [Yeni Birleşik uyarılarla](../platform/alerts-overview.md), uyarı kuralının önem derecesi ve [eylem gruplarıyla](../platform/action-groups.md) ilgili bildirim tercihleri, uyarı **deneyiminde yapılandırılmalıdır.** Aşağıdaki adımlar olmadan yalnızca Portal içi bildirimler alacaksınız.

1. Kullanılabilirlik testini kaydettikten sonra, Ayrıntılar sekmesinde, yeni yaptığınız teste göre üç noktaya tıklayın. "Uyarıyı Düzenle" seçeneğine tıklayın.

   ![Ekran görüntüsü menüden düzenleme uyarısını gösterir.](./media/availability-alerts/edit-alert.png)

2. İstenen önem derecesi düzeyi, kural açıklaması ve en önemlisi-bu uyarı kuralı için kullanmak istediğiniz bildirim tercihlerine sahip olan eylem grubunu ayarlayın.

   ![Ekran görüntüsü, kuralı düzenleyebileceğiniz kural yönetimi sayfasını gösterir.](./media/availability-alerts/set-action-group.png)

> [!NOTE]
> Bu deneyim aracılığıyla oluşturulan kullanılabilirlik uyarıları durum tabanlıdır. Bu, site kullanılamaz olarak algılandığında, uyarı ölçütleri karşılandığında tek bir uyarının oluşturulduğu anlamına gelir. Uyarı ölçütlerinin bir sonraki değerlendirilme durumunda site hala kapalıysa, bu yeni bir uyarı oluşturmaz. Bu nedenle, siteniz bir saat boyunca kapalıysa ve bir e-posta uyarısı oluşturduysanız, site geri geldiğinde yalnızca bir e-posta alacaksınız ve site yedeklenmişse sonraki bir e-posta alırsınız. Sitenin hala kullanılamadığını belirten sürekli uyarılar almazsınız.

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Y/Y konumları raporlama hatalarının X üzerinden uyarı

Yeni bir kullanılabilirlik testi oluşturduğunuzda, [Yeni Birleşik uyarılar deneyiminde](../platform/alerts-overview.md)X-Y konumları uyarı kuralı varsayılan olarak etkindir. "Klasik" seçeneğini seçerek veya uyarı kuralını devre dışı bırakmayı tercih edebilirsiniz.

> [!NOTE]
> Yukarıdaki adımları izleyerek uyarı tetiklediği zaman, eylem gruplarını bildirim alacak şekilde yapılandırın. Bu adım olmadan, yalnızca kural tetiklendiğinde Portal içi bildirimler alacaksınız.
>

### <a name="alert-on-availability-metrics"></a>Kullanılabilirlik ölçümleri uyarısı

[Yeni Birleşik uyarıları](../platform/alerts-overview.md)kullanarak, bölümlenmiş toplam kullanılabilirlik ve test süresi ölçümlerinde de uyarı verebilirsiniz:

1. Ölçüm deneyiminden bir Application Insights kaynağı seçin ve bir kullanılabilirlik ölçümü seçin:

    ![Kullanılabilirlik ölçümleri seçimi](./media/availability-alerts/select-metric.png)

2. Menüden uyarıları Yapılandır seçeneği, sizi uyarı kuralını ayarlamak için belirli testleri veya konumları seçebileceğiniz yeni deneyimle götürür. Bu uyarı kuralının eylem gruplarını burada da yapılandırabilirsiniz.

### <a name="alert-on-custom-analytics-queries"></a>Özel analiz sorgularında uyar

[Yeni Birleşik uyarıları](../platform/alerts-overview.md)kullanarak [özel günlük sorguları](../platform/alerts-unified-log.md)üzerinde uyarı alabilirsiniz. Özel sorgularla, kullanılabilirlik sorunlarının en güvenilir sinyalini almanıza yardımcı olan herhangi bir rastgele koşulla ilgili uyarı alabilirsiniz. Bu, TrackAvailability SDK 'sını kullanarak özel kullanılabilirlik sonuçları gönderiyorsanız de geçerlidir.

> [!Tip]
> Kullanılabilirlik verilerinde ölçümler, TrackAvailability SDK 'sını çağırarak gönderdiğiniz tüm özel kullanılabilirlik sonuçlarını içerir. Özel kullanılabilirlik sonuçlarıyla ilgili uyarı almak için ölçüm desteğiyle ilgili uyarı ' yı kullanabilirsiniz.
>

## <a name="automate-alerts"></a>Uyarıları otomatikleştirin

Azure Resource Manager şablonlarıyla bu işlemi otomatikleştirmek için [Kaynak Yöneticisi şablonuyla ölçüm uyarısı oluşturma](../platform/alerts-metric-create-templates.md#template-for-an-availability-test-along-with-a-metric-alert) belgelerine bakın.

## <a name="troubleshooting"></a>Sorun giderme

Adanmış [sorun giderme makalesi](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testleri](availability-multistep.md)
* [URL ping Web testleri](monitor-web-app-availability.md)

