---
title: Azure portal kullanarak bir Azure dış yük dengeleyiciyi başka bir Azure bölgesine taşıyın
description: Azure portal kullanarak bir Azure bölgesinden diğerine dış yük dengeleyici taşımak için Azure Resource Manager şablonu kullanın.
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: f83ff3d1d03354daef3466c1f48eaa505e378634
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94693758"
---
# <a name="move-an-external-load-balancer-to-another-region-by-using-the-azure-portal"></a>Azure portal kullanarak dış yük dengeleyiciyi başka bir bölgeye taşıyın

Bir dış yük dengeleyiciyi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, test için aynı yapılandırmaya sahip başka bir dış yük dengeleyici oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir dış yük dengeleyiciyi başka bir bölgeye taşımak isteyebilirsiniz.

Değişmez değer anlamda, bir Azure dış yük dengeleyiciyi bir bölgeden diğerine taşıyamazsınız. Ancak, bir dış yük dengeleyicinin var olan yapılandırmasını ve genel IP adresini dışarı aktarmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Daha sonra, yük dengeleyiciyi ve genel IP 'yi bir şablona aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve sonra şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgede oluşturabilirsiniz. Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [kaynak gruplarını şablonlara dışarı aktarma](../azure-resource-manager/management/manage-resource-groups-powershell.md#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Önkoşullar

- Azure dış yük dengeleyicinin, taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure dış yük dengeleyiciler bölgeler arasında taşınamaz. Yeni Yük dengeleyiciyi hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir dış yük dengeleyici yapılandırmasını dışarı aktarmak ve başka bir bölgede dış yük dengeleyici oluşturmak üzere bir şablon dağıtmak için, ağ katılımcısı rolü veya daha yüksek bir sürümü atanması gerekir.

- Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirler. Bu düzen, yük dengeleyiciler, ağ güvenlik grupları, genel IP 'Ler ve sanal ağlar dahil değildir ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, hedef bölgede dış yük dengeleyiciler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin yük dengeleyiciler eklenmesini desteklemek için yeterli kaynağa sahip olduğundan emin olun. Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits).

## <a name="prepare-and-move"></a>Hazırlama ve taşıma
Aşağıdaki yordamlarda, bir Kaynak Yöneticisi şablonu kullanarak taşıma için dış yük dengeleyiciyi hazırlama ve Azure portal kullanarak dış yük dengeleyici yapılandırmasını hedef bölgeye taşıma işlemleri gösterilmektedir. Önce dış yük dengeleyicinin genel IP yapılandırmasını dışarı aktarmanız gerekir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-the-public-ip-from-the-portal"></a>Genel IP şablonunu dışarı aktarma ve portaldan genel IP dağıtma

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.
2. Kaynak ortak IP 'yi içeren kaynak grubunu bulun ve seçin.
3. **Ayarları**  >  **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** altında **Dağıt** ' ı seçin.
5. **TEMPLATE**  >  Çevrimiçi düzenleyicide dosya parameters.jsaçmak için şablon **düzenleme parametrelerini** seçin.
8. Genel IP adının parametresini düzenlemek için, **Parametreler** altındaki **değer** özelliğini kaynak genel IP adından hedef genel IP 'niz adına değiştirin. Adı tırnak işaretleri içine alın.

    ```json
            {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "publicIPAddresses_myVM1pubIP_name": {
            "value": "<target-publicip-name>"
              }
             }
            }

    ```

    Düzenleyicide **Kaydet** ' i seçin.

9.  **TEMPLATE**  >  Çevrimiçi düzenleyicide dosya template.jsaçmak için şablon **düzenleme şablonu** ' nu seçin.

