---
title: Azure Data Factory Bağlan
description: Bu makalede, verileri kökenini izlemek üzere Azure Data Factory ve Azure purview 'ın nasıl bağlanacağı açıklanır.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/22/2020
ms.openlocfilehash: 010cfc307d2b2c10c31168fce73673fb1fb611b8
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2021
ms.locfileid: "99807657"
---
# <a name="how-to-connect-azure-data-factory-and-azure-purview"></a>Azure Data Factory ve Azure purview 'a bağlanma

Bu belgede, veri kökenini izlemek için bir Azure Data Factory hesabını Azure purview hesabıyla bağlamak için gereken adımlar açıklanmaktadır. Belge ayrıca kapsam kapsamının ayrıntılarına ve desteklenen kökenini desenlerine de sahiptir.

## <a name="view-existing-data-factory-connections"></a>Mevcut Data Factory bağlantılarını görüntüle

Birden çok Azure Veri Fabrikası, kökenini bilgilerini göndermek için tek bir Azure purview veri kataloğuna bağlanabilir. Geçerli sınır, her seferinde OnView yönetim merkezinden on Data Factory hesabı bağlamanıza olanak tanır. Purview veri kataloğunuza bağlı Data Factory hesaplarının listesini göstermek için aşağıdakileri yapın:

1. Sol gezinti bölmesinde **Yönetim Merkezi** ' ni seçin.
2. **Dış bağlantılar** altında **Data Factory bağlantı**' yı seçin.
3. Data Factory bağlantı listesi görüntülenir.

    :::image type="content" source="./media/how-to-link-azure-data-factory/data-factory-connection.png" alt-text="Data Factory bağlantı listesini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/data-factory-connection.png":::

4. Bağlantı **durumu** için çeşitli değerlere dikkat edin:

    - **Bağlandı**: Data Factory, veri kataloğuna bağlanır.
    - **Bağlantı kesildi**: Data Factory 'nin kataloğa erişimi vardır, ancak başka bir kataloğa bağlıdır. Sonuç olarak, veri kökenini otomatik olarak kataloğa bildirilmez.
    - **CannotAccess**: geçerli kullanıcının veri fabrikasına erişimi yok, bu nedenle bağlantı durumu bilinmiyor.
 >[!Note]
 >Data Factory bağlantılarını görüntülemek için, purview rollerinden herhangi biri atanmalıdır:
 >- Katılımcı
 >- Sahip
 >- Okuyucu
 >- Kullanıcı Erişimi Yöneticisi

## <a name="create-new-data-factory-connection"></a>Yeni Data Factory bağlantısı oluştur

>[!Note]
>Data Factory bağlantılarını eklemek veya kaldırmak için, purview rollerinden herhangi biri atanmalıdır:
>- Sahip
>- Kullanıcı Erişimi Yöneticisi
>
> Ayrıca, kullanıcıların Data Factory 'nin "Owner" veya "katkıda bulunan" olması gerekir. 

Mevcut bir Data Factory hesaplarını purview veri kataloğunuza bağlamak için aşağıdaki adımları izleyin.

1. Sol gezinti bölmesinde **Yönetim Merkezi** ' ni seçin.
2. **Dış bağlantılar** altında **Data Factory bağlantı**' yı seçin.
3. **Data Factory bağlantısı** sayfasında **Yeni**' yi seçin.

4. Listeden Data Factory hesabınızı seçin ve **Tamam**' ı seçin. Ayrıca, listenizi sınırlandırmak için abonelik adına göre filtreleyebilirsiniz.

    :::image type="content" source="./media/how-to-link-azure-data-factory/connect-data-factory.png" alt-text="Azure Data Factory nasıl bağlanagösterileceğini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/connect-data-factory.png":::

    Data Factory zaten geçerli purview hesabına bağlıysa veya Data Factory 'nin yönetilen bir kimliği yoksa bazı Data Factory örnekleri devre dışı bırakılabilir.

    Seçili veri fabrikaları zaten başka bir purview hesabına bağlıysa bir uyarı iletisi görüntülenir. Tamam ' ı seçtiğinizde, diğer purview hesabı ile Data Factory bağlantısının bağlantısı kesilir. Ek bir teyid gerekli değildir.


    :::image type="content" source="./media/how-to-link-azure-data-factory/warning-for-disconnect-factory.png" alt-text="Azure Data Factory bağlantısını kesmek için uyarı gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/warning-for-disconnect-factory.png":::

