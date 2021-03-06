---
title: Sayfa düzeni sürümleri
titleSuffix: Azure AD B2C
description: Özel ilkelerde UI özelleştirmesi için sayfa düzeni sürüm geçmişi.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 491549933817a127a92180b0e4b455e461e4d3a2
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99492255"
---
# <a name="page-layout-versions"></a>Sayfa düzeni sürümleri

Sayfa düzeni paketleri, sayfa öğelerinde düzeltmeler ve iyileştirmeler içerecek şekilde düzenli olarak güncelleştirilir. Aşağıdaki değişiklik günlüğü her sürümde tanıtılan değişiklikleri belirtir.

## <a name="self-asserted-page-selfasserted"></a>Otomatik olarak onaylanan sayfa (selfasted)

**2.1.2**
- Ispanyolca ve Fransızca gibi diller için yerelleştirme kodlama sorunu düzeltildi.

**2.1.1**

- `heading` `intro` Sayfada başlık olarak görüntülenmek için öğesine ek olarak bir uxstring eklendi. Bu, varsayılan olarak gizlidir.
- İCloud anahtarlığa parolaların kaydedilmesine yönelik destek eklendi.
- Düzeni seçmek için ilkeyi veya QueryString parametresini kullanma desteği eklendi `pageFlavor` (klasik, oceanBlue veya slateGray).
- Otomatik olarak onaylanan sayfada bildirimler eklendi.
- Artık sayfa yüklenirken odak ilk düzenlenebilir alana yerleştirilir.
- Artık birden çok alanda hata olduğunda odak ilk hata alanına yerleştirilir.
- Artık odak, e-posta doğrulama kodu doğrulandıktan sonra ' Değiştir ' düğmesine yerleştirilir.

**2.1.0**

- Yerelleştirme ve erişilebilirlik düzeltmeleri.

**2.0.0**

- Özel ilkelerde [görüntüleme denetimleri](display-controls.md) için destek eklendi.

**1.2.0**

