---
title: Azure Izleyici 'de günlük sorgu kapsamı Log Analytics
description: Azure Izleyici Log Analytics bir günlük sorgusunun kapsamını ve zaman aralığını açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/09/2020
ms.openlocfilehash: ddb73de87789934aa66893bdbe6519011cf324b3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96186108"
---
# <a name="log-query-scope-and-time-range-in-azure-monitor-log-analytics"></a>Azure Izleyici 'de günlük sorgusu kapsamı ve zaman aralığı Log Analytics
[Azure portal Log Analytics](./log-analytics-tutorial.md)bir [günlük sorgusu](log-query-overview.md) çalıştırdığınızda, sorgu tarafından değerlendirilen veri kümesi, seçtiğiniz kapsama ve zaman aralığına bağlıdır. Bu makalede kapsam ve zaman aralığı ve gereksinimlerinize bağlı olarak her bir şekilde nasıl ayarlayabileceğiniz açıklanır. Ayrıca, farklı kapsam türlerinin davranışlarını açıklar.


## <a name="query-scope"></a>Sorgu kapsamı
Sorgu kapsamı sorgu tarafından değerlendirilen kayıtları tanımlar. Bu, genellikle tek bir Log Analytics çalışma alanındaki veya Application Insights uygulamasındaki tüm kayıtları içerir. Log Analytics Ayrıca, belirli bir izlenen Azure kaynağı için bir kapsam ayarlamanıza olanak sağlar. Bu kaynak birden fazla çalışma alanına yazsa bile, bir kaynak sahibinin yalnızca verilerine odaklanmalarına olanak sağlar.

Kapsam her zaman Log Analytics penceresinin sol üst kısmında görüntülenir. Bir simge, kapsamın bir Log Analytics çalışma alanı mı yoksa Application Insights uygulaması mı olduğunu gösterir. Hiçbir simge başka bir Azure kaynağını gösterir.

![Portalda görünen kapsam](media/scope/scope.png)

Kapsam, Log Analytics başlamak için kullandığınız yönteme göre belirlenir ve bazı durumlarda, üzerine tıklayarak kapsamı değiştirebilirsiniz. Aşağıdaki tabloda, kullanılan farklı kapsam türleri ve her biri için farklı Ayrıntılar listelenmektedir.

> [!IMPORTANT]
> Application Insights içinde çalışma alanı tabanlı bir uygulama kullanıyorsanız, verileri diğer tüm günlük verileriyle birlikte bir Log Analytics çalışma alanında depolanır. Geriye dönük uyumluluk için, kapsamı olarak uygulamayı seçerken klasik Application Insights deneyimini alacaksınız. Log Analytics çalışma alanında bu verileri görmek için, kapsamı çalışma alanı olarak ayarlayın.

| Sorgu kapsamı | Kapsamdaki kayıtlar | Seçme | Kapsam değiştiriliyor |
|:---|:---|:---|:---|
| Log Analytics çalışma alanı | Log Analytics çalışma alanındaki tüm kayıtlar. | **Azure izleyici** menüsünde veya **Log Analytics çalışma alanları** menüsünden **Günlükler** ' i seçin.  | Kapsamı diğer kaynak türlerine değiştirebilir. |
| Application Insights uygulaması | Application Insights uygulamasındaki tüm kayıtlar. | Uygulamanın **Application Insights** menüsünden **Günlükler** ' i seçin. | Kapsam yalnızca başka bir Application Insights uygulama olarak değiştirilebilir. |
| Kaynak grubu | Kaynak grubundaki tüm kaynaklar tarafından oluşturulan kayıtlar. Birden çok Log Analytics çalışma alanındaki verileri içerebilir. | Kaynak grubu menüsünden **Günlükler** ' i seçin. | Kapsam değiştirilemiyor.|
| Abonelik | Abonelikteki tüm kaynaklar tarafından oluşturulan kayıtlar. Birden çok Log Analytics çalışma alanındaki verileri içerebilir. | Abonelik menüsünden **Günlükler** ' i seçin.   | Kapsam değiştirilemiyor. |
| Diğer Azure kaynakları | Kaynak tarafından oluşturulan kayıtlar. Birden çok Log Analytics çalışma alanındaki verileri içerebilir.  | Kaynak menüsünden **Günlükler** ' i seçin.<br>VEYA<br>**Azure izleyici** menüsünden **Günlükler** ' i seçin ve ardından yeni bir kapsam seçin. | Kapsam yalnızca aynı kaynak türüne değiştirilebilir. |

### <a name="limitations-when-scoped-to-a-resource"></a>Kaynak kapsamına alınan sınırlamalar

Sorgu kapsamı bir Log Analytics çalışma alanı veya Application Insights uygulaması olduğunda, portaldaki tüm seçenekler ve tüm sorgu komutları kullanılabilir. Aynı halde bir kaynak kapsamında, portalda aşağıdaki seçenekler tek bir çalışma alanı veya uygulamayla ilişkilendirildiğinden kullanılamaz:

- Kaydet
- Sorgu Gezgini
- Yeni uyarı kuralı

Sorgu kapsamı, kaynak veya kaynak kümesi için veri içeren herhangi bir çalışma alanı içerdiğinden, bir kaynağa kapsam yaparken aşağıdaki komutları kullanamazsınız:

- [uygulamanızda](app-expression.md)
- [alanında](workspace-expression.md)
 

## <a name="query-scope-limits"></a>Sorgu kapsamı sınırları
Tek bir sorgudaki dağıtılmış verileri otomatik olarak birleştirebileceğinizden bu yana, kapsamı bir kaynağa veya kaynak kümesine ayarlamak özellikle güçlü bir Log Analytics özelliğidir. Verilerin birden çok Azure bölgesindeki çalışma alanlarından alınması gerekiyorsa performansı önemli ölçüde etkileyebilir.

Log Analytics, belirli sayıda bölge kullanılırken bir uyarı veya hata vererek, birden çok bölgedeki çalışma alanlarını kapsayan sorgulardan aşırı yüke karşı korumaya yardımcı olur. Kapsam 5 veya daha fazla bölgede çalışma alanı içeriyorsa sorgunuz bir uyarı alır. çalışmaya devam eder, ancak tamamlanması aşırı zaman alabilir.

![Sorgu uyarısı](media/scope/query-warning.png)

Kapsam, 20 veya daha fazla bölgede çalışma alanları içeriyorsa, bu sorgunun çalışması engellenir. Bu durumda, çalışma alanı bölgelerinin sayısını azaltmanız ve sorguyu yeniden çalıştırmayı denemeniz istenecektir. Açılan listede, sorgunun kapsamındaki tüm bölgeler görüntülenir ve sorguyu çalıştırmayı yeniden denemeden önce bölge sayısını azaltmanız gerekir.

![Sorgu başarısız oldu](media/scope/query-failed.png)


## <a name="time-range"></a>Zaman aralığı
Zaman aralığı, kaydın oluşturulduğu zamana göre sorgu için değerlendirilen kayıt kümesini belirtir. Bu, aşağıdaki tabloda belirtildiği gibi çalışma alanı veya uygulamadaki her kayıtta **TimeGenerated** sütunu tarafından tanımlanır. Klasik bir Application Insights uygulaması için zaman **damgası** sütunu zaman aralığı için kullanılır.


Log Analytics penceresinin en üstündeki saat seçicisinden seçim yaparak zaman aralığını ayarlayın.  Önceden tanımlanmış bir süre seçebilir veya belirli bir zaman aralığı belirtmek için **özel** ' i seçebilirsiniz.

![Saat seçici](media/scope/time-picker.png)

Sorguda, yukarıdaki tabloda gösterildiği gibi standart zaman sütununu kullanan bir filtre ayarlarsanız, saat seçici **sorgu olarak ayarlanır** ve zaman seçici devre dışı bırakılır. Bu durumda, izleyen işlemin yalnızca filtrelenmiş kayıtlarla çalışması için filtreyi sorgunun en üstüne koymak en verimli yoldur.

![Filtrelenmiş sorgu](media/scope/query-filtered.png)

Başka bir çalışma alanından veya klasik uygulamadan veri almak için [çalışma alanı](workspace-expression.md) veya [uygulama](app-expression.md) komutunu kullanırsanız, zaman seçici farklı davranabilir. Kapsam bir Log Analytics çalışma alanım ve **uygulama** kullanıyorsanız ya da kapsam klasik bir Application Insights uygulaması ise ve **çalışma alanı** kullanıyorsanız, Log Analytics filtrede kullanılan sütunun saat filtresini belirlemesi gerektiğini anlayamayabilir.

Aşağıdaki örnekte, kapsam bir Log Analytics çalışma alanına ayarlanır.  Sorgu, başka bir Log Analytics çalışma alanından veri almak için **çalışma alanını** kullanır. Saat Seçici, beklenen **TimeGenerated** sütununu kullanan bir filtre gördüğünden **sorgu içinde ayarlanacak** şekilde değişir.

![Çalışma alanıyla sorgula](media/scope/query-workspace.png)

Sorgu, klasik bir Application Insights uygulamasından veri almak için **uygulama** kullanıyorsa, Log Analytics filtrenin **zaman damgası** sütununu tanımaz ve zaman seçici değişmeden kalır. Bu durumda, her iki filtre de uygulanır. Örnekte, **WHERE** yan tümcesinde 7 gün belirtse de yalnızca son 24 saat içinde oluşturulan kayıtlar sorguya dahil edilir.

![Uygulamayla sorgulama](media/scope/query-app.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal Log Analytics kullanma hakkında öğreticiyi](./log-analytics-tutorial.md)adım adım inceleyin.
- [Sorgu yazma hakkında öğreticiyi](get-started-queries.md)adım adım inceleyin.