---
title: Fonctions définies par l’utilisateur dans les modèles
description: Explique comment définir et utiliser des fonctions définies par l’utilisateur dans un modèle Azure Resource Manager.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: e2e68c6f6b9fc9eb40d282d87572ce1e805a11a8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476159"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Fonctions définies par l’utilisateur dans un modèle Azure Resource Manager

Dans votre modèle, vous pouvez créer vos propres fonctions. Ces fonctions peuvent être utilisées dans votre modèle. Les fonctions définies par l’utilisateur sont distinctes des [fonctions de modèles standard](template-functions.md) qui sont automatiquement disponibles dans votre modèle. Créez vos propres fonctions quand vous avez des expressions complexes qui sont utilisées de manière répétitive dans votre modèle.

Cet article explique comment ajouter des fonctions définies par l’utilisateur dans votre modèle Azure Resource Manager.

## <a name="define-the-function"></a>Définir la fonction

Vos fonctions requièrent une valeur pour l’espace de noms afin d’éviter tout conflit avec les fonctions de modèle. L’exemple suivant illustre une fonction qui renvoie un nom de compte de stockage :

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>Utiliser la fonction

L’exemple suivant montre comment appeler votre fonction.

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="limitations"></a>Limites

La définition d’une fonction utilisateur est soumise à certaines restrictions :

* La fonction ne peut pas accéder aux variables.
* La fonction ne peut utiliser que des paramètres définis dans l’autre fonction. Quand vous utilisez la fonction [parameters](template-functions-deployment.md#parameters) dans une fonction définie par l’utilisateur, vous êtes limité aux paramètres de cette fonction.
* La fonction ne peut pas appeler d’autres fonctions définies par l’utilisateur.
* La fonction ne peut pas utiliser la fonction [reference](template-functions-resource.md#reference) ni aucune des fonctions [list](template-functions-resource.md#list).
* Les paramètres de la fonction ne peuvent pas avoir de valeur par défaut.


## <a name="next-steps"></a>Étapes suivantes

* Pour en savoir plus sur les propriétés disponibles pour les fonctions définies par l’utilisateur, consultez [Comprendre la structure et la syntaxe des modèles Azure Resource Manager](template-syntax.md).
* Pour obtenir la liste complète des fonctions de modèles disponibles, consultez [Fonctions des modèles Azure Resource Manager](template-functions.md).
