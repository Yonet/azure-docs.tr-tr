---
title: İşlem örneği oluşturma ve yönetme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning işlem örneği oluşturmayı ve yönetmeyi öğrenin. Geliştirme ortamınız olarak veya geliştirme/test amaçları için işlem hedefi olarak kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, devx-track-azurecli
ms.author: sgilley
author: sdgilley
ms.reviewer: sgilley
ms.date: 10/02/2020
ms.openlocfilehash: 40882f2a0c1a65650d633d0784214afbeef9ae63
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94842898"
---
# <a name="create-and-manage-an-azure-machine-learning-compute-instance"></a>Azure Machine Learning işlem örneği oluşturma ve yönetme

Azure Machine Learning çalışma alanınızda bir [işlem örneği](concept-compute-instance.md) oluşturmayı ve yönetmeyi öğrenin.

Bulutta tam olarak yapılandırılmış ve yönetilen geliştirme ortamınız olarak bir işlem örneği kullanın. Geliştirme ve test için, örneği bir [eğitim işlem hedefi](concept-compute-target.md#train) veya bir [çıkarım hedefi](concept-compute-target.md#deploy)olarak da kullanabilirsiniz.   Bir işlem örneği, paralel olarak birden çok iş çalıştırabilir ve bir iş kuyruğuna sahiptir. Bir geliştirme ortamı olarak, bir işlem örneği çalışma alanınızdaki diğer kullanıcılarla paylaşılamaz.

Bu makalede şunları öğreneceksiniz:

* İşlem örneği oluşturma 
* İşlem örneğini yönetme (başlatma, durdurma, yeniden başlatma, silme)
* Terminal penceresine erişin 
* R veya Python paketlerini yükler
* Yeni ortamlar veya Jupyter çekirdekler oluşturma

İşlem örnekleri, kuruluşların SSH bağlantı noktalarını açmasına gerek kalmadan, işleri bir [sanal ağ ortamında](how-to-secure-training-vnet.md)güvenli bir şekilde çalıştırabilir. İş kapsayıcılı bir ortamda yürütülür ve model bağımlılıklarınızı bir Docker kapsayıcısında paketleyebilir. 

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

* [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

## <a name="create"></a>Oluşturma

**Tahmini süre**: yaklaşık 5 dakika.

İşlem örneği oluşturma, çalışma alanınız için tek seferlik bir işlemdir. Bu işlemi bir geliştirme iş istasyonu olarak veya eğitim için bir işlem hedefi olarak yeniden kullanabilirsiniz. Çalışma alanınıza eklenmiş birden çok işlem örneği olabilir.

VM ailesi kotası başına bölge başına adanmış çekirdekler ve işlem örneği oluşturma için geçerli olan toplam bölgesel kota, Azure Machine Learning eğitim işlem kümesi kotasıyla birleştirilmiş ve paylaşılır. İşlem örneği durdurulduğunda, işlem örneğini yeniden başlatabileceksiniz emin olmak için kota serbest bırakılır. Lütfen, işlem örneği oluşturulduktan sonra sanal makine boyutunu değiştirmek mümkün değildir.

Aşağıdaki örnek, bir işlem örneğinin nasıl oluşturulacağını gösterir:

# <a name="python"></a>[Python](#tab/python)

```python
import datetime
import time

from azureml.core.compute import ComputeTarget, ComputeInstance
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your instance
# Compute instance name should be unique across the azure region
compute_name = "ci{}".format(ws._workspace_id)[:10]

# Verify that instance does not exist already
try:
    instance = ComputeInstance(workspace=ws, name=compute_name)
    print('Found existing instance, use it.')
except ComputeTargetException:
    compute_config = ComputeInstance.provisioning_configuration(
        vm_size='STANDARD_D3_V2',
        ssh_public_access=False,
        # vnet_resourcegroup_name='<my-resource-group>',
        # vnet_name='<my-vnet-name>',
        # subnet_name='default',
        # admin_user_ssh_public_key='<my-sshkey>'
    )
    instance = ComputeInstance.create(ws, compute_name, compute_config)
    instance.wait_for_completion(show_output=True)
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [ComputeInstance sınıfı](/python/api/azureml-core/azureml.core.compute.computeinstance.computeinstance?preserve-view=true&view=azure-ml-py)
* [ComputeTarget. Create](/python/api/azureml-core/azureml.core.compute.computetarget?preserve-view=true&view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeInstance.wait_for_completion](/python/api/azureml-core/azureml.core.compute.computeinstance(class)?preserve-view=true&view=azure-ml-py#wait-for-completion-show-output-false--is-delete-operation-false-)


# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az ml computetarget create computeinstance  -n instance -s "STANDARD_D3_V2" -v
```

Daha fazla bilgi için, [az ml computetarget Create computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/create?preserve-view=true&view=azure-cli-latest#ext_azure_cli_ml_az_ml_computetarget_create_computeinstance) Reference bölümüne bakın.

# <a name="studio"></a>[Studio](#tab/azure-studio)

Azure Machine Learning Studio 'daki çalışma alanınızda, Not defterlerinizden birini çalıştırmaya hazırsanız **işlem** bölümünden veya **Not defterleri** bölümünde yeni bir işlem örneği oluşturun.

Studio 'da bir işlem örneği oluşturma hakkında bilgi için, bkz. [Azure Machine Learning Studio 'da işlem hedefleri oluşturma](how-to-create-attach-compute-studio.md#compute-instance).

---

Ayrıca, [Azure Resource Manager şablonuyla](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance)bir işlem örneği de oluşturabilirsiniz. 

### <a name="create-on-behalf-of-preview"></a>Adına oluştur (Önizleme)

Yönetici olarak, bir veri bilimcu adına bir işlem örneği oluşturabilir ve örneği bunlara ile atayabilirsiniz:
* [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance).  Bu şablonda gereken Tenantıd ve objectID 'yi bulma hakkında daha fazla bilgi için bkz. [kimlik doğrulama yapılandırması için kimlik nesne kimliklerini bulma](../healthcare-apis/find-identity-object-ids.md).  Bu değerleri Azure Active Directory portalında da bulabilirsiniz.
* REST API

İşlem örneğini oluşturduğunuz veri bilimcilerinin [Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md) izinleri olması gerekir: 
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/Başlat/eylem*
* *Microsoft. MachineLearningServices/Workspaces/hesaplar/durdur/eylem*
* *Microsoft. MachineLearningServices/Workspaces/hesaplar/yeniden Başlat/eylem*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/applicationaccess/Action*

Veri bilimcisi, işlem örneğini başlatabilir, durdurabilir ve yeniden başlatabilir. Bu işlemler için işlem örneğini kullanabilir:
* Jupyter
* Jupyıterlab
* RStudio
* Tümleşik Not defterleri

## <a name="manage"></a>Yönetme

Bir işlem örneğini başlatın, durdurun, yeniden başlatın ve silin. İşlem örneği otomatik olarak ölçeklenmez, bu nedenle devam eden ücretleri engellemek için kaynağı durdurmayı unutmayın.

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örneklerde, işlem örneği adı **örnek**

* Durumu Al

    ```python
    # get_status() gets the latest status of the ComputeInstance target
    instance.get_status()
    ```

* Durdur

    ```python
    # stop() is used to stop the ComputeInstance
    # Stopping ComputeInstance will stop the billing meter and persist the state on the disk.
    # Available Quota will not be changed with this operation.
    instance.stop(wait_for_completion=True, show_output=True)
    ```

* Başlangıç

    ```python
    # start() is used to start the ComputeInstance if it is in stopped state
    instance.start(wait_for_completion=True, show_output=True)
    ```

* Yeniden başlat

    ```python
    # restart() is used to restart the ComputeInstance
    instance.restart(wait_for_completion=True, show_output=True)
    ```

* Sil

    ```python
    # delete() is used to delete the ComputeInstance target. Useful if you want to re-use the compute name 
    instance.delete(wait_for_completion=True, show_output=True)
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki örneklerde, işlem örneği adı **örnek**

* Durdur

    ```azurecli-interactive
    az ml computetarget stop computeinstance -n instance -v
    ```

    Daha fazla bilgi için bkz. [az ml computetarget stop computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-computeinstance-stop).

* Başlangıç 

    ```azurecli-interactive
    az ml computetarget start computeinstance -n instance -v
    ```

    Daha fazla bilgi için bkz. [az ml computetarget start computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-computeinstance-start).

* Yeniden başlat 

    ```azurecli-interactive
    az ml computetarget restart computeinstance -n instance -v
    ```

    Daha fazla bilgi için bkz. [az ml computetarget restart computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-computeinstance-restart).

* Sil

    ```azurecli-interactive
    az ml computetarget delete -n instance -v
    ```

    Daha fazla bilgi için bkz. [az ml computetarget Delete computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-delete).

# <a name="studio"></a>[Studio](#tab/azure-studio)

Azure Machine Learning Studio 'daki çalışma alanınızda **işlem**' ı seçin ve ardından en üstteki **işlem örneği** ' ni seçin.

![İşlem örneğini yönetme](./media/concept-compute-instance/manage-compute-instance.png)

Aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Yeni bir işlem örneği oluştur 
* İşlem örnekleri sekmesini yenileyin.
* Bir işlem örneğini başlatın, durdurun ve yeniden başlatın.  Her çalıştığında örnek için ödeme yaparsınız. Maliyeti azaltmak için kullanmıyorsanız, işlem örneğini durdurun. Bir işlem örneğinin durdurulması onu kaldırır. Daha sonra ihtiyacınız olduğunda yeniden başlatın.
* Bir işlem örneğini silin.
* Yalnızca oluşturduğumuz göstermek için işlem örnekleri listesini filtreleyin.

Oluşturduğunuz (veya sizin için oluşturduğunuz) çalışma alanınızdaki her bir işlem örneği için şunları yapabilirsiniz:

* Jupyıter, Jupiterlab, RStudio 'yu işlem örneği üzerinde erişme
* İşlem örneğine SSH. SSH erişimi varsayılan olarak devre dışıdır ancak işlem örneği oluşturma sırasında etkinleştirilebilir. SSH erişimi, ortak/özel anahtar mekanizmasıyla gerçekleştirilir. Sekmesi, IP adresi, Kullanıcı adı ve bağlantı noktası numarası gibi SSH bağlantısı için Ayrıntılar verecektir.
* IP adresi ve bölge gibi belirli bir işlem örneği hakkındaki ayrıntıları alın.

---

[Azure RBAC](../role-based-access-control/overview.md) , çalışma alanındaki hangi kullanıcıların bir bilgi işlem örneği oluşturabileceğinizi, silebileceği, başlatabileceği, durdurabileceğinizi denetlemenize olanak tanır. Çalışma alanı katılımcısı ve sahip rolündeki tüm kullanıcılar çalışma alanı genelinde işlem örnekleri oluşturabilir, silebilir, başlatabilir, durdurabilir ve yeniden başlatabilir. Bununla birlikte, yalnızca belirli bir işlem örneği veya kendi adına oluşturulmuşsa atanan kullanıcı atanmış bir işlem örneği üzerinde Jupyıter, Jupiterlab ve RStudio erişimine izin verilir. Bir işlem örneği, kök erişimi olan tek bir kullanıcıya ayrılmıştır ve jupi/Jupiterlab/RStudio aracılığıyla oturum açabilir. İşlem örneğinde tek kullanıcı oturum açma işlemi olur ve tüm eylemler, bu kullanıcının Azure RBAC ve deneme çalıştırmaları için kimliğini kullanır. SSH erişimi, ortak/özel anahtar mekanizması aracılığıyla denetlenir.

Bu eylemler, Azure RBAC tarafından denetlenebilir:
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/okundu*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/yaz*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/Sil*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/Başlat/eylem*
* *Microsoft. MachineLearningServices/Workspaces/hesaplar/durdur/eylem*
* *Microsoft. MachineLearningServices/Workspaces/hesaplar/yeniden Başlat/eylem*


## <a name="access-the-terminal-window"></a>Terminal penceresine erişin

Aşağıdaki yollarla işlem örneğinizin Terminal penceresini açın:

* RStudio: sol üst taraftaki **Terminal** sekmesini seçin.
* Jupyter Laboratuvarı: Başlatıcı sekmesinde **diğer** başlığın altında bulunan **Terminal** kutucuğunu seçin.
* Jupyter: Dosyalar sekmesinde sağ üstteki **yeni>Terminal** ' i seçin.
* İşlem örneği oluşturulduğunda SSH erişimini etkinleştirdiyseniz makineye SSH.

Paketleri yüklemek ve ek çekirdekler oluşturmak için Terminal penceresini kullanın.

## <a name="install-packages"></a>Paketleri yükler

Paketleri doğrudan Jupyter Notebook veya RStudio 'Ya yükleyebilirsiniz:

* RStudio sağ alt köşedeki **paketler** sekmesini veya sol üstteki **konsol** sekmesini kullanın.  
* Python: Jupyter Notebook bir hücrede Install kodu ekleyin ve yürütün.

Ya da bir terminal penceresinden yükleyebilirsiniz. Python paketlerini **python 3,6-AzureML** ortamına yükler.  R paketlerini **r** ortamına yükler.

> [!NOTE]
> Bir not defteri içinde paket yönetimi için **% PIP** veya **% Conda** Magic işlevlerini kullanarak paketleri, tüm paketlere (Şu anda çalışan çekirdeklerdeki paketler dahil), tüm paketlere başvuran **! PIP** veya **! Conda** yerine **çalışmakta olan çekirdeğe** otomatik olarak yükler.

## <a name="add-new-kernels"></a>Yeni çekirdekler Ekle

> [!WARNING]
>  İşlem örneğini özelleştirirken, **azureml_py36** Conda ortamını veya **Python 3,6-azureml** çekirdeğini sildiğinizden emin olun. Bu, Jupyıter/jupen Terlab işlevselliği için gereklidir

İşlem örneğine yeni bir Jupyter çekirdeği eklemek için:

1. Jupyıter, Jupyıterlab veya Not defterleri bölmesinden ya da SSH 'den işlem örneğine yeni Terminal oluşturun
2. Yeni bir ortam oluşturmak için Terminal penceresini kullanın.  Örneğin, aşağıdaki kod oluşturulur `newenv` :

    ```shell
    conda create --name newenv
    ```

3. Ortamı etkinleştirin.  Örneğin, oluşturduktan sonra `newenv` :

    ```shell
    conda activate newenv
    ```

4. Yeni ortama privand ipykernel paketini yükleyip bu Conda env için bir çekirdek oluşturun

    ```shell
    conda install pip
    conda install ipykernel
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```

[Kullanılabilir Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) 'leri yüklenebilir.



## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim çalışması gönder](how-to-set-up-training-targets.md)
