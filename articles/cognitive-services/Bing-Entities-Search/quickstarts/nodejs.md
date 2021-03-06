---
title: 'Hızlı başlangıç: Node.js REST API kullanarak bir arama isteği gönderme Bing Varlık Arama'
titleSuffix: Azure Cognitive Services
description: Node.js kullanarak REST API Bing Varlık Arama bir istek göndermek ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: b9311329ea4115d49f36dd7d39782bbd748a356b
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97106113"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-nodejs"></a>Hızlı başlangıç: Bing Varlık Arama REST API Node.js kullanarak bir arama isteği gönderme

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Varlık Arama API'si ilk çağrısını yapmak ve JSON yanıtını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması, API 'ye bir haber arama sorgusu gönderir ve yanıtı görüntüler. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingEntitySearchv7.js)' da kullanılabilir.

Bu uygulama JavaScript 'e yazılsa da, API çoğu programlama dili ile uyumlu olan yeniden yazılmış bir Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [JavaScript Istek kitaplığı](https://github.com/request/request).

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve striclük ve HTTPS gereksinimlerini ayarlayın.

    ```javaScript
    'use strict';
    let https = require ('https');
    ```

2. API uç noktası, abonelik anahtarınız ve arama sorgunuz için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```javascript
    let subscriptionKey = 'ENTER YOUR KEY HERE';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/entities';
    
    let mkt = 'en-US';
    let q = 'italian restaurant near me';
    ```

3. Pazar ve sorgu parametrelerinizi adlı bir dizeye ekleyin `query` . Sorgunuzu URL ile kodlayıp kodlayadığınızdan emin olun `encodeURI()` .
    ```javascript 
    let query = '?mkt=' + mkt + '&q=' + encodeURI(q);
    ```

## <a name="handle-and-parse-the-response"></a>Yanıtı işleme ve ayrıştırma

1. `response_handler()`Parametresi olarak, http çağrısını alan adlı bir işlev tanımlayın `response` . 

2. Bu işlev içinde, JSON yanıtının gövdesini içeren bir değişken tanımlayın.  
    ```javascript
    let response_handler = function (response) {
        let body = '';
    };
    ```

3. Bayrak çağrıldığında yanıtın gövdesini saklayın `data` .
    ```javascript
    response.on('data', function (d) {
        body += d;
    });
    ```

4. Bir `end` bayrağa işaret EDILDIĞINDE JSON 'ı ayrıştırır ve yazdırın.

    ```javascript
    response.on ('end', function () {
    let json = JSON.stringify(JSON.parse(body), null, '  ');
    console.log (json);
    });
    ```

## <a name="send-a-request"></a>İstek gönderme

1. `Search()`Arama isteği göndermek için adlı bir işlev oluşturun. Burada, aşağıdaki adımları gerçekleştirin:

2. Bu işlev içinde, istek parametrelerinizi içeren bir JSON nesnesi oluşturun. `Get`Yöntemi için kullanın ve ana bilgisayar ve yol bilgilerinizi ekleyin. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin. 

3. `https.request()`Daha önce oluşturulan yanıt işleyicisine ve arama parametreleriniz ile isteği göndermek için kullanın.
    
   ```javascript
   let Search = function () {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + query,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };
    
    let req = https.request (request_params, response_handler);
    req.end ();
   }
      ```

2. İşlevi çağırın `Search()` .

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../overview.md )
* [Bing varlık arama API'si Başvurusu](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
