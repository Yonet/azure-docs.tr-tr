---
title: 'Öğretici: SafeConnect ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SafeConnect arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2019
ms.author: jeedes
ms.openlocfilehash: 299d75f6d039ae36cdc99f5e77ec0dc8ca456a04
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92676763"
---
# <a name="tutorial-integrate-safeconnect-with-azure-active-directory"></a>Öğretici: SafeConnect 'i Azure Active Directory tümleştirme

Bu öğreticide, SafeConnect 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SafeConnect 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de SafeConnect 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla güvenli bağlantı kurmak için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SafeConnect çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* SafeConnect **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-safeconnect-from-the-gallery"></a>Galeriden SafeConnect ekleme

SafeConnect 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize SafeConnect ' i eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama** ' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **safeconnect** yazın.
1. Sonuçlar panelinden **Safeconnect** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu safeconnect ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile SafeConnect 'teki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu SafeConnect ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[SafeConnect SSO 'Yu yapılandırın](#configure-safeconnect-sso)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
5. Safeconnect 'te kullanıcının Azure AD gösterimine bağlı olan, SafeConnect 'te B. Simon 'a karşılık gelen bir karşılığı olacak şekilde **[güvenli bağlantı oluşturma test kullanıcısı oluşturun](#create-safeconnect-test-user)** .
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **safeconnect** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma** ' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML** ' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://portal.myweblogon.com:8443/saml/login`

1. **SAML Ile tekli Sign-On ayarlama** sayfasında, **SAML Imzalama Sertifikası** bölümünde **meta veri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Güvenli Bağlan ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-safeconnect-sso"></a>SafeConnect SSO 'yu yapılandırma

**Safeconnect** tarafında çoklu oturum açmayı yapılandırmak için, Indirilen **meta veri XML** 'Sini ve uygun kopyalanmış URL 'Leri Azure Portal ' den [safeconnect destek ekibine](mailto:support@impulse.com)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory** ' i seçin, **Kullanıcılar** ' ı seçin ve ardından **tüm kullanıcılar** ' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur** 'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SafeConnect 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar** ' ı seçin ve ardından **tüm uygulamalar** ' ı seçin.
1. Uygulamalar listesinde, **Safeconnect** ' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar** ' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle** ' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-safeconnect-test-user"></a>Güvenli bağlantı test kullanıcısı oluşturma

Bu bölümde, SafeConnect 'te Britta Simon adlı bir Kullanıcı oluşturacaksınız. Safeconnect platformunda kullanıcıları eklemek için [safeconnect destek ekibi](mailto:support@impulse.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde SafeConnect kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SafeConnect 'te otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)