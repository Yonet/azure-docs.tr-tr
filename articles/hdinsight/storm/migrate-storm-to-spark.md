---
title: Azure HDInsight 3,6 Apache Storm HDInsight 4,0 Apache Spark geçirin
description: Apache Storm iş yüklerini Spark akışına veya Spark yapılandırılmış akışa geçirmeye yönelik farklılıklar ve geçiş akışı.
ms.service: hdinsight
ms.topic: how-to
ms.date: 01/16/2019
ms.openlocfilehash: aa57c01558cfdcf069b17fad9e86f7640553dcfd
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944788"
---
# <a name="migrate-azure-hdinsight-36-apache-storm-to-hdinsight-40-apache-spark"></a>Azure HDInsight 3,6 Apache Storm HDInsight 4,0 Apache Spark geçirin

Bu belge HDInsight 3,6 ' de HDInsight 4,0 ' de Apache Storm iş yüklerini nasıl geçirebileceğinizi açıklamaktadır. HDInsight 4,0 Apache Storm küme türünü desteklemez ve başka bir akış veri platformuna geçirmeniz gerekir. Apache Spark akış ve Spark yapılandırılmış akış olmak üzere iki uygun seçenek vardır. Bu belgede, bu platformlar arasındaki farklılıklar açıklanmakta ve ayrıca Apache Storm iş yüklerini geçirmek için bir iş akışı önerilmektedir.

## <a name="storm-migration-paths-in-hdinsight"></a>HDInsight 'ta fırtınası geçiş yolları

HDInsight 3,6 ' den Apache Storm geçirmek istiyorsanız, birden çok seçeneğiniz vardır:

* HDInsight 'ta Spark akışı 4,0
* HDInsight 'ta Spark yapılandırılmış akışı 4,0
* Azure Stream Analytics

Bu belge, Apache Storm Spark akışına ve Spark yapılandırılmış akışa geçiş için bir kılavuz sağlar.

> [!div class="mx-imgBorder"]
> ![HDInsight fırtınası geçiş yolu](./media/migrate-storm-to-spark/storm-migration-path.png)

## <a name="comparison-between-apache-storm-and-spark-streaming-spark-structured-streaming"></a>Apache Storm ile Spark akışı, Spark yapılandırılmış akışı arasında karşılaştırma

Apache Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir fırtınası uygulaması en az bir kez işlemeyi garanti edebilir ve [Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) tam bir kez işlemeyi garanti edebilir. Spark akışı ve Spark yapılandırılmış akış, bir düğüm hatası gerçekleşse bile herhangi bir giriş olayının tam olarak bir kez işlenmesini güvence altına almaz. Fırtınası her bir olayı işleyen bir modele sahiptir ve mikro Batch modelini Trident ile de kullanabilirsiniz. Spark akışı ve Spark yapılandırılmış akış Micro-Batch işleme modeli sağlar.

