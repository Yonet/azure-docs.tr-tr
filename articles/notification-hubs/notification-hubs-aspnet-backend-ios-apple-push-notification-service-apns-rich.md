---
title: Azure Notification Hubs zengin gönderim
description: Azure 'dan bir iOS uygulamasına zengin anında iletme bildirimleri göndermeyi öğrenin. Kod örnekleri, amaç-C ve C# dilinde yazılmıştır.
documentationcenter: ios
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 08/17/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 33626b7aee615d07ef88dd9fbca46e6512e2cafc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90090372"
---
# <a name="azure-notification-hubs-rich-push"></a>Azure Notification Hubs zengin gönderim

## <a name="overview"></a>Genel Bakış

Kullanıcıları anlık zengin içerikle birlikte kullanmak için, bir uygulama düz metnin ötesine gönderim yapmak isteyebilir. Bu bildirimler kullanıcı etkileşimlerini yükseltir ve URL, ses, resim/kupon gibi içerikler sunar. Bu öğretici [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticisini oluşturur ve yükleri içeren anında iletme bildirimlerinin nasıl gönderileceğini gösterir (örneğin, görüntüler).

Bu öğretici iOS 7 ve 8 ile uyumludur.

  ![Üç ekran görüntüsü: Gönder düğmesine sahip bir uygulama ekranı, bir cihazdaki başlangıç ekranı ve geri düğmesi olan bir Windows logosu.][IOS1]

Yüksek düzeyde:

1. Uygulama arka ucu:
   * Zengin yükü (Bu durumda görüntüde) arka uç veritabanında/yerel depolama alanında depolar.
   * Bu zengin bildirimin KIMLIĞINI cihaza gönderir.
2. Cihazdaki uygulama:
   * , Aldığı KIMLIKLE zengin yük isteyen arka uca iletişim kurar.
   * Veri alımı tamamlandığında cihazda Kullanıcı bildirimleri gönderir ve kullanıcılar daha fazla bilgi edinmek için dokunduğunda yükü hemen gösterir.

## <a name="webapi-project"></a>WebAPI projesi

1. Visual Studio 'da, [kullanıcıları bilgilendir](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticisinde oluşturduğunuz **apparka uç** projesini açın.
2. Kullanıcılara bildirmek istediğiniz görüntüyü alın ve proje dizininizde bir **img** klasörüne yerleştirin.
3. Çözüm Gezgini **tüm dosyaları göster** ' e tıklayın ve **projeye dahil**etmek için klasöre sağ tıklayın.
4. Görüntü seçiliyken, **Özellikler** penceresindeki **yapı eylemini** **gömülü kaynak**olarak değiştirin.

    ![Çözüm Gezgini ekran görüntüsü. Görüntü dosyası seçilidir ve Özellikler bölmesinde, gömülü kaynak derleme eylemi olarak listelenir.][IOS2]
5. İçinde `Notifications.cs` , aşağıdaki ifadeyi ekleyin `using` :

    ```csharp
    using System.Reflection;
    ```

6. `Notifications`Sınıfını aşağıdaki kodla değiştirin. Yer tutucuları Notification Hub kimlik bilgilerinizle ve görüntü dosyası adıyla değiştirdiğinizden emin olun:

    ```csharp
    public class Notification {
        public int Id { get; set; }
        // Initial notification message to display to users
        public string Message { get; set; }
        // Type of rich payload (developer-defined)
        public string RichType { get; set; }
        public string Payload { get; set; }
        public bool Read { get; set; }
    }

    public class Notifications {
        public static Notifications Instance = new Notifications();

        private List<Notification> notifications = new List<Notification>();

        public NotificationHubClient Hub { get; set; }

        private Notifications() {
            // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
            Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
        }

        public Notification CreateNotification(string message, string richType, string payload) {
            var notification = new Notification() {
                Id = notifications.Count,
                Message = message,
                RichType = richType,
                Payload = payload,
                Read = false
            };

            notifications.Add(notification);

            return notification;
        }

        public Stream ReadImage(int id) {
            var assembly = Assembly.GetExecutingAssembly();
            // Placeholder: image file name (for example, logo.png).
            return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
        }
    }
    ```

7. `NotificationsController.cs`' De, `NotificationsController` aşağıdaki kodla yeniden tanımlayın. Bu, cihaza bir ilk sessiz zengin bildirim KIMLIĞI gönderir ve görüntünün istemci tarafı alınmasına izin verir:

    ```csharp
    // Return http response with image binary
    public HttpResponseMessage Get(int id) {
        var stream = Notifications.Instance.ReadImage(id);

        var result = new HttpResponseMessage(HttpStatusCode.OK);
        result.Content = new StreamContent(stream);
        // Switch in your image extension for "png"
        result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

        return result;
    }

    // Create rich notification and send initial silent notification (containing id) to client
    public async Task<HttpResponseMessage> Post() {
        // Replace the placeholder with image file name
        var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

        var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

        // Silent notification with content available
        var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

        // Send notification to apns
        await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

8. Şimdi bu uygulamayı tüm cihazlardan erişilebilir hale getirmek için bir Azure Web sitesine yeniden dağıtın. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
9. Yayımlama hedefi olarak **Azure Web sitesini** seçin. Azure hesabınızla oturum açın ve var olan veya yeni bir Web sitesini seçin ve **bağlantı** SEKMESINDE **hedef URL** özelliğini bir yere göz önüne alın. Bu öğreticide daha sonra arka uç *uç* noktanız olarak bu URL 'ye başvurduk. **Yayımla**’yı seçin.

## <a name="modify-the-ios-project"></a>İOS projesini değiştirme

Yalnızca bir bildirimin *kimliğini* göndermek için uygulamanızın arka ucunu değiştirdiğimize göre, iOS UYGULAMANıZı bu kimliği işleyecek şekilde değiştirin ve arka ucunuzdaki zengin iletiyi alın:

1. İOS projenizi açın ve **hedefler** bölümünde ana uygulama hedeflerinize giderek uzak bildirimleri etkinleştirin.
2. **Özellikleri**seçin, **arka plan modlarını**etkinleştirin ve **uzak bildirimler** onay kutusunu işaretleyin.

    ![Yetenek ekranını gösteren iOS projesinin ekran görüntüsü. Arka plan modları açıktır ve uzaktan bildirimler onay kutusu seçilidir.][IOS3]
3. `Main.storyboard`' İ açın ve Kullanıcı öğreticisini [bildir](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticiden bir görünüm denetleyicinizin (Bu öğreticide giriş görünümü denetleyicisi olarak adlandırılır) bulunduğundan emin olun.
4. Görsel taslağınızı bir **Gezinti denetleyicisi** ekleyin ve giriş görünümü denetleyicisi ' ni denetimin **kök görünümü** haline getirmek için sürükleyin. **Ilk görünüm denetleyicisinin** öznitelikler denetçisinde yalnızca gezinti denetleyicisi için seçildiğinden emin olun.
5. Görsel taslağa bir **Görünüm denetleyicisi** ekleyin ve bir **görüntü görünümü**ekleyin. Bu, bildirim üzerine tıklayarak kullanıcıların daha fazla bilgi edinmeleri için göreceği sayfasıdır. Görsel taslağınızı aşağıdaki gibi görünmelidir:

    ![Görsel taslağın ekran görüntüsü. Üç uygulama ekranı görünür: bir gezinti görünümü, bir giriş görünümü ve bir görüntü görünümü.][IOS4]
6. Görsel taslakta bulunan **Giriş görünümü denetleyicisine** tıklayın ve kimlik denetçisi altında **özel sınıfı** ve **film şeridi kimliği** olarak **homeviewcontroller** olduğundan emin olun.
7. Görüntü görüntüleme denetleyicisi için **ımageviewcontroller**olarak aynısını yapın.
8. Ardından, az önce oluşturduğunuz Kullanıcı arabirimini işlemek için **ımageviewcontroller** adlı yeni bir görünüm denetleyicisi sınıfı oluşturun.
9. **Imageviewcontroller. h**içinde, denetleyicinin arabirim bildirimlerine aşağıdaki kodu ekleyin. İki tane bağlamak için görsel taslak görüntü görünümünden bu özelliklere denetimin sürükleyip sürüklediğinizden emin olun:

    ```objc
    @property (weak, nonatomic) IBOutlet UIImageView *myImage;
    @property (strong) UIImage* imagePayload;
    ```

10. İçinde `imageViewController.m` , aşağıdakilerin sonuna şunu ekleyin `viewDidload` :

    ```objc
    // Display the UI Image in UI Image View
    [self.myImage setImage:self.imagePayload];
    ```

11. ' De `AppDelegate.m` , oluşturduğunuz görüntü denetleyicisini içeri aktarın:

    ```objc
    #import "imageViewController.h"
    ```

12. Aşağıdaki bildirime sahip bir arabirim bölümü ekleyin:

    ```objc
    @interface AppDelegate ()

    @property UIImage* imagePayload;
    @property NSDictionary* userInfo;
    @property BOOL iOS8;

    // Obtain content from backend with notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

    // Redirect to Image View Controller after notification interaction
    - (void)redirectToImageViewWithImage: (UIImage *)img;

    @end
    ```

13. ' De `AppDelegate` , uygulamanızın ' de sessiz bildirimler için kaydolduktan emin olun `application: didFinishLaunchingWithOptions` :

    ```objc
    // Software version
    self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

    // Register for remote notifications for iOS8 and previous versions
    if (self.iOS8) {
        NSLog(@"This device is running with iOS8.");

        // Action
        UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
        richPushAction.identifier = @"richPushMore";
        richPushAction.activationMode = UIUserNotificationActivationModeForeground;
        richPushAction.authenticationRequired = NO;
        richPushAction.title = @"More";

        // Notification category
        UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
        richPushCategory.identifier = @"richPush";
        [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

        // Notification categories
        NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert |
                                                UIUserNotificationTypeBadge
                                                                                    categories:richPushCategories];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    }
    else {
        // Previous iOS versions
        NSLog(@"This device is running with iOS7 or earlier versions.");

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
    }

    return YES;
    ```

14. `application:didRegisterForRemoteNotificationsWithDeviceToken`Film şeridi Kullanıcı arabirimi değişikliğini hesaba eklemek için aşağıdaki uygulamayı değiştirin:

    ```objc
    // Access navigation controller which is at the root of window
    UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
    // Get home view controller from stack on navigation controller
    homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
    hvc.deviceToken = deviceToken;
    ```

15. Ardından, `AppDelegate.m` uç noktanıza görüntüyü almak için aşağıdaki yöntemleri ekleyin ve alma tamamlandığında yerel bir bildirim gönderin. Yer tutucuyu arka uç uç noktanızla değiştirdiğinizden emin olun `{backend endpoint}` :

    ```objc
    NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

    // Helper: retrieve notification content from backend with rich notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
        // Check if authenticated
        if (!authenticationHeader) return;

        NSURLSession* session = [NSURLSession
                                sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                delegate:nil
                                delegateQueue:nil];

        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"GET"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200) {
                // From NSData to UIImage
                self.imagePayload = [UIImage imageWithData:data];

                completion(nil);
            }
            else {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(error);
                else {
                    completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }

    // Handle silent push notifications when id is sent from backend
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
        self.userInfo = userInfo;
        int richId = [[self.userInfo objectForKey:@"richId"] intValue];
        NSString* richType = [self.userInfo objectForKey:@"richType"];

        // Retrieve image data
        if ([richType isEqualToString:@"img"]) {  
            [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                if (!error){
                    // Send local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                    // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                    localNotification.userInfo = self.userInfo;
                    localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];

                    // iOS8 categories
                    if (self.iOS8) {
                        localNotification.category = @"richPush";
                    }

                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    handler(UIBackgroundFetchResultNewData);
                }
                else{
                    handler(UIBackgroundFetchResultFailed);
                }
            }];
        }
        // Add "else if" here to handle more types of rich content such as url, sound files, etc.
    }
    ```

16. Aşağıdaki yöntemlerle görüntü görünümü denetleyicisini açarak önceki yerel bildirimi işleyin `AppDelegate.m` :

    ```objc
    // Helper: redirect users to image view controller
    - (void)redirectToImageViewWithImage: (UIImage *)img {
        UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
        UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main" bundle: nil];
        imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
        // Pass data/image to image view controller
        imgViewController.imagePayload = img;

        // Redirect
        [navigationController pushViewController:imgViewController animated:YES];
    }

    // Handle local notification sent above in didReceiveRemoteNotification
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
        if (application.applicationState == UIApplicationStateActive) {
            // Show in-app alert with an extra "more" button
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
            [alert show];
        }
        // App becomes active from user's tap on notification
        else {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
    }

    // Handle buttons in in-app alerts and redirect with data/image
    - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
        // Handle "more" button
        if (buttonIndex == 1)
        {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
        // Add "else if" here to handle more buttons
    }

    // Handle notification setting actions in iOS8
    - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
        // Handle richPush related buttons
        if ([identifier isEqualToString:@"richPushMore"]) {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
        completionHandler();
    }
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

1. XCode 'da uygulamayı fiziksel bir iOS cihazında çalıştırın (anında iletme bildirimleri benzeticide çalışmaz).
2. İOS uygulama kullanıcı arabiriminde, kimlik doğrulaması için aynı değere sahip bir Kullanıcı adı ve parola girin ve **oturum aç**' a tıklayın.
3. **Gönderim gönder** ' e tıklayın ve uygulama içi bir uyarı görmeniz gerekir. **Daha fazla**' ya tıklarsanız, uygulamanızın arka ucuna dahil etmek için seçtiğiniz görüntüye yönlendirilirsiniz.
4. Ayrıca, **gönderme gönder** ' e tıklayabilir ve cihazınızın giriş düğmesine hemen basabilirsiniz. Birkaç dakika içinde bir anında iletme bildirimi alacaksınız. Üzerine dokunarak veya daha fazla ' a tıkladığınızda, uygulamanıza ve zengin görüntü içeriğine yönlendirilirsiniz.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
