---
title: Azure Kubernetes hizmetlerindeki küme yapılandırması (AKS)
description: Azure Kubernetes hizmeti 'nde (AKS) bir kümeyi yapılandırmayı öğrenin
services: container-service
ms.topic: article
ms.date: 01/13/2020
ms.author: jpalma
author: palma21
ms.openlocfilehash: eacca50e00dfe8625d86362c444544e2fd5d5511
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201119"
---
# <a name="configure-an-aks-cluster"></a>AKS kümesini yapılandırma

AKS kümesi oluşturmanın bir parçası olarak, Küme yapılandırmanızı gereksinimlerinize uyacak şekilde özelleştirmeniz gerekebilir. Bu makalede AKS kümenizi özelleştirmek için birkaç seçenek sunulmaktadır.

## <a name="os-configuration"></a>İşletim sistemi yapılandırması

AKS artık, Kubernetes sürümlerindeki 1.18.8 'den yüksek olan kümeler için genel kullanıma yönelik düğüm işletim sistemi (OS) olarak Ubuntu 18,04 ' i desteklemektedir. 1.18. x altındaki sürümler için AKS Ubuntu 16,04, hala varsayılan temel görüntüdür. Kubernetes v 1.18. x ve Onward 'den, varsayılan temel AKS Ubuntu 18,04 ' dir.

### <a name="use-aks-ubuntu-1804-generally-available-on-new-clusters"></a>Yeni kümelerde AKS Ubuntu 18,04 genel kullanıma sunuldu

Kubernetes v 1.18 üzerinde oluşturulan kümeler veya düğüm görüntüsünde daha fazla varsayılan `AKS Ubuntu 18.04` . Desteklenen bir Kubernetes sürümünün 1,18 ' den küçük olan düğüm havuzları yine de `AKS Ubuntu 16.04` düğüm görüntüsü olarak alınır, ancak `AKS Ubuntu 18.04` küme veya düğüm havuzunun Kubernetes sürümü v 1.18 veya üzeri olarak güncelleştirildikten sonra olarak güncelleştirilir.

