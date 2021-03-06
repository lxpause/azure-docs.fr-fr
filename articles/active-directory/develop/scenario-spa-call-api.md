---
title: Générer une application monopage appelant une API web - Plateforme d’identités Microsoft | Azure
description: Découvrir comment générer une application monopage qui appelle une API web
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1171d8c3bc28c7b325cc8daf6cc072965363339c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965329"
---
# <a name="single-page-application-call-a-web-api"></a>Application monopage : Appeler une API web

Nous vous recommandons d’appeler la méthode `acquireTokenSilent` pour acquérir ou renouveler un jeton d’accès avant d’appeler une API web. Après avoir obtenu un jeton, vous pouvez appeler une API web protégée.

## <a name="call-a-web-api"></a>Appeler une API web

### <a name="javascript"></a>JavaScript

Utilisez le jeton d’accès acquis, en tant que porteur dans une requête HTTP, pour appeler une API web, telle que l’API Microsoft Graph. Par exemple :

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

Le wrapper MSAL Angular tire parti de l’intercepteur HTTP pour acquérir automatiquement des jetons d’accès en mode silencieux, et les joindre aux requêtes HTTP destinées aux API. Pour plus d’informations, consultez [Acquérir un jeton pour appeler une API](scenario-spa-acquire-token.md).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Passer en production](scenario-spa-production.md)
