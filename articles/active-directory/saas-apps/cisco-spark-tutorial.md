---
title: 'Öğretici Azure Active Directory: Cisco WebEx ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Cisco WebEx arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.openlocfilehash: 49e92c485c1a6a66dfb12b3c7a91f29939851d82
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92456113"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cisco-webex"></a>Öğretici: Cisco WebEx ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide Cisco WebEx Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz. Cisco WebEx 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Cisco WebEx erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Cisco WebEx 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Cisco WebEx çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Cisco WebEx, **SP** tarafından başlatılan SSO 'yu destekler.
* Cisco WebEx **Otomatik** Kullanıcı sağlamayı destekler.
* Cisco WebEx 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin boyutunu gerçek zamanlı olarak koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-cisco-webex-from-the-gallery"></a>Galeriden Cisco WebEx ekleme

Cisco WebEx 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Cisco WebEx eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Cisco WebEx** yazın.
1. Sonuçlar panelinden **Cisco WebEx** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cisco-webex"></a>Cisco WebEx için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Cisco WebEx Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için Cisco WebEx içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Cisco WebEx ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için **[Cisco WebEx 'ı yapılandırın](#configure-cisco-webex)** .
    1. Cisco WebEx **[Test kullanıcısını](#create-cisco-webex-test-user)** , kullanıcının Azure AD gösterimine bağlı olan Cisco WebEx 'de B. Simon 'a sahip olacak şekilde oluşturun.
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Cisco WebEx** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, Indirilen **hizmet sağlayıcısı meta veri** dosyasını karşıya yükleyin ve aşağıdaki adımları gerçekleştirerek uygulamayı yapılandırın:

    >[!Note]
    >Hizmet sağlayıcı meta veri dosyasını, Öğreticinin ilerleyen kısımlarında açıklanan **Cisco WebEx 'Yi yapılandırma** bölümünde bulabilirsiniz. 

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    c. Hizmet sağlayıcı meta veri dosyasını karşıya yükleme işleminin başarıyla tamamlanmasından sonra **tanımlayıcı** ve **yanıt URL** değerleri **temel SAML yapılandırması** bölümünde otomatik olarak doldurulur:

    **Oturum açma URL 'si** metin kutusunda, SP meta veri dosyasını karşıya yüklemeye göre otomatik doldurulmuş **yanıt URL 'si**değerini yapıştırın.

1. Cisco WebEx uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Cisco WebEx uygulaması, yukarıdakine ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
  
    | Name |  Kaynak özniteliği|
    | ---------------|--------- |
    | 'sini | User. UserPrincipalName |

1. **SAML Ile tekli Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Cisco WebEx ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

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

Bu bölümde, Cisco WebEx erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Cisco WebEx**öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-cisco-webex"></a>Cisco WebEx yapılandırma

1. Cisco WebEx içindeki yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemek**üzere **uygulamalar güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya Uzantı eklendikten sonra **Cisco WebEx 'ı ayarla** 'ya tıkladığınızda sizi Cisco WebEx uygulamasına yönlendirebilirsiniz. Buradan, Cisco WebEx 'de oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-8 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Cisco WebEx 'i el ile ayarlamak istiyorsanız, tam yönetici kimlik bilgilerinizle [Cisco Cloud Işbirliği yönetimi](https://admin.ciscospark.com/) ' nde oturum açın.

4. **Ayarlar** ' ı seçin ve **kimlik doğrulaması** bölümünde **Değiştir**' i tıklatın.

    ![Ekran görüntüsü, Değiştir ' i seçebileceğiniz kimlik doğrulama ayarlarını gösterir.](./media/cisco-spark-tutorial/tutorial-cisco-spark-10.png)
  
5. **3. taraf kimlik sağlayıcısını tümleştirin ' ı seçin. (Gelişmiş)** ve sonraki ekrana gidin.

6. **IDP meta verilerini Içeri aktar** sayfasında, Azure AD meta veri dosyasını sayfaya sürükleyip bırakın ya da Azure AD meta veri dosyasını bulup karşıya yüklemek için dosya tarayıcısı seçeneğini kullanın. Ardından, **meta verilerde (daha güvenli) bir sertifika yetkilisi tarafından imzalanmış sertifika iste** ' yi seçin ve **İleri**' ye tıklayın.

    ![Ekran görüntüsü g/ç meta verilerini Içeri aktar sayfasını gösterir.](./media/cisco-spark-tutorial/tutorial-cisco-spark-11.png)

7. **Test SSO bağlantısı**' nı seçin ve yeni bir tarayıcı sekmesi açıldığında, oturum açarak Azure AD ile kimlik doğrulaması yapın.

8. **Cisco Cloud Işbirliği yönetimi** tarayıcı sekmesine dönün. Test başarılı olduysa, **Bu test başarılı oldu öğesini seçin. Tek Sign-On seçeneğini etkinleştirip** **İleri**' ye tıklayın.

### <a name="create-cisco-webex-test-user"></a>Cisco WebEx test kullanıcısı oluşturma

Bu bölümde, Cisco WebEx içinde B. Simon adlı bir Kullanıcı oluşturacaksınız. Bu bölümde, Cisco WebEx içinde B. Simon adlı bir Kullanıcı oluşturacaksınız.

1. Tam yönetici kimlik bilgilerinizle [Cisco Cloud Işbirliği yönetimine](https://admin.ciscospark.com/) gidin.

2. **Kullanıcılar** ' a ve ardından **Kullanıcıları Yönet**' e tıklayın.
   
    ![Ekran görüntüsü, kullanıcıları yönetebileceğiniz kullanıcılar sayfasını gösterir.](./media/cisco-spark-tutorial/tutorial-cisco-spark-12.png) 

3. **Kullanıcı Yönet** penceresinde, **kullanıcıları el ile Ekle veya Değiştir** ' i seçin ve **İleri**' ye tıklayın.

4. **Ad ve e-posta adresi**seçin. Sonra, metin kutusunu aşağıdaki gibi doldurun:

    ![Ekran görüntüsü, kullanıcıları el ile ekleyebileceğiniz veya değiştirebileceğiniz Mange kullanıcıları iletişim kutusunu gösterir.](./media/cisco-spark-tutorial/tutorial-cisco-spark-13.png) 

    a. **Ilk ad** metin kutusuna **B**gibi kullanıcının adını yazın.

    b. **Soyadı** metin kutusunda, **Simon**gibi kullanıcı adının soyadını yazın.

    c. **E-posta adresi** metin kutusuna, gibi kullanıcının e-posta adresini yazın b.simon@contoso.com .

5. B. Simon eklemek için artı işaretine tıklayın. Ardından **İleri**' ye tıklayın.

6. **Kullanıcılar Için hizmet ekle** penceresinde **Kaydet** ' e ve ardından **son**' a tıklayın.

## <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Cisco WebEx kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Cisco WebEx ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)

- [Azure AD ile Cisco WebEx 'i deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security oturum denetimi nedir?](/cloud-app-security/proxy-intro-aad)

- [Cisco WebEx 'ı gelişmiş görünürlük ve denetimlerle koruma](/cloud-app-security/protect-webex)