---
title: Azure Stack Edge Pro cihazında Kubernetes iş yükü yönetimini anlayın | Microsoft Docs
description: Kubernetes iş yüklerinin Azure Stack Edge Pro cihazınızda nasıl yönetilebileceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: alkohli
ms.openlocfilehash: ef840b3d9db4e82eeecea37079a08ccb0858a77b
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96448540"
---
# <a name="kubernetes-workload-management-on-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazınızda Kubernetes iş yükü yönetimi

Azure Stack Edge Pro cihazınızda, işlem rolünü yapılandırırken bir Kubernetes kümesi oluşturulur. Kubernetes kümesi oluşturulduktan sonra kapsayıcılı uygulamalar, pods 'deki Kubernetes kümesine dağıtılabilir. İş yüklerinizi Kubernetes kümesine dağıtmak için kullanabileceğiniz farklı yöntemler vardır. 

Bu makalede, Azure Stack Edge Pro cihazınızda iş yüklerini dağıtmak için kullanılabilecek çeşitli yöntemler açıklanmaktadır.

## <a name="workload-types"></a>İş yükü türleri

Azure Stack Edge Pro cihazınıza dağıtabileceğiniz iki ortak iş yükü türü durum bilgisiz uygulamalardır ve durum bilgisi olan uygulamalardır.

- **Durum bilgisiz uygulamalar** durumlarını korumaz ve kalıcı depolamaya veri kaydetmez. Tüm Kullanıcı ve oturum verileri istemcisiyle kalır. Durum bilgisi olmayan uygulamalara bazı örnekler NGINX gibi Web ön uçları ve diğer Web uygulamaları içerir.

    Bir Kubernetes dağıtımı oluşturarak, kümenizde durum bilgisiz bir uygulamayı dağıtabilirsiniz. 

- Durum **bilgisi olan uygulamalar** , durumunun kaydedilmesini gerektirir. Durum bilgisi olan uygulamalar, verileri sunucu veya diğer kullanıcılar tarafından kullanılmak üzere kaydetmek için kalıcı birimler gibi kalıcı depolama alanı kullanır. Durum bilgisi olan uygulamalara örnek olarak [Azure SQL Edge](../azure-sql-edge/overview.md) ve MongoDB gibi veritabanları verilebilir.

    Durum bilgisi olan bir uygulamayı dağıtmak için bir Kubernetes dağıtımı oluşturabilirsiniz. 

## <a name="deployment-flow"></a>Dağıtım akışı

Azure Stack Edge Pro cihazında uygulama dağıtmak için şu adımları izleyin: 
 
1. **Erişimi yapılandırma**: ilk olarak, bir kullanıcı oluşturmak, bir ad alanı oluşturmak ve bu ad alanına kullanıcı erişimi vermek için PowerShell çalışma 'i kullanacaksınız.
2. **Depolamayı yapılandırma**: bundan sonra, dağıtabileceğiniz durum bilgisi olan uygulamalar için statik veya dinamik sağlama kullanarak kalıcı birimler oluşturmak üzere Azure Portal Azure Stack Edge kaynağını kullanacaksınız.
3. **Ağı yapılandırma**: son olarak, uygulamaları dışarıdan ve Kubernetes kümesi içinde göstermek için Hizmetleri kullanacaksınız.
 
## <a name="deployment-types"></a>Dağıtım türleri

İş yüklerinizi dağıtmanın üç temel yolu vardır. Bu dağıtım yöntemlerinin her biri, cihazdaki ayrı bir ad alanına bağlanmanıza ve sonra durum bilgisiz veya durum bilgisi olmayan uygulamalar dağıtmanıza olanak tanır.

![Kubernetes iş yükü dağıtımı](./media/azure-stack-edge-gpu-kubernetes-workload-management/kubernetes-workload-management-1.png)

