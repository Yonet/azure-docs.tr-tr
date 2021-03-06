---
title: Uyumsuz Azure Otomasyonu durum yapılandırma sunucularını düzelt
description: Bu makalede yapılandırma durumu düzeltebilecekler olan sunuculara istek üzerine yapılandırmaları nasıl yeniden uygulayabileceğiniz açıklanır.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: 4430b8cdfe9414ddbfd7aad3c3fe7827adbc8705
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86186376"
---
# <a name="remediate-noncompliant-azure-automation-state-configuration-servers"></a>Uyumsuz Azure Otomasyonu durum yapılandırma sunucularını düzelt

Sunucular Azure Otomasyonu durum yapılandırmasıyla kaydedildiğinde, yapılandırma modu `ApplyOnly` , veya olarak ayarlanır `ApplyandMonitor` `ApplyAndAutoCorrect` . Mod olarak ayarlanmamışsa `ApplyAndAutoCorrect` , her nedenden dolayı uyumlu bir durumdan alınan sunucular el ile düzeltilene kadar uyumsuz kalır.

Azure işlem, müşterilerin sanal makinelerde betikleri çalıştırmasına izin veren Run komutu adlı bir özellik sunar.
Bu belge, yapılandırma DRFT 'i el ile düzeltirken bu özellik için örnek betik sağlar.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>PowerShell kullanarak Windows sanal makinelerini doğru Drın

Komut özelliğini kullanarak Windows sanal makinelerini Drın 'i düzeltebilirsiniz `Run` . Bkz. [Run komutuyla PowerShell betiklerini WINDOWS sanal makinenizde çalıştırma](../virtual-machines/windows/run-command.md).

Bir Azure Otomasyonu durum yapılandırma düğümünü en son yapılandırmayı indirip uygulamayı zorlamak için [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) cmdlet 'ini kullanın.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Linux sanal makinelerini doğru DRT

Linux sanal makineleri için komutunu kullanma seçeneğiniz yoktur `Run` . Kayıt işlemini tekrarlayarak bu makineler için yalnızca DRFT 'yi düzeltebilirsiniz. 

Azure düğümleri için, Azure portal veya az Module cmdlet 'lerini kullanarak DRFT 'yi düzeltebilirsiniz. Bu işlemle ilgili ayrıntılar, [Azure Portal kullanarak VM 'Yi etkinleştirme](automation-dsc-onboarding.md#enable-a-vm-using-azure-portal)bölümünde belgelenmiştir.

Karma düğümler için Python betiklerini kullanarak DRFT 'yi düzeltebilirsiniz. Bkz. [Linux BILGISAYARıNDAN DSC Işlemlerini gerçekleştirme](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer).

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell cmdlet başvurusu için bkz. [az. Automation](/powershell/module/az.automation/?view=azps-3.7.0#automation).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Chocolatey ile sürekli dağıtımı ayarlama](automation-dsc-cd-chocolatey.md).
