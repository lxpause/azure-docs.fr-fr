---
title: Connecter des données F5 à Azure Sentinel | Microsoft Docs
description: Découvrez comment connecter des données F5 à Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2019
ms.author: rkarlin
ms.openlocfilehash: 9e7ceee07cfc81953709e3077a6af14388e40e99
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73475822"
---
# <a name="connect-f5-to-azure-sentinel"></a>Connecter F5 à Azure Sentinel

Cet article explique comment connecter votre appliance F5 à Azure Sentinel. Le connecteur de données F5 vous permet de connecter facilement vos journaux F5 à Azure Sentinel, de consulter des tableaux de bord, de créer des alertes personnalisées et d'améliorer les enquêtes. L'utilisation de F5 sur Azure Sentinel vous permettra d'obtenir davantage d'informations sur l'utilisation d'Internet au sein de votre organisation et améliorera ses fonctionnalités de sécurité. 


## <a name="how-it-works"></a>Fonctionnement

Vous devez déployer un agent sur une machine Linux dédiée (machine virtuelle ou ordinateur local) afin de prendre en charge les communications entre F5 et Azure Sentinel. Le diagramme suivant décrit la configuration dans le cas d'une machine virtuelle Linux dans Azure.

 ![CEF dans Azure](./media/connect-cef/cef-syslog-azure.png)

Cette configuration conviendra également si vous utilisez une machine virtuelle dans un autre cloud ou sur un ordinateur local. 

 ![CEF local](./media/connect-cef/cef-syslog-onprem.png)


## <a name="security-considerations"></a>Considérations relatives à la sécurité

Veillez à configurer la sécurité de la machine en fonction de la stratégie de sécurité de votre organisation. Par exemple, vous pouvez configurer votre réseau de sorte qu’il s’accorde à la stratégie de sécurité de votre réseau d’entreprise, et modifier les ports et les protocoles dans le démon pour les adapter à vos besoins. Vous pouvez utiliser les instructions suivantes pour améliorer la configuration de la sécurité de votre machine :  [Sécurisation de la machine virtuelle dans Azure](../virtual-machines/linux/security-policy.md), [Bonnes pratiques pour la sécurité réseau](../security/fundamentals/network-best-practices.md).

