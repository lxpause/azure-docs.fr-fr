---
title: Effectuer une rotation des certificats dans Azure Kubernetes Service (AKS)
description: Découvrez comment assurer la rotation de vos certificats dans un cluster Azure Kubernetes Service (AKS).
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 00d8546cb20d12c5f1a94bdcababa04a77c73133
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134217"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Effectuer une rotation des certificats dans Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) utilise des certificats pour l’authentification avec un grand nombre de ses composants. Régulièrement, vous pouvez être amené à procéder à une rotation de ces certificats pour des raisons de sécurité ou de stratégie. Par exemple, une stratégie peut demander une rotation de l’ensemble de vos certificats tous les 90 jours.

Cet article explique comment effectuer une rotation des certificats de votre cluster AKS.

## <a name="before-you-begin"></a>Avant de commencer

Pour les besoins de cet article, vous devez utiliser Azure CLI version 2.0.76 ou ultérieure. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI][azure-cli-install].


### <a name="install-aks-preview-cli-extension"></a>Installer l’extension CLI de préversion d’aks

Pour utiliser cette fonctionnalité, vous avez besoin de l’extension CLI *aks-preview* version 0.4.21 ou ultérieure. Installez l’extension Azure CLI *aks-preview* à l’aide de la commande [az extension add][az-extension-add], puis recherchez toutes les mises à jour disponibles à l’aide de la commande [az extension update][az-extension-update] :

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>Certificats AKS, autorités de certification et comptes de service

AKS génère et utilise les certificats, autorités de certification et comptes de service suivants :

* Le serveur d’API AKS crée une autorité de certification (CA) appelée « autorité de certification de cluster ».
* Le serveur d’API dispose d’une autorité de certification de cluster, qui signe les certificats pour la communication unidirectionnelle, du serveur d’API vers les kubelets.
* Chaque kubelet crée également une demande de signature de certificat (CSR), qui est signée par l’autorité de certification de cluster, pour la communication du kubelet vers le serveur d’API.
* Le magasin de valeurs de clés etcd dispose d’un certificat signé par l’autorité de certification de cluster, pour la communication émanant de etcd à destination du serveur d’API.
* Le magasin de valeurs de clé etcd crée une autorité de certification qui signe les certificats pour authentifier et autoriser la réplication des données entre les réplicas etcd du cluster AKS.
* L’agrégation d’API utilise l’autorité de certification de cluster pour émettre des certificats destinés à la communication avec d’autres API, telles que Open Service Broker pour Azure. L’agrégation d’API peut également disposer de sa propre autorité de certification pour émettre ces certificats, mais elle utilise actuellement l’autorité de certification de cluster.
* Chaque nœud utilise un jeton de compte de service (SA), qui est signé par l’autorité de certification de cluster.
* Le client `kubectl` dispose d’un certificat pour communiquer avec le cluster AKS.

> [!NOTE]
> Les clusters AKS créés avant le mois de mars 2019 possèdent des certificats qui expirent au bout de deux ans. Tout cluster créé après mars 2019 ou tout cluster dont la rotation des certificats a été effectuée dispose de certificats qui expirent au bout de 30 ans.

## <a name="rotate-your-cluster-certificates"></a>Procéder à la rotation de vos certificats de cluster

> [!WARNING]
> La rotation de vos certificats à l’aide de `az aks rotate-certs` peut entraîner jusqu’à 30 minutes d’interruption pour votre cluster AKS.

Utilisez [az aks get-credentials][az-aks-get-credentials] pour vous connecter à votre cluster AKS. Cette commande télécharge et configure également le certificat client `kubectl` sur votre machine locale.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

Utilisez `az aks rotate-certs` pour procéder à la rotation de tous les certificats, autorités de certification et comptes de services de votre cluster.

```console
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> L’exécution de `az aks rotate-certs` peut prendre jusqu’à 30 minutes. Si la commande échoue avant de se terminer, utilisez `az aks show` pour vérifier que l’état du cluster est *Certificate Rotating* (Rotation de certificat en cours). Si le cluster est dans l’état d’échec, réexécutez `az aks rotate-certs` pour procéder de nouveau à la rotation de vos certificats.

Vérifiez que les anciens certificats ne sont plus valides en exécutant une commande `kubectl`. Étant donné que vous n’avez pas mis à jour les certificats utilisés par `kubectl`, une erreur s’affiche.  Par exemple :

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

Mettez à jour le certificat utilisé par `kubectl` en exécutant `az aks get-credentials`.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

Vérifiez que les certificats ont été mis à jour en exécutant une commande `kubectl` qui, à présent, doit aboutir. Par exemple :

```console
kubectl get no
```

## <a name="next-steps"></a>Étapes suivantes

Cet article vous a montré comment procéder automatiquement à la rotation des certificats, autorités de certification et comptes de service de votre cluster. Pour plus d’informations sur les bonnes pratiques en matière de sécurité AKS, vous pouvez consulter [Meilleures pratiques relatives aux mises à jour et à la sécurité du cluster dans Azure Kubernetes Service (AKS)][aks-best-practices-security-upgrades].


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
