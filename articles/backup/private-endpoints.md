---
title: Özel Uç Noktalar
description: Azure Backup için özel uç noktalar oluşturma sürecini anlayın ve özel uç noktaları kullanmanın kaynaklarınızın güvenliğini sağlamaya yardımcı olur.
ms.topic: conceptual
ms.date: 05/07/2020
ms.openlocfilehash: 0d9d77c139896f9067f73943dbb213fc655f00f6
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99054881"
---
# <a name="private-endpoints-for-azure-backup"></a>Azure Backup için özel uç noktalar

Azure Backup, [Özel uç noktaları](../private-link/private-endpoint-overview.md)kullanarak kurtarma hizmetleri kasalarınızın verilerinizi güvenli bir şekilde yedekleyeve geri yüklemenize olanak tanır. Özel uç noktalar sanal ağınızdan bir veya daha fazla özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor.

Bu makale, Azure Backup için özel uç noktalar oluşturma sürecini anlamanıza yardımcı olur ve özel uç noktalar kullanmanın kaynaklarınızın güvenliğinin sağlanmasına yardımcı olur.

## <a name="before-you-start"></a>Başlamadan önce

- Özel uç noktalar yalnızca yeni kurtarma hizmetleri kasaları için oluşturulabilir (kasaya kayıtlı herhangi bir öğe yok). Bu nedenle, herhangi bir öğeyi kasaya korumayı denemeden önce özel uç noktaların oluşturulması gerekir.
- Bir sanal ağ, birden çok kurtarma hizmetleri Kasası için özel uç noktalar içerebilir. Ayrıca, bir kurtarma hizmetleri kasasında birden çok sanal ağda özel uç noktalar bulunabilir. Ancak, bir kasa için oluşturulabilecek en fazla özel uç nokta sayısı 12 ' dir.
- Kasa için özel bir uç nokta oluşturulduktan sonra kasa kilitlenir. Kasaların özel bir uç noktasını içeren ağlardan ayrı olarak erişilebilir olmayacaktır (yedeklemeler ve geri yüklemeler için). Kasa için tüm özel uç noktalar kaldırılırsa, kasaya tüm ağlardan erişilebilecektir.
- Yedekleme için özel bir uç nokta bağlantısı, depolama için Azure Backup tarafından kullanılanlarla birlikte alt ağınızdaki toplam 11 özel IP 'yi kullanır. Bu sayı, belirli Azure bölgeleri için daha yüksek (25 ' e kadar) olabilir. Bu nedenle, yedekleme için özel uç noktalar oluşturmaya çalıştığınızda yeterli sayıda özel IP 'nin kullanılabilir olmasını öneririz.
- Bir kurtarma hizmetleri Kasası (her ikisi de) Azure Backup ve Azure Site Recovery tarafından kullanıldığında, bu makalede yalnızca Azure Backup için özel uç noktaların kullanımı ele alınmaktadır.
- Azure Active Directory şu anda özel uç noktaları desteklemez. Bu nedenle Azure Active Directory bir bölgede çalışması için gereken IP 'Leri ve FQDN 'lerin, Azure VM 'lerinde veritabanlarının yedeklenmesi sırasında ve MARS Aracısı kullanılarak yedeklendiğinden güvenli ağdan giden erişime izin verilmesi gerekir. Ayrıca, geçerli olduğu şekilde Azure AD 'ye erişim izni vermek için NSG etiketlerini ve Azure Güvenlik Duvarı etiketlerini de kullanabilirsiniz.
- Ağ Ilkelerine sahip sanal ağlar özel uç noktalar için desteklenmez. Devam etmeden önce ağ Ilkelerini devre dışı bırakmanız gerekir.
- Kurtarma Hizmetleri kaynak sağlayıcısını, aboneliği 1 2020 Mayıs tarihinden önce kaydettirdiğiniz takdirde yeniden kaydetmeniz gerekir. Sağlayıcıyı yeniden kaydetmek için Azure portal aboneliğinize gidin, sol gezinti çubuğunda **kaynak sağlayıcısı** ' na gidin, ardından **Microsoft. recoveryservices** ' i seçin ve **yeniden kaydet**' i seçin.
- Kasa için özel uç noktalar etkinse SQL ve SAP HANA veritabanı yedeklemeleri için [çapraz bölge geri yükleme](backup-create-rs-vault.md#set-cross-region-restore) desteklenmez.

## <a name="recommended-and-supported-scenarios"></a>Önerilen ve desteklenen senaryolar

Kasa için özel uç noktalar etkinleştirildiğinden, bunlar SQL ve SAP HANA iş yüklerini yalnızca bir Azure VM ve MARS aracı yedeklemesi için yedekleme ve geri yükleme için kullanılır. Diğer iş yüklerinin yedeklenmesi için kasayı da kullanabilirsiniz (ancak özel uç noktalar gerektirmez). MARS Aracısı kullanılarak SQL ve SAP HANA iş yüklerinin ve yedeklemenin yedeğinin yanı sıra, Azure VM yedeklemesi için dosya kurtarma gerçekleştirmek üzere özel uç noktalar da kullanılır. Daha fazla bilgi için aşağıdaki tabloya bakın:

| Azure VM 'de iş yüklerini yedekleme (SQL, SAP HANA), MARS Aracısı kullanarak yedekleme | Yedekleme ve geri yüklemeye izin vermek için özel uç noktaların kullanımı, sanal ağlarınızdan Azure Backup veya Azure depolama için herhangi bir IP/FQDN 'nin, izin kullanılmasına gerek kalmadan önerilir. Bu senaryoda, SQL veritabanlarını barındıran VM 'Lerin Azure AD IP 'lerine veya FQDN 'Lere erişebildiğinden emin olun. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Azure VM yedeklemesi**                                         | VM yedeklemesi herhangi bir IP veya FQDN 'ye erişim izni vermeyi gerektirmez. Bu nedenle, disklerin yedeklenmesi ve geri yüklenmesi için özel uç noktalar gerektirmez.  <br><br>   Ancak, Özel uç noktaları içeren bir kasadan dosya kurtarma, kasa için özel bir uç nokta içeren sanal ağlarla kısıtlıdır. <br><br>    Acled yönetilmeyen diskler kullanılırken, diskleri içeren depolama hesabının, **güvenilir Microsoft hizmetlerine** erişim izni verdiğinden emin olun. |
| **Azure dosyaları yedeklemesi**                                      | Azure dosyaları yedeklemeleri yerel depolama hesabında depolanır. Bu nedenle, yedekleme ve geri yükleme için özel uç noktalar gerektirmez. |

## <a name="creating-and-using-private-endpoints-for-backup"></a>Yedekleme için özel uç noktalar oluşturma ve kullanma

Bu bölüm, sanal ağlarınızdaki Azure Backup için özel uç noktalar oluşturma ve kullanma ile ilgili adımlar hakkında bilgi alır.

>[!IMPORTANT]
> Bu belgede belirtilen sırada bulunan adımları izlemeniz önemle tavsiye edilir. Bunun başarısız olması, Özel uç noktaları kullanmak ve işlemi yeni bir kasa ile yeniden başlatmanızı gerektiren kasalardan dolayı uyumsuz olarak işlenmesine yol açabilir.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

Azure Resource Manager istemcisini kullanarak kasa oluşturmayı öğrenmek için [Bu bölüme](#create-a-recovery-services-vault-using-the-azure-resource-manager-client) bakın. Bu, yönetilen kimliği zaten etkinleştirilmiş bir kasa oluşturur. Kurtarma Hizmetleri kasaları hakkında daha fazla bilgi [edinin.](./backup-azure-recovery-services-vault-overview.md)

## <a name="enable-managed-identity-for-your-vault"></a>Kasanız için yönetilen kimliği etkinleştirin

Yönetilen kimlikler, kasasının özel uç noktalar oluşturmasına ve kullanmasına izin verir. Bu bölümde, kasalarınız için yönetilen kimliğin etkinleştirilmesi hakkında bilgi yer aldığı bir konuşuyor.

1. Kurtarma Hizmetleri kasanıza > **kimliğiniz**' ne gidin.

    ![Kimlik durumunu açık olarak değiştir](./media/private-endpoints/identity-status-on.png)

1. **Durumu** **Açık** olarak değiştirin ve **Kaydet**' i seçin.

1. Kasanın yönetilen kimliği olan bir **nesne kimliği** oluşturulur.

    >[!NOTE]
    >Etkinleştirildikten sonra, yönetilen kimlik devre dışı **bırakılmamalıdır (** geçici olarak bile). Yönetilen kimliğin devre dışı bırakılması tutarsız davranışa yol açabilir.

## <a name="grant-permissions-to-the-vault-to-create-required-private-endpoints"></a>Gerekli özel uç noktaları oluşturmak için kasaya izin verin

Azure Backup için gerekli özel uç noktaları oluşturmak için kasasının (kasanın yönetilen kimliği) aşağıdaki kaynak grupları için izinlere sahip olması gerekir:

- Hedef VNet 'i içeren kaynak grubu
- Özel uç noktaların oluşturulacağı kaynak grubu
- [Burada](#creating-private-endpoints-for-backup) ayrıntılı olarak anlatıldığı gibi özel DNS bölgelerini Içeren kaynak grubu

Bu üç kaynak grubu için **katılımcı** rolünü kasaya vermenizi öneririz (yönetilen kimlik). Aşağıdaki adımlarda, bunun belirli bir kaynak grubu için nasıl yapılacağı açıklanır (Bu, üç kaynak grubunun her biri için yapılması gerekir):

1. Kaynak grubuna gidin ve sol çubukta **Access Control (IAM)** bölümüne gidin.
1. **Access Control**' de, **rol ataması Ekle**' ye gidin.

    ![Rol ataması ekleyin](./media/private-endpoints/add-role-assignment.png)

1. **Rol ataması Ekle** bölmesinde, **rol** olarak **katkıda bulunan** ' ı seçin ve kasasının **adını** **sorumlu** olarak kullanın. Kasanızı seçin ve işiniz bittiğinde **Kaydet** ' i seçin.

    ![Rol ve asıl seçin](./media/private-endpoints/choose-role-and-principal.png)

İzinleri daha ayrıntılı bir düzeyde yönetmek için bkz. [rolleri ve izinleri el Ile oluşturma](#create-roles-and-permissions-manually).

## <a name="creating-and-approving-private-endpoints-for-azure-backup"></a>Azure Backup için özel uç noktalar oluşturma ve onaylama

### <a name="creating-private-endpoints-for-backup"></a>Yedekleme için özel uç noktalar oluşturma

Bu bölümde, kasanız için özel bir uç nokta oluşturma işlemi açıklanmaktadır.

1. Arama çubuğunda **özel bağlantı**' yı arayıp seçin. Bu sizi **özel bağlantı merkezine** götürür.

    ![Özel bağlantı ara](./media/private-endpoints/search-for-private-link.png)

1. Sol gezinti çubuğunda **Özel uç noktalar**' ı seçin. **Özel uç noktalar** bölmesinde bir kez **+ Ekle** ' yi seçerek kasanız Için özel bir uç nokta oluşturmaya başlayın.

    ![Özel bağlantı merkezinde özel uç nokta Ekle](./media/private-endpoints/add-private-endpoint.png)

1. **Özel uç nokta oluşturma** işleminde bir kez, Özel uç nokta bağlantınızın oluşturulması için ayrıntıları belirtmeniz gerekir.

    1. **Temel bilgiler**: özel uç noktalarınız için temel ayrıntıları girin. Bölge, kasala ve kaynakla aynı olmalıdır.

        ![Temel ayrıntıları doldur](./media/private-endpoints/basic-details.png)

    1. **Kaynak**: Bu sekme, bağlantınızı oluşturmak Istediğiniz PaaS kaynağını bahsetmeyi gerektirir. İstediğiniz abonelik için kaynak türünden **Microsoft. RecoveryServices/kasaults** ' ı seçin. İşiniz bittiğinde, **kaynak** olarak kurtarma hizmetleri kasasının adını seçin ve **hedef alt kaynak** olarak **AzureBackup** .

        ![Kaynak sekmesini doldur](./media/private-endpoints/resource-tab.png)

    1. **Yapılandırma**: yapılandırma bölümünde, Özel uç noktanın oluşturulmasını istediğiniz sanal ağı ve alt ağı belirtin. Bu, VM 'nin bulunduğu sanal ağ olacaktır. Özel **uç noktanızı** özel bir DNS bölgesiyle tümleştirmeyi tercih edebilirsiniz. Alternatif olarak, özel DNS sunucunuzu da kullanabilir veya özel bir DNS bölgesi oluşturabilirsiniz.

        ![Yapılandırma sekmesini doldur](./media/private-endpoints/configuration-tab.png)

        Azure Özel DNS bölgeleriyle tümleştirme yerine özel DNS sunucularınızı kullanmak istiyorsanız [Bu bölüme](#dns-changes-for-custom-dns-servers) bakın.  

    1. İsteğe bağlı olarak, Özel uç noktanız için **Etiketler** ekleyebilirsiniz.

    1. Ayrıntıları girerken ve bir kez **oluşturma** işlemi tamamlandıktan sonra devam edin. Doğrulama tamamlandığında, Özel uç noktayı oluşturmak için **Oluştur** ' u seçin.

## <a name="approving-private-endpoints"></a>Özel uç noktaları onaylama

Özel uç noktayı oluşturan kullanıcı aynı zamanda kurtarma hizmetleri kasasının sahibiyseniz, yukarıda oluşturulan özel uç nokta otomatik olarak onaylanır. Aksi takdirde, kasanın sahibi, kullanmadan önce özel uç noktasını onaylamalıdır. Bu bölümde Azure portal aracılığıyla özel uç noktaların el ile onaylanması ele alınmaktadır.

Özel uç noktaları onaylamak için Azure Resource Manager istemcisini kullanmak üzere [Azure Resource Manager istemcisini kullanarak özel uç noktaların el ile onaylanmasını](#manual-approval-of-private-endpoints-using-the-azure-resource-manager-client) inceleyin.

1. Kurtarma Hizmetleri kasanızda, sol taraftaki çubukta **Özel uç nokta bağlantıları** ' na gidin.
1. Onaylamak istediğiniz özel uç nokta bağlantısını seçin.
1. Üstteki çubukta **Onayla** ' yı seçin. Ayrıca, uç nokta bağlantısını reddetmek veya silmek istiyorsanız **Reddet** veya **Kaldır** ' ı seçebilirsiniz.

    ![Özel uç noktaları Onayla](./media/private-endpoints/approve-private-endpoints.png)

## <a name="using-private-endpoints-for-backup"></a>Yedekleme için özel uç noktaları kullanma

VNet 'iniz için oluşturulan özel uç noktalar onaylandığında, yedeklemelerinizi gerçekleştirmek ve geri yüklemek için bunları kullanmaya başlayabilirsiniz.

>[!IMPORTANT]
>Devam etmeden önce belgede belirtilen tüm adımları başarıyla tamamladığınızdan emin olun. Bu durumda, aşağıdaki denetim listesindeki adımları tamamlamış olmanız gerekir:
>
>1. (Yeni) bir kurtarma hizmetleri Kasası oluşturuldu
>1. Sistem tarafından atanan yönetilen kimliği kullanmak için kasa etkinleştirildi
>1. Kasanın yönetilen kimliğine ilgili izinler atandı
>1. Kasanız için özel bir uç nokta oluşturuldu
>1. Özel uç nokta onaylandı (otomatik onaylanmamışsa)

### <a name="backup-and-restore-of-workloads-in-azure-vm-sql-sap-hana"></a>Azure VM 'de iş yüklerini yedekleme ve geri yükleme (SQL, SAP HANA)

Özel uç nokta oluşturulup onaylandıktan sonra, istemci tarafında özel uç noktayı kullanmak için ek değişiklik yapmanız gerekmez. Güvenli ağınızdan kasaya tüm iletişim ve veri aktarımı özel uç nokta aracılığıyla gerçekleştirilir.
Ancak, bir sunucu (SQL/SAP HANA) bu sunucuya kaydedildikten sonra kasa için özel uç noktaları kaldırırsanız, kapsayıcıyı kasaya yeniden kaydetmeniz gerekir. Bunların korumasını durdurmanız gerekmez.

### <a name="backup-and-restore-through-mars-agent"></a>MARS Aracısı aracılığıyla yedekleme ve geri yükleme

Şirket içi kaynaklarınızı yedeklemek için MARS Aracısı kullanılırken, şirket içi ağınızın (yedeklenecek kaynakları içeren), kasa için özel bir uç nokta içeren Azure VNet ile eşlendiğinden emin olun. Daha sonra MARS aracısını yüklemeye devam edebilir ve yedeklemeyi burada açıklandığı gibi yapılandırabilirsiniz. Ancak, yedekleme için tüm iletişimin yalnızca eşlenmiş ağ aracılığıyla yapıldığından emin olmanız gerekir.

Ancak, bir MARS Aracısı kaydedildikten sonra kasa için özel uç noktaları kaldırırsanız, kapsayıcıyı kasaya yeniden kaydetmeniz gerekir. Bunların korumasını durdurmanız gerekmez.

## <a name="additional-topics"></a>Ek konu başlıkları

### <a name="create-a-recovery-services-vault-using-the-azure-resource-manager-client"></a>Azure Resource Manager istemcisini kullanarak bir kurtarma hizmetleri Kasası oluşturma

Kurtarma Hizmetleri kasasını oluşturabilir ve yönetilen kimliğini etkinleştirebilirsiniz (daha sonra, Azure Resource Manager istemcisini kullanarak daha sonra göreceğiniz şekilde, yönetilen kimliğin etkinleştirilmesi gerekir). Bunu yapmanın bir örneği aşağıda paylaşılır:

```rest
armclient PUT /subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>?api-version=2017-07-01-preview @C:\<filepath>\MSIVault.json
```

Yukarıdaki JSON dosyası aşağıdaki içeriğe sahip olmalıdır:

JSON iste:

```json
{
  "location": "eastus2",
  "name": "<vaultname>",
  "etag": "W/\"datetime'2019-05-24T12%3A54%3A42.1757237Z'\"",
  "tags": {
    "PutKey": "PutValue"
  },
  "properties": {},
  "id": "/subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>",
  "type": "Microsoft.RecoveryServices/Vaults",
  "sku": {
    "name": "RS0",
    "tier": "Standard"
  },
  "identity": {
    "type": "systemassigned"
  }
}
```

Yanıt JSON 'SI:

```json
{
   "location": "eastus2",
   "name": "<vaultname>",
   "etag": "W/\"datetime'2020-02-25T05%3A26%3A58.5181122Z'\"",
   "tags": {
     "PutKey": "PutValue"
   },
   "identity": {
     "tenantId": "<tenantid>",
     "principalId": "<principalid>",
     "type": "SystemAssigned"
   },
   "properties": {
     "provisioningState": "Succeeded",
     "privateEndpointStateForBackup": "None",
     "privateEndpointStateForSiteRecovery": "None"
   },
   "id": "/subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>",
   "type": "Microsoft.RecoveryServices/Vaults",
   "sku": {
     "name": "RS0",
     "tier": "Standard"
   }
 }
```

>[!NOTE]
>Bu örnekte oluşturulan kasa, Azure Resource Manager istemcisi aracılığıyla zaten sistem tarafından atanan bir yönetilen kimlikle oluşturulmuş.

### <a name="managing-permissions-on-resource-groups"></a>Kaynak gruplarında izinleri yönetme

Kasa için yönetilen kimliğin, kaynak grubunda ve özel uç noktaların oluşturulacağı sanal ağda aşağıdaki izinlere sahip olması gerekir:

- `Microsoft.Network/privateEndpoints/*` Bu, kaynak grubundaki özel uç noktalarda CRUD 'yi gerçekleştirmek için gereklidir. Kaynak grubuna atanmalıdır.
- `Microsoft.Network/virtualNetworks/subnets/join/action` Bu, özel IP 'nin özel uç noktayla birlikte eklendiği sanal ağda gereklidir.
- `Microsoft.Network/networkInterfaces/read` Bu, Özel uç nokta için oluşturulan ağ arabirimini almak üzere kaynak grubunda gereklidir.
- Özel DNS bölge katılımcısı rolü bu rol zaten var ve izinleri sağlamak için kullanılabilir `Microsoft.Network/privateDnsZones/A/*` `Microsoft.Network/privateDnsZones/virtualNetworkLinks/read` .

Gerekli izinlere sahip roller oluşturmak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

#### <a name="create-roles-and-permissions-manually"></a>Rolleri ve izinleri el ile oluşturma

Aşağıdaki JSON dosyalarını oluşturun ve rol oluşturmak için bölümün sonundaki PowerShell komutunu kullanın:

Üzerinde PrivateEndpointContributorRoleDef.js

```json
{
  "Name": "PrivateEndpointContributor",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows management of Private Endpoint",
  "Actions": [
    "Microsoft.Network/privateEndpoints/*",
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

Üzerinde NetworkInterfaceReaderRoleDef.js

```json
{
  "Name": "NetworkInterfaceReader",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows read on networkInterfaces",
  "Actions": [
    "Microsoft.Network/networkInterfaces/read"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

Üzerinde PrivateEndpointSubnetContributorRoleDef.js

```json
{
  "Name": "PrivateEndpointSubnetContributor",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows adding of Private Endpoint connection to Virtual Networks",
  "Actions": [
    "Microsoft.Network/virtualNetworks/subnets/join/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

```azurepowershell
 New-AzRoleDefinition -InputFile "PrivateEndpointContributorRoleDef.json"
 New-AzRoleDefinition -InputFile "NetworkInterfaceReaderRoleDef.json"
 New-AzRoleDefinition -InputFile "PrivateEndpointSubnetContributorRoleDef.json"
```

#### <a name="use-a-script"></a>Komut dosyası kullan

1. Azure portal **Cloud Shell** başlatın ve PowerShell penceresinde **dosyayı karşıya yükle** ' yi seçin.

    ![PowerShell penceresinde dosyayı karşıya yükle ' yi seçin](./media/private-endpoints/upload-file-in-powershell.png)

1. Şu betiği karşıya yükleyin: [VaultMsiPrereqScript](https://download.microsoft.com/download/1/2/6/126a410b-0e06-45ed-b2df-84f353034fa1/VaultMsiPrereqScript.ps1)

1. Giriş klasörünüze gidin (örneğin: `cd /home/user` )

1. Şu betiği çalıştırın:

    ```azurepowershell
    ./VaultMsiPrereqScript.ps1 -subscription <subscription-Id> -vaultPEResourceGroup <vaultPERG> -vaultPESubnetResourceGroup <subnetRG> -vaultMsiName <msiName>
    ```

    Parametreler şunlardır:

    - **abonelik**: * * kasa için özel bitiş noktasının oluşturulacağı kaynak grubuna ve kasasının özel uç noktasının eklendiği alt ağa sahip olan SubscriptionID

    - **Vaultperesourcegroup**: kasa için özel uç noktanın oluşturulacağı kaynak grubu

    - **Vaultpesubnetresourcegroup**: özel uç noktanın katılacağını alt ağın kaynak grubu

    - **Vaultmsiname**: kasasının, **vaultname** ile aynı olan MSI adı

1. Kimlik doğrulamasını tamamladıktan sonra, komut dosyası yukarıda belirtilen aboneliğin bağlamını alır. Kiracıda eksik olmaları durumunda uygun rolleri oluşturur ve bu roller, kasanın MSI 'ye roller atayacaktır.

### <a name="creating-private-endpoints-using-azure-powershell"></a>Azure PowerShell kullanarak özel uç noktalar oluşturma

#### <a name="auto-approved-private-endpoints"></a>Otomatik onaylanan özel uç noktalar

```azurepowershell
$vault = Get-AzRecoveryServicesVault `
        -ResourceGroupName $vaultResourceGroupName `
        -Name $vaultName
  
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
        -Name $privateEndpointConnectionName `
        -PrivateLinkServiceId $vault.ID `
        -GroupId "AzureBackup"  
  
$privateEndpoint = New-AzPrivateEndpoint `
        -ResourceGroupName $vmResourceGroupName `
        -Name $privateEndpointName `
        -Location $location `
        -Subnet $subnet `
        -PrivateLinkServiceConnection $privateEndpointConnection `
        -Force
```

### <a name="manual-approval-of-private-endpoints-using-the-azure-resource-manager-client"></a>Azure Resource Manager Istemcisi kullanılarak özel uç noktaların el ile onaylanması

1. Özel uç noktanız için özel uç nokta bağlantı KIMLIĞINI almak üzere **Getkasayı** kullanın.

    ```rest
    armclient GET /subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/vaults/<vaultname>?api-version=2017-07-01-preview
    ```

    Bu, Özel uç nokta bağlantı KIMLIĞINI döndürür. Bağlantı adı aşağıdaki gibi bağlantı KIMLIĞININ ilk bölümü kullanılarak alınabilir:

    `privateendpointconnectionid = {peName}.{vaultId}.backup.{guid}`

1. Yanıttaki **Özel uç nokta bağlantı kimliğini** (ve gerektiğinde **Özel uç nokta adını**) alın ve AŞAĞıDAKI JSON ve Azure Resource Manager URI 'sinde değiştirin ve aşağıdaki örnekte gösterildiği gibi, durumu "onaylandı/reddedildi/bağlantısı kesildi" olarak değiştirmeyi deneyin:

    ```rest
    armclient PUT /subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>/privateEndpointConnections/<privateendpointconnectionid>?api-version=2020-02-02-preview @C:\<filepath>\BackupAdminApproval.json
    ```

    NESNESINDE

    ```json
    {
    "id": "/subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>/privateEndpointConnections/<privateendpointconnectionid>",
    "properties": {
        "privateEndpoint": {
        "id": "/subscriptions/<subscriptionid>/resourceGroups/<pergname>/providers/Microsoft.Network/privateEndpoints/pename"
        },
        "privateLinkServiceConnectionState": {
        "status": "Disconnected",  //choose state from Approved/Rejected/Disconnected
        "description": "Disconnected by <userid>"
        }
    }
    }
    ```

### <a name="dns-changes-for-custom-dns-servers"></a>Özel DNS sunucuları için DNS değişiklikleri

#### <a name="create-dns-zones-for-custom-dns-servers"></a>Özel DNS sunucuları için DNS bölgeleri oluşturma

Üç özel DNS bölgesi oluşturmanız ve bunları sanal ağınıza bağlamanız gerekir. Blob ve kuyruğun aksine, yedekleme hizmeti genel URL 'Lerinin özel bağlantı DNS bölgelerine yeniden yönlendirme için Azure genel DNS 'e kaydolmayacağını aklınızda bulundurun. 

| **Bölge**                                                     | **Hizmet** |
| ------------------------------------------------------------ | ----------- |
| `privatelink.<geo>.backup.windowsazure.com`      | Backup      |
| `privatelink.blob.core.windows.net`                            | Blob        |
| `privatelink.queue.core.windows.net`                           | Kuyruk       |

>[!NOTE]
>Yukarıdaki metinde, *coğrafi* bölge kodu anlamına gelir. Örneğin, sırasıyla Orta Batı ABD ve Kuzey Avrupa için *wcus* ve *ne* yapın.

Bölge kodları [listesine](https://download.microsoft.com/download/1/2/6/126a410b-0e06-45ed-b2df-84f353034fa1/AzureRegionCodesList.docx) bakın. Ulusal bölgelerde URL adlandırma kuralları için aşağıdaki bağlantılara bakın:

- [Çin](/azure/china/resources-developer-guide#check-endpoints-in-azure)
- [Almanya](../germany/germany-developer-guide.md#endpoint-mapping)
- [US Gov](../azure-government/documentation-government-developer-guide.md)

#### <a name="adding-dns-records-for-custom-dns-servers"></a>Özel DNS sunucuları için DNS kayıtları ekleme

Bu, Özel uç noktanıza ait her FQDN için Özel DNS bölgenize giriş yapmanızı gerektirir.

Yedekleme, blob ve Kuyruk hizmeti için oluşturulan özel uç noktaları kullandığımızda not edilmelidir.

- Kasa için özel uç nokta, Özel uç nokta oluşturulurken belirtilen adı kullanıyor
- Blob ve kuyruk Hizmetleri için özel uç noktalara, kasa için aynı adı eklenir.

Örneğin, aşağıdaki resimde, *pee2epe* adlı özel bir uç nokta bağlantısı için oluşturulan üç özel uç nokta gösterilmektedir:

![Özel bir uç nokta bağlantısı için üç özel uç nokta](./media/private-endpoints/three-private-endpoints.png)

Yedekleme hizmeti () için DNS bölgesi `privatelink.<geo>.backup.windowsazure.com` :

1. **Özel bağlantı merkezinde** yedekleme için özel uç noktanıza gidin. Genel Bakış sayfası, Özel uç noktanız için FQDN ve özel IP 'Leri listeler.

1. Her FQDN ve özel IP için bir tür kaydı olarak bir giriş ekleyin.

    ![Her FQDN ve özel IP için giriş ekleme](./media/private-endpoints/add-entry-for-each-fqdn-and-ip.png)

Blob hizmeti () için DNS bölgesi `privatelink.blob.core.windows.net` :

1. **Özel bağlantı merkezinde** blob için özel uç noktanıza gidin. Genel Bakış sayfası, Özel uç noktanız için FQDN ve özel IP 'Leri listeler.

1. FQDN ve özel IP için bir tür kaydı olarak bir giriş ekleyin.

    ![Blob hizmeti için bir tür kaydı olarak FQDN ve özel IP girdisi ekleme](./media/private-endpoints/add-type-a-record-for-blob.png)

Kuyruk hizmeti () için DNS bölgesi `privatelink.queue.core.windows.net` :

1. **Özel bağlantı merkezinde** kuyruk için özel uç noktanıza gidin. Genel Bakış sayfası, Özel uç noktanız için FQDN ve özel IP 'Leri listeler.

1. FQDN ve özel IP için bir tür kaydı olarak bir giriş ekleyin.

    ![Kuyruk hizmeti için bir tür kaydı olarak FQDN ve özel IP girdisi ekleme](./media/private-endpoints/add-type-a-record-for-queue.png)

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

S. Var olan bir yedekleme Kasası için özel bir uç nokta oluşturabilir miyim?<br>
A. Hayır, Özel uç noktalar yalnızca yeni yedekleme kasaları için oluşturulabilir. Bu nedenle kasada hiç öğe korunmamış olmalıdır. Aslında, özel bitiş noktaları oluşturulmadan önce herhangi bir öğeyi kasaya koruma girişimi yapılabilir.

S. Bir öğeyi kasayma korumaya çalıştım, ancak başarısız oldu ve kasa hala onunla korunan herhangi bir öğe içermiyor. Bu kasa için özel uç noktalar oluşturabilir miyim?<br>
A. Hayır, kasadaki herhangi bir öğeyi geçmişteki bir şekilde korumaya yönelik herhangi bir girişimde bulunulmamalıdır.

S. Yedekleme ve geri yükleme için özel uç noktaları kullanan bir kasam var. Bu kasaya korunan yedekleme öğeleri olsa bile daha sonra bu kasa için özel uç noktalar ekleyebilir veya kaldırabilir miyim?<br>
A. Evet. Bir kasa ve korumalı yedekleme öğeleri için özel uç noktalar oluşturduysanız, daha sonra gerektiğinde özel uç noktaları ekleyebilir veya kaldırabilirsiniz.

S. Azure Backup için özel uç nokta Azure Site Recovery de kullanılabilir mi?<br>
A. Hayır, yedekleme için özel uç nokta yalnızca Azure Backup için kullanılabilir. Hizmet tarafından destekleniyorsa Azure Site Recovery için yeni bir özel uç nokta oluşturmanız gerekir.

S. Bu makaledeki adımlardan birini kaçırdım ve veri kaynağınızı korudum. Özel uç noktaları kullanmaya devam edebilir miyim?<br>
A. Makaledeki adımları takip etmez ve öğeleri korumaya devam etmek, kasaya özel uç noktalar kullanamayacak şekilde yol açabilir. Bu nedenle, öğeleri korumaya devam etmeden önce bu denetim listesine başvurmanız önerilir.

S. Azure özel DNS bölgesi veya tümleşik özel bir DNS bölgesi kullanmak yerine kendi DNS sunucunuzu kullanabilir miyim?<br>
A. Evet, kendi DNS sunucularınızı kullanabilirsiniz. Ancak, tüm gerekli DNS kayıtlarının bu bölümde önerildiği şekilde eklendiğinden emin olun.

S. Bu makaledeki işlemi uyguladıktan sonra, sunucum üzerinde ek adımlar gerçekleştirmem gerekir mi?<br>
A. Bu makalede ayrıntılı işlem yapıldıktan sonra, yedekleme ve geri yükleme için özel uç noktaları kullanmak üzere ek iş yapmanız gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Backup tüm güvenlik özellikleri](security-overview.md) hakkında bilgi edinin
