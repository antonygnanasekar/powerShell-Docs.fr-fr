---
ms.date: 2017-06-12
author: JKeithB
ms.topic: reference
keywords: wmf,powershell,configuration
ms.openlocfilehash: fc517cd204b8f2647b824f0b9ee8f0f8f62fb821
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
<a id="declare-base-class" class="xliff"></a>
# Déclarer une classe de base
Vous pouvez déclarer une classe Windows PowerShell comme type de base pour une autre classe Windows PowerShell.

```PowerShell
class bar
{
   [int]foo() 
       {
           return 100500
       }
}

class baz : bar {}

[baz]::new().foo() # return 100500
```

Vous pouvez également utiliser des types .NET Framework existants comme classes de base :

```PowerShell
class MyIntList : system.collections.generic.list[int]
{
    
}

$list = [MyIntList]::new()

$list.Add(100)

$list[0] # return 100
```

