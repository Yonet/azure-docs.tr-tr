---
title: Güvenlik Aracısı kimlik doğrulaması (Önizleme)
titleSuffix: Azure Defender for IoT
description: Mikro aracı kimlik doğrulamasını iki olası yöntemle gerçekleştirin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/20/2021
ms.topic: conceptual
ms.service: azure
ms.openlocfilehash: 018da32b90c7730f82eaa5aa2cd2b5c7a64719a6
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809986"
---
# <a name="micro-agent-authentication-methods-preview"></a>Mikro aracı kimlik doğrulama yöntemleri (Önizleme)

IoT mikro Aracısı için Defender ile kimlik doğrulama için iki seçenek vardır: 

- Bağlantı dizesi 

- Sertifika 

## <a name="authentication-using-a-connection-string"></a>Bağlantı dizesi kullanarak kimlik doğrulama 

Bir bağlantı dizesi kullanmak için, adlı bir dosyada, Defender aracı dizininde UTF-8 ile kodlanmış bağlantı dizesini kullanan bir dosya eklemeniz gerekir `connection_string.txt` . Örneğin,

```azurecli
echo “<connection string>” > connection_string.txt 
```

Bunu yaptıktan sonra bu komutu kullanarak hizmeti yeniden başlatmanız gerekir.

```azurecli
sudo systemctl restart defender-iot-micro-agent.service
``` 

## <a name="authentication-using-a-certificate"></a>Sertifika kullanarak kimlik doğrulama 


Sertifika kullanarak kimlik doğrulaması gerçekleştirmek için: 

1. Bir sertifikanın PEMENCODED ortak bölümünü, adlı bir dosyaya Defender Agent dizinine yerleştirin `certificate_public.pem` .
1. PEK kodlu özel anahtarı, adlı bir dosyaya Defender Agent dizinine yerleştirin `certificate_private.pem` .
1. Uygun bağlantı dizesini adlı bir dosyaya yerleştirin `connection_string.txt` . Örneğin,

    ```azurecli
    HostName=<the host name of the iot hub>;DeviceId=<the id of the device>;ModuleId=<the id of the module>;x509=true 
    ```

    Bu eylem, Defender aracısının kimlik doğrulaması için bir sertifikanın sağlanmasını beklediğini gösterir. 

1. Aşağıdaki kodu kullanarak hizmeti yeniden başlatın: 

    ```azurecli
    sudo systemctl restart defender-iot-micro-agent.service 
    ```

## <a name="ensure-the-micro-agent-is-running-correctly"></a>Mikro aracının doğru çalıştığından emin olun 

1. Şu komutu çalıştırın: 
    ```azurecli
    systemctl status defender-iot-micro-agent.service 
    ```
1. **Etkin** olduğundan ve işlemin çalışma süresinin uygun olduğundan emin olmak için hizmetin kararlı olup olmadığını denetleyin: 

    :::image type="content" source="media/concept-security-agent-authentication/active.png" alt-text="Etkin olduğundan emin olmak için hizmetinizin kararlı olduğundan emin olun.":::

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik duruşunu – CIS kıyaslaması](concept-security-posture.md)' nı kontrol edin.
