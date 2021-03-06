---
title: Arc özellikli Kubernetes kümesinde (Önizleme) GitOps kullanarak yapılandırmaları dağıtma
services: azure-arc
ms.service: azure-arc
ms.date: 02/09/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure yay etkin bir Kubernetes kümesi (Önizleme) yapılandırmak için Gilar 'ı kullanma
keywords: Gilar, Kubernetes, K8s, Azure, Arc, Azure Kubernetes hizmeti, AKS, kapsayıcılar
ms.openlocfilehash: 072bfc8c243eb9b69e06366961019b88b67e0941
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100392247"
---
# <a name="deploy-configurations-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Arc özellikli Kubernetes kümesinde (Önizleme) GitOps kullanarak yapılandırmaları dağıtma

Kubernetes ile ilgili olarak, Giüstler, bir git deposunda Kubernetes küme yapılandırmalarının (dağıtımlar, ad alanları vb.) istenen durumunu bildirme uygulamasıdır. Bu bildirimin ardından bir işleç kullanarak bu küme yapılandırmalarının yoklaması ve çekme tabanlı dağıtımı gelir. 

Bu makalede, Azure Arc etkin Kubernetes kümelerindeki Gilar iş akışlarının kurulumu ele alınmaktadır.

Kümeniz ve git deposu arasındaki bağlantı, `Microsoft.KubernetesConfiguration/sourceControlConfigurations` Azure Resource Manager uzantı kaynağı olarak oluşturulur. `sourceControlConfiguration`Kaynak özellikleri, Kubernetes kaynaklarının git 'ten kümenize nasıl akmasını gerektiğini temsil eder. Veriler `sourceControlConfiguration` , verilerin gizliliğini sağlamak için bir Azure Cosmos DB veritabanında şifreli olarak depolanır.

`config-agent`Kümenizde çalışan şu şekilde sorumludur:
* `sourceControlConfiguration`Azure Arc etkin Kubernetes kaynağında yeni veya güncelleştirilmiş uzantı kaynaklarını izleme.
* Her biri için Git deposunu izlemek üzere Flox operatörü dağıtma `sourceControlConfiguration` .
* Herhangi bir güncelleştirme uygulanıyor `sourceControlConfiguration` . 

`sourceControlConfiguration`Çoklu kiracı elde etmek için aynı Azure Arc etkin Kubernetes kümesinde birden çok kaynak oluşturabilirsiniz. Her biri farklı bir kapsamla oluşturarak, ilgili ad alanları dahilinde dağıtımları sınırlayın `sourceControlConfiguration` `namespace` .

Git deposu şunları içerebilir:
* YAML-ad alanları, ConfigMaps, dağıtımlar, DaemonSets vb. gibi geçerli bir Kubernetes kaynağını tanımlayan bildirimleri biçimlendirir. 
* Uygulamaları dağıtmak için Held grafikleri. 

Yaygın bir senaryo kümesi, kuruluşunuz için genel Azure rolleri ve bağlamaları, izleme veya günlük aracıları ya da küme genelinde hizmetler gibi temel bir yapılandırma tanımlamayı içerir.

Aynı model, heterojen ortamlar arasında dağıtılabilen daha büyük bir küme koleksiyonunu yönetmek için kullanılabilir. Örneğin, tek seferde birden fazla Kubernetes kümesi için geçerli olan kuruluşunuzun temel yapılandırmasını tanımlayan bir havuzunuz vardır. [Azure ilkesi](use-azure-policy.md) , bir `sourceControlConfiguration` kapsamdaki (abonelik veya kaynak grubu) tüm Azure Arc etkin Kubernetes kaynakları üzerinde belirli bir parametre kümesiyle bir oluşturma işlemini otomatik hale getirebilir.

Kapsama sahip bir yapılandırma kümesinin nasıl uygulanacağını öğrenmek için aşağıdaki adımları izleyin `cluster-admin` .

## <a name="before-you-begin"></a>Başlamadan önce

