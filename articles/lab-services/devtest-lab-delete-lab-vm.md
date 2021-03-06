---
title: Supprimer un labo ou une machine virtuelle d’un labo dans Azure DevTest Labs | Microsoft Docs
description: Cet article vous montre comment supprimer un labo ou une machine virtuelle d’un labo.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: spelluru
ms.openlocfilehash: 9634c70566aba21bdd28ee016c9fa94464ec9c1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127410"
---
# <a name="delete-a-lab-or-vm-in-a-lab-in-azure-devtest-labs"></a>Supprimer un labo ou une machine virtuelle d’un labo dans Azure DevTest Labs
Cet article vous montre comment supprimer un labo ou une machine virtuelle d’un labo.

## <a name="delete-a-lab"></a>Supprimer un labo
Quand vous supprimez une instance DevTest Labs d’un groupe de ressources, le service DevTest Labs effectue les actions suivantes : 

- Toutes les ressources qui ont été créées automatiquement au moment de la création du labo sont automatiquement supprimées. Le groupe de ressources lui-même n’est pas supprimé. Si vous avez créé manuellement des ressources dans ce groupe de ressources, le service ne les supprime pas. 
- Les machines virtuelles dans le labo et les groupes de ressources associés à ces machines virtuelles sont automatiquement supprimés. Quand vous créez une machine virtuelle dans un labo, le service crée des ressources (disque, interface réseau, adresse IP publique, etc.) pour la machine virtuelle dans un groupe de ressources distinct. Toutefois, si vous créez manuellement des ressources supplémentaires dans ces groupes de ressources, le service DevTest Labs ne supprime pas ces ressources et le groupe de ressources. 

Pour supprimer un labo, effectuez les actions suivantes : 

1. Connectez-vous au [Portail Azure](https://portal.azure.com).
2. Sélectionnez **Toutes les ressources** dans le menu gauche, sélectionnez **DevTest Labs** comme type de service, puis sélectionnez le labo.

    ![Sélectionner votre labo](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Dans la page **DevTest Lab**, cliquez sur **Supprimer** dans la barre d’outils. 

    ![Bouton Supprimer](media/devtest-lab-delete-lab-vm/delete-button.png)
4. Dans la page **Confirmation**, entrez le **nom** de votre labo, puis sélectionnez **Supprimer**. 

    ![Confirmer](media/devtest-lab-delete-lab-vm/confirm-delete.png)
5. Pour afficher l’état de l’opération, sélectionnez l’icône **Notifications** (cloche). 

    ![Notifications](media/devtest-lab-delete-lab-vm/delete-status.png)

 
## <a name="delete-a-vm-in-a-lab"></a>Supprimer une machine virtuelle dans un labo
Si vous supprimez une machine virtuelle dans un labo, seules certaines ressources créées au moment de la création du labo sont supprimées. Les ressources suivantes ne sont pas supprimées : 

-   Coffre de clés dans le groupe de ressources principal
-   Groupe à haute disponibilité, équilibreur de charge, adresse IP publique dans le groupe de ressources de la machine virtuelle. Ces ressources sont partagées par plusieurs machines virtuelles dans un groupe de ressources. 

La machine virtuelle, l’interface réseau et le disque associés à la machine virtuelle sont supprimés. 

Pour supprimer une machine virtuelle dans un labo, effectuez les actions suivantes : 

1. Connectez-vous au [Portail Azure](https://portal.azure.com).
2. Sélectionnez **Toutes les ressources** dans le menu gauche, sélectionnez **DevTest Labs** comme type de service, puis sélectionnez le labo.

    ![Sélectionner votre labo](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Sélectionnez **... (points de suspension)** pour la machine virtuelle dans la liste des machines virtuelles, puis sélectionnez **Supprimer**. 

    ![Supprimer une machine virtuelle dans le menu](media/devtest-lab-delete-lab-vm/delete-vm-menu-in-list.png)
4. Dans la boîte de dialogue de **confirmation**, sélectionnez **OK**. 
5. Pour afficher l’état de l’opération, sélectionnez l’icône **Notifications** (cloche). 

Pour supprimer une machine virtuelle de la **page Machine virtuelle**, sélectionnez **Supprimer** dans la barre d’outils, comme illustré dans l’image suivante :

![Supprimer une machine virtuelle de la page Machine virtuelle](media/devtest-lab-delete-lab-vm/delete-from-vm-page.png) 


## <a name="next-steps"></a>Étapes suivantes
Si vous souhaitez créer un labo, consultez les articles suivants : 

- [Création d’un laboratoire](devtest-lab-create-lab.md)
- [Ajouter une machine virtuelle au labo](devtest-lab-add-vm.md)