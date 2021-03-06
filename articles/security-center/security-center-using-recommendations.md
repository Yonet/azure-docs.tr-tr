---
title: Güvenliği geliştirmek için Azure Güvenlik Merkezi önerilerini kullanma | Microsoft Docs
description: " Güvenlik saldırısını azaltmaya yardımcı olması için Azure Güvenlik Merkezi 'nde güvenlik ilkeleri ve önerilerini nasıl kullanacağınızı öğrenin. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 4c6b8b0bfa78dca5ca32c8c72edcc463ebb9057a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91322147"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Güvenliği artırmaya yönelik Azure Güvenlik Merkezi önerileri

Bir güvenlik ilkesi yapılandırarak ve ardından Azure Güvenlik Merkezi tarafından sunulan önerileri uygulayarak önemli bir güvenlik olayı olasılığını azaltabilirsiniz. Bu makalede bir güvenlik saldırısını azaltmaya yardımcı olmak için Güvenlik Merkezi 'nde güvenlik ilkelerinin ve önerilerin nasıl kullanılacağı gösterilmektedir. 

Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu çözümlemek için sürekli taramaları otomatik olarak çalıştırır. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli güvenlik denetimlerini yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur. Güvenlik Merkezi, önerilerini 24 saat içinde, aşağıdaki özel durumlarla güncelleştirir:

- İşletim sistemi güvenlik yapılandırması önerileri 48 saat içinde güncelleştirilir
- Endpoint Protection sorunları önerileri 8 saat içinde güncelleştirilir

## <a name="scenario"></a>Senaryo
Bu senaryo, güvenlik merkezi önerilerini izleyerek ve işlem yaparak bir güvenlik olayının olasılığını azaltmaya yardımcı olmak için Güvenlik Merkezi 'ni nasıl kullanacağınızı gösterir. Senaryo, Güvenlik Merkezi [planlama ve işlemler kılavuzunda](security-center-planning-and-operations-guide.md#security-roles-and-access-controls)sunulan kurgusal Şirket, contoso ve rolleri kullanır. Bu senaryoda, aşağıdaki kişilerin rollerine odaklanıyoruz:

![Senaryo rolleri](./media/security-center-using-recommendations/scenario-roles.png)

Contoso yakın zamanda şirket içi kaynaklarından bazılarını Azure 'a geçirmiştir. Contoso, kaynaklarını korumak ve buluttaki kaynaklarından gelen güvenlik açıklarını azaltmak istiyor.

## <a name="use-azure-security-center"></a>Azure Güvenlik Merkezi’ni kullanma
Contoso BT güvenliği 'nin sunduğu David, güvenlik açıklarını engellemek ve algılamak için Contoso 'nun Azure Güvenlik Merkezi aboneliklerine Güvenlik Merkezi 'ni eklemek üzere zaten seçilmiş. 

Güvenlik Merkezi, contoso 'nun Azure kaynaklarının güvenlik durumunu otomatik olarak analiz eder ve varsayılan güvenlik ilkelerini uygular. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, güvenlik ilkesinde ayarlanan denetimlere göre **öneriler** oluşturur. 

David tüm aboneliklerinde Azure Güvenlik 'i Azure Defender etkinleştirilmiş olarak çalıştırır ve sunulan tüm öneriler ve güvenlik özellikleri paketini alır. Jeff Ayrıca, [Windows ve Linux sunucularında](quickstart-onboard-machines.md)Güvenlik Merkezi 'nin karma desteğinden yararlanmasını sağlamak için henüz buluta geçirmemiş olan mevcut şirket içi sunucuların tümünü de onpanolar.

Jeff, bir bulut iş yükü sahibidir. Jeff, contoso güvenlik ilkelerine uygun olarak güvenlik denetimlerini uygulamaktan sorumludur. 

Jeff aşağıdaki görevleri gerçekleştirir:

