---
ms.openlocfilehash: 9c1b521a0f10da77295fd2457793566d787cb2cd
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89570101"
---
> [!div class="mx-imgBorder"]
> :::image type="content" source="../../../media/quickstarts/overview-qs4.svg" alt-text="Sinyal akışı":::

Yukarıdaki diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. [Sınır modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , gerçek zamanlı bir akış protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetim yapar. Bir [RTSP kaynak](../../../media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [hareket algılama işlemcisi](../../../media-graph-concept.md#motion-detection-processor) düğümüne video çerçeveleri gönderir. RTSP kaynağı bir olay tarafından tetiklenene kadar kapalı kalan bir [sinyal kapısı işlemci](../../../media-graph-concept.md#signal-gate-processor) düğümüne aynı video çerçevelerini gönderir.

Hareket algılama işlemcisi videoda hareket algıladığında, sinyal kapısı işlemci düğümüne bir olay gönderir ve bunu tetikler. Ağ Geçidi, yapılandırılan süre boyunca açılır ve video çerçevelerini [Dosya havuzu](../../../media-graph-concept.md#file-sink) düğümüne gönderir. Bu havuz düğümü, videoyu uç cihazınızın yerel dosya sisteminde bir MP4 dosyası olarak kaydeder. Dosya yapılandırılan konuma kaydedilir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.
