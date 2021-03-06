---
title: Bir Azure VM 'yi önyüklerken mavi ekran hataları | Microsoft Docs
description: Önyükleme sırasında mavi ekran hatasının alındığı sorunu nasıl giderebileceğinizi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/28/2018
ms.author: genli
ms.openlocfilehash: 9a95ddf882e5edba9daa8ff91c02d1df1f50bceb
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632985"
---
# <a name="windows-shows-blue-screen-error-when-booting-an-azure-vm"></a>Windows, bir Azure VM 'yi önyüklerken mavi ekran hatasını gösterir
Bu makalede, Microsoft Azure ' de bir Windows sanal makinesini (VM) önyüklediğinizde karşılaşabileceğiniz mavi ekran hataları açıklanır. Destek bileti için veri toplamanıza yardımcı olacak adımları sağlar. 


## <a name="symptom"></a>Belirti 

Bir Windows VM 'si başlamıyor. Önyükleme [tanılamalarında](./boot-diagnostics.md)önyükleme ekran görüntülerini denetlediğinizde, mavi ekranda aşağıdaki hata iletilerinden birini görürsünüz:

- BILGISAYARıMDA bir sorun oluştu ve yeniden başlatılması gerekiyor. Yalnızca bazı hata bilgilerini topluyoruz ve sonra yeniden başlatabilirsiniz.
- BILGISAYARıNıZ bir sorunla karşılaştı ve yeniden başlatılması gerekiyor.

Bu bölümde, VM 'Leri yönetirken karşılaşabileceğiniz yaygın hata iletileri listelenmektedir:

## <a name="cause"></a>Nedeni

Neden bir durma hatası edinmenizin birden çok nedeni olabilir. En yaygın nedenler şunlardır:

- Sürücü ile ilgili sorun
- Bozuk sistem dosyası veya belleği
- Uygulama, belleğin yasak bir sektörüne erişir

## <a name="collect-memory-dump-file"></a>Bellek dökümü dosyası topla

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.

Bu sorunu çözmek için öncelikle kilitlenme için döküm dosyası toplamanız ve döküm dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM 'sine iliştirme

1. Etkilenen VM 'nin işletim sistemi diskinin anlık görüntüsünü bir yedekleme olarak alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).
2. [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](./troubleshoot-recovery-disks-portal-windows.md). 
3. Kurtarma sanal makinesine uzak masaüstü.

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bul ve bir destek bileti gönder

1. Kurtarma VM 'sinde, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\windowsadresine gitmeniz gerekir.
2. Memory. dmp dosyasını bulun ve ardından döküm dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) . 

Döküm dosyasını bulamıyorsanız, döküm günlüğünü ve seri konsolunu etkinleştirmek için sonraki adımı taşıyın.

### <a name="enable-dump-log-and-serial-console"></a>Döküm günlüğünü ve seri konsolunu etkinleştir

Döküm günlüğünü ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın.

1. Yükseltilmiş komut Istemi oturumunu açın (yönetici olarak çalıştır).
2. Şu betiği çalıştırın:

    Bu betikte, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık.  Bunu sanal makinenizde uygun değerle değiştirin.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

    1. Bu VM için seçtiğiniz boyuta bağlı olarak RAM 'e kadar bellek ayırmak için diskte yeterli alan olduğundan emin olun.
    2. Yeterli alan yoksa veya bu büyük boyutlu bir VM (G, GS veya E serisi) ise, bu dosyanın oluşturulacağı konumu değiştirebilir ve VM 'ye bağlı olan diğer tüm veri diskine başvurabilirsiniz. Bunu yapmak için aşağıdaki anahtarı değiştirmeniz gerekir:

    ```config-reg
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

    reg unload HKLM\BROKENSYSTEM
    ```

3. [İşletim sistemi diskini ayırın ve ardından işletim sistemi diskini ETKILENEN VM 'ye yeniden ekleyin](./troubleshoot-recovery-disks-portal-windows.md).
4. Sorunu yeniden oluşturmak için VM 'yi başlatın, ardından bir döküm dosyası oluşturulur.
5. İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin, döküm dosyasını toplayın ve ardından döküm dosyası ile [bir destek bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) iletin.
