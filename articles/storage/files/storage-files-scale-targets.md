---
title: Azure Dosyaları ölçeklenebilirlik ve performans hedefleri
description: Kapasite, istek hızı ve gelen ve giden bant genişliği sınırları dahil olmak üzere Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: e10f45af89e19f6fe62ff729f96d870e008c96ec
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611109"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure Dosyaları ölçeklenebilirlik ve performans hedefleri

[Azure dosyaları](storage-files-introduction.md) , bulutta ENDÜSTRI standardı SMB protokolü aracılığıyla erişilebilen tam olarak yönetilen dosya paylaşımları sunar. Bu makalede, Azure dosyaları ve Azure Dosya Eşitleme için ölçeklenebilirlik ve performans hedefleri ele alınmaktadır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri, yüksek kaliteli hedeflerdir, ancak dağıtımınızdaki diğer değişkenlerden etkilenebilir. Örneğin, bir dosya için üretilen iş, yalnızca Azure dosyaları hizmetini barındıran sunucular değil, kullanılabilir ağ bant genişliğiniz tarafından da sınırlanabilir. Azure dosyalarının ölçeklenebilirlik ve performansının gereksinimlerinizi karşılayıp karşılamadığını öğrenmek için kullanım modelinizi test etmenizi kesinlikle öneririz. Bu limitleri zaman içinde artırmayı de taahhüt ediyoruz. Lütfen aşağıdaki açıklamalarda veya [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)'ta, ne kadar yükseğine bakmak istediğinizi öğrenmek istediğiniz limitleri bize geri bildirimde bulunun.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçek hedefleri

Azure dosya paylaşımının üst kaynağı bir Azure Storage hesabıdır. Depolama hesabı, verileri depolamak için Azure dosyaları da dahil olmak üzere birden çok depolama hizmeti tarafından kullanılabilen Azure depolama havuzunu temsil eder. Depolama hesaplarında veri depolayan diğer hizmetler Azure Blob depolama, Azure kuyruk depolama ve Azure Tablo Depolama ' larıdır. Aşağıdaki hedefler, verileri depolama hesabında depolayan tüm depolama hizmetlerini uygular:

