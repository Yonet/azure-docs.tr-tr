---
title: RequestDisallowedByPolicy hatası
description: Kaynakları Azure Resource Manager ile dağıttığınızda RequestDisallowedByPolicy hatasının nedenini açıklar.
author: genlin
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 41581ba48da2f2e717c5abf2a749f8fd2b86ac06
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75477674"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Azure Kaynak ilkesi ile RequestDisallowedByPolicy hatası

Bu makalede RequestDisallowedByPolicy hatasının nedeni açıklanır. bu hata için çözüm de sağlar.

## <a name="symptom"></a>Belirti

Dağıtım sırasında kaynakları oluşturmanızı önleyen bir **Requestdisallowedbypolicy** hatası alabilirsiniz. Aşağıdaki örnekte hata gösterilmektedir:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Sorun giderme

Dağıtımınızı engelleyen ilkeyle ilgili ayrıntıları almak için aşağıdaki yöntemlerden birini kullanın:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell 'de, `Id` dağıtımınızı engelleyen ilkeyle ilgili ayrıntıları almak için bu ilke tanımlayıcısını parametre olarak belirtin.

```powershell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure CLI

Azure CLı 'de, ilke tanımının adını belirtin:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Çözüm

Güvenlik veya uyumluluk için abonelik yöneticileri, kaynakların nasıl dağıtıldığını sınırlayan ilkeler atayabilir. Örneğin, aboneliğiniz genel IP adresleri, ağ güvenlik grupları, User-Defined yollar veya yol tabloları oluşturmayı önleyen bir ilkeye sahip olabilir. **Belirtiler** bölümündeki hata mesajı ilkenin adını gösterir.
Bu sorunu çözmek için, kaynak ilkelerini gözden geçirin ve bu ilkelerle uyumlu kaynakların nasıl dağıtılacağını saptayın.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure İlkesi nedir?](../../governance/policy/overview.md)
- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)
