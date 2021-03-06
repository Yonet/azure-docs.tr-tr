---
title: Media Services V2-V3 geçiş örnekleri karşılaştırması
description: Azure Media Services V2 arasındaki kod farklılıklarını v3 olarak karşılaştırmanıza yardımcı olacak bir örnek kümesi.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2021
ms.author: inhenkel
ms.openlocfilehash: 640b9b40295ae9b9aea865f7b6159da6ff4a3251
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898316"
---
# <a name="media-services-migration-code-sample-comparison"></a>Media Services geçiş kodu örnek karşılaştırması

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

SDK 'lar arasında işlerin yapılma şeklini karşılaştırmak için bazı kod örneklerimizden yararlanabilirsiniz.

## <a name="samples-for-comparison"></a>Karşılaştırma örnekleri

Aşağıdaki tabloda, yaygın senaryolar için v2 ve v3 arasındaki karşılaştırma örneklerinin bir listesi verilmiştir.

|Senaryo|v2 API 'SI|v3 API 'SI|
|---|---|---|
|Bir varlık oluşturun ve bir dosyayı karşıya yükleyin |[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|İş gönder|[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>İlk olarak bir dönüştürme oluşturmayı ve sonra bir Işi göndermeyi gösterir.|
|AES şifrelemesi ile varlık yayımlama |1. oluştur `ContentKeyAuthorizationPolicyOption`<br/>2. oluştur `ContentKeyAuthorizationPolicy`<br/>3. oluştur `AssetDeliveryPolicy`<br/>4. `Asset` içerik oluşturma ve karşıya yükleme ya da gönderme `Job` ve kullanma `OutputAsset`<br/>5 `AssetDeliveryPolicy` . ilişkilendir `Asset`<br/>6. oluştur `ContentKey`<br/>7 `ContentKey` . iliştirme `Asset`<br/>8. oluştur `AccessPolicy`<br/>9. oluştur `Locator`<br/><br/>[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. oluştur `ContentKeyPolicy`<br/>2. oluştur `Asset`<br/>3. içeriği karşıya yükleyin veya `Asset` kullanın `JobOutput`<br/>4. oluştur `StreamingLocator`<br/><br/>[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|İş ayrıntılarını alma ve işleri yönetme |[V2 ile işleri yönetme](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[V3 ile işleri yönetme](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
