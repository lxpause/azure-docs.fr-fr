---
title: Problèmes de connexion et d’utilisation d’Azure SQL Database
description: Fournit la procédure de résolution des problèmes de connexion Azure SQL Database et d’autres problèmes SQL Database spécifiques
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
ms.custom: seo-lt-2019, OKR 11/2019
author: v-miegge
ms.author: ramakoni
ms.reviewer: carlrab
ms.date: 11/14/2019
ms.openlocfilehash: 0bd018d90f4ca2c64df56d27eebdc6c9160309ac
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082407"
---
# <a name="troubleshooting-connectivity-issues-and-other-errors-with-microsoft-azure-sql-database"></a>Résolution des problèmes de connectivité et autres erreurs avec Microsoft Azure SQL Database

En cas d’échec de la connexion à Azure SQL Database, vous recevez des messages d’erreur. Ces problèmes de connexion peuvent découler d’une reconfiguration d’Azure SQL Database, des paramètres de pare-feu, de l’expiration d’un délai de connexion ou d’informations de connexion incorrectes. En outre, vous ne pourrez plus vous connecter à Azure SQL Database si la limite de certaines de ses ressources est atteinte.

## <a name="transient-fault-error-messages"></a>Messages d’erreur pour les erreurs temporaires

L’infrastructure Azure a la capacité de reconfigurer dynamiquement les serveurs quand des charges de travail importantes sont à traiter dans le service SQL Database.  Ce comportement dynamique peut entraîner la perte par votre programme client de sa connexion à SQL Database. Ce type d’erreur état est connu sous le nom d’ *erreur temporaire*. Il est fortement recommandé que votre programme client possède une logique de nouvelle tentative afin qu’il puisse rétablir une connexion après avoir donné à l’erreur temporaire le temps de se corriger elle-même.  Nous vous recommandons de patienter 5 secondes avant votre première tentative. Si vous effectuez une nouvelle tentative avant 5 secondes, vous risquez de submerger le service cloud. Pour chaque nouvelle tentative, le délai doit augmenter de manière exponentielle, sans dépasser 60 secondes.

Pour obtenir des exemples de code de logique de nouvelle tentative, voir :

* [Bibliothèques de connexions pour SQL Database et SQL Server](sql-database-libraries.md)
* [Actions servant à corriger les erreurs de connexion et les erreurs temporaires dans la base de données SQL](sql-database-connectivity-issues.md)

> [!TIP]
> Pour résoudre les problèmes présentés dans les sections suivantes, suivez la procédure pas à pas de la section intitulée [Étapes pour résoudre les problèmes de connexion courants](#steps-to-fix-common-connection-issues).

### <a name="error-40613-database--x--on-server--y--is-not-currently-available"></a>Erreur 40613: La base de données < x > sur le serveur < y > n’est pas disponible actuellement

``40613: Database <DBname> on server < server name > is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of '< Tracing ID >'.``

Pour résoudre ce problème :

1. Recherchez des pannes connues dans le [tableau de bord des services Microsoft Azure](https://status.azure.com/status).
2. S’il n’y a aucune panne connue, accédez au [site web du support Microsoft Azure](https://azure.microsoft.com/support/options) pour ouvrir un cas de support.

Pour plus d’informations, consultez [Dépannage de l’erreur « La base de données sur le serveur n’est pas disponible actuellement »](sql-database-troubleshoot-common-connection-issues.md#troubleshoot-transient-errors).

### <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-database-server"></a>Une erreur liée au réseau ou propre à une instance s’est produite lors de l’établissement d’une connexion au serveur SQL Database

Ce problème se produit si l’application ne peut pas se connecter au serveur.

Pour le résoudre, suivez la procédure pas-à-pas de la section intitulée [Étapes pour résoudre les problèmes de connexion courants](#steps-to-fix-common-connection-issues).

### <a name="the-serverinstance-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>Le serveur/l’instance est introuvable ou inaccessible (erreurs 26, 40, 10053)

#### <a name="error-26-error-locating-server-specified"></a>Erreur 26 : Erreur lors de la localisation du serveur spécifié

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

#### <a name="error-40-could-not-open-a-connection-to-the-server"></a>Erreur 40 : Impossible d’ouvrir une connexion au serveur

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

#### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>Erreur 10053 : Une erreur de niveau transport s’est produite lors de la réception des résultats à partir du serveur.

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

#### <a name="cannot-connect-to-a-secondary-database"></a>Impossible de se connecter à une base de données secondaire

Une tentative de connexion à une base de données secondaire a échoué car la base de données est en cours de reconfiguration et qu’elle est occupée à appliquer de nouvelles pages tout en gérant une transaction active sur la base de données primaire.

#### <a name="adonet-and-blocking-period"></a>ADO.NET et période de blocage

Pour en savoir plus sur la *période de blocage* des clients qui utilisent ADO.NET, consultez la page [Regroupement de connexions SQL Server (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="list-of-transient-fault-error-codes"></a>Liste de codes d’erreur pour les erreurs temporaires

Les erreurs suivantes sont temporaires et doivent être relancées dans la logique d’application :

| Code d'erreur | severity | Description |
| ---:| ---:|:--- |
| 4060 |16 |Impossible d'ouvrir de base de données "%.&#x2a;ls" demandée par la connexion. La connexion a échoué. Pour plus d’informations, consultez [Erreurs 4000 à 4999](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999)|
| 40197 |17 |Le service a rencontré une erreur lors du traitement de votre demande. Réessayez. Code d'erreur % d.<br/><br/>Vous recevez cette erreur lorsque le service est arrêté en raison de mises à niveau logicielles ou matérielles, de pannes de matériel ou tout autre problème de basculement. Le code d’erreur (%d) incorporé au message d’erreur 40197] fournit des informations supplémentaires sur le type de défaillance ou de basculement survenu. 40020, 40143, 40166 et 40540 sont des exemples de codes d'erreur incorporés au message d'erreur 40197.<br/><br/>La reconnexion à votre serveur SQL Database vous reconnecte automatiquement à une copie saine de votre base de données. Votre application doit détecter l'erreur 40197, consigner le code d'erreur incorporé (%d) dans le message pour la résolution des problèmes, et essayer de se reconnecter à la base de données SQL jusqu'à ce que les ressources soient disponibles et que votre connexion soit rétablie. Pour plus d’informations, consultez [Erreurs temporaires](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 40501 |20 |Le service est actuellement occupé. Relancez la demande dans 10 secondes. ID de l'incident : %ls. Code : %d. Pour plus d'informations, consultez les pages suivantes : <br/>&bull; &nbsp;[Limites de ressources du serveur de base de données](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Limites de DTU pour les bases de données uniques](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md)|
| 40613 |17 |La base de données ’%.&#x2a;ls’ sur le serveur ’%.&#x2a;ls’ n’est pas disponible actuellement. Veuillez réessayer la connexion ultérieurement. Si le problème persiste, contactez le support technique en indiquant l'ID de suivi de session ’%.&#x2a;ls’’.<br/><br/> Cette erreur peut se produire s’il existe déjà une connexion d’administrateur dédiée à la base de données. Pour plus d’informations, consultez [Erreurs temporaires](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 49918 |16 |Impossible de traiter la requête. Ressources insuffisantes pour traiter la demande.<br/><br/>Le service est actuellement occupé. Relancez la requête ultérieurement. Pour plus d'informations, consultez les pages suivantes : <br/>&bull; &nbsp;[Limites de ressources du serveur de base de données](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Limites de DTU pour les bases de données uniques](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md) |
| 49919 |16 |Processus ne peut pas créer ou mettre à jour de la demande. Opérations de mise à jour ou de création en cours pour l'abonnement « % ld » trop nombreuses.<br/><br/>Le service est occupé à traiter plusieurs demandes de création ou de mise à jour pour votre abonnement ou le serveur. Les requêtes sont actuellement bloquées pour l’optimisation des ressources. Requête [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) pour les opérations en attente. Patientez jusqu’à ce que les demandes de création ou de mise à jour soient terminées ou supprimez l’une de vos requêtes en cours et réessayez votre requête ultérieurement. Pour plus d'informations, consultez les pages suivantes : <br/>&bull; &nbsp;[Limites de ressources du serveur de base de données](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Limites de DTU pour les bases de données uniques](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md) |
| 49920 |16 |Impossible de traiter la requête. Opérations en cours pour l'abonnement « % ld » trop nombreuses.<br/><br/>Le service est occupé à traiter plusieurs demandes pour cet abonnement. Les requêtes sont actuellement bloquées pour l’optimisation des ressources. Requête [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) pour l'état de l'opération. Patientez jusqu’à ce que les requêtes soient terminées ou supprimez l’une de vos requêtes en cours et réessayez votre requête ultérieurement. Pour plus d'informations, consultez les pages suivantes : <br/>&bull; &nbsp;[Limites de ressources du serveur de base de données](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Limites de DTU pour les bases de données uniques](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md) |
| 4221 |16 |La connexion au serveur de lecture secondaire a échoué en raison d’une longue attente sur 'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING'. Le réplica n’est pas disponible pour la connexion, car il manque des versions de ligne pour les transactions qui étaient en cours lorsque le réplica a été recyclé. Le problème peut être résolu en restaurant ou en validant les transactions actives au niveau du réplica principal. Les occurrences de cette erreur peuvent être réduites en évitant les transactions d’écriture longues sur le serveur principal. |

## <a name="cannot-connect-to-server-due-to-firewall-issues"></a>Impossible de se connecter au serveur en raison de problèmes de pare-feu

### <a name="error-40615-cannot-connect-to--servername-"></a>Erreur 40615 : Impossible de se connecter à <nom_serveur>

Pour résoudre ce problème, [configurez les paramètres du pare-feu sur SQL Database via le portail Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

### <a name="error-5-cannot-connect-to--servername-"></a>Erreur 5 : Impossible de se connecter à <nom_serveur>

Pour résoudre ce problème, vérifiez que le port 1433 est ouvert pour les connexions sortantes sur tous les pare-feu situés entre le client et Internet.

Pour plus d’informations, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>Impossible de se connecter au serveur (Erreurs 18456, 40531)

### <a name="login-failed-for-user--user-name-"></a>Échec de la connexion pour l’utilisateur '<nom_utilisateur>'

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

Pour résoudre ce problème, contactez votre administrateur de service afin d’obtenir un nom d’utilisateur et un mot de passe SQL Server valides.

En règle générale, l’administrateur de service peut utiliser les étapes suivantes pour ajouter les informations d’identification de connexion :

1. Connectez-vous au serveur à l’aide de SQL Server Management Studio (SSMS).
2. Exécutez la requête SQL suivante pour vérifier que le nom de connexion est bien désactivé :

   ```sql
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. Si le nom correspondant est désactivé, activez-le à l’aide de l’instruction suivante :

   ```sql
   Alter login <User name> enable
   ```

4. Si le nom d’utilisateur de connexion SQL n’existe pas, créez-le en procédant comme suit :

   1. Dans SSMS, double-cliquez sur **Sécurité** pour le développer.
   2. Cliquez avec le bouton droit sur **Connexions**, puis sélectionnez **Nouvelle connexion**.
   3. Dans le script généré avec des espaces réservés, modifiez et exécutez la requête SQL suivante :

   ```sql
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```

5. Double-cliquez sur **Base de données**.
6. Sélectionnez la base de données dont vous souhaitez autoriser l’accès.
7. Double-cliquez sur **Sécurité**.
8. Cliquez avec le bouton droit sur **Utilisateurs**, puis sélectionnez **Nouvel utilisateur**.
9. Dans le script généré avec des espaces réservés, modifiez et exécutez la requête SQL suivante :

   ```sql
   CREATE USER <user_name, sysname, user_name>
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```

   > [!NOTE]
   > Vous pouvez également utiliser `sp_addrolemember` pour mapper des utilisateurs spécifiques à des rôles de base de données spécifiques.

Pour en savoir plus, consultez [Gestion des bases de données et des connexions dans Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="connection-timeout-expired-errors"></a>Erreurs de délai d’expiration de connexion

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904) : Délai d’expiration de la connexion dépassé

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904) : Délai expiré

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System.Data.Entity.Core.EntityException: Échec du fournisseur sous-jacent sur Ouverture

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>Impossible de se connecter à <nom_serveur>

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

Ces exceptions peuvent se produire en raison de problèmes de connexion ou de requête. Pour confirmer que cette erreur est due à des problèmes de connectivité, consultez la section [Vérifier si une erreur est due à un problème de connectivité](#confirm-whether-an-error-is-caused-by-a-connectivity-issue).

Des expirations de délai de connexion se produisent car l’application ne peut pas se connecter au serveur. Pour le résoudre, suivez la procédure pas-à-pas de la section intitulée [Étapes pour résoudre les problèmes de connexion courants](#steps-to-fix-common-connection-issues).

## <a name="transient-errors-errors-40197-40545"></a>Erreurs temporaires (Erreurs 40197, 40545)

### <a name="error-40197-the-service-has-encountered-an-error-processing-your-request-please-try-again-error-code--code-"></a>Erreur 40197 : Le service a rencontré une erreur lors du traitement de votre demande. Réessayez. Code d’erreur <code>

Ce problème est dû à une erreur temporaire rencontrée lors de la reconfiguration ou du basculement sur le back-end.

Pour résoudre ce problème, patientez un peu et réessayez. Il n’est pas nécessaire d’ouvrir un cas de support, sauf si le problème persiste.

Nous vous recommandons de vérifier qu’une logique de nouvelle tentative est en place. Pour plus d’informations sur la logique de nouvelle tentative, consultez [Gestion des problèmes de connexion et des erreurs temporaires de base de données SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

## <a name="resource-governance-errors"></a>Erreurs de gouvernance des ressources

### <a name="error-10928-resource-id-d"></a>Erreur 10928 : ID de la ressource : %d

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

Pour contourner ce problème, essayez d’appliquer l’une des méthodes suivantes :

* Vérifiez s’il existe des requêtes de longue durée.

  > [!NOTE]
  > Il s’agit d’une approche minimaliste. Elle risque de ne pas suffire pour résoudre le problème.

1. Exécutez la requête SQL suivante pour vérifier la vue [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql), afin de voir toutes les requêtes bloquantes :

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. Déterminez la **mémoire tampon d’entrée** pour le bloqueur d’en-tête.
3. Paramétrez la requête du bloqueur d’en-tête.

   Pour obtenir une procédure de dépannage détaillée, consultez [Is my query running fine in the cloud?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx) (Ma requête s’exécute-t-elle correctement dans le cloud ?).

Si la base de données atteint constamment sa limite malgré la résolution des problèmes liés aux requêtes longues et bloquantes, effectuez une mise à niveau vers une édition disposant de davantage de ressources ([Editions](https://azure.microsoft.com/pricing/details/sql-database/)).

Pour plus d’informations sur les vues de gestion dynamique, veuillez consulter la page [Vues de gestion dynamique système](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views).

Pour plus d’informations sur les limites de base de données, consultez [Limites de ressources SQL Database des serveurs Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-10929-resource-id-1"></a>Erreur 10929 : ID de ressource : 1

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

### <a name="error-40501-the-service-is-currently-busy"></a>Erreur 40501 : Le service est actuellement occupé.

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

Il s’agit d’une erreur de limitation du moteur, indiquant que les limites de ressources sont dépassées.

Pour plus d’informations sur les limites de ressources, consultez [Limites de ressources du serveur de base de données](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>Erreur 40544 : La base de données a atteint son quota de taille.

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

Cette erreur se produit quand la base de données a atteint son quota de taille.

La procédure suivante peut vous aider à contourner le problème ou vous fournir des options supplémentaires :

1. Vérifiez la taille actuelle de la base de données à l’aide du tableau de bord dans le portail Azure.

   > [!NOTE]
   > Pour identifier les tables qui consomment le plus d’espace et les candidats potentiels au nettoyage, exécutez la requête SQL suivante :

   ```sql
   SELECT o.name,
    a.SUM(p.row_count) AS 'Row Count',
    b.SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. Si la taille actuelle ne dépasse pas la taille maximale prise en charge pour votre édition, vous pouvez utiliser ALTER DATABASE pour augmenter le paramètre MAXSIZE.
3. Si la base de données dépasse déjà la taille maximale prise en charge pour votre édition, essayez l’une de ces procédures :

   * Nettoyez normalement votre base de données. Par exemple, nettoyez les données indésirables en utilisant truncate/delete. Vous pouvez aussi déplacer les données à l’aide de SQL Server Integration Services (SSIS) ou de l’utilitaire Bulk Copy Program (BCP).
   * Partitionnez ou supprimez des données, supprimez des index ou consultez la documentation pour connaître les résolutions possibles.
   * Pour plus d’informations sur la mise à l’échelle des bases de données, consultez [Mettre à l’échelle des ressources de base de données unique](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-scale) et [Mettre à l’échelle des ressources de pool élastique](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool-scale).

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>Erreur 40549 : La session est arrêtée, car l’une des transactions est de longue durée.

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

Si cette erreur survient à plusieurs reprises, vous pouvez essayer de la résoudre en procédant ainsi :

1. Consultez la vue sys.dm_exec_requests pour voir toutes les sessions ouvertes qui ont une valeur élevée pour la colonne total_elapsed_time. Cette vérification s’effectue en exécutant le script SQL suivant :

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. Déterminez la mémoire tampon d’entrée pour la longue requête.
3. Ajustez la requête.

Vous pouvez également traiter vos requêtes par lot. Pour plus d’informations sur le traitement par lot, consultez [Comment utiliser le traitement par lot pour améliorer les performances des applications de base de données SQL](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance).

Pour obtenir une procédure de dépannage détaillée, consultez [Is my query running fine in the cloud?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx) (Ma requête s’exécute-t-elle correctement dans le cloud ?).

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>Erreur 40551 : La session a été arrêtée en raison d’une utilisation excessive de TEMPDB

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

Pour contourner ce problème, effectuez les étapes suivantes :

1. Modifiez les requêtes afin de réduire l’utilisation de l’espace de table temporaire.
2. Supprimez les objets temporaires une fois qu’ils ne sont plus nécessaires.
3. Tronquez les tables ou supprimez les tables inutilisées.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>Erreur 40552 : La session a été arrêtée en raison de l’utilisation excessive de l’espace réservé au journal des transactions

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

Pour corriger ce problème, essayez les méthodes suivantes :

* Ce problème est dû à des opérations d’insertion, de mise à jour ou de suppression.
Essayez de réduire le nombre de lignes qui sont sollicitées immédiatement en implémentant le traitement par lot ou en les fractionnant en plusieurs transactions plus petites.
* Ce problème est dû à des opérations de reconstruction d’index. Pour contourner ce problème, veillez à respecter la formule suivante : nombre de lignes affectées dans la table * (taille moyenne du champ mis à jour en octets + 80) < 2 Go

  > [!NOTE]
  > Pour la reconstruction d’index, la taille moyenne du champ mis à jour doit être remplacée par la taille moyenne de l’index.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>Erreur 40553 : La session a été arrêtée en raison d’une utilisation excessive de la mémoire

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

Pour contourner ce problème, essayez d’optimiser la requête.

Pour obtenir une procédure de dépannage détaillée, consultez [Is my query running fine in the cloud?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx) (Ma requête s’exécute-t-elle correctement dans le cloud ?).

### <a name="table-of-additional-resource-governance-error-messages"></a>Tableau des messages d’erreur de gouvernance des ressources supplémentaires

| Code d'erreur | severity | Description |
| ---:| ---:|:--- |
| 10928 |20 |ID de la ressource : %d. %d, la limite %s de la base de données a été atteinte. Pour en savoir plus, consultez [Limites de ressources de SQL Database pour les bases de données uniques et mises en pool](sql-database-resource-limits-database-server.md).<br/><br/>L’ID de ressource indique la ressource qui a atteint la limite. Pour les threads de travail, ID de la ressource = 1. Pour les sessions, l’ID de ressource = 2.<br/><br/>Pour en savoir plus sur cette erreur et sa résolution, consultez : <br/>&bull; &nbsp;[Limites de ressources du serveur de base de données](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Limites de DTU pour les bases de données uniques](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md) |
| 10929 |20 |ID de la ressource : %d. La garantie minimale de %s est %d ; la limite maximale est de %d et le taux d’utilisation actuel de la base de données est de %d. Toutefois, le serveur est trop occupé pour prendre en charge les requêtes supérieures à %d pour cette base de données. L’ID de ressource indique la ressource qui a atteint la limite. Pour les threads de travail, ID de la ressource = 1. Pour les sessions, l’ID de ressource = 2. Pour plus d'informations, consultez les pages suivantes : <br/>&bull; &nbsp;[Limites de ressources du serveur de base de données](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Limites de DTU pour les bases de données uniques](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md) <br/>Sinon, réessayez plus tard. |
| 40544 |20 |La base de données a atteint son quota de taille. Partitionnez ou supprimez des données, supprimez des index ou consultez la documentation pour connaître les résolutions possibles. Pour plus d’informations sur la mise à l’échelle des bases de données, consultez [Mettre à l’échelle des ressources de base de données unique](sql-database-single-database-scale.md) et [Mettre à l’échelle des ressources de pool élastique](sql-database-elastic-pool-scale.md).|
| 40549 |16 |La session est arrêtée, car l’une des transactions est de longue durée. Essayez de la raccourcir. Pour plus d’informations sur le traitement par lot, consultez [Comment utiliser le traitement par lot pour améliorer les performances des applications de base de données SQL](sql-database-use-batching-to-improve-performance.md).|
| 40550 |16 |La session a été arrêtée, car elle a acquis trop de verrous. Essayez de lire ou de modifier moins de lignes dans une transaction unique. Pour plus d’informations sur le traitement par lot, consultez [Comment utiliser le traitement par lot pour améliorer les performances des applications de base de données SQL](sql-database-use-batching-to-improve-performance.md).|
| 40551 |16 |La session a été arrêtée en raison de l’utilisation excessive de `TEMPDB` . Essayez de modifier votre requête pour réduire l’espace utilisé par la table temporaire.<br/><br/>Si vous utilisez des objets temporaires, conservez de l’espace dans la base de données `TEMPDB` en supprimant des objets temporaires une fois qu’ils ne sont plus nécessaires à la session. Pour plus d’informations sur l’utilisation de tempdb dans SQL Database, consultez [Base de données tempdb dans SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
| 40552 |16 |La session a été arrêtée en raison de l’utilisation excessive de l’espace réservé au journal des transactions. Essayez de modifier moins de lignes dans une transaction unique. Pour plus d’informations sur le traitement par lot, consultez [Comment utiliser le traitement par lot pour améliorer les performances des applications de base de données SQL](sql-database-use-batching-to-improve-performance.md).<br/><br/>Si vous effectuez des insertions en bloc à l’aide de l’utilitaire `bcp.exe` ou de la classe `System.Data.SqlClient.SqlBulkCopy`, essayez d’utiliser les options `-b batchsize` ou `BatchSize` permettant de limiter le nombre de lignes copiées sur le serveur à chaque transaction. Si vous reconstruisez un index en utilisant l’instruction `ALTER INDEX`, essayez d’utiliser l’option `REBUILD WITH ONLINE = ON`. Pour plus d’informations sur les tailles des journaux des transactions pour le modèle d’achat vCore, consultez : <br/>&bull; &nbsp;[Limites de vCores pour les bases de données uniques](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de ressources d’une instance gérée](sql-database-managed-instance-resource-limits.md)|
| 40553 |16 |La session a été arrêtée en raison d’une utilisation excessive de la mémoire. Essayez de modifier votre requête pour traiter moins de lignes.<br/><br/>La diminution du nombre d’opérations `ORDER BY` et `GROUP BY` dans votre code Transact-SQL réduit les besoins en mémoire de votre requête. Pour plus d’informations sur la mise à l’échelle des bases de données, consultez [Mettre à l’échelle des ressources de base de données unique](sql-database-single-database-scale.md) et [Mettre à l’échelle des ressources de pool élastique](sql-database-elastic-pool-scale.md).|

## <a name="elastic-pool-errors"></a>Erreurs relatives au pool élastique

Les erreurs suivantes sont liées à la création et à l’utilisation de pools élastiques :

| Code d'erreur | severity | Description | Action corrective |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |Le pool élastique a atteint sa limite de stockage. Le taux d’utilisation du stockage pour le pool élastique ne doit pas dépasser (%d) Mo. Tentative d’écriture de données dans une base de données alors que la limite de stockage du pool élastique a été atteinte. Pour plus d’informations sur les limites de ressources, consultez : <br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md) <br/> |Envisagez si possible d’augmenter le nombre de DTU du pool élastique ou d’ajouter de la capacité de stockage à ce dernier afin d’accroître sa limite de stockage. Vous pouvez aussi réduire l’espace de stockage utilisé par les bases de données individuelles qu’il contient ou supprimer certaines de ses bases de données. Pour plus d’informations sur la mise à l’échelle d’un pool élastique, consultez [Mettre à l’échelle des ressources de pool élastique](sql-database-elastic-pool-scale.md).|
| 10929 | 16 |La garantie minimale de %s est %d ; la limite maximale est de %d et le taux d’utilisation actuel de la base de données est de %d. Toutefois, le serveur est trop occupé pour prendre en charge les requêtes supérieures à %d pour cette base de données. Pour plus d’informations sur les limites de ressources, consultez : <br/>&bull; &nbsp;[Limites de DTU pour les pools élastiques](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Limites de vCores pour les pools élastiques](sql-database-vcore-resource-limits-elastic-pools.md) <br/> Sinon, réessayez plus tard. Nombre minimal de DTU/vCore par base de données ; nombre maximal de DTU/vCore par base de données. Le nombre total d’ouvriers simultanés (demandes) dans toutes les bases de données du pool élastique a failli dépasser la limite du pool. |Envisagez si possible d’augmenter le nombre de DTU ou de vCore du pool élastique afin d’accroître sa limite de rôles de travail, ou supprimez des bases de données du pool élastique. |
| 40844 | 16 |La base de données '%ls' sur le serveur '%ls' est une base de données présentant l’édition '%ls' dans un pool élastique. Elle ne peut pas présenter de relation de copie continue.  |N/A |
| 40857 | 16 |Pool élastique introuvable pour le serveur : '%ls'. Nom du pool élastique: '%ls'. Le pool élastique spécifié n’existe pas sur le serveur spécifié. | Indiquez un nom de pool élastique valide. |
| 40858 | 16 |Le pool élastique '%ls' existe déjà sur le serveur : '%ls'. Le pool élastique spécifié existe déjà sur le serveur SQL Database spécifié. | Saisissez un nouveau nom pour le pool élastique. |
| 40859 | 16 |Le pool élastique ne prend pas en charge le niveau de service '%ls'. Le niveau de service spécifié n’est pas pris en charge pour le provisioning du pool élastique. |Saisissez l’édition correcte, ou laissez le champ du niveau de service vide afin d’utiliser le niveau de service par défaut. |
| 40860 | 16 |La combinaison du pool élastique '%ls' et de l’objectif de service '%ls' n’est pas valide. Le pool élastique et le niveau de service ne peuvent être spécifiés ensemble que si le type de ressource est spécifié comme étant un « ElasticPool ». |Spécifiez la combinaison de pool élastique et de niveau de service adéquate. |
| 40861 | 16 |L’édition de base de données '%. **ls' ne peut pas être différente du niveau de service du pool élastique, qui correspond à '%.* *ls'. L’édition de base de données est différente du niveau de service du pool élastique. |Indiquez une édition de base de données identique au niveau de service du pool élastique.  Remarque : l’édition de base de données n’a pas besoin d’être spécifiée. |
| 40862 | 16 |Si l’objectif de service du pool élastique est spécifié, le nom du pool élastique doit l’être également. L’objectif de service du pool élastique n’identifie pas de manière unique un pool élastique. |Si vous utilisez l’objectif de service du pool élastique, spécifiez le nom du pool élastique. |
| 40864 | 16 |Le nombre de DTU du pool élastique doit se monter au minimum à (%d) pour le niveau de service '%.*ls'. Tentative de définition du nombre de DTU du pool élastique au-dessous de la limite minimale. |Essayez à nouveau de définir le nombre de DTU du pool élastique sur la limite minimale ou plus. |
| 40865 | 16 |Le nombre de DTU du pool élastique ne doit pas dépasser (%d) pour le niveau de service '%.*ls'. Tentative de définition du nombre de DTU du pool élastique au-dessus de la limite maximale. |Essayez à nouveau de définir le nombre de DTU du pool élastique sur la limite maximale ou moins. |
| 40867 | 16 |Le nombre maximal de DTU par base de données doit être au minimum (%d) pour le niveau de service '%.*ls'. Tentative de définition du nombre maximal de DTU par base de données en dessous de la limite prise en charge. | Envisagez d’utiliser le niveau de service du pool élastique prenant en charge le paramètre souhaité. |
| 40868 | 16 |Le nombre maximal de DTU par base de données ne peut pas dépasser (%d) pour le niveau de service '%.*ls'. Tentative de définition du nombre maximal de DTU par base de données au-delà de la limite prise en charge. | Envisagez d’utiliser le niveau de service du pool élastique prenant en charge le paramètre souhaité. |
| 40870 | 16 |Le nombre minimal de DTU par base de données ne peut pas dépasser (%d) pour le niveau de service '%.*ls'. Tentative de définition du nombre minimal de DTU par base de données au-delà de la limite prise en charge. | Envisagez d’utiliser le niveau de service du pool élastique prenant en charge le paramètre souhaité. |
| 40873 | 16 |Le nombre de bases de données (%d) et le nombre minimal de DTU par base de données (%d) ne peuvent pas dépasser le nombre de DTU du pool élastique (%d). Tentative de spécification d’un nombre minimal de DTU pour les bases de données dans le pool élastique dépassant le nombre de DTU du pool élastique. | Envisagez d’augmenter le nombre de DTU du pool élastique, ou de réduire le nombre minimal de DTU par base de données ou le nombre de bases de données dans le pool élastique. |
| 40877 | 16 |Impossible de supprimer un pool élastique, sauf s’il ne contient aucune base de données. Le pool élastique contient une ou plusieurs bases de données et ne peut donc pas être supprimé. |Supprimez les bases de données du pool élastique afin de pouvoir supprimer ce dernier. |
| 40881 | 16 |Le pool élastique '%.*ls' a atteint le nombre limite de bases de données.  Le nombre limite de bases de données ne peut pas dépasser (%d) pour un pool élastique incluant (%d) DTU. Tentative de création ou d’ajout d’une base de données au pool élastique alors que le nombre limite de bases de données du pool élastique a été atteint. | Envisagez d’augmenter le nombre de DTU du pool élastique, le cas échéant, afin d’accroître le nombre limite de bases de données, ou de supprimer des bases de données du pool élastique. |
| 40889 | 16 |La limite de stockage ou relative aux DTU pour le pool élastique '%.*ls' ne peut pas être abaissée, car l’espace de stockage risque de ne pas être suffisant pour les bases de données de ce pool. Tentative de définition de la limite de stockage du pool élastique en dessous du taux d’utilisation de son espace de stockage. | Envisagez de réduire le taux d’utilisation du stockage des bases de données individuelles dans le pool élastique, ou de supprimer des bases de données dans le pool afin de réduire sa limite de stockage ou celles des DTU. |
| 40891 | 16 |Le nombre minimal de DTU par base de données (%d) ne peut pas dépasser le nombre maximal de DTU par base de données (%d). Tentative de définition d’un nombre minimal de DTU par base de données supérieur au nombre maximal de DTU par base de données. |Vérifiez que le nombre minimal de DTU par base de données ne dépasse pas le nombre maximal de DTU par base de données. |
| TBD | 16 |La taille de l’espace de stockage d’une base de données individuelle dans un pool élastique ne peut pas dépasser la taille maximale autorisée par le pool élastique de niveau de service '%.*ls'. La taille maximale de la base de données dépasse la taille maximale autorisée par le niveau de service du pool élastique. |Définissez la taille maximale de la base de données dans les limites de la taille maximale autorisée par le niveau de service du pool élastique. |

## <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>Impossible d’ouvrir la base de données « Master » demandée par la connexion. La connexion a échoué

Ce problème se produit car le compte ne dispose pas de l’autorisation d’accès à la base de données MASTER. Toutefois, par défaut, SQL Server Management Studio (SSMS) tente de se connecter à la base de données MASTER.

Pour résoudre ce problème, effectuez les étapes suivantes :

1. Sur l’écran de connexion de SSMS, sélectionnez **Options**, puis **Propriétés de la connexion**.
2. Dans le champ **Connexion à une base de données**, entrez le nom de la base de données par défaut de l’utilisateur comme base de données de connexion par défaut, puis sélectionnez **Se connecter**.

   ![Propriétés de connexion](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>Vérifier si une erreur est due à un problème de connectivité

Pour vérifier si une erreur est due à un problème de connectivité, consultez la trace de la pile pour les frames qui montrent des appels visant à ouvrir une connexion, comme les suivants (notez la référence à la classe **SqlConnection**) :

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

Quand l’exception est déclenchée par des problèmes de requête, vous remarquerez une pile des appels semblable à la suivante (notez la référence à la classe **SqlCommand**). Dans ce cas, [affinez vos requêtes](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```

Pour obtenir des conseils supplémentaires sur l’affinage des performances, consultez les ressources suivantes :

* [Comment mettre à jour les index et les statistiques Azure SQL](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [Ajustement manuel des performances de requêtes dans Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
* [Supervision des performances d’Azure SQL Database à l’aide de vues de gestion dynamique](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
* [Utilisation du Magasin des requêtes dans Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)

## <a name="steps-to-fix-common-connection-issues"></a>Étapes pour résoudre les problèmes de connexion courants

1. Vérifiez que l’adresse TCP/IP est activée en tant que protocole client sur le serveur d’applications. Pour plus d’informations, consultez [Configurer des protocoles clients](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols). Sur les serveurs d’applications où aucun outil SQL Server n’est installé, vérifiez que l’adresse TCP/IP est activée en exécutant **cliconfg.exe** (utilitaire réseau du client SQL Server).
2. Vérifiez la chaîne de connexion de l’application pour être sûr qu’elle est configurée correctement. Par exemple, vérifiez que la chaîne de connexion spécifie le port correct (1433) et le nom complet du serveur.
Voir [Obtenir des informations de connexion SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information).
3. Essayez d’augmenter la valeur du délai d’expiration de connexion. Nous vous recommandons d’utiliser un délai d’expiration de connexion d’au moins 30 secondes.
4. Testez la connectivité entre le serveur d’applications et la base de données SQL Azure à l’aide de [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms), un fichier UDL, ping ou Telnet. Pour plus d’informations, consultez [Résolution des erreurs de connectivité à SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) et [Diagnostics pour les problèmes de connectivité](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues#diagnostics).

   > [!NOTE]
   > En guise d’étape de résolution des problèmes, vous pouvez également tester la connectivité sur un autre ordinateur client.

5. Nous vous recommandons de vérifier que la logique de nouvelle tentative est en place. Pour plus d’informations sur la logique de nouvelle tentative, consultez [Gestion des problèmes de connexion et des erreurs temporaires de Microsoft Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

Si ces procédures ne permettent pas de résoudre votre problème, essayez de collecter davantage de données, puis contactez le support. Si votre application est un service cloud, activez la journalisation. Cette étape retourne l’horodatage UTC de l’échec. En outre, SQL Azure retourne l’ID de suivi. Les [services de support technique Microsoft](https://azure.microsoft.com/support/options/) peuvent utiliser ces informations.

Pour plus d’informations sur la façon d’activer la journalisation, consultez [Activer la journalisation des diagnostics pour les applications dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/).

## <a name="next-steps"></a>Étapes suivantes

* [Architecture de connectivité Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture)
* [Contrôles d’accès réseau Azure SQL Database et Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-networkaccess-overview)