1,18 veya üzeri kümeler kullanılmadan önce AKS Ubuntu 18,04 düğüm havuzlarındaki iş yüklerinizi test etmek önemle önerilir. [Ubuntu 18,04 düğüm havuzlarını test](#test-aks-ubuntu-1804-generally-available-on-existing-clusters)etme hakkında bilgi edinin.

Düğüm görüntüsünü kullanarak bir küme oluşturmak için `AKS Ubuntu 18.04` , aşağıda gösterildiği gibi yalnızca Kubernetes v 1.18 veya üstünü çalıştıran bir küme oluşturun

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

### <a name="use-aks-ubuntu-1804-generally-available-on-existing-clusters"></a>Mevcut kümelerde AKS Ubuntu 18,04 genel kullanıma sunuldu

Kubernetes v 1.18 üzerinde oluşturulan kümeler veya düğüm görüntüsünde daha fazla varsayılan `AKS Ubuntu 18.04` . Desteklenen bir Kubernetes sürümünün 1,18 ' den küçük olan düğüm havuzları yine de `AKS Ubuntu 16.04` düğüm görüntüsü olarak alınır, ancak `AKS Ubuntu 18.04` küme veya düğüm havuzunun Kubernetes sürümü v 1.18 veya üzeri olarak güncelleştirildikten sonra olarak güncelleştirilir.

1,18 veya üzeri kümeler kullanılmadan önce AKS Ubuntu 18,04 düğüm havuzlarındaki iş yüklerinizi test etmek önemle önerilir. [Ubuntu 18,04 düğüm havuzlarını test](#test-aks-ubuntu-1804-generally-available-on-existing-clusters)etme hakkında bilgi edinin.

Kümeleriniz veya düğüm havuzlarınız `AKS Ubuntu 18.04` düğüm görüntüsüne uygunsa, bunları aşağıda gösterildiği gibi bir v 1.18 veya daha yüksek sürüme de yükseltmeniz yeterlidir.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

Yalnızca bir düğüm havuzunu yükseltmek istiyorsanız:

```azurecli
az aks nodepool upgrade -name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

### <a name="test-aks-ubuntu-1804-generally-available-on-existing-clusters"></a>Test AKS Ubuntu 18,04, mevcut kümelerde genel kullanıma sunuldu

Düğüm, Kubernetes v 1.18 üzerinde oluşturulan düğüm havuzları veya düğüm görüntüsünde daha fazla varsayılan `AKS Ubuntu 18.04` . Desteklenen bir Kubernetes sürümünün 1,18 ' den küçük olan düğüm havuzları yine de `AKS Ubuntu 16.04` düğüm görüntüsü olarak alınır, ancak `AKS Ubuntu 18.04` düğüm havuzu Kubernetes sürümü v 1.18 veya üzeri olarak güncelleştirildikten sonra olarak güncelleştirilir.

Üretim düğüm havuzlarınızı yükseltmeden önce AKS Ubuntu 18,04 düğüm havuzlarındaki iş yüklerinizi test etmek önemle önerilir.

Düğüm görüntüsünü kullanarak bir düğüm havuzu oluşturmak için `AKS Ubuntu 18.04` , yalnızca Kubernetes v 1.18 veya üstünü çalıştıran bir düğüm havuzu oluşturun. Küme denetim düzlemi 'nin en az v 1.18 veya daha büyük olması gerekir, ancak diğer düğüm havuzlarınız daha eski bir Kubernetes sürümünde kalabilirler.
Aşağıda, ilk olarak denetim düzlemi 'ni yükseltiyoruz ve yeni düğüm görüntüsü işletim sistemi sürümünü alacak v 1.18 ile yeni bir düğüm havuzu oluşturuluyor.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14 --control-plane-only

az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

### <a name="use-aks-ubuntu-1804-on-new-clusters-preview"></a>Yeni kümeler üzerinde AKS Ubuntu 18,04 kullanma (Önizleme)

Aşağıdaki bölümde, henüz bir Kubernetes sürümü 1.18. x veya üzeri kullanmadığınız ya da bu özelliğin genel kullanıma sunulmadan önce, işletim sistemi yapılandırması önizlemesi kullanılarak oluşturulan kümeler üzerinde AKS Ubuntu 18,04 ' ı nasıl kullandığınız ve test ettiğiniz açıklanmaktadır.

Aşağıdaki kaynakların yüklü olması gerekir:

- [Azure CLI][azure-cli-install], sürüm 2.2.0 veya üzeri
- Aks-Preview 0.4.35 uzantısı

Aks-Preview 0.4.35 uzantısını veya üstünü yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

Özelliği kaydedin `UseCustomizedUbuntuPreview` :

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı** olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Kümeyi, küme oluşturulduğunda Ubuntu 18,04 kullanacak şekilde yapılandırın. `--aks-custom-headers`Ubuntu 18,04 ' i varsayılan işletim sistemi olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

AKS Ubuntu 16,04 görüntüsüyle kümeler oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

### <a name="use-aks-ubuntu-1804-existing-clusters-preview"></a>AKS Ubuntu 18,04 mevcut kümelerini kullanma (Önizleme)

Ubuntu 18,04 kullanmak için yeni bir düğüm havuzu yapılandırın. `--aks-custom-headers`Bu düğüm havuzu Için Ubuntu 18,04 ' ı varsayılan işletim sistemi olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

AKS Ubuntu 16,04 görüntüsüyle düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

## <a name="container-runtime-configuration"></a>Kapsayıcı çalışma zamanı yapılandırması

Kapsayıcı çalışma zamanı, kapsayıcıları yürüten ve bir düğümdeki kapsayıcı görüntülerini yöneten yazılımdır. Çalışma zamanı,, Linux veya Windows üzerinde kapsayıcıları çalıştırmak için, özel sys çağrıları veya işletim sistemi (OS) işlevine özgü işlevselliğin sağlanmasına yardımcı olur. Kubernetes sürüm 1,19 düğüm havuzlarını ve daha büyük kullanımını `containerd` kapsayıcı çalışma zamanı olarak kullanan AKS kümeleri. For node havuzları için v 1.19 öncesinde Kubernetes kullanan AKS kümeleri, kapsayıcı çalışma zamanı olarak [Moby](https://mobyproject.org/) (yukarı akış Docker) kullanır.

![Docker CRı 1](media/cluster-configuration/docker-cri.png)

[`Containerd`](https://containerd.io/) , kapsayıcıları yürütmek ve bir düğümdeki görüntüleri yönetmek için gereken minimum işlev kümesini sağlayan bir [OCI](https://opencontainers.org/) (açık kapsayıcı girişimi) ile uyumlu bir çekirdek kapsayıcı çalışma zamanı. , Mart 2017 ' de Cloud Native COMPUTE Foundation 'a (CNCF) [bağlandı](https://www.cncf.io/announcement/2017/03/29/containerd-joins-cloud-native-computing-foundation/) . AKS 'in kullandığı güncel Moby sürümü `containerd` yukarıda gösterildiği gibi, üzerine kurulmuştur.

`containerd`-Tabanlı bir düğüm ve düğüm havuzları ile konuşmak yerine, `dockershim` kubelet, `containerd` Docker cri uygulamasıyla karşılaştırıldığında akıştaki ek atlamaları kaldırarak doğrudan CRI (container Runtime Interface) eklentisi aracılığıyla konuşuyor. Bu nedenle, daha iyi Pod başlangıç gecikmesi ve daha az kaynak (CPU ve bellek) kullanımı görürsünüz.

`containerd`AKS düğümleri için kullanarak, Pod başlangıç gecikmesi artar ve kapsayıcı çalışma zamanına göre düğüm kaynak tüketimi azalır. Bu geliştirmeler, kubelet 'in, `containerd` Moby/Docker mimarisi kuı eklentisini kullanarak doğrudan CRI eklentisi aracılığıyla konuştukça, bu yeni mimari tarafından etkinleştirilir `dockershim` ve bu `containerd` sayede akışta ek atlamaları vardır.

![Docker CRı 2](media/cluster-configuration/containerd-cri.png)

`Containerd` AKS 'deki Kubernetes 'in her bir GA sürümünde ve v 1.19 üzerindeki her yukarı akış Kubernetes sürümünde çalışarak, tüm Kubernetes ve AKS özelliklerini destekler.

> [!IMPORTANT]
> Düğüm havuzlarının bulunduğu kümeler `containerd` , kapsayıcı çalışma zamanı Için Kubernetes v 1.19 veya daha büyük varsayılan üzerinde oluşturulmuş. Desteklenen bir Kubernetes sürümünde, kapsayıcı çalışma zamanı için 1,19 ' den küçük olan düğüm havuzları bulunan kümeler `Moby` , ancak `ContainerD` düğüm havuzunun Kubernetes sürümü v 1.19 veya üzeri olarak güncelleştirildikten sonra olarak güncelleştirilecektir. Hala `Moby` Desteklenen sürümler için düğüm havuzlarını ve kümeleri, bu destek bitene kadar kullanmaya devam edebilirsiniz.
> 
> AKS düğüm havuzlarındaki iş yüklerinizi, `containerD` 1,19 veya üzeri kümeler kullanılmadan önce kullanarak test etmek kesinlikle önerilir.

Aşağıdaki bölümde, `containerD` henüz bir Kubernetes sürüm 1,19 veya üstünü kullanmayan ya da bu özelliğin genel kullanıma sunulmasından önce kapsayıcı çalışma zamanı yapılandırma önizlemesi kullanılarak oluşturulmuş kümeler üzerinde Ile AKS 'i nasıl kullanabileceğinizi ve test ettebileceğiniz açıklanmaktadır.

### <a name="use-containerd-as-your-container-runtime-preview"></a>`containerd`Kapsayıcı çalışma zamanı olarak kullanın (Önizleme)

Aşağıdaki önkoşulların olması gerekir:

- [Azure CLI][azure-cli-install], sürüm 2.8.0 veya üzeri yüklü
- Aks-Preview uzantısı sürüm 0.4.53 veya üzeri
- `UseCustomizedContainerRuntime`Özellik bayrağı kaydedildi
- `UseCustomizedUbuntuPreview`Özellik bayrağı kaydedildi

Aks-Preview 0.4.53 uzantısını veya üstünü yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

`UseCustomizedContainerRuntime`Ve özelliklerini kaydedin `UseCustomizedUbuntuPreview` :

```azurecli
az feature register --name UseCustomizedContainerRuntime --namespace Microsoft.ContainerService
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService

```

Durumun **kayıtlı** olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedContainerRuntime')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```  

### <a name="use-containerd-on-new-clusters-preview"></a>`containerd`Yeni kümeler üzerinde kullan (Önizleme)

Kümeyi, küme oluşturulduğunda kullanılacak şekilde yapılandırın `containerd` . `--aks-custom-headers` `containerd` Kapsayıcı çalışma zamanı olarak ayarlamak için bayrağını kullanın.

> [!NOTE]
> `containerd`Çalışma zamanı yalnızca AKS Ubuntu 18,04 görüntüsünü kullanan düğümlerde ve düğüm havuzlarında desteklenir.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804,ContainerRuntime=containerd
```

Moby (Docker) çalışma zamanına sahip kümeler oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

### <a name="use-containerd-on-existing-clusters-preview"></a>`containerd`Mevcut kümeler üzerinde kullan (Önizleme)

Kullanılacak yeni bir düğüm havuzu yapılandırın `containerd` . `--aks-custom-headers` `containerd` Bu düğüm havuzu için çalışma zamanı olarak ayarlamak üzere bayrağını kullanın.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804,ContainerRuntime=containerd
```

Moby (Docker) çalışma zamanı ile düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .


### <a name="containerd-limitationsdifferences"></a>`Containerd` sınırlamalar/farklar

* `containerd`Kapsayıcı çalışma zamanı olarak kullanmak için, temel işletim sistemi görüntünüz olarak AKS Ubuntu 18,04 kullanmanız gerekir.
* Docker araç takımı düğümlerde hala mevcut olsa da, Kubernetes `containerd` kapsayıcı çalışma zamanı olarak kullanır. Bu nedenle, Moby/Docker düğümlerde Kubernetes tarafından oluşturulan kapsayıcıları yönetmediğinden Docker komutlarını (gibi `docker ps` ) veya Docker API 'sini kullanarak Kapsayıcılarınızı görüntüleyemez veya bunlarla etkileşime giremezseniz.
* İçin `containerd` , [`crictl`](https://kubernetes.io/docs/tasks/debug-application-cluster/crictl) Kubernetes düğümlerinde (örneğin,) pods, kapsayıcılar ve kapsayıcı görüntüleri **sorunlarını gidermeye** yönelik Docker CLI yerine değiştirme CLI olarak kullanmanızı öneririz `crictl ps` . 
   * Docker CLı 'nin bütün işlevlerini sağlamaz. Yalnızca sorun gidermeye yöneliktir.
   * `crictl` , pods gibi kavramların yanı sıra mevcut olan kapsayıcıların daha fazla Kubernetes görünümünü sunar.
* `Containerd` standartlaştırılmış günlük biçimini kullanarak günlük kaydı yapar `cri` (Şu anda Docker 'ın JSON sürücüsünden aldığınız verilerden farklıdır). Günlük çözümünüzün `cri` günlük biçimini desteklemesi gerekir ( [kapsayıcılar Için Azure izleyici](../azure-monitor/insights/container-insights-enable-new-cluster.md)gibi)
* Artık Docker altyapısına erişemez veya Docker- `/var/run/docker.sock` ın-Docker (Dintıd) kullanabilirsiniz.
  * Şu anda Docker altyapısından uygulama günlükleri veya izleme verileri ayıklandıysanız, lütfen bunun yerine [kapsayıcılar Için Azure izleyici](../azure-monitor/insights/container-insights-enable-new-cluster.md) gibi bir şey kullanın. Ayrıca AKS, kararsızlığa neden olabilecek aracı düğümlerinde bant dışı komutların çalıştırılmasını desteklemez.
  * Moby/Docker kullanırken, görüntülerin oluşturulması ve yukarıdaki yöntemler aracılığıyla Docker altyapısının doğrudan kullanılmasıyla kesinlikle önerilmez. Kubernetes, bu tüketilen kaynakların tamamen farkında değildir ve bu yaklaşımlar [burada](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) [ve burada ayrıntılı olarak ayrıntılı](https://securityboulevard.com/2018/05/escaping-the-whale-things-you-probably-shouldnt-do-with-docker-part-1/)bir şekilde ortaya bulunur.
* Görüntü oluşturma-AKS kümenizde görüntü oluşturulmadığınız sürece geçerli Docker Build iş akışınızı normal olarak kullanmaya devam edebilirsiniz. Bu durumda, [ACR görevlerini](../container-registry/container-registry-quickstart-task-cli.md)kullanarak görüntü oluşturmak için önerilen yaklaşımın veya [Docker buildx](https://github.com/docker/buildx)gibi daha güvenli bir küme içi seçeneğinde geçiş yapmayı düşünün.

## <a name="generation-2-virtual-machines-preview"></a>2. nesil sanal makineler (Önizleme)

Azure [2. nesil (Gen2) sanal makineleri (VM)](../virtual-machines/generation-2.md)destekler. 2. nesil VM 'Ler, 1. nesil VM 'lerde desteklenmeyen önemli özellikleri destekler (Gen1). Bu özellikler, artan bellek, Intel Software Guard uzantıları (Intel SGX) ve sanallaştırılmış kalıcı bellek (vPMEM) içerir.

2. nesil sanal makineler, 1. nesil VM 'Ler tarafından kullanılan BIOS tabanlı mimaride değil, yeni UEFı tabanlı önyükleme mimarisini kullanır.
Yalnızca belirli SKU 'Lar ve boyutlar Gen2 VM 'Leri destekler. SKU 'nuzun Gen2 destekleyip desteklemediğini veya gerektirip gerektirmediğini görmek için [Desteklenen boyutlar listesini](../virtual-machines/generation-2.md#generation-2-vm-sizes)kontrol edin.

Ayrıca, AKS Gen2 VM 'lerinde Gen2 desteği olan tüm VM görüntüleri, yeni [aks Ubuntu 18,04 görüntüsünü](#os-configuration)kullanır. Bu görüntü tüm Gen2 SKU 'Larını ve boyutlarını destekler.

Önizleme sırasında Gen2 VM 'Leri kullanmak için şunları yapmanız gerekir:
- `aks-preview`CLI uzantısı yüklendi.
- `Gen2VMPreview`Özellik bayrağı kaydedildi.

Özelliği kaydedin `Gen2VMPreview` :

```azurecli
az feature register --name Gen2VMPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı** olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/Gen2VMPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Aks-Preview CLı uzantısını yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
```

Aks-Preview CLı uzantısını güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension update --name aks-preview
```

### <a name="use-gen2-vms-on-new-clusters-preview"></a>Yeni kümeler üzerinde Gen2 VM 'Leri kullanma (Önizleme)
Kümeyi, küme oluşturulduğunda seçili SKU için Gen2 VM 'Leri kullanacak şekilde yapılandırın. `--aks-custom-headers`Yeni bir kümede VM oluşturma olarak Gen2 ayarlamak için bayrağını kullanın.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

1. nesil (Gen1) VM 'Leri kullanarak düzenli bir küme oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` . Ayrıca, aşağıdaki gibi daha fazla Gen1 veya Gen2 VM eklemeyi de seçebilirsiniz.

### <a name="use-gen2-vms-on-existing-clusters-preview"></a>Mevcut kümeler üzerinde Gen2 VM 'Leri kullanma (Önizleme)
Yeni bir düğüm havuzunu Gen2 VM 'Leri kullanacak şekilde yapılandırın. `--aks-custom-headers`Gen2 'i bu düğüm havuzu IÇIN VM oluşturma olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks nodepool add --name gen2 --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

Normal Gen1 düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .


## <a name="ephemeral-os"></a>Kısa ömürlü işletim sistemi

Varsayılan olarak, Azure bir sanal makinenin işletim sistemi diskini otomatik olarak Azure depolama 'ya çoğaltarak veri kaybını önlemek için VM 'nin başka bir konağa yeniden konumlandırılması gerekir. Ancak, kapsayıcılar yerel durumunun kalıcı olmasını sağlayacak şekilde tasarlanmadığından, bu davranış, daha yavaş düğüm sağlama ve daha yüksek okuma/yazma gecikme süresi dahil olmak üzere bazı dezavantajları sağlarken sınırlı bir değer sunar.

Bunun aksine, kısa ömürlü işletim sistemi diskleri yalnızca, geçici bir disk gibi yalnızca ana makine üzerinde depolanır. Bu, daha hızlı okuma/yazma gecikmesi sağlar ve daha hızlı düğüm ölçekleme ve küme yükseltmeleriyle birlikte.

Geçici disk gibi, daha kısa bir işletim sistemi diski sanal makinenin fiyatına dahildir, bu nedenle ek depolama ücreti ödemeniz gerekmez.

> [!IMPORTANT]
>Bir kullanıcı işletim sistemi için açıkça yönetilen diskler istemediğinde, belirli bir nodepool yapılandırması için mümkünse AKS varsayılan olarak geçici işletim sistemi olarak çalışır.

Kısa ömürlü IŞLETIM sistemi kullanılırken, işletim sistemi diski VM önbelleğine sığacak olmalıdır. VM önbelleğinin boyutları, GÇ üretilen iş ("GiB 'de önbellek boyutu") yanındaki parantez içinde [Azure belgelerinde](../virtual-machines/dv3-dsv3-series.md) kullanılabilir.

Örnek olarak, varsayılan işletim sistemi disk boyutu olan 100 GB 'lık varsayılan VM boyutunu Standard_DS2_v2 kullanmak, bu VM boyutu, kısa ömürlü işletim sistemi destekler ancak yalnızca 86GB önbellek boyutuna sahiptir. Kullanıcı açıkça belirtilmemişse, bu yapılandırma varsayılan olarak yönetilen diskler olarak belirtilebilir. Bir kullanıcı açık olarak geçici bir işletim sistemi isteğinde bulunursa, doğrulama hatası alırlar.

Bir Kullanıcı, 60GB işletim sistemi diski ile aynı Standard_DS2_v2 isterse, bu yapılandırma varsayılan olarak kısa ömürlü işletim sistemi: 60 GB istenen boyutu, 86GB 'ın en yüksek önbellek boyutundan daha küçüktür.

100 GB işletim sistemi diski olan Standard_D8s_v3 kullanarak, bu VM boyutu, kısa ömürlü işletim sistemi destekler ve 200 GB önbellek alanı içerir. Bir kullanıcı işletim sistemi disk türünü belirtmezse, nodepool varsayılan olarak kısa ömürlü işletim sistemi alır. 

Kısa ömürlü işletim sistemi, Azure CLı 'nin en az sürüm 2.15.0 gerektirir.

### <a name="use-ephemeral-os-on-new-clusters"></a>Yeni kümelerde kısa ömürlü işletim sistemi kullan

Kümeyi, küme oluşturulduğunda kısa ömürlü işletim sistemi disklerini kullanacak şekilde yapılandırın. `--node-osdisk-type`Yeni küme için işletim sistemi disk türü olarak kısa ömürlü işletim sistemi ayarlamak için bayrağını kullanın.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS3_v2 --node-osdisk-type Ephemeral
```

Ağa bağlı işletim sistemi disklerini kullanarak düzenli bir küme oluşturmak isterseniz, bunu belirterek yapabilirsiniz `--node-osdisk-type=Managed` . Ayrıca, aşağıdaki şekilde daha kısa ömürlü işletim sistemi düğüm havuzları eklemeyi de seçebilirsiniz.

### <a name="use-ephemeral-os-on-existing-clusters"></a>Mevcut kümelerde kısa ömürlü işletim sistemi kullan
Kısa ömürlü işletim sistemi disklerini kullanmak için yeni bir düğüm havuzu yapılandırın. `--node-osdisk-type`Bu düğüm havuzu için işletim sistemi disk türü olarak işletim sistemi disk türü olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks nodepool add --name ephemeral --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS3_v2 --node-osdisk-type Ephemeral
```

> [!IMPORTANT]
> Kısa ömürlü IŞLETIM sistemiyle VM ve örnek görüntülerini VM önbelleğinin boyutuna dağıtabilirsiniz. AKS durumunda, varsayılan işletim sistemi disk yapılandırması 128GB kullanır, bu da 128 GB 'den büyük bir VM boyutuna ihtiyacınız olduğu anlamına gelir. Varsayılan Standard_DS2_v2, 86GB önbellek boyutuna sahiptir ve bu değer yeterince büyük değildir. Standard_DS3_v2, 17 GB boyutunda bir önbellek boyutuna sahiptir ve bu değer yeterince büyük olur. Ayrıca, kullanarak işletim sistemi diskinin varsayılan boyutunu azaltabilirsiniz `--node-osdisk-size` . AKS görüntülerinin en küçük boyutu 30 ' dır. 

Ağ bağlantılı işletim sistemi diskleri ile düğüm havuzları oluşturmak istiyorsanız, belirterek bunu yapabilirsiniz `--node-osdisk-type Managed` .

## <a name="custom-resource-group-name"></a>Özel kaynak grubu adı

Azure 'da bir Azure Kubernetes hizmet kümesi dağıttığınızda, çalışan düğümleri için ikinci bir kaynak grubu oluşturulur. Varsayılan olarak AKS, düğüm kaynak grubunu adı verir `MC_resourcegroupname_clustername_location` , ancak kendi adınızı de sağlayabilirsiniz.

Kendi kaynak grubu adınızı belirtmek için, aks-Preview Azure CLı uzantısı sürüm 0.3.2 veya üstünü yüklemelisiniz. Azure CLı 'yı kullanarak `--node-resource-group` `az aks create` kaynak grubu için özel bir ad belirtmek üzere komutun parametresini kullanın. AKS kümesi dağıtmak için bir Azure Resource Manager şablonu kullanıyorsanız, özelliğini kullanarak kaynak grubu adını tanımlayabilirsiniz `nodeResourceGroup` .

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

İkincil kaynak grubu, kendi aboneliğinizde Azure Kaynak sağlayıcısı tarafından otomatik olarak oluşturulur. Yalnızca küme oluşturulduğunda özel kaynak grubu adı belirtebilirsiniz. 

Düğüm kaynak grubuyla çalışırken şunları yapamazsınız:

- Düğüm kaynak grubu için mevcut bir kaynak grubu belirtin.
- Düğüm kaynak grubu için farklı bir abonelik belirtin.
- Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
- Düğüm kaynak grubu içindeki yönetilen kaynakların adlarını belirtin.
- Düğüm kaynak grubu içinde yönetilen kaynakların Azure tarafından oluşturulan etiketlerini değiştirin veya silin.

## <a name="next-steps"></a>Sonraki adımlar

- Kümenizdeki [düğüm görüntülerini nasıl yükselteceğinizi](node-image-upgrade.md) öğrenin.
- Kümenizi, Kubernetes 'in en son sürümüne nasıl yükselteceğinizi öğrenmek için bkz. [Azure Kubernetes hizmeti (AKS) kümesini yükseltme](upgrade-cluster.md) .
- [ `containerd` Ve Kubernetes](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/) hakkında daha fazla bilgi edinin
- Bazı yaygın AKS sorularına cevap bulmak için [aks hakkında sık sorulan soruların](faq.md) listesine bakın.
- [Kısa ömürlü işletim sistemi diskleri](../virtual-machines/ephemeral-os-disks.md)hakkında daha fazla bilgi edinin.


<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
