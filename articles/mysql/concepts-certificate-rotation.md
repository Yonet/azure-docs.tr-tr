---
title: MySQL için Azure veritabanı için sertifika döndürme
description: MySQL için Azure veritabanı 'nı etkileyecek kök sertifika değişikliklerinin yakında değişiklikler hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2021
ms.openlocfilehash: a65ac8d52c17a288447193fb8c0fba2c6e6c5554
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201272"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'ndaki kök CA 'daki değişiklikleri anlama

MySQL için Azure veritabanı, [veritabanı sunucusuna bağlanmak](concepts-connectivity-architecture.md)için kullanılan, SSL ile etkinleştirilen istemci uygulaması/sürücüsü için kök sertifikayı değiştiriyor. Şu anda kullanılabilir kök sertifika, standart bakım ve güvenlik en iyi uygulamalarının bir parçası olarak 15 Şubat 2021 (02/15/2021) tarihinde dolacak şekilde ayarlanmıştır. Bu makale, yaklaşan değişiklikler hakkında daha fazla ayrıntıyı, etkilenecek kaynakları ve uygulamanızın veritabanı sunucunuza bağlantıyı korumasından emin olmak için gerekli adımları sağlar.

>[!NOTE]
> Müşterilerin geri bildirimlerine bağlı olarak, var olan Baltidaha fazla kök CA 'nın 11 Ekim 2021 ' den itibaren 2020 ' ye kadar kök sertifikayı kullanımdan kaldırabiliyoruz. Bu uzantının, kullanıcılarımız etkilendiklerinde istemci değişikliklerini uygulaması için yeterli sağlama süresi sağlamasını umuyoruz.

> [!NOTE]
> Sapma ücretsiz iletişim
>
> Microsoft, farklı ve üçlü ortamları destekler. Bu makale _ana_ ve _bağımlı_ sözcüklere başvurular içerir. Kullanım açısından [ücretsiz iletişim Için Microsoft Stil Kılavuzu](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) , bu sözcükleri exclusionary kelimeleri olarak tanır. Bu makalede, şu anda yazılımda görüntülenen sözcükler olduklarından, bu makale tutarlılık için kullanılır. Yazılım, kelimeleri kaldırmak üzere güncelleniyorsa, bu makale hizalamayla olacak şekilde güncelleştirilir.
>

