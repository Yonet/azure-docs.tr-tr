---
title: Microsoft Threat Modeling Tool Release 1/29/2019
titleSuffix: Azure
description: 1/29/2019 ' de yayınlanan Microsoft Threat Modeling Tool için sürüm notlarını okuyun. Notlar, özellik değişikliklerini ve bilinen sorunları içerir.
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 01/25/2019
ms.openlocfilehash: 17147d412bd888cdd3cd270829ad6d6103867b34
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87539059"
---
# <a name="threat-modeling-tool-update-release-71601261---1292019"></a>Threat Modeling Tool güncelleştirme sürümü 7.1.60126.1 - 29.1.2019

Microsoft Threat Modeling Tool sürüm 7.1.60126.1 Ocak 29 2019 ' de yayımlanmıştır ve aşağıdaki değişiklikleri içerir:

- Gerekli en düşük .NET sürümü [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262)'a yükseltildi.
- .NET bağımlılığı nedeniyle gerekli en düşük Windows sürümü [Windows 10 yıldönümü güncelleştirmesine](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) yükseltildi.
- Araç seçenekleri menüsüne bir model doğrulama geçiş özelliği eklenmiştir.
- Tehdit özelliklerindeki birkaç bağlantı güncelleştirildi.
- Aracın giriş ekranında küçük UX değişiklikleri.
- Threat Modeling Tool artık ana bilgisayar işletim sisteminin TLS ayarlarını devralır ve TLS 1,2 veya üzerini gerektiren ortamlarda desteklenir.

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="model-validation-option"></a>Model doğrulama seçeneği

Müşteri geri bildirimlerine bağlı olarak, model doğrulamasını etkinleştirmek veya devre dışı bırakmak için araca bir seçenek eklenmiştir. Daha önce, şablonunuz iki nesne arasında tek bir tek yönlü veri akışı kullanıyorsa, Ileti çerçevesinde şunu belirten bir hata iletisi almış olabilirsiniz: ObjectsName en az bir ' any ' gerektirir. Model doğrulamayı devre dışı bırakmak, bu uyarıların görünümde gösterilmesini engeller.

Model doğrulamayı aç ve Kapat seçeneği, dosya >ayarları->Seçenekler menüsünde bulunabilir. Bu ayar için varsayılan değer devre dışı bırakıldı.

![Model doğrulama seçeneği](./media/threat-modeling-tool-releases-71601261/tmt_model_validation_option.png)

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- .NET sürümü gerekli
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek Gereksinimler
  - Araç ve şablonların güncelleştirmelerini almak için bir Internet bağlantısı gerekir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="unsupported-systems"></a>Desteklenmeyen sistemler

#### <a name="issue"></a>Sorun

Windows 10 Enterprise LTSB (sürüm 1507) gibi .NET 4.7.1 veya üstünü yükleyemeyecek Windows 10 sistemlerinin kullanıcıları, yükseltmeden sonra aracı açamaz. Windows 'un bu eski sürümleri artık Threat Modeling Tool için desteklenen platformlar değildir ve en son güncelleştirmeyi yüklememelidir.

#### <a name="workaround"></a>Geçici çözüm

En son güncelleştirmeyi yüklemiş olan Windows 10 Enterprise LTSB (sürüm 1507) kullanıcıları, uygulamalar & Özellikler 'deki kaldırma iletişim kutusu aracılığıyla Threat Modeling Tool önceki sürümüne geri dönebilir.

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Threat Modeling Tool belgeleri [docs.Microsoft.com](threat-modeling-tool.md)konumunda bulunur ve [Aracı kullanma hakkında](threat-modeling-tool-getting-started.md)bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)en son sürümünü indirin.
