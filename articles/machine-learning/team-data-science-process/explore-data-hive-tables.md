---
title: Explorer les données dans des tables Hive avec des requêtes Hive - Team Data Science Process
description: Utilisez des exemples de scripts Hive qui permettent d’explorer des données dans des tables Hive au sein d’un cluster Hadoop HDInsight.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c9761ade493641b6445cf151424f9598ab25e384
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303639"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Explorer les données dans des tables Hive avec des requêtes Hive

Cet article fournit des exemples de scripts Hive qui permettent d’explorer des données dans des tables Hive dans un cluster Hadoop HDInsight.

Cette tâche est une étape du [processus Team Data Science Process](overview.md).

## <a name="prerequisites"></a>Prérequis
Cet article suppose que vous avez :

* Créé un compte de stockage Azure. Pour des instructions, voir [Créer un compte Stockage Azure](../../storage/common/storage-quickstart-create-account.md).
* Approvisionné un cluster Hadoop personnalisé avec le service HDInsight. Si vous avez besoin d'aide, consultez [Personnaliser des clusters Hadoop Azure HDInsight pour l'analyse avancée](customize-hadoop-cluster.md).
* Chargé les données dans les tables Hive de clusters Hadoop Azure HDInsight. Si tel n’est pas le cas, commencez par suivre la procédure décrite sous [Créer et charger des données dans les tables Hive](move-hive-tables.md) .
* Activé l’accès à distance au cluster. Si vous avez besoin d’aide, consultez [Accéder au nœud principal du cluster Hadoop](customize-hadoop-cluster.md).
* Si vous avez besoin d’aide sur l’envoi de requêtes Hive, consultez [Comment envoyer des requêtes Hive](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Exemples de scripts de requête Hive pour l’exploration de données
1. Obtenir le nombre d’observations par partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Obtenir le nombre d'observations par jour  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Obtenir les niveaux dans une colonne catégorielle  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Obtenir le nombre de niveaux en combinant deux colonnes catégorielles  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Obtenir la distribution relative aux colonnes numériques  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Extraire les enregistrements à partir de la jointure des deux tables
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Scripts de requête supplémentaires pour les scénarios de données de courses de taxi
Des exemples de requêtes propres aux scénarios mettant en œuvre le jeu de [données NYC Taxi Trip](https://chriswhong.com/open-data/foil_nyc_taxi/) sont également disponibles dans le [référentiel Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Le schéma de données de ces requêtes est déjà spécifié et elles sont exécutables en l’état.

