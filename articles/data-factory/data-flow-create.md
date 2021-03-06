---
title: Eşleme veri akışı oluşturma
description: Azure Data Factory eşleme veri akışı oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: eaf36cc2690b3c0f8922c05432b3197b4ff30d9a
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "93026063"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory Veri Akışı Oluşturma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

ADF 'de veri akışlarını eşleme, hiçbir kodlama gerekmeden verileri ölçeklendirmeye yönelik bir yol sağlar. Bir dizi dönüştürme oluşturarak veri akışı tasarımcısında bir veri dönüştürme işi tasarlayabilirsiniz. Herhangi bir sayıdaki kaynak dönüşümle başlayın ve ardından veri dönüştürme adımları. Daha sonra, sonuçlarınızı bir hedefe aktarmak için havuz ile veri akışınızı doldurun.

İlk olarak Azure portal yeni bir v2 Data Factory oluşturmaya başlayın. Yeni fabrikanızı oluşturduktan sonra, Data Factory Kullanıcı arabirimini başlatmak için "yazar & Izleyicisi" kutucuğuna tıklayın.

![Ekran görüntüsü sürüm için seçili v2 ile yeni Data Factory bölmesini gösterir.](media/data-flow/v2portal.png "veri akışı oluşturma")

Data Factory kullanıcı arabiriminden olduktan sonra örnek veri akışlarını kullanabilirsiniz. Bu örnekler ADF Şablon Galerisi ' nden kullanılabilir. ADF 'de "şablondan işlem hattı" oluşturun ve şablon galerisinden veri akışı kategorisini seçin.

![Ekran görüntüsünde veri akışı seçili olarak veri akışı sekmesi görüntülenir.](media/data-flow/template.png "veri akışı oluşturma")

Azure Blob depolama hesabı bilgilerinizi girmeniz istenir.

![Ekran görüntüsü, Kullanıcı girişlerini girebileceğiniz veri akışı bölmesini kullanarak verileri dönüştürme ' yı gösterir.](media/data-flow/template2.png "veri akışı oluşturma 2")

[Bu örnekler için kullanılan veriler burada bulunabilir](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Örnekleri yürütebilmeniz için örnek verileri indirin ve dosyaları Azure Blob depolama hesaplarınıza depolayın.

## <a name="create-new-data-flow"></a>Yeni veri akışı oluştur

Veri akışları oluşturmak için ADF Kullanıcı arabirimindeki kaynak oluştur "artı işareti" düğmesini kullanın.

![Ekran görüntüsü, fabrika kaynakları menüsünden seçilen veri akışını gösterir.](media/data-flow/newresource.png "Yeni kaynak")

## <a name="next-steps"></a>Sonraki adımlar

Veri dönüşümünüzü [kaynak dönüşümle](data-flow-source.md)oluşturmaya başlayın.
