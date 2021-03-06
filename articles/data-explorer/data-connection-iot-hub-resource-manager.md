---
title: Créer une connexion de données IoT Hub pour Azure Data Explorer à l’aide d’un modèle Azure Resource Manager
description: Dans cet article, vous allez apprendre à créer une connexion de données IoT Hub pour Azure Data Explorer à l’aide d’un modèle Azure Resource Manager.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/28/2019
ms.openlocfilehash: 42b9f34802b8e6344f9008bf26a8bcc9f554adfb
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74669181"
---
# <a name="create-an-iot-hub-data-connection-for-azure-data-explorer-by-using-azure-resource-manager-template"></a>Créer une connexion de données IoT Hub pour Azure Data Explorer à l’aide d’un modèle Azure Resource Manager

> [!div class="op_single_selector"]
> * [Portal](ingest-data-iot-hub.md)
> * [C#](data-connection-iot-hub-csharp.md)
> * [Python](data-connection-iot-hub-python.md)
> * [Modèle Azure Resource Manager](data-connection-iot-hub-resource-manager.md)

L’Explorateur de données Azure est un service d’exploration de données rapide et hautement évolutive pour les données des journaux et les données de télémétrie. Azure Data Explorer offre une ingestion (chargement de données) à partir de hubs d’événements, de hubs IoT et d’objets blob écrits dans des conteneurs d’objets blob. Dans cet article, vous créez une connexion de données IoT Hub pour Azure Data Explorer à l’aide d’un modèle Azure Resource Manager.

## <a name="prerequisites"></a>Prérequis

* Si vous n’avez pas d’abonnement Azure, créez un [compte Azure gratuit](https://azure.microsoft.com/free/) avant de commencer.
* Créez [un cluster et une base de données](create-cluster-database-portal.md).
* Créez [une table et un mappage de colonnes](ingest-data-iot-hub.md#create-a-target-table-in-azure-data-explorer).
* Créez un [hub IoT avec une stratégie d’accès partagé configurée](ingest-data-iot-hub.md#create-an-iot-hub).

## <a name="azure-resource-manager-template-for-adding-an-iot-hub-data-connection"></a>Modèle Azure Resource Manager pour l’ajout d’une connexion de données IoT Hub

L’exemple suivant montre un modèle Azure Resource Manager pour l’ajout d’une connexion de données IoT Hub.  Vous pouvez [modifier et déployer le modèle dans le portail Azure](/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal#edit-and-deploy-the-template) en utilisant le formulaire.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "IotHubs_iothubdemo_name": {
            "type": "string",
            "defaultValue": "iothubdemo",
            "metadata": {
                "description": "Specifies the IoT Hub name."
            }
        },
        "iothubpolices_iothubowner_name": {
            "type": "string",
            "defaultValue": "iothubowner",
            "metadata": {
                "description": "Specifies the shared access policy name."
            }
        },
        "consumergroup_default_name": {
            "type": "string",
            "defaultValue": "$Default",
            "metadata": {
                "description": "Specifies the consumer group of the IoT Hub."
            }
        },
        "Clusters_kustocluster_name": {
            "type": "string",
            "defaultValue": "kustocluster",
            "metadata": {
                "description": "Specifies the name of the cluster"
            }
        },
        "databases_kustodb_name": {
            "type": "string",
            "defaultValue": "kustodb",
            "metadata": {
                "description": "Specifies the name of the database"
            }
        },
        "tables_kustotable_name": {
            "type": "string",
            "defaultValue": "kustotable",
            "metadata": {
                "description": "Specifies the name of the table"
            }
        },
        "mapping_kustomapping_name": {
            "type": "string",
            "defaultValue": "kustomapping",
            "metadata": {
                "description": "Specifies the name of the mapping rule"
            }
        },
        "dataformat_type": {
            "type": "string",
            "defaultValue": "csv",
            "metadata": {
                "description": "Specifies the data format"
            }
        },
        "dataconnections_kustodc_name": {
            "type": "string",
            "defaultValue": "kustodc",
            "metadata": {
                "description": "Name of the data connection to create"
            }
        },
        "subscriptionId": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "Specifies the subscriptionId of the IoT Hub"
            }
        },
        "resourceGroup": {
            "type": "string",
            "defaultValue": "[resourceGroup().name]",
            "metadata": {
                "description": "Specifies the resourceGroup of the IoT Hub"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "variables": {
    },
    "resources": [{
            "type": "Microsoft.Kusto/Clusters/Databases/DataConnections",
            "apiVersion": "2019-09-07",
            "name": "[concat(parameters('Clusters_kustocluster_name'), '/', parameters('databases_kustodb_name'), '/', parameters('dataconnections_kustodc_name'))]",
            "location": "[parameters('location')]",
            "kind": "IotHub",
            "properties": {
                "iotHubResourceId": "[resourceId(parameters('subscriptionId'), parameters('resourceGroup'), 'Microsoft.Devices/IotHubs', parameters('IotHubs_iothubdemo_name'))]",
                "consumerGroup": "[parameters('consumergroup_default_name')]",
                "sharedAccessPolicyName": "[parameters('iothubpolices_iothubowner_name')]",
                "tableName": "[parameters('tables_kustotable_name')]",
                "mappingRuleName": "[parameters('mapping_kustomapping_name')]",
                "dataFormat": "[parameters('dataformat_type')]"
            }
        }
    ]
}
```

[!INCLUDE [data-explorer-clean-resources](../../includes/data-explorer-clean-resources.md)]
