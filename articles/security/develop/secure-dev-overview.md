---
title: Microsoft Azure için güvenli geliştirme en iyi uygulamaları
description: Daha güvenli kod geliştirmenize ve bulutta daha güvenli bir uygulama dağıtmanıza yardımcı olacak en iyi uygulamalar.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
ms.subservice: security-develop
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: baaa311620f5c10948aa3494002ce359cc5dab28
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94517181"
---
# <a name="secure-development-best-practices-on-azure"></a>Azure 'da güvenli geliştirme en iyi uygulamaları
Bu makale serisi, bulut için uygulama geliştirirken göz önünde bulundurmanız gereken güvenlik etkinliklerini ve denetimleri sunar. Yaşam döngüsünün her aşamasında dikkate alınması gereken Microsoft Security Development Lifecycle (SDL) ve güvenlik sorularının ve kavramların aşamaları ele alınmıştır. Amaç, daha güvenli bir uygulama tasarlamak, geliştirmek ve dağıtmak için yaşam döngüsünün her aşamasında kullanabileceğiniz etkinlikleri ve Azure hizmetlerini tanımlamanıza yardımcı olmaktır.

Makalelerdeki öneriler, Azure güvenliği ile ilgili deneyimimizden ve müşterilerimizin deneyimlerinden gelir. Bu makaleleri, geliştirme projenizin belirli bir aşamasında göz önünde bulundurmanız gerekenler için bir başvuru olarak kullanabilirsiniz, ancak baştan sona en az bir kez baştan sona kadar tüm makalelerin de okunmasını öneririz. Tüm makalelerin okunması, projenizin önceki aşamalarında kaçırıldıklarınızın kavramlarını tanıtır. Ürününüzü bırakmadan önce bu kavramları uygulamak, güvenli yazılım oluşturmanıza, güvenlik uyumluluk gereksinimlerini karşılamanızı ve geliştirme maliyetlerini azaltmanıza yardımcı olabilir.

Bu makalelerin, güvenli Azure uygulamaları oluşturan ve dağıtan tüm düzeylerde yazılım tasarımcıları, geliştiriciler ve test ediciler için bir kaynak olması amaçlanmıştır.

## <a name="overview"></a>Genel Bakış

Güvenlik, herhangi bir uygulamanın en önemli yönlerinden biridir ve hemen bir şey olması basit bir şey değildir. Neyse ki Azure, uygulamanızı bulutta güvenli hale getirmenize yardımcı olabilecek birçok hizmet sunar. Bu makaleler, yazılım geliştirme yaşam döngüsünün her aşamasında uygulayabileceğiniz, daha güvenli kod geliştirmenize ve bulutta daha güvenli bir uygulama dağıtmanıza yardımcı olmak için etkinlikleri ve Azure hizmetlerini ele alabilir.

## <a name="security-development-lifecycle"></a>Güvenlik geliştirme yaşam döngüsü

