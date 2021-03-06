---
title: Ajouter le protocole IPv6 à une application IPv4 dans un réseau virtuel Azure – Azure CLI
titlesuffix: Azure Virtual Network
description: Cet article explique comment déployer des adresses IPv6 dans une application existante dans un réseau virtuel Azure avec Azure CLI.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/23/2019
ms.author: kumud
ms.openlocfilehash: 0631ea51894e7e0642a55cedee54422fddab623b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942074"
---
# <a name="add-ipv6-to-an-ipv4-application-in-azure-virtual-network---azure-cli-preview"></a>Ajouter le protocole IPv6 à une application IPv4 dans un réseau virtuel Azure – Azure CLI (préversion)

Cet article montre comment ajouter des adresses IPv6 à une application qui utilise une adresse IP publique IPv4 dans un réseau virtuel Azure pour un équilibreur de charge Standard Load Balancer avec Azure CLI. La mise à niveau sur place comprend un réseau virtuel et un sous-réseau, un équilibreur de charge standard avec des configurations front-end IPv4 + IPv6, des machines virtuelles avec des cartes réseau qui ont des configurations IPv4 + IPv6, un groupe de sécurité réseau et des adresses IP publiques.

> [!Important]
> La prise en charge du protocole IPv6 par le réseau virtuel Azure est actuellement en préversion publique. Cette préversion est fournie sans contrat de niveau de service et n’est pas recommandée pour les charges de travail de production. Certaines fonctionnalités peuvent être limitées ou non prises en charge. Consultez les [Conditions d’utilisation supplémentaires des préversions de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous décidez d’installer et d’utiliser Azure CLI en local, ce guide de démarrage rapide nécessite que vous utilisiez Azure CLI version 2.0.28 ou ultérieure. Exécutez `az --version` pour rechercher la version installée. Pour des informations d'installation ou de mise à niveau, consultez [Installer Azure CLI](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Prérequis

### <a name="register-the-service"></a>Enregistrer le service

Pour pouvoir déployer une application double pile dans Azure, vous devez configurer votre abonnement pour cette fonctionnalité en préversion avec l’interface Azure CLI suivante :

```azurecli
az feature register --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
az feature register --name AllowIPv6CAOnStandardLB --namespace Microsoft.Network
```
Il faut compter 30 minutes pour l’inscription de la fonctionnalité. Vous pouvez vérifier l’état de votre enregistrement en exécutant la commande Azure CLI suivante :

```azurelci
az feature show --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
az feature show --name AllowIPv6CAOnStandardLB --namespace Microsoft.Network
```
À l’issue de l’installation, exécutez la commande suivante :

```azurelci
az provider register --namespace Microsoft.Network
```

### <a name="create-a-standard-load-balancer"></a>Créer un équilibreur de charge standard
Cet article suppose que vous avez déployé un équilibreur de charge standard comme cela est décrit dans [Démarrage rapide : Créer un équilibreur Standard Load Balancer – Azure CLI](../load-balancer/quickstart-load-balancer-standard-public-cli.md).

## <a name="create-ipv6-addresses"></a>Créer des adresses IPv6

Créez des adresses IPv6 publiques avec [az network public-ip create](/cli/azure/network/public-ip) pour votre équilibreur Standard Load Balancer. L’exemple suivant crée une adresse IP publique IPv6 nommée *PublicIP_v6* dans le groupe de ressources *myResourceGroupSLB* :

```azurecli
  
az network public-ip create \
--name PublicIP_v6 \
--resource-group MyResourceGroupSLB \
--location EastUS \
--sku Standard \
--allocation-method static \
--version IPv6
```

## <a name="configure-ipv6-load-balancer-frontend"></a>Configurer l’équilibreur de charge IPv6 front-end

Configurez l’équilibreur de charge avec la nouvelle adresse IP IPv6 avec [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create) :

```azurecli
az network lb frontend-ip create \
--lb-name myLoadBalancer \
--name dsLbFrontEnd_v6 \
--resource-group MyResourceGroupSLB \
--public-ip-address PublicIP_v6
```

## <a name="configure-ipv6-load-balancer-backend-pool"></a>Configurer le pool back-end de l’équilibreur de charge IPv6

Créez le pool back-end pour les cartes réseau comportant des adresses IPv6 avec [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create) :

```azurecli
az network lb address-pool create \
--lb-name myLoadBalancer \
--name dsLbBackEndPool_v6 \
--resource-group MyResourceGroupSLB
```

## <a name="configure-ipv6-load-balancer-rules"></a>Configurer les règles d’équilibrage de charge IPv6

Créez des règles d’équilibrage de charge IPv6 avec [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create).

```azurecli
az network lb rule create \
--lb-name myLoadBalancer \
--name dsLBrule_v6 \
--resource-group MyResourceGroupSLB \
--frontend-ip-name dsLbFrontEnd_v6 \
--protocol Tcp \
--frontend-port 80 \
--backend-port 80 \
--backend-pool-name dsLbBackEndPool_v6
```

## <a name="add-ipv6-address-ranges"></a>Ajouter des plages d’adresses IPv6

Ajoutez des plages d’adresses IPv6 au réseau virtuel et au sous-réseau hébergeant l’équilibreur de charge, comme suit :

```azurecli
az network vnet update \
--name myVnet  `
--resource-group MyResourceGroupSLB \
--address-prefixes  "10.0.0.0/16"  "ace:cab:deca::/48"

az network vnet subnet update \
--vnet-name myVnet \
--name mySubnet \
--resource-group MyResourceGroupSLB \
--address-prefixes  "10.0.0.0/24"  "ace:cab:deca:deed::/64"  
```

## <a name="add-ipv6-configuration-to-nics"></a>Ajouter la configuration IPv6 à des cartes réseau

Configurez les cartes réseau de machines virtuelles comportant une adresse IPv6 avec [az network nic ip-config create](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create) :

```azurecli
az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name myNicVM1 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC2 \
--nic-name myNicVM2 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name myLoadBalancer

az network nic ip-config create \
--name dsIp6Config_NIC3 \
--nic-name myNicVM3 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name myLoadBalancer

```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Afficher le réseau virtuel double pile IPv6 dans le Portail Microsoft Azure
Vous pouvez afficher le réseau virtuel double pile IPv6 dans le Portail Microsoft Azure en procédant comme suit :
1. Dans la barre de recherche du portail, entrez *myVnet*.
2. Quand le nom **myVnet** apparaît dans les résultats de la recherche, sélectionnez-le. Cela permet d’afficher la page **Vue d’ensemble** du réseau virtuel double pile nommé *myVNet*. Le réseau virtuel double pile montre les trois cartes réseau, avec des configurations IPv4 et IPv6 situées dans le sous-réseau double pile nommé *mySubnet*.

  ![Réseau virtuel double pile IPv6 dans Azure](./media/ipv6-add-to-existing-vnet-powershell/ipv6-dual-stack-vnet.png)

> [!NOTE]
> L’adresse IPv6 du réseau virtuel Azure est disponible dans le Portail Microsoft Azure en lecture seule pour cette préversion.

## <a name="clean-up-resources"></a>Supprimer des ressources

Quand vous n’en avez plus besoin, vous pouvez utiliser la commande [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) pour supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées.

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyAzureResourceGroupSLB
```

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez mis à jour un équilibreur de charge standard existant avec une configuration d’adresse IP front-end IPv4 en une configuration double pile (IPv4 et IPv6). Vous avez également ajouté des configurations IPv6 aux cartes réseau des machines virtuelles du pool back-end. Pour en savoir plus sur la prise en charge du protocole IPv6 dans les réseaux virtuels Azure, voir [What is IPv6 for Azure Virtual Network?](ipv6-overview.md) (Rôle d’iPv6 pour un réseau virtuel Azure).
