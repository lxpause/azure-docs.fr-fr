---
title: Activité de flux de données dans Azure Data Factory
description: Comment exécuter des flux de données à l’intérieur d’un pipeline de fabrique de données.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 10/07/2019
ms.openlocfilehash: 47126d1cf51f4b27863bb0b11e73cfe5592b8d57
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74929882"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Activité de flux de données dans Azure Data Factory

Utilisez l’activité de flux de données pour transformer et déplacer des données par le biais des flux de données de mappage. Si vous débutez avec les flux de données, consultez [Vue d’ensemble des flux de données de mappage](concepts-data-flow-overview.md)

## <a name="syntax"></a>Syntaxe

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Propriétés type

Propriété | Description | Valeurs autorisées | Obligatoire
-------- | ----------- | -------------- | --------
dataflow | Référence au flux de données en cours d’exécution | DataFlowReference | OUI
integrationRuntime | Environnement de calcul sur lequel le flux de données s’exécute | IntegrationRuntimeReference | OUI
staging.linkedService | Si vous utilisez une source ou un récepteur SQL DW, le compte de stockage utilisé pour la préproduction de PolyBase | LinkedServiceReference | Uniquement si le flux de données lit ou écrit dans un entrepôt de données SQL DW
staging.folderPath | Si vous utilisez une source ou un récepteur SQL DW, chemin du dossier dans le compte de stockage blob utilisé pour la préproduction de PolyBase | Chaîne | Uniquement si le flux de données lit ou écrit dans un entrepôt de données SQL DW

![Exécuter un flux de données](media/data-flow/activity-data-flow.png "Exécuter un flux de données")

### <a name="data-flow-integration-runtime"></a>Runtime d’intégration de flux de données

Choisissez le runtime d’intégration à utiliser pour l’exécution de votre activité de flux de données. Par défaut, Data Factory utilise le runtime d’intégration Azure à résolution automatique avec quatre cœurs worker et aucune durée de vie (TTL). Ce runtime d’intégration a un type de calcul à usage général et s’exécute dans la même région que votre fabrique. Vous pouvez créer votre propre runtime d’intégration Azure qui définit des régions spécifiques, un type de calcul, un nombre de cœurs et une durée de vie pour l’exécution de votre activité de flux de données.

Pour les exécutions de pipeline, le cluster est un cluster de travail, dont le démarrage prend quelques minutes avant que l’exécution commence. Si aucune durée de vie n’est spécifiée, ce temps de démarrage est nécessaire sur chaque exécution de pipeline. Si vous spécifiez une durée de vie, un pool de clusters à chaud reste actif pendant la durée spécifiée après la dernière exécution, ce qui donne lieu à des temps de démarrage plus courts. Par exemple, si vous avez une durée de vie de 60 minutes et que vous exécutez un flux de données une fois par heure, le pool de clusters reste actif. Pour plus d’informations, consultez [Runtime d’intégration Azure](concepts-integration-runtime.md).

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!NOTE]
> La sélection du runtime d’intégration dans l’activité de flux de données s’applique uniquement aux *exécutions déclenchées* de votre pipeline. Le débogage de votre pipeline avec des flux de données s’exécute sur le cluster spécifié dans la session de débogage.

### <a name="polybase"></a>PolyBase

Si vous utilisez Azure SQL Data Warehouse comme récepteur ou source, vous devez choisir un emplacement de préproduction pour le chargement par lots PolyBase. PolyBase permet d’effectuer des chargements par lots en bloc au lieu de charger les données ligne par ligne. PolyBase réduit considérablement le temps de chargement dans l’instance SQL DW.

## <a name="parameterizing-data-flows"></a>Paramétrage de flux de données

### <a name="parameterized-datasets"></a>Jeux de données paramétrables

Si votre flux de données utilise des jeux de données paramétrables, définissez les valeurs de paramètre sous l’onglet **Paramètres**.

![Exécuter des paramètres de flux de données](media/data-flow/params.png "parameters")

### <a name="parameterized-data-flows"></a>Flux de données paramétrables

Si votre flux de données est paramétré, définissez les valeurs dynamiques de ses paramètres sous l’onglet **Paramètres**. Vous pouvez utiliser le langage d’expression de pipeline ADF (uniquement pour les types Chaîne) ou le langage d’expression de flux de données pour affecter des valeurs de paramètres littérales ou statiques. Pour plus d’informations, consultez [Paramètres de flux de données](parameters-data-flow.md).

![Exemple d’exécution de paramètres de flux de données](media/data-flow/parameter-example.png "Exemple de paramètres")

## <a name="pipeline-debug-of-data-flow-activity"></a>Débogage de pipeline de l’activité de flux de données

Pour effectuer une exécution de débogage de pipeline à l’aide d’une activité de flux de données, vous devez passer en mode de débogage de flux de données à l’aide du curseur **Débogage de flux de données** dans la barre supérieure. Le mode débogage vous permet d’exécuter le flux de données sur un cluster Spark actif. Pour plus d’informations, consultez [Mode de débogage](concepts-data-flow-debug-mode.md).

![Bouton Déboguer](media/data-flow/debugbutton.png "Bouton Déboguer")

Le pipeline de débogage s’exécute sur le cluster de débogage actif, et non sur l’environnement de runtime d’intégration spécifié dans les paramètres d’activité de flux de données. Vous pouvez choisir l’environnement de calcul de débogage lors du démarrage du mode de débogage.

## <a name="monitoring-the-data-flow-activity"></a>Supervision de l’activité de flux de données

L’activité de flux de données offre une expérience de supervision spéciale dans laquelle vous pouvez voir des informations relatives au partitionnement, au temps de phase et à la traçabilité des données. Ouvrez le volet de supervision à l’aide de l’icône de lunettes sous **Actions**. Pour plus d’informations, consultez [Supervision des flux de données](concepts-data-flow-monitoring.md).

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>Utiliser des résultats d’activité de flux de données dans une activité postérieure

L’activité de flux de données génère des métriques sur le nombre de lignes écrites dans chaque récepteur et le nombre de lignes lues à partir de chaque source. Ces résultats sont retournés dans la section `output` du résultat de l’exécution d’activité. Les métriques retournées sont au format du fichier json ci-dessous.

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

Par exemple, pour obtenir le nombre de lignes écrites dans un récepteur nommé « sink1 » dans une activité nommée « dataflowActivity », utilisez `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten`.

Pour obtenir le nombre de lignes lues à partir d’une source nommée « source1 » qui a été utilisée dans ce récepteur, utilisez `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead`.

> [!NOTE]
> Si un récepteur n’a aucune ligne écrite, il n’apparaît pas dans les métriques. Son existence peut être vérifiée à l’aide de la fonction `contains`. Par exemple, `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')` vérifie si des lignes ont été écrites dans sink1.

## <a name="next-steps"></a>Étapes suivantes

Consultez les activités de flux de contrôle prises en charge par Data Factory : 

- [Activité IfCondition](control-flow-if-condition-activity.md)
- [Activité d’exécution du pipeline](control-flow-execute-pipeline-activity.md)
- [Pour chaque activité](control-flow-for-each-activity.md)
- [Activité d’obtention des métadonnées](control-flow-get-metadata-activity.md)
- [Activité de recherche](control-flow-lookup-activity.md)
- [Activité Web](control-flow-web-activity.md)
- [Activité jusqu’à](control-flow-until-activity.md)