- Username/email ve Password alanları artık `form` Edge ve Internet Explorer 'ın (IE) bu bilgileri düzgün bir şekilde kaydetmesine izin vermek için HTML öğesini kullanır.
- Geliştirilmiş kullanıcı deneyimi için yapılandırılabilir bir kullanıcı girişi doğrulama gecikmesi eklendi.
- Erişilebilirlik düzeltmeleri
- Hata iletilerinin artık ekran okuyucusu tarafından okunabilmesi için bir erişilebilirlik sorunu düzeltildi. 
- Artık, e-posta doğrulandıktan sonra parola alanına odak konur.
- `autofocus`CheckBox denetiminden kaldırılır. 
- Telefon numarası doğrulaması için bir görüntü denetimi desteği eklendi.
- Artık `data-preload="true"` ÖZNITELIĞI [HTML etiketlerinizi](customize-ui-with-html.md#guidelines-for-using-custom-page-content) ekleyebilirsiniz
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerilere sahip olur.
- Chrome çevirisi desteği.
- Kullanıcı akış sayfalarında Şirket markası desteği eklendi.

**1.1.0**

- İptal uyarısı kaldırıldı
- Hata öğeleri için CSS sınıfı
- Hata mantığını gelişmiş göster/gizle
- Varsayılan CSS kaldırıldı

**1.0.0**

- İlk yayın

## <a name="unified-sign-in-sign-up-page-with-password-reset-link-unifiedssp"></a>Parola sıfırlama bağlantısı ile Birleşik oturum açma sayfası (unifiedssp)

**2.1.2**
- Ispanyolca ve Fransızca gibi diller için yerelleştirme kodlama sorunu düzeltildi.
- "Parolayı unuttum" bağlantısının, sosyal ıDP gibi talep değişim olarak kullanımına izin veriliyor.

**2.1.1**
- `heading` `intro` Sayfada başlık olarak görüntülenmek için öğesine ek olarak bir uxstring eklendi. Bu, varsayılan olarak gizlidir.
- Düzeni seçmek için ilkeyi veya QueryString parametresini kullanma desteği eklendi `pageFlavor` (klasik, oceanBlue veya slateGray).
- İCloud anahtarlığa parolaların kaydedilmesine yönelik destek eklendi.
- Artık birden çok alanda hata olduğunda odak ilk hata alanına yerleştirilir.
- Artık sayfa yüklenirken odak ilk düzenlenebilir alana yerleştirilir.
- Talep sağlayıcı seçim bağlantısı için yeni bir konum eklendi `bottomUnderFormClaimsProviderSelections` .
- Artık kullanılmayan UXStrings kaldırıldı.

**2.1.0**

- Birden çok kaydolma bağlantısı için destek eklendi.
- İlkede tanımlanan koşul kurallarına göre Kullanıcı girişi doğrulaması için destek eklendi.

**1.2.0**

- Username/email ve Password alanları artık `form` Edge ve Internet Explorer 'ın (IE) bu bilgileri düzgün bir şekilde kaydetmesine izin vermek için HTML öğesini kullanır.
- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](customize-ui-with-html.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerilere sahip olur.
- Chrome çevirisi desteği.
- Kullanıcı akış sayfalarında kiracı markalaması için destek eklendi.

**1.1.0**

- Oturumumu Açık tut (KMSı) denetimi eklendi

**1.0.0**

- İlk yayın

## <a name="mfa-page-multifactor"></a>MFA sayfası (çok faktörlü)

**1.2.2**
- İOS kullanılırken doğrulama kodu otomatik olarak doldurulmayla ilgili bir sorun düzeltildi.
- Android Web görünümünden bağlı olan tarafa belirteç yönlendirilirken bir sorun düzeltildi. 
- `heading` `intro` Sayfada başlık olarak görüntülenmek için öğesine ek olarak bir uxstring eklendi. Bu, varsayılan olarak gizlidir.  
- Düzeni seçmek için ilkeyi veya QueryString parametresini kullanma desteği eklendi `pageFlavor` (klasik, oceanBlue veya slateGray).

**1.2.1**

- Varsayılan şablonlarda erişilebilirlik düzeltmeleri

**1.2.0**

- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](customize-ui-with-html.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği.
- Kullanıcı akış sayfalarında kiracı markalaması için destek eklendi.

**1.1.0**

- ' Kodu onayla ' düğmesi kaldırıldı
- Kod için giriş alanı artık altı (6) karaktere kadar olan girişleri alır
- Sayfa, 6 basamaklı bir kod girildiğinde, tıklanmasına gerek kalmadan, otomatik olarak girilen kodu doğrulamaya çalışır
- Kod yanlışsa, giriş alanı otomatik olarak temizlenir
- Üç (3) yanlış kodla çalıştıktan sonra, B2C bağlı olan tarafa geri bir hata gönderir
- Erişilebilirlik düzeltmeleri
- Varsayılan CSS kaldırıldı

**1.0.0**

- İlk yayın

## <a name="exception-page-globalexception"></a>Özel durum sayfası (globalexception)

**1.2.0**

- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](customize-ui-with-html.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.1.0**

- Erişilebilirlik onarımı
- İlkeden hiçbir kişi olmadığında varsayılan ileti kaldırılmıştır
- Varsayılan CSS kaldırıldı

**1.0.0**

- İlk yayın

## <a name="other-pages-providerselection-claimsconsent-unifiedssd"></a>Diğer sayfalar (ProviderSelection, Claimsonay, UnifiedSSD)

**1.2.0**

- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](customize-ui-with-html.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.0.0**

- İlk yayın

## <a name="next-steps"></a>Sonraki adımlar

Özel ilkelerde uygulamalarınızın Kullanıcı arabirimini özelleştirmeye ilişkin ayrıntılar için bkz. [özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme](customize-ui-with-html.md).
