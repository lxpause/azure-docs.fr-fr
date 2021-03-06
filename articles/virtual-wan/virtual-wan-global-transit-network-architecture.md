---
title: Architecture du réseau de transit global Azure Virtual WAN | Microsoft Docs
description: En savoir plus sur l’architecture du réseau de transit global pour Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 10/18/2019
ms.author: cherylmc
ms.openlocfilehash: 9b736e897278c4656df83c30388efc812ac6193d
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607363"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Architecture du réseau de transit global Virtual WAN

Les entreprises modernes ont besoin d’une connectivité omniprésente entre les applications hyper-distribuées, les données et les utilisateurs dans le cloud et en local. L’architecture du réseau de transit global est adoptée par les entreprises pour consolider, connecter et contrôler l’empreinte informatique de l’entreprise mondiale moderne tournée vers le cloud.

L’architecture du réseau de transit global est basée sur un modèle classique de connectivité en étoile où le « hub » du réseau hébergé sur le cloud permet une connectivité transitive entre les terminaux qui peut être répartie sur différents types de « rayons ».

Dans ce modèle, un rayon peut être :
* Un réseau virtuel (VPN)
* Site de branche physique
* Utilisateur distant
* Internet

![Hub-and-spoke](./media/virtual-wan-global-transit-network-architecture/figure1.png)

**Figure 1 : Réseau hub-and-spoke de transit global**

La figure 1 montre la vue logique du réseau de transit global où les utilisateurs répartis géographiquement, les sites physiques et les réseaux virtuels sont interconnectés via un hub réseau hébergé dans le cloud. Cette architecture permet une connectivité de transit à un seul tronçon logique entre les points de terminaison du réseau.

## <a name="globalnetworktransit"></a>Réseau de transit global avec Virtual WAN

Azure Virtual WAN est un service réseau cloud managé par Microsoft. Tous les composants réseau qui composent ce service sont hébergés et managés par Microsoft. Pour plus d’informations sur Azure Virtual WAN, consultez [Vue d’ensemble d’Azure Virtual WAN](virtual-wan-about.md).

Azure Virtual WAN permet une architecture de réseau de transit global en fournissant une connectivité omniprésente et universelle entre les groupes distribués mondialement de charges de travail cloud dans les réseaux virtuels, les sites de succursale, les applications SaaS et PaaS, et les utilisateurs.

![WAN virtuel Azure](./media/virtual-wan-global-transit-network-architecture/figure2.png)

**Figure 2 : Réseau de transit global et Virtual WAN**

Dans l’architecture Azure Virtual WAN, les hubs WAN virtuels sont provisionnés dans des régions Azure, auxquelles vous pouvez choisir de connecter vos branches, réseaux virtuels et utilisateurs distants. Les sites de branche physiques sont connectés au hub avec Premium ExpressRoute ou des VPN site à site ; les réseaux virtuels sont connectés au hub par des connexions de réseau virtuel ; et les utilisateurs distants peuvent se connecter directement au hub à l’aide d’un VPN utilisateur (VPN point à site). Virtual WAN prend également en charge la connexion de réseaux virtuels inter-régions qui permet de connecter un réseau virtuel dans une région à un hub WAN virtuel situé dans une autre région.

Vous pouvez établir un WAN virtuel en créant un seul hub WAN virtuel dans la région qui possède le plus grand nombre de rayons (branches, réseaux virtuels, utilisateurs), puis en connectant à ce hub les rayons qui se trouvent dans d’autres régions. Cette approche est conseillée quand l’empreinte informatique de l’entreprise se limite principalement à une région avec quelques rayons à distance.  
  
## <a name="hubtohub"></a>Connectivité de hub à hub

