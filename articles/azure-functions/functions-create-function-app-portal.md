---
title: Azure portalında ilk işlevinizi oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: devx-track-csharp, mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: 63e9c87d1d94d6b803c27862bc9f2755e02f3111
ms.sourcegitcommit: 706e7d3eaa27f242312d3d8e3ff072d2ae685956
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99980953"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Azure portalında ilk işlevinizi oluşturma

Azure Işlevleri, öncelikle bir sanal makine (VM) oluşturmak veya bir Web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda çalıştırmanızı sağlar. Bu makalede, Azure Işlevleri 'ni kullanarak Azure portal bir "Hello World" HTTP tetikleyici işlevi oluşturma hakkında bilgi edineceksiniz.

>[!NOTE]
>Portal içi düzenlemesi yalnızca JavaScript, PowerShell, TypeScript ve C# betik işlevleri için desteklenir.<br><br>C# sınıf kitaplığı, Java ve Python işlevleri için, portalda işlev uygulamasını oluşturabilirsiniz, ancak aynı zamanda işlevleri yerel olarak oluşturmanız ve ardından bunları Azure 'da yayımlamanız gerekir. 

Bunun yerine, [işlevlerinizi yerel olarak geliştirmenizi](functions-develop-local.md) ve Azure 'da bir işlev uygulamasında yayımlamayı öneririz.  
Seçtiğiniz yerel geliştirme ortamı ve diliniz ile çalışmaya başlamak için aşağıdaki bağlantılardan birini kullanın:

| Visual Studio Code | Terminal/komut istemi | Visual Studio |
| --- | --- | --- |
|  &bull;&nbsp;[C ile çalışmaya başlama #](./create-first-function-vs-code-csharp.md)<br/>&bull;&nbsp;[Java kullanmaya başlayın](./create-first-function-vs-code-java.md)<br/>&bull;&nbsp;[JavaScript ile çalışmaya başlama](./create-first-function-vs-code-node.md)<br/>&bull;&nbsp;[PowerShell 'i kullanmaya başlama](./create-first-function-vs-code-powershell.md)<br/>&bull;&nbsp;[Python ile çalışmaya başlama](./create-first-function-vs-code-python.md) |&bull;&nbsp;[C ile çalışmaya başlama #](./create-first-function-cli-csharp.md)<br/>&bull;&nbsp;[Java kullanmaya başlayın](./create-first-function-cli-java.md)<br/>&bull;&nbsp;[JavaScript ile çalışmaya başlama](./create-first-function-cli-node.md)<br/>&bull;&nbsp;[PowerShell 'i kullanmaya başlama](./create-first-function-cli-powershell.md)<br/>&bull;&nbsp;[Python ile çalışmaya başlama](./create-first-function-cli-python.md) | [C ile çalışmaya başlama #](functions-create-your-first-function-visual-studio.md) |

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Sonra, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-an-http-trigger-function"></a><a name="create-function"></a>HTTP tetikleyici işlevi oluşturma

1. **İşlevler** penceresinin sol menüsünde **işlevler**' i seçin ve ardından üst menüden **Ekle** ' yi seçin. 
 
1. **Yeni işlev** penceresinden **http tetikleyicisi**' ni seçin.

    ![HTTP tetikleyici işlevini seçin](./media/functions-create-first-azure-function/function-app-select-http-trigger.png)

1. **Yeni işlev** penceresinde, **yeni işlev** için varsayılan adı kabul edin veya yeni bir ad girin. 

1. **Yetkilendirme düzeyi** açılan listesinden **anonim** ' i seçin ve ardından **işlev oluştur**' u seçin.

    Azure, HTTP tetikleyici işlevini oluşturur. Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni HTTP tetikleyici işlevinizde, sol menüden **Code + test** ' i seçin ve ardından üstteki menüden **Işlev URL 'sini al** ' ı seçin.

    ![İşlev URL 'sini Al ' ı seçin](./media/functions-create-first-azure-function/function-app-select-get-function-url.png)

1. **İşlev URL 'Sini al** iletişim kutusunda, açılan listeden **varsayılan** ' ı seçin ve ardından **Panoya Kopyala** simgesini seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesi değerini `?name=<your_name>` Bu URL 'nin sonuna ekleyin ve isteği çalıştırmak Için ENTER 'a basın. 

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    İstek URL 'SI bir [erişim anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys) () içeriyorsa `?code=...` , işlevi oluştururken **anonim** erişim düzeyi yerine **işlevi** seçersiniz demektir. Bu durumda, bunun yerine sonuna eklemeniz gerekir `&name=<your_name>` .

1. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. İzleme çıktısını görmek için, portalda **Code + test** sayfasına dönüp sayfanın altındaki **Günlükler** okunu genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
