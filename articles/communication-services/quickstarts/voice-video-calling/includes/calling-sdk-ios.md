---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: a8cfdc76694d52acee70cde0e3f1697cd8129d06
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691912"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Dağıtılan bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- Bir `User Access Token` çağrı istemcisini etkinleştirmek için. [Nasıl yapılır `User Access Token` ](../../access-tokens.md) hakkında daha fazla bilgi edinmek için
- İsteğe bağlı: [uygulamanıza çağrı ekleme ile çalışmaya](../getting-started-with-calling.md) başlama için hızlı başlangıcı doldurun

## <a name="setting-up"></a>Ayarlanıyor

### <a name="creating-the-xcode-project"></a>Xcode projesi oluşturma

Xcode 'da yeni bir iOS projesi oluşturun ve **tek görünüm uygulama** şablonunu seçin. Bu hızlı başlangıç, [swiftuı çerçevesini](https://developer.apple.com/xcode/swiftui/)kullanır, bu nedenle **dili** **Swift** ve **Kullanıcı arabirimine** **swiftuı** olarak ayarlamanız gerekir. Bu hızlı başlangıç sırasında birim testleri veya UI testleri oluşturuyoruz. **Birim testlerini dahil etme** işaretini kaldırın ve ayrıca **UI testlerini Ekle** seçeneğinin işaretini kaldırın.

:::image type="content" source="../media/ios/xcode-new-ios-project.png" alt-text="Xcode 'da yeni yeni proje oluştur penceresini gösteren ekran görüntüsü.":::

### <a name="install-the-package-and-dependencies-with-cocoapods"></a>CocoaPods ile paketi ve bağımlılıkları yükler

1. Uygulamanız için şöyle bir pod dosyası oluşturun:

   ```
   platform :ios, '13.0'
   use_frameworks!
   target 'AzureCommunicationCallingSample' do
     pod 'AzureCommunicationCalling', '~> 1.0.0-beta.5'
     pod 'AzureCommunication', '~> 1.0.0-beta.5'
     pod 'AzureCore', '~> 1.0.0-beta.5'
   end
   ```

2. Şu komutu çalıştırın: `pod install`.
3. Öğesini `.xcworkspace` XCode ile açın.

### <a name="request-access-to-the-microphone"></a>Mikrofona erişim isteyin

Cihazın mikrofonuna erişmek için uygulamanızın bilgi Özellik listesini bir ile güncelleştirmeniz gerekir `NSMicrophoneUsageDescription` . İlişkili değeri, `string` sistemin kullanıcıdan istek erişimi istemek için kullandığı iletişim kutusuna dahil edilecek bir öğesine ayarlarsınız.

`Info.plist`Proje ağacının girişine sağ tıklayın ve kaynak kodu **olarak aç**' ı seçin  >  . Aşağıdaki satırları en üst düzey bölümüne ekleyin `<dict>` ve dosyayı kaydedin.

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Need microphone access for VOIP calling.</string>
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Projenizin **ContentView. Swift** dosyasını açın ve `import` dosyanın en üstüne içeri aktarmak için bir bildirim ekleyin `AzureCommunicationCalling library` . Buna ek olarak, `AVFoundation` Bu kodda ses izni isteği için bu gerekir.

```swift
import AzureCommunicationCalling
import AVFoundation
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Azure Iletişim hizmetlerinin iOS için istemci kitaplığını çağıran bazı önemli özelliklerinden bazılarını işler.


| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ACSCallClient | ACSCallClient, çağıran istemci kitaplığı için ana giriş noktasıdır.|
| ACSCallAgent | ACSCallAgent, çağrıları başlatmak ve yönetmek için kullanılır. |
| CommunicationUserCredential | CommunicationUserCredential, CallAgent örneğini oluşturmak için belirteç kimlik bilgileri olarak kullanılır.| 
| CommunicationIndentifier | CommunicationIndentifier, aşağıdakilerden biri olabilecek Kullanıcı kimliğini temsil etmek için kullanılır: CommunicationUser/PhoneNumber/Callinguygulaması. |

> [!NOTE]
> Olay temsilcileri uygularken, uygulamanın olay abonelikleri gerektiren nesnelere güçlü bir başvuru tutması gerekir. Örneğin, `ACSRemoteParticipant` yöntemi çağrılırken bir nesne döndürüldüğünde `call.addParticipant` ve uygulama, dinlemek üzere temsilciyi ayarlarsa `ACSRemoteParticipantDelegate` , uygulamanın nesnesine bir güçlü başvuru tutması gerekir `ACSRemoteParticipant` . Aksi takdirde, bu nesne toplanırsa, çağıran SDK nesneyi çağırmayı denediğinde temsilci önemli bir özel durum oluşturur.

## <a name="initialize-the-acscallagent"></a>ACSCallAgent 'ı başlatma

`ACSCallAgent`Öğesinden bir örnek oluşturmak için `ACSCallClient` , `callClient.createCallAgent` başlatıldıktan sonra bir nesneyi zaman uyumsuz olarak döndüren yöntemi kullanmanız gerekir `ACSCallAgent`

Çağrı istemcisi oluşturmak için bir nesneyi geçirmeniz gerekir `CommunicationUserCredential` .

```swift

import AzureCommunication

let tokenString = "token_string"
var userCredential: CommunicationUserCredential?
do {
    userCredential = try CommunicationUserCredential(
        initialToken: tokenString, refreshProactively: true,
        tokenRefresher: self.fetchTokenSync
    )
} catch {
    print("Failed to create CommunicationCredential object")
}

// tokenProvider needs to be implemented by contoso which fetches new token
public func fetchTokenSync(then onCompletion: TokenRefreshOnCompletion) {
    let newToken = self.tokenProvider!.fetchNewToken()
    onCompletion(newToken, nil)
}
```

Yukarıda oluşturulan CommunicationUserCredential nesnesini ACSCallClient öğesine geçirin ve görünen adı ayarlayın.

```swift

callClient = CallClient()
let callAgentOptions:CallAgentOptions = CallAgentOptions()
options.displayName = "ACS iOS User"

callClient?.createCallAgent(userCredential: userCredential!,
    options: callAgentOptions,
    completionHandler: { (callAgent, error) in
        if error == nil {
            print("Create agent succeeded")
            self.callAgent = callAgent
        } else {
            print("Create agent failed")
        }
})

```

## <a name="place-an-outgoing-call"></a>Giden bir çağrı yerleştir

Bir çağrı oluşturmak ve başlatmak için, üzerindeki API 'lerden birini çağırmanız `ACSCallAgent` ve Iletişim Hizmetleri Yönetimi istemci kitaplığı aracılığıyla sağladığınız bir kullanıcının Iletişim Hizmetleri kimliğini sağlamanız gerekir.

Çağrı oluşturma ve başlatma zaman uyumludur. Çağrıda tüm olaylara abone olmanızı sağlayan bir çağrı örneği alacaksınız.

### <a name="place-a-11-call-to-a-user-or-a-1n-call-with-users-and-pstn"></a>Kullanıcılara 1:1 çağrısı veya Kullanıcı ve PSTN ile 1: n çağrısı koyun

```swift

let callees = [CommunicationUser(identifier: 'acsUserId')]
let oneToOneCall = self.callAgent.call(participants: callees, options: StartCallOptions())

```

### <a name="place-a-1n-call-with-users-and-pstn"></a>Kullanıcılar ve PSTN ile 1: n çağrısı yerleştir
Çağrıyı PSTN 'e yerleştirmek için, Iletişim hizmetleriyle alınan telefon numarasını belirtmeniz gerekir.
```swift

let pstnCallee = PhoneNumber('+1999999999')
let callee = CommunicationUser(identifier: 'acsUserId')
let groupCall = self.callAgent.call(participants: [pstnCallee, callee], options: StartCallOptions())

```

### <a name="place-a-11-call-with-with-video"></a>Görüntülü bir 1:1 çağrısı yerleştirin
Bir cihaz yöneticisi örneği almak için lütfen [buraya](#device-management) başvurun

```swift

let camera = self.deviceManager!.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: camera)
let videoOptions = VideoOptions(localVideoStream: localVideoStream)

let startCallOptions = StartCallOptions()
startCallOptions?.videoOptions = videoOptions

let callee = CommunicationUser(identifier: 'acsUserId')
let call = self.callAgent?.call(participants: [callee], options: startCallOptions)

```

### <a name="join-a-group-call"></a>Grup çağrısına katılır
Bir çağrıya katmak için, *Callagent* 'da API 'lerden birini çağırmanız gerekir

```swift

let groupCallContext = GroupCallContext()
groupCallContext?.groupId = UUID(uuidString: "uuid_string")!
let call = self.callAgent?.join(with: groupCallContext, joinCallOptions: JoinCallOptions())

```

### <a name="accept-an-incoming-call"></a>Gelen çağrıyı kabul et
Bir çağrıyı kabul etmek için, çağırma nesnesinde ' Accept ' metodunu çağırın.
CallAgent 'a bir temsilci ayarlayın 
```swift
final class CallHandler: NSObject, CallAgentDelegate
{
    public var incomingCall: Call?
 
    public func onCallsUpdated(_ callAgent: CallAgent!, args: CallsUpdatedEventArgs!) {
        if let incomingCall = args.addedCalls?.first(where: { $0.isIncoming }) {
            self.incomingCall = incomingCall
        }
    }
}

let firstCamera: VideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: firstCamera)
let acceptCallOptions = AcceptCallOptions()
acceptCallOptions!.videoOptions = VideoOptions(localVideoStream:localVideoStream!)
if let incomingCall = CallHandler().incomingCall {
   incomingCall.accept(options: acceptCallOptions,
                          completionHandler: { (error) in
                           if error == nil {
                               print("Incoming call accepted")
                           } else {
                               print("Failed to accept incoming call")
                           }
                       })
} else {
   print("No incoming call found to accept")
}
```

## <a name="push-notification"></a>Anında iletme bildirimi

Mobil anında iletme bildirimi, mobil cihazda aldığınız açılır bildirimdir. Çağırmak için VoIP (Internet Protokolü üzerinden ses) anında iletme bildirimleri üzerine odaklanacağız. Anında iletme bildirimine kaydolma, anında iletme bildirimini işleme ve anında iletme bildiriminin kaydını silme özelliklerini sunacağız.

### <a name="prerequisite"></a>Önkoşul

- 1. Adım: Xcode-> Imzalama & özellikleri-> özellik ekleme-> "anında Iletme bildirimleri"
- 2. Adım: Xcode-> Imzalama & özellikleri-> özellik ekleme-> "arka plan modları"
- 3. Adım: "arka plan modları"-> "IP üzerinden ses" ve "uzak bildirimler" seçeneğini belirleyin

:::image type="content" source="../media/ios/xcode-push-notification.png" alt-text="Xcode 'da nasıl özellik ekleneceğini gösteren ekran görüntüsü." lightbox="../media/ios/xcode-push-notification.png":::

#### <a name="register-for-push-notifications"></a>Anında Iletme bildirimleri için kaydolun

Anında iletme bildirimine kaydolmak için bir cihaz kayıt belirtecine sahip bir *Callagent* örneğine registerPushNotification () çağırın.

Başarılı başlatma işleminden sonra anında iletme bildiriminin çağrılması gerekir. `callAgent`Nesne yok edildiğinde, `logout` anında iletme bildirimlerinin kaydını otomatik olarak silecektir.


```swift

let deviceToken: Data = pushRegistry?.pushToken(for: PKPushType.voIP)
callAgent.registerPushNotifications(deviceToken: deviceToken,
                completionHandler: { (error) in
    if(error == nil) {
        print("Successfully registered to push notification.")
    } else {
        print("Failed to register push notification.")
    }
})

```

#### <a name="push-notification-handling"></a>Anında iletme bildirimi Işleme
Gelen çağrıları anında iletme bildirimlerini almak için, bir *Callagent* örneği üzerinde bir sözlük yüküne *handlepushnotification ()* çağırın.

```swift

let dictionaryPayload = pushPayload?.dictionaryPayload
callAgent.handlePushNotification(payload: dictionaryPayload, completionHandler: { (error) in
    if (error != nil) {
        print("Handling of push notification failed")
    } else {
        print("Handling of push notification was successful")
    }
})

```
#### <a name="unregister-push-notification"></a>Anında Iletme bildiriminin kaydını sil

Uygulamalar, anında iletme bildiriminin kaydını dilediğiniz zaman açabilir. Yalnızca `unRegisterPushNotification` *Callagent* üzerinde yöntemi çağırın.
> [!NOTE]
> Uygulamalar, oturum kapatma sırasında anında iletme bildiriminden otomatik olarak silinir.

```swift

callAgent.unRegisterPushNotifications(completionHandler: { (error) in
    if (error != nil) {
        print("Unregister of push notification failed, please try again")
    } else {
        print("Unregister of push notification was successful")
    }
})

```

## <a name="mid-call-operations"></a>Orta çağrı işlemleri

Video ve ses ile ilgili ayarları yönetmek için bir çağrı sırasında çeşitli işlemler gerçekleştirebilirsiniz.

### <a name="mute-and-unmute"></a>Sessiz ve aç

Yerel uç noktanın sesini kapatmak veya sesini açmak için `mute` ve `unmute` zaman uyumsuz API 'leri kullanabilirsiniz:

```swift
call!.mute(completionHandler: { (error) in
    if error == nil {
        print("Successfully muted")
    } else {
        print("Failed to mute")
    }
})

```

En Yerel aç

```swift
call!.unmute(completionHandler:{ (error) in
    if error == nil {
        print("Successfully un-muted")
    } else {
        print("Failed to unmute")
    }
})
```

### <a name="start-and-stop-sending-local-video"></a>Yerel video göndermeyi başlatma ve durdurma

Çağrıdan diğer katılımcılara yerel video göndermeye başlamak için API 'yi kullanın `startVideo` ve ile geçiş yapın `localVideoStream``camera`

```swift

let firstCamera: VideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: firstCamera)

call!.startVideo(stream: localVideoStream) { (error) in
    if (error == nil) {
        print("Local video started successfully")
    }
    else {
        print("Local video failed to start")
    }
}

```

Video göndermeye başladıktan sonra `ACSLocalVideoStream` örnek, `localVideoStreams` koleksiyon bir çağrı örneğine eklenir:

```swift

call.localVideoStreams[0]

```

En Yerel videoyu durdurmak için, `localVideoStream` şunu çağrısından geçirin `call.startVideo` :

```swift

call!.stopVideo(stream: localVideoStream) { (error) in
    if (error == nil) {
        print("Local video stopped successfully")
    } else {
        print("Local video failed to stop")
    }
}

```

## <a name="remote-participants-management"></a>Uzak katılımcılar yönetimi

Tüm uzak katılımcılar tür tarafından temsil edilir `ACSRemoteParticipant` ve `remoteParticipants` bir çağrı örneğindeki koleksiyon aracılığıyla kullanılabilir:

### <a name="list-participants-in-a-call"></a>Bir çağrıda katılımcıları listeleme

```swift

call.remoteParticipants

```

### <a name="remote-participant-properties"></a>Uzak katılımcı özellikleri

```swift

// [ACSRemoteParticipantDelegate] delegate - an object you provide to receive events from this ACSRemoteParticipant instance
var remoteParticipantDelegate = remoteParticipant.delegate

// [CommunicationIdentifier] identity - same as the one used to provision token for another user
var identity = remoteParticipant.identity

// ACSParticipantStateIdle = 0, ACSParticipantStateEarlyMedia = 1, ACSParticipantStateConnecting = 2, ACSParticipantStateConnected = 3, ACSParticipantStateOnHold = 4, ACSParticipantStateInLobby = 5, ACSParticipantStateDisconnected = 6
var state = remoteParticipant.state

// [ACSError] callEndReason - reason why participant left the call, contains code/subcode/message
var callEndReason = remoteParticipant.callEndReason

// [Bool] isMuted - indicating if participant is muted
var isMuted = remoteParticipant.isMuted

// [Bool] isSpeaking - indicating if participant is currently speaking
var isSpeaking = remoteParticipant.isSpeaking

// ACSRemoteVideoStream[] - collection of video streams this participants has
var videoStreams = remoteParticipant.videoStreams // [ACSRemoteVideoStream, ACSRemoteVideoStream, ...]

```

### <a name="add-a-participant-to-a-call"></a>Çağrıya katılımcı ekleme

Çağrıya (bir kullanıcı veya telefon numarası) bir katılımcı eklemek için, çağırabilirsiniz `addParticipant` . Bu, zaman uyumlu olarak uzak bir katılımcı örneği döndürür.

```swift

let remoteParticipantAdded: RemoteParticipant = call.add(participant: CommunicationUser(identifier: "userId"))

```

### <a name="remove-a-participant-from-a-call"></a>Bir katılımcıyı çağrıdan kaldırma
Bir katılımcıyı bir çağrıdan (Kullanıcı veya telefon numarası) kaldırmak için API 'yi çağırabilirsiniz  `removeParticipant` . Bu işlem zaman uyumsuz olarak çözümlenir.

```swift

call!.remove(participant: remoteParticipantAdded) { (error) in
    if (error == nil) {
        print("Successfully removed participant")
    } else {
        print("Failed to remove participant")
    }
}

```

## <a name="render-remote-participant-video-streams"></a>Uzak katılımcı video akışlarını işle

Uzak katılımcılar, bir çağrı sırasında video veya ekran paylaşımı başlatabilir.

### <a name="handle-remote-participant-videoscreen-sharing-streams"></a>Uzak katılımcı video/ekran paylaşım akışlarını işle

Uzak katılımcıların akışlarını listelemek için, `videoStreams` koleksiyonları inceleyin:

```swift

var remoteParticipantVideoStream = call.remoteParticipants[0].videoStreams[0]

```

### <a name="remote-video-stream-properties"></a>Uzak video akışı özellikleri

```swift

var type: MediaStreamType = remoteParticipantVideoStream.type // 'ACSMediaStreamTypeVideo'

var isAvailable: Bool = remoteParticipantVideoStream.isAvailable // indicates if remote stream is available

var id: Int = remoteParticipantVideoStream.id // id of remoteParticipantStream

```

### <a name="render-remote-participant-stream"></a>Uzak katılımcı akışını işle

Uzak katılımcı akışlarını işlemeye başlamak için:

```swift

let renderer: Renderer? = Renderer(remoteVideoStream: remoteParticipantVideoStream)
let targetRemoteParticipantView: RendererView? = renderer?.createView(with: RenderingOptions(scalingMode: ScalingMode.crop))
// To update the scaling mode later
targetRemoteParticipantView.update(scalingMode: ScalingMode.fit)

```

### <a name="remote-video-renderer-methods-and-properties"></a>Uzak video Oluşturucu yöntemleri ve özellikleri

```swift
// [Bool] isRendering - indicating if stream is being rendered
remoteVideoRenderer.isRendering()
// [Synchronous] dispose() - dispose renderer and all `RendererView` associated with this renderer. To be called when you have removed all associated views from the UI.
remoteVideoRenderer.dispose()
```

## <a name="device-management"></a>Cihaz yönetimi

`DeviceManager` ses/video akışlarını aktarmak için bir çağrıda kullanılabilecek yerel cihazları listelemenizi sağlar. Ayrıca, kullanıcıdan mikrofona/kameraya erişmek için izin isteme olanağı da sağlar. `deviceManager` `callClient` Nesnesine erişebilirsiniz:

```swift

self.callClient!.getDeviceManager(
    completionHandler: { (deviceManager, error) in
        if (error == nil) {
            print("Got device manager instance")
            self.deviceManager = deviceManager
        } else {
            print("Failed to get device manager instance")
        }
    })
```

### <a name="enumerate-local-devices"></a>Yerel cihazları listeleme

Yerel cihazlara erişmek için Aygıt Yöneticisi numaralandırma yöntemlerini kullanabilirsiniz. Sabit listesi, zaman uyumlu bir işlemdir.

```swift
// enumerate local cameras
var localCameras = deviceManager.getCameraList() // [ACSVideoDeviceInfo, ACSVideoDeviceInfo...]
// enumerate local cameras
var localMicrophones = deviceManager.getMicrophoneList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
// enumerate local cameras
var localSpeakers = deviceManager.getSpeakerList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
``` 

### <a name="set-default-microphonespeaker"></a>Varsayılan mikrofonu/konuşmacıyı ayarla

Aygıt Yöneticisi, bir çağrı başlatılırken kullanılacak varsayılan bir cihaz ayarlamanıza olanak sağlar. Yığın Varsayılanları ayarlanmamışsa, Iletişim Hizmetleri işletim sistemi varsayılanlarına geri döner.

```swift
// get first microphone
var firstMicrophone = self.deviceManager!.getMicrophoneList()![0]
// [Synchronous] set microphone
deviceManager.setMicrophone(microphoneDevice: firstMicrophone)
// get first speaker
var firstSpeaker = self.deviceManager!.getSpeakerList()![0]
// [Synchronous] set speaker
deviceManager.setSpeaker(speakerDevice: firstSpeaker)
```

### <a name="local-camera-preview"></a>Yerel kamera önizlemesi

`ACSRenderer`Yerel kameranızdan bir akış işlemeye başlamak için ' i kullanabilirsiniz. Bu akış diğer katılımcılara gönderilemez; Bu, yerel bir önizleme akışımıza sahiptir. Bu, zaman uyumsuz bir eylemdir.

```swift

let camera: VideoDeviceInfo = self.deviceManager!.getCameraList()![0]
let localVideoStream: LocalVideoStream = LocalVideoStream(camera: camera)
let renderer: Renderer = Renderer(localVideoStream: localVideoStream)
self.view = renderer!.createView()

```

### <a name="local-camera-preview-properties"></a>Yerel kamera önizleme özellikleri

İşleyici, işleme denetlemenizi sağlayan özellikler ve Yöntemler kümesine sahiptir:

```swift

// Constructor can take in ACSLocalVideoStream or ACSRemoteVideoStream
let localRenderer = Renderer(localVideoStream:localVideoStream)
let remoteRenderer = Renderer(remoteVideoStream:remoteVideoStream)

// [ACSStreamSize] size of the rendering view
localRenderer.size

// [ACSRendererDelegate] an object you provide to receive events from this ACSRenderer instance
localRenderer.delegate

// [Synchronous] create view
try! localRenderer.createView()

// [Synchronous] create view with rendering options
try! localRenderer.createView(with: RenderingOptions(scalingMode: ScalingMode.fit))

// [Synchronous] dispose rendering view
localRenderer.dispose()

```

## <a name="eventing-model"></a>Olay modeli

Değerler değiştiğinde bildirilmesi için özelliklerin ve koleksiyonların çoğuna abone olabilirsiniz.

### <a name="properties"></a>Özellikler
Olaylara abone olmak için `property changed` :

```swift
call.delegate = self
// Get the property of the call state by doing get on the call's state member
public func onCallStateChanged(_ call: Call!,
                               args: PropertyChangedEventArgs!)
{
    print("Callback from SDK when the call state changes, current state: " + call.state.rawValue)
}

 // to unsubscribe
 self.call.delegate = nil

```

### <a name="collections"></a>Koleksiyonlar
Olaylara abone olmak için `collection updated` :

```swift
call.delegate = self
// Collection contains the streams that were added or removed only
public func onLocalVideoStreamsChanged(_ call: Call!,
                                       args: LocalVideoStreamsUpdatedEventArgs!)
{
    print(args.addedStreams.count)
    print(args.removedStreams.count)
}
// to unsubscribe
self.call.delegate = nil
```