L’empreinte cloud d’une entreprise peut s’étendre sur plusieurs régions du cloud et, dans ce cas, il est primordial (du point de vue de la latence) d’accéder au cloud à partir d’une région la plus proche possible de son site physique et de ses utilisateurs. Un des principes clés de l’architecture du réseau de transit global consiste à permettre une connectivité inter-régions entre tous les points de terminaison des réseaux cloud et locaux. Cela signifie que le trafic à partir d’une branche connectée au cloud dans une région peut atteindre une autre branche ou un réseau virtuel dans une région différente grâce à la connectivité de hub à hub fournie par [Azure Global Network](https://azure.microsoft.com/global-infrastructure/global-network/).

![inter-régions](./media/virtual-wan-global-transit-network-architecture/figure3.png)

**Figure 3 : Connectivité inter-régions de Virtual WAN**

Quand plusieurs hubs sont activés dans un seul WAN virtuel, les hubs sont automatiquement interconnectés par des liaisons de hub à hub, ce qui fournit une connectivité globale entre les branches et les réseaux virtuels répartis dans des régions différentes. 

De plus, tous les hubs qui font partie du même WAN virtuel peuvent être associés à des stratégies de sécurité et d’accès différentes selon les régions. Pour plus d’informations, consultez [Contrôle de la sécurité et de la stratégie](#security), plus loin dans cet article.

## <a name="anytoany"></a>Connexion universelle

L’architecture du réseau de transit global permet une connectivité universelle via des hubs WAN virtuels. Cette architecture élimine ou réduit le besoin d’un maillage complet ou partiel de la connectivité entre les rayons, qui sont plus complexes à créer et gérer. En outre, comparativement aux réseaux maillés, le contrôle du routage dans le réseau en étoile est plus facile à configurer et gérer.

La connectivité universelle (dans le contexte d’une architecture globale) permet à une entreprise d’interconnecter ses utilisateurs, succursales, centres de données, réseaux virtuels et applications répartis dans le monde entier via un ou plusieurs hubs de « transit ». Azure Virtual WAN agit en tant que système de transit global.

![universel](./media/virtual-wan-global-transit-network-architecture/figure4.png)

**Figure 4 : Chemins d’accès du trafic Virtual WAN**

Azure Virtual WAN prend en charge les chemins d’accès de connectivité de transit global suivants : Les lettres entre parenthèses renvoient à la Figure 4.

* Branche à réseau virtuel (a)
* Branche à branche (b)
  * ExpressRoute Global Reach et Virtual WAN
* Utilisateur distant à réseau virtuel (c)
* Utilisateur distant à branche (d)
* Réseau virtuel à réseau virtuel (e)
* Branche à hub - Hub à branche (f)
* Branche à hub - Hub à réseau virtuel (g)
* Réseau virtuel à hub - Hub à réseau virtuel (h)

### <a name="branch-to-vnet-a-and-branch-to-vnet-cross-region-g"></a>Branche à réseau virtuel (a) et Branche à réseau virtuel inter-régions (g)

Branche à réseau virtuel est le principal chemin d’accès pris en charge par Azure Virtual WAN. Ce chemin d’accès vous permet de connecter des branches aux charges de travail d’entreprise Azure IAAS déployées dans Azure VNets. Les branches peuvent être connectées au WAN virtuel via ExpressRoute ou un VPN site à site. Le trafic transite vers des réseaux virtuels qui sont connectés aux hubs WAN virtuels par le biais de connexions de réseau virtuel. Le [transit par passerelle](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) explicite n’est pas requis pour Virtual WAN, car Virtual WAN active automatiquement le transit par passerelle vers le site de succursale. Pour plus d’informations sur la connexion d’un CPE SD-WAN à Virtual WAN, consultez [Partenaires Virtual WAN](virtual-wan-configure-automation-providers.md).

### <a name="expressroute-global-reach-and-virtual-wan"></a>ExpressRoute Global Reach et Virtual WAN

ExpressRoute vous permet de connecter vos réseaux locaux à Microsoft Cloud de manière privée et résiliente. Virtual WAN prend en charge les connexions de circuits ExpressRoute. La connexion d’un site de succursale à Virtual WAN avec ExpressRoute nécessite 1) un circuit Premium 2) que ce circuit se trouve dans un emplacement avec Global Reach.

ExpressRoute Global Reach est une fonctionnalité de module complémentaire pour ExpressRoute. Avec Global Reach, vous pouvez associer des circuits ExpressRoute afin de constituer un réseau privé entre vos réseaux locaux. Les branches connectées à Azure Virtual WAN à l’aide d’ExpressRoute nécessitent ExpressRoute Global Reach pour communiquer entre elles.

Dans ce modèle, chaque branche connectée au hub WAN virtuel à l’aide d’ExpressRoute peut se connecter à des réseaux virtuels utilisant le chemin « Branche à réseau virtuel ». Le trafic de branche à branche ne transite pas par le hub, car ExpressRoute Global Reach offre un meilleur chemin d’accès sur Azure WAN.

### <a name="branch-to-branch-b-and-branch-to-branch-cross-region-f"></a>Branche à branche (b) et Branche à branche inter-régions (f)

Les branches peuvent être connectées à un hub WAN virtuel par des circuits ExpressRoute et/ou des connexions VPN de site à site. Vous pouvez connecter les branches au hub WAN virtuel qui se trouve dans la région la plus proche de la branche.

Cette option permet aux entreprises de tirer parti de la dorsale principale d’Azure pour connecter des branches. Toutefois, même si cette fonctionnalité est disponible, vous devez comparer les avantages de la connexion des branches via Azure Virtual WAN à l’utilisation d’un réseau WAN privé.  

### <a name="remote-user-to-vnet-c"></a>Utilisateur distant à réseau virtuel (c)