10. Genel IP 'nin taşınacağı hedef bölgeyi düzenlemek için, **kaynaklar** altındaki **Location** özelliğini değiştirin:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]
    ```
  
    Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bölge için kod, boşluk içermeyen bölge adıdır. Örneğin, Orta ABD için kod **merkezileştirme**.
    
12. Gereksinimlerinize bağlı olarak, veya gerekirse, şablondaki diğer parametreleri de değiştirebilirsiniz:

    * **SKU 'su**. Dosyadaki template.js **SKU** altında bulunan **Name** özelliğini DEĞIŞTIREREK, yapılandırmadaki genel IP 'yi standart iken temel veya temel ile standart arasında değiştirebilirsiniz:

        ```json
          "resources": [
         {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
        ```

        Temel ve standart SKU genel IP 'Leri arasındaki farklar hakkında daha fazla bilgi için bkz. [genel IP adresi oluşturma, değiştirme veya silme](../virtual-network/virtual-network-public-ip-address.md).

    * **Genel IP ayırma yöntemi** ve **boşta kalma zaman aşımı**. **Publicıpallocationmethod** özelliğini **Dynamic** ya da **static** 'ten **Dynamic** **'e değiştirerek** genel IP ayırma yöntemini değiştirebilirsiniz. **Idletimeoutınminutes** özelliğini istenen değere değiştirerek boşta kalma zaman aşımını değiştirebilirsiniz. Varsayılan değer **4**' dir.

        ```json
          "resources": [
         {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []

        ```

        Ayırma yöntemleri ve boşta kalma zaman aşımı değerleri hakkında daha fazla bilgi için bkz. [genel IP adresi oluşturma, değiştirme veya silme](../virtual-network/virtual-network-public-ip-address.md).

 
13. Çevrimiçi düzenleyicide **Kaydet** ' i seçin.

14. **BASICS**  >  Hedef genel IP 'nin dağıtılacağı aboneliği seçmek için temel bilgiler **aboneliğini** seçin.

15. **BASICS**  >  Hedef genel IP 'nin dağıtılacağı kaynak grubunu seçmek için temel bilgiler **kaynak grubu** ' nu seçin. Hedef genel IP 'si için yeni bir kaynak grubu oluşturmak üzere **Yeni oluştur** ' u seçebilirsiniz. Adın mevcut kaynak genel IP 'nin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. Temel bir **BASICS**  >  **konumun** , genel IP 'nin dağıtılmasını istediğiniz hedef konuma ayarlandığını doğrulayın.

17. **Ayarlar**' ın altında, ad ' ın daha önce parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. **Hüküm ve koşullar** onay kutusunu seçin.

19. Hedef ortak IP 'yi dağıtmak için **satın al** ' ı seçin.

20. Taşınan yük dengeleyici için giden NAT için kullanılan başka bir genel IP varsa, önceki adımları yineleyerek ikinci giden genel IP 'yi dışa aktarıp hedef bölgeye dağıtın.

### <a name="export-the-external-load-balancer-template-and-deploy-the-load-balancer-from-the-azure-portal"></a>Dış yük dengeleyici şablonunu dışarı aktarın ve Azure portal yük dengeleyiciyi dağıtın

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.
2. Kaynak dış yük dengeleyiciyi içeren kaynak grubunu bulun ve seçin.
3. **Ayarları**  >  **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** altında **Dağıt** ' ı seçin.
5. **TEMPLATE**  >  Çevrimiçi düzenleyicide dosya parameters.jsaçmak için şablon **düzenleme parametrelerini** seçin.

5. Dış yük dengeleyici adının parametresini düzenlemek için, kaynak dış yük dengeleyici adının **değer** özelliğini hedef dış yük dengeleyicinizin adıyla değiştirin. Adı tırnak işaretleri içine alın.

    ```json
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
          "loadBalancers_myLoadbalancer_ext_name": {
          "value": "<target-external-lb-name>"
    },
          "publicIPAddresses_myPubIP_in_externalid": {
          "value": "<target-publicIP-resource-ID>"
    },

    ```

6.  Önceki adımlarda taşıdığınız hedef genel IP 'nin değerini düzenlemek için, önce kaynak KIMLIĞINI edinmeniz ve ardından dosyayı parameters.jsdosyasına yapıştırmanız gerekir. KIMLIĞI almak için:

    1. Başka bir tarayıcı sekmesi veya penceresinde, [Azure Portal](https://portal.azure.com) oturum açın ve **kaynak grupları**' nı seçin.
    2. Önceki adımlarda taşıdığınız ortak IP 'yi içeren hedef kaynak grubunu bulun. Bu klasörü seçin.
    3. **Ayarlar** > **Özellikler**'i seçin.
    4. Sağ taraftaki dikey pencerede, **kaynak kimliğini** vurgulayın ve panoya kopyalayın. Alternatif olarak, **kaynak kimliği** yolunun sağında **Panoya Kopyala** ' yı seçebilirsiniz.
    5. Kaynak KIMLIĞINI, diğer tarayıcı penceresinde veya sekmesinde açık olan **parametreleri Düzenle** düzenleyicisinde **değer** özelliğine yapıştırın:

        ```json
           ```json
           "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
           "contentVersion": "1.0.0.0",
           "parameters": {
              "loadBalancers_myLoadbalancer_ext_name": {
              "value": "<target-external-lb-name>"
        },
              "publicIPAddresses_myPubIP_in_externalid": {
              "value": "<target-publicIP-resource-ID>"
        },

        ```
    6. Çevrimiçi düzenleyicide **Kaydet** ' i seçin.


7.  Yük Dengeleyici için giden NAT ve giden kurallarını yapılandırdıysanız, giden genel IP 'nin dış KIMLIĞI için bu dosyada üçüncü bir giriş görürsünüz. Giden genel IP 'nin KIMLIĞINI edinmek için **hedef bölgedeki** önceki adımları yineleyin. Bu KIMLIĞI dosyadaki parameters.jsyapıştırın:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "value": "<target-external-lb-name>",

            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "value": "<target-publicIP-resource-ID>",

            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",

            }
        },
    ```

8.  **TEMPLATE**  >  Çevrimiçi düzenleyicide dosya template.jsaçmak için şablon **düzenleme şablonu** ' nu seçin.
9.  Dış yük dengeleyici yapılandırmasının taşınacağı hedef bölgeyi düzenlemek için, dosyadaki template.jsiçindeki **kaynaklar** altında **Location** özelliğini değiştirin:

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-external-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

10. Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bölge için kod, boşluk içermeyen bölge adıdır. Örneğin, Orta ABD için kod **merkezileştirme**.

11. Gereksinimlerinize bağlı olarak, veya gerekirse, şablondaki diğer parametreleri de değiştirebilirsiniz:

    * **SKU 'su**. Dosyadaki template.js **SKU** altında bulunan **Name** özelliğini değiştirerek, YAPıLANDıRMADAKI dış yük dengeleyicinin SKU 'sunu temel veya temel ile standart arasında değiştirebilirsiniz:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-external-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Temel ve standart SKU yük dengeleyiciler arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](./load-balancer-overview.md).

    * **Yük Dengeleme kuralları**. template.jsdosyadaki **loadBalancingRules** bölümüne giriş ekleyip çıkararak Yük Dengeleme kuralları ekleyebilir veya kaldırabilirsiniz:

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Yük Dengeleme kuralları hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview).

    * **Yoklamalar**. Dosyadaki template.js **yoklamalar** bölümünde girdileri ekleyerek veya kaldırarak, yapılandırmada yük dengeleyici için bir araştırma ekleyebilir veya kaldırabilirsiniz:

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Daha fazla bilgi için bkz. [Load Balancer sistem durumu araştırmaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

    * **Gelen NAT kuralları**. Dosyadaki template.js **ınboundnatrules** bölümüne giriş ekleyip kaldırarak yük dengeleyici IÇIN gelen NAT kuralları ekleyebilir veya kaldırabilirsiniz:

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Bir gelen NAT kuralı ekleme veya kaldırma işlemini gerçekleştirmek için, kuralın mevcut olması veya template.jsözellik olarak bir **tür** özelliği olarak kaldırılması gerekir:

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Gelen NAT kuralları hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview).

    * **Giden kuralları**. Dosyadaki template.js **outboundrules** özelliğini düzenleyerek, yapılandırmada giden kuralları ekleyebilir veya kaldırabilirsiniz:

        ```json
        "outboundRules": [
                    {
                        "name": "myOutboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "allocatedOutboundPorts": 10000,
                            "protocol": "All",
                            "enableTcpReset": false,
                            "idleTimeoutInMinutes": 15,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolOutbound')]"
                            },
                            "frontendIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPoutbound')]"
                                }
                            ]
                        }
                    }
                ]
        ```

         Daha fazla bilgi için bkz. [Load Balancer giden kuralları](./load-balancer-outbound-connections.md#outboundrules).

12. Çevrimiçi düzenleyicide **Kaydet** ' i seçin.

13. **BASICS**  >  Hedef dış yük dengeleyicinin dağıtılacağı aboneliği seçmek için temel bilgiler **aboneliğini** seçin.

15. **BASICS**  >  Hedef yük dengeleyicinin dağıtılacağı kaynak grubunu seçmek için temel bilgiler **kaynak grubu** ' nu seçin. Hedef dış yük dengeleyici için yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçebilirsiniz. Ya da daha önce genel IP için oluşturduğunuz mevcut kaynak grubunu seçebilirsiniz. Adın mevcut kaynak dış yük dengeleyicinin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **Temel bilgiler**  >  **konumunun** , dış yük dengeleyicinin dağıtılmasını istediğiniz hedef konuma ayarlandığını doğrulayın.

17. **Ayarlar**' ın altında, ad ' ın daha önce parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın. Yapılandırma içindeki genel IP 'Ler için kaynak kimliklerinin doldurulduğunu doğrulayın.

18. **Hüküm ve koşullar** onay kutusunu seçin.

19. Hedef ortak IP 'yi dağıtmak için **satın al** ' ı seçin.

## <a name="discard"></a>Vazgeç

Hedef genel IP ve dış yük dengeleyiciyi atmak istiyorsanız, bunları içeren kaynak grubunu silin. Bunu yapmak için, portalda panodaki kaynak grubunu seçin ve genel bakış sayfasının en üstündeki **Sil** ' i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri uygulamak ve genel IP ve dış yük dengeleyiciyi tamamlamak için kaynak genel IP ve dış yük dengeleyiciyi veya kaynak grubunu silin. Bunu yapmak için, portalda panodaki kaynak grubunu seçin ve sonra her sayfanın en üstünde **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure dış yük dengeleyiciyi bir bölgeden diğerine taşımış ve kaynak kaynakları temizledi. Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz.:


- [Kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Azure VM’lerini başka bir bölgeye taşıma](../site-recovery/azure-to-azure-tutorial-migrate.md)