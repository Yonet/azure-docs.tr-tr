---
title: Azure Data Factory kullanarak verileri ve ODBC veri depolarından veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak verileri ve ODBC veri depolarından kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: jingwang
ms.openlocfilehash: c92428666f0766f78475be16416027cdc6e71f20
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85506540"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri ve ODBC veri depolarından veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-odbc-connector.md)
> * [Güncel sürüm](connector-odbc.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri bir ODBC veri deposuna ve ' den kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu ODBC Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

ODBC kaynağından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan ODBC havuzuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu ODBC Bağlayıcısı **temel** veya **anonim** kimlik doğrulaması kullanarak **ODBC ile uyumlu tüm veri depolarından** veri kopyalamayı destekler. **64 bitlik BIR ODBC sürücüsü** gereklidir. ODBC havuzu için ADF, ODBC sürüm 2,0 standardını destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu ODBC bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir Integration Runtime ayarlayın. Ayrıntılar için bkz. [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesi.
- Integration Runtime makinesindeki veri deposu için 64 bitlik ODBC sürücüsünü yükler.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, ODBC bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

ODBC bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği: **ODBC** olarak ayarlanmalıdır | Evet |
| Dizisi | Kimlik bilgisi bölümünü dışlayarak bağlantı dizesi. Bağlantı dizesini gibi bir düzende belirtebilir `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"` veya Integration Runtime makinede ayarladığınız SISTEM DSN 'sini (veri kaynağı adı) kullanabilirsiniz `"DSN=<name of the DSN on IR machine>;"` (yine de bağlı hizmette kimlik bilgisi bölümünü belirtmeniz gerekir).<br>Ayrıca, Azure Key Vault bir parola yerleştirebilir ve  `password`   yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz.Daha ayrıntılı bilgi için [Azure Key Vault 'de mağaza kimlik bilgilerini](store-credentials-in-key-vault.md)inceleyin   .| Evet |
| authenticationType | ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler: **temel** ve **anonim**. | Evet |
| userName | Temel kimlik doğrulaması kullanıyorsanız Kullanıcı adını belirtin. | Hayır |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabı için parola belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Hayır |
| kimlik bilgisi | Sürücüye özgü özellik-değer biçiminde belirtilen bağlantı dizesinin erişim kimlik bilgisi kısmı. Örnek: `"RefreshToken=<secret refresh token>;"`. Bu alanı SecureString olarak işaretleyin. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Evet |

**Örnek 1: temel kimlik doğrulaması kullanma**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: anonim kimlik doğrulaması kullanma**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, ODBC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

ODBC ile uyumlu veri deposundan veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | DataSet 'in Type özelliği: **Odbctable** olarak ayarlanmalıdır | Evet |
| tableName | ODBC veri deposundaki tablonun adı. | Kaynak için Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse);<br/>Havuz için Evet |

**Örnek**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "OdbcTable",
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

`RelationalTable`Türü belirtilmiş veri kümesi kullanıyorsanız, hala olduğu gibi desteklenir, ancak yeni bir adım ileri kullanmanız önerilir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, ODBC kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="odbc-as-source"></a>Kaynak olarak ODBC

ODBC uyumlu veri deposundan verileri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği: **Odbcsource** olarak ayarlanmalıdır | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tableName" belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OdbcSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Yazılan kaynağı kullanıyorsanız, `RelationalSource` hala olduğu gibi desteklenmektedir, ileri ' yi kullanmaya devam etmeniz önerilir.

### <a name="odbc-as-sink"></a>Havuz olarak ODBC

ODBC uyumlu veri deposuna veri kopyalamak için kopyalama etkinliğindeki havuz türünü **Odbcsink**olarak ayarlayın. Aşağıdaki özellikler, kopyalama etkinliği **havuzu** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği havuzunun Type özelliği: **Odbcsink** olarak ayarlanmalıdır | Evet |
| writeBatchTimeout |Toplu ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br/>İzin verilen değerler: TimeSpan. Örnek: "00:30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında verileri SQL tablosuna ekler.<br/>İzin verilen değerler: Integer (satır sayısı). |Hayır (varsayılan 0-otomatik olarak algılanır) |
| Ön Copyscrıpt |Her çalıştırmada veri deposuna veri yazmadan önce yürütülecek kopyalama etkinliği için bir SQL sorgusu belirtin. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. |Hayır |

> [!NOTE]
> "WriteBatchSize" ayarlanmamışsa (otomatik algılanan), kopyalama etkinliği önce sürücünün Batch işlemlerini destekleyip desteklemediğini algılar ve bunu yoksa 10000 olarak ayarlayın veya yoksa 1 olarak ayarlayın. 0 dışında bir değer ayarlarsanız, kopyalama etkinliği değeri eşit kılar ve sürücü Batch işlemlerini desteklemiyorsa çalışma zamanında başarısız olur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bağlantı sorunlarını gidermek için **Integration Runtime Configuration Manager** **Tanılama** sekmesini kullanın.

1. **Integration Runtime Configuration Manager**başlatın.
2. **Tanılama** sekmesine geçin.
3. "Bağlantıyı Sına" bölümünde, veri deposu **türünü** (bağlı hizmet) seçin.
4. Veri deposuna bağlanmak için kullanılan **bağlantı dizesini** belirtin, **kimlik doğrulamasını** seçin ve **Kullanıcı adı**, **parola**ve/veya **kimlik bilgilerini**girin.
5. Veri deposuyla bağlantıyı test etmek için **Bağlantıyı Sına** ' ya tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
