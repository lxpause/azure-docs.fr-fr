---
title: Entité prédéfinie temperature – LUIS
titleSuffix: Azure Cognitive Services
description: Cet article contient des informations sur l’entité prédéfinie temperature dans Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: f2ea08694419caaaf54e4fed45c7c1589be2473d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499516"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Entité prédéfinie temperature pour une application LUIS
L’entité temperature extrait divers types de températures. Étant donné que cette entité est déjà formée, vous n’avez pas besoin d’ajouter d’exemples d’énoncés contenant des températures à l’application. L’entité temperature est prise en charge dans de [nombreuses cultures](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Types d’entités temperature
L’entité temperature est gérée à partir du dépôt GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819)

## <a name="resolution-for-prebuilt-temperature-entity"></a>Résolution pour entité prédéfinie temperature

Les objets d’entité suivants sont retournés pour la requête :

`set the temperature to 30 degrees`


#### <a name="v3-responsetabv3"></a>[Réponse V3](#tab/V3)

Le code JSON suivant a le paramètre `verbose` défini sur `false` :

```json
"entities": {
    "temperature": [
        {
            "number": 30,
            "units": "Degree"
        }
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Réponse détaillée V3](#tab/V3-verbose)
Le code JSON suivant a le paramètre `verbose` défini sur `true` :

```json
"entities": {
    "temperature": [
        {
            "number": 30,
            "units": "Degree"
        }
    ],
    "$instance": {
        "temperature": [
            {
                "type": "builtin.temperature",
                "text": "30 degrees",
                "startIndex": 23,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-responsetabv2"></a>[Réponse V2](#tab/V2)

L’exemple suivant montre la résolution de l’entité **builtin.temperature**.

```json
"entities": [
    {
        "entity": "30 degrees",
        "type": "builtin.temperature",
        "startIndex": 23,
        "endIndex": 32,
        "resolution": {
        "unit": "Degree",
        "value": "30"
        }
    }
]
```
* * * 

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur le [point de terminaison de prédiction V3](luis-migration-api-v3.md).

En savoir plus sur les entités [percentage](luis-reference-prebuilt-percentage.md), [number](luis-reference-prebuilt-number.md) et [age](luis-reference-prebuilt-age.md). 
