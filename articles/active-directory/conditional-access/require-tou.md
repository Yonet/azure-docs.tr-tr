---
title: Koşullu erişim için kullanım koşulları gerekir-Azure Active Directory
description: Bu hızlı başlangıçta, seçilen bulut uygulamalarına erişim için Azure Active Directory Koşullu erişim tarafından sağlanmadan önce kullanım şartlarınızın kabul edilip edilmediğini nasıl sağlayabileceğinizi öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ab484e8caaffaf57f19f1fcd1e65f4b8e723f86
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93077906"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Hızlı başlangıç: bulut uygulamalarına erişmeden önce kabul edilecek kullanım koşullarını gerektir

Ortamınızdaki belirli bulut uygulamalarına erişmeden önce, kullanım koşullarınızı (ToU) kabul etme biçiminde kullanıcılardan onay almak isteyebilirsiniz. Azure Active Directory (Azure AD) koşullu erişim size şunları sağlar:

- ToU yapılandırma basit bir yöntemi
- Koşullu erişim ilkesi aracılığıyla kullanım koşullarınızı kabul etmeyi gerektirme seçeneği  

Bu hızlı başlangıçta, ortamınızdaki seçili bir bulut uygulaması için ToU 'nın kabul edilmesi gereken bir [Azure AD koşullu erişim ilkesinin](./overview.md) nasıl yapılandırılacağı gösterilmektedir.

:::image type="content" source="./media/require-tou/5555.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta senaryoyu tamamlayabilmeniz için şunlar gerekir:

