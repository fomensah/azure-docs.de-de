---
title: Hinzufügen von Benutzern als Lab-Ersteller in Azure Lab Services
description: In diesem Artikel erfahren Sie, wie Sie einen Benutzer der Rolle „Lab-Ersteller“ für ein Labkonto in Azure Lab Services hinzufügen. Die Lab-Ersteller können Labs in diesem Labkonto erstellen.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 0538747ec639b3fab1a7b38193796d80a7736170
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444927"
---
# <a name="add-lab-creators-to-a-lab-account-in-azure-lab-services"></a>Hinzufügen von Lab-Erstellern zu einem Labkonto in Azure Lab Services
In diesem Artikel erfahren Sie, wie Sie Benutzer als Lab-Ersteller zu einem Labkonto in Azure Lab Services hinzufügen. Diese Benutzer können dann Classroom-Labs im Labkonto erstellen. 

## <a name="add-microsoft-user-account-to-lab-creator-role"></a>Hinzufügen eines Microsoft-Benutzerkontos zur Rolle „Lab-Ersteller“
Zum Einrichten eines Classroom-Labs in einem Labkonto muss der Benutzer Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen des Labkontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Wenn Sie zum Erstellen eines Classroom-Labs das gleiche Benutzerkonto verwenden möchten, können Sie diesen Schritt überspringen. Führen Sie die folgenden Schritte aus, um zum Erstellen eines Classroom-Labs ein anderes Benutzerkonto zu verwenden: 

Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Klassen zu erteilen, fügen Sie sie zur Rolle **Lab-Ersteller** hinzu:

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und klicken Sie auf der Symbolleiste auf **+ Rollenzuweisung hinzufügen**. 

    ![Zugriffssteuerung > Schaltfläche „Rollenzuweisung hinzufügen“](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** als **Rolle** die Option **Ersteller des Labs** aus. Wählen Sie den Benutzer aus, den Sie der Rolle „Ersteller des Labs“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen des Lab-Erstellers](../media/tutorial-setup-lab-account/add-lab-creator.png)

    > [!NOTE]
    > Wenn Sie einen Benutzer ohne Microsoft-Konto als Lab-Ersteller hinzufügen, finden Sie weitere Informationen im Abschnitt [Hinzufügen eines Benutzers ohne Microsoft-Konto als Lab-Ersteller](#add-a-non-microsoft-account-user-as-a-lab-creator). 

## <a name="add-a-non-microsoft-account-user-as-a-lab-creator"></a>Hinzufügen eines Benutzers ohne Microsoft-Konto als Lab-Ersteller
Wenn Sie einen Benutzer als Lab-Ersteller hinzufügen möchten, verwenden Sie sein E-Mail-Konto. Die folgenden Arten von E-Mail-Konten können verwendet werden:

- Ein E-Mail-Konto, das vom Office 365-Azure Active Directory (AAD) Ihrer Universität bereitgestellt wird 
- Ein Microsoft-E-Mail-Konto, z. B. `@outlook.com`, `@hotmail.com`, `@msn.com` oder `@live.com`
- Ein Microsoft-fremdes E-Mail-Konto, z. B. ein von Yahoo oder Google bereitgestelltes. Diese Kontotypen müssen jedoch mit einem Microsoft-Konto verknüpft werden.
- Ein GitHub-Konto. Dieses Konto muss mit einem Microsoft-Konto verknüpft werden.

### <a name="using-a-non-microsoft-email-account"></a>Verwenden eines Microsoft-fremden E-Mail-Kontos
Lab-Ersteller/Kursleiter können Microsoft-fremde E-Mail-Konten verwenden, um sich für ein Classroom-Lab zu registrieren und sich bei diesem anzumelden.  Die Anmeldung beim Lab-Dienst-Portal erfordert jedoch, dass die Kursleiter zunächst ein Microsoft-Konto erstellen, das mit ihrer Microsoft-fremden E-Mail-Adresse verknüpft ist.

Viele Kursleiter verfügen möglicherweise bereits über ein Microsoft-Konto, das mit ihren Microsoft-fremden E-Mail-Adressen verknüpft ist. Beispielsweise verfügen Kursleiter bereits über ein Microsoft-Konto, wenn sie ihre E-Mail-Adresse mit anderen Microsoft-Produkten oder -Diensten wie Office, Skype, OneDrive oder Windows verwendet haben.  

Wenn sich ein Kursleiter beim Lab-Dienst-Portal anmeldet, wird er aufgefordert, seine E-Mail-Adresse und sein Kennwort einzugeben. Wenn der Kursleiter versucht, sich mit einem Microsoft-fremden Konto anzumelden, das nicht mit einem Microsoft-Konto verknüpft ist, erhält er die folgende Fehlermeldung: 

![Fehlermeldung](../media/how-to-configure-student-usage/cant-find-account.png)

Um sich für ein Microsoft-Konto anzumelden, sollten Kursleiter [http://signup.live.com](http://signup.live.com) besuchen.  


### <a name="using-a-github-account"></a>Verwenden eines GitHub-Kontos
Kursleiter können auch ein vorhandenes GitHub-Konto verwenden, um sich für ein Classroom-Lab zu registrieren und bei diesem anzumelden. Wenn der Kursleiter bereits über ein Microsoft-Konto verfügt, das mit seinem GitHub-Konto verknüpft ist, kann er sich anmelden und sein Kennwort angeben, wie im vorherigen Abschnitt gezeigt. Wenn er sein GitHub-Konto noch nicht mit einem Microsoft-Konto verknüpft hat, sollte er **Anmeldeoptionen** auswählen:

![Link „Anmeldeoptionen“](../media/how-to-configure-student-usage/signin-options.png)

Auf der Seite **Anmeldeoptionen** wählt er die Option **Mit GitHub anmelden** aus.

![Link „Mit GitHub anmelden“](../media/how-to-configure-student-usage/signin-github.png)

Schließlich wird er aufgefordert, ein Microsoft-Konto zu erstellen, das mit seinem GitHub-Konto verknüpft ist. Dies geschieht automatisch, wenn der Kursleiter **Weiter** auswählt.  Anschließend wird der Kursleiter sofort angemeldet und mit dem Classroom-Lab verbunden.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
