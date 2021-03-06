---
title: Varlık dönüştürme REST API
description: REST API aracılığıyla bir varlığın nasıl dönüştürüleceğini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 889a70005f1cbabaad525147b4661ea04886138a
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445617"
---
# <a name="use-the-model-conversion-rest-api"></a>Model dönüştürme REST API’yi kullanma

[Model dönüştürme](model-conversion.md) hizmeti bir [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer)ile denetlenir. Bu makalede, dönüştürme hizmeti API 'SI ayrıntıları açıklanmaktadır.

## <a name="regions"></a>Bölgeler

İsteklerin gönderileceği temel URL 'Ler için [kullanılabilir bölgelerin listesine](../../reference/regions.md) bakın.

## <a name="common-headers"></a>Yaygın üst bilgiler

### <a name="common-request-headers"></a>Ortak istek üstbilgileri

Tüm istekler için bu üst bilgiler belirtilmelidir:

- **Yetkilendirme** üst bilgisi "taşıyıcı [ *belirteç* ]" değerine sahip olmalıdır, burada [ *belirteç* ] bir [hizmet erişim belirtecidir](../tokens.md).

### <a name="common-response-headers"></a>Ortak yanıt üst bilgileri

Tüm yanıtlar şu başlıkları içerir:

- **MS-CV** üst bilgisi, hizmet içindeki çağrıyı izlemek için kullanılabilecek benzersiz bir dize içerir.

## <a name="endpoints"></a>Uç Noktalar

Dönüştürme hizmeti şunları yapmak için üç REST API uç noktası sağlar:

- Azure uzaktan Işleme hesabınızla bağlantılı bir depolama hesabı kullanarak model dönüştürmeyi başlatın. 
- Belirtilen *paylaşılan erişim imzalarını (SAS)* kullanarak model dönüştürmeyi başlatın.
- dönüştürme durumunu sorgulama

### <a name="start-conversion-using-a-linked-storage-account"></a>Bağlı depolama hesabı kullanarak dönüştürmeyi Başlat
[Depolama hesaplarını bağlama](../create-an-account.md#link-storage-accounts)adımlarını Izleyerek Azure uzaktan işleme hesabınızın, belirtilen depolama hesabına erişimi olması gerekir.

| Uç Nokta | Yöntem |
|-----------|:-----------|
| /V1/accounts/ **AccountID** /Conversions/Create | POST |

JSON belgesinde kaydırılan, devam eden dönüştürmenin KIMLIĞINI döndürür. Alan adı "Conversionıd" dir.

#### <a name="request-body"></a>İstek gövdesi

> [!NOTE]
> `input.folderPath`Azure 'da dönüştürmeyi gerçekleştirmek için altındaki her şey alınacaktır. `input.folderPath`Belirtilmemişse, kapsayıcının tüm içeriği alınacaktır. Alınan tüm Bloblar ve klasörler [geçerli Windows dosya adlarına](/windows/win32/fileio/naming-a-file#naming-conventions)sahip olmalıdır.

```json
{
    "input":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container containing your input asset data>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the input blob container>",
        "inputAssetPath" : "<path to the model in the input blob container relative to the folderPath (or container root if no folderPath is specified)>"
    },
    "output":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container where the converted asset will be copied to>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the output blob container. Will contain the asset and log files>",
        "outputAssetFileName": "<optional: can be omitted or empty. The filename of the converted asset. If provided the filename needs to end in .arrAsset>"
    }
}
```
### <a name="start-conversion-using-provided-shared-access-signatures"></a>Belirtilen paylaşılan erişim imzalarını kullanarak dönüştürmeye başla
ARR hesabınız depolama hesabınıza bağlı değilse, bu REST arabirimi *paylaşılan erişim imzaları (SAS)* kullanarak erişim sağlamanıza olanak tanır.

| Uç Nokta | Yöntem |
|-----------|:-----------|
| /V1/accounts/ **AccountID** /Conversions/createwithsharedaccesssignature | POST |

JSON belgesinde kaydırılan, devam eden dönüştürmenin KIMLIĞINI döndürür. Alan adı `conversionId` .

#### <a name="request-body"></a>İstek gövdesi

İstek gövdesi, yukarıdaki REST çağrısını oluştur ile aynıdır, ancak giriş ve çıkış *paylaşılan erişim imzaları (SAS) belirteçlerini* içerir. Bu belirteçler, girişi okumak ve dönüştürme sonucunu yazmak için depolama hesabına erişim sağlar.

> [!NOTE]
> Bu SAS URI belirteçleri, tam URI değil, sorgu dizeleridir. 

> [!NOTE]
> `input.folderPath`Azure 'da dönüştürmeyi gerçekleştirmek için altındaki her şey alınacaktır. `input.folderPath`Belirtilmemişse, kapsayıcının tüm içeriği alınacaktır. Alınan tüm Bloblar ve klasörler [geçerli Windows dosya adlarına](/windows/win32/fileio/naming-a-file#naming-conventions)sahip olmalıdır.

```json
{
    "input":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container containing your input asset data>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the input blob container>",
        "inputAssetPath" : "<path to the model in the input blob container relative to the folderPath (or container root if no folderPath is specified)>",
        "containerReadListSas" : "<a container SAS token which gives read and list access to the given input blob container>"
    },
    "output":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container where the converted asset will be copied to>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the output blob container. Will contain the asset and log files>",
        "outputAssetFileName": "<optional: can be omitted or empty. The filename of the converted asset. If provided the filename needs to end in .arrAsset>",
        "containerWriteSas" : "<a container SAS token which gives write access to the given output blob container>"
    }
}
```

### <a name="poll-conversion-status"></a>Yoklama dönüştürme durumu
Yukarıdaki REST çağrılarından biriyle başlatılan devam eden bir dönüştürmenin durumu aşağıdaki arabirim kullanılarak sorgulanabilir:


| Uç Nokta | Yöntem |
|-----------|:-----------|
| /V1/accounts/ **AccountID** /Conversions/ **conversionıd** | GET |

Aşağıdaki değerlere sahip olabilir bir "durum" alanı olan bir JSON belgesi döndürür:

- Yaratıl
- Çalıştıran
- Başarılı
- Hataları

Durum "hata" ise, hata bilgilerini içeren "ileti" alt alanına sahip ek bir "hata" alanı olacaktır. Ek Günlükler, çıkış kapsayıcınıza yüklenecek.

## <a name="list-conversions"></a>Liste dönüştürmeleri

Bir hesap için tüm dönüştürmelerin bir listesini almak için arabirimini kullanın:

| Uç Nokta | Yöntem |
|-----------|:-----------|
| /V1/accounts/ **AccountID** /dönüşümler? skiptoken = **skiptoken** | GET |

| Parametre | Gerekli |
|-----------|:-----------|
| Accoun | Yes |
| skiptoken | No |

Dönüştürme dizisini ve bunların ayrıntılarını içeren bir JSON belgesi döndürür. Bu sorgu, tek seferde en fazla 50 dönüştürme döndürür. Alınacak daha fazla dönüştürme olduğu durumlarda, yanıt bir sonraki sonuç kümesini almak için sorgulanabilecek skipToken 'ı içeren bir **NextLink** özelliği içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürmesi için Azure Blob depolamayı kullanma](blob-storage.md)
- [Model dönüştürme](model-conversion.md)