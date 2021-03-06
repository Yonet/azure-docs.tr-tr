---
title: Yerel git deposundan dağıtma
description: Azure App Service için yerel git dağıtımını nasıl etkinleştireceğinizi öğrenin. Yerel makinenizden kod dağıtmanın en basit yöntemlerinden biri.
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.topic: article
ms.date: 06/18/2019
ms.reviewer: dariac
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 26fd8bc73fad3ea313641fc4b1e0f454ee2c0813
ms.sourcegitcommit: fa807e40d729bf066b9b81c76a0e8c5b1c03b536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97347787"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure App Service için yerel git dağıtımı

Bu nasıl yapılır Kılavuzu, uygulamanızı yerel bilgisayarınızdaki bir git deposundan [Azure App Service](overview.md) için nasıl dağıtacağınızı gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzundaki adımları takip etmek için:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Git 'ı yükler](https://www.git-scm.com/downloads).
  
- Dağıtmak istediğiniz koda sahip yerel bir git deposuna sahip olabilirsiniz. Örnek bir depoyu indirmek için yerel Terminal pencerenizde aşağıdaki komutu çalıştırın:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="deploy-with-kudu-build-server"></a>Kudu derleme sunucusu ile dağıtma

Kudu App Service yapı sunucusuyla uygulamanız için yerel git dağıtımını etkinleştirmenin en kolay yolu Azure Cloud Shell kullanmaktır. 

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>Dağıtım URL 'sini al

Mevcut bir uygulama için yerel git dağıtımını etkinleştirmek üzere URL 'YI almak için Cloud Shell çalıştırın [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source#az-webapp-deployment-source-config-local-git) . \<app-name>Ve ' i uygulamanızın \<group-name> adlarıyla ve Azure Kaynak grubuyla değiştirin.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```
> [!NOTE]
> Linux App-Service-plan kullanıyorsanız şu parametreyi eklemeniz gerekir:--Runtime Python | 3.7


Ya da git özellikli yeni bir uygulama oluşturmak için, [`az webapp create`](/cli/azure/webapp#az-webapp-create) Cloud Shell `--deployment-local-git` parametresiyle çalıştırın. , \<app-name> \<group-name> Ve ' yi \<plan-name> Yeni git uygulamanızın adı, Azure kaynak grubu ve Azure App Service planı ile değiştirin.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

Her iki komut şöyle bir URL döndürür: `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git` . Uygulamanızı bir sonraki adımda dağıtmak için bu URL 'YI kullanın.

Bu hesap düzeyi URL 'YI kullanmak yerine, uygulama düzeyinde kimlik bilgilerini kullanarak yerel git 'i de etkinleştirebilirsiniz. Azure App Service her uygulama için bu kimlik bilgilerini otomatik olarak oluşturur. 

Cloud Shell aşağıdaki komutu çalıştırarak uygulama kimlik bilgilerini alın. \<app-name>Ve ' i uygulamanızın \<group-name> adı ve Azure Kaynak grubu adıyla değiştirin.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Uygulamanızı bir sonraki adımda dağıtmak için döndüren URL 'YI kullanın.

### <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma

1. Yerel git deponuza yerel bir Terminal penceresi açın ve bir Azure uzak ekleyin. Aşağıdaki komutta, yerine \<url> önceki adımdan aldığınız dağıtım kullanıcısına özgü URL veya uygulamaya özel URL ile değiştirin.
   
   ```bash
   git remote add azure <url>
   ```
   
1. İle Azure 'a gönderin `git push azure master` . 
   
1. **Git kimlik bilgileri Yöneticisi** penceresinde, Azure oturum açma parolanızı değil, [dağıtım Kullanıcı parolanızı](#configure-a-deployment-user)girin.
   
1. Çıktıyı gözden geçirin. For ASP.NET için MSBuild, `npm install` Node.js için ve Python için bkz. çalışma zamanına özgü Otomasyon `pip install` . 
   
1. İçeriğin dağıtıldığını doğrulamak için Azure portal uygulamanıza gidin.

## <a name="deploy-with-azure-pipelines-builds"></a>Azure Pipelines Derlemeleriyle dağıtma

Hesabınız gerekli izinlere sahipse, uygulamanız için yerel git dağıtımını etkinleştirmek üzere Azure Pipelines (Önizleme) ayarlayabilirsiniz. 

- Azure hesabınızın Azure Active Directory yazma ve hizmet oluşturma izinlerine sahip olması gerekir. 
  
- Azure hesabınızın, Azure aboneliğinizde **sahip** rolü olmalıdır.

- Kullanmak istediğiniz Azure DevOps projesinde bir yönetici olmanız gerekir.

Azure Pipelines (Önizleme) ile uygulamanız için yerel git dağıtımını etkinleştirmek için:

1. [Azure Portal](https://portal.azure.com), **uygulama hizmetleri**' ni arayıp seçin. 

1. Azure App Service uygulamanızı seçin ve Sol menüdeki **Dağıtım Merkezi** ' ni seçin.
   
1. **Dağıtım Merkezi** sayfasında **Yerel git**' i seçin ve ardından **devam**' ı seçin. 
   
   ![Yerel git ' i seçin ve ardından devam ' ı seçin.](media/app-service-deploy-local-git/portal-enable.png)
   
1. **Yapı sağlayıcısı** sayfasında **Azure Pipelines (Önizleme)** öğesini seçin ve ardından **devam**' ı seçin. 
   
   ![Azure Pipelines (Önizleme) öğesini seçin ve ardından devam ' ı seçin.](media/app-service-deploy-local-git/pipeline-builds.png)

1. **Yapılandır** sayfasında, yeni bir Azure DevOps organizasyonu yapılandırın veya mevcut bir kuruluş belirtip **devam**' ı seçin.
   
   > [!NOTE]
   > Mevcut Azure DevOps kuruluşunuz listede yoksa Azure aboneliğinize bağlamanız gerekebilir. Daha fazla bilgi için bkz. [CD yayın işlem hattınızı tanımlama](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
   
1. App Service planı [fiyatlandırma katmanınıza](https://azure.microsoft.com/pricing/details/app-service/plans/)bağlı olarak, **hazırlama sayfasına dağıt** sayfasına bakabilirsiniz. [Dağıtım yuvalarının](deploy-staging-slots.md)etkinleştirilip etkinleştirilmeyeceğini seçin ve ardından **devam**' ı seçin.
   
1. **Özet** sayfasında, ayarları gözden geçirin ve **son**' u seçin.
   
1. Azure işlem hattı kullanılabilir olduğunda, git deposu URL 'sini bir sonraki adımda kullanmak üzere **Dağıtım Merkezi** sayfasından kopyalayın. 
   
   ![Git deposu URL 'sini Kopyala](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. Yerel Terminal pencerenizde yerel git deponuza bir Azure uzak ekleyin. Komutta, yerine \<url> önceki adımdan aldığınız git deposunun URL 'siyle değiştirin.
   
   ```bash
   git remote add azure <url>
   ```
   
1. İle Azure 'a gönderin `git push azure master` . 
   
1. **Git kimlik bilgileri Yöneticisi** sayfasında, VisualStudio.com Kullanıcı adınızla oturum açın. Diğer kimlik doğrulama yöntemleri için bkz. [Azure DevOps Services kimlik doğrulamasına genel bakış](/vsts/git/auth-overview?view=vsts).
   
1. Dağıtım tamamlandıktan sonra, konumundaki derleme ilerlemesini `https://<azure_devops_account>.visualstudio.com/<project_name>/_build` ve dağıtım ilerleme durumunu görüntüleyin `https://<azure_devops_account>.visualstudio.com/<project_name>/_release` .
   
