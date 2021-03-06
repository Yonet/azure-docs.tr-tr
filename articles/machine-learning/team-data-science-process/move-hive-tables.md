---
title: Hive tabloları oluşturma ve BLOB depolamadan veri yükleme-Team Data Science Işlemi
description: Hive tabloları oluşturmak ve Azure Blob depolamadan veri yüklemek için Hive sorguları kullanın. Hive tablolarını bölümlemek ve sorgu performansını artırmak için en Iyileştirilmiş satır sütunlu (ORC) biçimlendirmeyi kullanın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 5d61c0f5f26bc46b9c4a5bc4a793df1e10710004
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96006738"
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Azure Blob depolamadan Hive tabloları oluşturma ve veri yükleme

Bu makalede Hive tabloları oluşturan ve Azure Blob depolamadan veri yükleyen genel Hive sorguları sunulmaktadır. Ayrıca, yığın tablolarına bölümlendirme ve sorgu performansını artırmak için Iyileştirilmiş satır sütunlu (ORC) biçimlendirme kullanılarak da bazı yönergeler sunulmaktadır.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede sahip olduğunuz varsayılır:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz. [Azure depolama hesapları hakkında](../../storage/common/storage-introduction.md).
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi sağlandı.  Yönergelere ihtiyacınız varsa bkz. [HDInsight 'Ta kümeleri ayarlama](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).
* Kümeye uzaktan erişim etkinleştirildi, oturum açıldı ve Hadoop Command-Line konsolunu açtı. Yönergelere ihtiyacınız varsa bkz. [Apache Hadoop kümelerini yönetme](../../hdinsight/hdinsight-administer-use-portal-linux.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme
[Gelişmiş analiz için bir Azure sanal makinesi ayarlama](../../machine-learning/data-science-virtual-machine/overview.md)bölümünde belirtilen yönergeleri izleyerek bir Azure sanal makinesi oluşturduysanız, bu betik dosyası sanal makinede *C: \\ Users \\ \<user name\> \\ belgeleri \\ veri bilimi betikleri* dizinine indirilmelidir. Bu Hive sorguları yalnızca, gönderim için hazırlamak üzere uygun alanlara bir veri şeması ve Azure Blob depolama yapılandırması sağlamanızı gerektirir.

Hive tablolarının verilerinin **sıkıştırılmamış** tablosal biçiminde olduğunu ve verilerin Hadoop kümesi tarafından kullanılan depolama hesabının varsayılan (veya ek) kapsayıcısına yüklendiğini varsaytık.

**NYC Vergileni seyahat verilerinde** uygulama yapmak istiyorsanız şunları yapmanız gerekir:

* 24 [NYC TAXI seyahat veri](https://www.andresmh.com/nyctaxitrips) dosyalarını **Indirin** (12 seyahat dosyası ve 12 tarifeli havayolu dosyası),
* tüm dosyaları. csv dosyalarına **ayıklayın** ve ardından
* Bunları Azure Storage hesabının varsayılan (veya uygun kapsayıcısına) birine **yükleyin** ; Böyle bir hesabın seçenekleri [Azure HDInsight kümeleri Ile Azure depolama kullanma](../../hdinsight/hdinsight-hadoop-use-blob-storage.md) konusunda görünür. . Csv dosyalarını depolama hesabındaki varsayılan kapsayıcıya yükleme işlemi bu [sayfada](hive-walkthrough.md#upload)bulunabilir.

## <a name="how-to-submit-hive-queries"></a><a name="submit"></a>Hive sorguları gönderme
Hive sorguları kullanılarak gönderilebilir:

* [Hadoop kümesinin baş düğümüne 'daki Hadoop komut satırı aracılığıyla Hive sorguları gönderme](#headnode)
* [Hive Düzenleyicisi ile Hive sorguları gönderme](#hive-editor)
* [Azure PowerShell komutlarla Hive sorguları gönderme](#ps)

Hive sorguları SQL gibidir. SQL hakkında bilginiz varsa, [SQL kullanıcıları Için Hive sayfasını](https://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) yararlı bulabilirsiniz.

Hive sorgusu gönderirken, yığın sorgularından çıktının hedefini, ekranda ya da baş düğümdeki yerel bir dosyaya ya da bir Azure blobuna veya bir Azure Blob 'una göre denetleyebilirsiniz.

### <a name="submit-hive-queries-through-hadoop-command-line-in-headnode-of-hadoop-cluster"></a><a name="headnode"></a>Hadoop kümesinin baş düğümüne 'daki Hadoop komut satırı aracılığıyla Hive sorguları gönderme
Hive sorgusu karmaşıksa, doğrudan Hadoop kümesinin baş düğümünde gönderilmesi, bir Hive Düzenleyicisi veya Azure PowerShell betikleri ile gönderilmeden daha hızlı bir şekilde daha hızlı bir şekilde başa gönderim sağlar.

Hadoop kümesinin baş düğümünde oturum açın, baş düğüm masaüstündeki Hadoop komut satırını açın ve komut girin `cd %hive_home%\bin` .

Hadoop komut satırında Hive sorguları göndermenin üç yolu vardır:

* hemen
* '. HQL ' dosyalarını kullanma
* Hive komut konsoluyla

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Hive sorgularını doğrudan Hadoop komut satırında gönder.
`hive -e "<your hive query>;`Doğrudan Hadoop komut satırında basit Hive sorguları göndermek için komutunu çalıştırabilirsiniz. Burada, kırmızı kutu Hive sorgusunu gönderen komutu özetleyen bir örnektir ve yeşil kutu, Hive sorgusundan gelen çıktıyı özetler.

![Hive sorgusundan çıkış ile Hive sorgusu gönderme komutu](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>'. HQL ' dosyalarında Hive sorguları gönder
Hive sorgusu daha karmaşıktır ve birden çok satıra sahipse, komut satırında veya Hive komut konsolunda sorguları düzenlemeniz pratik değildir. Diğer bir seçenek de, Hadoop kümesinin baş düğümünde bir metin düzenleyicisi kullanarak Hive sorgularını baş düğümün yerel dizinindeki bir '. HQL ' dosyasına kaydeder. Ardından, '. HQL ' dosyasındaki Hive sorgusu, `-f` bağımsız değişken kullanılarak şu şekilde gönderilebilir:

```console
hive -f "<path to the '.hql' file>"
```

!['. HQL ' dosyasında Hive sorgusu](./media/move-hive-tables/run-hive-queries-3.png)

**İlerleme durumu ekranını gizle Hive sorgularının yazdırılması**

Varsayılan olarak, Hive sorgusu Hadoop komut satırında gönderildikten sonra harita/küçültme işinin ilerlemesi ekranda yazdırılır. Harita/iş ilerlemesini azaltma ekranını bastırmak için, `-S` komut satırında aşağıdaki gibi bir bağımsız değişken (büyük harfle "S") kullanabilirsiniz:

```console
hive -S -f "<path to the '.hql' file>"
hive -S -e "<Hive queries>"
```

#### <a name="submit-hive-queries-in-hive-command-console"></a>Hive sorgularını Hive komut konsoluna gönderebilirsiniz.
Ayrıca, Hadoop komut satırında komutunu çalıştırarak Hive komut konsolunu girip Hive `hive` sorgularını Hive komut konsoluna gönderebilirsiniz. Aşağıda bir örnek verilmiştir. Bu örnekte, iki kırmızı kutu Hive komut konsolunu girmek için kullanılan komutları ve sırasıyla Hive komut konsolunda gönderilen Hive sorgusunu vurgular. Yeşil kutu, Hive sorgusunun çıktısını vurgular.

![Hive komut konsolunu açın ve komut girin, Hive sorgu çıkışını görüntüleyin](./media/move-hive-tables/run-hive-queries-2.png)

Önceki örnekler, Hive sorgu sonuçlarının ekranda doğrudan çıktısını alır. Çıktıyı, baş düğümdeki yerel bir dosyaya veya bir Azure blobuna de yazabilirsiniz. Daha sonra, Hive sorgularının çıkışını daha fazla analiz etmek için diğer araçları kullanabilirsiniz.

**Çıktı Hive sorgusu sonuçları yerel bir dosyaya göre yapılır.**
Hive sorgu sonuçlarının baş düğümdeki yerel bir dizine çıkışını sağlamak için, Hive sorgusunu aşağıdaki gibi Hadoop komut satırına göndermeniz gerekir:

```console
hive -e "<hive query>" > <local path in the head node>
```

Aşağıdaki örnekte, Hive sorgusunun çıktısı dizindeki bir dosyaya yazılır `hivequeryoutput.txt` `C:\apps\temp` .

![Ekran görüntüsü, bir Hadoop komut satırı penceresinde Hive sorgusunun çıkışını gösterir.](./media/move-hive-tables/output-hive-results-1.png)

**Çıktı Hive sorgusu sonuçları bir Azure Blob 'una**

Ayrıca, Hadoop kümesinin varsayılan kapsayıcısı dahilinde Hive sorgu sonuçlarını bir Azure Blob 'una de aktarabilirsiniz. Bunun için Hive sorgusu aşağıdaki gibidir:

```console
insert overwrite directory wasb:///<directory within the default container> <select clause from ...>
```

Aşağıdaki örnekte, Hive sorgusunun çıktısı, `queryoutputdir` Hadoop kümesinin varsayılan kapsayıcısı içindeki bir blob dizinine yazılır. Burada, dizin adını yalnızca blob adı olmadan sağlamanız gerekir. Hem dizin hem de blob adlarını (gibi) sağlarsanız bir hata oluşur `wasb:///queryoutputdir/queryoutput.txt` .

![Ekran görüntüsünde, Hadoop komut satırı penceresinde önceki komut gösterilir.](./media/move-hive-tables/output-hive-results-2.png)

Azure Depolama Gezgini kullanarak Hadoop kümesinin varsayılan kapsayıcısını açarsanız, Hive sorgusunun çıkışını aşağıdaki şekilde gösterildiği gibi görebilirsiniz. Filtreyi yalnızca adlarda belirtilen harfler ile almak için (kırmızı kutu vurgulu olarak vurgulanır) uygulayabilirsiniz.

![Hive sorgusunun çıkışını gösteren Azure Depolama Gezgini](./media/move-hive-tables/output-hive-results-3.png)

### <a name="submit-hive-queries-with-the-hive-editor"></a><a name="hive-editor"></a>Hive Düzenleyicisi ile Hive sorguları gönderme
Bir Web tarayıcısına *https: \/ / \<Hadoop cluster name> . azurehdinsight.net/Home/HiveEditor* biçiminde bir URL girerek sorgu konsolunu (Hive Düzenleyicisi) de kullanabilirsiniz. Bu konsolu görmeniz için oturum açmış olmanız gerekir. bu nedenle, Hadoop kümesi kimlik bilgilerinizin burada olması gerekir.

### <a name="submit-hive-queries-with-azure-powershell-commands"></a><a name="ps"></a>Azure PowerShell komutlarla Hive sorguları gönderme
Ayrıca, PowerShell kullanarak Hive sorguları gönderebilirsiniz. Yönergeler için bkz. [PowerShell kullanarak Hive Işleri gönderme](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md).

## <a name="create-hive-database-and-tables"></a><a name="create-tables"></a>Hive veritabanı ve tabloları oluşturma
Hive sorguları [GitHub deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) paylaşılır ve buradan indirilebilir.

Hive tablosu oluşturan Hive sorgusu aşağıda verilmiştir.

```hiveql
create database if not exists <database name>;
CREATE EXTERNAL TABLE if not exists <database name>.<table name>
(
    field1 string,
    field2 int,
    field3 float,
    field4 double,
    ...,
    fieldN string
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");
```

Aşağıda, eklenti ve diğer yapılandırma için gereken alanların açıklamaları verilmiştir:

* **\<database name\>**: oluşturmak istediğiniz veritabanının adı. Yalnızca varsayılan veritabanını kullanmak istiyorsanız, "*veritabanı oluştur...*" sorgusu atlanabilir.
* **\<table name\>**: belirtilen veritabanı içinde oluşturmak istediğiniz tablonun adı. Varsayılan veritabanını kullanmak istiyorsanız, tabloya doğrudan tarafından başvurulabilir *\<table name\>* \<database name\> .
* **\<field separator\>**: veri dosyasında Hive tablosuna yüklenecek alanları sınırlandıran ayırıcı.
* **\<line separator\>**: veri dosyasındaki satırları sınırlandıran ayırıcı.
* **\<storage location\>**: Hive tablolarının verilerini kaydetmek için Azure depolama konumu. *Konum \<storage location\>* belirtmezseniz, veritabanı ve tablolar varsayılan olarak Hive kümesinin varsayılan kapsayıcısında *Hive/Warehouse/* Directory içinde depolanır. Depolama konumunu belirtmek istiyorsanız, depolama konumunun veritabanı ve tablolar için varsayılan kapsayıcı içinde olması gerekir. Bu konum, kümenin varsayılan kapsayıcısına göre *' wasb:/// \<directory 1> /'* veya *' wasb:/// \<directory 1> / \<directory 2> /'* biçiminde bir konum olarak adlandırılmalıdır. Sorgu yürütüldükten sonra, ilişkili dizinler varsayılan kapsayıcı içinde oluşturulur.
* **TBLPROPERTIES ("Skip. Header. Line. Count" = "1")**: veri dosyasında bir başlık satırı varsa, bu özelliği *Create Table* sorgusunun **sonuna** eklemeniz gerekir. Aksi takdirde, başlık satırı tabloya bir kayıt olarak yüklenir. Veri dosyasında bir başlık satırı yoksa, bu yapılandırma sorguda atlanabilir.

## <a name="load-data-to-hive-tables"></a><a name="load-data"></a>Hive tablolarına veri yükleme
Verileri bir Hive tablosuna yükleyen Hive sorgusu aşağıda verilmiştir.

```hiveql
LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;
```

* **\<path to blob data\>**: Hive tablosuna yüklenecek blob dosyası HDInsight Hadoop kümesinin varsayılan kapsayıcıda ise, *\<path to blob data\>* *' wasb:// \<directory in this container> / \<blob file name> '* biçiminde olmalıdır. Blob dosyası, HDInsight Hadoop kümesinin ek bir kapsayıcısında de olabilir. Bu durumda, *\<path to blob data\>* *' wasb:// \<container name> @ \<storage account name> . blob.Core.Windows.net/ \<blob file name> '* biçiminde olmalıdır.

  > [!NOTE]
  > Hive tablosuna yüklenecek blob verileri, Hadoop kümesi için depolama hesabının varsayılan veya ek kapsayıcısında olmalıdır. Aksi halde, veri *yükleme* sorgusu, verilere erişemediği konusunda şikayetçi olur.
  >
  >

## <a name="advanced-topics-partitioned-table-and-store-hive-data-in-orc-format"></a><a name="partition-orc"></a>Gelişmiş Konular: bölümlenmiş tablo ve Hive verilerini ORC biçiminde depola
Veriler büyükse, tablonun yalnızca birkaç bölümünü taraması gereken sorgular için tablo bölümlenmesi faydalıdır. Örneğin, bir Web sitesinin günlük verilerinin tarihlere göre bölümlenmesi mantıklıdır.

Hive tablolarının bölümlenmesi ek olarak, Hive verilerinin en Iyileştirilmiş satır sütunlu (ORC) biçiminde depolanması da faydalıdır. ORC biçimlendirmesi hakkında daha fazla bilgi için, bkz. <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">orc dosyaları kullanma, Hive verileri okurken, yazarken ve işlerken performansı geliştirir</a>.

### <a name="partitioned-table"></a>Bölümlenmiş tablo
Bölümlenmiş bir tablo oluşturan ve içine veri yükleyen Hive sorgusu aşağıda verilmiştir.

```hiveql
CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
(field1 string,
...
fieldN string
)
PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
    lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
    PARTITION (<partitionfieldname>=<partitionfieldvalue>);
```

Bölümlenmiş tabloları sorgularken, Bölüm koşulunun, **beginning** `where` arama verimliliğini artıran yan tümcesinin başına eklenmesi önerilir.

```hiveql
select
    field1, field2, ..., fieldN
from <database name>.<partitioned table name>
where <partitionfieldname>=<partitionfieldvalue> and ...;
```

### <a name="store-hive-data-in-orc-format"></a><a name="orc"></a>Hive verilerini ORC biçiminde depolayın
Blob depolamadan, ORC biçiminde depolanan Hive tablolarına doğrudan veri yükleyemezsiniz. Azure Bloblarından verileri ORC biçiminde depolanan Hive tablolarına yüklemek için gerçekleştirmeniz gereken adımlar aşağıda verilmiştir.

**TEXTFILE olarak depolanan** bir dış tablo oluşturun ve BLOB depolamadan tabloya veri yükleyin.

```hiveql
CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
(
    field1 string,
    field2 int,
    ...
    fieldN date
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
    lines terminated by '<line separator>' STORED AS TEXTFILE
    LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;
```

1. adımda, aynı alan sınırlayıcısı ile aynı şemaya sahip bir iç tablo oluşturun ve Hive verilerini ORC biçiminde depolayın.

```hiveql
CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
(
    field1 string,
    field2 int,
    ...
    fieldN date
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;
```

1. adımdaki dış tablodaki verileri seçin ve ORC tablosuna ekleyin

```hiveql
INSERT OVERWRITE TABLE <database name>.<ORC table name>
    SELECT * FROM <database name>.<external textfile table name>;
```

> [!NOTE]
> TEXTFILE tablosu *\<database name\> . \<external textfile table name\>* bölüm içerir, adım 3 ' te, `SELECT * FROM <database name>.<external textfile table name>` komut bölüm değişkenini döndürülen veri kümesindeki bir alan olarak seçer. İçine ekleniyor *\<database name\> . \<ORC table name\>* bu yana başarısız oluyor *\<database name\> . \<ORC table name\>* , tablo şemasında bir alan olarak bölüm değişkenine sahip değildir. Bu durumda, eklenecek alanları özel olarak seçmeniz gerekir *\<database name\> . \<ORC table name\>* şöyle:
>
>

```hiveql
INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
    SELECT field1, field2, ..., fieldN
    FROM <database name>.<external textfile table name>
    WHERE <partition variable>=<partition value>;
```

*\<external text file table name\>* Tüm veriler öğesine eklendikten sonra aşağıdaki sorgu kullanılırken öğesini bırakmak güvenlidir *\<database name\> . \<ORC table name\>*:

```hiveql
    DROP TABLE IF EXISTS <database name>.<external textfile table name>;
```

Bu yordamı tamamladıktan sonra, ORC biçimindeki verileri kullanmaya hazırlamış bir tablonuz olmalıdır.  
