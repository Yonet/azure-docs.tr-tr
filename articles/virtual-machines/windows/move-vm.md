---
title: Azure 'da bir Windows VM kaynağını taşıma
description: Bir Windows sanal makinesini Kaynak Yöneticisi dağıtım modelindeki başka bir Azure aboneliğine veya kaynak grubuna taşıyın.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/03/2019
ms.author: cynthn
ms.openlocfilehash: 1f919a4af85a15bbe80d7176c316100c3bad634a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86998927"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Bir Windows sanal makinesini başka bir Azure aboneliğine veya kaynak grubuna taşıma
Bu makalede, bir Windows sanal makinesini (VM) kaynak grupları veya abonelikler arasında taşıma işlemi adım adım gösterilmektedir. Bir kişisel abonelikte ilk olarak bir VM oluşturduysanız ve bundan sonra işinize devam etmek için bunu şirketinizin aboneliğine taşımak istiyorsanız abonelikler arasında geçiş yapmak yararlı olabilir. Sanal makineyi taşımak için durdurmanız gerekmez ve taşıma sırasında çalışmaya devam etmelidir.

> [!IMPORTANT]
>Yeni kaynak kimlikleri taşımanın bir parçası olarak oluşturulur. VM taşındıktan sonra, yeni kaynak kimliklerini kullanmak için araçlarınızı ve betikleri güncelleştirmeniz gerekecektir.
>
>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>VM 'yi taşımak için PowerShell 'i kullanma

Bir sanal makineyi başka bir kaynak grubuna taşımak için tüm bağımlı kaynakları da taşıdığınızdan emin olmanız gerekir. Bu kaynakların her birine ait kaynak KIMLIĞI ile bir liste almak için [Get-AzResource](/powershell/module/az.resources/get-azresource) cmdlet 'ini kullanın.

```azurepowershell-interactive
 Get-AzResource -ResourceGroupName myResourceGroup | Format-table -wrap -Property ResourceId
```

Her bir kaynağı hedefe taşımak için [Move-AzResource](/powershell/module/az.resources/move-azresource) Için kaynak kimliklerinin virgülle ayrılmış bir listesini oluşturmak üzere önceki komutun çıkışını kullanabilirsiniz.

```azurepowershell-interactive
Move-AzResource -DestinationResourceGroupName "myDestinationResourceGroup" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```

Kaynakları farklı aboneliğe taşımak için **-destinationsubscriptionıd** parametresini ekleyin.

```azurepowershell-interactive
Move-AzResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Belirtilen kaynakları taşımak istediğinizi onaylamanız istendiğinde, onaylamak için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
Kaynak grupları ve abonelikler arasında birçok farklı kaynak türünü taşıyabilirsiniz. Daha fazla bilgi için bkz. [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md).    
