---
title: Azure Güvenlik Merkezi tarafından desteklenen platformlar | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi tarafından desteklenen platformların bir listesini sağlar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2020
ms.author: memildin
ms.openlocfilehash: 31ff3620bcab6b10b59582daa255ff1f259ab0d3
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96852651"
---
# <a name="supported-platforms"></a>Desteklenen platformlar 

Bu sayfada, Azure Güvenlik Merkezi tarafından desteklenen platformlar ve ortamlar gösterilmektedir.

## <a name="combinations-of-environments"></a>Ortamların birleşimleri <a name="vm-server"></a>

Azure Güvenlik Merkezi, farklı türlerde karma ortamlarda sanal makineleri ve sunucuları destekler:

* Yalnızca Azure
* Azure ve şirket içi
* Azure ve diğer bulutlar
* Azure, diğer bulutlar ve şirket içi

Azure Güvenlik Merkezi, Azure aboneliği üzerinde etkinleştirilen bir Azure ortamında, abonelik içinde dağıtılan IaaS kaynaklarını otomatik olarak keşfeder.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güvenlik Merkezi [Log Analytics aracısına](../azure-monitor/platform/agents-overview.md#log-analytics-agent)bağlıdır. Makinelerinizin, bu aracı için desteklenen işletim sistemlerinden birini aşağıdaki sayfalarda açıklandığı şekilde çalıştırdığından emin olun:

* [Windows tarafından desteklenen işletim sistemleri için Log Analytics Aracısı](../azure-monitor/platform/agents-overview.md#supported-operating-systems)
* [Linux tarafından desteklenen işletim sistemleri için Log Analytics Aracısı](../azure-monitor/platform/agents-overview.md#supported-operating-systems)

Ayrıca, Log Analytics aracınızın, [verileri Güvenlik Merkezi 'ne göndermek için düzgün şekilde yapılandırıldığından](security-center-enable-data-collection.md#manual-agent) emin olun

Windows ve Linux 'ta kullanılabilen belirli güvenlik merkezi özellikleri hakkında daha fazla bilgi edinmek için bkz. [makineler Için özellik kapsamı](security-center-services.md).

> [!NOTE]
> Azure Defender, sunucuları korumak üzere tasarlansa da, **sunucular Için Azure Defender** 'ın çoğu özelliği Windows 10 makinelerinde desteklenir. Şu anda desteklenmeyen bir özellik, [Güvenlik Merkezi 'nin tümleşik EDR çözümüdür: Endpoint Için Microsoft Defender](security-center-wdatp.md).

## <a name="managed-virtual-machine-services"></a>Yönetilen sanal makine Hizmetleri <a name="virtual-machine"></a>

Ayrıca, Azure Kubernetes (AKS), Azure Databricks ve daha fazlası gibi Azure tarafından yönetilen bazı hizmetlerin bir parçası olarak bir müşteri aboneliğinde de sanal makineler oluşturulur. Güvenlik Merkezi bu sanal makineleri de bulur ve desteklenen bir işletim sistemi varsa Log Analytics Aracısı yüklenebilir ve yapılandırılabilir.

## <a name="cloud-services"></a>Cloud Services <a name="cloud-services"></a>

Bulut hizmetinde çalışan sanal makineler de desteklenir. Yalnızca üretim yuvalarında çalışan Cloud Services Web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi edinmek için bkz. [Azure Cloud Services 'ye genel bakış](../cloud-services/cloud-services-choose-me.md).

Azure Stack bulunan VM 'Ler için koruma de desteklenir. Güvenlik Merkezi 'nin Azure Stack tümleştirmesi hakkında daha fazla bilgi için, bkz. [Azure Stack sanal makinelerinizi Güvenlik Merkezi 'ne](quickstart-onboard-machines.md)ekleme. 

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin Log Analytics aracısını kullanarak verileri nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.