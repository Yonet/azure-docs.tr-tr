---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: erhopf
ms.openlocfilehash: 3f1a24db459cabd65d9ce17b89105c3b9ab8abb7
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188491"
---
1. Visual Studio 2019’u başlatın.

1. **.NET platformlar arası geliştirme** iş yükünün bulunduğundan emin olun. Visual **Tools** Studio  >  yükleyicisi 'ni açmak için Visual Studio menü çubuğunda Araçlar **ve Özellikler Al ' ı** seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın.

   ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-net-core-workload.png)

   Etkin değilse **.NET Core platformlar arası geliştirme**'nin yanındaki kutuyu işaretleyin ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenme birkaç saniye sürer.

1. Yeni bir Visual C# .NET Core Konsol Uygulaması oluşturun. **Yeni Proje** iletişim kutusunda, sol panelden **Yüklü** > **Visual C#** > **.NET Core**'u genişletin. Ardından **Console App (.NET Core)** seçeneğine tıklayın. Proje adı olarak *helloworld* girin.

   ![Yeni Proje iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C# konsol uygulaması oluşturma (.NET Core)")

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'i seçin.

   ![Çözüm Gezgini ekran görüntüsü, çözüm için NuGet Paketlerini Yönet vurgulanmış](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönet")

1. Sağ üst köşede, **paket kaynağı** alanında **NuGet.org**' yi seçin. `Microsoft.CognitiveServices.Speech` Paketi arayın ve **HelloWorld** projesine yükler.

   !["Çözüme yönelik paketleri Yönet" iletişim kutusunu gösteren ekran görüntüsü.](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet paketini yükler")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

   ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Lisansı kabul et")

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.
