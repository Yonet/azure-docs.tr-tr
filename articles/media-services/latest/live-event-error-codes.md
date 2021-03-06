---
title: Azure Media Services canlı olay hata kodları
description: Bu makalede, canlı olay hata kodları listelenmektedir.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: error-reference
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 310e70bc8c7ddbf3bcd37d4f0276fe04c2979e73
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98897858"
---
# <a name="media-services-live-event-error-codes"></a>Media Services canlı olay hata kodları

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu bölümdeki tablolarda, [canlı olay](live-events-outputs-concept.md) hata kodları listelenir.

## <a name="liveeventconnectionrejected"></a>Liveeventconnectionreddedildi

Canlı bir olay için [Event Grid](../../event-grid/index.yml) olaylarına abone olduğunuzda, [Liveeventconnectionreddedildi](media-services-event-schemas.md#liveeventconnectionrejected)  olayından aşağıdaki hatalardan birini görebilirsiniz.

| Sonuç kodu | Description |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Hatalı alma URL 'SI |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | IP izin verilenler listesinde kodlayıcı IP 'si yok |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | RTMP Kodlayıcısı setDataFrame komutu göndermedi. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Belirtilen codec desteklenmez. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED |Medya açıklaması bilgileri, gerçek medya verileri teslim edilmeden önce alınmadı.|
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED |Ses veya video türü için kaliteleri sayısı izin verilen üst sınırı aşıyor.|
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED |Canlı bir olay veya kanal hizmetindeki toplam gelen bit hızı izin verilen maksimum sınırı aştı.|
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | Video veya ses FLVTag zaman damgası, RTMP kodlayıcısından geçersizdir. |
| MPE_INGEST_FRAMERATE_EXCEEDED | Framerates ile gelen kodlayıcı alınan akışlar, canlı olayları/kanalları kodlamak için izin verilen en fazla 30fps sayısını aştı.|
| MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED | Gelen kodlayıcı alınan akışlar, şu izin verilen çözünürlükleri aştı: 1920x1088 for the Live Events/Channels ve 4096 x 2160 for pass-through canlı olaylar/kanallar.|
| MPE_INGEST_RTMP_TOO_LARGE_UNPROCESSED_FLV | Canlı olay, bir kerede büyük miktarda ses verisi veya anahtar çerçevesi olmayan büyük miktarda video verisi aldı. Doğru verilerle yeniden denenme şansı vermek için kodlayıcının bağlantısını kestik. |

## <a name="liveeventencoderdisconnected"></a>Liveeventencoderconnected bağlantısı kesildi

[Liveeventencoderconnected](media-services-event-schemas.md#liveeventencoderdisconnected) olayında aşağıdaki hatalardan birini görebilirsiniz.

|Sonuç kodu|Description|
|---|---|
|MPE_RTMP_SESSION_IDLE_TIMEOUT|RTMP oturumu izin verilen süre sınırı boyunca boşta kaldıktan sonra zaman aşımına uğradı.|
|MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID|Video veya ses FLVTag zaman damgası, RTMP kodlayıcısından geçersizdir.|
|MPE_CAPACITY_LIMIT_REACHED|Kodlayıcı verileri çok hızlı gönderiyor.|
|Bilinmeyen hata kodları|Bu hata kodları, karma eşlemesindeki girdileri yinelemek için bellek hatasından değişebilir.|


## <a name="see-also"></a>Ayrıca bkz.

[Akış uç noktası (başlangıç) hata kodları](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Media Services ile canlı akış](stream-live-tutorial-with-api.md)
