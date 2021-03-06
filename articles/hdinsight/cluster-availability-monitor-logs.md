---
title: HDInsight 'ta Azure Izleyici günlükleri ile küme kullanılabilirliğini izleme
description: Küme durumunu ve kullanılabilirliğini izlemek için Azure Izleyici günlüklerini nasıl kullanacağınızı öğrenin.
ms.service: hdinsight
ms.topic: how-to
ms.date: 08/12/2020
ms.openlocfilehash: d52cb1c5f3b1dd1b23adb39f2f65d0e66968e482
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98946960"
---
# <a name="how-to-monitor-cluster-availability-with-azure-monitor-logs-in-hdinsight"></a>HDInsight 'ta Azure Izleyici günlükleri ile küme kullanılabilirliğini izleme

HDInsight kümeleri, sorgulanabilir ölçümler ve Günlükler ve yapılandırılabilir uyarılar sağlayan Azure Izleyici günlüğü tümleştirmesini içerir. Bu makalede, Azure Izleyici 'nin kümenizi izlemek için nasıl kullanılacağı gösterilmektedir.

## <a name="azure-monitor-logs-integration"></a>Azure Izleyici günlük tümleştirmesi

Azure Izleyici günlükleri, HDInsight kümeleri gibi birden çok kaynak tarafından oluşturulan verilerin, birleştirilmiş bir izleme deneyimi elde etmek için tek bir yerde toplanmasını ve toplanmasını sağlar.

Bir önkoşul olarak, toplanan verileri depolamak için bir Log Analytics çalışma alanına ihtiyacınız olacaktır. Henüz bir tane oluşturmadıysanız, buradaki yönergeleri izleyebilirsiniz: [Log Analytics çalışma alanı oluşturun](../azure-monitor/learn/quick-create-workspace.md).

## <a name="enable-hdinsight-azure-monitor-logs-integration"></a>HDInsight Azure Izleyici günlükleri tümleştirmesini etkinleştirme

Portaldaki HDInsight küme kaynağı sayfasından **Azure İzleyicisi**' ni seçin. Ardından, **Etkinleştir** ' i seçin ve açılan listeden Log Analytics çalışma alanınızı seçin.

![HDInsight Operations Management Suite](media/cluster-availability-monitor-logs/azure-portal-monitoring.png)

Varsayılan olarak, bu, Edge düğümleri hariç tüm küme düğümlerine OMS aracısını yüklerse. Küme Edge düğümlerinde yüklü bir OMS Aracısı olmadığından, varsayılan olarak Log Analytics Edge düğümlerinde bulunan hiçbir telemetri yoktur.

## <a name="query-metrics-and-logs-tables"></a>Sorgu ölçümleri ve günlük tabloları

Azure Izleyici günlük tümleştirmesi etkinleştirildikten sonra (Bu işlem birkaç dakika sürebilir) **Log Analytics çalışma alanı** kaynağına gidin ve **Günlükler**' i seçin.

![Log Analytics çalışma alanı günlükleri](media/cluster-availability-monitor-logs/hdinsight-portal-logs.png)

Günlükler bir dizi örnek sorgu listeler, örneğin:

| Sorgu adı                      | Description                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Günümüzde kullanılabilirlik bilgisayarları    | Günlük gönderen bilgisayarların sayısını, her saat                     |
| Sinyalleri Listele                 | Son saatin tüm bilgisayar sinyalleriyle listeleme                           |
| Her bilgisayarın son sinyali | Her bilgisayar tarafından gönderilen son sinyali gösterme                             |
| Kullanılamayan bilgisayarlar           | Son 5 saat içinde sinyal göndermediği bilinen tüm bilgisayarları Listele |
| Kullanılabilirlik oranı               | Bağlı her bilgisayarın kullanılabilirlik oranını hesapla                |

