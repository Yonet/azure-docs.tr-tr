---
title: Avere vFXT-Azure desteğini etkinleştir
description: Azure için avere vFXT 'den, Müşteri Hizmetleri sağlamaya yardımcı olmak üzere, kümenizin destek verilerini otomatik olarak yüklemeyi nasıl etkinleştireceğinizi öğrenin.
author: ekpgh
ms.service: avere-vfxt
ms.topic: how-to
ms.date: 12/14/2019
ms.author: rohogue
ms.openlocfilehash: 93b99aa624a21d9312297e4279b1dcf053c79ae3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88272737"
---
# <a name="enable-support-uploads"></a>Destek karşıya yüklemelerini etkinleştirme

Azure için avere vFXT, kümeniz hakkında Destek verilerini otomatik olarak karşıya yükleyebilir. Bu karşıya yüklemeler, destek personelinin olası en iyi müşteri hizmetini sağlamasına imkan tanır.

## <a name="steps-to-enable-uploads"></a>Karşıya yüklemeleri etkinleştirme adımları

Desteği etkinleştirmek için avere denetim masasından bu adımları izleyin. (Denetim Masası 'nı açmayı öğrenmek için [vFXT kümesine erişin](avere-vfxt-cluster-gui.md) .)

1. Üstteki **Ayarlar** sekmesine gidin.
1. Sol taraftaki **destek** bağlantısına tıklayın ve gizlilik ilkesini kabul edin.

   ![Gizlilik ilkesini kabul etmek için avere Denetim Masası 'Nı ve onay düğmesini içeren açılır pencereyi gösteren ekran görüntüsü](media/avere-vfxt-privacy-policy.png)

1. Destek yapılandırması sayfasında, soldaki üçgene tıklayarak **Müşteri bilgileri** bölümünü açın.
1. **Karşıya yükleme bilgilerini yeniden doğrula** düğmesine tıklayın.
1. Kümenin destek adını **benzersiz küme adı**olarak ayarlayın. Bu adın, personeli desteklemek için kümenizi benzersiz bir şekilde tanımladığından emin olun.
1. **Istatistik izleme**, **genel bilgiler karşıya yükleme**ve **kilitlenme bilgileri karşıya yükleme**kutularını işaretleyin.
1. **Gönder**' e tıklayın.

   ![Destek ayarları sayfasının tamamlanan müşteri bilgisi bölümünün bulunduğu ekran görüntüsü](media/avere-vfxt-support-info.png)

1. Bölümü genişletmek için **güvenli proaktif destek (SPS)** sol tarafındaki üçgeni tıklatın.
1. **SPS 'Yi etkinleştir bağlantısının**kutusunu işaretleyin.
1. **Gönder**' e tıklayın.

   ![Destek ayarları sayfasında tamamlanmış güvenli proaktif destek bölümünün bulunduğu ekran görüntüsü](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Sonraki adımlar

Kümeye şirket içi veya mevcut bir bulut depolama sistemi eklemeniz gerekiyorsa, [depolamayı yapılandırma](avere-vfxt-add-storage.md)bölümündeki yönergeleri izleyin.

Kümeye istemci eklemeye başlamaya hazırsanız, [avere vFXT kümesini bağlama](avere-vfxt-mount-clients.md)konusunu okuyun.
