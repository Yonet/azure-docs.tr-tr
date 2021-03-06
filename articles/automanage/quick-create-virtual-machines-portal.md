---
title: Hızlı başlangıç-Azure portal VM 'Ler için Azure oto yönetimini etkinleştirin
description: Azure portal yeni veya var olan bir VM 'de sanal makineler için kolayca hızlı bir şekilde etkinleştirmeyi öğrenin.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 09/04/2020
ms.author: jushiman
ms.openlocfilehash: 69f43b626bb50171ceaca1b7a8761bec040d1dd6
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92897237"
---
# <a name="quickstart-enable-azure-automanage-for-virtual-machines-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal sanal makineler için Azure oto yönetimini etkinleştirin

Yeni veya mevcut bir sanal makinede oto yönetimini etkinleştirmek için Azure portal kullanarak sanal makineler için Azure 'u Oto için bir kez daha kullanmaya başlayın.


## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) .

> [!NOTE]
> Ücretsiz deneme hesaplarının bu öğreticide kullanılan sanal makinelere erişimi yoktur. Lütfen Kullandıkça Öde aboneliğine yükseltin.

> [!IMPORTANT]
> Mevcut bir oto Yönet hesabı kullanarak, oto yönetimini etkinleştirmek için VM 'lerinizi içeren kaynak grubunda **katkıda** bulunan rolüne sahip olmanız gerekir. Yeni bir bir oto Yönet hesabıyla bir oto yönetimi etkinleştirirseniz, aşağıdaki izinlere sahip olmanız gerekir: **sahip** rolü veya **katılımcısı** , aboneliğinizdeki **Kullanıcı erişimi yönetici** rolleriyle birlikte.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com/) oturum açın.


## <a name="enable-automanage-for-vms-on-an-existing-vm"></a>Mevcut bir VM 'de VM 'Ler için oto yönetimini etkinleştir

1. Arama çubuğunda, **Automanage: Azure sanal makine en iyi uygulamaları** ' nı arayıp seçin.

2. **Mevcut VM 'de etkinleştir '** i seçin.

    :::image type="content" source="media\quick-create-virtual-machine-portal\zero-vm-list-view.png" alt-text="Mevcut VM 'de etkinleştirin.":::

3. **Makineleri seçin** dikey penceresinde:
    1. **Abonelik** ve **kaynak grubunuza** göre VM 'leri listesini filtreleyin.
    1. Eklemek istediğiniz her bir sanal makinenin onay kutusunu işaretleyin.
    1. **Seç** düğmesine tıklayın.

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-select-machine.png" alt-text="Mevcut VM 'de etkinleştirin.":::

4. **Yapılandırma profili** altında, Araştır ' a tıklayın **ve profilleri ve tercihleri değiştirin** .

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-quick-create.png" alt-text="Mevcut VM 'de etkinleştirin.":::

5. **Yapılandırma profilini seç + Tercihler** dikey penceresinde:
    1. Sol taraftaki bir profil seçin: test için *geliştirme/test* *, üretim için üretim.*
    1. **Seç** düğmesine tıklayın.

    :::image type="content" source="media\quick-create-virtual-machine-portal\browse-production-profile.png" alt-text="Mevcut VM 'de etkinleştirin.":::

6. **Etkinleştir** düğmesine tıklayın.


## <a name="enable-automanage-for-vms-on-a-new-vm"></a>Yeni bir VM 'de VM 'Ler için oto yönetimini etkinleştir

Yeni bir sanal makine oluşturmak ve oto yönetimi etkinleştirmek için [burada](https://aka.ms/automanageportalnextstep) Azure Portal oturum açın.

1. Hızlı Başlangıç bölümünde oluşturma adımlarını izleyin [-Azure Portal bir Windows sanal makinesi oluşturun](..\virtual-machines\windows\quick-create-portal.md).

2. SANAL ağınız dağıtıldıktan sonra, en altta bulunan **İleri doğru adımları** içeren dağıtım durumu sayfasını görürsünüz.

    :::image type="content" source="media\quick-create-virtual-machine-portal\create-next-steps.png" alt-text="Mevcut VM 'de etkinleştirin.":::

3. **Sonraki adımlarda** , **sanal makine en iyi uygulamalarını etkinleştir** ' i seçin.

4. Otomatik **Yönet – Azure sanal makine en iyi uygulamaları** sayfasında, **makineler** OTOMATIK olarak yeni oluşturulan VM 'niz tarafından doldurulur.

    :::image type="content" source="media\quick-create-virtual-machine-portal\create-new-enable-overview.png" alt-text="Mevcut VM 'de etkinleştirin.":::

5. **Yapılandırma profili** altında, Araştır ' a tıklayın **ve profilleri ve tercihleri değiştirin** .

6. **Yapılandırma profilini seç + Tercihler** dikey penceresinde:
    1. Sol taraftaki bir profil seçin: test için *geliştirme/test* *, üretim için üretim.*
    1. **Seç** düğmesine tıklayın.

    :::image type="content" source="media\quick-create-virtual-machine-portal\browse-production-profile.png" alt-text="Mevcut VM 'de etkinleştirin.":::

7. **Etkinleştir** düğmesine tıklayın.

## <a name="disable-automanage-for-vms"></a>VM 'Ler için oto yönetimini devre dışı bırak

Oto yönetimini devre dışı bırakarak sanal makineler için Azure oto yönetimi 'ni hızlıca durdurun.

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Mevcut VM 'de etkinleştirin.":::

1. Otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin.
1. Devre dışı bırakmak istediğiniz sanal makinenin yanındaki onay kutusunu seçin.
1. **Devre dışı bırak automanagent** düğmesine tıklayın.
1. Kabul etmiş önce **devre dışı bırakmak** için ortaya çıkan açılan pencerede iletiyi dikkatle okuyun.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makineler için Azure oto yönetimi 'ni denemek üzere yeni bir kaynak grubu oluşturduysanız ve artık gerekmiyorsa, kaynak grubunu silebilirsiniz. Grubun silinmesi, VM 'yi ve kaynak grubundaki tüm kaynakları da siler.

Azure oto yönetimi, içindeki kaynakları depolamak için varsayılan kaynak gruplarını oluşturur. Tüm kaynakları temizlemek için "DefaultResourceGroupRegionName" ve "AzureBackupRGRegionName" adlandırma kuralına sahip kaynak gruplarını kontrol edin.

1. **Kaynak grubunu** seçin.
1. Kaynak grubunun sayfasında **Sil** ' i seçin.
1. İstendiğinde, kaynak grubunun adını onaylayın ve **Sil** ' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta VM 'Ler için Azure oto yönetimi 'ni etkinleştirdiniz. 

Sanal makinenizde oto yönetimi etkinleştirirken özelleştirilmiş tercihleri nasıl oluşturup uygulayabileceğinizi öğrenin. 

> [!div class="nextstepaction"]
> [VM 'ler için Azure oto yönetimi-özel yapılandırma profili](virtual-machines-custom-preferences.md)
