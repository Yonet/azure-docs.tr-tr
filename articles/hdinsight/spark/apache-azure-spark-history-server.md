---
title: Uygulamalarda hata ayıklamak için Apache Spark geçmiş sunucuda genişletilmiş özellikleri kullanma-Azure HDInsight
description: Spark uygulamalarında hata ayıklamak ve tanılamak için Apache Spark geçmiş sunucusundaki genişletilmiş özellikleri kullanın-Azure HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/25/2019
ms.openlocfilehash: 1d53d9e9ee2a7fa6588ea0993b3bebdb2a287351
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98941049"
---
# <a name="use-the-extended-features-of-the-apache-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Spark uygulamalarında hata ayıklamak ve tanılamak için Apache Spark geçmiş sunucusunun genişletilmiş özelliklerini kullanın

Bu makalede, Apache Spark geçmiş sunucusunun genişletilmiş özelliklerinin, tamamlanan veya çalışan Spark uygulamalarında hata ayıklamak ve tanılamak için nasıl kullanılacağı gösterilir. Uzantı bir **veri** sekmesi, bir **grafik** sekmesi ve bir **Tanılama** sekmesi içerir. **Veri** sekmesinde Spark işinin giriş ve çıkış verilerini kontrol edebilirsiniz. **Grafik** sekmesinde veri akışını denetleyebilir ve iş grafiğini yeniden çalabilirsiniz. **Tanılama** sekmesinde **veri eğme**, **zaman eğriltme** ve **yürütücü Kullanım Analizi** özelliklerine başvurabilirsiniz.

## <a name="get-access-to-the-spark-history-server"></a>Spark geçmiş sunucusuna erişim sağlayın

Spark geçmiş sunucusu, tamamlanan ve Spark uygulamalarının çalıştırıldığı Web kullanıcı arabirimidir. Azure portal ya da URL 'den açabilirsiniz.

### <a name="open-the-spark-history-server-web-ui-from-the-azure-portal"></a>Spark geçmiş sunucusu Web Kullanıcı arabirimini Azure portal açın

