---
title: 'Öğretici: adanmış SQL havuzlarıyla verileri çözümlemeye başlama'
description: Bu öğreticide, SQL havuzunun analitik yeteneklerini araştırmak için NYC TAXI örnek verilerini kullanacaksınız.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 683da659dcfa07c0a105382f4cc93d1f4dfb21b5
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98219547"
---
# <a name="analyze-data-with-dedicated-sql-pools"></a>Adanmış SQL havuzları ile verileri analiz etme

Azure SYNAPSE Analytics, özel bir SQL havuzu ile verileri analiz etme yeteneği sağlar. Bu öğreticide, özel bir SQL havuzunun yeteneklerini araştırmak için NYC TAXI verilerini kullanacaksınız.

## <a name="load-the-nyc-taxi-data-into-sqlpool1"></a>NYC TAXI verilerini SQLPOOL1 'e yükleme

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin, **+** Yeni kaynak eklemek için düğmeye tıklayın ve ardından yeni SQL betiği oluşturun.
1. Betiğin üzerindeki ' Bağlan ' açılan listesinde ' SQLPOOL1 ' havuzunu (Bu öğreticinin [1. adımında](./get-started-create-workspace.md) oluşturulan havuz) seçin.
1. Aşağıdaki kodu girin:
    ```
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );

    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/QID6392_20171107_05910_0.txt.gz'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```
1. Betiği yürütmek için Çalıştır düğmesine tıklayın.
1. Bu betik 60 saniyeden daha az bir süre içinde sona acaktır. NYC TAXI verilerinin 2.000.000 satırlarını dbo adlı bir tabloya yükler **. Seyahat**.

## <a name="explore-the-nyc-taxi-data-in-the-dedicated-sql-pool"></a>Özel SQL havuzundaki NYC TAXI verilerini keşfet

1. SYNAPSE Studio 'da **veri** merkezine gidin.
1. **SQLPOOL1**  >  **Tables** bölümüne gidin. 
1. Dbo öğesine sağ tıklayın **. Seyahat** tablosu ve **Yeni SQL betiği** Seç  >  **ilk 100 satır seçin**.
1. Yeni bir SQL betiği oluşturulup çalışırken bekleyin.
1. SQL **komut dosyasının en üstünde,** **SQLPOOL1** adlı SQL havuzuna otomatik olarak ayarlandığını unutmayın.
1. SQL komut dosyasının metnini bu kodla değiştirin ve çalıştırın.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount;
    ```

    Bu sorgu, toplam seyahat mesafeleri ve ortalama seyahat mesafesinin, pascların sayısıyla ilişkisini gösterir.
1. SQL komut dosyası sonucu penceresinde, sonuçların bir çizgi grafik olarak görselliğini görmek için **görünümü** **grafik** olarak değiştirin.
    
    > [!NOTE]
    > Çalışma alanı etkinleştirilmiş adanmış bir SQL Havuzu (eski adıyla SQL DW) veri merkezindeki araç ipucu aracılığıyla tanımlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Spark kullanarak çözümleme](get-started-analyze-spark.md)