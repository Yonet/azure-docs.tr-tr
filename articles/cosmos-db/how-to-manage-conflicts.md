---
title: Azure Cosmos DB bölgeler arasındaki çakışmaları yönetme
description: Son yazıcı-WINS veya özel bir çakışma çözümleme ilkesi oluşturarak Azure Cosmos DB çakışmaların nasıl yönetileceğini öğrenin
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 06/11/2020
ms.author: anfeldma
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: 8f98c2201159350f5774f4d2b05102384f31f3af
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339351"
---
# <a name="manage-conflict-resolution-policies-in-azure-cosmos-db"></a>Azure Cosmos DB çakışma çözümleme ilkelerini yönetme
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Çok bölgeli yazmalarda, birden fazla istemci aynı öğeye yazdığında çakışmalar oluşabilir. Bir çakışma oluştuğunda, farklı çakışma çözümleme ilkeleri kullanarak çakışmayı çözebilirsiniz. Bu makalede, çakışma çözümleme ilkelerinin nasıl yönetileceği açıklanır.

## <a name="create-a-last-writer-wins-conflict-resolution-policy"></a>Son yazıcı oluşturma-WINS çakışma çözümü ilkesi

Bu örnekler, son yazıcı WINS çakışma çözümleme ilkesiyle bir kapsayıcının nasıl ayarlanacağını gösterir. Son-yazıcı için varsayılan yol, zaman damgası alanı veya `_ts` özelliğidir. SQL API 'SI için bu, sayısal türde Kullanıcı tanımlı bir yola de ayarlanabilir. Çakışma durumunda en yüksek değer kazanır. Yol ayarlanmamışsa veya geçersizse, varsayılan olarak olur `_ts` . Bu ilkeyle çözümlenen çakışmalar çakışma akışında gösterilmez. Bu ilke, tüm API 'Ler tarafından kullanılabilir.

### <a name="net-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>.NET SDK

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/myCustomId",
      },
  });
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.lwwCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.LastWriterWins,
            ResolutionPath = "/myCustomId",
        }
    });
```
---

### <a name="java-v4-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-javav4"></a> Java v4 SDK 'Sı

# <a name="async"></a>[Eş](#tab/api-async)

   Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=ManageConflictResolutionLWWAsync)]

# <a name="sync"></a>[Eşitle](#tab/api-sync)

   Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/sync/SampleDocumentationSnippets.java?name=ManageConflictResolutionLWWSync)]

--- 

### <a name="java-v2-sdks"></a><a id="create-custom-conflict-resolution-policy-lww-javav2"></a>Java v2 SDK 'Ları

# <a name="async-java-v2-sdk"></a>[Zaman uyumsuz Java v2 SDK](#tab/async)

[Async Java v2 SDK](sql-api-sdk-async-java.md) (Maven [com. Microsoft. Azure:: Azure-cosmosdb](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb))

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

# <a name="sync-java-v2-sdk"></a>[Java v2 SDK Eşitle](#tab/sync)

[Java v2 SDK 'Sını Eşitle](sql-api-sdk-java.md) (Maven [com. Microsoft. Azure:: Azure-DocumentDB](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb))

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
lwwCollection.setConflictResolutionPolicy(lwwPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, lwwCollection);
```
---

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: lwwContainer } = await database.containers.createIfNotExists(
  {
    id: this.lwwContainerName,
    conflictResolutionPolicy: {
      mode: "LastWriterWins",
      conflictResolutionPath: "/myCustomId"
    }
  }
);
```

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-python"></a>Python SDK'sı

```python
udp_collection = {
    'id': self.udp_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'LastWriterWins',
        'conflictResolutionPath': '/myCustomId'
    }
}
udp_collection = self.try_create_document_collection(
    create_client, database, udp_collection)
