---
title: Synchroniser les attributs avec Azure AD pour le mappage | Microsoft Docs
description: Découvrez comment synchroniser les attributs de votre répertoire local Active Directory avec Azure AD. Lors de la configuration des utilisateurs pour les applications SaaS, servez-vous de la fonctionnalité d’extension de répertoire pour ajouter des attributs sources qui ne sont pas synchronisés par défaut.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mimart
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9460bc924ea662646360d1a3f5087949a39a03f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806115"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Synchronisez un attribut à partir d’un répertoire Active Directory local vers Azure AD pour la configuration vers une application donnée

Lors de la personnalisation des mappages d’attributs pour la configuration utilisateur, vous constaterez peut-être que l’attribut à mapper n’apparaît pas dans la liste **Attribut source**. Cet article vous montre comment ajouter l’attribut manquant en le synchronisant de votre répertoire Active Directory (AD) local vers Azure Active Directory (Azure AD).

Azure AD doit contenir toutes les données requises pour créer un profil utilisateur lors de la configuration des comptes utilisateur d’Azure AD vers une application SaaS. Pour rendre les données disponibles, vous devez parfois synchroniser les attributs de votre répertoire AD local vers Azure AD. Azure AD Connect synchronise automatiquement certains attributs avec Azure AD, mais pas tous. En outre, certains attributs (par exemple, SAMAccountName) synchronisés par défaut ne peuvent pas être exposés par le biais de l’API Graph Azure AD. Dans ce cas, vous pouvez utiliser la fonctionnalité d’extension de répertoire Azure AD Connect pour synchroniser l’attribut avec Azure AD. De cette façon, l’attribut est visible pour l’API Graph Azure AD et le service de configuration Azure AD.

Si les données dont vous avez besoin pour la configuration se trouvent dans Active Directory, mais ne sont pas disponibles pour la configuration à cause des raisons décrites ci-dessus, procédez comme suit.
 
## <a name="sync-an-attribute"></a>Synchroniser un attribut 

1. Ouvrez l’Assistant Azure AD Connect et sélectionnez Tâches, puis **Personnaliser les options de synchronisation**.

   ![Page Tâches supplémentaires de l’Assistant Azure Active Directory Connect](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Connectez-vous en tant qu’Administrateur général Azure AD. 

3. Sur la page **Fonctionnalités facultatives**, sélectionnez **Synchronisation des attributs des extensions d’annuaire**.
 
   ![Page Fonctionnalités facultatives de l’Assistant Azure Active Directory Connect](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Sélectionnez le ou les attributs que vous souhaitez étendre à Azure AD.
   > [!NOTE]
   > La recherche **Attributs disponibles** respecte la casse.

   ![Page de sélection des extensions d’annuaire de l’Assistant Azure Active Directory Connect](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Terminez l’Assistant Azure AD Connect et autorisez l’exécution d’un cycle de synchronisation complète. Lorsque le cycle est terminé, le schéma est étendu et les nouvelles valeurs sont synchronisées entre votre répertoire AD local et Azure AD.
 
6. Dans le Portail Azure, lorsque vous [modifiez des mappages d’attributs utilisateur](customize-application-attributes.md), la liste **Attribut source** contient désormais l’attribut ajouté au format `<attributename> (extension_<appID>_<attributename>)`. Sélectionnez l’attribut et mappez-le à l’application cible pour la configuration.

   ![Page de sélection des extensions d’annuaire de l’Assistant Azure Active Directory Connect](media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> À l’heure actuelle, il n’est pas possible de configurer les attributs de référence à partir du répertoire AD local, comme **managedby** ou **DN/DistinguishedName**. Vous pouvez demander cette fonctionnalité sur [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory). 

## <a name="next-steps"></a>Étapes suivantes

* [Définir qui est concerné par la configuration](define-conditional-rules-for-provisioning-user-accounts.md)
