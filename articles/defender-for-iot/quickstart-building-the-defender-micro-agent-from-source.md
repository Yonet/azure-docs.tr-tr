---
title: Kaynak koddan Defender Micro Agent 'ı oluşturma (Önizleme)
titleSuffix: Azure Defender for IoT
description: Mikro Aracı, dağıtımınızı özelleştirmek için kullanılabilecek bir altyapı içerir.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/18/2021
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: 782c14c6feee56c4b19d5a234cb41186d51decb2
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99810179"
---
# <a name="build-the-defender-micro-agent-from-source-code"></a>Kaynak koddan Defender Micro Agent 'ı oluşturma

Mikro Aracı, dağıtımınızı özelleştirmek için kullanılabilen bir altyapıyı içerir. Kullanılabilir yapılandırma parametrelerinin bir listesini görmek için `configs/LINUX_BASE.conf` dosyasına bakın.

Tek bir dağıtım için, temel dosyayı değiştirin `.conf` . 

Birden fazla dağıtıma ihtiyacınız varsa, temel yapılandırmadan kalıtımla alabilir ve değerlerini geçersiz kılabilirsiniz. 

Değerleri geçersiz kılmak için:

1. Yeni bir `.dist` dosya oluşturun.

1. `CONF_DEFINE_BASE(${g_plat_config_path} LINUX_BASE.conf)`En üste ekleyin.
 
1. İhtiyaç duyduğunuz her şey için yeni değerler tanımlayın, örnek: 

    `set(ASC_LOW_PRIORITY_INTERVAL 60*60*24)` 

1. `.dist`Oluştururken dosyaya bir başvuru verin. Örneğin, 

    `cmake -DCMAKE_BUILD_TYPE=Debug -Dlog_level=DEBUG -Dlog_level_cmdline:BOOL=ON -DIOT_SECURITY_MODULE_DIST_TARGET=UBUNTU1804 ..` 

## <a name="baseline-configuration-signing"></a>Taban çizgisi yapılandırma imzası 

Aracı, varsayılan olarak, değişiklik yapılmasını azaltmak için diske yerleştirilmiş yapılandırma dosyalarının orijinalliğini doğrular.

Önişlemci bayrağını tanımlayarak bu işlemi durdurabilirsiniz `ASC_BASELINE_CONF_SIGN_CHECK_DISABLE` .

Üretim ortamları için imza denetimini kapatmayı önermiyoruz. 

Üretim senaryolarında farklı bir yapılandırmaya ihtiyacınız varsa IoT ekibi için Defender ile görüşün. 

## <a name="prerequisites"></a>Önkoşullar 

1. IoT kaynak kodu için Defender 'a erişim istemek için hesap yöneticinize başvurun.
 
1. Kaynak kodu kopyalayın veya diskteki bir klasöre ayıklayın.

1. Bu dizine gidin.

1. Aşağıdaki kodu kullanarak alt modülleri çekin:

    ```bash
    git submodule update --init
    ```
    
1. Ardından, Azure IoT SDK için alt modülleri aşağıdaki kodla çekin: 

    ```bash
    git -C deps/azure-iot-sdk-c/ submodule update –init
    ```
 

1. Yürütme izni ekleyin ve geliştirici ortamı kurulum betiğini çalıştırın:

    ```bash
    chmod +x scripts/install_development_environment.sh && ./scripts/install_development_environment.sh 
    ```

1. Yapı çıktıları için bir dizin oluşturun: 

    ```bash
    mkdir cmake 
    ```

1. Seçim [Vscode](https://code.visualstudio.com/download ) indirme ve yükleme 

1. Seçim VSCode için [C/C++ uzantısını](https://code.visualstudio.com/docs/languages/cpp ) yükler.

## <a name="building-the-defender-iot-micro-agent"></a>Defender IoT mikro Aracısı oluşturma 

1. Dizini VSCode ile açın 

1. `cmake`Dizinine gidin. 

1. Aşağıdaki komutu çalıştırın: 

    ```bash
    cmake -DCMAKE_BUILD_TYPE=Debug -Dlog_level=DEBUG -Dlog_level_cmdline:BOOL=ON -DIOT_SECURITY_MODULE_DIST_TARGET<the appropriate distro configuration file name> .. 
    
    cmake --build . -- -j${env:NPROC}
    ```

    Örnek: 

    ```bash
    cmake -DCMAKE_BUILD_TYPE=Debug -Dlog_level=DEBUG -Dlog_level_cmdline:BOOL=ON -DIOT_SECURITY_MODULE_DIST_TARGETUBUNTU1804 ..
    
    cmake --build . -- -j${env:NPROC}
    ```

## <a name="next-steps"></a>Sonraki adımlar

[IoT Için Azure Defender çözümünüzü yapılandırın](quickstart-configure-your-solution.md).
