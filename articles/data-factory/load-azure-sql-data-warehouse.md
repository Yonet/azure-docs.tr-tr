---
title: Azure Synapse Analytics'e veri yükleme
description: Azure SYNAPSE Analytics 'e veri kopyalamak için Azure Data Factory kullanma
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/29/2020
ms.openlocfilehash: 8071b0fb08f6456b4eb59a3dae875ab0c90ab64d
ms.sourcegitcommit: 8c8c71a38b6ab2e8622698d4df60cb8a77aa9685
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99223516"
---
# <a name="load-data-into-azure-synapse-analytics-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SYNAPSE Analytics 'e veri yükleme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[Azure SYNAPSE Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) , hem ilişkisel hem de ilişkisel olmayan çok büyük hacimli verileri işleyebilen bulut tabanlı, genişleme bir veritabanıdır. Azure SYNAPSE Analytics, kurumsal veri ambarı iş yükleri için optimize edilmiş, yüksek düzeyde paralel işleme (MPP) mimarisi üzerine kurulmuştur. Depolama ve işlem ölçeğini bağımsız olarak ölçeklendirmeye yönelik esnekliğe sahip bulut esnekliği sunar.

Azure Data Factory kullandığınızda Azure SYNAPSE Analytics 'i kullanmaya artık hiç olmadığı kadar kolay. Azure Data Factory, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Hizmeti kullanarak bir Azure SYNAPSE analizini mevcut sisteminizdeki verilerle doldurabilir ve analiz çözümlerinizi oluştururken zamandan tasarruf edebilirsiniz.

Azure Data Factory, verileri Azure SYNAPSE Analytics 'e yüklemek için aşağıdaki avantajları sunmaktadır:

* **Kolayca ayarlanabilir**: komut dosyası gerekmeden sezgisel 5 adımlı bir sihirbaz.
* **Zengin veri deposu desteği**: zengin bir şirket içi ve bulut tabanlı veri deposu kümesi için yerleşik destek. Ayrıntılı bir liste için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)tablosuna bakın.
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığı, verilerinizin hiçbir şekilde coğrafi sınır bırakmamasını sağlar.
* **PolyBase kullanarak benzersiz olmayan performans**: PolyBase, verileri Azure SYNAPSE Analytics 'e taşımanın en etkili yoludur. Azure Blob depolama ve Data Lake Store dahil olmak üzere tüm veri deposu türlerinden yüksek yük hızları elde etmek için hazırlama blobu özelliğini kullanın. (PolyBase, Azure Blob depolamayı ve varsayılan olarak Azure Data Lake Store destekler.) Ayrıntılar için bkz. [etkinlik performansını kopyalama](copy-activity-performance.md).

Bu makalede, _Azure SQL veritabanından Azure SYNAPSE Analytics 'e veri yüklemek_ için Data Factory veri kopyalama aracının nasıl kullanılacağı gösterilmektedir. Diğer veri deposu türlerinden veri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için bkz. [Azure Data Factory kullanarak Azure SYNAPSE Analytics 'e veri kopyalama](connector-azure-sql-data-warehouse.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Azure SYNAPSE Analytics: veri ambarı, SQL veritabanından kopyalandığı verileri tutar. Azure SYNAPSE analizlerinizi bilmiyorsanız [Azure SYNAPSE Analytics oluşturma](../synapse-analytics/sql-data-warehouse/load-data-from-azure-blob-storage-using-copy.md)' daki yönergelere bakın.
* Azure SQL veritabanı: Bu öğretici, verileri Azure SQL veritabanı 'nda Adventure Works LT örnek veri kümesinden kopyalar. [Azure SQL veritabanı 'nda örnek veritabanı oluşturma](../azure-sql/database/single-database-create-quickstart.md)bölümündeki YÖNERGELERI Izleyerek SQL veritabanında bu örnek veritabanını oluşturabilirsiniz.
* Azure depolama hesabı: Azure depolama, toplu kopyalama işleminde _hazırlama_ blobu olarak kullanılır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-account-create.md) bölümündeki yönergelere bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol taraftaki menüden veri ve analiz **kaynak oluştur**' u seçin  >    >  **Data Factory**:

2. **Yeni Veri Fabrikası** sayfasında, aşağıdaki öğeler için değerler girin:

    * **Ad**: ad Için *Loadsqldwdemo* girin. Veri fabrikanızın adı * genel olarak benzersiz olmalıdır. "Data Factory Name ' LoadSQLDWDemo ' kullanılamıyor" hatasını alırsanız Veri Fabrikası için farklı bir ad girin. Örneğin _**, Name**_**ADFTutorialDataFactory** adını kullanabilirsiniz. Data Factory 'yi yeniden oluşturmayı deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: veri fabrikasının oluşturulacağı Azure aboneliğinizi seçin. 
    * **Kaynak grubu**: açılan listeden var olan bir kaynak grubunu seçin veya **Yeni oluştur** seçeneğini belirleyin ve bir kaynak grubunun adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
    * **Sürüm**: **V2**'yi seçin.
    * **Konum**: veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları diğer konumlarda ve bölgelerde olabilir. Bu veri depoları Azure Data Lake Store, Azure depolama, Azure SQL veritabanı vb. içerir.

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra, veri fabrikanıza gidin. Aşağıdaki görüntüde gösterildiği gibi **Data Factory** giriş sayfasını görürsünüz:

   ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)

   Veri Tümleştirme Uygulamasını ayrı bir sekmede açmak için **Oluştur ve İzle** kutucuğunu seçin.

## <a name="load-data-into-azure-synapse-analytics"></a>Azure Synapse Analytics'e veri yükleme

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala** kutucuğunu seçin.

2. **Özellikler** sayfasında, **görev adı** alanı Için **Copyfromsqltosqldw** ' i belirtin ve **İleri**' yi seçin.

    ![Özellikler sayfası](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)