Pour utiliser la communication TLS entre la solution de sécurité et la machine Syslog, vous devez configurer le démon Syslog (rsyslog or syslog-ng) pour communiquer avec le TLS : [Chiffrement du trafic Syslog avec TLS –rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [Chiffrement des messages du journal avec TLS – syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).

 
## <a name="prerequisites"></a>Prérequis
Assurez-vous que la machine Linux que vous utilisez en tant que proxy exécute l'un des systèmes d'exploitation suivants :

- 64 bits
  - CentOS 6 et 7
  - Amazon Linux 2017.09
  - Oracle Linux 6 et 7
  - Red Hat Enterprise Linux Server 6 et 7
  - Debian GNU/Linux 8 et 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS et 18.04 LTS
  - SUSE Linux Enterprise Server 12
- 32 bits
   - CentOS 6
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 6
   - Debian GNU/Linux 8 et 9
   - Ubuntu Linux 14.04 LTS et 16.04 LTS
 
 - Versions du démon
   - Syslog-ng : 2.1 - 3.22.1
   - Rsyslog : v8
  
 - RFC Syslog pris en charge
   - RFC Syslog 3164
   - RFC Syslog 5424
 
Assurez-vous que votre machine répond également aux exigences suivantes : 
- Autorisations
    - Vous devez disposer d'autorisations élevées (sudo) sur votre machine. 
- Configuration logicielle requise
    - Vérifiez que Python est en cours d'exécution sur votre machine
## <a name="step-1-deploy-the-agent"></a>ÉTAPE 1 : Déployer l’agent

Au cours de cette étape, vous devez sélectionner la machine Linux qui fera office de proxy entre Azure Sentinel et votre solution de sécurité. Vous devez exécuter un script sur la machine proxy qui :
- Installe l'agent Log Analytics et le configure en fonction des besoins pour écouter les messages Syslog sur le port 514 via TCP et envoyer les messages CEF à votre espace de travail Azure Sentinel.
- Configure le démon Syslog pour transférer les messages CEF à l'agent Log Analytics via le port 25226.
- Configure l'agent Syslog pour collecter les données et les envoyer en toute sécurité à Log Analytics, où elles sont analysées et enrichies.
 
 
1. Dans le portail Azure Sentinel, cliquez sur **Data Connectors** (Connecteurs de données) et sélectionnez **F5** (F5), puis **Open Connector Page** (Ouvrir la page du connecteur). 

1. Sous **Installer et configurer l'agent Syslog**, sélectionnez le type de votre machine (Azure, autre cloud ou locale). 
   > [!NOTE]
   > Étant donné que le script de l'étape suivante installe l'agent Log Analytics et connecte la machine à votre espace de travail Azure Sentinel, vérifiez que cette machine n'est pas connectée à un autre espace de travail.
1. Vous devez disposer d'autorisations élevées (sudo) sur votre machine. Vérifiez que vous disposez de Python sur votre machine à l'aide de la commande suivante : `python –version`

1. Exécutez le script suivant sur votre machine proxy.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Pendant l'exécution du script, vérifiez que vous ne recevez pas de messages d'erreur ou d'avertissement.


## <a name="step-2-configure-your-f5-to-send-cef-messages"></a>ÉTAPE 2 : Configurer votre appliance F5 pour envoyer des messages CEF

1. Accédez à [Configuration de la journalisation des événements de sécurité des applications](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-11-5-0/12.html) et suivez les instructions pour configurer la journalisation à distance, en suivant les recommandations ci-dessous :
   - Définissez le **Type de stockage distant** sur **CEF**.
   - Définissez le **Protocole** sur **TCP**.
   - Définissez l'**adresse IP** sur l'adresse IP du serveur Syslog.
   - Définissez le **numéro de port** sur **514** ou le port que vous avez défini pour l'agent.
   - Vous pouvez définir la **Taille maximale de chaîne de requête** sur la taille que vous avez définie dans votre agent.

1. Pour utiliser le schéma pertinent dans Log Analytics pour les événements CEF, recherchez `CommonSecurityLog`.

## <a name="step-3-validate-connectivity"></a>ÉTAPE 3 : Valider la connectivité

1. Ouvrez Log Analytics pour vous assurer que les journaux sont reçus à l'aide du schéma CommonSecurityLog.<br> Plus de 20 minutes peuvent être nécessaires avant que vos journaux ne commencent à apparaître dans Log Analytics. 

1. Avant d'exécuter le script, nous vous recommandons d'envoyer des messages à partir de votre solution de sécurité pour vous assurer qu'ils sont transmis à la machine proxy Syslog que vous avez configurée. 
1. Vous devez disposer d'autorisations élevées (sudo) sur votre machine. Vérifiez que vous disposez de Python sur votre machine à l'aide de la commande suivante : `python –version`
1. Exécutez le script suivant pour vérifier la connectivité entre l'agent, Azure Sentinel et votre solution de sécurité. Celui-ci vérifie que le transfert de démon est correctement configuré, écoute les ports appropriés et s'assure que rien ne bloque la communication entre le démon et l'agent Log Analytics. Le script envoie également des messages fictifs « TestCommonEventFormat » pour vérifier la connectivité de bout en bout. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`


## <a name="next-steps"></a>Étapes suivantes
Dans ce document, vous avez appris à connecter F5 à Azure Sentinel. Pour en savoir plus sur Azure Sentinel, voir les articles suivants :
- Découvrez comment [avoir une visibilité sur vos données et les menaces potentielles](quickstart-get-visibility.md).
- Prise en main de la [détection des menaces avec Azure Sentinel](tutorial-detect-threats.md).
