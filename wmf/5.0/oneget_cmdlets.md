---
ms.date: 2017-06-12
author: JKeithB
ms.topic: reference
keywords: wmf,powershell,configuration
ms.openlocfilehash: 6cba004890fc4b1dfac40920f751f61b0530cce9
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
<a id="packagemanagement-cmdlets" class="xliff"></a>
# Applets de commande PackageManagement
Il s’agit du noyau de PackageManagement pour prendre en charge la découverte, l’installation et l’inventaire des logiciels. Essayez les applets de commande pour effectuer ces opérations :
-   Find-Package
-   Find-PackageProvider
-   Get-Package
-   Get-PackageProvider
-   Get-PackageSource
-   Import-PackageProvider
-   Install-Package
-   Install-PackageProvider
-   Register-PackageSource
-   Save-Package
-   Set-PackageSource
-   Uninstall-Package
-   Unregister-PackageSource

PackageManagement étant un module PowerShell, vous pouvez effectuer les opérations suivantes pour mettre à jour PackageManagement lui-même :
```powershell
PS C:\> Install-Module PackageManagement –Force
```
Dans ce cas, vous devrez réaccéder à la session PowerShell pour basculer vers la nouvelle version de PackageManagement.

<a id="find-package-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890709aspx" class="xliff"></a>
## [Applet de commande Find-Package](https://technet.microsoft.com/en-us/library/dn890709.aspx)
Cette applet de commande permet de détecter des packages logiciels dans les sources de packages disponibles à l’aide des fournisseurs de packages chargés.
```powershell
# Find all available Windows PowerShell module packages from galleries registered
# with PowerShellGet provider
Find-Package -Provider PowerShellGet -Source PSGallery

# Find a package from a provider that is not yet installed
# This will bootstrap NuGet provider and then search for jquery package using NuGet
# with <http://www.nuget.org/api/v2/> as source
Find-Package -Name jquery –Provider NuGet -Source http://www.nuget.org/api/v2/

# Find package with name and version
# Here we are assuming that the user already registered nuget.org using
# Register-PackageSource. You can specify either the provider or the source, or
# neither. For the latter, performance may be less optimal as it searches through all
# the providers and registered sources.
Find-Package -Name jquery –Provider NuGet –RequiredVersion 2.1.4 -Source nuget.org
```

<a id="find-packageprovider-cmdlethttpstechnetmicrosoftcomen-uslibrarymt676544aspx" class="xliff"></a>
## [Applet de commande Find-PackageProvider](https://technet.microsoft.com/en-us/library/mt676544.aspx)
L’applet de commande Find-PackageProvider recherche les fournisseurs PackageManagement correspondants qui sont disponibles dans les sources de packages inscrites auprès de PowerShellGet. Il s’agit de fournisseurs de packages disponibles pour l’installation avec l’applet de commande Install-PackageProvider. Par défaut, cela comprend les modules disponibles dans PowerShell Gallery avec les balises « PackageManagement » et « Provider ». 

Find-PackageProvider recherche également les fournisseurs PackageManagement correspondants qui sont disponibles dans le magasin d’objets blob Azure PackageManagement où nous utilisons le fournisseur de programme d’amorçage PackageManagement pour les rechercher et les installer.
```powershell
#Find all available package providers in PackageManagement azure blob store as well as in PowerShellGallery.com
Find-PackageProvider

#Find all versions of a provider
Find-PackageProvider -Name "Nuget" -AllVersions

#Find a provider from a specified source
Find-PackageProvider -Name "Gistprovider" -Source "PSGallery"
```

<a id="get-package-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890704aspx" class="xliff"></a>
## [Applet de commande Get-Package](https://technet.microsoft.com/en-us/library/dn890704.aspx)
Cette applet de commande retourne une liste de tous les packages logiciels qui ont été installés à l’aide de PackageManagement.
```powershell
# Get all the packages installed by Programs provider
Get-Package –Provider Programs

# Get all the packages installed by NuGet provider at c:\test using the dynamic
# parameter destination
Get-Package –Provider NuGet -Destination c:\test
```

<a id="get-packageprovider-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890703aspx" class="xliff"></a>
## [Applet de commande Get-PackageProvider](https://technet.microsoft.com/en-us/library/dn890703.aspx)
Vous pouvez inventorier les fournisseurs de packages qui sont chargés et prêts à être utilisés sur l’ordinateur local à l’aide de cette applet de commande.
```powershell
# Get all currently loaded package providers
Get-PackageProvider

# The following cmdlet will show all the package providers available on the machine (including those that are not loaded):
Get-PackageProvider -ListAvailable
```

<a id="get-packagesource-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890705aspx" class="xliff"></a>
## [Applet de commande Get-PackageSource](https://technet.microsoft.com/en-us/library/dn890705.aspx)
Cette applet de commande obtient une liste des sources de packages qui sont inscrites pour un fournisseur de package.
```powershelll
# Get all package sources
Get-PackageSource

# Get all package sources for a specific provider
Get-PackageSource –ProviderName PowerShellGet
```

<a id="import-packageprovider-cmdlethttpstechnetmicrosoftcomen-uslibrarymt676545aspx" class="xliff"></a>
## [Applet de commande Import-PackageProvider](https://technet.microsoft.com/en-us/library/mt676545.aspx)
Cette applet de commande ajoute des packages Package Management à la session active.
```powershell
# Import a package provider from the local machine
Import-PackageProvider –Name MyProvider

#The -Name parameter can be either the name of the provider or the full path to the provider. Currently, we support .dll, .exe and.psm1 for the full path case. If the name of the provider is used for the -Name parameter, then additional version parameters such as -RequiredVersion, -MinimumVersion and -MaximumVersion may be specified. Otherwise, the latest version of the provider will be imported.

#If a package provider is not yet loaded to your system, we can discover and install on-demand. You can use explicit discovery and install cmdlets to do so:
 Find-PackageProvider
 Install-PackageProvider –Name MyProvider

#After installed, follow the Import-PackageProvider to load it to your system.

# Import a specific version of a package provider. PackageManagement supports installations of multiple versions of a package provider using PackageProvider cmdlets (not by bootstrapper provider). You can install another version of a package provider given that you already have one up running by:
Find-PackageProvider –Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force
Get-PackageProvider –ListAvailable
Import-PackageProvider –Name "Nuget" -RequiredVersion "2.8.5.201" -Verbose
Import-PackageProvider –Name MyProvider –RequiredVersion xxxx -force
```

<a id="-install-package-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890711aspx" class="xliff"></a>
##[ Applet de commande Install-Package](https://technet.microsoft.com/en-us/library/dn890711.aspx)

Cette applet de commande permet d’installer des packages logiciels dans les sources de packages disponibles à l’aide des fournisseurs de packages chargés.
```powershell
# Install a package by name.
# NuGet provider requires us to provide the dynamic parameter destination path
# when we use this provider to install. Not all providers will require you to supply
# dynamic parameters for PackageManagement cmdlets.
Install-Package -Name jquery -Source nuget.org -Destination c:\test

# Install a package by piping.
Find-Package -Name jquery –Provider NuGet | Install-Package -Destination c:\test
```

<a id="install-packageprovider-cmdlethttpstechnetmicrosoftcomen-uslibrarymt676543aspx" class="xliff"></a>
## [Applet de commande Install-PackageProvider](https://technet.microsoft.com/en-us/library/mt676543.aspx)
Cette applet de commande installe un ou plusieurs fournisseurs de packages PackageManagement.
```powershell
# Install a package provider from the PowerShell Gallery
Install-PackageProvider –Name "Gistprovider" -Verbose

# Install a specified version of a package provider
Find-PackageProvider –Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force

# Find a provider and install it
Find-PackageProvider –Name "Gistprovider" | Install-PackageProvider -Verbose

# Install a provider to the current user’s module folder
Install-PackageProvider –Name Gistprovider –Verbose –Scope CurrentUser
```

<a id="register-packagesource-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890701aspx" class="xliff"></a>
## [Applet de commande Register-PackageSource](https://technet.microsoft.com/en-us/library/dn890701.aspx)
Cette applet de commande ajoute une source de package pour un fournisseur de package spécifié.
Chaque fournisseur PackageManagement peut avoir une ou plusieurs sources de logiciels, ou dépôts. PackageManagement fournit des applets de commande PowerShell pour ajouter/supprimer/interroger la source. Par exemple, vous pouvez inscrire une source de package pour le fournisseur NuGet :
```powershell
Register-PackageSource -Name "NugetSource" -Location "http://www.nuget.org/api/v2" –ProviderName nuget
```

<a id="save-package-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890708aspx" class="xliff"></a>
## [Applet de commande Save-Package](https://technet.microsoft.com/en-us/library/dn890708.aspx)
Cette applet de commande enregistre des packages sur l’ordinateur local sans les installer.
```powershell
# Saves jquery package to c:\test using NuGetProvider
# Notes that the -Path parameter must point to an existing location
Save-Package -Name jquery –Provider NuGet -Path c:\test

# Save a package by piping.
Find-Package -Name jquery -Source http://www.nuget.org/api/v2/ | Save-Package -Path c:\test
Find-Package -source c:\test
```

<a id="set-packagesource-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890710aspx" class="xliff"></a>
## [Applet de commande Set-PackageSource](https://technet.microsoft.com/en-us/library/dn890710.aspx)
Cette applet de commande modifie les informations relatives à une source de package existante. 
```powershell
#Set-PackageSource changes the values for a source that has already been registered by running the Register-PackageSource cmdlet. By #running Set-PackageSource, you can change the source name and location.
Set-PackageSource  -Name nuget.org -Location  http://www.nuget.org/api/v2 -NewName nuget2 -NewLocation https://www.nuget.org/api/v2 
```

<a id="uninstall-package-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890702aspx" class="xliff"></a>
## [Applet de commande Uninstall-Package](https://technet.microsoft.com/en-us/library/dn890702.aspx)
Cette applet de commande désinstalle des packages installés sur l’ordinateur local.
```powershell
# Uninstall jquery using nuget
Uninstall-Package -Name jquery –Provider NuGet -Destination c:\test

# Uninstall a package with by piping with Get-Package
Get-Package -Name jquery –Provider NuGet -Destination c:\test | Uninstall-Package
```

<a id="unregister-packagesource-cmdlethttpstechnetmicrosoftcomen-uslibrarydn890707aspx" class="xliff"></a>
## [Applet de commande Unregister-PackageSource](https://technet.microsoft.com/en-us/library/dn890707.aspx)
```powershell
# Unregister a package source for the NuGet provider. You can use command Unregister-PackageSource, to disconnect with a repository, and Get-PackageSource, to discover what the repositories are associated with that provider.
Unregister-PackageSource  -Name "NugetSource"
```

