---
title: Sistem önkoşulları
description: IoT için Azure Defender 'ı çalıştırmak için gereken sistem önkoşullarını alın.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/30/2020
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: 8ee3afcae69ca6c082452e590eb8370bcc122af4
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97844515"
---
# <a name="system-prerequisites"></a>Sistem önkoşulları
Bu makalede IoT için Azure Defender çalıştırmaya yönelik sistem önkoşulları listelenmektedir.

## <a name="minimum-requirements"></a>Minimum gereksinimler

- Bağlantı noktası üzerinden trafik izlemeyi destekleyen ağ anahtarları.
- NTA sensörlerine yönelik donanım araçları.
- Azure aboneliği katkıda bulunan rolü. Yalnızca taahhüt edilen cihazları tanımlama ve Azure Sentinel ile bağlantı için ekleme sırasında gereklidir.
- Bulut bağlantılı yönetim için Azure IoT Hub (ücretsiz veya Standart katman) **katılımcısı** rolü. **IoT Için Azure Defender** özelliğinin etkinleştirildiğinden emin olun.
- Cihaz düzeyinde güvenlik modülü desteği için, IoT aracıları için Defender, artan cihaz ve platformların bir listesini destekler. [Desteklenen platformlar listesine](how-to-deploy-agent.md)bakın.

## <a name="supported-service-regions"></a>Desteklenen hizmet bölgeleri

IoT için Defender tüm Avrupa bölgelerinden gelen tüm trafiği Batı Avrupa bölgesel veri merkezine yönlendirir. Kalan tüm bölgelerden gelen trafiği Orta ABD bölgesel veri merkezine yönlendirir.

Daha fazla bilgi için bkz. [Desteklenen bölgeler IoT Hub](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).

## <a name="see-also"></a>Ayrıca bkz.

- [Gerekli gereçlerini tanımla](how-to-identify-required-appliances.md)
- [IoT Ağ kurulumu için Azure Defender hakkında](how-to-set-up-your-network.md)
