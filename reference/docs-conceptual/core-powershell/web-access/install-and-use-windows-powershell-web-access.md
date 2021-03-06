---
ms.date: 2017-06-05
keywords: powershell,applet de commande
title: "installer et utiliser Accès Web Windows PowerShell"
ms.openlocfilehash: a860f7c22829da46f0458ea729fa0afd1fe4fb6f
ms.sourcegitcommit: 598b7835046577841aea2211d613bb8513271a8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2017
---
<a id="install-and-use-windows-powershell-web-access" class="xliff"></a>
#  Installer et utiliser Accès Web Windows PowerShell

Mise à jour : 5 novembre 2013

S’applique à : Windows Server 2012 R2, Windows Server 2012

Accès Web Windows PowerShell®, fonctionnalité introduite initialement dans Windows Server® 2012, joue le rôle de passerelle Windows PowerShell, en fournissant une console web Windows PowerShell ciblée vers un ordinateur distant. Elle permet aux professionnels de l’informatique d’exécuter des commandes et des scripts Windows PowerShell à partir d’une console Windows PowerShell dans un navigateur web, sans nécessiter Windows PowerShell, ni de logiciel de gestion à distance ou d’installation d’un plug-in de navigateur sur le périphérique client. Tout ce qui est requis pour exécuter la console web Windows PowerShell est une passerelle Accès Web Windows PowerShell correctement configurée et un navigateur de périphérique client qui prend en charge JavaScript® et accepte les cookies.

Un périphérique client est par exemple un ordinateur portable, un ordinateur personnel non professionnel, une tablette, une borne Web publique, un ordinateur sans système d’exploitation Windows ou encore un navigateur de téléphone portable. Les informaticiens peuvent effectuer des tâches de gestion stratégiques sur des serveurs Windows distants à partir de périphériques ayant accès à une connexion Internet et à un navigateur Web.

Après avoir installé et configuré correctement la passerelle, les utilisateurs peuvent accéder à une console Windows PowerShell à l’aide d’un navigateur web. Quand les utilisateurs ouvrent le site web Accès Web Windows PowerShell sécurisé, ils peuvent exécuter une console web Windows PowerShell après une authentification réussie.

L’installation et la configuration d’Accès Web Windows PowerShell forment un processus en trois étapes :

1.  Installation d’Accès Web Windows PowerShell

2.  Configuration de la passerelle

3.  Configuration des règles d’autorisation qui permettent aux utilisateurs d’accéder à la console web Windows PowerShell

Avant d’installer et de configurer Accès Web Windows PowerShell, nous vous recommandons de lire ce guide dans son intégralité, qui inclut des instructions sur la façon d’installer, de sécuriser et de désinstaller cette fonctionnalité. La rubrique [Utiliser la console web Windows PowerShell](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx) décrit comment les utilisateurs se connectent à la console web, et couvre les limitations et les différences entre la console Windows PowerShell basée sur le web et la console **powershell.exe**. Les utilisateurs finaux de la console web doivent lire la rubrique [Utiliser la console web Windows PowerShell](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx), mais il n’est pas nécessaire pour eux de lire le reste de ce guide.

La présente rubrique ne fournit pas d’instructions approfondies sur les opérations de serveur web (IIS), mais décrit uniquement les étapes requises pour configurer la passerelle Accès Web Windows PowerShell. Pour plus d’informations sur la configuration et la sécurisation des sites web dans IIS, voir les ressources de documentation IIS dans la section Voir aussi.

Le diagramme suivant illustre le fonctionnement d’Accès Web Windows PowerShell.

<span><img src="https://i-technet.sec.s-msft.com/dynimg/IC564303.jpeg" title="Windows PowerShell Web Access diagram" alt="Windows PowerShell Web Access diagram" id="ee15fa8f-ce13-49e5-933d-514f6d60a2b1" /></span>

Dans cette rubrique :

-   [Configuration requise pour exécuter Accès Web Windows PowerShell](#BKMK_reqs)

-   [Prise en charge de navigateurs et de périphériques clients](#BKMK_browser)

-   [Déploiement (rapide) recommandé](#BKMK_recm)

-   [Déploiement personnalisé](#BKMK_custom)

-   [Configurer un certificat authentique](#BKMK_configcert)

<a href="" id="BKMK_reqs"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Configuration requise pour exécuter Accès Web Windows PowerShell</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_0" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Accès Web Windows PowerShell nécessite l’exécution, sur le serveur sur lequel la passerelle doit s’exécuter, d’un serveur web (IIS), de .NET Framework 4.5 et de Windows PowerShell 3.0 ou Windows PowerShell 4.0. Vous pouvez installer Accès Web Windows PowerShell sur un serveur qui exécute Windows Server 2012 R2 ou Windows Server 2012 en utilisant l’Assistant Ajout de rôles et de fonctionnalités dans le Gestionnaire de serveur ou des applets de commande de déploiement Windows PowerShell pour le Gestionnaire de serveur. Quand vous installez Accès Web Windows PowerShell à l’aide du Gestionnaire de serveur ou de ses applets de commande de déploiement, les rôles et fonctionnalités requis sont automatiquement ajoutés dans le cadre du processus d’installation.

Accès Web Windows PowerShell permet aux utilisateurs distants d’accéder aux ordinateurs de votre organisation en utilisant Windows PowerShell dans un navigateur web. Bien qu’Accès Web Windows PowerShell soit un outil de gestion pratique et puissant, l’accès web présente des risques de sécurité et doit être configuré de manière aussi sécurisée que possible. Nous recommandons aux administrateurs qui configurent la passerelle Accès Web Windows PowerShell d’utiliser les couches de sécurité disponibles, à la fois les règles d’autorisation basées sur des applets de commande incluses avec Accès Web Windows PowerShell et les couches de sécurité disponibles dans le serveur web (IIS) et les applications tierces. Cette documentation contient à la fois des exemples non sécurisés destinés uniquement aux environnements de test et des exemples recommandés pour des déploiements sécurisés.

<a href="" id="BKMK_browser"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Prise en charge de navigateurs et de périphériques clients</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Accès Web Windows PowerShell prend en charge les navigateurs suivants. Bien que les navigateurs mobiles ne soient pas officiellement pris en charge, nombre d’entre eux sont susceptibles de pouvoir exécuter la console web Windows PowerShell. D’autres navigateurs acceptant les cookies, exécutant JavaScript et exécutant des sites Web HTTPS sont censés fonctionner, mais n’ont pas été testés officiellement.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Navigateurs d’ordinateur de bureau pris en charge</span></a>

------------------------------------------------------------------------

-   Windows® Internet Explorer® pour Microsoft Windows® 8.0, 9.0, 10.0 et 11.0

-   Mozilla Firefox® 10.0.2

-   Google Chrome™ 17.0.963.56m pour Windows

-   Apple Safari® 5.1.2 pour Windows

-   Apple Safari 5.1.2 pour Mac OS®

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Navigateurs ou appareils mobiles testés de façon minimale</span></a>

------------------------------------------------------------------------

-   Windows Phone 7 et 7.5

-   Google Android WebKit 3.1 Browser Android 2.2.1 (Kernel 2.6)

-   Apple Safari pour système d’exploitation iPhone 5.0.1

-   Apple Safari pour système d’exploitation iPad 2 5.0.1

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Configuration requise des navigateurs</span></a>

------------------------------------------------------------------------

Pour utiliser la console web Accès Web Windows PowerShell, les navigateurs doivent :

-   autoriser les cookies à partir du site web de la passerelle Accès Web Windows PowerShell ;

-   être en mesure d’ouvrir et de lire des pages HTTPS ;

-   ouvrir et exécuter des sites Web qui utilisent JavaScript.

<a href="" id="BKMK_recm"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Déploiement (rapide) recommandé</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Vous pouvez installer la passerelle Accès Web Windows PowerShell sur un serveur qui exécute Windows Server 2012 R2 ou Windows Server 2012 en utilisant des applets de commande Windows PowerShell, ou l’Assistant Ajout de rôles et de fonctionnalités qui s’ouvre à partir du Gestionnaire de serveur. Pour une installation et une configuration rapides, utilisez les applets de commande Windows PowerShell, comme décrit dans cette section.

-   [Étape 1 : installer Accès Web Windows PowerShell](#BKMK_step1)

-   [Étape 2 : configurer la passerelle](#BKMK_step2)

-   [Étape 3 : configurer une règle d’autorisation restrictive](#BKMK_step3)

<a href="" id="BKMK_step1"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Étape 1 : installer Accès Web Windows PowerShell</span></a>

------------------------------------------------------------------------

<a id="to-install-windows-powershell-web-access-by-using-windows-powershell-cmdlets" class="xliff"></a>
#### Pour installer Accès Web Windows PowerShell à l’aide des applets de commande Windows PowerShell

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Dans l’écran d’**accueil** de Windows, cliquez avec le bouton droit sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Dans Windows PowerShell 3.0 et 4.0, il n’est pas nécessaire d’importer le module d’applet de commande du Gestionnaire de serveur dans la session Windows PowerShell avant d’exécuter des applets de commande qui font partie du module. Un module est automatiquement importé la première fois que vous exécutez une applet de commande qui fait partie du module. En outre, les applets de commande Windows PowerShell ne respectent pas la casse.</p></td>
    </tr>
    </tbody>
    </table>

2.  Tapez ce qui suit, puis appuyez sur **Entrée**, où *computer_name* représente le nom de l’ordinateur distant sur lequel vous souhaitez installer Accès Web Windows PowerShell, le cas échéant. Le paramètre <span class="code">Restart</span> redémarre automatiquement les serveurs de destination, si besoin.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_374a9c21-4f6e-471e-b957-bb190a594533'); "Copier dans le Presse-papiers.")

        Install-WindowsFeature -Name WindowsPowerShellWebAccess -ComputerName <computer_name> -IncludeManagementTools -Restart

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Installer Accès Web Windows PowerShell à l’aide d’applets de commande Windows PowerShell n’ajoute pas les outils de gestion de serveur web (IIS) par défaut. Si vous voulez installer les outils de gestion sur le même serveur que la passerelle Accès Web Windows PowerShell, ajoutez le paramètre <span class="code">IncludeManagementTools</span> à la commande d’installation (comme indiqué dans cette étape). Si vous gérez le site web Accès Web Windows PowerShell à partir d’un ordinateur distant, installez le composant logiciel enfichable Gestionnaire des services Internet en installant les <a href="http://go.microsoft.com/fwlink/?LinkID=304145">outils d’administration de serveur distant pour Windows 8.1</a> ou les <a href="http://go.microsoft.com/fwlink/p/?LinkID=238560">outils d’administration pour serveur distant pour Windows 8</a> sur l’ordinateur à partir duquel vous voulez gérer la passerelle.</p></td>
    </tr>
    </tbody>
    </table>

    Pour installer des rôles et fonctionnalités sur un disque dur virtuel hors connexion, vous devez ajouter les paramètres <span class="code">ComputerName</span> et <span class="code">VHD</span>. Le paramètre <span class="code">ComputerName</span> contient le nom du serveur sur lequel monter le disque dur virtuel, tandis que le paramètre <span class="code">VHD</span> contient le chemin du fichier VHD sur le serveur spécifié.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_d841d509-347e-49d0-bf54-8d1f306bece6'); "Copier dans le Presse-papiers.")

        Install-WindowsFeature -Name WindowsPowerShellWebAccess -VHD <path> -ComputerName <computer_name> -IncludeManagementTools -Restart

3.  Quand l’installation est terminée, vérifiez qu’Accès Web Windows PowerShell a été installé sur les serveurs de destination en exécutant l’applet de commande **Get-WindowsFeature** sur un serveur de destination, dans une console Windows PowerShell ouverte avec des droits d’utilisateur élevés. Vous pouvez également vérifier qu’Accès Web Windows PowerShell a été installé dans la console Gestionnaire de serveur en sélectionnant un serveur de destination dans la page **Tous les serveurs**, puis en affichant la vignette **Rôles et fonctionnalités** du serveur sélectionné. Vous pouvez aussi afficher le fichier Lisez-moi associé à Accès Web Windows PowerShell.

4.  Une fois cette fonctionnalité installée, vous êtes invité à consulter le fichier Lisez-moi dans lequel se trouvent des instructions d’installation requises simples pour la passerelle. Ces instructions d’installation sont également fournies dans la section suivante, [Étape 2 : Configurer la passerelle](#BKMK_step2). Le chemin du fichier Lisez-moi est <span class="computerOutputInline">C:\\Windows\\Web\\PowerShellWebAccess\\wwwroot\\README.txt</span>.

<a href="" id="BKMK_step2"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Étape 2 : configurer la passerelle</span></a>

------------------------------------------------------------------------

L’applet de commande **Install-PswaWebApplication** constitue un moyen rapide de configurer l’Accès Web Windows PowerShell. Bien que vous puissiez ajouter le paramètre <span class="code">UseTestCertificate</span> à l’applet de commande <span class="code">Install-PswaWebApplication</span> pour installer un certificat SSL auto-signé à des fins de test, cette pratique n’est pas sécurisée ; pour un environnement de production sécurisé, utilisez toujours un certificat SSL valide qui a été signé par une autorité de certification. Les administrateurs peuvent remplacer le certificat de test par le certificat signé de leur choix à l’aide de la console du Gestionnaire des services Internet.

Vous pouvez terminer la configuration de l’application web d’Accès Web Windows PowerShell en exécutant l’applet de commande <span class="code">Install-PswaWebApplication</span> ou les étapes de configuration basées sur l’interface graphique utilisateur dans le Gestionnaire des services Internet. Par défaut, l’applet de commande installe l’application web, **pswa** (et son pool d’applications, **pswa_pool**), dans le conteneur **Site Web par défaut**, comme indiqué dans le Gestionnaire des services Internet ; si vous le voulez, vous pouvez obliger l’applet de commande à modifier le conteneur de site par défaut de l’application web. Le Gestionnaire des services Internet propose des options de configuration pour les applications Web, telles que la modification du numéro de port ou du certificat SSL (Secure Sockets Layer).

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC17938.jpeg" title="System_CAPS_security" alt="System_CAPS_security" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-security" /></span><span class="alertTitle"> Remarque sur la sécurité </span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nous conseillons vivement aux administrateurs de configurer la passerelle de façon à utiliser un certificat valide signé par une autorité de certification.</p></td>
</tr>
</tbody>
</table>

-   [Pour configurer la passerelle Accès Web Windows PowerShell avec un certificat de test à l’aide d’Install-PswaWebApplication](#BKMK_testcert)

-   [Pour configurer la passerelle Windows PowerShell Web Access avec un certificat authentique à l’aide de Install-PswaWebApplication et IIS Manager](#BKMK_gencert)

<a id="to-configure-the-windows-powershell-web-access-gateway-with-a-test-certificate-by-using-install-pswawebapplication" class="xliff"></a>
#### Pour configurer la passerelle Accès Web Windows PowerShell avec un certificat de test à l’aide de Install-PswaWebApplication

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell.

    -   Sur le Bureau Windows, cliquez avec le bouton droit sur **Windows PowerShell** dans la barre des tâches.

    -   Dans l’écran d’**accueil** de Windows, cliquez sur **Windows PowerShell**.

2.  Tapez ce qui suit, puis appuyez sur **Entrée**.

    **Install-PswaWebApplication -UseTestCertificate**

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC17938.jpeg" title="System_CAPS_security" alt="System_CAPS_security" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-security" /></span><span class="alertTitle"> Remarque sur la sécurité </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Le paramètre <span class="code">UseTestCertificate</span> doit uniquement être utilisé dans un environnement de test privé. Pour un environnement de production sécurisé, nous vous recommandons d’utiliser un certificat valide signé par une autorité de certification.</p></td>
    </tr>
    </tbody>
    </table>

    L’exécution de l’applet de commande permet d’installer l’application web Accès Web Windows PowerShell au sein du conteneur de site web IIS par défaut. L’applet de commande crée l’infrastructure requise pour exécuter Accès Web Windows PowerShell sur le site web par défaut, https://&lt;server_name&gt;/pswa. Pour installer l’application web dans un autre site web, indiquez son nom en ajoutant le paramètre <span class="code">WebSiteName</span>. Pour modifier le nom de l’application web (par défaut, il s’agit de <span class="code">pswa</span>), ajoutez le paramètre <span class="code">WebApplicationName</span>.

    Les paramètres suivants peuvent être configurés en exécutant l’applet de commande. Vous pouvez les modifier manuellement dans la console du Gestionnaire des services Internet, si vous le souhaitez.

    -   Path: /pswa

    -   ApplicationPool : pswa_pool

    -   EnabledProtocols: http

    -   PhysicalPath: %*windir*%/Web/PowerShellWebAccess/wwwroot

    <span class="label">Exemple :</span> <span class="code">Install-PswaWebApplication -webApplicationName myWebApp -useTestCertificate</span>

    Dans cet exemple, le site web obtenu pour Accès Web Windows PowerShell est https://&lt; *server_name*&gt;/myWebApp.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Vous ne pouvez pas vous connecter tant que les utilisateurs ne se voient pas accorder l’accès au site Web en ajoutant des règles d’autorisation. Pour plus d’informations, consultez <a href="#BKMK_step3">Étape 3 : Configurer une règle d’autorisation restrictive</a> et <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Règles d’autorisation et fonctionnalités de sécurité d’Accès Web Windows PowerShell</a>.</p></td>
    </tr>
    </tbody>
    </table>

<a id="to-configure-the-windows-powershell-web-access-gateway-with-a-genuine-certificate-by-using-install-pswawebapplication-and-iis-manager" class="xliff"></a>
#### Pour configurer la passerelle Windows PowerShell Web Access avec un certificat authentique à l’aide de Install-PswaWebApplication et IIS Manager

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell.

    -   Sur le Bureau Windows, cliquez avec le bouton droit sur **Windows PowerShell** dans la barre des tâches.

    -   Dans l’écran d’**accueil** de Windows, cliquez sur **Windows PowerShell**.

2.  Tapez ce qui suit, puis appuyez sur **Entrée**.

    **Install-PswaWebApplication**

    Les paramètres de passerelle suivants peuvent être configurés en exécutant l’applet de commande. Vous pouvez les modifier manuellement dans la console du Gestionnaire des services Internet, si vous le souhaitez. Vous pouvez également spécifier des valeurs pour les paramètres <span class="code">WebsiteName</span> et <span class="code">WebApplicationName</span> de l’applet de commande <span class="code">Install-PswaWebApplication</span>.

    -   Path: /pswa

    -   ApplicationPool : pswa_pool

    -   EnabledProtocols: http

    -   PhysicalPath: %*windir*%/Web/PowerShellWebAccess/wwwroot

3.  Ouvrez la console du Gestionnaire des services Internet en procédant de l’une des manières suivantes.

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows. Dans le menu **Outils** du Gestionnaire de serveur, cliquez sur **Gestionnaire des services Internet (IIS)**.

    -   Dans l’écran d’**accueil** de Windows, cliquez sur **Gestionnaire de serveur**.

4.  Dans le volet de l’arborescence du Gestionnaire des services Internet, développez le nœud du serveur sur lequel Accès Web Windows PowerShell est installé jusqu’à ce que le dossier **Sites** apparaisse. Développez le dossier **Sites**.

5.  Sélectionnez le site web dans lequel vous avez installé l’application web d’Accès Web Windows PowerShell. Dans le volet **Actions**, cliquez sur **Liaisons**.

6.  Dans la boîte de dialogue **Liaisons de site**, cliquez sur **Ajouter**.

7.  Dans la boîte de dialogue **Ajouter la liaison de site**, dans le champ **Type**, sélectionnez **https**.

8.  Dans le champ **Certificat SSL**, sélectionnez votre certificat signé dans le menu déroulant. Cliquez sur **OK**. Pour plus d’informations sur l’obtention d’un certificat, consultez [Pour configurer un certificat SSL dans le Gestionnaire des services Internet](#BKMK_cert) dans cette rubrique.

    L’application web d’Accès Web Windows PowerShell est à présent configurée pour utiliser votre certificat SSL signé. Vous pouvez accéder à Accès Web Windows PowerShell en ouvrant https://&lt;server_name&gt;/pswa dans une fenêtre de navigateur.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Vous ne pouvez pas vous connecter tant que les utilisateurs ne se voient pas accorder l’accès au site Web en ajoutant des règles d’autorisation. Pour plus d’informations, consultez <a href="#BKMK_step3">Étape 3 : Configurer une règle d’autorisation restrictive</a> et <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Règles d’autorisation et fonctionnalités de sécurité d’Accès Web Windows PowerShell</a>.</p></td>
    </tr>
    </tbody>
    </table>

<a href="" id="BKMK_step3"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Étape 3 : configurer une règle d’autorisation restrictive</span></a>

------------------------------------------------------------------------

Une fois Accès Web Windows PowerShell installé et la passerelle configurée, les utilisateurs peuvent ouvrir la page de connexion dans un navigateur, mais ils ne peuvent pas se connecter tant que l’administrateur d’Accès Web Windows PowerShell ne leur a pas octroyé explicitement un accès. Le contrôle d’accès d’Accès Web Windows PowerShell est géré à l’aide de l’ensemble d’applets de commande Windows PowerShell décrit dans le tableau suivant. Il n’existe aucune interface utilisateur graphique équivalente pour ajouter ou gérer les règles d’autorisation. Pour plus d’informations sur les applets de commande Accès Web Windows PowerShell, consultez les rubriques de référence des applets de commande, [Applets de commande Accès Web Windows PowerShell](https://technet.microsoft.com/library/hh918342.aspx).

Pour plus de détails sur les règles d’autorisation et la sécurité d’Accès Web Windows PowerShell, consultez [Règles d’autorisation et fonctionnalités de sécurité d’Accès Web Windows PowerShell](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx).

<a id="to-add-a-restrictive-authorization-rule" class="xliff"></a>
#### Pour ajouter une règle d’autorisation restrictive

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Dans l’écran d’**accueil** de Windows, cliquez avec le bouton droit sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

2.  <span class="label">Étape facultative pour restreindre l’accès utilisateur à l’aide de configurations de sessions :</span> vérifiez que les configurations de sessions que vous voulez utiliser dans vos règles existent déjà. Si elles n’ont pas encore été créées, utilisez les instructions relatives à la création de configurations de sessions dans [about_Session_Configuration_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx) sur MSDN.

3.  Tapez ce qui suit, puis appuyez sur **Entrée**.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_f9e7959b-75d0-4d63-8f8e-02334a8dd09d'); "Copier dans le Presse-papiers.")

        Add-PswaAuthorizationRule -UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Cette règle d’autorisation accorde à un utilisateur spécifique l’accès à un ordinateur sur le réseau auquel il a généralement accès, avec l’accès à une configuration de session spécifique ayant comme portée les besoins ordinaires de l’utilisateur en matière de script et d’applet de commande. Dans l’exemple suivant, un utilisateur nommé <span class="code">JSmith</span> dans le domaine <span class="code">Contoso</span> se voit accorder un accès pour gérer l’ordinateur <span class="code">Contoso_214</span> et utiliser une configuration de session nommée <span class="code">NewAdminsOnly</span>.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_ebd5bc5e-ec5d-4955-a86a-63843e480e37'); "Copier dans le Presse-papiers.")

        Add-PswaAuthorizationRule -UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Vérifiez que la règle a été créée en exécutant l’applet de commande **Get-PswaAuthorizationRule** ou **Test-PswaAuthorizationRule -UserName &lt;domain\\user | computer\\user&gt; -ComputerName** &lt;nom_ordinateur&gt;. Par exemple, **Test-PswaAuthorizationRule -UserName Contoso\\JSmith -ComputerName Contoso_214**.

Après avoir configuré une règle d’autorisation, les utilisateurs autorisés peuvent se connecter à la console web et commencer à utiliser Accès Web Windows PowerShell.

<a href="" id="BKMK_custom"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Déploiement personnalisé</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Vous pouvez installer la passerelle Accès Web Windows PowerShell sur un serveur qui exécute Windows Server 2012 R2 ou Windows Server 2012 en utilisant l’Assistant Ajout de rôles et de fonctionnalités dans le Gestionnaire de serveur. Après avoir installé Accès Web Windows PowerShell, vous pouvez personnaliser la configuration de la passerelle dans le Gestionnaire des services Internet.

<a href="" id="BKMK_custom1"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Étape 1 : installer Accès Web Windows PowerShell</span></a>

------------------------------------------------------------------------

<a id="to-install-windows-powershell-web-access-by-using-the-add-roles-and-features-wizard" class="xliff"></a>
#### Pour installer Accès Web Windows PowerShell à l’aide de l’Assistant Ajout de rôles et de fonctionnalités

1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.

    -   Dans l’écran d’**accueil** de Windows, cliquez sur **Gestionnaire de serveur**.

2.  Dans le menu **Gérer**, cliquez sur **Ajouter des rôles et fonctionnalités**.

3.  Dans la page **Sélectionner le type d’installation**, sélectionnez **Installation basée sur un rôle ou une fonctionnalité**. Cliquez sur **Suivant**.

4.  Dans la page **Sélectionner le serveur de destination**, sélectionnez un serveur dans le pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Pour sélectionner un disque dur virtuel hors connexion en guise de serveur de destination, choisissez d’abord le serveur sur lequel monter le disque dur virtuel, puis sélectionnez le fichier VHD. Pour plus d’informations sur l’ajout de serveurs à votre pool de serveurs, consultez l’Aide du Gestionnaire de serveur. Une fois que vous avez sélectionné le serveur de destination, cliquez sur **Suivant**.

5.  Dans la page **Sélectionner des fonctionnalités** de l’Assistant, développez **Windows PowerShell**, puis sélectionnez **Accès Web Windows PowerShell**.

6.  Notez que vous êtes invité à ajouter les fonctionnalités requises, telles que le .NET Framework 4.5 et les services de rôle du serveur Web (IIS). Ajoutez les fonctionnalités requises et continuez.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Installer Accès Web Windows PowerShell à l’aide de l’Assistant Ajout de rôles et de fonctionnalités installe également le serveur web (IIS), y compris le composant logiciel enfichable Gestionnaire des services Internet. Le composant logiciel enfichable et d’autres outils de gestion des services Internet IIS sont installés par défaut si vous utilisez l’Assistant Ajout de rôles et de fonctionnalités. Si vous installez Accès Web Windows PowerShell à l’aide d’applets de commande Windows PowerShell comme décrit dans la procédure suivante, les outils de gestion ne sont pas ajoutés par défaut.</p></td>
    </tr>
    </tbody>
    </table>

7.  Dans la page **Confirmer les sélections d’installation**, si les fichiers de fonctionnalités d’Accès Web Windows PowerShell ne sont pas enregistrés sur le serveur de destination que vous avez sélectionné à l’étape 4, cliquez sur **Spécifier un autre chemin d’accès source**, puis indiquez le chemin des fichiers de fonctionnalités. Sinon, cliquez sur **Installer**.

8.  Une fois que vous avez cliqué sur **Installer**, la page **Progression de l’installation** affiche la progression de l’installation, les résultats et des messages tels que des avertissements, échecs ou étapes de configuration de post-installation requises pour Accès Web Windows PowerShell. Une fois cette fonctionnalité installée, vous êtes invité à consulter le fichier Lisez-moi dans lequel se trouvent des instructions d’installation requises simples pour la passerelle. Ces instructions sont également incluses dans la présente rubrique. Le chemin du fichier Lisez-moi est <span class="computerOutputInline">C:\\Windows\\Web\\PowerShellWebAccess\\wwwroot\\README.txt</span>.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Étape 2 : configurer la passerelle</span></a>

------------------------------------------------------------------------

Les instructions données dans cette section concernent l’installation de l’application web d’Accès Web Windows PowerShell dans un sous-répertoire, et non dans le répertoire racine, de votre site web. Cette procédure est l’équivalent basé sur l’interface graphique utilisateur des actions effectuées par l’applet de commande <span class="code">Install-PswaWebApplication</span>. Cette section inclut également des instructions sur la manière d’utiliser le Gestionnaire des services Internet pour configurer la passerelle Accès Web Windows PowerShell en tant que site web racine.

-   [Pour configurer la passerelle dans un site Web existant à l’aide du Gestionnaire des services Internet](#BKMK_configman)

-   [Pour configurer la passerelle comme site web racine avec un certificat de test à l’aide du Gestionnaire des services Internet](#BKMK_configroot)

-   

<a id="to-use-iis-manager-to-configure-the-gateway-in-an-existing-website" class="xliff"></a>
#### Pour configurer la passerelle dans un site Web existant à l’aide du Gestionnaire des services Internet

1.  Ouvrez la console du Gestionnaire des services Internet en procédant de l’une des manières suivantes.

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows. Dans le menu **Outils** du Gestionnaire de serveur, cliquez sur **Gestionnaire des services Internet (IIS)**.

    -   Sur l’écran d’**accueil** de Windows, tapez une partie du nom **Gestionnaire des services Internet (IIS)**. Cliquez sur le raccourci quand il s’affiche dans les résultats **Applications**.

2.  Créez un pool d’applications pour Accès Web Windows PowerShell. Développez le nœud du serveur de passerelle dans l’arborescence du Gestionnaire des services Internet, sélectionnez **Pools d’applications**, puis cliquez sur **Ajouter un pool d’applications** dans le volet **Actions**.

3.  Ajoutez un nouveau pool d’applications portant le nom **pswa_pool** ou indiquez un autre nom. Cliquez sur **OK**.

4.  Dans le volet de l’arborescence du Gestionnaire des services Internet, développez le nœud du serveur sur lequel Accès Web Windows PowerShell est installé jusqu’à ce que le dossier **Sites** apparaisse. Sélectionnez le dossier **Sites**.

5.  Cliquez avec le bouton droit sur le site web (par exemple, **Site web par défaut**) auquel vous voulez ajouter le site web Accès Web Windows PowerShell, puis cliquez sur **Ajouter une application**.

6.  Dans le champ **Alias**, tapez pswa ou indiquez un autre alias. L’alias devient le nom du répertoire virtuel. Par exemple, **pswa** dans l’URL suivante représente l’alias spécifié durant cette étape : https://&lt;server_name&gt;/pswa.

7.  Dans le champ **Pool d’applications**, sélectionnez le pool d’applications que vous avez créé à l’étape 3.

8.  Dans le champ **Chemin d’accès physique**, naviguez jusqu’à l’emplacement de l’application. Vous pouvez utiliser l’emplacement par défaut, %windir%/Web/PowerShellWebAccess/wwwroot. Cliquez sur **OK**.

9.  Suivez les étapes de la procédure [Pour configurer un certificat SSL dans le Gestionnaire des services Internet](#BKMK_cert) dans cette rubrique.

10. <span class="label">Étape de sécurité facultative :</span> Avec le site web sélectionné dans l’arborescence, double-cliquez sur **Paramètres SSL** dans le volet de contenu. Sélectionnez **Exiger SSL**, puis dans le volet **Actions**, cliquez sur **Appliquer**. Dans le volet **Paramètres SSL**, vous pouvez éventuellement exiger que les utilisateurs qui se connectent au site web d’Accès Web Windows PowerShell possèdent des certificats clients. Ceux-ci aident à vérifier l’identité d’un utilisateur de périphérique client. Pour plus d’informations sur la manière dont l’exigence de certificats clients permet de renforcer la sécurité d’Accès Web Windows PowerShell, consultez [Règles d’autorisation et fonctionnalités de sécurité d’Accès Web Windows PowerShell](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx) dans ce guide.

11. Ouvrez une session de navigateur sur un périphérique client. Pour plus d’informations sur les navigateurs et périphériques pris en charge, consultez [Prise en charge de navigateurs et de périphériques client](#BKMK_browser) dans cette rubrique.

12. Ouvrez le nouveau site web Accès Web Windows PowerShell, https://&lt; *gateway_server_name*&gt;/pswa.

    Le navigateur doit afficher la page de connexion de la console d’Accès Web Windows PowerShell.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Vous ne pouvez pas vous connecter tant que les utilisateurs ne se voient pas accorder l’accès au site Web en ajoutant des règles d’autorisation.</p></td>
    </tr>
    </tbody>
    </table>

13. Dans une session Windows PowerShell qui a été ouverte avec des droits d’utilisateur élevés (Exécuter en tant qu’administrateur), exécutez le script suivant dans lequel *application_pool_name* représente le nom du pool d’applications que vous avez créé à l’étape 3, afin de donner les droits d’accès au pool d’applications dans le fichier d’autorisations.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_c1a80a93-8fcf-4beb-a025-5f81bfb8bdae'); "Copier dans le Presse-papiers.")

        $applicationPoolName = "<application_pool_name>"
        $authorizationFile = "C:\windows\web\powershellwebaccess\data\AuthorizationRules.xml"
        c:\windows\system32\icacls.exe $authorizationFile /grant ('"' + "IIS AppPool\$applicationPoolName" + '":R') > $null

    Pour afficher les droits d’accès existants sur le fichier d’autorisations, exécutez la commande suivante :

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_ac2179c2-9548-4a17-8663-267fdd105380'); "Copier dans le Presse-papiers.")

        c:\windows\system32\icacls.exe $authorizationFile

<a id="to-use-iis-manager-to-configure-the-gateway-as-a-root-website-with-a-test-certificate" class="xliff"></a>
#### Pour configurer la passerelle comme site web racine avec un certificat de test à l’aide du Gestionnaire des services Internet

1.  Ouvrez la console du Gestionnaire des services Internet en procédant de l’une des manières suivantes.

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows. Dans le menu **Outils** du Gestionnaire de serveur, cliquez sur **Gestionnaire des services Internet (IIS)**.

    -   Sur l’écran d’**accueil** de Windows, tapez une partie du nom **Gestionnaire des services Internet (IIS)**. Cliquez sur le raccourci quand il s’affiche dans les résultats **Applications**.

2.  Dans le volet de l’arborescence du Gestionnaire des services Internet, développez le nœud du serveur sur lequel Accès Web Windows PowerShell est installé jusqu’à ce que le dossier **Sites** apparaisse. Sélectionnez le dossier **Sites**.

3.  Dans le volet **Actions**, cliquez sur **Ajouter un site Web**.

4.  Tapez le nom du site web, comme **Accès Web Windows PowerShell**.

5.  Un pool d’applications est créé automatiquement pour le nouveau site Web. Pour utiliser un autre pool d’applications, cliquez sur **Sélectionner** pour sélectionner un pool d’applications à associer au nouveau site web. Sélectionnez l’autre pool d’applications dans la boîte de dialogue **Sélectionner un pool d’applications**, puis cliquez sur **OK**.

6.  Dans la zone de texte **Chemin d’accès physique**, accédez à %*windir*%/Web/PowerShellWebAccess/wwwroot.

7.  Dans le champ **Type** de la zone **Liaison**, sélectionnez **https**.

8.  Assignez au site Web un numéro de port qui n’est pas déjà utilisé par un autre site ou une autre application. Pour localiser les ports ouverts, vous pouvez exécuter la commande **netstat** dans une fenêtre d’invite de commandes. Le numéro de port par défaut est 443.

    Modifiez le port par défaut si un autre site Web utilise déjà le port 443 ou si vous avez d’autres raisons d’ordre sécuritaire. Si un autre site web qui s’exécute sur votre serveur de passerelle utilise votre port sélectionné, un avertissement s’affiche quand vous cliquez sur **OK** dans la boîte de dialogue **Ajouter un site Web**. Vous devez utiliser un port inutilisé pour exécuter Accès Web Windows PowerShell.

9.  Selon les besoins de votre organisation, spécifiez éventuellement un nom d’hôte qui a du sens pour votre organisation et ses utilisateurs, comme **www.contoso.com**. Cliquez sur **OK**.

10. Pour un environnement de production plus sécurisé, nous vous recommandons vivement de fournir un certificat valide signé par une autorité de certification. Vous devez fournir un certificat SSL, car les utilisateurs peuvent uniquement se connecter à Accès Web Windows PowerShell par le biais d’un site web HTTPS. Pour plus d’informations sur l’obtention d’un certificat, consultez [Pour configurer un certificat SSL dans le Gestionnaire des services Internet](#BKMK_cert) dans cette rubrique.

11. Cliquez sur **OK** pour fermer la boîte de dialogue **Ajouter un site Web**.

12. Dans une session Windows PowerShell qui a été ouverte avec des droits d’utilisateur élevés (Exécuter en tant qu’administrateur), exécutez le script suivant dans lequel *application_pool_name* représente le nom du pool d’applications que vous avez créé à l’étape 4, afin de donner les droits d’accès au pool d’applications dans le fichier d’autorisations.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_35ae9944-ca44-4af7-9c96-616083b3e3db'); "Copier dans le Presse-papiers.")

        $applicationPoolName = "<application_pool_name>"
        $authorizationFile = "C:\windows\web\powershellwebaccess\data\AuthorizationRules.xml"
        c:\windows\system32\icacls.exe $authorizationFile /grant ('"' + "IIS AppPool\$applicationPoolName" + '":R') > $null

    Pour afficher les droits d’accès existants sur le fichier d’autorisations, exécutez la commande suivante :

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_0eb6d70a-2807-498b-b088-fa5233ed68d5'); "Copier dans le Presse-papiers.")

        c:\windows\system32\icacls.exe $authorizationFile

13. Avec le nouveau site web sélectionné dans le volet de l’arborescence du Gestionnaire des services Internet, cliquez sur **Démarrer** dans le volet **Actions** pour démarrer le site web.

14. Ouvrez une session de navigateur sur un périphérique client. Pour plus d’informations sur les navigateurs et périphériques pris en charge, voir [Prise en charge de navigateurs et de périphériques client](#BKMK_browser) dans ce document.

15. Ouvrez le site web Accès Web Windows PowerShell.

    Étant donné que le site web racine pointe vers le dossier Accès Web Windows PowerShell, le navigateur doit afficher la page de connexion Accès Web Windows PowerShell quand vous ouvrez https://&lt;*gateway_server_name*&gt;. Il n’est pas nécessaire d’ajouter **/pswa** à l’URL.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Remarque </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Vous ne pouvez pas vous connecter tant que les utilisateurs ne se voient pas accorder l’accès au site Web en ajoutant des règles d’autorisation.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Étape 3 : configurer une règle d’autorisation restrictive</span></a>

------------------------------------------------------------------------

Une fois Accès Web Windows PowerShell installé et la passerelle configurée, les utilisateurs peuvent ouvrir la page de connexion dans un navigateur, mais ils ne peuvent pas se connecter tant que l’administrateur d’Accès Web Windows PowerShell ne leur a pas octroyé explicitement un accès. Le contrôle d’accès d’Accès Web Windows PowerShell est géré à l’aide de l’ensemble d’applets de commande Windows PowerShell décrit dans le tableau suivant. Il n’existe aucune interface utilisateur graphique équivalente pour ajouter ou gérer les règles d’autorisation. Pour plus d’informations sur les applets de commande Accès Web Windows PowerShell, consultez les rubriques de référence des applets de commande, [Applets de commande Accès Web Windows PowerShell](https://technet.microsoft.com/library/hh918342.aspx).

Pour plus de détails sur les règles d’autorisation et la sécurité d’Accès Web Windows PowerShell, consultez [Règles d’autorisation et fonctionnalités de sécurité d’Accès Web Windows PowerShell](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx).

<a id="to-add-a-restrictive-authorization-rule" class="xliff"></a>
#### Pour ajouter une règle d’autorisation restrictive

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Dans l’écran d’**accueil** de Windows, cliquez avec le bouton droit sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

2.  <span class="label">Étape facultative pour restreindre l’accès utilisateur à l’aide de configurations de sessions :</span> vérifiez que les configurations de sessions que vous voulez utiliser dans vos règles existent déjà. Si elles n’ont pas encore été créées, utilisez les instructions relatives à la création de configurations de sessions dans [about_Session_Configuration_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx) sur MSDN.

3.  Tapez ce qui suit, puis appuyez sur **Entrée**.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_4df22c91-f56f-4bb5-91e7-99f9b365ed5d'); "Copier dans le Presse-papiers.")

        Add-PswaAuthorizationRule -UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Cette règle d’autorisation accorde à un utilisateur spécifique l’accès à un ordinateur sur le réseau auquel il a généralement accès, avec l’accès à une configuration de session spécifique ayant comme portée les besoins ordinaires de l’utilisateur en matière de script et d’applet de commande. Dans l’exemple suivant, un utilisateur nommé <span class="code">JSmith</span> dans le domaine <span class="code">Contoso</span> se voit accorder un accès pour gérer l’ordinateur <span class="code">Contoso_214</span> et utiliser une configuration de session nommée <span class="code">NewAdminsOnly</span>.

    [Copier](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_efc3999a-2905-453f-86cd-014b41658ffc'); "Copier dans le Presse-papiers.")

        Add-PswaAuthorizationRule -UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Vérifiez que la règle a été créée en exécutant l’applet de commande **Get-PswaAuthorizationRule** ou **Test-PswaAuthorizationRule -UserName &lt;domain\\user | computer\\user&gt; -ComputerName** &lt;nom_ordinateur&gt;. Par exemple, **Test-PswaAuthorizationRule -UserName Contoso\\JSmith -ComputerName Contoso_214**.

Après avoir configuré une règle d’autorisation, les utilisateurs autorisés peuvent se connecter à la console web et commencer à utiliser Accès Web Windows PowerShell.

<a href="" id="BKMK_configcert"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Configurer un certificat authentique</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_4" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Pour un environnement de production sécurisé, utilisez toujours un certificat SSL valide signé par une autorité de certification. La procédure décrite dans cette section détaille comment obtenir et appliquer un certificat SSL valide à partir d’une autorité de certification.

<a id="to-configure-an-ssl-certificate-in-iis-manager" class="xliff"></a>
### Pour configurer un certificat SSL dans le Gestionnaire des services Internet

1.  Dans le volet de l’arborescence du Gestionnaire des services Internet, sélectionnez le serveur sur lequel Accès Web Windows PowerShell est installé.

2.  Dans le volet de contenu, double-cliquez sur **Certificats de serveur**.

3.  Dans le volet **Actions**, effectuez l’une des opérations suivantes. Pour plus d’informations sur la configuration des certificats de serveur dans IIS, consultez [Configuration des certificats de serveur dans IIS 7](https://technet.microsoft.com/library/cc732230.aspx).

    -   Cliquez sur **Importer** pour importer un certificat existant valide depuis un emplacement sur votre réseau.

    -   Cliquez sur **Créer une demande de certificat** pour demander un certificat auprès d’une autorité de certification comme VeriSign™, Thawte ou GeoTrust®. Le nom courant du certificat doit correspondre à l’en-tête d’hôte dans la demande. Par exemple, si le navigateur client demande http://www.contoso.com/, le nom courant doit également être http://www.contoso.com/. Il s’agit de l’option recommandée la plus sécurisée pour fournir un certificat à la passerelle Accès Web Windows PowerShell.

    -   Cliquez sur **Créer un certificat auto-signé** pour créer un certificat que vous pouvez utiliser immédiatement, puis faire signer ultérieurement par une autorité de certification si besoin. Spécifiez un nom convivial pour le certificat auto-signé, comme **Accès Web Windows PowerShell**. Cette option est considérée comme non sécurisée et recommandée uniquement dans un environnement de test privé.

4.  Après avoir créé ou obtenu un certificat, sélectionnez le site web auquel il est appliqué (par exemple, le **Site Web par défaut**) dans le volet de l’arborescence du Gestionnaire des services Internet, puis cliquez sur **Liaisons** dans le volet **Actions**.

5.  Dans la boîte de dialogue **Ajouter la liaison de site**, ajoutez une liaison **https** pour le site, si aucune n’est déjà affichée. Si vous n’utilisez pas de certificat auto-signé, spécifiez le nom d’hôte de l’étape 3 de cette procédure. Si vous utilisez un certificat auto-signé, vous pouvez ignorer cette étape.

6.  Sélectionnez le certificat que vous avez obtenu ou créé à l’étape 3 de cette procédure, puis cliquez sur **OK**.

<a href="" id="BKMK_using"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Utilisation de la console Web Windows PowerShell</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_5" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Une fois qu’Accès Web Windows PowerShell est installé et la configuration de la passerelle terminée comme décrit dans cette rubrique, la console web Windows PowerShell est prête à être utilisée. Pour plus d’informations sur la prise en main de la console web, consultez [Utiliser la console web Windows PowerShell](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx).

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Voir aussi</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_6" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Documentation d’Internet Information Services (IIS) 7.0](https://technet.microsoft.com/library/cc753433.aspx)
[Aide du Gestionnaire des services Internet 7.0](https://technet.microsoft.com/library/cc732664.aspx)
[Configurer la sécurité du serveur Web (IIS 7)](https://technet.microsoft.com/library/cc731278.aspx)
[Ressources de déploiement de la sécurité IPsec](https://technet.microsoft.com/network/bb531150)

<span>Afficher :</span> Hérité Protégé

<span class="stdr-votetitle">Cette page vous a-t-elle été utile ?</span>
Oui Non

Vous avez d’autres commentaires ?

<span class="stdr-count"><span class="stdr-charcnt">1500</span> caractères restants</span> Soumettre Ignorer

<span class="stdr-thankyou">Merci !</span> <span class="stdr-appreciate">Votre avis nous intéresse.</span>

[Gérer votre profil](https://social.technet.microsoft.com/profile)

|

<a href="javascript:void(0)" id="SiteFeedbackLinkOpener"><span id="FeedbackButton" class="FeedbackButton clip20x21"> <img src="https://i-technet.sec.s-msft.com/Areas/Epx/Content/Images/ImageSprite.png?v=635975720914499532" alt="Site Feedback" id="feedBackImg" class="cl_footer_feedback_icon" /> </span>Commentaires sur le site</a> Commentaires sur le site

<a href="javascript:void(0)" id="SiteFeedbackLinkCloser">x</a>

Faites-nous part de votre expérience...

La page s’est elle chargée rapidement ?

<span> Oui<span> </span></span> <span> Non<span> </span></span>

Êtes-vous satisfait de la conception de la page ?

<span> Oui<span> </span></span> <span> Non<span> </span></span>

Donnez-nous votre avis

-   [Bulletin d’informations](https://technet.microsoft.com/cc543196.aspx)
-   |
-   [Contactez-nous](https://technet.microsoft.com/cc512759.aspx)
-   |
-   [Déclaration de confidentialité](https://privacy.microsoft.com/privacystatement)
-   |
-   [Conditions d'utilisation](https://technet.microsoft.com/cc300389.aspx)
-   |
-   [Marques](https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/)
-   |

© 2016 Microsoft

© 2016 Microsoft

Les codes et les scripts développés par un tiers et en rapport à ce site doivent faire l’objet d’une licence fournie par le tiers, qui indique qu’il détient son code. Microsoft n’est pas partie prenante. Consultez les conditions d’utilisation CDN Ajax ASP.NET (http://www.asp.net/ajaxlibrary/CDN.ashx).
<img src="https://m.webtrends.com/dcsjwb9vb00000c932fd0rjc7_5p3t/njs.gif?dcsuri=/nojavascript&amp;WT.js=No" alt="DCSIMG" id="Img1" width="1" height="1" />

