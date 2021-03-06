---
title: 'Hızlı başlangıç: Azure Databricks kullanarak Azure Data Lake Storage 2. verileri çözümleme | Microsoft Docs'
description: Azure portal ve Azure Data Lake Storage 2. depolama hesabı kullanarak Azure Databricks Spark işini çalıştırmayı öğrenin.
author: normesta
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 06/12/2020
ms.reviewer: jeking
ms.openlocfilehash: e289bea6b1a23f1622ced62656164d9865303298
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95912833"
---
# <a name="quickstart-analyze-data-with-databricks"></a>Hızlı başlangıç: Databricks ile verileri analiz etme

Bu hızlı başlangıçta, bir depolama hesabında depolanan veriler üzerinde analiz gerçekleştirmek için Azure Databricks kullanarak bir Apache Spark işi çalıştırırsınız. Spark işinin bir parçası olarak, demografik tabanlı ücretsiz/ücretli kullanıma yönelik Öngörüler elde etmek için bir radyo kanalı abonelik verilerini çözümleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

* Hiyerarşik ad alanı özelliği etkin olan bir depolama hesabı. Bir tane oluşturmak için, bkz. [Azure Data Lake Storage 2. kullanılacak depolama hesabı oluşturma](create-data-lake-storage-account.md).

* Bir Azure hizmet sorumlusunun kiracı KIMLIĞI, uygulama KIMLIĞI ve parolası, atanan bir **Depolama Blobu veri katılımcısı** rolüne sahiptir. [Hizmet sorumlusu oluşturun](../../active-directory/develop/howto-create-service-principal-portal.md).

  > [!IMPORTANT]
  > Rolü Data Lake Storage 2. depolama hesabının kapsamına atayın. Üst kaynak grubuna veya aboneliğine bir rol atayabilirsiniz, ancak bu rol atamaları depolama hesabına yayana kadar izinlerle ilgili hatalar alırsınız.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure Portal, **kaynak**  >  **Analizi** oluştur  >  **Azure Databricks**' u seçin.

    ![Azure portal databricks](./media/data-lake-storage-quickstart-create-databricks-account/azure-databricks-on-portal.png "Azure portal databricks")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-workspace.png "Azure Databricks çalışma alanı oluşturma")

    Aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümüne ilişkin kaynakları tutan bir kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/management/overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Tercih ettiğiniz başka bir genel bölgeyi seçebilirsiniz.        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

3. Hesabın oluşturulması birkaç dakika sürer. İşlem durumunu izlemek için üstteki ilerleme çubuğunu görüntüleyin.

4. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Yeni**  >  **küme**' yi seçin.

    ![Azure 'da databricks](./media/data-lake-storage-quickstart-create-databricks-account/databricks-on-azure.png "Azure 'da databricks")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure 'da Databricks Spark kümesi oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-spark-cluster.png "Azure 'da Databricks Spark kümesi oluşturma")

    Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    - Küme için bir ad girin.
     
    - **120 dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

4. **Küme oluştur**' u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="create-notebook"></a>Not Defteri Oluştur

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturacak ve ardından depolama hesabını yapılandırmak için kod parçacıklarını çalıştıracaksınız.

1. [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks içinde bir not defteri oluşturmayı ve > Not Defteri Oluştur menü seçeneğini vurgulamaları gösteren ekran görüntüsü.](./media/data-lake-storage-quickstart-create-databricks-account/databricks-create-notebook.png "Databricks 'te Not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks 'te Not defteri oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-details.png "Databricks 'te Not defteri oluşturma")

    **Oluştur**’u seçin.

4. Aşağıdaki kod bloğunu kopyalayıp ilk hücreye yapıştırın, ancak henüz bu kodu çalıştırmayın.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<appID>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<password>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")

   ```
5. Bu kod bloğunda, `storage-account-name` `appID` `password` `tenant-id` Bu kod bloğundaki,, ve yer tutucu değerlerini hizmet sorumlusunu oluştururken topladığınız değerlerle değiştirin. `container-name`Yer tutucu değerini, kapsayıcıya vermek istediğiniz ada ayarlayın.

6. Bu bloktaki kodu çalıştırmak için **SHIFT + enter** tuşlarına basın.

## <a name="ingest-sample-data"></a>Örnek verileri ekleme

Bu bölüme başlamadan önce aşağıdaki önkoşulları tamamlamanız gerekir:

Aşağıdaki kodu bir not defteri hücresine girin:

```bash
%sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json
```

Hücrede, kodu çalıştırmak için **SHIFT + enter** tuşlarına basın.

Şimdi bu değerin altındaki yeni bir hücrede aşağıdaki kodu girin ve köşeli ayraçlar içinde görüntülenen değerleri daha önce kullandığınız değerlerle değiştirin:

```python
dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/")
```

Hücrede, kodu çalıştırmak için **SHIFT + enter** tuşlarına basın.

## <a name="run-a-spark-sql-job"></a>Spark SQL işi çalıştırma

Verilerde bir Spark SQL işi çalıştırmak için aşağıdaki görevleri gerçekleştirin.

1. **small_radio_json.json** adlı örnek JSON veri dosyasındaki verileri kullanarak geçici tablo oluşturmak için bir SQL deyimi çalıştırın. Aşağıdaki kod parçacığında yer tutucu değerlerini kapsayıcınızın adı ve depolama hesabı adı ile değiştirin. Daha önceden oluşturduğunuz not defterini kullanarak kod parçacığını not defterindeki yeni bir kod hücresine yapıştırın ve sonra SHIFT + ENTER tuşlarına basın.

    ```sql
    %sql
    DROP TABLE IF EXISTS radio_sample_data;
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path  "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json"
    )
    ```

    Komut başarıyla tamamlandıktan sonra, JSON dosyasındaki tüm verileri Databricks kümesinde tablo olarak görüntüleyebilirsiniz.

    `%sql` dili sihirli komutu, not defteri başka bir türde olsa bile not defterinden bir SQL kodu çalıştırmanızı sağlar. Daha fazla bilgi için bkz. [Bir not defterinde dilleri karıştırma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

2. Çalıştırdığınız sorguyu daha iyi anlamak için örnek JSON verilerinin bir anlık görüntüsüne bakalım. Kod hücresine aşağıdaki kod parçacığını yapıştırın ve **SHIFT + ENTER** tuşuna basın.

    ```sql
    %sql
    SELECT * from radio_sample_data
    ```

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir tablo çıktısı görürsünüz (yalnızca bazı sütunlar gösterilmiştir):

    ![Örnek JSON verileri](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sample-csv-data.png "Örnek JSON verileri")

    Diğer ayrıntıların yanı sıra, örnek veriler bir radyo kanalının (sütun adı, **cinsiyet**) hedef kitlesinin cinsiyetini ve aboneliğin ücretsiz mi yoksa ücretli mi (sütun adı, **düzey**) olduğunu yakalar.

4. Bu durumda, bu verilerin her bir cinsiyet, ücretsiz hesaba sahip kullanıcı sayısı ve ücretli hesabı olan abone sayısı için gösterilecek görsel bir açıklamasını oluşturursunuz. Tablo çıktısının alt kısmında bulunan **Çubuk grafik** simgesine ve sonra **Çizim Seçenekleri**’ne tıklayın.

    ![Çubuk grafik oluştur](./media/data-lake-storage-quickstart-create-databricks-account/create-plots-databricks-notebook.png "Çubuk grafik oluştur")

5. **Çizimi Özelleştir** menüsünde, değerleri ekran görüntüsünde gösterilen şekilde sürükleyip bırakın.

    ![Çizim çizimi ekranını ve sürüklediğiniz ve bırakabilirsiniz değerleri gösteren ekran görüntüsü.](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-customize-plot.png "Çubuk grafiği Özelleştir")

    - **Anahtarlar**’ı **cinsiyet** olarak ayarlayın.
    - **Seri gruplandırmalar**’ı **düzey** olarak ayarlayın.
    - **Değerler**’ı **düzey** olarak ayarlayın.
    - **Toplama**’yı **SAYI** olarak ayarlayın.

6. **Uygula**’ya tıklayın.

7. Çıktı aşağıdaki ekran görüntüsünde gösterildiği gibi görsel açıklamayı gösterir:

     ![Çubuk grafiği Özelleştir](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sql-query-output-bar-chart.png "Çubuk grafiği Özelleştir")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makaleyle işiniz bittiğinde kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanında **Kümeler**'i seçin ve sonlandırmak istediğiniz kümeyi bulun. Fare imlecini **Eylemler** sütununun altındaki üç noktanın üzerine götürün ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/data-lake-storage-quickstart-create-databricks-account/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Otomatik olarak durduğu kümeyi el ile sonlandırdıysanız, kümeyi oluştururken süre **\_ \_ etkinlik süresi dolduktan sonra Sonlandır** onay kutusunu işaretlediyseniz. Bu seçeneği belirlerseniz küme belirtilen zaman boyunca devre dışı olması halinde durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks’te bir Spark kümesi oluşturdunuz ve Data Lake Storage 2. Nesil etkin bir depolama hesabındaki verileri kullanarak bir Spark işi çalıştırdınız.

Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](/azure/databricks/scenarios/databricks-extract-load-sql-data-warehouse).

- Diğer veri kaynaklarından Azure Databricks verileri içeri aktarmayı öğrenmek için bkz. [Spark veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- Azure Databricks çalışma alanından Azure Data Lake Storage 2. erişmenin diğer yolları hakkında bilgi edinmek için bkz. [Azure Data Lake Storage 2.](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html).