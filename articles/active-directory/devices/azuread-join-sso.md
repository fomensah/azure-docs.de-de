---
title: Funktionsweise von SSO für lokale Ressourcen auf in Azure AD eingebundenen Geräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte konfigurieren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9d8c0cd803424e117bd4dc7a3382b7b32df2d05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672703"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>Funktionsweise von SSO für lokale Ressourcen auf in Azure AD eingebundenen Geräten

Es wird Sie wahrscheinlich nicht überraschen, dass für ein in Azure Active Directory (Azure AD) eingebundenes Gerät einmaliges Anmelden (Single Sign-On, SSO) für die Cloud-Apps Ihres Mandanten möglich ist. Falls Ihre Umgebung über eine lokale Active Directory-Instanz verfügt, können Sie SSO auf diese Geräte erweitern.

In diesem Artikel wird die entsprechende Vorgehensweise beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

 Wenn in Azure AD eingebundene Computer nicht mit dem Netzwerk Ihrer Organisation verbunden sind, ist ein VPN oder eine andere Netzwerkinfrastruktur erforderlich. Für das lokale einmalige Anmelden ist eine Kommunikation in Sichtverbindung mit Ihren lokalen AD DS-Domänencontrollern erforderlich.

## <a name="how-it-works"></a>Funktionsweise 

Da Sie sich nur einen Benutzernamen und das zugehörige Kennwort merken müssen, vereinfacht SSO den Zugriff auf Ihre Ressourcen und erhöht die Sicherheit Ihrer Umgebung. Mit einem in Azure AD eingebundenen Gerät verfügen Ihre Benutzer bereits über eine SSO-Umgebung für die Cloud-Apps in Ihrer Umgebung. Falls Ihre Umgebung über eine Azure AD-Instanz und eine lokale AD-Instanz verfügt, ist die Wahrscheinlichkeit hoch, dass Sie die SSO-Option auf Ihre lokalen Branchen-Apps, Dateifreigaben und Drucker erweitern möchten.

In Azure AD eingebundene Geräte haben keine Informationen zu Ihrer lokalen AD-Umgebung, da sie nicht darin eingebunden sind. Sie können aber mit Azure AD Connect zusätzliche Informationen zu Ihrer lokalen AD-Umgebung auf diesen Geräten bereitstellen.

Eine Umgebung, die sowohl über eine Azure AD-Instanz als auch eine lokale AD-Instanz verfügt, wird auch als Hybridumgebung bezeichnet. Bei Verwendung einer Hybridumgebung haben Sie Azure AD Connect wahrscheinlich bereits bereitgestellt, um Ihre lokalen Identitätsinformationen mit der Cloud zu synchronisieren. Im Rahmen des Synchronisierungsprozesses synchronisiert Azure AD Connect die lokalen Benutzerinformationen mit Azure AD. Wenn sich ein Benutzer in einer Hybridumgebung an einem in Azure AD eingebundenen Gerät anmeldet, ist der Ablauf wie folgt:

1. Azure AD sendet den Namen der lokalen Domäne, deren Mitglied der Benutzer ist, zurück an das Gerät.
1. Der Dienst für die lokale Sicherheitsautorität (Local Security Authority, LSA) ermöglicht die Kerberos-Authentifizierung auf dem Gerät.

Bei einem Zugriffsversuch auf eine Ressource, die Kerberos in der lokalen Umgebung des Benutzers anfordert, wird vom Gerät Folgendes ausgeführt:

1. Die Informationen der lokalen Domäne und die Benutzeranmeldeinformationen werden an den ermittelten DC gesendet, um den Benutzer zu authentifizieren.
1. Ein Kerberos [Ticket-Granting Ticket (TGT)](/windows/desktop/secauthn/ticket-granting-tickets) wird empfangen, das zum Zugreifen auf in AD eingebundene Ressourcen verwendet wird. Wenn der Versuch, das TGT für die AAD Connect-Domäne zu erhalten, fehlschlägt (das zugehörige DCLocator-Timeout kann zu einer Verzögerung führen), werden Wiederholungsversuche mithilfe von Einträgen der Anmeldeinformationsverwaltung unternommen, oder der Benutzer erhält ein Authentifizierungspopup, das Anmeldeinformationen für die Zielressource anfordert.

Alle Apps, die für die **integrierte Windows-Authentifizierung** konfiguriert sind, erhalten SSO auf nahtlose Weise, wenn ein Benutzer darauf zugreift.

Für Windows Hello for Business ist eine zusätzliche Konfiguration erforderlich, um lokales einmaliges Anmelden über ein in Azure AD eingebundenes Gerät zu ermöglichen. Weitere Informationen finden Sie unter [Configure Azure AD joined devices for On-premises Single-Sign On using Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base) (Konfigurieren von in Azure AD eingebundenen Geräten für lokales einmaliges Anmelden mit Windows Hello for Business). 

## <a name="what-you-get"></a>Ergebnis

Mit SSO haben Sie auf einem in Azure AD eingebundenen Gerät folgende Möglichkeiten: 

- Zugreifen auf einen UNC-Pfad auf einem AD-Mitgliedsserver
- Zugreifen auf einen AD-Mitgliedswebserver, der für integrierte Windows-Sicherheit konfiguriert ist 

Falls Sie Ihre lokale AD-Instanz über ein Windows-Gerät verwalten möchten, können Sie die [Remoteserver-Verwaltungstools für Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) installieren.

Verwenden Sie Folgendes:

- Snap-In „Active Directory-Benutzer und -Computer“ (ADUC) zum Verwalten aller AD-Objekte. Allerdings müssen Sie die Domäne, mit der eine Verbindung hergestellt werden soll, manuell angeben.
- DHCP-Snap-In zum Verwalten eines in AD eingebundenen DHCP-Servers. Allerdings müssen Sie ggf. den DHCP-Servernamen oder die -Adresse angeben.
 
## <a name="what-you-should-know"></a>Wichtige Informationen

Unter Umständen müssen Sie Ihre [domänenbasierte Filterung](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) in Azure AD Connect anpassen, um sicherzustellen, dass die Daten zu den erforderlichen Domänen synchronisiert werden.

Apps und Ressourcen, die von der Active Directory-Computerauthentifizierung abhängig sind, funktionieren nicht, da in Azure AD eingebundene Geräte nicht über ein Computerobjekt in AD verfügen. 

Es ist nicht möglich, Dateien auf einem in Azure AD eingebundenen Gerät für andere Benutzer freizugeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Worum handelt es sich bei der Geräteverwaltung in Azure Active Directory?](overview.md). 