Güvenli yazılım geliştirme için aşağıdaki en iyi yöntemler, proje yönteminize ([şelale](https://en.wikipedia.org/wiki/Waterfall_model), [çevik](https://en.wikipedia.org/wiki/Agile_software_development)veya [DevOps](https://en.wikipedia.org/wiki/DevOps)) bağımsız olarak, gereksinim analizinden, yazılım geliştirme yaşam döngüsünün her bir aşamasına kadar güvenlik tümleştirilmesine gerek duyar. Yüksek profilli veri ihlallerinin uyanma ve işlemsel güvenlik kusuruna yararlanma konusunda, daha fazla geliştirici güvenlik için geliştirme sürecinde ele alınması gerektiğini anlamaktır.

Daha sonra geliştirme yaşam döngünüzün bir sorununu düzelttikten sonra, bu daha fazla çözüm size ücretlendirilecektir. Güvenlik sorunları özel durum değildir. Yazılım geliştirmelerinizin erken aşamalarında güvenlik sorunlarını yoksayabilirsiniz, izleyen her aşama önceki aşamanın güvenlik açıklarını da devralınabilir. Son ürününüz birden çok güvenlik sorununu ve bir ihlal olasılığını birikmiş olacak. Geliştirme yaşam döngüsünün her aşamasında güvenlik oluşturmak, sorunları erken yakalemenize yardımcı olur ve Geliştirme maliyetlerinizi azaltmanıza yardımcı olur.

Yaşam döngüsünün her aşamasında güvenli yazılım geliştirme uygulamalarını yerine getirmek için kullanabileceğiniz etkinlikler ve Azure hizmetleri tanıtmak üzere Microsoft [güvenlik geliştirme yaşam döngüsü (SDL)](/previous-versions/windows/desktop/cc307891(v=msdn.10)) aşamalarını izliyoruz.

SDL aşamaları şunlardır:

  - Eğitim
  - Gereksinimler
  - Tasarım
  - Uygulama
  - Doğrulama
  - Yayınla
  - Yanıt

![Güvenlik Geliştirme Yaşam Döngüsü](./media/secure-dev-overview/01-sdl-phase.png)

Bu makalelerde, SDL aşamalarını tasarım, geliştirme ve dağıtma olarak gruplandırıyoruz.

## <a name="engage-your-organizations-security-team"></a>Kuruluşunuzun güvenlik ekibine katılın

Kuruluşunuz, geliştirme yaşam döngüsü sırasında başlangıçtan itibaren güvenlik etkinliklerinin bitmesini sağlayan resmi bir uygulama güvenlik programına sahip olabilir. Kuruluşunuzda güvenlik ve uyumluluk takımları varsa, uygulamanızı geliştirmeye başlamadan önce bu kişilere da uyduğunuzdan emin olun. Kaçırılmış herhangi bir görev olup olmadığı, SDL her aşamasında sorun.

Birçok okuyucunun, uyum sağlamak için bir güvenlik veya uyumluluk takımına sahip olabileceğini anladık. Bu makaleler, SDL 'nin her aşamasında göz önünde bulundurmanız gereken güvenlik soruları ve kararlarında size rehberlik etmenize yardımcı olabilir.

## <a name="resources"></a>Kaynaklar

Güvenli uygulamalar geliştirme ve uygulamalarınızın güvenliğini sağlamaya yardımcı olma hakkında daha fazla bilgi edinmek için aşağıdaki kaynakları kullanın:

[Microsoft Security Development lifecycle (SDL)](/previous-versions/windows/desktop/cc307891(v=msdn.10)) : SDL, Microsoft 'un geliştiricilerin daha güvenli yazılım oluşturmalarına yardımcı olan bir yazılım geliştirme işlemidir. Geliştirme maliyetlerini azaltırken güvenlik uyumluluk gereksinimlerini karşılamanıza yardımcı olur.

[Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page) – OWASP, Web uygulaması güvenliği alanında ücretsiz olarak kullanılabilen makaleler, Yöntemler, Araçlar ve teknolojiler üreten bir çevrimiçi toplulukdır.

[Bir patron gibi sola itim,](https://code.likeagirl.io/pushing-left-like-a-boss-part-1-80f1f007da95?WT.mc_id=docs-blog-tajanca) geliştiricilerin daha güvenli kod oluşturmak için tamamlaması gereken farklı türlerdeki uygulama güvenliği etkinliklerini özetleyen bir dizi çevrimiçi makale.

[Microsoft](../../active-directory/develop/index.yml) Identity Platform – Microsoft Identity platformu, Azure AD kimlik hizmeti ve geliştirici platformunun bir evrimi. Bu, bir kimlik doğrulama hizmeti, açık kaynak kitaplıkları, uygulama kaydı ve yapılandırma, tam geliştirici belgeleri, kod örnekleri ve diğer geliştirici içeriğinden oluşan tam özellikli bir platformdur. Microsoft Identity platformu, OAuth 2,0 ve OpenID Connect gibi sektör standardı protokolleri destekler.

[Azure çözümleri için En Iyi güvenlik uygulamaları](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions/) : Azure kullanarak bulut çözümlerini tasarlarken, dağıtırken ve yönetirken en iyi güvenlik uygulamalarının bir koleksiyonu. Bu kağıdın BT profesyonelleri için bir kaynak olması amaçlanmıştır. Bu, güvenli Azure çözümleri oluşturup dağıtan tasarımcılar, mimarlar, geliştiriciler ve test ediciler içerebilir.

[Azure 'Da güvenlik ve uyumluluk şemaları](https://servicetrust.microsoft.com/ViewPage/BlueprintOverview) – Azure Güvenlik ve uyumluluk planları, katı yönetmeliklere ve standartlarla uyumlu bulut destekli uygulamalar oluşturmanıza ve bunları başlatmanıza yardımcı olabilecek kaynaklardır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenli uygulamaları tasarlamanıza, geliştirmenize ve dağıtmanıza yardımcı olabilecek güvenlik denetimleri ve etkinlikleri önerilir.

- [Güvenli uygulamalar tasarlama](secure-design.md)
- [Güvenli uygulamalar geliştirme](secure-develop.md)
- [Güvenli uygulamalar dağıtma](secure-deploy.md)