---
title: Kullanılabilirlik ve artıklık Azure Key Vault-Azure Key Vault | Microsoft Docs
description: Azure Key Vault kullanılabilirliği ve artıklığı hakkında bilgi edinin.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: sudbalas
ms.openlocfilehash: d66fe736936963e601aad7cba7bdaa94f0c3ec3f
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96518456"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault kullanılabilirliği ve yedekliliği

, Tek tek hizmet bileşenleri başarısız olsa bile, anahtarlarınızın ve sırlarınızın uygulamanız için kullanılabilir kalmasını sağlamak için birden çok artıklık katmanı Azure Key Vault.

> [!NOTE]
> Bu kılavuz, kasaların için geçerlidir. Yönetilen HSM havuzları, farklı bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma modeli kullanır. Daha fazla bilgi için bkz. [YÖNETILEN HSM olağanüstü durum kurtarma Kılavuzu](../managed-hsm/disaster-recovery-guide.md) .

Anahtar kasanızın içeriği bölge içinde ve bir ikincil bölgeye en az 150 mil uzakta, ancak anahtarlarınızın ve gizli dizilerlerinizin yüksek dayanıklılığını sürdürmek için aynı coğrafya dahilinde çoğaltılır. Belirli bölge çiftleri hakkında daha fazla bilgi için bkz. [Azure eşleştirilmiş bölgeler](../../best-practices-availability-paired-regions.md). Eşleştirilmiş bölge modelinin özel durumu Brezilya Güney, yalnızca Brezilya Güney içinde verileri güncel tutma seçeneğinin kullanılmasına izin verir. Brezilya Güney, tek konum/bölge içinde verilerinizi üç kez çoğaltmak için bölge yedekli depolama (ZRS) kullanır.   

Anahtar Kasası hizmetindeki tek tek bileşenler başarısız olursa, işlevselliğe bir azalma olmadığından emin olmak için, içindeki bölge adımındaki alternatif bileşenler. Bu işlemi başlatmak için herhangi bir işlem yapmanız gerekmez, otomatik olarak gerçekleşir ve sizin için saydam hale gelir.

Tüm Azure bölgesinin kullanılamadığı nadir bir olayda, bu bölgedeki Azure Key Vault yaptığınız istekler, Brezilya Güney bölgesinin olması dışında ikincil bir bölgeye otomatik olarak yönlendirilir (*Yük devredildi*). Birincil bölge yeniden kullanılabilir olduğunda, istekler birincil bölgeye geri yönlendirilir (*başarısız* olur). Yine de, bu otomatik olarak gerçekleştiğinden herhangi bir işlem yapmanız gerekmez.

Brezilya Güney bölgesinde, Azure anahtar kasalarınızın bir bölge hatası senaryosunda kurtarılmasını planlamalısınız. Azure anahtar kasanızı yedeklemek ve seçtiğiniz bir bölgeye geri yüklemek için [Azure Key Vault yedekleme](backup.md)'de ayrıntılı adımları uygulayın. 

Bu yüksek kullanılabilirlik tasarımı aracılığıyla Azure Key Vault bakım etkinlikleri için kapalı kalma süresi gerektirmez.

Bilmeniz için bazı uyarılar vardır:

* Bölge yük devretmesi durumunda, hizmetin yük devretmesinin tamamlanması birkaç dakika sürebilir. Yük devretmeden önce bu süre içinde yapılan istekler başarısız olabilir.
* Anahtar kasanıza bağlanmak için özel bağlantı kullanıyorsanız, bağlantı yük devri durumunda bağlantının yeniden kurulması 20 dakikaya kadar sürebilir. 
* Yük devretme sırasında, anahtar kasanızın salt okuma modunda olması gerekir. Bu modda desteklenen istekler şunlardır:
  * Sertifikaları listeleme
  * Sertifika Al
  * Gizli dizileri listeleme
  * Gizli dizileri al
  * Anahtarları Listele
  * Get (özellikleri) anahtarları
  * Şifreleme
  * Şifre Çözme
  * Ilacağını
  * Unwrap
  * Doğrulama
  * İşaret
  * Backup

* Yük devretme sırasında, Anahtar Kasası özelliklerinde değişiklik yapamazsınız. Erişim ilkesi veya güvenlik duvarı yapılandırma ve ayarlarını değiştiremeyeceksiniz.

* Yük devretme işlemi geri alındıktan sonra, tüm istek türleri (okuma *ve* yazma istekleri dahil) kullanılabilir.
