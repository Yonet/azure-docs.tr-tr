---
title: Azure Key Vault’u Kubernetes ile tümleştirme
description: Bu öğreticide, Kubernetes pods 'ye bağlamak için gizli dizi kapsayıcısı depolama arabirimi (CSı) sürücüsünü kullanarak Azure anahtar kasanızdan gizli dizi ve gizli dizileri alırsınız.
author: msmbaldwin
ms.author: mbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 09/25/2020
ms.openlocfilehash: f4981036ca92f6efe2d3e23ea1f507a3a1f3c70a
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98234265"
---
# <a name="tutorial-configure-and-run-the-azure-key-vault-provider-for-the-secrets-store-csi-driver-on-kubernetes"></a>Öğretici: Kubernetes 'te gizli dizi için Azure Key Vault sağlayıcıyı yapılandırma ve çalıştırma

> [!IMPORTANT]
> CSı sürücüsü, Azure teknik desteği tarafından desteklenmeyen açık kaynaklı bir projem. Lütfen [GitHub bağlantısı üzerinde](https://github.com/Azure/secrets-store-csi-driver-provider-azure/issues)Bu Key Vault, CSI sürücüsü ile ilgili tüm geri bildirim ve sorunları rapor edin. Bu araç, kullanıcıların kümelere kendi kendine yüklenmesi ve topluluğumuza geri bildirim toplaması için sağlanır.


Bu öğreticide, gizli dizileri Kubernetes pods 'ye bağlamak için gizli dizi kapsayıcısı depolama arabirimi (CSı) sürücüsünü kullanarak Azure anahtar kasanızdan gizli dizi ve gizli dizi bilgileri alabilirsiniz.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri kullanın.
> * Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtın.
> * Held ve gizlilikler deposunun CSı sürücüsünü yükler.
> * Azure Anahtar Kasası oluşturun ve gizli dizilerinizi ayarlayın.
> * Kendi SecretProviderClass nesneniz oluşturun.
> * Ana kasanızdan bağlı gizli dizileri kullanarak Pod 'nizi dağıtın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Bu öğreticiye başlamadan önce [Azure CLI](/cli/azure/install-azure-cli-windows?view=azure-cli-latest)'yı yükleyebilirsiniz.

Bu öğreticide, Linux düğümlerinde Azure Kubernetes hizmetini çalıştırdığınız varsayılmaktadır.

## <a name="use-managed-identities"></a>Yönetilen kimlikleri kullanma

Bu diyagramda, yönetilen kimlik için AKS – Key Vault tümleştirme akışı gösterilmektedir:

![Yönetilen kimlik için AKS – Key Vault tümleştirme akışını gösteren diyagram](../media/aks-key-vault-integration-flow.png)

## <a name="deploy-an-azure-kubernetes-service-aks-cluster-by-using-the-azure-cli"></a>Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Azure Cloud Shell kullanmanız gerekmez. Azure CLı yüklü olan komut isteminize (Terminal) sahip olmanız yeterli olacaktır. 

[Azure CLI kullanarak bir Azure Kubernetes hizmet kümesi dağıtma](../../aks/kubernetes-walkthrough.md)bölümünde "kaynak grubu oluşturma", "aks kümesi oluşturma" ve "kümeye bağlanma" bölümlerine gidin. 

> [!NOTE] 
> Pod kimliği kullanmayı planlıyorsanız, aşağıdaki komutta gösterildiği gibi Kubernetes kümesini oluştururken etkinleştirin:
>
> ```azurecli
> az aks create -n contosoAKSCluster -g contosoResourceGroup --kubernetes-version 1.16.9 --node-count 1 --enable-managed-identity
> ```

1. [Path ortam değişkeninizi](https://www.java.com/en/download/help/path.xml) indirdiğiniz *kubectl.exe* dosyası olarak ayarlayın.
1. Aşağıdaki komutu kullanarak Kubernetes sürümünüzü denetleyin ve istemci ve sunucu sürümünü çıkartır. İstemci sürümü, yüklediğiniz *kubectl.exe* dosyasıdır ve sunucu sürümü, kümenizin üzerinde çalıştığı Azure Kubernetes hizmetidir (aks).
    ```azurecli
    kubectl version
    ```
1. Kubernetes sürümünüzün 1.16.0 veya üzeri olduğundan emin olun. Windows kümeleri için Kubernetes sürümünüzün 1.18.0 veya üzeri olduğundan emin olun. Aşağıdaki komut hem Kubernetes kümesini hem de düğüm havuzunu yükseltir. Komutun yürütülmesi birkaç dakika sürebilir. Bu örnekte, kaynak grubu *Contosoresourcegroup* ve Kubernetes kümesi *Contosoakscluster*' dir.
    ```azurecli
    az aks upgrade --kubernetes-version 1.16.9 --name contosoAKSCluster --resource-group contosoResourceGroup
    ```
1. Oluşturduğunuz AKS kümesinin meta verilerini göstermek için aşağıdaki komutu kullanın. Daha sonra kullanmak üzere **PrincipalId**, **ClientID**, **SubscriptionID** ve **noderesourcegroup** 'u kopyalayın. AKS kümesi, Yönetilen kimlikler etkinleştirilmiş olarak oluşturulmadıysa, **PrincipalId** ve **ClientID** null olacaktır. 

    ```azurecli
    az aks show --name contosoAKSCluster --resource-group contosoResourceGroup
    ```

    Çıktıda her iki parametre de vurgulanır:
    
    ![Azure CLı 'nin PrincipalId ve ClientID değerleri ile ekran görüntüsü, ](../media/kubernetes-key-vault-2.png) ![ SubscriptionID ve nodeResourceGroup değerleriyle vurgulanan Azure CLI ekran görüntüsü](../media/kubernetes-key-vault-3.png)
    
## <a name="install-helm-and-the-secrets-store-csi-driver"></a>Held ve gizlilikler deposunun CSı sürücüsünü yükleyip
> [!NOTE]
> Aşağıdaki yükleme yalnızca Linux üzerinde AKS üzerinde çalışmaktadır. Gizli dizileri depolama CSı sürücü yüklemesi hakkında daha fazla bilgi için bkz. [Azure Key Vault Provider for gizlilikler Store CSI Driver](https://github.com/Azure/secrets-store-csi-driver-provider-azure) 

Gizli anahtar deposu CSı sürücüsünü yüklemek için öncelikle [Held](https://helm.sh/docs/intro/install/)'yi yüklemeniz gerekir.

Gizli dizileri [depolama CSI](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/charts/csi-secrets-store-provider-azure/README.md) sürücü arabirimi sayesinde, Azure Anahtar Kasası Örneğinizde depolanan gizli dizileri alabilir ve sonra gizli Içeriği Kubernetes pods 'ye bağlamak için sürücü arabirimini kullanabilirsiniz.

1. Held sürümünün v3 veya üzeri olduğundan emin olun:
    ```azurecli
    helm version
    ```
1. Sürücü için gizli anahtar deposu CSı sürücüsünü ve Azure Key Vault sağlayıcısını yükler:
    ```azurecli
    helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts

    helm install csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --generate-name
    ```

## <a name="create-an-azure-key-vault-and-set-your-secrets"></a>Azure Anahtar Kasası oluşturma ve sırlarınızı ayarlama

Kendi anahtar kasanızı oluşturmak ve sırlarınızı ayarlamak için [Azure CLI kullanarak Azure Key Vault bir gizli anahtarı ayarlama ve alma](../secrets/quick-create-cli.md)bölümündeki yönergeleri izleyin.

> [!NOTE] 
> Azure Cloud Shell kullanmanız veya yeni bir kaynak grubu oluşturmanız gerekmez. Daha önce Kubernetes kümesi için oluşturduğunuz kaynak grubunu kullanabilirsiniz.

## <a name="create-your-own-secretproviderclass-object"></a>Kendi SecretProviderClass nesneniz oluşturma

Özel bir SecretProviderClass nesneniz olan gizli dizi parametrelerini sağlayıcıya özgü parametrelerle oluşturmak için [Bu şablonu kullanın](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/v1alpha1_secretproviderclass_service_principal.yaml). Bu nesne, anahtar kasanıza kimlik erişimi sağlar.

Örnek SecretProviderClass YAML dosyasında eksik parametreleri girin. Aşağıdaki parametreler gereklidir:

* **Useratandıdentityıd**: # [gerekli] değer boşsa, varsayılan olarak VM 'de sistem tarafından atanan kimliği kullanır 
* **Keyvaultname**: anahtar kasanızın adı
* **nesneler**: bağlamak istediğiniz tüm gizli içerik için kapsayıcı
    * **ObjectName**: gizli içeriğin adı
    * **ObjectType**: nesne türü (gizli, anahtar, sertifika)
* **resourceGroup**: kaynak grubunun adı # [sürüm için gerekli < 0.0.4] anahtar kasasının kaynak grubu
* **SubscriptionID**: anahtar KASASıNıN abonelik kimliği # [sürüm < 0.0.4] için gereken anahtar kasasının abonelik kimliği
* **Tenantıd**: anahtar kasaınızın Kiracı kimliği veya dizin kimliği

Tüm gerekli alanların belgelerine buradan ulaşabilirsiniz: [bağlantı](https://github.com/Azure/secrets-store-csi-driver-provider-azure#create-a-new-azure-key-vault-resource-or-use-an-existing-one)

Güncelleştirilmiş şablon aşağıdaki kodda gösterilmiştir. Bunu bir YAML dosyası olarak indirin ve gerekli alanları girin. Bu örnekte, Anahtar Kasası **contosoKeyVault5**' dir. İki gizli dizi vardır, **secret1** ve **secret2**.

> [!NOTE] 
> Yönetilen kimlikler kullanıyorsanız, **Usepodidentity** değerini *true* olarak ayarlayın ve **useratandidentityıd** değerini tırnak işareti (**""**) olarak ayarlayın. 

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: azure-kvname
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"                   # [REQUIRED] Set to "true" if using managed identities
    useVMManagedIdentity: "false"             # [OPTIONAL] if not provided, will default to "false"
    userAssignedIdentityID: "servicePrincipalClientID"       # [REQUIRED]  If you're using a user-assigned identity as the VM's managed identity, specify the identity's client id. If the value is empty, it defaults to use the system-assigned identity on the VM
                                                         
    keyvaultName: "contosoKeyVault5"          # [REQUIRED] the name of the key vault
                                              #     az keyvault show --name contosoKeyVault5
                                              #     the preceding command will display the key vault metadata, which includes the subscription ID, resource group name, key vault 
    cloudName: ""                                # [OPTIONAL for Azure] if not provided, Azure environment will default to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1                 # [REQUIRED] object name
                                              #     az keyvault secret list --vault-name "contosoKeyVault5"
                                              #     the above command will display a list of secret names from your key vault
          objectType: secret                  # [REQUIRED] object types: secret, key, or cert
          objectVersion: ""                   # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: secret2
          objectType: secret
          objectVersion: ""
    resourceGroup: "contosoResourceGroup"     # [REQUIRED] the resource group name of the key vault
    subscriptionId: "subscriptionID"          # [REQUIRED] the subscription ID of the key vault
    tenantId: "tenantID"                      # [REQUIRED] the tenant ID of the key vault
```
Aşağıdaki görüntüde, **az keykasa Show--Name contosoKeyVault5** for the ilgili vurgulanan meta verilerle konsol çıktısı gösterilmektedir:

!["Az keykasa Show--Name contosoKeyVault5" için konsol çıktısını gösteren ekran görüntüsü](../media/kubernetes-key-vault-4.png)

## <a name="assign-managed-identity"></a>Yönetilen kimlik ata

Oluşturduğunuz AKS kümesine belirli roller atayın. 

1. Kullanıcı tarafından atanan yönetilen kimlik oluşturmak, listelemek veya okumak için, AKS kümenizin [yönetilen kimlik operatörü](../../role-based-access-control/built-in-roles.md#managed-identity-operator) rolüne atanması gerekir. **$ClientID** Kubernetes kümesinin ClientID 'si olduğundan emin olun. Kapsam için, bu, özellikle AKS kümesi oluşturulduğunda yapılmış olan düğüm kaynak grubu Azure abonelik hizmetiniz altında olacaktır. Bu kapsam, aşağıda atanan rollerden yalnızca o gruptaki kaynakların etkilendiğinden emin olur. 

    ```azurecli
    RESOURCE_GROUP=contosoResourceGroup
    
    az role assignment create --role "Managed Identity Operator" --assignee $clientId --scope /subscriptions/<SUBID>/resourcegroups/$RESOURCE_GROUP
    
    az role assignment create --role "Virtual Machine Contributor" --assignee $clientId --scope /subscriptions/<SUBID>/resourcegroups/$RESOURCE_GROUP
    ```

1. Azure Active Directory (Azure AD) kimliğini AKS 'e yükler.
    ```azurecli
    helm repo add aad-pod-identity https://raw.githubusercontent.com/Azure/aad-pod-identity/master/charts

    helm install pod-identity aad-pod-identity/aad-pod-identity
    ```

1. Bir Azure AD kimliği oluşturun. Çıktıda, daha sonra kullanmak üzere **ClientID** ve **PrincipalId** ' yi kopyalayın.
    ```azurecli
    az identity create -g $resourceGroupName -n $identityName
    ```

1. Anahtar kasanızın önceki adımında oluşturduğunuz Azure AD kimliğine *okuyucu* rolünü atayın ve sonra anahtar kasaınızdan gizli dizileri almak için kimlik izinlerini verin. Azure AD kimliğinden **ClientID** ve **PrincipalId** ' i kullanın.
    ```azurecli
    az role assignment create --role "Reader" --assignee $principalId --scope /subscriptions/<SUBID>/resourceGroups/contosoResourceGroup/providers/Microsoft.KeyVault/vaults/contosoKeyVault5

    az keyvault set-policy -n contosoKeyVault5 --secret-permissions get --spn $clientId
    az keyvault set-policy -n contosoKeyVault5 --key-permissions get --spn $clientId
    ```

## <a name="deploy-your-pod-with-mounted-secrets-from-your-key-vault"></a>Ana kasanızdan bağlı gizli dizileri kullanarak Pod 'nizi dağıtın

SecretProviderClass nesneniz yapılandırmak için aşağıdaki komutu çalıştırın:
```azurecli
kubectl apply -f secretProviderClass.yaml
```

### <a name="use-managed-identities"></a>Yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanıyorsanız, kümenizde daha önce oluşturduğunuz kimliğe başvuran bir *AzureIdentity* oluşturun. Daha sonra, oluşturduğunuz AzureIdentity 'e başvuran bir *AzureIdentityBinding* oluşturun. Aşağıdaki şablondaki parametreleri doldurun ve sonra da *Podidentityandbinding. YAML* olarak kaydedin.  

```yml
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentity
metadata:
    name: "azureIdentityName"               # The name of your Azure identity
spec:
    type: 0                                 # Set type: 0 for managed service identity
    resourceID: /subscriptions/<SUBID>/resourcegroups/<RESOURCEGROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<AZUREIDENTITYNAME>
    clientID: "managedIdentityClientId"     # The clientId of the Azure AD identity that you created earlier
---
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentityBinding
metadata:
    name: azure-pod-identity-binding
spec:
    azureIdentity: "azureIdentityName"      # The name of your Azure identity
    selector: azure-pod-identity-binding-selector
```
    
Bağlamayı yürütmek için aşağıdaki komutu çalıştırın:

```azurecli
kubectl apply -f podIdentityAndBinding.yaml
```

Sonra, Pod öğesini dağıtırsınız. Aşağıdaki kod, önceki adımdan Pod kimlik bağlamasını kullanan dağıtım YAML dosyasıdır. Bu dosyayı *Pod Bindingdeployment. YAML* olarak kaydedin.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-secrets-store-inline
  labels:
    aadpodidbinding: azure-pod-identity-binding-selector
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: secrets-store-inline
          mountPath: "/mnt/secrets-store"
          readOnly: true
  volumes:
    - name: secrets-store-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: azure-kvname
```

Pod 'nizi dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli
kubectl apply -f podBindingDeployment.yaml
```

### <a name="check-the-pod-status-and-secret-content"></a>Pod durum ve gizli içeriğini denetleme 

Dağıttığınız bir pod 'yi göstermek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl get pods
```

Pod uygulamanızın durumunu denetlemek için şu komutu çalıştırın:
```azurecli
kubectl describe pod/nginx-secrets-store-inline
```

![Pod 'un "çalışıyor" durumunu görüntüleyen ve tüm olayları "normal" olarak gösteren Azure CLı çıkışının ekran görüntüsü ](../media/kubernetes-key-vault-6.png)

Çıkış penceresinde, dağıtılan Pod, *çalışıyor* durumunda olmalıdır. Alttaki **Olaylar** bölümünde tüm olay türleri *normal* olarak görüntülenir.

Pod 'un çalıştığını doğruladıktan sonra Pod 'un anahtar kasasındaki gizli dizileri içerdiğini doğrulayabilirsiniz.

Pod 'da bulunan tüm gizli dizileri göstermek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- ls /mnt/secrets-store/
```

Belirli bir gizli dizi içeriğini göstermek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

Gizli dizi içeriğinin görüntülendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Anahtar kasanızın kurtarılabilir olduğundan emin olmak için, bkz.:
> [!div class="nextstepaction"]
> [Geçici silme özelliğini aç](./key-vault-recovery.md)