>[!Note]
>Artık aynı anda 10 ' dan fazla veri fabrikası eklemeyi destekliyoruz. Aynı anda 10 ' dan fazla veri fabrikası eklemek istiyorsanız, lütfen bir destek bileti dosyası sağlayın.

### <a name="how-does-the-authentication-work"></a>Kimlik doğrulaması nasıl çalışır?

Bir purview kullanıcısı, erişimi olan bir Data Factory kaydettiğinde arka uçta aşağıdakiler olur:

1. **Data Factory MSI** , PURVIEW RBAC rolüne eklenmiştir: **purview veri Curator**.

    :::image type="content" source="./media/how-to-link-azure-data-factory/adf-msi.png" alt-text="Azure Data Factory MSI gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/adf-msi.png":::
     
2. Data Factory işlem hattının yeniden yürütülmesi gerekir, böylece kökenini meta verileri purview 'a geri gönderilir.
3. Yürütme sonrası Data Factory meta veriler purview 'a gönderilir.

### <a name="remove-data-factory-connections"></a>Data Factory bağlantılarını kaldırma
Bir Data Factory bağlantısını kaldırmak için aşağıdakileri yapın:

1. **Data Factory bağlantısı** sayfasında, bir veya daha fazla veri fabrikası bağlantısının yanındaki **Kaldır** düğmesini seçin.
2. Seçili Veri Fabrikası bağlantılarını silmek için açılan pencerede **Onayla** ' yı seçin.

    :::image type="content" source="./media/how-to-link-azure-data-factory/remove-data-factory-connection.png" alt-text="Bağlantıyı kaldırmak için veri fabrikalarının nasıl seçileceğini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/remove-data-factory-connection.png":::

## <a name="configure-a-self-hosted-ir-to-collect-lineage-from-on-prem-sql"></a>Şirket içi SQL 'den kökenini toplamak için kendinden konak IR yapılandırma

Data Factory kopyalama etkinliği için kökenini, şirket içi SQL veritabanları için kullanılabilir. Azure Data Factory ile veri hareketi için şirket içinde barındırılan tümleştirme çalışma zamanı çalıştırıyorsanız ve Azure purview 'da kökenini yakalamak istiyorsanız, sürümün 4.8.7418.1 veya üzeri olduğundan emin olun. Şirket içinde barındırılan tümleştirme çalışma zamanı hakkında daha fazla bilgi için bkz. [Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma ve yapılandırma](../data-factory/create-self-hosted-integration-runtime.md).

## <a name="supported-azure-data-factory-activities"></a>Desteklenen Azure Data Factory etkinlikleri

Azure purview, aşağıdaki Azure Data Factory etkinliklerinden çalışma zamanı kökenini yakalar:

- Veri Kopyalama
- Veri Akışı
- SSIS paketini Yürüt

> [!IMPORTANT]
> Kaynak veya hedef desteklenmeyen bir veri depolama sistemi kullanıyorsa Azure purview kökenini bırakır.

Data Factory ile takip görünümü arasındaki tümleştirme, aşağıdaki bölümlerde açıklandığı gibi, Data Factory tarafından desteklenen veri sistemlerinin yalnızca bir alt kümesini destekler.

### <a name="data-factory-copy-data-support"></a>Data Factory Veri Kopyalama desteği

| Veri depolama sistemi | Kaynak olarak desteklenir | 
| ------------------- | ------------------- | 
| ADLS 1. Nesil | Yes | 
| ADLS 2. Nesil | Yes | 
| Azure Blob | Yes |
| Azure Cosmos DB (SQL API) | Yes | 
| Azure Cosmos DB (Mongo API) | Yes |
| Azure Bilişsel Arama | Yes | 
| Azure Veri Gezgini | Yes | 
| Maria DB için Azure veritabanı \* | Yes | 
| MYSQL için Azure veritabanı \* | Yes | 
| PostgreSQL için Azure veritabanı \* | Yes |
| Azure Dosya Depolama | Evet | 
| Azure Tablo Depolama | Evet |
| Azure SQL veritabanı \* | Yes | 
| Azure SQL MI \* | Yes | 
| Azure SYNAPSE Analytics (eski adıyla SQL DW) \* | Yes | 
| Şirket içi SQL Server  \* | Yes | 
| Amazon S3 | Evet | 
| Teradata | Evet | 
| SAP tablo Bağlayıcısı | Yes |
| SAP ECC | Yes | 
| Hive | Yes | 

