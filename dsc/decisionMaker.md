---
ms.date: 2017-06-12
author: eslesar
ms.topic: conceptual
keywords: dsc,powershell,configuration,setup
title: "Présentation de la configuration de l’état souhaité pour les décideurs"
ms.openlocfilehash: e8cb26c659dbacde814b1ceaedc0b1fb8dae9d63
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
<a id="desired-state-configuration-overview-for-decision-makers" class="xliff"></a>
# Présentation de la configuration de l’état souhaité pour les décideurs

Ce document décrit les avantages de l’utilisation de la configuration de l’état souhaité (DSC) PowerShell en entreprise. Il ne s’agit pas d’un guide technique.

<a id="what-is-desired-state-configuration" class="xliff"></a>
## Qu’est-ce que la configuration de l’état souhaité ?

La configuration d’état souhaité (DSC) Windows PowerShell est une plateforme de gestion de la configuration intégrée à Windows, basée sur des normes ouvertes. DSC est suffisamment flexible pour fonctionner de manière fiable et cohérente à chaque étape du cycle de vie de déploiement (développement, test, préproduction, production), ainsi qu’au cours d’une montée en puissance parallèle. 

DSC repose sur des « [configurations](https://msdn.microsoft.com/en-us/powershell/dsc/configurations) ».
Une configuration est un document facile à lire qui décrit un environnement d’ordinateurs (ou « nœuds ») ayant des caractéristiques spécifiques. Ces caractéristiques peuvent être aussi simples que de vérifier qu’une fonctionnalité spécifique de Windows est activée, et aussi complexes que de déployer SharePoint. 

DSC comprend également des fonctions intégrées d’analyse et de création de rapports. Si un système n’est plus conforme, DSC peut déclencher une alerte et corriger le système. 

<a id="benefits-of-using-desired-state-configuration" class="xliff"></a>
## Avantages de l’utilisation de la configuration de l’état souhaité

Les configurations sont conçues pour être facilement lues, stockées et mises à jour. Les configurations déclarent l’état dans lequel doivent être les appareils cibles. Vous n’avez donc pas à écrire d’instructions sur l’état des appareils. L’apprentissage, l’adoption, l’implémentation et la maintenance par le biais de DSC est une solution beaucoup moins coûteuse. 

La création de configurations permet de capturer les étapes d’un déploiement complexe sous la forme d’une « source unique de vérité » à un emplacement unique. Cela évite ainsi les erreurs lors de déploiements répétés d’un ensemble spécifique d’ordinateurs. Les déploiements sont également plus rapides et plus fiables. Le temps de réponse est ainsi raccourci pour les déploiements complexes.

Les configurations peuvent également être partagées via [PowerShell Gallery](https://powershellgallery.com). Cela signifie que des scénarios courants et des meilleures pratiques sont peut-être déjà disponibles pour le travail que vous devez effectuer.


<a id="desired-state-configuration-and-devops" class="xliff"></a>
## Configuration de l’état souhaité et DevOps

[DevOps](http://blogs.technet.com/b/ashleymcglone/archive/2015/11/20/devops-for-n00bs-ie-windows-people.aspx) est une association de personnes, de technologies et de cultures qui permet un déploiement et une itération rapides. DSC a été conçu autour de DevOps. Le fait de n’utiliser qu’une seule configuration pour définir un environnement signifie que les développeurs peuvent coder leurs spécifications dans une configuration et vérifier cette configuration dans le contrôle de code source. Les équipes opérationnelles peuvent facilement déployer le code sans avoir à passer par des processus manuels sujets aux erreurs. 

Les configurations sont également [pilotées par les données](https://msdn.microsoft.com/en-us/powershell/dsc/configdata), ce qui permet aux équipes opérationnelles d’identifier et de modifier plus facilement les environnements, sans intervention des développeurs. 

<a id="desired-state-configuration-on--and-off-premises" class="xliff"></a>
## Configuration d’état souhaité sur site et hors site

DSC peut être utilisé pour gérer des déploiements sur site et hors site. Pour les solutions sur site, DSC possède un [serveur collecteur](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) qui peut être utilisé pour centraliser la gestion des ordinateurs et créer des rapports sur leur état. Pour les solutions cloud, DSC peut être utilisé partout où Windows est utilisable. Il existe également des offres Azure spécifiques basées sur la configuration d’état souhaité, telles que [Azure Automation](https://azure.microsoft.com/en-us/documentation/services/automation/), qui centralise la création de rapports DSC. 

<a id="dsc-and-compatibility" class="xliff"></a>
## DSC et compatibilité

Même si DSC a fait son apparition avec Windows Server 2012 R2, il est disponible pour les systèmes d’exploitation de bas niveau via le package Windows Management Framework (WMF). Pour plus d’informations sur le package WMF, reportez-vous à la [page d’accueil de PowerShell](https://msdn.microsoft.com/en-us/powershell/). 

DSC peut également servir à gérer Linux. Pour plus d’informations, consultez [Bien démarrer avec DSC pour Linux](https://msdn.microsoft.com/en-us/powershell/dsc/lnxgettingstarted).

