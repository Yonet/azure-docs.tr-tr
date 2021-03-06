---
title: Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı
description: Bu makalede, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için ITSM ürünlerinizi/hizmetlerinizi Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile bağlama hakkında bilgi sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: a372cdcd05267f3bdb093f676948a79c473ad955
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804029"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı ile ITSM ürünlerine/hizmetlerine bağlanma
Bu makalede, çalışma öğelerinizi merkezi olarak yönetmek için ıTSM ürününüz/hizmetiniz ile Log Analytics BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantının nasıl yapılandırılacağı hakkında bilgi sağlanır. ISMC hakkında daha fazla bilgi için bkz. [genel bakış](./itsmc-overview.md).

Aşağıdaki ıTSM ürünleri/hizmetleri desteklenir. Ürünün ıSMC 'a nasıl bağlanacağı hakkında ayrıntılı bilgileri görüntülemek için ürünü seçin.

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

> [!NOTE]
> Çeriyi ve Provance müşterilerinin, tümleştirmede başka bir çözüm olarak, Cherwell ve Provance uç noktasına [Web kancası eylemi](./action-groups.md#webhook) kullanmasını sağlıyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)