[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Diğer depolama hizmetlerinden genel amaçlı depolama hesabı kullanımı, Depolama hesabınızdaki Azure dosya paylaşımlarınızı etkiler. Örneğin, Azure Blob depolama ile en fazla depolama hesabı kapasitesine ulaştıysanız, Azure dosya paylaşımınız maksimum dosya boyutu altında olsa bile Azure dosya paylaşımınızda yeni dosyalar oluşturamazsınız.

## <a name="azure-files-scale-targets"></a>Azure Dosyalar ölçek hedefleri

Azure dosyaları için göz önünde bulundurmanız gereken üç sınırlama kategorisi vardır: depolama hesapları, paylaşımlar ve dosyalar.

Örneğin: Premium dosya paylaşımlarında, tek bir paylaşım 100.000 ıOPS elde edebilir ve tek bir dosya 5.000 ıOPS 'ye kadar ölçeklendirebilir. Bu nedenle, bir paylaşımda üç dosya varsa, bu paylaşımdan alabileceğiniz maksimum ıOPS 15.000 ' dir.

### <a name="standard-storage-account-limits"></a>Standart depolama hesabı limitleri

Bu sınırlar için [Azure depolama hesabı ölçek hedefleri](#azure-storage-account-scale-targets) bölümüne bakın.

### <a name="premium-filestorage-account-limits"></a>Premium FileStorage hesap limitleri

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Depolama hesabı sınırları tüm paylaşımlar için geçerlidir. FileStorage hesaplarının en büyük değerine kadar ölçeklendirilmesi yalnızca dosya depolama hesabı başına yalnızca bir paylaşımın olması durumunda ulaşılabilir.

### <a name="file-share-and-file-scale-targets"></a>Dosya paylaşma ve dosya ölçeği hedefleri

> [!NOTE]
> 5 TiB 'den büyük standart dosya paylaşımlarının belirli sınırlamaları vardır. Daha büyük dosya paylaşımı boyutlarını etkinleştirmeye yönelik sınırlamalar ve yönergelerin bir listesi için, planlama kılavuzunun [Standart dosya paylaşımları üzerinde daha büyük dosya paylaşımlarını etkinleştirme](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib) bölümüne bakın.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure Dosya Eşitleme ölçek hedefleri

Azure Dosya Eşitleme sınırsız kullanım hedefi ile tasarlanmıştır, ancak sınırsız kullanım her zaman mümkün değildir. Aşağıdaki tabloda Microsoft 'un test olan sınırları ve hangi hedeflerin sabit sınırları olduğunu gösterilmektedir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure Dosya Eşitleme performans ölçümleri

Azure Dosya Eşitleme Aracısı Azure dosya paylaşımlarına bağlanan bir Windows Server makinesinde çalıştığından, etkin eşitleme performansı altyapınızdaki bir dizi etkene bağlıdır: Windows Server ve temel disk yapılandırması, sunucu ile Azure depolama arasındaki ağ bant genişliği, dosya boyutu, toplam veri kümesi boyutu ve veri kümesindeki etkinlik. Azure Dosya Eşitleme dosya düzeyinde çalıştığından Azure Dosya Eşitleme tabanlı bir çözümün performans özellikleri, saniye başına işlenen nesne (dosya ve dizin) sayısında daha fazla ölçülür.

Azure Dosya Eşitleme için performans iki aşamada kritik öneme sahiptir:

1. **İlk bir kerelik sağlama**: ilk sağlama performansını iyileştirmek için en iyi dağıtım ayrıntıları için [Azure dosya eşitleme ile ekleme](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) bölümüne bakın.
2. **Devam eden eşitleme**: veriler başlangıçta Azure dosya paylaşımlarında oluşturulduktan sonra Azure dosya eşitleme birden çok uç noktayı eşitlenmiş halde tutar.

Bir aşamanın her biri için dağıtımınızı planlamaya yardımcı olmak üzere, bir sistem üzerinde yapılandırmaya sahip iç test sırasında gözlemlenen sonuçlar aşağıda verilmiştir

| Sistem yapılandırması | Ayrıntılar |
|-|-|
| CPU | 64 MiB L3 önbellek ile 64 sanal çekirdek |
| Bellek | 128 GiB |
| Disk | Pil gücüyle RAID 10 ile SAS diskleri |
| Ağ | 1 Gbps ağ |
| İş Yükü | Genel Amaçlı dosya sunucusu|

| İlk bir kerelik sağlama  | Ayrıntılar |
|-|-|
| Nesne sayısı | 25.000.000 nesneleri |
| Veri kümesi boyutu| ~ 4,7 TiB |
| Ortalama dosya boyutu | ~ 200 KiB (en büyük dosya: 100 GiB) |
| İlk bulut değişikliği numaralandırması | saniyede 20 nesne  |
| Aktarım hızını karşıya yükle | Her eşitleme grubu için saniyede 20 nesne |
| Ad alanı Indirme üretilen Işi | saniyede 400 nesne |

### <a name="initial-one-time-provisioning"></a>İlk bir kerelik sağlama

**İlk bulut değişikliği numaralandırması**: yeni bir eşitleme grubu oluşturulduğunda, ilk bulut değişikliği numaralandırması çalıştırılacak ilk adımdır. Bu süreçte, sistem Azure dosya paylaşımındaki tüm öğeleri listelemeye çalışır. Bu işlem sırasında, hiçbir eşitleme etkinliği olmaz. bir öğe, bulut uç noktasından sunucu uç noktasına indirilmeyecek ve sunucu uç noktasından bulut uç noktasına hiçbir öğe yüklenemeyecektir. İlk bulut değişikliği numaralandırması tamamlandıktan sonra eşitleme etkinliği sürdürülecek.
Performans hızı saniyede 20 nesne olur. Müşteriler, bulut paylaşımındaki öğelerin sayısını belirleyerek ve saati gün olarak almak için aşağıdaki formül ' i kullanarak ilk bulut değişikliği numaralandırması işleminin tamamlanması için gereken süreyi tahmin edebilir. 

   **İlk bulut numaralandırması için (gün olarak) süre = (bulut uç noktasındaki nesne sayısı)/(20 * 60 * 60 * 24)**

**Ad alanı indirme üretilen işi** Var olan bir eşitleme grubuna yeni bir sunucu uç noktası eklendiğinde Azure Dosya Eşitleme Aracısı, bulut uç noktasındaki dosya içeriğini indirmez. Önce tam ad alanını eşitler ve sonra, dosyaları tamamen veya bulut katmanlaması etkinse, sunucu uç noktasında ayarlanan bulut katmanlaması ilkesi için arka plan geri yüklemeyi tetikler.


| Devam eden eşitleme  | Ayrıntılar  |
|-|--|
| Eşitlenen nesne sayısı| 125.000 nesneleri (%1 karmaşıklık) |
| Veri kümesi boyutu| 50 GiB |
| Ortalama dosya boyutu | ~ 500 KiB |
| Aktarım hızını karşıya yükle | Her eşitleme grubu için saniyede 20 nesne |
| Tam Indirme performansı * | saniyede 60 nesne |

* Bulut katmanlaması etkinse, bazı dosya verilerinin yalnızca bir kısmı indirildiğinden daha iyi performans gözlemleyebilirsiniz. Azure Dosya Eşitleme yalnızca, uç noktaların hiçbirinde değiştirildiklerinde önbelleğe alınmış dosyaların verilerini indirir. Katmanlı veya yeni oluşturulan dosyalar için, aracı dosya verilerini indirmez ve bunun yerine yalnızca ad alanını tüm sunucu uç noktalarına eşitler. Aracı Ayrıca katmanlı dosyaların kullanıcı tarafından erişilen kısmi karşıdan yüklenmesini de destekler. 

> [!Note]  
> Yukarıdaki numaralar, deneyiminizle ilgili olarak bir performans göstergesi değildir. Gerçek performans, bu bölümün başlangıcında özetlenen şekilde, birden çok etkene bağlıdır.

Dağıtımınız için genel bir kılavuz olarak birkaç şeyi göz önünde bulundurmanız gerekir:

- Nesne aktarım hızı, sunucudaki eşitleme gruplarının sayısıyla orantılı olarak ölçeklendirilir. Sunucuda birden çok eşitleme grubuna veri bölmek, sunucu ve ağ tarafından da sınırlı olan daha iyi aktarım hızı verir.
- Nesne aktarım hızı, saniye başına işlem için MIB ile orantılı şekilde orantılıdır. Daha küçük dosyalar için, saniye başına işlenen nesne sayısı açısından daha yüksek aktarım hızı, saniye başına ise alt MIB ile karşılaşırsınız. Buna karşılık, daha büyük dosyalar için, saniye başına işlenen daha az sayıda nesne ve saniye başına daha fazla MIB alacaksınız. Saniyedeki MIB, Azure dosyaları ölçek hedefleri ile sınırlıdır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Dosyalar dağıtımını planlama](storage-files-planning.md)
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
