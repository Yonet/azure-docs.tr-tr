---
title: 'Model: bir ilke tanımındaki alan özellikleri'
description: Bu Azure Ilke düzeninde, alan özelliklerinin bir ilke tanımında nasıl kullanılacağına ilişkin bir örnek verilmiştir.
ms.date: 10/14/2020
ms.topic: sample
ms.openlocfilehash: 267c687f78f0bbb100843faee40ab6f3d3cbb64c
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92072977"
---
# <a name="azure-policy-pattern-field-properties"></a>Azure Ilke deseninin: alan özellikleri

[Alan](../concepts/definition-structure.md#fields) işleci belirtilen bir [koşul](../concepts/definition-structure.md#conditions)için belirtilen özelliği veya [diğer adı](../concepts/definition-structure.md#aliases) bir sağlanan değere değerlendirir.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, kuruluşunuzun coğrafi konum gereksinimlerini karşılayan izin verilen bölgeleri tanımlamanızı sağlar. İzin verilen kaynaklar, **Lıfallodilimlocations** (_dizi_) parametresinde tanımlanmıştır. Tanımıyla eşleşen kaynaklar [reddedilir](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-fields.json":::

### <a name="explanation"></a>Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-fields.json" range="18-36" highlight="3,7,11":::

**Alan** işleci, **allof** [mantıksal işleci](../concepts/definition-structure.md#logical-operators) içinde üç kez kullanılır.

- İlk kullanım, `location` özelliği **notın** koşulu Ile **Lıfallodilimlocations** parametresine göre değerlendirir. **Notın** , bir _dizi_ beklediği ve parametresi bir _dizi_olduğu için çalışıyor. `location`Oluşturulan veya güncellenen kaynak onaylanan listede yoksa, bu öğe true olarak değerlendirilir.
- İkinci kullanım özelliği de değerlendirir `location` , ancak kaynağın _Global_olup olmadığını görmek için **Not al** koşulunu kullanır. `location`Oluşturulan veya güncellenen kaynağın _genel_olmaması durumunda bu öğe true olarak değerlendirilir.
- Son kullanım, özelliği değerlendirir `type` ve kaynak türünün _Microsoft. AzureActiveDirectory/b2cDirectories_olmadığını doğrulamak Için **notquals** koşulunu kullanır. Değilse, bu öğe true olarak değerlendirilir.

**Allof** Logical işlecinin tüm üç koşul deyimleri doğru olarak değerlendirilüyorsa, kaynak oluşturma veya güncelleştirme Azure ilkesi tarafından engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.