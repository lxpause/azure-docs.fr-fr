---
title: Modèles Azure Resource Manager pour l’API Azure Cosmos DB pour MongoDB
description: Utilisez des modèles Azure Resource Manager pour créer et configurer l’API Azure Cosmos DB pour MongoDB.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: e0d28de67082c4b4d582f62653ad474e9ad5dadd
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73960578"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Gérer les ressources de l’API MongoDB Azure Cosmos DB à l’aide de modèles Azure Resource Manager

Cet article explique comment effectuer différentes opérations pour automatiser la gestion de bases de données, conteneurs et comptes Azure Cosmos DB avec des modèles Azure Resource Manager. Les exemples qu’il contient ne portent que sur l’API d’Azure Cosmos DB pour MongoDB. Pour obtenir des exemples liés à d’autres comptes de type d’API, consultez les articles Utiliser des modèles Azure Resource Manager avec l’API d’Azure Cosmos DB pour [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) et [Table](manage-table-with-resource-manager.md).

## Créer une API Azure Cosmos DB pour un compte, une base de données et une collection MongoDB<a id="create-resource"></a>

Créez des ressources Azure Cosmos DB à l’aide d’un modèle Azure Resource Manager. Ce modèle crée un compte Azure Cosmos pour l’API MongoDB avec deux collections qui partagent un débit de 400 RU/s au niveau de la base de données. Copiez le modèle et déployez-le comme indiqué ci-dessous ou consultez la [galerie Démarrage rapide Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/) et procédez au déploiement à partir du Portail Azure. Vous pouvez également télécharger le modèle sur votre ordinateur local ou créer un modèle et spécifier le chemin d’accès local avec le paramètre `--template-file`.

> [!NOTE]
> Les noms de comptes doivent être en minuscules et contenir au plus 44 caractères.
> Pour mettre à jour les RU/s, renvoyez le modèle avec des valeurs de propriété de débit mises à jour.

[!code-json[create-cosmos-mongo](~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json)]

### <a name="deploy-via-the-azure-cli"></a>Déployer par le biais de l’interface de ligne de commande Azure

Pour déployer le modèle Azure Resource Manager avec Azure CLI, **copiez** le script et sélectionnez **Essayer** pour ouvrir Azure Cloud Shell. Pour coller le script, cliquez avec le bouton droit dans l’interpréteur de commandes, puis sélectionnez **Coller** :

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

La commande `az cosmosdb show` montre le compte Azure Cosmos nouvellement créé, une fois que ce dernier a été configuré. Si vous choisissez d’utiliser une version de l’interface Azure CLI installée localement au lieu d’utiliser Cloud Shell, consultez l’article [Interface de ligne de commande Azure (CLI)](/cli/azure/).

## <a name="next-steps"></a>Étapes suivantes

Voici quelques ressources supplémentaires :

- [Documentation Azure Resource Manager](/azure/azure-resource-manager/)
- [Schéma de fournisseurs de ressources Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Modèles Démarrage rapide Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Résolution des erreurs courantes dans des déploiements Azure avec Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)