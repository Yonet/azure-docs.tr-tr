---
title: Azure AD 'de bulut eşitlemesi Azure AD Connect önkoşulları
description: Bu makalede, bulut eşitlemesi için ihtiyaç duyduğunuz ön koşullar ve donanım gereksinimleri açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/11/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b83c9b0ece933ad71810c50e89ae296aa218ec75
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98614153"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-sync"></a>Azure AD Connect bulut eşitleme önkoşulları
Bu makale, kimlik çözümünüz olarak Azure Active Directory (Azure AD) bulut eşitlemesini bağlama ve kullanma hakkında rehberlik sağlar.

## <a name="cloud-provisioning-agent-requirements"></a>Bulut sağlama Aracısı gereksinimleri
Bulut eşitlemesini Azure AD Connect kullanmak için aşağıdakiler gerekir:

- Aracı hizmetini çalıştırmak için Azure AD Connect Cloud Sync gMSA (grup yönetilen hizmet hesabı) oluşturmak için etki alanı yöneticisi veya kuruluş yöneticisi kimlik bilgileri. 
- Azure AD kiracınız için konuk kullanıcı olmayan bir karma kimlik yöneticisi hesabı.
- Windows 2012 R2 veya üzeri ile sağlama aracısına yönelik bir şirket içi sunucu.  Bu sunucu, [Active Directory Yönetim Katmanı modelini](/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)temel alan bir katman 0 sunucusu olmalıdır.
- Şirket içi güvenlik duvarı konfigürasyonları.

## <a name="group-managed-service-accounts"></a>Grup Tarafından Yönetilen Hizmet Hesapları
Grup tarafından yönetilen hizmet hesabı, otomatik parola yönetimi, Basitleştirilmiş hizmet asıl adı (SPN) yönetimi, yönetimi diğer yöneticilere devretmek ve ayrıca bu işlevselliği birden çok sunucuya genişleten bir yönetilen etki alanı hesabıdır.  Azure AD Connect bulut eşitlemesi destekler ve aracıyı çalıştırmak için gMSA kullanır.  Bu hesabı oluşturmak için kurulum sırasında yönetici kimlik bilgileri istenir.  Hesap (domain\provAgentgMSA $) olarak görünür.  Bir gMSA hakkında daha fazla bilgi için bkz. [Grup yönetilen hizmet hesapları](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) 

### <a name="prerequisites-for-gmsa"></a>GMSA önkoşulları:
1.  GMSA etki alanı ormanındaki Active Directory şemasının Windows Server 2012 ' e güncelleştirilmesi gerekir
2.  Bir etki alanı denetleyicisindeki [POWERSHELL RSAT modülleri](/windows-server/remote/remote-server-administration-tools)
3.  Etki alanındaki en az bir etki alanı denetleyicisinin Windows Server 2012 çalıştırması gerekir.
4.  Aracının yüklenmekte olduğu etki alanına katılmış bir sunucunun Windows Server 2012 veya üzeri olması gerekir.

### <a name="custom-gmsa-account"></a>Özel gMSA hesabı
Özel bir gMSA hesabı oluşturuyorsanız, hesabın aşağıdaki izinlere sahip olduğundan emin olmanız gerekir.

|Tür |Name |Access |Uygulanan Öğe| 
|-----|-----|-----|-----|
|İzin Ver |gMSA hesabı |Tüm özellikleri oku |Alt cihaz nesneleri| 
|İzin Ver |gMSA hesabı|Tüm özellikleri oku |Alt InetOrgPerson nesneleri| 
|İzin Ver |gMSA hesabı |Tüm özellikleri oku |Alt bilgisayar nesneleri| 
|İzin Ver |gMSA hesabı |Tüm özellikleri oku |Descendant foreignSecurityPrincipal nesneleri| 
|İzin Ver |gMSA hesabı |Tam denetim |Alt grup nesneleri| 
|İzin Ver |gMSA hesabı |Tüm özellikleri oku |Alt Kullanıcı nesneleri| 
|İzin Ver |gMSA hesabı |Tüm özellikleri oku |Alt öğe Iletişim nesneleri| 
|İzin Ver |gMSA hesabı |Kullanıcı nesneleri oluştur/Sil|Bu nesne ve tüm bağımlı nesneler| 

