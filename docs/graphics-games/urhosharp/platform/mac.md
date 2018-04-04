---
title: Podpora UrhoSharp Mac
description: Mac konkrétní nastavení a funkcí pro UrhoSharp.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 4f439d8f848e84b0e775a56204a1834b55c7ad10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
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


