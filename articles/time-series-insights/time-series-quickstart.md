---
title: 'Hızlı başlangıç: Azure Time Series Insights Explorer-Azure Time Series Insights | Microsoft Docs'
description: Azure Time Series Insights Gezginini kullanmaya nasıl başlaleyeceğinizi öğrenin. Büyük hacimli IoT verilerini görselleştirin ve ortamınızın önemli özelliklerini görselleştirin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 09/30/2020
ms.openlocfilehash: dd5ed52187a7cd23142b59e20c91c6d125946a72
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91613790"
---
# <a name="quickstart-explore-azure-time-series-insights-gen1"></a>Hızlı başlangıç: araştırma Azure Time Series Insights Gen1

> [!CAUTION]
> Bu bir Gen1 makaledir.

Bu Azure Time Series Insights gezgin hızlı başlangıcı, ücretsiz bir tanıtım ortamında Azure Time Series Insights kullanmaya başlamanıza yardımcı olur. Bu hızlı başlangıçta, Web tarayıcınızı kullanarak büyük hacimlerinizde çok sayıda IoT verisi ve genel kullanıma sunulan turu temel özellikleri görselleştirme hakkında bilgi edineceksiniz.

Azure Time Series Insights, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. IoT çözümünüzü hızla doğrulayabilmeniz ve görev açısından kritik cihazlara maliyetli kapalı kalma süresi oluşmasını önlemek için verilerinizin genel bir görünümünü sunar. Azure Time Series Insights, gizli eğilimleri keşfetmenize, anormallikleri belirlemenize ve neredeyse gerçek zamanlı olarak kök neden analizleri yürütmenize yardımcı olur.

Daha fazla esneklik için, güçlü [REST API 'leri](./concepts-query-overview.md) ve [istemci SDK 'sı](https://github.com/microsoft/tsiclient)aracılığıyla önceden var olan bir uygulamaya Azure Time Series Insights ekleyebilirsiniz. API 'Leri, zaman serisi verilerini depolamak, sorgulamak ve tercih ettiğiniz bir istemci uygulamasında kullanmak için kullanabilirsiniz. Ayrıca, mevcut uygulamanıza Kullanıcı arabirimi bileşenleri eklemek için istemci SDK 'sını de kullanabilirsiniz.

Bu Azure Time Series Insights gezgin hızlı başlangıcı, özelliklerin kılavuzlu bir gezintisini sunmaktadır.

> [!IMPORTANT]
> Henüz bir tane oluşturmadıysanız, [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="prepare-the-demo-environment"></a>Tanıtım ortamını hazırlama

1. Tarayıcınızda [Gen1 demo](https://insights.timeseries.azure.com/demo)sayfasına gidin.

1. İstenirse, Azure hesabı kimlik bilgilerinizi kullanarak Azure Time Series Insights Gezgininde oturum açın.

1. Azure Time Series Insights hızlı tur sayfası görüntülenir. Hızlı tura başlamak için **İleri ' yi** seçin.

   [![Hızlı başlangıç hoş geldiniz-Ileri Seç](media/quickstart/quickstart-welcome.png)](media/quickstart/quickstart-welcome.png#lightbox)

## <a name="explore-the-demo-environment"></a>Tanıtım ortamını keşfet

1. **Zaman seçim paneli** görüntülenir. Görselleştirilecek zaman aralığını seçmek için bu paneli kullanın.

   [![Zaman seçim paneli](media/quickstart/quickstart-time-selection-panel.png)](media/quickstart/quickstart-time-selection-panel.png#lightbox)

1. Bir zaman dilimi seçin ve bölgeye sürükleyin. Ardından **Ara**' yı seçin.

   [![Zaman aralığı seçin](media/quickstart/quickstart-select-time.png)](media/quickstart/quickstart-select-time.png#lightbox)

   Azure Time Series Insights belirttiğiniz zaman dilimi için bir grafik görselleştirmesi görüntüler. Çizgi grafik içinde çeşitli eylemler gerçekleştirebilirsiniz. Örneğin, filtreleyebilir, sabitleyebilir, sıralayabilir ve yığın oluşturabilirsiniz.

   **Zaman seçimi paneline**dönmek için gösterilen aşağı oku seçin:

   [![Grafik](media/quickstart/quickstart-select-down-arrow.png)](media/quickstart/quickstart-select-down-arrow.png#lightbox)

1. Yeni bir arama terimi eklemek için **terimler panelinde** **Ekle** ' yi seçin.

   [![Arama terimleri bölmesi Ekle](media/quickstart/quickstart-add-terms.png)](media/quickstart/quickstart-add-terms.png#lightbox)

1. Grafikte bir bölgeyi seçip bölgeye sağ tıklayabilir ve **Olayları Keşfet**’i seçebilirsiniz.

   [![Olayları Keşfet](media/quickstart/quickstart-explore-events.png)](media/quickstart/quickstart-explore-events.png#lightbox)

   Ham verilerinizin bir Kılavuzu, araştırırken bulunan bölgeden görüntülenir.

   [![Olayları keşfet-kılavuz veri görünümü](media/quickstart/quickstart-explore-events-grid-data.png)](media/quickstart/quickstart-explore-events-grid-data.png#lightbox)

## <a name="select-and-filter-data"></a>Verileri seçme ve filtreleme

1. Grafikteki değerleri değiştirmek için koşullarınızı düzenleyin. Farklı değer türlerini çapraz ilişkilendirmek için başka bir terim ekleyin.

   [![Terim ekleme](media/quickstart/quickstart-add-a-term.png)](media/quickstart/quickstart-add-a-term.png#lightbox)

1. Tüm seçili arama koşullarını göstermek veya improvised serisi filtreleme için **filtre serisi** kutusuna bir filtre terimi girmek Için, **filtre serisi** kutusunu boş bırakın.

   [![Seriyi filtrele](media/quickstart/quickstart-filter-series.png)](media/quickstart/quickstart-filter-series.png#lightbox)

   Bu hızlı başlangıçta, bir istasyona ait sıcaklığı ve basıncı karşılıklı olarak ilişkilendirmek için **Station5** terimini girin.

Hızlı başlangıcı bitirdikten sonra örnek veri kümesiyle deneme yaparak farklı görselleştirmeler oluşturabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladığınıza göre, oluşturduğunuz kaynakları temizleyin:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, Azure Time Series Insights kaynak grubunuzu bulun.
1. Her kaynağı ayrı ayrı **Sil** veya Kaldır ' a tıklayarak kaynak grubunun tamamını (ve içinde yer alan tüm kaynakları) silin.

## <a name="next-steps"></a>Sonraki adımlar

* Kendi Azure Time Series Insights ortamınızı oluşturmaya hazırsınız. [Azure Time Series Insights ortamınızın planını](time-series-insights-environment-planning.md)okuyun.
