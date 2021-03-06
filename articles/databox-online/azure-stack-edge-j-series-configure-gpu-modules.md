---
title: Microsoft Azure Stack Edge Pro GPU cihazında GPU modülünü çalıştırma | Microsoft Docs
description: Azure portal aracılığıyla bir Azure Stack Edge Pro cihazında GPU üzerinde bir modülün nasıl yapılandırılacağını ve çalıştırılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 01/04/2021
ms.author: alkohli
ms.openlocfilehash: d172ce98ba93360c621a91fb0e2a55d022470943
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97935569"
---
# <a name="configure-and-run-a-module-on-gpu-on-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazında GPU 'da modül yapılandırma ve çalıştırma

Azure Stack Edge Pro cihazınız bir veya daha fazla grafik Işleme birimi (GPU) içerir. GPU 'Lar, paralel işleme özellikleri sundukları ve merkezi Işleme birimlerinden (CPU) görüntü işlemeye daha hızlı bir şekilde, AI hesaplamaları için popüler bir seçimdir. Azure Stack Edge Pro cihazınızda bulunan GPU hakkında daha fazla bilgi için [Azure Stack Edge Pro cihaz teknik özellikleri](azure-stack-edge-gpu-technical-specifications-compliance.md)' ne gidin.

Bu makalede, Azure Stack Edge Pro cihazınızda GPU üzerinde bir modülün nasıl yapılandırılacağı ve çalıştırılacağı açıklanır. Bu makalede, NVIDIA T4 GPU 'ları için yazılmış genel olarak kullanılabilir kapsayıcı modülü **basamaklarını** kullanacaksınız. Bu yordam, bu GPU 'Lar için NVIDIA tarafından yayımlanan diğer modülleri yapılandırmak üzere kullanılabilir.


## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. GPU etkin 1 düğümlü Azure Stack Edge Pro cihazına erişirsiniz. Bu cihaz, Azure'da bir kaynakla birlikte etkinleştirilir.  

## <a name="configure-module-to-use-gpu"></a>Modülü GPU kullanacak şekilde yapılandırma

Bir modülü, bir modülü çalıştırmak için Azure Stack Edge Pro cihazınızda GPU 'YU kullanacak şekilde yapılandırmak için<!--Can it be simplified? "To configure a module to be run by the GPU on your Azure Stack Edge Pro device,"?--> Bu adımları izleyin.

1. Azure portal, aygıtınızla ilişkili kaynağa gidin.

2. **Genel bakış** bölümünde **IoT Edge**' yi seçin.

    ![Modülü GPU 1 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-1.png)

3. **IoT Edge hizmeti 'Ni etkinleştir** bölümünde **Ekle**' yi seçin.

   ![Modülü GPU 2 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-2.png)

