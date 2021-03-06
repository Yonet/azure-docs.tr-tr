---
title: Azure 'da bir Windows 10 veya Windows Server 2016 VM 'ye uzaktan bağlandığınızda netvsc.sys sorunu giderme | Microsoft Docs
description: Azure 'da bir Windows 10 veya Windows Server 2016 VM 'ye bağlanırken netsvc.sys ilgili bir RDP sorunuyla ilgili sorunları nasıl giderebileceğinizi öğrenin.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/19/2018
ms.author: genli
ms.openlocfilehash: 0f5a414f00ffa50114f090fc19f37b8a85428547
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86509010"
---
# <a name="cannot-connect-remotely-to-a-windows-10-or-windows-server-2016-vm-in-azure-because-of-netvscsys"></a>netvsc.sys nedeniyle Azure 'da Windows 10 veya Windows Server 2016 VM 'ye uzaktan bağlanılamıyor

Bu makalede bir Hyper-V Server 2016 ana bilgisayarında Windows 10 veya Windows Server 2016 Datacenter sanal makinesine (VM) bağlandığınızda ağ bağlantısı bulunmayan bir sorunun nasıl giderileceği açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Uzak Masaüstü Protokolü (RDP) kullanarak bir Azure Windows 10 veya Windows Server 2016 VM 'sine bağlanamazsınız. [Önyükleme tanılamasında](boot-diagnostics.md), ekranda ağ arabirim kartı (NIC) üzerinde kırmızı bir çapraz gösterilir. Bu, işletim sistemi tam olarak yüklendikten sonra sanal makinenin bağlantısının olmadığını gösterir.

Genellikle, bu sorun Windows [build 14393](https://support.microsoft.com/help/4093120/) ve [derleme 15063](https://support.microsoft.com/help/4015583/)' de oluşur. İşletim sisteminizin sürümü bu sürümlerden daha sonra ise, bu makale senaryonuz için de geçerlidir. Sistemin sürümünü denetlemek için, [seri erişim konsolu özelliğinde](serial-console-windows.md)bir cmd oturumu açın ve ardından **ver**' i çalıştırın.

## <a name="cause"></a>Nedeni

Bu sorun, yüklü netvsc.sys sistem dosyasının sürümü **10.0.14393.594** veya **10.0.15063.0**olduğunda meydana gelebilir. Bu netvsc.sys sürümleri sistemin Azure platformuyla etkileşimini engelleyebilir.


## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin [sistem diskinin bir anlık görüntüsünü](../windows/snapshot-copy-managed-disk.md) yedek olarak alın. Bu sorunu gidermek için, sanal makinenin sistem diskini bir kurtarma VM 'sine ekleyerek seri konsolu 'Nu kullanın veya [VM 'yi çevrimdışı onarın](#repair-the-vm-offline) .


### <a name="use-the-serial-console"></a>Seri konsolunu kullanma

[Seri konsoluna bağlanın, bir PowerShell örneği açın](serial-console-windows.md)ve ardından aşağıdaki adımları izleyin.

> [!NOTE]
> VM 'niz üzerinde seri konsol etkinleştirilmemişse, [sanal makineyi çevrimdışı olarak Onar](#repair-the-vm-offline) bölümüne gidin.

1. Bir PowerShell örneğinde, dosyanın sürümünü almak için aşağıdaki komutu çalıştırın (**c:\windows\system32\drivers\netvsc.sys**):

   ```
   (get-childitem "$env:systemroot\system32\drivers\netvsc.sys").VersionInfo.FileVersion
   ```

2. İlgili güncelleştirmeyi aynı bölgeden çalışan bir sanal makineye bağlı yeni veya mevcut bir veri diskine indirin:

   - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562)   veya sonraki bir güncelleştirme
   - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) veya sonraki bir güncelleştirme

3. Yardımcı programı diskini çalışan sanal makineden ayırın ve sonra bozuk VM 'ye bağlayın.

4. Güncelleştirmeyi VM 'ye yüklemek için aşağıdaki komutu çalıştırın:

   ```
   dism /ONLINE /add-package /packagepath:<Utility Disk Letter>:\<KB .msu or .cab>
   ```

5. VM’yi yeniden başlatın.

### <a name="repair-the-vm-offline"></a>VM 'yi çevrimdışı onarma

1. [Sistem diskini bir kurtarma VM 'Sine bağlayın](./troubleshoot-recovery-disks-portal-windows.md).

2. Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın.

3. Diskin Disk Yönetimi konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun. Eklenmiş sistem diskine atanan sürücü harfini unutmayın.

4. Değişiklikler üzerinde geri almanın gerekli olması durumunda **\Windows\system32\config** klasörünün bir kopyasını oluşturun.

5. Kurtarma VM 'sinde, kayıt defteri Düzenleyicisi 'ni (regedit.exe) başlatın.

6. **HKEY_LOCAL_MACHINE** anahtarını seçin ve sonra menüden **Dosya**  >  **yükleme Hive** ' yi seçin.

7. **\Windows\system32\config** klasöründeki sistem dosyasını bulun.

8. **Aç**' ı seçin, ad Için **brokensystem** yazın, **HKEY_LOCAL_MACHINE** anahtarını genişletin ve ardından **brokensystem**adlı ek anahtarı bulun.

9. Şu konuma gidin:

   ```
   HKLM\BROKENSYSTEM\ControlSet001\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}
   ```

10. Her alt anahtarda (0000 gibi), **MICROSOFT Hyper-V ağ bağdaştırıcısı**olarak görüntülenen **DriverDesc** değerini inceleyin.

11. Alt anahtarda, sanal makinenin ağ bağdaştırıcısının sürücü sürümü olan **DriverVersion** değerini inceleyin.

12. Uygun güncelleştirmeyi indirin:

    - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562)   veya sonraki bir güncelleştirme
    - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) veya sonraki bir güncelleştirme

13. Sistem diskini, üzerinde güncelleştirmeyi indirebileceğiniz bir kurtarma sanal makinesine veri diski olarak ekleyin.

14. Güncelleştirmeyi VM 'ye yüklemek için aşağıdaki komutu çalıştırın:

    ```
    dism /image:<OS Disk letter>:\ /add-package /packagepath:c:\temp\<KB .msu or .cab>
    ```

15. Kovanları çıkarmak için aşağıdaki komutu çalıştırın:

    ```
    reg unload HKLM\BROKENSYSTEM
    ```

16. [Sistem diskini ayırın ve VM 'yi yeniden oluşturun](./troubleshoot-recovery-disks-portal-windows.md).

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa, sorununuzun hızla çözülmesini sağlamak için [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
