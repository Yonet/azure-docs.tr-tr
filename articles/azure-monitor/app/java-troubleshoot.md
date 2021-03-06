---
title: Java Web projesinde Application Insights sorunlarını giderme
description: Sorun giderme kılavuzu-Application Insights ile canlı Java uygulamalarını izleme.
ms.topic: conceptual
ms.date: 03/14/2019
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
ms.openlocfilehash: df1f58418452ecafacddd013ab815ebca5cb8a35
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100097560"
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java-sdk"></a>Java SDK için sorun giderme ve soru-cevap Application Insights

> [!IMPORTANT]
> Java uygulamalarını izlemek için önerilen yaklaşım, kodu değiştirmeden otomatik izleme kullanmaktır. Lütfen [Application Insights Java 3,0 Aracısı](./java-in-process-agent.md)için yönergeleri izleyin.

[Java 'Da Azure Application Insights][java]sorular veya sorunlar var mı? Aşağıda birkaç ipucu verilmiştir.

## <a name="build-errors"></a>Derleme hataları
**Çakışan Küreler veya IntelliJ fikri içinde, Maven veya Gradle aracılığıyla Application Insights SDK 'Sı eklerken derleme veya sağlama toplamı doğrulama hataları alıyorum.**

* Dependency `<version>` öğesi joker karakter (ör. (Maven) `<version>[2.0,)</version>` veya (Gradle)) içeren bir model kullanıyorsa `version:'2.+'` , gibi belirli bir sürüm belirtmeyi deneyin `2.6.2` .

## <a name="no-data"></a>Veri yok
**Başarıyla Application Insights ekledim ve uygulamamı çalıştırdım, ancak portalda hiç veri gördüm.**

* Bir dakika bekleyip Yenile ' ye tıklayın. Grafikler düzenli aralıklarla yenilenir, ancak el ile de yenileme yapabilirsiniz. Yenileme aralığı, grafiğin zaman aralığına bağlıdır.
* ApplicationInsights.xml dosyasında tanımlanmış bir izleme anahtarınız olup olmadığını denetleyin (Projenizdeki kaynaklar klasöründe) veya ortam değişkeni olarak yapılandırılmış.
* `<DisableTelemetry>true</DisableTelemetry>`XML dosyasında düğüm olmadığını doğrulayın.
* Güvenlik duvarınızdaki dc.services.visualstudio.com 'e giden trafik için 80 ve 443 TCP bağlantı noktalarını açmanız gerekebilir. [Güvenlik Duvarı özel durumlarının tam listesini](./ip-addresses.md) görüntüleyin
* Microsoft Azure başlangıç panosunda, hizmet durumu haritasına bakın. Bazı uyarı göstergeleri varsa, Tamam ' a dönene kadar bekleyin ve ardından Application Insights uygulaması dikey penceresini kapatıp yeniden açın.
* [](#debug-data-from-the-sdk) `<SDKLogger />` ApplicationInsights.xml dosyasındaki kök düğümün altına bir öğe ekleyerek günlüğü açın (Projenizdeki kaynaklar klasöründe) ve tüm şüpheli Günlükler için, AI: Info/warn/Error ' a ait olan girişleri kontrol edin. 
* Doğru ApplicationInsights.xml dosyanın Java SDK 'Sı tarafından başarılı bir şekilde yüklendiğinden emin olun. Bu, konsolun "yapılandırma dosyası başarıyla bulundu" bildiriminin çıkış iletilerine bakar.
* Yapılandırma dosyası bulunamazsa, yapılandırma dosyasının nerede arandığını görmek için çıkış iletilerini denetleyin ve ApplicationInsights.xml bu arama konumlarından birinde bulunduğundan emin olun. Thumb kuralı olarak, yapılandırma dosyasını Application Insights SDK JARs yakınına yerleştirebilirsiniz. Örneğin: Tomcat 'te bu, WEB-INF/Classes klasörü anlamına gelir. Geliştirme sırasında, ApplicationInsights.xml Web projenizin Resources klasörüne yerleştirebilirsiniz.
* Ayrıca, SDK ile ilgili bilinen sorunlar için [GitHub sorunları sayfasına](https://github.com/microsoft/ApplicationInsights-Java/issues) bakın.
* Sürüm Çakışma sorunlarından kaçınmak için lütfen Application Insights Core, Web, aracı ve günlüğe kaydetme uygulamalarına ait aynı sürümü kullandığınızdan emin olun.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Verileri görmek için kullandım, ancak durdu
* Aylık veri noktaları kotasına ulaştınız misiniz? Bulmak için ayarları/kotayı ve fiyatlandırmayı açın. Bu durumda, planınızı yükseltebilir veya ek kapasite için ödeme yapabilirsiniz. [Fiyatlandırma şemasına](https://azure.microsoft.com/pricing/details/application-insights/)bakın.
* Son zamanlarda SDK 'nizi yükseltmişseniz mi? Lütfen proje dizini içinde yalnızca benzersiz SDK jar dosyaları dışındaki bulunduğundan emin olun. SDK 'nın iki farklı sürümü mevcut olmamalıdır.
* Doğru AI kaynağına mı bakıyorsunuz? Lütfen uygulamanızın Ikey değerini Telemetriyi beklediğiniz kaynakla eşleştirin. Aynı olmaları gerekir.

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Beklediğiniz tüm verileri göremiyorum
* Kullanım ve tahmini maliyet sayfasını açın ve [örnekleme](./sampling.md) işleminin çalışıp çalışmadığını denetleyin. (%100 iletim, örnekleme işleminin işlem içinde olmadığı anlamına gelir.) Application Insights hizmeti, yalnızca uygulamanızdan gelen telemetrinin bir bölümünü kabul edecek şekilde ayarlanabilir. Bu, aylık telemetri kotasında tutmanıza yardımcı olur.
* SDK örneklemesi açık mı? Yanıt Evet ise, veriler tüm uygulanabilir türler için belirtilen hızda örneklenir.
* Java SDK 'sının eski bir sürümünü çalıştırıyor musunuz? Sürüm 2.0.1 ile başlayarak, aralıklı ağ ve arka uç hatalarının yanı sıra yerel sürücülerde veri kalıcılığını işlemek için hata toleransı mekanizması sunuyoruz.
* Aşırı telemetri nedeniyle kısıtlanıyor musunuz? BILGI günlüğünü açarsanız, "uygulama kısıtlandı" bir günlük iletisi görürsünüz. Geçerli sınırımız, 32K telemetri öğe/saniye.

### <a name="java-agent-cannot-capture-dependency-data"></a>Java Aracısı bağımlılık verilerini yakalayamaz
* Java aracısını [Yapılandır](java-agent.md) ' ı izleyerek Java aracısını yapılandırdınız mı?
* Hem Java aracı jar 'in hem de AI-Agent.xml dosyasının aynı klasöre yerleştirildiğinden emin olun.
* Otomatik toplamaya çalıştığınız bağımlılığın otomatik toplama için desteklendiğinden emin olun. Şu anda yalnızca MySQL, MsSQL, Oracle DB ve Redsıs bağımlılık koleksiyonu için Azure önbelleğini destekliyoruz.

## <a name="no-usage-data"></a>Kullanım verisi yok
**İstekler ve yanıt süreleri hakkında veri görüyorum, ancak sayfa görüntüleme, tarayıcı veya Kullanıcı verisi yok.**

Uygulamanızı sunucudan telemetri gönderecek şekilde ayarlayın. Şimdi bir sonraki adımınız Web [tarayıcısından telemetri göndermek için Web sayfalarınızı ayarlamanıza][usage]olanak sağlar.

Alternatif olarak, istemciniz [telefonda veya diğer bir cihazdaki][platforms]bir uygulama ise, bu bilgisayardan telemetri gönderebilirsiniz.

Hem istemci hem de sunucu telemetrinizi ayarlamak için aynı izleme anahtarını kullanın. Veriler aynı Application Insights kaynağında görünür ve olayları istemci ve sunucudan ilişkilendirebileceksiniz.

## <a name="disabling-telemetry"></a>Telemetri devre dışı bırakılıyor
**Telemetri toplamayı nasıl devre dışı bırakabilirim?**

Kod:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Veya**

Güncelleştirme ApplicationInsights.xml (Projenizdeki kaynaklar klasöründe). Kök düğümü altına aşağıdakileri ekleyin:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

XML yöntemini kullanarak, değeri değiştirirken uygulamayı yeniden başlatmanız gerekir.

## <a name="changing-the-target"></a>Hedefi değiştirme
**Projem hangi Azure kaynağını veri gönderdiğini nasıl değiştirebilirim?**

* [Yeni kaynağın izleme anahtarını alır.][java]
* Azure Toolkit for Eclipse kullanarak projenize Application Insights eklediyseniz, Web projenize sağ tıklayın, **Azure**' ı seçin **Application Insights yapılandırın** ve anahtarı değiştirin.
* Izleme anahtarını ortam değişkeni olarak yapılandırdıysanız lütfen ortam değişkeninin değerini yeni Ikey ile güncelleştirin.
* Aksi takdirde, ApplicationInsights.xml içindeki anahtarı projenizdeki Resources klasöründe güncelleştirin.

## <a name="debug-data-from-the-sdk"></a>SDK 'dan veri ayıklama

**SDK 'nın ne yaptığını nasıl öğrenebilirim?**

API 'de neler olduğu hakkında daha fazla bilgi edinmek için `<SDKLogger/>` ApplicationInsights.xml yapılandırma dosyasının kök düğümü altına ekleyin.

### <a name="applicationinsightsxml"></a>ApplicationInsights.xml

Günlükçüde bir dosyanın çıktısını almak için de talimat verebilirsiniz:

```XML
  <SDKLogger type="FILE"><!-- or "CONSOLE" to print to stderr -->
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AISDK</BaseFolderPath>
</SDKLogger>
```

### <a name="spring-boot-starter"></a>Spring Boot Starter

Application Insights Spring Boot Starter 'ı kullanarak Spring Boot uygulamalarıyla SDK günlüğünü etkinleştirmek için aşağıdakileri `application.properties` dosyasına ekleyin:

```yaml
azure.application-insights.logger.type=file
azure.application-insights.logger.base-folder-path=C:/agent/AISDK
azure.application-insights.logger.level=trace
```

veya standart hataya yazdırmak için:

```yaml
azure.application-insights.logger.type=console
azure.application-insights.logger.level=trace
```

### <a name="java-agent"></a>Java Aracısı

JVM aracı günlüğünü etkinleştirmek için [AI-Agent.xml dosyasını](java-agent.md)güncelleştirin:

```xml
<AgentLogger type="FILE"><!-- or "CONSOLE" to print to stderr -->
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AIAGENT</BaseFolderPath>
</AgentLogger>
```

### <a name="java-command-line-properties"></a>Java komut satırı özellikleri
_Sürüm 2.4.0 beri_

Komut satırı seçeneklerini kullanarak günlüğe kaydetmeyi etkinleştirmek için yapılandırma dosyalarını değiştirmeden:

```
java -Dapplicationinsights.logger.file.level=trace -Dapplicationinsights.logger.file.uniquePrefix=AI -Dapplicationinsights.logger.baseFolderPath="C:/my/log/dir" -jar MyApp.jar
```

veya standart hataya yazdırmak için:

```
java -Dapplicationinsights.logger.console.level=trace -jar MyApp.jar
```

## <a name="the-azure-start-screen"></a>Azure başlangıç ekranı
**[Azure Portal](https://portal.azure.com)aradım. Harita Uygulamam hakkında bir şey mı anladı?**

Hayır, dünyanın dört bir yanındaki Azure sunucularının sistem durumunu gösterir.

*Azure başlangıç panosu 'nda (ana ekran), Uygulamam hakkında nasıl veri bulabilirim?*

[Uygulamanızı Application Insights için ayarladığımldığında][java], sırasıyla Araştır ' a tıklayın, Application Insights ' i seçin ve uygulamanız için oluşturduğunuz uygulama kaynağını seçin. Gelecekte daha hızlı bir şekilde yararlanmak için uygulamanızı başlangıç panosuna sabitleyebilirsiniz.

## <a name="intranet-servers"></a>Intranet sunucuları
**İntranetinizdeki bir sunucuyu izleyebilir miyim?**

Evet, sunucunuz ortak internet üzerinden Application Insights portalına telemetri gönderebilmektedir.

SDK 'nın portala veri göndermesini sağlamak için [sunucunuzun güvenlik duvarında bazı giden bağlantı noktalarını açmanız](./ip-addresses.md#outgoing-ports) gerekebilir.

## <a name="data-retention"></a>Veri saklama
**Veriler portalda ne kadar süreyle tutulur? Güvenli mi?**

Bkz. [veri saklama ve gizliliği][data].

## <a name="debug-logging"></a>Hata ayıklama günlüğü
Application Insights kullanır `org.apache.http` . Bu, ad alanı altındaki Application Insights çekirdek jar dosyaları dışındaki içinde yer alan yeniden konumlandırılır `com.microsoft.applicationinsights.core.dependencies.http` . Bu Application Insights, `org.apache.http` tek bir kod tabanında aynı yerde bulunan farklı sürümlerin bulunduğu senaryoları işlemesini sağlar.

>[!NOTE]
>Uygulamadaki tüm ad alanları için hata ayıklama düzeyinde günlüğe yazmayı etkinleştirirseniz, olarak yeniden adlandırıldı dahil tüm çalışan modüller tarafından kabul edilir `org.apache.http` `com.microsoft.applicationinsights.core.dependencies.http` . Application Insights, günlük çağrısı Apache kitaplığı tarafından yapıldığından, bu çağrılar için filtreleme uygulayamayacak. Hata ayıklama düzeyinde günlüğe kaydetme, önemli miktarda günlük verisi üretir ve canlı üretim örnekleri için önerilmez.

## <a name="next-steps"></a>Sonraki adımlar
**Java sunucusu uygulamamın Application Insights ayarladım. Başka ne yapabilirim?**

* [Web sayfalarınızın kullanılabilirliğini izleyin][availability]
* [Web sayfası kullanımını izle][usage]
* [Cihaz uygulamalarınızdaki kullanımı izleyin ve sorunları tanılayın][platforms]
* [Uygulamanızın kullanımını izlemek için kod yazın][track]
* [Tanılama günlüklerini yakala][javalogs]

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)
* [GitHub 'da sorun dosya](https://github.com/microsoft/ApplicationInsights-Java/issues)

<!--Link references-->

[availability]: ./monitor-web-app-availability.md
[data]: ./data-retention-privacy.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[platforms]: ./platforms.md
[track]: ./api-custom-events-metrics.md
[usage]: javascript.md

