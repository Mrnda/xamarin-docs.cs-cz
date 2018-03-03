---
title: Podpora UrhoSharp Mac
description: "Mac konkrétní nastavení a funkcí"
ms.topic: article
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: cdaca45c3132abf3f52d407d4ce59c680248fce7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-mac-support"></a>Podpora UrhoSharp Mac

_Mac konkrétní nastavení a funkcí_

Při Urho je knihovny přenosných tříd a umožňuje stejné rozhraní API pro použití na celém různé platformy pro herní logiku, je stále nutné inicializovat Urho v ovladači konkrétní platformu a v některých případech, můžete využít výhod funkcí konkrétní platformu .

Na stránkách předpokládat, že `MyGame` je podtřídou třídy `Application` třídy.

# <a name="macos-x"></a>MacOS X

**Podporované architektury:** x86/x86-64 pro 32bitové a 64bitové.

# <a name="creating-a-project"></a>Vytvoření projektu

Vytvoření projektu konzoly, odkazovat Urho NuGet a pak se ujistěte, že je možné najít prostředky (adresáře, která obsahuje adresář dat).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Příklad

[Úplný příklad](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


