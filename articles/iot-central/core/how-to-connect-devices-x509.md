---
title: Azure IoT Central uygulamasında cihazları X. 509.440 sertifikalarıyla bağlama
description: IoT Central uygulama için Node.js cihaz SDK 'sını kullanarak X. 509.440 sertifikalarıyla cihazları bağlama
author: dominicbetts
ms.author: dobett
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: device-developer
ms.openlocfilehash: bffff099e8df2b944cbef50a074ef625267ed238
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944635"
---
# <a name="how-to-connect-devices-with-x509-certificates-using-nodejs-device-sdk-for-iot-central-application"></a>IoT Central uygulama için Node.js cihaz SDK 'sını kullanarak X. 509.440 sertifikalarıyla cihazları bağlama

IoT Central, bir cihaz ve uygulamanız arasındaki iletişimin güvenliğini sağlamak için hem paylaşılan erişim imzalarını (SAS) hem de X. 509.440 sertifikalarını destekler. [Azure IoT Central uygulama öğreticisine bir istemci uygulaması oluşturma ve bağlama](./tutorial-connect-device.md) ÖĞRETICISINE SAS kullanır. Bu makalede, kod örneğini X. 509.440 kullanacak şekilde nasıl değiştireceğiniz hakkında bilgi edineceksiniz.  X. 509.440 sertifikaları üretim ortamlarında önerilir. Daha fazla bilgi için bkz. [Azure IoT Central 'ye bağlanma](./concepts-get-connected.md).

Bu makalede, genellikle üretim ortamında kullanılan X. 509.440- [Group](how-to-connect-devices-x509.md#use-a-group-enrollment) kayıtlarını kullanmanın iki yolu ve [bireysel](how-to-connect-devices-x509.md#use-an-individual-enrollment) kayıtlar test için yararlıdır.

## <a name="prerequisites"></a>Önkoşullar

- [Bir istemci uygulamasını oluşturma ve Azure IoT Central uygulamanızın (JavaScript)](./tutorial-connect-device.md) öğreticisine bağlama işleminin tamamlanması.
- [Git](https://git-scm.com/download/).
- [OpenSSL](https://www.openssl.org/)indirin ve yükleyin. Windows kullanıyorsanız, [SourceForge üzerindeki OpenSSL sayfasından](https://sourceforge.net/projects/openssl/)ikili dosyaları kullanabilirsiniz.

## <a name="use-a-group-enrollment"></a>Grup kaydı kullanma

Bir üretim ortamında Grup kaydı ile X. 509.440 sertifikaları kullanın. Bir grup kaydında, IoT Central uygulamanıza bir kök veya ara X. 509.440 sertifikası eklersiniz. Kök veya ara sertifikadan türetilmiş yaprak sertifikalara sahip cihazlar uygulamanıza bağlanabilir.

## <a name="generate-root-and-device-cert"></a>Kök ve cihaz sertifikası oluştur

Bu bölümde, bir cihazı kayıt grubunun sertifikasından türetilmiş bir sertifikaya bağlamak için bir X. 509.952 sertifikası kullanırsınız ve bu, IoT Central uygulamanıza bağlanabilir.

> [!WARNING]
> X. 509.440 sertifikalarını oluşturmanın bu yolu yalnızca test amaçlıdır. Bir üretim ortamında, sertifika oluşturma için resmi, güvenli mekanizmanızı kullanmanız gerekir.

1. Bir komut istemi açın. Sertifika oluşturma betikleri için GitHub deposunu kopyalayın:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node.git
    ```

1. Sertifika Oluşturucu betiğine gidin ve gerekli paketleri yüklemek için:

    ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    npm install
    ```

1. Bir kök sertifika oluşturun ve betiği çalıştırarak bir cihaz sertifikası türetirsiniz:

    ```cmd/sh
    node create_test_cert.js root mytestrootcert
    node create_test_cert.js device sample-device-01 mytestrootcert
    ```

    > [!TIP]
    > Bir cihaz KIMLIĞI harfler, rakamlar ve `-` karakteri içerebilir.

Bu komutlar, her biri kök ve cihaz sertifikası için üç dosya üretir

filename | dekiler
-------- | --------
\<name\>_cert. pek | X509 sertifikasının genel bölümü
\<name\>_key. pek | X509 sertifikası için özel anahtar
\<name\>_fullchain. pek | X509 sertifikası için tüm Anahtarlık.

## <a name="create-a-group-enrollment"></a>Grup kaydı oluşturma

1. IoT Central uygulamanızı açın ve sol bölmedeki **Yönetim**  ' e gidin ve **cihaz bağlantısı**' nı seçin.

1. **+ Kayıt grubu oluştur**' u seçin ve _MyX509Group_ adlı yeni bir kayıt grubu oluşturarak sertifika kanıtlama türü **(X. 509.440)** yazın.

1. Oluşturduğunuz kayıt grubunu açın ve **birincil Yönet**' i seçin.

1. Dosya seçeneğini belirleyin ve daha önce oluşturduğunuz _mytestrootcert_cert. ped_ adlı kök sertifika dosyasını karşıya yükleyin:

    ![Sertifika karşıya yükleme](./media/how-to-connect-devices-x509/certificate-upload.png)

1. Doğrulamayı tamamlayıp doğrulama kodunu oluşturun, kopyalayın ve ardından komut isteminde bir X. 509.952 doğrulama sertifikası oluşturmak için kullanın:

    ```cmd/sh
    node create_test_cert.js verification --ca mytestrootcert_cert.pem --key mytestrootcert_key.pem --nonce  {verification-code}
    ```

1. Doğrulamayı tamamlamaya yönelik _verification_cert. ped_ imzalı doğrulama sertifikasını karşıya yükleyin:

    ![Doğrulanan sertifika](./media/how-to-connect-devices-x509/verified.png)

Artık bu birincil kök sertifikadan türetilmiş bir X. 509.440 sertifikası olan cihazları bağlayabilirsiniz.

Kayıt grubunu kaydettikten sonra, KIMLIK kapsamını bir yere getirin.

## <a name="run-sample-device-code"></a>Örnek cihaz kodunu Çalıştır

1. **SampleDevice01_key. pek** ve **sampleDevice01_cert. pea** dosyalarını, **simple_thermostat.js** uygulamasını içeren _Azure-IoT-SDK-node/Device/Samples/PNP_ klasörüne kopyalayın. Bu uygulamayı [cihaz bağlama (JavaScript) öğreticisini](./tutorial-connect-device.md)tamamladığınızda kullandınız.

1. **simple_thermostat.js** uygulamasını içeren _Azure-IoT-SDK-node/Device/Samples/PNP_ klasörüne gidin ve X. 509.440 paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-security-x509 --save
    ```

1. **simple_thermostat.js** dosyasını bir metin düzenleyicisinde açın.

1. Deyimlerini, `require` aşağıdakileri içerecek şekilde düzenleyin:

    ```javascript
    const fs = require('fs');
    const X509Security = require('azure-iot-security-x509').X509Security;
    ```

1. Değişkeni başlatmak için aşağıdaki dört satırı "DPS bağlantı bilgileri" bölümüne ekleyin `deviceCert` :

    ```javascript
    const deviceCert = {
      cert: fs.readFileSync(process.env.IOTHUB_DEVICE_X509_CERT).toString(),
      key: fs.readFileSync(process.env.IOTHUB_DEVICE_X509_KEY).toString()
    };
    ```

1. `provisionDevice`İlk satırı aşağıdaki ile değiştirerek istemciyi oluşturan işlevi düzenleyin:

    ```javascript
    var provSecurityClient = new X509Security(registrationId, deviceCert);
    ```

1. Aynı işlevde, değişkeni ayarlayan satırı `deviceConnectionString` aşağıdaki gibi değiştirin:

    ```javascript
    deviceConnectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';x509=true';
    ```

1. `main`İşlevinde, öğesini çağıran satırdan sonra aşağıdaki satırı ekleyin `Client.fromConnectionString` :

    ```javascript
    client.setOptions(deviceCert);
    ```

1. Kabuk ortamınızda aşağıdaki iki ortam değişkenini ayarlayın:

    ```cmd/sh
    set IOTHUB_DEVICE_X509_CERT=sampleDevice01_cert.pem
    set IOTHUB_DEVICE_X509_KEY=sampleDevice01_key.pem
    ```

    > [!TIP]
    > [İstemci uygulamasını oluşturma ve Azure IoT Central uygulamanızın öğreticisine bağlama öğreticinizi tamamladıktan sonra](./tutorial-connect-device.md) diğer gerekli ortam değişkenlerini ayarlarsınız.

1. Betiği yürütün ve cihazın başarıyla sağlandığını doğrulayın:

    ```cmd/sh
    node simple_thermostat.js
    ```

    Ayrıca, telemetrinin panoda göründüğünü doğrulayabilirsiniz.

    ![Cihaz telemetrisini doğrulama](./media/how-to-connect-devices-x509/telemetry.png)

## <a name="use-an-individual-enrollment"></a>Bireysel kayıt kullanma

Cihazınızı ve çözümünüzü test etmek için bireysel kayıt ile X. 509.440 sertifikalarını kullanın. Tek bir kayıtta, IoT Central uygulamanızda kök veya ara X. 509.440 sertifikası yoktur. Cihazlar, uygulamanıza bağlanmak için otomatik olarak imzalanan bir X. 509.440 sertifikası kullanır.

## <a name="generate-self-signed-device-cert"></a>Otomatik olarak imzalanan cihaz sertifikası oluştur

Bu bölümde, tek bir cihazı kaydetmek için kullanılan ayrı kayıt için cihazları bağlamak üzere otomatik olarak imzalanan bir X. 509.952 sertifikası kullanırsınız. Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır.

Betiği çalıştırarak kendinden imzalı bir X. 509.952 cihaz sertifikası oluşturun. Sertifika adı için yalnızca küçük harfli alfasayısal karakterler ve kısa çizgiler kullandığınızdan emin olun:

  ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    node create_test_cert.js device mytestselfcertprimary
    node create_test_cert.js device mytestselfcertsecondary 
  ```

## <a name="create-individual-enrollment"></a>Bireysel kayıt oluştur

1. Azure IoT Central uygulamasında **cihazlar**' ı seçin ve **cihaz kimliğine** sahip yeni bir cihazı, termostat cihaz şablonundan _mytestselfcertprimary_ olarak oluşturun. **Kimlik kapsamını** bir yere getirin, daha sonra kullanırsınız.

1. Oluşturduğunuz cihazı açın ve **Bağlan**' ı seçin.

1. **Bağlama yöntemi** olarak **bireysel** kayıtları ve **(X. 509.440) sertifikaları** seçin:

    ![Bireysel kayıt](./media/how-to-connect-devices-x509/individual-device-connect.png)

1. Birincil altında dosya seçeneği ' ni seçin ve daha önce oluşturduğunuz _mytestselfcertprimary_cert. ped_ adlı sertifika dosyasını karşıya yükleyin.

1. İkincil sertifika için dosya seçeneğini belirleyin ve _mytestselfcertsecondary_cert. ped_ adlı sertifika dosyasını karşıya yükleyin. Sonra **Kaydet**' i seçin:

    ![Ayrı kayıt sertifikasını karşıya yükleme](./media/how-to-connect-devices-x509/individual-enrollment.png)

Cihaz artık X. 509.440 sertifikası ile sağlanıyor.

## <a name="run-a-sample-individual-enrollment-device"></a>Örnek bir bireysel kayıt cihazı çalıştırma

1. _Mytestselfcertprimary_key. pek_ ve _mytestselfcertprimary_cert. pea_ dosyalarını, **simple_thermostat.js** uygulamasını içeren _Azure-IoT-SDK-node/Device/Samples/PNP_ klasörüne kopyalayın. Bu uygulamayı [cihaz bağlama (JavaScript) öğreticisini](./tutorial-connect-device.md)tamamladığınızda kullandınız.

1. Yukarıdaki örnekte kullandığınız ortam değişkenlerini aşağıdaki şekilde değiştirin:

    ```cmd/sh
    set IOTHUB_DEVICE_DPS_DEVICE_ID=mytestselfcertprimary
    set IOTHUB_DEVICE_X509_CERT=mytestselfcertprimary_cert.pem
    set IOTHUB_DEVICE_X509_KEY=mytestselfcertprimary_key.pem
    ```

1. Betiği yürütün ve cihazın başarıyla sağlandığını doğrulayın:

    ```cmd/sh
    node environmentalSensor.js
    ```

    Ayrıca, telemetrinin panoda göründüğünü doğrulayabilirsiniz.

    ![Telemetri bireysel kaydı](./media/how-to-connect-devices-x509/telemetry-primary.png)

Yukarıdaki adımları _mytestselfcertsecondary_ sertifikası için de yineleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık X. 509.440 sertifikalarını kullanarak cihazları nasıl bağlayacağınızı öğrendiğinize göre, önerilen sonraki adım [Azure CLI kullanarak cihaz bağlantısını izlemeyi](howto-monitor-devices-azure-cli.md) öğrenmektedir
