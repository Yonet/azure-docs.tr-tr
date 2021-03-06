---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı hazırlama için Flock yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/30/2019
ms.author: Zhchia
ms.openlocfilehash: 01c3f6429d2a5c8443ac128d763033dc8c53cbc7
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359417"
---
# <a name="tutorial-configure-flock-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Flock yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Flock 'a otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Flock ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Azure AD kiracısı.
* [Flock kiracısı](https://flock.com/pricing/)
* Yönetici izinleriyle Flock 'ta bir kullanıcı hesabı.

## <a name="assigning-users-to-flock"></a>Kullanıcıları Flock 'a atama 

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Flock 'a erişmesi gerektiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Flock 'a atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-flock"></a>Kullanıcıları Flock 'a atamaya yönelik önemli ipuçları 

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için Flock 'a tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Flock 'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="setup-flock--for-provisioning"></a>Sağlama için Flock kurulumu

Azure AD ile otomatik Kullanıcı hazırlama için Flock 'ı yapılandırmadan önce, Flock üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. [Flock](https://web.flock.com/?)'ta oturum açın. **Ayarlar simgesi**  >  **takımınızı yönetme** ' ye tıklayın.

    :::image type="content" source="media/flock-provisioning-tutorial/icon.png" alt-text="Flock Web sitesinin ekran görüntüsü. Ayarlar simgesi vurgulanır ve kısayol menüsü görünür olur. Bu menüde, takımınızı yönetin vurgulanacaktır." border="false":::

2. **Kimlik doğrulama ve sağlama '** yı seçin.

    :::image type="content" source="media/Flock-provisioning-tutorial/auth.png" alt-text="Flock Web sitesindeki bir menünün ekran görüntüsü. Kimlik doğrulama ve sağlama öğesi vurgulanır." border="false":::

3. **API belirtecini** kopyalayın. Bu değerler, Azure portal Flock uygulamanızın sağlama sekmesinde bulunan **gizli dizi belirteci** alanına girilecektir.

    :::image type="content" source="media/Flock-provisioning-tutorial/provisioning.png" alt-text="Flock Web sitesindeki sağlama sekmesinin ekran görüntüsü. Bir P I belirteci altında, bir değer vurgulanır. Belirtecin yanında bir kopya belirteci düğmesi bulunur." border="false":::


## <a name="add-flock--from-the-gallery"></a>Galeriden Flock ekleyin

Azure AD ile otomatik Kullanıcı sağlama için Flock 'u yapılandırmak üzere Azure AD Uygulama Galerisi 'nden yönetilen SaaS uygulamaları listenize Flock eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Flock eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** ' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Flock** girin, sonuçlar panelinde **Flock** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde Flock](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-flock"></a>Otomatik Kullanıcı sağlamasını Flock olarak yapılandırma  

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak Kullanıcı ve/veya grup atamalarını Azure AD 'de Kullanıcı ve/veya grup atamalarına göre oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, Flock [Çoklu oturum açma öğreticisinde](Flock-tutorial.md)sunulan yönergeleri Izleyerek Flock için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilse de

### <a name="to-configure-automatic-user-provisioning-for-flock--in-azure-ad"></a>Azure AD 'de Flock için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar** 'ı ve ardından **Tüm uygulamalar** 'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Flock** ' u seçin.

    ![Uygulamalar listesindeki Flock bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu** 'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. Yönetici kimlik bilgileri bölümünde, `https://api.flock-staging.com/v2/scim` sırasıyla **kiracı URL 'Si** ve **gizli belirteç** içinde alınan ve **API belirteci** değerlerini girin. Azure AD 'nin Flock 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Flock hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder** ' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet** ’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Flock olarak eşitler** ' ı seçin.

    ![Kullanıcı eşlemelerini Flock](media/flock-provisioning-tutorial/usermapping.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Flock 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Flock içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Kullanıcı özniteliklerini Flock](media/flock-provisioning-tutorial/userattribute.png)

11. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

12. Azure AD sağlama hizmetini Flock için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Flock 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

14. Hazırlama işlemini başlatmak için **Kaydet** 'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin sonraki eşitlemeler daha uzun sürer. Kullanıcıların ve/veya grupların sağlaması için ne kadar süreceğine ilişkin daha fazla bilgi için bkz. [kullanıcıları sağlamak için ne kadar sürer](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

İlerleme durumunu izlemek için **geçerli durum** bölümünü kullanabilir ve Azure AD sağlama hizmeti tarafından Flock üzerinde gerçekleştirilen tüm eylemleri açıklayan sağlama etkinliği raporunuzun bağlantılarını izleyebilirsiniz. Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md). Azure AD sağlama günlüklerini okumak için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).



## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)