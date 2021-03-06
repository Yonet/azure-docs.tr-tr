---
title: Öğretici-Azure Stream Analytics işlerinde Azure Işlevleri çalıştırma
description: Bu öğreticide, Stream Analytics işlerine bir çıktı havuzu olarak Azure İşlevlerini yapılandırma hakkında bilgi edineceksiniz.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: tutorial
ms.custom: mvc, devx-track-csharp
ms.date: 01/27/2020
ms.openlocfilehash: 74e09e61a6132858d716686bdb6687bb670f0d33
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879519"
---
# <a name="tutorial-run-azure-functions-from-azure-stream-analytics-jobs"></a>Öğretici: Azure Stream Analytics işlerden Azure Işlevleri çalıştırma 

İşlevleri Stream Analytics işinin çıktı havuzlarından biri olarak yapılandırarak, Azure Stream Analytics'ten Azure İşlevleri’ni çalıştırabilirsiniz. İşlevler, Azure veya üçüncü taraf hizmetlerinde gerçekleşen olayların tetiklediği kodu uygulamanıza olanak tanıyan olay odaklı, isteğe bağlı bir işlem deneyimidir. İşlevlerin tetikleyicilere yanıt vermeye yönelik bu özelliği, hizmeti Stream Analytics işleri için doğal bir çıktı haline getirmektedir.

Stream Analytics, HTTP tetikleyicileri aracılığıyla İşlevleri çağırır. İşlevlerin çıkış bağdaştırıcısı, kullanıcıların Stream Analytics sorgularına göre olayların tetiklenmesini sağlayacak şekilde Stream Analytics’e İşlevleri bağlamasına olanak tanır. 

> [!NOTE]
> Çok kiracılı kümede çalışan bir Stream Analytics işinden bir sanal ağ (VNet) içindeki Azure Işlevlerine bağlantı desteklenmez.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Stream Analytics işi oluşturma ve çalıştırma
> * Redsıs örneği için Azure önbelleği oluşturma
> * Azure İşlevi oluşturma
> * For Redfor sonuçları için Azure önbelleğini denetle

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>İşlev çalıştırmak için bir Stream Analytics işi yapılandırma 

Bu bölümde, Redsıs için Azure önbelleğine veri yazan bir işlevi çalıştırmak üzere bir Stream Analytics işinin nasıl yapılandırılacağı gösterilmektedir. Stream Analytics işi Azure Event Hubs’dan olayları okur ve işlevi çağıran bir sorgu çalıştırır. Bu işlev Stream Analytics işinden verileri okur ve redin için Azure önbelleğine yazar.

![Azure hizmetleri arasındaki ilişkileri gösteren diyagram](./media/stream-analytics-with-azure-functions/image1.png)

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Girdi olarak Event Hubs ile bir Stream Analytics işi oluşturma

Bir olay hub’ı oluşturmak, olay oluşturucu uygulamasını başlamak ve bir Stream Analytics işi oluşturmak için [Gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) öğreticisini takip edin. Sorguyu ve çıktıyı oluşturma adımlarını atlayın. Bunun yerine, bir Azure Işlevleri çıkışı ayarlamak için aşağıdaki bölümlere bakın.

## <a name="create-an-azure-cache-for-redis-instance"></a>Redsıs örneği için Azure önbelleği oluşturma

1. [Önbellek oluşturma](../azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)bölümünde açıklanan adımları kullanarak Reda Için Azure önbelleğinde önbellek oluşturun.  

2. Önbelleği oluşturduktan sonra **Ayarlar** altında **Erişim Anahtarları**’nı seçin. **Birincil bağlantı dizesi**’ni not edin.

   ![Redsıs bağlantı dizesi için Azure önbelleğinin ekran görüntüsü](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-cache-for-redis"></a>Azure Işlevlerinde Redsıs için Azure önbelleğine veri yazabilmesi için bir işlev oluşturma

1. İşlevler belgesinin [İşlev uygulaması oluşturma](../azure-functions/functions-get-started.md) bölümüne bakın. Bu bölümde, CSharp dilini kullanarak Azure Işlevlerinde bir işlev uygulaması ve [http ile tetiklenen bir işlev](../azure-functions/functions-get-started.md)oluşturma işlemi adım adım açıklanmaktadır.  

2. **run.csx** işlevine göz atın. Aşağıdaki kodla güncelleştirin. **" \<your Azure Cache for Redis connection string goes here\> "** Değerini, önceki bölümde aldığınız redo birincil bağlantı dizesi için Azure önbelleği ile değiştirin. 

    ```csharp
    using System;
    using System.Net;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
        // Get the request body
        dynamic dataArray = await req.Content.ReadAsAsync<object>();

        // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

        if (dataArray.ToString().Length > 262144)
        {
            return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
        }
        var connection = ConnectionMultiplexer.Connect("<your Azure Cache for Redis connection string goes here>");
        log.Info($"Connection string.. {connection}");
    
        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = connection.GetDatabase();
        log.Info($"Created database {db}");
    
        log.Info($"Message Count {dataArray.Count}");

        // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
        for (var i = 0; i < dataArray.Count; i++)
        {
            string time = dataArray[i].time;
            string callingnum1 = dataArray[i].callingnum1;
            string key = time + " - " + callingnum1;
            db.StringSet(key, dataArray[i].ToString());
            log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
            // Simple get of data types from the cache
            string value = db.StringGet(key);
            log.Info($"Database got: {value}");
        }

        return req.CreateResponse(HttpStatusCode.OK, "Got");
    }

   ```

   Stream Analytics işlevden "HTTP İstek Varlığı Çok Büyük" özel durumunu aldığında İşlevler’e gönderdiği toplu işlerin boyutunu azaltır. Aşağıdaki kod, Stream Analytics büyük bir toplu iş göndermemesini sağlar. İşlevde kullanılan en büyük toplu iş sayı ve boyut değerlerinin Stream Analytics portalına girilen değerlerle tutarlı olduğundan emin olun.

    ```csharp
    if (dataArray.ToString().Length > 262144)
        {
            return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
        }
   ```

3. Tercih ettiğiniz bir metin düzenleyicisinde **project.json** adlı bir JSON dosyası oluşturun. Aşağıdaki kodu yapıştırın ve yerel bilgisayarınıza kaydedin. Bu dosya, C# işlevinin gerektirdiği NuGet paket bağımlılıklarını içerir.  
   
    ```json
    {
        "frameworks": {
            "net46": {
                "dependencies": {
                    "StackExchange.Redis":"1.1.603",
                    "Newtonsoft.Json": "9.0.1"
                }
            }
        }
    }

   ```
 
4. Azure portalına geri dönün. **Platform özellikleri** sekmesinden işlevinize göz atın. **Geliştirme Araçları** altında **App Service Düzenleyicisi**’ni seçin. 
 
   ![Ekran görüntüsü, App Service Düzenleyicisi seçili olan platform özellikleri sekmesini gösterir.](./media/stream-analytics-with-azure-functions/image3.png)

5. App Service Düzenleyicisi’nde kök dizininize sağ tıklayın ve **project.json** dosyasını karşıya yükleyin. Karşıya yükleme başarılı olduktan sonra sayfayı yenileyin. Şu anda **project.lock.json** adlı otomatik olarak oluşturulmuş bir dosya görmeniz gerekir. Otomatik olarak oluşturulan dosya, project.json dosyasındaki .dll dosyalarının başvurularını içerir.  

   ![Ekran görüntüsü, menüden karşıya yükleme dosyalarını gösterir.](./media/stream-analytics-with-azure-functions/image4.png)

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>Çıktı olarak işlevle Stream Analytics işini güncelleştirme

1. Stream Analytics işinizi Azure portalında açın.  

2. İşlevinize göz atın ve **genel bakış**  >  **çıkışları**  >  **Ekle**' yi seçin. Yeni bir çıktı eklemek için havuz seçeneği olarak **Azure İşlevi**’ni seçin. Işlevler çıkış bağdaştırıcısı aşağıdaki özelliklere sahiptir:  

   |**Özellik adı**|**Açıklama**|
   |---|---|
   |Çıktı diğer adı| İşin sorgusunda çıktıya başvurmak için kullandığınız kolay ad. |
   |İçeri aktarma seçeneği| Geçerli abonelikteki işlevi kullanabilir veya işlev başka bir abonelikte bulunuyorsa ayarları el ile sağlayabilirsiniz. |
   |İşlev Uygulaması| İşlevler uygulamanızın adı. |
   |İşlev| İşlevler uygulamanızdaki işlevin adı (run.csx işlevinizin adı).|
   |En Büyük Toplu İş Boyutu|İşleviniz için bayt cinsinden gönderilen her çıkış toplu işinin maksimum boyutunu ayarlar. Varsayılan olarak, bu değer 262.144 bayta ayarlanır (256 KB).|
   |En Büyük Toplu İş Sayısı|İşleve gönderilen her toplu işteki en büyük olay sayısını belirtir. Varsayılan değer 100’dür. Bu özellik isteğe bağlıdır.|
   |Anahtar|Başka bir abonelikteki işlevi kullanmanıza olanak sağlar. İşlevinize erişmek için anahtar değerini sağlayın. Bu özellik isteğe bağlıdır.|

3. Çıktı diğer adı için bir ad belirtin. Bu öğreticide, **saop1** olarak adlandırılmıştır, ancak dilediğiniz adı kullanabilirsiniz. Diğer ayrıntıları girin.

4. Stream Analytics işinizi açın ve sorguyu aşağıdaki gibi güncelleştirin. Çıkış havuzunu **saop1** olarak belirtmediyseniz, sorguda değiştirmeyi unutmayın.  

   ```sql
    SELECT
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Komut satırında aşağıdaki komutu çalıştırarak telcodatagen.exe uygulamasını başlatın. Komut biçimini kullanır `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]` .  
   
   ```cmd
   telcodatagen.exe 1000 0.2 2
   ```
    
6.  Stream Analytics işini başlatın.

## <a name="check-azure-cache-for-redis-for-results"></a>For Redfor sonuçları için Azure önbelleğini denetle

1. Azure portal gidin ve Redsıs için Azure önbelleğinizi bulun. **Konsol**’u seçin.  

2. Verilerinizi Redsıs için Azure önbelleğinde olduğunu doğrulamak üzere [redsıs komutları Için Azure önbelleğini](https://redis.io/commands) kullanın. (Komut Get {Key} biçimini alır.) Örneğin:

   **Get "19.12.2017 21:32:24 - 123414732"**

   Bu komut, belirtilen anahtarın değerini yazdırmalıdır:

   ![Redsıs çıkışı için Azure önbelleğinin ekran görüntüsü](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="error-handling-and-retries"></a>Hata işleme ve yeniden deneme

Olayları Azure Işlevlerine gönderirken bir hata oluşursa, en fazla işlem Stream Analytics. Http hatası 413 (varlık çok büyük) dışında tüm http özel durumları başarılı olana kadar yeniden denenir. Bir varlık çok büyük hatası, [yeniden deneme veya bırakma ilkesine](stream-analytics-output-error-policy.md)tabi veri hatası olarak değerlendirilir.

> [!NOTE]
> Stream Analytics Azure Işlevlerine yönelik HTTP isteklerinin zaman aşımı 100 saniyeye ayarlanır. Azure Işlevleri uygulamanız bir toplu işi işlemek için 100 saniyeden daha fazla sürerse, hata Stream Analytics ve Batch için rety.

Zaman aşımları için yeniden deneme, çıkış havuzuna yazılan yinelenen olayların oluşmasına neden olabilir. Başarısız bir toplu iş için yeniden deneme Stream Analytics, toplu işlemdeki tüm olayları yeniden dener. Örneğin, Stream Analytics Azure Işlevlerine gönderilen 20 olaydan oluşan toplu işi göz önünde bulundurun. Azure Işlevlerinin bu toplu işlemdeki ilk 10 olayı işlemesi için 100 saniye aldığını varsayalım. 100 saniye geçtikten sonra, Azure Işlevlerinden olumlu bir yanıt almadığından ve aynı toplu iş için başka bir istek gönderildiğinden, Stream Analytics isteği askıya alır. Toplu işlemdeki ilk 10 olay, Azure Işlevleri tarafından yeniden işlenir ve bu da yinelenen bir işlem oluşmasına neden olur. 

## <a name="known-issues"></a>Bilinen sorunlar

Azure portalında En Büyük Toplu İş Boyutu/En Büyük Toplu İş Sayısı değerini boş (varsayılan) olarak sıfırlamaya çalıştığınızda değer kaydedildikten sonra daha önce girilen değerle değiştirilir. Bu durumda bu alanların varsayılan değerlerini el ile girin.

Azure işlevlerinizin [http yönlendirme](/sandbox/functions-recipes/routes?tabs=csharp) kullanımı şu anda Stream Analytics tarafından desteklenmiyor.

Bir sanal ağda barındırılan Azure Işlevlerine bağlanma desteği etkin değil.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın.  
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Işlevi çalıştıran bir basit Stream Analytics işi oluşturdunuz. Stream Analytics işleri hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Stream Analytics işlerinde JavaScript kullanıcı tanımlı işlevleri çalıştırma](stream-analytics-javascript-user-defined-functions.md)
