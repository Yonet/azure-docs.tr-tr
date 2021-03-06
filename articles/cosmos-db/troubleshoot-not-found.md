---
title: Bulunamayan Azure Cosmos DB özel durumların sorunlarını giderme
description: Bulunamayan özel durumları tanılamayı ve gidermeyi öğrenin.
author: j82w
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 7b112cc80984a761e780f134731476f9dff4f687
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99525780"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-not-found-exceptions"></a>Azure Cosmos DB bulunamadı özel durumları tanılama ve sorun giderme
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

HTTP durum kodu 404 kaynağın artık mevcut olmadığını gösterir.

## <a name="expected-behavior"></a>Beklenen davranış
Bir uygulamanın kod 404 beklediği ve senaryoyu doğru işlediği birçok geçerli senaryo vardır.

## <a name="a-not-found-exception-was-returned-for-an-item-that-should-exist-or-does-exist"></a>Var olan veya mevcut olması gereken bir öğe için bulunamadı özel durumu döndürüldü
Bir durum kodu 404, öğenin var olması veya mevcut olması durumunda döndürülmesinin olası nedenleri aşağıda verilmiştir.

### <a name="the-read-session-is-not-available-for-the-input-session-token"></a>Giriş oturumu belirteci için okuma oturumu kullanılamıyor

#### <a name="solution"></a>Çözüm:
1. Geçerli SDK 'nizi mevcut en son sürüme güncelleştirin. Bu özel hatanın en yaygın nedenleri en yeni SDK sürümlerinde düzeltildi.

### <a name="race-condition"></a>Yarış durumu
Birden çok SDK istemci örneği vardır ve yazma işleminden önce okuma gerçekleşti.

#### <a name="solution"></a>Çözüm:
1. Azure Cosmos DB için varsayılan hesap tutarlılığı, oturum tutarlılığı olur. Bir öğe oluşturulduğunda veya güncelleştirilirken, yanıt, okuma isteğinin bu değişikliğe sahip bir yinelemeden okumasını sağlamak için SDK örnekleri arasında geçirilemeyen bir oturum belirteci döndürür.
1. [Tutarlılık düzeyini](./consistency-levels.md) [daha güçlü bir düzeye](./consistency-levels.md)değiştirin.

### <a name="invalid-partition-key-and-id-combination"></a>Geçersiz bölüm anahtarı ve KIMLIK birleşimi
Bölüm anahtarı ve KIMLIK bileşimi geçerli değil.

#### <a name="solution"></a>Çözüm:
Yanlış kombinasyona neden olan uygulama mantığını düzeltemedi. 

### <a name="invalid-character-in-an-item-id"></a>Öğe KIMLIĞINDE geçersiz karakter
Öğe KIMLIĞINDE [geçersiz bir karakterle](/dotnet/api/microsoft.azure.documents.resource.id?preserve-view=true&view=azure-dotnet#remarks) Azure Cosmos DB bir öğe eklenir.

#### <a name="solution"></a>Çözüm:
KIMLIĞI özel karakterler içermeyen farklı bir değerle değiştirin. KIMLIĞI değiştirmek bir seçenek değilse, özel karakterlerin kaçış KIMLIĞINI Base64 olarak kodlayabilirsiniz. Base64, değiştirilmesini gerektiren geçersiz '/' karakterini içeren bir ad oluşturabilir.

KIMLIğIN kapsayıcıda zaten eklenmiş olan öğeler, ad tabanlı başvurular yerine RID değerleri kullanılarak değiştirilebilir.
```c#
// Get a container reference that uses RID values.
ContainerProperties containerProperties = await this.Container.ReadContainerAsync();
string[] selfLinkSegments = containerProperties.SelfLink.Split('/');
string databaseRid = selfLinkSegments[1];
string containerRid = selfLinkSegments[3];
Container containerByRid = this.cosmosClient.GetContainer(databaseRid, containerRid);

// Invalid characters are listed here.
//https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet&preserve-view=true#remarks
FeedIterator<JObject> invalidItemsIterator = this.Container.GetItemQueryIterator<JObject>(
    @"select * from t where CONTAINS(t.id, ""/"") or CONTAINS(t.id, ""#"") or CONTAINS(t.id, ""?"") or CONTAINS(t.id, ""\\"") ");
while (invalidItemsIterator.HasMoreResults)
{
    foreach (JObject itemWithInvalidId in await invalidItemsIterator.ReadNextAsync())
    {
        // Choose a new ID that doesn't contain special characters.
        // If that isn't possible, then Base64 encode the ID to escape the special characters.
        byte[] plainTextBytes = Encoding.UTF8.GetBytes(itemWithInvalidId["id"].ToString());
        itemWithInvalidId["id"] = Convert.ToBase64String(plainTextBytes).Replace('/', '!');

        // Update the item with the new ID value by using the RID-based container reference.
        JObject item = await containerByRid.ReplaceItemAsync<JObject>(
            item: itemWithInvalidId,
            ID: itemWithInvalidId["_rid"].ToString(),
            partitionKey: new Cosmos.PartitionKey(itemWithInvalidId["status"].ToString()));

        // Validating the new ID can be read by using the original name-based container reference.
        await this.Container.ReadItemAsync<ToDoActivity>(
            item["id"].ToString(),
            new Cosmos.PartitionKey(item["status"].ToString())); ;
    }
}
```

### <a name="time-to-live-purge"></a>Canlı temizleme süresi
Öğe, [yaşam süresi (TTL)](./time-to-live.md) özellik kümesine sahipti. TTL özelliğinin süresi aşıldığı için öğe temizlendi.

#### <a name="solution"></a>Çözüm:
Öğenin temizlenmeden kaçınmak için TTL özelliğini değiştirin.

### <a name="lazy-indexing"></a>Geç dizin oluşturma
[Yavaş dizin oluşturma](index-policy.md#indexing-mode) yakalanmamıştır.

#### <a name="solution"></a>Çözüm:
Dizin oluşturmanın, dizin oluşturma ilkesini yakalamak veya değiştirmek için bekleyin.

### <a name="parent-resource-deleted"></a>Üst kaynak silindi
İçinde öğenin bulunduğu veritabanı veya kapsayıcı silinmiş.

#### <a name="solution"></a>Çözüm:
1. Üst kaynağı [geri yükleyin](./configure-periodic-backup-restore.md#request-restore) veya kaynakları yeniden oluşturun.
1. Silinen kaynağı değiştirmek için yeni bir kaynak oluşturun.

### <a name="7-containercollection-names-are-case-sensitive"></a>7. kapsayıcı/koleksiyon adları büyük/küçük harfe duyarlıdır
Cosmos DB kapsayıcı/koleksiyon adları büyük/küçük harfe duyarlıdır.

#### <a name="solution"></a>Çözüm:
Cosmos DB bağlanırken tam adı kullandığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-dot-net-sdk.md) .
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md)için performans yönergeleri hakkında bilgi edinin.
* Java v4 SDK Azure Cosmos DB kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-java-sdk-v4-sql.md) .
* [Java v4 SDK](performance-tips-java-sdk-v4-sql.md)için performans yönergeleri hakkında bilgi edinin.
