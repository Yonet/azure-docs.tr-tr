---
title: Öneri oluşturucu oluşturma
titleSuffix: Azure Cognitive Search
description: Otomatik tamamlama veya otomatik önerilmiş sorgu koşulları çağıran istekleri öneri araçları ve formül oluşturarak Azure Bilişsel Arama 'de yazma öncesi sorgu eylemlerini etkinleştirin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/24/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 4390291eb96c11b8fb7fdb48eb92abaf802b80c0
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030790"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>Sorgu için otomatik tamamlamayı ve önerilen sonuçları etkinleştirmek üzere bir öneri aracı oluşturun

Azure Bilişsel Arama 'de, "sizin yazarken arama" bir *öneri aracı* aracılığıyla etkinleştirilir. Bir öneri aracı, alanlar koleksiyonundan oluşan bir iç veri yapısıdır. Alanlar, kısmi şartlarda eşleşmeleri desteklemek için önek dizileri oluşturarak ek simgeleştirme özelliğine sahiptir.

Örneğin, bir öneri aracı bir şehir alanı içeriyorsa, "Seattle" terimi için "Sea", "koltuk", "Seatt" ve "seattl" sonuç ön eki birleşimleri oluşturulur. Ön ekler, bir öneri aracı Fields koleksiyonunda belirtilen her alan için bir tane ters dizinler halinde depolanır.

## <a name="typeahead-experiences-in-cognitive-search"></a>Bilişsel Arama 'de typeahead deneyimleri

Bir öneri aracı iki deneyimi destekler: *otomatik tamamlama*, bir dönem sorgusunun tamamına yönelik kısmi bir girişi tamamlar ve davet eden *öneriler* ile belirli bir eşleştirmeye kadar tıklamıştır. AutoComplete bir sorgu oluşturur. Öneriler, eşleşen bir belge oluşturur.

