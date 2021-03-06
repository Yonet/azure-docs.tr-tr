---
title: Metin Analizi API'si v2 için geçiş kılavuzu
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Metin Analizi API'si sürüm 3 ' ü kullanacak şekilde taşımayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/22/2021
ms.author: aahi
ms.openlocfilehash: 0faa7a6f5a3d2efc8bbef11308b308e3305a00d5
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99096330"
---
# <a name="migrate-to-version-3x-of-the-text-analytics-api"></a>Metin Analizi API'si sürüm 3. x ' e geçirin

Metin Analizi API'si sürüm 2,1 kullanıyorsanız, bu makale uygulamanızı 3. x sürümünü kullanacak şekilde yükseltmenize yardımcı olur. Sürüm 3,0 genel kullanıma sunulmuştur ve genişletilmiş [adlandırılmış varlık tanıma (ner)](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-features-and-versions) ve [model sürümü oluşturma](concepts/model-versioning.md)gibi yeni özellikler sunar. Bir v 3.1 (v 3.1-Preview. x) önizleme sürümü de bulunur ve bu da, [fikrinizi araştırma](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)gibi özellikler ekler. V2 'de kullanılan modeller gelecekteki güncelleştirmeleri almaz. 

## <a name="sentiment-analysis"></a>[Yaklaşım Analizi](#tab/sentiment-analysis)

### <a name="feature-changes"></a>Özellik değişiklikleri 

2,1 sürümündeki Yaklaşım Analizi, API 'ye gönderilen her belge için 0 ile 1 arasındaki yaklaşım puanlarını döndürür ve daha fazla pozitif yaklaşım gösteren 1 ' e yaklaşır. Sürüm 3 Bunun yerine yaklaşım etiketlerini ("pozitif" veya "negatif" gibi), hem tümceler hem de belge için bir bütün olarak ve bunlarla ilişkili güven puanlarını döndürür. 

### <a name="steps-to-migrate"></a>Geçiş adımları

#### <a name="rest-api"></a>REST API

