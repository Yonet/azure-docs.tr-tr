---
title: Azure Stream Analytics Önizleme özellikleri
description: Bu makalede, şu anda önizleme aşamasında olan Azure Stream Analytics özellikleri listelenmektedir.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 8/07/2020
ms.openlocfilehash: d07a2364cbd2b63ef9e38387b6e915f7e63676ae
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98012164"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics Önizleme özellikleri

Bu makale, şu anda Azure Stream Analytics için Önizlemedeki tüm özellikleri özetler. Bir üretim ortamında Önizleme özelliklerinin kullanılması önerilmez.

## <a name="public-previews"></a>Genel önizlemeler

Aşağıdaki özellikler genel önizlemede. Günümüzde bu özelliklerden faydalanabilirsiniz, ancak bunları üretim ortamınızda kullanmayın.

### <a name="authenticate-to-sql-database-output-with-managed-identities-preview"></a>Yönetilen kimlikler ile SQL veritabanı çıktısına kimlik doğrulama (Önizleme)

Azure Stream Analytics, Azure SQL veritabanı çıkış havuzları için [yönetilen kimlik kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/overview.md) destekler. Yönetilen kimlikler, Kullanıcı tabanlı kimlik doğrulama yöntemlerinin, parola değişiklikleri nedeniyle yeniden kimlik doğrulaması ihtiyacı gibi sınırlamaları ortadan kaldırır. 

### <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Azure Machine Learning tarafından yönetilen özel ML modelleriyle gerçek zamanlı yüksek performans Puanlama

Azure Stream Analytics, Azure Machine Learning tarafından yönetilen ve Azure Kubernetes Service (AKS) veya Azure Container Instances (acı) ' de barındırılan ve kod yazmanızı gerektirmeyen bir iş akışı kullanarak yüksek performanslı ve gerçek zamanlı Machine Learning Puanlama destekler. Önizleme için [kaydolun](https://aka.ms/asapreview1)

### <a name="c-custom-de-serializers"></a>C# özel de serileştiriciler
Geliştiriciler, verileri prototip, XML veya özel bir biçimde işlemek için Azure Stream Analytics gücünden yararlanabilir. C# ' de [özel seri hale getiriciler](custom-deserializer-examples.md) uygulayabilirsiniz ve bu, Azure Stream Analytics tarafından alınan olayların serileştirilmek için kullanılabilir.

### <a name="extensibility-with-c-custom-code"></a>C# özel kodlu genişletilebilirlik

Bulutta veya IoT Edge Stream Analytics modülleri oluşturan geliştiriciler özel C# işlevlerini yazabilir veya yeniden kullanabilir ve [Kullanıcı tanımlı işlevler](stream-analytics-edge-csharp-udf-methods.md)aracılığıyla doğrudan sorgu içinde çağırabilir.

### <a name="debug-query-steps-in-visual-studio"></a>Visual Studio 'da sorgu adımlarında hata ayıkla

Visual Studio için Azure Stream Analytics araçları 'nda yerel test yaparken, veri diyagramında ara satır kümesini kolayca önizleyebilirsiniz. 


### <a name="live-data-testing-in-visual-studio"></a>Visual Studio 'da canlı veri testi

Azure Stream Analytics için Visual Studio Araçları, Olay Hub 'ı veya IoT Hub gibi bulut kaynaklarından canlı etkinlik akışlarına karşı sorguları test etmenizi sağlayan yerel test özelliğini geliştirir. [Visual Studio için Azure Stream Analytics araçları 'nı kullanarak canlı verileri yerel olarak test](stream-analytics-live-data-local-testing.md)etme hakkında bilgi edinin.

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Azure Stream Analytics için Visual Studio Code

Azure Stream Analytics işleri Visual Studio Code olarak yazılabilir. [Vs Code kullanmaya başlama Öğreticimizi](./quick-create-visual-studio-code.md)inceleyin.

### <a name="local-testing-with-live-data-in-visual-studio-code"></a>Visual Studio Code içindeki canlı verilerle yerel test

İşi Azure 'a göndermeden önce, sorgularınızı yerel makinenizde canlı verilere karşı test edebilirsiniz. Her test yinelemesi ortalama olarak iki ile üç saniyeden az sürer ve çok verimli bir geliştirme süreci elde edilir.

## <a name="other-previews"></a>Diğer önizlemeler

Ayrıca, istek üzerine Önizleme bölümünde aşağıdaki özellikler de mevcuttur.

### <a name="support-for-azure-stack"></a>Azure Stack için destek
Azure IoT Edge çalışma zamanında etkinleştirilen bu özellik, Azure Stack çalışan yerel girişler ve çıktılar için yerel destek gibi özel Azure Stack özelliklerinden yararlanır (örneğin, Event Hubs, IoT Hub, BLOB depolama). Bu yeni tümleştirme, verilerinizi nerede oluşturulduğu yere çözümleyebilecekleri karma mimariler oluşturmanızı, gecikme süresini azaltmayı ve öngörüleri en üst düzeye çıkarmanızı sağlar.
Bu özellik, verilerinizin oluşturulduğu yere yakın, gecikme süresini düşürürken ve öngörüleri en üst düzeye çıkarmanızı sağlayan karma mimariler oluşturmanıza olanak sağlar. Bu önizlemeye [kaydolmanız](https://aka.ms/asapreview1) gerekir.