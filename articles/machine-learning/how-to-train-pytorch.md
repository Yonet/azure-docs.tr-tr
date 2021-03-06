---
title: Derin öğrenme PyTorch modellerini eğitme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning kullanarak PyTorch eğitim betiklerinizi kurumsal ölçekte çalıştırmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: minxia
author: mx-iao
ms.reviewer: peterlu
ms.date: 01/14/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 962054943a68aa61ac681de97eeebc10fe3f2b0a
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98216640"
---
# <a name="train-pytorch-models-at-scale-with-azure-machine-learning"></a>PyTorch modellerini Azure Machine Learning ölçeklendirerek eğitme

Bu makalede, Azure Machine Learning kullanarak [Pytorch](https://pytorch.org/) eğitim betiklerinizi kurumsal ölçekte çalıştırmayı öğrenin.

Bu makaledeki örnek betikler,, PyTorch 'ın aktarım öğrenimi [öğreticisini](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)temel alan derin bir öğrenme sinir ağı (DNN) oluşturmak üzere tavuk ve Türkiye görüntülerini sınıflandırmak için kullanılır. Aktarım öğrenimi, bir problemi farklı ancak ilgili bir soruna çözmeyle elde edilen bilgileri uygulayan bir tekniktir. Bu işlem, sıfırdan daha az veri, saat ve işlem kaynağı isteyerek eğitim sürecini kısayollar. Aktarım öğrenimi hakkında daha fazla bilgi edinmek için [derin öğrenme vs Machine Learning](./concept-deep-learning-vs-machine-learning.md#what-is-transfer-learning) makalesine bakın.

Derin bir öğrenme PyTorch modelini baştan sona eğiyor veya mevcut bir modeli buluta çıkarksanız, elastik bulut işlem kaynakları kullanarak açık kaynaklı eğitim işlerini ölçeklendirmek için Azure Machine Learning kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modellerini oluşturabilir, dağıtabilir, sürüm ve izleyebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu kodu şu ortamlardan birinde çalıştırın:

- Azure Machine Learning işlem örneği-indirme veya yükleme gerekli değil

    - Öğreticiyi doldurun: SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için [ortamı ve çalışma alanını kurma](tutorial-1st-experiment-sdk-setup.md) .
    - Not defteri sunucusundaki örnekler derin öğrenimi klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > ml-çerçeveleri > pytorch > tren-hyperparameter-ayarla-dağıt-with-pytorch** klasörü. 
 
 - Kendi Jupyter Notebook sunucunuz
    - [Azure Machine Learning SDK 'sını](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py) (>= 1.15.0) yükler.
    - [Bir çalışma alanı yapılandırma dosyası oluşturun](how-to-configure-environment.md#workspace).
    - [Örnek betik dosyalarını indirin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch)`pytorch_train.py`
     
    Ayrıca, bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) GitHub örnekleri sayfasından bulabilirsiniz. Not defteri, akıllı hiper parametre ayarlamayı, model dağıtımını ve Not defteri pencere öğelerini kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölüm, gerekli Python paketlerini yükleyerek, bir çalışma alanı başlatarak, işlem hedefini oluşturarak ve eğitim ortamını tanımlayarak Eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içeri aktarın.

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment
from azureml.core import Environment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz [`workspace`](/python/api/azureml-core/azureml.core.workspace.workspace?preserve-view=true&view=azure-ml-py) .

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="get-the-data"></a>Verileri alma

Veri kümesi, her bir sınıf için 100 doğrulama görüntüleriyle birlikte, her biri Turkey ve tavkens için 120 eğitim görüntülerinden oluşur. Veri kümesini eğitim betiğimizin bir parçası olarak indirecek ve ayıklayacağız `pytorch_train.py` . Görüntüler, [Açık görüntüler v5 veri kümesinin](https://storage.googleapis.com/openimages/web/index.html)bir alt kümesidir.

### <a name="prepare-training-script"></a>Eğitim betiğini hazırla

Bu öğreticide, eğitim betiği `pytorch_train.py` zaten sağlanmış. Uygulamada, tüm özel eğitim komut dosyalarını olduğu gibi alabilir ve Azure Machine Learning ile çalıştırabilirsiniz.

Eğitim betiğinizin bir klasörünü oluşturun.

```python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)
shutil.copy('pytorch_train.py', project_folder)
```

### <a name="create-a-compute-target"></a>İşlem hedefi oluştur

PyTorch işinizin üzerinde çalışacağı bir işlem hedefi oluşturun. Bu örnekte, GPU özellikli Azure Machine Learning işlem kümesi oluşturun.

```Python
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

### <a name="define-your-environment"></a>Ortamınızı tanımlama

Eğitim betiğinin bağımlılıklarını kapsülleyen Azure ML [ortamını](concept-environments.md) tanımlamak için özel bir ortam tanımlayabilir veya BIR Azure ML seçkin ortamı kullanabilirsiniz.

#### <a name="use-a-curated-environment"></a>Seçkin bir ortam kullanma
Azure ML, kendi ortamınızı tanımlamak istemiyorsanız önceden oluşturulmuş, seçkin ortamlar sağlar. Azure ML 'nin, pytorch 'in farklı sürümlerine karşılık gelen ve bazı CPU ve GPU seçkin ortamları vardır. Daha fazla bilgi için [buraya](resource-curated-environments.md)bakın.

Seçkin bir ortam kullanmak istiyorsanız, bunun yerine aşağıdaki komutu çalıştırabilirsiniz:

```python
curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
```

Seçkin ortama dahil olan paketleri görmek için Conda bağımlılıklarını diske yazabilirsiniz:
```python
pytorch_env.save_to_directory(path=curated_env_name)
```

Seçkin ortamın eğitim betiğinizin gerektirdiği tüm bağımlılıkları içerdiğinden emin olun. Aksi takdirde, ortamı eksik bağımlılıkları içerecek şekilde değiştirmeniz gerekir. Ortam değiştirilirse, ' AzureML ' öneki, seçkin ortamlar için ayrıldığından, buna yeni bir ad vermeniz gerekeceğini unutmayın. Conda Dependencies YAML dosyasını değiştirdiyseniz, yeni bir adla yeni bir ortam oluşturabilirsiniz, örneğin:
```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')
```

Bunun yerine, seçkin ortam nesnesini doğrudan değiştirdiyseniz, bu ortamı yeni bir adla klonlayabilirsiniz:
```python
pytorch_env = pytorch_env.clone(new_name='pytorch-1.6-gpu')
```

#### <a name="create-a-custom-environment"></a>Özel ortam oluşturma

Ayrıca, eğitim betiğinin bağımlılıklarını kapsülleyen kendi Azure ML ortamınızı de oluşturabilirsiniz.

İlk olarak, Conda bağımlılıklarınızı bir YAML dosyasında tanımlayın; Bu örnekte, dosyanın adı `conda_dependencies.yml` .

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - future==0.17.1
  - pillow
```

Bu Conda ortam belirtiminden bir Azure ML ortamı oluşturun. Ortam, çalışma zamanında bir Docker kapsayıcısı olarak paketlenir.

Varsayılan olarak, bir temel görüntü belirtilmemişse Azure ML, temel görüntü olarak bir CPU görüntüsü kullanacaktır `azureml.core.environment.DEFAULT_CPU_IMAGE` . Bu örnek, bir GPU kümesinde eğitim yaptığından, gerekli GPU sürücüleri ve bağımlılıkları olan bir GPU temel görüntüsü belirtmeniz gerekecektir. Azure ML, Microsoft Container Registry (MCR) üzerinde yayınlanan bir temel görüntü kümesini saklar. daha fazla bilgi için bkz. [Azure/AzureML-containers](https://github.com/Azure/AzureML-Containers) GitHub deposu.

```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')

# Specify a GPU base image
pytorch_env.docker.enabled = True
pytorch_env.docker.base_image = 'mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04'
```

> [!TIP]
> İsteğe bağlı olarak, yalnızca tüm bağımlılıklarınızı doğrudan özel bir Docker görüntüsü veya Dockerfile içinde yakalayabilir ve ortamınızı bundan oluşturabilirsiniz. Daha fazla bilgi için bkz. [özel görüntüyle eğitme](how-to-train-with-custom-image.md).

Ortamları oluşturma ve kullanma hakkında daha fazla bilgi için, bkz. [Azure Machine Learning yazılım ortamları oluşturma ve kullanma](how-to-use-environments.md).

## <a name="configure-and-submit-your-training-run"></a>Eğitim çalıştırmanızı yapılandırma ve gönderme

### <a name="create-a-scriptrunconfig"></a>ScriptRunConfig oluşturma

Eğitim betiğinizi, kullanılacak ortamı ve üzerinde çalıştırılacak işlem hedefini de içeren eğitim işinizin yapılandırma ayrıntılarını belirtmek için bir [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) nesnesi oluşturun. Eğitim betiğinizdeki bağımsız değişkenler, parametresinde belirtilmişse komut satırı aracılığıyla geçirilir `arguments` . 

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_train.py',
                      arguments=['--num_epochs', 30, '--output_dir', './outputs'],
                      compute_target=compute_target,
                      environment=pytorch_env)
```

> [!WARNING]
> Azure Machine Learning, tüm kaynak dizinini kopyalayarak eğitim betikleri çalıştırır. Karşıya yüklemek istemediğiniz gizli verileriniz varsa, bir [. Ignore dosyası](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots) kullanın veya kaynak dizine eklemeyin. Bunun yerine, bir Azure ML [veri kümesi](how-to-train-with-datasets.md)kullanarak verilerinize erişin.

ScriptRunConfig ile işleri yapılandırma hakkında daha fazla bilgi için bkz. [eğitim çalıştırmalarını yapılandırma ve gönderme](how-to-set-up-training-targets.md).

> [!WARNING]
> Pytorch eğitim işlerinizi yapılandırmak için daha önce pytorch tahmin aracı 'ı kullandıysanız, 1.19.0 SDK sürümü itibariyle estimators 'ın kullanım dışı olduğuna lütfen emin olun. Azure ML SDK ile >= 1.15.0, ScriptRunConfig, derinlemesine öğrenme çerçeveleri kullanan eğitim işlerini yapılandırmak için önerilen yoldur. Yaygın geçiş soruları için bkz. [Estimator to ScriptRunConfig Migration Guide](how-to-migrate-from-estimators-to-scriptrunconfig.md).

## <a name="submit-your-run"></a>Çalıştırmanızı gönder

[Run nesnesi](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py) , iş çalışırken ve tamamlandıktan sonra çalışma geçmişi için arabirim sağlar.

```Python
run = Experiment(ws, name='Tutorial-pytorch-birds').submit(src)
run.wait_for_completion(show_output=True)
```

### <a name="what-happens-during-run-execution"></a>Çalıştırma yürütme sırasında ne olur?
Çalıştırma yürütüldüğü için aşağıdaki aşamalardan geçer:

- **Hazırlama**: bir Docker görüntüsü, tanımlanan ortama göre oluşturulur. Görüntü, çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonra çalışacak şekilde önbelleğe alınır. Günlükler, çalıştırma geçmişine de kaydedilir ve ilerlemeyi izlemek için görüntülenebilir. Bunun yerine, seçkin bir ortam belirtilmişse, bu eklenen ortamı destekleyen önbelleğe alınmış görüntü kullanılacaktır.

- **Ölçeklendirme**: Batch AI kümesi, çalışmayı yürütmek için daha fazla düğüm gerektiriyorsa, küme ölçeği büyütmeyi dener ve şu anda kullanılabilir.

- **Çalışıyor**: betik klasöründeki tüm betikler işlem hedefine yüklenir, veri depoları bağlanır veya kopyalanır ve `script` yürütülür. Stdout ve **./logs** klasörü çıkışları, çalıştırma geçmişine akışla kaydedilir ve çalıştırmayı izlemek için kullanılabilir.

- **Işlem sonrası**: çalıştırmanın **./çıktılar** klasörü, çalışma geçmişine kopyalanır.

## <a name="register-or-download-a-model"></a>Bir modeli kaydetme veya indirme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.

```Python
model = run.register_model(model_name='pytorch-birds', model_path='outputs/model.pt')
```

> [!TIP]
> Dağıtım nasıl yapılır, model kaydettirme hakkında bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan, dağıtım için doğrudan [bir işlem hedefi oluşturmaya](how-to-deploy-and-where.md#choose-a-compute-target) geçebilirsiniz.

Ayrıca, Çalıştır nesnesini kullanarak modelin yerel bir kopyasını indirebilirsiniz. Eğitim betiğinde `pytorch_train.py` , PyTorch Save nesnesi modeli yerel bir klasöre (yerel olarak işlem hedefine) devam ettirir. Bir kopyasını indirmek için Run nesnesini kullanabilirsiniz.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

# Download the model from run history
run.download_file(name='outputs/model.pt', output_file_path='./model/model.pt'), 
```

## <a name="distributed-training"></a>Dağıtılmış eğitim

Azure Machine Learning Ayrıca, eğitim iş yüklerinizi ölçeklendirebilmeniz için çok düğümlü dağıtılmış PyTorch işlerini destekler. Dağıtılmış PyTorch işlerini kolayca çalıştırabilir ve Azure ML, düzenleme işini sizin için yönetecektir.

Azure ML, hem Horovod hem de PyTorch 'ın yerleşik DistributedDataParallel modülü ile dağıtılmış PyTorch işlerinin çalıştırılmasını destekler.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) , bir açık kaynak, hepsi Uber tarafından geliştirilen dağıtılmış eğitimin çerçevesini azaltır. Eğitim için dağıtılmış PyTorch kodunu yazmak üzere kolay bir yol sunar.

Eğitim kodunuzun, dağıtılmış eğitim için Horovod ile işaretlenmiş olması gerekir. PyTorch ile Horovod kullanma hakkında daha fazla bilgi için [horovod belgelerine](https://horovod.readthedocs.io/en/stable/pytorch.html)bakın.

Ayrıca, eğitim ortamınızın **horovod** paketini içerdiğinden emin olun. PyTorch seçkin bir ortam kullanıyorsanız, horovod bağımlılıklardan biri olarak zaten dahil edilmiştir. Kendi ortamınızı kullanıyorsanız, horovod bağımlılığının eklendiğinden emin olun, örneğin:

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - horovod==0.19.5
```

Azure ML üzerinde MPı/Horovod kullanarak dağıtılmış bir işi yürütmek için, ScriptRunConfig oluşturucusunun parametresine bir [Mpicontısıdiation](/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?preserve-view=true&view=azure-ml-py) belirtmeniz gerekir `distributed_job_config` . Aşağıdaki kod, düğüm başına bir işlem çalıştıran 2 düğümlü dağıtılmış bir işi yapılandırır. Düğüm başına birden çok işlem çalıştırmak istiyorsanız (örneğin, küme SKU 'sunda birden fazla GPU varsa), ek olarak, `process_count_per_node` Mpiconation (varsayılan değer) parametresinde parametreyi de belirtin `1` .

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import MpiConfiguration

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_horovod_mnist.py',
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=MpiConfiguration(node_count=2))
```

Azure ML 'de Horovod ile dağıtılmış PyTorch çalıştırmaya ilişkin tam bir öğretici için bkz. [horovod Ile dağıtılmış pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-horovod).

### <a name="distributeddataparallel"></a>DistributedDataParallel
Eğitim kodunuzda **Torch. Distributed** Package kullanılarak oluşturulan pytorch 'ın yerleşik [distributeddataparallel](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html) modülünü kullanıyorsanız, dağıtılmış işi Azure ML aracılığıyla da başlatabilirsiniz.

Bir dağıtılmış PyTorch işini DistributedDataParallel ile çalıştırmak için, ScriptRunConfig oluşturucusunun parametresine bir [Pytorchconfiguration](/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration?preserve-view=true&view=azure-ml-py) belirtin `distributed_job_config` . Torch. Distributed için NCCL arka ucunu kullanmak istiyorsanız, `communication_backend='Nccl'` PyTorchConfiguration içinde belirtin. Aşağıdaki kod 2 düğümlü dağıtılmış bir işi yapılandıracak. NCP CL arka ucu, PyTorch dağıtılmış GPU eğitimi için önerilen arka uçta bulunur.

PyTorchConfiguration aracılığıyla yapılandırılan dağıtılmış PyTorch işleri için Azure ML, işlem hedefinin düğümlerinde aşağıdaki ortam değişkenlerini ayarlar:

* `AZ_BATCHAI_PYTORCH_INIT_METHOD`: İşlem grubunun paylaşılan dosya sistemi başlatması URL 'SI
* `AZ_BATCHAI_TASK_INDEX`: çalışan işlemin küresel sıralaması

Bu ortam değişkenlerini, ScriptRunConfig parametresi aracılığıyla eğitim betiğinin karşılık gelen bağımsız değişkenlerine belirtebilirsiniz `arguments` .

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import PyTorchConfiguration

args = ['--dist-backend', 'nccl',
        '--dist-url', '$AZ_BATCHAI_PYTORCH_INIT_METHOD',
        '--rank', '$AZ_BATCHAI_TASK_INDEX',
        '--world-size', 2]

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_mnist.py',
                      arguments=args,
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=PyTorchConfiguration(communication_backend='Nccl', node_count=2))
```

Bunun yerine, dağıtılmış eğitim için gloo arka ucunu kullanmak istiyorsanız, `communication_backend='Gloo'` bunun yerine belirtin. Dağıtılmış CPU eğitimi için gloo arka ucu önerilir.

Azure ML 'de dağıtılmış PyTorch çalıştırmaya ilişkin tam bir öğretici için bkz. [DistributedDataParallel Ile dağıtılmış pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-nccl-gloo).

### <a name="troubleshooting"></a>Sorun giderme

* **Horovod kapatılmış**: çoğu durumda, "AbortedError: Horovod kapatılmış" olarak karşılaşırsanız, Horovod 'nin kapatılmasına neden olan işlemlerden birinde temel alınan bir özel durum vardı. MPI işindeki her derecelendirmeye Azure ML'de özel bir günlük dosyası ayrılır. Bu günlükler `70_driver_logs` olarak adlandırılır. Dağıtılmış eğitim söz konusu olduğunda, günlüklerin ayırt edilmesini kolaylaştırmak için günlük adlarında `_rank` soneki kullanılır. Horovod 'nin kapatılmasına neden olan hatayı tam olarak bulmak için, tüm günlük dosyalarını `Traceback` inceleyin ve driver_log dosyalarının sonundaki bölümüne bakın. Bu dosyalardan biri size gerçek temel özel durumu verecektir. 

## <a name="export-to-onnx"></a>ONNX 'e aktar

[Onnx çalışma zamanıyla](concept-onnx.md)çıkarımı iyileştirmek için, eğitimli PyTorch MODELINIZI onnx biçimine dönüştürün. Çıkarım veya model Puanlama, dağıtılan modelin tahmin için en yaygın olarak üretim verilerinde kullanıldığı aşamadır. Örnek için [öğreticiye](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Machine Learning PyTorch kullanarak derin bir öğrenme, sinir ağı eğitildiniz ve kaydettiniz. Modeli dağıtmayı öğrenmek için model dağıtım makalemize devam edin.

* [Modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)
* [Eğitim sırasında çalıştırma ölçümlerini izleyin](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
* [Azure 'da dağıtılmış derin öğrenme eğitimi için başvuru mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)
