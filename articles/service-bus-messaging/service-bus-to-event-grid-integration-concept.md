---
title: Azure Service Bus - Event Grid tümleştirmesine genel bakış | Microsoft Docs
description: Bu makalede, Azure Service Bus mesajlaşma 'nın Azure Event Grid ile nasıl tümleştirildiğini gösteren bir açıklama sunulmaktadır.
documentationcenter: .net
author: spelluru
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 71ee21c971b71c4000a123d1561e7e93d21203e1
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629156"
---
# <a name="azure-service-bus-to-event-grid-integration-overview"></a>Azure Service Bus - Event Grid tümleştirmesine Genel Bakış

Azure Service Bus, Azure Event Grid’e yeni bir tümleştirme başlatmıştır. Bu özelliğin temel kullanım modelinde, düşük ileti hacmine sahip Service Bus kuyruklarının veya aboneliklerinin sürekli olarak iletiler için yoklama yapan bir alıcısının olması gerekmez. 

Service Bus artık bir alıcı mevcut olmadığında ve bir kuyrukta veya abonelikte iletiler olduğunda olayları Event Grid’e gönderebilir. Service Bus ad alanlarınıza Event Grid abonelikleri oluşturabilir, bu olayları dinleyebilir ve bir alıcı başlatarak olaylara tepki verebilirsiniz. Bu özellik sayesinde Service Bus’ı reaktif programlama modellerinde kullanabilirsiniz.

Özelliği etkinleştirmek için gereken öğeler:

* En az bir Service Bus kuyruğu olan bir Service Bus Premium ad alanı veya en az bir aboneliği olan bir Service Bus konusu.
* Service Bus ad alanına katkıda bulunan erişimi.
* Ek olarak, Service Bus ad alanı için bir Event Grid aboneliğiniz olması da gerekir. Bu abonelik, alınacak iletiler olduğuna dair Event Grid’den bildirim alır. Normalde aboneler, web uygulaması ile iletişim kuran bir web kancası, Azure İşlevleri veya Azure App Service’in Logic Apps özelliği olabilir. Daha sonra abone iletileri işler. 

![19][]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="verify-that-you-have-contributor-access"></a>Katkıda bulunan erişimine sahip olduğunuzu doğrulama
Service Bus ad alanına gidin ve ardından **erişim denetimi (IAM)** seçeneğini belirleyin ve **rol atamaları** sekmesini seçin. Ad alanına katkıda bulunan erişimi olduğunu doğrulayın. 

### <a name="events-and-event-schemas"></a>Olaylar ve olay şemaları

Service Bus şu anda iki senaryo için olaylar gönderir:

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#deadletter-messages-available-event)
* [Activemessagesavailabledönembildirimleri](#active-messages-available-periodic-notifications)
* [DeadletterMessagesAvailablePeriodicNotifications](#deadletter-messages-available-periodic-notifications)

Ayrıca Service Bus, standart Event Grid güvenliğini ve [kimlik doğrulaması mekanizmalarını](../event-grid/security-authentication.md) da kullanır.

Daha fazla bilgi için bkz. [Azure Event Grid olay şemaları](../event-grid/event-schema.md).

#### <a name="active-messages-available-event"></a>Etkin İletiler Kullanılabilir olayı

Bir kuyrukta veya abonelikte etkin iletileriniz varsa ve bir alıcı dinleme gerçekleştirmiyorsa bu olay oluşturulur.

Bu olayın şeması:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="deadletter-messages-available-event"></a>Iletileri sahipsiz kullanılabilir olayı

İletiler içeren ve etkin alıcılar içermeyen Geçerliliğini Yitirmiş kuyruk başına en az bir olay alırsınız.

Bu olayın şeması:

```JSON
[{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

#### <a name="active-messages-available-periodic-notifications"></a>Etkin Iletiler kullanılabilir dönemsel bildirimler

Bu olay, belirli bir kuyrukta veya abonelikte etkin dinleyiciler olsa bile belirli bir kuyrukta veya abonelikte etkin iletileriniz varsa düzenli olarak oluşturulur.

Olay şeması aşağıdaki gibidir.

```json
[{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailablePeriodicNotifications",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

#### <a name="deadletter-messages-available-periodic-notifications"></a>Iletileri sahipsiz kullanılabilir düzenli bildirimler

Bu olay, belirli bir kuyruğun veya aboneliğin sahipsiz varlığında etkin dinleyiciler olsa bile, belirli bir kuyrukta veya abonelikte iletileri yok saydıysanız düzenli olarak oluşturulur.

Olay şeması aşağıdaki gibidir.

```json
[{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailablePeriodicNotifications",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-many-events-are-emitted-and-how-often"></a>Ne sıklıkla ve kaç tane olay gönderilir?

Ad alanında birden fazla kuyruk ve konu veya abonelik varsa, kuyruk başına en az bir ve abonelik başına bir olay alırsınız. Service Bus varlığında bir ileti yoksa ve yeni bir ileti gelirse olaylar hemen gönderilir. Alternatif olarak, Service Bus etkin bir alıcı algılamazsa iki dakikada bir olaylar gönderilir. İleti taraması, olayları kesintiye uğratmaz.

Varsayılan olarak Service Bus, ad alanındaki tüm varlıklar için olaylar gönderir. Yalnızca belirli varlıklar için olaylar almak istiyorsanız sonraki bölüme bakın.

### <a name="use-filters-to-limit-where-you-get-events-from"></a>Olayları aldığınız yeri sınırlamak için filtreleri kullanma

Örneğin, yalnızca ad alanınızdaki bir abonelik veya kuyruktan olaylar almak istiyorsanız, Event Grid tarafından sağlanan *ile başlar* veya *ile biter* filtrelerini kullanabilirsiniz. Bazı arabirimlerde filtrelere, *ön* ve *sonek* filtreleri adı verilir. Tümü için değil, birden fazla kuyruk ve abonelik için olaylar almak istiyorsanız, birden fazla Event Grid aboneliği oluşturabilir ve her birine ilişkin bir filtre sağlayabilirsiniz.

## <a name="create-event-grid-subscriptions-for-service-bus-namespaces"></a>Service Bus ad alanları için Event Grid abonelikleri oluşturma

Service Bus ad alanları için Event Grid abonelikleri oluşturmanın üç farklı yolu vardır:

* Azure portalında
* [Azure CLI](#azure-cli-instructions)’da
* [PowerShell](#powershell-instructions) 'de

## <a name="azure-portal-instructions"></a>Azure portalı yönergeleri

Yeni Event Grid aboneliği oluşturmak için aşağıdakileri yapın:
1. Azure portalında ad alanınıza gidin.
2. Sol bölmede **Event Grid**’i seçin. 
3. **Olay aboneliği** seçin.  

   Aşağıdaki resimde, bir Event Grid aboneliği içeren bir ad alanı gösterilmektedir:

   ![Event Grid abonelikleri](./media/service-bus-to-event-grid-integration-concept/sbtoeventgridportal.png)

   Aşağıdaki resimde, belirli bir filtreleme olmadan bir işleve veya web kancasına nasıl abone olunacağı gösterilmektedir:

   ![21][]

## <a name="azure-cli-instructions"></a>Azure CLI yönergeleri

İlk olarak, Azure CLI sürüm 2.0 veya üzerinin yüklü olduğundan emin olun. [Yükleyiciyi indirin](/cli/azure/install-azure-cli). **Windows + X**' i seçin ve ardından Yönetici izinleriyle yeni bir PowerShell konsolu açın. Alternatif olarak Azure portalındaki bir komut kabuğunu kullanabilirsiniz.

Şu kodu yürütün:

 ```azurecli-interactive
az login

az account set -s "<Azure subscription name>"

namespaceid=$(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name "<service bus namespace>" --resource-group "<resource group that contains the service bus namespace>" --query id --output tsv

az eventgrid event-subscription create --resource-id $namespaceid --name "<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>" --endpoint "<your_function_url>" --subject-ends-with "<YOUR SERVICE BUS SUBSCRIPTION NAME>"
```

BASH kullanıyorsanız 

## <a name="powershell-instructions"></a>PowerShell yönergeleri

Azure PowerShell’in yüklenmiş olduğundan emin olun. [Yükleyiciyi indirin](/powershell/azure/install-Az-ps). **Windows + X** tuşlarına basın ve Yönetici izinleriyle yeni bir PowerShell konsolu açın. Alternatif olarak Azure portalındaki bir komut kabuğunu kullanabilirsiniz.

```powershell-interactive
Connect-AzAccount

Select-AzSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module Az.ServiceBus

$NSID = (Get-AzServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id

New-AzEVentGridSubscription -EventSubscriptionName "<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>" -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith "<YOUR SERVICE BUS SUBSCRIPTION NAME>"
```

Buradan diğer kurulum seçeneklerini keşfedebilir veya olayların akışa alınmasını test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Service Bus ve Event Grid [örnekleri](service-bus-to-event-grid-integration-example.md) alın.
* [Event Grid](../event-grid/index.yml) hakkında daha fazla bilgi edinin.
* [Azure İşlevleri](../azure-functions/index.yml) hakkında daha fazla bilgi edinin.
* [Logic Apps](../logic-apps/index.yml) hakkında daha fazla bilgi edinin.
* [Service Bus](/azure/service-bus/) hakkında daha fazla bilgi edinin.

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[renkli]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
