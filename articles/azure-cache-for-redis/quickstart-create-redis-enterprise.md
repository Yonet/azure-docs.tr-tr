---
title: 'Hızlı başlangıç: Kurumsal katman önbelleği oluşturma'
description: Bu hızlı başlangıçta, Redsıs kurumsal katmanı için bir Azure önbelleği örneği oluşturmayı öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: mvc
ms.topic: quickstart
ms.date: 10/28/2020
ms.openlocfilehash: 95f6e2cf7803ff0b152f33c08f170725b5a0e94e
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95795685"
---
# <a name="quickstart-create-an-enterprise-tier-cache-preview"></a>Hızlı başlangıç: Kurumsal katman önbelleği oluşturma (Önizleme)

Redsıs ' Kurumsal katmanları için Azure önbelleği, Azure 'da tamamen tümleşik ve yönetilen [Redsıs Enterprise](https://redislabs.com/redis-enterprise/) sağlar. Şu anda önizleme olarak kullanılabilir. Bu önizlemede iki yeni katman vardır:
* Verileri depolamak için bir sanal makinede geçici bellek (DRAM) kullanan kuruluş
* Verileri depolamak için hem geçici hem de geçici olmayan belleği (NVMe veya SSD) kullanan Enterprise Flash.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce bir Azure aboneliğine sahip olmanız gerekir. Hesabınız yoksa bir [Hesap](https://azure.microsoft.com/)oluşturun. Daha fazla bilgi için bkz. [Kurumsal katman gereksinimleri](cache-overview.md#enterprise-tier-requirements).

## <a name="create-a-cache"></a>Bir önbellek oluşturma
1. Önbellek oluşturmak için, önizleme davetinizdeki bağlantıyı kullanarak Azure portal oturum açın ve **kaynak oluştur**' u seçin.

1. **Yeni** sayfada **veritabanları** ' nı seçin ve ardından **redsıs için Azure önbelleği**' ni seçin.
   
   :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Redsıs için Azure önbelleğini seçin":::
   
1. **Yeni Redis Cache** sayfasında, yeni önbelleğiniz için ayarları yapılandırın.
   
   | Ayar      | Önerilen değer  | Açıklama |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonelik** | Açılır ve aboneliğinizi seçin. | Redsıs örneği için bu yeni Azure önbelleğinin oluşturulacağı abonelik. | 
   | **Kaynak grubu** | Açılır ve bir kaynak grubu seçin veya **Yeni oluştur** ' u seçin ve yeni bir kaynak grubu adı girin. | Önbelleğinizin ve diğer kaynaklarınızın oluşturulacağı kaynak grubunun adı. Tüm uygulama kaynaklarınızı tek bir kaynak grubuna yerleştirerek, bunları birlikte kolayca yönetebilir veya silebilirsiniz. | 
   | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca rakam, harf veya kısa çizgi içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlamalı ve bitmeli ve ardışık kısa çizgi içeremez. Önbellek örneğinizin *ana bilgisayar adı* olacaktır *\<DNS name> . <Azure region> . redisenterprise.cache.azure.net*. | 
   | **Konum** | Açılır ve bir konum seçin. | Önizleme sırasında kurumsal katmanlar sınırlı Azure bölgelerinde kullanılabilir. |
   | **Önbellek türü** | Açılır ve bir *Kurumsal* veya *Kurumsal Flash* katmanı ve bir boyut seçin. |  Katman, önbellek için kullanılabilen boyut, performans ve özellikleri belirler. |
   
   :::image type="content" source="media/cache-create/enterprise-tier-basics.png" alt-text="Kurumsal katman temelleri":::

   > [!NOTE] 
   > Devam etmeden önce "Şartlar" altındaki kutuyu kontrol ettiğinizden emin olun.
   >

1. **İleri ' yi seçin: ağ oluşturma** ve atlama.

   > [!NOTE] 
   > Özel bağlantı seçeneği kullanıma alındı ve bölgenizde hemen kullanılamıyor olabilir.
   >

1. **İleri: Gelişmiş** ' i seçin ve **kümeleme ilkesini** **Kurumsal** olarak ayarlayın.
   
   Varsayılan ayarları koruyabilir veya uygun şekilde değiştirebilirsiniz. **Yalnızca TLS aracılığıyla erişime Izin ver** seçeneği açıldığında, uygulamanızdaki yeni önbelleğe erışmek için TLS kullanmanız gerekir.

   :::image type="content" source="media/cache-create/enterprise-tier-advanced.png" alt-text="Kurumsal katman gelişmiş":::

   > [!NOTE] 
   > Redsıs modülleri henüz Enterprise Flash katmanında desteklenmez. Redl modülünü kullanmayı planlıyorsanız, bir kurumsal katman önbelleği seçtiğinizden emin olun.
   >
   
1. **İleri ' yi seçin: Etiketler** ve atla.

1. **Sonraki: Gözden geçirme ve oluşturma**’yı seçin.

   :::image type="content" source="media/cache-create/enterprise-tier-summary.png" alt-text="Kurumsal katman Özeti":::

1. Ayarları gözden geçirin ve **Oluştur**' a tıklayın.
   
   Önbelleğin oluşturulması biraz zaman alır. Redsıs **genel bakış** sayfasında ilerlemeyi izleyebilirsiniz. **Durum** **çalışıyor** olarak görüntülendiğinde, önbellek kullanıma hazırdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, redin için Azure önbelleğinin kurumsal katman örneğini oluşturmayı öğrendiniz.

> [!div class="nextstepaction"]
> [Redsıs için Azure önbelleği kullanan bir ASP.NET Web uygulaması oluşturun.](./cache-web-app-howto.md)

