---
title: Azure Iletişim hizmetlerinde takımlar birlikte çalışabilirliği ile çalışmaya başlama
titleSuffix: An Azure Communication Services quickstart
description: Bu hızlı başlangıçta, Azure Iletişim sohbeti istemci kitaplığı ile bir takım toplantısına nasıl katılacağınızı öğreneceksiniz.
author: askaur
ms.author: askaur
ms.date: 12/08/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 59e259df5d9e564e11dbebd416569727c3857632
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98932314"
---
# <a name="quickstart-join-your-chat-app-to-a-teams-meeting"></a>Hızlı başlangıç: sohbet uygulamanızı bir takımlar toplantısı 'na ekleme

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

JavaScript istemci kitaplığını kullanarak sohbet çözümünüzü Microsoft ekiplerine bağlayarak Azure Iletişim Hizmetleri 'ni kullanmaya başlayın. 

## <a name="prerequisites"></a>Önkoşullar 

1. Bir [takımlar dağıtımı](/deployoffice/teams-install). 
2. Çalışan bir [sohbet uygulaması](./get-started.md). 

## <a name="enable-teams-interoperability"></a>Takımların birlikte çalışabilirliğini etkinleştir 

Bir Konuk Kullanıcı olarak toplantıya katılan bir Iletişim Hizmetleri kullanıcısı, Toplantı sohbetine yalnızca takımlar toplantı çağrısına katıldığında erişebilir. Bir ekip hizmetine bir Iletişim Hizmetleri kullanıcısının nasıl ekleneceğini öğrenmek için [takımlar birlikte çalışma](../voice-video-calling/get-started-teams-interop.md) belgelerine bakın.

Bu özelliği kullanmak için her iki varlığın sahip olduğu kuruluşun bir üyesi olmanız gerekir.

[!INCLUDE [Join Teams meetings](./includes/meeting-interop-javascript.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](../create-communication-resource.md#clean-up-resources)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Sohbet Hero örneğine](../../samples/chat-hero-sample.md) göz atın
- [Sohbet 'in nasıl çalıştığı](../../concepts/chat/concepts.md) hakkında daha fazla bilgi edinin