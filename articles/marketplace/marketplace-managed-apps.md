---
title: Azure uygulamaları yönetilen uygulama teklifi Yayımlama Kılavuzu-Azure Marketi
description: Bu makalede, Azure Marketi 'nde yönetilen bir uygulama yayımlama gereksinimleri açıklanmaktadır.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: msjogarrig
ms.author: jogarrig
ms.date: 09/04/2020
ms.openlocfilehash: d4fb3354b7035149b80191528b2f5335b593b764
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96433541"
---
# <a name="publishing-guide-for-azure-managed-applications"></a>Azure yönetilen uygulamalar için Yayımlama Kılavuzu

Azure *yönetilen uygulama* teklifi, Azure Market 'te Azure uygulaması yayımlamanın bir yoludur. Yönetilen uygulamalar, Azure Marketi aracılığıyla dağıtılan ve faturalandırılan Transact teklifleridir. Kullanıcının gördüğü listeleme seçeneği *Şimdi alın*.

Bu makalede, yönetilen uygulama teklif türü için gereksinimler açıklanmaktadır.

Aşağıdaki koşullarda yönetilen uygulama teklifi türünü kullanın:

- Bir sanal makine (VM) ya da hizmet olarak altyapı (IaaS) tabanlı bir çözüm kullanarak müşteriniz için abonelik tabanlı bir çözüm dağıtacağız.
- Siz veya müşteriniz çözümün bir iş ortağı tarafından yönetilmesini gerektirir.

>[!NOTE]
>Örneğin, bir iş ortağı bir sistem tümleştirici veya yönetilen hizmet sağlayıcısı (MSP) olabilir.  

## <a name="managed-application-offer-requirements"></a>Yönetilen uygulama teklifi gereksinimleri

|Gereksinimler |Ayrıntılar  |
|---------|---------|
|Bir Azure aboneliği | Yönetilen uygulamalar bir müşterinin aboneliğine dağıtılmalıdır, ancak üçüncü taraf tarafından yönetilebilir. |
|Faturalandırma ve ölçüm    |  Kaynaklar bir müşterinin Azure aboneliğinde sağlanır. Kullandıkça Öde ödeme modelini kullanan VM 'Ler, müşteri ile Microsoft aracılığıyla işlem yapar ve müşterinin Azure aboneliği aracılığıyla faturalandırılır. <br><br> Kendi lisansını getir VM 'Leri için, Microsoft, müşteri aboneliğinde oluşan tüm altyapı maliyetlerini faturalandırır, ancak müşteriyle doğrudan Transact yazılım lisanslama ücretleri.        |
|Azure ile uyumlu bir sanal sabit disk (VHD)    |   VM 'Ler Windows veya Linux üzerinde oluşturulmalıdır.<br><br>Linux VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure 'da desteklenen Linux dağıtımları](../virtual-machines/linux/endorsed-distros.md).<br><br>Windows VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure Uygulama teklifi oluşturma](./create-new-azure-apps-offer.md). |

---

> [!NOTE]
> Yönetilen uygulamaların Azure Marketi aracılığıyla dağıtılabilir olması gerekir. Müşteri iletişimi sorun oluşturacaksa, müşteri adayı paylaşımını etkinleştirdikten sonra ilgilenen müşterilere ulaşın.  

> [!Note]
> Bir bulut çözümü sağlayıcısı (CSP) iş ortağı kanalı kabul etme artık kullanılabilir. Teklifinizi Microsoft CSP iş ortağı kanalları aracılığıyla pazarlama hakkında daha fazla bilgi için bkz. [Cloud Solution Providers](./cloud-solution-providers.md).

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure Marketi ile bulut işletmenizi nasıl büyütireceğinizi](https://azuremarketplace.microsoft.com/sell)öğrenin.

Iş Ortağı Merkezi 'nde çalışmaya kaydolmak ve başlamak için:

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için bkz. [Azure Uygulama teklifi oluşturma](./create-new-azure-apps-offer.md) .