- **Azure AD Premium sürümüne erişim** -Azure AD koşullu erişim, Azure AD Premium bir özelliktir.
- **Isabella Simonsen adlı bir sınama hesabı** -bir sınama hesabı oluşturmayı bilmiyorsanız bkz. [bulut tabanlı kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Oturum açma bilgilerinizi test etme

Bu adımın amacı, koşullu erişim ilkesi olmadan oturum açma deneyiminin bir izlenimini sağlamaktır.

**Oturum açma bilgilerinizi test etmek için:**

1. [Azure Portal](https://portal.azure.com/) Isabella Simonsen olarak oturum açın.
1. Oturumunuzu kapatın.

## <a name="create-your-terms-of-use"></a>Kullanım koşullarınızı oluşturun

Bu bölüm, örnek bir ToU oluşturma adımları sağlar. Bir ToU oluşturduğunuzda, **koşullu erişim ilkesi şablonlarıyla zorlamak** için bir değer seçersiniz. **Özel ilke** seçildiğinde, ToU oluşturulduktan hemen sonra yeni bir koşullu erişim ilkesi oluşturmak için iletişim kutusu açılır.

**Kullanım koşullarınızı oluşturmak için:**

1. Microsoft Word 'de yeni bir belge oluşturun.
1. **Kullanım koşullarımı** yazın ve ardından belgeyi **mytou.pdf** olarak bilgisayarınıza kaydedin.
1. [Azure Portal](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. Azure portal sol gezinti çubuğunda **Azure Active Directory** ' e tıklayın.

   ![Azure Active Directory](./media/require-tou/02.png)

1. **Azure Active Directory** sayfasındaki **güvenlik** bölümünde **koşullu erişim** ' e tıklayın.

   ![Koşullu Erişim](./media/require-tou/03.png)

1. **Yönet** bölümünde **kullanım koşulları** ' a tıklayın.

   :::image type="content" source="./media/require-tou/04.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

1. Üstteki menüde **yeni terimler** ' e tıklayın.

   :::image type="content" source="./media/require-tou/05.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

1. **Yeni kullanım koşulları** sayfasında:

   :::image type="content" source="./media/require-tou/112.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

   1. **Ad** metın kutusuna **tou My** yazın.
   1. **Görünen ad** metın kutusuna **tou My** yazın.
   1. Kullanım koşullarınızı PDF dosyanızı karşıya yükleyin.
   1. **Dil** olarak, **İngilizce** ' yi seçin.
   1. **Kullanıcıların kullanım koşullarını genişletmesini gerektirirken** **Açık** ' ı seçin.
   1. **Koşullu erişim ilkesi şablonlarıyla zorlamak** için **özel ilke** ' yi seçin.
   1. **Oluştur** 'a tıklayın.

## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturma

Bu bölüm, gerekli koşullu erişim ilkesinin nasıl oluşturulacağını gösterir. Bu hızlı başlangıçtaki senaryo şunları kullanır:

- Bir bulut uygulaması için, ToU 'nizin kabul edilmesini gerektiren Azure portal yer tutucu olarak. 
- Koşullu erişim ilkesini test etmek için örnek Kullanıcı.  

İlkenizde şunları ayarlayın:

| Ayar | Değer |
| --- | --- |
| Kullanıcılar ve gruplar | Isabella Simonsen |
| Bulut uygulamaları | Microsoft Azure yönetimi |
| Erişim verme | TOU My |

:::image type="content" source="./media/require-tou/1234.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

**Koşullu erişim ilkenizi yapılandırmak için:**

1. **Yeni** sayfada, **ad** metin kutusuna, **Isabella için tou gerektir** yazın.

   ![Ad](./media/require-tou/71.png)

1. **Atama** bölümünde **Kullanıcılar ve gruplar** ' a tıklayın.

   :::image type="content" source="./media/require-tou/06.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

1. **Kullanıcılar ve gruplar** sayfasında:

   :::image type="content" source="./media/require-tou/24.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

   1. **Kullanıcı ve Grup Seç** ' e tıklayın ve ardından **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Seç** ’e tıklayın.
   1. **Seç** sayfasında, **Isabella Simonsen** ' ı seçin ve ardından **Seç** ' e tıklayın.
   1. **Kullanıcılar ve gruplar** sayfasında **bitti** ' ye tıklayın.
1. **Bulut uygulamaları** ' na tıklayın.

   :::image type="content" source="./media/require-tou/08.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

1. **Bulut uygulamaları** sayfasında:

   ![Bulut uygulamalarını seçin](./media/require-tou/26.png)

   1. **Uygulama Seç** ' e tıklayın.
   1. **Seç** ’e tıklayın.
   1. **Seç** sayfasında, **Microsoft Azure Yönetim** ' i seçin ve ardından **Seç** ' e tıklayın.
   1. **Bulut uygulamaları** sayfasında **bitti** ' ye tıklayın.
1. **Erişim denetimleri** bölümünde, **izin ver** ' e tıklayın.

   ![Erişim denetimleri](./media/require-tou/10.png)

1. **İzin** sayfasında:

   ![İzin verme](./media/require-tou/111.png)

   1. **Erişime izin ver** ’i seçin.
   1. **Tou 'Imi** seçin.
   1. **Seç** ’e tıklayın.
1. **Ilkeyi etkinleştir** bölümünde, **Açık** ' a tıklayın.

   ![İlkeyi etkinleştirme](./media/require-tou/18.png)

1. **Oluştur** 'a tıklayın.

## <a name="evaluate-a-simulated-sign-in"></a>Benzetimli bir oturum açmayı değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre muhtemelen, beklendiği gibi çalışıp çalışmadığını bilmeniz gerekir. İlk adım olarak, test kullanıcılarınız için bir oturum açma benzetimi yapmak üzere koşullu erişim ilke Aracı ' nı kullanın. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.  

**What If** ilkesi değerlendirme aracı 'nı başlatmak için, şunu ayarlayın:

- **Isabella Simonsen** Kullanıcı olarak
- Bulut uygulaması olarak **Microsoft Azure yönetimi**

**What If** tıklamak şunları gösteren bir benzetim raporu oluşturur:

- **Uygulanacak Ilkelerde** **Isabella için tou gerektir**
- **Tou** olarak **izin ver denetimleri** .

![Ne tür ilke aracı](./media/require-tou/79.png)

**Koşullu erişim ilkenizi değerlendirmek için:**

1. [Koşullu erişim ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüden **What If** ' ye tıklayın.  

   ![What If](./media/require-tou/14.png)

1. **Kullanıcılar** ' a tıklayın, **Isabella Simonsen** ' i seçin ve ardından **Seç** ' e tıklayın.

   ![Kullanıcı](./media/require-tou/15.png)

1. Bir bulut uygulaması seçmek için:

   :::image type="content" source="./media/require-tou/16.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

   1. **Bulut uygulamaları** ' na tıklayın.
   1. **Bulut uygulamaları sayfasında** , **Uygulama Seç** ' e tıklayın.
   1. **Seç** ’e tıklayın.
   1. **Seç** sayfasında, **Microsoft Azure Yönetim** ' i seçin ve ardından **Seç** ' e tıklayın.
   1. Bulut uygulamaları sayfasında **bitti** ' ye tıklayın.
1. **What If** ' ye tıklayın.

## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test etme

Önceki bölümde, benzetimli bir oturum açma değerlendirmesini öğrendiniz. Bir simülasyonu ek olarak, beklenen şekilde çalıştığından emin olmak için koşullu erişim ilkenizi da test etmelisiniz.

İlkenizi test etmek için, **Isabella Simonsen** test hesabınızı kullanarak [Azure Portal](https://portal.azure.com) oturum açmayı deneyin. Kullanım koşullarınızı kabul etmenizi gerektiren bir iletişim kutusu görmeniz gerekir.

:::image type="content" source="./media/require-tou/57.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısını ve koşullu erişim ilkesini silin:

- Bir Azure AD kullanıcısını nasıl silebileceğinizi bilmiyorsanız bkz. [Azure AD 'de kullanıcıları silme](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- İlkenizi silmek için ilkenizi seçin ve sonra hızlı erişim araç çubuğunda **Sil** ' e tıklayın.

    :::image type="content" source="./media/require-tou/33.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

- Kullanım koşullarınızı silmek için, seçin ve ardından araç çubuğunda en üstteki **terimi Sil** ' e tıklayın.

    :::image type="content" source="./media/require-tou/29.png" alt-text="Azure portal ekran görüntüsü. Isabella için T. U gerektir adlı bir ilkeyi tanımlayan bir bölme görünür." border="false":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirli uygulamalar için MFA gerektirme](../authentication/tutorial-enable-azure-mfa.md)