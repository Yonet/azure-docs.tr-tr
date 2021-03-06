---
title: Azure IoT Hub buluttan cihaza seçenekleri | Microsoft Docs
description: Geliştirici Kılavuzu-buluttan cihaza iletişimler için doğrudan yöntemlerin, cihaz ikizi istenen özelliklerinin veya buluttan cihaza mesajların ne zaman kullanılacağı konusunda rehberlik.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: 10206aced4f38f4d157f46703aac2d28ec863274
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87319159"
---
# <a name="cloud-to-device-communications-guidance"></a>Buluttan cihaza iletişim Kılavuzu

IoT Hub, cihaz uygulamaları için bir arka uç uygulamasına işlevsellik sunan üç seçenek sunar:

* Sonucun hemen onayını gerektiren iletişimler için [doğrudan Yöntemler](iot-hub-devguide-direct-methods.md) . Doğrudan yöntemler genellikle bir fanı açma gibi cihazlarda etkileşimli denetim için kullanılır.

* Ikizi, uzun süre çalışan komutların, cihazı istenen belirli bir duruma koymaya yönelik [istenen özelliklerdir](iot-hub-devguide-device-twins.md) . Örneğin, telemetri gönderme aralığını 30 dakikaya ayarlayın.

* Cihaz uygulamasına tek yönlü bildirimler için [buluttan cihaza iletiler](iot-hub-devguide-messages-c2d.md) .

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Çeşitli buluttan cihaza iletişim seçeneklerinin ayrıntılı bir karşılaştırması aşağıda verilmiştir.

| Kategoriler | Doğrudan yöntemler | İkizi 'ın istenen özellikleri | Buluttan cihaza iletiler |
| ---------- | -------------- | ------------------------- | ------------------------ |
| Senaryo | Bir fanı açma gibi anında onay gerektiren komutlar. | Cihazı istenen belirli bir duruma koymak için uzun süredir çalışan komutlar. Örneğin, telemetri gönderme aralığını 30 dakikaya ayarlayın. | Cihaz uygulamasına tek yönlü bildirimler. |
| Veri akışı | İki yönlü. Cihaz uygulaması yönteme hemen yanıt verebilir. Çözüm arka ucu, isteği bağlamsal olarak sonucunu alır. | Tek yönlü. Cihaz uygulaması, özellik değişikliğine sahip bir bildirim alır. | Tek yönlü. Cihaz uygulaması iletiyi alır
| Dayanıklılık | Bağlantısı kesilen cihazlara bağlantı kurulmadı. Çözüm arka ucu, cihazın bağlı olmadığı bildirilir. | Özellik değerleri cihaz ikizi korunur. Cihaz, bir sonraki yeniden bağlantı noktasında okur. Özellik değerleri [IoT Hub sorgu diliyle](iot-hub-devguide-query-language.md)alınabilir. | İletiler, 48 saate kadar IoT Hub korunabilir. |
| Targets | **DeviceID**veya [işleri](iot-hub-devguide-jobs.md)kullanan birden çok cihaz kullanan tek bir cihaz. | **DeviceID**veya [işleri](iot-hub-devguide-jobs.md)kullanan birden çok cihaz kullanan tek bir cihaz. | **DeviceID**tarafından tek bir cihaz. |
| Boyut | En yüksek doğrudan Yöntem yük boyutu 128 KB 'tır. | İstenen en fazla özellik boyutu 32 KB 'tır. | En çok 64 KB ileti. |
| Frequency | Yüksek. Daha fazla bilgi için bkz. [IoT Hub sınırları](iot-hub-devguide-quotas-throttling.md). | Orta. Daha fazla bilgi için bkz. [IoT Hub sınırları](iot-hub-devguide-quotas-throttling.md). | Düşük. Daha fazla bilgi için bkz. [IoT Hub sınırları](iot-hub-devguide-quotas-throttling.md). |
| Protokol | MQTT veya AMQP kullanılarak kullanılabilir. | MQTT veya AMQP kullanılarak kullanılabilir. | Tüm protokollerde kullanılabilir. HTTPS kullanılırken cihaz yoklamalıdır. |

Aşağıdaki öğreticilerde doğrudan yöntemleri, istenen özellikleri ve buluttan cihaza iletileri nasıl kullanacağınızı öğrenin:

* [Doğrudan yöntemler kullanma](quickstart-control-device-node.md)
* [Cihazları yapılandırmak için istenen özellikleri kullanma](tutorial-device-twins.md) 
* [Buluttan cihaza iletileri gönderme](iot-hub-node-node-c2d.md)
