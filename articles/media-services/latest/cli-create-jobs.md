---
title: Azure CLı betik örneği-iş oluşturma ve gönderme
description: Bu konudaki Azure CLI betiği, HTTPS URL’sini kullanarak bir İşi basit bir kodlama Dönüşümüne göndermeyi gösterir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 3e35ab78b83c0daa96e9b958d70f6e341cdcc537
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98891429"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI örneği: İş oluşturma ve gönderme

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Seçeneklerden biri, bir HTTPS URL 'sini iş girişi olarak belirtmektir (Bu makalede gösterildiği gibi). 

## <a name="prerequisites"></a>Önkoşullar 

[Media Services hesabı oluşturun](./create-account-howto.md).

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Örnek betik

Çalıştırdığınızda `az ams job start` , iş çıktısında bir etiket ayarlayabilirsiniz. Etiket daha sonra bu çıktı varlığının ne için olduğunu belirlemek için kullanılabilir. 

- Etikete bir değer atarsanız, '--output-varlıklarını ' ' assetname = Label ' olarak ayarlayın
- Etikete bir değer atamadıysanız, '--output-varlıklarını ' ' assetname = "olarak ayarlayın.
  ' A "=" eklemenize dikkat edin `output-assets` . 

```azurecli
az ams job start \
  --name testJob001 \
  --transform-name testEncodingTransform \
  --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' \
  --files 'Ignite-short.mp4' \
  --output-assets testOutputAssetName= \
  -a amsaccount \
  -g amsResourceGroup 
```

Aşağıdakine benzer bir yanıt alırsınız:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

## <a name="next-steps"></a>Sonraki adımlar

[az AMS Job (CLı)](/cli/azure/ams/job?view=azure-cli-latest)
