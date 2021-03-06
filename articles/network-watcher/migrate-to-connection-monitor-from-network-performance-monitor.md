---
title: Ağ Performansı İzleyicisi bağlantı Izleyicisi 'ne geçir
titleSuffix: Azure Network Watcher
description: Ağ Performansı İzleyicisi 'den bağlantı Izleyicisine geçiş yapmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: 0bb46c17ece9a38d9f1e10c79a4b026efa0ece4c
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99833805"
---
# <a name="migrate-to-connection-monitor-from-network-performance-monitor"></a>Ağ Performansı İzleyicisi bağlantı Izleyicisi 'ne geçir

> [!IMPORTANT]
> 1 Temmuz 2021 ' den itibaren, mevcut bir çalışma alanına yeni testler ekleyemez veya Ağ Performansı İzleyicisi yeni bir çalışma alanı etkinleştiremeyeceksiniz. 1 Temmuz 2021 ' den önce oluşturulan testleri kullanmaya devam edebilirsiniz. Geçerli iş yüklerinizde hizmet kesintisini en aza indirmek için, testlerinizi Ağ Performansı İzleyicisi 'den Azure ağ Izleyicisi 'ndeki yeni bağlantı Izleyicisinden, 29 Şubat 2024 tarihinden önce geçirin.

Ağ Performansı İzleyicisi (NPM) ' den yeni, iyileştirilmiş bağlantı Izleyicisine, tek tıklamayla ve sıfır kapalı kalma süresine sahip testleri geçirebilirsiniz. Avantajlar hakkında daha fazla bilgi edinmek için bkz. [Bağlantı İzleyicisi](./connection-monitor-overview.md).


## <a name="key-points-to-note"></a>Önemli noktaları göz önünde

Geçiş, aşağıdaki sonuçları üretmenize yardımcı olur:

* Şirket içi aracılar ve güvenlik duvarı ayarları olduğu gibi çalışır. Değişiklik gerekmiyor. Azure sanal makinelerinde yüklü Log Analytics aracıların ağ Izleyicisi uzantısıyla değiştirilmeleri gerekir.
* Mevcut testler, > test grubu > test biçimiyle bağlantı Izleyicisine eşlenir. **Düzenle**' yi seçerek yeni bağlantı izleyicisinin özelliklerini görüntüleyip değiştirebilir, değişiklikler yapmak için bir şablon indirebilir ve Azure Resource Manager aracılığıyla şablonu gönderebilirsiniz.
* Aracılar Log Analytics çalışma alanına ve ölçümlere veri gönderir.
* Veri izleme:
   * **Log Analytics verileri**: geçişten önce, veriler NPM 'Nin networkmonitoring tablosunda yapılandırıldığı çalışma alanında kalır. Geçişten sonra, veriler NetworkMonitoring tablosuna ve aynı çalışma alanındaki ConnectionMonitor_CL tablosuna gider. Test NPM 'de devre dışı bırakıldıktan sonra, veriler yalnızca ConnectionMonitor_CL tablosunda depolanır.
   * **Günlük tabanlı uyarılar, panolar ve tümleştirmeler**: yeni ConnectionMonitor_CL tablosuna göre sorguları el ile düzenlemeniz gerekir. Uyarıları ölçümlerde yeniden oluşturmak için, bkz. [Bağlantı İzleyicisi Ile ağ bağlantısı izleme](./connection-monitor-overview.md#metrics-in-azure-monitor).
    
## <a name="prerequisites"></a>Önkoşullar

* Aboneliğinizde ve Log Analytics çalışma alanının bölgesinde ağ izleyicisinin etkinleştirildiğinden emin olun.
* Log Analytics aracıları yüklü Azure sanal makineleri, ağ Izleyicisi uzantısıyla etkinleştirilmelidir.

## <a name="migrate-the-tests"></a>Testleri geçirme

Testleri Ağ Performansı İzleyicisi bağlantı Izleyicisine geçirmek için aşağıdakileri yapın:

1. Ağ Izleyicisi 'nde **Bağlantı İzleyicisi**' ni seçin ve ardından **NPM 'den testleri geçir** sekmesini seçin. 

    :::image type="content" source="./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png" alt-text="Ağ Performansı İzleyicisi testleri bağlantı Izleyicisine geçirme" lightbox="./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png":::
    
1. Aşağı açılan listelerde, aboneliğinizi ve çalışma alanınızı seçin ve ardından geçirmek istediğiniz NPM özelliğini seçin. 
1. Testleri geçirmek için **Içeri aktar** ' ı seçin.

Geçiş başladıktan sonra aşağıdaki değişiklikler gerçekleşir: 
* Yeni bir bağlantı İzleyicisi kaynağı oluşturulur.
   * Her bölge ve abonelik için bir bağlantı İzleyicisi oluşturulur. Şirket içi aracılarla testler için yeni bağlantı izleyici adı olarak biçimlendirilir `<workspaceName>_"on-premises"` . Azure aracılarıyla testler için yeni bağlantı izleyici adı olarak biçimlendirilir `<workspaceName>_<Azure_region_name>` .
   * İzleme verileri artık NPM 'nin etkinleştirildiği Log Analytics çalışma alanında saklanır, Connectionmonitor_CL adlı yeni bir tabloda. 
   * Test adı, test grubu adı olarak ileri taşınır. Test açıklaması geçirilmez.
   * Kaynak ve hedef uç noktaları yeni test grubunda oluşturulur ve kullanılır. Şirket içi aracılar için uç noktalar olarak biçimlendirilir `<workspaceName>_"endpoint"_<FQDN of on-premises machine>` . Azure için, geçiş testleri çalıştırmayan aracıları içeriyorsa, aracıları etkinleştirmeniz ve yeniden geçirmeniz gerekir.
   * Hedef bağlantı noktası ve yoklama aralığı, *TC_ \<testname>* ve *TC_ \<testname> _AppThresholds* adlı bir test yapılandırmasına taşınır. Protokol, bağlantı noktası değerlerine göre ayarlanır. Başarı eşikleri ve diğer isteğe bağlı özellikler boş bırakılır.
* NPM devre dışı bırakılmazsa, geçirilen testler NetworkMonitoring ve ConnectionMonitor_CL tablolarına veri gönderilmeye devam edebilir. Bu yaklaşım, mevcut günlük tabanlı uyarıların ve tümleştirmelerin etkilenmemesini sağlar.
* Yeni oluşturulan Bağlantı İzleyicisi, bağlantı Izleyicisi 'nde görünür.

Geçişten sonra şunları yaptığınızdan emin olun:
* NPM 'deki testleri el ile devre dışı bırakın. Bunu yapana kadar ücretlendirilmeye devam edersiniz. 
* NPM 'yi devre dışı bırakırken, ConnectionMonitor_CL tablosunda uyarılarınızı yeniden oluşturun veya ölçümleri kullanın. 
* Tüm dış tümleştirmeleri ConnectionMonitor_CL tablosuna geçirin. Dış tümleştirmelere örnek olarak Power BI ve Grafana ' deki panolar ve güvenlik bilgileri ve olay yönetimi (SıEM) sistemleriyle tümleştirmeler verilebilir.


## <a name="next-steps"></a>Sonraki adımlar

Bağlantı Izleyicisi hakkında daha fazla bilgi edinmek için bkz.:
* [Bağlantı Izleyicisi 'nden (klasik) bağlantı Izleyicisi 'ne geçiş](./migrate-to-connection-monitor-from-connection-monitor-classic.md)
* [Azure portal kullanarak bağlantı Izleyicisi oluşturma](./connection-monitor-create-using-portal.md)
