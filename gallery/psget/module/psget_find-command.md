---
ms.date: 2017-06-12
contributor: manikb
ms.topic: reference
keywords: gallery,powershell,cmdlet,psget
title: Find-Command
ms.openlocfilehash: f867f12b1c6efad30a04581c6f36c5a77a2fb2ae
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
<a id="find-command" class="xliff"></a>
# Find-Command

Recherche des commandes PowerShell dans des modules.

<a id="description" class="xliff"></a>
## Description
L’applet de commande Find-Command recherche des commandes PowerShell, telles que les applets de commande, alias, fonctions et workflows. Find-Command recherche des modules dans les référentiels enregistrés.
Pour chaque commande qu’elle détecte, cette applet de commande retourne un objet PSGetCommandInfo. Vous pouvez passer un objet PSGetCommandInfo à l’applet de commande Install-Module pour installer le module qui contient la commande.

- Find-Command permet de filtrer avec des paramètres de version : MinimumVersion, RequiredVersion, AllVersions.
  - Ces paramètres sont mutuellement exclusifs.
  - Ces paramètres de version sont autorisés uniquement avec le nom de module unique sans les caractères génériques.
  - Si le paramètre RequiredVersion n’est pas spécifié, Find-Command retourne la dernière version du module qui est supérieure ou égale à la version minimale spécifiée ou la dernière version du module si aucune version minimale n’est spécifiée.
  - Si le paramètre RequiredVersion est spécifié, Find-Command retourne uniquement la version du module qui correspond exactement à la version spécifiée.
- Find-Command peut filtrer les métadonnées de modules avec le paramètre -Tag
- Find-Command peut filtrer le langage de recherche propre au référentiel avec le paramètre -Filter.
- Find-Command peut filtrer les modules à partir de l’ensemble ou de certains des référentiels enregistrés.

<a id="cmdlet-syntax" class="xliff"></a>
## Syntaxe de l’applet de commande
```powershell
Get-Command -Name Find-Command -Module PowerShellGet -Syntax
```

<a id="cmdlet-online-help-reference" class="xliff"></a>
## Référence de l’aide en ligne de l’applet de commande

[Find-Command](http://go.microsoft.com/fwlink/?LinkId=733636)

<a id="example-commands" class="xliff"></a>
## Exemples de commandes
```powershell

# Find a specific command
Find-Command -Name Get-ScriptAnalyzerRule

Name                                Version    ModuleName                          Repository
----                                -------    ----------                          ----------
Get-ScriptAnalyzerRule              1.5.0      PSScriptAnalyzer                    PSGallery

# Find multiple commands
Find-Command -Name Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer

# Find all available commands from all registered repositories
Find-Command

# Find available commands from few registered repositories
Find-Command -Repository PSGallery,PrivatePSGallery

# Find all commands in a specified repository
Find-Command -Repository PSGallery

# Find a command defined in a specific module
Find-Command -Name Get-ScriptAnalyzerRule -Module PSScriptAnalyzer

# Find commands from all versions of a module
Find-Command -ModuleName PSReadline -AllVersions

# Find commands with module name and MinimumVersion.
Find-Command -ModuleName PSReadline -MinimumVersion 1.1

# Find commands with module name and exact version
Find-Command -ModuleName AzureRM -RequiredVersion 1.4.0

# Find commands defined modules with -Filter based search. -Filter searches in description and module names
Find-Command -Filter Cookbook
Find-Command -Filter RBAC

# Find all commands with tags Azure or DSC in module metadata
Find-Command -Tag Azure, DSC

```

