---
title: Tüketil 'ye Genel Bakış
description: Tüketil 'ye genel bakış edinme
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 77cb6a693c5a73d0498b0acf9bc4ad8cc9f8f32f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83773999"
---
# <a name="consul"></a>Tüketil

## <a name="overview"></a>Genel Bakış

[Tüketil][consul] , çalışma zamanı platformları genelinde Hizmetleri bağlamaya ve güvenli hale getirmeye yönelik çoklu bir veri merkezi hizmet ağı çözümüdür. [Connect][consul-features] , hizmet ağı özellikleri sağlayan bileşendir.

## <a name="architecture"></a>Mimari

Tüketil, [Varsayılan olarak][consul-sidecar] [Envoy][envoy-proxy]tabanlı parçalardan oluşan bir veri düzlemi sağlar. Tüketil 'nin takılabilir bir ara sunucu mimarisi vardır. Bu akıllı proxy 'ler, yerleşik uygulamalarınızın ve iş yüklerinizin içindeki ve içindeki tüm ağ trafiğini denetler.

Denetim düzlemi yapılandırmayı ve ilkeyi aşağıdaki [Bileşenler][consul-architecture]aracılığıyla yönetir:

- **Sunucu** -tüketil küme durumunu tutan sunucu modunda çalışan bir Tüketil Aracısı.

- **İstemci** -hafif istemci modunda çalışan bir Tüketil Aracısı. Her işlem düğümünün çalışan bir Istemci Aracısı olmalıdır. Bu istemci aracıları, iş yükleri ve tüketilen Tüketil yapılandırması arasındaki yapılandırma ve ilkedir. 

Aşağıdaki mimari diyagramda, veri düzlemi ve denetim düzlemi içindeki çeşitli bileşenlerin nasıl etkileşimde bulunduğu gösterilmektedir.

![Tüketil bileşenlerine ve mimarisine genel bakış.](media/servicemesh/consul/about-architecture.png)


## <a name="selection-criteria"></a>Seçim ölçütü

İş yükleriniz için Tüketil değerlendirirken aşağıdaki alanların anlaşılması ve dikkate alınması önemlidir:

- [Tüketil Ilkeleri](#consul-principles)
- [Özellikler](#capabilities)
- [Senaryolar](#scenarios)


### <a name="consul-principles"></a>Tüketil ilkeleri

Aşağıdaki ilkeler, Tüketil projesini [gösterir][consul-principles] :

- **API-odaklı** -tüm yapılandırma ve ilke ile birlikte.

- Çalışma zamanı platformları (Kubernetes, VM 'Ler, sunucusuz) arasında her yerden bağlanma iş yüklerini **çalıştırın ve bağlayın** .

- Altyapı genelinde güvenli şekilde bağlama iş yüklerini **genişletin ve tümleştirin** .


### <a name="capabilities"></a>Özellikler

Tüketil aşağıdaki özellik kümesini sağlar:

- **Kafes** – ağ geçidi (çoklu veri merkezi), sanal makineler (küme düğümleri dışında), hizmet eşitleme, yerleşik hata ayıklama seçeneği

- **Proxy 'ler** – Envoy, yerleşik proxy, takılabilir, L4 proxy Windows iş yükleri için kullanılabilir

- **Trafik yönetimi** – yönlendirme, bölme, çözümleme

- **İlke** – amaçları, ACL 'ler

- **Güvenlik** – yetkilendirmeyle, kimlik doğrulama, şifreleme, SPIFFE tabanlı kimlikler, dış CA (kasa), sertifika yönetimi ve döndürme

- **Observability** – ölçümler, UI panosu, Prometheus, grafana


### <a name="scenarios"></a>Senaryolar

Tüketil, aşağıdaki senaryolar için uygundur ve önerilir:

- Mevcut Tüketil bağlı iş yüklerini genişletme

- Sertifika yönetimi etrafındaki uyumluluk gereksinimleri

- Çoklu küme hizmeti ağı

- Hizmet kafeslerine dahil edilecek VM tabanlı iş yükleri



## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgelerde, Azure Kubernetes Service (AKS) üzerinde Tüketil 'yi nasıl yükleyebileceğiniz açıklanmaktadır:

> [!div class="nextstepaction"]
> [Azure Kubernetes Service (AKS) ' de Tüketil 'yi kurma][consul-install]

Ayrıca, Tüketil özelliklerini ve mimarisini de inceleyebilirsiniz:

- [Tüketil kullanmaya başlama Öğreticileri][consul-getting-started]
- [Tüketil özellikleri][consul-features]
- [Tüketil mimarisi][consul-architecture]
- [Tüketil-Connect 'in nasıl çalıştığı][consul-how-connect-works]

<!-- LINKS - external -->
[consul]: https://www.consul.io/mesh.html
[consul-features]: https://www.consul.io/docs/connect/index.html
[consul-architecture]: https://www.consul.io/docs/internals/architecture.html
[consul-sidecar]: https://www.consul.io/docs/connect/proxies.html
[consul-how-connect-works]: https://www.consul.io/docs/connect/connect-internals.html
[consul-principles]: https://www.consul.io/
[consul-getting-started]:https://learn.hashicorp.com/consul?track=gs-consul-service-mesh#gs-consul-service-mesh

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[consul-install]: ./servicemesh-consul-install.md