Mevcut bir Azure Arc etkin Kubernetes bağlı kümesine sahip olduğunuzu doğrulayın. Bağlı bir kümeye ihtiyacınız varsa bkz. [Azure yay etkin Kubernetes kümesi hızlı başlangıç](./connect-cluster.md).

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

Bu makalede kullanılan [örnek depo](https://github.com/Azure/arc-k8s-demo) , birkaç ad alanı sağlamak, ortak bir iş yükü dağıtmak ve takıma özgü bazı yapılandırmalar sağlamak isteyen bir küme operatörü etrafında yapılandırılmıştır. Bu depoyu kullanmak, kümenizde aşağıdaki kaynakları oluşturur:


* **Ad alanları:** `cluster-config` , `team-a` , `team-b`
* **Dağıtım:**`cluster-config/azure-vote`
* **Configmap:**`team-a/endpoints`

`config-agent`Yeni veya güncelleştirilmiş Için Azure 'ı yoklar `sourceControlConfiguration` . Bu görev 30 saniyeye kadar sürer.

Özel bir depoyu ile ilişkilendirirken `sourceControlConfiguration` , [özel bir git deposundan yapılandırma Uygula](#apply-configuration-from-a-private-git-repository)bölümündeki adımları uygulayın.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

`k8sconfiguration`Bağlı bir kümeyi [örnek git deposuna](https://github.com/Azure/arc-k8s-demo)bağlamak için Azure CLI uzantısını kullanın. 
1. Bu yapılandırmayı adlandırın `cluster-config` .
1. Aracıyı, ad alanında işlecini dağıtmasını bildirin `cluster-config` .
1. İşleç izinlerini verin `cluster-admin` .

```azurecli
az k8sconfiguration create --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url https://github.com/Azure/arc-k8s-demo --scope cluster --cluster-type connectedClusters
```

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Pending",
    "lastConfigApplied": "0001-01-01T00:00:00",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": null,
  "id": "/subscriptions/<sub id>/resourceGroups/<group name>/providers/Microsoft.Kubernetes/connectedClusters/<cluster name>/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-k8s-demo",
  "resourceGroup": "MyRG",
  "sshKnownHostsContents": "",
  "systemData": {
    "createdAt": "2020-11-24T21:22:01.542801+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-11-24T21:22:01.542801+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
  },
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
  ```

#### <a name="use-a-public-git-repo"></a>Ortak bir git deposu kullanma

| Parametre | Biçimlendir |
| ------------- | ------------- |
| `--repository-url` | http [s]:/sunucu/repo [. git] veya git:/sunucu/repo [. git]

#### <a name="use-a-private-git-repo-with-ssh-and-flux-created-keys"></a>SSH ve Flox tarafından oluşturulan anahtarlarla özel bir git deposu kullanma

| Parametre | Biçimlendir | Notlar
| ------------- | ------------- | ------------- |
| `--repository-url` | ssh://user@server/repo[. git] veya user@server:repo [. git] | `git@` değişebilir `user@`

> [!NOTE]
> Flox tarafından üretilen ortak anahtar, git hizmeti sağlayıcınızdaki Kullanıcı hesabına eklenmelidir. Anahtar Kullanıcı hesabı yerine depoya eklenirse, `git@` `user@` URL 'de yerine kullanın. Daha fazla ayrıntı için [özel git deposundan yapılandırmaya Uygula](#apply-configuration-from-a-private-git-repository) bölümüne atlayın.

#### <a name="use-a-private-git-repo-with-ssh-and-user-provided-keys"></a>SSH ve Kullanıcı tarafından sağlanmış anahtarlarla özel bir git deposu kullanma

| Parametre | Biçimlendir | Notlar |
| ------------- | ------------- | ------------- |
| `--repository-url`  | ssh://user@server/repo[. git] veya user@server:repo [. git] | `git@` değişebilir `user@` |
| `--ssh-private-key` | [pek biçiminde](https://aka.ms/PEMformat) Base64 kodlamalı anahtar | Anahtarı doğrudan sağlama |
| `--ssh-private-key-file` | yerel dosyanın tam yolu | Yerel dosyanın pek biçimli anahtarını içeren tam yolunu sağlayın

> [!NOTE]
> Kendi özel anahtarınızı doğrudan veya bir dosyaya sağlayın. Anahtar [pek biçiminde](https://aka.ms/PEMformat) olmalı ve yeni satır (\n) ile bitmelidir.  İlişkili ortak anahtar, git hizmeti sağlayıcınızdaki Kullanıcı hesabına eklenmelidir. Anahtar, Kullanıcı hesabı yerine depoya eklenirse, yerine kullanın `git@` `user@` . Daha fazla ayrıntı için [özel git deposundan yapılandırmaya Uygula](#apply-configuration-from-a-private-git-repository) bölümüne atlayın.

#### <a name="use-a-private-git-host-with-ssh-and-user-provided-known-hosts"></a>SSH ve Kullanıcı tarafından sağlanmış bilinen konaklarla özel bir git Konağı kullanma

| Parametre | Biçimlendir | Notlar |
| ------------- | ------------- | ------------- |
| `--repository-url`  | ssh://user@server/repo[. git] veya user@server:repo [. git] | `git@` değişebilir `user@` |
| `--ssh-known-hosts` | base64 kodlu | Bilinen ana bilgisayarlar içeriğini doğrudan sağlama |
| `--ssh-known-hosts-file` | yerel dosyanın tam yolu | Yerel bir dosyada bilinen ana bilgisayarlar içeriği sağlama |

> [!NOTE]
> SSH bağlantısını kurmadan önce git deposunun kimliğini doğrulamak için Flox işleci, bilinen ana bilgisayar dosyasındaki ortak git konaklarının bir listesini tutar. Yaygın olmayan bir git deposu veya kendi git ana bilgisayarınızı kullanıyorsanız, akıcı x 'in deponuzu tanımlayabilmesi için ana bilgisayar anahtarını sağlamanız gerekebilir. Known_hosts içeriğinizi doğrudan veya bir dosya içinde sağlayabilirsiniz. Kendi içeriğinizi sağlarken yukarıda açıklanan SSH anahtar senaryolarından biriyle birlikte [known_hosts içerik biçimi belirtimlerini](https://aka.ms/KnownHostsFormat) kullanın.

#### <a name="use-a-private-git-repo-with-https"></a>HTTPS ile özel bir git deposu kullanma

| Parametre | Biçimlendir | Notlar |
| ------------- | ------------- | ------------- |
| `--repository-url` | https://server/repo[. git] | Temel kimlik doğrulaması ile HTTPS |
| `--https-user` | ham veya base64 kodlu | HTTPS Kullanıcı adı |
| `--https-key` | ham veya base64 kodlu | HTTPS kişisel erişim belirteci veya parolası

> [!NOTE]
> HTTPS helmrelease özel kimlik doğrulaması yalnızca Held operatörü grafik sürümü 1.2.0 + (varsayılan) ile desteklenir.
> Şu anda Azure Kubernetes Hizmetleri tarafından yönetilen kümeler için HTTPS helk Release özel kimlik doğrulaması desteklenmez.
> Proxy 'niz aracılığıyla Git deposuna erişmek için Flox 'e ihtiyacınız varsa, Azure Arc aracılarını ara sunucu ayarlarıyla güncelleştirmeniz gerekecektir. Daha fazla bilgi için bkz. [giden ara sunucu kullanarak bağlanma](./connect-cluster.md#connect-using-an-outbound-proxy-server).

#### <a name="additional-parameters"></a>Ek parametreler

Aşağıdaki isteğe bağlı parametrelerle yapılandırmayı özelleştirin:

| Parametre | Açıklama |
| ------------- | ------------- |
| `--enable-helm-operator`| Hele grafik dağıtımları desteğini etkinleştirmek için geçiş yapın. |
| `--helm-operator-params` | Hele işleci için grafik değerleri (etkinse). Örneğin, `--set helm.versions=v3`. |
| `--helm-operator-version` | Hele işlecinin grafik sürümü (etkinse). 1.2.0 + sürümünü kullanın. Varsayılan: ' 1.2.0 '. |
| `--operator-namespace` | İşleç ad alanının adı. Varsayılan: ' default '. En fazla: 23 karakter. |
| `--operator-params` | İşleç parametreleri. Tek tırnak içinde verilmelidir. Örneğin, ```--operator-params='--git-readonly --sync-garbage-collection --git-branch=main' ``` 

##### <a name="options-supported-in----operator-params"></a>Desteklenen Seçenekler  `--operator-params` :

| Seçenek | Açıklama |
| ------------- | ------------- |
| `--git-branch`  | Kubernetes bildirimleri için kullanılacak git deposunun dalı. Varsayılan değer ' Master '. Daha yeni depoların adlı kök dalı vardır `main` ve bu durumda ayarlamanız gerekir `--git-branch=main` . |
| `--git-path`  | Kubernetes bildirimlerini bulmak için Flox için Git deposu içindeki göreli yol. |
| `--git-readonly` | Git deposu salt okunurdur, Flox buna yazmaya çalışmaz. |
| `--manifest-generation`  | Etkinleştirilirse, Flox. Flox. YAML 'yi arayacaktır ve Kustomize veya diğer bildirim oluşturucularını çalıştırır. |
| `--git-poll-interval`  | Yeni işlemeler için git deposunun yoklamak için gereken süre. Varsayılan değer `5m` (5 dakikadır). |
| `--sync-garbage-collection`  | Etkinleştirilirse, Flox oluşturduğu kaynakları silecek, ancak artık git içinde mevcut değil. |
| `--git-label`  | Eşitleme ilerlemesini izlemek için etiket. Git dalını etiketlemek için kullanılır.  `flux-sync` varsayılan değerdir. |
| `--git-user`  | Git yürütmesi için Kullanıcı adı. |
| `--git-email`  | Git yürütmesi için kullanılacak e-posta. 

Flox 'in depoya yazmasını ve `--git-user` ya da `--git-email` ayarlanmamasını istemiyorsanız, `--git-readonly` otomatik olarak ayarlanır.

Daha fazla bilgi için bkz. [Flox belgeleri](https://aka.ms/FluxcdReadme).

> [!TIP]
> `sourceControlConfiguration`Azure Arc etkin Kubernetes kaynağının **Gima** sekmesindeki Azure Portal oluşturabilirsiniz.

## <a name="validate-the-sourcecontrolconfiguration"></a>SourceControlConfiguration 'ı doğrulama

Azure CLı kullanarak `sourceControlConfiguration` başarıyla oluşturulduğunu doğrulayın.

```azurecli
az k8sconfiguration show --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

`sourceControlConfiguration`Kaynak, uyumluluk durumu, iletiler ve hata ayıklama bilgileriyle güncelleştirilir.

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2020-12-10T18:26:52.801000+00:00",
    "message": "...",
    "messageLevel": "Information"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": {
    "chartValues": "",
    "chartVersion": ""
  },
  "id": "/subscriptions/<sub id>/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git",
  "resourceGroup": "AzureArcTest",
  "sshKnownHostsContents": null,
  "systemData": {
    "createdAt": "2020-12-01T03:58:56.175674+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-12-10T18:30:56.881219+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
},
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Bir `sourceControlConfiguration` oluşturulduğunda veya güncelleştirilirken, bu konuda birkaç şey meydana gelir:

1. Azure Arc, `config-agent` Yeni veya güncelleştirilmiş yapılandırmalar () için Azure Resource Manager izler `Microsoft.KubernetesConfiguration/sourceControlConfigurations` ve yeni yapılandırmayı izler `Pending` .
1. `config-agent`Yapılandırma özelliklerini okur ve hedef ad alanını oluşturur.
1. Azure Arc, `controller-manager` bir Kubernetes hizmet hesabını uygun izinle ( `cluster` veya `namespace` kapsamla) hazırlar ve sonra bir örneğini dağıtır `flux` .
1. SSH seçeneğini akışkan x tarafından oluşturulan anahtarlarla birlikte kullanıyorsanız, `flux` BIR SSH anahtarı oluşturur ve ortak anahtarı günlüğe kaydeder.
1. `config-agent`Raporlar, `sourceControlConfiguration` Azure 'daki kaynağa geri bildirir.

Sağlama işlemi gerçekleşirken, `sourceControlConfiguration` birkaç durum değişikliği arasında hareket eder. Yukarıdaki komutla ilerlemeyi izleyin `az k8sconfiguration show ...` :

| Aşama değişikliği | Description |
| ------------- | ------------- |
| `complianceStatus`-> `Pending` | İlk ve devam eden durumları temsil eder. |
| `complianceStatus` -> `Installed`  | `config-agent` , kümeyi başarıyla yapılandırıp dağıtımı hatasız olarak dağıtabilecek `flux` . |
| `complianceStatus` -> `Failed` | `config-agent` bir hata dağıtımıyla karşılaşıldı `flux` , Ayrıntılar yanıt gövdesinde kullanılabilir olmalıdır `complianceStatus.message` . |

## <a name="apply-configuration-from-a-private-git-repository"></a>Özel bir git deposundan yapılandırma Uygula

Özel bir git deposu kullanıyorsanız, deponuzda SSH ortak anahtarını yapılandırmanız gerekir. SSH ortak anahtarı, akıcı x 'in oluşturduğu veya sağladığınız bir tane olacaktır. Ortak anahtarı belirli bir git deposunda veya depoya erişimi olan git kullanıcısına yapılandırabilirsiniz. 

### <a name="get-your-own-public-key"></a>Kendi ortak anahtarınızı alın

Kendi SSH anahtarlarınızı oluşturduysanız, özel ve ortak anahtarlarınız zaten vardır.

#### <a name="get-the-public-key-using-azure-cli"></a>Azure CLı kullanarak ortak anahtarı alın 

Flox anahtarları oluşturduğunda aşağıdakiler yararlı olur.

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --cluster-type connectedClusters --query 'repositoryPublicKey' 
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

#### <a name="get-the-public-key-from-the-azure-portal"></a>Azure portal ortak anahtarı al

Flox anahtarları oluşturduğunda aşağıdakiler yararlı olur.

1. Azure portal bağlı küme kaynağına gidin.
2. Kaynak sayfasında, "Gilar" ı seçin ve bu küme için yapılandırmaların listesini görüntüleyin.
3. Özel Git deposunu kullanan yapılandırmayı seçin.
4. Açılan bağlam penceresinde, pencerenin alt kısmında **Depo ortak anahtarını** kopyalayın.

#### <a name="add-public-key-using-github"></a>GitHub kullanarak ortak anahtar ekleme

Aşağıdaki seçeneklerden birini kullanın:

* Seçenek 1: Kullanıcı hesabınıza ortak anahtar ekleyin (hesabınızdaki tüm depolar için geçerlidir):  
    1. GitHub ' ı açın ve sayfanın sağ üst köşesindeki profil simgesine tıklayın.
    2. **Ayarlar**’a tıklayın.
    3. **SSH ve GPG anahtarlarına** tıklayın.
    4. **Yenı SSH anahtarı**' na tıklayın.
    5. Bir başlık girin.
    6. Ortak anahtarı çevreleyen tırnak işareti olmadan yapıştırın.
    7. **SSH anahtarı Ekle**' ye tıklayın.

* Seçenek 2: ortak anahtarı git deposuna bir dağıtım anahtarı olarak ekleyin (yalnızca bu depo için geçerlidir):  
    1. GitHub ' u açın ve depoya gidin.
    1. **Ayarlar**’a tıklayın.
    1. **Anahtarları dağıt**' a tıklayın.
    1. **Dağıtım anahtarı Ekle**' ye tıklayın.
    1. Bir başlık girin.
    1. **Yazma erişimine Izin ver**' i işaretleyin.
    1. Ortak anahtarı çevreleyen tırnak işareti olmadan yapıştırın.
    1. **Anahtar Ekle**' ye tıklayın.

#### <a name="add-public-key-using-an-azure-devops-repository"></a>Azure DevOps deposu kullanarak ortak anahtar ekleme

Anahtarı SSH Anahtarlarınıza eklemek için aşağıdaki adımları kullanın:

1. Sağ üst taraftaki (profil görüntüsünün yanında) **Kullanıcı ayarları** altında **SSH ortak anahtarlar**' a tıklayın.
1. **+ Yeni anahtar**' ı seçin.
1. Bir ad sağlayın.
1. Ortak anahtarı çevreleyen tırnak işareti olmadan yapıştırın.
1. **Ekle**'ye tıklayın.

## <a name="validate-the-kubernetes-configuration"></a>Kubernetes yapılandırmasını doğrulama

Örneği yükledikten sonra `config-agent` `flux` , git deposunda tutulan Kaynaklar kümeye akışa başlamalıdır. Ad alanları, dağıtımlar ve kaynakların aşağıdaki komutla oluşturulup oluşturulmadıysa emin olun:

```console
kubectl get ns --show-labels
```

**Çıktıların**

```console
NAME              STATUS   AGE    LABELS
azure-arc         Active   24h    <none>
cluster-config    Active   177m   <none>
default           Active   29h    <none>
itops             Active   177m   fluxcd.io/sync-gc-mark=sha256.9oYk8yEsRwWkR09n8eJCRNafckASgghAsUWgXWEQ9es,name=itops
kube-node-lease   Active   29h    <none>
kube-public       Active   29h    <none>
kube-system       Active   29h    <none>
team-a            Active   177m   fluxcd.io/sync-gc-mark=sha256.CS5boSi8kg_vyxfAeu7Das5harSy1i0gc2fodD7YDqA,name=team-a
team-b            Active   177m   fluxcd.io/sync-gc-mark=sha256.vF36thDIFnDDI2VEttBp5jgdxvEuaLmm7yT_cuA2UEw,name=team-b
```

,, `team-a` `team-b` `itops` Ve `cluster-config` ad alanlarının oluşturulduğunu görebiliriz.

`flux`İşleci `cluster-config` , şu deneyimle yönlendirerek ad alanına dağıtıldı `sourceControlConfig` :

```console
kubectl -n cluster-config get deploy  -o wide
```

**Çıktıların**

```console
NAME             READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES                         SELECTOR
cluster-config   1/1     1            1           3h    flux         docker.io/fluxcd/flux:1.16.0   instanceName=cluster-config,name=flux
memcached        1/1     1            1           3h    memcached    memcached:1.5.15               name=memcached
```

## <a name="further-exploration"></a>Keşfetmeye devam edin

Yapılandırma deposunun bir parçası olarak dağıtılan diğer kaynakları şu kullanarak keşfedebilirsiniz:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Yapılandırma silme

`sourceControlConfiguration`Azure CLI veya Azure Portal kullanarak bir silme.  Sil komutunu başlattıktan sonra, `sourceControlConfiguration` kaynak Azure 'da hemen silinir. İlişkili nesnelerin kümeden tam silme işlemi 10 dakika içinde gerçekleşmelidir. Kaldırıldığında, `sourceControlConfiguration` ilişkili nesnelerin tam silinmesi bir saate kadar sürebilir.

> [!NOTE]
> Bir kapsam oluşturulduktan sonra `sourceControlConfiguration` `namespace` , `edit` ad alanında rol bağlama olan kullanıcılar bu ad alanına iş yüklerini dağıtabilir. `sourceControlConfiguration`Ad alanı kapsamı silindiğinde, ad alanı değişmeden kalır ve bu diğer iş yüklerinin kesilmesini önlemek için silinmez. Gerekirse, bu ad alanını ile el ile silebilirsiniz `kubectl` .  
> Değişiklik yapıldığında, izlenen git deposundan dağıtımların sonucu olan kümedeki değişiklikler silinmez `sourceControlConfiguration` .

```azurecli
az k8sconfiguration delete --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak denetimi yapılandırması ile Held kullanma](./use-gitops-with-helm.md)
- [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)