> [!Note]
> Kökenini özelliği Data Factory kopyalama etkinliğinde belirli performans ek yüküne sahiptir. Veri Fabrikası bağlantıları kuran kullanıcılar için, purview 'da daha uzun süren belirli kopyalama işlerinin gözlemleyebilirsiniz. Genellikle etki, yok edilebilir değildir. Kopyalama işlerinin her zamanki gibi daha uzun sürmesi durumunda lütfen zaman karşılaştırmayla desteğe başvurun.

### <a name="data-factory-data-flow-support"></a>Data Factory veri akışı desteği

| Veri depolama sistemi | Desteklenir |
| ------------------- | ------------------- | 
| ADLS 1. Nesil | Yes |
| ADLS 2. Nesil | Yes |
| Azure Blob | Yes |
| Azure SQL veritabanı \* | Yes |
| Azure SYNAPSE Analytics (eski adıyla SQL DW) \* | Yes |

### <a name="data-factory-execute-ssis-package-support"></a>SSIS paketi desteğini Data Factory Yürüt

| Veri depolama sistemi | Desteklenir |
| ------------------- | ------------------- |
| Azure Blob | Yes |
| ADLS 1. Nesil | Yes |
| ADLS 2. Nesil | Yes |
| Azure SQL veritabanı \* | Yes |
| Azure SQL MI \*| Yes |
| Azure SYNAPSE Analytics (eski adıyla SQL DW) \* | Yes |
| Şirket içi SQL Server \* | Yes |
| Azure Dosya Depolama | Yes |

*\* SQL (Azure ve şirket içi) senaryolarında Azure purview, kökenini veya tarama için saklı yordamları veya betikleri desteklemez. Kökenini yalnızca tablo ve görünüm kaynaklarıyla sınırlandırılmıştır.*

> [!Note]
> Azure Data Lake Storage 2. Nesil genel kullanıma sunuldu. Bugün kullanmaya başlamanızı öneririz. Daha fazla bilgi için bkz. [ürün sayfası](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/).

## <a name="supported-lineage-patterns"></a>Desteklenen kökenini desenleri

