---
title: Bekleyen veri için Azure Depolama şifrelemesi
description: Azure depolama, verilerinizi buluta kalıcı yapmadan önce otomatik olarak şifreleyerek korur. Depolama hesabınızdaki verilerin şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.date: 09/17/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: da1acc6316d7af87ffe35b9560919c324373591a
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96484609"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Bekleyen veri için Azure Depolama şifrelemesi

Azure depolama, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler. Azure depolama şifrelemesi, verilerinizi korur ve kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

## <a name="about-azure-storage-encryption"></a>Azure depolama şifrelemesi hakkında

Azure depolama 'daki veriler, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi, Windows 'da BitLocker şifrelemeye benzer.

Azure depolama şifrelemesi, hem Kaynak Yöneticisi hem de klasik depolama hesapları da dahil olmak üzere tüm depolama hesapları için etkinleştirilmiştir. Azure depolama şifrelemesi devre dışı bırakılamıyor. Verileriniz varsayılan olarak güvenli hale getirildiğinden, Azure depolama şifrelemesi 'nin avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

Bir depolama hesabındaki veriler, performans katmanından (Standart veya Premium), erişim katmanından (sık veya seyrek erişimli) veya dağıtım modelinden (Azure Resource Manager veya klasik) bağımsız olarak şifrelenir. Arşiv katmanındaki tüm Bloblar da şifrelenir. Tüm Azure depolama artıklığı seçenekleri şifrelemeyi destekler ve coğrafi çoğaltma etkinleştirildiğinde hem birincil hem de ikincil bölgelerdeki tüm veriler şifrelenir. Blob 'lar, diskler, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm Azure depolama kaynakları şifrelenir. Tüm nesne meta verileri de şifrelenir. Azure depolama şifrelemesi için ek bir maliyet yoktur.

20 Ekim 2017 ' den sonra Azure depolama 'ya yazılan her Blok Blobu, ekleme Blobu veya Sayfa Blobu şifrelenir. Bu tarihten önce oluşturulan Bloblar, bir arka plan işlemi tarafından şifrelenmeye devam eder. 20 Ekim 2017 ' den önce oluşturulan bir Blobun şifrelemeye zorlamak için, blobu yeniden yazabilirsiniz. Bir Blobun şifreleme durumunu denetleme hakkında bilgi edinmek için bkz. [bir Blobun şifreleme durumunu denetleme](../blobs/storage-blob-encryption-status.md).

