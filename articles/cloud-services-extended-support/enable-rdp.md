---
title: Cloud Services uzak masaüstünü uygulama (genişletilmiş destek)
description: Cloud Services için uzak masaüstünü etkinleştir (genişletilmiş destek)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 53f873013a6f16ce5a28ee5d915afa556057f643
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744674"
---
# <a name="apply-the-remote-desktop-extension-to-azure-cloud-services-extended-support"></a>Uzak Masaüstü uzantısını Azure Cloud Services uygulama (genişletilmiş destek)

Azure portal, uygulama dağıtıldıktan sonra bile uzak masaüstünü etkinleştirmek için Uzak Masaüstü uzantısını kullanır. Bulut hizmetinizin uzak masaüstü ayarları uzak masaüstünü etkinleştirmenizi, yerel yönetici hesabını güncelleştirmenizi, kimlik doğrulamasında kullanılan sertifikaları seçmenizi ve bu sertifikaların sona erme tarihini ayarlamanızı sağlar. 

## <a name="apply-remote-desktop--extension"></a>Uzak Masaüstü uzantısını Uygula
1. Uzak masaüstünü etkinleştirmek istediğiniz bulut hizmetine gidin ve sol gezinti bölmesinde **"Uzak Masaüstü"** seçeneğini belirleyin.

    :::image type="content" source="media/remote-desktop-1.png" alt-text="Görüntüde Uzak Masaüstü seçeneğinin seçilmesi gösterilmektedir Azure portal":::

2. **Ekle**’yi seçin.
3. İçin uzak masaüstünü etkinleştirmek üzere rolleri seçin.
4. Kullanıcı adı, parola, süre sonu ve sertifika (gerekli değil) için gerekli alanları girin.

    :::image type="content" source="media/remote-desktop-2.png" alt-text="Görüntü, Uzak masaüstüne bağlanmak için gereken bilgileri gösterir.":::

5. İşiniz bittiğinde **Kaydet**' i seçin. Rol örneklerinizin bağlantı almaya hazırlanmadan önce bu işlem birkaç dakika sürer.

## <a name="connect-to-role-instances-with-remote-desktop-enabled"></a>Uzak Masaüstü etkinken rol örneklerine bağlanma
Roller üzerinde Uzak Masaüstü etkinleştirildikten sonra, Azure portal doğrudan bir bağlantı başlatabilirsiniz.

1. **Rol ve örneklere** tıklayarak örnek ayarlarını açın.

    :::image type="content" source="media/remote-desktop-3.png" alt-text="Görüntüde yapılandırma dikey penceresinde roller ve örnekler seçeneğinin seçilmesi gösterilmektedir.":::

2. Uzak Masaüstü 'nün yapılandırıldığı bir rol örneği seçin.
3. Uzak Masaüstü bağlantısı dosyasını indirmek için **Bağlan** ' a tıklayın.

    :::image type="content" source="media/remote-desktop-4.png" alt-text="Görüntü, Azure portal çalışan rolü örneğini seçmeyi gösterir.":::
    
4. Rol örneğine bağlanmak için dosyayı açın.


## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.