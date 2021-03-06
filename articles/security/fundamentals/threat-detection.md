---
title: Azure tehdit koruması | Microsoft Docs
description: Azure için Azure AD Kimlik Koruması hizmeti gibi yerleşik tehdit koruması işlevleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2021
ms.author: terrylan
ms.openlocfilehash: eb8332bda2105c3f83c0c1cc28fb7db4b1ca0102
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99549958"
---
# <a name="azure-threat-protection"></a>Azure tehdit koruması

Azure, Azure Active Directory (Azure AD), Azure Izleyici günlükleri ve Azure Güvenlik Merkezi gibi hizmetler aracılığıyla yerleşik tehdit koruması işlevselliği sunar. Bu güvenlik hizmetleri ve özellikleri koleksiyonu, Azure dağıtımlarınızda neler olduğunu anlamak için basit ve hızlı bir yol sağlar.

Azure, uygulama dağıtımlarınızın gereksinimlerini karşılamak için güvenliği yapılandırmak ve özelleştirmek üzere çok sayıda seçenek sunar. Bu makalede, bu gereksinimlerin nasıl karşıladığı açıklanır.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması

[Azure AD kimlik koruması](../../active-directory/identity-protection/overview-identity-protection.md) , kuruluşunuzun kimliklerini etkileyebilecek risk algılamaları ve olası güvenlik açıklarına genel bir bakış sağlayan bir [Azure Active Directory Premium P2](../../active-directory/fundamentals/active-directory-whatis.md) Edition özelliğidir. Kimlik koruması, [Azure AD anormal etkinlik raporları](../../active-directory/reports-monitoring/overview-reports.md)aracılığıyla kullanılabilen mevcut Azure AD anomali algılama yeteneklerini kullanır ve gerçek zamanlı anormallikleri tespit eden yeni risk algılama türlerini tanıtır.

![Azure AD Kimlik Koruması diyagramı](./media/threat-detection/azure-threat-detection-fig1.png)

Kimlik koruması, bir kimliğin tehlikede olduğunu gösterebilen anormallikleri ve risk algılamalarını tespit etmek için uyarlamalı makine öğrenme algoritmaları ve buluşsal yöntemler kullanır. Bu verileri kullanarak, kimlik koruması, bu risk algılamalarını araştırmak ve uygun düzeltme ya da risk azaltma eylemi gerçekleştirmek için raporlar ve uyarılar oluşturur.

Azure Active Directory Kimlik Koruması, bir izleme ve raporlama aracından daha fazla. Risk algılamaları temelinde, kimlik koruması her bir kullanıcı için bir Kullanıcı risk düzeyi hesapladığında, böylece kuruluşunuzun kimliklerini otomatik olarak korumak için risk tabanlı ilkeleri yapılandırabilirsiniz.

Bu risk tabanlı ilkeler, Azure Active Directory ve [EMS](../../active-directory/conditional-access/overview.md)tarafından sunulan diğer [koşullu erişim denetimlerine](../../active-directory/conditional-access/overview.md) ek olarak, parola sıfırlama ve çok faktörlü kimlik doğrulama zorlaması içeren Uyarlamalı düzeltme eylemlerini otomatik olarak engelleyebilir veya sunabilir.

### <a name="identity-protection-capabilities"></a>Kimlik koruma özellikleri

Azure Active Directory Kimlik Koruması, bir izleme ve raporlama aracından daha fazla. Kuruluşunuzun kimliklerini korumak için, belirtilen risk düzeyine ulaşıldığında algılanan sorunlara otomatik olarak yanıt veren risk tabanlı ilkeler yapılandırabilirsiniz. Bu ilkeler, Azure Active Directory ve EMS tarafından sunulan diğer koşullu erişim denetimlerine ek olarak, parola sıfırlama ve çok faktörlü kimlik doğrulama zorlaması dahil olmak üzere Uyarlamalı düzeltme eylemlerini otomatik olarak engelleyebilir veya başlatabilir.

Azure kimlik koruması 'nın hesaplarınız ve kimliklerinizin güvenliğini sağlamaya yardımcı olabilecek bazı yollarla ilgili örnekler şunlardır:

[Risk algılamaları ve riskli hesaplar algılanıyor](../../active-directory/identity-protection/overview-identity-protection.md)
-   Makine öğrenimi ve buluşsal kuralları kullanarak altı riskli algılama türünü tespit edin.
-   Kullanıcı risk düzeylerini hesaplayın.
-   Güvenlik açıklarını vurgulayarak genel güvenlik duruşunu geliştirmek için özel öneriler sağlar.

[Risk algılamalarını araştırma](../../active-directory/identity-protection/overview-identity-protection.md)
-   Risk algılamaları için bildirimleri gönderin.
-   İlgili ve bağlamsal bilgileri kullanarak risk algılamalarını araştırın.
-   Araştırmalar izlemek için temel iş akışları sağlayın.
-   Parola sıfırlama gibi Düzeltme eylemlerine kolay erişim sağlar.

[Risk tabanlı, koşullu erişim ilkeleri](../../active-directory/identity-protection/overview-identity-protection.md)
-   Oturum açma işlemlerini engelleyerek veya Multi-Factor Authentication sorunlarını isteyerek riskli oturum açma işlemlerini azaltabilirsiniz.
-   Riskli Kullanıcı hesaplarını engelleyin veya güvenli hale getirin.
-   Kullanıcıların Multi-Factor Authentication 'a kaydolmesini gerektir.

