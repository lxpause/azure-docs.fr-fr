---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 07780c94960c581ce69e4fdd45b346c75ab8b098
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329553"
---
### <a name="set-the-storage-account-connection"></a>Définir la connexion de compte de stockage

Ouvrez le fichier local.settings.json et copiez la valeur d’`AzureWebJobsStorage`, qui est la chaîne de connexion de compte de stockage. Définissez la variable d’environnement `AZURE_STORAGE_CONNECTION_STRING` sur la chaîne de connexion à l’aide de la commande Bash suivante :

```azurecli-interactive
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

Lorsque vous définissez la chaîne de connexion dans la variable d’environnement `AZURE_STORAGE_CONNECTION_STRING`, vous pouvez accéder à votre compte de stockage sans devoir vous authentifier à chaque fois.