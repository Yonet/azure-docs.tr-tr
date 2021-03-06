---
title: API Management için Azure Güvenlik temeli
description: API Management için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 8a572221ca8899c5e4f4cf76e4b89c995952a2f3
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99258566"
---
# <a name="azure-security-baseline-for-api-management"></a>API Management için Azure Güvenlik temeli

API Management için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: Azure API Management, bir Azure sanal ağı (VNet) içinde dağıtılabilir ve bu sayede ağ içindeki arka uç hizmetlerine erişebilir. Geliştirici portalı ve API Management ağ geçidi, Internet 'ten (harici) veya yalnızca VNET 'te (Iç) erişilebilir olacak şekilde yapılandırılabilir.
- Harici: API Management ağ geçidi ve geliştirici portalına bir dış yük dengeleyici aracılığıyla genel İnternet üzerinden erişilebilir. Ağ Geçidi, sanal ağ içindeki kaynaklara erişebilir.
- İç: API Management ağ geçidi ve geliştirici portalına yalnızca iç yük dengeleyici aracılığıyla sanal ağ içinden erişilebilir. Ağ Geçidi, sanal ağ içindeki kaynaklara erişebilir.

API Management dağıtıldığı alt ağa gelen ve giden trafik, ağ güvenlik grubu kullanılarak denetlenebilir.

* [Sanal ağlar ile Azure API Management’ı kullanma](./api-management-using-with-vnet.md)

* [Azure API Management hizmetini iç sanal ağ ile kullanma](./api-management-using-with-internal-vnet.md)

* [API Management'ı Application Gateway ile için sanal ağda tümleştirme](./api-management-howto-integrate-internal-vnet-appgateway.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: API Management dağıtıldığı alt ağa gelen ve giden trafik, ağ güvenlik grupları (NSG 'ler) kullanılarak denetlenebilir. API Management alt ağına bir NSG dağıtın ve NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Dikkat: API Management alt ağında bir NSG yapılandırılırken, açık olması gereken bir bağlantı noktası kümesi vardır. Bu bağlantı noktalarından herhangi biri kullanılamıyorsa API Management düzgün çalışmayabilir ve erişilemez hale gelebilir.

