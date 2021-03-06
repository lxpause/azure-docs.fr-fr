---
title: Utiliser Azure Event Grid avec des événements dans le schéma CloudEvents
description: Décrit comment définir le schéma CloudEvents pour les événements dans Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: babanisa
ms.openlocfilehash: 6a0e24ce7fa11c6373fbaada40cd9f1b1e7f55a2
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325470"
---
# <a name="use-cloudevents-v10-schema-with-event-grid"></a>Utiliser le schéma CloudEvents v1.0 avec Event Grid

En plus de son [schéma d’événement par défaut](event-schema.md), Azure Event Grid prend en charge en mode natif les événements dans l’[implémentation JSON de CloudEvents v1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) et la [liaison de protocole HTTP](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md). [CloudEvents](https://cloudevents.io/) est une [spécification ouverte](https://github.com/cloudevents/spec/blob/v1.0/spec.md) qui décrit les données d’événement.

CloudEvents simplifie l’interopérabilité grâce à un schéma d’événement commun pour la publication et la consommation des événements basés sur le cloud. Ce schéma permet l’utilisation d’outils uniformes, la mise en œuvre de méthodes de routage et de gestion des événements standard, ainsi que de méthodes universelles pour désérialiser le schéma d’événement externe. Avec un schéma commun, vous pouvez plus facilement intégrer le travail entre plusieurs plateformes.

Plusieurs [collaborateurs](https://github.com/cloudevents/spec/blob/master/community/contributors.md), dont Microsoft, travaillent à l’élaboration de CloudEvents par le biais de la [Cloud Native Computing Foundation](https://www.cncf.io/). Il est actuellement disponible en version 1.0.

Cet article décrit comment utiliser le schéma CloudEvents avec Event Grid.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Installer la fonctionnalité d'évaluation

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Schéma CloudEvents

Voici un exemple d’événement Stockage Blob Azure dans le format CloudEvents :

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

Une description détaillée des champs disponibles, de leurs types et définitions dans CloudEvents v 0.1 est [disponible ici](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes).

Les valeurs des en-têtes pour les événements remis dans le schéma CloudEvents et le schéma Event Grid sont identiques, à l’exception de `content-type`. Pour le schéma CloudEvents, cette valeur d’en-tête est `"content-type":"application/cloudevents+json; charset=utf-8"`. Pour le schéma Event Grid, cette valeur d’en-tête est `"content-type":"application/json; charset=utf-8"`.

## <a name="configure-event-grid-for-cloudevents"></a>Configurer Event Grid pour CloudEvents

Vous pouvez utiliser Event Grid pour l’entrée et la sortie des événements dans le schéma CloudEvents. Vous pouvez utiliser CloudEvents pour des événements système, tels que les événements Stockage Blob et les événements IoT Hub, et des événements personnalisés. Il peut aussi transformer ces événements dans un sens ou dans l’autre.


| Schéma d’entrée       | Schéma de sortie
|--------------------|---------------------
| Format CloudEvents | Format CloudEvents
| Format Event Grid  | Format CloudEvents
| Format CloudEvents | Format Event Grid
| Format Event Grid  | Format Event Grid

Pour tous les schémas d’événement, Event Grid requiert une validation au moment de la publication sur une rubrique de grille d’événement et de la création d’un abonnement aux événements. Pour en savoir plus, consultez la page [Sécurité et authentification pour Event Grid](security-authentication.md).

### <a name="input-schema"></a>Schéma d’entrée

Vous définissez le schéma d’entrée pour une rubrique personnalisée lorsque vous la créez.

Pour l’interface de ligne de commande Azure, consultez :

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventschemav1_0
```

Pour PowerShell, utilisez la commande suivante :

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventSchemaV1_0
```

La version actuelle de CloudEvents ne prend pas en charge le traitement par lots des événements. Pour publier des événements avec le schéma CloudEvents sur une rubrique, publiez chaque événement individuellement.

### <a name="output-schema"></a>Schéma de sortie

Vous définissez le schéma de sortie lorsque vous créez l’abonnement aux événements.

Pour l’interface de ligne de commande Azure, consultez :

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventschemav1_0
```

Pour PowerShell, utilisez la commande suivante :
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventSchemaV1_0
```

 Actuellement, vous ne pouvez pas utiliser un déclencheur Event Grid pour une application Azure Functions quand l’événement est remis dans le schéma CloudEvents. Utilisez un déclencheur HTTP. Pour obtenir des exemples d’implémentation d’un déclencheur HTTP qui reçoit des événements dans le schéma CloudEvents, consultez [Utiliser un déclencheur HTTP comme déclencheur Event Grid](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

 ## <a name="endpoint-validation-with-cloudevents-v10"></a>Validation de point de terminaison avec CloudEvents v1.0

Si vous êtes déjà familiarisé avec Event Grid, vous pouvez être conscient de la négociation de validation de point de terminaison de Event Grid pour empêcher les abus. CloudEvents v1.0 implémente sa propre [sémantique de protection contre les abus](security-authentication.md#webhook-event-delivery) à l’aide de la méthode HTTP OPTIONS. Pour plus d'informations à ce sujet, cliquez [ici](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection). Lors de l’utilisation du schéma CloudEvents pour la sortie, Event Grid l’utilise avec la protection contre les abus CloudEvents v1.0 à la place du mécanisme d’événement de validation Event Grid.

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’information sur la surveillance des remises des événements, consultez [Surveiller la remise des messages Event Grid](monitor-event-delivery.md).
* Nous vous encourageons à tester et à commenter CloudEvents, ainsi qu’à y [contribuer](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md).
* Pour plus d’informations sur la création d’un abonnement Azure Event Grid, consultez [Schéma d’abonnement à Event Grid](subscription-creation-schema.md).
