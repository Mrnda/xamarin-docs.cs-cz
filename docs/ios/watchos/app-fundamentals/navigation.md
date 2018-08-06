---
title: Práce s watchOS navigace v Xamarinu
description: Tento dokument popisuje, jak pracovat s navigací v aplikaci watchOS. Popisuje modální rozhraní, hierarchické navigační a na stránce rozhraní.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c9bcfc388164060549ca7010d11671abfa8230ac
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790637"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>Práce s watchOS navigace v Xamarinu

Nejjednodušší navigační možnost k dispozici v hodinek je jednoduchý [Modální místní](#modal) které se zobrazí nad aktuální scény.

Pro více scény sledovat existuje aplikace jsou dvě vzorů navigace k dispozici:

- [Hierarchická navigace](#Hierarchical_Navigation)
- [Na stránce rozhraní](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>Modální rozhraní

Použití `PresentController` metoda modálně otevřete řadič rozhraní. Řadič rozhraní musí být již definována v **Interface.storyboard**.

```csharp
PresentController ("pageController","some context info");
```

Modálně uvedené řadiče použít na celou obrazovku (na předchozím scény). Ve výchozím nastavení je název hodnotu **zrušit** a na tuto položku se zavřít kontroleru.

Prostřednictvím kódu programu zavřete řadičem modálně uvedené, volání `DismissController`.

```csharp
DismissController();
```

Modální obrazovky může být buď jeden scény, nebo použijte na stránce rozložení.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>Hierarchická navigace

Uvede scény jako zásobníku, která může být navigaci zpátky pomocí, podobně jako `UINavigationController` funguje v systému iOS. Scény můžete vloženy do zásobníku navigační a odebrány (buď prostřednictvím kódu programu, nebo po výběru uživatele).

![](navigation-images/hierarchy-1.png "Scény může být vloženy do zásobníku navigační") ![](navigation-images/hierarchy-2.png "scény může být odebrány z navigační zásobníku")

Stejně jako u iOS, levé straně edge prstem přejde zpět na rodičovský ovladač v zásobníku hierarchické navigace.

Obě [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) a [WatchTables](https://developer.xamarin.com/samples/WatchTables) ukázky zahrnují hierarchické navigace.

### <a name="pushing-and-popping-in-code"></a>Vložení a odebrání v kódu

Podívejte se na Kit nevyžaduje "navigační controller" která v přepsání protíná má být vytvořen jako iOS nemá – jednoduše push řadiče pomocí `PushController` metoda a navigační zásobníku se automaticky vytvoří.

```csharp
PushController("secondPageController","some context info");
```

Bude obsahovat obrazovky sledování **zpět** tlačítko v levé horní části, ale můžete odebrat scény také programově pomocí zásobníku navigační `PopController`.

```csharp
PopController();
```

Stejně jako u iOS, je také možné vrátit se do kořenového adresáře pomocí zásobníku navigační `PopToRootController`.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Pomocí Segues

Segues lze vytvořit mezi scény ve scénáři, můžete definovat hierarchické navigace. Chcete-li získat kontext pro cíl scény, volání operačního systému `GetContextForSegue` k chybě při inicializaci nový řadič rozhraní.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```
<a name="Page-Based_Interfaces"/>

## <a name="page-based-interfaces"></a>Na stránce rozhraní

Na stránce rozhraní prstem zleva doprava, podobně jako `UIPageViewController` funguje v systému iOS. Ve spodní části obrazovky zobrazit stránku, která je aktuálně zobrazený se zobrazí indikátor tečky.

![](navigation-images/paged-1.png "První stránka ukázka") ![](navigation-images/paged-2.png "ukázka druhé stránce") ![](navigation-images/paged-5.png "ukázka páté stránky")


Chcete-li rozhraní založené na stránku na hlavní uživatelské rozhraní pro sledování aplikace, použijte `ReloadRootControllers` s pole rozhraní řadiče a kontexty:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Jej můžete zobrazit na stránce řadiče, která není kořenovou pomocí `PresentController` z jednoho z jiných scény v aplikaci.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