* [Azure API Management için NSG yapılandırmasını anlama](./api-management-using-with-vnet.md#-common-network-configuration-issues)

* [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: kritik Web/HTTP API 'lerini korumak için, iç moddaki bir sanal ağ (VNet) içinde API Management yapılandırır ve bir Azure Application Gateway yapılandırır. Application Gateway bir PaaS hizmetidir. Ters proxy görevi görür ve L7 yük dengelemesi, yönlendirme, Web uygulaması güvenlik duvarı (WAF) ve diğer hizmetleri sağlar.

İç VNET 'te sağlanan API Management Application Gateway ön uç ile birleştirmek aşağıdaki senaryolara izin vermez:
- Tüm API 'Leri iç tüketicilere ve dış tüketicilere sunmak için tek bir API Management kaynağı kullanın.
- Bir API alt kümesini dış tüketicilere göstermek için tek bir API Management kaynağı kullanın.
- Genel Internet 'ten API Management erişimi değiştirme ve kapatma gibi bir yöntem sağlar.

Note: Bu özellik, API Management Premium ve geliştirici katmanlarında kullanılabilir.

* [Application Gateway ile iç VNET 'te API Management tümleştirme](./api-management-howto-integrate-internal-vnet-appgateway.md)

* [Azure Application Gateway anlama](../application-gateway/index.yml)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: bir sanal ağ (VNet) içinde API Management iç modda yapılandırın ve bir Azure Application Gateway yapılandırın. Application Gateway bir PaaS hizmetidir. Ters proxy görevi görür ve L7 yük dengelemesi, yönlendirme, Web uygulaması güvenlik duvarı (WAF) ve diğer hizmetleri sağlar.

İç VNET 'te sağlanan API Management Application Gateway ön uç ile birleştirmek aşağıdaki senaryolara izin vermez:
- Tüm API 'Leri iç tüketicilere ve dış tüketicilere sunmak için tek bir API Management kaynağı kullanın.
- Bir API alt kümesini dış tüketicilere göstermek için tek bir API Management kaynağı kullanın.
- Genel Internet 'ten API Management erişimi değiştirme ve kapatma gibi bir yöntem sağlar.

Note: Bu özellik, API Management Premium ve geliştirici katmanlarında kullanılabilir.

Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

* [Application Gateway ile iç VNET 'te API Management tümleştirme](./api-management-howto-integrate-internal-vnet-appgateway.md)

* [Azure Application Gateway anlama](../application-gateway/index.yml)

* [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](../security-center/azure-defender.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: API Management dağıtıldığı alt ağa gelen ve giden trafik, ağ güvenlik grupları (NSG) kullanılarak denetlenebilir. API Management alt ağına bir NSG dağıtın ve NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Dikkat: API Management alt ağında bir NSG yapılandırılırken, açık olması gereken bir bağlantı noktası kümesi vardır. Bu bağlantı noktalarından herhangi biri kullanılamıyorsa API Management düzgün çalışmayabilir ve erişilemez hale gelebilir.

* [Azure API Management için NSG yapılandırmasını anlama](./api-management-using-with-vnet.md#-common-network-configuration-issues)

* [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: bir sanal ağ (VNet) içinde API Management iç modda yapılandırın ve bir Azure Application Gateway yapılandırın. Application Gateway bir PaaS hizmetidir. Ters proxy görevi görür ve L7 yük dengelemesi, yönlendirme, Web uygulaması güvenlik duvarı (WAF) ve diğer hizmetleri sağlar. WAF Application Gateway, yaygın güvenlik açıklarından ve güvenlik açıklarından koruma sağlar ve aşağıdaki iki modda çalışabilir:
- Algılama modu: tüm tehdit uyarılarını Izler ve günlüğe kaydeder. Tanılama bölümünde Application Gateway için günlüğü tanılamayı açabilirsiniz. WAF günlüğünün seçili ve açık olduğundan emin olmanız gerekir. Web uygulaması güvenlik duvarı, algılama modunda çalışırken gelen istekleri engellemez.
- Önleme modu: kuralların algılamadığı yetkisiz ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu WAF günlüklerinde bu tür saldırıları kaydeder.

İç VNET 'te sağlanan API Management Application Gateway ön uç ile birleştirmek aşağıdaki senaryolara izin vermez:
- Tüm API 'Leri iç tüketicilere ve dış tüketicilere sunmak için tek bir API Management kaynağı kullanın.
- Bir API alt kümesini dış tüketicilere göstermek için tek bir API Management kaynağı kullanın.
- Genel Internet 'ten API Management erişimi değiştirme ve kapatma gibi bir yöntem sağlar.

Note: Bu özellik, API Management Premium ve geliştirici katmanlarında kullanılabilir.

* [Application Gateway ile iç VNET 'te API Management tümleştirme](./api-management-howto-integrate-internal-vnet-appgateway.md)

* [Azure Application Gateway WAF 'yi anlayın](../web-application-firewall/ag/ag-overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: Web/HTTP API 'lerine trafik akışını yönetmek için API Management dış veya dahili modda App Service ortamı Ilişkili bir sanal ağa (VNet) dağıtın.

Dahili modda, API Management önünde bir Azure Application Gateway yapılandırın. Application Gateway bir PaaS hizmetidir. Ters proxy görevi görür ve L7 yük dengelemesi, yönlendirme, Web uygulaması güvenlik duvarı (WAF) ve diğer hizmetleri sağlar. WAF Application Gateway, yaygın güvenlik açıklarından ve güvenlik açıklarından koruma sağlar.

İç VNET 'te sağlanan API Management Application Gateway ön uç ile birleştirmek aşağıdaki senaryolara izin vermez:
- Tüm API 'Leri iç tüketicilere ve dış tüketicilere sunmak için tek bir API Management kaynağı kullanın.
- Bir API alt kümesini dış tüketicilere göstermek için tek bir API Management kaynağı kullanın.
- Genel Internet 'ten API Management erişimi değiştirme ve kapatma gibi bir yöntem sağlar.

Note: Bu özellik, API Management Premium ve geliştirici katmanlarında kullanılabilir.

* [Özel API 'Leri dış tüketicilere sunma](/azure/architecture/example-scenario/apps/publish-internal-apis-externally)

* [VNET içinde API Management kullanma](./api-management-using-with-vnet.md)

* [Azure Application Gateway Azure Web uygulaması güvenlik duvarı](../web-application-firewall/ag/ag-overview.md)

* [Azure Application Gateway anlama](../application-gateway/overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: API Management alt ağlarınız üzerinde kullanılan ağ güvenlik grupları (NSG 'ler) üzerindeki ağ erişim denetimlerini tanımlamak Için sanal ağ (VNet) hizmet etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Dikkat: API Management alt ağında bir NSG yapılandırılırken, açık olması gereken bir bağlantı noktası kümesi vardır. Bu bağlantı noktalarından herhangi biri kullanılamıyorsa API Management düzgün çalışmayabilir ve erişilemez hale gelebilir.

* [Hizmet etiketlerini anlama ve kullanma](../virtual-network/service-tags-overview.md)

* [API Management için gereken bağlantı noktaları](./api-management-using-with-vnet.md#-common-network-configuration-issues)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure API Management dağıtımlarınızla ilgili ağ ayarları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure API Management dağıtımlarınızın ve ilgili kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Apimanave" Microsoft. Network "ad alanlarında Azure Ilke diğer adlarını kullanın. 

Ayrıca, Azure Resource Manager şablonları, Azure rol tabanlı erişim denetimi (Azure RBAC) ve tek bir şema tanımında ilkeler gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Yeni aboneliklere, ortamlara kolayca şema uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetime yönetim sağlayabilirsiniz.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grupları (NSG 'ler) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa//veya ağ trafiğine izin veren tüm kuralların iş gereksinimini ve/veya süresini (vb.) belirtebilirsiniz.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure API Management dağıtımlarınızla ilişkili ağ kaynaklarında yapılan değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

* [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure API Management için zaman kaynaklarını korur.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulama ve gerçekleştirme, uzun süreli/arşiv depolama veya çevrimdışı analiz Için günlükleri Azure depolama 'ya gönderme ya da Azure Event Hubs kullanarak günlükleri Azure 'da diğer analizler çözümüne aktarma. Azure API Management, varsayılan olarak Azure Izleyici 'de günlükleri ve ölçümleri çıktı olarak verir. Günlüğe kaydetme ayrıntı düzeyi, hizmet genelinde ve API başına ayrı olarak yapılandırılabilir.

Azure Izleyici 'nin yanı sıra Azure API Management bir veya birkaç Azure Application Insights hizmeti ile tümleştirilebilir. Application Insights için günlük kaydı ayarları, hizmet başına veya API başına temelinde yapılandırılabilir.

İsteğe bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf güvenlik olayına ve olay yönetimine (SıEM) isteğe bağlı olarak, etkinleştirin ve yerleşik verileri etkinleştirin.

* [Tanılama ayarlarını yapılandırma](../azure-monitor/platform/diagnostic-settings.md#create-in-azure-portal)

* [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

* [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

* [Özel günlük ve analiz işlem hattı oluşturma](./api-management-howto-log-event-hubs.md)

* [Azure Application Insights ile tümleştirme](./api-management-howto-app-insights.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: denetim düzlemi denetim günlüğü için, Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve etkinlik günlüklerini, Azure 'daki diğer analiz çözümlerinde ve başka bir yerde dışarı aktarmak için Azure Event Hubs, uzun süreli safekeepıng Için Azure depolama 'ya bir Log Analytics çalışma alanına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure API Management hizmetiniz için denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" i belirleyebilirsiniz.

Veri düzlemi denetim günlüğü için, tanılama günlükleri, denetim ve sorun giderme amacıyla önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri, etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynaklarınız üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisi tarafından gerçekleştirilen işlemler hakkında bilgi sağlar.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

* [Azure API Management için tanılama ayarlarını etkinleştirme](./api-management-howto-use-azure-monitor.md#activity-logs)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

* [Günlükleri bir Azure depolama hesabına arşivleme](../azure-monitor/platform/resource-logs.md#send-to-azure-storage)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: Azure API Management, Azure izleyici 'de günlük ve ölçümleri sürekli olarak sunarak API 'lerinizin durumunu ve durumunu neredeyse gerçek zamanlı bir görünürlük sağlar. Azure Izleyici ve Log Analytics çalışma alanı (ler) ile, API Management ve ilgili kaynaklardan gelen ölçüm ve günlüklerde gözden geçirebilir, sorgulayabilir, görselleştirebilir, yönlendirebilir, arşivleyebilir, uyarıları yapılandırabilir ve eylemler gerçekleştirebilirsiniz. Anormal davranışlar için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin.

İsteğe bağlı olarak, API Management Azure Application Insights ile tümleştirin ve birincil veya ikincil izleme, izleme, raporlama ve uyarı aracı olarak kullanın.

* [Azure API Management için günlükleri izleme ve gözden geçirme](./api-management-howto-use-azure-monitor.md)

* [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/log-query/get-started-queries.md)

* [Log Analytics çalışma alanını anlayın](../azure-monitor/log-query/log-analytics-tutorial.md)

* [Azure Application Insights ile tümleştirme](./api-management-howto-app-insights.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını ve Azure API Management örneklerinizin tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve toplanan verilere göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin. Log Analytics çalışma alanı sorgularınızı temel alan uyarılar oluşturabilirsiniz.

Beklenmeyen bir şey olduğunda size bilgi vermek için ölçüm uyarıları oluşturun. Örneğin, Azure API Management örneğiniz belirli bir süre boyunca beklenen en yüksek kapasiteyi aşmışsa veya önceden tanımlanmış bir süre içinde belirli bir sayıda yetkisiz ağ geçidi isteği veya hatası varsa, bildirim alın.

İsteğe bağlı olarak, API Management Azure Application Insights ile tümleştirin ve birincil veya ikincil izleme, izleme, raporlama ve uyarı aracı olarak kullanın.

İsteğe bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

* [Azure API Management için tanılama ayarlarını etkinleştirme](./api-management-howto-use-azure-monitor.md#activity-logs)

* [Azure API Management için bir uyarı kuralı yapılandırma](./api-management-howto-use-azure-monitor.md#set-up-an-alert-rule)

* [Azure API Management örneğinin kapasite ölçümlerini görüntüleme](./api-management-capacity.md)

* [Azure Application Insights ile tümleştirme](./api-management-howto-app-insights.md)

* [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure API Management, kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure API Management, Kullanıcı tarafından erişilebilen DNS ile ilgili günlükleri işlemez veya oluşturmuyor.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure API Management Denetim düzlemi 'ne (Azure Portal) yönetici erişimi olan hesapların envanterini saklayın.

Azure Active Directory (AD), açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. API Management, API Management Hizmetleri ve varlıkları için ayrıntılı erişim yönetimini etkinleştirmek üzere bu rollere ve Role-Based Access Control bağımlıdır.

Ayrıca, API Management API Management Kullanıcı sisteminde yerleşik bir Yöneticiler grubu içerir. Geliştirici portalındaki API 'lerin API Management Denetim görünürlüğünde gruplar ve Yöneticiler grubunun üyeleri tüm API 'Leri görebilir.

Yönetim hesaplarının yönetimi ve bakımı için Azure Güvenlik Merkezi 'nin önerilerini izleyin.

* [Azure API Management'te Rol Tabanlı Erişim Denetimini kullanma](./api-management-role-based-access-control.md)

* [Azure API Management örneği altında kullanıcıların listesini alma](/powershell/module/az.apimanagement/get-azapimanagementuser)

* [Azure AD 'de PowerShell ile bir dizin rolüne atanan kullanıcıların listesini alma](/powershell/module/az.resources/get-azroleassignment)

* [Azure AD 'de PowerShell ile dizin rolü tanımı alma](/powershell/module/az.resources/get-azroledefinition)

* [Azure Güvenlik Merkezi 'nden kimlik ve erişim önerilerini anlayın](../security-center/recommendations-reference.md#recs-identityandaccess)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Kılavuz**: Azure API Management varsayılan parola/anahtar kavramı içermez.

API 'lere erişimi güvenli hale getirmenin bir yolu olan Azure API Management abonelikleri, bununla birlikte, oluşturulan bir çift abonelik anahtarı ile birlikte gelir. Müşteriler bu abonelik anahtarlarını dilediğiniz zaman yeniden oluşturabilir.

* [Azure API Management aboneliklerini anlama](./api-management-subscriptions.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

* [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](../security-center/security-center-identity-access.md)

* [Azure Ilkesini kullanma](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: Azure API Management, Azure AD tarafından sunulan SSO özelliklerinden yararlanmak üzere Geliştirici Portalında kullanıcıların kimliğini doğrulamak için bir kimlik sağlayıcısı olarak Azure Active Directory faydalanabilecek şekilde yapılandırılabilir. Yeni geliştirici portalı kullanıcıları, yapılandırıldıktan sonra ilk kez Azure AD ile kimlik doğrulaması yaparak ve ardından portalda kaydolma işlemini tamamladıktan sonra, kimlik doğrulamasından sonra, bir kez kullanıma hazır kayıt işlemini tercih edebilir.

* [Azure API Management'ta geliştirici hesaplarını yetkilendirmek için Azure Active Directory kullanın](./api-management-howto-aad.md)

Alternatif olarak, oturum açma/kaydolma işlemi, temsilciyle daha da özelleştirilebilir. Temsili, geliştirici portalındaki yerleşik işlevselliği kullanmanın aksine, geliştirici oturum açma/kaydolma ve ürün aboneliklerinizi işlemek için mevcut Web sitenizi kullanmanıza olanak sağlar. Web sitenizin kullanıcı verilerine sahip olması ve bu adımların doğrulanmasını özel bir şekilde gerçekleştirmesini sağlar.

* [Kullanıcı kaydı ve ürün aboneliği için temsilci seçme](./api-management-howto-setup-delegation.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

* [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

* [Privileged Identity Management dağıtma (PıM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

* [Azure AD risk algılamalarını anlama](../active-directory/identity-protection/overview-identity-protection.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından Azure Portal erişimine izin vermek Için, koşullu erişim adlı konum kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: mümkün olduğunda, merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure AD 'yi kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure Active Directory kullanarak Geliştirici hesaplarının kimliğini doğrulamak için Azure API Management geliştirici portalınızı yapılandırın.

Azure Active Directory (AD) ile OAuth 2,0 protokolünü kullanarak API 'lerinizi korumak için Azure API Management örneğinizi yapılandırın.

* [API Management Azure 'da Azure Active Directory kullanarak Geliştirici hesaplarını yetkilendirme](./api-management-howto-aad.md)

* [Azure Active Directory ve API Management ile OAuth 2,0 kullanarak bir API 'yi koruma](./api-management-howto-protect-backend-with-aad.md)

* [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Müşteriler, grup üyeliklerini verimli bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanabilir. Yalnızca doğru kullanıcıların uygun erişime sahip olmaya devam etmesini sağlamak için, Kullanıcı erişimi düzenli olarak incelenebilir.

Müşteriler API Management Kullanıcı hesaplarının envanterini tutabilir ve gerektiğinde erişimi mutabık hale getirebilirsiniz. API Management, geliştiriciler, API Management birlikte sunulan API 'lerin tüketicilerinden oluşur. Varsayılan olarak, yeni oluşturulan geliştirici hesapları etkindir ve geliştiriciler grubuyla ilişkilendirilir. Etkin durumda olan geliştirici hesapları, aboneliklerine sahip oldukları tüm API 'Lere erişmek için kullanılabilir.

Yöneticiler özel gruplar oluşturabilir veya ilişkili Azure Active Directory kiracılarındaki dış gruplardan yararlanabilir. Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir.

* [Azure API Management'ta kullanıcı hesaplarını yönetme](./api-management-howto-create-or-invite-developers.md)

* [API Management kullanıcıların listesini alma](/powershell/module/az.apimanagement/get-azapimanagementuser)

* [Azure API Management'ta geliştirici hesaplarını yönetmek için grup oluşturma ve kullanma](./api-management-howto-create-groups.md)

* [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Kılavuz**: Azure API Management 'de kimlik sağlayıcısı olarak Azure Active Directory kullanarak Geliştirici hesaplarının kimliğini doğrulamak için Azure API Management örneğinizi yapılandırın.

Azure Active Directory (AD) ile OAuth 2,0 protokolünü kullanarak API 'lerinizi korumak için Azure API Management örneğinizi yapılandırın.

Geçerli bir belirtecin varlığını ve geçerliliğini zorlamaya yardımcı olmak için, gelen API isteklerine JWT doğrulama ilkesi yapılandırın.

Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturun ve denetim günlüklerini ve oturum açma günlüklerini Log Analytics çalışma alanına gönderin. Log Analytics içindeki istenen uyarıları yapılandırın. Ayrıca, Log Analytics çalışma alanını Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye ekleyebilirsiniz.

İlkeyi kullanarak API Management gelişmiş izlemeyi yapılandırın `log-to-eventhub` , güvenlik çözümlemesi için gereken ek bağlam bilgilerini yakalayın ve Azure Sentinel veya üçüncü taraf SIEM 'ye gönderin.

* [API Management Azure 'da Azure Active Directory kullanarak Geliştirici hesaplarını yetkilendirme](./api-management-howto-aad.md)

* [Azure Active Directory ve API Management ile OAuth 2,0 kullanarak bir API 'yi koruma](./api-management-howto-protect-backend-with-aad.md)

* [API Management erişim kısıtlama ilkeleri](./api-management-access-restriction-policies.md)

* [Azure AD günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

* [Yerleşik Azure Sentinel](../sentinel/quickstart-onboard.md)

* [API 'lerin gelişmiş izlenmesi](./api-management-log-to-eventhub-sample.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: denetim düzleminde hesap oturum açma davranışı sapması (Azure Portal) için, otomatik yanıtları Kullanıcı kimlikleriyle ilgili şüpheli eylemlere yönelik olarak yapılandırmak için Azure ACTIVE DIRECTORY (ad) kimlik koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

* [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası Azure API Management için şu anda desteklenmiyor.

* [Müşteri Kasası tarafından desteklenen hizmetler listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure API Management örnekleri, sanal ağ (VNet)/subnet ile ayrılmalıdır ve uygun şekilde etiketlenemez.

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Sanal ağlar ile Azure API Management’ı kullanma](./api-management-using-with-vnet.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri şu anda Azure API Management için kullanılamaz.

Microsoft, Azure API Management için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yönetim DÜZLEMI çağrıları TLS üzerinden Azure Resource Manager üzerinden yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor. Veri düzlemi çağrıları TLS ve desteklenen kimlik doğrulama mekanizmalarından biri (örneğin, istemci sertifikası veya JWT) ile güvenli hale getirilmiş olabilir.

* [Azure API Management veri korumayı anlama](#data-protection)

* [Azure API Management TLS ayarlarını yönetme](./api-management-howto-manage-protocols-ciphers.md)

* [Azure Active Directory ile Azure API Management API 'Lerini koruma](./api-management-howto-protect-backend-with-aad.md)

* [Azure Active Directory B2C ile Azure API Management API 'Lerini koruma](./howto-protect-backend-frontend-azure-ad-b2c.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri şu anda Azure API Management için kullanılamaz. Bu gibi hassas bilgileri işleyen Azure API Management hizmetlerini etiketleyebilir ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygulayabilirsiniz.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure API Management erişimini denetlemek için rol tabanlı erişim denetimi kullanın. Azure API Management, API Management Hizmetleri ve varlıkları (örneğin, API 'Ler ve ilkeler) için ayrıntılı erişim yönetimini sağlamak üzere Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) kullanır.

* [Azure API Management'te Rol Tabanlı Erişim Denetimini kullanma](./api-management-role-based-access-control.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft, Azure API Management için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: sertifikalar, anahtarlar ve gizli dizi değerleri gibi hassas veriler hizmet tarafından yönetilen hizmet örneği anahtarları için şifrelenir. Tüm şifreleme anahtarları hizmet örneği başına alınır ve hizmet yönetilir.

* [Azure API Management ile bekleyen veri korumasını/şifrelemeyi anlayın](#data-protection)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, üretim Azure işlevleri uygulamalarına ve diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

* [Azure Izleyici ve Azure etkinlik günlüğü 'Nü Azure 'da kullanma API Management](./api-management-howto-use-azure-monitor.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Şu anda kullanılamıyor; Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi Şu anda Azure API Management için kullanılamıyor.

Microsoft tarafından taranan ve düzeltme eki uygulanan temel platform. Hizmet yapılandırması ile ilgili güvenlik açıklarını azaltmak için kullanılabilen güvenlik denetimlerini gözden geçirin.

* [Azure API Management kullanılabilen güvenlik denetimlerini anlama]()

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Şu anda kullanılamıyor; Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi Şu anda Azure API Management için kullanılamıyor.

Microsoft tarafından taranan ve düzeltme eki uygulanan temel platform. Hizmet yapılandırması ile ilgili güvenlik açıklarını azaltmak için müşterinin kullanabildiği güvenlik denetimlerini gözden geçirmesi için müşteri.

* [Azure API Management kullanılabilen güvenlik denetimlerini anlama]()

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

* [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription)

* [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesi 'ni kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak abonelikleriniz içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/index.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

* [Azure API Management rol tabanlı erişim denetimi](./api-management-role-based-access-control.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesi ile Azure API Management hizmetiniz için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure API Management hizmetlerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Apimanaize" ad alanındaki Azure Ilke diğer adlarını kullanın.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias)

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Ilkesi ile Azure API Management hizmetleriniz için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure API Management örneklerinin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Apimanaitimi" ad alanındaki Azure Ilke diğer adlarını kullanın. Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız Azure devops veya Azure Repos kullanarak Azure API Management hizmeti yapılandırmanızı güvenli bir şekilde depolayın ve yönetin.

* [Azure DevOps 'da dosyaları depolama](/azure/devops/repos/git/gitworkflow)

* [Azure Repos belgeleri](/azure/devops/repos/index)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Kılavuz**: Azure Ilkesi ile Azure API Management hizmetleriniz için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure API Management örneklerinin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Apimanaitimi" ad alanındaki Azure Ilke diğer adlarını kullanın. Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Kılavuz**: Azure API Management için yapılandırma yönetimi gerçekleştirmek üzere Azure API Management DevOps Resource Kit ' i kullanın.

Ayrıca Azure Ilkesi ile Azure API Management hizmetleriniz için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure API Management örneklerinin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Apimanaitimi" ad alanındaki Azure Ilke diğer adlarını kullanın. Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: sertifikaları yönetmek için Key Vault kullanın ve onları tekrar Döndür olarak ayarlayın. Özel etki alanı SSL sertifikasını yönetmek için Azure Key Vault kullanıyorsanız, sertifikanın gizli değil, sertifika olarak Key Vault yerleştirildiğinden emin olun.

* [Key Vault anahtar dönüşü için kılavuzlarla özel etki alanı adları ayarlama](./configure-custom-domain.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Microsoft

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: API Management örneğinizin Azure Key Vault gibi DIĞER Azure AD korumalı kaynaklarına kolayca ve güvenli bir şekilde erişmesini sağlamak için Azure ACTIVE DIRECTORY (ad) tarafından oluşturulan yönetilen hizmet kimliği kullanın.

* [API Management örneği için yönetilen kimlik oluşturma](./api-management-howto-use-managed-service-identity.md)

* [Yönetilen kimlikle kimlik doğrulama için ilke](./api-management-authentication-policies.md#ManagedIdentity)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik Bilgisi Tarayıcısı ayrıca bulunan kimlik bilgilerinin Azure Key Vault gibi daha güvenlik konumlara aktarılmasını sağlar.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Azure API Management), ancak müşteri içeriği üzerinde çalışmaz.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: uygulanamaz; Bu öneri, verileri depolamak için tasarlanan işlem dışı kaynaklar için tasarlanmıştır.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Azure API Management), ancak müşteri içeriği üzerinde çalışmaz.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, verileri depolamak için tasarlanan işlem dışı kaynaklar için tasarlanmıştır.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Azure API Management), ancak müşteri içeriği üzerinde çalışmaz.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: API 'lerinizi Azure API Management aracılığıyla yayımlayarak ve yönetirken, el ile tasarlayarak, uygulamanıza ve yönetmenize olanak tanıyan hata toleransı ve altyapı özellikleri avantajlarından yararlanabilirsiniz. API Management, veri düzleminin hiçbir işlem yükü eklemeden bölgesel hatalara neden olan çok bölgeli dağıtımı destekler.

API Management hizmet yedekleme ve geri yükleme özellikleri, olağanüstü durum kurtarma stratejisi uygulamak için gereken yapı taşlarını sağlar. Yedekleme ve geri yükleme işlemleri, el ile veya otomatik gerçekleştirilebilir.

* [API Management veri düzlemi birden çok bölgeye nasıl dağıtılır](./api-management-howto-deploy-multi-region.md)

* [Azure API Management'ta hizmet yedekleme ve geri yükleme işlevlerini kullanarak acil durumda kurtarma](./api-management-howto-disaster-recovery-backup-restore.md#calling-the-backup-and-restore-operations)

* [API Management yedekleme işlemini çağırma](/rest/api/apimanagement/2019-12-01/apimanagementservice/backup)

* [API Management geri yükleme işlemini çağırma](/rest/api/apimanagement/2019-12-01/apimanagementservice/restore)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Azure API Management tam sistem yedeklemesi ve geri yükleme işlemleri gerçekleştirmek için sunulan yedekleme ve geri yükleme işlemleri.

Yönetilen kimlikler, Azure Key Vault API Management özel etki alanı adları için sertifika almak üzere kullanılabilir. Azure Key Vault içinde depolanan tüm sertifikaları yedekleyin.

* [Azure API Management'ta hizmet yedekleme ve geri yükleme işlevlerini kullanarak acil durumda kurtarma](./api-management-howto-disaster-recovery-backup-restore.md#calling-the-backup-and-restore-operations)

* [Azure Key Vault sertifikalarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: hizmetin ve yedeklemelerdeki sertifikaların bir test geri yüklemesini gerçekleştirerek yedeklemeleri doğrulayın.

* [API Management geri yükleme işlemini çağırma](/rest/api/apimanagement/2019-12-01/apimanagementservice/restore)

* [Azure Key Vault sertifikalarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: Azure API Management, yedeklemeleri müşterinin sahip olduğu Azure depolama hesaplarına yazar. Yedeklemenizi korumak için Azure Storage güvenlik önerilerini izleyin.

* [Azure API Management'ta hizmet yedekleme ve geri yükleme işlevlerini kullanarak acil durumda kurtarma](./api-management-howto-disaster-recovery-backup-restore.md#calling-the-backup-and-restore-operations)

* [BLOB depolama için güvenlik önerisi](../storage/blobs/security-recommendations.md)

Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault Soft-Delete etkinleştirin.

* [Key Vault Soft-Delete etkinleştirme](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Tüm personelin rollerine ek olarak algılama aşamasından olay sonrası gözden geçirme aşamasına kadar tüm olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları bulunduğundan emin olun.

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [Kendi olay yanıtı planınızı oluşturmaya yardımcı olmak için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzuyla yararlanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun. Azure kaynaklarının önem düzeyine ve olayın oluştuğu ortama bağlı olarak uyarıların çözümünde önceliği belirlemek sizin sorumluluğunuzdadır.

* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

* [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve açıkları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

* [Uyarıların Azure Sentinel’e akışını yapma](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-to-remediate-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini yapın ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltdiğinizden emin olun

**Rehberlik**: * [lütfen Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.)

* [Microsoft 'un Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma sınamasını yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
