---
title: Yönetilen bir uygulamayı izlemek için Azure portal kullanma
description: Yönetilen bir uygulama için kullanılabilirliği ve Uyarıları izlemek üzere Azure portal nasıl kullanacağınızı gösterir.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: afe78dd00ecebdc54b6d73c4c8324729e117d95b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75651754"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Yönetilen uygulamanın dağıtılan bir örneğini izleme

Azure aboneliğinize yönetilen bir uygulama dağıttıktan sonra, uygulamanın durumunu denetlemek isteyebilirsiniz. Bu makalede, durumu denetlemek için Azure portal seçenekleri gösterilmektedir. Yönetilen uygulamanızdaki kaynakların kullanılabilirliğini izleyebilirsiniz. Ayrıca, uyarıları ayarlayabilir ve görüntüleyebilirsiniz.

## <a name="view-resource-health"></a>Kaynak durumunu görüntüleme

1. Yönetilen uygulama örneğinizi seçin.

   ![Yönetilen uygulama seçin](./media/monitor-managed-application-portal/select-managed-application.png)

1. **Kaynak Durumu**’nu seçin.

   ![Kaynak durumunu seçin](./media/monitor-managed-application-portal/select-resource-health.png)

1. Yönetilen uygulamanızdaki kaynakların kullanılabilirliğini görüntüleyin.

   ![Kaynak durumunu görüntüleme](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Uyarıları görüntüleme

1. **Uyarıları**seçin.

   ![Uyarıları Seç](./media/monitor-managed-application-portal/select-alerts.png)

1. Yapılandırılmış uyarı kurallarınız varsa, oluşan uyarılarla ilgili bilgileri görürsünüz.

   ![Uyarıları görüntüleme](./media/monitor-managed-application-portal/view-alerts.png)

1. Uyarı kuralları eklemek için **+ Yeni uyarı kuralı**' nı seçin.

   ![Uyarı oluşturma](./media/monitor-managed-application-portal/create-new-alert.png)

Yönetilen uygulama örneğiniz veya yönetilen uygulamadaki kaynaklar için uyarılar oluşturabilirsiniz. Uyarı oluşturma hakkında daha fazla bilgi için bkz. [Microsoft Azure uyarılara genel bakış](../../azure-monitor/platform/alerts-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulama örnekleri için bkz. [Azure yönetilen uygulamalar Için örnek projeler](sample-projects.md).
* Yönetilen bir uygulama dağıtmak için bkz. [Azure Portal aracılığıyla Service Catalog uygulaması dağıtma](deploy-service-catalog-quickstart.md).