```

## <a name="create-a-custom-conflict-resolution-policy-using-a-stored-procedure"></a>Saklı yordam kullanarak özel bir çakışma çözümleme ilkesi oluşturma

Bu örnekler çakışmayı çözümlemek için saklı yordama içeren özel çakışma çözümleme ilkesine sahip bir kapsayıcı ayarlama adımlarını göstermektedir. Bu çakışmalar, saklı yordamınızdaki bir hata olmadığı takdirde çakışma akışında gösterilmez. İlke kapsayıcı ile oluşturulduktan sonra, saklı yordamı oluşturmanız gerekir. Aşağıdaki .NET SDK örneği bir örnek gösterir. Bu ilke yalnızca çekirdek (SQL) API 'sinde desteklenir.

### <a name="sample-custom-conflict-resolution-stored-procedure"></a>Örnek özel çakışma çözümü saklı yordamı

Özel çakışma çözümü saklı yordamları aşağıda gösterilen işlev imzası kullanılarak uygulanmalıdır. İşlev adının, saklı yordamı kapsayıcı ile kaydedilirken kullanılan adla eşleşmesi gerekmez, ancak adlandırmayı basitleştirir. Bu saklı yordam için uygulanması gereken parametrelerin açıklaması aşağıdadır.

- **ıncomingıtem** : çakışmaların üretilerek işlemede eklenen veya güncellenen öğe. , Silme işlemleri için null.
- **existingıtem** : Şu anda taahhüt edilen öğe. Bu değer bir güncelleştirmede null değil ve INSERT veya delete için null.
- **ı# kaldırıldı** : ıncomingıtem 'ın daha önce silinmiş bir öğeyle çakışıp çakışmadığını belirten Boolean. Doğru olduğunda, existingItem da null olur.
- **Conflictingıtems** : KAPSAYıCıDA, ID veya diğer benzersiz dizin özelliklerindeki ıncomingıtem ile çakışan tüm öğelerin taahhüt edilen sürümünün dizisi.

> [!IMPORTANT]
> Her türlü saklı yordamda olduğu gibi, özel bir çakışma çözümü yordamı aynı bölüm anahtarına sahip tüm verilere erişebilir ve çakışmaları çözümlemek için herhangi bir ekleme, güncelleştirme veya silme işlemi gerçekleştirebilir.

Bu örnek saklı yordam, yoldan en düşük değeri seçerek çakışmaları çözer `/myCustomId` .

```javascript
function resolver(incomingItem, existingItem, isTombstone, conflictingItems) {
  var collection = getContext().getCollection();

  if (!incomingItem) {
      if (existingItem) {

          collection.deleteDocument(existingItem._self, {}, function (err, responseOptions) {
              if (err) throw err;
          });
      }
  } else if (isTombstone) {
      // delete always wins.
  } else {
      if (existingItem) {
          if (incomingItem.myCustomId > existingItem.myCustomId) {
              return; // existing item wins
          }
      }

      var i;
      for (i = 0; i < conflictingItems.length; i++) {
          if (incomingItem.myCustomId > conflictingItems[i].myCustomId) {
              return; // existing conflict item wins
          }
      }

      // incoming item wins - clear conflicts and replace existing with incoming.
      tryDelete(conflictingItems, incomingItem, existingItem);
  }

  function tryDelete(documents, incoming, existing) {
      if (documents.length > 0) {
          collection.deleteDocument(documents[0]._self, {}, function (err, responseOptions) {
              if (err) throw err;

              documents.shift();
              tryDelete(documents, incoming, existing);
          });
      } else if (existing) {
          collection.replaceDocument(existing._self, incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      } else {
          collection.createDocument(collection.getSelfLink(), incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      }
  }
}
```

### <a name="net-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-dotnet"></a>.NET SDK

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
DocumentCollection udpCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.udpCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
          ConflictResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver"),
      },
  });

//Create the stored procedure
await clients[0].CreateStoredProcedureAsync(
UriFactory.CreateStoredProcedureUri(this.databaseName, this.udpCollectionName, "resolver"), new StoredProcedure
{
    Id = "resolver",
    Body = File.ReadAllText(@"resolver.js")
});
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.udpCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.Custom,
            ResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver")
        }
    });

await container.Scripts.CreateStoredProcedureAsync(
    new StoredProcedureProperties("resolver", File.ReadAllText(@"resolver.js"))
);
```
---

### <a name="java-v4-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-javav4"></a> Java v4 SDK 'Sı

# <a name="async"></a>[Eş](#tab/api-async)

   Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=ManageConflictResolutionSprocAsync)]

