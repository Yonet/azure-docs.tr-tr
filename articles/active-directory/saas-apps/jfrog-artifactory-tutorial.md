---
title: 'Öğretici: JFrog Artifactory ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve JFrog Artifactory arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.openlocfilehash: f0fafa5c0cc2e0b1bf0f4e11db3265824feb5296
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100374719"
---
# <a name="tutorial-integrate-jfrog-artifactory-with-azure-active-directory"></a>Öğretici: JFrog Artifactory 'yi Azure Active Directory tümleştirme

Bu öğreticide, JFrog Artifactory 'yi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. JFrog Artifactory 'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de JFrog Artifactory erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla JFrog Artifactory ' de otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* JFrog Artifactory çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* JFrog Artifactory **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* JFrog Artifactory **, tam zamanında** Kullanıcı sağlamayı destekliyor

## <a name="adding-jfrog-artifactory-from-the-gallery"></a>Galeriden JFrog Artifactory ekleme

JFrog Artifactory 'nin tümleştirmesini Azure AD 'ye göre yapılandırmak için galerideki JFrog Artifactory 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **JFrog Artifactory** yazın.
1. Sonuçlar panelinden **JFrog Artifactory** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak JFrog Artifactory Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve JFrog Artifactory içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu JFrog Artifactory ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[JFrog Artifactory SSO 'Yu yapılandırın](#configure-jfrog-artifactory-sso)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
5. **[JFrog Artifactory test kullanıcısı oluşturun](#create-jfrog-artifactory-test-user)** -bu, kullanıcının Azure AD gösterimine bağlı olan JFrog Artifactory 'de B. Simon 'a karşılık gelen bir değere sahip olmalıdır.
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **JFrog Artifactory** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`<servername>.jfrog.io`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:
    
    - Artifactory 6. x için: `https://<servername>.jfrog.io/artifactory/webapp/saml/loginResponse`
    - Artifactory 7. x için: `https://<servername>.jfrog.io/<servername>/webapp/saml/loginResponse`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:
    - Artifactory 6. x için: `https://<servername>.jfrog.io/<servername>/webapp/`
    - Artifactory 7. x için: `https://<servername>.jfrog.io/ui/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [JFrog Artifactory istemci destek ekibine](https://support.jfrog.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. JFrog Artifactory uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. Kullanıcı öznitelikleri iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Ekran görüntüsü, düzenleme denetimi olarak adlandırılan Kullanıcı özniteliklerini gösterir.](common/edit-attribute.png)

1. Yukarıdaki ' a ek olarak, JFrog Artifactory, bir dizi ek özniteliğin SAML yanıtına geri geçirilmesini bekler. **Grup talepleri (Önizleme)** Iletişim kutusundaki **Kullanıcı öznitelikleri & talepler** bölümünde aşağıdaki adımları uygulayın:

    a. **Talepte döndürülen gruplar ' ın** yanındaki **kaleme** tıklayın.

    ![Ekran görüntüsü, düzenleme simgesi seçili olan talepleri & Kullanıcı özniteliklerini gösterir.](./media/jfrog-artifactory-tutorial/config04.png)

    ![Ekran görüntüsü tüm gruplar seçili olan grup talepleri bölümünü gösterir.](./media/jfrog-artifactory-tutorial/config05.png)

    b. Radyo listesinden **tüm gruplar** ' ı seçin.

    c. **Kaydet**’e tıklayın.

4. **SAML Ile tekli Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, sertifikayı Indirip bilgisayarınıza kaydetmek Için sertifikayı bulun **(base64)** ve **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](./media/jfrog-artifactory-tutorial/certificate-base.png)

6. Artifactory 'yi (SAML hizmeti sağlayıcısı adı) ' tanımlayıcı ' alanıyla yapılandırın (bkz. 4. adım). **JFrog Artifactory 'Yi ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   - Artifactory 6. x için: `https://<servername>.jfrog.io/artifactory/webapp/saml/loginResponse` 
   - Artifactory 7. x için: `https://<servername>.jfrog.io/<servername>/webapp/saml/loginResponse`

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-jfrog-artifactory-sso"></a>JFrog Artifactory SSO 'yu yapılandırma

**JFrog Artifactory** tarafında çoklu oturum açmayı yapılandırmak için ihtiyaç duyduğunuz her şey, SAML yapılandırması ekranındaki Artifactory Yöneticisi tarafından yapılandırılabilir.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, JFrog Artifactory 'ye erişim vererek, B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **JFrog Artifactory**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-jfrog-artifactory-test-user"></a>JFrog Artifactory test kullanıcısı oluşturma

Bu bölümde, JFrog Artifactory içinde B. Simon adlı bir Kullanıcı oluşturulur. JFrog Artifactory, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı JFrog Artifactory 'de zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

### <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde JFrog Artifactory kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız JFrog Artifactory ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)