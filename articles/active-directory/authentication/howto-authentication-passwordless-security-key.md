---
title: Passwordless güvenlik anahtarı oturum açma (Önizleme)-Azure Active Directory
description: FIDO2 güvenlik anahtarlarını (Önizleme) kullanarak Azure AD 'de passwordless güvenlik anahtarı oturum açma özelliğini etkinleştirme
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/14/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ac8cf172a13e7198233170634ee4a3954793cd2
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96743437"
---
# <a name="enable-passwordless-security-key-sign-in-preview"></a>Passwordless güvenlik anahtarı oturumunu etkinleştir (Önizleme)

Bugün parola kullanan ve paylaşılan bir BILGISAYAR ortamına sahip olan kuruluşlar için güvenlik anahtarları, çalışanların Kullanıcı adı veya parola girmeden kimlik doğrulamasının sorunsuz bir yolunu sağlar. Güvenlik anahtarları, çalışanlar için geliştirilmiş üretkenlik sağlar ve daha iyi güvenliğe sahiptir.

Bu belge güvenlik anahtarı tabanlı passwordless kimlik doğrulamasını etkinleştirmeye odaklanır. Bu makalenin sonunda, FIDO2 güvenlik anahtarı kullanarak Azure AD hesabınızla Web tabanlı uygulamalarda oturum açabilirsiniz.

> [!NOTE]
> FIDO2 güvenlik anahtarları Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="requirements"></a>Gereksinimler

- [Azure AD Multi-Factor Authentication](howto-mfa-getstarted.md)
- [Birleşik güvenlik bilgileri kaydı önizlemesini](concept-registration-mfa-sspr-combined.md) etkinleştir
- Uyumlu [FIDO2 güvenlik anahtarları](concept-authentication-passwordless.md#fido2-security-keys)
- WebAuthN, Windows 10 sürüm 1903 veya üstünü gerektirir * *

Web Apps ve hizmetlerinde oturum açmak için güvenlik anahtarlarını kullanmak üzere, WebAuthN protokolünü destekleyen bir tarayıcıya sahip olmanız gerekir. Bunlara Microsoft Edge, Chrome, Firefox ve Safari dahildir.

## <a name="prepare-devices-for-preview"></a>Cihazları önizleme için hazırlama

Azure AD 'ye katılmış cihazlarda en iyi deneyim Windows 10 sürüm 1903 veya üzeri sürümlerde bulunur.

Karma Azure AD 'ye katılmış cihazlar Windows 10 sürüm 2004 veya üstünü çalıştırmalıdır.

## <a name="enable-passwordless-authentication-method"></a>Passwordless kimlik doğrulama yöntemini Etkinleştir

### <a name="enable-the-combined-registration-experience"></a>Birleşik kayıt deneyimini etkinleştir

Passwordless kimlik doğrulama yöntemlerinin kayıt özellikleri, Birleşik kayıt özelliğini kullanır. Birleşik kayıt özelliğini etkinleştirmek için [Birleşik güvenlik bilgileri kaydını (Önizleme) etkinleştirme](howto-registration-mfa-sspr-combined.md)makalesindeki adımları izleyin.

### <a name="enable-fido2-security-key-method"></a>FIDO2 güvenlik anahtarı yöntemini Etkinleştir

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **kimlik doğrulama yöntemleri**  >  **kimlik doğrulama yöntemi ilkesi 'ne (Önizleme)** gidin.
1. Method **FIDO2 Security Key** altında aşağıdaki seçenekleri belirleyin:
   1. **Etkinleştir** -Evet veya Hayır
   1. **Hedef** -tüm kullanıcılar veya kullanıcıları seçin
1. Yapılandırmayı **kaydedin** .

## <a name="user-registration-and-management-of-fido2-security-keys"></a>FIDO2 güvenlik anahtarlarının Kullanıcı kaydı ve yönetimi

1. [https://myprofile.microsoft.com](https://myprofile.microsoft.com) adresine göz atın.
1. Henüz yoksa oturum açın.
1. **Güvenlik bilgileri**' ne tıklayın.
   1. Kullanıcının zaten en az bir Azure AD Multi-Factor Authentication yöntemi varsa, bir FIDO2 güvenlik anahtarını hemen kaydedebilirler.
   1. Kayıtlı en az bir Azure AD Multi-Factor Authentication yöntemi yoksa, bir tane eklemesi gerekir.
1. **Yöntem Ekle** ' ye tıklayıp **güvenlik anahtarı**' nı seçerek bir FIDO2 güvenlik anahtarı ekleyin.
1. **USB cihazı** veya **NFC cihazını** seçin.
1. Anahtarınızı hazırlayın ve **İleri ' yi** seçin.
1. Bir kutu görünür ve kullanıcıdan güvenlik anahtarınız için bir PIN oluşturmasını/girmesini ister ve ardından anahtar için Biyometri ya da Touch için gerekli hareketi gerçekleştirir.
1. Kullanıcı, Birleşik kayıt deneyimine döndürülür ve kullanıcının birden çok tane varsa bunu belirleyebilmesi için anahtar için anlamlı bir ad sağlaması istenir. **İleri**’ye tıklayın.
1. İşlemi gerçekleştirmek için **bitti** ' ye tıklayın.

## <a name="sign-in-with-passwordless-credential"></a>Passwordless kimlik bilgileriyle oturum açın

Aşağıdaki örnekte, bir Kullanıcı FIDO2 güvenlik anahtarını zaten sağladı. Kullanıcı, Windows 10 sürüm 1903 veya üzeri sürümlerde desteklenen bir tarayıcı içinde FIDO2 güvenlik anahtarı ile Web 'de oturum açmayı tercih edebilir.

![Güvenlik anahtarı oturum açma Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliği önizlemede geri bildirimde bulunmak veya sorun yaşıyorsanız, aşağıdaki adımları kullanarak Windows geri bildirim Merkezi uygulaması aracılığıyla paylaşabilirsiniz:

1. **Geri Bildirim Hub 'ını** başlatın ve oturum açtığınızdan emin olun.
1. Aşağıdaki kategoriye göre geri bildirim gönderin:
   - Kategori: güvenlik ve Gizlilik
   - Alt Kategori: FıDO
1. Günlükleri yakalamak için, **sorunu yeniden oluşturmak** için seçeneğini kullanın

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="security-key-provisioning"></a>Güvenlik anahtarı sağlama

Güvenlik anahtarlarının yönetici tarafından sağlanması ve devre dışı sağlanması genel önizlemede bulunmamaktadır.

### <a name="upn-changes"></a>UPN değişiklikleri

Karma Azure AD 'ye katılmış ve Azure AD 'ye katılmış cihazlarda UPN değişikliğine izin veren bir özelliği desteklemeye çalışıyoruz. Bir kullanıcının UPN 'si değişirse değişiklik için FIDO2 güvenlik anahtarlarını artık değiştiremezsiniz. Çözüm, cihazı sıfırlayıp kullanıcının yeniden kaydolmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[FIDO2 güvenlik anahtarı Windows 10 oturum açma](howto-authentication-passwordless-security-key-windows.md)

[Şirket içi kaynaklarda FIDO2 kimlik doğrulamasını etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md)

[Cihaz kaydı hakkında daha fazla bilgi edinin](../devices/overview.md)

[Azure AD Multi-Factor Authentication hakkında daha fazla bilgi](../authentication/howto-mfa-getstarted.md)