# <a name="sync"></a>[Eşitle](#tab/api-sync)

   Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/sync/SampleDocumentationSnippets.java?name=ManageConflictResolutionSprocSync)]

--- 

### <a name="java-v2-sdks"></a><a id="create-custom-conflict-resolution-policy-stored-proc-javav2"></a>Java v2 SDK 'Ları

# <a name="async-java-v2-sdk"></a>[Zaman uyumsuz Java v2 SDK](#tab/async)

[Async Java v2 SDK](sql-api-sdk-async-java.md) (Maven [com. Microsoft. Azure:: Azure-cosmosdb](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb))

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy("resolver");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

# <a name="sync-java-v2-sdk"></a>[Java v2 SDK Eşitle](#tab/sync)

[Java v2 SDK 'Sını Eşitle](sql-api-sdk-java.md) (Maven [com. Microsoft. Azure:: Azure-DocumentDB](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb))

```java
DocumentCollection udpCollection = new DocumentCollection();
udpCollection.setId(this.udpCollectionName);
ConflictResolutionPolicy udpPolicy = ConflictResolutionPolicy.createCustomPolicy(
        String.format("dbs/%s/colls/%s/sprocs/%s", this.databaseName, this.udpCollectionName, "resolver"));
udpCollection.setConflictResolutionPolicy(udpPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, udpCollection);
```
---

Kapsayıcınız oluşturulduktan sonra `resolver` saklı yordamı oluşturmanız gerekir.

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

Kapsayıcınız oluşturulduktan sonra `resolver` saklı yordamı oluşturmanız gerekir.

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-python"></a>Python SDK'sı

```python
udp_collection = {
    'id': self.udp_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'Custom',
        'conflictResolutionProcedure': 'dbs/' + self.database_name + "/colls/" + self.udp_collection_name + '/sprocs/resolver'
    }
}
udp_collection = self.try_create_document_collection(
    create_client, database, udp_collection)
```

Kapsayıcınız oluşturulduktan sonra `resolver` saklı yordamı oluşturmanız gerekir.

## <a name="create-a-custom-conflict-resolution-policy"></a>Özel bir çakışma çözümleme ilkesi oluşturma

Bu örnekler özel çakışma çözümleme ilkesine sahip bir kapsayıcı ayarlama adımlarını göstermektedir. Bu çakışmalar çakışma akışında görünür.

### <a name="net-sdk"></a><a id="create-custom-conflict-resolution-policy-dotnet"></a>.NET SDK

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
DocumentCollection manualCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.manualCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
      },
  });
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.manualCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.Custom
        }
    });
```
---

### <a name="java-v4-sdk"></a><a id="create-custom-conflict-resolution-policy-javav4"></a> Java v4 SDK 'Sı

# <a name="async"></a>[Eş](#tab/api-async)

   Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=ManageConflictResolutionCustomAsync)]

# <a name="sync"></a>[Eşitle](#tab/api-sync)

   Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/sync/SampleDocumentationSnippets.java?name=ManageConflictResolutionCustomSync)]

--- 

### <a name="java-v2-sdks"></a><a id="create-custom-conflict-resolution-policy-javav2"></a>Java v2 SDK 'Ları

# <a name="async-java-v2-sdk"></a>[Zaman uyumsuz Java v2 SDK](#tab/async)

[Async Java v2 SDK](sql-api-sdk-async-java.md) (Maven [com. Microsoft. Azure:: Azure-cosmosdb](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb))

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy();
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

# <a name="sync-java-v2-sdk"></a>[Java v2 SDK Eşitle](#tab/sync)

[Java v2 SDK 'Sını Eşitle](sql-api-sdk-java.md) (Maven [com. Microsoft. Azure:: Azure-DocumentDB](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb))

```java
DocumentCollection manualCollection = new DocumentCollection();
manualCollection.setId(this.manualCollectionName);
ConflictResolutionPolicy customPolicy = ConflictResolutionPolicy.createCustomPolicy(null);
manualCollection.setConflictResolutionPolicy(customPolicy);
DocumentCollection createdCollection = client.createCollection(database.getSelfLink(), collection, null).getResource();
```
---

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const {
  container: manualContainer
} = await database.containers.createIfNotExists({
  id: this.manualContainerName,
  conflictResolutionPolicy: {
    mode: "Custom"
  }
});
```

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-python"></a>Python SDK'sı

