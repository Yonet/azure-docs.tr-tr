---
title: Azure blok zinciri çalışma ekranı 'nda uygulama kullanma
description: Azure blok zinciri çalışma ekranı önizlemesinde uygulama sözleşmelerini kullanma hakkında öğretici.
ms.date: 10/14/2019
ms.topic: tutorial
ms.reviewer: brendal
ms.openlocfilehash: 5761bf1294691c2d50e0e389fe69ec286df4a06c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "74324053"
---
# <a name="tutorial-using-applications-in-azure-blockchain-workbench"></a>Öğretici: Azure blok zinciri çalışma ekranı 'nda uygulama kullanma

Blokları oluşturmak ve sözleşme işlemleri yapmak için blok zinciri çalışma ekranı ' nı kullanabilirsiniz. Ayrıca, durum ve işlem geçmişi gibi sözleşme ayrıntılarını da görüntüleyebilirsiniz.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Yeni sözleşme oluştur
> * Bir sözleşmede eylem gerçekleştirin

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Blok zinciri çalışma ekranı dağıtımı. Daha fazla bilgi için bkz. dağıtım ayrıntıları için [Azure blok zinciri çalışma ekranı dağıtımı](deploy.md)
* Blok zinciri çalışma ekranı 'nda dağıtılan bir blok zinciri uygulaması. Bkz. [Azure blok zinciri üzerinde blok zinciri uygulaması oluşturma çalışma ekranı](create-app.md)

Tarayıcınızda [blok zinciri çalışma ekranı](deploy.md#blockchain-workbench-web-url) ' nı açın.

![Blockchain Workbench](./media/use/workbench.png)

Blok zinciri çalışma ekranının bir üyesi olarak oturum açmanız gerekir. Listelenen bir uygulama yoksa, blok zinciri çalışma ekranı 'nın bir üyesi olursunuz, ancak herhangi bir uygulamanın üyesi değildir. Blok zinciri çalışma ekranı Yöneticisi uygulamalara üye atayabilir.

## <a name="create-new-contract"></a>Yeni sözleşme oluştur

Yeni bir sözleşme oluşturmak için, sözleşme **başlatıcısı**olarak belirtilen bir üye olmanız gerekir. Sözleşmeye yönelik uygulama rollerini ve başlatıcılarını tanımlama hakkında bilgi için bkz. [yapılandırmaya genel bakış](configuration.md#workflows). Uygulama rollerine üye atama hakkında daha fazla bilgi için bkz. [uygulamaya üye ekleme](manage-users.md#add-member-to-application).

1. Blok zinciri çalışma ekranı uygulaması bölümünde, oluşturmak istediğiniz sözleşmeyi içeren uygulama kutucuğunu seçin. Etkin sözleşmelerin bir listesi görüntülenir.

2. Yeni sözleşme oluşturmak için **yeni sözleşme**' yi seçin.

    ![Yeni sözleşme düğmesi](./media/use/contract-list.png)

3. **Yeni sözleşme** bölmesi görüntülenir. İlk parametre değerlerini belirtin. **Oluştur**’u seçin.

    ![Yeni sözleşme bölmesi](./media/use/new-contract.png)

    Yeni oluşturulan sözleşme, diğer etkin sözleşmelerin bulunduğu listede görüntülenir.

    ![Etkin sözleşmeler listesi](./media/use/active-contracts.png)

## <a name="take-action-on-contract"></a>Sözleşme üzerinde işlem yap

Sözleşmenin bulunduğu duruma bağlı olarak, Üyeler sözleşmenin bir sonraki durumuna geçiş yapmak için işlemler gerçekleştirebilir. Eylemler bir [durum](configuration.md#states)içinde [geçiş](configuration.md#transitions) olarak tanımlanır. Geçişe izin verilen bir uygulamaya ait Üyeler veya örnek rolü bu eylemi gerçekleştirebilir. 

1. Blok zinciri çalışma ekranı uygulaması bölümünde, eylemi gerçekleştirmek için sözleşmeyi içeren uygulama kutucuğunu seçin.
2. Listeden sözleşmeyi seçin. Sözleşmeyle ilgili ayrıntılar farklı bölümlerde görüntülenir. 

    ![Sözleşme ayrıntıları](./media/use/contract-details.png)

    | Section  | Açıklama  |
    |---------|---------|
    | Durum | Sözleşme aşamaları dahilinde geçerli ilerlemeyi listeler |
    | Ayrıntılar | Sözleşmenin geçerli değerleri |
    | Eylem | Son eylemle ilgili ayrıntılar |
    | Etkinlik | Sözleşmenin işlem geçmişi |
    
3. **Eylem** bölümünde **eylem al**' ı seçin.

4. Sözleşmenin geçerli durumuyla ilgili ayrıntılar bir bölmede görüntülenir. Açılan kutuda almak istediğiniz eylemi seçin. 

    ![Eylem seçin](./media/use/choose-action.png)

5. Eylemi başlatmak için **eylem al** ' ı seçin.
6. Eylem için parametreler gerekliyse, eylem için değerleri belirtin.

    ![İşlem yapın](./media/use/take-action.png)

7. Eylemi yürütmek için **eylem al** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure blok zinciri uygulama sürümü oluşturma](version-app.md)
