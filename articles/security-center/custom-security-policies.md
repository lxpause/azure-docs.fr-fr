---
title: Créer des stratégies de sécurité personnalisées dans Azure Security Center | Microsoft Docs
description: Définitions de stratégie Azure personnalisées surveillées dans Azure Security Center.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 3e60c0e165b8f9ec79aac2048011d9e315b0ce18
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483962"
---
# <a name="using-custom-security-policies-preview"></a>Utilisation de stratégies de sécurité personnalisées (préversion)

Pour vous aider à sécuriser vos systèmes et votre environnement, Azure Security Center génère des recommandations de sécurité. Ces recommandations sont basées sur les meilleures pratiques du secteur, qui sont incorporées à la stratégie de sécurité par défaut générique fournie à tous les clients. Elles peuvent également provenir des connaissances que Security Center a des normes et réglementations du secteur.

Avec cette fonctionnalité en préversion, vous pouvez ajouter vos propres initiatives *personnalisées*. Vous recevez ensuite des recommandations si votre environnement ne suit pas les stratégies que vous créez.

Comme nous l’avons vu [ici](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#definition-location) dans la documentation Azure Policy, quand vous spécifiez un emplacement pour votre initiative personnalisée, il doit correspondre à un groupe d’administration ou à un abonnement. 

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>Pour ajouter une initiative personnalisée à votre abonnement 

1. Dans la barre latérale de Security Center, ouvrez la page **Stratégie de sécurité**.

1. Sélectionnez un abonnement ou un groupe d’administration auquel vous voulez ajouter une initiative personnalisée.

    [![Sélection d’un abonnement pour lequel vous allez créer votre stratégie personnalisée](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > Vous devez ajouter des standards personnalisés au niveau de l’abonnement (ou à un niveau supérieur) pour qu’ils soient évalués et affichés dans Security Center. 
    >
    > Lorsque vous ajoutez une norme personnalisée, elle attribue une *initiative* à cette étendue. Nous vous recommandons donc de sélectionner l’étendue la plus vaste requise pour cette attribution.

1. Dans la page Stratégie de sécurité, sous vos initiatives personnalisées (préversion), cliquez sur **Ajouter une initiative personnalisée**.

    [![Cliquez sur *Ajouter une initiative personnalisée*](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    La page suivante apparaît :

    ![Créer ou ajouter une stratégie](media/custom-security-policies/create-or-add-custom-policy.png)

1. Dans la page Ajouter des initiatives personnalisées, passez en revue la liste de stratégies personnalisées déjà créées dans votre organisation. Si vous en voyez une que vous souhaitez attribuer à votre abonnement, cliquez sur **Ajouter**. Si aucune initiative de la liste ne répond à vos besoins, ignorez cette étape.

1. Pour créer une nouvelle initiative personnalisée :

    1. Cliquez sur **Créer une nouvelle**.
    1. Entrez l’emplacement et le nom de la définition.
    1. Sélectionnez les stratégies à inclure, puis cliquez sur **Ajouter**.
    1. Entrez les paramètres souhaités.
    1. Cliquez sur **Enregistrer**.
    1. Dans la page Ajouter des initiatives personnalisées, cliquez sur Actualiser ; votre nouvelle initiative s’affichera comme étant disponible.
    1. Cliquez sur **Ajouter** et affectez-la à votre abonnement.

    > [!NOTE]
    > La création d’initiatives nécessite les informations d’identification du propriétaire de l’abonnement. Pour plus d’informations sur les rôles Azure, consultez [Autorisations dans Azure Security Center](security-center-permissions.md).

1. Pour afficher les recommandations qui en résultent pour votre stratégie, cliquez sur **Recommandations** dans la barre latérale pour ouvrir la page de recommandations. Les recommandations apparaissent avec une étiquette « Personnalisée » et sont disponibles dans un délai d’une heure environ.

    [![Recommandations personnalisées](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)


## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez appris à créer des stratégies de sécurité personnalisées. 

Pour d’autres informations connexes, consultez les articles suivants : 

- [La vue d’ensemble des stratégies de sécurité](tutorial-security-policy.md)
- [Liste des stratégies de sécurité intégrées](security-center-policy-definitions.md)