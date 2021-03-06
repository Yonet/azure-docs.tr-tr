---
title: Web uygulaması öğreticisi-FHıR için Azure API 'YI ayarlama
description: Bu öğreticide basit bir Web uygulaması dağıtma örneği gösterilmektedir. Bu ilk öğreticide, FHıR için Azure API 'sinin önkoşulları ve dağıtımı açıklanmaktadır
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.custom: devx-track-js
ms.openlocfilehash: cb183b5c8aff018d4dc73819b938b24ad0daa934
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91975799"
---
# <a name="deploy-javascript-app-to-read-data-from-fhir-service"></a>FHıR hizmetinden veri okumak için JavaScript uygulaması dağıtma
Bu öğreticide, bir FHıR hizmetinden veri okuyan küçük bir JavaScript uygulaması dağıtacaksınız. Bu öğreticideki adımlar şunlardır:
1. FHıR sunucusu dağıtma
1. Genel istemci uygulamasını kaydetme
1. Uygulamaya erişimi test etme
1. Bu FHıR verilerini okuyan bir Web uygulaması oluşturun

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici kümesine başlamadan önce aşağıdaki öğelere ihtiyacınız olacaktır:
1. Bir Azure aboneliği
1. Azure Active Directory kiracısı
1. [Postman](https://www.getpostman.com/) yüklendi

> [!NOTE]
> Bu öğreticide, FHıR hizmeti, Azure AD uygulaması ve Azure AD kullanıcıları aynı Azure AD kiracısında bulunur. Böyle bir durum söz konusu değilse, bu öğreticiyle birlikte yine de devam edebilir, ancak ek adımlar yapmak için başvurulan belgelerden bazılarını izlemeniz gerekebilir.

## <a name="deploy-azure-api-for-fhir"></a>FHIR için Azure API'sini dağıtma
Öğreticideki ilk adım, FHıR kurulumu için Azure API 'nizi doğru şekilde almanızı sağlar.

1. Henüz yapmadıysanız, [FHıR Için Azure API](fhir-paas-portal-quickstart.md)'sini dağıtın.
1. FHıR için Azure API 'niz dağıtıldıktan sonra, FHıR için Azure API 'nize gidip CORS ' yi seçerek [CORS](configure-cross-origin-resource-sharing.md) ayarlarını yapılandırın. 
    1. **Kaynakları** * olarak ayarla
    1. **Üst bilgileri** * olarak ayarla
    1. **Yöntemler**altında **Tümünü Seç** ' i seçin.
    1. **Maksimum yaşı** **600** olarak ayarlayın

## <a name="next-steps"></a>Sonraki Adımlar
Artık FHıR için Azure API 'niz olduğuna göre, bir ortak istemci uygulamasını kaydetmeye hazırsınız demektir.

>[!div class="nextstepaction"]
>[Genel istemci uygulamasını kaydetme](tutorial-web-app-public-app-reg.md)
