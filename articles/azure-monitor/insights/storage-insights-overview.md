---
title: Depolama için Azure Izleyici ile Azure depolama hizmetlerini izleme | Microsoft Docs
description: Bu makalede, Azure depolama hesaplarıyla ilgili performans ve kullanım sorunlarını hızlı bir şekilde anlamak için depolama yöneticileri sağlayan depolama için Azure Izleyici özelliği açıklanır.
ms.subservice: ''
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 05/11/2020
ms.openlocfilehash: 47799898251923b327954c764dcccb8157ba5119
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98208650"
---
# <a name="monitoring-your-storage-service-with-azure-monitor-for-storage"></a>Depolama hizmetinizi depolama için Azure Izleyici ile izleme

Depolama için Azure Izleyici, Azure depolama hizmetleri Performanslarınızın, kapasitesinin ve kullanılabilirliğinden oluşan Birleşik bir görünüm sunarak Azure depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sağlar. Depolama kapasitesini ve performansı iki şekilde gözlemleyebilirsiniz, depolama hesabı gruplarına bakmak için doğrudan Azure Izleyici 'den bir depolama hesabından veya görünümünden görüntüleyebilirsiniz. 

Bu makale, depolama hesaplarının, etkin noktalara odaklanıp gecikme süresi, azaltma ve kullanılabilirlik sorunlarını tanılayıp, depolama hesaplarının sistem durumu ve performansı hakkında uygun bir şekilde işlem yapılabilir bilgi türetmeye yönelik Azure Izleyici deneyimini anlamanıza yardımcı olur.

## <a name="introduction-to-azure-monitor-for-storage"></a>Depolama için Azure Izleyici 'ye giriş

Deneyime girmeden önce, bilgilerin nasıl sunulduklarını ve görselleştirir. Depolama özelliğini doğrudan bir depolama hesabından veya Azure Izleyici 'den seçip depolama için Azure Izleyici, tutarlı bir deneyim sunar. 

Birleşik BT, şunları sağlar:

* Depolama hizmetinin veya API işleminin sistem durumuna göre kullanılabilirliğinden bir anlık görüntü görünümünü gösteren **Ölçek perspektifinde** , depolama hizmetinin aldığı toplam istek sayısını gösteren kullanım ve depolama hizmeti ya da API işlem türünün istekleri işleme aldığı ortalama süreyi gösteren gecikme süresi. Ayrıca, kapasiteyi blob, dosya, tablo ve kuyruğa göre de görüntüleyebilirsiniz.

* Sorunları tanılamaya veya kategori kullanılabilirliği, performans, hatalara ve kapasiteye göre ayrıntılı analiz gerçekleştirmeye yardımcı olması için belirli bir depolama hesabının **detayına** bakın. Bu seçeneklerden herhangi birini seçmek, ölçümlerin derinlemesine bir görünümünü sağlar.  

* Hangi ölçümleri görmek istediğinizi değiştirmek, sınırlarınız ile hizalı olan eşikleri değiştirmek veya ayarlamak ve kendi çalışma kitabınız olarak kaydetmek için **özelleştirilebilir** . Çalışma kitabındaki grafikler Azure panosuna sabitlenebilir.  

Bu özellik, herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez, depolama hesaplarınızdaki depolama ölçümleri varsayılan olarak toplanır. Azure depolama 'da kullanılabilen ölçümler hakkında bilginiz varsa, Azure depolama [ölçümlerini](../../storage/blobs/monitor-blob-storage.md)Inceleyerek Azure depolama ölçümlerinde açıklama ve tanımı görüntüleyin.

>[!NOTE]
>Bu özelliğe erişmek için ücret alınmaz ve [Azure izleyici fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/) sayfasında açıklandığı gibi, yalnızca yapılandırdığınız veya etkinleştirdiğiniz Azure izleyici temel özellikleri için ücretlendirilirsiniz.


## <a name="view-from-azure-monitor"></a>Azure Izleyici 'den görüntüle

Azure Izleyici 'den, aboneliğinizdeki birden çok depolama hesabından işlem, gecikme süresi ve kapasite ayrıntılarını görüntüleyebilir, performansı, kapasite sorunlarını ve hataları belirlemenize yardımcı olabilirsiniz.

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve kullanılabilirliğini görüntülemek için aşağıdaki adımları gerçekleştirin.

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Azure portal sol bölmeden **izleyici** ' yi seçin ve **Öngörüler** bölümünde **depolama hesapları**' nı seçin.

    ![Birden çok depolama hesabı görünümü](./media/storage-insights-overview/multiple-storage-accounts-view-01.png)

### <a name="overview-workbook"></a>Genel Bakış çalışma kitabı

Seçili abonelik için **genel bakış** çalışma kitabında tablo, abonelik içinde gruplanmış 5 adede kadar depolama hesabına yönelik etkileşimli depolama ölçümlerini ve hizmet kullanılabilirliği durumunu görüntüler. Aşağıdaki açılan listelerden seçtiğiniz seçeneklere göre sonuçlara filtre uygulayabilirsiniz:

* **Abonelikler** -yalnızca depolama hesaplarına sahip abonelikler listelenir.  

* **Depolama hesapları** -varsayılan olarak 5 depolama hesabı önceden seçilmiştir. Kapsam seçicideki tüm veya birden çok depolama hesabı seçerseniz, en fazla 200 depolama hesabı döndürülür. Örneğin, üç abonelik üzerinde seçtiğiniz toplam 573 depolama hesabınız varsa, yalnızca 200 hesapları görüntülenir. 

* **Zaman aralığı** -varsayılan olarak, yapılan ilgili seçimlere göre son 4 saati bilgileri görüntüler.

Açılan listelerin altındaki sayaç kutucuğu, abonelikteki toplam depolama hesabı sayısını kaydeder ve toplamın kaç tane seçili olduğunu yansıtır. Çalışma kitabındaki, işlem ölçümlerini veya hataları rapor eden sütunlar için koşullu renk kodlaması veya heavmaps vardır. En büyük renk en yüksek değere sahiptir ve en düşük değere göre daha açık bir renge sahiptir. Hata tabanlı sütunlarda değer kırmızı, ölçüm tabanlı sütunlar için ise değer mavi renkte olur.

**Kullanılabilirlik**, **e2e gecikme süresi**, **sunucu gecikmesi** ve **işlem hata türü/hata** sütunlarında bir değer seçin, sizi bu depolama hesabı için seçilen sütunla eşleşen belirli depolama ölçümleri türüne uyarlanmış bir rapora yönlendirir. Her bir kategorinin çalışma kitapları hakkında daha fazla bilgi için aşağıdaki [ayrıntılı depolama çalışma kitapları](#detailed-storage-workbooks) bölümüne bakın. 

>[!NOTE]
>Raporda hangi hataların gösterilebileceği hakkında ayrıntılı bilgi için bkz. [yanıt türü şeması](../../storage/blobs/monitor-blob-storage-reference.md#metrics-dimensions) ve **serverothererror**, **clienentothererror**, **clientkısıtlanror** gibi yanıt türlerini arayın. Seçili depolama hesaplarına bağlı olarak, üçten fazla üç tür hata bildirildiğinde, diğer tüm hatalar **diğeri** kategorisi altında gösterilir.

Varsayılan **kullanılabilirlik** eşiği:

* Uyarı-%99
* Kritik-%90

Bir kullanılabilirlik eşiğini gözlemlerinizin veya gereksinimlerin sonuçlarına göre ayarlamak için, [kullanılabilirlik eşiğini Değiştir](#modify-the-availability-threshold)' i gözden geçirin. 

### <a name="capacity-workbook"></a>Kapasite çalışma kitabı

Sayfanın üst kısmındaki **kapasiteyi** seçin ve **Kapasite** çalışma kitabı açılır. Bu, hesap içinde her bir veri hizmeti tarafından kullanılan ve daha fazla depolama alanı kullanımını belirlemenize yardımcı olmak için kullanılan hesapta ve kapasitede kullanılan toplam depolama miktarını gösterir.

![Birden çok depolama hesabı kapasite çalışma kitabı](./media/storage-insights-overview/storage-account-capacity-02.png) 

Çalışma kitabındaki sütunlar için, bir mavi değerle kapasite ölçümlerini rapor eden koşullu renk kodlaması veya ısı haritalarını vardır. En büyük renk en yüksek değere sahiptir ve en düşük değere göre daha açık bir renge sahiptir.

Çalışma kitabındaki sütunlardan herhangi biri altında bir değer seçtiğinizde, depolama hesabı için **Kapasite** çalışma kitabına ayrıntıya gidebilirsiniz. Ayrıntıya gitme raporu hakkında daha fazla ayrıntı aşağıdaki [ayrıntılı depolama çalışma kitapları](#detailed-storage-workbooks) bölümünde açıklanmaktadır. 

## <a name="view-from-a-storage-account"></a>Bir depolama hesabından görüntüle

Bir depolama hesabından doğrudan VM'ler için Azure İzleyici erişmek için:

1. Azure portal depolama hesapları ' nı seçin.

2. Listeden bir depolama hesabı seçin. Izleme bölümünde Öngörüler ' i seçin.

    ![Depolama hesabı için genel bakış çalışma kitabı sayfasını gösteren ekran görüntüsü.](./media/storage-insights-overview/storage-account-direct-overview-01.png)

Depolama hesabı için **genel bakış** çalışma kitabında, hızlı bir şekilde değerlendirmenize yardımcı olan çeşitli depolama performans ölçümleri gösterilmektedir:

* Denetim dışında bir sorunun, **dağıtım sütununun dağıtıldığı** bölgedeki depolama hizmetini etkileyip etkilemediğini anında görmek için depolama hizmetinin sistem durumu.

* Depolama kapasitesi, kullanılabilirlik, işlemler ve gecikme süresi ile ilgili en önemli ayrıntıları gösteren etkileşimli performans grafikleri.  

* Ölçüm ve durum kutucukları, hizmet kullanılabilirliğini, depolama hizmetindeki toplam işlem sayısını, E2E gecikmesini ve sunucu gecikmesini vurgular.

**Hatalara**, **performansa**, **kullanılabilirliğine** ve **kapasiteye** yönelik düğmelerden birini seçmek ilgili çalışma kitabını açar. 

![Seçili depolama hesabına genel bakış sayfası](./media/storage-insights-overview/storage-account-capacity-01.png)

## <a name="detailed-storage-workbooks"></a>Ayrıntılı depolama çalışma kitapları

Birden fazla depolama hesabına **genel bakış** çalışma kitabındaki **kullanılabilirlik**, **e2e gecikme süresi**, **sunucu gecikmesi** ve **işlem hata türü/hata** sütunlarında bir değer seçtiğinizden veya belirli bir depolama hesabındaki **genel bakış** çalışma kitabından **hatalara**, **performansa**, **kullanılabilirliğe** ve **kapasiteye** yönelik düğmelerden birini seçerek, her biri bu kategoriye özel bir dizi etkileşimli depolama ile ilgili bilgi sağlar.  

* **Kullanılabilirlik** , **kullanılabilirlik** çalışma kitabını açar. Depolama hesabında tanımlanan, seçili zaman aralığını temsil eden bir eğilim çizgisi ve hesaptaki her bir veri hizmeti için bir kullanılabilirlik eğilimi grafiği içeren her nesnenin kullanılabilir sistem durumunu gösteren bir tablo olan Azure depolama hizmeti 'nin geçerli sistem durumunu gösterir.  

    ![Kullanılabilirlik raporu örneği](./media/storage-insights-overview/storage-account-availability-01.png)

* **E2e gecikme süresi** ve **sunucu gecikmesi** **performans** çalışma kitabını açar. Bu, E2E gecikme süresi ve sunucu gecikmesini gösteren bir toplama durumu kutucuğu, E2E ve sunucu gecikmesi performans grafiği ve depolama hesabında tanımlanan veri hizmetine göre sınıflandırılan API 'nin başarılı çağrılarındaki gecikme süresini ortadan kaldırarak bir tablo içerir.

    ![Performans raporu örneği](./media/storage-insights-overview/storage-account-performance-01.png)

* Kılavuzda listelenen herhangi bir hata kategorisinden birini seçerek **hata** çalışma kitabını açın. Raporda, bir yandan ve başarılı istekler, istemci kısıtlama hataları, Clienentothererror özniteliğine özel işlem **yanıt türü** boyut ölçümü için bir performans grafiği ve **Yanıt türüne göre** **API adına** ve işlemlere göre işlemler, diğer tüm istemci tarafı hatalarının ölçüm kutucukları gösterilmektedir.

   ![Hata raporu örneği](./media/storage-insights-overview/storage-account-failures-01.png)

* **Kapasite** , **Kapasite** çalışma kitabını açar. Bu, bölmelerde ve grafikteki hesaptaki her bir depolama veri nesnesi için kullanılan toplam depolama miktarını ve hesapta kaç veri nesnesinin depolandığını gösterir.  

    ![Seçili depolama hesabı kapasitesi sayfası](./media/storage-insights-overview/storage-account-capacity-01.png) 

## <a name="pin-and-export"></a>PIN ve dışarı aktarma

Bölümün sağ üst köşesindeki raptiye simgesini seçerek ölçüm bölümlerinin herhangi birini bir Azure panosuna sabitleyebilirsiniz.

![Ölçüm bölümü panoya sabitle örneği](./media/storage-insights-overview/workbook-pin-example.png)

Çoklu abonelik ve depolama hesabı **genel bakış** veya **Kapasite** çalışma kitapları, raptiye simgesinin sağ tarafındaki aşağı ok simgesini seçerek Excel biçimindeki sonuçları dışarı aktarmayı destekler.

![Çalışma kitabı kılavuz sonuçlarını dışarı aktarma örneği](./media/storage-insights-overview/workbook-export-example.png)

## <a name="customize-azure-monitor-for-storage"></a>Depolama için Azure Izleyicisini özelleştirme

Bu bölümde, veri analizi ihtiyaçlarınızı desteklemeye yönelik olarak özelleştirmek üzere çalışma kitabını düzenlemeyle ilgili yaygın senaryolar vurgulanmıştır:

* Çalışma kitabını her zaman belirli bir abonelik veya depolama hesabı seçmek üzere kapsama
* Kılavuzdaki ölçümleri değiştirme
* Kullanılabilirlik eşiğini değiştirme
* Renk işlemeyi değiştirme

Özelleştirmeler, yayımlanan çalışma kitabımızda varsayılan yapılandırmanın üzerine yazılmasını engellemek için özel bir çalışma kitabına kaydedilir. Çalışma kitapları, sizin için özel **Raporlarım** bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği **paylaşılan raporlar** bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kaydettikten sonra başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Çalışma kitabı galerisini komut çubuğundan Başlat](./media/storage-insights-overview/workbook-command-bar-gallery.png)

### <a name="specifying-a-subscription-or-storage-account"></a>Abonelik veya depolama hesabı belirtme

Çoklu abonelik ve depolama hesabı **genel bakış** veya **Kapasite** çalışma kitaplarını her çalıştırmada belirli bir abonelik veya depolama hesabı için kapsama göre yapılandırabilir ve aşağıdaki adımları gerçekleştirin.

1. Portalda **izleyici** ' yi seçin ve ardından sol bölmedeki **depolama hesapları** ' nı seçin.

2. **Genel bakış** çalışma kitabında, komut çubuğundan **Düzenle**' yi seçin.

3. **Abonelikler** açılan listesinden, varsayılan olarak kullanmak istediğiniz bir veya daha fazla aboneliği seçin. Çalışma kitabının toplam 10 aboneliği seçmeyi desteklediğini unutmayın.  

4. **Depolama hesapları** açılır listesinden, varsayılan olarak istediğiniz bir veya daha fazla hesabı seçin. Çalışma kitabının toplam 200 depolama hesabı seçmeyi desteklediğini unutmayın. 

5. Özelleştirmelerinizle çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **farklı kaydet** ' i seçin ve ardından okuma moduna dönmek Için **Düzenle bitti** ' ye tıklayın.  

### <a name="modify-metrics-and-colors-in-the-workbook"></a>Çalışma kitabındaki ölçümleri ve renkleri değiştirme

Önceden oluşturulmuş çalışma kitapları ölçüm verileri içerir ve görselleştirmelerin herhangi birini değiştirme veya kaldırma ve takımınızın özel ihtiyaçlarına göre özelleştirme imkanına sahip olursunuz.

Bizim örneğimizde, nasıl yapılacağını göstermek için çoklu abonelik ve depolama hesabı kapasitesi çalışma kitabıyla birlikte çalışıyoruz:

* Ölçüyü kaldırma
* Renk işlemeyi Değiştir

Önceden oluşturulmuş **hatalardan**, **performans**, **kullanılabilirlik** ve **Kapasite** çalışma kitaplarının herhangi birine karşı aynı değişiklikleri gerçekleştirebilirsiniz.

1. Portalda **izleyici** ' yi seçin ve ardından sol bölmedeki **depolama hesapları** ' nı seçin.

2. Kapasite çalışma kitabına geçiş yapmak için **kapasiteyi** seçin ve komut çubuğundan komut çubuğundan **Düzenle** ' yi seçin.

    ![Çalışma kitabını değiştirmek için Düzenle ' yi seçin](./media/storage-insights-overview/workbook-edit-workbook.png)

3. Ölçümler bölümünün yanındaki **Düzenle**' yi seçin.

    ![Kapasite çalışma kitabı ölçümlerini değiştirmek için Düzenle ' yi seçin](./media/storage-insights-overview/edit-metrics-capacity-workbook-01.png)

4. **Kullanılan kapasite zaman çizelgesi** sütununu kaldıracağız, bu nedenle ölçümler kılavuzunda **sütun ayarları** ' nı seçin.

    ![Sütun ayarlarını Düzenle](./media/storage-insights-overview/edit-capacity-workbook-resource-grid.png)

5. **Sütun ayarlarını Düzenle** bölmesinde,  **Microsoft. Storage/Storageaccounts-Capacity-usedcapacity zaman çizelgesi $ | sütunları bölümünde öğesini seçin. Hesap kapasite zaman çizelgesi $** ve açılan liste **sütunu Işleyicisinin** altında **gizli** öğesini seçin.

6. Değişiklerinizi uygulamak için **Kaydet ve Kapat ' ı** seçin.

Şimdi, rapordaki kapasite ölçümlerinin renk temasını, mavi yerine yeşil kullanmak üzere değiştirelim.

1. Ölçüm kılavuzunda **sütun ayarları** ' nı seçin.

2. **Sütun ayarlarını Düzenle** bölmesinde, Microsoft **sütunları** bölümünde ' ı seçin **. depolama/Storageaccounts-kapasite-usedcapacity $ `|` Microsoft. Storage/Storageaccounts/Blobservices-Capacity-blobcapacity $ `|` Microsoft. Storage/Storageaccounts/fileservices-Capacity-filecapacity $ `|` Microsoft. Storage/Storageaccounts/queueservices-Capacity-queuecapacity $ `|` Microsoft. Storage/Storageaccounts/tableservices-Capacity-tablecapacity $**. Aşağı açılan liste **renk paleti** altında **yeşil**' i seçin.

3. Değişiklerinizi uygulamak için **Kaydet ve Kapat ' ı** seçin.

4. Özelleştirmelerinizle çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **farklı kaydet** ' i seçin ve ardından okuma moduna dönmek Için **Düzenle bitti** ' ye tıklayın.  

### <a name="modify-the-availability-threshold"></a>Kullanılabilirlik eşiğini değiştirme

Bu örnekte, depolama hesabı kapasite çalışma kitabıyla birlikte çalışıyoruz ve kullanılabilirlik eşiğini nasıl değiştireceğiniz gösterilir. Varsayılan olarak, kutucuk ve ızgara raporlama yüzdesi kullanılabilirliği, minimum 90 eşiğini ve en fazla 99 eşiğini kullanarak yapılandırılır. En düşük eşik değerini, **API adı ızgarası tarafından kullanılabilirlik** açısından **%85** olarak değiştirebiliyoruz. bu, eşiğin yüzde 85 ' den küçük olması durumunda sağlık durumunun kritik olarak değiştiği anlamına gelir. 

1. Portaldan **depolama hesapları** ' nı seçin ve ardından listeden bir depolama hesabı seçin.

2. Sol bölmeden **Öngörüler** ' i seçin.

3. Çalışma kitabında kullanılabilirlik çalışma kitabına geçiş yapmak için **kullanılabilirlik** ' i seçin ve ardından komut çubuğundan **Düzenle** ' yi seçin. 

4. Sayfanın en altına aşağı kaydırın ve **API kılavuzunun kullanılabilirliği** ' nin yanındaki sol tarafta bulunan **Düzenle**' yi seçin.

    ![API adına göre kullanılabilirliği düzenleme kılavuz ayarları](./media/storage-insights-overview/availability-workbook-avail-by-apiname.png)

5. **Sütun ayarları** ' nı seçin ve ardından **sütun ayarlarını Düzenle** bölmesinde, **sütunlar** bölümünde **kullanılabilirlik (%) seçeneğini belirleyin. (Eşikler + biçimlendirilen)**.

6. **Kritik** sistem durumu için değeri **90** ' den **85** ' e değiştirin ve ardından **Kaydet ve Kapat**' a tıklayın.

    ![Kritik durum için kullanılabilirlik eşik değerini değiştirme](./media/storage-insights-overview/edit-column-settings-capacity-workbook-01.png)

7. Özelleştirmelerinizle çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **farklı kaydet** ' i seçin ve ardından okuma moduna dönmek Için **Düzenle bitti** ' ye tıklayın.

## <a name="troubleshooting"></a>Sorun giderme

Genel sorun giderme kılavuzu için, adanmış çalışma kitabı tabanlı Öngörüler [sorun giderme makalesine](troubleshoot-workbooks.md)bakın.

Bu bölüm, depolama için Azure Izleyicisi 'ni kullanırken karşılaşabileceğiniz bazı yaygın sorunların tanılanması ve sorun gidermede size yardımcı olur. Belirli sorununuzla ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

### <a name="resolving-performance-capacity-or-availability-issues"></a>Performans, kapasite veya kullanılabilirlik sorunlarını çözme

Depolama için Azure Izleyici ile belirttiğiniz depolamayla ilgili sorunları gidermenize yardımcı olması için bkz. Azure Storage [sorun giderme kılavuzu](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance).  

### <a name="why-can-i-only-see-200-storage-accounts"></a>Neden 200 depolama hesabı görmem yeterlidir?

Seçilen depolama hesaplarının sayısı, seçilen aboneliklerin sayısından bağımsız olarak 200 sınırına sahiptir.

### <a name="how-to-change-the-coloring-and-threshold-for-availability"></a>Kullanılabilirlik için renklendirme ve eşik nasıl değiştirilir?

Kullanılabilirliği ve eşikleri kullanılabilirlik için değiştirme hakkında ayrıntılı adımlar için [kullanılabilirlik eşiğini değiştirme](#modify-the-availability-threshold) bölümüne bakın.

### <a name="how-to-analyze-and-troubleshoot-the-data-shown-in-azure-monitor-for-storage"></a>Depolama için Azure Izleyici 'de gösterilen verileri analiz etme ve giderme

 Depolama için Azure Izleyici 'de gösterilen Azure depolama verilerini çözümleme ve sorun giderme hakkında bilgi için [bkz. izleyici, tanılama ve sorun giderme Microsoft Azure depolama](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md) makalesi.

### <a name="why-dont-i-see-all-the-types-of-errors-in-metrics"></a>Ölçümlerde neden tüm hata türlerini görmüyorum?

Şu anda en fazla üç farklı hata türü gösteriliyor ve hataların geri kalanı tek bir demet içinde birlikte gruplandırılır. SplitByLimit kullanılarak denetlenir ve değiştirilebilir. Bu özelliği değiştirmek için:

1. Çalışma kitabını Düzenle ' ye tıklayın.
2. Ölçümler ' e gidin, Düzenle ' ye tıklayın ve ardından **işlemler, toplam** veya düzenlemek istediğiniz ölçümler ' i seçin.

    ![Ölçümler 'e gidin ve "Işlemler, toplamlar" üzerinde Düzenle ' ye tıklayın.](./media/storage-insights-overview/fqa7.png)

3. Ardından bölme sayısını değiştirin.

    ![Ölçüm parametrelerini seçin "](./media/storage-insights-overview/fqa7-2.png)

Hatalı hata türlerini n + 1 olarak, geriye kalan hatalar için de n + 1 olarak belirtin.

###  <a name="i-saved-my-workbook-while-on-some-storage-account-why-cant-i-find-it-now"></a>Çalışma kitabımı bazı depolama hesabı sırasında kaydettik. Neden şimdi bulamıyorum?

Her çalışma kitabı, bu dosyayı kaydettiğiniz depolama hesabına kaydedilir. Kullanıcının çalışma kitabını kaydettiği belirli bir depolama hesabını bulmayı deneyin. Aksi takdirde, kaynağı (depolama hesabı) bilmeden belirli bir çalışma kitabını bulmanın bir yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

* Sorunları algılamaya yardımcı olmak üzere otomatik uyarı ayarlamak için [ölçüm uyarılarını](../platform/alerts-metric.md) ve [hizmet durumu bildirimlerini](../../service-health/alerts-activity-log-service-notifications-portal.md) yapılandırın.

* Çalışma kitaplarının desteklemek için tasarlandıkları senaryoları, mevcut raporların yeni nasıl yazılacağını ve özelleştirildiğini ve [Azure izleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](../platform/workbooks-overview.md)konusunu gözden geçirin.

* Azure depolama ile ilgili sorunları tanımlamak, tanılamak ve sorunlarını gidermek için Depolama Analizi ve diğer araçları kullanma hakkında ayrıntılı bir kılavuz için bkz. [izleme, tanılama ve sorun giderme Microsoft Azure depolama](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md).
