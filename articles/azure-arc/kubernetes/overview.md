---
title: Azure Arc etkin Kubernetes 'e genel bakış
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: Bu makalede, Azure Arc etkin Kubernetes 'e genel bakış sunulmaktadır.
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.custom: references_regions
ms.openlocfilehash: 7e48ebf98f12e79cb154fb50d8e6dbdfaea1cd95
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371316"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>Azure Arc özellikli Kubernetes Önizlemesi nedir?

Azure Arc etkin Kubernetes önizlemesi 'ni kullanarak Azure 'un içindeki veya dışındaki Kubernetes kümelerini iliştirebilir ve yapılandırabilirsiniz. Azure yaya bir Kubernetes kümesi eklendiğinde Azure portal görüntülenir. Azure Resource Manager KIMLIĞI ve yönetilen kimlik olur. Kümeler standart Azure aboneliklerine iliştirilir, bir kaynak grubunda bulunur ve diğer tüm Azure kaynakları gibi etiketleri alabilir. 

Bir Kubernetes kümesini Azure 'a bağlamak için, küme yöneticisinin aracıları dağıtması gerekir. Bu aracılar adlı bir Kubernetes ad alanında çalışır `azure-arc` ve standart Kubernetes dağıtımlarıdır. Aracılar Azure ile bağlantı sağlanmasından, Azure Arc günlüklerini ve ölçümlerini toplamaya ve yapılandırma isteklerini izlemeye sorumludur. 

Azure Arc etkin Kubernetes, transit verileri güvenli hale getirmek için sektör standardı SSL 'yi destekler. Ayrıca veriler, verilerin gizliliğini sağlamak için bir Azure Cosmos DB veritabanında şifreli olarak depolanır.
 
> [!NOTE]
> Azure Arc etkin Kubernetes önizleme aşamasındadır. Bunu üretim iş yükleri için önermiyoruz.

## <a name="supported-kubernetes-distributions"></a>Desteklenen Kubernetes dağıtımları

Azure Arc etkin Kubernetes, Azure 'da AKS-Engine, Azure Stack hub 'da AKS-Engine, GKE, EKS ve VMware vSphere kümesi gibi tüm bulut Yerel Bilgi Işlem altyapısı (CNCF) sertifikalı Kubernetes kümesiyle birlikte çalışarak.

Azure Arc etkin Kubernetes özellikleri, aşağıdaki dağıtımlara göre yay ekibi tarafından test edilmiştir:
* RedHat OpenShift 4,3
* Ranhi RKE 1.0.8
* Kurallı Charmed Kubernetes 1,18
* AKS altyapısı
* Azure Stack hub 'da AKS altyapısı
* Azure Stack HIN üzerinde AKS
* Küme API sağlayıcısı Azure

## <a name="supported-scenarios"></a>Desteklenen senaryolar 

Azure Arc etkin Kubernetes bu senaryoları destekler: 

* Envanter, gruplama ve etiketleme için Azure dışında çalışan Kubernetes 'i bağlayın.

* Gilar tabanlı yapılandırma yönetimini kullanarak uygulamaları dağıtın ve yapılandırma uygulayın. 

* Kümelerinizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyici 'yi kullanın. 

* Kubernetes için Azure Ilkesini kullanarak ilkeleri uygulayın. 

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Desteklenen bölgeler 

Şu bölgelerde Azure Arc etkin Kubernetes Şu anda destekleniyor: 

* Doğu ABD 
* West Europe

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

* Azure Arc etkin Kubernetes ve Azure Kubernetes hizmeti (AKS) arasındaki fark nedir?

    Azure Kubernetes hizmeti (AKS), Azure tarafından yönetilen Kubernetes teklifidir. AKS, Azure 'da yönetilen bir Kubernetes kümesi dağıtmayı kolaylaştırır. AKS, sorumluluğun çoğunu Azure'a devrederek Kubernetes yönetiminin karmaşıklığını ve işlemsel yükünü azaltır. Kubernetes ana düğümler Azure tarafından yönetilir. Siz yalnızca aracı düğümlerini yönetir ve sürdürürsünüz.

    Azure Arc etkin Kubernetes, Azure 'un Azure Izleyici ve Azure Ilkesi gibi yönetim özelliklerini genişletmek için Kubernetes kümelerini Azure 'a bağlamanıza olanak tanır. Temel Kubernetes kümesinin kendi kendine Bakımı sizin tarafınızdan yapılır.

* Azure 'da çalışan Azure Kubernetes hizmet kümelerimi Azure 'a bağlamaya ihtiyacım var mı?

    Hayır. Azure Arc 'ın Azure Izleyici gibi etkin Kubernetes özellikleri, Azure Ilkesi (Gatekeeper), Azure 'da zaten kaynak temsili olan AKS ile yerel olarak kullanılabilir.
    
* AKS kümemi Azure Stack HI 'ye Azure yaya bağlamanız gerekir mi? Azure Stack hub veya Azure Stack Edge üzerinde çalışan Kubernetes kümeleri nelerdir?

    Evet, bu kümelerin Azure yaya bağlanması avantajlara sahiptir. Azure Resource Manager Bu Kubernetes kümeleri için bir kaynak temsili sağlar. Bu kaynak gösterimini kullanarak, küme yapılandırması, Azure Izleyici, Azure Ilkesi (Gatekeeper) gibi yetenekler bu Kubernetes kümelerine Genişletilebilir

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi bağlama](./connect-cluster.md)
