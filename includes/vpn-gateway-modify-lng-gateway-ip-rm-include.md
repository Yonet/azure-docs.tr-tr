---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/15/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 22d68722afe4be6113263a7e7282dde3f188b18a
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027082"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress---no-gateway-connection"></a><a name="gwipnoconnection"></a> 'GatewayIpAddress' yerel ağ geçidini değiştirmek için - ağ geçidi bağlantısı yok

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın.

Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Farklı bir ad kullanırsanız mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="to-modify-the-local-network-gateway-gatewayipaddress---existing-gateway-connection"></a><a name="gwipwithconnection"></a> 'GatewayIpAddress' yerel ağ geçidini değiştirmek için - ağ geçidi bağlantısı var

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Zaten bir ağ geçidi bağlantısı varsa öncelikle bu bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 

1. Bağlantıyı kaldırın. ' Get-AzVirtualNetworkGatewayConnection ' cmdlet 'ini kullanarak bağlantınızın adını bulabilirsiniz.

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1
   ```
2. 'GatewayIpAddress' değerini değiştirin. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Bunu kullanmazsanız, mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name Site1 `
   -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
   -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
   ```
3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırıyoruz. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](/powershell/module/Azurerm.Network/New-AzureRmVirtualNetworkGatewayConnection) sayfasına bakın.  VirtualNetworkGateway adını almak için ' Get-AzVirtualNetworkGateway ' cmdlet 'ini çalıştırabilirsiniz.
   
    Değişkenleri ayarlayın.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1

   $vnetgw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
   ```
   
    Bağlantıyı oluşturun.

   ```azurepowershell-interactive 
   New-AzVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
   -Location "East US" `
   -VirtualNetworkGateway1 $vnetgw `
   -LocalNetworkGateway2 $local `
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```