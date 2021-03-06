---
title: 'Öğretici: Amazon Web Services (AWS) ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Amazon Web Services (AWS) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/08/2020
ms.author: jeedes
ms.openlocfilehash: 286dc20ba70c78f8248f611abd75e0acc303c068
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736196"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-amazon-web-services-aws"></a>Öğretici: Amazon Web Services (AWS) ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Amazon Web Services (AWS) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Amazon Web Services (AWS) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Amazon Web Services erişimi olan denetim (AWS).
* Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Services (AWS) için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

> [!Note]
> Azure AD, AWS SSO 'SU ile çoklu oturum açma tümleştirmesini desteklemez, AWS 'den farklı bir üründür. AWS 'nin [buraya](https://docs.aws.amazon.com/singlesignon/latest/userguide/azure-ad-idp.html)bahsetmesine rağmen, Azure AD, müşterilerin tek tek hesaplarda koşullu erişim ilkelerini kullanarak daha iyi güvenlik denetimleri elde etmenizi ve bu uygulamaları daha iyi idare etmelerini sağlamak için AWS IAM tümleştirmesini kullanmasını önerir.

![Azure AD ve AWS ilişkisi diyagramı](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Birden çok örnek için birden çok tanımlayıcı yapılandırabilirsiniz. Örneğin:

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Bu değerlerle Azure AD, değerini kaldırır **#** ve `https://signin.aws.amazon.com/saml` SAML belirtecindeki hedef kitle URL 'si olarak doğru değeri gönderir.

Bu yaklaşım aşağıdaki nedenlerle önerilir:

- Her uygulama size benzersiz bir x509 sertifikası sağlar. AWS uygulama örneğinin her bir örneği daha sonra ayrı bir AWS hesabı temelinde yönetilebilen farklı bir sertifika sona erme tarihine sahip olabilir. Bu durumda genel sertifika geçişi daha kolay.

- Azure AD 'de bir AWS uygulamasıyla Kullanıcı sağlamayı etkinleştirebilir ve ardından hizmetimiz bu AWS hesabındaki tüm rolleri getirir. Uygulama üzerinde AWS rollerini el ile eklemeniz veya güncelleştirmeniz gerekmez.

- Uygulama sahibini uygulama için ayrı ayrı atayabilirsiniz. Bu kişi, uygulamayı doğrudan Azure AD 'de yönetebilir.

> [!Note]
> Yalnızca bir galeri uygulaması kullandığınızdan emin olun.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AWS çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Amazon Web Services (AWS) **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Services (AWS) ekleme

Amazon Web Services (AWS) tümleştirmesini Azure AD ile yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Amazon Web Services (AWS) eklemeniz gerekir.

1. İş hesabı, okul hesabı veya kişisel Microsoft hesabı kullanarak Azure portal oturum açın.
1. Azure portal, araması yapın ve **Azure Active Directory** seçin.
1. Azure Active Directory genel bakış menüsünde **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulama eklemek için **Yeni uygulama** ' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Web Services (AWS)** yazın.
1. Sonuçlar panelinden **Amazon Web Services (AWS)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-amazon-web-services-aws"></a>Amazon Web Services (AWS) için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu Amazon Web Services (AWS) ile yapılandırın ve test edin. SSO 'nun çalışması için, Amazon Web Services (AWS) ' de bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Amazon Web Services (AWS) ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Amazon Web Services (AWS) SSO 'Yu yapılandırın](#configure-amazon-web-services-aws-sso)** .
    1. Kullanıcının Azure AD gösterimine bağlı olan Amazon Web Services (AWS) için B. Simon 'a sahip olmak üzere **[Amazon Web Services (AWS) test kullanıcısı oluşturun](#create-amazon-web-services-aws-test-user)** .
    1. **[Amazon Web Services (AWS) içinde rol sağlamayı yapılandırma](#how-to-configure-role-provisioning-in-amazon-web-services-aws)**
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Amazon Web Services (AWS)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde hem **TANıMLAYıCıYı (varlık kimliği)** hem de **yanıt URL** 'sini aynı varsayılan değerle güncelleştirin: `https://signin.aws.amazon.com/saml` . Yapılandırma değişikliklerini kaydetmek için **Kaydet** ' i seçmeniz gerekir.

1. Birden fazla örnek yapılandırırken bir tanımlayıcı değeri sağlayın. İkinci örnekten başlayarak, bir işaret da dahil olmak **#** üzere benzersiz BIR SPN değeri belirtmek için aşağıdaki biçimi kullanın.

    `https://signin.aws.amazon.com/saml#2`

1. AWS uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. AWS uygulaması, yukarıdakine ek olarak, aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Name  | Kaynak özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | Roleoturumadı | User. UserPrincipalName | `https://aws.amazon.com/SAML/Attributes` |
    | Rol | Kullanıcı. atandroles |  `https://aws.amazon.com/SAML/Attributes` |
    | SessionDuration | "900 saniye (15 dakika) ila 43200 saniye (12 saat) arasında bir değer belirtin" |  `https://aws.amazon.com/SAML/Attributes` |

    > [!NOTE]
    > AWS, uygulamaya atanan kullanıcılar için roller bekliyor. Kullanıcılara uygun roller atanabilmeleri için lütfen bu rolleri Azure AD 'de ayarlayın. Azure AD 'de rolleri nasıl yapılandıracağınızı anlamak için [buraya](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview) bakın

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** (3. adım) iletişim kutusunda **sertifika ekle**' yi seçin.

    ![Yeni SAML sertifikası oluştur](common/add-saml-certificate.png)

1. Yeni bir SAML imzalama sertifikası oluşturun ve ardından **Yeni sertifika**' yı seçin. Sertifika bildirimleri için bir e-posta adresi girin.
   
    ![Yeni SAML sertifikası](common/new-saml-certificate.png) 

1. **SAML Imzalama sertifikası** bölümünde, **Federasyon meta veri XML** 'i bulun ve sertifikayı Indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](./media/amazon-web-service-tutorial/certificate.png)

1. **Amazon Web Services ayarla (AWS)** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal, araması yapın ve **Azure Active Directory** seçin.
1. Azure Active Directory genel bakış menüsünde **Kullanıcılar**  >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Amazon Web Services (AWS) erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Amazon Web Services (AWS)** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-amazon-web-services-aws-sso"></a>Amazon Web Services (AWS) SSO 'yu yapılandırma

1. Farklı bir tarayıcı penceresinde, AWS şirket sitenizde yönetici olarak oturum açın.

2. **AWS giriş** seçeneğini belirleyin.

    ![AWS şirket sitesinin, AWS giriş simgesi vurgulanmış ekran görüntüsü][11]

3. **Kimlik ve erişim yönetimi '** ni seçin.

    ![AWS Hizmetleri sayfasının ekran görüntüsü, ıAM vurgulanmış][12]

4. **Kimlik sağlayıcıları**  >  **sağlayıcı oluştur**' u seçin.

    ![Kimlik sağlayıcıları ve sağlayıcı oluşturma vurgulanmış olarak ıAM sayfasının ekran görüntüsü][13]

5. **Sağlayıcıyı yapılandır** sayfasında, aşağıdaki adımları uygulayın:

    ![Yapılandırma sağlayıcısının ekran görüntüsü][14]

    a. **Sağlayıcı türü** için **SAML**' yi seçin.

    b. **Sağlayıcı adı** için bir sağlayıcı adı yazın (örneğin: *Waad*).

    c. İndirilen **meta veri dosyanızı** Azure Portal yüklemek Için **Dosya Seç**' i seçin.

    d. **Sonraki adımı** seçin.

6. **Sağlayıcı bilgilerini doğrula** sayfasında **Oluştur**' u seçin.

    ![Create vurgulanan ile sağlayıcı bilgilerini doğrulama ekran görüntüsü][15]

7. Rol   >  **oluşturma rolü**' nü seçin.

    ![Roller sayfasının ekran görüntüsü][16]

8. **Rol oluştur** sayfasında, aşağıdaki adımları uygulayın:  

    ![Rol Oluştur sayfasının ekran görüntüsü][19]

    a. **Güvenilen varlık türünü seçin** altında **SAML 2,0 Federasyonu**' ni seçin.

    b. **SAML 2,0 sağlayıcısı seçin** altında, daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *Waad*).

    c. **Programlı ve AWS Yönetim Konsolu erişimine Izin ver '** i seçin.
  
    d. Şunu seçin: **İleri: İzinler**.

9. **İzin Ilkeleri Ekle** iletişim kutusunda, uygun ilkeyi kuruluşunuza göre ekleyin. Sonra Ileri ' yi seçin **: İnceleme**.  

    ![İzin ilkesi Ekle iletişim kutusunun ekran görüntüsü][33]

10. **Gözden geçirme** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Inceleme iletişim kutusunun ekran görüntüsü][34]

    a. **Rol adı** alanına rol adınızı girin.

    b. **Rol açıklaması**' nda, açıklamayı girin.

    c. **Rol oluştur**' u seçin.

    d. Gereken sayıda rol oluşturun ve bunları kimlik sağlayıcısına eşleyin.

