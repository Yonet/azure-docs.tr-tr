---
title: Azure Application Gateway için Azure Güvenlik temeli
description: Azure Application Gateway için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 4f28665998dcac9f641d4142a0dea60707fb02e9
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2021
ms.locfileid: "99805369"
---
# <a name="azure-security-baseline-for-azure-application-gateway"></a>Azure Application Gateway için Azure Güvenlik temeli

Azure Application Gateway için Azure Güvenlik taban çizgisi, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: tüm sanal ağ Azure Application Gateway alt ağ dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu (NSG) olduğundan emin olun. Azure Application Gateway ağ güvenlik grupları desteklenirken, NSG 'niz ve Azure Application Gateway beklendiği gibi çalışması için uygun olması gereken bazı kısıtlamalar ve gereksinimler vardır.

* [Azure Application Gateway ile NSG 'leri kullanma ile ilgili kısıtlamaları ve gereksinimleri anlayın](./configuration-overview.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Application Gateway alt ağlarınız ile ilişkili ağ güvenlik grupları (NSG 'ler) IÇIN NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Note: Azure Application Gateway alt ağlarınız ile ilişkili NSG akış günlüklerinin, izin verilen trafiği göstermeyeceği bazı durumlar vardır. Yapılandırmanız aşağıdaki senaryoda eşleşiyorsa NSG akış günlüklerinizin izin verilen trafiğini görmezsiniz:
- Application Gateway v2 'yi dağıttıysanız
- Application Gateway alt ağında bir NSG var
- NSG akış günlüklerini bu NSG 'de etkinleştirdiniz

* [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md)

