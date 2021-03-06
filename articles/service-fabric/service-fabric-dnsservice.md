---
title: Azure Service Fabric DNS hizmeti
description: Kümenin içinden mikro hizmetleri keşfetmek için Service Fabric DNS hizmetini kullanın.
ms.topic: conceptual
ms.date: 7/20/2018
ms.custom: devx-track-csharp
ms.openlocfilehash: f7f06920820cdc73f8d3101ab24ee46625931ee4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91268052"
---
# <a name="dns-service-in-azure-service-fabric"></a>Azure Service Fabric'te DNS Hizmeti
DNS hizmeti, kümenizde etkinleştirebileceğiniz ve DNS protokolünü kullanarak diğer hizmetleri keşfedebilmeniz için kullanabileceğiniz isteğe bağlı bir sistem hizmetidir. 

Birçok hizmet, özellikle Kapsayıcılı hizmetler, önceden var olan bir URL aracılığıyla adreslenebilir. Bu hizmetleri, Service Fabric Adlandırma Hizmeti protokolü yerine standart DNS protokolünü kullanarak çözebilmek istenebilir. DNS hizmeti, DNS adlarını bir hizmet adıyla eşlemenizi sağlar ve bu nedenle uç nokta IP adreslerini çözümleyebilir. Bu işlevsellik, farklı platformlarda Kapsayıcılı hizmetlerin taşınabilirliği sağlar ve Adlandırma Hizmeti 'ten yararlanmak üzere kodu yeniden yazmak yerine mevcut hizmet URL 'Lerini kullanmanıza izin vererek "kaldırma ve kaydırma" senaryolarını daha kolay hale getirir. 

DNS hizmeti, DNS adlarını hizmet adlarıyla eşler, bu da hizmet uç noktasını döndürmek için Adlandırma Hizmeti tarafından çözümlenir. Hizmetin DNS adı, oluşturma sırasında sağlanır. Aşağıdaki diyagramda, DNS hizmetinin durum bilgisi olmayan hizmetler için nasıl çalıştığı gösterilmektedir.

![DNS adlarının, durum bilgisi olmayan hizmetler için DNS hizmeti tarafından hizmet adlarıyla nasıl eşleştirildiğini gösteren diyagram.](./media/service-fabric-dnsservice/stateless-dns.png)

Service Fabric sürüm 6,3 ' den başlayarak, Service Fabric DNS protokolü bölümlenmiş durum bilgisi olan hizmetlerin adreslenmesi için bir düzen içerecek şekilde genişletilmiştir. Bu uzantılar, durum bilgisi olan hizmet DNS adı ve bölüm adı birleşimini kullanarak belirli bölüm IP adreslerini çözümlemeyi mümkün kılar. Üç bölümleme şeması desteklenir:

- Adlandırılmış bölümlendirme
- Ranşlı bölümlendirme
- Tek bölümlendirme

Aşağıdaki diyagramda, DNS hizmetinin bölümlenmiş durum bilgisi olan hizmetler için nasıl çalıştığı gösterilmektedir.

![DNS adlarının bölümlenmiş durum bilgisi olmayan hizmetler için DNS hizmeti tarafından hizmet adlarıyla nasıl eşleştirildiğini gösteren diyagram.](./media/service-fabric-dnsservice/stateful-dns.png)

Dinamik bağlantı noktaları DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktalarında sunulan hizmetleri çözümlemek için [ters proxy hizmetini](./service-fabric-reverseproxy.md)kullanın.

## <a name="enabling-the-dns-service"></a>DNS hizmetini etkinleştirme
> [!NOTE]
> Service Fabric Services için DNS hizmeti henüz Linux üzerinde desteklenmiyor.

Portalı kullanarak bir küme oluşturduğunuzda, DNS hizmeti, **küme yapılandırma** menüsündeki **DNS hizmetini dahil et** onay kutusunda varsayılan olarak etkindir:

![Portal üzerinden DNS hizmetini etkinleştirme](./media/service-fabric-dnsservice/enable-dns-service.png)

Kümenizi oluşturmak için portalını kullanmıyorsanız veya var olan bir kümeyi güncelleştiriyorsanız, bir şablonda DNS hizmetini etkinleştirmeniz gerekir:

- Yeni bir küme dağıtmak için, [örnek şablonları](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) kullanabilir ya da kendi kaynak yöneticisi şablonunuzu oluşturabilirsiniz. 
- Var olan bir kümeyi güncelleştirmek için, portaldaki kümenin kaynak grubuna giderek kümenin geçerli durumunu ve gruptaki diğer kaynakları yansıtan bir şablonla çalışmak için **Otomasyon betiği** ' ne tıklayabilirsiniz. Daha fazla bilgi için bkz. [kaynak grubundan şablonu dışarı aktarma](../azure-resource-manager/templates/export-template-portal.md).

Şablonunuz olduktan sonra, DNS hizmetini aşağıdaki adımlarla etkinleştirebilirsiniz:

1. `apiversion` `2017-07-01-preview` Kaynağının kaynak için veya sonraki bir sürüme ayarlandığından emin olun `Microsoft.ServiceFabric/clusters` ve yoksa, aşağıdaki örnekte gösterildiği gibi güncelleştirin:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Artık DNS hizmetini aşağıdaki yollarla etkinleştirin:

   - DNS hizmetini varsayılan ayarlarla etkinleştirmek için, `addonFeatures` `properties` Aşağıdaki örnekte gösterildiği gibi bölümün içindeki bölümüne ekleyin:

        ```json
          "properties": {
            ...
            "addonFeatures": [
              "DnsService"
              ],
            ...
          }
        ```

   - Hizmeti varsayılan ayarlarla etkinleştirmek için `DnsService` `fabricSettings` bölümün içindeki bölümüne bir bölüm ekleyin `properties` . Bu durumda, ' ye DnsService eklemeniz gerekmez `addonFeatures` . DNS hizmeti için ayarlanacak özellikler hakkında daha fazla bilgi edinmek için bkz. [DNS hizmeti ayarları](./service-fabric-cluster-fabric-settings.md#dnsservice).

       ```json
           "properties": {
             ...  
             "fabricSettings": [
               ...
               {
                 "name": "DnsService",
                 "parameters": [
                   {
                     "name": "IsEnabled",
                     "value": "true"
                   },
                   {
                     "name": "PartitionSuffix",
                     "value": "--"
                   },
                   {
                     "name": "PartitionPrefix",
                     "value": "--"
                   }
                 ]
               },
               ...
              ]
            }
       ```
3. Küme şablonunu yaptığınız değişikliklerle güncelleştirdikten sonra, bunları uygulayın ve yükseltmenin tamamlanmasını sağlayın. Yükseltme tamamlandığında, DNS sistem hizmeti kümenizde çalışmaya başlar. Hizmet adı `fabric:/System/DnsService` , ve Service Fabric Explorer 'ın **sistem** hizmeti bölümünde bulunabilir. 

> [!NOTE]
> DNS devre dışı iken etkin olarak yükseltilirken Service Fabric Explorer yeni durumu yansıtmayabilir. Çözümlemek için Azure Resource Manager şablonunuzda UpgradePolicy öğesini değiştirerek düğümleri yeniden başlatın. Daha fazla bilgi için [Service Fabric şablonu başvurusuna](/azure/templates/microsoft.servicefabric/2019-03-01/clusters/applications) bakın.

> [!NOTE]
> Yerel bir makinede geliştirme yaparken DNS hizmetini etkinleştirmek bazı DNS ayarlarını geçersiz kılar. İnternet 'e bağlanma sorunlarıyla karşılaşırsanız DNS ayarlarınızı kontrol edin.

## <a name="setting-the-dns-name-for-your-service"></a>Hizmetiniz için DNS adını ayarlama
Hizmetleriniz için bir DNS adı ApplicationManifest.xml dosyasında veya PowerShell komutları aracılığıyla varsayılan hizmetler için bildirimli olarak ayarlayabilirsiniz.