### <a name="azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management

[Azure Active Directory Privileged Identity Management (PIM)](../../active-directory/privileged-identity-management/pim-configure.md)ile kuruluşunuzdaki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Bu özellik, Azure AD 'deki kaynaklara ve Microsoft 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetler erişimi içerir.

![Azure AD Privileged Identity Management diyagramı](./media/threat-detection/azure-threat-detection-fig2.png)

PıM şunları yapmanıza yardımcı olur:

-   Microsoft 365 ve Intune gibi Microsoft çevrimiçi hizmetler için Azure AD yöneticileri ve tam zamanında (JıT) yönetim erişimi hakkında uyarı ve raporlar alın.

-   Yönetici erişim geçmişi ve yönetici atamalarındaki değişiklikler hakkında rapor alın.

-   Ayrıcalıklı bir role erişim hakkında uyarı alın.

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure izleyici günlükleri](../../azure-monitor/index.yml) , şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft bulut tabanlı bir BT yönetimi çözümüdür. Azure Izleyici günlükleri bulut tabanlı bir hizmet olarak uygulandığından, altyapı hizmetlerinde en az yatırım ile hızlı bir şekilde çalışır duruma getirebilirsiniz. Yeni güvenlik özellikleri otomatik olarak dağıtılır, devam eden bakım ve yükseltme maliyetleri kaydedilir.

Azure Izleyici günlükleri, değerli hizmetleri kendi kendine sağlamaya ek olarak, mevcut güvenlik yönetimi yatırımlarınızı buluta genişletmek için [System Center Operations Manager](/archive/blogs/cbernier/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started)gibi System Center bileşenleriyle tümleştirilebilir. System Center ve Azure Izleyici günlükleri, eksiksiz bir karma yönetim deneyimi sağlamak için birlikte çalışabilir.

### <a name="holistic-security-and-compliance-posture"></a>Bütünsel güvenlik ve uyumluluk sonrası

[Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md) , ilgilenmeniz gereken önemli sorunlar için yerleşik arama SORGULARıYLA kuruluşunuzun BT güvenlik duruşuna kapsamlı bir görünüm sağlar. Bu pano, size bilgisayarlarınızın güvenlik durumuyla ilgili yüksek düzeyde öngörü sağlar. Son 24 saat, 7 gün veya başka bir özel zaman çerçevesi içindeki tüm olayları da görüntüleyebilirsiniz.

Azure Izleyici günlükleri, yazılım güncelleştirme değerlendirmesi, kötü amaçlı yazılımdan koruma değerlendirmesi ve yapılandırma temelleri de dahil olmak üzere herhangi bir ortamın genel güvenlik duruşunu hızlı ve kolay bir şekilde anlamanıza yardımcı olur. Güvenlik günlüğü verilerine, güvenlik ve uyumluluk denetim süreçlerini kolaylaştırmak için erişilebilir.

### <a name="insight-and-analytics"></a>Öngörüler ve analiz
[Azure izleyici günlüklerinin](../../azure-monitor/log-query/log-query-overview.md) merkezinde Azure tarafından barındırılan depo vardır.

![Insight ve Analytics diyagramı](./media/threat-detection/azure-threat-detection-fig4.png)

Veri kaynaklarını yapılandırarak ve aboneliğinize çözümler ekleyerek bağlı kaynaklardan depoya veri toplayabilirsiniz.

Veri kaynakları ve çözümleri, her biri kendi özellikler kümesiyle ayrı kayıt türleri oluşturur, ancak bunları depoya sorgularda yine de analiz edebilirsiniz. Çeşitli kaynaklarla toplanan çeşitli verilerle çalışmak için aynı araçları ve yöntemleri kullanabilirsiniz.

Azure Izleyici günlükleri ile etkileşimlerinizin çoğu, herhangi bir tarayıcıda çalışan Azure portal ve toplanan verileri çözümlemek ve üzerinde işlem yapmak için yapılandırma ayarlarına ve birden çok araca erişmenizi sağlar. Portalından şunları kullanabilirsiniz:
* Toplanan verileri çözümlemek için sorgular oluşturduğunuz yerde [günlük aramaları](../../azure-monitor/log-query/log-query-overview.md) .
* En değerli aramalarınızın grafik görünümleriyle özelleştirebileceğiniz [panolar](../../azure-monitor/learn/tutorial-logs-dashboards.md).
* Ek işlevsellik ve analiz araçları sağlayan [çözümler](../../azure-monitor/insights/solutions.md).

Çözümler Azure Izleyici günlüklerine işlevsellik ekler. Bunlar öncelikle bulutta çalışır ve Log Analytics deposunda toplanan verilerin analizini sağlar. Çözümler ayrıca, günlük aramalarıyla çözümlenebilecek yeni kayıt türlerini veya çözümün Log Analytics panosunda sağladığı ek bir kullanıcı arabirimini kullanarak da tanımlayabilir.

Güvenlik Merkezi, bu tür çözümlerin bir örneğidir.

### <a name="automation-and-control-alert-on-security-configuration-drifts"></a>Otomasyon ve denetim: güvenlik yapılandırması Drifts uyarı

Azure Otomasyonu, PowerShell 'i temel alan ve bulutta çalışan runbook 'larla yönetim işlemlerini otomatikleştirir. Runbook'lar yerel kaynakların yönetilmesi için yerel veri merkezinizdeki bir sunucuda da yürütülebilir. Azure Otomasyonu, PowerShell Istenen durum yapılandırması (DSC) ile yapılandırma yönetimi sağlar.

