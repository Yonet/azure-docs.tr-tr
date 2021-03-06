---
title: Azure Information Protection Azure Sentinel 'e bağlama
description: Azure Information Protection veri bağlayıcısını yapılandırarak, Azure Information Protection 'den Azure Sentinel 'e günlük kaydı bilgilerini akışa alma.
services: sentinel
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: yelevin
ms.openlocfilehash: 82ea47037902ce3a9449f71a9edf62cb80863d4b
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94655995"
---
# <a name="connect-data-from-azure-information-protection"></a>Azure Information Protection verileri bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Azure Information Protection veri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Information Protection veri bağlayıcısını yapılandırarak [Azure Information Protection](https://azure.microsoft.com/services/information-protection/) günlük bilgilerini Azure Sentinel 'e akışla aktarabilirsiniz. Azure Information Protection, önemli verilerinizi bulutta veya şirket içinde depolanmış olsun denetlemenize ve güvenli hale getirmeye yardımcı olur.

[Azure Information Protection için merkezi raporlama](/azure/information-protection/reports-aip) zaten yapılandırılmışsa, bu hizmetten günlüğe kaydetme bilgileri Azure Sentinel için seçtiğiniz aynı Log Analytics çalışma alanında depolanıyorsa, bu veri bağlayıcısının yapılandırmasını atlayabilirsiniz. Azure Information Protection günlük bilgileri Azure Sentinel 'de zaten kullanılabilir.

Ancak, Azure Information Protection günlük bilgileri Azure Sentinel için seçtiğiniz farklı bir Log Analytics çalışma alanına gitecekse, aşağıdakilerden birini yapın:

- Azure Sentinel 'de seçili çalışma alanını değiştirin.

- Bu veri bağlayıcısını yapılandırarak yapabileceğiniz Azure Information Protection çalışma alanını değiştirin.
    
    Çalışma alanını değiştirirseniz, Azure Information Protection için yeni raporlama verileri artık Azure Sentinel için kullandığınız çalışma alanında depolanır ve geçmiş veriler Azure Sentinel 'de kullanılamaz. Ayrıca, önceki çalışma alanı özel sorgular, uyarılar veya REST API 'Ler için yapılandırılmışsa, Azure Information Protection için kullanmaya devam etmek istiyorsanız bunların Azure Sentinel çalışma alanı için yeniden yapılandırılması gerekir. Azure Information Protection kullanan istemciler ve hizmetler için yeniden yapılandırma gerekmez.

## <a name="prerequisites"></a>Önkoşullar

- Kiracınız için aşağıdaki Azure AD yönetici rollerinden biri: 
    - Azure Information Protection Yöneticisi
    - Güvenlik yöneticisi
    - Uyumluluk Yöneticisi
    - Uyumluluk verileri Yöneticisi
    - Genel yönetici
    
    > [!NOTE]
    > Kiracınız [Birleşik etiketleme platformundan](/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform)ise Azure Information Protection yönetici rolünü kullanamazsınız.
    
    Bu yönetici rolleri yalnızca Azure Information Protection bağlayıcısını yapılandırmak için gereklidir ve Azure Sentinel Azure Information Protection bağlı olduğunda gerekli değildir.

- Azure Sentinel ve Azure Information Protection için kullanmakta olduğunuz Log Analytics çalışma alanına okuma ve yazma izinleri.

- Azure Information Protection Azure portal eklendi. Bu adımla ilgili yardıma ihtiyacınız varsa, [Azure portal Azure Information Protection ekleme](/azure/information-protection/quickstart-viewpolicy#add-azure-information-protection-to-the-azure-portal)bölümüne bakın.

## <a name="connect-to-azure-information-protection"></a>Azure Information Protection Bağlan

Azure Information Protection için bir Log Analytics çalışma alanı yapılandırmadıysanız veya Azure Information Protection günlüğe kaydetme bilgilerini depolayan çalışma alanını değiştirmeniz gerekiyorsa aşağıdaki yönergeleri kullanın.

1. Azure Sentinel 'de **veri bağlayıcıları**  >  **Azure Information Protection (Önizleme)** öğesini seçin.

2. **Bağlayıcı sayfasını aç**' ı seçin.

3. **Yapılandırma** altında **Azure Information Protection günlüklerini bağla**' yı seçin.

4. **Analiz yapılandırma (Önizleme)** dikey penceresinde, şu anda Azure Sentinel için kullandığınız çalışma alanını seçin. Farklı bir çalışma alanı seçerseniz Azure Information Protection raporlama verileri Azure Sentinel tarafından kullanılamaz.

5. Bir çalışma alanı seçtikten sonra **Tamam**' ı seçin. Bağlayıcı **durumu** **bağlı** olarak değişir.

6. Azure Information Protection rapor verileri, seçilen çalışma alanındaki **InformationProtectionLogs_CL** tablosunda depolanır. 
    
    Bu raporlama verileri için Azure Izleyici 'de ilgili şemayı kullanmak için, **ınformationprotectionevents**' i arayın. Bu olay işlevleri hakkında daha fazla bilgi için, Azure Information Protection belgelerindeki [olay işlevleri Için kolay şema başvurusu](/azure/information-protection/reports-aip#friendly-schema-reference-for-event-functions) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Azure Information Protection Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.