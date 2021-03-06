---
title: Azure Red Hat OpenShift kümesini yükseltme
description: OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesini yükseltmeyi öğrenin
ms.service: container-service
ms.topic: article
ms.date: 1/10/2021
author: sakthi-vetrivel
ms.author: suvetriv
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.openlocfilehash: 98ab8ff1e321929a9007c28f487d5861f6ac9930
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99064216"
---
# <a name="upgrade-an-azure-red-hat-openshift-aro-cluster"></a>Azure Red Hat OpenShift (ARO) kümesini yükseltme

ARO kümesi yaşam döngüsünün bir parçası, en son OpenShift sürümüne düzenli yükseltmeler gerçekleştirmeyi içerir. En son güvenlik sürümlerini uygulamanız veya en son özellikleri almak için yükseltmeniz önemlidir. Bu makalede, OpenShift web konsolunu kullanarak bir OpenShift kümesindeki tüm bileşenlerin nasıl yükseltileceğini gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLı sürüm 2.0.65 daha sonra çalıştırıyor olmanız gerekir. Geçerli sürümünüzü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli)

Bu makalede, mevcut bir Azure Red Hat OpenShift kümesine, ayrıcalıkları olan bir kullanıcı olarak erişiminiz olduğunu varsaymaktadır `admin` .

## <a name="check-for-available-aro-cluster-upgrades"></a>Kullanılabilir ARO kümesi yükseltmelerini denetle

OpenShift web konsolundan, **Yönetim**  >  **kümesi ayarları** ' nı seçin ve **Ayrıntılar** sekmesini açın.

Kümenizin **güncelleştirme durumu** **güncelleştirmeleri** yansıttığını belirlerseniz, kümenizi güncelleştirebilirsiniz.

## <a name="upgrade-your-aro-cluster"></a>ARO kümenizi yükseltme

Önceki adımda yer alan web konsolundan, öğesini güncelleştirmek istediğiniz sürüm için doğru **kanal olarak ayarlayın** , örneğin `stable-4.5` .

Güncelleştirilecek bir sürüm seçin ve **Güncelleştir**' i seçin. Güncelleştirme durumunun şu şekilde olduğunu göreceksiniz: `Update to <product-version> in progress` . Işleçler ve düğümler için ilerleme çubuklarını izleyerek küme güncelleştirmesinin ilerlemesini gözden geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [OC CLı kullanarak bir ARO kümesini yükseltmeyi öğrenin](https://docs.openshift.com/container-platform/4.6/updating/updating-cluster-between-minor.html)
- Kullanılabilir OpenShift kapsayıcı platformu Danışma belgeleri ve güncelleştirmeleriyle ilgili bilgileri müşteri portalının [erkıta bölümünde](https://access.redhat.com/downloads/content/290/ver=4.6/rhel---8/4.6.0/x86_64/product-errata) bulabilirsiniz.
  
