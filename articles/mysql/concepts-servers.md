---
title: Sunucu kavramları-MySQL için Azure veritabanı
description: Bu konuda, MySQL sunucuları için Azure veritabanı ile çalışmaya yönelik konular ve yönergeler sağlanmaktadır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 5477aa0c4eb2a66af816b287a8b6d7da8a53952f
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94534900"
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda sunucu kavramları

Bu makalede MySQL sunucuları için Azure veritabanı ile çalışmaya yönelik konular ve yönergeler sağlanmaktadır.

## <a name="what-is-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı nedir?

MySQL için Azure veritabanı sunucusu, birden çok veritabanı için merkezi bir yönetim noktasıdır. Bu, şirket içi dünyada bildiğiniz aynı MySQL sunucu yapısıdır. Özellikle, MySQL için Azure veritabanı hizmeti yönetilir, performans garantisi sağlar ve sunucu düzeyinde erişim ve özellikler sunar.

MySQL için Azure veritabanı sunucusu:

- Bir Azure aboneliği içinde oluşturulur.
- , Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Güçlü yaşam süresi semantiğinin bulunduğu bir kapsayıcıdır; bir sunucuyu silin ve kapsanan veritabanlarını siler.
- Bir bölgedeki kaynakları birlikte bulur.
- Sunucu ve veritabanı erişimi için bir bağlantı uç noktası sağlar.
- Veritabanlarına uygulanan yönetim ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, konfigürasyonlar vb.
- Birden çok sürümde kullanılabilir. Daha fazla bilgi için bkz. [MySQL Için Azure veritabanı veritabanı sürümleri](./concepts-supported-versions.md).

MySQL sunucusu için Azure Veritabanı içinde bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak veya kaynakları paylaşmak için birden çok veritabanı oluşturmak üzere sunucu başına tek bir veritabanı oluşturmayı tercih edebilirsiniz. Fiyatlandırma Katmanı, sanal çekirdek ve depolama (GB) yapılandırmasına bağlı olarak, fiyatlandırma sunucu başına yapılandırılır. Daha fazla bilgi için bkz. [fiyatlandırma katmanları](./concepts-pricing-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>Nasıl yaparım? bir MySQL sunucusu için Azure veritabanı 'na bağlanıp kimlik doğrulaması yapılsın mı?

Aşağıdaki öğeler veritabanınıza güvenli erişimin sağlanmasına yardımcı olur.

|     |     |
| :-- | :-- |
| **Kimlik doğrulama ve yetkilendirme** | MySQL için Azure veritabanı sunucusu yerel MySQL kimlik doğrulamasını destekler. Sunucu Yöneticisi oturum açma bilgileri ile sunucuya bağlanabilir ve kimlik doğrulaması yapabilirsiniz. |
| **Protokol** | Hizmet MySQL tarafından kullanılan ileti tabanlı bir protokolü destekler. |
| **TCP/IP** | Protokol TCP/IP üzerinden ve UNIX-etki alanı Yuvaları üzerinden desteklenir. |
| **Güvenlik duvarı** | Verilerinizin korunmasına yardımcı olmak için, hangi bilgisayarların iznine sahip olduğunu belirtene kadar bir güvenlik duvarı kuralı, veritabanı sunucunuza tüm erişimi engeller. Bkz. [MySQL Için Azure veritabanı sunucu güvenlik duvarı kuralları](./concepts-firewall-rules.md). |
| **SSL** | Hizmet, uygulamalarınız ve veritabanı sunucunuz arasında SSL bağlantısı uygulanmasını destekler.  Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md). |

## <a name="stopstart-an-azure-database-for-mysql-preview"></a>MySQL için Azure veritabanı 'nı Durdur/Başlat (Önizleme)

MySQL için Azure veritabanı, kullanımda olmadığında sunucuyu **durdurma** ve etkinlik sürdürüleceği zaman sunucuyu **başlatma** olanakları sağlar. Bu aslında, veritabanı sunucularında maliyetleri kaydetmek ve yalnızca kullanımda olduğunda kaynak için ödeme yapmak amacıyla yapılır. Bu, geliştirme ve test iş yükleri için ve yalnızca günün bir parçası için sunucuyu kullanırken daha da önemli olur. Sunucuyu durdurduğunuzda, tüm etkin bağlantılar bırakılır. Daha sonra, sunucuyu yeniden çevrimiçi duruma getirmek istediğinizde [Azure Portal](how-to-stop-start-server.md) veya [CLI](how-to-stop-start-server.md)kullanabilirsiniz.

Sunucu **durdurulmuş** durumdaysa, sunucunun işlem faturalandırılmaz. Ancak, sunucu yeniden başlatıldığında veri dosyalarının kullanılabilir olduğundan emin olmak için, depolama alanı faturalandırılmaya devam eder.

> [!IMPORTANT]
> Sunucuyu **durdurduğunuzda** bir esnette sonraki 7 gün için bu durumda kalır. Bu süre içinde el ile **başlatmadıysanız** , sunucu otomatik olarak 7 gün sonunda başlatılır. Sunucuyu kullanmıyorsanız yeniden **durdurmayı** seçebilirsiniz.

Sunucu durdurulduğunda, sunucuda hiçbir yönetim işlemi gerçekleştirilemez. Sunucudaki yapılandırma ayarlarını değiştirmek için [sunucusunu başlatmanız](how-to-stop-start-server.md)gerekir.

### <a name="limitations-of-stopstart-operation"></a>Durdur/Başlat işleminin sınırlamaları
- Okuma çoğaltması yapılandırmalarında (kaynak ve çoğaltmalar) desteklenmez.

## <a name="how-do-i-manage-a-server"></a>Sunucu Nasıl yaparım? mi?

Azure portal veya Azure CLı kullanarak MySQL için Azure veritabanı sunucularını yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmete genel bakış için bkz. [MySQL Için Azure veritabanı 'Na genel bakış](./overview.md)
- **Fiyatlandırma katmanınıza** dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [fiyatlandırma katmanları](./concepts-pricing-tiers.md)
- Hizmete bağlanma hakkında daha fazla bilgi için bkz. [MySQL Için Azure veritabanı bağlantı kitaplıkları](./concepts-connection-libraries.md).