- **Yerel dağıtım**: Bu dağıtım, `kubectl` Kubernetes 'i dağıtmanıza olanak sağlayan gibi komut satırı erişim aracıdır `yamls` . Kubernetes kümesine Azure Stack Edge Pro 'Yu bir dosya aracılığıyla erişirsiniz `kubeconfig` . Daha fazla bilgi için, [kubectl aracılığıyla bir Kubernetes kümesine erişme](azure-stack-edge-gpu-create-kubernetes-cluster.md)konusuna gidin.

- **IoT Edge dağıtımı**: Bu, Azure IoT Hub 'e bağlanan IoT Edge. Ad alanı aracılığıyla Azure Stack Edge Pro cihazınızda Kubernetes kümesine bağlanırsınız `iotedge` . Bu ad alanına dağıtılmış olan IoT Edge aracıları Azure ile bağlantı sağlar. `IoT Edge deployment.json`Yapılandırmayı Azure DevOps CI/CD kullanarak uygularsınız. Ad alanı ve IoT Edge yönetimi, bulut operatörü aracılığıyla yapılır.

- **Azure Arc etkin Kubernetes dağıtımı**: Azure Arc etkin Kubernetes, Kubernetes kümelerinizde uygulamalar dağıtmanıza imkan tanıyan bir karma yönetim aracıdır. İle Azure Stack Edge Pro cihazınızdan Kubernetes kümesine bağlanırsınız `azure-arc namespace` . Bu ad alanında dağıtılan aracılar Azure bağlantısının sorumluluğundadır. Dağıtım yapılandırmasını, Gilar tabanlı yapılandırma yönetimini kullanarak uygularsınız. 
    
    Azure Arc etkin Kubernetes, kümenizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyicisini kullanmanıza da imkan tanır. Daha fazla bilgi için, [Azure Arc etkin Kubernetes nedir?](../azure-arc/kubernetes/overview.md)bölümüne bakın.

## <a name="choose-the-deployment-type"></a>Dağıtım türünü seçin

Uygulamaları dağıtma sırasında aşağıdaki bilgileri göz önünde bulundurun:

- **Tek veya birden çok tür**: tek bir dağıtım seçeneği veya farklı dağıtım seçenekleri karışımı seçebilirsiniz.
- **Bulutta yerel** olarak: uygulamalarınıza bağlı olarak, kubectl veya bulut dağıtımı aracılığıyla IoT Edge ve Azure Arc aracılığıyla yerel dağıtım seçeneğini belirleyebilirsiniz. 
    - Yerel bir dağıtım seçtiğinizde, Azure Stack Edge Pro cihazınızın dağıtıldığı ağla sınırlı olursunuz.
    - Dağıtabileceğiniz bir bulut aracınız varsa, bulut operatörüzü dağıtmanız ve bulut yönetimi kullanmanız gerekir.
- **IoT vs Azure Arc**: dağıtım seçimi, ürün senaryolarınızın amacına de bağlıdır. IoT veya IoT ekosistemiyle daha derin tümleştirme sağlayan uygulamalar veya kapsayıcılar dağıtıyorsanız, uygulamalarınızı dağıtmak için IoT Edge seçin. Mevcut Kubernetes dağıtımlarınız varsa, Azure Arc tercih edilen seçenek olacaktır.


## <a name="next-steps"></a>Sonraki adımlar

Bir uygulamayı kubectl aracılığıyla yerel olarak dağıtmak için bkz.:

- [Kubectl aracılığıyla Azure Stack Edge Pro 'unuzda durum bilgisiz bir uygulamayı dağıtın](azure-stack-edge-j-series-deploy-stateless-application-kubernetes.md).

IoT Edge aracılığıyla bir uygulama dağıtmak için, bkz.:

- [IoT Edge aracılığıyla Azure Stack Edge Pro 'unuzda örnek bir modül dağıtın](azure-stack-edge-gpu-deploy-sample-module.md).

Azure Arc aracılığıyla bir uygulama dağıtmak için, bkz.:

- [Azure Arc kullanarak bir uygulamayı dağıtın](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md).