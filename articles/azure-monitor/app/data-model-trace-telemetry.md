---
title: Azure Application Insights-Datenmodell – Ablaufverfolgungstelemetrie
description: Application Insights-Datenmodell für Ablaufverfolgungstelemetrie
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 31958b26cdb8a7897cf0051af6600014c07949fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671952"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Ablaufverfolgungstelemetrie: Application Insights-Datenmodell

Ablaufverfolgungstelemetrie stellt (in [Application Insights](../../azure-monitor/app/app-insights-overview.md)) Ablaufverfolgungsanweisungen im Format `printf` dar, die sich für eine Textsuche eignen. `Log4Net`, `NLog` und andere textbasierte Protokolldateieinträge werden in Instanzen dieses Typs übersetzt. Die Ablaufverfolgung weist für die Erweiterbarkeit keine Messungen auf.

## <a name="message"></a>`Message`

Ablaufverfolgungsmeldung.

Maximale Länge: 32.768 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Untersuchen von .NET-Ablaufverfolgungsprotokollen in Application Insights](../../azure-monitor/app/asp-net-trace-logs.md).
- [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](../../azure-monitor/app/java-trace-logs.md).
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](data-model.md).
- [Schreiben benutzerdefinierter Telemetriedaten für die Ablaufverfolgung](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](../../azure-monitor/app/platforms.md).
