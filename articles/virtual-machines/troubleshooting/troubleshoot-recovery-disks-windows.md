---
title: Azure PowerShell ile bir Windows sorun giderme sanal makinesi kullanma | Microsoft Docs
description: Azure PowerShell kullanarak işletim sistemi diskini bir kurtarma VM 'sine bağlayarak Azure 'da Windows VM sorunlarını nasıl giderebileceğinizi öğrenin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: fbdccc2efb72626d1974f4cd81be9d2ef4c522d4
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201595"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Azure PowerShell kullanarak işletim sistemi diskini bir kurtarma VM 'sine ekleyerek bir Windows sanal makinesi sorunlarını giderme
Azure 'daki Windows sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Ortak bir örnek, VM 'nin başarıyla önyükleme yapabilmesini önleyen başarısız bir uygulama güncelleştirmesidir. Bu makalede, tüm hataları gidermek için diski başka bir Windows VM 'ye bağlamak üzere Azure PowerShell kullanma ve ardından özgün VM 'nizi onarma işlemlerinin nasıl yapılacağı açıklanır. 

> [!Important]
> Bu makaledeki betikler yalnızca [yönetilen disk](../managed-disks-overview.md)kullanan VM 'ler için geçerlidir. 

 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Artık, bir VM 'nin işletim sistemi diskini değiştirmek için Azure PowerShell kullanabilirsiniz. Artık VM 'yi silip yeniden oluşturma gereksinimi yoktur.

Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM 'yi durdurun.
2. VM 'nin işletim sistemi diskinden bir anlık görüntü oluşturun.
3. İşletim sistemi diski anlık görüntüsünden bir disk oluşturun.
4. Diski bir kurtarma VM 'sine veri diski olarak ekleyin.
5. Kurtarma sanal makinesine bağlanın. Kopyalanan işletim sistemi diskindeki sorunları gidermek için dosyaları düzenleyin veya herhangi bir araç çalıştırın.
6. Diski Kurtarma VM 'sinden çıkarın ve ayırın.
7. Etkilenen VM için işletim sistemi diskini değiştirin.