Azure depolama şifrelemesini temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz. [şifreleme API 'si: yeni nesil](/windows/desktop/seccng/cng-portal).

Azure yönetilen diskler için şifreleme ve anahtar yönetimi hakkında daha fazla bilgi için bkz. Windows VM 'ler için [Azure yönetilen disklerin sunucu tarafı şifrelemesi](../../virtual-machines/disk-encryption.md) veya Linux VM 'Leri için [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](../../virtual-machines/disk-encryption.md) .

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Yeni bir depolama hesabındaki veriler varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifrelenir. Verilerinizin şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanmaya devam edebilir veya kendi anahtarlarınız ile şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz iki seçeneğiniz vardır. Her iki tür anahtar yönetimi veya her ikisini de kullanabilirsiniz:

- Blob depolamadaki ve Azure dosyalarındaki verileri şifrelemek ve şifrelerini çözmek için, *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. <sup>1, 2</sup> müşteri tarafından yönetilen anahtarların Azure Key Vault veya Azure Key Vault yönetilen donanım güvenlik MODELI (HSM) (Önizleme) içinde depolanması gerekir. Müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi için bkz. [Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları kullanma](./customer-managed-keys-overview.md).
- BLOB depolama işlemlerinde, *müşteri tarafından sağlanmış bir anahtar* belirtebilirsiniz. Blob depolamaya karşı okuma veya yazma isteği yapan bir istemci, blob verilerinin şifrelenme ve şifresinin çözülmesi üzerinde ayrıntılı denetim isteğine yönelik bir şifreleme anahtarı içerebilir. Müşteri tarafından sunulan anahtarlar hakkında daha fazla bilgi için bkz. [BLOB depolama için istekte bir şifreleme anahtarı sağlama](../blobs/encryption-customer-provided-keys.md).

Aşağıdaki tabloda, Azure depolama şifrelemesi için anahtar yönetim seçenekleri karşılaştırılmaktadır.

| Anahtar yönetimi parametresi | Microsoft tarafından yönetilen anahtarlar | Müşteri tarafından yönetilen anahtarlar | Müşteri tarafından sunulan anahtarlar |
|--|--|--|--|
| Şifreleme/şifre çözme işlemleri | Azure | Azure | Azure |
| Desteklenen Azure depolama hizmetleri | Tümü | BLOB depolama, Azure dosyaları<sup>1, 2</sup> | Blob depolama |
| Anahtar depolama | Microsoft anahtar deposu | Azure Key Vault veya Key Vault HSM | Müşterinin kendi anahtar deposu |
| Anahtar döndürme sorumluluğu | Microsoft | Müşteri | Müşteri |
| Anahtar denetimi | Microsoft | Müşteri | Müşteri |

<sup>1</sup> müşteri tarafından yönetilen anahtarları kuyruk depolama ile kullanmayı destekleyen bir hesap oluşturma hakkında bilgi için, bkz. [kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)oluşturma.<br />
<sup>2</sup> müşteri tarafından yönetilen anahtarları tablo depolama ile kullanmayı destekleyen bir hesap oluşturma hakkında bilgi için bkz. [tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json).

> [!NOTE]
> Microsoft tarafından yönetilen anahtarlar uyumluluk gereksinimleri uyarınca uygun şekilde döndürülür. Belirli anahtar döndürme gereksinimleriniz varsa, Microsoft, döndürmeyi kendiniz yönetebilmeniz ve denetedebilmeniz için müşteri tarafından yönetilen anahtarlara geçmeniz önerilir.

## <a name="doubly-encrypt-data-with-infrastructure-encryption"></a>Şüpheli verileri altyapı şifrelemesiyle şifreleme

Verilerin güvenliğini sağlamak için yüksek düzeyde güvence gerektiren müşteriler, Azure depolama altyapısı düzeyinde 256 bit AES şifrelemesini de etkinleştirebilir. Altyapı şifrelemesi etkinleştirildiğinde, bir depolama hesabındaki veriler, &mdash; hizmet düzeyinde iki kez ve altyapı düzeyinde &mdash; iki farklı şifreleme algoritmalarıyla bir kez şifrelenir. Azure depolama verilerinin çift şifrelemesi, şifreleme algoritmalarından veya anahtarlardan birinin tehlikeye girdiği bir senaryoya karşı koruma sağlar. Bu senaryoda, ek şifreleme katmanı verilerinizi korumaya devam eder.

Hizmet düzeyi şifreleme, Azure Key Vault ile Microsoft tarafından yönetilen anahtarların veya müşteri tarafından yönetilen anahtarların kullanımını destekler. Altyapı düzeyinde şifreleme, Microsoft tarafından yönetilen anahtarları kullanır ve her zaman ayrı bir anahtar kullanır.

Altyapı şifrelemesini sağlayan bir depolama hesabı oluşturma hakkında daha fazla bilgi için, bkz. [verilerin çift şifrelenmesi için altyapı şifrelemesi etkinleştirilmiş bir depolama hesabı oluşturma](infrastructure-encryption-enable.md).

## <a name="encryption-scopes-for-blob-storage-preview"></a>BLOB depolama için şifreleme kapsamları (Önizleme)

Varsayılan olarak, bir depolama hesabı depolama hesabı kapsamındaki bir anahtarla şifrelenir. Verilerinizi şifreleyen anahtara erişimi korumak ve denetlemek için, Microsoft tarafından yönetilen anahtarları veya Azure Key Vault depolanan müşteri tarafından yönetilen anahtarları kullanmayı seçebilirsiniz.

Şifreleme kapsamları, kapsayıcının veya ayrı bir blob düzeyinde şifrelemeyi isteğe bağlı olarak yönetmenizi sağlar. Aynı depolama hesabında bulunan ancak farklı müşterilere ait olan veriler arasında güvenli sınırlar oluşturmak için şifreleme kapsamlarını kullanabilirsiniz.

Azure depolama kaynak sağlayıcısı 'nı kullanarak bir depolama hesabı için bir veya daha fazla şifreleme kapsamı oluşturabilirsiniz. Bir şifreleme kapsamı oluşturduğunuzda, kapsamın Microsoft tarafından yönetilen bir anahtarla mi yoksa Azure Key Vault depolanan müşteri tarafından yönetilen bir anahtarla mi korunduğunu belirtin. Aynı depolama hesabındaki farklı şifreleme kapsamları, Microsoft tarafından yönetilen veya müşteri tarafından yönetilen anahtarları kullanabilir.

Bir şifreleme kapsamı oluşturduktan sonra, bir kapsayıcı veya blob oluşturmak için bu şifreleme kapsamının bir istekte belirtebilirsiniz. Şifreleme kapsamı oluşturma hakkında daha fazla bilgi için bkz. [şifreleme kapsamları oluşturma ve yönetme (Önizleme)](../blobs/encryption-scope-manage.md).

> [!NOTE]
> Önizleme sırasında Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) ve Okuma Erişimli Coğrafi bölge-yedekli depolama (RA-GZRS) hesapları ile şifreleme kapsamları desteklenmez.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

