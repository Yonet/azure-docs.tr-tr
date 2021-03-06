---
title: Etkinlik günlükleri Azure DevTest Labs | Microsoft Docs
description: Bu makalede Azure DevTest Labs için etkinlik günlüklerini görüntüleme adımları sunulmaktadır.
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: 24b5eb25f1c8c9044ad6e77545381a43a11714f9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87099790"
---
# <a name="view-activity-logs-for-labs-in-azure-devtest-labs"></a>Azure DevTest Labs Labs için etkinlik günlüklerini görüntüleme 
Bir veya daha fazla Laboratuvarı oluşturduktan sonra, laboratuvarlarınızın nasıl ve ne zaman eriştiğini, değiştirildiğini ve yönetildiğini ve kim tarafından yapılacağını izlemek isteyeceksiniz. Azure DevTest Labs, bu işlemleri laboratuvarlara karşı sağlamak için Azure Izleyici 'yi özellikle **etkinlik günlükleri**'ni kullanır. 

Bu makalede, Azure DevTest Labs bir laboratuvardaki etkinlik günlüklerinin nasıl görüntüleneceği açıklanır.

## <a name="view-activity-log-for-a-lab"></a>Laboratuvar için etkinlik günlüğünü görüntüleme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. **Tüm hizmetler**' i seçin ve **DevOps** bölümünde **DevTest Labs** ' i seçin. DevOps bölümünde, **DevTest Labs** ' in yanındaki * (Star) **DEVOPS** seçeneğini belirleyin. Bu eylem, bir sonraki sefer kolayca erişebilmek için **DevTest Labs** 'i sol gezinti menüsüne ekler. Ardından, sol gezinti menüsünde **DevTest Labs** ' i seçebilirsiniz.

    ![Tüm hizmetler-DevTest Labs seçin](./media/devtest-lab-create-lab/all-services-select.png)
1. Laboratuvarlar listesinden laboratuvarınızı seçin.
1. Laboratuvarın giriş sayfasında, sol taraftaki menüden **Konfigürasyonlar ve ilkeler** ' i seçin. 

    :::image type="content" source="./media/activity-logs/configuration-policies-link.png" alt-text="Sol menüdeki yapılandırma ve ilkeleri seçin":::
1. **Yapılandırma ve ilkeler** sayfasında, **Yönet**' ın altındaki sol menüde **etkinlik günlüğü** ' nü seçin. Laboratuvarda gerçekleştirilen işlemler için girişleri görmeniz gerekir. 

    :::image type="content" source="./media/activity-logs/activity-log.png" alt-text="Sol menüdeki yapılandırma ve ilkeleri seçin":::    
1. Ayrıntılarını görmek için bir olay seçin. **Özet** sayfasında, işlem adı, zaman damgası ve işlemi kimin gerçekleştirmediği gibi bilgileri görürsünüz. 
    
    :::image type="content" source="./media/activity-logs/stop-vm-event.png" alt-text="Sol menüdeki yapılandırma ve ilkeleri seçin":::        
1. Daha fazla ayrıntı görmek için **JSON** sekmesine geçin. Aşağıdaki örnekte, sanal makinenin adını ve VM üzerinde gerçekleştirilen işlemi (durduruldu) görebilirsiniz.

    :::image type="content" source="./media/activity-logs/stop-vm-event-json.png" alt-text="Sol menüdeki yapılandırma ve ilkeleri seçin":::           
1. Değişiklik geçmişini görmek için **değişiklik geçmişi (Önizleme)** sekmesine geçin. Aşağıdaki örnekte, VM 'de yapılan değişikliği görürsünüz. 

    :::image type="content" source="./media/activity-logs/change-history.png" alt-text="Sol menüdeki yapılandırma ve ilkeleri seçin":::             
1. Değişiklik hakkında daha fazla ayrıntı görmek için değişiklik geçmişi listesindeki değişikliği seçin. 

    :::image type="content" source="./media/activity-logs/change-details.png" alt-text="Sol menüdeki yapılandırma ve ilkeleri seçin":::             

Etkinlik günlükleri hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğü](../azure-monitor/platform/activity-log.md).

## <a name="next-steps"></a>Sonraki adımlar

- Etkinlik günlüklerinde **Uyarı** ayarlama hakkında bilgi edinmek için bkz. [uyarı oluşturma](create-alerts.md).
- Etkinlik günlükleri hakkında daha fazla bilgi için bkz.  [Azure etkinlik günlüğü](../azure-monitor/platform/activity-log.md).