Uygulamanız REST API kullanıyorsa, istek bitiş noktasını, yaklaşım analizi için v3 uç noktasına güncelleştirin. Örneğin: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment` . Ayrıca, [API 'nin yanıtında](how-tos/text-analytics-how-to-sentiment-analysis.md#view-the-results)döndürülen yaklaşım etiketlerini kullanmak için uygulamayı güncelleştirmeniz gerekir. 

JSON yanıtının örnekleri için başvuru belgelerine bakın.
* [Sürüm 2,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)
* [Sürüm 3,0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Sentiment) 
* [Sürüm 3,1-Önizleme](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Sentiment)

#### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

## <a name="ner-and-entity-linking"></a>[NER ve varlık bağlama](#tab/named-entity-recognition)

### <a name="feature-changes"></a>Özellik değişiklikleri

Sürüm 2,1 ' de Metin Analizi API'si, adlandırılmış varlık tanıma (NER) ve varlık bağlama için bir uç nokta kullanır. Sürüm 3, genişletilmiş adlandırılmış varlık algılaması sağlar ve NER ve varlık bağlama istekleri için ayrı uç noktalar kullanır. V 3.1-Preview. 1 ' den başlayarak, NER kişisel `pii` ve sistem durumu bilgilerini de algılayabilir `phi` . 

### <a name="steps-to-migrate"></a>Geçiş adımları

#### <a name="rest-api"></a>REST API

Uygulamanız REST API kullanıyorsa, istek bitiş noktasını, NER ve/veya varlık bağlama için v3 uç noktalarına güncelleştirin.

Varlık Bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

HI
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

Ayrıca, [API 'nin yanıtında](how-tos/text-analytics-how-to-entity-linking.md#view-results)döndürülen [varlık kategorilerini](named-entity-types.md) kullanmak için uygulamanızı güncelleştirmeniz gerekecektir.

JSON yanıtının örnekleri için başvuru belgelerine bakın.
* [Sürüm 2,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)
* [Sürüm 3,0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral) 
* [Sürüm 3,1-Önizleme](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/EntitiesRecognitionGeneral)

#### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

#### <a name="version-21-entity-categories"></a>Sürüm 2,1 varlık kategorileri

Aşağıdaki tabloda, NER v 2.1 için döndürülen varlık kategorileri listelenmektedir.

| Kategori   | Açıklama                          |
|------------|--------------------------------------|
| Kişi   |   Kişilerin adları.  |
|Konum    | Doğal ve insan tarafından oluşturulan yer işaretleri, yapılar, coğrafi özellikler ve geopolitik varlıklar |
|Kuruluş | Şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları. Bu varlık türünde ülke almallikleri ve dini dahil değildir. |
| PhoneNumber | Telefon numaraları (yalnızca ABD ve AB telefon numaraları). |
| E-posta | E-posta adresleri. |
| URL | Web sitelerinin URL 'Leri. |
| IP | Ağ IP adresleri. |
| DateTime | Günün tarihleri ve saatleri.| 
| Tarih | Takvim tarihleri. |
| Saat | Günün saati |
| DateRange | Tarih aralıkları. |
| TimeRange | Zaman aralıkları. |
| Süre | Sürelerde. |
| Ayarla | Ayarlama, yinelenme süreleri. |
| Miktar | Sayılar ve sayısal miktarlar. |
| Sayı | Sayılarının. |
| Yüzde | Değerleri.|
| Sıralı | Sıra sayıları. |
| Yaş | Geçirir. |
| Para Birimi | Ayarlarsanız. |
| Boyut | Boyutlar ve ölçümler. |
| Sıcaklık | Sıcak. |

## <a name="language-detection"></a>[Dil algılama](#tab/language-detection)

### <a name="feature-changes"></a>Özellik değişiklikleri 

Dil algılama özelliği, uç nokta sürümünün dışında v3 'de değişmemiştir, ancak JSON yanıtı `ConfidenceScore` yerine içerir `score` . V3 Ayrıca çıktıda yalnızca tek bir dili döndürür. 

### <a name="steps-to-migrate"></a>Geçiş adımları

#### <a name="rest-api"></a>REST API

Uygulamanızda REST API kullanılıyorsa, dil algılama için istek uç noktasını v3 uç noktasına güncelleştirin. Örneğin: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/languages` . Ayrıca, `ConfidenceScore` `score` [API 'nin yanıtı](how-tos/text-analytics-how-to-language-detection.md#step-3-view-the-results)yerine kullanmak üzere uygulamayı güncelleştirmeniz gerekecektir. 

JSON yanıtının örnekleri için başvuru belgelerine bakın.
* [Sürüm 2,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7)
* [Sürüm 3,0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Languages) 
* [Sürüm 3,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Languages)

#### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

## <a name="key-phrase-extraction"></a>[Anahtar tümceciği ayıklama](#tab/key-phrase-extraction)

### <a name="feature-changes"></a>Özellik değişiklikleri 

Anahtar tümceciği ayıklama özelliği, uç nokta sürümü dışındaki v3 'de değişmemiştir.

### <a name="steps-to-migrate"></a>Geçiş adımları

#### <a name="rest-api"></a>REST API

Uygulamanız REST API kullanıyorsa, anahtar tümceciği ayıklama için istek uç noktasını v3 uç noktasına güncelleştirin. Örnek: `https://<your-custom-subdomain>.api.cognitiveservices.azure.com/text/analytics/v3.0/keyPhrases`

JSON yanıtının örnekleri için başvuru belgelerine bakın.
* [Sürüm 2,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)
* [Sürüm 3,0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/KeyPhrases) 
* [Sürüm 3,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/KeyPhrases)

#### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

---

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si nedir?](overview.md)
* [Dil desteği](language-support.md)
* [Model sürümü oluşturma](concepts/model-versioning.md)