Vous pouvez activer l’accès à distance direct et sécurisé à Azure à l’aide d’une connexion point à site entre un utilisateur distant et un WAN virtuel. Les utilisateurs distants d’entreprise n’ont plus besoin d’accéder au cloud en utilisant un VPN d’entreprise.

### <a name="remote-user-to-branch-d"></a>Utilisateur distant à branche (d)

Le chemin d’accès utilisateur distant à branche permet aux utilisateurs distants qui utilisent une connexion point à site sur Azure d’accéder aux charges de travail locales et aux applications en transit à travers le cloud. Ce chemin d’accès offre aux utilisateurs distants la flexibilité d’accéder aux charges de travail qui sont déployées à la fois dans Azure et en local. Les entreprises peuvent activer un service central d’accès distant sécurisé basé sur le cloud dans Azure Virtual WAN.

### <a name="vnet-to-vnet-transit-e-and-vnet-to-vnet-cross-region-h"></a>Réseau virtuel à réseau virtuel (e) et Réseau virtuel à réseau virtuel inter-régions (h)

Le transit de réseau virtuel à réseau virtuel permet de relier des réseaux virtuels entre eux afin d’interconnecter les applications multiniveau qui sont distribuées sur plusieurs réseaux virtuels. Vous pouvez également connecter des réseaux virtuels entre eux par le biais du peering de réseaux virtuels. Cette approche est plus adaptée dans certains scénarios où le transit via le hub VWAN n’est pas nécessaire.

## <a name="security"></a>Contrôle de la sécurité et de la stratégie

Les hubs Azure Virtual WAN interconnectent tous les points de terminaison réseau sur le réseau hybride et peuvent potentiellement voir l’ensemble du trafic sur le réseau de transit. Il est possible de changer des hubs Virtual WAN en hubs virtuels sécurisés en déployant le Pare-feu Azure à l’intérieur des hubs VWAN pour contrôler la stratégie, l’accès et la sécurité dans le cloud. L’orchestration des Pare-feu Azure dans les hubs WAN virtuels peut être effectuée par Azure Firewall Manager.

[Azure Firewall Manager](https://go.microsoft.com/fwlink/?linkid=2107683) fournit les fonctionnalités nécessaires pour gérer et adapter la sécurité des réseaux de transit global. Avec Azure Firewall Manager, vous pouvez gérer de manière centralisée le routage, les stratégies globales, les services de sécurité Internet avancée par le biais d’un tiers et du Pare-feu Azure.

![hub virtuel sécurisé avec le Pare-feu Azure](./media/virtual-wan-global-transit-network-architecture/figure5.png)

**Figure 5 : Hub virtuel sécurisé avec le Pare-feu Azure**

Le Pare-feu Azure dans le hub WAN virtuel prend en charge les chemins de connectivité suivants pour le transit de sécurisé global. Les lettres entre parenthèses renvoient à la Figure 5.

* Transit sécurisé de réseau virtuel à réseau virtuel (e)
* Réseau virtuel à Internet ou un service de sécurité tiers (i)
* Branche à Internet ou un service de sécurité tiers (j)

### <a name="vnet-to-vnet-secured-transit-e"></a>Transit sécurisé de réseau virtuel à réseau virtuel (e)

Le transit sécurisé de réseau virtuel à réseau virtuel permet d’interconnecter des réseaux virtuels par le biais du Pare-feu Azure dans le hub WAN virtuel.

### <a name="vnet-to-internet-or-third-party-security-service-i"></a>Réseau virtuel à Internet ou un service de sécurité tiers (i)

Le transit sécurisé de réseau virtuel à Internet ou un tiers permet de connecter des réseaux virtuels à Internet ou à des services de sécurité tiers pris en charge, par le biais du Pare-feu Azure dans le hub WAN virtuel.

### <a name="branch-to-internet-or-third-party-security-service-j"></a>Branche à Internet ou un service de sécurité tiers (j)
Le transit sécurisé de branche à Internet ou un tiers permet de connecter des branches à Internet ou à des services de sécurité tiers pris en charge, par le biais du Pare-feu Azure dans le hub WAN virtuel.

## <a name="next-steps"></a>Étapes suivantes

Créez une connexion à l’aide de Virtual WAN et déployez le Pare-feu Azure dans les hubs VWAN.

* [Connexions de site à site à l’aide de Virtual WAN](virtual-wan-site-to-site-portal.md)
* [Connexions ExpressRoute à l’aide du Virtual WAN](virtual-wan-expressroute-portal.md)
* [Azure Firewall Manager pour déployer le Pare-feu Azure dans un WAN virtuel](https://go.microsoft.com/fwlink/?linkid=2107683)