```python
database = client.ReadDatabase("dbs/" + self.database_name)
manual_collection = {
    'id': self.manual_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'Custom'
    }
}
manual_collection = client.CreateContainer(database['_self'], collection)
```

## <a name="read-from-conflict-feed"></a>Çakışma akışından okuma

Bu örnekler, kapsayıcının çakışma akışından okuma yöntemlerini göstermektedir. Çakışmalar yalnızca otomatik olarak çözümlenmezse veya özel bir çakışma ilkesi kullanılıyorsa çakışma akışında gösterilir.

### <a name="net-sdk"></a><a id="read-from-conflict-feed-dotnet"></a>.NET SDK

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
FeedResponse<Conflict> conflicts = await delClient.ReadConflictFeedAsync(this.collectionUri);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

```csharp
FeedIterator<ConflictProperties> conflictFeed = container.Conflicts.GetConflictQueryIterator();
while (conflictFeed.HasMoreResults)
{
    FeedResponse<ConflictProperties> conflicts = await conflictFeed.ReadNextAsync();
    foreach (ConflictProperties conflict in conflicts)
    {
        // Read the conflicted content
        MyClass intendedChanges = container.Conflicts.ReadConflictContent<MyClass>(conflict);
        MyClass currentState = await container.Conflicts.ReadCurrentAsync<MyClass>(conflict, new PartitionKey(intendedChanges.MyPartitionKey));

        // Do manual merge among documents
        await container.ReplaceItemAsync<MyClass>(intendedChanges, intendedChanges.Id, new PartitionKey(intendedChanges.MyPartitionKey));

        // Delete the conflict
        await container.Conflicts.DeleteAsync(conflict, new PartitionKey(intendedChanges.MyPartitionKey));
    }
}
```
---

### <a name="java-v2-sdks"></a><a id="read-from-conflict-feed-javav2"></a>Java v2 SDK 'Ları

# <a name="async-java-v2-sdk"></a>[Zaman uyumsuz Java v2 SDK](#tab/async)

[Async Java v2 SDK](sql-api-sdk-async-java.md) (Maven [com. Microsoft. Azure:: Azure-cosmosdb](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb))

```java
FeedResponse<Conflict> response = client.readConflicts(this.manualCollectionUri, null)
                    .first().toBlocking().single();
for (Conflict conflict : response.getResults()) {
    /* Do something with conflict */
}
```
# <a name="sync-java-v2-sdk"></a>[Java v2 SDK Eşitle](#tab/sync)

[Java v2 SDK 'Sını Eşitle](sql-api-sdk-java.md) (Maven [com. Microsoft. Azure:: Azure-DocumentDB](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb))

```java
Iterator<Conflict> conflictsIterator = client.readConflicts(this.collectionLink, null).getQueryIterator();
while (conflictsIterator.hasNext()) {
    Conflict conflict = conflictsIterator.next();
    /* Do something with conflict */
}
```
---

### <a name="nodejsjavascripttypescript-sdk"></a><a id="read-from-conflict-feed-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const container = client
  .database(this.databaseName)
  .container(this.lwwContainerName);

const { result: conflicts } = await container.conflicts.readAll().toArray();
```

### <a name="python"></a><a id="read-from-conflict-feed-python"></a>Python

```python
conflicts_iterator = iter(client.ReadConflicts(self.manual_collection_link))
conflict = next(conflicts_iterator, None)
while conflict:
    # Do something with conflict
    conflict = next(conflicts_iterator, None)
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki Azure Cosmos DB kavramları hakkında bilgi edinin:

- [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
- [Uygulamalarınızda çok bölgeli yazmaları yapılandırma](how-to-multi-master.md)
- [İstemcileri çoklu barındırma için yapılandırma](how-to-manage-database-account.md#configure-multiple-write-regions)
- [Azure Cosmos DB hesabınızdan bölge ekleme veya kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
- [Uygulamalarınızda çok bölgeli yazma işlemleri](how-to-multi-master.md).
- [Bölümleme ve veri dağıtımı](partitioning-overview.md)
- [Azure Cosmos DB’de dizin oluşturma](index-policy.md)