|  |Storm |Spark Streaming | Spark yapılandırılmış akışı|
|---|---|---|---|
|**Olay işleme garantisi**|En az bir kez <br> Tek bir kez (Trident) |[Tam olarak bir kez](https://spark.apache.org/docs/latest/streaming-programming-guide.html)|[Tam olarak bir kez](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)|
|**İşlem modeli**|Gerçek zamanlı <br> Mikro Batch (Trident) |Mikro Batch |Mikro Batch |
|**Olay saati desteği**|[Evet](https://storm.apache.org/releases/2.0.0/Windowing.html)|Hayır|[Evet](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)|
|**Diller**|Java, vb.|Scala, Java, Python|Python, R, Scala, Java, SQL|

### <a name="spark-streaming-vs-spark-structured-streaming"></a>Spark akışı ile Spark yapılandırılmış akışı

Spark yapısal akışı Spark akışını (DStreams) değiştiriyor. Yapılandırılmış akış geliştirmeler ve bakım almaya devam eder, ancak DStreams yalnızca bakım modunda olur. **Note: Bu noktayı vurgulamak için bağlantılara ihtiyacınız vardır**. Yapılandırılmış akış, kaynak ve havuzları için DStreams kadar birçok özelliğe sahip değildir ve bu nedenle, uygun Spark akış işleme seçeneğini belirlemek için gereksinimlerinizi değerlendirin.

## <a name="streaming-single-event-processing-vs-micro-batch-processing"></a>Akış (tek olay) işleme ve Micro-Batch işleme karşılaştırması

Fırtınası, tek bir olayı işleyen bir model sağlar. Bu, tüm gelen kayıtların geldikçe hemen işleneceği anlamına gelir. Spark akış uygulamaları, bu toplu işi işlenmek üzere göndermeden önce her bir mikro-toplu olay toplamanız gerekir. Buna karşılık, olay temelli bir uygulama her olayı hemen işler. Spark akış gecikmesi genellikle birkaç saniye altında. Mikro Batch yaklaşımının avantajları daha verimli veri işleme ve daha basit toplu hesaplamalardır.

> [!div class="mx-imgBorder"]
> ![akış ve mikro-Batch işleme](./media/migrate-storm-to-spark/streaming-and-micro-batch-processing.png)

## <a name="storm-architecture-and-components"></a>Fırtınası mimarisi ve bileşenleri

Storm topolojileri döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Veriler grafikteki bileşenler arasında akar. Her bileşen bir veya daha fazla veri akışı kullanır ve isteğe bağlı olarak bir veya daha fazla akış yayar.

|Bileşen |Açıklama |
|---|---|
|Spout|Verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.|
|Sü|Spout veya diğer cıvatlardan yayılan akışları kullanır. Boltlar topolojiye isteğe bağlı olarak akışlar yayabilir. Boltlar ayrıca HDFS, Kafka veya HBase gibi dış hizmetlere veya depolama alanlarına veri yazmaktan sorumludur.|

> [!div class="mx-imgBorder"]
> ![fırtınası bileşenleri etkileşimi](./media/migrate-storm-to-spark/apache-storm-components.png)

Fırtınası, fırtınası kümesinin çalışmasını tutan aşağıdaki üç Daemon 'ları oluşur.

|İnin |Description |
|---|---|
|Nimbus|Hadoop JobTracker 'e benzer şekilde, kodu kümeye dağıtmaktan ve makinelere görevler atamaya ve hatalara yönelik izlemeye karşı sorumludur.|
|Zookeeper|Küme düzenlemesi için kullanılır.|
|Gözetmen|Makinesine atanan işleri dinler ve Nimbus ' deki yönergeleri temel alarak çalışan süreçlerini başlatır ve sonlandırır. Her çalışan işlemi bir topolojinin alt kümesini yürütür. Kullanıcının uygulama mantığı (Spomalar ve sürgüsü) burada çalıştırılır.|

> [!div class="mx-imgBorder"]
> ![Nimbus, Zookeeper ve gözetmen Daemon 'ları](./media/migrate-storm-to-spark/nimbus-zookeeper-supervisor.png)

## <a name="spark-streaming-architecture-and-components"></a>Spark akış mimarisi ve bileşenleri

Aşağıdaki adımlarda, bileşenlerin Spark akışı (DStreams) ve Spark yapılandırılmış akışında birlikte nasıl çalıştığı özetlenmektedir:

* Spark akışı başlatıldığında, sürücü, görevi yürütücü içinde başlatır.
* Yürütücü akış veri kaynağından bir akış alır.
* Yürütücü veri akışları aldığında, akışı bloklara böler ve bellekte tutar.
* Veri blokları diğer yürüticilerine çoğaltılır.
* İşlenen veriler daha sonra hedef veri deposunda depolanır.

> [!div class="mx-imgBorder"]
> ![çıkış için Spark akış yolu](./media/migrate-storm-to-spark/spark-streaming-to-output.png)

## <a name="spark-streaming-dstream-workflow"></a>Spark streaming (DStream) iş akışı

Her toplu iş aralığı geçtiğinde, bu aralıktaki tüm verileri içeren yeni bir RDD oluşturulur. Sürekli RDDs kümeleri bir DStream 'e toplanır. Örneğin, toplu iş aralığı bir ikinci uzunsa DStream, saniye içinde alınan tüm verileri içeren bir RDD 'yi içeren bir toplu işlem yayar. DStream işlenirken, sıcaklık olayı Bu toplu işlemlerden birinde görünür. Spark akış uygulaması olayları içeren toplu işleri işler ve sonunda her bir RDD 'de depolanan veriler üzerinde işlem yapar.

> [!div class="mx-imgBorder"]
> ![Spark akış işleme toplu işlemleri](./media/migrate-storm-to-spark/spark-streaming-batches.png)

Spark akışı ile kullanılabilen farklı dönüşümlerle ilgili ayrıntılar için bkz. [DStreams üzerindeki dönüşümler](https://spark.apache.org/docs/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="spark-structured-streaming"></a>Spark yapılandırılmış akışı

Spark yapılandırılmış akış, bir veri akışını derinlemesine sınırsız bir tablo olarak temsil eder. Yeni veri geldiğinde tablo büyümeye devam eder. Bu giriş tablosu, uzun süre çalışan bir sorgu tarafından sürekli işlenir ve sonuçlar bir çıkış tablosuna gönderilir.

Yapılandırılmış akışta, veriler sisteme ulaşır ve hemen bir giriş tablosuna alınır. Bu giriş tablosuna yönelik işlemler gerçekleştiren sorguları (DataFrame ve DataSet API 'Leri kullanarak) yazarsınız.

Sorgu çıktısı sorgunuzun sonuçlarını içeren bir *sonuç tablosu* oluşturur. İlişkisel bir veritabanı gibi, bir dış veri deposu için sonuçlar tablosundan veri çizebilirsiniz.

Giriş tablosundan verilerin işlendiği zaman zamanlaması, tetikleyici aralığı tarafından denetlenir. Varsayılan olarak, tetikleyici aralığı sıfırdır, bu nedenle yapılandırılmış akış, verileri ulaştığı anda işlemeye çalışır. Uygulamada, bu, yapılandırılmış akış önceki sorgunun çalışmasını işlemeyi tamamladıktan sonra, yeni alınan tüm verilere karşı başka bir işlem çalıştırması başlattığı anlamına gelir. Tetikleyiciyi, zaman tabanlı toplu işlerle işlenmek üzere bir aralıkta çalışacak şekilde yapılandırabilirsiniz.

> [!div class="mx-imgBorder"]
> ![yapılandırılmış akışta verilerin işlenmesi](./media/migrate-storm-to-spark/structured-streaming-data-processing.png)

> [!div class="mx-imgBorder"]
> ![yapılandırılmış akış için programlama modeli](./media/migrate-storm-to-spark/structured-streaming-model.png)

## <a name="general-migration-flow"></a>Genel geçiş akışı

Geçiş işleminden Spark 'a önerilen geçiş akışı, aşağıdaki ilk mimariyi varsayar:

* Kafka, akış veri kaynağı olarak kullanılır
* Kafka ve fırtınası aynı sanal ağa dağıtıldı
* Fırtınası tarafından işlenen veriler, Azure depolama veya Azure Data Lake Storage 2. gibi bir veri havuzuna yazılır.

    > [!div class="mx-imgBorder"]
    > ![kabul eden geçerli ortamın diyagramı](./media/migrate-storm-to-spark/presumed-current-environment.png)

Uygulamanızı fırtınası 'dan Spark akış API 'Lerinden birine geçirmek için aşağıdakileri yapın:

1. **Yeni bir küme dağıtın.** Aynı sanal ağa yeni bir HDInsight 4,0 Spark kümesi dağıtın ve Spark akışını veya Spark yapılandırılmış akış uygulamanızı bu kümeye dağıtın ve tamamen test edin.

    > [!div class="mx-imgBorder"]
    > ![HDInsight 'ta yeni Spark dağıtımı](./media/migrate-storm-to-spark/new-spark-deployment.png)

1. **Eski fırtınası kümesinde kullanmayı durdurun.** Mevcut bir fırtınası içinde, akış veri kaynağından veri tüketmeyi durdurun ve verilerin hedef havuza yazılmasını tamamlamasını bekleyin.

    > [!div class="mx-imgBorder"]
    > ![geçerli kümede kullanmayı durdur](./media/migrate-storm-to-spark/stop-consuming-current-cluster.png)

1. **Yeni Spark kümesinde kullanmayı başlatın.** Yeni dağıtılan HDInsight 4,0 Spark kümesinden veri akışı başlatın. Şu anda, işlem en son Kafka uzaklığında tüketerek alınır.

    > [!div class="mx-imgBorder"]
    > ![yeni kümede kullanmayı Başlat](./media/migrate-storm-to-spark/start-consuming-new-cluster.png)

1. **Eski kümeyi gerektiği gibi kaldırın.** Anahtar tamamlandıktan ve düzgün şekilde çalışmaya başladıktan sonra eski HDInsight 3,6 fırtınası kümesini gerektiği gibi kaldırın.

    > [!div class="mx-imgBorder"]
    > ![gerekli olan eski HDInsight kümelerini kaldırın](./media/migrate-storm-to-spark/remove-old-clusters1.png)

## <a name="next-steps"></a>Sonraki adımlar

Fırtınası, Spark akışı ve Spark yapılandırılmış akış hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Apache Spark akışa genel bakış](../spark/apache-spark-streaming-overview.md)
* [Apache Spark yapılandırılmış akışa genel bakış](../spark/apache-spark-structured-streaming-overview.md)
