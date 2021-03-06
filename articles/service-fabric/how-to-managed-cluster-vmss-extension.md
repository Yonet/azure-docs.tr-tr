---
title: Service Fabric yönetilen küme düğümü türüne (Önizleme) bir sanal makine ölçek kümesi uzantısı ekleyin
description: Service Fabric yönetilen küme düğümü türü bir sanal makine ölçek kümesi uzantısı ekleme
ms.topic: article
ms.date: 09/28/2020
ms.openlocfilehash: 64df4b82795f382e176d66dc61470296447b9e29
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98788087"
---
# <a name="add-a-virtual-machine-scale-set-extension-to-a-service-fabric-managed-cluster-node-type-preview"></a>Service Fabric yönetilen küme düğümü türüne (Önizleme) bir sanal makine ölçek kümesi uzantısı ekleyin

Service Fabric yönetilen bir kümedeki her düğüm türü bir sanal makine ölçek kümesi tarafından desteklenir. Bu, Service Fabric yönetilen küme düğümü türlerine [sanal makine ölçek kümesi uzantıları](../virtual-machines/extensions/overview.md) eklemenize olanak sağlar.

[Add-Azservicefabricmanagednodetypevmexgerpowershell](/powershell/module/az.servicefabric/add-azservicefabricmanagednodetypevmextension) komutunu kullanarak düğüm türüne bir sanal makine ölçek kümesi uzantısı ekleyebilirsiniz.

Alternatif olarak, Azure Resource Manager şablonunuzda Service Fabric yönetilen küme düğümü türünde bir sanal makine ölçek kümesi uzantısı da oluşturabilirsiniz, örneğin:

```json
{
    "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
    "apiVersion": "[variables('sfApiVersion')]",
    "name": "[concat(parameters('clusterName'), '/', parameters('nodeTypeName'))]",
    "dependsOn": [
        "[concat('Microsoft.ServiceFabric/managedclusters/', parameters('clusterName'))]"
    ],
    "location": "[resourceGroup().location]",
    "properties": {
        "isPrimary": true,
        "vmInstanceCount": 3,
        "dataDiskSizeGB": 100,
        "vmSize": "Standard_D2",
        "vmImagePublisher": "MicrosoftWindowsServer",
        "vmImageOffer": "WindowsServer",
        "vmImageSku": "2019-Datacenter",
        "vmImageVersion": "latest",
        "vmExtensions": [{
            "name": "ExtensionA",
            "properties": {
                "publisher": "ExtensionA.Publisher",
                "type": "KeyVaultForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                }
            }
        }]
    }
}
```

Service Fabric yönetilen küme düğümü türlerini yapılandırma hakkında daha fazla bilgi için bkz. [yönetilen küme düğümü türü](/azure/templates/microsoft.servicefabric/2020-01-01-preview/managedclusters/nodetypes).

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric yönetilen kümeler hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Service Fabric yönetilen kümeler hakkında sık sorulan sorular](./faq-managed-cluster.md)
