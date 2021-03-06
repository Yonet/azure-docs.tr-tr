---
title: Azure geçiş 'de Hyper-V değerlendirmesi desteği
description: Azure geçişi sunucu değerlendirmesi ile Hyper-V değerlendirmesi desteği hakkında bilgi edinin
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 06/14/2020
ms.openlocfilehash: 5b5c85b599f02cedc3bb1bda84c28ef2169c8e2d
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96754054"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Hyper-V değerlendirmesi için destek matrisi

Bu makalede, Azure geçişi [: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak Hyper-V VM 'lerini Azure 'a geçiş için değerlendirdikten sonra Önkoşullar ve destek gereksinimleri özetlenmektedir. Hyper-V VM 'lerini Azure 'a geçirmek istiyorsanız, [geçiş desteği matrisini](migrate-support-matrix-hyper-v-migration.md)gözden geçirin.

Hyper-V VM değerlendirmesi ayarlamak için bir Azure geçişi projesi oluşturun ve sunucu değerlendirme aracını projeye ekleyin. Araç eklendikten sonra [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve makine meta verilerini ve performans verilerini Azure 'a gönderir. Bulma işlemi tamamlandıktan sonra, bulunan makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri** | Tek bir [Azure geçişi projesinde](migrate-support-matrix.md#azure-migrate-projects)35.000 adede kadar Hyper-V VM 'yi bulabilir ve değerlendirebilirsiniz.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Hyper-V VM 'lerine ek olarak, bir proje VMware VM 'Leri ve fiziksel sunucuları, her biri için değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure geçişi gereci en fazla 5000 Hyper-V VM 'yi bulabilir.<br/><br/> Gereç, 300 adede kadar Hyper-V konaklarına bağlanabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Bir grup için tek bir değerlendirmede en fazla 35.000 sanal makineyi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .



## <a name="hyper-v-host-requirements"></a>Hyper-V konağı gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Hyper-V konağı**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir.<br/><br/> Hyper-V konağı Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2 çalıştırabilir. Bu işletim sistemlerinin sunucu çekirdeği yüklemesi de desteklenir. <br/>Windows Server 2012 çalıştıran Hyper-V konaklarında yer alan VM'leri değerlendiremezsiniz.
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. <br/> Yönetici izinleri atamak istemiyorsanız, bir yerel veya etki alanı kullanıcı hesabı oluşturun ve bu gruplara kullanıcı hesabını ekleyin-uzak yönetim kullanıcıları, Hyper-V yöneticileri ve performans Izleyicisi kullanıcıları. |
| **PowerShell uzaktan iletişim**   | Her Hyper-V konağında [PowerShell uzaktan iletişim](/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7) özelliğinin etkinleştirilmesi gerekir. |
| **Hyper-V Çoğaltma**       | Hyper-V çoğaltması kullanıyorsanız (veya aynı VM tanımlayıcılarına sahip birden fazla sanal makine varsa) ve Azure geçişi 'ni kullanarak hem özgün hem de çoğaltılan VM 'Leri keşfetiyorsanız, Azure geçişi tarafından oluşturulan değerlendirme doğru olmayabilir. |


## <a name="vm-requirements"></a>VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Geçiş için tüm işletim sistemleri değerlendirilenebilir.  |
| **Tümleştirme Hizmetleri**       | [Hyper-V tümleştirme hizmetlerinin](/virtualization/hyper-v-on-windows/reference/integration-services) , işletim sistemi bilgilerini yakalamak için değerlendirmekte olduğunuz VM 'lerde çalışıyor olması gerekir. |
| **Depolama** | Yerel disk, DAS, JBOD, depolama alanları, CSV, SMB. VHD/VHDX 'in depolandığı bu Hyper-V konak depolaması desteklenir. <br/> IDE ve SCSI sanal denetleyicileri destekleniyor| 

## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Gereci, portaldan indirtiğiniz sıkıştırılmış bir Hyper-V VHD veya bir [PowerShell betiği](deploy-appliance-script.md)kullanarak dağıtabilirsiniz.

- Hyper-V için [gereç gereksinimleri](migrate-appliance.md#appliance---hyper-v) hakkında bilgi edinin.
- Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken URL 'ler hakkında bilgi edinin.
- Azure Kamu 'da, [betiği kullanarak](deploy-appliance-script-government.md)gereci dağıtmanız gerekir.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**Cihaz** | **Bağlantı**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/><br/> 44368 numaralı bağlantı noktası üzerinden gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ``` https://<appliance-ip-or-name>:44368 ```<br/><br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 (HTTPS) bağlantı noktalarında giden bağlantılar.
**Hyper-V konağı/kümesi** | Genel Bilgi Modeli (CıM) oturumu kullanarak Hyper-V VM 'Leri için meta verileri ve performans verilerini çekmek üzere WinRM bağlantı noktası 5985 (HTTP) veya 5986 (HTTPS) üzerinde gelen bağlantı.

## <a name="agent-based-dependency-analysis-requirements"></a>Aracı tabanlı bağımlılık Analizi gereksinimleri

[Bağımlılık Analizi](concepts-dependency-visualization.md) , değerlendirmek ve Azure 'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo, aracı tabanlı bağımlılık analizini ayarlamaya yönelik gereksinimleri özetler. Hyper-V Şu anda yalnızca aracı tabanlı bağımlılık görselleştirmesini destekler. 

**Gereksinim** | **Ayrıntılar** 
--- | --- 
**Dağıtımdan önce** | Sunucu değerlendirme aracı projeye eklenerek bir Azure geçişi projesi olması gerekir.<br/><br/>  Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız<br/><br/> İlk kez bir proje oluşturmayı [öğrenin](create-manage-projects.md) .<br/> Mevcut bir projeye değerlendirme aracı eklemeyi [öğrenin](how-to-assess.md) .<br/> [Hyper-V VM](how-to-set-up-appliance-hyper-v.md)'lerinin değerlendirmesi Için Azure geçişi gerecini ayarlamayı öğrenin.
**Azure Devlet Kurumları** | Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.
**Log Analytics** | Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](../azure-monitor/log-query/log-query-overview.md) [hizmet eşlemesi](../azure-monitor/insights/service-map.md) çözümünü kullanır.<br/><br/> Yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesiyle ilişkilendirirsiniz. Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez. <br/><br/> Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde bulunmalıdır. Diğer bölgelerdeki çalışma alanları bir projeyle ilişkilendirilemez.<br/><br/> Çalışma alanının [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions)bir bölgede olması gerekir.<br/><br/> Log Analytics, Azure geçişi ile ilişkili çalışma alanı, geçiş projesi anahtarıyla ve proje adıyla etiketlenir.
**Gerekli aracılar** | Çözümlemek istediğiniz her makinede aşağıdaki aracıları yükleyebilirsiniz:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md).<br/> [Bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Şirket içi makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidi indirip yüklemeniz gerekir.<br/><br/> [Bağımlılık aracısını](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) yükleme ve [MMA](how-to-create-group-machine-dependencies.md#install-the-mma)hakkında daha fazla bilgi edinin.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure geçişi Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma alanının [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions)bir bölgede olması gerekir.<br/><br/> Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez.
**Maliyetler** | Hizmet Eşlemesi çözümü ilk 180 gün boyunca ücret almaz (Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirdiğinizden itibaren)/<br/><br/> 180 günden sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Eşlemesi dışında herhangi bir çözümün kullanılması Log Analytics için [Standart ücretler](https://azure.microsoft.com/pricing/details/log-analytics/) doğurur.<br/><br/> Azure geçişi projesi silindiğinde, çalışma alanı onunla birlikte silinmez. Projeyi sildikten sonra Hizmet Eşlemesi kullanımı ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir/<br/><br/>Azure genel kullanım (GA-28 Şubat 2018) geçirmeden önce oluşturduğunuz projeleriniz varsa, ek Hizmet Eşlemesi ücretleri tahakkuk etmeyebilirsiniz. Yalnızca 180 günden sonra ödemeyi sağlamak için, GA 'nin mevcut çalışma alanları Ücretlendirilebilir olmaya devam ettiğinden yeni bir proje oluşturmanızı öneririz.
**Yönetim** | Aracıları çalışma alanına kaydettiğinizde, Azure geçişi projesi tarafından sunulan KIMLIĞI ve anahtarı kullanırsınız.<br/><br/> Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz.<br/><br/> İlişkili Azure geçişi projesini silerseniz, çalışma alanı otomatik olarak silinmez. [El Ile silin](../azure-monitor/platform/manage-access.md).<br/><br/> Azure geçişi projesini silmediğiniz takdirde Azure geçişi tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.
**İnternet bağlantısı** | Makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidini yüklemeniz gerekir.
**Azure Devlet Kurumları** | Aracı tabanlı bağımlılık Analizi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V VM değerlendirmesi için hazırlanma](./tutorial-discover-hyper-v.md)