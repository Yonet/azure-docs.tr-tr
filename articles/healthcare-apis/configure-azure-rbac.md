---
title: FHıR için Azure API 'SI için Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) yapılandırma
description: Bu makalede, FHıR veri düzlemi için Azure API 'SI için Azure RBAC 'nin nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/15/2020
ms.author: matjazl
ms.reviewer: dseven
ms.openlocfilehash: 5cadfad445c76726b1b825b131de4016a57979fa
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93391849"
---
# <a name="configure-azure-rbac-for-fhir"></a>FHıR için Azure RBAC 'yi yapılandırma 

Bu makalede, FHıR veri düzlemi için Azure API 'sine erişim atamak üzere [Azure rol tabanlı erişim denetimi 'nin (Azure RBAC)](../role-based-access-control/index.yml) nasıl kullanılacağını öğreneceksiniz. Azure RBAC, veri düzlemi kullanıcıları Azure aboneliğinizle ilişkili Azure Active Directory kiracısında yönetildiğinde veri düzlemi erişimi atamaya yönelik tercih edilen yöntemlerdir. Dış Azure Active Directory kiracı kullanıyorsanız, [Yerel RBAC atama başvurusuna](configure-local-rbac.md)bakın.

## <a name="confirm-azure-rbac-mode"></a>Azure RBAC modunu onaylama

Azure RBAC 'yi kullanmak için, FHıR için Azure API 'nizin, veri düzlemi için Azure abonelik kiracınızı kullanacak şekilde yapılandırılması ve atanmış kimlik nesne kimlikleri olmaması gerekir. FHıR için Azure API 'nizin **kimlik doğrulama** dikey penceresini inceleyerek ayarlarınızı doğrulayabilirsiniz:

:::image type="content" source="media/rbac/confirm-azure-rbac-mode.png" alt-text="Azure RBAC modunu onaylama":::

**Yetkili** , aboneliğinizle Ilişkili Azure Active Directory kiracısına ayarlanmalıdır ve **Izin verilen nesne kimlikleri** etiketli kutuda GUID olmamalıdır. Ayrıca, kutunun devre dışı olduğunu ve bir etiketin veri düzlemi rollerini atamak için Azure RBAC 'nin kullanılması gerektiğini belirten bir etiket olduğunu fark edeceksiniz.

## <a name="assign-roles"></a>Rol atama

Kullanıcıları, hizmet sorumlularını veya grupları FHıR veri düzlemine erişim izni vermek için **erişim denetimi (IAM)** öğesine tıklayın, ardından **rol atamaları** ' na tıklayın ve **+ Ekle** ' ye tıklayın:

:::image type="content" source="media/rbac/add-azure-rbac-role-assignment.png" alt-text="Azure rol ataması Ekle":::

**Rol** seçiminde, fhır veri düzlemi için yerleşik rollerden birini arayın:

:::image type="content" source="media/rbac/built-in-fhir-data-roles.png" alt-text="Yerleşik FHıR veri rolleri":::

Arasından seçim yapabilirsiniz:

* FHıR veri okuyucu: FHıR verilerini okuyabilir (ve arayabilir).
* FHıR veri yazıcı: FHıR verilerini okuyabilir, yazabilir ve geçici olarak silebilir.
* FHıR veri verme programı: verileri okuyabilir ve dışarı aktarabilir ( `$export` işleç).
* FHıR verileri katılımcısı: tüm veri düzlemi işlemlerini gerçekleştirebilir.

Bu roller ihtiyacınız olması için yeterli değilse [özel roller de oluşturabilirsiniz](../role-based-access-control/tutorial-custom-role-powershell.md).

**Seç** kutusunda, rolü atamak istediğiniz kullanıcı, hizmet sorumlusu veya grup için arama yapın.

## <a name="caching-behavior"></a>Önbelleğe alma davranışı

FHıR için Azure API 'SI, kararları 5 dakikaya kadar önbelleğe alacak. İzin verilen nesne kimlikleri listesine ekleyerek FHıR sunucusuna bir kullanıcı erişimi verirseniz veya onları listeden kaldırırsanız, bu, en fazla beş dakika sonra yayarak yayma izinleridir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR veri düzlemi için Azure rolleri atamayı öğrendiniz. FHIR için Azure API 'SI ile ilgili ek ayarlar hakkında bilgi edinmek için:
 
>[!div class="nextstepaction"]
>[FHıR için Azure API için ek ayarlar](azure-api-for-fhir-additional-settings.md)