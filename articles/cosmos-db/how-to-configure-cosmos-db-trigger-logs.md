---
title: Configurer et lire les journaux lors de l’utilisation du déclencheur Azure Functions pour Cosmos DB
description: Découvrez comment exposer les journaux au pipeline de journalisation Azure Functions lors de l’utilisation du déclencheur Azure Functions pour Cosmos DB
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 17b2d4854e3222e5bdb607c6c80b0ec5ed6177d3
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70093598"
---
# <a name="how-to-configure-and-read-the-logs-when-using-azure-functions-trigger-for-cosmos-db"></a>Guide pratique pour configurer et lire les journaux lors de l’utilisation du déclencheur Azure Functions pour Cosmos DB

Cet article explique comment configurer votre environnement Azure Functions pour envoyer les journaux du déclencheur Azure Functions pour Cosmos DB à la [solution de supervision](../azure-functions/functions-monitoring.md) que vous avez configurée.

## <a name="included-logs"></a>Journaux inclus

Le déclencheur Azure Functions pour Cosmos DB utilise la [bibliothèque du processeur de flux de modification](./change-feed-processor.md) en interne, qui génère plusieurs journaux de contrôle de l’intégrité vous permettant de superviser les opérations internes dans le cadre du [dépannage](./troubleshoot-changefeed-functions.md).

Les journaux de contrôle de l’intégrité décrivent le comportement du déclencheur Azure Functions pour Cosmos DB lors des tentatives d’opérations dans des scénarios d’équilibrage de charge ou d’initialisation.

## <a name="enabling-logging"></a>Activation de la journalisation

Pour activer la journalisation en cas d’utilisation du déclencheur Azure Functions pour Cosmos DB, recherchez le fichier `host.json` dans votre projet ou application Azure Functions et [configurez le niveau de journalisation nécessaire](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson). Vous devez activer les traces pour `Host.Triggers.CosmosDB` comme indiqué dans l’exemple suivant :

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Une fois qu’Azure Functions a été déployé avec la configuration mise à jour, les journaux du déclencheur Azure Functions pour Cosmos DB sont inclus dans les traces. Vous pouvez consulter les journaux dans le fournisseur de journalisation configuré sous *Category* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Interroger les journaux

Exécutez la requête suivante pour interroger les journaux générés par le déclencheur Azure Functions pour Cosmos DB dans l’outil [Azure Application Insights Analytics](../azure-monitor/app/analytics.md) :

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Étapes suivantes

* [Activez la supervision](../azure-functions/functions-monitoring.md) dans vos applications Azure Functions.
* Découvrez comment [diagnostiquer et résoudre les problèmes courants](./troubleshoot-changefeed-functions.md) quand vous utilisez le déclencheur Azure Functions pour Cosmos DB.