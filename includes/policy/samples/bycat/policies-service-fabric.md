---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/13/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 22f6f040d333a526940a688c16277be568520f50
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83651568"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |GitHub |
|---|---|---|---|---|
|[Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F617c02be-7f02-4efd-8836-3180d47b6c68) |Service Fabric bietet drei Schutzebenen („None“, „Sign“ und „EncryptAndSign“) für die Kommunikation zwischen zwei Knoten unter Verwendung eines primären Clusterzertifikats. Legen Sie die Schutzebene fest, um sicherzustellen, dass alle zwischen Knoten übertragenen Nachrichten verschlüsselt und digital signiert werden. |Audit, Deny, Disabled |1.1.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |
|[Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb54ed75b-3e1a-44ac-a333-05ba39b99ff0) |Hiermit wird überwacht, ob die Clientauthentifizierung in Service Fabric ausschließlich über Azure Active Directory erfolgt. |Audit, Deny, Disabled |1.1.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |
