---
title: Upgraden einer öffentlichen Load Balancer-Instanz von Basic auf Standard – Azure Load Balancer
description: In diesem Artikel erfahren Sie, wie Sie eine öffentliche Azure Load Balancer-Instanz von der Basic- auf die Standard-SKU upgraden.
services: load-balancer
author: irenehua
ms.service: load-balancer
ms.topic: article
ms.date: 01/23/2020
ms.author: irenehua
ms.openlocfilehash: f1c85c98b9ea0dcaa6498622a0667bffba080401
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858392"
---
# <a name="upgrade-azure-public-load-balancer"></a>Upgraden einer öffentlichen Azure Load Balancer-Instanz
[Azure Load Balancer Standard](load-balancer-overview.md) bietet umfangreiche Funktionen sowie Hochverfügbarkeit durch Zonenredundanz. Weitere Informationen zu Load Balancer-SKUs finden Sie in der [Vergleichstabelle](https://docs.microsoft.com/azure/load-balancer/skus#skus).

Ein Upgrade umfasst drei Phasen:

1. Migrieren der Konfiguration
2. Hinzufügen virtueller Computer zu Back-End-Pools von Load Balancer Standard

Dieser Artikel behandelt die Migration einer Konfiguration. Die Vorgehensweise zu Hinzufügen virtueller Computer zu Back-End-Pools kann abhängig von Ihrer spezifischen Umgebung variieren. Einige generelle Empfehlungen dazu finden Sie jedoch [hier](#add-vms-to-backend-pools-of-standard-load-balancer).

## <a name="upgrade-overview"></a>Upgradeübersicht

Es gibt ein Azure PowerShell-Skript, in dem folgende Vorgänge ausgeführt werden:

* Erstellt eine Load Balancer-Instanz mit Standard-SKU in der angegebenen Ressourcengruppe und am angegebenen Standort.
* Nahtloses Kopieren der Konfigurationen der Load Balancer-Instanz mit Basic-SKU in die neu erstellte Load Balancer Standard-Instanz.
* Erstellt eine Standardausgangsregel, die ausgehende Konnektivität ermöglicht.

### <a name="caveatslimitations"></a>Vorbehalte/Einschränkungen

* Das Skript unterstützt nur Upgrades für öffentliche Load Balancer-Instanzen. Anleitungen für ein Upgrade des internen Load Balancers im Tarif „Basic“ finden Sie auf [dieser Seite](https://docs.microsoft.com/azure/load-balancer/upgrade-basicinternal-standard).
* Die Load Balancer Standard-Instanz hat eine neue öffentliche Adresse. Die mit der vorhandenen Load Balancer Basic-Instanz verknüpften IP-Adressen können aufgrund der abweichenden SKUs nicht nahtlos auf die Load Balancer Standard-Instanz übertragen werden.
* Wenn die Load Balancer Standard-Instanz in einer anderen Region erstellt wird, können die virtuellen Computer aus der alten Region nicht der neu erstellten Load Balancer Standard-Instanz zugeordnet werden. Erstellen Sie zur Umgehung dieser Einschränkung einen neuen virtuellen Computer in der neuen Region.
* Wenn Ihre Load Balancer-Instanz über keine Front-End-IP-Konfiguration oder über keinen Back-End-Pool verfügt, tritt beim Ausführen des Skripts wahrscheinlich ein Fehler auf. Stellen Sie sicher, dass die Angaben vorhanden sind.

## <a name="download-the-script"></a>Herunterladen des Skripts

Laden Sie das Migrationsskript aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzurePublicLBUpgrade/2.0) herunter.
## <a name="use-the-script"></a>Verwenden des Skripts

Je nach dem, wie Ihre lokale PowerShell-Umgebung eingerichtet und eingestellt ist, gibt es zwei Optionen für Sie:

* Haben Sie die Azure Az-Module nicht installiert, oder haben Sie kein Problem damit, die Azure Az-Module zu deinstallieren, sollten Sie die `Install-Script`-Option verwenden, um das Skript auszuführen.
* Wenn Sie die Azure Az-Module beibehalten müssen, besteht die beste Vorgehensweise darin, das Skript herunterzuladen und direkt auszuführen.

Um festzustellen, ob Sie die Azure Az-Module installiert haben, führen Sie `Get-InstalledModule -Name az` aus. Werden keine installierten Az-Module angezeigt, können Sie die `Install-Script`-Methode verwenden.

### <a name="install-using-the-install-script-method"></a>Installieren mit der Install-Script-Methode

Damit Sie diese Option nutzen können, dürfen keine Azure Az-Module auf Ihrem Computer installiert sein. Wenn diese installiert sind, zeigt der folgende Befehl einen Fehler an. Sie können entweder die Azure Az-Module deinstallieren oder die andere Option nutzen, um das Skript manuell herunterzuladen und es auszuführen.
  
Führen Sie das Skript mit dem folgenden Befehl aus:

`Install-Script -Name AzurePublicLBUpgrade`

Mit diesem Befehl werden auch die erforderlichen Az-Module installiert.  

### <a name="install-using-the-script-directly"></a>Installieren durch direktes Verwenden des Skripts

Haben Sie einige Azure Az-Module installiert, und können (oder möchten) Sie diese nicht deinstallieren, laden Sie das Skript manuell herunter, indem Sie die Registerkarte **Manual Download** im Skript-Downloadlink verwenden. Das Skript wird als reine NUPKG-Datei heruntergeladen. Informationen, wie das Skript aus dieser NUPKG-Datei installiert wird, finden Sie unter [Manuelles Herunterladen des Pakets](/powershell/scripting/gallery/how-to/working-with-packages/manual-download).

So führen Sie das Skript aus

1. Verwenden Sie `Connect-AzAccount`, um eine Verbindung mit Azure herzustellen.

1. Verwenden Sie `Import-Module Az`, um die Az-Module zu importieren.

1. Untersuchen Sie die erforderlichen Parameter:

   * **oldRgName: [Zeichenfolge]: Erforderlich:** Die Ressourcengruppe für die vorhandene Load Balancer Basic-Instanz, die Sie upgraden möchten. Wählen Sie zur Ermittlung dieses Zeichenfolgenwerts im Azure-Portal Ihre Load Balancer Basic-Quelle aus, und klicken Sie auf die **Übersicht** für den Lastenausgleich. Die Ressourcengruppe befindet sich auf dieser Seite.
   * **oldLBName: [Zeichenfolge]: Erforderlich:** Der Name der vorhandene Load Balancer Basic-Instanz, die Sie upgraden möchten. 
   * **newrgName: [Zeichenfolge]: Erforderlich:** Die Ressourcengruppe, in der die Load Balancer Standard-Instanz erstellt wird. Hierbei kann es sich um eine neue oder um eine bereits vorhandene Ressourcengruppe handeln. Beachten Sie bei Verwendung einer vorhandenen Ressourcengruppe, dass der Name der Load Balancer-Instanz innerhalb der Ressourcengruppe eindeutig sein muss. 
   * **newlocation: [Zeichenfolge]: Erforderlich:** Der Standort, an dem die Load Balancer Standard-Instanz erstellt wird. Es empfiehlt sich, für die Load Balancer Standard-Instanz den Standort der gewählten Load Balancer Basic-Instanz zu übernehmen, um die Verknüpfung mit anderen vorhandenen Ressourcen zu erleichtern.
   * **newLBName: [Zeichenfolge]: Erforderlich:** Der Name der zu erstellenden Load Balancer Standard-Instanz.
1. Führen Sie das Skript mit den entsprechenden Parametern aus. Es kann fünf bis sieben Minuten dauern, bis es fertig ist.

    **Beispiel**

   ```azurepowershell
   AzurePublicLBUpgrade.ps1 -oldRgName "test_publicUpgrade_rg" -oldLBName "LBForPublic" -newrgName "test_userInput3_rg" -newlocation "centralus" -newLbName "LBForUpgrade"
   ```

### <a name="add-vms-to-backend-pools-of-standard-load-balancer"></a>Hinzufügen virtueller Computer zu Back-End-Pools von Load Balancer Standard

Überprüfen Sie zunächst, ob durch das Skript erfolgreich eine neue öffentliche Load Balancer Standard-Instanz mit exakt der migrierten Konfiguration Ihrer öffentlichen Load Balancer Basic-Instanz erstellt wurde. Sie können dies über das Azure-Portal prüfen.

Senden Sie zum manuellen Testen etwas Datenverkehr über die Load Balancer Standard-Instanz.
  
In den folgenden Szenarien wird gezeigt, wie Sie virtuelle Computer zu Back-End-Pools der neu erstellten öffentlichen Load Balancer Standard-Instanz hinzufügen und sie konfigurieren (einschließlich Empfehlungen):

* **Verschieben vorhandener virtueller Computer aus Back-End-Pools der alten öffentlichen Load Balancer Basic-Instanz in Back-End-Pools einer neu erstellten öffentlichen Load Balancer Standard-Instanz:**
    1. Melden Sie sich zur Durchführung der Aufgaben dieser Schnellstartanleitung am [Azure-Portal](https://portal.azure.com) an.
 
    1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die **neu erstellte Load Balancer Standard-Instanz** aus.
   
    1. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools**.
   
    1. Wählen Sie den Back-End-Pool aus, der dem Back-End-Pool der Load Balancer Basic-Instanz entspricht. Wählen Sie den folgenden Wert aus: 
      - **Virtual Machine** (Virtueller Computer): Wählen Sie in der Dropdownliste die virtuellen Computer aus dem entsprechenden Back-End-Pool der Load Balancer Basic-Instanz aus.
    1. Wählen Sie **Speichern** aus.
    >[!NOTE]
    >Für virtuelle Computer mit öffentlichen IP-Adressen müssen zuerst Standard-IP-Adressen erstellt werden. Hierbei können allerdings keine identischen IP-Adressen garantiert werden. Heben Sie die Zuordnung der virtuellen Computer zu Basic-IP-Adressen auf, und ordnen Sie sie den neu erstellten Standard-IP-Adressen zu. Anschließend können Sie die Schritte zum Hinzufügen von virtuellen Computern zum Back-End-Pool der Load Balancer Standard-Instanz ausführen. 

* **Erstellen neuer virtueller Computer, um sie den Back-End-Pools der neu erstellten öffentlichen Load Balancer Standard-Instanz hinzuzufügen:**
    * Weitere Informationen zum Erstellen eines virtuellen Computers sowie zum Zuordnen des virtuellen Computers zu Load Balancer Standard finden Sie [hier](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal#create-virtual-machines).

### <a name="create-an-outbound-rule-for-outbound-connection"></a>Erstellen einer Ausgangsregel für ausgehende Verbindungen

Befolgen Sie die [Anweisungen](https://docs.microsoft.com/azure/load-balancer/configure-load-balancer-outbound-portal#create-outbound-rule-configuration), um eine Ausgangsregel zu erstellen, die Ihnen Folgendes ermöglicht:
* Eine ausgehende NAT von Grund auf neu definieren
* Das Verhalten einer vorhandenen NAT für ausgehenden Datenverkehr skalieren und optimieren

## <a name="common-questions"></a>Häufig gestellte Fragen

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Gibt es irgendwelche Einschränkungen mit dem Azure PowerShell-Skript zum Migrieren der Konfiguration von v1 zu v2?

Ja. Lesen Sie [Vorbehalte/Einschränkungen](#caveatslimitations).

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-basic-load-balancer-to-the-newly-created-standard-load-balancer"></a>Leitet das Azure PowerShell-Skript auch den Datenverkehr von meiner Load Balancer Basic-Instanz zur neu erstellten Load Balancer Standard-Instanz um?

Nein. Das Azure PowerShell-Skript migriert nur die Konfiguration. Die Migration des Datenverkehrs liegt in Ihrer Verantwortung und muss von Ihnen gesteuert werden.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Ich hatte beim Verwenden dieses Skripts einige Probleme. Wie erhalte ich Hilfe?
  
Sie können eine E-Mail an slbupgradesupport@microsoft.com senden, eine Supportanfrage beim Azure-Support öffnen, oder beides tun.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Standard Load Balancer](load-balancer-overview.md)
