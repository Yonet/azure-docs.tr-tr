---
title: Azure Stack Edge Pro ile sertifika gereksinimleri ve sorun giderme | Microsoft Docs
description: Azure Stack Edge Pro cihazındaki sertifika gereksinimlerini ve sorun giderme sorunlarını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 11/17/2020
ms.author: alkohli
ms.openlocfilehash: de41bd030ea73ac68bfac5fbfbd03ae14cf7980f
ms.sourcegitcommit: 642988f1ac17cfd7a72ad38ce38ed7a5c2926b6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94874245"
---
# <a name="certificate-requirements"></a>Sertifika gereksinimleri

Bu makalede, sertifikaların Azure Stack Edge Pro cihazınıza yüklenebilmesi için karşılanması gereken sertifika gereksinimleri açıklanmaktadır. Bu gereksinimler PFX sertifikaları, verme yetkilisi, sertifika konu adı ve konu alternatif adı ve desteklenen sertifika algoritmalarıyla ilgilidir.

## <a name="certificate-issuing-authority"></a>Sertifika verme yetkilisi

Sertifika verme gereksinimleri aşağıdaki gibidir:

* Sertifikalar, bir iç sertifika yetkilisinden veya bir genel sertifika yetkilisinden verilmelidir.

* Otomatik olarak imzalanan sertifikaların kullanılması desteklenmez.

* Sertifikanın *verilen:* alanı, kök CA sertifikaları hariç, şunun *tarafından verilen: alanı ile* aynı olmamalıdır.


## <a name="certificate-algorithms"></a>Sertifika algoritmaları

Sertifika algoritmaları aşağıdaki gereksinimlere sahip olmalıdır:

* Sertifikaların RSA anahtar algoritmasını kullanması gerekir.

* Yalnızca Microsoft RSA/SChannel Şifreleme sağlayıcısı olan RSA sertifikaları desteklenir.

* Sertifika imza algoritması SHA1 olamaz.

* Minimum anahtar boyutu 4096 ' dir.

## <a name="certificate-subject-name-and-subject-alternative-name"></a>Sertifika konu adı ve konu diğer adı

Sertifikalar aşağıdaki konu adına ve konu alternatif adı gereksinimlerine sahip olmalıdır:

* Sertifikanın konu alternatif adı (SAN) alanlarındaki tüm ad alanlarını kapsayan tek bir sertifika kullanabilirsiniz. Alternatif olarak, her bir ad alanı için ayrı sertifikaları kullanabilirsiniz. Her iki yaklaşım da, ikili büyük nesne (blob) gibi gerekli olduğu durumlarda uç noktalar için joker karakterler kullanılmasını gerektirir.

* Konu adlarının (konu adındaki ortak ad) konu alternatif adı uzantısında konu diğer adlarının bir parçası olduğundan emin olun.

* Sertifikanın SAN alanlarında tüm ad alanlarını kapsayan tek bir joker karakter sertifikası kullanabilirsiniz.

* Bir uç nokta sertifikası oluştururken aşağıdaki tabloyu kullanın:

    |Tür |Konu adı (SN)  |Konu alternatif adı (SAN)  |Konu adı örneği |
    |---------|---------|---------|---------|
    |Azure Resource Manager|`management.<Device name>.<Dns Domain>`|`login.<Device name>.<Dns Domain>`<br>`management.<Device name>.<Dns Domain>`|`management.mydevice1.microsoftdatabox.com` |
    |Blob depolama|`*.blob.<Device name>.<Dns Domain>`|`*.blob.< Device name>.<Dns Domain>`|`*.blob.mydevice1.microsoftdatabox.com` |
    |Yerel Kullanıcı arabirimi| `<Device name>.<DnsDomain>`|`<Device name>.<DnsDomain>`| `mydevice1.microsoftdatabox.com` |
    |Her iki uç nokta için birden çok SAN tek sertifika|`<Device name>.<dnsdomain>`|`<Device name>.<dnsdomain>`<br>`login.<Device name>.<Dns Domain>`<br>`management.<Device name>.<Dns Domain>`<br>`*.blob.<Device name>.<Dns Domain>`|`mydevice1.microsoftdatabox.com` |
    |Node|`<NodeSerialNo>.<DnsDomain>`|`*.<DnsDomain>`<br><br>`<NodeSerialNo>.<DnsDomain>`|`mydevice1.microsoftdatabox.com` |
    |VPN|`AzureStackEdgeVPNCertificate.<DnsDomain>`<br><br> * AzureStackEdgeVPNCertificate sabit kodlanmış.  | `*.<DnsDomain>`<br><br>`<AzureStackVPN>.<DnsDomain>` | `edgevpncertificate.microsoftdatabox.com`|
    
