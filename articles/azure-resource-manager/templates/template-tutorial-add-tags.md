---
title: Tutoriel - Ajouter des étiquettes aux ressources dans un modèle
description: Ajoutez des étiquettes aux ressources que vous déployez dans votre modèle Azure Resource Manager. Les étiquettes vous permettent d’organiser logiquement les ressources.
author: mumian
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ed5e5c63bed90136e498491a6735cbaa127cf1f9
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75689681"
---
# <a name="tutorial-add-tags-in-your-resource-manager-template"></a>Tutoriel : Ajouter des étiquettes à votre modèle Resource Manager

Dans ce tutoriel, vous apprenez à ajouter des étiquettes aux ressources dans votre modèle. Les [étiquettes](../management/tag-resources.md) vous aident à organiser logiquement vos ressources. Les valeurs des étiquettes apparaissent dans les rapports financiers. Ce tutoriel dure environ **8 minutes**.

## <a name="prerequisites"></a>Conditions préalables requises

Nous vous recommandons de suivre le [tutoriel sur les modèles de démarrage rapide](template-tutorial-quickstart-template.md), mais il n’est pas obligatoire.

Vous devez disposer de Visual Studio Code avec l’extension Outils Resource Manager et, au choix, d’Azure PowerShell ou d’Azure CLI. Pour plus d’informations, consultez les [outils de modèle](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Vérifier le modèle

Votre modèle précédent a déployé un compte de stockage, un plan App Service et une application web.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json)]

Après avoir déployé ces ressources, vous pouvez avoir besoin d’effectuer un suivi des coûts, et de rechercher les ressources qui appartiennent à une catégorie. La possibilité d’ajouter des étiquettes vous permet de résoudre ces problèmes.

## <a name="add-tags"></a>Ajouter des étiquettes

Vous étiquetez des ressources pour ajouter des valeurs qui vous aident à identifier leur utilisation. Par exemple, vous pouvez ajouter des étiquettes qui listent l’environnement et le projet. Vous pouvez ajouter des étiquettes qui identifient un centre de coûts ou l’équipe propriétaire de la ressource. Ajoutez des valeurs significatives pour votre organisation.

L’exemple suivant met en évidence les modifications apportées au modèle. Copiez l’intégralité du fichier et remplacez votre modèle par son contenu.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json?range=1-118&highlight=46-52,64,86,103)]

## <a name="deploy-template"></a>Déployer un modèle

Le moment est venu de déployer le modèle et d’examiner les résultats.

Si vous n’avez pas créé le groupe de ressources, consultez [Créer un groupe de ressources](template-tutorial-create-first-template.md#create-resource-group). L’exemple suppose que vous avez défini la variable **templateFile** sur le chemin du fichier de modèle, comme indiqué dans le [premier tutoriel](template-tutorial-create-first-template.md#deploy-template).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

## <a name="verify-deployment"></a>Vérifier le déploiement

Vous pouvez vérifier le déploiement en explorant le groupe de ressources à partir du portail Azure.

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Dans le menu de gauche, sélectionnez **Groupes de ressources**.
1. Sélectionnez le groupe de ressources sur lequel vous avez effectué le déploiement.
1. Sélectionnez l’une des ressources, par exemple la ressource de compte de stockage. Vous voyez qu’elle contient désormais des étiquettes.

   ![Afficher les mots clés](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous passez au tutoriel suivant, vous n’avez pas besoin de supprimer le groupe de ressources.

Si vous arrêtez maintenant, vous pouvez nettoyer les ressources que vous avez déployées en supprimant le groupe de ressources.

1. Dans le portail Azure, sélectionnez **Groupe de ressources** dans le menu de gauche.
2. Entrez le nom du groupe de ressources dans le champ **Filtrer par nom**.
3. Sélectionnez le nom du groupe de ressources.
4. Sélectionnez **Supprimer le groupe de ressources** dans le menu supérieur.

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez ajouté des étiquettes aux ressources. Dans le tutoriel suivant, vous allez apprendre à utiliser des fichiers de paramètres pour simplifier le passage de valeurs au modèle.

> [!div class="nextstepaction"]
> [Utiliser un fichier de paramètres](template-tutorial-use-parameter-file.md)
