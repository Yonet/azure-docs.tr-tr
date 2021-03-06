---
title: İşletim sistemi başlatma sorunlarını giderme – Windows Update yükleme kapasitesi
description: Windows Update (KB) bir hata aldığı ve bir Azure VM 'de yanıt vermeyen sorunları gidermeye yönelik adımlar.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 6359e486-7b02-4c1e-995c-ef6d505f85f4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2020
ms.author: v-miegge
ms.openlocfilehash: 0c0ec45eee86031e1533b97ccf352de0ecf70e38
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98633163"
---
# <a name="troubleshoot-os-start-up--windows-update-installation-capacity"></a>İşletim sistemi başlatma sorunlarını giderme – Windows Update yükleme kapasitesi

Bu makalede, Windows Update (KB) bir hata aldığı ve yanıt vermeyen bir Azure sanal makinesindeki (VM) sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için önyükleme tanılamayı kullandığınızda ekran görüntüsünün devam etmekte Windows Update (KB) görüntülediğini, ancak hata kodu ile başarısız olduğunu görürsünüz: **C01A001D**. Aşağıdaki görüntüde, "Error C01A001D güncelleştirme işlemi # # # # # # # # # # (# # # # # #)" iletisiyle takılmış olan Windows Update (KB) gösterilmektedir:

![Windows Update (KB), "hata C01A001D uygulama güncelleştirme işlemi X/Y (Z)" iletisiyle takıldı.](./media/troubleshoot-windows-update-installation-capacity/1.png)

## <a name="cause"></a>Nedeni

Bu durumda, işletim sistemi (OS), dosya sisteminde çekirdek dosya oluşturuoluşturulamadığı için bir Windows Update (KB) yüklemesini tamamlayamıyor. Bu hata kodu temelinde işletim sistemi diske hiçbir dosya yazamadı.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış:

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.

1. Bir onarım VM 'si oluşturun ve erişin.
1. Diskteki boş alan.
1. Seri konsolu ve bellek dökümü toplamayı etkinleştirin.
1. VM 'yi yeniden derleyin.

> [!NOTE]
> Bu hatayla karşılaşıldığında, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda bu sorunu giderin.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım sanal makinesine bağlanın.

### <a name="free-up-space-on-the-disk"></a>Diskte yer açın

Sorunu gidermek için:

- En büyük 1 TB boyutunda değilse, diski 1 TB 'ye kadar yeniden boyutlandırın.
- Disk Temizleme işlemi gerçekleştirin.
- Sürücünün parçasını kaldır.

1. Diskin dolu olup olmadığını denetleyin. Disk boyutu 1 TB altındaysa, [PowerShell kullanarak](../windows/expand-os-disk.md)en fazla 1 TB 'a kadar genişletin.
1. Disk zaten 1 TB ise disk temizleme gerçekleştirmeniz gerekecektir.
   1. Boş alan boşaltmak için [Disk Temizleme aracını](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) kullanın.
1. Yeniden boyutlandırma ve Temizleme işlemi tamamlandıktan sonra, aşağıdaki komutu kullanarak sürücüyü serbest bırakma:

   ```
   defrag <LETTER ASSIGN TO THE OS DISK>: /u /x /g
   ```
   
Parçalama düzeyine bağlı olarak, parçalama birkaç saat sürebilir.

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Seri konsol ve bellek dökümü toplamayı etkinleştirme

**Önerilir**: VM 'yi yeniden oluşturmadan önce, aşağıdaki betiği çalıştırarak seri konsol ve bellek dökümü toplamasını etkinleştirin:

1. Yükseltilmiş bir komut istemi oturumunu yönetici olarak açın.
1. Aşağıdaki komutları çalıştırın:

   **Seri konsolunu etkinleştirin**:
   
   ```
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON 
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```

1. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutundan (RAM) daha büyük olduğunu doğrulayın.

   İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirin ve bu konumu, yeterli boş alana sahip olan VM 'ye bağlı herhangi bir veri diskine başvurun. Konumu değiştirmek için, aşağıdaki komutlarda **% systemroot%** değerini **F:** gibi veri diskinin sürücü harfiyle değiştirin.

   İşletim sistemi dökümünü etkinleştirmek için önerilen yapılandırma:

    **Bozuk işletim sistemi diskini yükleyin:**

   ```
   REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM 
   ```
   
   **ControlSet001 üzerinde etkinleştir**:

   ```
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
   ```
   
   **ControlSet002 üzerinde etkinleştir**:

   ```
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
   ```
   
   **Bozuk işletim sistemi diskini kaldır**:

   ```
   REG UNLOAD HKLM\BROKENSYSTEM
   ```
   
### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

VM 'yi yeniden derlemek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
