---
title: DPS kullanarak cihazları otomatik olarak yönetme
titleSuffix: Azure Digital Twins
description: Cihaz sağlama hizmeti 'ni (DPS) kullanarak Azure dijital TWINS 'te IoT cihazlarını sağlamak ve devre dışı bırakmak için otomatik bir işlem ayarlama bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 9/1/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: e783e5dd3b0f1952928d1c36c682c5be1cba2599
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98044399"
---
# <a name="auto-manage-devices-in-azure-digital-twins-using-device-provisioning-service-dps"></a>Cihaz sağlama hizmeti 'ni (DPS) kullanarak Azure dijital TWINS 'de cihazları otomatik olarak yönetme

Bu makalede, Azure Digital TWINS 'i [cihaz sağlama hizmeti (DPS)](../iot-dps/about-iot-dps.md)ile tümleştirmeyi öğreneceksiniz.

Bu makalede açıklanan çözüm, cihaz sağlama hizmeti 'ni kullanarak Azure dijital TWINS 'de IoT Hub cihazları **_sağlama_** ve **_devre dışı bırakma_** sürecini otomatikleştirmenize imkan tanır. 

_Sağlama_ ve _devre dışı bırakma_ aşamaları hakkında daha fazla bilgi Için ve tüm kurumsal IoT projelerinde ortak olan genel cihaz yönetim aşamaları kümesini daha iyi anlamak için, IoT Hub cihaz yönetimi belgelerinin [ *cihaz yaşam döngüsü* bölümüne](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) bakın.

## <a name="prerequisites"></a>Önkoşullar

Sağlamayı ayarlamadan önce, modeller ve TWINS içeren bir **Azure dijital TWINS örneğine** sahip olmanız gerekir. Bu örnek, verileri temel alarak dijital ikizi bilgilerini güncelleştirme özelliği ile de ayarlanmalıdır. 

Bu ayarı zaten yoksa, Azure dijital TWINS [*öğreticisini izleyerek oluşturabilirsiniz: uçtan uca çözümü bağlama*](tutorial-end-to-end.md). Öğretici, modellerle bir Azure dijital TWINS örneği, bağlantılı bir Azure [IoT Hub](../iot-hub/about-iot-hub.md)ve veri akışını yaymaya yönelik çeşitli [Azure işlevleri](../azure-functions/functions-overview.md) ayarlama konusunda size kılavuzluk eder.

Örneğinizi ayarlarken, bu makalenin ilerleyen kısımlarında aşağıdaki değerlere sahip olmanız gerekir. Bu değerleri yeniden toplamanız gerekiyorsa, yönergeler için aşağıdaki bağlantıları kullanın.
* Azure Digital TWINS örnek **_ana bilgisayar adı_** ([portalda bul](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure Event Hubs bağlantı dizesi **_bağlantı dizesi_** ([portalda bul](../event-hubs/event-hubs-get-connection-string.md#get-connection-string-from-the-portal))

Bu örnek ayrıca cihaz sağlama hizmetini kullanarak sağlamayı içeren bir **cihaz simülatörü** kullanır. Cihaz simülatörü şurada bulunur: [Azure dijital TWINS ve IoT Hub tümleştirme örneği](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Örnek bağlantısına gidip başlık altındaki *posta indir* düğmesini seçerek makinenizde örnek projeyi alın. İndirilen klasörü sıkıştırmayı açın.

Cihaz simülatörü **Node.js**, sürüm 10.0. x veya üzerini temel alır. [*Geliştirme ortamınızı hazırlama*](https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md) Bu öğretici Için Node.js Windows veya Linux 'ta nasıl yükleneceğini açıklar.

## <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki görüntüde, cihaz sağlama hizmeti ile Azure dijital TWINS kullanılarak bu çözümün mimarisi gösterilmektedir. Hem cihaz sağlama hem de devre dışı bırakma akışını gösterir.

:::image type="content" source="media/how-to-provision-using-dps/flows.png" alt-text="Bir cihazın ve çeşitli Azure hizmetlerinin bir uçtan uca senaryosunda bir görünümü. Veri akışı, bir termostat cihaz ve DPS arasında geri ve ileri akar. Veriler aynı zamanda DPS 'den IoT Hub 'ye ve Azure dijital TWINS 'e ' ayırma ' etiketli bir Azure işlevi aracılığıyla akar. El ile ' cihaz silme ' eyleminden alınan veriler, IoT Hub > Event Hubs Azure Işlevleri > Azure dijital TWINS > aracılığıyla akar.":::

Bu makale iki bölüme ayrılmıştır:
* [*Cihaz sağlama hizmeti 'ni kullanarak cihazı otomatik sağlama*](#auto-provision-device-using-device-provisioning-service)
* [*IoT Hub yaşam döngüsü olaylarını kullanarak cihazı otomatik olarak devre dışı bırakma*](#auto-retire-device-using-iot-hub-lifecycle-events)

Mimarideki her adımın daha derin açıklamaları için, makalenin ilerleyen bölümlerinde tek tek bölümlerine bakın.

## <a name="auto-provision-device-using-device-provisioning-service"></a>Cihaz sağlama hizmeti 'ni kullanarak cihazı otomatik sağlama

Bu bölümde, cihazları aşağıdaki yoldan otomatik sağlamak için Azure dijital TWINS 'e cihaz sağlama hizmeti iliştirirsiniz. Bu, [daha önce](#solution-architecture)gösterilen tam mimarinin bir alıntısıdır.

:::image type="content" source="media/how-to-provision-using-dps/provision.png" alt-text="Flow sağlama--bir çözüm mimarisi diyagramı akışının, sayı etiketleme bölümlerinin bir alıntısıdır. Veriler, bir termostat cihazı ve DPS (cihaz > DPS için 1 ve DPS > cihaz için 5) arasında ileri ve geri akar. Veriler aynı zamanda DPS 'den IoT Hub (4) ve Azure Digital TWINS 'e (3) ' ayırma ' (2) etiketli bir Azure işlevi aracılığıyla akar.":::

İşlem akışının açıklaması aşağıda verilmiştir:
1. Cihaz, kimliğini kanıtlamak için bilgi tanımlamayı sağlayan DPS uç noktası ile iletişim kurar.
2. DPS kayıt listesine karşı kayıt KIMLIĞI ve anahtarı doğrulayarak cihaz kimliğini doğrular ve ayırmayı yapmak için bir [Azure işlevi](../azure-functions/functions-overview.md) çağırır.
3. Azure işlevi, cihaz için Azure dijital TWINS 'te yeni bir [ikizi](concepts-twins-graph.md) oluşturur.
4. DPS, cihazı bir IoT Hub 'ına kaydeder ve cihazın istenen ikizi durumunu doldurur.
5. IoT Hub 'ı cihaza cihaz KIMLIĞI bilgilerini ve IoT Hub bağlantı bilgilerini döndürür. Cihaz artık IoT Hub 'ına bağlanabilir.

Aşağıdaki bölümler, bu otomatik sağlama cihaz akışını ayarlama adımlarında size yol gösterir.

### <a name="create-a-device-provisioning-service"></a>Cihaz sağlama hizmeti oluşturma

Cihaz sağlama hizmeti kullanılarak yeni bir cihaz sağlandığında, bu cihaz için yeni bir ikizi Azure dijital TWINS 'te oluşturulabilir.

IoT cihazları sağlamak için kullanılacak bir cihaz sağlama hizmeti örneği oluşturun. Aşağıdaki Azure CLı yönergelerini kullanabilir veya Azure portal kullanabilirsiniz: [*hızlı başlangıç: IoT Hub cihaz sağlama hizmeti 'ni Azure Portal ayarlama*](../iot-dps/quick-setup-auto-provision.md).

Aşağıdaki Azure CLı komutu bir cihaz sağlama hizmeti oluşturacaktır. Bir ad, kaynak grubu ve bölge belirtmeniz gerekecektir. Bu komut, [makinenizde yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)Azure CLI 'niz varsa [Cloud Shell](https://shell.azure.com)veya yerel olarak çalıştırılabilir.

```azurecli-interactive
az iot dps create --name <Device Provisioning Service name> --resource-group <resource group name> --location <region; for example, eastus>
```

### <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Ardından, bir işlev uygulaması içinde HTTP isteği ile tetiklenen bir işlev oluşturacaksınız. Uçtan uca öğreticide oluşturulan işlev uygulamasını kullanabilirsiniz ([*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)) veya kendi kendinize.

Bu işlev, cihaz sağlama hizmeti tarafından, yeni bir cihaz sağlamak için [özel bir ayırma ilkesinde](../iot-dps/how-to-use-custom-allocation-policies.md) kullanılacaktır. Azure işlevleri ile HTTP istekleri kullanma hakkında daha fazla bilgi için bkz. Azure [*Için Azure http istek tetikleyicisi işlevleri*](../azure-functions/functions-bindings-http-webhook-trigger.md).

İşlev uygulaması projenizin içinde yeni bir işlev ekleyin. Ayrıca, projeye yeni bir NuGet paketi ekleyin: `Microsoft.Azure.Devices.Provisioning.Service` .

Yeni oluşturulan işlev kodu dosyasında aşağıdaki kodu yapıştırın.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIotHub_allocate.cs":::

Dosyayı kaydedin ve ardından işlev uygulamanızı yeniden yayımlayın. İşlev uygulamasını yayımlama yönergeleri için, uçtan uca öğreticinin [*uygulamayı yayımlama*](tutorial-end-to-end.md#publish-the-app) bölümüne bakın.

### <a name="configure-your-function"></a>İşlevinizi yapılandırma

Daha sonra, oluşturduğunuz Azure Digital TWINS örneğine başvuruyu içeren işlev uygulamanızda ortam değişkenlerini ayarlamanız gerekir. Uçtan uca öğreticiyi kullandıysanız ([*öğretici: uçtan uca çözümü bağlama*](tutorial-end-to-end.md)), ayar zaten yapılandırılır.

Bu Azure CLı komutuyla ayarı ekleyin:

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

Son adım öğreticideki [*işlev uygulamasına Izin atama*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) bölümünde açıklandığı gibi, Izinler ve yönetilen kimlik rolü atamasının işlev uygulaması için doğru yapılandırıldığından emin olun.

### <a name="create-device-provisioning-enrollment"></a>Cihaz sağlama kaydı oluşturma

Daha sonra, **özel bir ayırma işlevi** kullanarak cihaz sağlama hizmeti 'nde bir kayıt oluşturmanız gerekir. Özel ayırma ilkeleri hakkında cihaz sağlama hizmetleri makalesinin [*kayıt oluştur*](../iot-dps/how-to-use-custom-allocation-policies.md#create-the-enrollment) ve [*benzersiz cihaz anahtarlarını türet*](../iot-dps/how-to-use-custom-allocation-policies.md#derive-unique-device-keys) bölümünde bunu yapmak için yönergeleri izleyin.

Bu akıştan gezinirken, bu kaydı yeni oluşturduğunuz işleve bağlayacaksınız. Bu işlem adım adım sırasında, **cihazları hub 'lara nasıl atamak Istediğinizi seçer**. Kayıt oluşturulduktan sonra, bu makalenin cihaz simülatörünü yapılandırmak için kayıt adı ve birincil veya ikincil SAS anahtarı daha sonra kullanılacaktır.

### <a name="set-up-the-device-simulator"></a>Cihaz simülatörünü ayarlama

Bu örnek, cihaz sağlama hizmeti kullanılarak sağlamayı içeren bir cihaz simülatörü kullanır. Cihaz simülatörü şurada bulunur: [Azure dijital TWINS ve IoT Hub tümleştirme örneği](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Örneği henüz indirmediyseniz, örnek bağlantısına gidip başlık altında *posta yükle* düğmesini seçerek hemen alın. İndirilen klasörü sıkıştırmayı açın.

Bir komut penceresi açın ve indirilen klasöre ve ardından *cihaz simülatör* dizinine gidin. Aşağıdaki komutu kullanarak projenin bağımlılıklarını yükler:

```cmd
npm install
```

Sonra, *. env. Template* dosyasını *. env* adlı yeni bir dosyaya kopyalayın ve bu ayarları girin:

```cmd
PROVISIONING_HOST = "global.azure-devices-provisioning.net"
PROVISIONING_IDSCOPE = "<Device Provisioning Service Scope ID>"
PROVISIONING_REGISTRATION_ID = "<Device Registration ID>"
ADT_MODEL_ID = "dtmi:contosocom:DigitalTwins:Thermostat;1"
PROVISIONING_SYMMETRIC_KEY = "<Device Provisioning Service enrollment primary or secondary SAS key>"
```

Dosyayı kaydedin ve kapatın.

### <a name="start-running-the-device-simulator"></a>Cihaz simülatörünü çalıştırmaya başla

Hala, komut pencerenizde *cihaz simülatör* dizininde, aşağıdaki komutu kullanarak cihaz simülatörünü başlatın:

```cmd
node .\adt_custom_register.js
```

Kayıtlı ve IoT Hub bağlı olduğunu ve sonra ileti gönderilmeye başladığınızı görmeniz gerekir.
:::image type="content" source="media/how-to-provision-using-dps/output.png" alt-text="Cihaz kaydı ve ileti gönderme Komut penceresi gösterme":::

### <a name="validate"></a>Doğrulama

Bu makalede ayarladığınız akışın bir sonucu olarak, cihaz Azure dijital TWINS 'e otomatik olarak kaydedilir. Aşağıdaki [Azure Digital TWıNS CLI](how-to-use-cli.md) komutunu kullanarak, oluşturduğunuz Azure dijital TWINS örneğindeki cihazın ikizi bulun.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

Azure dijital TWINS örneğinde bulunan cihazın ikizi görmeniz gerekir.
:::image type="content" source="media/how-to-provision-using-dps/show-provisioned-twin.png" alt-text="Yeni oluşturulan ikizi gösteren Komut penceresi":::

## <a name="auto-retire-device-using-iot-hub-lifecycle-events"></a>IoT Hub yaşam döngüsü olaylarını kullanarak cihazı otomatik olarak devre dışı bırakma

Bu bölümde, aşağıdaki yoldan cihazları otomatik olarak devre dışı bırakmak için Azure dijital TWINS 'e IoT Hub yaşam döngüsü olayları iliştirirsiniz. Bu, [daha önce](#solution-architecture)gösterilen tam mimarinin bir alıntısıdır.

:::image type="content" source="media/how-to-provision-using-dps/retire.png" alt-text="Cihaz akışını devre dışı bırakma--akış, sayı etiketleme bölümlerinin bulunduğu çözüm mimarisi diyagramının bir alıntısıdır. Termostat cihazı, diyagramdaki Azure hizmetleriyle bağlantı olmadan gösterilir. El ile ' cihaz silme ' eyleminden alınan veriler IoT Hub (1) > Event Hubs (2) > Azure Işlevleri > Azure dijital TWINS (3).":::

İşlem akışının açıklaması aşağıda verilmiştir:
1. Bir dış veya el ile işlem, IoT Hub bir cihazın silinmesini tetikler.
2. IoT Hub cihazı siler ve bir [Olay Hub 'ına](../event-hubs/event-hubs-about.md)yönlendirilecek bir [cihaz yaşam döngüsü](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) olayı oluşturur.
3. Azure işlevi, Azure dijital TWINS 'de cihazın ikizi siler.

Aşağıdaki bölümlerde, bu otomatik devre dışı bırakma cihaz akışını ayarlama adımları gösterilmektedir.

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Artık IoT Hub yaşam döngüsü olaylarını almak için kullanılacak bir Azure [Olay Hub](../event-hubs/event-hubs-about.md)'ı oluşturmanız gerekir. 

Aşağıdaki bilgileri kullanarak, [*Olay Hub 'ı oluşturma*](../event-hubs/event-hubs-create.md) hızlı başlangıç bölümünde açıklanan adımları izleyin:
* Uçtan uca öğreticiyi kullanıyorsanız ([*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)), uçtan uca öğretici için oluşturduğunuz kaynak grubunu yeniden kullanabilirsiniz.
* Olay Hub 'ınızı *lifecycleevents* veya istediğiniz bir şeyi adlandırın ve oluşturduğunuz ad alanını unutmayın. Yaşam döngüsü işlevini ayarlarken ve sonraki bölümlerde yolu IoT Hub, bunları kullanacaksınız.

### <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Sonra, bir işlev uygulaması içinde Event Hubs tetiklenen bir işlev oluşturacaksınız. Uçtan uca öğreticide oluşturulan işlev uygulamasını kullanabilirsiniz ([*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)) veya kendi kendinize. 

Event hub tetikleyicinizi *lifecycleevents* olarak adlandırın ve Olay Hub 'ı tetikleyicisini önceki adımda oluşturduğunuz Olay Hub 'ına bağlayın. Farklı bir olay hub 'ı adı kullandıysanız, bunu aşağıdaki tetikleyici adı ile eşleşecek şekilde değiştirin.

Bu işlev, var olan bir cihazı devre dışı bırakmak için IoT Hub cihaz yaşam döngüsü olayını kullanacaktır. Yaşam döngüsü olayları hakkında daha fazla bilgi için bkz. [*telemetri dışı olayları IoT Hub*](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events). Azure işlevleri ile Event Hubs kullanma hakkında daha fazla bilgi için bkz. Azure [*için azure Event Hubs tetikleyicisi işlevleri*](../azure-functions/functions-bindings-event-hubs-trigger.md).

Yayınlanan işlev uygulamanızın içinde, *Event hub tetikleyicisi* türünde yeni bir işlev sınıfı ekleyin ve aşağıdaki kodu yapıştırın.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIotHub_delete.cs":::

Projeyi kaydedin, sonra işlev uygulamasını yeniden yayımlayın. İşlev uygulamasını yayımlama yönergeleri için, uçtan uca öğreticinin [*uygulamayı yayımlama*](tutorial-end-to-end.md#publish-the-app) bölümüne bakın.

### <a name="configure-your-function"></a>İşlevinizi yapılandırma

Daha sonra, oluşturduğunuz Azure dijital TWINS örneğine ve Olay Hub 'ına başvuruyu içeren işlev uygulamanızda ortam değişkenlerini ayarlamanız gerekir. Uçtan uca öğreticiyi kullandıysanız ([*öğretici: uçtan uca çözümü bağlama*](./tutorial-end-to-end.md)), ilk ayar önceden yapılandırılmıştır.

Bu Azure CLı komutuyla ayarı ekleyin. Bu komut, [makinenizde yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)Azure CLI 'niz varsa [Cloud Shell](https://shell.azure.com)veya yerel olarak çalıştırılabilir.

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

Daha sonra, yeni oluşturulan olay hub 'ına bağlanmak için işlev ortam değişkenini yapılandırmanız gerekecektir.

```azurecli-interactive
az functionapp config appsettings set --settings "EVENTHUB_CONNECTIONSTRING=<Event Hubs SAS connection string Listen>" -g <resource group> -n <your App Service (function app) name>
```

Son adım öğreticideki [*işlev uygulamasına Izin atama*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) bölümünde açıklandığı gibi, Izinler ve yönetilen kimlik rolü atamasının işlev uygulaması için doğru yapılandırıldığından emin olun.

### <a name="create-an-iot-hub-route-for-lifecycle-events"></a>Yaşam döngüsü olayları için IoT Hub yolu oluşturma

Artık cihaz yaşam döngüsü olaylarını yönlendirmek için bir IoT Hub yolu ayarlamanız gerekir. Bu durumda, tarafından tanımlanan cihaz silme olaylarını özellikle dinleyecaksınız `if (opType == "deleteDeviceIdentity")` . Bu, bir cihazın ve dijital ikizi 'in kullanımdan kaldırılması sonuçlandırılıyor ve dijital ikizi öğesinin silinmesini tetikler.

IoT Hub yolu oluşturma yönergeleri şu makalede açıklanmıştır: [*farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanın*](../iot-hub/iot-hub-devguide-messages-d2c.md). *Telemetri olmayan olaylar* bölümünde, **cihaz yaşam döngüsü olaylarını** yolun veri kaynağı olarak kullanabileceğiniz açıklanmaktadır.

Bu kurulum için uygulamanız gereken adımlar şunlardır:
1. Özel bir IoT Hub Olay Hub 'ı uç noktası oluşturun. Bu uç noktanın, [*Olay Hub 'ı oluşturma*](#create-an-event-hub) bölümünde oluşturduğunuz Olay Hub 'ını hedeflemesi gerekir.
2. *Cihaz yaşam döngüsü olayları* rotası ekleyin. Önceki adımda oluşturulan uç noktayı kullanın. Cihaz yaşam döngüsü olaylarını yalnızca, yönlendirme sorgusunu ekleyerek silme olaylarını gönderecek şekilde sınırlayabilirsiniz `opType='deleteDeviceIdentity'` .
    :::image type="content" source="media/how-to-provision-using-dps/lifecycle-route.png" alt-text="Rota Ekle":::

Bu akıştan doldurduktan sonra, her şey cihazları devre dışı bırakmak için uçtan uca ayarlanır.

### <a name="validate"></a>Doğrulama

Kullanımdan kaldırma işleminin tetiklenmesi için cihazı IoT Hub el ile silmeniz gerekir.

[Bu makalenin ilk yarısında](#auto-provision-device-using-device-provisioning-service)IoT Hub ve karşılık gelen dijital ikizi bir cihaz oluşturdunuz. 

Şimdi IoT Hub gidin ve cihazı silin (bunu bir [Azure CLI komutuyla](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest&preserve-view=true#ext-azure-cli-iot-ext-az-iot-hub-device-identity-delete) veya [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Devices%2FIotHubs)' de yapabilirsiniz). 

Cihaz Azure dijital TWINS 'den otomatik olarak kaldırılacak. 

Azure Digital TWINS örneğindeki cihazın ikizi silindiğini doğrulamak için aşağıdaki [Azure Digital TWINS CLI](how-to-use-cli.md) komutunu kullanın.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

Cihazın ikizi artık Azure dijital TWINS örneğinde bulunamadığını görmeniz gerekir.
:::image type="content" source="media/how-to-provision-using-dps/show-retired-twin.png" alt-text="İkizi gösterme Komut penceresi":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

Azure Cloud Shell veya yerel Azure CLı kullanarak, [az Group Delete](/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu, kaynak grubunu kaldırır; Azure dijital TWINS örneği; IoT Hub ve Hub cihaz kaydı; olay Kılavuzu konusu ve ilişkili abonelikler; depolama gibi ilişkili kaynaklar da dahil olmak üzere, Olay Hub 'ları ve her ikisi de Azure Işlevleri uygulaması.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

```azurecli-interactive
az group delete --name <your-resource-group>
```

Ardından, yerel makinenizden indirdiğiniz proje örnek klasörünü silin.

## <a name="next-steps"></a>Sonraki adımlar

Cihazlar için oluşturulan dijital TWINS, Azure dijital TWINS 'de düz bir hiyerarşi olarak depolanır, ancak model bilgileri ve kuruluş için çok düzeyli bir hiyerarşiyle zenginleştirilebilir. Bu kavram hakkında daha fazla bilgi edinmek için şunu okuyun:

* [*Kavramlar: dijital TWINS ve ikizi grafiği*](concepts-twins-graph.md)

Azure dijital TWINS 'te zaten depolanmış olan model ve grafik verilerini kullanarak bu bilgileri otomatik olarak sağlamak için özel mantık yazabilirsiniz. TWINS grafiğindeki bilgileri yönetme, yükseltme ve alma hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [*Nasıl yapılır: dijital ikizi yönetme*](how-to-manage-twin.md)
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)