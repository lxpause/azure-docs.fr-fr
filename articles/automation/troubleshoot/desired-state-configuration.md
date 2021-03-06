---
title: Résoudre les problèmes avec la Configuration de l’état souhaité Azure Automation (DSC)
description: Cet article fournit des informations sur la résolution des problèmes de la Configuration de l’état souhaité
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3d358ac1fb766804b35d969f4d06bc6c07e62661
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951460"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Dépanner la Configuration de l’état souhaité

Cet article fournit des informations sur la résolution des problèmes de la Configuration de l’état souhaité.

## <a name="steps-to-troubleshoot-desired-state-configuration-dsc"></a>Étapes à suivre pour dépanner la Configuration de l’état souhaité

Si vous rencontrez des erreurs en compilant ou en déployant des configurations dans Azure State Configuration, voici quelques étapes pour diagnostiquer le problème.

1. **Veillez à ce que votre configuration compile avec succès sur votre ordinateur local :**  Azure State configuration repose sur PowerShell DSC. Vous trouverez la documentation relative à la syntaxe et au langage DSC dans les [documents PowerShell DSC](https://docs.microsoft.com/powershell/scripting/overview).

   En compilant votre configuration DSC sur votre ordinateur local, vous pouvez détecter et résoudre les erreurs courantes, telles que :

   - **Modules manquants**
   - **Erreurs de syntaxe**
   - **Erreurs logiques**

2. **Afficher les journaux DSC sur votre nœud :** Si votre configuration compile avec succès, mais échoue lorsqu’elle est appliquée à un nœud, vous pouvez trouver les informations détaillées dans les journaux. Pour plus d’informations sur l’emplacement des journaux DSC, consultez [Où sont les journaux d’activité DSC](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

   De plus, le [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) peut vous aider à analyser des informations détaillées dans les journaux DSC. Si vous contactez le support technique, vous aurez besoin de ces journaux pour diagnostiquer votre problème.

   Vous pouvez installer **xDscDiagnostics** sur votre ordinateur local à l’aide des instructions figurant sous [Installer le module de version stable](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

   Pour installer **xDscDiagnostics** sur votre machine Azure, vous pouvez utiliser [az vm run-command](/cli/azure/vm/run-command) ou [Invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand). Vous pouvez aussi utiliser l’option **Run Command** depuis le portail, en suivant les étapes dans [Exécuter des scripts PowerShell dans votre machine virtuelle Windows avec Run Command](../../virtual-machines/windows/run-command.md).

   Pour plus d’informations sur l’utilisation de **xDscDiagnostics**, consultez [Utilisation de xDscDiagnostics pour analyser des journaux DSC](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs), ainsi que les [Cmdlets xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics#cmdlets).
3. **Veillez à ce que vos nœuds et votre espace de travail Automation disposent des modules nécessaires :** La configuration d’état souhaité dépend des modules installés sur le nœud.  Lorsque vous utilisez la configuration de l’état d’Azure Automation, importez les modules requis dans votre compte Automation à l’aide des étapes indiquées dans [Importer des modules](../shared-resources/modules.md#import-modules). Les configurations peuvent également avoir une dépendance sur des versions de modules spécifiques.  Pour plus d’informations, consultez [Résoudre les problèmes liés aux modules](shared-resources.md#modules).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Erreurs courantes avec la Configuration d’état souhaité (DSC)

### <a name="unsupported-characters"></a>Scénario : Une configuration contenant des caractères spéciaux ne peut pas être supprimée à partir du portail

#### <a name="issue"></a>Problème

Lorsque vous tentez de supprimer une configuration DSC à partir du portail, vous rencontrez l’erreur suivante :

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Cause :

Cette erreur est due à un problème temporaire dont la résolution est prévue.

#### <a name="resolution"></a>Résolution :

* Utilisez l’applet de commande Az « Remove-AzAutomationDscConfiguration » pour supprimer la configuration.
* La documentation relative à cette applet de commande n’a pas encore été mise à jour.  En attendant, reportez-vous à la documentation du module AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Scénario : Impossible d’inscrire l’agent DSC

#### <a name="issue"></a>Problème

Lorsque vous tentez d’exécuter `Set-DscLocalConfigurationManager` ou une autre applet de commande DSC, vous recevez l’erreur :

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Cause :

Cette erreur est habituellement due à un pare-feu, la machine se trouvant derrière un serveur proxy, ou à d’autres erreurs de réseau.

#### <a name="resolution"></a>Résolution :

Vérifiez que votre machine a accès aux points de terminaison appropriés pour Azure Automation DSC et réessayez. Pour obtenir la liste des ports et adresses nécessaires, consultez [Planification réseau](../automation-dsc-overview.md#network-planning).

### <a name="a-nameunauthorizedascenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>Scénario : Les rapports d’état retournent le code de réponse « Non autorisé »

#### <a name="issue"></a>Problème

Lors de l’inscription d’un nœud à l’aide de State Configuration (DSC), vous recevez l’un des messages d’erreur suivants :

```error
The attempt to send status report to the server https://{your automation account url}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Cause :

Ce problème est dû à un certificat incorrect ou expiré.  Pour plus d’informations, consultez [Expiration du certificat et réinscription](../automation-dsc-onboarding.md#certificate-expiration-and-re-registration).

### <a name="resolution"></a>Résolution :

Suivez les étapes indiquées ci-dessous pour réinscrire le nœud DSC défaillant.

Tout d’abord, désinscrivez le nœud en procédant comme suit.

1. À partir du Portail Azure, sous **Accueil** -> **Comptes Automation** -> {votre compte Automation} -> **State Configuration (DSC)**
2. Cliquez sur « Nœuds », puis sur le nœud qui rencontre des problèmes.
3. Cliquez sur « Désinscrire » pour désinscrire le nœud.

Désinstallez ensuite l’extension DSC du nœud.

1. À partir de la Portail Azure, sous **Accueil** -> **Machine virtuelle** -> {Noeud défaillant} -> **Extensions**
2. Cliquez sur « Microsoft.Powershell.DSC ».
3. Cliquez sur « Désinstaller » pour désinstaller l’extension DSC PowerShell.

Puis, supprimez du nœud tous les certificats incorrects ou expirés.

Sur le nœud défaillant et à partir d’une invite PowerShell avec élévation de privilèges, exécutez ce qui suit :

```powershell
$certs = @()
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC"}
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC-OaaS Client Authentication"}
$certs += dir cert:\localmachine\CA | ?{$_.subject -like "CN=AzureDSCExtension*"}
"";"== DSC Certificates found: " + $certs.Count
$certs | FL ThumbPrint,FriendlyName,Subject
If (($certs.Count) -gt 0)
{ 
    ForEach ($Cert in $certs) 
    {
        RD -LiteralPath ($Cert.Pspath) 
    }
}
```

Enfin, réinscrivez le nœud défaillant en procédant comme suit.

1. À partir du Portail Azure, sous **Accueil** -> **Comptes Automation** -> {votre compte Automation} -> **State Configuration (DSC)**
2. Cliquez sur « Nœuds ».
3. Cliquez sur le bouton « Ajouter ».
4. Sélectionnez le nœud défaillant.
5. Cliquez sur « Connexion », puis sélectionnez les options souhaitées.

### <a name="failed-not-found"></a>Scénario : Le nœud est en état d’échec avec une erreur « Introuvable »

#### <a name="issue"></a>Problème

Un rapport pour le nœud indique un état **Échec** et contient l’erreur :

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Cause :

Cette erreur se produit généralement quand le nœud est affecté à un nom de configuration (par exemple ABC) au lieu d’un nom de configuration de nœud (par exemple ABC.WebServer).

#### <a name="resolution"></a>Résolution :

* Assurez-vous d’affecter le nœud avec « nom de configuration de nœud » et non pas le « nom de configuration ».
* Vous pouvez affecter une configuration de nœud à un nœud à l'aide du portail Azure ou d’une applet de commande PowerShell.

  * Pour affecter une configuration de nœud à un nœud à l’aide du Portail Azure, ouvrez la page **Nœuds DSC**, sélectionnez un nœud, puis cliquez sur le bouton **Attribuer une configuration de nœud**.
  * Pour affecter une configuration de nœud à un nœud à l’aide d’une applet de commande PowerShell, utilisez l’applet de commande **Set-AzureRmAutomationDscNode**.

### <a name="no-mof-files"></a>Scénario : Aucune configuration de nœud (fichiers MOF) n’a été produite au cours d’une compilation de configuration

#### <a name="issue"></a>Problème

Votre tâche de compilation DSC s’interrompt avec l’erreur :

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Cause :

Quand l’expression qui suit le mot-clé **Node** dans la configuration DSC s’évalue à `$null`, aucune configuration de nœud n’est générée.

#### <a name="resolution"></a>Résolution :

Une des solutions suivantes corrige ce problème :

* Vérifiez que l’expression en regard du mot clé **Node** dans la définition de la configuration n’est pas $null.
* Si vous effectuez une transmission de ConfigurationData pendant la compilation de la configuration, vérifiez que vous transmettez les valeurs attendues nécessaires à la configuration depuis [ConfigurationData](../automation-dsc-compile.md).

### <a name="dsc-in-progress"></a>Scénario : Le rapport du nœud DSC se bloque à l’état « en cours »

#### <a name="issue"></a>Problème

La sortie de l’agent DSC est la suivante :

```error
No instance found with given property values
```

#### <a name="cause"></a>Cause :

Vous avez mis à niveau votre version de WMF et endommagé WMI.

#### <a name="resolution"></a>Résolution :

Pour résoudre ce problème, suivez les instructions fournies dans l’article [Problèmes connus liés à la Configuration d’état souhaité (DSC)](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc).

### <a name="issue-using-credential"></a>Scénario : Il est impossible d’utiliser des informations d’identification dans une configuration DSC

#### <a name="issue"></a>Problème

Votre tâche de compilation DSC a été interrompue avec l’erreur :

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Cause :

Vous avez utilisé des informations d’identification dans une configuration, mais n’avez pas fourni la bonne valeur de **ConfigurationData** pour définir **PSDscAllowPlainTextPassword** sur true pour chaque configuration de nœud.

#### <a name="resolution"></a>Résolution :

* Assurez-vous de transmettre la bonne valeur **ConfigurationData** pour définir **PSDscAllowPlainTextPassword** sur true pour chaque configuration de nœud mentionnée dans la configuration. Pour plus d’informations, consultez les [ressources d’Azure Automation DSC](../automation-dsc-compile.md#working-with-assets-in-azure-automation-during-compilation).

### <a name="failure-processing-extension"></a>Scénario : Intégration à partir de l’extension dsc, erreur « Échec lors du traitement de l’extension »

#### <a name="issue"></a>Problème

Lors de l’intégration à l’aide de l’extension DSC, un échec se produit avec l’erreur :

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Cause :

Cette erreur se produit généralement quand le nœud est affecté à un nom de configuration de nœud qui n’existe pas dans le service.

#### <a name="resolution"></a>Résolution :

* Assurez-vous que vous affectez le nœud avec un nom de configuration de nœud qui correspond exactement au nom dans le service.
* Vous pouvez choisir de ne pas inclure le nom de configuration de nœud, ce qui entraîne l’intégration du nœud,sans affectation de configuration de nœud

### <a name="cross-subscription"></a>Scénario : L’inscription d’un nœud avec PowerShell retourne l’erreur « Une ou plusieurs erreurs se sont produites »

#### <a name="issue"></a>Problème

Lors de l’inscription d’un nœud à l’aide de `Register-AzAutomationDSCNode` ou de `Register-AzureRMAutomationDSCNode`, vous recevez l’erreur suivante.

```error
One or more errors occurred.
```

#### <a name="cause"></a>Cause :

Cette erreur se produit lorsque vous tentez d’inscrire un nœud qui réside dans un abonnement distinct du compte Automation.

#### <a name="resolution"></a>Résolution :

Traitez ce nœud d’un autre abonnement comme s’il se trouvait dans un cloud distinct ou localement.

Suivez les étapes ci-dessous pour inscrire le nœud.

* Windows : [Machines physiques/virtuelles Windows locales ou dans un cloud autre qu’Azure/AWS](../automation-dsc-onboarding.md#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws).
* Linux : [Machines physiques/virtuelles Linux locales ou dans un cloud autre qu’Azure](../automation-dsc-onboarding.md#physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure).

### <a name="agent-has-a-problem"></a>Scénario : Message d’erreur : « Échec de l’approvisionnement »

#### <a name="issue"></a>Problème

Lors de l’inscription d’un nœud, l’erreur suivante s’affiche :

```error
Provisioning has failed
```

#### <a name="cause"></a>Cause :

Ce message s’affiche lorsqu’il y a un problème de connectivité entre le nœud et Azure.

#### <a name="resolution"></a>Résolution :

Déterminez si votre nœud se trouve dans un réseau virtuel privé ou s’il a d’autres problèmes pour se connecter à Azure.

Pour plus d’informations, consultez [Résolution des erreurs d’intégration des solutions](onboarding.md).

### <a name="failure-linux-temp-noexec"></a>Scénario : Pendant l’application d’une configuration dans Linux, un échec se produit avec une erreur générale

#### <a name="issue"></a>Problème

Pendant l’application d’une configuration dans Linux, un échec se produit avec l’erreur :

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Cause :

Les clients ont déterminé que si l’emplacement `/tmp` est défini sur `noexec`, la version actuelle de DSC ne parvient pas à appliquer les configurations.

#### <a name="resolution"></a>Résolution :

* Supprimez l’option `noexec` de l’emplacement `/tmp`.

### <a name="compilation-node-name-overlap"></a>Scénario : Les noms des configurations de nœuds qui se chevauchent peuvent entraîner une version incorrecte

#### <a name="issue"></a>Problème

Si un script de configuration unique est utilisé pour générer plusieurs configurations de nœuds et que certaines des configurations de nœuds ont un nom qui est un sous-ensemble d’autres, un problème dans le service de compilation peut entraîner l’attribution d’une configuration incorrecte.  Cela se produit uniquement lors de l’utilisation d’un seul script pour générer des configurations avec des données de configuration par nœud, et uniquement quand le chevauchement de nom se produit au début de la chaîne.

Par exemple, si un script de configuration unique est utilisé pour générer des configurations en fonction de données de nœuds transmises sous forme de table de hachage à l’aide d’applets de commande, et que les données de nœuds incluent un serveur nommé « serveur » et « 1serveur ».

#### <a name="cause"></a>Cause :

Problème connu avec le service de compilation.

#### <a name="resolution"></a>Résolution :

La meilleure solution de contournement consiste à compiler localement ou dans un pipeline CI/CD, et à charger les fichiers MOF directement dans le service.  Si la compilation dans le service est une exigence, la meilleure solution suivante consiste à fractionner les travaux de compilation afin qu’il n’y ait pas de chevauchement des noms.

## <a name="next-steps"></a>Étapes suivantes

Si votre problème ne figure pas dans cet article ou si vous ne parvenez pas à le résoudre, utilisez un des canaux suivants pour obtenir de l’aide :

* Obtenez des réponses de la part d’experts Azure via les [Forums Windows](https://azure.microsoft.com/support/forums/)
* Connectez-vous avec [@AzureSupport](https://twitter.com/azuresupport), qui est le compte Microsoft Azure officiel pour améliorer l’expérience client en connectant la communauté Azure aux ressources appropriées : réponses, support technique et experts.
* Si vous avez besoin de plus d’aide, vous pouvez signaler un incident au support Azure. Accédez au [site du support Azure](https://azure.microsoft.com/support/options/) , puis cliquez sur **Obtenir un support**.