Mevcut bir aracıyı gMSA hesabı kullanmak üzere yükseltmeyle ilgili adımlar için bkz. [Grup yönetilen hizmet hesapları](how-to-install.md#group-managed-service-accounts).

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim merkezinde

1. Azure AD kiracınızda yalnızca bulutta yer alan bir karma kimlik yöneticisi hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olursa veya kullanılamaz hale gelirse kiracınızın yapılandırmasını yönetebilirsiniz. [Yalnızca bulut karma kimlik yöneticisi hesabı ekleme](../fundamentals/add-users-azure-active-directory.md)hakkında bilgi edinin. Bu adımın tamamlanması, kiracınızdan kilitlenmemesini sağlamak açısından önemlidir.
1. Azure AD kiracınıza bir veya daha fazla [özel etki alanı adı](../fundamentals/add-custom-domain.md) ekleyin. Kullanıcılarınız bu etki alanı adlarından biriyle oturum açabilir.

### <a name="in-your-directory-in-active-directory"></a>Active Directory dizininizde

Dizin özniteliklerini eşitlemeye hazırlamak için [ıddüzeltmesini aracını](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) çalıştırın.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. En az 4 GB RAM ve .NET 4.7.1 + çalışma zamanı ile Windows Server 2012 R2 veya üstünü çalıştıran etki alanına katılmış bir konak sunucusu belirler.

2. Yerel sunucudaki PowerShell yürütme ilkesinin tanımsız veya RemoteSigned olarak ayarlanması gerekir.

3. Sunucularınız ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
    - Aracıların Azure AD 'ye aşağıdaki bağlantı noktaları üzerinden *giden* istekler yapabilmeleri için emin olun:

      | Bağlantı noktası numarası | Nasıl kullanılır? |
      | --- | --- |
      | **80** | TLS/SSL sertifikası doğrulanırken sertifika iptal listelerini (CRL 'Ler) indirir.  |
      | **443** | Hizmetle tüm giden iletişimi işler. |
      | **8080** (isteğe bağlı) | Aracılar 443, 8080 numaralı bağlantı noktası kullanılamıyorsa, her 10 dakikada bir bu durumu bağlantı noktası üzerinden raporlar. Bu durum Azure AD portalında görüntülenir. |

    - Güvenlik duvarınız kaynak kullanıcılara göre kuralları zorunlu tutuyorsa ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın.
    - Güvenlik duvarınız veya ara sunucunuz güvenli sonekler belirtmenize izin veriyorsa, \* . msappproxy.net ve. ServiceBus.Windows.net öğesine bağlantı ekleyin \* . Aksi takdirde, haftalık olarak güncellenen [Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişime izin verin.
    - Aracılarınızın ilk kayıt için login.windows.net ve login.microsoftonline.com adreslerine erişmesi gerekir. Bu URL 'Ler için güvenlik duvarınızı da açın.
    - Sertifika doğrulaması için şu URL 'Leri engellemeyi kaldırın: mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 ve www \. Microsoft.com:80. Bu URL'ler diğer Microsoft ürünleriyle sertifika doğrulaması yapmak için kullanılır, dolayısıyla bu URL'lerin engellemesini zaten kaldırmış olabilirsiniz.

    >[!NOTE]
    > Bulut sağlama aracısının Windows Server Core 'a yüklenmesi desteklenmez.

### <a name="additional-requirements"></a>Ek gereksinimler

- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS gereksinimleri

> [!NOTE]
> Aktarım Katmanı Güvenliği (TLS), güvenli iletişimler için sağlayan bir protokoldür. TLS ayarlarının değiştirilmesi ormanın tamamını etkiler. Daha fazla bilgi için bkz. [Windows 'Da WinHTTP 'de varsayılan güvenli protokoller olarak tls 1,1 ve tls 1,2 ' i etkinleştirmek Için güncelleştirme](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

Azure AD Connect Cloud sağlama Aracısı 'nı barındıran Windows Server, yüklemeden önce TLS 1,2 ' i etkinleştirmiş olmalıdır.

TLS 1,2 ' i etkinleştirmek için aşağıdaki adımları izleyin.

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:

    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Sunucuyu yeniden başlatın.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Aşağıda bilinen sınırlamalar verilmiştir:

### <a name="delta-synchronization"></a>Delta Eşitleme

- Delta eşitleme için Grup kapsamı filtrelemesi 1500 taneden fazla üyeyi desteklemez.
- Grup kapsamı filtresinin bir parçası olarak kullanılan bir grubu sildiğinizde, grubun üyesi olan kullanıcılar silinmez. 
- Kapsam içindeki OU veya grubu yeniden adlandırdığınızda, Delta eşitlemesi kullanıcıları kaldırmaz.

### <a name="provisioning-logs"></a>Sağlama Günlükleri
- Sağlama günlükleri, oluşturma ve güncelleştirme işlemleri arasında açıkça ayrım içermez.  Bir güncelleştirme için oluşturma işlemi ve bir oluşturma için güncelleştirme işlemi görebilirsiniz.

### <a name="group-re-naming-or-ou-re-naming"></a>Grup yeniden adlandırma veya OU yeniden adlandırma
- Belirli bir yapılandırma için kapsamda olan AD ' de bir grubu veya OU 'yu yeniden adlandırırsanız, bulut eşitleme işi AD 'deki ad değişikliğini tanıyamaz. İş karantinaya alınmayacak ve sağlıklı olmaya devam edecektir.


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut eşitlemesi nedir?](what-is-cloud-sync.md)