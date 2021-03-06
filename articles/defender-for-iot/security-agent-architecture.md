---
title: Güvenlik aracılarına genel bakış
description: IoT hizmetinde Azure Defender 'da kullanılan aracılar için güvenlik Aracısı mimarisini anlayın.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: ff837fe88f878c522366b2b6bc19a1ef3954b667
ms.sourcegitcommit: 2501fe97400e16f4008449abd1dd6e000973a174
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99820662"
---
# <a name="security-agent-reference-architecture"></a>Güvenlik Aracısı başvuru mimarisi

IoT için Azure Defender, IoT Hub aracılığıyla güvenlik verilerini günlüğe kaydetmek, işlemek, toplamak ve göndermek için kullanılan güvenlik aracıları için başvuru mimarisi sağlar.

Güvenlik aracıları kısıtlanmış bir IoT ortamında çalışacak şekilde tasarlanmıştır ve kullandıkları kaynaklarla karşılaştırıldığında bunların sağladığı değer bakımından yüksek düzeyde özelleştirilebilir.

Güvenlik aracıları aşağıdaki özellikleri destekler:

- Mevcut cihaz kimliğiyle veya ayrılmış bir modül kimliğiyle kimlik doğrulaması yapın. Daha fazla bilgi için bkz. [Güvenlik Aracısı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md).

- Temel Işletim sisteminden (Linux, Windows) ham güvenlik olayları toplayın. Kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi edinmek için bkz. [Defender for IoT Aracısı yapılandırması](how-to-agent-configuration.md).

- Ham güvenlik olaylarını IoT Hub aracılığıyla gönderilen iletilere toplayın.

- **Azureiotsecurity** modülünün kullanımı üzerinden uzaktan yapılandırma ikizi. Daha fazla bilgi için bkz. [IoT Aracısı için bir Defender yapılandırma](how-to-agent-configuration.md).

IoT güvenlik aracıları için Defender, açık kaynaklı projeler olarak geliştirilmiştir ve GitHub 'dan kullanılabilir:

- [IoT C tabanlı aracı için Defender](https://github.com/Azure/Azure-IoT-Security-Agent-C)
- [IoT için Defender C# tabanlı aracı](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Aracılı desteklenen platformlar

IoT için Defender, 32 bit ve 64 bit Windows için farklı yükleyici aracıları ve 32 bit ve 64 bit Linux için de aynı şekilde sunulmaktadır. Aşağıdaki tabloya göre cihazlarınızın her biri için doğru aracı yükleyicisine sahip olduğunuzdan emin olun:

| Mimari | Linux | Windows | Ayrıntılar |
|--|--|--|--|
| 32 bit | C | C# |  |
| 64 bit | C# veya C | C# | Daha kısıtlı veya en az cihaz kaynağı olan cihazlar için C Aracısı kullanmanızı öneririz. |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT güvenlik modülü mimarisi ve kullanılabilir yükleyiciler için Defender hakkında üst düzey bir genel bakış aldınız.

IoT dağıtımı için Defender 'ı kullanmaya devam etmek için aşağıdaki makaleleri kullanın:

- [Güvenlik Aracısı kimlik doğrulama yöntemlerini](concept-security-agent-authentication-methods.md) anlama
- [Güvenlik aracısını](how-to-deploy-agent.md) seçme ve dağıtma
- IoT [Sistem önkoşulları](quickstart-system-prerequisites.md) Için Defender 'ı gözden geçirin
- [IoT Hub Için Defender 'ı nasıl etkinleştirebileceğinizi](quickstart-onboard-iot-hub.md) öğrenin
- [IoT Için Defender](resources-frequently-asked-questions.md) aracılığıyla hizmet hakkında daha fazla bilgi edinin