- Güvenlik Merkezi tarafından sunulan güvenlik önerilerini izleme
- Güvenlik önerilerini değerlendirin ve önerileri uygulayıp uygulamamaları veya kapatmak için karar verin.
- Güvenlik önerilerini Uygula

### <a name="remediate-threats-using-recommendations"></a>Önerileri kullanarak tehditleri düzeltin
Günlük izleme etkinliklerinin bir parçası olarak, Jeff Azure 'da oturum açar ve Güvenlik Merkezi 'ni açar. 

1. Jeff, iş yükünün aboneliklerini seçer.

2. Jeff, aboneliklerin ne kadar güvenli olduğunu gösteren genel bir bakış elde etmek için **güvenli puanı** denetler ve puanın 548 olduğunu görür.

3. Jeff, ilk olarak hangi önerilerin işleneceğini belirlemek için gerekir. Jeff, güvenli puan ' i tıklatır ve [güvenli puanı](secure-score-security-controls.md)ne kadar geliştirdiğine göre önerileri işlemeye başlar.

4. Jeff 'in birçok bağlı sanal makinesi olduğundan, Jeff [varlık envanterinde](asset-inventory.md)makinelere odaklanmaya karar verir.

5. Jeff, varlık envanterini açtığında önerilerin bir listesi görüntülenir. Jeff, bunları güvenli puan etkisine göre işler.

6. Jeff, Internet 'e yönelik çok sayıda sanal makineye sahiptir ve bağlantı noktaları kullanıma sunulduğundan, bir saldırganın sunucular üzerinde denetim kazanmasını sağlayacağından endişelenirler. Bu nedenle Jeff [**, tam ZAMANıNDA VM erişimi**](security-center-just-in-time.md)kullanmayı seçer.

Jeff, yüksek öncelikli ve orta öncelikli önerilere ilerlemeye devam eder ve uygulama üzerinde kararlar verir. Her öneri için, Jeff, Güvenlik Merkezi tarafından hangi kaynakların etkilendiğini, ne kadar güvenli puanı etkilediğini, her bir önerinin ne anlama geldiğini, her bir sorunun nasıl azaltılacağını gösteren düzeltme adımlarını anlamak için sunulan ayrıntılı bilgilere bakar.

### <a name="enforce-recommendations-to-prevent-security-misconfigurations"></a>Güvenlik Yapılandırması yapılandırmalarını engellemek için öneriler zorla

Kullanıcıların Jeff 'in Puanını olumsuz yönde etkileyen kaynaklar oluşturmadığından emin olmak için, bunlar için en önemli önerilerin zorla ve reddetme seçeneklerini yapılandırır. [Zorla/reddetme önerilerini kullanarak yanlış yapılandırma önleme konusunda](prevent-misconfigurations.md)daha fazla bilgi edinin.


## <a name="conclusion"></a>Sonuç
Güvenlik Merkezi 'ndeki izleme önerileri, bir saldırı gerçekleşmeden önce güvenlik açıklarını ortadan kaldırmanıza yardımcı olur. Önerileri düzelttireceğiniz zaman, güvenli puanınızın ve iş yüklerinizin güvenlik sonrası güvenliğini iyileştirmenize neden olacak. Güvenlik Merkezi, dağıttığınız yeni kaynakları otomatik olarak bulur, güvenlik ilkenize göre değerlendirir ve bunların güvenliğini sağlamak için yeni öneriler sağlar.


## <a name="next-steps"></a>Sonraki adımlar
Kaynaklarınızın zaman içinde güvende tutulmasına emin olmak için Güvenlik Merkezi 'ndeki önerileri düzenli olarak kontrol ettiğiniz bir izleme sürecinizde olduğundan emin olun.

Bu senaryo, güvenlik saldırısını azaltmaya yardımcı olmak için Güvenlik Merkezi 'nde güvenlik ilkelerini ve önerilerini nasıl kullanacağınızı gösterdi.

[Güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)ile tehditleri nasıl yanıtlayacağınızı öğrenin.
