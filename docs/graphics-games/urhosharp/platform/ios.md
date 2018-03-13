---
title: UrhoSharp iOS a tvOS podpory
description: "iOS a tvOS konkrétní nastavení a funkcí pro UrhoSharp."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 465fed25f360f29ad0b63146add8de939fa8924e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS a tvOS podpory

_iOS a tvOS konkrétní nastavení a funkcí_

Při Urho je knihovny přenosných tříd a umožňuje stejné rozhraní API pro použití na celém různé platformy pro herní logiku, je stále nutné inicializovat Urho v ovladači konkrétní platformu a v některých případech, můžete využít výhod funkcí konkrétní platformu .

Na stránkách předpokládat, že `MyGame` je sublcass z `Application` třídy.

## <a name="ios-and-tvos"></a>iOS a tvOS

**Podporované architektury:** armv7, arm64, i386

## <a name="creating-a-project"></a>Vytvoření projektu

Vytvoření projektu iOS a potom přidat Data do adresáře prostředků a zajistěte, aby všechny soubory **BundleResource** jako **akce sestavení**.

![Projekt instalace](ios-images/image-4.png "přidat Data do adresáře prostředků")

## <a name="configuring-and-launching-urho"></a>Konfigurace a spuštění Urho

Přidání direktivy using pro příkazy `Urho` a `Urho.iOS` obory názvů a poté přidejte tento kód pro inicializaci Urho, jakož i spuštění aplikace:

```csharp
new MyGame().Run();
```

Všimněte si, že vzhledem k tomu, že očekává iOS `FinishedLaunching` k dokončení, by měl fronty volání `Run()` spustit po dokončení metody, to je běžné stylu:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

Je důležité zakázat optimalizace PNG, protože Optimalizátor výchozí iOS PNG vygeneruje bitové kopie, které Urho nemůže využívat aktuálně správně

## <a name="custom-embedding-of-urho"></a>Vlastní vložení Urho

Můžete můžete případně na situaci, kdy Urho převzít obrazovce celá aplikace, a pokud chcete použít jako součást vaší aplikace, můžete vytvořit `UrhoSurface` tedy `UIView` , můžete vložit do aplikace.

Je to, co by musíte udělat:

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

To bude hostitelem třídě Urho, a poté by proveďte:

```csharp
new MyGame().Run ();
```

