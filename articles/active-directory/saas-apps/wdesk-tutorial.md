---
title: 'Öğretici: Wdesk ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Wdesk arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/02/2020
ms.author: jeedes
ms.openlocfilehash: 11539e7d6e685703810537cf69e0e7ed3ec0be4c
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609097"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wdesk"></a>Öğretici: Wdesk ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Wdesk 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Wdesk 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Wdesk 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Wdesk 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Wdesk çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Wdesk **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler
* Wdesk 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve çıkartılmasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-wdesk-from-the-gallery"></a>Galeriden Wdesk ekleme

Wdesk 'in tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Wdesk 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **wdesk** yazın.
1. Sonuçlar panelinden **Wdesk** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre wdesk ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Wdesk 'teki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD SSO 'yu Wdesk ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Wdesk SSO 'Yu yapılandırma](#configure-wdesk-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[Wdesk test kullanıcısı oluşturun](#create-wdesk-test-user)** -bu kullanıcının Azure AD gösterimine bağlı olan wdesk 'e ait B. Simon 'a karşılık gelen bir karşılığı olmalıdır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Wdesk ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **wdesk** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Ekran görüntüsü; tanımlayıcı girebileceğiniz, yanıt U R L ve Kaydet ' i seçebileceğiniz temel SAML yapılandırmasını gösterir.](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Ekran görüntüsü, U R L 'ye bir Işaret girebileceğiniz ek U R 'Leri ayarlamayı gösterir.](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve Sign-On URL 'siyle güncelleştirin. Bu değerleri, SSO 'yu yapılandırırken WDesk portalından alırsınız.

4. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Wdesk 'ı ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon** girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension . Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Wdesk 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **wdesk**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Wdesk**' i seçin.

    ![Uygulamalar listesindeki Wdesk bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-wdesk-sso"></a>Wdesk SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Wdesk 'de güvenlik yöneticisi olarak oturum açın.

1. Sol alt tarafta **yönetici** ' ye tıklayın ve **Hesap Yöneticisi**' ni seçin:
 
    ![Ekran görüntüsü, Yönetici menüsünden Hesap Yöneticisi ' ni gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Wdesk Yöneticisi 'nde **güvenlik**' e gidin ve **SAML**  >  **SAML ayarları**:

    ![Ekran görüntüsü SAML sekmesinden seçilen SAML ayarlarını gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

1. **SAML Kullanıcı kimliği ayarları** altında **SAML Kullanıcı kimliği ' nin Wdesk Kullanıcı adı**' na bakın.

    ![Ekran görüntüsü, SAML kullanıcısı g Masası Kullanıcı adı ' nı seçebileceğiniz SAML Kullanıcı g/ç ayarlarını gösterir.](./media/wdesk-tutorial/wdesk-username.png)

4. **Genel ayarlar** altında **SAML çoklu oturum açmayı etkinleştir**' i işaretleyin:

    ![Ekran görüntüsü, SAML çoklu oturum açmayı etkinleştir ' i seçebileceğiniz SAML ayarlarını Düzenle ' yi gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

5. **Hizmet sağlayıcı ayrıntıları**' nın altında, aşağıdaki adımları uygulayın:

    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz hizmet sağlayıcısı ayrıntılarını gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

    1. **Oturum açma URL** 'sini kopyalayın ve Azure Portal oturum açma **URL 'si** metin kutusuna yapıştırın.

    1. **Meta veri URL 'sini** kopyalayıp Azure Portal **tanımlayıcı** metin kutusuna yapıştırın.

    1. **Tüketici URL 'sini** kopyalayıp Azure Portal **Yanıtla URL** metin kutusuna yapıştırın.

    1. Değişiklikleri kaydetmek için Azure portal **Kaydet** ' e tıklayın.      

1. IDP ayarlarını **Düzenle** iletişim kutusunu açmak Için **IDP ayarlarını yapılandır** öğesine tıklayın. Azure portal kaydettiğiniz **Metadata.xml** dosyasını bulmak Için **Dosya Seç** ' e tıklayın, ardından karşıya yükleyin.
    
    ![Ekran görüntüsü, meta verileri karşıya yükleyebileceğiniz g/ç ayarlarını Düzenle ' ye gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
1. **Değişiklikleri Kaydet**' e tıklayın.

    ![Ekran görüntüsü değişiklikleri Kaydet düğmesini gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

### <a name="create-wdesk-test-user"></a>Wdesk test kullanıcısı oluşturma

Azure AD kullanıcılarının Wdesk 'de oturum açmasını sağlamak için, Wdesk 'e sağlanması gerekir. Wdesk 'de sağlama, el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Wdesk 'de güvenlik yöneticisi olarak oturum açın.

2. **Yönetici**  >  **hesabı Yöneticisi**' ne gidin.

     ![Ekran görüntüsü, Yönetici menüsünden Hesap Yöneticisi ' ni gösterir.](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. **Kişiler** altında **Üyeler** ' e tıklayın.

4. Şimdi **üye Ekle iletişim kutusunu** açmak Için **üye Ekle** ' ye tıklayın. 
   
    ![Ekran görüntüsü üye Ekle ' yi seçebileceğiniz üye sekmesini gösterir.](./media/wdesk-tutorial/createuser1.png)  

5. **Kullanıcı** metin kutusunda, kullanıcının Kullanıcı adını girip b.simon@contoso.com **devam** düğmesine tıklayın.

    ![Ekran görüntüsü, Kullanıcı girebileceğiniz üye Ekle iletişim kutusunu gösterir.](./media/wdesk-tutorial/createuser3.png)

6.  Ayrıntıları aşağıda gösterildiği gibi girin:
  
    ![Ekran görüntüsü, bir kullanıcı için temel bilgileri ekleyebileceğiniz üye Ekle iletişim kutusunu gösterir.](./media/wdesk-tutorial/createuser4.png)
 
    a. **E-posta** metin kutusuna kullanıcının e-postasını girin b.simon@contoso.com .

    b. **Ad** metin kutusuna **B** gibi kullanıcının adını girin.

    c. **Soyadı** metin kutusuna, **Simon** gibi kullanıcı adının soyadını girin.

7. **Üye kaydet** düğmesine tıklayın.  

    ![Ekran görüntüsü, üye Kaydet düğmesi ile hoş geldiniz e-postası gönder düğmesini gösterir.](./media/wdesk-tutorial/createuser5.png)

### <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Wdesk kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Wdesk 'e otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)

- [Microsoft Cloud App Security oturum denetimi nedir?](/cloud-app-security/proxy-intro-aad)