* [Azure Application Gateway için tanılama ve günlüğe kaydetme hakkında SSS](./application-gateway-faq.yml#what-types-of-logs-does-application-gateway-provide)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Kılavuz**: Azure Web uygulaması güvenlik duvarını (WAF), gelen trafiğin ek incelemesi için kritik Web uygulamaları önünde dağıtın. Web uygulaması güvenlik duvarı (WAF), genel sık kullanılan ve güvenlik açıklarına karşı Web uygulamalarınızın merkezi korumasını sağlayan bir hizmettir (Azure Application Gateway özelliğidir). Azure WAF, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemeleri ve DDoS saldırıları gibi saldırıları engellemek için gelen Web trafiğini inceleyerek Azure App Service Web uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir. WAF, OWASP (açık Web uygulaması güvenlik projesi) çekirdek kural kümeleri 3,1 (yalnızca WAF_v2), 3,0 ve 2.2.9 kurallarını temel alır.

* [Azure Application Gateway özelliklerini anlama](./features.md)

* [Azure WAF 'yi anlama](../web-application-firewall/ag/ag-overview.md)

* [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: DDoS saldırılarına karşı koruma sağlamak için Azure Application Gateway üretim örneklarınızla Ilişkili Azure sanal ağlarınızda DDoS standart korumasını etkinleştirin. Bilinen kötü amaçlı IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

* [DDoS korumasını yapılandırma](../ddos-protection/manage-ddos-protection.md)

* [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](../security-center/azure-defender.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Kılavuz**: Azure Application Gateway alt ağlarınız ile ilişkili ağ güvenlik grupları (NSG 'ler) IÇIN NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Note: Azure Application Gateway alt ağlarınız ile ilişkili NSG akış günlüklerinin, izin verilen trafiği göstermeyeceği bazı durumlar vardır. Yapılandırmanız aşağıdaki senaryoda eşleşiyorsa NSG akış günlüklerinizin izin verilen trafiğini görmezsiniz:
- Application Gateway v2 'yi dağıttıysanız
- Application Gateway alt ağında bir NSG var
- NSG akış günlüklerini bu NSG 'de etkinleştirdiniz

* [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md)

* [Azure Application Gateway için tanılama ve günlüğe kaydetme hakkında SSS](./application-gateway-faq.yml#what-types-of-logs-does-application-gateway-provide)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure Web uygulaması güvenlik duvarını (WAF), gelen trafiğin ek incelemesi için kritik Web uygulamaları önünde dağıtın. Web uygulaması güvenlik duvarı (WAF), genel sık kullanılan ve güvenlik açıklarına karşı Web uygulamalarınızın merkezi korumasını sağlayan bir hizmettir (Azure Application Gateway özelliğidir). Azure WAF, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemeleri ve DDoS saldırıları gibi saldırıları engellemek için gelen Web trafiğini inceleyerek Azure App Service Web uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir. WAF, OWASP (açık Web uygulaması güvenlik projesi) çekirdek kural kümeleri 3,1 (yalnızca WAF_v2), 3,0 ve 2.2.9 kurallarını temel alır.

Alternatif olarak, Azure için ID/IP 'ler özellikleri içeren Azure Marketi 'nde bulunan Barbcuda WAF gibi birden çok Market seçeneği vardır.

* [Azure Application Gateway özelliklerini anlama](./features.md)

* [Azure WAF 'yi anlama](../web-application-firewall/ag/ag-overview.md)

* [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)

* [Barbcuda WAF bulut hizmetini anlama](../app-service/environment/app-service-app-service-environment-web-application-firewall.md#configuring-your-barracuda-waf-cloud-service)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: güvenilen SERTIFIKALAR için HTTPS/SSL özellikli Web uygulamaları için Azure Application Gateway dağıtın.

* [Application Gateway dağıtma](./quick-create-portal.md)

* [Application Gateway HTTPS kullanacak şekilde yapılandırma](./create-ssl-portal.md)

* [Azure Web uygulaması ağ geçitleri ile katman 7 yük dengelemesini anlama](./overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak Için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanındaki hizmet etiketi adını (ör. GatewayManager) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Azure Application Gateway alt ağlarınız ile ilişkili ağ güvenlik grupları (NSG 'ler) için, Application Gateway v1 SKU 'SU için 65503-65534 TCP bağlantı noktalarında gelen Internet trafiğine izin vermeniz gerekir ve hedef alt ağla birlikte GatewayManager hizmet etiketi ile v2 SKU 'SU için TCP bağlantı noktaları 65200-65535. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları Azure sertifikaları tarafından korunur (kilitlidir). Bu ağ geçitlerinin müşterileri de dahil olmak üzere dış varlıklar bu uç noktalar üzerinde iletişim kuramaz.

* [Hizmet etiketlerini anlama ve kullanma](../virtual-network/service-tags-overview.md)

* [Azure Application Gateway yapılandırmasına genel bakış](./configuration-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Application Gateway dağıtımlarınızla ilgili ağ ayarları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Application Gateway, Azure sanal ağları ve ağ güvenlik gruplarının ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Network" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, yerleşik ilke tanımını da kullanabilirsiniz.

Ayrıca, Azure Resource Manager şablonları, Azure rol tabanlı erişim denetimi (Azure RBAC) ve tek bir şema tanımında ilkeler gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Yeni aboneliklere, ortamlara kolayca şema uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetime yönetim sağlayabilirsiniz.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Kılavuz**: Azure Application Gateway alt ağlarınızla ilişkili ağ güvenlik grupları (NSG 'ler) ve ağ güvenliği ve trafik akışı ile ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure Application Gateway dağıtımlarınızla ilgili ağ ayarları ve kaynakları için değişiklikleri tespit edin. Kritik ağ ayarlarında veya kaynaklarda yapılan değişiklikler gerçekleştiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

* [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure App Service gibi Azure kaynakları için kullanılan zaman kaynağını korur.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure Application Gateway alt ağını korumak için kullanılan Azure Application Gateway için denetim düzlemi düzeyinde ve ağ güvenlik grupları (NSG 'ler) gibi ilgili kaynaklarda gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" ı belirleyebilirsiniz.

Etkinlik günlüklerine ek olarak, Azure Application Gateway dağıtımlarınız için tanılama ayarlarını yapılandırabilirsiniz. Tanılama ayarları, bir kaynağın tercih ettiğiniz hedefe (depolama hesapları, Event Hubs ve Log Analytics) ait platform günlüklerinin ve ölçümlerinin akış dışa aktarılmasını yapılandırmak için kullanılır.

Azure Application Gateway Ayrıca Azure Application Insights ile yerleşik tümleştirme sunar. Application Insights günlük, performans ve hata verilerini toplar. Application Insights, performans bozuklularını otomatik olarak algılar ve sorunları tanılamanıza ve Web uygulamalarınızın nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Verileri standart saklama süresinden daha uzun tutmak için Application Insights Telemetriyi merkezi bir konuma aktarmak üzere sürekli dışarı aktarmayı etkinleştirebilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

* [Azure Application Gateway için tanılama ayarlarını etkinleştirme](./application-gateway-diagnostics.md)

* [Application Insights etkinleştirme](../azure-monitor/app/app-insights-overview.md)

* [Sürekli dışarı aktarmayı yapılandırma](../azure-monitor/app/export-telemetry.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure Application Gateway alt ağını korumak için kullanılan Azure Application Gateway için denetim düzlemi düzeyinde ve ağ güvenlik grupları (NSG 'ler) gibi ilgili kaynaklarda gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" ı belirleyebilirsiniz.

Etkinlik günlüklerine ek olarak, Azure Application Gateway dağıtımlarınız için tanılama ayarlarını yapılandırabilirsiniz. Tanılama ayarları, bir kaynağın tercih ettiğiniz hedefe (depolama hesapları, Event Hubs ve Log Analytics) ait platform günlüklerinin ve ölçümlerinin akış dışa aktarılmasını yapılandırmak için kullanılır.

Azure Application Gateway Ayrıca Azure Application Insights ile yerleşik tümleştirme sunar. Application Insights günlük, performans ve hata verilerini toplar. Application Insights, performans bozuklularını otomatik olarak algılar ve sorunları tanılamanıza ve Web uygulamalarınızın nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Verileri standart saklama süresinden daha uzun tutmak için Application Insights Telemetriyi merkezi bir konuma aktarmak üzere sürekli dışarı aktarmayı etkinleştirebilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

* [Azure Application Gateway için tanılama ayarlarını etkinleştirme](./application-gateway-diagnostics.md)

* [Application Insights etkinleştirme](../azure-monitor/app/app-insights-overview.md)

* [Sürekli dışarı aktarmayı yapılandırma](../azure-monitor/app/export-telemetry.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarının yanı sıra Azure Application Gateway için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve toplanan verilere göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin.

Azure Application Gateway 'leriniz de dahil olmak üzere tüm dağıtılan ağ kaynakları için sistem durumu ve ölçümlerinin kapsamlı bir görünümünü görmek üzere ağlar için Azure Izleyicisini kullanın.

İsteğe bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

* [Azure Application Gateway için tanılama ayarlarını etkinleştirme](./application-gateway-diagnostics.md)

* [Ağlar için Azure Izleyicisini kullanma](../azure-monitor/insights/network-insights-overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: gelen trafiğin ek Incelemesi Için Azure Web uygulaması güvenlik duvarı (WAF) v2 SKU 'su kritik Web uygulamalarının önünde dağıtın. Web uygulaması güvenlik duvarı (WAF), genel sık kullanılan ve güvenlik açıklarına karşı Web uygulamalarınızın merkezi korumasını sağlayan bir hizmettir (Azure Application Gateway özelliğidir). Azure WAF, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemeleri ve DDoS saldırıları gibi saldırıları engellemek için gelen Web trafiğini inceleyerek Azure App Service Web uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir. WAF, OWASP (açık Web uygulaması güvenlik projesi) çekirdek kural kümeleri 3,1 (yalnızca WAF_v2), 3,0 ve 2.2.9 kurallarını temel alır.

Azure VF 'niz için tanılama ayarlarını ve Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve toplanan verilere göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin. Log Analytics çalışma alanı sorgularınızı temel alan uyarılar oluşturabilirsiniz.

Azure Application Gateway 'leriniz de dahil olmak üzere tüm dağıtılan ağ kaynakları için sistem durumu ve ölçümlerinin kapsamlı bir görünümünü görmek üzere ağlar için Azure Izleyicisini kullanın. Ağlar için Azure Izleyici konsolu 'nda Azure Application Gateway uyarılarını görüntüleyebilir ve oluşturabilirsiniz.

* [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

* [Azure Application Gateway için tanılama ayarlarını etkinleştirme](./application-gateway-diagnostics.md)

* [Ağlar için Azure Izleyicisini kullanma](../azure-monitor/insights/network-insights-overview.md)

* [Azure 'da uyarı oluşturma](../azure-monitor/learn/tutorial-response.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) v2 'yi kritik Web uygulamalarının önünde, gelen trafiğin ek incelemesi için dağıtın. Web uygulaması güvenlik duvarı (WAF), genel sık kullanılan ve güvenlik açıklarına karşı Web uygulamalarınızın merkezi korumasını sağlayan bir hizmettir (Azure Application Gateway özelliğidir). Azure WAF, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemeleri ve DDoS saldırıları gibi saldırıları engellemek için gelen Web trafiğini inceleyerek Azure App Service Web uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir.

Azure Application Gateway dağıtımlarınız için tanılama ayarlarını yapılandırın. Tanılama ayarları, bir kaynağın tercih ettiğiniz hedefe (depolama hesapları, Event Hubs ve Log Analytics) ait platform günlüklerinin ve ölçümlerinin akış dışa aktarılmasını yapılandırmak için kullanılır.

* [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)

* [Azure WAF için tanılama ayarlarını yapılandırma](./application-gateway-diagnostics.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Application Gateway, Kullanıcı tarafından erişilebilen DNS ile ilgili günlükleri işlemez veya oluşturmuyor.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

* [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Application Gateway için denetim düzlemi erişimi, Azure ACTIVE DIRECTORY (ad) ile denetlenir. Azure AD varsayılan parola kavramına sahip değildir.

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

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

**Rehberlik**: API çağrıları aracılığıyla Azure uygulama ağ geçitleriniz ile etkileşim kurmak için kullanılabilecek bir belirteç almak üzere bir Azure uygulama kaydı (hizmet sorumlusu) kullanın.

* [Azure REST API 'Lerini çağırma](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

* [Azure AD ile istemci uygulamanızı (hizmet sorumlusu) kaydetme](/rest/api/azure/#register-your-client-application-with-azure-ad)

* [Azure Kurtarma Hizmetleri API bilgileri](/rest/api/recoveryservices/)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Kılavuz**: Azure AD MFA 'yı etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

* [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

* [Riskli etkinlik bayrağıyla işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md)

* [Azure Güvenlik Merkezi’nde kullanıcıların kimliğini ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (AAD) kullanın. AAD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. AAD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

* [AAD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Kılavuz**: Azure AD, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

* [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/index.yml)

* [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Kılavuz**: Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek herhangi bir SIEM/izleme aracıyla tümleştirmenize imkan tanır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.

* [Azure Etkinlik Günlüklerini Azure İzleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak için Azure AD kimlik koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

* [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: uygulanamaz; Müşteri Kasası Azure Application Gateway için geçerli değildir.

* [Müşteri Kasası tarafından desteklenen hizmetler listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Tüm sanal ağ Azure Application Gateway alt ağ dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu (NSG) olduğundan emin olun. Azure Application Gateway ağ güvenlik grupları desteklenirken, NSG 'niz ve Azure Application Gateway beklendiği gibi çalışması için uygun olması gereken bazı kısıtlamalar ve gereksinimler vardır.

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Azure Application Gateway ile NSG 'leri kullanma ile ilgili kısıtlamaları ve gereksinimleri anlayın](./configuration-overview.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: tüm sanal ağ Azure Application Gateway alt ağ dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu (NSG) olduğundan emin olun. Veri akışını azaltmaya yardımcı olmak için giden trafiği yalnızca güvenilen konumlara kısıtlayın. Azure Application Gateway ağ güvenlik grupları desteklenirken, NSG 'niz ve Azure Application Gateway beklendiği gibi çalışması için uygun olması gereken bazı kısıtlamalar ve gereksinimler vardır.

* [Azure Application Gateway ile NSG 'leri kullanma ile ilgili kısıtlamaları ve gereksinimleri anlayın](./configuration-overview.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure uygulama ağ geçitleriniz için TLS ile uçtan uca şifrelemeyi yapılandırma.

* [Azure Application Gateway kullanarak uçtan uca TLS Yapılandırma](./end-to-end-ssl-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: geçerli değil, Azure Application Gateway müşteri verilerini bekleyen depolamaz.

Microsoft, Azure Application Gateway için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure Application Gateway denetim düzlemine erişimi denetlemek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın (Azure Portal).

* [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: uygulanamaz; Azure Application Gateway, müşteri verilerini depolamaz.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, üretim Azure Application Gateway örneklerine ve diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Şu anda kullanılamıyor; Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi henüz Azure Application Gateway için kullanılabilir değil.

Microsoft tarafından taranan ve düzeltme eki uygulanan temel platform. Hizmet yapılandırması ile ilgili güvenlik açıklarını azaltmak için Azure Application Gateway için kullanılabilen güvenlik denetimlerini gözden geçirin.

* [Azure PaaS hizmetleri için özellik kapsamı (güvenlik açığı değerlendirmesi dahil)](../security-center/features-paas.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: henüz kullanılamıyor; Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi henüz Azure Application Gateway için kullanılabilir değil.

Microsoft tarafından taranan ve düzeltme eki uygulanan temel platform. Hizmet yapılandırması ile ilgili güvenlik açıklarını azaltmak için Azure Application Gateway için kullanılabilen güvenlik denetimlerini gözden geçirin.

* [Azure PaaS hizmetleri için özellik kapsamı (güvenlik açığı değerlendirmesi dahil)](../security-center/features-paas.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: henüz kullanılamıyor; Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi henüz Azure Application Gateway için kullanılabilir değil.

Microsoft tarafından taranan ve düzeltme eki uygulanan temel platform. Hizmet yapılandırması ile ilgili güvenlik açıklarını azaltmak için Azure Application Gateway için kullanılabilen güvenlik denetimlerini gözden geçirin.

* [Azure PaaS hizmetleri için özellik kapsamı (güvenlik açığı değerlendirmesi dahil)](../security-center/features-paas.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

* [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

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

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: onaylanan Azure kaynaklarını tanımlayın.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/index.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Tüm sanal ağ Azure Application Gateway alt ağ dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu (NSG) olduğundan emin olun. Azure Application Gateway ağ güvenlik grupları desteklenirken, NSG 'niz ve Azure Application Gateway beklendiği gibi çalışması için uygun olması gereken bazı kısıtlamalar ve gereksinimler vardır.

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Azure Application Gateway ile NSG 'leri kullanma ile ilgili kısıtlamaları ve gereksinimleri anlayın](./configuration-overview.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Application Gateway dağıtımlarınızla ilgili ağ ayarları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Application Gateway, Azure sanal ağları ve ağ güvenlik gruplarının ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Network" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, yerleşik ilke tanımını da kullanabilirsiniz.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

* [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını ve sistem yapılandırmalarının uyarı vermesi, denetlemesi ve uygulanması için özel ilkeler oluşturmak üzere "Microsoft. Network" ad alanındaki Azure ilkesi diğer adları ' nı kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: yerleşik Azure ilke tanımlarını ve sistem yapılandırmalarının uyarı vermesi, denetlemesi ve uygulanması için özel ilkeler oluşturmak üzere "Microsoft. Network" ad alanındaki Azure ilkesi diğer adları ' nı kullanın. Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yoksa dağıt] Azure ilkesini kullanın.

* [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure Application Gateway Azure ACTIVE DIRECTORY (ad) içinde otomatik olarak yönetilen kimlik sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Sertifikaları güvenli bir şekilde depolamak için Azure Key Vault kullanın. Azure Key Vault, gizli dizileri, anahtarları ve SSL sertifikalarını korumak için kullanabileceğiniz, platform tarafından yönetilen bir gizli depodır. Azure Application Gateway, HTTPS özellikli dinleyicilerine eklenen sunucu sertifikaları için Key Vault tümleştirmeyi destekler. Bu destek Application Gateway v2 SKU 'SU ile sınırlıdır.

* [Azure PowerShell kullanarak SSL sonlandırmasını Key Vault sertifikalarla yapılandırma](./configure-keyvault-ps.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure Application Gateway Azure ACTIVE DIRECTORY (ad) içinde otomatik olarak yönetilen kimlik sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Sertifikaları güvenli bir şekilde depolamak için Azure Key Vault kullanın. Azure Key Vault, gizli dizileri, anahtarları ve SSL sertifikalarını korumak için kullanabileceğiniz, platform tarafından yönetilen bir gizli depodır. Azure Application Gateway, HTTPS özellikli dinleyicilerine eklenen sunucu sertifikaları için Key Vault tümleştirmeyi destekler. Bu destek Application Gateway v2 SKU 'SU ile sınırlıdır.

* [Azure PowerShell kullanarak SSL sonlandırmasını Key Vault sertifikalarla yapılandırma](./configure-keyvault-ps.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik Bilgisi Tarayıcısı ayrıca bulunan kimlik bilgilerinin Azure Key Vault gibi daha güvenlik konumlara aktarılmasını sağlar.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) v2 'yi kritik Web uygulamalarının önünde, gelen trafiğin ek incelemesi için dağıtın. Web uygulaması güvenlik duvarı (WAF), genel sık kullanılan ve güvenlik açıklarına karşı Web uygulamalarınızın merkezi korumasını sağlayan bir hizmettir (Azure Application Gateway özelliğidir). Azure WAF, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemeleri ve DDoS saldırıları gibi saldırıları engellemek için gelen Web trafiğini inceleyerek Azure App Service Web uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir.

Azure Application Gateway dağıtımlarınız için tanılama ayarlarını yapılandırın. Tanılama ayarları, bir kaynağın tercih ettiğiniz hedefe (depolama hesapları, Event Hubs ve Log Analytics) ait platform günlüklerinin ve ölçümlerinin akış dışa aktarılmasını yapılandırmak için kullanılır.

* [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)

* [Azure WAF için tanılama ayarlarını yapılandırma](./application-gateway-diagnostics.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: uygulanamaz; Azure Application Gateway, müşteri verilerini depolamaz.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) kullanılırken WAF ilkelerini yapılandırabilirsiniz. Bir WAF ilkesi iki tür güvenlik kuralından oluşur: müşteri tarafından yazılan özel kurallar ve Azure tarafından yönetilen önceden yapılandırılmış kuralların bir koleksiyonu olan yönetilen kural kümeleri. Azure tarafından yönetilen kural kümeleri, yaygın bir güvenlik tehditleri kümesine karşı koruma dağıtmanın kolay bir yolunu sağlar. Bu tür RuleSets 'ler Azure tarafından yönetildiğinden, yeni saldırı imzalarından korunmak için kurallar gerektiği şekilde güncelleştirilir.

* [Azure tarafından yönetilen WAF kural kümelerini anlama](../web-application-firewall/ag/ag-overview.md#waf-policy-and-rules)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Kılavuz**: Azure Application Gateway müşteri verilerini depolamaz. Ancak, özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure DevOps veya Azure Repos kullanın.

Azure DevOps Services donanım hatası, hizmet kesintisi veya bölge çapında olağanüstü durum gibi durumlarda verilerin kullanılabilir olmasını sağlamak için Azure depolama alanlarına özgü birçok farklı özellikten faydalanır. Azure DevOps ekibi ayrıca verileri yanlışlıkla veya kötü niyetli kişiler tarafından silinmeye karşı korumak için gerekli yordamları izler.

* [Azure DevOps 'da veri kullanılabilirliğini anlama](/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability)

* [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Azure Key Vault içindeki müşteri tarafından yönetilen sertifikaları yedekleyin.

* [Azure 'da Anahtar Kasası sertifikalarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: yedeklenen müşteri tarafından yönetilen sertifikaların geri yüklemesini test edin.

* [Anahtar Kasası sertifikalarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Key Vault için geçici silmenin etkinleştirildiğinden emin olun. Geçici silme, silinen anahtar kasalarının ve anahtar, gizli dizi ve sertifika gibi kasa nesnelerinin kurtarılmasına olanak tanır.

* [Azure Key Vault geçici silme kullanma](../key-vault/general/key-vault-recovery.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Tüm personelin rollerine ek olarak algılama aşamasından olay sonrası gözden geçirme aşamasına kadar tüm olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları bulunduğundan emin olun.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../security-center/security-center-planning-and-operations-guide.md)

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve açıkları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

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

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: 

* [Sızma testlerinizin Microsoft ilkelerini ihlal etmemesini sağlamak için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin