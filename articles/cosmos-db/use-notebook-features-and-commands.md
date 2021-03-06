---
title: Utiliser les fonctionnalités et commandes de notebook intégrées dans Azure Cosmos DB
description: Découvrez comment utiliser les fonctionnalités et commandes intégrées pour effectuer des opérations courantes à l’aide des notebooks intégrés d’Azure Cosmos DB.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: dech
ms.openlocfilehash: 4a9bd554e0858024d656dbf35d6fb00995e6f4bd
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672484"
---
# <a name="use-built-in-notebook-commands-and-features-in-azure-cosmos-db"></a>Utiliser les fonctionnalités et commandes de notebook intégrées dans Azure Cosmos DB

Les notebooks Jupyter intégrés dans Azure Cosmos DB vous permettent d’analyser et de visualiser vos données via le portail Azure. Cet article explique comment utiliser les fonctionnalités et commandes de notebook intégrées pour effectuer des opérations courantes.

## <a name="install-a-new-package"></a>Installer un nouveau package
Dès lors que vous avez activé la prise en charge des notebooks pour vos comptes Azure Cosmos, vous pouvez ouvrir un nouveau notebook et installer un package.

Dans une nouvelle cellule de code, insérez et exécutez le code suivant en remplaçant ``PackageToBeInstalled`` par le package Python souhaité.
```python
import sys
!{sys.executable} -m pip install PackageToBeInstalled –user
```
Ce package pourra être utilisé à partir de n’importe quel notebook de l’espace de travail du compte Azure Cosmos. 