![Azure Otomasyonu diyagramı](./media/threat-detection/azure-threat-detection-fig7.png)

Azure 'da barındırılan DSC kaynaklarını oluşturabilir ve yönetebilir ve bunları bulut ve şirket içi sistemlere uygulayabilirsiniz. Bunu yaparak yapılandırmalarını tanımlayabilir ve otomatik olarak uygulayabilir ya da güvenlik yapılandırmalarının ilke içinde kalmasını sağlamaya yardımcı olmak için, DRFT üzerinde raporlar alabilir.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, hibrit bulut ortamınızı korumanıza yardımcı olur. Bağlı kaynaklarınız için sürekli güvenlik değerlendirmeleri gerçekleştirerek, bulunan güvenlik açıkları için ayrıntılı güvenlik önerileri sağlayabilir.

Güvenlik Merkezi 'nin önerileri, yaygın uyumluluk çerçevelerine dayalı olarak güvenlik ve uyumluluk en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi [Azure Güvenlik kıyaslaması](../benchmarks/introduction.md) tabanlıdır. Bu, yaygın olarak, bulut merkezli güvenlik üzerinde bir odaklanarak [Internet güvenliği (CIS) Için merkezden](https://www.cisecurity.org/benchmark/azure/) ve [ulusal standartlar ve teknolojı Enstitüsü (NIST)](https://www.nist.gov/) üzerinde denetimler oluşturur.