Örnek olarak, yukarıdaki ekran görüntüsünde gösterildiği gibi bu sorguda **Çalıştır** ' ı seçerek **kullanılabilirlik oranı** örnek sorgusunu çalıştırın. Bu, kümenizde her bir düğümün kullanılabilirlik oranını yüzde olarak gösterir. Aynı Log Analytics çalışma alanına ölçümleri göndermek için birden çok HDInsight kümesi etkinleştirdiyseniz, bu kümelerdeki tüm düğümlerin (kenar düğümleri hariç) kullanılabilirlik oranını görürsünüz.

![Log Analytics çalışma alanı günlüklerinin kullanılabilirlik oranı ' örnek sorgu](media/cluster-availability-monitor-logs/portal-availability-rate.png)

> [!NOTE]  
> Kullanılabilirlik oranı, 24 saatlik bir dönemde ölçülür, bu sayede doğru kullanılabilirlik ücretleri görüntülenmeden önce kümenizin en az 24 saat boyunca çalışması gerekir.

Sağ üst köşedeki **sabitle** ' ye tıklayarak bu tabloyu paylaşılan bir panoya sabitleyebilirsiniz. Yazılabilir bir paylaşılan panonuz yoksa, nasıl bir tane oluşturabileceğiniz hakkında bilgi edinebilirsiniz: [Azure Portal panoları oluşturma ve paylaşma](../azure-portal/azure-portal-dashboards.md#publish-and-share-a-dashboard).

## <a name="azure-monitor-alerts"></a>Azure Izleyici uyarıları

Ayrıca, bir ölçüm değeri veya bir sorgu sonuçlarının belirli koşullara uyması durumunda tetiklenecek Azure Izleyici uyarılarını da ayarlayabilirsiniz. Örnek olarak, bir veya daha fazla düğüm 5 saat içinde bir sinyal göndermediği zaman bir e-posta göndermek için bir uyarı oluşturalım (yani, kullanılamaz olarak kabul edilir).

**Günlüklerde**, aşağıda gösterildiği gibi, bu sorguda **Çalıştır** ' ı seçerek **kullanılamayan bilgisayarlar** örnek sorgusunu çalıştırın.

![Log Analytics çalışma alanı günlükleri ' kullanılamayan bilgisayarlar ' örneği](media/cluster-availability-monitor-logs/portal-unavailable-computers.png)

Tüm düğümler varsa, bu sorgu şimdilik sıfır sonuç döndürmelidir. Bu sorgu için uyarınızı yapılandırmaya başlamak üzere **Yeni uyarı kuralı** ' na tıklayın.

![Log Analytics çalışma alanı yeni uyarı kuralı](media/cluster-availability-monitor-logs/portal-logs-new-alert-rule.png)

Bir uyarının üç bileşeni vardır: kuralın oluşturulacağı *kaynak* (bu durumda Log Analytics çalışma alanı), uyarının tetiklendiği *koşul* ve uyarı tetiklendiğinde ne olacağını belirleyen *eylem grupları* .
Sinyal mantığını yapılandırmayı tamamlaması için aşağıda gösterildiği gibi **koşul başlığına** tıklayın.

![Portal uyarısı kural oluşturma koşulu](media/cluster-availability-monitor-logs/portal-condition-title.png)

Bu işlem, **sinyal mantığını Yapılandır**' ını açar.

**Uyarı mantığı** bölümünü aşağıdaki şekilde ayarlayın:

*Temel alan: sonuç sayısı, koşul: büyüktür, eşik: 0.*

Bu sorgu yalnızca sonuç olarak kullanılamayan düğümleri döndürdüğünden, sonuç sayısı 0 ' dan büyük olursa uyarının tetiklenmesi gerekir.

**Değerlendirilen temelinde** bölümünde, kullanılamayan düğümleri ne sıklıkta denetlemek istediğinize göre **dönemi** ve **sıklığı** ayarlayın.

Bu uyarının amacı için **period = Frequency** olduğundan emin olmak istiyorsunuz. Period, sıklık ve diğer uyarı parametreleri hakkında daha fazla bilgi [burada](../azure-monitor/platform/alerts-unified-log.md#alert-logic-definition)bulunabilir.

Sinyal mantığını yapılandırmayı bitirdiğinizde **bitti** ' yi seçin.

![Uyarı kuralı, sinyal mantığını yapılandırır](media/cluster-availability-monitor-logs/portal-configure-signal-logic.png)

Zaten mevcut bir eylem grubunuz yoksa, **eylem grupları** bölümünde **Yeni oluştur** ' a tıklayın.

![Uyarı kuralı yeni eylem grubu oluşturuyor](media/cluster-availability-monitor-logs/portal-create-new-action-group.png)

Bu işlem, **eylem grubu Ekle**' ye açılır. Bir **eylem grubu adı**, **kısa ad**, **abonelik** ve **kaynak grubu seçin.** **Eylemler** bölümünde, **eylem adı** ' nı seçin ve **eylem türü** olarak **e-posta/SMS/Push/ses'** i seçin.

> [!NOTE]
> Bir uyarının bir Azure Işlevi, LogicApp, Web kancası, ıTSM ve Otomasyon Runbook 'u gibi bir e-posta/SMS/Push/sesden farklı olarak tetikleyebileceği birkaç başka eylem vardır. [Daha fazla bilgi edinin.](../azure-monitor/platform/action-groups.md#action-specific-information)

Bu, **e-posta/SMS/Push/seslendirmeyi** açar. Alıcı için bir **ad** seçin, **e-posta** kutusunu **işaretleyin** ve uyarının gönderilmesini istediğiniz e-posta adresini yazın. Eylem grubunuzu yapılandırmayı tamamlaymak için **e-posta/SMS/Push/sesde** **Tamam** ' ı ve ardından **eylem grubu Ekle** ' yi seçin.

![Uyarı kuralı ekleme eylem grubu oluşturur](media/cluster-availability-monitor-logs/portal-add-action-group.png)

Bu dikey pencereler kapatıldıktan sonra eylem **grupları** bölümünün altında listelenmiş eylem grubunuzu görmeniz gerekir. Son olarak, uyarı **ayrıntıları** bölümünü bir **Uyarı kuralı adı** ve **açıklaması** yazıp bir **önem derecesi** seçerek doldurun. Bitiş için **Uyarı kuralı oluştur** ' a tıklayın.

![Portal Uyarı kuralı sonu oluşturuyor](media/cluster-availability-monitor-logs/portal-create-alert-rule-finish.png)

> [!TIP]
> **Önem derecesi** belirtme özelliği, birden çok uyarı oluştururken kullanılabilecek güçlü bir araçtır. Örneğin, tek bir baş düğüm aşağı gittiğinde bir uyarı oluşturmak için bir uyarı (sev 1) ve her iki baş düğümün da önemli olmayan olayda kritik (sev 0) oluşturan başka bir uyarı oluşturabilirsiniz.

Bu uyarının koşulu karşılandığında, uyarı harekete geçeceğiz ve uyarı ayrıntılarına şu şekilde bir e-posta alacaksınız:

![Azure Izleyici uyarı e-postası örneği](media/cluster-availability-monitor-logs/portal-oms-alert-email.png)

Ayrıca, **Log Analytics çalışma alanınızdaki** **uyarılara** giderek, önem derecesine göre gruplandırılan tüm uyarıları da görüntüleyebilirsiniz.

![Log Analytics çalışma alanı uyarıları](media/cluster-availability-monitor-logs/hdi-portal-oms-alerts.png)

Önem düzeyi gruplandırmada (yukarıda vurgulanan gibi **sev 1** ) seçilmesi, bu önem derecesindeki tüm uyarıların kayıtlarını aşağıda gösterildiği gibi gösterir:

![Log Analytics çalışma alanı önem derecesi bir uyarı](media/cluster-availability-monitor-logs/portal-oms-alerts-sev1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Küme kullanılabilirliği - Apache Ambari](./hdinsight-cluster-availability.md)
* [Azure Izleyici günlüklerini kullanma](hdinsight-hadoop-oms-log-analytics-tutorial.md)
