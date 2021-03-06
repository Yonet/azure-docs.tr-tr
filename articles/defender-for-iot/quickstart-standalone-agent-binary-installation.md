---
title: IoT mikro Aracısı (Önizleme) için Defender 'ı yükler
titleSuffix: Azure Defender for IoT
description: Defender Micro Agent 'ı yüklemeyi ve kimlik doğrulamasını öğrenin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/18/2021
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: 0841bbd8baa524d3eea3afcbffc0aa5ead41409e
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99810195"
---
# <a name="install-defender-for-iot-micro-agent-preview"></a>IoT mikro Aracısı (Önizleme) için Defender 'ı yükler

Bu makalede, Defender mikro aracısının nasıl yükleneceğine ve kimlik doğrulamasının nasıl yapılacağı hakkında bir açıklama sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

IoT modülü için Defender 'ı yüklemeden önce IoT Hub bir modül kimliği oluşturmanız gerekir. Modül kimliği oluşturma hakkında daha fazla bilgi için bkz. Create a [azureiotsecurity Module ikizi](quickstart-create-security-twin.md)

## <a name="install-the-package"></a>Paketi yükler

[Bu yönergeleri](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software)izleyerek Microsoft paket deposunu yükleyip yapılandırın. 

DEMIN 9 ' da, yönergeler eklenmesi gereken depoyu içermez, depoyu eklemek için aşağıdaki komutları kullanın: 

```azurecli
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add - 

sudo apt-get install software-properties-common

sudo apt-add-repository https://packages.microsoft.com/debian/9/multiarch/prod

sudo apt-get update
```

Defender Micro Agent paketini debir ve Ubuntu tabanlı Linux dağıtımlarına yüklemek için aşağıdaki komutu kullanın:

```azurecli
sudo apt-get install defender-iot-micro-agent 
```

## <a name="micro-agent-authentication-methods"></a>Mikro aracı kimlik doğrulama yöntemleri 

IoT mikro aracısının Defender kimliğini doğrulamak için kullanılan iki seçenek şunlardır: 

- Bağlantı dizesi. 

- Sertifika.

### <a name="authenticate-using-a-connection-string"></a>Bağlantı dizesi kullanarak kimlik doğrulama

Bir bağlantı dizesi kullanarak kimlik doğrulaması yapmak için:

1. `connection_string.txt`Aşağıdaki komutu girerek Defender aracı dizin yolunda UTF-8 ile kodlanmış bağlantı dizesini içeren adlı bir dosya yerleştirin `/var/defender_iot_micro_agent` :

    ```azurecli
    sudo bash -c 'echo "<connection string" > /var/defender_iot_micro_agent/connection_string.txt' 
    ```

    `connection_string.txt`Şimdi aşağıdaki yol konumunda bulunmalıdır `/var/defender_iot_micro_agent/connection_string.txt` .

1. Bu komutu kullanarak hizmeti yeniden başlatın:  

    ```azurecli
    sudo systemctl restart defender-iot-micro-agent.service 
    ```

### <a name="authenticate-using-a-certificate"></a>Sertifika kullanarak kimlik doğrulama

Sertifika kullanarak kimlik doğrulaması yapmak için:

1. [Bu yönergeleri](../iot-hub/iot-hub-security-x509-get-started.md)izleyerek bir sertifikayı temin edin.

1. Sertifikanın PEMENCODED ortak kısmını ve özel anahtarı, içindeki Defender Agent dizinine ve içindeki adlı dosyasına yerleştirin `certificate_public.pem` `certificate_private.pem` . 

1. İlgili bağlantı dizesini `connection_string.txt` dosyasına yerleştirin. bağlantı dizesi şuna benzemelidir: 

    `HostName=<the host name of the iot hub>;DeviceId=<the id of the device>;ModuleId=<the id of the module>;x509=true` 

    Bu dize, kimlik doğrulaması için bir sertifikanın sağlanması beklendiğinde Defender Agent 'ı uyarır. 

1. Aşağıdaki komutu kullanarak hizmeti yeniden başlatın:  

    ```azurecli
    sudo systemctl restart defender-iot-micro-agent.service
    ```

### <a name="validate-your-installation"></a>Yüklemenizi doğrulama

Yüklemenizi doğrulamak için:

1. Mikro aracısının aşağıdaki komutla düzgün çalıştığından emin olun:  

    ```azurecli
    systemctl status defender-iot-micro-agent.service
    ```
1. Hizmetin kararlı olduğundan `active` ve işlemin çalışma süresinin uygun olduğundan emin olun

    :::image type="content" source="media/quickstart-standalone-agent-binary-installation/active-running.png" alt-text="Hizmetinizin kararlı ve etkin olduğundan emin olun.":::
 
## <a name="testing-the-system-end-to-end"></a>Sistemi uçtan uca test etme 

Cihazda bir tetikleyici dosyası oluşturarak sistemi uçtan uca test edebilirsiniz. Tetikleyici dosyası, aracıdaki ana hat taramasının dosyayı temel ihlal olarak algılamasına neden olur. 

Aşağıdaki komutla dosya sisteminde bir dosya oluşturun:

```azurecli
sudo touch /tmp/DefenderForIoTOSBaselineTrigger.txt 
```
Hub 'da bir temel doğrulama hatası önerisi, `CceId` CIS-de,-9-DEFENDER_FOR_IOT_TEST_CHECKS-0,0 ile gerçekleşir: 

:::image type="content" source="media/quickstart-standalone-agent-binary-installation/validation-failure.png" alt-text="Hub 'da oluşan taban çizgisi doğrulama hatası önerisi." lightbox="media/quickstart-standalone-agent-binary-installation/validation-failure-expanded.png":::

Önerinin hub 'da görünmesi için bir saate kadar bekleyin. 

## <a name="micro-agent-versioning"></a>Mikro aracı sürümü oluşturma 

Defender IoT mikro aracısının belirli bir sürümünü yüklemek için şu komutu çalıştırın: 

```azurecli
sudo apt-get install defender-iot-micro-agent=<version>
```

## <a name="next-steps"></a>Sonraki adımlar

[Kaynak kodundan Defender Micro Agent oluşturma](quickstart-building-the-defender-micro-agent-from-source.md)