## <a name="pfx-certificate"></a>PFX sertifikası

Azure Stack Edge Pro cihazınıza yüklü PFX sertifikaları aşağıdaki gereksinimleri karşılamalıdır:

* Sertifikalarınızı SSL yetkilisinden aldığınızda, sertifikaların tam imzalama zincirini aldığınızdan emin olur.

* Bir PFX sertifikasını dışarı aktardığınızda, **Mümkünse, zincirdeki tüm sertifikaları dahil et** seçeneğini seçtiğinizden emin olun.

* Uç nokta, yerel kullanıcı arabirimi, düğüm, VPN ve Wi-Fi için bir PFX sertifikası kullanarak hem genel hem de özel anahtarlar Azure Stack Edge Pro için gereklidir. Özel anahtarın yerel makine anahtarı özniteliği ayarlanmış olmalıdır.

* Sertifikanın PFX şifrelemesi 3DES olmalıdır. Bu, bir Windows 10 istemcisinden veya Windows Server 2016 sertifika deposundan dışarı aktarılırken kullanılan varsayılan şifrelemedir. 3DES ile ilgili daha fazla bilgi için bkz. [Üçlü des](https://en.wikipedia.org/wiki/Triple_DES).

* Sertifika PFX dosyaları, *anahtar kullanımı* alanında geçerli *dijital Imzaya* ve *KeyEncipherment* değerlerine sahip olmalıdır.

* Sertifika PFX dosyaları, *Gelişmiş anahtar kullanımı* alanında *sunucu kimlik doğrulaması (1.3.6.1.5.5.7.3.1)* ve *istemci kimlik doğrulaması (1.3.6.1.5.5.7.3.2)* değerlerini içermelidir.

* Azure Stack hazırlık Denetleyicisi aracını kullanıyorsanız, tüm sertifika PFX dosyalarının parolalarının dağıtım sırasında aynı olması gerekir. Daha fazla bilgi için bkz. [Azure Stack hub hazırlık Denetleyicisi aracı kullanarak Azure Stack Edge Pro için sertifika oluşturma](azure-stack-edge-j-series-create-certificates-tool.md).

* Sertifika PFX parolasının parolası karmaşık bir parola olmalıdır. Dağıtım parametresi olarak kullanıldığı için bu parolayı bir yere unutmayın.

* Yalnızca Microsoft RSA/SChannel Şifreleme sağlayıcısı ile RSA sertifikalarını kullanın.

Daha fazla bilgi için bkz. [özel ANAHTARLA PFX sertifikalarını dışarı aktarma](azure-stack-edge-j-series-manage-certificates.md#export-certificates-as-pfx-format-with-private-key).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge Pro ile sertifika kullanma](azure-stack-edge-j-series-manage-certificates.md)

[Azure Stack hub hazırlık Denetleyicisi aracını kullanarak Azure Stack Edge Pro için sertifikalar oluşturma](azure-stack-edge-j-series-create-certificates-tool.md)

[Özel anahtarla PFX sertifikalarını dışarı aktar](azure-stack-edge-j-series-manage-certificates.md#export-certificates-as-pfx-format-with-private-key)

[Sertifika hatalarını giderme](azure-stack-edge-j-series-certificate-troubleshooting.md)