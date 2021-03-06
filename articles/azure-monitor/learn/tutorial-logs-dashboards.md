---
title: Azure Log Analytics verilerinin panolarını oluşturma ve paylaşma | Microsoft Docs
description: Bu öğretici, Log Analytics Panolarınızın tüm kayıtlı günlük sorgularını görselleştirerek ortamınızı görüntülemek için size tek bir lens sağladığını anlamanıza yardımcı olur.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 05/28/2020
ms.custom: mvc
ms.openlocfilehash: 880d173c6eef9d267e8860ebad805e071706de42
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96186159"
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Log Analytics verilerinin panolarını oluşturma ve paylaşma

Log Analytics panolar, tüm kayıtlı günlük sorgularınızı görselleştirerek kuruluştaki BT işletimsel verilerini bulmanıza, ilişkilendirilebilme ve paylaşmanıza olanak sağlayabilir.  Bu öğretici, BT operasyon destek ekibiniz tarafından erişilecek paylaşılan bir panoyu desteklemek için kullanılacak bir günlük sorgusu oluşturmayı ele almaktadır.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure portalında paylaşılan bir pano oluşturma
> * Performans günlüğü sorgusunu görselleştirin 
> * Paylaşılan bir panoya günlük sorgusu ekleme 
> * Paylaşılan bir panodaki kutucuğu özelleştirme

Bu öğreticideki örneği tamamlamak için [Log Analytics çalışma alanına bağlı](quick-collect-azurevm.md) mevcut bir sanal makinenizin olması gerekir.  
 
## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-shared-dashboard"></a>Paylaşılan pano oluşturma
Varsayılan [panonuzu](../../azure-portal/azure-portal-dashboards.md)açmak için **Pano** ' yı seçin. Panonuz aşağıdaki örnekteki gibi görünecektir.

![Azure portalı panosu](media/tutorial-logs-dashboards/log-analytics-portal-dashboard.png)

Burada tüm Azure kaynaklarınız arasında Azure Log Analytics telemetri verileri gibi BT için en önemli olan işlem verilerini bir araya getirebilirsiniz.  Bir günlük sorgusunu görselleştirmeye başlamadan önce, ilk olarak bir pano oluşturup paylaşalım.  Daha sonra, bir çizgi grafik olarak işlenecek ve panoya ekleyeceğiniz örnek performans günlüğü sorgusuna odaklanabiliyoruz.  

> [!NOTE]
> Aşağıdaki grafik türleri, günlük sorguları kullanılarak Azure panolarında desteklenir:
> - areachart
> - columnChart
> - piechart (panoda halka olarak işlenir)
> - dağınık tergrafik
> - timechart

Bir pano oluşturmak için geçerli pano adının yanındaki **Yeni pano** düğmesini seçin.

![Azure portalında yeni pano oluşturma](media/tutorial-logs-dashboards/log-analytics-create-dashboard-01.png)

Bu işlem yeni, boş ve özel bir pano oluşturur ve panonuzu adlandırıp kutucuklar ekleyebileceğiniz ya da yeniden düzenleyebileceğiniz özelleştirme modunu açar. Panonun adını düzenleyin ve bu öğretici için *örnek panoyu* belirtin ve ardından **özelleştirmeyi bitti**' yi seçin.<br><br> ![Özelleştirilmiş Azure panosunu kaydetme](media/tutorial-logs-dashboards/log-analytics-create-dashboard-02.png)

Bir pano oluşturulduğunda varsayılan olarak gizlidir, yani onu yalnızca siz görebilirsiniz. Panoyu başkalarının görebilmesi için, diğer pano komutlarıyla birlikte görünen **Paylaş** düğmesini kullanın.

![Azure portalında yeni pano paylaşma](media/tutorial-logs-dashboards/log-analytics-share-dashboard.png) 

Panonuzun yayımlanması için bir abonelik ve kaynak grubu seçmeniz istenir. Kolaylık olması için portalın yayımlama deneyimi, bir kaynak grubuna panoları yerleştirdiğiniz **panolar** adlı bir modelde size kılavuzluk eder.  Seçili olan aboneliği doğrulayın ve sonra **Yayımla**’ya tıklayın.  Panoda görünen bilgilere erişim, [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md)ile denetlenir.   

## <a name="visualize-a-log-query"></a>Günlük sorgusu görselleştirin
[Log Analytics](../log-query/log-analytics-tutorial.md) , günlük sorgularıyla ve bunların sonuçlarıyla çalışmak için kullanılan özel bir portaldır. Birden fazla satırda sorgu düzenleme, seçerek kod yürütme, bağlama duyarlı IntelliSense ve Akıllı Analiz özellikleri mevcuttur. Bu öğreticide, grafik biçiminde bir performans görünümü oluşturmak, gelecekteki bir sorgu için kaydetmek ve daha önce oluşturulan paylaşılan panoya sabitlemek için Log Analytics kullanacaksınız.

Azure Izleyici menüsünde **Günlükler** ' i seçerek Log Analytics açın. Yeni bir boş sorgu ile başlar.

![Giriş sayfası](media/tutorial-logs-dashboards/homepage.png)

Hem Windows hem de Linux bilgisayarlarına yönelik işlemci kullanım kayıtlarını, bilgisayar ve TimeGenerated 'e göre gruplanmış ve görsel bir grafikte görüntülenecek şekilde döndürmek için aşağıdaki sorguyu girin. Sorguyu çalıştırmak ve elde edilen grafiği görüntülemek için **Çalıştır** ' a tıklayın.

```Kusto
Perf 
| where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" 
| summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1hr), Computer 
| render timechart
```

Sayfanın üst kısmından **Kaydet** düğmesini seçerek sorguyu kaydedin.

![Sorguyu Kaydet](media/tutorial-logs-dashboards/save-query.png)

**Sorguyu Kaydet** denetim panelinde *Azure VM 'ler Işlemci kullanımı* ve *panolar* gibi bir kategori gibi bir ad girin ve ardından **Kaydet**' e tıklayın.  Bu şekilde, kullanabileceğiniz ve değiştirebileceğiniz bir ortak sorgu kitaplığı oluşturabilirsiniz.  Son olarak, sayfanın sağ üst köşesinden **panoya sabitle** düğmesini seçerek ve ardından Pano adını seçerek bunu daha önce oluşturulan paylaşılan panoya sabitleyin.

Artık panoya sabitlenmiş bir sorgumuz olduğuna göre, genel bir başlığının ve altında bir yorumun olduğunu fark edeceksiniz.

![Azure panosu örneği](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-01.png)

 Bu panoya, görüntüleyenler tarafından kolayca anlaşılacak, anlamlı bir ad vermeliyiz.  Kutucuğun başlığını ve alt başlığını özelleştirmek için Düzenle düğmesine tıklayın ve ardından **Güncelleştir**' e tıklayın.  Değişiklikleri yayımlamanızı veya atmanızı isteyen bir başlık görüntülenir.  **Kopyayı kaydet**' e tıklayın.  

![Örnek pano yapılandırması tamamlandı](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portal bir pano oluşturmayı ve buna bir günlük sorgusu eklemeyi öğrendiniz.  Günlük sorgusu sonuçlarına göre uygulayabileceğiniz farklı yanıtları öğrenmek için sonraki öğreticiye ilerleyin.  

> [!div class="nextstepaction"]
> [Log Analytics uyarılarla olaylara yanıt verme](tutorial-response.md)