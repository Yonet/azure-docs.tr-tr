---
title: NFSv 4.1 varsayılan etki alanını Azure NetApp Files için yapılandırın | Microsoft Docs
description: Azure NetApp Files ile NFSv 4.1 kullanmak için NFS istemcisinin nasıl yapılandırılacağını açıklar.
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/14/2020
ms.author: b-juche
ms.openlocfilehash: c3c853190d5f63bbe9012727d8b7b7ac91da135f
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92072161"
---
# <a name="configure-nfsv41-default-domain-for-azure-netapp-files"></a>Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırma

NFSv4, bir kimlik doğrulama etki alanı kavramını tanıtır. Azure NetApp Files Şu anda hizmetten NFS istemcisine yalnızca kök Kullanıcı eşlemesini desteklemektedir. NFSv 4.1 işlevini Azure NetApp Files birlikte kullanmak için NFS istemcisini güncelleştirmeniz gerekir.

## <a name="default-behavior-of-usergroup-mapping"></a>Kullanıcı/Grup eşlemesinin varsayılan davranışı

`nobody`NFSv4 etki alanı varsayılan olarak olarak ayarlandığından kök eşleme varsayılan olarak kullanıcıya ayarlanır `localdomain` . Bir Azure NetApp Files NFSv 4.1 birimini kök olarak bağladığınızda, dosya izinlerini şöyle görürsünüz:  

![NFSv 4.1 için Kullanıcı/Grup eşlemesinin varsayılan davranışı](../media/azure-netapp-files/azure-netapp-files-nfsv41-default-behavior-user-group-mapping.png)

Yukarıdaki örnekte gösterildiği gibi, kullanıcısının `file1` olması gerekir `root` , ancak varsayılan olarak ile eşlenir `nobody` .  Bu makalede, `file1` `root` ayarını olarak değiştirerek kullanıcının nasıl ayarlanacağı gösterilmektedir `idmap Domain` `defaultv4iddomain.com` .  

## <a name="steps"></a>Adımlar 

1. `/etc/idmapd.conf`Dosyayı NFS istemcisinde düzenleyin.   
    Satırın açıklamasını kaldırın `#Domain` (yani, `#` satırdan kaldırın) ve değerini `localdomain` olarak değiştirin `defaultv4iddomain.com` . 

    İlk yapılandırma: 
    
    ![NFSv 4.1 için başlangıç yapılandırması](../media/azure-netapp-files/azure-netapp-files-nfsv41-initial-config.png)

    Yapılandırma güncelleştirildi:
    
    ![NFSv 4.1 için yapılandırma güncelleştirildi](../media/azure-netapp-files/azure-netapp-files-nfsv41-updated-config.png)

2. Bağlı olan tüm NFS birimlerinin bağlantısını çıkarın.
3. Dosyayı güncelleştirin `/etc/idmapd.conf` .
4. `rpcbind`Konakta () hizmeti yeniden başlatın `service rpcbind restart` veya yalnızca konağı yeniden başlatın.
5. NFS birimlerini gereken şekilde bağlayın.   

    Bkz. [Windows veya Linux sanal makineleri için bir birimi bağlama veya çıkarma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md). 

Aşağıdaki örnek, sonuçta elde edilen Kullanıcı/Grup değişikliğini göstermektedir: 

![Elde edilen Kullanıcı/Grup değişikliğine bir örnek gösteren ekran görüntüsü.](../media/azure-netapp-files/azure-netapp-files-nfsv41-resulting-config.png)

Örnekte gösterildiği gibi Kullanıcı/Grup artık ' dan ' a değiştirilmiştir `nobody` `root` .

## <a name="behavior-of-other-non-root-users-and-groups"></a>Diğer (kök olmayan) kullanıcıların ve grupların davranışı

Azure NetApp Files, NFSv 4.1 birimlerindeki dosyalarla veya klasörlerle ilişkili izinlere sahip olan yerel kullanıcıları (bir konakta yerel olarak oluşturulan kullanıcılar) destekler. Ancak, hizmet şu anda birden çok düğümdeki kullanıcıları/grupları eşleştirmeyi desteklemez. Bu nedenle, bir konakta oluşturulan kullanıcılar varsayılan olarak başka bir konakta oluşturulan kullanıcılara eşlenmiyor. 

Aşağıdaki örnekte, `Host1` üç mevcut test Kullanıcı hesabı ( `testuser01` , `testuser02` , `testuser03` ) vardır: 

![Konak1 'in üç mevcut test Kullanıcı hesabına sahip olduğunu gösteren ekran görüntüsü.](../media/azure-netapp-files/azure-netapp-files-nfsv41-host1-users.png)

Üzerinde `Host2` , test Kullanıcı hesaplarının oluşturulmadığını, ancak aynı birimin her iki konağa da bağlı olduğunu unutmayın:

![NFSv 4.1 için sonuç yapılandırması](../media/azure-netapp-files/azure-netapp-files-nfsv41-host2-users.png)

## <a name="next-step"></a>Sonraki adım 

[Windows veya Linux sanal makineleri için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)

