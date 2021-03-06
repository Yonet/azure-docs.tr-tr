---
title: IP güvenlik duvarı kurallarını yapılandırma
description: Azure SYNAPSE Analytics 'te IP güvenlik duvarı kurallarını yapılandırmanızı öğretir bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: b937dad6c3c8f5a5773ca7779493b41c905307b1
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99226514"
---
# <a name="azure-synapse-analytics-ip-firewall-rules"></a>Azure SYNAPSE Analytics IP güvenlik duvarı kuralları

Bu makalede IP güvenlik duvarı kuralları açıklanacaktır ve bunları Azure SYNAPSE Analytics 'te nasıl yapılandırabileceğiniz öğretir.

## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları

IP güvenlik duvarı kuralları her isteğin kaynak IP adresine bağlı olarak Synapse çalışma alanınıza erişim izni verir veya erişimi reddeder. Çalışma alanınız için IP güvenlik duvarı kurallarını yapılandırabilirsiniz. Çalışma alanı düzeyinde yapılandırılan IP güvenlik duvarı kuralları, çalışma alanının tüm genel uç noktalarına uygulanır (adanmış SQL havuzları, sunucusuz SQL havuzu ve geliştirme).

## <a name="create-and-manage-ip-firewall-rules"></a>IP güvenlik duvarı kurallarını oluşturma ve yönetme

IP Güvenlik Duvarı kurallarının bir Synapse çalışma alanına Eklenme iki yolu vardır. Çalışma alanınıza bir IP Güvenlik Duvarı eklemek için **güvenlik + ağ** ' ı seçin ve çalışma alanı oluşturma SıRASıNDA **tüm IP adreslerinden gelen bağlantılara izin ver** ' i işaretleyin.

![Güvenlik + ağ düğmesini vurgulayan ekran görüntüsü.](./media/synpase-workspace-ip-firewall/ip-firewall-1.png)

![Azure portal SYNAPSE çalışma alanı IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-2.png)

Ayrıca, çalışma alanı oluşturulduktan sonra bir Synapse çalışma alanına IP güvenlik duvarı kuralları ekleyebilirsiniz. Azure portal güvenlik altında **güvenlik** **duvarları** ' nı seçin. Yeni bir IP güvenlik duvarı kuralı eklemek için, buna bir ad, başlangıç IP 'si ve bitiş IP 'si verin. Tümünü seçtikten sonra **Kaydet**'i seçin.

![Azure portal 'de Azure SYNAPSE Workspace IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-3.png)

## <a name="connect-to-synapse-from-your-own-network"></a>Kendi ağınızdan Synapse’e bağlanma

SYNAPSE Studio kullanarak SYNAPSE çalışma alanınıza bağlanabilirsiniz. Çalışma alanınızdaki SQL kaynaklarına (adanmış SQL havuzları ve sunucusuz SQL havuzu) bağlanmak için SQL Server Management Studio (SSMS) de kullanabilirsiniz.

Ağınızdaki ve yerel bilgisayarınızdaki güvenlik duvarının SYNAPSE Studio için 80, 443 ve 1443 TCP bağlantı noktalarında giden iletişime izin verdiğinden emin olun.

Ayrıca, SYNAPSE Studio için UDP bağlantı noktası 53 ' de giden iletişime izin vermeniz gerekir. SSMS ve Power BI gibi araçları kullanarak bağlanmak için TCP bağlantı noktası 1433 ' de giden iletişime izin vermeniz gerekir.

SQL bağlantı ilkesi, çalışma alanı için *varsayılan* olarak ayarlanır. İstemcilerin giden iletişime izin [vermek IÇIN IP](../../azure-sql/database/connectivity-architecture.md#connection-policy)adresleri ve bağlantı noktaları hakkında daha fazla bilgi edinebilirsiniz.




## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE çalışma alanı](../quickstart-create-workspace.md) oluşturma

[Yönetilen çalışma alanı sanal ağı](./synapse-workspace-managed-vnet.md) Ile Azure SYNAPSE çalışma alanı oluşturma