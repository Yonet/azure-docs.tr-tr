---
title: Azure Analysis Services veritabanı rollerini ve kullanıcılarını yönetme | Microsoft Docs
description: Azure 'daki bir Analysis Services sunucusundaki veritabanı rollerini ve kullanıcıları yönetmeyi öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: owend
ms.reviewer: minewiskan
ms.custom: references_regions
ms.openlocfilehash: 31910e92ba4d5cbb1f133eaff6880fafb809b772
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99054102"
---
# <a name="manage-database-roles-and-users"></a>Veritabanı rollerini ve kullanıcılarını yönetme

Model veritabanı düzeyinde, tüm kullanıcılar bir role ait olmalıdır. Roller, model veritabanı için belirli izinlere sahip kullanıcıları tanımlar. Bir role eklenen herhangi bir kullanıcı veya güvenlik grubunun, sunucusuyla aynı abonelikte bir Azure AD kiracısında hesabı olması gerekir. 

Rolleri nasıl tanımlayacağınızı kullandığınız araca göre farklılık gösteren, ancak efekt aynı.

Rol izinleri şunları içerir:
*  **Yönetici** -kullanıcıların veritabanı için tam izinleri vardır. Yönetici izinlerine sahip veritabanı rolleri sunucu yöneticilerden farklıdır.
*  **İşlem** -kullanıcılar, veritabanında işlem işlemlerine bağlanabilir ve bu işlemleri gerçekleştirebilir ve model veritabanı verilerini analiz edebilir.
*  **Okuma** -kullanıcılar, model veritabanı verilerine bağlanmak ve analiz etmek için bir istemci uygulaması kullanabilir.

Tablosal model projesi oluştururken, Visual Studio 'da Analysis Services projelerle rol Yöneticisi kullanarak roller oluşturur ve bu rollere kullanıcı veya grup ekleyebilirsiniz. Bir sunucuya dağıtıldığında, rol ve Kullanıcı üyeleri eklemek veya kaldırmak için SQL Server Management Studio (SSMS), [Analysis Services PowerShell cmdlet 'leri](/analysis-services/powershell/analysis-services-powershell-reference)veya [tablo modeli komut dosyası dili](/analysis-services/tmsl/tabular-model-scripting-language-tmsl-reference) (tmsl) kullanın.

Bir **güvenlik grubu** eklerken kullanın `obj:groupid@tenantid` .

**Hizmet sorumlusu** kullanımı eklenirken `app:appid@tenantid` .

## <a name="to-add-or-manage-roles-and-users-in-visual-studio"></a>Visual Studio 'da rol ve Kullanıcı ekleme veya yönetme  
  
1.  **Tablosal Model Gezgini**' nde **Roller**' e sağ tıklayın.  
  
2.  **Rol Yöneticisi**'nde **Yeni**'ye tıklayın.  
  
3.  Rol için bir ad yazın.  
  
     Varsayılan olarak, varsayılan rolün adı her yeni rol için artımlı olarak numaralandırılır. Üye türünü açıkça tanımlayan bir ad yazmanız önerilir, örneğin finans yöneticileri veya Insan kaynakları uzmanları.  
  
4.  Aşağıdaki izinlerden birini seçin:  
  
    |İzin|Description|  
    |----------------|-----------------|  
    |**Hiçbiri**|Üyeler model şemasını okuyamıyor veya değiştiremezler ve verileri sorgulayamaz.|  
    |**Okuyamaz**|Üyeler veri sorgulayabilir (satır filtrelerine göre), ancak model şemasını değiştiremezler.|  
    |**Okuma ve Işleme**|Üyeler, verileri sorgulayabilir (satır düzeyi filtrelere göre) ve Işlemi çalıştırabilir ve tüm işlemleri Işleyebilir, ancak model şemasını değiştiremezler.|  
    |**İşleme**|Üyeler, Işlem çalıştırabilir ve tüm işlemleri Işleyebilir. Model şeması okunamıyor veya değiştirilemiyor ve veri sorgulanamıyor.|  
    |**Yönetici**|Üyeler model şemasını değiştirebilir ve tüm verileri sorgulayabilir.|   
  
5.  Oluşturmakta olduğunuz rolün okuma veya okuma ve Işleme izni varsa, bir DAX formülü kullanarak satır filtreleri ekleyebilirsiniz. **Satır filtreleri** sekmesine tıklayın, sonra bir tablo seçin, sonra **DAX filtresi** alanına tıklayın ve ardından bir DAX formülü yazın.
  
6.  **Üyeler**  >  **dış Ekle**' ye tıklayın.  
  
