---
title: Bilişsel hizmetler için Customer-Managed anahtarları
titleSuffix: Cognitive Services
description: Azure portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırma hakkında bilgi edinin. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmanıza, döndürmenize, devre dışı bırakmanızı ve iptal edebilmesini sağlar.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 05/28/2020
ms.author: egeaney
ms.openlocfilehash: 1d161c82c087fd86a3774f0d121330260b1574e4
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94366103"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-for-cognitive-services"></a>Bilişsel hizmetler için Azure Key Vault müşteri tarafından yönetilen anahtarları yapılandırın

Bilişsel hizmetler için Azure Key Vault Customer-Managed anahtarları etkinleştirme işlemi ürüne göre farklılık gösterir. Hizmete özgü yönergeler için bu bağlantıları kullanın:

## <a name="vision"></a>Görsel

* [Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme](../Custom-Vision-Service/custom-vision-encryption-of-data-at-rest.md)
* [Yüz Hizmetleri, bekleyen verilerin şifrelenmesi](../Face/face-encryption-of-data-at-rest.md)
* [Bekleyen verilerin form tanıyıcı şifrelemesini şifreleme](../form-recognizer/form-recognizer-encryption-of-data-at-rest.md)

## <a name="language"></a>Dil

* [Bekleyen verilerin hizmet şifrelemesini Language Understanding](../LUIS/luis-encryption-of-data-at-rest.md)
* [Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma](../QnAMaker/qna-maker-encryption-of-data-at-rest.md)
* [Bekleyen verilerin Translator şifrelemesi](../translator/translator-encryption-of-data-at-rest.md)

## <a name="decision"></a>Karar

* [Bekleyen verilerin şifrelenmesi Content Moderator](../Content-Moderator/content-moderator-encryption-of-data-at-rest.md)
* [Bekleyen verilerin kişiselleştirici şifrelemesi](../personalizer/personalizer-encryption-of-data-at-rest.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault nedir](../../key-vault/general/overview.md)?
* [Bilişsel hizmetler Customer-Managed anahtar Isteği formu](https://aka.ms/cogsvc-cmk)