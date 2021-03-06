---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: d4e8d99cd7c67136f359772664eb017c6207e6e4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188314"
---
### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>Sanal makine için TCP uç noktası oluşturma
SQL Server’a İnternet’ten erişmek için, sanal makinenin gelen TCP iletişimi dinleyeceği bir uç nokta olmalıdır. Bu Azure yapılandırma adımı, gelen TCP bağlantı noktası trafiğini sanal makinenin erişebildiği bir TCP bağlantı noktasına yönlendirir.

> [!NOTE]
> Aynı bulut hizmeti veya sanal ağ içinde bağlanıyorsanız, genel olarak erişilebilen bir uç nokta oluşturmanız gerekmez. Bu durumda, sonraki adımdan devam edebilirsiniz. Daha fazla bilgi için bkz. [Bağlantı Senaryoları](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. Azure Portal’da **Sanal makineler (klasik)** öğesini seçin.
2. Ardından SQL Server sanal makinenizi seçin.
3. **Uç noktalar**’ı seçin ve sonra da Uç Noktalar dikey penceresinin en üstündeki **Ekle** düğmesine tıklayın.
   
    ![Uç Nokta Oluşturmak için İzlenecek Portal Adımları](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. **Uç Nokta Ekle** dikey penceresinde, SQLEndpoint gibi bir **Ad** sağlayın.
5. **Protokol** olarak **TCP**’yi seçin.
6. **Genel bağlantı noktası** olarak **57500** gibi bir bağlantı noktası numarası belirtin.
7. **Özel bağlantı noktası** olarak, SQL Server'ın varsayılan değeri **1433** olan dinleme bağlantı noktasını belirtin.
8. **Tamam**'a tıklayarak uç noktayı oluşturun.