> [!IMPORTANT]
> Şifreleme kapsamları önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA 'Lar) Şu anda kullanılamıyor.
>
> Beklenmeyen maliyetleri önlemek için, şu anda ihtiyaç duymayan tüm şifreleme kapsamlarını devre dışı bıraktığınızdan emin olun.

### <a name="create-a-container-or-blob-with-an-encryption-scope"></a>Şifreleme kapsamı ile bir kapsayıcı veya blob oluşturma

Bir şifreleme kapsamı altında oluşturulan Bloblar, bu kapsam için belirtilen anahtarla şifrelenir. Blob oluştururken tek bir blob için şifreleme kapsamı belirtebilir veya bir kapsayıcı oluştururken varsayılan bir şifreleme kapsamı belirtebilirsiniz. Bir kapsayıcı düzeyinde varsayılan bir şifreleme kapsamı belirtildiğinde, bu kapsayıcıdaki tüm Bloblar varsayılan kapsamla ilişkili anahtarla şifrelenir.

Varsayılan şifreleme kapsamına sahip bir kapsayıcıda bir blob oluşturduğunuzda, kapsayıcı varsayılan şifreleme kapsamının geçersiz kılmalara izin verecek şekilde yapılandırıldıysa varsayılan şifreleme kapsamını geçersiz kılan bir şifreleme kapsamı belirtebilirsiniz. Varsayılan şifreleme kapsamının geçersiz kılmalarını engellemek için kapsayıcıyı tek bir blob için geçersiz kılmaları reddedecek şekilde yapılandırın.

Şifreleme kapsamı devre dışı bırakılmadığı sürece, bir şifreleme kapsamına ait olan bir Blobun okuma işlemleri saydam bir şekilde gerçekleşir.

### <a name="disable-an-encryption-scope"></a>Şifreleme kapsamını devre dışı bırakma

Bir şifreleme kapsamını devre dışı bıraktığınızda, şifreleme kapsamıyla gerçekleştirilen sonraki okuma veya yazma işlemleri, HTTP hata kodu 403 (yasak) ile başarısız olur. Şifreleme kapsamını yeniden etkinleştirirseniz okuma ve yazma işlemleri normal olarak devam eder.

Bir şifreleme kapsamı devre dışı bırakıldığında, artık bu için faturalandırılırsınız. Gereksiz ücretlerden kaçınmak için gerekli olmayan tüm şifreleme kapsamlarını devre dışı bırakın.

Şifreleme kapsamınız Azure Key Vault için müşteri tarafından yönetilen anahtarlarla korunuyorsa, şifreleme kapsamını devre dışı bırakmak için anahtar kasasında ilişkili anahtarı da silebilirsiniz. Azure Key Vault içindeki müşteri tarafından yönetilen anahtarların geçici silme ve Temizleme koruması ile korunduğunu ve silinen bir anahtarın bu özellikler tarafından tanımlanan davranışa tabi olduğunu aklınızda bulundurun. Daha fazla bilgi için Azure Key Vault belgelerinde aşağıdaki konulardan birine bakın:

- [PowerShell ile geçici silmeyi kullanma](../../key-vault/general/key-vault-recovery.md)
- [CLı ile geçici silmeyi kullanma](../../key-vault/general/key-vault-recovery.md)

> [!NOTE]
> Bir şifreleme kapsamının silinmesi mümkün değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault nedir?](../../key-vault/general/overview.md)
- [Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarlar](customer-managed-keys-overview.md)
- [BLOB depolama için şifreleme kapsamları (Önizleme)](../blobs/encryption-scope-overview.md)