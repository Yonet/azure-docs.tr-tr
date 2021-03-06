---
title: 'Öğretici: bir Gatsby sitesini Azure statik Web Apps yayımlama'
description: Bu öğreticide, bir Gatsby uygulamasının Azure statik Web Apps nasıl dağıtılacağı gösterilmektedir.
services: static-web-apps
author: aaronpowell
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 05/08/2020
ms.author: aapowell
ms.custom: devx-track-js
ms.openlocfilehash: e24a2a19eb39e2c6c7612631ad98f95cb4c5b9ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91250173"
---
# <a name="tutorial-publish-a-gatsby-site-to-azure-static-web-apps-preview"></a>Öğretici: Azure statik Web Apps önizleme 'ye bir Gatsby sitesi yayımlama

Bu makalede, bir [Gatsby](https://gatsbyjs.org) Web uygulamasının nasıl oluşturulacağı ve [Azure statik Web Apps](overview.md)nasıl dağıtılacağı gösterilir. Nihai sonuç, uygulamanın nasıl oluşturulduğunu ve yayımlanabileceğini denetlemenizi sağlayan yeni bir statik Web Apps sitedir (ilişkili GitHub eylemleriyle birlikte).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> - Bir Gatsby uygulaması oluşturma
> - Azure statik Web Apps sitesi kurma
> - Gatsby uygulamasını Azure 'a dağıtma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. Bir hesabınız yoksa, [ücretsiz olarak bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/).
- GitHub hesabı. Bir hesabınız yoksa, [ücretsiz olarak bir hesap oluşturabilirsiniz](https://github.com/join).
- [Node.js](https://nodejs.org) yüklendi.

## <a name="create-a-gatsby-app"></a>Bir Gatsby uygulaması oluşturma

Gatsby komut satırı arabirimini (CLı) kullanarak bir Gatsby uygulaması oluşturun:

1. Bir Terminal açın
1. [NPX](https://www.npmjs.com/package/npx) aracını kullanarak Gatsby CLI ile yeni bir uygulama oluşturun. Bu birkaç dakika sürebilir.

   ```bash
   npx gatsby new static-web-app
   ```

1. Yeni oluşturulan uygulamaya git

   ```bash
   cd static-web-app
   ```

1. Git deposu başlatma

   ```bash
   git init
   git add -A
   git commit -m "initial commit"
   ```

## <a name="push-your-application-to-github"></a>Uygulamanızı GitHub 'a gönderme

Yeni bir Azure statik Web Apps kaynağı oluşturmak için GitHub 'da bir depoya sahip olmanız gerekir.

1. [https://github.com/new](https://github.com/new)Adlı **Gatsby-static-Web-App**' den boş bir GitHub deposu oluşturun (Benioku dosyası oluşturmayın).

1. Ardından, yeni oluşturduğunuz GitHub deposunu yerel deponuza uzak olarak ekleyin. Aşağıdaki komutta yer tutucu yerine GitHub Kullanıcı adınızı eklediğinizden emin olun `<YOUR_USER_NAME>` .

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/gatsby-static-web-app
   ```

1. Yerel deponuzu GitHub 'a iletin.

   ```bash
   git push --set-upstream origin master
   ```

## <a name="deploy-your-web-app"></a>Web uygulamanızı dağıtın

Aşağıdaki adımlarda, yeni bir statik site uygulamasının nasıl oluşturulacağı ve bir üretim ortamına nasıl dağıtılacağı gösterilmektedir.

### <a name="create-the-application"></a>Uygulama oluşturma

1. [Azure Portal](https://portal.azure.com) gidin
1. **Kaynak oluştur** seçeneğine tıklayın
1. **Statik Web Uygulamaları** için arama yapın
1. **Statik Web Uygulamaları (Önizleme)** seçeneğine tıklayın
1. **Oluştur** seçeneğine tıklayın

   :::image type="content" source="./media/publish-gatsby/create-in-portal.png" alt-text="Portalda statik bir Web Apps (Önizleme) oluşturma":::

1. _Abonelik_için, listelenen aboneliği kabul edin veya açılan listeden yeni bir tane seçin.

1. _Kaynak grubu_' nda **Yeni**' yi seçin. _Yeni kaynak grubu adı_' nda, **Gatsby-static-Web-App** girin ve **Tamam**' ı seçin.

1. Ardından, **ad** kutusunda uygulamanız için bir ad. Geçerli karakterler `a-z` ,, `A-Z` ve `0-9` içerir `-` .

1. _Bölge_için bir kullanılabilir bölge seçin.

1. _SKU_için **ücretsiz**' i seçin.

   :::image type="content" source="./media/publish-gatsby/basic-app-details.png" alt-text="Portalda statik bir Web Apps (Önizleme) oluşturma":::

1. **GitHub Ile oturum aç** düğmesine tıklayın.

1. Depoyu oluşturduğunuz **kuruluşu** seçin.

1. _Depo_ olarak **Gatsby-static-Web-App** ' i seçin.

1. _Dal_ için **ana öğe**seçin.

   :::image type="content" source="./media/publish-gatsby/completed-github-info.png" alt-text="Portalda statik bir Web Apps (Önizleme) oluşturma":::

### <a name="build"></a>Yapı

Sonra, yapı işleminin uygulamanızı derlemek için kullandığı yapılandırma ayarlarını ekleyin.

1. Derleme yapılandırmasını düzenlemek için **İleri: Derleme >** düğmesine tıklayın

1. GitHub eylemlerdeki adımın ayarlarını yapılandırmak için _uygulama konumunu_ olarak ayarlayın **/** .

1. _Uygulama yapıtı konumunu_ **ortak**olarak ayarlayın.

   Şu anda bir API dağıtmadığından, _API konumu_ için bir değer gerekli değildir.

   :::image type="content" source="./media/publish-gatsby/build-details.png" alt-text="Portalda statik bir Web Apps (Önizleme) oluşturma":::

### <a name="review-and-create"></a>Gözden geçir ve oluştur

1. Ayrıntıların tümünün doğru olduğunu doğrulamak için **gözden geçir + oluştur** düğmesine tıklayın.

1. App Service statik Web uygulamasının oluşturulmasını başlatmak ve dağıtım için bir GitHub eylemi sağlamak için **Oluştur** ' a tıklayın.

1. Dağıtım tamamlandıktan sonra, kaynak ' a **gidin**.

1. Kaynak ekranında, dağıtılan uygulamanızı açmak için _URL_ bağlantısına tıklayın. GitHub eyleminin tamamlanabilmesi için bir dakika veya iki dakika beklemeniz gerekebilir.

   :::image type="content" source="./media/publish-gatsby/deployed-app.png" alt-text="Portalda statik bir Web Apps (Önizleme) oluşturma":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cleanup-resource](../../includes/static-web-apps-cleanup-resource.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel etki alanı ekleme](custom-domain.md)