VM onarım komutlarını, 1, 2, 3, 4, 6 ve 7. adımları otomatik hale getirmek için kullanabilirsiniz. Daha fazla belge ve yönergeler için bkz. [Azure sanal makine onarım komutlarını kullanarak Windows VM 'Yi onarma](repair-windows-vm-using-azure-virtual-machine-repair-commands.md).

[En son Azure PowerShell](/powershell/azure/) yüklü olduğundan ve aboneliğinizde oturum açtığınızdan emin olun:

```powershell
Connect-AzAccount
```

Aşağıdaki örneklerde parametre adlarını kendi değerlerinizle değiştirin. 

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Önyükleme sorunlarını gidermenize yardımcı olması için, Azure 'da sanal makinenizin ekran görüntüsünü görüntüleyebilirsiniz. Bu ekran görüntüsü, bir VM 'nin neden önyükleme başarısız olduğunu belirlemenize yardımcı olabilir. Aşağıdaki örnek, adlı kaynak grubunda adlı Windows sanal makinesinin ekran görüntüsünü alır `myVM` `myResourceGroup` :

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

VM 'nin neden önyükleme başarısız olduğunu anlamak için ekran görüntüsünü gözden geçirin. Belirtilen tüm hata iletilerini veya hata kodlarını dikkate alın.

## <a name="stop-the-vm"></a>VM’yi durdurma

Aşağıdaki örnek, adlı kaynak grubundan adlı VM 'yi durduruyor `myVM` `myResourceGroup` :

```powershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Sonraki adıma geçmeden önce VM 'nin silme işlemini tamamlamasını bekleyin.


## <a name="create-a-snapshot-from-the-os-disk-of-the-vm"></a>VM 'nin işletim sistemi diskinden anlık görüntü oluşturma

Aşağıdaki örnek, `mySnapshot` ' myVM ' ADLı VM 'nin işletim sistemi diskinden adında bir anlık görüntü oluşturur. 

```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  

#Get the VM
$vm = get-azvm `
-ResourceGroupName $resourceGroupName `
-Name $vmName

#Create the snapshot configuration for the OS disk
$snapshot =  New-AzSnapshotConfig `
-SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
-Location $location `
-CreateOption copy

#Take the snapshot
New-AzSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```

Anlık görüntü, bir VHD 'nin tam ve salt okunurdur kopyasıdır. Bir sanal makineye bağlanamaz. Bir sonraki adımda, bu anlık görüntüden bir disk oluşturacağız.

## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma

Bu betik adlı anlık görüntüden adında bir yönetilen disk oluşturur `newOSDisk` `mysnapshot` .  

```powershell
#Set the context to the subscription Id where Managed Disk will be created
#You can skip this step if the subscription is already selected

$subscriptionId = 'yourSubscriptionId'

Select-AzSubscription -SubscriptionId $SubscriptionId

#Provide the name of your resource group
$resourceGroupName ='myResourceGroup'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'mySnapshot' 

#Provide the name of the Managed Disk
$diskName = 'newOSDisk'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
$storageType = 'Standard_LRS'

#Provide the Azure region (e.g. westus) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'eastus'

$snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
 
$diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
 
New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
```
Artık özgün işletim sistemi diskinin bir kopyasına sahipsiniz. Sorun giderme amacıyla bu diski başka bir Windows sanal makinesine bağlayabilirsiniz.

## <a name="attach-the-disk-to-another-windows-vm-for-troubleshooting"></a>Sorunu gidermek için diski başka bir Windows VM 'ye iliştirme

Şimdi özgün işletim sistemi diskinin kopyasını bir veri diski olarak bir VM 'ye ekleyeceğiz. Bu işlem, yapılandırma hatalarını düzeltmenize veya diskteki ek uygulama ya da sistem günlük dosyalarını incelemenizi sağlar. Aşağıdaki örnek adlı sanal makineye adlı diski iliştirir `newOSDisk` `RecoveryVM` .

> [!NOTE]
> Diski eklemek için, özgün işletim sistemi diskinin ve kurtarma sanal makinesinin kopyasının aynı konumda olması gerekir.

```powershell
$rgName = "myResourceGroup"
$vmName = "RecoveryVM"
$location = "eastus" 
$dataDiskName = "newOSDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="connect-to-the-recovery-vm-and-fix-issues-on-the-attached-disk"></a>Kurtarma VM 'sine bağlanma ve ekli diskteki sorunları çözme

1. Uygun kimlik bilgilerini kullanarak kurtarma VM 'nize RDP. Aşağıdaki örnek, adlı kaynak grubunda adlı VM için RDP bağlantı dosyasını indirir `RecoveryVM` `myResourceGroup` ve `C:\Users\ops\Documents` "

    ```powershell
    Get-AzRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "RecoveryVM" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Veri diski otomatik olarak algılanmalı ve bağlanmalıdır. Aşağıdaki gibi, sürücü harfini öğrenmek için eklenen birimlerin listesini görüntüleyin:

    ```powershell
    Get-Disk
    ```

    Aşağıdaki örnek çıktıda disk **2** diske bağlı disk gösterilmektedir. ( `Get-Volume` Sürücü harfini görüntülemek için de kullanabilirsiniz):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        newOSDisk                                  Healthy             Online       127 GB MBR
    ```

Özgün işletim sistemi diskinin kopyası bağlandıktan sonra, gereken tüm bakım ve sorun giderme adımlarını gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-original-os-disk"></a>Özgün işletim sistemi diskinin bağlantısını kaldır ve ayır
Hatalarınız çözümlendikten sonra, var olan diski kurtarma sanal makinenizin bağlantısını çıkarırın. Diski Kurtarma VM 'sine ekleme kiralamasının serbest bırakılması bitinceye kadar diskinizi başka bir VM ile kullanamazsınız.

1. RDP oturumunuzla, kurtarma sanal makinenizin veri diskini çıkarın. Önceki cmdlet 'ten disk numarası gereklidir `Get-Disk` . Ardından, `Set-Disk` diski çevrimdışı olarak ayarlamak için kullanın:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Diskin artık yeniden çevrimdışı olarak ayarlandığını onaylayın `Get-Disk` . Aşağıdaki örnek çıktı, diskin artık çevrimdışı olarak ayarlandığını göstermektedir:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. RDP oturumunuzla çıkış yapın. Azure PowerShell oturumunuzda, `newOSDisk` ' RecoveryVM ' ADLı VM 'den adlı diski kaldırın.

    ```powershell
    $myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "RecoveryVM"
    Remove-AzVMDataDisk -VM $myVM -Name "newOSDisk"
    Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Etkilenen VM için işletim sistemi diskini değiştirme

İşletim sistemi disklerini değiştirmek için Azure PowerShell kullanabilirsiniz. VM 'yi silip yeniden oluşturmanız gerekmez.

Bu örnek, adlı VM 'yi durdurup `myVM` `newOSDisk` yeni işletim sistemi diski olarak adlandırılan diski atar. 

```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name  -sto

# Update the VM with the new OS disk. Possible values of StorageAccountType include: 'Standard_LRS' and 'Premium_LRS'
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -StorageAccountType <Type of the storage account >

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```

## <a name="verify-and-enable-boot-diagnostics"></a>Önyükleme tanılamayı doğrulama ve etkinleştirme

Aşağıdaki örnek, adlı kaynak grubunda adlı sanal makinede tanılama uzantısını sunar `myVMDeployed` `myResourceGroup` :

```powershell
$myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM 'ye YÖNELIK RDP bağlantılarında sorun giderme](troubleshoot-rdp-connection.md). VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [WINDOWS VM 'de uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

Kaynak Yöneticisi kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../../azure-resource-manager/management/overview.md).
