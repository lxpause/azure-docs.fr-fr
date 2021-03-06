---
title: Gestion des charges de travail
description: Conseils pour l’implémentation de la gestion des charges de travail dans Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload-management
ms.date: 10/30/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 2563ea4ab498c11c846cfe79f0e668f7d491c2e7
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692361"
---
# <a name="what-is-workload-management"></a>Qu’est-ce que la gestion des charges de travail ?

L’exécution de charges de travail mixtes peut poser des problèmes de ressources sur les systèmes chargés.  Les architectes de solution cherchent des moyens de séparer les activités d’entreposage de données classiques (comme le chargement, la transformation et l’interrogation des données) pour s’assurer qu’il existe suffisamment de ressources pour répondre aux contrats SLA.  

L’isolation physique du serveur peut conduire à des poches d’infrastructure sous-exploitées, surutilisées ou dans un état où les caches sont constamment occupés avec le démarrage et l’arrêt du matériel.  Un schéma de gestion des charges de travail réussi gère efficacement les ressources, garantit une utilisation hautement efficace des ressources et optimise le retour sur investissement (ROI).

Le concept de « charge de travail d’entrepôt de données » fait référence à l’ensemble des opérations se produisant dans un entrepôt de données. La complexité et l’étendue de ces composants dépendent du niveau de maturité de l’entrepôt de données.  La charge de travail d’entrepôt de données englobe : 
- Le processus complet de chargement des données dans l’entrepôt 
- L’analyse et la création de rapports pour l’entrepôt de données
- La gestion des données dans l’entrepôt de données 
- L’exportation de données à partir de l’entrepôt de données

La capacité de performances d’un entrepôt de données est déterminée par les [unités de l’entrepôt de données](what-is-a-data-warehouse-unit-dwu-cdwu.md).
- Pour afficher les ressources affectées pour tous les profils de performances, consultez [Limites de mémoire et de concurrencememory-concurrency-limits.md]).
- Pour ajuster la capacité, vous pouvez [l’augmenter ou la réduire](quickstart-scale-compute-portal.md).


## <a name="workload-management-concepts"></a>Concepts de gestion des charges de travail
Par le passé, vous gériez les performances des requêtes sur SQL Data Warehouse par le biais de [classes de ressources](resource-classes-for-workload-management.md).  Les classes de ressources autorisaient l’attribution de mémoire à une requête en fonction de l’appartenance à un rôle.  La principale difficulté avec les classes de ressources est que, une fois celles-ci configurées, il n’existait aucune gouvernance ou possibilité de contrôler la charge de travail.  

Par exemple, l’octroi d’une appartenance à un rôle d’utilisateur ad hoc à smallrc permettait à cet utilisateur de consommer 100 % de la mémoire sur le système.  Avec les classes de ressources, il n’existe aucun moyen de réserver et de garantir la disponibilité des ressources pour les charges de travail critiques.

La gestion des charges de travail sur SQL Data Warehouse se compose de trois concepts de haut niveau : [La classification des charges de travail](sql-data-warehouse-workload-classification.md), [l’importance de la charge de travail](sql-data-warehouse-workload-importance.md) et [l’isolation de la charge de travail](sql-data-warehouse-workload-isolation.md).  Ces fonctionnalités vous permettent de mieux contrôler la façon dont votre charge de travail utilise les ressources système.

La classification des charges de travail est le concept d’affectation d’une requête à un groupe de charge de travail et de définition de niveaux d’importance.  Historiquement, cette affectation était effectuée via l’appartenance à un rôle à l’aide de [sp_addrolemember](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#change-a-users-resource-class).  Vous pouvez à présent utiliser le [CLASSIFIEUR CRÉER UNE CHARGE DE TRAVAIL](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql).  La fonctionnalité de classification fournit un ensemble plus riche d’options, avec les balises, les sessions et le temps nécessaire pour classer les requêtes.

L’importance de la charge de travail influence l’ordre dans lequel une requête accède aux ressources.  Sur un système occupé, une requête ayant une importance plus élevée a accès en premier aux ressources.  L’importance peut également garantir un accès ordonné aux verrous. 

L’isolation de la charge de travail réserve des ressources pour un groupe de charge de travail.  Les ressources réservées dans un groupe de charge de travail sont conservées exclusivement pour que le groupe de charge de travail puisse s’exécuter.  Les groupes de charges de travail vous permettent également de définir la quantité de ressources affectées par requête, de la même façon que les classes de ressources.  Les groupes de charges de travail vous permettent de réserver ou de limiter la quantité de ressources qu’un ensemble de requêtes peut consommer.  Enfin, les groupes de charge de travail sont un mécanisme permettant d’appliquer des règles, comme le délai de requête, aux requêtes.  


## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations sur la classification des charges de travail, consultez [Classification des charges de travail](sql-data-warehouse-workload-classification.md).  
- Pour plus d’informations sur l’isolation de la charge de travail, consultez [Isolation de la charge de travail](sql-data-warehouse-workload-isolation.md).  
- Pour plus d’informations sur l’importance de la charge de travail, consultez [Importance de la charge de travail](sql-data-warehouse-workload-importance.md).  