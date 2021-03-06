---
title: Prise en main d’Azure Cost Management pour les partenaires
description: Cet article explique aux partenaires comment utiliser les fonctionnalités d’Azure Cost Management et comment autoriser l’accès à Cost Management à leurs clients.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/15/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: ecef301d2745cf7c86f61f0ffa9106c7bfd10623
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74219222"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>Prise en main d’Azure Cost Management pour les partenaires

Azure Cost Management est disponible de façon native pour les partenaires qui ont intégré leurs clients à un contrat client Microsoft et [acheté un plan Azure](/partner-center/purchase-azure-plan). Cet article explique comment les partenaires utilisent les fonctionnalités d’[Azure Cost Management](index.yml) pour afficher les coûts des abonnements dans le plan Azure. Il leur explique également comment autoriser l’accès à Cost Management à leurs clients. Les clients peuvent utiliser les fonctionnalités de Cost Management si elles sont activées par leur partenaire CSP.

Les partenaires CSP utilisent Cost Management pour :

- Comprendre les coûts facturés et associer les coûts au client, aux abonnements, aux groupes de ressources et aux services.
- Bénéficier d’une présentation intuitive des coûts liés à Azure avec la vue d’[analyse des coûts](quick-acm-cost-analysis.md), qui offre des fonctionnalités d’analyse par client, par abonnement, par groupe de ressources, par ressource, par compteur, par service et selon de nombreux autres aspects.
- Visualiser les coûts des ressources pour lesquels un crédit Partenaires (PEC, Partner Earned Credit) est appliqué dans l’analyse des coûts.
- Configurer des notifications et une automatisation à l’aide de [budgets](tutorial-acm-create-budgets.md) programmatiques ainsi que des alertes quand les coûts dépassent les budgets.
- Activer la stratégie Azure Resource Manager qui accorde aux clients l’accès aux données de Cost Management. Les clients peuvent ensuite voir les données de coût de consommation pour leurs abonnements selon les [tarifs de paiement à l’utilisation](https://azure.microsoft.com/pricing/calculator/).

Voici un exemple montrant les coûts pour tous les clients.
![Exemple montrant les coûts pour tous les clients](./media/get-started-partners/customer-costs1.png)

Voici un exemple montrant les coûts pour un client unique.
![Exemple montrant les coûts pour un client unique](./media/get-started-partners/customer-costs2.png)

Toutes les fonctionnalités disponibles dans Azure Cost Management sont également disponibles avec les API REST. Utilisez les API pour automatiser les tâches de gestion des coûts.

## <a name="prerequisites"></a>Prérequis

Azure Cost Management nécessite un accès en lecture à votre abonnement ou votre compte de facturation. L’accès peut être accordé à n’importe quel niveau au-dessus de vos ressources : du compte de facturation ou d’un groupe d’administration jusqu’aux groupes de ressources individuels où vous gérez vos applications. Pour plus d’informations sur l’autorisation et l’attribution de l’accès à Azure Cost Management pour un compte de facturation, consultez [Attribuer des rôles et des autorisations aux utilisateurs](/partner-center/permissions-overview). Les rôles **Administrateur général** et **Agent d’administration** peuvent gérer les coûts d’un compte de facturation.

Pour accéder à la liste complète des types de comptes pris en charge, consultez [Comprendre les données de Cost Management](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Utilisation des étendues par Cost Management

Les étendues vous permettent de gérer les données de facturation, de définir des rôles propres aux paiements, d’afficher les factures et de gérer les comptes de manière générale. Les rôles de facturation et de compte sont gérés séparément des étendues utilisées pour la gestion des ressources, qui utilisent le contrôle d’accès en fonction du rôle (RBAC). Pour distinguer clairement l’intention des différentes étendues, y compris les différences en termes de contrôle d’accès, elles sont désignées comme étendues de facturation et étendues RBAC, respectivement.

Pour comprendre les étendues de facturation, les étendues RBAC et le fonctionnement de la gestion des coûts avec les étendues, consultez [Comprendre et utiliser des étendues](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>Gérer les coûts avec les étendues de facturation de locataire partenaire

Une fois que vous avez intégré vos clients à un contrat client Microsoft, les _étendues de facturation_ suivantes sont disponibles dans votre locataire. Utilisez les étendues pour gérer les coûts dans Cost Management.

### <a name="billing-account-scope"></a>Étendue de compte de facturation

Utilisez l’étendue de compte de facturation afin de voir les coûts avant impôts pour l’ensemble de vos clients et profils de facturation. Les coûts facturés liés aux produits basés sur la consommation sont uniquement affichés pour les clients bénéficiant du contrat client Microsoft. Cependant, les coûts facturés sont indiqués pour les produits basés sur les achats pour les clients bénéficiant du contrat client Microsoft et de l’offre CSP. Actuellement, la devise utilisée par défaut pour l’affichage des coûts dans l’étendue est le dollar américain. Les budgets définis pour l’étendue sont également exprimés en USD.

Quelles que soient les différentes devises de facturation du client, les partenaires utilisent l’étendue du compte de facturation afin de définir les budgets et de gérer les coûts en USD pour les clients, les abonnements, les ressources et les groupes de ressources.

Les partenaires filtrent également les coûts selon une devise de facturation spécifique pour les clients dans la vue d’analyse des coûts. Sélectionnez la liste **Coût réel** pour voir les coûts dans les devises de facturation client prises en charge.

![Exemple montrant le choix de devises pour le coût réel](./media/get-started-partners/actual-cost-selector.png)

Utilisez la [vue des coûts amortis](quick-acm-cost-analysis.md#customize-cost-views) dans les étendues de facturation pour voir les coûts amortis des instances réservées sur une période de réservation.

### <a name="billing-profile-scope"></a>Étendue de profil de facturation

Utilisez l’étendue de profil de facturation pour voir les coûts avant impôts dans la devise de facturation pour tous vos clients et pour tous les produits et abonnements inclus dans une facture. Vous pouvez filtrer les coûts dans un profil de facturation pour une facture spécifique à l’aide du filtre **InvoiceID**. Le filtre permet d’afficher les coûts de consommation et d’achat de produits pour une facture spécifique. Vous pouvez également filtrer les coûts pour un client spécifique sur la facture pour voir les coûts avant impôts.

Une fois que vous avez intégré les clients à un contrat client Microsoft, vous recevez une facture incluant tous les frais pour tous les produits (consommation, achats et droits) pour ces clients disposant d’un contrat de client Microsoft. Quand elles sont établies avec la même devise, ces factures incluent également les frais relatifs au droit et aux produits achetés, tels que SaaS, la Place de marché Azure et les réservations pour les clients qui utilisent encore l’offre CSP.

Pour simplifier le rapprochement des frais avec la facture client, l’étendue de profil de facturation vous permet de voir tous les coûts s’accumulant sur une facture pour vos clients. À l’instar de la facture, l’étendue indique les coûts pour chaque client bénéficiant du nouveau contrat client Microsoft. L’étendue indique également tous les frais pour les produits de droit client toujours inclus dans l’offre CSP actuelle.

Les étendues de profil et de compte de facturation sont les seules qui indiquent les frais pour les produits de droit et basés sur les achats, tels que la Place de marché Azure et les achats de réservation.

Les profils de facturation définissent les abonnements inclus dans une facture. Les profils de facturation sont l’équivalent fonctionnel d’une inscription à un contrat Entreprise. Un profil de facturation correspond à l’étendue dans laquelle les factures sont générées.

Actuellement, la devise de facturation du client est la devise utilisée par défaut quand vous consultez les coûts dans l’étendue de profil de facturation. Les budgets définis dans l’étendue de profil de facturation sont exprimés dans la devise de facturation.

Les partenaires peuvent utiliser l’étendue pour effectuer un rapprochement avec les factures. Par ailleurs, ils utilisent l’étendue afin de définir des budgets dans la devise de facturation pour les éléments suivants :

- Facture filtrée spécifique
- Client
- Subscription
- Resource group
- Ressource
- Service Azure
- Compteur
- ResellerMPNID

### <a name="customer-scope"></a>Étendue de client

Les partenaires utilisent l’étendue pour gérer les coûts associés aux clients intégrés au contrat client Microsoft. L’étendue permet aux partenaires de voir les coûts avant impôts pour un client spécifique. Vous pouvez également filtrer les coûts avant impôts pour un abonnement, un groupe de ressources ou une ressource spécifique.

L’étendue de client n’inclut pas les clients bénéficiant de l’offre CSP actuelle. L’étendue comprend uniquement les clients qui disposent d’un contrat client Microsoft. Les coûts de droit (et non les coûts d’utilisation d’Azure) pour les clients de l’offre CSP actuelle sont disponibles dans les étendues de compte de facturation et de profil de facturation quand vous appliquez le filtre client.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Accès des partenaires aux étendues de facturation dans Cost Management

Seuls les utilisateurs ayant les rôles **Administrateur général** et **Agent d’administration** peuvent gérer et voir les coûts pour les comptes de facturation, les profils de facturation et les clients directement dans le locataire Azure du partenaire. Pour plus d’informations sur les rôles de l’espace Partenaires, consultez [Affecter des rôles et des autorisations aux utilisateurs](/partner-center/permissions-overview).

## <a name="enable-cost-management-in-the-customer-tenant"></a>Autoriser l’accès à la gestion des coûts dans le locataire client

Les partenaires peuvent autoriser l’accès à Cost Management une fois les clients intégrés à un contrat client Microsoft. Ils peuvent ainsi activer une stratégie permettant aux clients de voir leurs coûts calculés selon les tarifs de paiement à l’utilisation au détail. Les coûts sont indiqués dans la devise de facturation du client pour sa consommation dans les étendues de groupe de ressources et d’abonnement RBAC.

Quand la stratégie de visualisation des coûts est activée par le partenaire, tout utilisateur disposant d’un accès Azure Resource Manager à l’abonnement peut gérer et analyser les coûts selon les tarifs de paiement à l’utilisation. En effet, les revendeurs et les clients qui disposent de l’accès RBAC approprié aux abonnements Azure peuvent voir les coûts.

Quelle que soit la stratégie appliquée, les partenaires peuvent également voir les coûts s’ils ont accès à l’abonnement et au groupe de ressources.

### <a name="enable-the-policy-to-view-azure-usage-charges"></a>Activer la stratégie de visualisation des frais d’utilisation d’Azure

Les partenaires utilisent les informations suivantes pour activer la stratégie de visualisation des frais d’utilisation d’Azure pour leurs clients.

Dans le portail Azure, connectez-vous au locataire partenaire, puis cliquez sur **Gestion des coûts + facturation**. Sélectionnez un compte de facturation, puis cliquez sur **Clients**. La liste des clients est associée au compte de facturation.

Dans la liste des clients, sélectionnez le client auquel vous souhaitez donner l’autorisation de voir les coûts.

![Sélectionner des clients dans Cost Management](./media/get-started-partners/customer-list.png)

Sous **Paramètres**, cliquez sur **Stratégies**.

La stratégie de visualisation des coûts actuelle est indiquée pour les frais d’**utilisation d’Azure** associés aux abonnements du client sélectionné.
![Stratégie permettant aux clients de voir les frais de paiement à l’utilisation](./media/get-started-partners/cost-management-billing-policies.png)

Quand la stratégie est définie sur **Non**, Azure Cost Management n’est pas disponible pour les utilisateurs d’abonnement associés au client. La stratégie de visualisation des coûts est désactivée par défaut pour tous les utilisateurs d’abonnement sauf si elle est activée par un partenaire.

Quand la stratégie de coût est définie sur **Oui**, les utilisateurs d’abonnement associés au locataire client peuvent voir les frais d’utilisation selon les tarifs de paiement à l’utilisation.

Quand la stratégie de visualisation des coûts est activée, tous les services associés à l’utilisation d’abonnements indiquent les coûts selon les tarifs de paiement à l’utilisation. L’utilisation de réservations apparaît avec des frais nuls pour les coûts réels et amortis. Les achats et les droits ne sont pas associés à un abonnement spécifique. Ainsi, les achats ne sont pas affichés dans l’étendue d’abonnement.

Pour voir les coûts relatifs au locataire client, ouvrez la vue Gestion des coûts + facturation, puis cliquez sur Comptes de facturation. Cliquez sur un compte dans la liste des comptes de facturation.

![Sélectionner un compte de facturation](./media/get-started-partners/select-billing-account.png)

Sous **Facturation**, cliquez sur **Abonnements Azure**, puis cliquez sur un client.

![Sélectionner un client d’abonnement Azure](./media/get-started-partners/subscriptions-select-customer.png)

Cliquez sur **Analyse des coûts** et commencez à examiner les coûts.
L’analyse des coûts, les budgets et les alertes sont disponibles pour les étendues RBAC de groupe de ressources et d’abonnement selon les tarifs de paiement à l’utilisation.

![Voir l’analyse des coûts en tant que client ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

Les vues amorties et les coûts réels pour les instances réservées dans les étendues RBAC indiquent des frais nuls. Les coûts des instances réservées sont indiqués uniquement dans les étendues de facturation dans lesquelles les achats ont été effectués.

## <a name="analyze-costs-in-cost-analysis"></a>Analyser les coûts dans la vue d’analyse des coûts

Les partenaires peuvent explorer et analyser les coûts dans la vue d’analyse des coûts pour l’ensemble des clients, pour un client spécifique ou pour une facture. Dans la vue d’[analyse des coûts](quick-acm-cost-analysis.md), vous pouvez également [enregistrer des vues](quick-acm-cost-analysis.md#saving-and-sharing-customized-views) et exporter des données dans des fichiers [CSV et PNG](quick-acm-cost-analysis.md#automation-and-offline-analysis).

Vous pouvez utiliser les fonctionnalités Filtrer et Regrouper lors de l’analyse des coûts pour analyser les frais en fonction de plusieurs champs. Les champs spécifiques au partenaire sont affichés dans la section suivante.

## <a name="data-fields"></a>Champs de données

Les champs de données suivants sont disponibles dans les fichiers de détails sur l’utilisation et les API Cost Management. Lorsqu’elles sont disponibles, des informations équivalentes sur l’Espace partenaires sont affichées. Pour les champs en gras suivants, les partenaires peuvent utiliser les fonctionnalités Filtrer et Regrouper lors de l’analyse des coûts pour analyser les frais en fonction de plusieurs champs. Les champs en gras s’appliquent uniquement aux contrats client Microsoft pris en charge par les partenaires.

| **Nom du champ** | **Description** | **Équivalence sur l’Espace partenaires** |
| --- | --- | --- |
| invoiceId | ID de facture affiché sur la facture associée à la transaction spécifique. | Numéro de facture à l’emplacement où la transaction est affichée. |
| previousInvoiceID | Référence à une facture d’origine s’il y a un remboursement (coût négatif). Ce champ est renseigné uniquement en cas de remboursement. | N/A |
| billingAccountName | Nom du compte de facturation représentant le partenaire. Il accumule tous les coûts associés aux clients qui ont été intégrés à un contrat client Microsoft et les clients CSP qui ont effectué des achats de droits tels que SaaS, la Place de marché Azure et les réservations. | N/A |
| billingAccountID | Identificateur du compte de facturation représentant le partenaire. | ID racine de commerce du partenaire MCAPI. Utilisé dans une requête, mais non inclus dans une réponse.|
| billingProfileID | Identificateur du profil de facturation qui regroupe les coûts de l’ensemble des factures dans une même devise, pour tous les clients qui ont été intégrés dans un contrat client Microsoft et les clients CSP ayant effectué des achats de droits tels que SaaS, la Place de marché Azure et les réservations. | ID de groupe de facturation du partenaire MCAPI. Utilisé dans une requête, mais non inclus dans une réponse. |
| billingProfileName | Nom du profil de facturation qui regroupe les coûts de l’ensemble des factures dans une même devise, pour tous les clients qui ont été intégrés dans un contrat client Microsoft et les clients CSP ayant effectué des achats de droits tels que SaaS, la Place de marché Azure et les réservations. | N/A |
| invoiceSectionName | Nom du projet dont le coût est répertorié dans la facture. Non applicable pour les contrats client Microsoft intégrés par les partenaires. | N/A |
| invoiceSectionID | Identificateur du projet dont le coût est répertorié dans la facture. Non applicable pour les contrats client Microsoft intégrés par les partenaires. | N/A |
| **CustomerTenantID** | Identificateur du locataire Azure Active Directory de l’abonnement du client. | ID d’organisation du client : l’ID de locataire Azure Active Directory du client. |
| **CustomerName** | Nom du locataire Azure Active Directory de l’abonnement du client. | Nom de l’organisation du client comme indiqué dans l’Espace partenaires. Cet élément est important pour le rapprochement de la facture avec vos informations système. |
| **CustomerTenantDomainName** | Nom de domaine du locataire Azure Active Directory de l’abonnement du client. | Domaine du locataire Azure Active Directory du client. |
| **PartnerTenantID** | Identificateur du locataire Azure Active Directory du partenaire. | ID de locataire Azure Active Directory du partenaire appelé ID partenaire, au format GUID. |
| **PartnerName** | Nom du locataire Azure Active Directory du partenaire. | Nom du partenaire. |
| **ResellerMPNID** | Valeur MPNID du revendeur associée à l’abonnement. | ID MPN du revendeur en mémoire pour l’abonnement. Non disponible pour l’activité en cours. |
| costCenter | Centre de coûts associé à l’abonnement. | N/A |
| billingPeriodStartDate | Date de début de la période de facturation, comme indiqué sur la facture. | N/A |
| billingPeriodEndDate | Date de fin de la période de facturation, comme indiqué sur la facture. | N/A |
| servicePeriodStartDate | Date de début de la période d’évaluation pendant laquelle l’utilisation du service a été évaluée, dans le but de déterminer les frais. Les tarifs associés aux services Azure sont déterminés pour la période d’évaluation. | ChargeStartDate dans l’Espace partenaires. Date de début du cycle de facturation, sauf en cas de dates de données d’utilisation latente non facturées précédemment, lors d’un cycle de facturation précédent. L’heure est toujours définie sur le début de la journée, 0:00. |
| servicePeriodEndDate | Date de fin de la période pendant laquelle l’utilisation du service a été évaluée, dans le but de déterminer les frais. Les tarifs associés aux services Azure sont déterminés en fonction de la période d’évaluation. | N/A |
| date | Pour les données de consommation Azure, ce champ affiche la date d’utilisation évaluée. Pour une instance réservée, il affiche la date d’achat. Dans le cas des frais récurrents et ponctuels, tels que la Place de marché et le support, il affiche la date d’achat. | N/A |
| productID | Identificateur du produit qui a accumulé des frais par consommation ou achat. Il s’agit de la clé concaténée de productID et SKuID, comme indiqué dans l’Espace partenaires. | ID du produit. |
| product | Nom du produit qui a accumulé des frais par consommation ou achat, comme indiqué dans la facture. | Nom du produit dans le catalogue. |
| serviceFamily | Affiche la famille de services du produit acheté ou facturé. Par exemple, le stockage ou le calcul. | N/A |
| productOrderID | Identificateur de la ressource ou nom du plan Azure auquel appartient l’abonnement. Par exemple, « Azure Plan ». | N/A |
| productOrderName | Nom du plan Azure auquel appartient l’abonnement. Par exemple, « Azure Plan ». | N/A|
| consumedService | Service consommé (taxonomie héritée) tel qu’il est utilisé dans les détails sur l’utilisation d’EA hérités. | Service affiché dans l’Espace partenaires. Par exemple, Microsoft.Storage, Microsoft.Compute et microsoft.operationalinsights. |
| meterID | Identificateur contrôlé pour la consommation mesurée. | ID du compteur utilisé. |
| meterName | Identifie le nom du compteur associé à la consommation mesurée. | ID du compteur consommé. |
| meterCategory | Identifie le service de niveau supérieur pour l’utilisation. | Service de niveau supérieur pour l’utilisation. |
| meterSubCategory | Définit le type ou la sous-catégorie de service Azure qui peuvent affecter le tarif. | Type de service Azure pouvant affecter le tarif.|
| meterRegion | Identifie l’emplacement du centre de données pour certains services dont le prix est basé sur cet emplacement. | Emplacement régional du centre de données pour les services où cela s’avère possible et le cas échéant. |
| ID d’abonnement | Identificateur unique de l’abonnement Azure généré par Microsoft. | N/A |
| subscriptionName | Nom de l’abonnement Azure. | N/A |
| Terme | Affiche le terme de validité de l’offre. Par exemple, les instances réservées affichent 12 mois d’une période annuelle de l’instance réservée. Pour les achats ponctuels ou récurrents, cette période indique un mois pour SaaS, la Place de marché Azure et le support. Cela ne s’applique pas à la consommation d’Azure. | N/A |
| publisherType (firstParty, thirdPartyReseller, thirdPartyAgency) | Type de serveur de publication qui identifie le serveur de publication en tant que premier tiers, revendeur tiers ou agence tierce. | N/A |
| partNumber | Numéro de référence de l’instance réservée inutilisée et des services de la Place de marché Azure. | N/A |
| publisherName | Nom de l’éditeur du service, y compris Microsoft ou des serveurs de publication tiers. | Nom de l’éditeur du produit.|
| reservationId | Identificateur de l’achat de l’instance réservée. | N/A |
| reservationName | Nom de l’instance réservée. | N/A |
| reservationOrderId | ID de commande de l’instance réservée. | N/A |
| frequency | Fréquence des paiements d’une instance réservée. | N/A |
| resourceGroup | Nom du groupe de ressources Azure utilisé pour la gestion des ressources du cycle de vie. | Nom du groupe de ressources. |
| instanceID (ou) ResourceID | Identificateur de l’instance de ressource. | Affiché sous la forme d’un URI de ressource comprenant des propriétés de ressources complètes. |
| resourceLocation | Nom de l’emplacement de la ressource. | Emplacement de la ressource. |
| Location | Emplacement normalisé de la ressource. | N/A |
| effectivePrice | Prix unitaire effectif du service, dans la devise de tarification. Il est unique pour un produit, une famille de services, un compteur et une offre. Utilisé avec la tarification dans la grille tarifaire associée au compte de facturation. Lorsque la tarification est hiérarchisée ou qu’une quantité est incluse, cette option affiche le prix de consommation combiné. | Prix unitaire une fois les ajustements effectués. |
| Quantité | Quantité mesurée achetée ou consommée. Quantité du compteur utilisée au cours de la période de facturation. | Nombre d’unités. Assurez-vous que cela correspond aux informations de votre système de facturation au cours du rapprochement. |
| unitOfMeasure | Identifie l’unité dans laquelle le service est facturé. Par exemple, des Go et des heures. | Identifie l’unité dans laquelle le service est facturé. Par exemple, Go, heures, 10 000 s. |
| pricingCurrency | Devise définissant le prix unitaire. | Devise de la liste de prix.|
| billingCurrency | Devise définissant le coût facturé. | Devise de la région du client. |
| chargeType | Définit le type de frais représenté par le coût dans Azure Cost Management, comme un achat et un remboursement. | Type de frais ou d’ajustement. Non disponible pour l’activité en cours. |
| costinBillingCurrency | Coût étendu ou combiné avant l’application des taxes, dans la devise facturée. | N/A |
| CostInPricingCurrency | Coût étendu ou combiné avant l’application des taxes, dans la devise de tarification, à corréler avec les prix. | N/A |
| **costinUSD** | Estimation du coût du coût étendu ou combiné avant l’application des taxes, en USD. | N/A |
| **paygCostInBillingCurrency** | Affiche les coûts si la tarification est indiquée dans les prix de vente au détail. Indique les tarifs du paiement à l’utilisation dans la devise de facturation. Disponible uniquement dans les étendues RBAC. | N/A |
| **paygCostInUSD** | Affiche les coûts si la tarification est indiquée dans les prix de vente au détail. Affiche les tarifs du paiement à l’utilisation, en USD. Disponible uniquement dans les étendues RBAC. | N/A |
| exchangeRate | Taux de change utilisé pour convertir un montant de la devise de tarification vers la devise de facturation. | Appelé PCToBCExchangeRate dans l’Espace partenaires. Taux de change pour la conversion de la devise de tarification vers la devise de facturation.|
| exchangeRateDate | Date associée au taux de change utilisé pour convertir un montant de la devise de tarification vers la devise de facturation. | Appelé PCToBCExchangeRateDat dans l’Espace partenaires. Date du taux de change pour la conversion de la devise de tarification vers la devise de facturation.|
| isAzureCreditEligible | Indique si le coût est éligible au paiement en crédits Azure. | N/A |
| serviceInfo1 | Champ hérité capturant les métadonnées facultatives propres au service. | Métadonnées de service Azure interne. |
| serviceInfo2 | Champ hérité capturant les métadonnées facultatives propres au service. | Informations sur le service. Par exemple, le type d’image d’une machine virtuelle et le nom du fournisseur de services Internet pour ExpressRoute.|
| additionalInfo | Métadonnées relatives au service. Par exemple, le type d’image d’une machine virtuelle. | Toutes les informations supplémentaires non couvertes sont dans les autres colonnes. Métadonnées spécifiques au service. Par exemple, le type d’image d’une machine virtuelle.|
| tags | Balise que vous affectez au compteur. Utilisez des balises pour regrouper les enregistrements de facturation. Par exemple, vous pouvez utiliser des balises pour répartir les coûts par département qui utilise le compteur. | Balises ajoutées par le client.|
| **partnerEarnedCreditRate** | Taux de remise appliqué en présence d’un crédit Partenaires basé sur l’accès au lien d’administrateur partenaire. | Taux de remise pour le crédit Partenaires. Par exemple, 0 % ou 15 %. |
| **partnerEarnedCreditApplied** | Indique si le crédit Partenaires a été appliqué. | N/A |

## <a name="view-partner-earned-credit-pec-resource-costs"></a>Voir les coûts des ressources associés au crédit Partenaires

Dans Azure Cost Management, les partenaires peuvent utiliser l’analyse des coûts pour voir les coûts qui ont bénéficié des avantages du crédit Partenaires.

Dans le portail Azure, connectez-vous au locataire partenaire, puis sélectionnez **Gestion des coûts + facturation**. Sous **Gestion des coûts**, cliquez sur **Analyse des coûts**.

La vue d’analyse des coûts présente les coûts du compte de facturation du partenaire. Sous **Étendue**, sélectionnez l’étendue requise pour le partenaire, un client spécifique ou un profil de facturation afin de rapprocher les factures.

Dans un graphique en anneau, cliquez sur la liste déroulante et sélectionnez **PartnerEarnedCreditApplied** pour analyser les coûts relatifs au crédit PEC.

![Exemple illustrant comment voir le crédit Partenaires](./media/get-started-partners/cost-analysis-pec1.png)

Quand la propriété **PartnerEarnedCreditApplied** a la valeur _True_, le coût associé bénéficie de l’accès administrateur pour le crédit Partenaires.

Quand la propriété **PartnerEarnedCreditApplied** a la valeur _False_, le coût associé n’est pas éligible au crédit ou le service acheté n’est pas éligible au crédit Partenaires.

L’affichage des données d’utilisation des services prend généralement entre 8 et 24 heures dans Cost Management. Pour plus d’informations, consultez la section [La fréquence de mise à jour des données d’utilisation varie](understand-cost-mgt-data.md#usage-data-update-frequency-varies). Les crédits PEC s’affichent dans un délai de 48 heures à compter de l’heure d’accès dans Azure Cost Management.


Vous pouvez également grouper et filtrer sur la propriété **PartnerEarnedCreditApplied** à l’aide des options **Grouper par**. Utilisez les options pour examiner les coûts avec et sans crédit PEC.

![Grouper ou filtrer selon le crédit Partenaires](./media/get-started-partners/cost-analysis-pec2.png)

## <a name="cost-management-rest-apis"></a>API REST Cost Management

Les partenaires et les clients peuvent utiliser les API Cost Management décrites dans les sections suivantes pour les tâches courantes.

### <a name="azure-cost-management-apis---direct-and-indirect-providers"></a>API Azure Cost Management : Fournisseurs directs et indirects

Les partenaires ayant accès aux étendues de facturation d’un locataire partenaire peuvent utiliser les API suivantes pour afficher les coûts facturés.

Les API de l’étendue de l’abonnement peuvent être appelées par un partenaire, quelle que soit la stratégie de coût, s’il a accès à l’abonnement. Les autres utilisateurs ayant accès à l’abonnement, comme le client ou le revendeur, peuvent appeler les API uniquement après l’activation de la stratégie de coût par le partenaire pour le locataire client.


#### <a name="to-get-a-list-of-billing-accounts"></a>Pour obtenir une liste des comptes de facturation

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-customers"></a>Pour obtenir une liste des clients

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-subscriptions"></a>Pour obtenir une liste des abonnements

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-invoices-for-a-period-of-time"></a>Pour obtenir la liste des factures pendant une période donnée

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/invoices?api-version=2019-10-01-preview&periodStartDate={periodStartDate}&periodEndDate={periodEndDate}
```

L’appel d’API retourne un tableau de factures qui contient des éléments similaires au code JSON suivant.

```
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/{billingAccountID}/billingProfiles/{BillingProfileID}/invoices/{InvoiceID}",
      "name": "{InvoiceID}",
      "properties": {
        "amountDue": {
          "currency": "USD",
          "value": x.xx
        },
        ...
    }
```

Utilisez la valeur du champ ID renvoyé précédemment et remplacez-la dans l’exemple suivant comme étendue pour rechercher les détails d’utilisation.

```
GET https://management.azure.com/{id}/providers/Microsoft.Consumption/UsageDetails?api-version=2019-10-01
```

L’exemple retourne les enregistrements d’utilisation associés à la facture spécifique.


#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>Pour obtenir la stratégie permettant aux clients de voir les coûts

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>Pour définir la stratégie permettant aux clients de voir les coûts

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-get-azure-service-usage-for-a-billing-account"></a>Pour obtenir l’utilisation des services Azure pour un compte de facturation

```
GET https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-download-a-customers-azure-service-usage"></a>Pour télécharger l’utilisation des services Azure d’un client

L’appel GET suivant est une opération asynchrone.

```
GET https://management.azure.com/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/usageDetails/download?api-version=2019-10-01 -verbose
```

Appelez l’URI `Location` retourné dans la réponse pour vérifier l’état de l’opération. Lorsque l’état est *Terminé*, la propriété `downloadUrl` contient un lien que vous pouvez utiliser pour télécharger le rapport généré.


#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>Pour obtenir ou télécharger la grille tarifaire des services Azure consommés

Tout d’abord, utilisez la publication suivante.

```
POST https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

Ensuite, appelez la valeur de la propriété de l’opération asynchrone. Par exemple :

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheetDownloadOperations/{operation}?sessiontoken=0:11186&api-version=2019-10-01-preview
```
L’appel GET précédent retourne le lien de téléchargement contenant la table de tarification.


#### <a name="to-get-aggregated-costs"></a>Pour recevoir des coûts agrégés

```
POST https://management.azure.com/providers/microsoft.billing/billingAccounts/{billingAccountName}/providers/microsoft.costmanagement/query?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-partner"></a>Créer un budget pour un partenaire

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/providers/Microsoft.CostManagement/budgets/partnerworkshopbudget?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-customer"></a>Créer un budget pour un client

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2019-10-01
```

#### <a name="delete-a-budget"></a>Supprimer un budget

```
PUT
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/budgets/{budgetName}?api-version=2019-10-01
```


## <a name="next-steps"></a>Étapes suivantes
- [Démarrer l’analyse des coûts](quick-acm-cost-analysis.md) dans Cost Management
- [Créer et gérer des budgets](tutorial-acm-create-budgets.md) dans Cost Management
