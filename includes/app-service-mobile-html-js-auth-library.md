---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 5fe9fe8ced675f68161f0df9f2665b47f9d47ac5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188882"
---
### <a name="how-to-authenticate-with-a-provider-server-flow"></a><a name="server-auth"></a>Nasıl yapılır: Bir sağlayıcı ile (Sunucu Akışı) kimlik doğrulaması
Mobile Apps hizmetinin uygulamanızdaki kimlik doğrulama işlemini yönetmesi için kimlik sağlayıcınıza uygulamanızı kaydetmeniz gerekir. Ardından, Azure App Service'te sağlayıcınız tarafından verilen uygulama kimliği ile parolasını yapılandırmanız gerekir.
Daha fazla bilgi için [Uygulamanıza kimlik doğrulaması ekleme](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md) öğreticisine bakın.

Kimlik sağlayıcınızı kaydettikten sonra sağlayıcınızın adıyla `.login()` yöntemini çağırın. Örneğin, Facebook ile oturum açabilmek için aşağıdaki kodu kullanın:

```javascript
client.login("facebook").done(function (results) {
     alert("You are now signed in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

'aad', 'facebook', 'google', 'microsoftaccount' ve 'twitter', sağlayıcı için geçerli değerlerdir.

> [!NOTE]
> Google Kimlik Doğrulaması şu anda Sunucu Akışı ile çalışmamaktadır.  Google kimlik doğrulamasını yapmak için bir [istemci akışı yöntemi](#client-auth) kullanmanız gerekir.

Bu durumda, OAuth 2.0 kimlik doğrulama akışını Azure App Service yönetir.  Seçili sağlayıcının oturum açma sayfasını görüntüler ve kimlik sağlayıcısıyla başarılı oturum açtıktan sonra bir Uygulama Hizmeti kimlik doğrulama belirteci oluşturur. Oturum açma işlevi tamamlandığında, hem kullanıcı kimliğini hem de App Service kimlik doğrulama belirtecini sırasıyla userId ve authenticationToken alanlarında ortaya çıkaran bir JSON nesnesi döndürür. Bu belirteç önbelleğe alınabilir süresi sona erene kadar yeniden kullanılabilir.

### <a name="how-to-authenticate-with-a-provider-client-flow"></a><a name="client-auth"></a>Nasıl yapılır: Bir sağlayıcı ile (İstemci Akışı) kimlik doğrulaması

Uygulamanız kimlik sağlayıcısı ile bağımsız olarak da iletişim kurabilir ve sonra döndürülen belirteci kimlik doğrulaması için App Service’e döndürebilir. Bu istemci akışı, kullanıcılar için çoklu oturum açma deneyimi sağlamanıza veya kimlik sağlayıcısından ek kullanıcı verileri almanıza olanak tanır.

#### <a name="social-authentication-basic-example"></a>Sosyal Kimlik Doğrulaması temel örneği

Bu örnekte kimlik doğrulaması için Facebook istemci SDK’sı kullanılır:

```javascript
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now signed in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
Bu örnek, ilgili sağlayıcı SDK’sı tarafından sağlanan belirtecin belirteç değişkenine depolandığını varsayar.

### <a name="how-to-obtain-information-about-the-authenticated-user"></a><a name="auth-getinfo"></a>Nasıl yapılır: Kimliği doğrulanmış kullanıcı hakkında bilgi edinme

Kimlik doğrulama bilgileri, herhangi bir AJAX kitaplığı ile HTTP çağrısı kullanılarak `/.auth/me` uç noktasından alınabilir.  `X-ZUMO-AUTH` üst bilgisini kimlik doğrulama belirtecinize ayarlandığınızdan emin olun.  Kimlik doğrulama belirteci `client.currentUser.mobileServiceAuthenticationToken` içine depolanır.  Örneğin, fetch API’sini kullanmak için:

```javascript
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Fetch, [bir npm paketi](https://www.npmjs.com/package/whatwg-fetch) olarak mevcuttur veya [CDNJS](https://cdnjs.com/libraries/fetch)’den tarayıcı ile indirilebilir. Bilgileri getirmek için jQuery veya başka bir AJAX API’si de kullanabilirsiniz.  Veriler bir JSON nesnesi olarak alınır.
