---
title: Déploiement conditionnel avec des modèles
description: Décrit comment déployer une ressource de manière conditionnelle dans un modèle Azure Resource Manager.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: 8ffff0794357ed772bcaea9dce1f93fc7601295a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476563"
---
# <a name="conditional-deployment-in-resource-manager-templates"></a>Déploiement conditionnel dans des modèles Azure Resource Manager

Parfois, vous devez déployer une ressource de manière conditionnelle dans un modèle. Pour spécifier si la ressource est déployée, utilisez l’élément `condition`. La valeur de cet élément est résolue en true ou false. Lorsque la valeur est true, la ressource est créée. Lorsque la valeur est false, la ressource n’est pas créée. La valeur ne peut être appliquée qu’à l’ensemble de la ressource.

## <a name="new-or-existing-resource"></a>Ressource nouvelle ou existante

Vous pouvez utiliser un déploiement conditionnel pour créer une ressource ou en utiliser une existante. L’exemple suivant montre comment utiliser condition pour déployer un nouveau compte de stockage ou utiliser un compte de stockage existant.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[parameters('location')]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Lorsque le paramètre **newOrExisting** a la valeur **new**, la condition donne le résultat true. Le compte de stockage est déployé. En revanche, quand le paramètre **newOrExisting** a la valeur **existing**, la condition donne le résultat false et le compte de stockage n’est pas déployé.

Pour un exemple de modèle complet qui utilise l’élément `condition`, consultez [VM with a new or existing Virtual Network, Storage, and Public IP](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) (Machine virtuelle avec un réseau virtuel, un stockage et une adresse IP publique nouveaux ou existants).

## <a name="allow-condition"></a>Autoriser une condition

Vous pouvez transmettre une valeur de paramètre indiquant si une condition est autorisée. L’exemple suivant déploie un serveur SQL et autorise éventuellement des adresses IP Azure.

```json
{
    "type": "Microsoft.Sql/servers",
    "name": "[parameters('serverName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('location')]",
    "properties": {
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
        "version": "12.0"
    },
    "resources": [
        {
            "condition": "[parameters('allowAzureIPs')]",
            "type": "firewallRules",
            "name": "AllowAllWindowsAzureIps",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/', parameters('serverName'))]"
            ],
            "properties": {
                "endIpAddress": "0.0.0.0",
                "startIpAddress": "0.0.0.0"
            }
        }
    ]
}
```

Pour le modèle complet, voir [Serveur logique SQL Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

## <a name="runtime-functions"></a>Fonctions de runtime

Si vous utilisez une fonction de [référence](template-functions-resource.md#reference) ou de [liste](template-functions-resource.md#list) avec une ressource qui est déployée conditionnellement, la fonction est évaluée même si la ressource n’est pas déployée. Vous obtenez une erreur si la fonction fait référence à une ressource qui n’existe pas.

Utilisez la fonction [if](template-functions-logical.md#if) pour vous assurer que la fonction est évaluée uniquement pour les conditions lorsque la ressource est déployée. Consultez la [fonction if](template-functions-logical.md#if) pour un exemple de modèle qui utilise « if » et « reference » avec une ressource déployée de manière conditionnelle.

Vous définissez une [ressource comme étant dépendante](define-resource-dependency.md) d’une ressource conditionnelle exactement comme vous le feriez pour une autre ressource. Quand une ressource conditionnelle n’est pas déployée, Azure Resource Manager la supprime automatiquement des dépendances nécessaires.

## <a name="condition-with-complete-mode"></a>Condition avec le mode complet

Si vous déployez un modèle en [mode complet](deployment-modes.md) et qu’une ressource n’est pas déployée parce que la condition donne false, le résultat dépend de la version de l’API REST utilisée. Si vous utilisez une version antérieure à 2019-05-10, la ressource **n’est pas supprimée**. Avec 2019-05-10 ou ultérieur, elle **est supprimée**. Les dernières versions d’Azure PowerShell et d’Azure CLI suppriment la ressource lorsque la condition a la valeur false.

## <a name="next-steps"></a>Étapes suivantes

* Pour obtenir des recommandations sur la création de modèles, consultez [Bonnes pratiques relatives aux modèles Azure Resource Manager](template-best-practices.md).
* Pour créer plusieurs instances d’une ressource, voir [Itération de variable, de propriété ou de ressource dans les modèles Azure Resource Manager](create-multiple-instances.md).