---
title: Exemple de script Azure CLI - Exporter à partir d’un magasin Azure App Configuration | Microsoft Docs
description: Fournit des informations et des exemples de scripts pour l’exportation à partir d’un magasin Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 7969300e9865c943c0f3759277bd26529c0c5464
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74184883"
---
# <a name="export-from-an-azure-app-configuration-store"></a>Exporter à partir d’un magasin Azure App Configuration

Cet exemple de script exporte des valeurs de clés à partir d’un magasin Azure App Configuration.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande localement, vous devez exécuter Azure CLI version 2.0 ou une version ultérieure pour poursuivre la procédure décrite dans cet article. Exécutez `az --version` pour trouver la version. Si vous devez effectuer une installation ou une mise à niveau, consultez [Installer Azure CLI](/cli/azure/install-azure-cli).

Vous devez d’abord installer l’extension CLI d’Azure App Configuration en exécutant la commande suivante :

        az extension add -n appconfig

## <a name="sample-script"></a>Exemple de script

```azurecli-interactive
#!/bin/bash

# Export all key-values
az appconfig kv export --name myTestAppConfigStore --file ~/Export.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour exporter à partir d’un magasin App Configuration. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [az appconfig kv export](/cli/azure/ext/appconfig/appconfig/kv#ext-appconfig-az-appconfig-kv-export) | Exporte à partir d’une ressource de magasin App Configuration. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

Vous trouverez des exemples supplémentaires de scripts d’interface de ligne de commande App Configuration dans les [exemples d’interface de ligne de commande Azure App Configuration](../cli-samples.md).