## <a name="what-update-is-going-to-happen"></a>Hangi güncelleştirme gerçekleşecektir?

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından oluşturulan yerel bir sertifika dosyası kullanır. Şu anda müşteriler, [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)yer alan bir MySQL Için Azure veritabanı sunucusuna bağlanmak üzere önceden tanımlanmış sertifikayı kullanabilir. Bununla birlikte, [sertifika yetkilisi (CA) tarayıcı Forumu](https://cabforum.org/),   CA satıcıları tarafından verilen birden çok sertifikanın uyumsuz olması için en son yayımlanmış raporlar.

Sektörün uyumluluk gereksinimlerine göre, CA satıcıları uyumlu olmayan CA 'Lar için CA sertifikalarını iptal etmeyi, sunucuların uyumlu CA 'lar tarafından verilen sertifikaları kullanmalarını ve bu uyumlu CA 'lardan CA sertifikaları kullanmasını gerektirir. MySQL için Azure veritabanı şu anda bu uyumlu olmayan sertifikalardan birini kullandığından, istemci uygulamalarının SSL bağlantılarını doğrulamak için kullandığı, MySQL sunucularınızda olası etkileri en aza indirmek için uygun eylemlerin (Bu konunun ilerleyen kısımlarında açıklanmıştır) yapıldığından emin olmamız gerekir.

Yeni sertifika 15 Şubat 2021 tarihinden itibaren kullanılacaktır (02/15/2021). Bir MySQL istemcisinden bağlanırken (sslmode = Verify-CA veya sslmode = Verify-Full), CA doğrulaması veya sunucu sertifikasının tam doğrulamasını kullanırsanız, uygulama yapılandırmanızı 15 Şubat 2021 tarihinden önce güncelleştirmeniz gerekir (03/15/2021).

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Nasıl yaparım? Veritabanımın etkilenip etkilenmediğini öğrensin mi?

SSL/TLS kullanan tüm uygulamalar ve kök sertifikanın kök sertifikayı güncelleştirmesi gerekir. Bağlantı dizenizi inceleyerek bağlantılarınızın kök sertifikayı doğrulayıp doğrulamayıp belirleyemeyeceğini belirleyebilirsiniz.

* Bağlantı dizeniz veya içeriyorsa `sslmode=verify-ca` `sslmode=verify-identity` , sertifikayı güncelleştirmeniz gerekir.
* Bağlantı dizeniz,, `sslmode=disable` veya içerdiğinde, `sslmode=allow` `sslmode=prefer` `sslmode=require` sertifikaları güncelleştirmeniz gerekmez.
* Java bağlayıcıları kullanıyorsanız ve bağlantı dizeniz useSSL = false veya requireSSL = false içeriyorsa, sertifikaları güncelleştirmeniz gerekmez.
* Bağlantı dizeniz sslmode 'u belirtmezse, sertifikaları güncelleştirmeniz gerekmez.

Bağlantı dizesini soyutlayan bir istemci kullanıyorsanız, sertifikaları doğrulayıp doğrulamadığını anlamak için istemci belgelerini gözden geçirin.
MySQL için Azure veritabanı sslkipini anlamak için [SSL modu açıklamalarını](concepts-ssl-connection-security.md#ssl-default-settings)gözden geçirin.

Sertifikaların beklenmedik şekilde iptal edilmesine veya iptal edilmiş bir sertifikayı güncelleştirmeye yönelik olarak kesintiye uğratılmasını önlemek için, [**"bağlantıyı sürdürmek Için ne yapmam gerekir?"**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) bölümüne bakın.

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Bağlantıyı sürdürmek için ne yapmam gerekir?

Sertifikaların beklenmedik şekilde iptal edildiği veya iptal edilen bir sertifikayı güncelleştirme nedeniyle kesintiye uğramasını önlemek için aşağıdaki adımları kullanın. Bu düşünce, geçerli sertifikayı ve yeni birini birleştiren yeni bir *. pem* dosyası OLUŞTURMAKTıR ve SSL sertifika doğrulaması sırasında izin verilen değerlerden biri kullanılacaktır. Aşağıdaki adımlara bakın:

* Aşağıdaki bağlantılardan BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 kök CA 'sını indirin:

  * [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)
  * [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)

* Hem **Baltimorecyıbertrustroot** hem de **DigiCertGlobalRootG2** sertifikaları dahil olmak üzere bir birleştirilmiş CA sertifika deposu oluşturun.

  * Java (MySQL Connector/J) kullanıcıları için şunu yürütün:

    ```console
    keytool -importcert -alias MySQLServerCACert -file D:\BaltimoreCyberTrustRoot.crt.pem -keystore truststore -storepass password -noprompt
    ```

    ```console
    keytool -importcert -alias MySQLServerCACert2 -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password -noprompt
    ```

    Ardından orijinal anahtar deposu dosyasını yeni oluşturulan yeni bir anahtar ile değiştirin:

    * System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file");
    * System. setProperty ("javax. net. SSL. trustStorePassword", "Password");

  * .NET (MySQL Connector/NET, MySQLConnector) kullanıcıları için, Windows sertifika deposunda, güvenilen kök sertifika yetkililerinde **Baltimorecyıbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifikayı içeri aktarın.

        ![Azure Database for MySQL .net cert](media/overview/netconnecter-cert.png)

  * Linux üzerinde SSL_CERT_DIR kullanarak .NET kullanıcıları için, SSL_CERT_DIR tarafından belirtilen dizinde **Baltimorecrivbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifika dosyasını oluşturun.

  * Diğer (MySQL Client/MySQL çalışma ekranı/C/C++/Go/Python/Ruby/PHP/NodeJS/Perl/Swift) kullanıcıları için iki CA sertifika dosyasını aşağıdaki biçimde birleştirebilirsiniz:</b>

     </br>-----BAŞLANGıÇ SERTIFIKASı-----  </br>(Root CA1: BaltimoreCyberTrustRoot. CRT. pek)  </br>-----SON SERTIFIKA-----  </br>-----BAŞLANGıÇ SERTIFIKASı-----  </br>(Root CA2: DigiCertGlobalRootG2. CRT. pek)  </br>-----SON SERTIFIKA-----

* Özgün kök CA ped dosyasını birleştirilmiş kök CA dosyası ile değiştirin ve uygulamanızı/istemcinizi yeniden başlatın.
* Daha sonra, yeni sertifika sunucu tarafında dağıtıldıktan sonra, CA PEI dosyanızı DigiCertGlobalRootG2. CRT. ped olarak değiştirebilirsiniz.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Sertifikayı güncelleştirmeden ne etkisi olabilir?

MySQL için Azure veritabanı verilen sertifikayı burada belgelenen şekilde kullanıyorsanız, veritabanının ulaşılamaz olması nedeniyle uygulamanızın kullanılabilirliği kesintiye uğramış olabilir. Uygulamanıza bağlı olarak, aşağıdakiler dahil, ancak bunlarla sınırlı olmamak üzere çeşitli hata iletileri alabilirsiniz:

* Geçersiz sertifika/iptal edilmiş sertifika
* Bağlantı zaman aşımına uğradı

> [!NOTE]
> Lütfen sertifika değişikliği yapılıncaya kadar **Baltidaha fazla sertifikayı** kaldırmayın veya değiştirmeyin. Değişiklik yapıldıktan sonra, Baltidaha fazla sertifikayı bırakması için güvenli olacak şekilde bir iletişim göndereceğiz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="1-if-im-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. SSL/TLS kullanmıyorsanız yine de kök CA 'yı güncelleştirmem gerekir mi?

  SSL/TLS kullanmıyorsanız hiçbir eylem gerekmez.

### <a name="2-if-im-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. SSL/TLS kullanıyorum, kök CA 'yı güncelleştirmek için veritabanı sunucusunu yeniden başlatmem gerekir mi?

Hayır, yeni sertifikayı kullanmaya başlamak için veritabanı sunucusunu yeniden başlatmanız gerekmez. Bu kök sertifika, istemci tarafı bir değişiklik ve gelen istemci bağlantılarının veritabanı sunucusuna bağlanabildiklerinden emin olmak için yeni sertifikayı kullanması gerekir.

### <a name="3-what-will-happen-if-i-dont-update-the-root-certificate-before-february-15-2021-02152021"></a>3. kök sertifikayı 15 Şubat 2021 ' den önce güncelleştirdiğimde ne olur? (02/15/2021)?

Kök sertifikayı 15 Şubat 2021 (02/15/2021) tarihinden önce güncelleştirmemeniz durumunda, SSL/TLS aracılığıyla bağlanan ve kök sertifika için doğrulama yapan uygulamalarınız MySQL veritabanı sunucusuyla iletişim kuramaz ve uygulama MySQL veritabanı sunucunuza bağlantı sorunlarıyla karşılaşacaktır.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-mysql"></a>4. MySQL için Azure veritabanı ile App Service kullanılıyorsa ne etkiler?

MySQL için Azure veritabanı 'na bağlanan Azure Uygulama Hizmetleri için, uygulama ile SSL kullanma yönteminize bağlı olarak iki olası senaryo vardır.

* Bu yeni sertifika platform düzeyinde App Service eklendi. Uygulamanızdaki App Service platforma dahil olan SSL sertifikalarını kullanıyorsanız hiçbir işlem gerekmez.
* Kodunuzda SSL sertifika dosyasının yolunu açıkça dahil ediyorsanız, yeni sertifikayı indirmeniz ve kodu yeni sertifikayı kullanacak şekilde güncelleştirmeniz gerekir. Bu senaryonun iyi bir örneği, [App Service belgelerinde](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress) paylaşıldığından App Service özel kapsayıcılar kullandığınızda oluşur

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-mysql"></a>5. MySQL için Azure veritabanı ile Azure Kubernetes Hizmetleri (AKS) kullanılıyorsa etkisi nedir?

Azure Kubernetes Services (AKS) kullanarak MySQL için Azure veritabanı 'na bağlanmaya çalışıyorsanız, adanmış bir müşterilerin ana bilgisayar ortamından erişime de benzer. [Buradaki](../aks/ingress-own-tls.md)adımlara bakın.

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-mysql"></a>6. MySQL için Azure veritabanı 'na bağlanmak üzere Azure Data Factory kullanıyorsanız etkisi nedir?

Bağlayıcı, Azure Integration Runtime kullanan bir bağlayıcı için, Azure 'da barındırılan ortamdaki Windows sertifika deposundaki sertifikalar üzerinden faydalanır. Bu sertifikalar zaten yeni uygulanan sertifikalarla uyumludur ve bu nedenle herhangi bir eylem gerekmez.

Şirket içinde barındırılan Integration Runtime kullanan bir bağlayıcı için, bağlantı dizinizdeki SSL sertifikası dosyasının yolunu açıkça eklediğiniz yerde, [yeni sertifikayı](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) indirmeniz ve bağlantı dizesini kullanmak için güncelleştirmeniz gerekir.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. bu değişiklik için bir veritabanı sunucusu bakım kapalı kalma süresi planlamanız gerekir mi?

Hayır. Buradaki değişiklik yalnızca istemci tarafında, veritabanı sunucusuna bağlanmak için olduğundan, bu değişiklik için veritabanı sunucusu için gerekli bakım kapalı kalma süresi yoktur.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-february-15-2021-02152021"></a>8. bu değişiklik için 15 Şubat 2021 tarihinden önce zamanlanan kapalı kalma süresi (02/15/2021) yoksa ne olur?

Sunucuya bağlanmak için kullanılan istemcilerin, [buradaki](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity)çözüm bölümünde açıklandığı gibi sertifika bilgilerini güncelleştirmesi gerektiğinden, bu durumda sunucu için kapalı kalma süresine gerek kalmaz.

### <a name="9-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>9.15 Şubat 2021 (02/15/2021) sonrasında yeni bir sunucu oluştururum, bundan etkilenecek mıyım?

15 Şubat 2021 (02/15/2021) ' den sonra oluşturulan sunucular için, SSL kullanarak bağlanmak üzere uygulamalarınız için yeni verilen sertifikayı kullanabilirsiniz.

### <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. Microsoft sertifikalarını ne sıklıkla güncelleştiriyor veya süre sonu ilkesi nedir?

MySQL için Azure veritabanı tarafından kullanılan bu sertifikalar, güvenilen sertifika yetkilileri (CA) tarafından sağlanır. Bu nedenle, MySQL için Azure veritabanı 'nda bu sertifikaların desteklenmesi, CA tarafından bu sertifikaların desteğine bağlıdır. Bununla birlikte, bu örnekte olduğu gibi, önceden tanımlanmış bu sertifikalarda, en erken düzeltilmelidir.

### <a name="11-if-im-using-read-replicas-do-i-need-to-perform-this-update-only-on-source-server-or-the-read-replicas"></a>11. okuma çoğaltmaları kullanıyorsam, bu güncelleştirmeyi yalnızca kaynak sunucuda veya okuma çoğaltmalarıyla gerçekleştirmem gerekir mi?

Bu güncelleştirme istemci tarafı değişikliği olduğundan, istemci Çoğaltma sunucusundan veri okumak için kullanılıyorsa, bu istemciler için değişiklikleri de uygulamanız gerekir.

### <a name="12-if-im-using-data-in-replication-do-i-need-to-perform-any-action"></a>12. veri çoğaltma 'yı kullanıyorsam herhangi bir eylem gerçekleştirmem gerekir mi?

MySQL için Azure veritabanı 'na bağlanmak üzere [veri çoğaltma](concepts-data-in-replication.md) 'yı kullanıyorsanız, göz önünde bulundurmanız gereken iki şey vardır:

* Veri çoğaltma bir sanal makineden (Şirket içi veya Azure sanal makinesi) MySQL için Azure veritabanı 'na ait ise, çoğaltmayı oluşturmak için SSL 'nin kullanıldığını denetlemeniz gerekir. **BAĞıMLı durumu göster** ' i çalıştırın ve aşağıdaki ayarı denetleyin.  

    ```azurecli-interactive
    Master_SSL_Allowed            : Yes
    Master_SSL_CA_File            : ~\azure_mysqlservice.pem
    Master_SSL_CA_Path            :
    Master_SSL_Cert               : ~\azure_mysqlclient_cert.pem
    Master_SSL_Cipher             :
    Master_SSL_Key                : ~\azure_mysqlclient_key.pem
    ```

    CA_file, SSL_Cert ve SSL_Key için sertifikanın sağlandığını görürseniz, [yeni sertifikayı](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)ekleyerek dosyayı güncelleştirmeniz gerekir.

* Veri çoğaltma, MySQL için iki Azure veritabanı arasındaysa, **çağrı MySQL.az_replication_change_master** yürüterek çoğaltmayı sıfırlamanız ve son parametre olarak yeni ikili kök sertifikayı sağlamanız gerekir [master_ssl_ca](howto-data-in-replication.md#link-source-and-replica-servers-to-start-data-in-replication)

### <a name="13-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>13. SSL 'nin kullanıldığını doğrulamak için sunucu tarafı sorgumuz var mı?

Sunucuya bağlanmak için SSL bağlantısı kullanıp kullandığınızı doğrulamak için [SSL doğrulaması](howto-configure-ssl.md#step-4-verify-the-ssl-connection)' na başvurun.

### <a name="14-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>14. sertifika dosyasında DigiCertGlobalRootG2 zaten varsa, gerekli bir eylem var mı?

Hayır. Sertifika dosyanızda zaten **DigiCertGlobalRootG2** varsa herhangi bir işlem yapmanız gerekmez.

###    <a name="15-what-if-i-have-further-questions"></a>15. daha fazla sorunuz varsa ne yapmalıyım?

Sorularınız varsa, [Microsoft Q&A](mailto:AzureDatabaseforMySQL@service.microsoft.com)'daki topluluk uzmanlarının yanıtlarını alın. Destek planınız varsa ve teknik yardıma ihtiyacınız varsa [bizimle iletişime geçin](mailto:AzureDatabaseforMySQL@service.microsoft.com).
