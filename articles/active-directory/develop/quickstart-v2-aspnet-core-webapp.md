---
title: "Hızlı başlangıç: Microsoft 'a ASP.NET Core Web uygulamasına oturum açma ekleme | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir uygulamanın OpenID Connect kullanarak bir ASP.NET Core Web uygulamasında Microsoft oturum açma işlemini nasıl uyguladığını öğrenirsiniz.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/11/2020
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: 9e9c4773b8f50d3d7b47f0eb94c039a42bee1d04
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99584457"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hızlı başlangıç: Microsoft 'a ASP.NET Core Web uygulamasına oturum açma ekleme

Bu hızlı başlangıçta, bir ASP.NET Core Web uygulamasının kullanıcılara herhangi bir Azure Active Directory (Azure AD) kuruluştan nasıl oturum açıp çalıştırabileceğinizi gösteren bir kod örneği indirip çalıştırırsınız.  

Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.

> [!div renderon="docs"]
> ## <a name="prerequisites"></a>Önkoşullar
>
> * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) veya [Visual Studio Code](https://code.visualstudio.com/)
> * [.NET Core SDK 3.1 +](https://dotnet.microsoft.com/download)
>
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. <a href="https://aka.ms/aspnetcore2-1-aad-quickstart-v2/" target="_blank">Azure portal uygulama kayıtları <span class="docon docon-navigate-external x-hidden-focus"></span> </a> hızlı başlangıç deneyimine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. <a href="https://portal.azure.com/" target="_blank">Azure Portal <span class="docon docon-navigate-external x-hidden-focus"></span> </a>oturum açın.
> 1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. **Azure Active Directory**'yi bulun ve seçin.
> 1. **Yönet** altında   >  **Yeni kayıt** uygulama kayıtları ' yi seçin.
> 1. Uygulamanız için bir **ad** girin (örneğin,) `AspNetCore-Quickstart` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
> 1. **Yeniden yönlendirme URI 'si** girin `https://localhost:44321/` .
> 1. **Kaydet**’i seçin.
> 1. **Yönet** altında **kimlik doğrulaması**' nı seçin.
> 1. **Yeniden yönlendirme URI 'leri** altında **URI Ekle**' yi seçin ve ardından girin `https://localhost:44321/signin-oidc` .
> 1. **Ön kanal oturum kapatma URL 'sini** girin `https://localhost:44321/signout-oidc` .
> 1. **Örtük verme ve karma akışlar** altında **Kimlik belirteçleri**' ni seçin.
> 1. **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için ve için bir **yeniden YÖNLENDIRME URI** 'si ve `https://localhost:44321/` `https://localhost:44321/signin-oidc` bir **ön kanal oturum kapatma URL 'si** ekleyin `https://localhost:44321/signout-oidc` . İstek KIMLIĞI belirteçleri yetkilendirme uç noktası tarafından verilecek.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-aspnet-core-project"></a>2. Adım: ASP.NET Core projenizi Indirin

> [!div renderon="docs"]
> [ASP.NET Core çözümünü indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div renderon="portal" class="sxs-lookup"]
> Projeyi çalıştırın.

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor.
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-configure-your-aspnet-core-project"></a>3. Adım: ASP.NET Core projenizi yapılandırma
> 1. . Zip arşivini sürücünüzün köküne yakın bir yerel klasöre ayıklayın. Örneğin, *C:\Azure-Samples*.
> 1. Visual Studio 2019 ' de çözümü açın.
> 1. appsettings.jsdosya *üzerinde* açın ve aşağıdakileri değiştirin:
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here",
>    "TenantId": "common",
>    ```
>
>    - `Enter_the_Application_Id_here`Azure Portal kaydettiğiniz uygulamanın **uygulama (ISTEMCI) kimliğiyle** değiştirin. Uygulamanın **genel bakış** sayfasında **uygulama (istemci) kimliğini** bulabilirsiniz.
>    - `common`Aşağıdakilerden biriyle değiştirin:
>       - Uygulamanız **yalnızca bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri **Dizin (kiracı) kimliği** (GUID) veya **kiracı adı** (örneğin,) ile değiştirin `contoso.onmicrosoft.com` . Uygulamanın **genel bakış** sayfasında **Dizin (kiracı) kimliğini** bulabilirsiniz.
>       - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>       - Uygulamanız **tüm Microsoft hesabı kullanıcıları** destekliyorsa, bu değeri şu şekilde bırakın `common`
>
> Bu hızlı başlangıçta, dosyadaki *appsettings.js* başka bir değeri değiştirmeyin.
>
> #### <a name="step-4-build-and-run-the-application"></a>4. Adım: Uygulamayı derleme ve çalıştırma
>
> Hata **ayıklamayı başlatmak**> **hata** ayıklama menüsünü seçerek veya anahtara basarak uygulamayı Visual Studio 'da derleyin ve çalıştırın `F5` .
>
> Kimlik bilgileriniz istenir ve ardından uygulamanızın gerektirdiği izinleri onaylıyoruz. Onay isteminde **kabul et** ' i seçin.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-01-consent.png" alt-text="> kullanıcıdan uygulamanın istediği izinleri gösteren onay iletişim kutusu":::
>
> İstenen izinlere katıldıktan sonra uygulama, Azure Active Directory kimlik bilgilerinizi kullanarak başarıyla oturum açtığınızı gösterir.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-02-signed-in.png" alt-text="Çalışan Web uygulamasını ve oturum açan kullanıcıyı görüntüleyen Web tarayıcısı":::

## <a name="more-information"></a>Daha fazla bilgi

Bu bölüm, kullanıcıların oturum açması için gereken koda genel bir bakış sağlar. Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenlerin ve ayrıca mevcut bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız yararlı olabilir.

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft. AspNetCore. Authentication* ara yazılımı, `Startup` barındırma işlemi başlatıldığında yürütülen bir sınıfı kullanır:

```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"));

      services.AddControllersWithViews(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
          options.Filters.Add(new AuthorizeFilter(policy));
      });
      services.AddRazorPages()
          .AddMicrosoftIdentityUI();
  }
