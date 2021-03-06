---
title: 'Azure AD Connect eşitleme: yanlışlıkla Silmeleri engelle | Microsoft Docs'
description: Bu konuda Azure AD Connect ' de yanlışlıkla silme (yanlışlıkla silmeleri önleme) özelliğinin önlenmesi açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16d48cda87b8226ebc3bbab179c1034abf0a486f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90084618"
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect Eşitleme: Yanlışlıkla Silmeleri Engelleme
Bu konuda Azure AD Connect ' de yanlışlıkla silme (yanlışlıkla silmeleri önleme) özelliğinin önlenmesi açıklanmaktadır.

Azure AD Connect yüklenirken, yanlışlıkla silinmekten kaçınmak varsayılan olarak etkindir ve 500 'den fazla silimiyle dışarı aktarmaya izin vermez şekilde yapılandırılır. Bu özellik, birçok kullanıcıyı ve nesneyi etkileyebilecek yanlışlıkla gerçekleştirilen yapılandırma değişikliklerinden ve şirket içi dizin değişikliklerinden koruma sağlamak için tasarlanmıştır.

## <a name="what-is-prevent-accidental-deletes"></a>Yanlışlıkla silmeleri engelleme
Çok sayıda silme gördüğünüzde yaygın senaryolar şunlardır:

* Bir [OU](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) veya [etki alanının](how-to-connect-sync-configure-filtering.md#domain-based-filtering) tamamının seçilmemiş olduğu [filtrelemeye](how-to-connect-sync-configure-filtering.md) yönelik değişiklikler.
* Bir kuruluş birimi içindeki tüm nesnelerin silinmesi.
* Bir kuruluş biriminin yeniden adlandırılması nedeniyle içindeki tüm nesnelerin eşitleme kapsamı dışında olarak değerlendirilmesi.

500 nesnelerinin varsayılan değeri `Enable-ADSyncExportDeletionThreshold` , kullanılarak Azure Active Directory Connect yüklenen AD eşitleme modülünün bir parçası olan PowerShell ile değiştirilebilir. Bu değeri, kuruluşunuzun boyutuna uyacak şekilde yapılandırmanız gerekir. Eşitleme Zamanlayıcısı her 30 dakikada bir çalıştığından, bu değer 30 dakika içinde görülen silme sayısıdır.

Azure AD 'ye dışarı aktarılmayacak kadar çok sayıda silme işlemi varsa, dışarı aktarma işlemi duraklar ve aşağıdakine benzer bir e-posta alırsınız:

![Yanlışlıkla silmeleri engelleme](./media/how-to-connect-sync-feature-prevent-accidental-deletes/email.png)

> *Merhaba (Teknik kişi). At (Time) Kimlik eşitlemesi hizmeti, silme sayısının (kuruluş adı) için yapılandırılan silme eşiğini aştığını algıladı. Bu Kimlik eşitlemesi çalıştırmak için toplam (sayı) nesne gönderildi. Bu, (sayı) nesnelerinin yapılandırılmış silme eşiği değerini karşılandı veya aştı. Devam etmeden önce bu silinmelerin işlenmesi gerektiğini onaylamanız gerekir. Bu e-posta iletisinde listelenen hata hakkında daha fazla bilgi için lütfen yanlışlıkla Silinenleri engellemeye bakın.*
>
> 

`stopped-deletion-threshold-exceeded`Dışarı aktarma profili için **Synchronization Service Manager** Kullanıcı arabirimine baktığınızda durumu da görebilirsiniz.
![Yanlışlıkla silmeleri Service Manager Kullanıcı arabirimini engelle](./media/how-to-connect-sync-feature-prevent-accidental-deletes/syncservicemanager.png)

Bu beklenmiyorsa, inceleyin ve düzeltici eylemler gerçekleştirin. Hangi nesnelerin silinmek üzere olduğunu görmek için aşağıdakileri yapın:

1. Başlangıç menüsünden **eşitleme hizmetini** başlatın.
2. **Bağlayıcılar**'a gidin.
3. **Azure Active Directory**türündeki bağlayıcıyı seçin.
4. Sağdaki **Eylemler** altında **bağlayıcı alanı ara**' yı seçin.
5. **Kapsam**altındaki açılır pencerede, **bu yana bağlantısı kesildi** ' ı seçin ve geçmişte bir zaman seçin. **Ara**’ya tıklayın. Bu sayfa, silinecek tüm nesnelerin bir görünümünü sağlar. Her bir öğeye tıklayarak nesne hakkında ek bilgi edinebilirsiniz. Ayrıca, kılavuzda görünür olacak ek öznitelikler eklemek için **sütun ayarı** ' na de tıklayabilirsiniz.

![Bağlayıcı alanını ara](./media/how-to-connect-sync-feature-prevent-accidental-deletes/searchcs.png)

[!NOTE] Tüm silinmelerde emin değilseniz ve daha güvenli bir yol aşağı gitmek istiyorsanız. PowerShell cmdlet 'ini kullanarak, `Enable-ADSyncExportDeletionThreshold` istenmeyen silinmelere izin verebilecek eşiği devre dışı bırakmak yerine yeni bir eşik ayarlamak için: 

## <a name="if-all-deletes-are-desired"></a>Tüm silmeler isteniyorsa
Tüm silmeler isteniyorsa, şunları yapın:

1. Geçerli silme eşiğini almak için PowerShell cmdlet 'ini çalıştırın `Get-ADSyncExportDeletionThreshold` . Bir Azure AD Genel yönetici hesabı ve parolası sağlayın. Varsayılan değer 500'dür.
2. Bu korumayı geçici olarak devre dışı bırakmak ve bu silmeleri devam etmek için PowerShell cmdlet 'ini çalıştırın: `Disable-ADSyncExportDeletionThreshold` . Bir Azure AD Genel yönetici hesabı ve parolası sağlayın.
   ![Ekran görüntüsünde, Azure AD Genel Yöneticisi Kullanıcı adı ve parolasının girileceği bir iletişim kutusu gösterilir.](./media/how-to-connect-sync-feature-prevent-accidental-deletes/credentials.png)
3. Azure Active Directory Bağlayıcısı seçiliyken, **Çalıştır** eylemini seçin ve **dışarı aktar**' ı seçin.
4. Korumayı yeniden etkinleştirmek için PowerShell cmdlet 'ini çalıştırın: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500` . 500 değerini, geçerli silme eşiğini alırken gördüğünüz değerle değiştirin. Bir Azure AD Genel yönetici hesabı ve parolası sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
