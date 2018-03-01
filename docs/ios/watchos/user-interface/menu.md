---
title: "Menu – ovládací prvek (Force Touch)"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 87ab9bdf6af68448649d1b3e518743a73bb23fe7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="menu-control-force-touch"></a>Menu – ovládací prvek (Force Touch)

Podívejte se, že Kit poskytuje gesto Force Touch, která aktivuje nabídky při implementaci na obrazovce aplikace sledovat.

![](menu-images/menu.png "Zobrazení nabídky Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Neodpovídá na požadavky Force Touch

Pokud `Menu` byl implementován rozhraní řadiče, když uživatel provede Force Touch, zobrazí se v nabídce. Pokud byl implementován žádné nabídky, je na obrazovce stručně animovaný dojde k žádné další akce.

Platnost úpravy nejsou přidruženy žádné konkrétní element na obrazovce. jenom jednu nabídku lze připojit k řadiči rozhraní a objeví se bez ohledu na to, kde dojde k vynucené Touch stiskněte klávesu na obrazovce.

Mezi jeden a čtyři nabídky lze zobrazit možnosti.


## <a name="adding-a-menu"></a>Přidávání nabídek

A `Menu` musí být přidaný do `InterfaceController` ve scénáři v době návrhu. Když ovládacího prvku nabídka je přetáhli řadič rozhraní je žádné vizuální označení na verze preview storyboard ale **nabídky** se zobrazí v **Osnova dokumentu** odsazení:

![](menu-images/menu-action.png "Úprava nabídky v době návrhu")

Až čtyři nabídky můžete přidat položky do ovládacího prvku nabídky. Lze nastavit v **vlastnosti** odsazení. Můžete nastavit následující atributy:

- Název, a
- Vlastní Image, nebo
- Bitovou kopii systému: přijmout, přidat, blok, odmítnout, informace, možná, více, ztlumení, pozastavení, přehrání, opakujte obnovení, sdílené složky, náhodně, mluvčího, Koš.

Vytvoření `Action` výběrem **události** části **vlastnosti** odsazení a zadejte název pro metodu akce. Částečné metody vytvoří v kódu, který může být implementován ve třídě controller rozhraní takto:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Vlastní Image

Podobně jako u karty bitové kopie v iOS, obrázků položek nabídek vyžadují neprůhledného vzor s alfa kanálu, který umožňuje na pozadí zobrazíte pomocí.

Měli byste přidat obrázků použitých pro nabídky do projektu aplikace sledovat (ne sledování aplikace rozšíření projektu) pro nejlepší výkon.


## <a name="changing-the-menu-items"></a>Změna položky nabídky

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Přidání za běhu

Nelze způsobit `Menu` mají být přidány do řadič rozhraní za běhu, i když kolekce `MenuItem`s *můžete* změnit prostřednictvím kódu programu.
Použití `AddMenuItem` metoda, jak je znázorněno:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Rozhraní API sady sledování Xamarin.iOS aktuálně vyžaduje `selector` pro `AdMenuItem` metodu, která musí být deklarován takto:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Odebrání za běhu

`ClearAllMenuItems` Nelze volat metodu odebrat všechny *prostřednictvím kódu programu přidané* položky nabídky.

Položky nabídky, které jsou nakonfigurované ve scénáři, nelze ji vymazat.



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Doc nabídce společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