```

`AddAuthentication()`Yöntemi, hizmetini tarayıcı senaryolarında kullanılan ve kimlik doğrulama tabanlı kimlik doğrulaması ekleyecek şekilde yapılandırır ve bunu OpenID Connect olarak ayarlar.

İçeren çizgi, `.AddMicrosoftIdentityWebApp` uygulamanıza Microsoft Identity platform kimlik doğrulamasını ekler. Daha sonra `AzureAD` yapılandırma dosyasında *appsettings.js* bölümünde yer alan bilgileri temel alarak Microsoft Identity platformunu kullanarak oturum açmak için yapılandırılır:

| *appsettings.js* anahtarda | Description                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | Azure portal kayıtlı uygulamanın **uygulama (istemci) kimliği** .                                                                                       |
| `Instance`             | Kullanıcının kimlik doğrulaması için güvenlik belirteci hizmeti (STS) uç noktası. Bu değer genellikle `https://login.microsoftonline.com/` Azure genel bulutu 'nı gösterir. |
| `TenantId`             | İş veya okul hesaplarıyla veya Microsoft kişisel hesaplarıyla kullanıcıların oturum açması için kiracınızın adı veya kiracı KIMLIĞI (GUID) veya *ortak* .                             |

`Configure()`Yöntemi, `app.UseAuthentication()` adlandırılmış işlevlerini etkinleştiren iki önemli yöntem içerir `app.UseAuthorization()` . Ayrıca `Configure()` yönteminde, Microsoft Identity Web 'in yollarını en az bir çağrısıyla `endpoints.MapControllerRoute()` veya çağrısına kaydetmeniz gerekir `endpoints.MapControllers()` .

```csharp
app.UseAuthentication();
app.UseAuthorization();

app.UseEndpoints(endpoints =>
{

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});

// endpoints.MapControllers(); // REQUIRED if MapControllerRoute() isn't called.
```

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Özniteliğini kullanarak bir denetleyiciyi veya denetleyici yöntemlerini koruyabilirsiniz `[Authorize]` . Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek denetleyiciye veya yöntemlere erişimi kısıtlar. Bu, kullanıcının kimlik doğrulaması yapılmadığı takdirde denetleyiciye erişmek için kimlik doğrulama sınaması başlatılabileceği anlamına gelir.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu ASP.NET Core öğreticisini içeren GitHub deposu, nasıl yapılacağını gösteren yönergeler ve daha fazla kod örneği içerir:

- Yeni bir ASP.NET Core Web uygulamasına kimlik doğrulaması ekleme
- Microsoft Graph, diğer Microsoft API 'Lerini veya kendi Web API 'lerinizi çağırın
- Yetkilendirme Ekle
- Ulusal bulutlarda veya sosyal kimliklerle Kullanıcı oturum açma

> [!div class="nextstepaction"]
> [GitHub 'da Web uygulaması öğreticileri ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
