---
title: Podpora UrhoSharp Mac
description: Tento dokument popisuje podporu systému macOS pro UrhoSharp. Popisuje postup vytvoření projektu a obsahuje odkaz na některé ukázkový kód.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: aae7b09231ae0e8f88bb9435f50fadd2ff822c1a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783340"
---
# <a name="urhosharp-mac-support"></a>Podpora UrhoSharp Mac

_Mac konkrétní nastavení a funkcí_

Při Urho je knihovny přenosných tříd a umožňuje stejné rozhraní API pro použití na celém různé platformy pro herní logiku, je stále nutné inicializovat Urho v ovladači konkrétní platformu a v některých případech, můžete využít výhod funkcí konkrétní platformu .

Na stránkách předpokládat, že `MyGame` je podtřídou třídy `Application` třídy.

## <a name="macos"></a>macOS

**Podporované architektury:** x86/x86-64 pro 32bitové a 64bitové.

## <a name="creating-a-project"></a>Vytvoření projektu

Vytvoření projektu konzoly, odkazovat Urho NuGet a pak se ujistěte, že je možné najít prostředky (adresáře, která obsahuje adresář dat).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Příklad

[Úplný příklad](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


