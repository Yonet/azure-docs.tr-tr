---
title: 'Azure AD Connect: eşitleme sırasında hata giderme | Microsoft Docs'
description: Azure AD Connect eşitleme sırasında karşılaşılan hatalarla ilgili sorunları nasıl giderebileceğinizi açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/29/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88fda4ec810d0b410dcd75ac9c6be69bd54b16d9
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092659"
---
# <a name="troubleshooting-errors-during-synchronization"></a>Eşitleme sırasında karşılaşılan Hataları giderme
Kimlik verileri, Windows Server Active Directory (AD DS) Azure Active Directory (Azure AD) ile eşitlendiğinde hatalar oluşabilir. Bu makalede, farklı eşitleme hatası türlerine genel bir bakış sağlanır. bu hatalara neden olan olası senaryoların bazıları ve hataları gidermeye yönelik olası yollar sunulmaktadır. Bu makale, ortak hata türlerini içerir ve olası tüm hataları kapsamayabilir.

 Bu makalede, okuyucunun [Azure AD ve Azure AD Connect temel alınan tasarım kavramlarını](plan-connect-design-concepts.md)öğrenildiği varsayılmaktadır.

Azure AD Connect \( ağustos 2016 veya üzeri en son sürümü ile eşitleme \) hatalarının bir raporu, eşitleme için Azure AD Connect Health bir parçası olarak [Azure Portal](https://aka.ms/aadconnecthealth) sunulmaktadır.

1 Eylül 2016 ' den itibaren [Azure Active Directory yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md) özelliği, tüm *Yeni* Azure Active Directory kiracılar için varsayılan olarak etkinleştirilir. Bu özellik yaklaşan aylarda mevcut kiracılar için otomatik olarak etkinleştirilecek.

Azure AD Connect eşitlemede devam eden dizinlerden üç tür işlem gerçekleştirir: Içeri aktar, Eşitle ve dışarı aktar. Hatalar tüm işlemlerde meydana getirebilir. Bu makale, genellikle Azure AD 'ye aktarma sırasında oluşan hatalara odaklanır.

## <a name="errors-during-export-to-azure-ad"></a>Azure AD 'ye aktarma sırasında hatalar
Aşağıdaki bölümde Azure AD bağlayıcısı kullanılarak Azure AD 'ye dışarı aktarma işlemi sırasında oluşabilecek farklı eşitleme hatası türleri açıklanmaktadır. Bu bağlayıcı ad biçimi "contoso" ile tanımlanabilir. *onmicrosoft.com*".
Azure AD 'ye dışarı aktarma sırasında oluşan hatalar, \( \) \( Azure Active Directory Azure AD Connect eşitleme altyapısı tarafından başarısız olan ekleme, güncelleştirme, silme vs. işlemi \) başarısız olduğunu gösterir.

![Dışarı aktarma hataları genel bakış](./media/tshoot-connect-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Veri uyumsuzluğu hataları
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Description
* Azure AD Connect \( Sync Engine \) , nesneleri eklemek veya güncelleştirmek için Azure Active Directory bildirir; Azure AD, Azure AD 'Deki nesnelerin **ImmutableID** özniteliğiyle **sourcetutturucu** özniteliğini kullanarak gelen nesneyle eşleşir. Bu eşleştirmeye **sabit eşleşme** denir.
* Azure AD, **ImmutableID** **özniteliğiyle eşleşen** herhangi bir nesne **bulmazsa** , yeni bir nesne sağlamadan önce, bir eşleşme bulmak için proxyAddresses ve userPrincipalName özniteliklerini kullanmaya geri döner. Bu eşleştirmeye, **yumuşak eşleşme** denir. Hafif eşleşme, şirket içinde aynı varlığı (kullanıcılar, gruplar) temsil eden eşitleme sırasında eklenen/güncellenen yeni nesneler ile Azure AD 'de zaten mevcut olan (Azure AD 'de bulunan) nesneleri eşleştirmek için tasarlanmıştır.
* Sabit eşleşme eşleşen bir nesne bulamazsa **ve** yumuşak eşleşme eşleşen bir nesne bulduğunda **InvalidSoftMatch** hatası oluşur, ancak bu nesnenin gelen nesnenin *Sourcetutturucu* öğesinden farklı bir *ImmutableID* değeri varsa, eşleşen nesnenin şirket içi Active Directory başka bir nesneyle eşitlenmiş olması önerilir.

Diğer bir deyişle, yumuşak eşleşmenin çalışması için, ile ile eşleşen nesnenin *ImmutableID* için herhangi bir değere sahip olmaması gerekir. Bir değeri olan *ImmutableID* ile ayarlanmış herhangi bir nesne sabit eşleşme başarısız olursa ancak yumuşak eşleşme ölçütlerini karşılarken, Işlem InvalidSoftMatch eşitleme hatasına neden olur.

Azure Active Directory şeması iki veya daha fazla nesnenin aşağıdaki özniteliklerle aynı değere sahip olmasını sağlamıyor. \(Bu, kapsamlı bir liste değildir.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* ObjectId

> [!NOTE]
> [Azure ad özniteliği yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md) özelliği, Azure Active Directory varsayılan davranışı olarak da kullanıma alınıyor.  Bu, Azure AD 'yi şirket içi AD ortamlarında bulunan yinelenen ProxyAddresses ve UserPrincipalName özniteliklerini idare yöntemiyle daha dayanıklı hale getirerek Azure AD Connect (diğer eşitleme istemcileri) tarafından görülen eşitleme hatalarının sayısını azaltır. Bu özellik, yineleme hatalarını düzelmez. Bu nedenle verilerin hala düzeltilmesi gerekir. Ancak, Azure AD 'de yinelenen değerler nedeniyle, aksi durumda engellenen yeni nesnelerin sağlanmasını sağlar. Bu, eşitleme istemcisine döndürülen eşitleme hatalarının sayısını da azaltır.
> Kiracınız için bu özellik etkinleştirilirse, yeni nesnelerin sağlanması sırasında görülen InvalidSoftMatch eşitleme hatalarını görmezsiniz.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>InvalidSoftMatch için örnek senaryolar
1. ProxyAddresses özniteliği için aynı değere sahip iki veya daha fazla nesne şirket içi Active Directory var. Azure AD 'de yalnızca bir tane sağlanmakta.
2. UserPrincipalName özniteliği için aynı değere sahip iki veya daha fazla nesne şirket içi Active Directory var. Azure AD 'de yalnızca bir tane sağlanmakta.
3. Active Directory bir nesne, Azure Active Directory özniteliği aynı değere sahip olan şirket içi eklenmiş. Şirket içine eklenen nesne Azure Active Directory sağlanmadı.
4. Bir nesne, Azure Active Directory bir hesabın userPrincipalName özniteliğiyle aynı değere sahip Active Directory Şirket içi olarak eklendi. Nesne Azure Active Directory sağlanmadı.
5. Eşitlenen bir hesap orman A 'dan B ormanına taşındı. Azure AD Connect (Sync Engine), Sourcebağlayıcısını hesaplamak için Objectguıd özniteliğini kullanıyor. Orman taşıdıktan sonra, Sourcebağlayıcının değeri farklıdır. Yeni nesne (B ormanında), Azure AD 'deki mevcut nesneyle eşitlenemeyebilir.
6. Eşitlenmiş bir nesne, şirket içi Active Directory içinden yanlışlıkla silindi ve Azure Active Directory hesabı silinmeden aynı varlık için Active Directory yeni bir nesne (Kullanıcı gibi) oluşturuldu. Yeni hesap mevcut Azure AD nesnesiyle eşitlenemiyor.
7. Azure AD Connect kaldırıldı ve yeniden yüklendi. Yeniden yükleme sırasında, Sourcetutturucu olarak farklı bir öznitelik seçildi. Daha önce eşitlenen tüm nesneler, InvalidSoftMatch hatasıyla eşitlemeyi durdurdu.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith** , *contoso.com* 'in şirket içi Active Directory Azure Active Directory eşitlenen bir Kullanıcı
2. Bob Smith 'in **userPrincipalName** 'i **ebrunun \@ contoso.com** olarak ayarlanmıştır.
3. **"abcdefghgpqrstuv = ="** , Azure Active Directory Içindeki Bob Smith 'In **ImmutableID** 'ı olan, Bob smith 'In şirket içi Active Directory **Objectguıd** 'Si kullanılarak Azure AD Connect tarafından hesaplanan **sourcebağlantıdır** .
4. Bob, **proxyAddresses** özniteliği için de aşağıdaki değerlere sahiptir:
   * SMTP bobs@contoso.com
   * SMTP bob.smith@contoso.com
   * **SMTP: Bob \@ contoso.com**
5. Yeni bir kullanıcı olan **Bob Taylor**, şirket içi Active Directory eklenir.
6. Bob Taylor **userPrincipalName** , **bobt \@ contoso.com** olarak ayarlanmıştır.
7. **"abcdefghijkl0123456789 = =" "** , Azure AD Connect tarafından, şirket içi Active Directory 'ın Bob Taylor **Objectguıd** 'ı kullanılarak hesaplanan **sourcetutturucu** . Bob Taylor nesnesi henüz Azure Active Directory ile eşitlenmedi.
8. Bob Taylor proxyAddresses özniteliği için aşağıdaki değerlere sahiptir
   * SMTP bobt@contoso.com
   * SMTP bob.taylor@contoso.com
   * **SMTP: Bob \@ contoso.com**
9. Eşitleme sırasında Azure AD Connect, şirket içi Active Directory Bob Taylor eklenmesini algılar ve Azure AD 'nin aynı değişikliği yapmasını ister.
10. Azure AD, önce sabit eşleşme gerçekleştirecek. Diğer bir deyişle, ImmutableID ile "abcdefghijkl0123456789 = =" değerine eşit herhangi bir nesne varsa arama yapılır. Azure AD 'de başka hiçbir nesne bu ımutableıd değerine sahip olmadığından, sabit eşleşme başarısız olacak.
11. Daha sonra Azure AD, emre Taylor ile aynı şekilde yararlanmaya çalışacaktır. Yani, SMTP dahil olmak üzere üç değere eşit proxyAddresses sahip herhangi bir nesne varsa arama yapılır: bob@contoso.com
12. Azure AD, Bob Smith 'in nesnesini, yumuşak eşleşme ölçütleriyle eşleşecek şekilde bulacak. Ancak bu nesne ImmutableID = "abcdefghgpqrstuv = =" değerine sahiptir. Bu nesnenin şirket içi Active Directory başka bir nesneden eşitlendiğini belirtir. Bu nedenle, Azure AD bu nesneleri geçici olarak eşleşemez ve bir **InvalidSoftMatch** eşitleme hatası ile sonuçlanır.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>InvalidSoftMatch hatasını çözme
InvalidSoftMatch hatasının en yaygın nedeni, farklı Sourcetutturucu \( ImmutableID ile \) aynı değere sahip olan proxyAddresses ve/veya UserPrincipalName öznitelikleri için aynı değere sahiptir. Geçersiz yumuşak eşleşmeyi onarmak için

1. Hataya neden olan yinelenen proxyAddresses, userPrincipalName veya diğer öznitelik değerini belirler. Ayrıca \( , çakışmaya hangi iki veya daha fazla \) nesne dahil olduğunu da belirleyebilirsiniz. [Eşitleme için Azure AD Connect Health](./how-to-connect-health-sync.md) tarafından oluşturulan rapor, iki nesneyi belirlemenize yardımcı olabilir.
2. Hangi nesnenin yinelenen değere sahip olmaya devam etmesi gerektiğini ve hangi nesnenin olmaması gerektiğini belirler.
3. Yinelenen değeri bu değere sahip OLMAMASı gereken nesneden kaldırın. Değişikliği nesnenin kaynağı olan dizinde yapmanız gerekir. Bazı durumlarda, çakışmada nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD 'de değişikliği yaptıysanız değişikliği Azure AD Connect eşitlemeye izin verin.

Eşitleme için Azure AD Connect Health içindeki eşitleme hata raporları her 30 dakikada bir güncelleştirilir ve en son eşitleme denemesinden alınan hataları içerir.

> [!NOTE]
> Imutableıd, tanım olarak nesnenin kullanım ömrü içinde değişmemelidir. Yukarıdaki listeden aklınızda bulunan bazı senaryolarla yapılandırılmamışsa, Azure AD Connect, kullanmaya devam etmek istediğiniz mevcut bir Azure AD nesnesine sahip olan aynı varlığı (aynı kullanıcı/grup/ilgili kişi vb.) temsil eden AD nesnesi için Sourcebağlayıcının farklı bir değerini hesapladığı bir durumda Azure AD Connect.
>
>

#### <a name="related-articles"></a>İlgili Makaleler
* [Yinelenen veya geçersiz öznitelikler Microsoft 365 dizin eşitlemesini engelliyor](https://support.microsoft.com/kb/2647098)

### <a name="objecttypemismatch"></a>Objecttypeuyuşmazlık
#### <a name="description"></a>Description
Azure AD, iki nesneyle hafif bir şekilde eşleşmek istediğinde, farklı "nesne türü" (örneğin, Kullanıcı, Grup, kişi vb.) iki nesnesinin, yumuşak eşleşme gerçekleştirmek için kullanılan öznitelikler için aynı değerlere sahip olması mümkündür. Azure AD 'de bu özniteliklerin çoğaltılmasına izin verilmemesine neden olarak, işlem "Objecttypeuyuşmazlıkla" eşitleme hatasına neden olabilir.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Objecttypeuyuşmazlık hatası için örnek senaryolar
* Microsoft 365 ' de posta etkin bir güvenlik grubu oluşturulur. Yönetici, Microsoft 365 grubunun ProxyAddresses özniteliği için aynı değere sahip şirket içi AD 'de (henüz Azure AD ile eşitlenmemiş) yeni bir kullanıcı veya kişi ekler.

#### <a name="example-case"></a>Örnek durum
1. Yönetici, vergi departmanı için Microsoft 365 yeni posta etkin bir güvenlik grubu oluşturur ve farklı bir e-posta adresi sağlar tax@contoso.com . Bu gruba SMTP 'nin ProxyAddresses öznitelik değeri atandı **: tax \@ contoso.com**
2. Yeni bir Kullanıcı Contoso.com birleştirir ve şirket içi kullanıcı için bir hesap oluşturulur: proxyAddress as **SMTP: tax \@ contoso.com**
3. Azure AD Connect Yeni Kullanıcı hesabını eşitlecektir, "Objecttypeuyuşmazlığını" hatasını alır.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Objecttypeuyuşmazlığını çözme hatası
Objecttypeuyuşmazlık hatasının en yaygın nedeni, farklı türde (Kullanıcı, Grup, kişi vb.) iki nesne olan ProxyAddresses özniteliği için aynı değere sahiptir. Objecttypeuyuşmazlığını onarmak için:

1. Hataya neden olan yinelenen proxyAddresses (veya diğer öznitelik) değerini belirler. Ayrıca \( , çakışmaya hangi iki veya daha fazla \) nesne dahil olduğunu da belirleyebilirsiniz. [Eşitleme için Azure AD Connect Health](./how-to-connect-health-sync.md) tarafından oluşturulan rapor, iki nesneyi belirlemenize yardımcı olabilir.
2. Hangi nesnenin yinelenen değere sahip olmaya devam etmesi gerektiğini ve hangi nesnenin olmaması gerektiğini belirler.
3. Yinelenen değeri bu değere sahip OLMAMASı gereken nesneden kaldırın. Değişikliği nesnenin kaynağı olan dizinde yapmanız gerektiğini unutmayın. Bazı durumlarda, çakışmada nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD 'de değişikliği yaptıysanız değişikliği Azure AD Connect eşitlemeye izin verin. Eşitleme için Azure AD Connect Health içindeki eşitleme hata raporu 30 dakikada bir güncelleştirilir ve en son eşitleme denemesinden alınan hataları içerir.

## <a name="duplicate-attributes"></a>Yinelenen öznitelikler
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Description
Azure Active Directory şeması iki veya daha fazla nesnenin aşağıdaki özniteliklerle aynı değere sahip olmasını sağlamıyor. Bu, Azure AD 'deki her bir nesne belirli bir örnekte bu özniteliklerin benzersiz bir değerine sahip olmaya zorlanır.

* ProxyAddresses
* UserPrincipalName

Azure AD Connect yeni bir nesne eklemeye veya var olan bir nesneyi, Azure Active Directory daha önce başka bir nesneye atanmış yukarıdaki özniteliklerin değeri ile güncelleştirmeye çalışırsa, işlem "AttributeValueMustBeUnique" eşitleme hatasına neden olur.

#### <a name="possible-scenarios"></a>Olası senaryolar:
1. Yinelenen değer zaten eşitlenmiş bir nesneye atandı ve bu, eşitlenen başka bir nesneyle çakışıyor.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith** , contoso.com 'in şirket içi Active Directory Azure Active Directory eşitlenen bir Kullanıcı
2. Bob Smith 'in şirket içi **userPrincipalName** 'i **ebrunun \@ contoso.com** olarak ayarlanmıştır.
3. Bob, **proxyAddresses** özniteliği için de aşağıdaki değerlere sahiptir:
   * SMTP bobs@contoso.com
   * SMTP bob.smith@contoso.com
   * **SMTP: Bob \@ contoso.com**
4. Yeni bir kullanıcı olan **Bob Taylor**, şirket içi Active Directory eklenir.
5. Bob Taylor **userPrincipalName** , **bobt \@ contoso.com** olarak ayarlanmıştır.
6. **Bob Taylor** , **proxyAddresses** özniteliği ı için aşağıdaki değerlere sahiptir. SMTP: bobt@contoso.com II. SMTP bob.taylor@contoso.com
7. Bob Taylor nesnesi Azure AD ile başarıyla eşitlendi.
8. Yönetici Bob Taylor **proxyAddresses** özniteliğini şu değerle güncelleştirmeye karar verdi: i. **SMTP: Bob \@ contoso.com**
9. Azure AD, Azure AD 'de Bob Taylor nesnesini yukarıdaki değerle güncelleştirmeye çalışacak, ancak bu ProxyAddresses değeri zaten Bob Smith 'e atandığından, "AttributeValueMustBeUnique" hatasına neden olacak şekilde bu işlem başarısız olur.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>AttributeValueMustBeUnique hatasını çözme
AttributeValueMustBeUnique hatasının en yaygın nedeni, farklı Sourcebağlayıcısını ImmutableID olan iki nesne \( \) proxyAddresses ve/veya UserPrincipalName öznitelikleri için aynı değere sahiptir. AttributeValueMustBeUnique hatasını onarmak için

1. Hataya neden olan yinelenen proxyAddresses, userPrincipalName veya diğer öznitelik değerini belirler. Ayrıca \( , çakışmaya hangi iki veya daha fazla \) nesne dahil olduğunu da belirleyebilirsiniz. [Eşitleme için Azure AD Connect Health](./how-to-connect-health-sync.md) tarafından oluşturulan rapor, iki nesneyi belirlemenize yardımcı olabilir.
2. Hangi nesnenin yinelenen değere sahip olmaya devam etmesi gerektiğini ve hangi nesnenin olmaması gerektiğini belirler.
3. Yinelenen değeri bu değere sahip OLMAMASı gereken nesneden kaldırın. Değişikliği nesnenin kaynağı olan dizinde yapmanız gerektiğini unutmayın. Bazı durumlarda, çakışmada nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD 'de değişikliği yaptıysanız, hata değişikliğini Azure AD Connect.

#### <a name="related-articles"></a>İlgili Makaleler
-[Yinelenen veya geçersiz öznitelikler Microsoft 365 dizin eşitlemesini engelliyor](https://support.microsoft.com/kb/2647098)

## <a name="data-validation-failures"></a>Veri doğrulama sorunları
### <a name="identitydatavalidationfailed"></a>Identitydatavalidationfailed
#### <a name="description"></a>Description
Azure Active Directory, verilerin dizine yazılmasına izin vermeden önce verilerin kendisinde çeşitli kısıtlamalar uygular. Bu kısıtlamalar, son kullanıcıların bu verilere bağlı uygulamaları kullanırken olası en iyi deneyimleri almasını sağlamaktır.

#### <a name="scenarios"></a>Senaryolar
a. UserPrincipalName özniteliği değeri geçersiz/desteklenmeyen karakterler içeriyor.
b. UserPrincipalName özniteliği gerekli biçime uymuyor.

#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Identitydatavalidationfailed hatası nasıl düzeltilir?
a. UserPrincipalName özniteliğinde desteklenen karakterlerin ve gerekli biçimin bulunduğundan emin olun.

#### <a name="related-articles"></a>İlgili Makaleler
* [Microsoft 365 için Dizin eşitleme yoluyla Kullanıcı sağlamaya hazırlanma](https://support.office.com/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Description
Bu durum, bir kullanıcının UserPrincipalName öğesinin soneki bir Federasyon etki alanından başka bir Federasyon etki alanına değiştirildiğinde **"Federateddomainchangeerror"** eşitleme hatası ile sonuçlanır.

#### <a name="scenarios"></a>Senaryolar
Eşitlenmiş bir kullanıcı için, UserPrincipalName soneki bir Federasyon etki alanından şirket içi diğer bir Federasyon etki alanına değiştirilmiştir. Örneğin, *userPrincipalName = bob \@ contoso.com* , *userPrincipalName = Bob \@ fabrikam.com* olarak değiştirilmiştir.

#### <a name="example"></a>Örnek
1. Contoso.com için bir hesap olan Bob Smith, UserPrincipalName ile Active Directory yeni bir kullanıcı olarak eklendi bob@contoso.com
2. Bob, Fabrikam.com adlı farklı bir Contoso.com bölümüne gider ve UserPrincipalName özniteliği olarak değiştirilir bob@fabrikam.com
3. Hem contoso.com hem de fabrikam.com etki alanları Azure Active Directory olan Federasyon etki alanlardır.
4. Bob 'un userPrincipalName 'i güncelleştirilmemiş ve "FederatedDomainChangeError" eşitleme hatası ile sonuçlanıyor.

#### <a name="how-to-fix"></a>Nasıl düzeltilir
Bir kullanıcının UserPrincipalName soneki bob@**contoso.com** 'den Bob \@ **fabrikam.com**'e güncelleştirilirse, her ikisi de **contoso.com** ve **fabrikam.com** **federe etki alanlarındaysa**, eşitleme hatasını onarmak için bu adımları izleyin

1. Azure AD 'de kullanıcının UserPrincipalName değerini bob@contoso.com olarak güncelleştirin bob@contoso.onmicrosoft.com . Azure AD PowerShell modülü ile aşağıdaki PowerShell komutunu kullanabilirsiniz: `Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Eşitlemeyi denemek için bir sonraki eşitleme döngüsüne izin verin. Bu zaman eşitleme başarılı olur ve Bob 'un UserPrincipalName değerini bob@fabrikam.com beklenen şekilde güncelleştirir.

#### <a name="related-articles"></a>İlgili Makaleler
* [Kullanıcı hesabının UPN 'sini farklı bir Federasyon etki alanı kullanacak şekilde değiştirdikten sonra değişiklikler Azure Active Directory eşitleme aracı tarafından eşitlenmedi](/azure/active-directory/hybrid/howto-troubleshoot-upn-changes)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Description
Bir öznitelik izin verilen boyut sınırını, uzunluk sınırını veya Azure Active Directory şeması tarafından ayarlanan sayı sınırını aştığında, eşitleme işlemi **LargeObject** veya **Exceededallodilimlength** eşitleme hatası ile sonuçlanır. Bu hata, genellikle aşağıdaki öznitelikler için oluşur

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Olası senaryolar
1. Bob 'un userCertificate özniteliği, Bob 'a atanan çok sayıda sertifikayı depoluyor. Bunlar, eski, son kullanma sertifikaları içerebilir. Sabit sınır 15 sertifikadır. Kullanıcısertifikası özniteliğiyle LargeObject hatalarının nasıl işleneceği hakkında daha fazla bilgi için lütfen [userCertificate özniteliğinin neden olduğu LargeObject hatalarını işleme](tshoot-connect-largeobjecterror-usercertificate.md)makalesine başvurun.
2. Bob 'un Usersmmecertificate özniteliği, Bob 'a atanan çok sayıda sertifikayı depoluyor. Bunlar, eski, son kullanma sertifikaları içerebilir. Sabit sınır 15 sertifikadır.
3. Bob 'un Active Directory thumbnailPhoto kümesi, Azure AD 'de eşitlenecek kadar büyük.
4. Active Directory ProxyAddresses özniteliğinin otomatik popülasyonu sırasında, bir nesnenin çok fazla ProxyAddresses atanmış olması gerekir.

### <a name="how-to-fix"></a>Nasıl düzeltilir
1. Hataya neden olan özniteliğin izin verilen sınırlamanın içinde olduğundan emin olun.

## <a name="existing-admin-role-conflict"></a>Var olan yönetici rolü çakışması

### <a name="description"></a>Description
Kullanıcı nesnesi şu olduğunda, eşitleme sırasında bir kullanıcı nesnesi üzerinde **var olan bir yönetici rolü çakışması** oluşur:

- yönetim izinleri ve
- Mevcut bir Azure AD nesnesi ile aynı UserPrincipalName

Azure AD Connect, şirket içi AD 'den bir kullanıcı nesnesiyle atanmış bir yönetici rolü olan bir kullanıcı nesnesiyle yazılım ile eşleştirme yapılmasına izin verilmez.  Daha fazla bilgi için bkz. [Azure AD userPrincipalName popülasyonu](plan-connect-userprincipalname.md)

![Var olan yönetici](media/tshoot-connect-sync-errors/existingadmin.png)


### <a name="how-to-fix"></a>Nasıl düzeltilir
Bu sorunu gidermek için aşağıdakileri yapın:

1. Azure AD hesabını (Owner) tüm yönetici rollerinden kaldırın. 
2. Bulutta karantinaya alınan nesneyi **kalıcı olarak silin** . 
3. Sonraki eşitleme döngüsünün, şirket içi kullanıcıyla bulut hesabıyla (bulut kullanıcısı artık küresel bir GA olmadığından) geçici olarak eşleşen bir işlem ele alınacaktır. 
4. Sahip için rol üyeliklerini geri yükleyin. 

>[!NOTE]
>Şirket içi kullanıcı nesnesi ile Azure AD Kullanıcı nesnesi arasındaki yumuşak eşleşmesinden sonra, mevcut kullanıcı nesnesine yönetici rolünü atayabilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar
* [Active Directory Yönetim Merkezi Active Directory nesneleri bulma](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560661(v=ws.10))
* [Azure Active Directory PowerShell kullanarak bir nesne için Azure Active Directory sorgulama](/previous-versions/azure/jj151815(v=azure.100))