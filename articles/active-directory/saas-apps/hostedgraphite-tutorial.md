---
title: 'Öğretici: barındırılan Graphite ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ile barındırılan Graphite arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 786c6309f685af6b8e42108c490ce86c7844bc81
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92443257"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Öğretici: barındırılan Graphite ile Azure Active Directory tümleştirme

Bu öğreticide, barındırılan Graphite Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Barındırılan Graphite 'yi Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de barındırılan Graphite erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla barındırılan Graphite (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini barındırılan Graphite ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Barındırılan Graphite çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Barındırılan Graphite **SP ve ıDP** tarafından başlatılan SSO 'yu destekliyor
* Barındırılan Graphite **, tam zamanında** Kullanıcı sağlamayı destekliyor

## <a name="adding-hosted-graphite-from-the-gallery"></a>Galeriden barındırılan Graphite ekleme

Barındırılan Graphite 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, galerideki barındırılan Graphite 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden barındırılan Graphite eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **barındırılan Graphite**yazın, sonuç panelinden **barındırılan Graphite** ' ı seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuçlar listesinde barındırılan Graphite](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre barındırılan Graphite ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve barındırılan Graphite içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı barındırılan Graphite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Barındırılan Graphite çoklu oturum açmayı yapılandırma](#configure-hosted-graphite-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Barındırılan Graphite **[test kullanıcısı oluşturun](#create-hosted-graphite-test-user)** -kullanıcının Azure AD gösterimine bağlı olan barındırılan Graphite 'Da Britta Simon 'un bir karşılığı vardır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı barındırılan Graphite ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/) **barındırılan Graphite** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Ekran görüntüsü; tanımlayıcı girebileceğiniz, yanıt U R L ve Kaydet ' i seçebileceğiniz temel SAML yapılandırmasını gösterir.](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.hostedgraphite.com/metadata/<user id>`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.hostedgraphite.com/complete/saml/<user id>`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Ekran görüntüsü, U R L 'ye bir Işaret girebileceğiniz ek U R 'Leri ayarlamayı gösterir.](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.hostedgraphite.com/login/saml/<user id>/`

    > [!NOTE]
    > Bunların gerçek değerler olduğunu lütfen unutmayın. Bu değerleri gerçek tanımlayıcı, yanıt URL 'si ve oturum açma URL 'SI ile güncelleştirmeniz gerekir. Bu değerleri almak için, uygulama tarafınızda Access->SAML kurulumuna gidebilir veya [barındırılan Graphite destek ekibine](mailto:help@hostedgraphite.com)başvurabilirsiniz.

6. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Barındırılan Graphite ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-hosted-graphite-single-sign-on"></a>Barındırılan Graphite tek Sign-On yapılandırma

1. Barındırılan Graphite kiracınızda yönetici olarak oturum açın.

2. Kenar çubuğu 'ndaki **SAML kurulum sayfasına** gidin (**ERIŞIM-> SAML kurulumu**).

    ![Ekran görüntüsü, SAML kurulumu seçiliyken erişim menüsünü gösterir.](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

3. Bu URL 'Lerin, Azure portal **temel SAML yapılandırması** bölümünde yapılan yapılandırmanızla eşleştiğinden emin olun.

    ![Ekran görüntüsü temel SAML yapılandırmasını gösterir.](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

4. **Varlık veya veren kimliği** ve **SSO oturum açma URL 'si** metin kutularına, Azure Portal kopyaladığınız **Azure AD tanımlayıcısı** ve **oturum açma URL 'si** değerini yapıştırın.

    ![Ekran görüntüsü, kimlik sağlayıcısı için girişleri gösterir.](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

5. **Varsayılan Kullanıcı rolü**olarak **salt okunurdur** ' ı seçin.

    ![Ekran görüntüsü varsayılan kullanıcı rolünü gösterir ve salt okunurdur.](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

6. Azure portal 'den indirilen Not defteri 'nde Base-64 kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve **X. 509.440 sertifika** metin kutusuna yapıştırın.

    ![Ekran görüntüsünde X nokta 509 sertifikası gösterilmektedir.](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

7. **Kaydet** düğmesine tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına ** \@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, barındırılan Graphite erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **barındırılan Graphite**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **barındırılan Graphite**' ı seçin.

    ![Uygulamalar listesinde barındırılan Graphite bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-hosted-graphite-test-user"></a>Barındırılan Graphite test kullanıcısı oluştur

Bu bölümde, barındırılan Graphite içinde Britta Simon adlı bir Kullanıcı oluşturulur. Barındırılan Graphite, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı barındırılan Graphite 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, aracılığıyla barındırılan Graphite destek ekibine başvurmanız gerekir <mailto:help@hostedgraphite.com> .

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde barındırılan Graphite kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız barındırılan Graphite için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)