3. **Kaynak veri deposu** sayfasında, aşağıdaki adımları izleyin:
    >[!TIP]
    >Bu öğreticide, *SQL kimlik* doğrulamasını kaynak veri deponuzu kimlik doğrulama türü olarak kullanacaksınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz:*hizmet sorumlusu* ve gerekirse *yönetilen kimlik* . Ayrıntılar için [Bu makaledeki](./connector-azure-sql-database.md#linked-service-properties) ilgili bölümlere bakın.
    >Veri depolarının gizli dizilerini güvenli bir şekilde depolamak için bir Azure Key Vault kullanılması da önerilir. Ayrıntılı çizimler için [Bu makaleye](./store-credentials-in-key-vault.md) bakın.

    a. **+ Yeni bağlantı oluştur**' a tıklayın.

    b. Galeriden **Azure SQL veritabanı** ' nı seçin ve **devam**' ı seçin. Bağlayıcıları filtrelemek için arama kutusuna "SQL" yazabilirsiniz.

    ![Azure SQL veritabanını seçme](./media/load-azure-sql-data-warehouse/select-azure-sql-db-source.png)

    c. **Yeni bağlı hizmet** sayfasında, açılır listeden sunucu ADıNıZı ve DB adını seçin ve Kullanıcı adını ve parolayı belirtin. Ayarları doğrulamak için **Bağlantıyı Sına** ' ya tıklayın ve ardından **Oluştur**' u seçin.

    ![Azure SQL veritabanını yapılandırma](./media/load-azure-sql-data-warehouse/configure-azure-sql-db.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve **İleri**'ye tıklayın.

4. **Verilerin kopyalanacağı tabloları seçin veya özel bir sorgu sayfası kullanın** sayfasında, tabloları filtrelemek Için **SalesLT** girin. Kopyaya ait tüm tabloları kullanmak için **(Tümünü Seç)** kutusunu seçin ve ardından **İleri**' yi seçin.

    ![Kaynak tabloları Seç](./media/load-azure-sql-data-warehouse/select-source-tables.png)

5. **Filtre Uygula** sayfasında ayarlarınızı belirtin veya **İleri**' yi seçin.

6. **Hedef veri deposu** sayfasında, aşağıdaki adımları izleyin:
    >[!TIP]
    >Bu öğreticide, *SQL kimlik doğrulamasını* hedef veri deponuzu kimlik doğrulama türü olarak kullanacaksınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz:*hizmet sorumlusu* ve gerekirse *yönetilen kimlik* . Ayrıntılar için [Bu makaledeki](./connector-azure-sql-data-warehouse.md#linked-service-properties) ilgili bölümlere bakın.
    >Veri depolarının gizli dizilerini güvenli bir şekilde depolamak için bir Azure Key Vault kullanılması da önerilir. Ayrıntılı çizimler için [Bu makaleye](./store-credentials-in-key-vault.md) bakın.

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur** ' a tıklayın

    b. Galeriden **Azure SYNAPSE Analytics** ' i seçin ve **devam**' ı seçin.

    ![Azure SYNAPSE Analytics 'i seçme](./media/load-azure-sql-data-warehouse/select-azure-sql-dw-sink.png)

    c. **Yeni bağlı hizmet** sayfasında, açılır listeden sunucu ADıNıZı ve DB adını seçin ve Kullanıcı adını ve parolayı belirtin. Ayarları doğrulamak için **Bağlantıyı Sına** ' ya tıklayın ve ardından **Oluştur**' u seçin.

    ![Azure SYNAPSE Analytics 'i yapılandırma](./media/load-azure-sql-data-warehouse/configure-azure-sql-dw.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı havuz hizmetini seçin ve **İleri**'ye tıklayın.

7. **Tablo eşleme** sayfasında, içeriği gözden geçirin ve **İleri**' yi seçin. Akıllı tablo eşleştirmesi görüntülenir. Kaynak tablolar, tablo adlarına göre hedef tablolarla eşleştirilir. Hedefte bir kaynak tablosu yoksa, Azure Data Factory varsayılan olarak aynı ada sahip bir hedef tablo oluşturur. Ayrıca, bir kaynak tabloyu var olan bir hedef tabloyla eşleyebilirsiniz.

   ![Tablo eşleme sayfası](./media/load-azure-sql-data-warehouse/table-mapping.png)

8. **Sütun eşleme** sayfasında, içeriği gözden geçirin ve **İleri**' yi seçin. Akıllı tablo eşleme, sütun adını temel alır. Tabloları otomatik olarak oluşturalım Data Factory, kaynak ve hedef mağazalar arasında uyumsuzluklar olduğunda veri türü dönüştürme gerçekleşebilir. Kaynak ve hedef sütun arasında desteklenmeyen bir veri türü dönüştürmesi varsa, karşılık gelen tablonun yanında bir hata mesajı görürsünüz.

    ![Sütun eşleme sayfası](./media/load-azure-sql-data-warehouse/schema-mapping.png)

9. **Ayarlar** sayfasında, aşağıdaki adımları izleyin:

    a. **Hazırlama ayarları** bölümünde, yeni bir hazırlama depolaması Için **+ Yeni** öğesine tıklayın. Depolama, PolyBase kullanılarak Azure SYNAPSE Analytics 'e yüklenmeden önce verileri hazırlamak için kullanılır. Kopyalama işlemi tamamlandıktan sonra, Azure Blob depolama alanındaki geçici veriler otomatik olarak temizlenir.

    b. **Yeni bağlı hizmet** sayfasında, depolama hesabınızı seçin ve bağlı hizmeti dağıtmak için **Oluştur** ' u seçin.

    c. **Varsayılan kullanım türü** seçeneğinin işaretini kaldırın ve ardından **İleri**' yi seçin.

    ![PolyBase 'i yapılandırma](./media/load-azure-sql-data-warehouse/configure-polybase.png)

10. **Özet** sayfasında, ayarları gözden geçirin ve **İleri**' yi seçin.

    ![Özet sayfası](./media/load-azure-sql-data-warehouse/summary-page.png)

11. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin. 
 
12. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. İşlem hattı çalıştırması başarıyla tamamlandığında, etkinlik çalıştırma ayrıntılarını görüntülemek veya işlem hattını yeniden çalıştırmak için işlem **hattı adı** sütununun altındaki **Copyfromsqltosqldw** bağlantısını seçin.

    [![İşlem hattı çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png)](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png#lightbox)

13. İşlem hattı çalıştırmaları görünümüne geri dönmek için üstteki **tüm işlem hattı çalıştırmaları** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/activity-monitoring.png)

14. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik çalıştırmaları görünümünde **etkınlık adı** ' nın altındaki **Ayrıntılar** bağlantısını (eyegözlük simgesi) seçin. Kaynaktan havuza, veri işleme, karşılık gelen süre ile yürütme adımlarına ve kullanılan yapılandırmalara göre veri hacmi gibi ayrıntıları izleyebilirsiniz.
    ![Önce etkinliğin çalışma ayrıntılarını izle](./media/load-azure-sql-data-warehouse/monitor-activity-run-details-1.png)

    ![Etkinliğin etkinlik çalıştırma ayrıntıları ikincisini izleyin](./media/load-azure-sql-data-warehouse/monitor-activity-run-details-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure SYNAPSE Analytics desteği hakkında bilgi edinmek için aşağıdaki makaleye ilerleyin:

> [!div class="nextstepaction"]
>[Azure SYNAPSE Analytics Bağlayıcısı](connector-azure-sql-data-warehouse.md)