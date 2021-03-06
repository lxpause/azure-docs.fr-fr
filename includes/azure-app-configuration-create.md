---
title: Fichier Include
description: Fichier Include
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: c98a17be394887ef4e008b079467c85d4ded7e09
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393300"
---
1. Pour créer un magasin App Configuration, connectez-vous au [portail Azure](https://portal.azure.com). En haut à gauche du volet, sélectionnez **+ Créer une ressource**. Dans la zone **Rechercher dans la Place de marché**, entrez **App Configuration** et sélectionnez Entrée.

    ![Rechercher App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

1. Sélectionnez **Configuration d’application** dans les résultats de la recherche, puis **Créer**.

1. Dans le volet **App Configuration** > **Créer**, entrez les paramètres suivants :

    | Paramètre | Valeur suggérée | Description |
    |---|---|---|
    | **Nom de la ressource** | Nom globalement unique | Entrez un nom de ressource unique à utiliser pour la ressource du magasin App Configuration. Le nom doit être une chaîne de 1 à 63 caractères et contenir uniquement des chiffres, des lettres et le caractère `-`. Le nom ne peut ni commencer ni se terminer par le caractère `-`, et n’accepte pas les caractères `-` consécutifs.  |
    | **Abonnement** | Votre abonnement | Sélectionnez l’abonnement Azure à utiliser pour tester App Configuration. Si votre compte n’a qu’un seul abonnement, il est automatiquement sélectionné et la liste **Abonnement** n’est pas affichée. |
    | **Groupe de ressources** | *AppConfigTestResources* | Sélectionnez ou créez un groupe de ressources pour votre ressource du magasin App Configuration. Ce groupe est utile pour organiser plusieurs ressources que vous souhaitez supprimer en même temps que vous supprimez ce groupe de ressources. Pour plus d’informations, consultez [Utilisation des groupes de ressources pour gérer vos ressources Azure](/azure/azure-resource-manager/resource-group-overview). |
    | **Lieu** | *USA Centre* | Utilisez **Emplacement** pour indiquer l’emplacement géographique de l’hébergement de votre magasin de configuration d’application. Pour des performances optimales, créez la ressource dans la même région que les autres composants de votre application. |

    ![Créer une ressource de magasin App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

1. Sélectionnez **Créer**. Le déploiement peut prendre quelques minutes.

1. À la fin du déploiement, sélectionnez **Paramètres** > **Clés d’accès**. Prenez note de la chaîne de connexion de la clé primaire, en lecture seule ou en lecture-écriture. Vous utilisez cette chaîne de connexion plus tard pour configurer votre application, afin qu’elle communique avec le magasin App Configuration que vous avez créé.