> [!TIP]
> Si votre notebook nécessite un package personnalisé, nous vous recommandons d’y ajouter une cellule pour installer le package. En effet, si vous [réinitialisez l’espace de travail](#reset-notebooks-workspace), les packages sont supprimés.  

## <a name="run-a-sql-query"></a>Exécuter une requête SQL

Vous pouvez utiliser la commande magic ``%%sql`` pour exécuter une [requête SQL](sql-query-getting-started.md) sur un conteneur quelconque de votre compte. Utilisez la syntaxe suivante :

```bash
%%sql --database {database_id} --container {container_id}
{Query text}
```

- Remplacez ``{database_id}`` et ``{container_id}`` par le nom de la base de données et du conteneur dans votre compte Cosmos. Si les arguments ``--database`` et ``--container`` ne sont pas fournis, la requête est exécutée sur la [base de données et le conteneur par défaut](#set-default-database-for-queries).
- Vous pouvez exécuter n’importe quelle requête SQL valide dans Azure Cosmos DB. Le texte de la requête doit figurer sur une nouvelle ligne.

Par exemple : 
```bash
%%sql --database RetailDemo --container WebsiteData
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
Exécutez ```%%sql?``` dans une cellule pour afficher la documentation d’aide sur la commande magic SQL dans le notebook.

## <a name="run-a-sql-query-and-output-to-a-pandas-dataframe"></a>Exécuter une requête SQL et générer les résultats dans un DataFrame Pandas

Vous pouvez générer les résultats d’une requête ``%%sql`` dans un [DataFrame Pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html#dataframe). Utilisez la syntaxe suivante : 

```bash
%%sql --database {database_id} --container {container_id} --output {outputDataFrameVar}
{Query text}
```
- Remplacez ``{database_id}`` et ``{container_id}`` par le nom de la base de données et du conteneur dans votre compte Cosmos. Si les arguments ``--database`` et ``--container`` ne sont pas fournis, la requête est exécutée sur la [base de données et le conteneur par défaut](#set-default-database-for-queries).
- Remplacez ``{outputDataFrameVar}`` par le nom de la variable DataFrame qui contiendra les résultats.
- Vous pouvez exécuter n’importe quelle requête SQL valide dans Azure Cosmos DB. Le texte de la requête doit figurer sur une nouvelle ligne. 

Par exemple :

```bash
%%sql --database RetailDemo --container WebsiteData --output df_cosmos
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
```bash
df_cosmos.head(10)

    Action  ItemRevenue Country Item
0   Viewed  9.00    Tunisia Black Tee
1   Viewed  19.99   Antigua and Barbuda Flannel Shirt
2   Added   3.75    Guinea-Bissau   Socks
3   Viewed  3.75    Guinea-Bissau   Socks
4   Viewed  55.00   Czech Republic  Rainjacket
5   Viewed  350.00  Iceland Cosmos T-shirt
6   Added   19.99   Syrian Arab Republic    Button-Up Shirt
7   Viewed  19.99   Syrian Arab Republic    Button-Up Shirt
8   Viewed  33.00   Tuvalu  Red Top
9   Viewed  14.00   Cape Verde  Flip Flop Shoes
```

## <a name="set-default-database-for-queries"></a>Définir la base de données par défaut pour les requêtes
Vous pouvez définir la base de données par défaut qui sera utilisée par les commandes ```%%sql``` pour le notebook. Remplacez ```{database_id}``` par le nom de votre base de données.

```bash
%database {database_id}
```
Exécutez ```%database?``` dans une cellule pour afficher la documentation dans le notebook.

## <a name="set-default-container-for-queries"></a>Définir le conteneur par défaut pour les requêtes
Vous pouvez définir le conteneur par défaut qui sera utilisé par les commandes ```%%sql``` pour le notebook. Remplacez ```{container_id}``` par le nom de votre conteneur.

```bash
%container {container_id}
```
Exécutez ```%container?``` dans une cellule pour afficher la documentation dans le notebook.

## <a name="use-the-built-in-python-sdk"></a>Utiliser le SDK Python intégré
La version 4 du [SDK Python Azure Cosmos DB pour API SQL](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) est installée et incluse dans l’environnement du notebook pour le compte Cosmos.

Utilisez l’instance ``cosmos_client`` intégrée pour exécuter n’importe quelle opération du SDK. 

Par exemple :

```python
## Import modules as needed
from azure.cosmos.partition_key import PartitionKey

## Create a new database if it doesn't exist
database = cosmos_client.create_database_if_not_exists('RetailDemo')

## Create a new container if it doesn't exist
container = database.create_container_if_not_exists(id='WebsiteData', partition_key=PartitionKey(path='/CartID'))
```
Consultez [Exemples du kit SDK Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos/samples). 

> [!IMPORTANT]
> Le SDK Python intégré est pris en charge uniquement avec les comptes d’API SQL (Core). Pour les autres API, vous devez [installer le pilote Python](#install-a-new-package) correspondant à l’API. 

## <a name="create-a-custom-instance-of-cosmos_client"></a>Créer une instance personnalisée de ``cosmos_client``
Pour plus de flexibilité, vous pouvez créer une instance personnalisée de ``cosmos_client``. Vous pourrez ainsi :

- Personnaliser la [stratégie de connexion](https://docs.microsoft.com/python/api/azure-cosmos/azure.cosmos.documents.connectionpolicy?view=azure-python-preview)
- Exécuter des opérations sur un compte Cosmos différent de celui dans lequel vous vous trouvez

Vous pouvez accéder à la chaîne de connexion et à la clé primaire du compte actuel à l’aide des [variables d’environnement](#access-the-account-endpoint-and-primary-key-env-variables). 

```python
import os
import azure.cosmos.cosmos_client as cosmos
import azure.cosmos.documents as documents

# These should be set to a region you've added for Cosmos DB
region_1 = "Central US" 
region_2 = "East US 2"

custom_connection_policy = documents.ConnectionPolicy()
custom_connection_policy.PreferredLocations = [region_1, region_2] # Set the order of regions the SDK will route requests to. The regions should be regions you've added for Cosmos, otherwise this will error.

# Create a new instance of CosmosClient, getting the endpoint and key from the environment variables
custom_client = cosmos.CosmosClient(os.environ["COSMOS_ENDPOINT"], {'masterKey': os.environ["COSMOS_KEY"]}, connection_policy=custom_connection_policy)
```
## <a name="access-the-account-endpoint-and-primary-key-env-variables"></a>Accéder aux variables d’environnement de point de terminaison et de clé primaire du compte
```python
import os

endpoint = os.environ["COSMOS_ENDPOINT"]
primary_key = os.environ["COSMOS_KEY"]
```
> [!IMPORTANT]
> Les variables d’environnement ``COSMOS_ENDPOINT`` et ``COSMOS_KEY`` s’appliquent uniquement à l’API SQL. Pour les autres API, recherchez le point de terminaison et la clé dans le panneau **Chaînes de connexion** ou **Clés** de votre compte Cosmos.  

## <a name="reset-notebooks-workspace"></a>Réinitialiser l’espace de travail des notebooks
Pour rétablir les paramètres par défaut de l’espace de travail des notebooks, sélectionnez **Réinitialiser l’espace de travail** dans la barre de commandes. Cette opération supprime tous les packages personnalisés installés et redémarre le serveur Jupyter. Vos notebooks, fichiers et ressources Cosmos ne seront pas affectés.  

![Réinitialiser l’espace de travail des notebooks](media/use-notebook-features-and-commands/reset-workspace.png)

## <a name="next-steps"></a>Étapes suivantes

- Découvrez les avantages de [notebooks Jupyter d’Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)
- Apprenez-en davantage sur le [Kit Python Azure Cosmos DB pour API SQL](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos).
