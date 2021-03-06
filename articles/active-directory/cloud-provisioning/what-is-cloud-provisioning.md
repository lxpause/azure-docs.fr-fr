---
title: Présentation du provisionnement cloud Azure AD Connect. | Microsoft Docs
description: Décrit le provisionnement cloud Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10c778025c3def384f621cb5311d0d71ccb457d7
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951120"
---
# <a name="what-is-azure-ad-connect-cloud-provisioning"></a>Qu’est-ce que le provisionnement cloud Azure AD Connect ?
Le provisionnement cloud Azure AD Connect est un nouvel agent Microsoft conçu pour répondre à vos objectifs d’identité hybrides, et vous permettre de les atteindre en matière de synchronisation des utilisateurs, des groupes et des contacts sur Azure AD.  Il peut être utilisé avec la synchronisation Azure AD Connect, et offre les avantages suivants :
    
- Prise en charge de la synchronisation sur un locataire Azure AD, à partir d’un environnement de forêt Active Directory déconnecté multiforêt. Les scénarios courants incluent la fusion et l’acquisition, dans lesquelles les forêts Active Directory de la société acquise sont isolées des forêts Active Directory de la société parente et des sociétés qui ont historiquement plusieurs forêts Active Directory.
- Installation simplifiée avec des agents de provisionnement légers. Les agents jouent le rôle de pont entre Active Directory et Azure AD, avec toute la configuration de synchronisation gérée dans le cloud. 
- Plusieurs agents de provisionnement peuvent être utilisés pour simplifier les déploiements à haute disponibilité, particulièrement critiques pour les organisations qui reposent sur la synchronisation de hachage de mot de passe entre Active Directory et Azure AD.


![Qu’est-ce qu’Azure AD Connect ?](media/what-is-cloud-provisioning/architecture.png)

## <a name="how-is-azure-ad-connect-cloud-provisioning-different-from-azure-ad-connect-sync"></a>En quoi le provisionnement cloud Azure AD Connect est-il différent de la synchronisation Azure AD Connect ?
Avec le provisionnement cloud Azure AD Connect, le provisionnement d’Active Directory sur Azure AD est orchestré dans Microsoft Online Services. Une organisation a uniquement besoin de déployer, dans son environnement IaaS local, un agent léger qui fait office de pont entre Azure AD et Active Directory. La configuration du provisionnement est stockée dans Azure AD et gérée dans le cadre du service.

Le tableau suivant présente une comparaison entre Azure AD Connect et le provisionnement cloud Azure AD Connect :

| Fonctionnalité | Synchronisation Azure Active Directory Connect| Provisionnement cloud Azure Active Directory Connect |
|:--- |:---:|:---:|
|Connexion à une forêt AD locale|● |● |
| Connexion à plusieurs forêts AD locales |● |● |
| Connexion à plusieurs forêts Active Directory locales déconnectées | |● |
| Modèle d’installation d’agent léger | |● |
| Plusieurs agents actifs pour la haute disponibilité | |● |
| Connexion aux annuaires LDAP|●| | 
| Prise en charge des objets utilisateur |● |● |
| Prise en charge des objets groupe |● |● |
| Prise en charge des objets contact |● |● |
| Prise en charge des objets appareil |● | |
| Autorisation de la personnalisation de base pour les flux d’attributs |● |● |
| Synchronisation des attributs Active Directory définis par le client (extensions d’annuaire) |● | |
| Prise en charge de la synchronisation de hachage de mot de passe |●|●|
| Prise en charge de l’authentification directe |●||
| Prise en charge de la fédération |●|●|
| Authentification unique transparente|● |●|
| Installation des supports sur un contrôleur de domaine |● |● |
| Prise en charge pour Windows Server 2012 et Windows Server 2012 R2 |● |● |
| Filtrage sur les domaines/unités d’organisation/groupes |● |● |
| Filtrage sur les valeurs d’attributs des objets |● | |
| Autorisation d’un ensemble minimal d’attributs à synchroniser (MinSync) |● |● |
| Autorisation du blocage des attributs circulant d’AD vers Azure AD |● |● |
| Autorisation de la personnalisation avancée pour les flux d’attributs |● | |
| Prise en charge de la réécriture (mots de passe, appareils, groupes) |● | |
| Support Azure AD Domain Services|● | |
| Configuration Exchange hybride |● | |

## <a name="next-steps"></a>Étapes suivantes 

- [Présentation du provisionnement](what-is-provisioning.md)
- [Installer le provisionnement cloud](how-to-install.md)
