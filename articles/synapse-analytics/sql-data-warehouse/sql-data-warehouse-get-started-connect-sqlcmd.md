---
title: Sqlcmd ile bağlanma
description: SYNAPSE SQL havuzuna bağlanmak ve sorgulamak için sqlcmd komut satırı yardımcı programını kullanın.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 3d1d8d3ce3afece5a979aadc27cd82dc7ddaf0d5
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98676246"
---
# <a name="connect-to-sql-pool-in-azure-synapse-analytics-with-sqlcmd"></a>Sqlcmd ile Azure SYNAPSE Analytics 'te SQL havuzuna bağlanma

> [!div class="op_single_selector"]
>
> * [Power BI](/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

Bir SQL havuzuna bağlanmak ve sorgu eklemek için [sqlcmd] [sqlcmd] komut satırı yardımcı programını kullanın.  

## <a name="1-connect"></a>1. Bağlan

[Sqlcmd] [sqlcmd] ile çalışmaya başlamak için, komut istemi ' ni açın ve **sqlcmd** yazıp SQL havuzunuzun bağlantı dizesini girin. Bağlantı dizesi için aşağıdaki parametreler gereklidir:

* **Server (-S):**`<`Sunucu Adı`>`.database.windows.net biçiminde belirtilmiş sunucu
* **Veritabanı (-d):** SQL havuzu adı.
* **Alıntılanmış tanımlayıcıları etkinleştir (-ı):** Bir SQL havuzu örneğine bağlanmak için tırnak işareti tanımlayıcıları etkinleştirilmelidir.

SQL Server Kimlik Doğrulamasını kullanmak için kullanıcı adı/parola parametrelerini eklemeniz gerekir:

* **User (-U):**`<`Kullanıcı`>` biçimindeki sunucu kullanıcısı
* **Password (-P):** Kullanıcıyla ilişkili parola.

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory Tümleşik kimlik doğrulamasını kullanmak için Azure Active Directory parametrelerini eklemeniz gerekir:

* **Azure Active Directory Kimlik Doğrulaması (-G):** Kimlik doğrulaması için Azure Active Directory kullanın

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Active Directory kullanarak kimlik doğrulaması yapmak için [Azure Active Directory Kimlik Doğrulamasını etkinleştirmeniz](sql-data-warehouse-authentication.md) gerekir.

## <a name="2-query"></a>2. sorgu

Bağlantının ardından desteklenen herhangi bir Transact-SQL deyimini örnekte yayımlayabilirsiniz.  Bu örnekte sorgular etkileşimli modda gönderilir.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Bu sonraki örnekler -Q seçeneği kullanarak veya SQL’i sqlcmd öğesine ekleyerek sorgularınızı toplu iş modunda nasıl çalıştırabileceğinizi gösterir.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Sonraki adımlar

Sqlcmd 'de kullanılabilen seçenekler hakkında daha fazla bilgi için bkz. [sqlcmd belgeleri](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true).