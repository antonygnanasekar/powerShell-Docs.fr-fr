---
ms.date: 2017-06-12
author: eslesar
ms.topic: conceptual
keywords: dsc,powershell,configuration,setup
title: "Spécification de dépendances entre nœuds"
ms.openlocfilehash: dcdf9f8ef4b74d23bd083767db2cc4aafc0ee83b
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
<a id="specifying-cross-node-dependencies" class="xliff"></a>
# Spécification de dépendances entre nœuds

> S’applique à : Windows PowerShell 5.0

DSC fournit des ressources spéciales, **WaitForAll**, **WaitForAny** et **WaitForSome**, qui peuvent être utilisées dans les configurations pour spécifier les dépendances sur les configurations sur d’autres nœuds. Le comportement de ces ressources est le suivant :

* **WaitForAll** : réussit si la ressource spécifiée est dans l’état souhaité sur tous les nœuds cibles définis dans la propriété **NodeName**.
* **WaitForAny** : réussit si la ressource spécifiée est dans l’état souhaité sur au moins l’un des nœuds cibles définis dans la propriété **NodeName**.
* **WaitForSome** : spécifie une propriété **NodeCount** en plus d’une propriété **NodeName**. La ressource réussit si elle est dans l’état souhaité sur un nombre minimal de nœuds (spécifié par **NodeCount**) défini par la propriété **NodeName**. 

<a id="using-waitforxxxx-resources" class="xliff"></a>
## Utilisation de ressources WaitForXXXX

Pour utiliser les ressources **WaitForXXXX**, vous créez un bloc de ressources du type de ressource qui spécifie la ressource DSC et les nœuds à attendre. Vous utilisez ensuite la propriété **DependsOn** dans tous les autres blocs de ressources dans votre configuration pour attendre que les conditions spécifiées dans le nœud **WaitForXXXX** réussissent.

Par exemple, dans la configuration suivante, le nœud cible attend que la ressource **xADDomain** se termine sur le nœud **MyDC** avec un nombre maximal de 30 tentatives, à des intervalles de 15 secondes, avant que le nœud cible ne puisse joindre le domaine.

```powershell
Configuration JoinDomain

{
    Import-DscResource -Module xComputerManagement, xActiveDirectory

    Node myPC
    {
        WindowsFeature InstallAD
        {
            Ensure = 'Present' 
            Name = 'AD-Domain-Services' 
        }

        xADDomain NewDomain 
        { 
            DomainName = 'Contoso.com'            
            DomainAdministratorCredential = (Get-Credential)
            SafemodeAdministratorPassword = (Get-Credential)
            DatabasePath = "C:\Windows\NTDS"
            LogPath = "C:\Windows\NTDS"
            SysvolPath = "C:\Windows\Sysvol"
        }

    }

    Node myDomainJoinedServer
    {

        WaitForAll DC
        {
            ResourceName      = '[xADDomain]NewDomain'
            NodeName          = 'MyDC'
            RetryIntervalSec  = 15
            RetryCount        = 30
        }

        xComputer JoinDomain
        {
            Name             = 'myPC'
            DomainName       = 'Contoso.com'
            Credential       = (Get-Credential)
            DependsOn        ='[WaitForAll]DC'
        }
    }
}
```

>**Remarque** : Par défaut les ressources WaitForXXX effectuent une tentative, puis échouent. Même si ce n’est pas obligatoire, vous indiquez généralement un intervalle avant nouvelle tentative et un nombre.

<a id="see-also" class="xliff"></a>
## Voir aussi
* [Configurations DSC](configurations.md)
* [Ressources DSC](resources.md)
* [Configuration du Gestionnaire de configuration local](metaConfig.md)

