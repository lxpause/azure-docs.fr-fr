---
title: Clouds et régions dans lesquels Azure Media Services v3 est disponible | Microsoft Docs
description: Cet article traite des clouds Azure et régions dans lesquels Azure Media Services v3 est disponible.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/07/2019
ms.author: juliako
ms.openlocfilehash: 7b2691f543cf38a56eefb1e8521169aeccbf3221
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65409279"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Clouds et régions dans lesquels Azure Media Services v3 existe

Azure Media Services v3 est disponible via le manifeste Azure Resource Manager dans Azure global, Azure Government, Azure Germany, Azure China 21Vianet. Cela étant, toutes les fonctionnalités de Media Services ne sont pas disponibles dans l'ensemble des clouds Azure. Ce document présente la disponibilité des principaux composants Media Services v3.

## <a name="feature-availability-in-azure-clouds"></a>Disponibilité des fonctionnalités dans les clouds Azure

| Fonctionnalité|Régions Azure à l'échelle internationale | Azure Government|Azure Germany|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Disponible | Non disponible | Non disponible | Non disponible |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Disponible | Non disponible | Non disponible | Non disponible |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Disponible | Non disponible | Non disponible | Non disponible |
| [StandardEncoderPreset](encoding-concept.md) | Disponible | Disponible | Disponible | Disponible |
| [LiveEvents](live-streaming-overview.md) | Disponible | Disponible | Disponible | Disponible |
| [StreamingEndpoints](streaming-endpoint-concept.md) | Disponible | Disponible | Disponible | Disponible |

## <a name="regionsgeographieslocations"></a>Régions/zones géographiques/emplacements

[Régions dans lesquelles le service Azure Media Services est déployé](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)

### <a name="region-code-name"></a>Nom de code de la région 

S'il vous faut fournir le paramètre **location**, vous devez fournir le nom de code de la région comme valeur **location**. Pour obtenir le nom de code de la région dans laquelle se trouve votre compte et vers laquelle votre appel doit être routé, vous pouvez exécuter la ligne suivante dans l’[interface de ligne de commande Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

```bash
az account list-locations
```

Une fois que vous avez exécuté la ligne ci-dessus, vous obtenez une liste de toutes les régions Azure. Accédez à la région Azure qui présente la valeur *displayName* que vous recherchez et utilisez sa valeur *name* pour le paramètre **location**.

Par exemple, pour la région Azure West US 2 (USA Ouest 2) affichée ci-dessous, vous allez utiliser « westus2 » pour le paramètre **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="endpoints"></a>Points de terminaison  

Il est important de connaître les points de terminaison suivants lors de la connexion à des comptes Media Services à partir de différents clouds Azure nationaux.

### <a name="global-azure"></a>Azure global

|Points de terminaison ||
| --- | --- | 
| Azure Resource Manager |  `https://management.azure.com/` |
| Authentication | `https://login.microsoftonline.com/` | 
| Audience du jeton | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Government

|Points de terminaison||
| --- | --- | 
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| Authentication | `https://login.microsoftonline.us/` | 
| Audience du jeton | `https://management.core.usgovcloudapi.net/` |

### <a name="azure-germany"></a>Azure Germany

| Points de terminaison ||
| --- | --- |  
| Azure Resource Manager | `https://management.cloudapi.de/` |
| Authentication | `https://login.microsoftonline.de/` |
| Audience du jeton | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

|Points de terminaison||
| --- | --- | 
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| Authentication | `https://login.chinacloudapi.cn/` |
| Audience du jeton |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>Voir aussi

* [Régions Azure](https://azure.microsoft.com/global-infrastructure/regions/)
* [Zones géographiques Azure](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Emplacements Azure](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>Étapes suivantes

[Vue d’ensemble de Media Services v3](media-services-overview.md)
