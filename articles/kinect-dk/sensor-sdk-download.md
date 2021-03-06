---
title: Azure Kinect algılayıcı SDK indirmesi
description: Windows ve Linux 'ta Azure Kinect algılayıcı SDK 'sını indirme ve yükleme hakkında bilgi edinin.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Azure, Kinect, SDK, indirme güncelleştirme, en son, kullanılabilir, yükleme
ms.openlocfilehash: 2fd14781c42192c713d826729f8fab6c698d6321
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505486"
---
# <a name="azure-kinect-sensor-sdk-download"></a>Azure Kinect algılayıcı SDK indirmesi

Bu sayfada, Azure Kinect algılayıcı SDK 'sının her bir sürümü için indirme bağlantıları bulunur. Yükleyici, Azure Kinect için geliştirmeye yönelik gereken tüm dosyaları sağlar.

## <a name="azure-kinect-sensor-sdk-contents"></a>Azure Kinect algılayıcı SDK içeriği

- Azure Kinect DK kullanarak uygulama derlemek için üst bilgiler ve kitaplıklar.
- Azure Kinect DK kullanan uygulamalar için yeniden dağıtılabilir DLL 'Ler gerekir.
- [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md).
- [Azure Kinect Kaydedicisi](azure-kinect-recorder.md).
- [Azure Kinect üretici yazılımı aracı](azure-kinect-firmware-tool.md).

## <a name="windows-installation-instructions"></a>Windows yükleme yönergeleri

Azure Kinect algılayıcı SDK 'sının en son ve önceki sürümleri için yükleme ayrıntılarını [buradan](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md)bulabilirsiniz.

Kaynak kodunu [buradan](https://github.com/microsoft/Azure-Kinect-Sensor-SDK)bulabilirsiniz.

> [!NOTE]
> SDK 'yı yüklerken, yüklediğiniz yolu unutmayın. Örneğin, "C:\Program Files\Azure Kinect SDK 1,2". Bu yoldaki makalelerde başvurulan araçları bulacaksınız.

## <a name="linux-installation-instructions"></a>Linux yükleme yönergeleri

Şu anda desteklenen tek dağıtım Ubuntu 18,04 ' dir. Diğer dağıtımlar için destek istemek için [Bu sayfaya](https://aka.ms/azurekinectfeedback)bakın.

İlk olarak, [buradaki](/windows-server/administration/linux-package-repository-for-microsoft-software)yönergeleri izleyerek [Microsoft 'un paket deposunu](https://packages.microsoft.com/)yapılandırmanız gerekecektir.

Artık gerekli paketleri yükleyebilirsiniz. `k4a-tools`Paket [Azure Kinect görüntüleyicisini](azure-kinect-viewer.md), [Azure Kinect kaydedicisini](record-sensor-streams-file.md)ve [Azure Kinect üretici yazılımı aracını](azure-kinect-firmware-tool.md)içerir. Yüklemek için şunu çalıştırın

 `sudo apt install k4a-tools`

 `libk4a<major>.<minor>-dev`Paket, oluşturulacak üst bilgileri ve CMake dosyalarını içerir `libk4a` .
Paket, bağlı `libk4a<major>.<minor>` olan yürütülebilir dosyaları çalıştırmak için gereken paylaşılan nesneleri içerir `libk4a` .

 Temel öğreticiler `libk4a<major>.<minor>-dev` paketini gerektirir. Yüklemek için şunu çalıştırın

 `sudo apt install libk4a1.1-dev`

Komut başarılı olursa SDK kullanıma hazırlanmaya devam edilir.

## <a name="change-log-and-older-versions"></a>Değişiklik günlüğü ve eski sürümler

Azure Kinect algılayıcı SDK 'sının değişiklik günlüğünü [buradan](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/CHANGELOG.md)edinebilirsiniz.

Azure Kinect algılayıcı SDK 'sının daha eski bir sürümüne ihtiyacınız varsa [burada](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md)bulun.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Kinect DK’yi kurma](set-up-azure-kinect-dk.md)