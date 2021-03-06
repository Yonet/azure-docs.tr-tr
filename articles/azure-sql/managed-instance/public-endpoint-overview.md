---
title: Güvenli Azure SQL yönetilen örneği genel uç noktaları
description: Azure SQL yönetilen örneğindeki genel uç noktaları güvenli bir şekilde kullanın
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, sstein
ms.date: 05/08/2019
ms.openlocfilehash: d9c1828732b9a4e0e85c3af2263f097edd54437d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91332857"
---
# <a name="use-azure-sql-managed-instance-securely-with-public-endpoints"></a>Genel uç noktalarla güvenli Azure SQL yönetilen örneği kullanma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneği, [genel uç noktalar](../../virtual-network/virtual-network-service-endpoints-overview.md)üzerinden Kullanıcı bağlantısı sağlayabilir. Bu makalede, bu yapılandırmanın nasıl daha güvenli hale kullanılacağı açıklanmaktadır.

## <a name="scenarios"></a>Senaryolar

Azure SQL yönetilen örneği, sanal ağının içinden bağlantıya izin veren özel bir uç nokta sağlar. Varsayılan seçenek, maksimum yalıtımı sağlamaktır. Ancak, genel uç nokta bağlantısı sağlamanız gereken senaryolar vardır:

- Yönetilen örnek, tek kiracılı hizmet olarak platform (PaaS) teklifleriyle tümleştirmelidir.
- VPN kullanırken mümkün olandan daha yüksek miktarda veri alışverişi yapmanız gerekir.
- Şirket ilkeleri, şirket ağları içinde PaaS 'yi yasaklar.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Ortak uç nokta erişimi için yönetilen bir örnek dağıtma

Zorunlu olmasa da, genel uç nokta erişimiyle yönetilen bir örnek için ortak dağıtım modeli, örneği ayrılmış bir yalıtılmış sanal ağda oluşturmaktır. Bu yapılandırmada, sanal ağ yalnızca sanal küme yalıtımı için kullanılır. Yönetilen örneğin IP adresi alanının bir kurumsal ağın IP adresi alanıyla örtüştüğü ne olursa olsun.

## <a name="secure-data-in-motion"></a>Hareket halindeki verileri güvenli hale getirme

İstemci sürücüsü şifrelemeyi destekliyorsa SQL yönetilen örnek veri trafiği her zaman şifrelenir. Yönetilen örnek ile diğer Azure sanal makineleri veya Azure hizmetleri arasında gönderilen veriler hiçbir şekilde Azure 'un omurgasını bırakır. Yönetilen örnek ve şirket içi ağ arasında bağlantı varsa Azure ExpressRoute 'u kullanmanızı öneririz. ExpressRoute, verileri genel İnternet üzerinden taşımadan kaçınmanıza yardımcı olur. Yönetilen örnek özel bağlantısı için yalnızca özel eşleme kullanılabilir.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Gelen ve giden bağlantıyı kilitle

Aşağıdaki diyagramda önerilen güvenlik yapılandırması gösterilmektedir:

![Gelen ve giden bağlantıyı kilitlemek için güvenlik yapılandırması](./media/public-endpoint-overview/managed-instance-vnet.png)

Yönetilen bir örnek adanmış bir [genel uç nokta adresine](management-endpoint-find-ip-address.md)sahiptir. İstemci tarafı giden güvenlik duvarında ve ağ güvenlik grubu kurallarında, giden bağlantıyı sınırlamak için bu genel uç nokta IP adresini ayarlayın.

Yönetilen örneğe giden trafiğin güvenilen kaynaklardan geldiğinden emin olmak için, iyi bilinen IP adreslerine sahip kaynaklardan bağlanmanız önerilir. 3342 numaralı bağlantı noktasında yönetilen örnek genel uç noktasına erişimi sınırlandırmak için bir ağ güvenlik grubu kullanın.

İstemcilerin şirket içi bir ağdan bağlantı başlatması gerektiğinde, kaynak adresin iyi bilinen bir IP adresi kümesine çevrildiğinden emin olun. Bunu yapmazsanız (örneğin, bir mobil iş gücünün tipik bir senaryo olması halinde), [noktadan sıteye VPN bağlantıları ve özel uç nokta](point-to-site-p2s-configure.md)kullanmanızı öneririz.

Azure 'dan bağlantı başlatılırsa, trafiğin iyi bilinen bir atanan [sanal IP adresinden](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip) (örneğin, bir sanal makine) gelmesini öneririz. Sanal IP (VIP) adreslerini yönetmeyi daha kolay hale getirmek için [genel IP adresi öneklerini](../../virtual-network/public-ip-address-prefix.md)kullanmak isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Örnekleri yönetmek için genel uç noktayı yapılandırma hakkında bilgi edinin: [ortak uç noktayı yapılandırma](public-endpoint-configure.md)