8.  **Dış üye Ekle**' de, kiracınızdaki kullanıcıları veya grupları e-posta adresine göre Azure AD 'ye girin. Tamam ' a tıkladıktan sonra Rol Yöneticisi ' ni kapattıktan sonra, roller ve rol üyeleri tablosal Model Gezgini 'nde görünür. 
 
     ![Tablosal model Gezgininde roller ve kullanıcılar](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Azure Analysis Services sunucunuza dağıtın.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>SSMS 'de rol ve Kullanıcı ekleme veya yönetme

Dağıtılan bir model veritabanına roller ve kullanıcılar eklemek için sunucu yöneticisi olarak sunucuya veya yönetici izinlerine sahip bir veritabanı rolünde zaten olmalıdır.

1. Nesne Exporer 'da **Roller**  >  **Yeni rol**' e sağ tıklayın.

2. **Rol oluştur**' da bir rol adı ve açıklama girin.

3. Bir izin seçin.

   |İzin|Description|  
   |----------------|-----------------|  
   |**Tam denetim (yönetici)**|Üyeler model şemasını değiştirebilir, işleyebilir ve tüm verileri sorgulayabilir.| 
   |**İşlem veritabanı**|Üyeler, Işlem çalıştırabilir ve tüm işlemleri Işleyebilir. Model şeması değiştirilemiyor ve veri sorgulanamıyor.|  
   |**Okuyamaz**|Üyeler veri sorgulayabilir (satır filtrelerine göre), ancak model şemasını değiştiremezler.|  
  
4. **Üyelik**' e tıklayın, ardından kiracınızda Azure AD 'ye e-posta adresini girerek bir kullanıcı veya grup girin.

     ![Kullanıcı ekleme](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Oluşturmakta olduğunuz rolün okuma izni varsa, bir DAX formülü kullanarak satır filtresi ekleyebilirsiniz. **Satır filtreleri**' ne tıklayın, bir tablo seçin ve **DAX FILTRESI** alanına bir DAX formülü yazın. 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>Bir TMSL betiği kullanarak rol ve Kullanıcı eklemek için

Bir TMSL betiğini SSMS içindeki XMLA penceresinde veya PowerShell kullanarak çalıştırabilirsiniz. [Createorreplace](/analysis-services/tmsl/createorreplace-command-tmsl) komutunu ve [Roller](/analysis-services/tmsl/roles-object-tmsl) nesnesini kullanın.

**Örnek TMSL betiği**

Bu örnekte, B2B dış kullanıcısı ve bir grubu, satış bı veritabanı için okuma izinlerine sahip analist rolüne eklenir. Hem dış kullanıcı hem de grup aynı kiracı Azure AD 'de olmalıdır.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="to-add-roles-and-users-by-using-powershell"></a>PowerShell kullanarak rol ve Kullanıcı eklemek için

[SqlServer](/analysis-services/powershell/analysis-services-powershell-reference) modülü, bir tablosal model betik DILI (tmsl) sorgusu veya betiği kabul eden, göreve özgü veritabanı yönetim cmdlet 'leri ve genel amaçlı Invoke-ASCmd cmdlet 'i sağlar. Aşağıdaki cmdlet 'ler veritabanı rollerini ve kullanıcılarını yönetmek için kullanılır.
  
|Cmdlet|Açıklama|
|------------|-----------------| 
|[Add-Rolemebir](/powershell/module/sqlserver/Add-RoleMember)|Bir veritabanı rolüne üye ekleyin.| 
|[Remove-Rolemeoda](/powershell/module/sqlserver/remove-rolemember)|Bir üyeyi veritabanı rolünden kaldırın.|   
|[Invoke-ASCmd](/powershell/module/sqlserver/invoke-ascmd)|Bir TMSL betiği yürütün.|

## <a name="row-filters"></a>Satır filtreleri  

Satır filtreleri, belirli bir rolün üyeleri tarafından bir tablodaki hangi satırların sorgulandığını tanımlar. Satır filtreleri, bir modeldeki her tablo için DAX formülleri kullanılarak tanımlanır.  
  
Satır filtreleri yalnızca okuma ve okuma ve Işleme izinleri olan roller için tanımlanabilir. Varsayılan olarak, belirli bir tablo için bir satır filtresi tanımlanmamışsa, başka bir tablodan çapraz filtreleme uygulanmadığı takdirde Üyeler tablodaki tüm satırları sorgulayabilir.
  
 Satır filtreleri, söz konusu rolün üyeleri tarafından sorgulanabilecek satırları tanımlamak için doğru/yanlış değerine değerlendirilmesi gereken bir DAX formülü gerektirir. DAX formülünde bulunmayan satırlar sorgulanamıyor. Örneğin, aşağıdaki satır filtreleri ifadesi olan Customers tablosu *= Customers [Country] = "USA"*, satış rolü ÜYELERI yalnızca ABD 'deki müşterileri görebilir.  
  
Satır filtreleri belirtilen satırlar ve ilgili satırlar için geçerlidir. Bir tabloda birden çok ilişki olduğunda filtreler, etkin olan ilişki için güvenlik sağlar. Satır filtreleri, ilişkili tablolar için tanımlanan diğer satır dosyası'leri ile kesişen, örneğin:  
  
|Tablo|DAX ifadesi|  
|-----------|--------------------|  
|Bölge|= Region [Ülke] = "USA"|  
|ProductCategory|= ProductCategory [ad] = "Bisiklet"|  
|İşlemler|= İşlem [Year] = 2016|  
  
 Net etkisi, Üyeler müşterinin ABD 'de olduğu, ürün kategorisinin bisiklet olduğu ve yılın 2016 olduğu veri satırlarını sorgulayabilir. Kullanıcılar, bu izinleri veren başka bir rolün üyesi olmadıkları takdirde ABD dışındaki işlemleri, Bisiklet olmayan işlemleri veya 2016 içinde olmayan işlemleri sorgulayamaz.
  
 Bir tablonun tamamına yönelik tüm satırlara erişimi reddetmek için, *= false ()* filtresini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

  [Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)   
  [Azure Analysis Services’ı PowerShell ile yönetme](analysis-services-powershell.md)  
  [Tablosal model betik dili (TMSL) başvurusu](/analysis-services/tmsl/tabular-model-scripting-language-tmsl-reference)
