---
title: Azure Otomasyonu Güncelleştirme Yönetimi özelliğini kaldır
description: Bu makalede, Güncelleştirme Yönetimi kullanmayı durdurmayı ve Log Analytics çalışma alanından bir Otomasyon hesabının bağlantısını kesmeyi anlatır.
services: automation
ms.date: 07/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 1d83f859fce33b9499d01c4b58e69f56fdbbb293
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92222949"
---
# <a name="remove-update-management-from-automation-account"></a>Otomasyon hesabından Güncelleştirme Yönetimi kaldır

Azure Otomasyonu Güncelleştirme Yönetimi kullanarak sanal makinelerinizde güncelleştirmelerin yönetimini etkinleştirdikten sonra, bunu kullanmayı durdurmayı ve yapılandırmayı hesapla ve bağlantılı Log Analytics çalışma alanından kaldırmayı seçebilirsiniz.  Bu makalede, yönetilen VM 'Ler, Otomasyon hesabınız ve Log Analytics çalışma alanınızdan Güncelleştirme Yönetimi tamamen nasıl kaldıracakları açıklanır.

## <a name="sign-into-the-azure-portal"></a>Azure portalda oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="remove-management-of-vms"></a>VM 'lerin yönetimini kaldırma

Güncelleştirme Yönetimi kaldırmadan önce, VM 'lerinizi yönetmeyi durdurmanız gerekir. Özelliğin kaydını silmek için [güncelleştirme yönetimi VM 'Leri kaldırma](remove-vms.md) bölümüne bakın.

## <a name="remove-updatemanagement-solution"></a>UpdateManagement çözümünü kaldır

Otomasyon hesabının çalışma alanından bağlantısını kaldırabilmeniz için önce Güncelleştirme Yönetimi tamamen kaldırmak için aşağıdaki adımları izlemeniz gerekir. **Güncelleştirmeler** çözümünü çalışma alanından kaldıracaksınız.

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Azure portal, **tüm hizmetler**' i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, giriş listenize göre öneriler uygular. **Log Analytics**’i seçin.

3. Log Analytics çalışma alanları listenizde, Güncelleştirme Yönetimi etkinleştirildiğinde seçtiğiniz çalışma alanını seçin.

4. Sol tarafta **çözümler**' i seçin.  

5. Çözümler listesinde güncelleştirmeler ' i **(çalışma alanı adı)** seçin. Çözümün **genel bakış** sayfasında **Sil**' i seçin. Onaylamanız istendiğinde **Evet**' i seçin.

## <a name="unlink-workspace-from-automation-account"></a>Çalışma alanının Otomasyon hesabından bağlantısını kaldırma

1. Azure portal **Otomasyon hesapları**' nı seçin.

2. Otomasyon hesabınızı açın ve sol taraftaki **Ilgili kaynaklar** altında **bağlantılı çalışma alanı** ' nı seçin.

3. **Çalışma alanının bağlantısını kaldır** sayfasında, **çalışma alanının bağlantısını kaldır** ' ı seçin ve istemlere yanıtlayın.

   ![Çalışma alanının bağlantısını Kaldır sayfası](media/remove-feature/automation-unlink-workspace-blade.png)

Log Analytics çalışma alanının bağlantısını kesmeyi denediğinde, menüdeki **Bildirimler** altında ilerlemeyi izleyebilirsiniz.

Alternatif olarak, Log Analytics çalışma alanınızın, çalışma alanının içinden Otomasyon hesabınızla bağlantısını kaldırabilirsiniz:

1. Azure Portal'da **Log Analytics**'i seçin.

2. Çalışma alanından **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin.

3. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

Otomasyon hesabının bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

## <a name="cleanup-automation-account"></a>Temizleme Otomasyon hesabı

Güncelleştirme Yönetimi, Azure SQL izlemenin önceki sürümlerini destekleyecek şekilde yapılandırıldıysa, özelliğin kurulumu, kaldırmanız gereken Otomasyon varlıklarını oluşturmuş olabilir. Güncelleştirme Yönetimi için, artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz:

   * Zamanlamayı Güncelleştir-her birinin, oluşturduğunuz güncelleştirme dağıtımıyla eşleşen bir adı vardır.
   * Güncelleştirme Yönetimi için oluşturulan karma çalışan grupları-her biri *machine1.contoso.com_9ceb8108-26c9-4051-B6B3-227600d715c8*' e benzer şekilde adlandırılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu özelliği yeniden etkinleştirmek için bkz. [Otomasyon hesabından güncelleştirme yönetimi etkinleştirme](enable-from-automation-account.md), [Azure portal göz atarak güncelleştirme yönetimi etkinleştirme](enable-from-portal.md), [runbook 'Tan GÜNCELLEŞTIRME YÖNETIMI etkinleştirme](enable-from-runbook.md)veya [bir Azure VM 'den güncelleştirme yönetimi etkinleştirme](enable-from-vm.md).
