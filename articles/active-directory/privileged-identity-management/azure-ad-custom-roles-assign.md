---
title: Azure AD özel rolü atama-Privileged Identity Management (PıM)
description: Atama Privileged Identity Management için bir Azure AD özel rolü atama (PıM)
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75e004980ada23a616b57e3c7eb1afb241e09598
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371656"
---
# <a name="assign-an-azure-ad-custom-role-in-privileged-identity-management"></a>Privileged Identity Management bir Azure AD özel rolü atama

Bu makalede, Azure Active Directory (Azure AD) yönetim deneyiminde uygulamaları yönetmek için oluşturulan özel rollere tam zamanında ve zamana bağlanacak bir atama oluşturmak üzere Privileged Identity Management (PıM) ' nin nasıl kullanılacağı açıklanır.

- Azure AD 'de uygulama yönetimi için özel roller oluşturma hakkında daha fazla bilgi için, bkz. [Azure Active Directory (Önizleme) Içinde özel yönetici rolleri](../roles/custom-overview.md).
- Privileged Identity Management henüz kullanmadıysanız [Privileged Identity Management kullanmaya başlama](pim-getting-started.md)hakkında daha fazla bilgi alın.
- Privileged Identity Management yönetmek için başka bir yöneticiye erişim verme hakkında daha fazla bilgi için bkz. [Privileged Identity Management yönetmek için diğer yöneticilere erişim verme](pim-how-to-give-access-to-pim.md).

> [!NOTE]
> Azure AD özel rolleri, önizleme sırasında yerleşik Dizin rolleriyle tümleştirilmiştir. Yetenek genel kullanıma sunulduğunda, rol yönetimi yerleşik roller deneyiminde gerçekleşmeyecektir. Aşağıdaki başlığı görürseniz, bu rollerin [yerleşik roller deneyiminde](pim-how-to-activate-role.md) yönetilmesi gerekir ve bu makale uygulanmaz:
>
> [![Azure AD > Privileged Identity Management seçin.](media/pim-how-to-add-role-to-user/pim-new-version.png)](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

## <a name="assign-a-role"></a>Rol atama

Privileged Identity Management, Azure Active Directory (Azure AD) uygulama yönetiminde oluşturabileceğiniz özel rolleri yönetebilir.  Aşağıdaki adımlar özel bir dizin rolüne uygun bir atama yapar.

1. Azure portal [Privileged Identity Management](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart) Için, ayrıcalıklı rol yöneticisi rolüne atanan bir kullanıcı hesabıyla oturum açın.
1. **Azure AD özel rollerini (Önizleme)** seçin.

    ![Uygun rol atamalarını görmek için Azure AD özel rolleri Önizlemesi ' ni seçin](./media/azure-ad-custom-roles-assign/view-custom.png)

1. Azure AD uygulamalarına yönelik özel rollerin listesini görmek için **Roller** ' i seçin.

    ![Rolleri seçin uygun rol atamaları listesini görüntüleyin](./media/azure-ad-custom-roles-assign/view-roles.png)

1. Atama sayfasını açmak için **üye Ekle** ' yi seçin.
1. Rol atamasının kapsamını tek bir uygulamayla kısıtlamak için, **kapsam** ' ı seçerek bir uygulama kapsamı belirtin.

    ![Azure AD 'de uygun rol atamalarının kapsamını kısıtlama](./media/azure-ad-custom-roles-assign/set-scope.png)

1. **Rol seç listesini** **açmak için bir rol seçin** öğesini seçin.

    ![kullanıcıya atanacak uygun rolü seçin](./media/azure-ad-custom-roles-assign/select-role.png)

1. Atamak istediğiniz bir rol seçin ve ardından **Seç**' e tıklayın. **Üye seçin** listesi açılır.

    ![rolü atadığınız kullanıcıyı seçin](./media/azure-ad-custom-roles-assign/select-member.png)

1. Role atamak istediğiniz kullanıcıyı seçin ve ardından **Seç**' e tıklayın. **Üyelik ayarları** listesi açılır.

    ![Rol atama türünü uygun veya etkin olarak ayarla](./media/azure-ad-custom-roles-assign/membership-settings.png)

1. **Üyelik ayarları** sayfasında **uygun** veya **etkin**' i seçin:

    - **Uygun** atamalar, rolü kullanabilmesi için önce rolün atandığı kullanıcının bir eylem gerçekleştirmesini gerektirir. Eylemler, bir Multi-Factor Authentication denetimini geçirmeyi, iş gerekçesini sağlamayı veya belirlenen onaylayanlardan onay istemeyi içerebilir.
    - **Etkin** atamalar, atanan kullanıcının rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmez. Etkin kullanıcılar role her zaman atanmış ayrıcalıklara sahiptir.

1. **Kalıcı** onay kutusu varsa ve kullanılabiliyorsa (rol ayarlarına bağlı olarak), atamanın kalıcı olup olmadığını belirtebilirsiniz. Atamanın kalıcı olarak uygun veya kalıcı olarak atanmasını sağlamak için onay kutusunu seçin. Atama süresi belirtmek için onay kutusunu temizleyin.
1. Yeni rol atamasını oluşturmak için **Kaydet** ' e ve ardından **Ekle**' ye tıklayın. Atama işlemi durumu bildirimi görüntülenir.

Rol atamasını doğrulamak için, açık bir rol içinde, **atamalar**  >  **ata** ' yı seçin ve rol atamalarınızın uygun veya etkin olarak doğru şekilde tanımlandığını doğrulayın.

 ![Rol atamasının uygun veya etkin olarak görünür olup olmadığını denetleyin](./media/azure-ad-custom-roles-assign/verify-assignments.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolünü etkinleştirme](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol atamasını kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD 'de rol tanımları](../roles/permissions-reference.md)
