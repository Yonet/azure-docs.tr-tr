---
title: B2B kurumsal tümleştirme için XML 'i doğrulama
description: Enterprise Integration Pack ile Azure Logic Apps şemaları kullanarak XML doğrulama
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 10/21/2019
ms.openlocfilehash: ff21b059e712489c1914b2d12c6aa6a3d78d66d3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "74792171"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps’te B2B kurumsal tümleştirme için XML doğrulama

B2B senaryolarında genellikle, bir anlaşmada bulunan ticari iş ortaklarının, herhangi bir veri işleme başlamadan önce, değiş tokuş ettikleri iletilerin geçerli olduğundan emin olması gerekir. Enterprise Integration Pack ile kullanılabilen XML doğrulama eylemini kullanarak önceden tanımlanmış bir şemaya karşı belgeleri doğrulayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* XML doğrulama eylemini kullanmak istediğiniz boş veya mevcut bir mantıksal uygulama. Logic Apps 'e yeni başladıysanız, [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: Ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)' yı gözden geçirin.

* Azure aboneliğinizle ilişkili bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , XML doğrulama eylemini kullanmayı planladığınız mantıksal uygulamayla BAĞLANTıLıDıR ve XML içeriğini doğrulamak için kullanmak istediğiniz şemayı içerir. Hem mantıksal uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

## <a name="add-xml-validation-action"></a>XML doğrulama eylemi Ekle

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Boş bir mantıksal uygulamanız varsa, mantıksal uygulama tasarımcısında, ara kutusuna `HTTP request` filtreniz olarak girin ve **bir http isteği alındığında** tetikleyiciyi seçin. Aksi halde, bir sonraki adıma devam edin.

1. İş akışınızın son adımında **yeni adım**' ı seçin.

   Varolan adımlar arasında bir eylem eklemek için, işaretçinizi, artı işareti () görünecek şekilde bu adımları bağlayan oka taşıyın **+** . Bu artı işaretini seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, **yerleşik**' i seçin. Arama kutusuna `xml validation` filtreniz olarak yazın. Eylemler listesinden **XML doğrulaması**' nı seçin.

   !["XML doğrulaması" eylemini bul ve Seç](./media/logic-apps-enterprise-integration-xml-validation/select-xml-validation-action.png)

1. Doğrulamak istediğiniz XML içeriğini belirtmek için, **içerik** kutusunun içine tıklayarak dinamik içerik listesinin görünmesini sağlayın.

   ![Dinamik içerik listesini aç](./media/logic-apps-enterprise-integration-xml-validation/open-dynamic-content-list.png)

   Dinamik içerik listesi, iş akışında yer alan önceki adımlardan çıkışları temsil eden özellik belirteçlerini gösterir. Listede beklenen bir özellik gösterilmiyorsa, **daha fazla**göster ' i seçebileceğiniz tetikleyiciyi veya eylem başlığını kontrol edin.

1. Dinamik içerik listesinden, doğrulamak istediğiniz içeriğe sahip olan özelliği seçin.

   Bu örnek, tetikleyiciden gelen **gövde** çıkışını seçer.

   ![Doğrulanacak içeriği seçin](./media/logic-apps-enterprise-integration-xml-validation/select-content-to-validate.png)

1. Doğrulama için kullanmak istediğiniz şemayı belirtmek için, **şema adı** listesini açın ve bağlı tümleştirme hesabınıza eklediğiniz doğrulama şemasını seçin.

   ![Doğrulama için kullanılacak şemayı seçin](./media/logic-apps-enterprise-integration-xml-validation/select-validation-schema.png)

1. Mantıksal uygulamanızı kaydedin.

   Artık doğrulama ayarınızı ayarlamayı tamamladınız. Gerçek bir dünya uygulamasında, doğrulanan verileri SalesForce gibi bir iş kolu (LOB) uygulamasında depolamak isteyebilirsiniz. Doğrulanan çıktıyı Salesforce 'a göndermek için bir eylem ekleyin.

1. Doğrulama eyleminizi test etmek için, mantıksal uygulamanızın iş akışını tetiklemek üzere bir istek gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin