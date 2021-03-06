---
title: NAT Gateway oluşturma ve test etme-Azure PowerShell
titlesuffix: Azure Virtual Network NAT
description: Bu öğreticide, Azure PowerShell kullanarak NAT ağ geçidinin oluşturulması ve NAT hizmetinin test olması gösterilmektedir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to test a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.subservice: nat
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2020
ms.author: allensu
ms.openlocfilehash: f319e77bd4801a2844a54ba8eba955c2b062fe7e
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98934293"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-powershell-and-test-the-nat-service"></a>Öğretici: Azure PowerShell kullanarak NAT ağ geçidi oluşturma ve NAT hizmetini test etme

Bu öğreticide, Azure 'da sanal makineler için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturacaksınız. NAT ağ geçidini test etmek için bir kaynak ve hedef sanal makine dağıtırsınız. Genel bir IP adresine giden bağlantılar yaparak NAT ağ geçidini test edersiniz. Bu bağlantılar kaynaktan hedef sanal makineye gönderilir. Bu öğretici, basitlik için aynı kaynak grubundaki iki farklı sanal ağda kaynak ve hedef dağıtır.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell kullanarak tamamlayabilirsiniz veya ilgili komutları yerel olarak çalıştırabilirsiniz.  Azure Cloud Shell hiç kullanmadıysanız, [Şimdi oturum açmanız](https://shell.azure.com)gerekir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek **eastus2** konumunda **Myresourcegroupnat** adlı bir kaynak grubu oluşturur:


```azurepowershell-interactive
$rgname = 'myResourceGroupNAT'
$loc = 'eastus2'

$rg = New-AzResourceGroup -Name $rgname -Location $loc

```

## <a name="create-the-nat-gateway"></a>NAT ağ geçidini oluşturma

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Internet 'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresi gerekir. **Myresourcegroupnat** Içinde **Mypublicipsource** adlı bir genel IP adresi kaynağı oluşturmak Için [New-azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu kullanın. Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIPsource** adlı bir değişkende depolanır.

```azurepowershell-interactive
$pipname = 'myPublicIPsource'
$alm = 'Static'
$sku = 'Standard'

$publicIPsource = 
New-AzPublicIpAddress -Name $pipname -ResourceGroupName $rg.ResourceGroupName -AllocationMethod $alm -Sku $sku -Location $rg.Location

```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

 **Myresourcegroupnat** içinde **myPublicIPprefixsource** adlı bir genel IP ön eki kaynağı oluşturmak Için [New-azpublicipprefix](/powershell/module/az.network/new-azpublicipprefix) komutunu kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIPPrefixsource** adlı bir değişkende depolanır.

```azurepowershell-interactive
$prefixname = 'mypublicIPprefixsource'

$publicIPPrefixsource = 
New-AzPublicIpPrefix -Name $prefixname -ResourceGroupName $rg.ResourceGroupName -PrefixLength 31 -Location $rg.Location

```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini oluşturma ve yapılandırma işlemlerinin ayrıntıları verilmiştir:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel IP havuzu ve genel IP öneki.
  - Boşta kalma zaman aşımını varsayılan olarak 4 dakika ile 10 dakika arasında değiştirin.

[New-AzNatGateway](/powershell/module/az.network/new-aznatgateway)ile genel BIR Azure NAT ağ geçidi oluşturun. Bu komutun sonucu, **Mtypublicipsource** genel IP adresini ve **myPublicIPprefixsource** genel IP önekini kullanan **mynatgateway** adlı bir ağ geçidi kaynağı oluşturur. Boşta kalma zaman aşımı 10 dakikaya ayarlanır.  Bu komutun sonucu, daha sonra kullanılmak üzere **$natGateway** adlı bir değişkende depolanır.

```azurepowershell-interactive
$sku = 'Standard'
$natname = 'myNATgateway'

$natGateway = 
New-AzNatGateway -Name $natname -ResourceGroupName $rg.ResourceGroupName -PublicIpAddress $publicIPsource -PublicIpPrefix $publicIPPrefixsource -Sku $sku -IdleTimeoutInMinutes 10 -Location $rg.Location

  ```

Bu noktada, NAT ağ geçidi çalışır ve yok, bir sanal ağın hangi alt ağlarının bunu kullanması gerektiğini yapılandırmaktır.

## <a name="prepare-the-source-for-outbound-traffic"></a>Kaynağı giden trafik için hazırlama

Tam test ortamının kurulumu boyunca size kılavuzluk edeceğiz. NAT ağ geçidini doğrulamak üzere açık kaynaklı araçları kullanarak bir test ayarlayacaksınız. Daha önce oluşturduğumuz NAT ağ geçidini kullanacak kaynak ile başlayacağız.

### <a name="configure-virtual-network-for-source"></a>Kaynak için sanal ağı Yapılandır

Sanal ağı oluşturun ve alt ağı ağ geçidiyle ilişkilendirin.

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)kullanarak **Myresourcegroupnat** ' de [New-Azvirtualnetworksubnetconfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) kullanarak **mysubnetsource** adlı bir alt ağ ile **myvnetsource** adlı bir sanal ağ oluşturun. Sanal ağın IP adresi alanı **192.168.0.0/16**' dır. Sanal ağ içindeki alt ağ **192.168.0.0/24**' dir.  Komutların sonucu, daha sonra kullanılmak üzere **$subnetsource** ve **$vnetsource** adlı değişkenlere depolanır.

```azurepowershell-interactive
$subnetname = 'mySubnetsource'
$subnetprefix = '192.168.0.0/24'
$vnetname = 'myVnetsource'
$vnetprefix = '192.168.0.0/16'

$subnetsource = 
New-AzVirtualNetworkSubnetConfig -Name $subnetname -AddressPrefix $subnetprefix -NatGateway $natGateway

$vnetsource = 
New-AzVirtualNetwork -Name $vnetname -ResourceGroupName $rg.ResourceGroupName -AddressPrefix $vnetprefix -Subnet $subnetsource -Location $rg.Location

```

Internet hedeflerine giden tüm trafik artık NAT hizmetini kullanıyor.  UDR 'yi yapılandırmak gerekli değildir.

NAT ağ geçidini test etmeden önce bir kaynak VM 'si oluşturuyoruz.  Bu sanal makineye dışarıdan erişmek için örnek düzeyi genel IP olarak genel bir IP adresi kaynağı atayacağız. Bu adres yalnızca test için erişim için kullanılır.  NAT hizmetinin diğer giden seçeneklere göre nasıl öncelikli olduğunu göstereceğiz.

Ayrıca, bu VM 'yi ortak IP olmadan oluşturabilir ve bir alıştırma olarak genel IP 'si olmadan bir sıçrama kutusu olarak kullanmak için başka bir VM oluşturabilirsiniz.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için genel IP oluşturma

SANAL makineye erişmek için kullanılacak bir genel IP oluşturacağız.  **Myresourcegroupnat** Içinde **Mypublicipvm** adlı bir genel IP adresi kaynağı oluşturmak Için [New-azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIpsourceVM** adlı bir değişkende depolanır.

```azurepowershell-interactive
$sku = 'Standard'
$pipvmname = 'myPublicIpsourceVM'
$alm = 'Static'

$publicIpsourceVM = 
New-AzPublicIpAddress -Name $pipvmname -ResourceGroupName $rg.ResourceGroupName -AllocationMethod $alm -sku $sku -Location $rg.Location

```

### <a name="create-an-nsg-and-expose-ssh-endpoint-for-vm"></a>NSG oluşturma ve sanal makine için SSH uç noktası gösterme

Standart genel IP adresleri ' varsayılan olarak güvenli ' olduğundan, SSH için gelen erişime izin veren bir NSG oluşturacağız. NAT hizmeti akış yönü farkındır. Bu NSG, aynı alt ağda bir NAT ağ geçidi yapılandırıldıktan sonra giden için kullanılmaz. **Mynsgsource** adlı bir NSG kaynağı oluşturmak için [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) komutunu kullanın. [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) kullanarak, **Myresourcegroupnat** içinde **SSH** adlı SSH erişimi için bir NSG kuralı oluşturun. Bu komutun sonucu, daha sonra kullanılmak üzere **$nsgsource** adlı değişkende saklanacaktır.

```azurepowershell-interactive
$rnm = 'ssh'
$rdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$nsnm = 'myNSGsource'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $rdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$nsgsource = 
New-AzNetworkSecurityGroup -ResourceGroupName $rg.ResourceGroupName -Name $nsnm -SecurityRules $sshrule -Location $rg.Location

```

### <a name="create-nic-for-source-vm"></a>Kaynak VM için NIC oluşturma

**Mynicsource** adlı [New-aznetworkınterface](/powershell/module/az.network/new-aznetworkinterface) ile bir ağ arabirimi oluşturun. Bu komut genel IP adresini ve ağ güvenlik grubunu ilişkilendirir. Bu komutun sonucu, daha sonra kullanılmak üzere **$nicsource** adlı bir değişkende depolanır.

```azurepowershell-interactive
$nin = 'myNicsource'

$nicsource = 
New-AzNetworkInterface -ResourceGroupName $rg.ResourceGroupName -Name $nin -NetworkSecurityGroupID $nsgsource.Id -PublicIPAddressID $publicIPVMsource.Id -SubnetID $vnetsource.Subnets[0].Id -Location $rg.Location

```

### <a name="create-a-source-vm"></a>Kaynak VM oluşturma

#### <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için SSH-keygen kullanın.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048

```
PuTTy kullanımı dahil olmak üzere SSH anahtar çiftlerinin oluşturulması konusunda daha ayrıntılı bilgi edinmek için bkz. [Windows ile SSH anahtarları kullanma](../virtual-machines/linux/ssh-from-windows.md).

Cloud Shell kullanarak SSH anahtar çiftini oluşturursanız, anahtar çifti kapsayıcı görüntüde depolanır. Bu [depolama hesabı otomatik olarak oluşturulur](../cloud-shell/persisting-shell-storage.md). Anahtarlarınızı aldıktan sonra depolama hesabını veya içindeki dosya paylaşımından silme.

#### <a name="create-vm-configuration"></a>VM yapılandırması oluştur

PowerShell’de bir VM oluşturmak için kullanılacak görüntü, boyut ve kimlik doğrulaması seçenekleri gibi ayarların olduğu bir yapılandırma oluşturursunuz. Ardından bu yapılandırma VM’yi derlemek için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte, SSH anahtarı ~/. ssh/id_rsa. pub ' da depolanır.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmn = 'myVMsource'
$vms = 'Standard_DS1_v2'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigsource = 
New-AzVMConfig -VMName $vmn -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigsource -Linux -ComputerName $vmn -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigsource -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigsource -Id $nicsource.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigsource -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
**Myresourcegroupnat** içinde [New-Azvm](/powershell/module/az.compute/new-azvm) ile **MYVMSOURCE** adlı bir VM oluşturmak için yapılandırma tanımlarını birleştirin.

```azurepowershell-interactive
New-AzVM -ResourceGroupName $rg.ResourceGroupName -VM $vmConfigsource -Location $rg.Location

```

Komut hemen geri dönebilirken, VM 'nin dağıtılması birkaç dakika sürebilir.

## <a name="prepare-destination-for-outbound-traffic"></a>Giden trafik için hedefi hazırlama

Artık, NAT hizmeti tarafından, test yapmanıza olanak tanımak için çevrilmiş giden trafik için bir hedef oluşturacağız.

### <a name="configure-virtual-network-for-destination"></a>Sanal ağı hedef için yapılandırma

Hedef sanal makinenin nerede olacağı bir sanal ağ oluşturuyoruz.  Bu komutlar, kaynak VM ile aynı adımlardır. Hedef uç noktayı açığa çıkarmak için küçük değişiklikler eklendi.

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)kullanarak **Myresourcegroupnat** Içinde [New-Azvirtualnetworksubnetconfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) kullanarak **mysubnetdestination** adlı bir alt ağ ile **myvnetdestination** adlı bir sanal ağ oluşturun. Sanal ağın IP adresi alanı **192.168.0.0/16**' dır. Sanal ağ içindeki alt ağ **192.168.0.0/24**' dir.  Komutların sonucu, daha sonra kullanılmak üzere **$subnetdestination** ve **$vnetdestination** adlı değişkenlere depolanır.

```azurepowershell-interactive
$sbdn = 'mySubnetdestination'
$spfx = '192.168.0.0/24'
$vdn = 'myVnetdestination'
$vpfx = '192.168.0.0/16'

$subnetdestination = 
New-AzVirtualNetworkSubnetConfig -Name $sbdn -AddressPrefix $spfx

$vnetdestination = 
New-AzVirtualNetwork -Name $vdn -ResourceGroupName $rg.ResourceGroupName -AddressPrefix $vpfx -Subnet $subnetdestination -Location $rg.Location

```

### <a name="create-public-ip-for-destination-vm"></a>Hedef VM için genel IP oluşturma

Hedef VM 'ye erişmek için kullanılacak bir genel IP oluşturacağız.  **Myresourcegroupnat** Içinde **Mypublicipdestinationvm** adlı bir genel IP adresi kaynağı oluşturmak Için [New-azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) ' i kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIpdestinationVM** adlı bir değişkende depolanır.

```azurepowershell-interactive
$sku = 'Standard'
$all = 'Static'
$pipd = 'myPublicIPdestinationVM'

$publicIpdestinationVM = 
New-AzPublicIpAddress -Name $pipd -ResourceGroupName $rg.ResourceGroupName -AllocationMethod $all -Sku $sku -Location $rg.Location

```

### <a name="create-an-nsg-and-expose-ssh-and-http-endpoint-for-vm"></a>Bir NSG oluşturma ve sanal makine için SSH ve HTTP uç noktasını kullanıma sunma

Standart genel IP adresleri ' güvenli olarak güvenlidir ', SSH için gelen erişime izin veren bir NSG oluşturacağız. **Mynsgdestination** adlı bir NSG kaynağı oluşturmak için [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) komutunu kullanın. **SSH ADLı SSH** erişimi için bir NSG kuralı oluşturmak için [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) ' i kullanın.  **Http ADLı http** erişimi için bir NSG kuralı oluşturmak için [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) ' i kullanın. Her iki kural de **Myresourcegroupnat** içinde oluşturulacaktır. Bu komutun sonucu, daha sonra kullanılmak üzere **$nsgdestination** adlı bir değişkende depolanır.

```azurepowershell-interactive
$snm = 'ssh'
$sdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$hnm = 'http'
$hdsc = 'HTTP access'
$nsnm = 'myNSGdestination'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $snm -Description $sdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$httprule = 
New-AzNetworkSecurityRuleConfig -Name $hnm -Description $hdsc -Access $acc -Protocol $prt -Direction $dir -Priority 101 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$nsgdestination = 
New-AzNetworkSecurityGroup -ResourceGroupName $rg.ResourceGroupName -Name $nsnm -SecurityRules $sshrule,$httprule -Location $rg.Location

```

### <a name="create-nic-for-destination-vm"></a>Hedef VM için NIC oluşturma

**Mynicdestination** adlı [New-aznetworkınterface](/powershell/module/az.network/new-aznetworkinterface) ile bir ağ arabirimi oluşturun. Bu komut, genel IP adresi ve ağ güvenlik grubuyla ilişkilendirilecektir. Bu komutun sonucu, daha sonra kullanılmak üzere **$nicdestination** adlı bir değişkende depolanır.

```azurepowershell-interactive
$nnm = 'myNicdestination'

$nicdestination = 
New-AzNetworkInterface -ResourceGroupName $rg.ResourceGroupName -Name $nnm -NetworkSecurityGroupID $nsgdestination.Id -PublicIPAddressID $publicIPdestinationVM.Id -SubnetID $vnetdestination.Subnets[0].Id -Location $rg.Location

```

### <a name="create-a-destination-vm"></a>Hedef VM oluşturma

#### <a name="create-vm-configuration"></a>VM yapılandırması oluştur

PowerShell’de bir VM oluşturmak için kullanılacak görüntü, boyut ve kimlik doğrulaması seçenekleri gibi ayarların olduğu bir yapılandırma oluşturursunuz. Ardından bu yapılandırma VM’yi derlemek için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte, SSH anahtarı ~/. ssh/id_rsa. pub ' da depolanır.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration

$vmd = 'myVMdestination'
$vms = 'Standard_DS1_v2'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigdestination = New-AzVMConfig -VMName $vmd -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigdestination -Linux -ComputerName $vmd -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigdestination -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigdestination -Id $nicdestination.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigdestination -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
**Myresourcegroupnat** içinde [New-Azvm](/powershell/module/az.compute/new-azvm) ile **MYVMDESTINATION** adlı bir VM oluşturmak için yapılandırma tanımlarını birleştirin.

```azurepowershell-interactive

New-AzVM -ResourceGroupName $rg.ResourceGroupName -VM $vmConfigdestination -Location $rg.Location

```

Komut hemen geri dönebilirken, VM 'nin dağıtılması birkaç dakika sürebilir.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Hedef VM 'de bir Web sunucusunu ve test yükünü hazırlama

İlk olarak hedef sanal makinenin IP adresini bulduk.  VM 'nin genel IP adresini almak için [Get-Azpublicıpaddress](/powershell/module/az.network/get-azpublicipaddress)komutunu kullanın. 

```azurepowershell-interactive
$pipname = 'myPublicIPdestinationVM'
  
$destip = Get-AzPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -Name $pipname | select IpAddress

$destip

``` 

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve sonra sonraki adımlarda kullanabilmeniz için bir not defteri 'ne yapıştırın. Bu, hedef sanal makine olduğunu gösterir.

### <a name="sign-in-to-destination-vm"></a>Hedef VM 'de oturum açma

SSH kimlik bilgileri, önceki işlemden Cloud Shell depolanmalıdır.  Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```azurepowershell-interactive
ssh azureuser@$destip

```

Oturum açtıktan sonra aşağıdaki komutları kopyalayıp yapıştırın.  

```bash
sudo apt -y update && \
sudo apt -y upgrade && \
sudo apt -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100

```

Bu komutlar, sanal makinenizi güncelleştirir, NGINX 'i yükler ve bir 100 KB dosya oluşturur. Bu dosya, NAT hizmeti kullanılarak kaynak VM 'den alınacaktır.

Hedef VM ile SSH oturumunu kapatın.

## <a name="prepare-test-on-source-vm"></a>Kaynak VM 'de testi hazırlama

İlk olarak, kaynak VM 'nin IP adresini keşfetmemiz gerekir.  VM 'nin genel IP adresini almak için [Get-Azpublicıpaddress](/powershell/module/az.network/get-azpublicipaddress)komutunu kullanın. 

```azurepowershell-interactive
$pipname = 'myPublicIPsourceVM'

$srcip = Get-AzPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -Name $pipname | select IpAddress

$srcip

``` 

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve sonra sonraki adımlarda kullanabilmeniz için bir not defteri 'ne yapıştırın. Bunun kaynak sanal makine olduğunu gösterir.

### <a name="log-into-source-vm"></a>Kaynak VM 'de oturum açma

Yine, SSH kimlik bilgileri Cloud Shell depolanır. Tarayıcınızda [Azure Cloud Shell](https://shell.azure.com) için yeni bir sekme açın.  Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```azurepowershell-interactive
ssh azureuser@$srcip

```

NAT hizmetinin test edilmesine hazırlanmak için aşağıdaki komutları kopyalayıp yapıştırın.

```bash
sudo apt -y update && \
sudo apt -y upgrade && \
sudo apt install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Bu komutlar, sanal makinenizi günceller, git 'i yükler, GitHub 'dan [Hey](https://github.com/rakyll/hey) 'yi yükler ve kabuk ortamınızı güncelleştiracaktır.

Artık NAT hizmetini test etmeye hazırsınız.

## <a name="validate-nat-service"></a>NAT hizmetini doğrula

Kaynak VM 'de oturum açarken, hedef IP adresine istek oluşturmak için **kıvrımlı** ve **Hey** kullanabilirsiniz.

100-Kbayt dosyasını almak için kıvrımlı kullanın.  **\<ip-address-destination>** Aşağıdaki örnekte yerine, daha önce kopyaladığınız hedef IP adresini değiştirin.  **--Output** parametresi, alınan dosyanın atılacağını gösterir.

```bash
curl http://<ip-address-destination>/100k --output /dev/null

```

Ayrıca, **Hey** kullanarak bir dizi istek da oluşturabilirsiniz. Yeniden, **\<ip-address-destination>** daha önce kopyaladığınız hedef IP adresi ile değiştirin.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k

```

Bu komut, 30 saniyelik zaman aşımı süresi ile 10 istek 100 üretir. TCP bağlantısı yeniden kullanılamayacak.  Her istek 100 Kbayt alır.  Çalıştırmanın sonunda **, bu,** NAT hizmetinin ne kadar iyi olduğunu gösteren bir istatistik rapor eder.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name $rg.ResourceGroupName

```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir NAT ağ geçidi oluşturdunuz, kaynak ve hedef VM oluşturdunuz ve sonra NAT ağ geçidini test ediyor.

NAT hizmeti 'nin çalışıyor olduğunu görmek için Azure Izleyici 'de ölçümleri gözden geçirin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanılayın.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi eklenerek kolayca adreslenir.

- [Sanal ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI kullanarak NAT Gateway kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure PowerShell kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure Portal kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmaya yönelik hızlı başlangıç.

> [!div class="nextstepaction"]
