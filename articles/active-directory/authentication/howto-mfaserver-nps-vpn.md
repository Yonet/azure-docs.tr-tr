---
title: Azure MFA sunucusu ve üçüncü taraf VPN 'Ler Azure Active Directory
description: Azure MFA sunucusu 'nun Cisco, Citrix ve Juniper ile tümleştirileceği adım adım yapılandırma kılavuzlarından bazıları.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88fe09199cb50d2a3796c3b638dca1a723016dc4
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96742026"
---
# <a name="advanced-scenarios-with-azure-mfa-server-and-third-party-vpn-solutions"></a>Azure MFA sunucusu ve üçüncü taraf VPN çözümleriyle gelişmiş senaryolar

Azure Multi-Factor Authentication Sunucusu (Azure MFA sunucusu), çeşitli üçüncü taraf VPN çözümleriyle sorunsuz bir şekilde bağlanmak için kullanılabilir. Bu makalede Cisco &reg; asa VPN gereci, Citrix NetScaler SSL VPN gereci ve Juniper Networks güvenli erişim/Pulse Secure Connect GÜVENLI SSL VPN gereci ele alınmaktadır. Bu üç ortak gereçi ele almak için yapılandırma kılavuzlarını oluşturduk. Azure MFA sunucusu, AD FS için RADIUS, LDAP, IIS veya talep tabanlı kimlik doğrulaması kullanan diğer sistemlerle de tümleştirilebilir. Daha fazla ayrıntı için [Azure MFA sunucu yapılandırmalarında](howto-mfaserver-deploy.md#next-steps)daha fazla bilgi bulabilirsiniz.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için MFA sunucusu sağlamamaktadır. Oturum açma olayları sırasında çok faktörlü kimlik doğrulaması (MFA) gerektirmek isteyen yeni müşteriler, bulut tabanlı Azure AD Multi-Factor Authentication kullanmalıdır.
>
> Bulut tabanlı MFA 'yı kullanmaya başlamak için bkz. [öğretici: Azure AD Multi-Factor Authentication Ile güvenli Kullanıcı oturum açma olayları](tutorial-enable-azure-mfa.md).
>
> Bulut tabanlı MFA kullanıyorsanız bkz. [VPN altyapınızı Azure MFA Ile tümleştirme](howto-mfa-nps-extension-vpn.md).
>
> MFA sunucusunu 1 Temmuz 2019 tarihinden önce etkinleştiren mevcut müşteriler, en son sürümü, gelecekteki güncelleştirmeleri indirebilir ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilir.

## <a name="cisco-asa-vpn-appliance-and-azure-mfa-server"></a>Cisco ASA VPN gereci ve Azure MFA sunucusu
Azure MFA sunucusu, &reg; Cisco AnyConnect &reg; VPN oturumu açma ve Portal erişimi için ek güvenlik sağlamak üzere CISCO asa VPN gereci ile tümleşir.  LDAP ya da RADIUS protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma kılavuzlarını indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için, AnyConnect VPN ve Azure MFA yapılandırması ile Cisco ASA](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | LDAP kullanarak Cisco ASA VPN gerecinizi Azure MFA ile tümleştirme |
| [RADIUS için AnyConnect VPN ve Azure MFA yapılandırması ile Cisco ASA](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | RADIUS kullanarak Cisco ASA VPN gerecinizi Azure MFA ile tümleştirme |

## <a name="citrix-netscaler-ssl-vpn-and-azure-mfa-server"></a>Citrix NetScaler SSL VPN ve Azure MFA sunucusu
Azure MFA sunucusu, Citrix NetScaler SSL VPN oturumu açma ve Portal erişimi için ek güvenlik sağlamak üzere Citrix NetScaler SSL VPN gereci ile tümleşir.  LDAP ya da RADIUS protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma kılavuzlarını indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [Citrix NetScaler SSL VPN ve LDAP için Azure MFA yapılandırması](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | LDAP kullanarak Citrix NetScaler SSL VPN 'nizi Azure MFA gereci ile tümleştirme |
| [Citrix NetScaler SSL VPN ve RADIUS için Azure MFA yapılandırması](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | RADIUS kullanarak Citrix NetScaler SSL VPN gerecinizi Azure MFA ile tümleştirin |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-mfa-server"></a>Juniper/Pulse Secure SSL VPN gereci ve Azure MFA sunucusu
Azure MFA sunucusu, Juniper/Pulse güvenli SSL VPN oturumu açma ve Portal erişimi için ek güvenlik sağlamak üzere Juniper/Pulse Secure SSL VPN gereci ile tümleşir.  LDAP ya da RADIUS protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma kılavuzlarını indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [Juniper/Pulse Secure SSL VPN ve LDAP için Azure MFA yapılandırması](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | LDAP kullanarak Juniper/Pulse güvenli SSL VPN 'nizi Azure MFA gereci ile tümleştirme |
| [RADIUS için Juniper/Pulse Secure SSL VPN ve Azure MFA yapılandırması](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | RADIUS kullanarak Juniper/Pulse Secure SSL VPN gerecinizi Azure MFA ile tümleştirme |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure için NPS Uzantısı ile mevcut kimlik doğrulama altyapınızı artırmak Multi-Factor Authentication](howto-mfa-nps-extension.md)

- [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)
