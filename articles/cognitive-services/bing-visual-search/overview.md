---
title: Bing Görsel Arama API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama bir resimle ilgili olarak benzer resimler veya alışveriş kaynakları gibi ayrıntılar veya içgörüler sağlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 12/19/2019
ms.author: scottwhi
ms.openlocfilehash: 7dfc704fb38550993adb7835d4500dee890117a8
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96486989"
---
# <a name="what-is-the-bing-visual-search-api"></a>Bing Görsel Arama API’si nedir?

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Görsel Arama API'si bir görüntü için öngörüleri döndürür. Bir görüntüyü karşıya yükleyebilir veya bir URL sağlayabilirsiniz. Öngörüler görsel olarak benzer görüntüler, alışveriş kaynakları, görüntü içeren Web sayfaları ve daha fazlasını içerir. Bing Görsel Arama API'si tarafından döndürülen Öngörüler, Bing.com/images 'de gösterilenlere benzerdir. 

[Bing resim arama API'si](../bing-image-search/overview.md)kullanıyorsanız, bir görüntüyü karşıya yüklemek yerine, bu API 'nin arama Bing Görsel Arama sonuçlarındaki öngörüleri belirteçlerini kullanabilirsiniz.

> [!IMPORTANT]
> Bing Resim Arama API'si kullanarak görüntü öngörüleri alırsanız, daha kapsamlı öngörüler sağlayan Bing Görsel Arama API'si geçmeyi göz önünde bulundurun.

## <a name="insights"></a>Insights

Bing Görsel Arama kullanarak aşağıdaki öngörüleri bulabilirsiniz:

| İçgörü                              | Açıklama |
|--------------------------------------|-------------|
| Görsel olarak benzer görüntüler              | Giriş resmine görsel olarak benzeyen görüntülerin listesi. |
| Görsel açıdan benzer ürünler            | Gösterilen ürüne görsel olarak benzeyen ürünler.            |
| Alışveriş kaynakları                     | Girdi görüntüsünde gösterilen öğeyi satın alabileceğiniz yer.            |
| İlgili aramalar                     | Başkaları tarafından yapılan veya görüntünün içeriğini temel alan ilgili aramalar.            |
| Görüntüyü içeren Web sayfaları     | Giriş görüntüsünü içeren Web sayfaları.            |
| Tarifler                              | Giriş görüntüsünde gösterilen çanağı oluşturmak için Tarifler içeren Web sayfaları.            |
| Varlıklar                             | İyi bilinen kişiler, Yerlerim ve şeyler. |

Bing Görsel Arama, öngörülere ek olarak, giriş görüntüsünden elde edilen çeşitli terimleri (Etiketler) döndürür. Etiketler, kullanıcıların görüntüde bulunan kavramları keşfetmesine olanak tanır. Örneğin, giriş görüntüsü faur Athlete ise etiketlerden biri Athlete adı olabilir, başka bir etiket de spor olabilir. Ya da, giriş görüntüsü bir Apple pasta ise, Etiketler Apple pasta, Pyalar ve teknoloji olabilir.

Bing Görsel Arama sonuçlar, görüntüde ilgilendiğiniz bölgelere yönelik sınırlayıcı kutuları da içerir. Örneğin, görüntüde birkaç ünlüler varsa, sonuçlar tanınan ünlülerin her biri için sınırlayıcı kutular içerebilir. Ya da Bing görüntüde bir ürün veya giyiği tanırsa, sonuç tanınan öğe için bir sınırlayıcı kutusu içerebilir.

## <a name="workflow"></a>İş akışı

Bing Görsel Arama API'si, yeniden bir Web hizmetidir ve HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır. Hizmet için REST API ya da SDK 'sını kullanabilirsiniz.

1. Bing Arama API'leri erişmek için bilişsel [Hizmetler hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/cognitive-services/).
2. Geçerli bir arama sorgusuyla API 'ye bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak Bing Görsel Arama API'si [etkileşimli tanıtımı](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/)deneyin.
Demo, bir arama sorgusunu hızlı bir şekilde nasıl özelleştirebileceğinizi ve görüntüleri görüntüler.

İlk isteğinizi hızla kullanmaya başlamak için hızlı başlangıçlara bakın:

* [C#](quickstarts/csharp.md)

* [Java](quickstarts/java.md)

* [node.js](quickstarts/nodejs.md)

* [Python](quickstarts/python.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Görüntüler-görsel arama](/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) başvurusu, uç noktalar, istek üstbilgileri, yanıtlar ve görüntü tabanlı arama sonuçları istemek için kullanabileceğiniz sorgu parametreleri hakkında tanımları ve bilgileri açıklar.

* [BING arama API kullanımı ve görüntüleme gereksinimleri](../bing-web-search/use-display-requirements.md) , Bing arama API 'leri aracılığıyla elde edilen içerik ve bilgilerin kabul edilebilir kullanımlarını belirtir.

* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/overview.md) ziyaret edin.