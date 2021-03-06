---
title: Azure Machine Learning işlem hatlarını yürütme
description: Azure Machine Learning işlem hatlarınızı Azure Data Factory işlem hatlarınız içinde nasıl çalıştıracağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: daperlov
author: djpmsft
manager: anandsub
ms.date: 07/16/2020
ms.openlocfilehash: 7239c1516c4a04b57249ea4f39bff4aec9156d72
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87337696"
---
# <a name="execute-azure-machine-learning-pipelines-in-azure-data-factory"></a>Azure Data Factory Azure Machine Learning işlem hatlarını yürütme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Machine Learning işlem hatlarınızı, Azure Data Factory işlem hatlarında bir adım olarak çalıştırın. Machine Learning yürütme işlem hattı etkinliği, olası kredi varsayılanlarını tanımlama, yaklaşımı belirleme ve müşteri davranış düzenlerini çözümleme gibi toplu tahmin senaryolarına olanak tanır.

Aşağıdaki videoda, bu özelliğin altı dakikalık bir giriş ve gösterimi sunulmaktadır.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/How-to-execute-Azure-Machine-Learning-service-pipelines-in-Azure-Data-Factory/player]

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Machine Learning Execute Pipeline",
    "type": "AzureMLExecutePipeline",
    "linkedServiceName": {
        "referenceName": "AzureMLService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mlPipelineId": "machine learning pipeline ID",
        "experimentName": "experimentName",
        "mlPipelineParameters": {
            "mlParameterName": "mlParameterValue"
        }
    }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
name | İşlem hattındaki etkinliğin adı | Dize | Evet
tür | Etkinliğin türü ' AzureMLExecutePipeline ' | Dize | Evet
linkedServiceName | Azure Machine Learning bağlı hizmet | Bağlı hizmet başvurusu | Evet
Mlpipelineıd | Yayınlanan Azure Machine Learning işlem hattının KIMLIĞI | Dize (veya dize resultType 'ı olan ifade) | Evet
experimentName | Machine Learning işlem hattı çalıştırmasının çalışma geçmişi deneme adı | Dize (veya dize resultType 'ı olan ifade) | Hayır
mlPipelineParameters | Yayımlanan Azure Machine Learning ardışık düzen uç noktasına geçirilecek anahtar, değer çiftleri. Anahtarların yayımlanan Machine Learning işlem hattında tanımlanan işlem hattı parametrelerinin adlarıyla eşleşmesi gerekir | Anahtar değer çiftlerine sahip nesne (veya resultType nesnesi olan Ifade) | Hayır
mlParentRunId | Ana Azure Machine Learning işlem hattı çalıştırma KIMLIĞI | Dize (veya dize resultType 'ı olan ifade) | Hayır
Devam Onstepfailure | Bir adım başarısız olursa Machine Learning ardışık düzen çalıştırmasında diğer adımların yürütülmesine devam edilip edilmeyeceğini belirtir | boolean | Hayır

> [!NOTE]
> Machine Learning işlem hattı adı ve KIMLIĞINDE açılan öğeleri doldurmak için, kullanıcının ML işlem hatlarını listeleme izni olması gerekir. ADF UX, oturum açmış kullanıcının kimlik bilgilerini kullanarak doğrudan AzureMLService API 'Lerini çağırır.  

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka yollarla nasıl dönüştürebileceğinizi açıklayan aşağıdaki makalelere bakın:

* [Veri akışı yürütme etkinliği](control-flow-execute-data-flow-activity.md)
* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akışı etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