[C# ' de ilk uygulamanızı oluşturma bölümündeki](tutorial-csharp-type-ahead-and-suggestions.md) aşağıdaki ekran görüntüsünde her ikisi de gösterilmektedir. Otomatik tamamlama anticipates olası bir dönem, "tw" ile "ın" ile tamamlanıyor. Otel adı gibi bir alan, dizinden eşleşen bir otel arama belgesini temsil ettiğinde, öneriler mini arama sonuçlardır. Öneriler için, açıklayıcı bilgiler sağlayan herhangi bir alanı yüzeysel yapabilirsiniz.

![Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması")

Bu özellikleri ayrı olarak veya birlikte kullanabilirsiniz. Bu davranışları Azure Bilişsel Arama uygulamak için bir dizin ve sorgu bileşeni vardır. 

+ Arama dizini tanımına bir öneri aracı ekleyin. Bu makalenin geri kalanında bir öneri aracı oluşturmaya odaklanılmıştır.

+ [Aşağıda listelenen API](#how-to-use-a-suggester)'lerden birini kullanarak bir öneri Isteği veya AutoComplete isteği biçiminde öneri aracı etkin bir sorgu çağırın.

İsteğe bağlı arama türü, dize alanları için alan temelinde desteklenir. Ekran görüntüsünde gösterilenle benzer bir deneyim istiyorsanız, aynı arama çözümü içinde her iki typeahead davranışını da uygulayabilirsiniz. Her iki istek de, bir Kullanıcı en az üç karakter girişi dizesi sağladıktan sonra, belirli dizin ve yanıtların *belge* koleksiyonunu hedefler.

## <a name="how-to-create-a-suggester"></a>Öneri aracı oluşturma

Bir öneri aracı oluşturmak için bir [Dizin tanımına](/rest/api/searchservice/create-index)bir tane ekleyin. Bir öneri aracı, typeahead deneyiminin etkinleştirildiği bir ad ve alan koleksiyonu alır. ve [her bir özelliği ayarlayın](#property-reference). Bir öneri aracı oluşturmak için en iyi zaman, onu kullanacak alanı da tanımlamanız durumunda olur.

+ Yalnızca dize alanlarını kullanın.

+ Dize alanı bir karmaşık türün parçasıysa (örneğin, adres içindeki bir şehir alanı), üst öğeyi şu alana ekleyin: `"Address/City"` (REST ve C# ve Python) veya `["Address"]["City"]` (JavaScript).

+ Alanda varsayılan standart Lucene Analyzer ( `"analyzer": null` ) veya bir [dil Çözümleyicisi](index-add-language-analyzers.md) (örneğin,) kullanın `"analyzer": "en.Microsoft"` .

Önceden var olan alanları kullanarak bir öneri aracı oluşturmaya çalışırsanız, API buna izin vermez. Ön ekler, iki veya daha fazla karakter kombinasyondaki kısmi koşullar tüm koşullara göre simgeleştirilir, dizin oluşturma sırasında oluşturulur. Mevcut alanlar zaten simgeleştirilmiş olduğunda, bir öneri aracı eklemek istiyorsanız dizini yeniden oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure bilişsel arama dizinini yeniden oluşturma](search-howto-reindex.md).

### <a name="choose-fields"></a>Alanları seçin

Bir öneri aracı birçok özelliğe sahip olsa da, öncelikle bir arama türü deneyiminizin etkinleştirilebileceği dize alanları koleksiyonudur. Her dizin için bir öneri aracı bulunur, bu nedenle öneri aracı listesi hem öneriler hem de otomatik tamamlama için içerik katkıda bulunan tüm alanları içermelidir.

Ek içeriğin daha fazla terim tamamlanma olasılığı olduğu için, öğesinden daha büyük bir alan için otomatik tamamlama avantajları.

Öneriler, diğer yandan, alan seçiminiz seçmeli olduğunda daha iyi sonuçlar üretir. Önerinizin bir arama belgesi için bir ara sunucu olduğunu unutmayın, böylece tek bir sonucu en iyi temsil eden alanları isteyeceksiniz. Adlar, başlıklar veya birden çok eşleşme arasında ayrım yapan diğer benzersiz alanlar en iyi şekilde çalışır. Alanlar Yinelenen değerlerden oluşur, öneriler aynı sonuçlardan oluşur ve bir Kullanıcı hangisinin tıklayabileceklerini bilmez.

Her iki arama türü deneyiminden de faydalanmak için, otomatik tamamlama için ihtiyacınız olan tüm alanları ekleyin, ancak **$Select**, **$top**, **$Filter** ve **searchfields** değerlerini kullanarak önerilerin sonuçlarını denetleyin.

### <a name="choose-analyzers"></a>Çözümleyiciler seçin

Bir çözümleyici seçiminiz, alanların nasıl simgeleştirilmiş ve daha sonra önekli olduğunu belirler. Örneğin, "bağlama duyarlı" gibi bir hecelenmiş dize için, dil Çözümleyicisi kullanmak bu belirteç kombinasyonlarına neden olur: "bağlam", "hassas", "bağlama duyarlı". Standart Lucene Çözümleyicisi 'ni kullandınız, hecelenmiş dize yok. 

Çözümleyiciler değerlendirirken, koşulların nasıl işlendiği hakkında Öngörüler için [metin ANALIZI API](/rest/api/searchservice/test-analyzer) 'sini kullanmayı göz önünde bulundurun. Bir dizin oluşturduktan sonra, belirteç çıkışını görüntülemek için bir dizedeki çeşitli Çözümleyicileri deneyebilirsiniz.

[Özel çözümleyiciler](index-add-custom-analyzers.md) veya [önceden tanımlanmış çözümleyiciler](index-add-custom-analyzers.md#predefined-analyzers-reference) kullanan alanlar (Standart Lucene hariç), zayıf sonuçları engellemek için açıkça izin verilmez.

> [!NOTE]
> Çözümleyici kısıtlamasından çalışmanız gerekiyorsa, örneğin belirli sorgu senaryolarında bir anahtar sözcük veya Ngram Çözümleyicisi gerekiyorsa, aynı içerik için iki ayrı alan kullanmanız gerekir. Bu, diğer alanlardan birinin bir öneri aracı sahip olmasını sağlar, ancak diğeri özel bir çözümleyici yapılandırmasıyla ayarlanacaktır.

## <a name="create-using-the-portal"></a>Portalı kullanarak oluşturma

Dizin oluşturmak için **dizin ekleme** veya **veri alma** Sihirbazı 'nı kullanırken, bir öneri aracı etkinleştirme seçeneğiniz vardır:

1. Dizin tanımında, öneri aracı için bir ad girin.

1. Yeni alanlar için her alan tanımında, öneri aracı sütununda bir onay kutusu seçin. Onay kutusu yalnızca dize alanlarında kullanılabilir. 

Daha önce belirtildiği gibi, çözümleyici seçimi simgeleştirme ve ön eklemeyi etkiler. Öneri araçları etkinleştirilirken alan tanımının tamamını göz önünde bulundurun. 

## <a name="create-using-rest"></a>REST kullanarak oluşturma

REST API [Dizin oluşturma](/rest/api/searchservice/create-index) veya [güncelleştirme dizini](/rest/api/searchservice/update-index)aracılığıyla öneri araçları ekleyin. 

  ```json
  {
    "name": "hotels-sample-index",
    "fields": [
      . . .
          {
              "name": "HotelName",
              "type": "Edm.String",
              "facetable": false,
              "filterable": false,
              "key": false,
              "retrievable": true,
              "searchable": true,
              "sortable": false,
              "analyzer": "en.microsoft",
              "indexAnalyzer": null,
              "searchAnalyzer": null,
              "synonymMaps": [],
              "fields": []
          },
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["HotelName"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

## <a name="create-using-net"></a>.NET kullanarak oluşturma

C# ' de, bir [SearchSuggester nesnesi](/dotnet/api/azure.search.documents.indexes.models.searchsuggester)tanımlayın. `Suggesters` , bir Searchındex nesnesi üzerinde bir koleksiyon, ancak yalnızca bir öğe alabilir. Dizin tanımına bir öneri aracı ekleyin.

```csharp
private static void CreateIndex(string indexName, SearchIndexClient indexClient)
{
    FieldBuilder fieldBuilder = new FieldBuilder();
    var searchFields = fieldBuilder.Build(typeof(Hotel));

    var definition = new SearchIndex(indexName, searchFields);

    var suggester = new SearchSuggester("sg", new[] { "HotelName", "Category", "Address/City", "Address/StateProvince" });
    definition.Suggesters.Add(suggester);

    indexClient.CreateOrUpdateIndex(definition);
}
```

## <a name="property-reference"></a>Özellik başvurusu

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        | Öneri aracı tanımında belirtilmiştir, ancak otomatik tamamlama veya öneriler isteği üzerinde de çağırılır. |
|`sourceFields`| Öneri aracı tanımında belirtilmiştir. Bu, içeriğin kaynağı olan dizindeki bir veya daha fazla alanın listesidir. Alanların ve türünde olması gerekir `Edm.String` `Collection(Edm.String)` . Alanda bir çözümleyici belirtilmişse, [Bu listeden](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzername) bir adlandırılmış sözlü Çözümleyicisi olmalıdır (özel çözümleyici değil).<p/> En iyi uygulama olarak, bir arama çubuğunda veya açılan listede bir tamamlanmış dize olup olmadığı için, yalnızca beklenen ve uygun bir yanıta ödünç veren alanları belirtin.<p/>Bir otel adı duyarlık içerdiğinden iyi bir adaydır. Açıklamalar ve açıklamalar gibi ayrıntılı alanlar çok yoğun. Benzer şekilde, Kategoriler ve Etiketler gibi yinelenen alanlar daha az etkilidir. Örneklerde, birden fazla alanı dahil etbileceğinizi göstermek için, "Category" de yer alır. |
|`searchMode`  | Yalnızca REST parametresi, portalda da görünür. Bu parametre .NET SDK 'da kullanılamaz. Aday tümcecikleri aramak için kullanılan stratejiyi gösterir. Şu anda desteklenen tek mod, `analyzingInfixMatching` bir terimin başlangıcında Şu anda eşleşen.|

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>Öneri aracı kullanma

Bir sorguda bir öneri aracı kullanılır. Bir öneri aracı oluşturulduktan sonra, bir arama türü deneyimi için aşağıdaki API 'lerden birini çağırın:

+ [Öneriler REST API](/rest/api/searchservice/suggestions)
+ [Otomatik tamamlama REST API](/rest/api/searchservice/autocomplete)
+ [Mümütasync yöntemi](/dotnet/api/azure.search.documents.searchclient.suggestasync)
+ [Oto Tamteasync yöntemi](/dotnet/api/azure.search.documents.searchclient.autocompleteasync)

Bir arama uygulamasında, istemci kodu, kısmi sorguyu toplamak ve eşleşmeyi sağlamak için [jQuery UI AutoComplete](https://jqueryui.com/autocomplete/) gibi bir kitaplıktan faydalanmalıdır. Bu görev hakkında daha fazla bilgi için bkz. [otomatik tamamlama veya önerilen sonuçları istemci koduna ekleme](search-autocomplete-tutorial.md).

API kullanımı, AutoComplete REST API aşağıdaki çağrısında gösterilmiştir. Bu örnekte iki örnek vardır. İlk olarak, tüm sorgularda olduğu gibi, işlem bir dizinin belgeler koleksiyonuna karşı yapılır ve sorgu, bu örnekte kısmi sorgu sağlayan bir **arama** parametresi içerir. İkinci olarak, isteğe **suggesterName** eklemeniz gerekir. Dizinde tanımlı bir öneri aracı yoksa otomatik tamamlama veya önerilere çağrı başarısız olur.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2020-06-30
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>Örnek kod

+ [C# ' de ilk uygulamanızı oluşturma (Ders 3-bir arama-yazma)](tutorial-csharp-type-ahead-and-suggestions.md) örneği örnek, önerilen sorguları, otomatik tamamlamayı ve çok yönlü gezintiyi gösterir. Bu kod örneği, korumalı alan Azure Bilişsel Arama hizmetinde çalışır ve önceden yüklenmiş bir öneri aracı dizini kullanır. bu nedenle, tüm yapmanız gerekir, uygulamayı çalıştırmak için F5 'e basın. Abonelik ya da oturum açma gerekli değildir.

## <a name="next-steps"></a>Sonraki adımlar

İsteklerin formül oluşturma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye önerilir.

> [!div class="nextstepaction"]
> [İstemci koduna otomatik tamamlama ve öneriler ekleme](search-autocomplete-tutorial.md)