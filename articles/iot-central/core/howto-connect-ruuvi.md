---
title: Azure IoT Central bir RuuviTag bağlantısı | Microsoft Docs
description: IoT Central uygulamanıza bir RuuviTag ortam algılayıcısı bağlamayı öğrenin.
services: iot-central
ms.service: iot-central
ms.topic: how-to
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 11/27/2019
ms.openlocfilehash: 7cf0d31895eaeaa41c23f9f07664b27bed1d3126
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96006396"
---
# <a name="connect-a-ruuvitag-sensor-to-your-azure-iot-central-application"></a>Azure IoT Central uygulamanıza bir RuuviTag algılayıcısı bağlama

*Bu makale, çözüm oluşturucuları için geçerlidir.*

Bu makalede, bir çözüm Oluşturucusu olarak, bir RuuviTag algılayıcısı Microsoft Azure IoT Central uygulamanıza nasıl bağlayabileceğinizi açıklanmaktadır.

Ruuvi etiketi nedir?

RuuviTag, iş müşterilerinin, geliştiricilerin, mekanizmalarının, öğrencilerin ve barındırmacıların ihtiyaçlarını karşılamak için tasarlanan, gelişmiş bir açık kaynaklı algılayıcı işaret platformudur. Cihaz, kendi kutudan çıkar almaz ve ihtiyacınız olan yere dağıtmanız için hazır hale gelir. Bu, bir ortam algılayıcısı ve yerleşik hızlandırma içeren bir Bluetooth LE işaret ediyor.

RuuviTag, BLE üzerinden iletişim kurar (Bluetooth düşük enerji) ve Azure IoT Central ile iletişim kurmak için bir ağ geçidi cihazı gerektirir. IoT Central bağlantı kurmak üzere bir RuuviTag 'i etkinleştirmek için Rigado Cascade 500 gibi bir ağ geçidi cihazına sahip olduğunuzdan emin olun.

Bir Rigado basamaklı 500 ağ geçidi cihazı kurmak istiyorsanız lütfen [buradaki yönergeleri](./howto-connect-rigado-cascade-500.md) izleyin.

## <a name="prerequisites"></a>Önkoşullar

RuuviTag sensörlerinden bağlantı kurmak için aşağıdaki kaynaklara ihtiyacınız vardır:

* RuuviTag algılayıcısı. Daha fazla bilgi için lütfen [Ruuvitag](https://ruuvi.com/)adresini ziyaret edin.
* Bir Rigado Cascade 500 cihazı veya başka bir BLE ağ geçidi. Daha fazla bilgi için lütfen [Rigado](https://www.rigado.com/)adresini ziyaret edin.
* Azure IoT Central uygulaması. Daha fazla bilgi için [Yeni uygulama oluşturma](./quick-deploy-iot-central.md)bölümüne bakın.

## <a name="add-a-ruuvitag-device-template"></a>RuuviTag cihaz şablonu ekleme

Azure IoT Central uygulama örneğinize bir RuuviTag algılayıcısı eklemek için, uygulamanız içinde karşılık gelen bir cihaz şablonunu yapılandırmanız gerekir.

Bir RuuviTag cihaz şablonu eklemek için:

1. Sol bölmedeki ***cihaz şablonları** _ sekmesine gidin, _* + Yeni * *: ![ yeni cihaz şablonu oluştur ](./media/howto-connect-ruuvi/devicetemplate-new.png) sayfasında, **_özel şablon oluşturma_*_ veya _*_önceden yapılandırılmış bir cihaz şablonu kullanma_*_ seçeneği sunulur.
1. Aşağıda gösterildiği gibi önceden yapılandırılmış cihaz şablonları listesinden RuuviTag cihaz şablonunu seçin:  ![ RuuviTag cihaz şablonunu seçin](./media/howto-connect-ruuvi/devicetemplate-preconfigured.png)
1. Sonraki adıma devam etmek için Ileri 'yi seçin _*_: Özelleştir_*_ .
1. Bir sonraki ekranda, C500 cihaz şablonunu IoT Central uygulamanıza eklemek için _*_Oluştur_*_ ' u seçin.

## <a name="connect-a-ruuvitag-sensor"></a>RuuviTag algılayıcısı bağlama

Daha önce belirtildiği gibi, RuuviTag ' i IoT Central uygulamanızla bağlamak için bir ağ geçidi cihazı ayarlamanız gerekir. Aşağıdaki adımlarda, bir Rigado basamaklı 500 ağ geçidi cihazı ayarlamış olduğunuz varsayılmaktadır.  

1. Rigado Cascade 500 cihazınızda güç yapın ve ağ bağlantınıza bağlayın (Ethernet veya kablosuz aracılığıyla)
1. RuuviTag 'in kapağını kapatın ve pille bağlantıyı güvenli hale getirmek için plastik sekmesini çekin.
1. IoT Central uygulamanızda zaten yapılandırılmış bir Rigado basamaklı 500 ağ geçidine RuuviTag Close koyun.
1. Yalnızca birkaç saniye içinde, RuuviTag 'niz IoT Central içindeki cihazlar listenizde görünmelidir.  
    ![RuuviTag cihaz listesi](./media/howto-connect-ruuvi/ruuvi-devicelist.png)

Artık IoT Central uygulamanızda bu RuuviTag ' i kullanabilirsiniz.  

## <a name="create-a-simulated-ruuvitag"></a>Benzetimli RuuviTag oluştur

Fiziksel bir RuuviTag cihazınız yoksa, Azure IoT Central uygulamanızda test için kullanılacak bir benzetimli Ruuvıtag algılayıcısı oluşturabilirsiniz.

Benzetimli RuuviTag oluşturmak için:

1. _ * Cihaz > RuuviTag * * öğesini seçin.
1. **+ Yeni** seçeneğini belirleyin.
1. Benzersiz bir **CIHAZ kimliği** ve kolay bir **Cihaz adı** belirtin.  
1. **Benzetimli** ayarı etkinleştirin.
1. **Oluştur**’u seçin.  

## <a name="next-steps"></a>Sonraki Adımlar

Bir cihaz geliştiricisiyseniz, önerilen bazı sonraki adımlar şunlardır:

- [Azure IoT Central cihaz bağlantısı](./concepts-get-connected.md) hakkında bilgi edinin
- [Azure CLI kullanarak cihaz bağlantısını izlemeyi](./howto-monitor-devices-azure-cli.md) öğrenin
