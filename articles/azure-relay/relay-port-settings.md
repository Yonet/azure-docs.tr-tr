---
title: Azure Relay bağlantı noktası ayarları | Microsoft Docs
description: Bu makale, Azure Relay için bağlantı noktası değerleri için gereken yapılandırmayı açıklayan bir tablo içerir.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 97640debe81041ff7e2b082c6a9ac606d6088664
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85314280"
---
# <a name="azure-relay-port-settings"></a>Azure Relay bağlantı noktası ayarları

Aşağıdaki tabloda Azure Relay için bağlantı noktası değerleri için gereken yapılandırma açıklanmaktadır.

## <a name="hybrid-connections"></a>Karma Bağlantılar

Karma Bağlantılar, bağlantı noktası 443 üzerinde WebSockets kullanarak, yalnızca **https** kullanan temel taşıma MEKANIZMASı olarak TLS kullanır. 

## <a name="wcf-relays"></a>WCF Geçişleri
  
|Bağlama|Taşıma Güvenliği|Bağlantı noktası|  
|-------------|------------------------|----------|  
|[Basichttprelaybinding sınıfı](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (istemci)|Evet|HTTPS| 
|" |Hayır|HTTP|  
|[Basichttprelaybinding sınıfı](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (hizmet)|Herhangi biri|9351/HTTP|  
|[Neteventrelaybinding sınıfı](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (istemci)|Evet|9351/HTTPS|  
|" |Hayır|9350/HTTP|  
|[Neteventrelaybinding sınıfı](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (hizmet)|Herhangi biri|9351/HTTP|  
|[Nettcprelaybinding sınıfı](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (istemci/hizmet)|Herhangi biri|5671/9352/HTTP (karma kullanılıyorsa 9352/9353)|  
|[NetOnewayRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (istemci)|Evet|9351/HTTPS|  
|" |Hayır|9350/HTTP|  
|[NetOnewayRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (hizmet)|Herhangi biri|9351/HTTP|  
|[WebHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (istemci)|Evet|HTTPS|  
|" |Hayır|HTTP|  
|[WebHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (hizmet)|Herhangi biri|9351/HTTP|  
|[WS2007HttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (istemci)|Evet|HTTPS|  
|" |Hayır|HTTP|  
|[WS2007HttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (hizmet)|Herhangi biri|9351/HTTP|

## <a name="next-steps"></a>Sonraki adımlar
Azure Relay hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:
* [Azure Relay nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)