Hizmetinizin DNS adı küme genelinde çözülebilir, bu sayede DNS adının küme genelinde benzersizliği olduğundan emin olmak önemlidir. 

Bir adlandırma şeması kullanmanız önemle önerilir `<ServiceDnsName>.<AppInstanceName>` ; Örneğin, `service1.application1` . Bir uygulama Docker Compose kullanılarak dağıtılırsa, hizmetler otomatik olarak bu adlandırma şemasını kullanarak DNS adlarına atanır.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>ApplicationManifest.xml varsayılan bir hizmetin DNS adını ayarlama
Projenizi Visual Studio 'da veya en sevdiğiniz düzenleyicide açın ve ApplicationManifest.xml dosyasını açın. Varsayılan hizmetler bölümüne gidin ve her bir hizmet için `ServiceDnsName` özniteliği ekleyin. Aşağıdaki örnekte, hizmetinin DNS adının nasıl ayarlanacağı gösterilmektedir. `service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
      <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
```
Uygulama dağıtıldıktan sonra, Service Fabric Gezgini 'ndeki hizmet örneği, aşağıdaki şekilde gösterildiği gibi bu Örneğin DNS adını gösterir: 

![hizmet uç noktaları](./media/service-fabric-dnsservice/service-fabric-explorer-dns.png)

Aşağıdaki örnek, durum bilgisi olan bir hizmetin DNS adını olarak ayarlar `statefulsvc.app` . Hizmet, adlandırılmış bölümlendirme şeması kullanır. Bölüm adlarının küçük olduğunu unutmayın. Bu, DNS sorgularına hedeflenecek bölümler için gereksinimdir; daha fazla bilgi için bkz. [durum bilgisi olan hizmet bölümünde DNS sorguları yapma](#preview-making-dns-queries-on-a-stateful-service-partition).

```xml
    <Service Name="Stateful1" ServiceDnsName="statefulsvc.app" />
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="2" MinReplicaSetSize="2">
        <NamedPartition>
         <Partition Name="partition1" />
         <Partition Name="partition2" />
        </NamedPartition>
      </StatefulService>
    </Service>
```

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>PowerShell kullanarak bir hizmetin DNS adını ayarlama
Bir hizmetin DNS adını PowerShell komutunu kullanarak oluştururken ayarlayabilirsiniz `New-ServiceFabricService` . Aşağıdaki örnek, DNS adıyla yeni bir durum bilgisi olmayan hizmet oluşturur `service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="preview-making-dns-queries-on-a-stateful-service-partition"></a>Önizle Durum bilgisi olan bir hizmet bölümünde DNS sorguları yapma
Service Fabric sürüm 6,3 ' den başlayarak, Service Fabric DNS hizmeti hizmet bölümlerinin sorgularını destekler.

DNS sorgularında kullanılacak bölümler için aşağıdaki adlandırma kısıtlamaları geçerlidir:

   - Bölüm adları, DNS uyumlu olmalıdır.
   - Çoklu etiketli bölüm adları (ada sahip, '. ', vb.) kullanılmamalıdır.
   - Bölüm adları küçük harf olmalıdır.

Bir bölümü hedefleyen DNS sorguları aşağıdaki gibi biçimlendirilir:

```
    <First-Label-Of-Partitioned-Service-DNSName><PartitionPrefix><Target-Partition-Name>< PartitionSuffix>.<Remaining- Partitioned-Service-DNSName>
```
Burada:

- *İlk-bölümlenmiş-Service-DnsName etiketi* , hizmet DNS adınızın ilk kısmıdır.
- *PartitionPrefix* , küme bildiriminin DNSservice bölümünde veya kümenin Kaynak Yöneticisi şablonuyla ayarlanabilir bir değerdir. "--" Varsayılan değeri. Daha fazla bilgi için bkz.  [DNS hizmeti ayarları](./service-fabric-cluster-fabric-settings.md#dnsservice).
- *Hedef-bölüm adı* , bölümün adıdır. 
- *Partitionsuffix* , küme bildiriminin DNSservice bölümünde veya kümenin Kaynak Yöneticisi şablonuyla ayarlanabilir bir değerdir. Varsayılan değer boş bir dizedir. Daha fazla bilgi için bkz.  [DNS hizmeti ayarları](./service-fabric-cluster-fabric-settings.md#dnsservice).
- *Kalan-bölümlenmiş-Service-DnsName* , hizmet DNS adınızın kalan kısmıdır.

Aşağıdaki örneklerde, ve için varsayılan ayarlarına sahip bir kümede çalışan bölümlenmiş hizmetlere yönelik DNS sorguları gösterilmektedir `PartitionPrefix` `PartitionSuffix` : 

- Bir hizmetin "0" bölümünü `backendrangedschemesvc.application` , bir ranşlı bölümleme şeması kullanan DNS adına sahip bir hizmette çözümlemek için kullanın `backendrangedschemesvc-0.application` .
- Adlandırılmış bölümlendirme şeması kullanan DNS adına sahip bir hizmetin "ilk" bölümünü çözümlemek için `backendnamedschemesvc.application` kullanın `backendnamedschemesvc-first.application` .

DNS hizmeti, bölümün birincil çoğaltmasının IP adresini döndürür. Bölüm belirtilmemişse, hizmet rastgele seçilmiş bir bölümün birincil çoğaltmasının IP adresini döndürür.

## <a name="using-dns-in-your-services"></a>Hizmetinizdeki DNS kullanma
Birden fazla hizmet dağıtıyorsanız, iletişim kuracak diğer hizmetlerin uç noktalarını bir DNS adı kullanarak bulabilirsiniz. DNS hizmeti, durum bilgisi olmayan hizmetler için ve Service Fabric sürüm 6,3 ve sonrasında, durum bilgisi olan hizmetler için kullanılır. 6,3 ' dan önceki Service Fabric sürümlerinde çalışan durum bilgisi olan hizmetler için, belirli bir hizmet bölümünü çağırmak üzere http çağrıları için yerleşik [ters ara sunucu hizmetini](./service-fabric-reverseproxy.md) kullanabilirsiniz. 

Dinamik bağlantı noktaları DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktaları kullanan Hizmetleri çözümlemek için ters proxy hizmetini kullanabilirsiniz.

Aşağıdaki kod, bir durum bilgisiz hizmetin DNS aracılığıyla nasıl çağrılacağını gösterir. Yalnızca DNS adını, bağlantı noktasını ve herhangi bir isteğe bağlı yolu URL 'nin bir parçası olarak sağladığınız normal bir http çağrıdır.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

Aşağıdaki kod, durum bilgisi olan bir hizmetin belirli bir bölümündeki çağrıyı gösterir. Bu durumda, DNS adı Bölüm adını (partition2) içerir. Çağrı, ve için varsayılan değerleri içeren bir küme olduğunu varsayar `PartitionPrefix` `PartitionSuffix` .

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service2-partition1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="known-issues"></a>Bilinen Sorunlar
* 6,3 ve üzeri sürümler Service Fabric için DNS adında kısa çizgi içeren hizmet adları için DNS aramalarıyla ilgili bir sorun vardır. Bu sorunla ilgili daha fazla bilgi için lütfen aşağıdaki [GitHub sorununu](https://github.com/Azure/service-fabric-issues/issues/1197)izleyin. Bunun için bir onarım sonraki 6,3 güncelleştirmesine geliyor. 

* Service Fabric Services için DNS hizmeti henüz Linux üzerinde desteklenmiyor. DNS hizmeti, Linux üzerinde kapsayıcılar için desteklenir. Fabric Client/ServicePartitionResolver kullanarak el ile çözümleme, kullanılabilir alternatiftir.

## <a name="next-steps"></a>Sonraki adımlar
[Connect ve hizmetlerle iletişim kurma](service-fabric-connect-and-communicate-with-services.md) ile küme içindeki hizmet iletişimi hakkında daha fazla bilgi edinin