4. **IoT Edge hizmeti oluştur**' da, IoT Hub kaynağınızın ayarlarını girin:

   |Alan   |Değer    |
   |--------|---------|
   |Abonelik      | Azure Stack Edge kaynağı tarafından kullanılan abonelik. |
   |Kaynak grubu    | Azure Stack Edge kaynağı tarafından kullanılan kaynak grubu. |
   |IoT Hub           | **Yeni oluştur** veya **var olanı kullan** seçeneklerinden birini belirleyin. <br> Varsayılan olarak IoT kaynağı oluşturulurken Standart katmanı (S1) kullanılır. Bir ücretsiz katman IoT kaynağı kullanmak için kaynağı oluşturun ve sonra da mevcut kaynağı seçin. <br> Her durumda IoT Hub kaynak, Azure Stack Edge kaynağı tarafından kullanılan aynı abonelik ve kaynak grubunu kullanır.     |
   |Ad              | Yeni bir IoT Hub kaynağı için belirtilen varsayılan adı kullanmak istemiyorsanız, farklı bir ad girin. |

   Ayarları tamamladığınızda, **gözden geçir + oluştur**' u seçin. IoT Hub kaynağınız için ayarları gözden geçirin ve **Oluştur**' u seçin.

   ![İşlem 2 ile çalışmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-3.png)

   Bir IoT Hub kaynağı için kaynak oluşturma birkaç dakika sürer. Kaynak oluşturulduktan sonra **genel bakış** IoT Edge hizmetin çalıştığını gösterir.

   ![İşlem 3 ' ü kullanmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-4.png)

5. Uç işlem rolünün yapılandırıldığını onaylamak için **Özellikler**' i seçin.

   ![İşlem 4 ' ü kullanmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-5.png)

6. **Özellikler**' de **IoT Edge cihaz** bağlantısını seçin.

   ![Modülü GPU 6 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-2.png)

   Sağ bölmede, Azure Stack Edge Pro cihazınıza ilişkin IoT Edge cihazı görürsünüz. Bu cihaz, IoT Hub kaynağını oluştururken oluşturduğunuz IoT Edge cihazına karşılık gelir.
 
7. Bu cihaz IoT Edge seçin.

   ![Modülü GPU 7 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-3.png)

8. **Modül ayarla**' yı seçin.

   ![Modülü GPU 8 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-4.png)

9. **+ Ekle** ' yi seçin ve **+ IoT Edge modülünü** seçin. 

    ![Modülü GPU 9 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-5.png)

10. **IoT Edge modülü Ekle** sekmesinde:

    1. **Görüntü URI**'sini sağlayın. Genel olarak kullanılabilen NVIDIA modül **rakamlarını** burada kullanacaksınız. 
    
    2. **Yeniden başlatma ilkesini** **her zaman** olarak ayarlayın.
    
    3. **İstenen durumu** **çalışıyor** olarak ayarlayın.
    
    ![Modülü GPU 10 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-6.png)

11. **Ortam değişkenleri** sekmesinde, değişkenin adını ve karşılık gelen değeri belirtin. 

    1. Geçerli modülün bu cihazda tek bir GPU kullanmasını sağlamak için NVIDIA_VISIBLE_DEVICES kullanın. 

    2. Değeri 0 veya 1 olarak ayarlayın. 0 veya 1 değeri, cihaz tarafından bu modül için en az bir GPU kullanılmasını sağlar. Bu değeri 0, 1 olarak ayarlarsanız, cihazınızdaki GPU 'Ların bu modül tarafından kullanılmakta olduğu anlamına gelir.

       ![Modülü GPU 11 ' i kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-7.png)

       NVIDIA GPU ile kullanabileceğiniz ortam değişkenleri hakkında daha fazla bilgi için, [NVIDIA kapsayıcı çalışma zamanına](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec)gidin.

    > [!NOTE]
    > GPU yalnızca bir modüle eşleştirilebilir. Modül, her ikisini de veya hiçbir GPU 'yu kullanabilir.

12. Modülünüzün adını girin. Bu noktada kapsayıcı oluşturma seçeneği sağlayıp modül ikizi ayarlarını değiştirebilir veya işiniz bittiğinde **Ekle**' yi seçin. 

    ![Modülü GPU 12 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-8.png)

13. Modülün çalıştığından emin olun ve **gözden geçir + oluştur**' u seçin.

    ![Modülü GPU 13 ' ü kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-9.png)

14. **Gözden geçir + oluştur** sekmelerinde, seçtiğiniz dağıtım seçenekleri görüntülenir. Seçenekleri gözden geçirin ve **Oluştur**' u seçin.
    
    ![Modülü GPU 14 ' ü kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-10.png)

15. Modülün **çalışma zamanı durumunu** bir yere getirin.
    
    ![Modülü GPU 15 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-11.png)

    Modülün dağıtılması birkaç dakika sürer. **Yenile** ' yi seçin ve **çalışma zamanı durumu** güncelleştirmesini **çalışıyor** olarak görmeniz gerekir.

    ![Modülü GPU 16 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-12.png)


## <a name="next-steps"></a>Sonraki adımlar

- [NVıDıA GPU ile kullanabileceğiniz ortam değişkenleri](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec)hakkında daha fazla bilgi edinin.