1. [Azure Portal](https://portal.azure.com/)Spark kümesini açın. Daha fazla bilgi için bkz. [kümeleri listeleme ve gösterme](../hdinsight-administer-use-portal-linux.md#showClusters).
2. **Küme panolarından** **Spark geçmiş sunucusu**' nu seçin. İstendiğinde Spark kümesi için yönetici kimlik bilgilerini girin.

    ![Spark geçmiş sunucusunu Azure portal başlatın.](./media/apache-azure-spark-history-server/azure-portal-dashboard-spark-history.png "Spark geçmiş sunucusu")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>URL 'ye göre Spark geçmiş sunucusu Web Kullanıcı arabirimini açın

Spark geçmiş sunucusunu öğesine giderek açın `https://CLUSTERNAME.azurehdinsight.net/sparkhistory` , burada **clustername** , Spark kümenizin adıdır.

Spark geçmiş sunucusu Web Kullanıcı arabirimi şu resme benzer şekilde görünebilir:

![Spark geçmiş sunucusu sayfası.](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)

## <a name="use-the-data-tab-in-the-spark-history-server"></a>Spark geçmiş sunucusundaki veri sekmesini kullanın

İş KIMLIĞINI seçin ve ardından veri görünümünü görmek için araç menüsündeki **veriler** ' i seçin.

+ Ayrı sekmeleri seçerek **girişleri**, **çıkışları** ve **tablo işlemlerini** gözden geçirin.

    ![Spark uygulaması için veriler sayfasında veri sekmeleri.](./media/apache-azure-spark-history-server/apache-spark-data-tabs.png)

+ **Kopyala** düğmesini seçerek tüm satırları kopyalayın.

    ![Spark uygulaması sayfasındaki verileri kopyalayın.](./media/apache-azure-spark-history-server/apache-spark-data-copy.png)

+ Tüm verileri olarak kaydedin. **CSV düğmesini seçerek** CSV dosyası.

    ![Verileri bir olarak kaydedin. Spark uygulaması için veriler sayfasından CSV dosyası.](./media/apache-azure-spark-history-server/apache-spark-data-save.png)

+ **Arama** alanına anahtar sözcükler girerek verilerde arama yapın. Arama sonuçları hemen görüntülenecektir.

    ![Spark uygulaması için verilerde veri arama sayfası.](./media/apache-azure-spark-history-server/apache-spark-data-search.png)

+ Tabloyu sıralamak için sütun başlığını seçin. Daha fazla ayrıntı göstermek için bir satırı genişletmek üzere artı işaretini seçin. Bir satırı daraltmak için eksi işaretini seçin.

    ![Spark uygulaması için veriler sayfasında veri tablosu.](./media/apache-azure-spark-history-server/apache-spark-data-table.png)

+ Sağdaki **kısmi indirme** düğmesini seçerek tek bir dosyayı indirin. Seçili dosya yerel olarak indirilecek. Dosya artık mevcut değilse, bu işlem hata iletilerini göstermek için yeni bir sekme açar.

    ![Spark uygulaması için veriler sayfasında veri indirme satırı.](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ İndirme menüsünden genişleterek **tam yolu** Kopyala veya **göreli yolu Kopyala** seçeneğini belirleyerek bir tam yolu veya göreli yolu kopyalayın. Azure Data Lake Storage dosyalar için **Azure Depolama Gezgini aç** ' ı seçerek Azure Depolama Gezgini başlatın ve oturum açtıktan sonra klasörü bulun.

    ![Spark uygulaması için veriler sayfasında tam yolu kopyalayın ve göreli yol seçeneklerini kopyalayın.](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Tek bir sayfada görüntülenecek çok fazla satır varsa, gezinmek için tablonun altındaki sayfa numaralarını seçin.

    ![Spark uygulaması için veriler sayfasında sayfa numaraları.](./media/apache-azure-spark-history-server/apache-spark-data-page.png)

+ Daha fazla bilgi için, araç ipucunu göstermek üzere **Spark uygulamasının veriler** ' in yanındaki soru işaretine gelin veya seçin.

    ![Spark uygulaması için veriler sayfasında daha fazla bilgi alın.](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+  Sorunlar hakkında geri bildirim göndermek için **bize geri bildirim sağla**' yı seçin.

    ![Spark uygulaması için veriler sayfasında geri bildirim sağlayın.](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="use-the-graph-tab-in-the-spark-history-server"></a>Spark geçmiş sunucusunda Graph sekmesini kullanma

+ İş KIMLIĞINI seçin ve sonra iş grafiğini görmek için araç menüsündeki **grafik** ' i seçin. Varsayılan olarak, grafik tüm işleri gösterir. **Iş kimliği** açılır menüsünü kullanarak sonuçları filtreleyin.

    ![Spark uygulaması & Iş grafiği sayfasında Iş KIMLIĞI açılır menüsü.](./media/apache-azure-spark-history-server/apache-spark-graph-jobid.png)

+ Varsayılan olarak **ilerleme** seçilidir. **Görüntüle** açılan menüsünde **Oku** veya **yazılmış** ' i seçerek veri akışını kontrol edin.

    ![Spark uygulaması & Iş grafiği sayfasında veri akışını kontrol edin.](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

+ Her görevin arka plan rengi bir ısı eşlemesine karşılık gelir.

   ![Spark uygulaması & Iş grafiği sayfasında ısı haritası.](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)


    |Renk |Description |
    |---|---|
    |Yeşil|İş başarıyla tamamlandı.|
    |Orange|Görev başarısız oldu, ancak bu, işin nihai sonucunu etkilemez. Bu görevler daha sonra başarılı olabilecek yinelenen veya yeniden deneme örnekleri var.|
    |Mavi|Görev çalışıyor.|
    |Beyaz|Görev çalışmayı bekliyor veya aşama atlandı.|
    |Kırmızı|Görev başarısız oldu.|

     ![Spark uygulaması & Iş grafiği sayfasında bir görevi çalıştırma.](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)

     Atlanan aşamalar beyaz olarak görüntülenir.
    ![Spark uygulaması & Iş grafiği sayfasında atlanan bir görev.](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Spark uygulaması & Iş grafiği sayfasında başarısız olan bir görev.](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)

     > [!NOTE]  
     > Tamamlanan İşler için kayıttan yürütme kullanılabilir. İşi yeniden oynatmak için **kayıttan yürütme** düğmesini seçin. Durdur düğmesini seçerek işi istediğiniz zaman durdurun. Bir iş kayıttan yürütüldüğünde, her görev durumunu renge göre görüntüler. Tamamlanmamış işler için kayıttan yürütme desteklenmez.

+ İş grafiğinde yakınlaştırmak veya uzaklaştırmak için kaydırma yapın ya da ekrana sığacak şekilde **yakınlaştırmak Için Yakınlaştır** ' ı seçin.

    ![Spark uygulaması & Iş grafiği sayfasına sığacak şekilde Yakınlaştır ' ı seçin.](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Görevler başarısız olduğunda, araç ipucunu görmek için grafik düğümünün üzerine gelin ve sonra yeni bir sayfada açmak için aşamayı seçin.

    ![Spark uygulaması & Iş grafiği sayfasında araç ipucunu görüntüleyin.](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Spark uygulaması & Iş grafiği sayfasında, görevler şu koşullara uyuyorsa araç ipuçları ve küçük simgeler görüntülenir:
  + Veri eğriltme: veri okuma boyutu > bu aşama içindeki tüm görevlerin ortalama veri okuma boyutu * 2 *ve* veri okuma boyutu > 10 MB.
  + Zaman eğriltme: yürütme süresi > bu aşama içindeki tüm görevlerin ortalama yürütme süresi * 2 *ve* yürütme süresi > 2 dakika.

    ![Spark uygulaması & Iş grafiği sayfasında eğilmiş görev simgesi.](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ İş grafiği düğümü her aşama hakkında aşağıdaki bilgileri görüntüler:
  + ID
  + Ad veya açıklama
  + Toplam görev numarası
  + Okunan veriler: giriş boyutu ve karışık okuma boyutu toplamı
  + Veri yazma: çıkış boyutunun ve karışık yazma boyutunun toplamı
  + Yürütme Süresi: son denemede ilk deneme ve tamamlanma zamanının başlangıç saati arasındaki süre
  + Satır sayısı: giriş kayıtlarının toplamı, çıkış kayıtları, okuma kayıtlarını karıştırma ve yazma kayıtlarını karıştırma
  + İlerleme Durumu

    > [!NOTE]  
    > Varsayılan olarak, iş grafiği düğümü her aşamanın son denemesindeki bilgileri (aşama yürütme süresi dışında) görüntüler. Ancak kayıttan yürütme sırasında, iş grafiği düğümü her girişimde ilgili bilgileri gösterir.

    > [!NOTE]  
    > Veri okuma ve veri yazma boyutları için, 1MB = 1000 KB = 1000 * 1000 bayt kullandık.

+ Sorunlar hakkında görüşlerinizi **bize geri bildirim sağlayın**' i seçerek gönderin.

    ![Spark uygulaması & Iş grafiği sayfasındaki geri bildirim seçeneği.](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="use-the-diagnosis-tab-in-the-spark-history-server"></a>Spark geçmiş sunucusundaki tanılama sekmesini kullanın

İş KIMLIĞI ' ni seçin ve ardından araç menüsünde **Tanılama** ' yı seçerek iş tanılama görünümünü görüntüleyin. **Tanılama** sekmesi **veri eğriltme**, **zaman eğriltme** ve **yürütücü kullanım analizini** içerir.

+ Sırasıyla sekmeleri seçerek **veri eğriltme**, **zaman eğriltme** ve **yürütücü kullanım analizini** gözden geçirin.

    ![Tanılama sekmesinin içindeki veri eğriltme sekmesi.](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Veri eğriltme

**Veri eğriltme** sekmesini seçin. Karşılık gelen eğilmiş görevler belirtilen parametrelere göre görüntülenir.

#### <a name="specify-parameters"></a>Parametreleri belirtin

**Parametreleri belirtin** bölümü, veri eğriliğini algılamak için kullanılan parametreleri görüntüler. Varsayılan kural: okunan görev verileri ortalama görev verilerinin üç katından fazla okunan ve okunan görev verisi 10 MB 'tan fazla. Asimetrik görevler için kendi kuralınızı tanımlamak istiyorsanız parametrelerinizi seçebilirsiniz. **Çarpıklık** ve **eğme grafik** bölümleri buna uygun olarak güncelleştirilir.

#### <a name="skewed-stage"></a>Eğilmiş aşama

**Asimetrik aşama** bölümü, belirtilen ölçütlere uyan, asimetrik görevlere sahip aşamaları görüntüler. Bir aşamada birden çok asimetrik görev varsa, **asimetrik aşama** bölümü yalnızca en çok asimetrik görevi (yani veri eğriliği için en büyük verileri) görüntüler.

![Tanılama sekmesi içindeki veri eğriltme sekmesinin daha büyük görünümü.](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

##### <a name="skew-chart"></a>Grafiği eğ

**Eğme aşaması** tablosunda bir satır seçtiğinizde, **eğriltme grafiği** verileri okuma ve yürütme zamanına göre daha fazla görev dağıtım ayrıntısı görüntüler. Asimetrik görevler kırmızı olarak işaretlenir ve normal görevler mavi olarak işaretlenir. Performans değerlendirmesi için grafik en fazla 100 örnek görevi görüntüler. Görev ayrıntıları sağ alt panelde görüntülenir.

![Spark Kullanıcı arabirimindeki 10. aşama için eğriliği grafiği.](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zaman eğriltme

**Zaman eğriltme** sekmesi, görev yürütme zamanına göre asimetrik görevleri görüntüler.

#### <a name="specify-parameters"></a>Parametreleri belirtin

**Parametreleri belirtin** bölümü, zaman eğriliği algılamak için kullanılan parametreleri görüntüler. Varsayılan kural: görev yürütme süresi, ortalama yürütme zamanının üç katından fazla ve görev yürütme süresi 30 saniyeden fazla. Parametreleri gereksinimlerinize göre değiştirebilirsiniz. **Çarpıklık** ve **eğme grafiğinde** , ilgili aşamalar ve görev bilgileri, tıpkı **veri eğriltme** sekmesinde olduğu gibi görüntülenir.

**Zaman eğriliği** seçtiğinizde, filtrelenmiş sonuç, **parametreleri belirt** bölümünde ayarlanan parametrelere göre **asimetrik aşama** bölümünde görüntülenir. **Çarpıtılmış aşama** bölümünde bir öğe seçtiğinizde, ilgili grafik üçüncü bölümde drafted ve görev ayrıntıları sağ alt panelde görüntülenir.

![Tanılama sekmesi içindeki zaman eğriltme sekmesi.](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis-graphs"></a>Yürütücü Kullanım Analizi grafikleri

**Yürütücü kullanım grafiği** , işin gerçek yürütücü ayırmasını ve çalışma durumunu görüntüler.  

**Yürütücü Kullanım Analizi**' ni seçtiğinizde, yürütücü kullanımı hakkında dört farklı eğri drafted: **ayrılmış Yürüticileri**, **yürüticileri**, **boştaki yürüticileri** ve **en fazla yürütücü örneğini** çalıştırın. Her **yürütücü eklenen** veya **yürütücü tarafından kaldırılan** olay, ayrılan yürütmeleri arttırır veya azaltır. Daha fazla karşılaştırma için **işler** sekmesinde **olay zaman çizelgesini** kontrol edebilirsiniz.

![Tanılama sekmesinin içindeki yürütücü Kullanım Analizi sekmesi.](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

Tüm taslaklarına karşılık gelen içeriği seçmek veya seçimini kaldırmak için renk simgesini seçin.

 ![Yürütücü Kullanım Analizi sekmesinde grafiği seçin.](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="faq"></a>SSS

### <a name="how-do-i-revert-to-the-community-version"></a>Nasıl yaparım? topluluk sürümüne dönmek istiyor musunuz?

Topluluk sürümüne dönmek için aşağıdaki adımları uygulayın.

1. Kümeyi ambarı 'nda açın.
1. **Spark2**  >  **configs** sayfasına gidin.
1. **Özel spark2-varsayılanlar**' ı seçin.
1. **Özellik Ekle...** seçeneğini belirleyin.
1. **Spark. UI. geliştirmesi. Enabled = False** ekleyin ve sonra kaydedin.
1. Özelliği şimdi **yanlış** olarak ayarlanır.
1. Yapılandırmayı kaydetmek için **Kaydet** ' i seçin.

    ![Apache ambarı 'nda bir özelliği devre dışı bırakın.](./media/apache-azure-spark-history-server/apache-spark-turn-off.png)

1. Sol panelde **Spark2** öğesini seçin. Ardından **Özet** sekmesinde **Spark2 geçmiş sunucusu**' nu seçin.

    ![Apache ambarı 'ndaki Özet görünümü.](./media/apache-azure-spark-history-server/apache-spark-restart1.png)

1. Spark geçmiş sunucusunu yeniden başlatmak için **Spark2 geçmiş sunucusunun** sağ tarafındaki **başlatılan** düğmesini seçin ve ardından açılan menüden **Yeniden Başlat** ' ı seçin.

    ![Apache ambarı 'nda Spark geçmiş sunucusunu yeniden başlatın.](./media/apache-azure-spark-history-server/apache-spark-restart2.png)  

1. Spark geçmiş sunucusu Web Kullanıcı arabirimini yenileyin. Topluluk sürümüne döndürülür.

### <a name="how-do-i-upload-a-spark-history-server-event-to-report-it-as-an-issue"></a>Nasıl yaparım?, bir Spark geçmiş sunucu olayını bir sorun olarak bildirmek üzere karşıya yükleyebilir mi?

Spark geçmiş sunucusunda bir hata halinde çalıştırırsanız, olayı raporlamak için aşağıdaki adımları uygulayın.

1. Spark geçmiş sunucusu Web Kullanıcı arabiriminde **İndir** ' i seçerek olayı indirin.

    ![Olayı Spark geçmiş sunucusu kullanıcı arabirimine indirin.](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. **Spark uygulaması & Iş grafiği** sayfasından **bize geri bildirim sağla** ' yı seçin.

    ![Spark uygulaması & Iş grafiği sayfasında geri bildirim sağlayın](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Hatanın başlığını ve açıklamasını girin. Sonra,. zip dosyasını düzenleme alanına sürükleyin ve **yeni sorun gönder**' i seçin.

    ![Karşıya yükleyin ve yeni bir sorun gönderin.](./media/apache-azure-spark-history-server/apache-spark-file-issue.png)

### <a name="how-do-i-upgrade-a-jar-file-in-a-hotfix-scenario"></a>Bir düzeltme senaryosunda. jar dosyasını yükseltmek Nasıl yaparım? mi?

' Yi bir düzeltme ile yükseltmek istiyorsanız, aşağıdaki betiği kullanarak Yükseltilecek olan öğesini kullanın `spark-enhancement.jar*` .

**upgrade_spark_enhancement. sh**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

#### <a name="usage"></a>Kullanım

`upgrade_spark_enhancement.sh https://${jar_path}`

#### <a name="example"></a>Örnek

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`

#### <a name="use-the-bash-file-from-the-azure-portal"></a>Azure portal bash dosyasını kullanın

1. [Azure Portal](https://ms.portal.azure.com)başlatın ve sonra kümenizi seçin.
2. Aşağıdaki parametrelerle bir [betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md) doldurun.

    |Özellik |Değer |
    |---|---|
    |Betik türü|-Özel|
    |Name|UpgradeJar|
    |Bash betiği URI 'SI|`https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh`|
    |Düğüm türleri|Baş, çalışan|
    |Parametreler|`https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`|

     ![Betik eylemini Azure portal gönder](./media/apache-azure-spark-history-server/apache-spark-upload1.png)

## <a name="known-issues"></a>Bilinen sorunlar

+ Şu anda Spark geçmiş sunucusu yalnızca Spark 2,3 ve 2,4 için geçerlidir.

+ RDD kullanan giriş ve çıkış verileri, **veri** sekmesinde görüntülenmez.

## <a name="next-steps"></a>Sonraki adımlar

+ [HDInsight üzerinde Apache Spark kümesi için kaynakları yönetme](apache-spark-resource-manager.md)
+ [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)

## <a name="suggestions"></a>Öneriler

Bu aracı kullanırken geri bildiriminiz varsa veya herhangi bir sorun yaşıyorsanız, () adresine bir e-posta gönderin [hdivstool@microsoft.com](mailto:hdivstool@microsoft.com) .
