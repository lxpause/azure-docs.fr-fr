---
title: Entité prédéfinie keyPhrase - LUIS
titleSuffix: Azure Cognitive Services
description: Cet article contient des informations sur l’entité prédéfinie keyPhrase dans Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 51d1bd515651824545d486207ad4a74476aa7092
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491270"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>Entité prédéfinie keyPhrase pour une application LUIS
L’entité KeyPhrase extrait une variété d’expressions clés d’un énoncé. Il est inutile d’ajouter des exemples d’énoncés contenant keyPhrase à l’application. L’entité keyPhrase est prise en charge dans de [nombreuses cultures](luis-language-support.md#languages-supported) dans le cadre des fonctionnalités d’[analyse de texte](../text-analytics/overview.md). 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Résolution pour une entité prédéfinie keyPhrase

Les objets entités suivants sont retournés pour la requête :

`where is the educational requirements form for the development and engineering group`

#### <a name="v3-responsetabv3"></a>[Réponse V3](#tab/V3)

Le code JSON suivant a le paramètre `verbose` défini sur `false` :

```json
"entities": {
    "keyPhrase": [
        "educational requirements",
        "development"
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Réponse détaillée V3](#tab/V3-verbose)
Le code JSON suivant a le paramètre `verbose` défini sur `true` :

```json
"entities": {
    "keyPhrase": [
        "educational requirements",
        "development"
    ],
    "$instance": {
        "keyPhrase": [
            {
                "type": "builtin.keyPhrase",
                "text": "educational requirements",
                "startIndex": 13,
                "length": 24,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "development",
                "startIndex": 51,
                "length": 11,
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

L’exemple suivant montre la résolution de l’entité **builtin.keyPhrase**.

```json
"entities": [
    {
        "entity": "development",
        "type": "builtin.keyPhrase",
        "startIndex": 51,
        "endIndex": 61
    },
    {
        "entity": "educational requirements",
        "type": "builtin.keyPhrase",
        "startIndex": 13,
        "endIndex": 36
    }
]
```
* * * 

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur le [point de terminaison de prédiction V3](luis-migration-api-v3.md).

En savoir plus sur les entités [percentage](luis-reference-prebuilt-percentage.md), [number](luis-reference-prebuilt-number.md) et [age](luis-reference-prebuilt-age.md).