Azure purview tarafından desteklenen birkaç kökenini deseni vardır. Oluşturulan kökenini verileri, Data Factory etkinliklerde kullanılan kaynak ve havuz türüne bağlıdır. Data Factory, 80 kaynak ve havuzları destekler, ancak [desteklenen Azure Data Factory etkinliklerinde](#supported-azure-data-factory-activities)listelendiği gibi Azure purview yalnızca bir alt kümeyi destekler.

Kökenini bilgilerini göndermek üzere Data Factory yapılandırmak için, bkz. [kökenini ile çalışmaya başlama](catalog-lineage-user-guide.md#get-started-with-lineage).

Kökenini görünümünde bilgi bulmanın bazı ek yolları şunları içerir:

- Araç ipucunda varlık hakkındaki ek bilgileri önizlemek için **kökenini** sekmesinde şekillerin üzerine gelin.
- Ait olduğu varlık türünü görmek veya varlıkları değiştirmek için düğümü veya kenarı seçin.
- Bir veri kümesinin sütunları **kökenini** sekmesinin sol tarafında görüntülenir. Sütun düzeyi kökenini hakkında daha fazla bilgi için bkz. [sütun düzeyi kökenini](catalog-lineage-user-guide.md#column-level-lineage).

### <a name="data-lineage-for-11-operations"></a>1:1 işlemleri için veri kökenini

Veri kökenini yakalama için en yaygın model, tek bir giriş veri kümesinden verileri arasında bir işlemle birlikte tek bir çıkış veri kümesine taşımaktır.

Bu düzene bir örnek aşağıdaki gibi olabilir:

- 1 Kaynak/giriş: *Müşteri* (SQL tablosu)
- 1 havuz/çıkış: *Customer1.csv* (Azure blob)
- 1 işlem: *CopyCustomerInfo1 \#Customer1.csv* (Data Factory kopyalama etkinliği)

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-copy-lineage.png" alt-text="Tek bir Data Factory kopyalama işlemi için kökenini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/adf-copy-lineage.png":::

### <a name="data-movement-with-11-lineage-and-wildcard-support"></a>1:1 kökenini ve joker karakter desteğiyle veri taşıma

Kökenini yakalamaya yönelik başka bir yaygın senaryo, dosyaları tek bir giriş veri kümesinden tek bir çıkış veri kümesine kopyalamak için bir joker karakter kullanmaktır. Joker karakter kopyalama etkinliğinin dosya adının ortak bir bölümünü kullanarak kopyalama için birden çok dosya ile eşleşmesini sağlar. Azure purview, ilgili kopyalama etkinliği tarafından kopyalanmış her bir dosya için dosya düzeyi kökenini yakalar.

Bu düzene bir örnek aşağıdaki gibi olabilir:

* Kaynak/giriş: *Customercall \* . csv* (ADLS 2. yolu)
* Havuz/çıkış: *Customercall \* . csv* (Azure Blob dosyası)
* 1 işlem: *CopyGen2ToBlob \#CustomerCall.csv* (Data Factory kopyalama etkinliği)  

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-copy-lineage-wildcard.png" alt-text="Joker karakter desteğiyle tek bir kopyalama işlemi için kökenini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/adf-copy-lineage-wildcard.png":::

### <a name="data-movement-with-n1-lineage"></a>N:1 kökenini ile veri taşıma

Birleştirme, birleştirme ve benzeri veri işlemleri gerçekleştirmek için veri akışı etkinliklerini kullanabilirsiniz. Hedef veri kümesi oluşturmak için birden fazla kaynak veri kümesi kullanılabilir. Bu örnekte, Azure purview, bireysel giriş dosyaları için dosya düzeyi kökenini bir veri akışı etkinliğinin parçası olan bir SQL tablosuna yakalar.

Bu düzene bir örnek aşağıdaki gibi olabilir:

* 2 kaynak/giriş: *Customer.csv*, *Sales. Parquet* (ADLS 2. yolu)
* 1 havuz/çıkış: *Şirket verileri* (Azure SQL tablosu)
* 1 işlem: *Dataflowblobstosql* (Data Factory veri akışı etkinliği)

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-data-flow-lineage.png" alt-text="Bir n-bir D F veri akışı işlemi için kökenini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/adf-data-flow-lineage.png":::

### <a name="lineage-for-resource-sets"></a>Kaynak kümeleri için kökenini

Kaynak kümesi, katalogdaki, temel depolamadaki birçok bölüm dosyasını temsil eden mantıksal bir nesnedir. Daha fazla bilgi için bkz. [kaynak kümelerini anlama](concept-resource-sets.md). Azure purview, Azure Data Factory kökenini yakalar, tek tek bölüm dosyalarını normalleştirmek ve tek bir mantıksal nesne oluşturmak için kuralları uygular.

Aşağıdaki örnekte, bir Azure Blobundan Azure Data Lake Gen2 kaynak kümesi üretilir:

* 1 Kaynak/giriş: *çalışan \_management.csv* (Azure blob)
* 1 havuz/çıkış: *çalışan \_management.csv* (Azure Data Lake Gen 2)
* 1 işlem: *CopyBlobToAdlsGen2 \_ RS* (Data Factory kopyalama etkinliği)

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-resource-set-lineage.png" alt-text="Bir kaynak kümesi için kökenini gösteren ekran görüntüsü." lightbox="./media/how-to-link-azure-data-factory/adf-resource-set-lineage.png":::

## <a name="next-steps"></a>Sonraki adımlar

- [Catalog kökenini Kullanıcı Kılavuzu](catalog-lineage-user-guide.md)
- [Kökenini için Azure veri paylaşımıyla bağlantı](how-to-link-azure-data-share.md)
