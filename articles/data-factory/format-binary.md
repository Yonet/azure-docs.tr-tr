---
title: Azure Data Factory ikili biçimi
description: Bu konu başlığı altında, Azure Data Factory Ikili biçimi nasıl ele alınacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/29/2020
ms.author: jingwang
ms.openlocfilehash: be4338942057cf52bf26b7fd1e15f77e724d49f8
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96348917"
---
# <a name="binary-format-in-azure-data-factory"></a>Azure Data Factory ikili biçimi

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

İkili biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

[Copy etkinliğinde](copy-activity-overview.md), [GetMetadata etkinliğinde](control-flow-get-metadata-activity.md)veya [Delete etkinliğinde](delete-activity.md)ikili veri kümesini kullanabilirsiniz. ADF, Ikili veri kümesi kullanırken dosya içeriğini ayrıştırmaz ancak olduğu gibi kabul etmez. 

>[!NOTE]
>Copy etkinliğinde binary veri kümesini kullanırken, yalnızca Ikili veri kümesinden Ikili veri kümesine kopyalayabilirsiniz.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Ikili veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| tür             | Veri kümesinin Type özelliği **binary** olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında kendi konum türü ve desteklenen özellikleri vardır `location` . **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Yes      |
| sıkıştırma | Dosya sıkıştırmayı yapılandırmak için özellik grubu. Etkinlik yürütmesi sırasında sıkıştırma/açma işlemi yapmak istediğinizde bu bölümü yapılandırın. | Hayır |
| tür | İkili dosyaları okumak/yazmak için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler şunlardır **bzip2**, **gzip**, **söndür**, **zipsöndür**, **tar** veya **targzip**. <br>Örneğin, dosya tabanlı havuz veri deposuna **açılan** dosyaları açmak için kopyalama **etkinliği kullanılırken,** / **TarGzip** / **Tar** varsayılan dosyalar klasörüne çıkarılır: `<path specified in dataset>/<folder named as source compressed file>/` , `preserveZipFileNameAsFolder` / `preserveCompressionFileNameAsFolder` sıkıştırılmış dosyaların adının klasör yapısı olarak korunup korunmayacağını denetlemek için [etkinlik kaynağını kopyalama](#binary-as-source) üzerinde kullanın.| Hayır       |
| düzey | Sıkıştırma oranı. Kopyalama etkinliği havuzunda veri kümesi kullanıldığında Uygula.<br>İzin verilen değerler **en iyi** veya **en hızlardır**.<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmasa bile, sıkıştırma işleminin mümkün olduğunca hızlı bir şekilde tamamlanmalıdır.<br>- **En iyi**: işlemin tamamlanmasını daha uzun sürse bile sıkıştırma işlemi en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için bkz. [sıkıştırma düzeyi](/dotnet/api/system.io.compression.compressionlevel) konusu. | Hayır       |

Aşağıda, Azure Blob depolamada bir Ikili veri kümesi örneği verilmiştir:

```json
{
    "name": "BinaryDataset",
    "properties": {
        "type": "Binary",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "ZipDeflate"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Ikili kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

>[!NOTE]
>Copy etkinliğinde binary veri kümesini kullanırken, yalnızca Ikili veri kümesinden Ikili veri kümesine kopyalayabilirsiniz.

### <a name="binary-as-source"></a>Kaynak olarak ikili

Aşağıdaki özellikler kopyalama etkinliği **_ \_ kaynağı \**** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **Binarysource** olarak ayarlanmalıdır. | Yes      |
| formatSettings | Bir özellik grubu. Aşağıdaki **ikili okuma ayarları** tablosuna bakın. | Hayır       |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında kendi desteklenen okuma ayarları vardır `storeSettings` . **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

Altında desteklenen **ikili okuma ayarları** `formatSettings` :

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | FormatSettings türünün **Binaryreadsettings** olarak ayarlanması gerekir. | Yes      |
| compressionProperties | Belirli bir sıkıştırma codec bileşeni için verileri açmak üzere bir özellik grubu. | Hayır       |
| preserveZipFileNameAsFolder<br>(*`compressionProperties` -> `type` as `ZipDeflateReadSettings`*) | Giriş veri kümesi **Zipsöndür** sıkıştırma ile yapılandırıldığında geçerlidir. Kaynak ZIP dosya adının kopyalama sırasında klasör yapısı olarak korunup korunmayacağını gösterir.<br>- **True (varsayılan)** olarak ayarlandığında Data Factory daraltılmış dosyaları içine yazar `<path specified in dataset>/<folder named as source zip file>/` .<br>- **False** olarak ayarlandığında Data Factory ZIP dosyalarını doğrudan öğesine yazar `<path specified in dataset>` . Yarış veya beklenmedik davranışlara engel olmak için farklı kaynak ZIP dosyalarında yinelenen dosya adlarında bulunmadığından emin olun.  | Hayır |
| preserveCompressionFileNameAsFolder<br>(*`compressionProperties` -> `type` `TarGZipReadSettings` veya `TarReadSettings` olarak*) | Giriş veri kümesi **targık** sıkıştırması ile yapılandırıldığında geçerlidir / **Tar** . Kaynak sıkıştırılmış dosya adının kopyalama sırasında klasör yapısı olarak korunup korunmayacağını gösterir.<br>- **True (varsayılan)** olarak ayarlandığında Data Factory, açılan dosyaları içine yazar `<path specified in dataset>/<folder named as source compressed file>/` . <br>- **False** olarak ayarlandığında Data Factory açılan dosyaları doğrudan öğesine yazar `<path specified in dataset>` . Yarış veya beklenmedik davranışlara engel olmak için farklı kaynak dosyalarında yinelenen dosya adlarında bulunmadığından emin olun. | Hayır |

```json
"activities": [
    {
        "name": "CopyFromBinary",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "BinarySource",
                "storeSettings": {
                    "type": "AzureBlobStorageReadSettings",
                    "recursive": true,
                    "deleteFilesAfterCompletion": true
                },
                "formatSettings": {
                    "type": "BinaryReadSettings",
                    "compressionProperties": {
                        "type": "ZipDeflateReadSettings",
                        "preserveZipFileNameAsFolder": false
                    }
                }
            },
            ...
        }
        ...
    }
]
```

### <a name="binary-as-sink"></a>Havuz olarak ikili

Aşağıdaki özellikler kopyalama etkinliği **_ \_ Havuz \**** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **Binarysink** olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında kendi desteklenen yazma ayarları vardır `storeSettings` . **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)