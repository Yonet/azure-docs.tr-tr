---
title: Azure Media Services ile video dosyaları akışı Node.js
description: Bu öğreticinin adımlarını izleyerek yeni bir Azure Media Services hesabı oluşturun, bir dosya kodlayın ve Azure Media Player için akışını yapın.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: mvc, devx-track-js
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 711db5135345fd9b69da9aaea5bc4dbb929d27c9
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98895144"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---nodejs"></a>Öğretici: URL 'yi temel alarak uzak bir dosyayı kodlayın ve videoyu akışa Node.js

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu öğreticide, Azure Media Services kullanarak çok çeşitli tarayıcılarda ve cihazlarda akış videolarının kodlanması ve başlatılması ne kadar kolay olduğunu gösterir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.

Bu makaledeki örnek, bir HTTPS URL 'SI aracılığıyla erişilebilir hale getirmek istediğiniz içeriği kodluyor. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Öğreticinin sonuna kadar video akışını sağlayabileceksiniz.  

![Videoyu yürütme](./media/stream-files-nodejs-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/download/) yüklensin
- [Media Services hesabı oluşturun](./create-account-howto.md).<br/>Kaynak grubu adı ve Media Services hesap adı için kullandığınız değerleri anımsadığınızdan emin olun.
- [Azure CLI Ile Access Azure Media Services API 'sindeki](./access-api-howto.md) adımları izleyin ve kimlik bilgilerini kaydedin. API 'ye erişmek için bunları kullanmanız gerekir.

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, akış Node.js örneğini içeren bir GitHub deposunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
 ```

Örnek, [Streamfilesörnek](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/master/AMSv3Samples/StreamFilesSample) klasöründe bulunur.

Projeyi indirdiğiniz [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L25) açın. Değerleri, `endpoint config` [API 'lere eriştiğiniz](./access-api-howto.md)kimlik bilgileriyle değiştirin.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Bir **dönüşüm** oluşturur (ilk olarak, belirtilen dönüştürmenin var olup olmadığını denetler). 
2. Kodlama **işinin** çıkışı olarak kullanılan bir çıktı **varlığı** oluşturur.
3. Bir HTTPS URL 'sini temel alan, **işin** girişini oluşturur.
4. Daha önce oluşturulmuş giriş ve çıktıyı kullanarak kodlama **işini** gönderir.
5. İşin durumunu denetler.
6. Bir **akış Bulucu** oluşturur.
7. Akış URL'leri oluşturur.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

1. Uygulama kodlanmış dosyaları indirir. Çıkış dosyalarının gitmesini istediğiniz ve [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L39) dosyasındaki **outputfolder** değişkeninin değerini güncelleştiren bir klasör oluşturun.
1. **Komut istemi**' ni açın, örnek dizinine gidin ve aşağıdaki komutları yürütün.

    ```
    npm install 
    node index.js
    ```

Çalışmayı tamamladıktan sonra benzer bir çıktı görmeniz gerekir:

![Yerel dizine indirilen üç dosyanın URL 'Lerini gösteren StreamFileSample örnek uygulamasının çıkışıyla çıkış içeren bir komut penceresinin ekran görüntüsü.](./media/stream-files-nodejs-quickstart/run.png)

## <a name="test-with-azure-media-player"></a>Azure Media Player ile test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir Web tarayıcısı açın ve adresine gidin [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) .
2. **URL:** kutusuna, uygulamayı çalıştırdığınızda aldığınız akış URL değerlerinden birini yapıştırın. 
 
     URL 'yi HLS, Dash veya kesintisiz biçimde yapıştırabilir ve Azure Media Player cihazınızda otomatik olarak kayıttan yürütmek için uygun bir akış protokolüne geçiş yapar.
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunuzda, bu öğretici için oluşturduğunuz Media Services ve depolama hesapları da dahil olmak üzere herhangi bir kaynağa ihtiyacınız yoksa, kaynak grubunu silin.

Aşağıdaki CLı komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="see-also"></a>Ayrıca bkz.

[İş hata kodları](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services kavramlar](concepts-overview.md)