11. Azure AD Kullanıcı sağlaması içindeki AWS hesabından rolleri getirmek için AWS hizmet hesabı kimlik bilgilerini kullanın. Bu için AWS konsolu ana öğesini açın.

12. **Hizmetler**' i seçin. **Güvenlik, kimlik & uyumluluğu** altında, **IAM**' i seçin.

    ![Hizmet ve ıAM ile AWS konsol giriş ekranının ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. IAM bölümünde **ilkeler**' i seçin.

    ![Ilke vurgulanmış şekilde ıAM bölümünün ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Azure AD Kullanıcı sağlaması içindeki AWS hesabından rolleri getirmeye yönelik **Ilke oluştur** ' a tıklayarak yeni bir ilke oluşturun.

    ![Rol oluştur seçeneğinin, ilke oluştur vurgulanarak ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. AWS hesaplarından tüm rolleri getirmek için kendi ilkenizi oluşturun.

    ![JSON vurgulanmış şekilde ilke oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy1.png)

    a. **Ilke oluştur** bölümünde **JSON** sekmesini seçin.

    b. İlke belgesinde aşağıdaki JSON 'u ekleyin:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                "iam:ListRoles"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

    c. İlkeyi doğrulamak için **Ilkeyi gözden geçir** ' i seçin.

    ![İlke Oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy5.png)

16. Yeni ilkeyi tanımlayın.

    ![Ad ve açıklama alanları vurgulanmış şekilde ilke oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy2.png)

    a. **Ad** için **AzureAD_SSOUserRole_Policy** girin.

    b. **Açıklama** için **Bu ilkenin, AWS hesaplarından rolleri getirmeye izin verilecek şekilde** girin.

    c. **İlke oluştur**'u seçin.

17. AWS ıAM hizmetinde yeni bir kullanıcı hesabı oluşturun.

    a. AWS ıAM konsolunda **Kullanıcılar**' ı seçin.

    ![AWS ıAM konsolunun, kullanıcıların vurgulandığı ekran görüntüsü](./media/amazon-web-service-tutorial/policy3.png)

    b. Yeni bir kullanıcı oluşturmak için **Kullanıcı Ekle**' yi seçin.

    ![Kullanıcı Ekle düğmesinin ekran görüntüsü](./media/amazon-web-service-tutorial/policy4.png)

    c. **Kullanıcı Ekle** bölümünde:

    ![Kullanıcı adı ve erişim türü vurgulanmış şekilde Kullanıcı ekleme sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/adduser1.png)

    * Kullanıcı adını **Azureadrolemanager** olarak girin.

    * Erişim türü için **programlı erişim**' i seçin. Bu şekilde, Kullanıcı API 'Leri çağırabilir ve AWS hesabından rolleri alabilir.

    * **Sonraki izinleri** seçin.

18. Bu Kullanıcı için yeni bir ilke oluşturun.

    ![Ekran görüntüsü, Kullanıcı ekleme sayfası ' nı gösterir; burada Kullanıcı için bir ilke oluşturabilirsiniz.](./media/amazon-web-service-tutorial/adduser2.png)

    a. **Mevcut ilkeleri doğrudan Ekle**' yi seçin.

    b. Yeni oluşturulan ilkeyi filtre bölümünde **AzureAD_SSOUserRole_Policy** arama yapın.

    c. İlkeyi seçin ve ardından **İleri: gözden geçir**' i seçin.

19. Ekli Kullanıcı için ilkeyi gözden geçirin.

    ![Kullanıcı Ekle seçeneğinin vurgulandığı Kullanıcı ekleme sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/adduser3.png)

    a. Kullanıcı adını, erişim türünü ve kullanıcıyla eşlenen ilkeyi gözden geçirin.

    b. **Kullanıcı oluştur**' u seçin.

20. Kullanıcının Kullanıcı kimlik bilgilerini indirin.

    ![Ekran görüntüsü Kullanıcı kimlik bilgilerini almak için bir c s v Indir düğmesi olan Kullanıcı Ekle sayfasını gösterir.](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kullanıcı **erişim anahtarı kimliğini** ve gizli dizi **erişim anahtarını** kopyalayın.

    b. AWS konsolundan rolleri getirmek için bu kimlik bilgilerini Azure AD Kullanıcı sağlama bölümüne girin.

    c. **Kapat**’ı seçin.

### <a name="how-to-configure-role-provisioning-in-amazon-web-services-aws"></a>Amazon Web Services (AWS) içinde rol sağlamayı yapılandırma

1. Azure AD yönetim portalında AWS uygulamasında **sağlama**' ya gidin.

    ![Sağlama vurgulanmış şekilde AWS uygulamasının ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning.png)

2. Erişim anahtarını ve gizli anahtarı, sırasıyla **ClientSecret** ve **gizli belirteç** alanlarına girin.

    ![Yönetici kimlik bilgileri iletişim kutusunun ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning1.png)

    a. **ClientSecret** alanına AWS Kullanıcı erişimi anahtarını girin.

    b. **Gizli belirteç** alanına AWS Kullanıcı gizli anahtarını girin.

    c. **Bağlantıyı Sına**'yı seçin.

    d. **Kaydet**' i seçerek ayarı kaydedin.

3. **Ayarlar** bölümünde, **sağlama durumu** için **Açık**' ı seçin. Sonra **Kaydet**'i seçin.

    ![Vurgulanan Ayarlar bölümünün ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning2.png)

> [!NOTE]
> Sağlama hizmeti rolleri yalnızca AWS 'den Azure AD 'ye aktarır. Hizmet, Azure AD 'den AWS 'ye Kullanıcı ve grup sağlamaz.

> [!NOTE]
> Sağlama kimlik bilgilerini kaydettikten sonra, ilk eşitleme döngüsünün çalışmasını beklemeniz gerekir. Eşitleme genellikle yaklaşık 40 dakika sürer. Durumu **sağlama** sayfasının en altında, **geçerli durum** altında görebilirsiniz.

### <a name="create-amazon-web-services-aws-test-user"></a>Amazon Web Services (AWS) test kullanıcısı oluştur

Bu bölümün amacı, Amazon Web Services (AWS) içinde B. Simon adlı bir Kullanıcı oluşturmaktır. Amazon Web Services (AWS), bir kullanıcının SSO için sisteminde oluşturulmasına gerek kalmaz, bu nedenle burada herhangi bir işlem gerçekleştirmeniz gerekmez.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu işlem, oturum akışını başlatabileceğiniz Amazon Web Services (AWS) oturum açma URL 'sine yönlendirir.  

* Amazon Web Services (AWS) oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'yu ayarladığınız Amazon Web Services (AWS) otomatik olarak oturum açmış olmanız gerekir 

Uygulamayı dilediğiniz modda test etmek için Microsoft Access panel ' i de kullanabilirsiniz. Erişim panelindeki Amazon Web Services (AWS) kutucuğuna tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Amazon Web Services (AWS) otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).


## <a name="known-issues"></a>Bilinen sorunlar

 * **Sağlama** bölümünde, **eşlemeler** alt bölümünde "yükleniyor..." görüntülenir ileti eşleştirmez ve hiçbir şekilde öznitelik eşlemelerini görüntülemez. Bugün desteklenen tek sağlama iş akışı, bir kullanıcı veya grup ataması sırasında AWS 'den Azure AD 'ye roller için içeri aktarmaktır. Bunun için öznitelik eşlemeleri önceden belirlenir ve yapılandırılamaz.

 * **Sağlama** bölümü yalnızca bir AWS kiracısı için tek seferde bir kimlik bilgileri kümesi girmeyi destekler. İçeri aktarılan tüm roller `appRoles` AWS kiracısı Için Azure AD [ `servicePrincipal` nesnesinin](/graph/api/resources/serviceprincipal?view=graph-rest-beta) özelliğine yazılır.

   Azure AD 'ye, sağlama için Galeriden birden çok AWS kiracısı (tarafından temsil edilir `servicePrincipals` ) eklenebilir. Bununla birlikte, tüm içeri aktarılan rollerin, `servicePrincipals` SSO için kullanılmak üzere tek başına sağlanması için kullanılan birden fazla AWS 'den otomatik olarak yazamayacak bilinen bir sorun vardır `servicePrincipal` .

   Geçici bir çözüm olarak, sağlama yapılandırılmış her bir AWS ' de içeri aktarılan her bir ı ayıklamak için [MICROSOFT Graph API](/graph/api/resources/serviceprincipal?view=graph-rest-beta) 'sini kullanabilirsiniz `appRoles` `servicePrincipal` . Daha sonra, bu rol dizelerini SSO 'nun yapılandırıldığı AWS 'ye ekleyebilirsiniz `servicePrincipal` .

* Rollerin AWS 'den Azure AD 'ye aktarılmak için aşağıdaki gereksinimleri karşılaması gerekir:

  * Roller tam olarak bir SAML sağlayıcısına sahip olmalıdır ve AWS 'de tanımlı
  * Rol için ARN (Amazon kaynak adı) ve ilişkili SAML-Provider için ARN 'ın Birleşik uzunluğu 240 karakterden az olmalıdır.

## <a name="change-log"></a>Değişiklik günlüğü

* 01/12/2020-119 karakterden daha fazla 239 karakter uzunluğundaki rol uzunluğu sınırı. 

## <a name="next-steps"></a>Sonraki adımlar

Amazon Web Services (AWS) yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)


[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
