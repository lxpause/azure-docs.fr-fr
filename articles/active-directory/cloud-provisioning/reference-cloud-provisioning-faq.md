---
title: Questions fréquentes (FAQ) sur le provisionnement cloud Azure AD Connect
description: Ce document décrit les questions fréquemment posées sur le provisionnement cloud.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbaafdce6f2510b58966f4b6c18e45a3fcd4a664
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997086"
---
# <a name="azure-active-directory-connect-faq"></a>FAQ Azure Active Directory Connect

Découvrez les questions fréquemment posées sur le provisionnement cloud Azure Active Directory (Azure AD) Connect.

## <a name="general-installation"></a>Installation générale

**Q : Quelle est la fréquence d’exécution du provisionnement cloud ?**

L’exécution du provisionnement cloud est planifiée toutes les 2 minutes. Toutes les 2 minutes, toutes les modifications éventuelles d’utilisateur, de groupe et de hachage de mot de passe sont provisionnées dans Azure AD.

**Q : Je vois des échecs de synchronisation des hachages de mot de passe lors de la première exécution. Pourquoi ?**

Ceci est normal. Ces échecs sont dus au fait que l’objet utilisateur n’est pas présent dans Azure AD. Une fois que l’utilisateur a été provisionné dans Azure AD, les hachages de mot de passe devraient être provisionnés lors de l’exécution suivante. Attendez après quelques exécutions et vérifiez que la synchronisation des hachages de mot de passe ne présente plus ces erreurs.

**Q : Quelle est la différence entre la synchronisation et le provisionnement cloud Azure AD Connect ?**

Avec la synchronisation Azure AD Connect, le provisionnement s’exécute sur le serveur de synchronisation local. La configuration est stockée sur le serveur de synchronisation local. Avec le provisionnement cloud Azure AD Connect, la configuration du provisionnement est stockée dans le cloud et s’exécute dans le cloud dans le cadre du service de provisionnement Azure AD. 

**Q : Puis-je utiliser le provisionnement cloud pour effectuer une synchronisation à partir de plusieurs forêts Active Directory ?**

Oui. Le provisionnement cloud permet d’effectuer une synchronisation à partir de plusieurs forêts Active Directory. Dans l’environnement multiforêt, toutes les références (par exemple, le gestionnaire) doivent se trouver dans le domaine.  

**Q : Comment l’agent est-il mis à jour ?**

Les agents sont mis à niveau automatiquement par Microsoft. Pour l’équipe informatique, cela réduit les efforts de test et de validation des nouvelles versions de l’agent. 

**Q : Puis-je désactiver la mise à niveau automatique ?**

Il n’existe aucune méthode établie permettant de désactiver la mise à niveau automatique.

**Q : Puis-je changer l’ancre source pour le provisionnement cloud ?**

Par défaut, le provisionnement cloud utilise ms-ds-consistency-GUID avec une valeur de secours pour ObjectGUID comme ancre source. Il n’existe aucune méthode établie permettant de changer l’ancre source.

**Q : Je vois de nouveaux principaux de service avec le ou les noms de domaine Active Directory lors de l’utilisation du provisionnement cloud. Est-ce normal ?**

Oui, le provisionnement cloud crée un principal de service pour la configuration du provisionnement avec le nom de domaine comme nom de principal de service. N’apportez aucune modification à la configuration du principal de service.

**Q : Que se passe-t-il quand un utilisateur synchronisé est tenu de changer son mot de passe à la prochaine ouverture de session ?**

Si la synchronisation des hachages de mot de passe est activée dans le provisionnement cloud et que l’utilisateur synchronisé est tenu de changer son mot de passe à la prochaine ouverture de session dans l’annuaire Active Directory local, le provisionnement cloud ne provisionne pas le hachage de mot de passe changé dans Azure AD. Une fois que l’utilisateur a changé le mot de passe, le hachage de mot de passe utilisateur est provisionné de l’annuaire Active Directory à Azure AD.

**Q : Le provisionnement cloud prend-il en charge l’écriture différée de ms-ds-consistencyGUID pour tout objet ?**

Non, le provisionnement cloud ne prend pas en charge l’écriture différée de ms-ds-consistencyGUID pour tout objet (y compris les objets utilisateur). 

**Q : Je provisionne des utilisateurs à l’aide du provisionnement cloud. J’ai supprimé la configuration. Pourquoi les anciens objets synchronisés sont-ils toujours visibles dans Azure AD ?** 

Lorsque vous supprimez la configuration, le provisionnement cloud ne nettoie pas les objets synchronisés dans Azure AD. Pour vous assurer de ne plus avoir les anciens objets, changez l’étendue de la configuration en spécifiant un groupe vide ou des unités d’organisation. Une fois que le provisionnement s’exécute et nettoie les objets, désactivez et supprimez la configuration. 

## <a name="next-steps"></a>Étapes suivantes 

- [Présentation du provisionnement](what-is-provisioning.md)
- [Présentation du provisionnement cloud Azure AD Connect](what-is-cloud-provisioning.md)
