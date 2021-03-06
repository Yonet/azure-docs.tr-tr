---
title: Ortamınızı Blueprint Işleci için ayarlama
description: Azure ortamınızı Blueprint Işleci Azure yerleşik rolüyle kullanılmak üzere nasıl yapılandıracağınızı öğrenin.
ms.date: 02/05/2021
ms.topic: how-to
ms.openlocfilehash: 0e0f6680ab39481a480919af10fadc0f7103a1fb
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99591610"
---
# <a name="configure-your-environment-for-a-blueprint-operator"></a>Ortamınızı Blueprint İşleci için yapılandırma

Şema tanımlarınızın ve şema atamalarının yönetimi farklı takımlara atanabilir. Bir işlem ekibinin, merkezi olarak denetlenen şema tanımlarının atamalarını yönetmekten sorumlu olması halinde, bir mimarın ve idare ekibinin, şema tanımlarınızın yaşam döngüsü yönetiminden sorumlu olması yaygındır.

**Blueprint işleci** yerleşik rolü, bu tür bir senaryoda kullanılmak üzere özel olarak tasarlanmıştır. Rol, kuruluşların şema tanımlarının atanmasını yönetmesine, ancak bunları değiştirebilme yeteneğine izin vermez. Bunun yapılması, Azure ortamınızda bazı yapılandırmalar gerektirir ve bu makalede gerekli adımlar açıklanmaktadır.

## <a name="grant-permission-to-the-blueprint-operator"></a>Blueprint Işlecine izin verme

Birinci adım, şema atamak için şema **operatörü** rolünü hesaba veya güvenlik grubuna (önerilen) vermekle aynıdır. Bu eylem, yönetim grubu hiyerarşisinde, işlemler ekibinin şema atama erişiminin olması gereken tüm yönetim gruplarını ve abonelikleri kapsayan en yüksek düzeyde yapılmalıdır. Bu izinleri verirken en az ayrıcalık ilkesini izlemeniz önerilir.

1. Önerilen [Güvenlik grubu oluşturma ve üye ekleme](../../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

1. Hesap veya güvenlik grubuna bir **Blueprint işlecinin** [rol atamasını ekleyin](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment)

## <a name="user-assign-managed-identity"></a>Kullanıcı tarafından yönetilen kimliği atama

Şema tanımı, sistem tarafından atanan veya Kullanıcı tarafından atanan yönetilen kimlikleri kullanabilir. Ancak, **şema operatörü** rolünü kullanırken, şema tanımının Kullanıcı tarafından atanan yönetilen kimlik kullanacak şekilde yapılandırılması gerekir. Ayrıca, şema **operatörü** rolüne verilen hesap veya güvenlik grubuna kullanıcı tarafından atanan yönetilen kimlik Için **yönetilen kimlik operatörü** rolü verilmelidir. Bu izin olmadan, şema atamaları izin eksikliği nedeniyle başarısız olur.

1. Atanan bir Blueprint tarafından kullanılmak üzere [Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity) .

1. Kullanıcı tarafından atanan yönetilen kimliğe, hedeflenen kapsamın şema tanımının gerektirdiği herhangi bir rol veya izin verin.

1. Hesap veya güvenlik grubuna **yönetilen kimlik işlecinin** [rol atamasını ekleyin](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment) . Rol atamasını Kullanıcı tarafından atanan yeni yönetilen kimliğe kapsam.

1. **Şema operatörü** olarak, Kullanıcı tarafından atanan yeni yönetilen kimliği kullanan [bir şema atayın](../create-blueprint-portal.md#assign-a-blueprint) .

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.