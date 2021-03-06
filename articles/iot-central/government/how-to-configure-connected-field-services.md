---
title: Connecter votre application Azure IoT Central avec Dynamics 365 for Field Service | Microsoft Docs
description: Découvrez comment créer une solution de bout en bout avec Azure IoT Central et Dynamics 365 for Field Service
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 957e9337bf8ea5941b140ba4f3266417d36df6a7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498771"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Créer une solution de bout en bout avec Azure IoT Central et Dynamics 365 for Field Service 

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En tant que générateur, vous pouvez activer l’intégration de votre application Azure IoT Central à d’autres systèmes métier. 


Par exemple, dans une solution de gestion des déchets connectée, vous pouvez optimiser l’envoi des camions de ramassage des déchets sur la base des données de capteurs IoT équipant les conteneurs de déchets connectés. Dans votre [application de gestion des déchets connectée à IoT Central](./tutorial-connected-waste-management.md), vous pouvez configurer des règles et actions qui déclenchent des alertes dans Dynamics 365 for Field Service. Ce scénario est implémenté à l’aide de Microsoft Flow, que vous pouvez configurer directement dans IoT Central pour automatiser les flux de travail entre les applications et les services. Par ailleurs, en fonction des activités de service au sein de Dynamics 365 for Field Service, des informations peuvent être renvoyées à Azure IoT Central. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Comment connecter votre application Azure IoT Central avec Dynamics 365 for Field Service 

Vous pouvez facilement implémenter les processus d’intégration de bout en bout ci-dessous au travers d’une expérience de configuration pure :
* Azure IoT Central peut envoyer des informations relatives à des anomalies de périphériques (sous forme d’alertes IoT) à un service de terrain connecté à des fins de diagnostic.
* Ce service de terrain connecté peut créer des cas de support ou des ordres de travail déclenchés à partir d’anomalies de périphériques.
* Il peut également planifier des techniciens pour l’inspection afin d’éviter les temps d’arrêt.
* Le tableau de bord d’appareil Azure IoT Central peut être mis à jour avec les informations de planification et de service appropriées.


## <a name="next-steps"></a>Étapes suivantes
* Apprenez-en davantage sur les [modèles IoT Central Government](./overview-iot-central-government.md)
* Apprenez-en davantage sur [IoT Hub](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central)
* Apprenez-en davantage sur [Dynamics 365 for Field Service](https://docs.microsoft.com/dynamics365/field-service/cfs-iot-overview)
