---
title: Sınırlamalar
description: SDK özellikleri için kod sınırlamaları
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 68c0c04feba2779598a500c84b2ba4a9086b104d
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99593967"
---
# <a name="limitations"></a>Sınırlamalar

Birçok özellik boyut, sayı veya diğer sınırlamalara sahiptir.

## <a name="azure-frontend"></a>Azure ön ucu

Ön uç API 'SI için aşağıdaki sınırlamalar geçerlidir (C++ ve C#):
* İşlem başına toplam [Remoterenderingclient](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient) örneği: 16.
* [Remoterenderingclient](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient)başına toplam [renderingsession](/dotnet/api/microsoft.azure.remoterendering.renderingsession) örneği: 16.

## <a name="objects"></a>Nesneler

* Tek bir türdeki izin verilen toplam nesne ([Entity](../concepts/entities.md), [CutPlaneComponent](../overview/features/cut-planes.md), vb.): 16.777.215.
* İzin verilen toplam etkin kesme düzlemleri: 8.

## <a name="geometry"></a>Geometri

* **Animasyon:** Animasyonlar, [oyun nesnelerinin](../concepts/entities.md)bireysel dönüştürmelerini hareketlendirmek ile sınırlıdır. Kaplama veya köşe animasyonları olan iskelet animasyonları desteklenmez. Kaynak varlık dosyasından animasyon parçaları korunmaz. Bunun yerine, nesne dönüştürme animasyonlarını istemci kodu tarafından yönlendirilmelidir.
* **Özel gölgelendiriciler:** Özel gölgelendiriciler yazma desteklenmiyor. Yalnızca yerleşik [renk malzemeleri](../overview/features/color-materials.md) veya [PBR malzemeleri](../overview/features/pbr-materials.md) kullanılabilir.
* Bir varlığın **en fazla benzersiz malzeme sayısı** : 65.535. Otomatik malzeme sayısı azaltma hakkında daha fazla bilgi için bkz. [malzeme çoğaltmayı](../how-tos/conversion/configure-model-conversion.md#material-de-duplication) kaldırma bölümü.
* **Tek bir dokunun en büyük boyutu**: 16.384 x 16.384. Daha büyük kaynak dokuları, dönüştürme işlemi tarafından boyut olarak azaltılır.

### <a name="overall-number-of-polygons"></a>Toplam poligonu sayısı

Tüm yüklü modeller için izin verilen sayıda poligon, [oturum yönetim REST API](../how-tos/session-rest-api.md#create-a-session)geçirilen sanal makinenin boyutuna bağlıdır:

| Sunucu boyutu | Maksimum çokgen sayısı |
|:--------|:------------------|
|Stand| 20.000.000 |
|Premium| sınır yok |

Bu kısıtlamayla ilgili ayrıntılı bilgi için [sunucu boyutu](../reference/vm-sizes.md) bölümüne bakın.

## <a name="platform-limitations"></a>Platform sınırlamaları

**Windows 10 Masaüstü**

* Win32/x64 desteklenen tek Win32 platformudur. Win32/x86 desteklenmez.

**HoloLens 2**

* [BD kamerasından işleme](/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) özelliği desteklenmiyor.