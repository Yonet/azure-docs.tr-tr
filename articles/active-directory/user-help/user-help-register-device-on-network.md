---
title: Kişisel cihazları bir kuruluşun ağına kaydetme-Azure AD
description: Kuruluşunuzun korunan kaynaklarına erişebilmek için kişisel cihazınızı kuruluşunuzun ağına nasıl kaydedeceğinizi öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 08/31/2020
ms.author: curtand
ms.reviewer: jairoc
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 0435b99525c34eb72d7cc5315ccb4359859cd528
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90033056"
---
# <a name="register-your-personal-device-on-your-organizations-network"></a>Kişisel cihazınızı kuruluşunuzun ağına kaydetme
Kuruluşunuzun ağına kişisel cihazınızı (genellikle telefon veya tablet) kaydedin. Cihazınız kaydolduktan sonra kuruluşunuzun kısıtlanmış kaynaklarına erişebilir.

>[!Note]
>Bu makalede tanıtım amacıyla bir Windows cihazı kullanılmaktadır, ancak iOS, Android veya macOS çalıştıran cihazları da kaydedebilirsiniz.

## <a name="what-happens-when-you-register-your-device"></a>Cihazınızı kaydettiğinizde ne olur?
Cihazınızı kuruluşunuzun ağına kaydederken aşağıdaki eylemler gerçekleşecektir:

- Windows cihazınızı kuruluşunuzun ağına kaydeder.

- İsteğe bağlı olarak, kuruluşunuzun seçeneklerine bağlı olarak [iki öğeli kimlik doğrulama](multi-factor-authentication-end-user-first-time.md) veya [güvenlik bilgisi](./security-info-setup-signin.md)aracılığıyla iki adımlı doğrulama ayarlamanız istenebilir.

- İsteğe bağlı olarak, kuruluşunuzun seçeneklerine bağlı olarak, Microsoft Intune gibi mobil cihaz yönetimine otomatik olarak kaydolmuş olabilirsiniz. Microsoft Intune kaydolma hakkında daha fazla bilgi için bkz. [cihazınızı Intune 'A kaydetme](/intune-user-help/enroll-your-device-in-intune-all).

- İş veya okul hesabınızın Kullanıcı adı ve parolasını kullanarak oturum açma işlemine geçebilirsiniz.

## <a name="to-register-your-windows-device"></a>Windows cihazınızı kaydetmek için

Kişisel cihazınızı ağınıza kaydetmek için aşağıdaki adımları izleyin.

1. **Ayarları**açın ve ardından **hesaplar**' ı seçin.

    ![Ayarlar ekranındaki hesaplar](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. **İşe veya okula erişim**' i seçin ve ardından **erişim iş veya okul** ekranından **Bağlan** ' ı seçin.

    ![Bağlan seçeneği vurgulanmış şekilde iş veya okul ekranına erişin](./media/user-help-register-device-on-network/register-device-access-work-school-connect.png)

3. **İş veya okul hesabı ekle** ekranında, iş veya okul hesabınız için e-posta adresinizi yazın ve ardından **İleri**' yi seçin. Örneğin, alain@contoso.com.

4. İş veya okul hesabınızda oturum açın ve **oturum aç**' ı seçin.

5. Kimlik doğrulama isteğinizi onaylama (iki adımlı doğrulama kullanıyorsanız) ve Windows Hello 'Yu ayarlama (gerekliyse) dahil olmak üzere kayıt işleminin geri kalanını tamamlayın.

## <a name="to-verify-that-youre-registered"></a>Kaydoldığınızı doğrulamak için
Ayarlarınıza bakarak kaydolduğunuzdan emin olabilirsiniz.

1. **Ayarları**açın ve ardından **hesaplar**' ı seçin.

    ![Ayarlar ekranındaki hesaplar](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. **İşe veya okula erişim**' i seçin ve iş veya okul hesabınızı görtığınızdan emin olun.

    ![Bağlı contoso hesabıyla iş veya okul ekranına erişin](./media/user-help-register-device-on-network/register-device-setup-verify.png)

## <a name="next-steps"></a>Sonraki adımlar
Kişisel cihazınızı kuruluşunuzun ağına kaydettikten sonra, kaynaklarınızın çoğuna erişebilmelisiniz.

- Kuruluşunuz iş cihazınıza katılmayı istiyorsa, bkz. [iş cihazınızı kuruluşunuzun ağına ekleme](user-help-join-device-on-network.md).