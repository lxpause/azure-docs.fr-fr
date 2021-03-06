---
title: Tester une application dans le portail LUIS
titleSuffix: Azure Cognitive Services
description: Utilisez LUIS (Language Understanding) pour travailler en continu sur votre application afin d’affiner et d’améliorer sa compréhension de la langue.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 628547e8254bb0055cf1f09af50e79b68311a759
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74221782"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Tester votre application LUIS dans le portail LUIS

[Le test](luis-concept-test.md) d’une application est un processus itératif. Après avoir formé votre application LUIS, testez-la avec des exemples d’énoncés afin de voir si les intentions et les entités sont reconnues correctement. Si ce n’est pas le cas, apportez des mises à jour à l’application LUIS, puis formez-la et testez-la à nouveau. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Effectuer l’apprentissage avant le test

Pour tester la version la plus récente de l’application active, sélectionnez **Effectuer l’apprentissage** dans le menu supérieur avant de réaliser le test. 

## <a name="test-an-utterance"></a>Tester un énoncé

L’énoncé de test ne doit pas être exactement le même que les exemples d’énoncés donnés dans l’application. Il doit inclure le choix de mots, la longueur de l’expression et l’utilisation de l’entité que vous attendez d’un utilisateur. 

1. Accédez à votre application en sélectionnant son nom dans la page **My Apps** (Mes applications). 

1. Pour accéder au panneau déroulant **Test**, sélectionnez **Test** dans le panneau supérieur de votre application.

    > [!div class="mx-imgBorder"]
    > ![Page Effectuer l’apprentissage de l’application et la tester](./media/luis-how-to-interactive-test/test.png)

1. Entrez un énoncé dans la zone de texte, puis sélectionnez Entrée. Vous pouvez entrer autant d’énoncés de test que vous le souhaitez dans **Test**, mais uniquement un énoncé à la fois.

1. L’énoncé, son intention principale et son score sont ajoutés à la liste des énoncés sous la zone de texte.

    ![Le test interactif identifie l’intention incorrecte](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Inspecter le score

Vous inspectez les détails du résultat du test dans le panneau **Inspecter**. 
 
1. Avec le panneau déroulant **Test** ouvert, sélectionnez **Inspecter** pour consulter un énoncé à comparer. 

    ![Sélectionnez le bouton Inspecter pour voir plus de détails sur les résultats de test](./media/luis-how-to-interactive-test/inspect.png)

1. Le panneau **Inspection** s’affiche. Le panneau inclut l’intention de score la plus élevée, ainsi que toutes les entités identifiées. Le panneau affiche le résultat de l’énoncé sélectionné.

    ![Le panneau inclut l’intention de score la plus élevée, ainsi que toutes les entités identifiées. Le panneau affiche le résultat de l’énoncé sélectionné.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Intention correcte avec le score le plus élevé

1. Si l’intention avec le score le plus élevé est incorrecte, sélectionnez le bouton **Modifier**.

1.  Dans la liste déroulante, sélectionnez l’intention appropriée pour l’énoncé.

    ![Sélectionner l’intention correcte](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Afficher les résultats des sentiments

Si **Analyse des sentiments** est configurée sur la page **[Publier](luis-how-to-publish-app.md#enable-sentiment-analysis)** , les résultats du test incluent le sentiment trouvé dans l’énoncé. 

![Image du panneau Test avec l’analyse des sentiments](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Corriger l’intention du modèle mis en correspondance

Si vous utilisez [Modèles](luis-concept-patterns.md) et si l’énoncé a été mis en correspondance avec un modèle, mais que l’intention prédite est incorrecte, sélectionnez le lien **Modifier** en regard du modèle, puis sélectionnez l’intention correcte.

## <a name="compare-with-published-version"></a>Comparer avec la version publiée

Vous pouvez tester la version active de votre application avec la version de [point de terminaison](luis-glossary.md#endpoint) publiée. Dans le panneau **Inspecter**, sélectionnez **Comparer avec la version publiée**. Tout test exécuté sur le modèle publié est déduit du solde de votre quota d’abonnement Azure. 

![Comparer avec la version publiée](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Afficher le JSON de point de terminaison dans le panneau de test
Vous pouvez afficher le JSON de point de terminaison retourné pour la comparaison en sélectionnant la **vue Afficher JSON**.

![Réponse JSON publiée](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Paramètres supplémentaires dans le panneau de test

### <a name="luis-endpoint"></a>Point de terminaison LUIS

Si vous avez plusieurs points de terminaison LUIS, utilisez le lien **Paramètres supplémentaires** dans le panneau Test publié pour modifier le point de terminaison utilisé pour le test. Si vous ne savez pas quel point de terminaison utiliser, sélectionnez la valeur par défaut **Starter_Key**. 

> [!div class="mx-imgBorder"]
> ![Panneau de test avec le lien Paramètres supplémentaires mis en surbrillance](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)

<!--
###  View Bing Spell Check corrections in test panel

Requirements to view the spelling corrections: 

* Published app
* Bing Spell Check [service key](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). The service key is not stored and needs to be reset for each browser session. 

Use the following procedure to include the [Bing Spell Check v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) service  in the Test pane results. 

1. In the **Test** pane, enter an utterance. When the utterance is predicted, select **[Inspect](#inspect-score)** underneath the utterance you entered. 

1. When the **Inspect** panel opens, select **[Compare with Published](#compare-with-published-version)**. 

1. When the **Published** panel opens, select **[Additional Settings](#additional-settings-in-test-panel)**.

1. In the pop-up dialog, check **Enable Bing Spell Check** and enter the key, then select **Done**. 
    ![Enter Bing Spell Check service key](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key-text.png)

1. Enter a query with an incorrect spelling such as `book flite to seattle` and select enter. The incorrect spelling of the word `flite` is replaced in the query sent to LUIS and the resulting JSON shows both the original query, as `query`, and the corrected spelling in the query, as `alteredQuery`.

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
-->

## <a name="batch-testing"></a>Test par lot
Consultez les [concepts](luis-concept-batch-test.md) du test par lot et apprenez [comment](luis-how-to-batch-test.md) tester un lot d’énoncés.

## <a name="next-steps"></a>Étapes suivantes

Si le test indique que votre application LUIS ne reconnaît pas les entités et les intentions correctes, vous pouvez travailler à l’amélioration de la précision de votre application LUIS en étiquetant plus d’énoncés ou en ajoutant des fonctionnalités. 

* [Étiqueter des énoncés suggérés avec LUIS](luis-how-to-review-endpoint-utterances.md) 
* [Utiliser les fonctionnalités pour améliorer les performances de votre application LUIS](luis-how-to-add-features.md) 