Güvenlik Merkezi 'nin tümleşik bulut iş yükü koruma platformu (CWPP), **Azure Defender**, Azure ve hibrit kaynaklarınızı ve iş yüklerinizi gelişmiş, akıllı ve korumalı hale getirir. Azure Defender 'ı etkinleştirmek, bir dizi ek güvenlik özelliği getirir (bkz. [Azure Defender 'A giriş](../../security-center/azure-defender.md)). Güvenlik Merkezi 'ndeki Azure Defender panosu, ortamınız için CWP özelliklerinin görünürlüğünü ve denetimini sağlar:

:::image type="content" source="../../security-center/media/azure-defender/sample-defender-dashboard.png" alt-text="Azure Defender panosu örneği" lightbox="../../security-center/media/azure-defender/sample-defender-dashboard.png":::

Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Bunlar Microsoft’un bulut ve şirket içindeki genel varlığından edinilen kapsamlı bir telemetri kümesine erişebilmektedir. Geniş kapsamlı ve çeşitlilik barındıran bu veri kümeleri Microsoft’un yeni saldırı modellerini ve şirket içi müşteri ve kuruluş ürünlerinin yanı sıra çevrimiçi hizmetleri üzerindeki eğilimleri keşfetmesini sağlamaktadır.

Bu nedenle, Güvenlik Merkezi, saldırganlar yeni ve giderek daha fazla gelişmiş açıklardan yararlanan algılama algoritmalarını hızla güncelleştirebilir. Bu yaklaşım, hızlı hareket eden bir tehdit ortamıyla hızınızı korumanıza yardımcı olur.

:::image type="content" source="../../security-center/media/security-center-managing-and-responding-alerts/alerts-page.png" alt-text="Azure Güvenlik Merkezi 'nin güvenlik uyarıları listesi":::

Azure Defender, kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik bilgilerini otomatik olarak toplar. Tehditleri belirlemek için, bu bilgileri, birden fazla kaynaktaki bilgileri bir ilişkilendirme ile analiz eder.

Azure Defender uyarıları Güvenlik Merkezi 'nde, tehditleri nasıl düzelteceğiyle ilgili önerilerle birlikte önceliklendirilir.

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri ve [makine öğrenimi](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojilerinin sıçramalar, tüm bulut genelinde olayları değerlendirmek için kullanılır. Gelişmiş analizler, el ile yaklaşımlar aracılığıyla belirlenmesi ve saldırıların evrimini tahmin etmek için imkansız olabilecek tehditleri algılayabilir. Bu güvenlik analizi türleri sonraki bölümlerde ele alınmıştır.

### <a name="threat-intelligence"></a>Tehdit bilgileri

Microsoft, bir dizi küresel tehdit zekası erişimine sahiptir.

Azure, Microsoft 365, Microsoft CRM Online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Digital Crimes birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden fazla kaynaktan telemetri akışı.

![Tehdit bilgileri bulguları](./media/threat-detection/azure-threat-detection-fig10.jpg)

Araştırmacılar ayrıca, büyük bulut hizmeti sağlayıcıları arasında paylaşılan tehdit bilgileri bilgilerini alır ve üçüncü taraflardan gelen tehdit zekası akışlarına abone olurlar. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir. Bazı örnekler:

-   **Machine Learning 'in gücünü güçlendirin**: Azure Güvenlik Merkezi, Azure dağıtımlarınızı hedefleyen tehditleri algılamak için kullanılabilecek bulut ağı etkinliğiyle ilgili büyük miktarda veri erişimine sahiptir.

-   **Deneme yanılma algılaması**: makine öğrenimi, Secure Shell (SSH), Uzak Masaüstü Protokolü (RDP) ve SQL bağlantı noktalarına karşı deneme yanılma saldırılarını algılamaya izin veren uzaktan erişim denemelerinin geçmiş bir modelini oluşturmak için kullanılır.

-   **Giden DDoS ve Botnet algılama**: bulut kaynaklarını hedefleyen saldırılara yönelik yaygın bir amaç, diğer saldırıları yürütmek için bu kaynakların işlem gücünü kullanmaktır.

-   **Yeni davranış analizi sunucuları ve VM 'ler**: bir sunucu veya sanal makine tehlikeye atıldıktan sonra saldırganlar, algılama, kalıcılığı sağlama ve gereksinimini gidererek güvenlik denetimleri sağlayarak bu sistemde kötü amaçlı kod yürütmeye yönelik çok çeşitli teknikler sunmaktadır.

-   **Azure SQL veritabanı tehdit algılama**: veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri belirten anormal veritabanı etkinliklerini BELIRLEYEN Azure SQL veritabanı için tehdit algılama.

### <a name="behavioral-analytics"></a>Davranış analizi

Davranış analizi, verileri analiz eden ve bilinen modeller koleksiyonuyla karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir.

![Davranış analizi bulguları](./media/threat-detection/azure-threat-detection-fig11.jpg)

Desenler, uzman analistlerine göre kötü amaçlı davranışların dikkatle çözümlenmesi aracılığıyla da belirlenir. Azure Güvenlik Merkezi, sanal makine günlüklerinin, sanal ağ cihaz günlüklerinin, yapı günlüklerinin, kilitlenme dökümlerinin ve diğer kaynakların analizine dayalı olarak güvenliği aşılmış kaynakları belirlemek için davranış analizini kullanabilir.

Ayrıca, desenler, geniş bir kampanyanın destek kanıtını denetlemek için diğer sinyallerle bağıntılı bir şekilde yapılır. Bu bağıntı yerleşik tehlike göstergeleriyle tutarlı olayları tanımlamaya yardımcı olur.

Bazı örnekler:
-   **Şüpheli işlem yürütme**: Saldırganlar tespit edilmeden kötü amaçlı yazılım yürütmeye yönelik çeşitli teknikler kullanmaktadır. Örneğin, bir saldırgan kötü amaçlı yazılıma yasal sistem dosyalarıyla aynı adı verebilir, ancak bu dosyaları alternatif bir konuma yerleştirebilir, bir zararsız dosyası ile benzer bir ad kullanabilir ya da dosyanın gerçek uzantısını maskeleyebilir. Güvenlik Merkezi modelleri süreç davranışlarını ve bu gibi aykırı değerleri tespit etmek için işlem yürütmelerini izler.

-   **Gizli kötü amaçlı yazılım ve yararlanma denemeleri**: Gelişmiş kötü amaçlı yazılım, diskte depolanan yazılım bileşenlerini hiçbir şekilde yazmadan veya şifreleyerek geleneksel kötü amaçlı yazılımdan koruma ürünleri oluşturabilir. Bununla birlikte, kötü amaçlı yazılımın izleme için bellekte yer bırakması gerektiğinden, bu tür kötü amaçlı yazılımlar bellek analizi kullanılarak algılanabilir. Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar. Azure Güvenlik Merkezi, kilitlenme dökümündeki belleği analiz ederek, yazılım içindeki güvenlik açıklarını kötüye kullanma, gizli verilere erişme ve makinenizin performansını etkilemeden güvenliği aşılmış bir makine içinde sürekli olarak kalıcı hale getirmek için kullanılan teknikleri algılayabilir.

-   **Yan yana hareket ve iç keşif**: tehlikeye giren bir ağda kalıcı hale getirmek ve değerli verileri bulmak ve toplamak için saldırganlar genellikle güvenliği aşılmış makineden aynı ağ içindeki diğer kullanıcılara geçici olarak geçiş yapmayı dener. Güvenlik Merkezi, uzak komut yürütme, ağ yoklama ve hesap listeleme gibi bir saldırganın ağ içindeki Saldırganlarını genişletme girişimlerini saptamak için işlem ve oturum açma etkinliklerini izler.

-   **Kötü amaçlı PowerShell betikleri**: PowerShell, saldırganlar tarafından çeşitli amaçlarla hedef sanal makinelerde kötü amaçlı kod yürütmek için kullanılabilir. Güvenlik Merkezi şüpheli etkinliklerin kanıtı için PowerShell etkinliğini inceler.

-   **Giden saldırılar**: Saldırganlar genellikle bulut kaynaklarını ek saldırılar yerleştirmek üzere kullanma amacıyla bulut kaynaklarını hedefler. Örneğin, güvenliği aşılmış sanal makineler, diğer sanal makinelere karşı deneme yanılma saldırılarını başlatmak, istenmeyen posta göndermek veya açık bağlantı noktalarını ve diğer cihazları taramak için kullanılabilir. Ağ trafiğine machine learning uygulayan Güvenlik Merkezi giden ağ iletişimlerinin normu aştığını algılayabilir. İstenmeyen posta algılandığında, Güvenlik Merkezi, e-postaların büyük bir olasılıkla e-posta trafiğini Microsoft 365 ve yasal bir e-posta kampanyasının sonucu olup olmadığını belirleyebilir.

### <a name="anomaly-detection"></a>Anormallik algılama

Azure Güvenlik Merkezi, tehditleri tanımlamak için anormallik algılamayı da kullanır. Davranış analizinden (büyük veri kümelerinden türetilmiş bilinen modellere bağlıdır) farklı olarak anormallik algılama daha fazla “kişiselleştirilmiştir” ve dağıtımlarınıza özel taban çizgilerine odaklanır. Dağıtımlarınızın normal etkinliklerini belirlemek için makine öğrenimi uygulanır ve ardından güvenlik olayını temsil eden aykırı değer koşullarını tanımlamak için kurallar oluşturulur. Bir örneği aşağıda verilmiştir:

-   **Gelen RDP/SSH deneme yanılma saldırıları**: dağıtımlarınız, her gün ve, varsa oturum açma işlemleri olan diğer sanal makinelerde çok sayıda oturum açma izni olan sanal makinelere meşgul olabilir. Azure Güvenlik Merkezi, bu sanal makineler için temel oturum açma etkinliğini belirleyebilir ve makine öğrenimini kullanarak normal oturum açma etkinliklerini tanımlayabilir. Oturum açma ile ilgili özellikler için tanımlanan taban çizgisiyle bir tutarsızlık varsa, bir uyarı oluşturulabilir. Yine machine learning neyin önemli olduğunu belirler.

### <a name="continuous-threat-intelligence-monitoring"></a>Sürekli tehdit bilgisi izleme

Azure Güvenlik Merkezi, tehdit yatadaki değişiklikleri sürekli olarak izleyen dünya genelinde güvenlik araştırması ve veri bilimi ekipleriyle çalışır. Buna aşağıdaki girişimler dahildir:

-   **Tehdit bilgileri izleme**: tehdit bilgileri, mevcut veya ortaya çıkan tehditler hakkında mekanizmalar, göstergeler, etkiler ve eyleme dönüştürülebilir öneriler içerir. Bu bilgiler güvenlik topluluğunda paylaşılır ve Microsoft, iç ve dış kaynaklardan gelen tehdit bilgileri akışlarını sürekli olarak izler.

-   **Sinyal paylaşımı**: bulut ve şirket içi hizmetler, sunucular ve istemci uç noktası cihazlarının geniş Microsoft portföyündeki güvenlik ekiplerinden Öngörüler paylaşılır ve çözümlenir.

-   **Microsoft güvenlik uzmanları**: Microsoft 'un, adli ve Web saldırısı algılama gibi özelleştirilmiş güvenlik alanları üzerinde çalışan ekiplerle sürekli katılım.

-   **Algılama ayarlaması**: algoritmalar gerçek müşteri veri kümelerine karşı çalıştırılır ve güvenlik araştırmacıları, sonuçları doğrulamak için müşterilerle birlikte çalışır. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu birleştirilmiş çabalar, anında yararlanabilmeniz için yeni ve geliştirilmiş algılamalara sahiptir. Yapmanız gereken bir işlem yok.

## <a name="threat-protection-features-other-azure-services"></a>Tehdit koruması özellikleri: diğer Azure hizmetleri

### <a name="virtual-machines-microsoft-antimalware"></a>Sanal makineler: Microsoft Antimalware

Azure için [Microsoft kötü amaçlı yazılımdan koruma](antimalware.md) , uygulamalar ve kiracı ortamları için, insan müdahalesi olmadan arka planda çalışacak şekilde tasarlanan tek bir aracı çözümüdür. Kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere temel güvenli veya gelişmiş özel yapılandırma ile uygulama iş yüklerinizin ihtiyaçlarına göre koruma dağıtabilirsiniz. Azure kötü amaçlı yazılımdan koruma, Azure sanal makineleri için tüm Azure PaaS sanal makinelerine otomatik olarak yüklenen bir güvenlik seçeneğidir.

#### <a name="microsoft-antimalware-core-features"></a>Microsoft kötü amaçlı yazılımdan koruma temel özellikleri

Azure 'un uygulamalarınız için Microsoft Antimalware 'i dağıtan ve etkinleştiren özellikleri şunlardır:

-   **Gerçek zamanlı koruma**: kötü amaçlı yazılım yürütülmesini algılamak ve engellemek için bulut hizmetlerindeki ve sanal makinelerdeki etkinlikleri izler.

-   **Zamanlanmış tarama**: etkin olarak çalışan programlar dahil olmak üzere, düzenli aralıklarla, kötü amaçlı yazılım algılamak için hedeflenen tarama

-   Kötü amaçlı **yazılım düzeltmesi**: kötü amaçlı dosyaları silme veya karantinaya alma ve kötü amaçlı kayıt defteri girişlerini temizleme gibi algılanan kötü amaçlı yazılımlara otomatik olarak davranır.

-   **İmza güncelleştirmeleri**: korumanın önceden belirlenmiş bir sıklıkta güncel olduğundan emin olmak için en son koruma imzalarını (virüs tanımları) otomatik olarak yüklenir.

-   **Antimalware Engine güncelleştirmeleri**: Microsoft Antimalware Engine otomatik olarak güncelleştirir.

-   **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri**: Microsoft Antimalware platformunu otomatik olarak güncelleştirir.

-   **Etkin koruma**: Microsoft etkin koruma sistemi aracılığıyla gerçek zamanlı zaman uyumlu imza teslimi sağlayan, gelişen tehdit yatakına hızlı yanıt sağlamak için Microsoft Azure algılanan tehditler ve şüpheli kaynaklarla ilgili telemetri meta verilerini raporlar.

-   **Örnek raporlama**: hizmeti iyileştirmenize ve sorun gidermeye yardımcı olmak için Microsoft kötü amaçlı yazılımdan koruma hizmeti 'ne örnekler sağlar ve rapor verir.

-   **Dışlamalar**: uygulama ve hizmet yöneticilerinin belirli dosya, işlem ve sürücüleri korumadan dışlama ve performans ve diğer nedenlerle tarama için yapılandırmasına izin verir.

-   **Kötü amaçlı yazılımdan koruma olayı koleksiyonu**: işletim sistemi olay günlüğünde gerçekleştirilen kötü amaçlı yazılımdan koruma hizmeti durumunu, şüpheli etkinlikleri ve düzeltme eylemlerini kaydeder ve bunları müşterinin Azure depolama hesabına toplar.

### <a name="azure-sql-database-threat-detection"></a>Azure SQL veritabanı tehdit algılama

[Azure SQL veritabanı tehdit algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) , Azure SQL veritabanı hizmetinde yerleşik olarak bulunan yeni bir güvenlik zekası özelliğidir. Azure SQL veritabanı tehdit algılama, anormal veritabanı etkinliklerini öğrenmek, profil ve algılamak için saat içinde çalışarak, veritabanı için olası tehditleri tanımlar.

Güvenlik ofisleri veya diğer belirlenen yöneticiler, oluşan şüpheli veritabanı etkinlikleri hakkında anında bildirim alabilir. Her bildirim şüpheli etkinliğin ayrıntılarını sağlar ve tehdidi nasıl daha fazla araştırıp azaltılacağını önerir.

Şu anda Azure SQL veritabanı tehdit algılama, olası güvenlik açıklarını ve SQL ekleme saldırılarını ve anormal veritabanı erişim düzenlerini algılar.

Tehdit algılama e-posta bildirimi alındığında, kullanıcılar postadaki ayrıntılı bir bağlantı aracılığıyla ilgili denetim kayıtlarını gezinebiliyor ve görüntüleyebilir. Bu bağlantı bir denetim görüntüleyicisini veya şüpheli olay süresi etrafında ilgili denetim kayıtlarını aşağıdakilere göre gösteren önceden yapılandırılmış bir denetim Excel şablonu açar:

-   Anormal veritabanı etkinlikleriyle veritabanı/sunucu için depolama alanını denetleyin.

-   Denetim günlüğünü yazmak için olay sırasında kullanılan ilgili denetim depolama tablosu.

-   Olay oluşumunda hemen sonraki saatin denetim kayıtları.

-   Olay sırasında benzer bir olay KIMLIĞI ile kayıtları denetleme (bazı detektörler için isteğe bağlı).

SQL veritabanı tehdit algılayıcıları aşağıdaki algılama yöntemlerden birini kullanır:

-   **Belirleyici algılama**: bilinen SALDıRıLARDAN eşleşen SQL istemci sorgularında şüpheli desenleri (kurallar tabanlı) algılar. Bu metodolojide yüksek algılama ve düşük yanlış pozitif, ancak sınırlı kapsam, "atomik algılamalar" kategorisinde yer aldığından sınırlıdır.

-   **Davranış algılama**: veritabanında en son 30 gün içinde görülmeyen olağan dışı davranışlar olan anormal etkinlikleri algılar. SQL istemci anomus etkinliğine örnek olarak, başarısız oturum açma işlemleri veya sorgular, ayıklanan yüksek miktarda veri hacmi, alışılmadık kurallı sorgular veya veritabanına erişmek için bilinmeyen IP adresleri olabilir.

### <a name="application-gateway-web-application-firewall"></a>Application Gateway Web uygulaması güvenlik duvarı

[Web uygulaması güvenlik duvarı (WAF)](../../app-service/environment/app-service-app-service-environment-web-application-firewall.md) , standart [uygulama teslim denetimi](https://kemptechnologies.com/in/application-delivery-controllers) işlevleri için bir Application Gateway kullanan Web uygulamalarına koruma sağlayan bir [Azure Application Gateway](../../web-application-firewall/ag/ag-overview.md) özelliğidir. Web uygulaması güvenlik duvarı, bunu [Açık Web uygulaması güvenlik projesi (OWASP) en çok 10 genel Web güvenlik açığına](https://owasp.org/www-project-top-ten/)karşı koruyarak yapar.

![Application Gateway Web uygulaması güvenlik duvarı diyagramı](./media/threat-detection/azure-threat-detection-fig13.png)

Korumalar şunları içerir:

-   SQL ekleme koruması.

-   Çapraz site betik koruması.

-   Komut ekleme, HTTP isteği atılama, HTTP yanıtı bölme ve uzak dosya ekleme saldırısı gibi yaygın web saldırıları koruması.

-   HTTP protokol ihlallerine karşı koruma.

-   Eksik konak Kullanıcı Aracısı ve kabul başlıkları gibi HTTP protokol anormallara karşı koruma.

-   Botlar, gezginler ve tarayıcılarla karşı korunma.

-   Yaygın uygulama yapılandırması hataları (Apache, IIS vb.) tespit edilir.

Uygulama ağ geçidinizdeki WAF 'yi yapılandırmak aşağıdaki avantajları sağlar:

-   Web uygulamalarınızı, arka uç kodunu değiştirmeden Web güvenlik açıklarına ve saldırılarına karşı korur.

-   Birden çok Web uygulamasını bir uygulama ağ geçidinin arkasında aynı anda korur. Uygulama ağ geçidi, 20 ' ye kadar Web sitesini barındırmayı destekler.

-   Application Gateway WAF günlükleri tarafından oluşturulan gerçek zamanlı raporları kullanarak Web uygulamalarını saldırılara karşı izler.

-   Uyumluluk gereksinimlerini karşılamaya yardımcı olur. Belirli uyumluluk denetimleri, internet 'e yönelik tüm uç noktaların bir WAF çözümü tarafından korunmasını gerektirir.

### <a name="anomaly-detection-api-built-with-azure-machine-learning"></a>Anomali algılama API 'SI: Azure Machine Learning ile oluşturuldu

Anomali algılama API 'si, zaman serisi verilerinizde çeşitli anormal desenleri algılamada yararlı olan bir API 'dir. API, uyarı oluşturmak, panolar aracılığıyla izlemek veya bilet oluşturma sistemlerinizle bağlantı oluşturmak için kullanılabilecek zaman serisindeki her bir veri noktasına bir anomali puanı atar.

[Anomali algılama API 'si](../../machine-learning/team-data-science-process/apps-anomaly-detection-api.md) , zaman serisi verilerinde aşağıdaki tür bozukluklar algılayabilir:

-   **Ani artışlar ve DIB 'ler**: bir hizmet için oturum açma hatalarının sayısını veya bir e-ticaret sitesinde kullanıma alma sayısını izlerken, olağan dışı artışlar veya DIB 'ler güvenlik saldırılarını veya hizmet kesintilerini gösterebilir.

-   **Olumlu ve olumsuz eğilimler**: bellek kullanımını bilgi işlem sırasında izlerken boş bellek boyutunu küçültme olası bir bellek sızıntısını gösterir. Hizmet sırası uzunluğu izleme için, kalıcı bir yukarı doğru eğilim, temeldeki bir yazılım sorununa işaret edebilir.

-   **Dinamik değer aralığındaki düzey değişiklikler ve değişiklikler**: bir hizmet yükseltmesinden sonra bir hizmetin gecikme süreleriyle ilgili düzey değişiklikler veya yükseltme sonrasında daha düşük özel durum düzeyleri izlemeye ilgi çekici olabilir.

Machine Learning tabanlı API şunları sunar:

-   **Esnek ve sağlam algılama**: anomali algılama modelleri, kullanıcıların duyarlık ayarlarını yapılandırmalarına ve mevsimler ve mevsimsiz veri kümeleri arasındaki anormallikleri algılamasına olanak tanır. Kullanıcılar, algılama API 'sini gereksinimlerine göre daha az veya daha hassas hale getirmek için anomali algılama modelini ayarlayabilir. Bu, dönemsel desenler olmadan ve olmayan verilerde daha az veya daha fazla görünür bozukluklar tespit ediyor demektir.

-   **Ölçeklenebilir ve zamanında algılama**: uzmanların etki alanı bilgisi tarafından ayarlanan mevcut eşiklerle izlemenin geleneksel bir yolu pahalıdır ve dinamik olarak değişen milyonlarca veri kümesine ölçeklenebilir değildir. Bu API 'deki anomali algılama modelleri öğrenilir ve modeller hem geçmiş hem de gerçek zamanlı verilerden otomatik olarak ayarlanır.

-   **Proaktif ve** eyleme dönüştürülebilir algılama: erken anomali algılama için yavaş eğilim ve düzey değişiklik algılama uygulanabilir. Algılanan erken olağan dışı sinyaller, insanların sorun alanlarının araştırılması ve üzerinde işlem yapması için kullanılabilir. Buna ek olarak, kök neden analiz modelleri ve uyarı araçları bu anomali algılama API hizmetinin üzerine geliştirilebilir.

Anomali algılama API 'SI, hizmet durumu ve KPI izleme, IoT, performans izleme ve ağ trafiği izleme gibi çok çeşitli senaryolar için etkili ve verimli bir çözümdür. Bu API 'nin yararlı olabilecek bazı popüler senaryolar aşağıda verilmiştir:

- BT departmanlarının olayları, hata kodunu, kullanım günlüğünü ve performansı (CPU, bellek vb.) zamanında izlemek için araçlara ihtiyacı vardır.

-   Çevrimiçi ticaret siteleri, müşteri etkinliklerini, sayfa görünümlerini, tıklamalarını vb. izlemek ister.

-   Yardımcı program şirketleri su, gaz, elektrik ve diğer kaynakların tüketimini izlemek ister.

-   Tesis veya yapı Yönetim Hizmetleri, sıcaklık, Moisture, trafik vb. izlemek istiyor.

-   IoT/Üreticiler iş akışını, kaliteyi ve benzerlerini izlemek için zaman serisinde algılayıcı verilerini kullanmak ister.

-   Çağrı merkezleri, hizmet talep eğilimi, olay hacmi, bekleme sırası uzunluğu vb. gibi hizmet sağlayıcılarının izlenmesi gerekir.

-   İş Analizi grupları, iş KPI 'Larını (örn. satış hacmi, müşteri duymaları veya fiyatlandırma) olağan dışı hareketleri gerçek zamanlı olarak izlemek istiyor.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](/cloud-app-security/what-is-cloud-app-security) , Microsoft bulut güvenlik yığınının kritik bir bileşenidir. Bu, bulut uygulamalarının taahhüdünden tam olarak yararlanabilmeniz için kuruluşunuz hareket etmenize yardımcı olabilecek kapsamlı bir çözümdür. Bu, daha fazla etkinlik halinde geliştirilmiş görünürlük aracılığıyla denetim içinde kalır. Ayrıca, bulut uygulamaları genelinde kritik veri korumasını artırmaya da yardımcı olur.

Kuruluşunuz, gölge BT’yi ortaya çıkarmaya, riskleri değerlendirmeye, ilkeleri zorunlu tutmaya, etkinlikleri araştırmaya ve tehditleri durdurmaya yardımcı olan araçlarla kritik verilerin denetimini elde tutarak buluta daha güvenli bir şekilde geçebilir.

| Kategori | Açıklama |
| -------- | ----------- |
| Bulma | Cloud App Security ile gölge BT’yi ortaya çıkarın. Bulut ortamınızdaki uygulamaları, etkinlikleri, kullanıcıları, verileri ve dosyaları bularak görünürlük kazanın. Bulutunuza bağlı olan üçüncü taraf uygulamalarını keşfedin.|
|Araştır | Riskli uygulamaları, belirli kullanıcıları ve ağınızdaki dosyaları derinlemesine incelemek üzere bulut inceleme araçlarını kullanarak bulut uygulamalarınızı araştırın. Buluttan toplanan verilerdeki modelleri bulun. Bulutunuzu izlemek için raporlar oluşturun. |
| Denetim | Ağ bulut trafiği üzerinde en yüksek denetim düzeyine ulaşmak için ilkeler ve uyarılar ayarlayarak riski azaltın. Kullanıcılarınızı güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için Cloud App Security’yi kullanın. |
| Koruma | Uygulamaları tasdik etmek veya önlemek, veri kaybı engellemesini zorlamak, izinleri ve paylaşımı denetlemek ve özel raporlar ve uyarılar oluşturmak için Cloud App Security kullanın. |
| Denetim | Ağ bulut trafiği üzerinde en yüksek denetim düzeyine ulaşmak için ilkeler ve uyarılar ayarlayarak riski azaltın. Kullanıcılarınızı güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için Cloud App Security’yi kullanın. |


![Cloud App Security diyagramı](./media/threat-detection/azure-threat-detection-fig14.png)

Cloud App Security, görünürlüğünü bulutla tümleştirerek şu şekilde tümleşir:

-   Bulut ortamınızı ve kuruluşunuzun kullandığı bulut uygulamalarını eşlemek ve tanımlamak için Cloud Discovery kullanma.

-   Bulutta uygulamaları tasdikleme ve uygulama

-   , Bağlandığınız uygulamaların görünürlüğü ve idaresi için sağlayıcı API 'Lerinden yararlanan, dağıtımı kolay uygulama bağlayıcılarını kullanma.

-   ' İ ayarlayarak sürekli denetim sağlamanıza ve sonra sürekli olarak ince ayar yapmanıza yardımcı olur.

Bu kaynaklardan veri toplamada Cloud App Security, üzerinde gelişmiş analizler çalıştırır. Anormal etkinlikler hakkında derhal uyarı verir ve bulut ortamınızda derin görünürlük edinmenizi sağlar. Cloud App Security’de bir ilke yapılandırabilir ve bu ilkeyi kullanarak bulut ortamınızdaki her şeyi koruyabilirsiniz.

## <a name="third-party-threat-protection-capabilities-through-the-azure-marketplace"></a>Azure Marketi aracılığıyla üçüncü taraf tehdit koruması özellikleri

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı

Web uygulaması güvenlik duvarı gelen Web trafiğini inceler ve SQL eklemelerini, siteler arası betikleri, kötü amaçlı yazılım yüklemelerini, uygulama DDoS saldırılarını ve Web uygulamalarınıza hedeflenmiş diğer saldırıları engeller. Ayrıca, arka uç Web sunucularından gelen ve veri kaybı önleme (DLP) için yanıtları inceler. Tümleşik Erişim denetimi altyapısı, yöneticilerin güçlü kimlik doğrulaması ve Kullanıcı denetimi sağlayan kimlik doğrulama, yetkilendirme ve hesaplama (AAA) için ayrıntılı erişim denetimi ilkeleri oluşturmalarına olanak tanır.

Web uygulaması güvenlik duvarı aşağıdaki avantajları sağlar:

-   Uygulamanıza karşı SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemelerini, uygulama DDoS 'yi veya diğer saldırıları algılar ve engeller.

-   Kimlik doğrulama ve erişim denetimi.

-   Hassas verileri algılamak için giden trafiği tarar ve bilgilerin sızmasını maskeleyebilir ya da engelleyebilir.

-   Önbelleğe alma, sıkıştırma ve diğer trafik iyileştirmeleri gibi özellikleri kullanarak Web uygulaması içeriklerinin teslimini hızlandırır.

Azure Marketi 'nde bulunan Web uygulaması güvenlik duvarları örnekleri için bkz. [barırcuda WAF, brokar sanal Web uygulaması güvenlik duvarı (vWAF), Rpva SecureSphere ve ThreatSTOP IP Firewall](https://azuremarketplace.microsoft.com/marketplace/apps/barracudanetworks.waf).

## <a name="next-steps"></a>Sonraki adımlar

- [Bugünün tehditlerine yanıt verme](../../security-center/security-center-alerts-overview.md#respond-threats): Azure kaynaklarınızı hedefleyen etkin tehditleri belirlemenize yardımcı olur ve hızla yanıt vermek için gereken öngörüleri sağlar.

- [Azure SQL veritabanı tehdit algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/): veritabanlarınıza yönelik olası tehditler hakkındaki kaygılarınızı ele almanıza yardımcı olur.