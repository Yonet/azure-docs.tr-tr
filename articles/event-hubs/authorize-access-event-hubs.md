---
title: Azure Event Hubs'a erişimi yetkilendirme
description: Bu makalede, Azure Event Hubs kaynaklarına erişimi yetkilendirmek için farklı seçenekler hakkında bilgi sağlanır.
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 5fc35387f4ac28ad4dd28bea0013bcdf1e1e9f02
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92332391"
---
# <a name="authorize-access-to-azure-event-hubs"></a>Azure Event Hubs'a erişimi yetkilendirme
Bir olay hub 'ından olayları/verileri her yayımladığınızda veya kullandığınızda, istemciniz Event Hubs kaynaklara erişmeye çalışıyor. Güvenli bir kaynağa yönelik her isteğin, hizmetin verileri yayımlamak/kullanmak için gerekli izinlere sahip olduğundan emin olmak için yetkilendirilmiş olması gerekir. 

Azure Event Hubs, güvenli kaynaklara erişimi yetkilendirmek için aşağıdaki seçenekleri sunar:

- Azure Active Directory
- Paylaşılan erişim imzası

> [!NOTE]
> Bu makale hem Event Hubs hem de [Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) senaryolar için geçerlidir. 

## <a name="azure-active-directory"></a>Azure Active Directory
Event Hubs kaynakları için Azure Active Directory (Azure AD) tümleştirmesi, bir istemcinin kaynaklara erişimi üzerinde ayrıntılı denetim için Azure rol tabanlı erişim denetimi (Azure RBAC) sağlar. Güvenlik sorumlusuna, bir Kullanıcı, Grup veya uygulama hizmeti sorumlusu olabilecek izinler vermek için Azure RBAC kullanabilirsiniz. Güvenlik sorumlusunun bir OAuth 2,0 belirteci döndürmesi için Azure AD tarafından kimliği doğrulanır. Belirteç, bir Event Hubs kaynağına erişim isteğine yetki vermek için kullanılabilir.

Azure AD ile kimlik doğrulama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Active Directory kullanarak Azure Event Hubs istek kimliklerini doğrulama](authenticate-application.md)
- [Azure Active Directory kullanarak Event Hubs kaynaklarına erişim yetkisi verin](authorize-access-azure-active-directory.md).

## <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları 
Event Hubs kaynakları için paylaşılan erişim imzaları (SAS), Event Hubs kaynaklara sınırlı bir temsilci erişimi sağlar. İmzanın geçerli olduğu veya izin verdiği izinlerde zaman aralığındaki kısıtlamalar, kaynakları yönetirken esneklik sağlar. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak kimlik doğrulama](authenticate-shared-access-signature.md). 

Azure AD tarafından döndürülen bir OAuth 2,0 belirtecini kullanarak kullanıcıları veya uygulamaları yetkilendirmek, paylaşılan erişim imzaları (SAS) üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile, erişim belirteçlerini kodunuza depolamanız ve potansiyel güvenlik açıklarına karşı risk uygulamanız gerekmez. Event Hubs kaynaklarına ayrıntılı erişim sağlamak için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD, SAS belirteçlerini yönetmeye gerek olmadan veya güvenliği aşılmış bir SAS iptal etme konusunda endişelenmeden benzer yetenekler sunmaktadır. 

Varsayılan olarak, tüm Event Hubs kaynakları güvenlidir ve yalnızca hesap sahibi tarafından kullanılabilir. İstemcilere, Olay Hub 'ı kaynaklarına erişim izni vermek için yukarıda özetlenen yetkilendirme stratejilerinden herhangi birini de kullanabilirsiniz. Microsoft, en yüksek güvenlik ve kullanım kolaylığı için mümkün olduğunda Azure AD 'nin kullanılmasını önerir.

SAS kullanarak yetkilendirme hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzalarını kullanarak Event Hubs kaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md).

## <a name="next-steps"></a>Sonraki adımlar
- GitHub deponuzda yayınlanan [Azure RBAC örneklerini](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac) inceleyin. 
- Aşağıdaki makalelere bakın:
    - [Azure Active Directory kullanarak bir uygulamadan Azure Event Hubs istek kimliklerini doğrulama](authenticate-application.md)
    - [Event Hubs kaynaklara erişmek için Azure Active Directory ile yönetilen bir kimliğin kimliğini doğrulama](authenticate-managed-identity.md)
    - [Paylaşılan erişim Imzalarını kullanarak Azure Event Hubs istek kimliklerini doğrulama](authenticate-shared-access-signature.md)
    - [Azure Active Directory kullanarak Event Hubs kaynaklarına erişim yetkisi verme](authorize-access-azure-active-directory.md)
    - [Paylaşılan erişim Imzalarını kullanarak Event Hubs kaynaklarına erişim yetkisi verme](authorize-access-shared-access-signature.md)

