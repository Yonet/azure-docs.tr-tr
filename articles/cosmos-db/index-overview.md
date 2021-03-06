---
title: Azure Cosmos DB’de dizin oluşturma
description: Dizin oluşturmanın Azure Cosmos DB, Aralık, uzamsal, bileşik dizinler gibi farklı türlerde dizinlerin nasıl çalıştığını anlayın.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: tisande
ms.openlocfilehash: b7349a08b93810dcc3befd6058302d6c4573ab8d
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019338"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Azure Cosmos DB’de dizin oluşturma - Genel bakış
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB, şema veya dizin yönetimiyle uğraşmak zorunda kalmadan uygulamanızda yineleme yapmanızı sağlayan şemadan bağımsız bir veritabanıdır. Varsayılan olarak, Azure Cosmos DB herhangi bir şemayı tanımlamaya veya ikincil dizinleri yapılandırmaya gerek kalmadan, [kapsayıcıdaki](account-databases-containers-items.md#azure-cosmos-containers) tüm öğeler için her özelliği otomatik olarak dizine ekler.

Bu makalenin amacı Azure Cosmos DB'nin verileri nasıl dizine aldığını ve sorgu performansını geliştirmek için dizinleri nasıl kullandığını açıklamaktır. [Dizin oluşturma ilkelerinin](index-policy.md)nasıl özelleştirileceğine ilişkin araştırmadan önce bu bölümden gitmeniz önerilir.

## <a name="from-items-to-trees"></a>Öğelerden ağaçlara

Bir öğe kapsayıcıda her depolandığında, içeriği bir JSON belgesi olarak yansıtıldıktan sonra ağaç gösterimine dönüştürülür. Bu, söz konusu öğenin her özelliğinin bir ağaçta düğüm olarak temsil edildiği anlamına gelir. Sözde kök düğüm, öğenin tüm ilk düzey özelliklerine üst öğe olarak oluşturulur. Yaprak düğümleri bir öğe tarafından taşınan gerçek skaler değerleri içerir.

Örnek olarak, şu öğeyi göz önünde bulundurun:

```json
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

Aşağıdaki ağaç tarafından temsil edilir:

:::image type="content" source="./media/index-overview/item-as-tree.png" alt-text="Ağaç olarak temsil edilen önceki öğe" border="false":::

Dizilerin ağaçta nasıl kodlandığını unutmayın: dizideki her giriş dizideki bu girdinin diziniyle etiketlenmiş bir ara düğüm alır (0, 1 vb.).

## <a name="from-trees-to-property-paths"></a>Ağaçlardan Özellik yollarına

Azure Cosmos DB öğeleri ağaçlara dönüştürmesinin nedeni, özelliklerin bu ağaçlar içindeki yollarıyla başvurulmasını sağlar. Bir özelliğin yolunu almak için, ağacı kök düğümden bu özelliğe çapraz geçiş yapabilir ve her bir çapraz düğüm etiketini birleştirebilirsiniz.

Yukarıda açıklanan örnek öğeden her bir özelliğin yolları aşağıda verilmiştir:

- /Locations/0/Country: "Almanya"
- /Locations/0/City: "Berlin"
- /Locations/1/Country: "Fransa"
- /Locations/1/City: "Paris"
- /Headquarters/Country: "Belçika"
- /Headquarters/Employees: 250
- /EXPORTS/0/City: "Moskova"
- /EXPORTS/1/City: "Atina"

Bir öğe yazıldığında, Azure Cosmos DB her bir özelliğin yolunu ve karşılık gelen değerini etkin bir şekilde dizine ekler.

## <a name="types-of-indexes"></a><a id="index-types"></a>Dizin türleri

Azure Cosmos DB Şu anda üç tür dizini desteklemektedir. Dizin oluşturma ilkesini tanımlarken bu dizin türlerini yapılandırabilirsiniz.

### <a name="range-index"></a>Aralık dizini

**Aralık** dizini sıralı ağaç benzeri yapıyı temel alır. Aralık Dizin türü için kullanılır:

- Eşitlik sorguları:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
   ```

   ```sql
   SELECT * FROM c WHERE c.property IN ("value1", "value2", "value3")
   ```

   Bir dizi öğesinde eşitlik eşleşmesi
   ```sql
    SELECT * FROM c WHERE ARRAY_CONTAINS(c.tags, "tag1")
    ```

- Aralık sorguları:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ```
  (,,,,,,,, için geçerlidir `>` `<` `>=` `<=` `!=` )

- Bir özelliğin varlığı denetleniyor:

   ```sql
   SELECT * FROM c WHERE IS_DEFINED(c.property)
   ```

- Dize sistem işlevleri:

   ```sql
   SELECT * FROM c WHERE CONTAINS(c.property, "value")
   ```

   ```sql
   SELECT * FROM c WHERE STRINGEQUALS(c.property, "value")
   ```

- `ORDER BY` lardır

   ```sql
   SELECT * FROM container c ORDER BY c.property
   ```

- `JOIN` lardır

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Aralık dizinleri, skaler değerlerde (dize veya sayı) kullanılabilir. Yeni oluşturulan kapsayıcıların varsayılan dizin oluşturma ilkesi tüm dizeler veya sayılar için aralık dizinlerini zorunlu tutar. Aralık dizinlerini yapılandırma hakkında bilgi edinmek için bkz. [Aralık dizin oluşturma ilkesi örnekleri](how-to-manage-indexing-policy.md#range-index)

### <a name="spatial-index"></a>Uzamsal dizin

**Uzamsal** dizinler,-Points, çizgiler, çokgenler ve MultiPolygon gibi Jeo-uzamsal nesnelerde verimli sorgular sağlar. Bu sorgular ST_DISTANCE, ST_WITHIN, ST_INTERSECTS anahtar sözcüklerini kullanır. Aşağıda, uzamsal dizin türü kullanan bazı örnekler verilmiştir:

- Jeo-uzamsal uzaklık sorguları:

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Sorgular içindeki Jeo uzamsal:

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] })
   ```

- Jeo-uzamsal Kesiştirme sorguları:

   ```sql
   SELECT * FROM c WHERE ST_INTERSECTS(c.property, { 'type':'Polygon', 'coordinates': [[ [31.8, -5], [32, -5], [31.8, -5] ]]  })  
   ```

Uzamsal dizinler, doğru biçimli [geojson](./sql-query-geospatial-intro.md) nesnelerinde kullanılabilir. Noktaları, LineStrings, çokgenler ve MultiPolygon Şu anda desteklenmektedir. Bu dizin türünü kullanmak için, `"kind": "Range"` Dizin oluşturma ilkesini yapılandırırken özelliğini kullanarak ayarlayın. Uzamsal dizinleri yapılandırma hakkında bilgi edinmek için bkz. [uzamsal dizin oluşturma ilkesi örnekleri](how-to-manage-indexing-policy.md#spatial-index)

### <a name="composite-indexes"></a>Bileşik dizinler

**Bileşik** dizinler, birden çok alanda işlem gerçekleştirirken verimliliği artırır. Bileşik dizin türü için kullanılır:

- `ORDER BY` birden çok özelliklerde sorgular:

```sql
 SELECT * FROM container c ORDER BY c.property1, c.property2
```

- Filtre ve içeren sorgular `ORDER BY` . Bu sorgular, Filter özelliği yan tümcesine eklenirse bileşik bir dizin kullanabilir `ORDER BY` .

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' ORDER BY c.property1, c.property2
```

- En az bir özelliğin eşitlik filtresi olduğu iki veya daha fazla özelliğe filtre içeren sorgular

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' AND c.property2 > 'value'
```

Tek bir filtre koşulu Dizin türünden birini kullandığında, sorgu altyapısı ilk olarak kalanı taramadan önce bunu değerlendirir. Örneğin, gibi bir SQL sorgunuz varsa `SELECT * FROM c WHERE c.firstName = "Andrew" and CONTAINS(c.lastName, "Liu")`

* Yukarıdaki sorgu önce dizin kullanılarak firstName = "Andrew" olan girdileri filtreleyecek. Ardından, CONTAINS filtre koşulunu değerlendirmek için firstName = "Andrew" girdilerini sonraki bir işlem hattı aracılığıyla geçirin.

* Dizini kullanan ek filtre koşulları ekleyerek dizini (ör. IÇERIR) kullanmayan işlevleri kullanırken, sorguları hızlandırabilir ve tam kapsayıcı taramalarından kaçınabilirsiniz. Filter yan tümceleri sırası önemli değildir. Sorgu altyapısı, hangi koşulların daha seçmeli olduğunu anlayabilir ve sorguyu uygun şekilde çalıştıracaktır.

Bileşik dizinleri yapılandırma hakkında bilgi edinmek için bkz. [bileşik dizin oluşturma ilkesi örnekleri](how-to-manage-indexing-policy.md#composite-index)

## <a name="querying-with-indexes"></a>Dizinlerle sorgulama

Verileri dizinlerken ayıklanan yollar, bir sorgu işlenirken dizinde arama yapmayı kolaylaştırır. `WHERE`Dizinli yolların listesiyle bir sorgunun yan tümcesini eşleştirerek sorgu koşulunun çok çabuk eşleşen öğelerini tanımlamak mümkündür.

Örneğin, aşağıdaki sorguyu göz önünde bulundurun: `SELECT location FROM location IN company.locations WHERE location.country = 'France'` . Sorgu koşulu (herhangi bir konumda ülke/bölge olarak "Fransa" bulunduğu öğeler üzerinde filtreleme) aşağıdaki kırmızı renkle eşleşir:

:::image type="content" source="./media/index-overview/matching-path.png" alt-text="Ağaç içindeki belirli bir yolu eşleştirme" border="false":::

> [!NOTE]
> `ORDER BY`Tek bir özelliğe göre siparişlerin *her zaman* bir Aralık dizinine ihtiyacı olan ve başvurduğu yolun bir tane yoksa başarısız olacağı bir yan tümce. Benzer şekilde, `ORDER BY` birden çok özelliğe göre siparişlerin *her zaman* bir bileşik dizine ihtiyacı olan bir sorgu.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma ilkesi](index-policy.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)