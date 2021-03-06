---
title: Önceden satın alma ile Azure Databricks maliyetlerini iyileştirme
description: Para tasarrufu sağlamak için ayrılmış kapasite ile Azure Databricks ücretleri için nasıl ön ödeme yapabileceğinizi öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: 390a8b421a7b34391bde689e4b968fa98cdbaf76
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98599167"
---
# <a name="optimize-azure-databricks-costs-with-a-pre-purchase"></a>Önceden satın alma ile Azure Databricks maliyetlerini iyileştirme

Bir veya üç yıl için Azure Databricks işleme birimlerini (DBCU) önceden satın aldığınızda Azure Databricks birimi (DBU) maliyetlerinizden tasarruf sağlayabilirsiniz. Önceden satın alınan DBCU’ları, satın alma döneminde isteğiniz zaman kullanabilirsiniz. Sanal makinelerden farklı olarak, önceden satın alınan birimlerin saatlik olarak süresi sona ermez ve satın alma dönemi boyunca istediğiniz zaman bunları kullanırsınız.

Tüm Azure Databricks öğeleri, önceden satın alınan DBU’lardaki kesintileri otomatik olarak kullanır. Önceden satın alma indirimleri almak için DBU kullanımına ilişkin Azure Databricks çalışma alanlarınıza önceden satın alınan bir planı yeniden dağıtmanız veya atamanız gerekmez.

Önceden satın alma indirimi yalnızca DBU kullanımı için geçerlidir. İşlem, depolama ve ağ iletişimi gibi diğer ücretler ayrı olarak ücretlendirilir.

## <a name="determine-the-right-size-to-buy"></a>Satın alınacak doğru boyutu belirleme

Databricks önceden satın alımı, tüm Databricks iş yükleri ve katmanları için geçerlidir. Önceden satın alımı, ön ödemeli Databricks işleme birimleri havuzu gibi düşünebilirsiniz. Kullanım, iş yüküne veya katmana bakılmaksızın havuzdan düşülür. Kullanım şu oranda düşülür:

| **İş yükü** | **DBU uygulama oranı - Standart katman** | **DBU uygulama oranı - Premium katman** |
| --- | --- | --- |
| Veri Analizi | 0,4 | 0,55 |
| Veri Mühendisliği | 0,15 | 0,30 |
| Veri Mühendisliği Hafif | 0,07 | 0,22 |

Örneğin, bir Veri Analizi - Standart Katman miktarı kullanıldığında, önceden satın alınan Databricks işleme birimleri, 0,4 birim düşülür.

Satın almadan önce, farklı iş yükleri ve katmanlar için kullanılan toplam DBU miktarını hesaplayın. DBCU’yu normalleştirmek için önceki oranları kullanın ve sonraki bir veya üç yıla ait toplam kullanıma ilişkin bir projeksiyon çalıştırın.

## <a name="purchase-databricks-commit-units"></a>Databricks işleme birimleri satın alma

[Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D) Databricks planları satın alabilirsiniz. Ayrılmış kapasite satın almak için, en az bir kurumsal abonelik için sahip rolüne sahip olmanız gerekir.

- Şunlar için Sahip rolünde olmanız gerekir: en az bir Kurumsal Anlaşma (teklif numaraları: MS-AZR-0017P or MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi ya da kullandıkça öde fiyatlarıyla sunulan bireysel abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P).
- EA abonelikleri için, EA Portal’da Ayrılmış Örnek Ekle seçeneğinin etkinleştirilmesi gerekir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.
- Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Veya bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.

**Satın almak için:**

1. [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D) gidin.
1. Bir abonelik seçin. Ayrılmış kapasitenin ödemesini yapmak için kullanılan aboneliği seçmek amacıyla **Abonelik** listesini kullanın. Ayrılmış kapasiteye ilişkin peşin maliyetler, aboneliğin ödeme yöntemiyle ücretlendirilir. Ücretler, kaydın Azure Ön Ödemesi (eski adıyla parasal taahhüt) bakiyesinden düşülür veya fazla kullanım olarak ücretlendirilir.
1. Bir kapsam seçin. **Kapsam** listesini kullanarak bir abonelik kapsamı seçin:
    - **Tek kaynak grubu kapsamı**: Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular.
    - **Tek abonelik kapsamı**: Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.
    - **Paylaşılan kapsam**: Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. Kurumsal Anlaşma müşterileri için faturalama bağlamı kayıttır.
1. Kaç tane Azure Databricks işleme birimi satın almak istediğinizi seçin ve satın alma işlemini tamamlayın.


![Azure portalında Azure Databricks satın alma işlemini gösteren örnek](./media/prepay-databricks-reserved-capacity/data-bricks-pre-purchase.png)

## <a name="change-scope-and-ownership"></a>Kapsamı ve sahipliği değiştirme

Satın alma işleminden sonra bir rezervasyon üzerinde aşağıdaki değişiklikleri yapabilirsiniz:

- Rezervasyon kapsamını güncelleştirme
- Azure rol tabanlı erişim denetimi (Azure RBAC)

Databricks işleme birimi önceden satın alımını bölemez veya birleştiremezsiniz. Rezervasyonları yönetme hakkında daha fazla bilgi için bkz. [Satın aldıktan sonra rezervasyonları yönetme](manage-reserved-vm-instance.md).

## <a name="cancellations-and-exchanges"></a>İptaller ve değişimler

Databricks önceden satın alma planları için iptal ve değişim desteklenmez. Tüm satın alma işlemleri nihaidir.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
  - [Azure Databricks önceden satın alma DBCU indiriminin nasıl uygulandığını anlama](reservation-discount-databricks.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