1. İçeriğin dağıtıldığını doğrulamak için Azure portal uygulamanıza gidin.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Dağıtım sorunlarını giderme

Azure 'da bir App Service uygulamasına yayımlamak için git kullandığınızda aşağıdaki genel hata iletilerini görebilirsiniz:

|İleti|Nedeni|Çözüm
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|Uygulama çalışır durumda değil.|Uygulamayı Azure portal başlatın. Web uygulaması durdurulduğunda git dağıtımı kullanılamaz.|
|`Couldn't resolve host 'hostname'`|' Azure ' uzak için adres bilgileri yanlış.|`git remote -v`ILIŞKILI URL ile birlikte tüm uzaktan kumandalar listelemek için komutunu kullanın. ' Azure ' uzak için URL 'nin doğru olduğundan emin olun. Gerekirse, doğru URL 'YI kullanarak bu uzak kopyayı kaldırın ve yeniden oluşturun.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'main'.`|Sırasında bir dal belirtmediniz veya ' `git push` `push.default` de değer ayarlamadıysanız `.gitconfig` .|`git push`Ana dalı belirterek yeniden çalıştırın: `git push azure master` .|
|`src refspec [branchname] does not match any.`|' Azure ' uzak üzerinde ana dışında bir dala gönderim çalıştınız.|`git push`Ana dalı belirterek yeniden çalıştırın: `git push azure master` .|
|`RPC failed; result=22, HTTP code = 5xx.`|HTTPS üzerinden büyük bir git deposu göndermeye çalışırsanız bu hata oluşabilir.|Daha büyük olması için yerel makinedeki git yapılandırmasını değiştirin `postBuffer` . Örneğin: `git config --global http.postBuffer 524288000`.|
|`Error - Changes committed to remote repository but your web app not updated.`|Bir Node.js uygulamasını, ek gerekli modülleri belirten _package.js_ bir dosya ile dağıttınız.|Hatada `npm ERR!` daha fazla bağlam için bu hatadan önce hata iletilerini gözden geçirin. Aşağıda bu hatanın bilinen nedenleri ve ilgili `npm ERR!` iletiler verilmiştir:<br /><br />**Dosyada hatalı biçimlendirilmiş package.js**: `npm ERR! Couldn't read dependencies.`<br /><br />**Yerel modülün Windows için ikili bir dağıtımı yok**:<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />veya <br />`npm ERR! [modulename@version] preinstall: \make || gmake\ `|

## <a name="additional-resources"></a>Ek kaynaklar

- [Proje kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
- [Azure App Service için sürekli dağıtım](deploy-continuous-deployment.md)
- [Örnek: bir Web uygulaması oluşturma ve yerel bir git deposundan kod dağıtma (Azure CLı)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Örnek: bir Web uygulaması oluşturma ve yerel bir git deposundan kod dağıtma (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
