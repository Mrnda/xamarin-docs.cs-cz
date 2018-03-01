---
title: Visual Design Updates
description: "Zkoumání nové funkce systému iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2904a7da73f5bf6e8960f65239d1f8dc52ab1aba
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="visual-design-updates"></a>Visual Design Updates

_Zkoumání nové funkce systému iOS 11_

Společnost Apple vydala v iOS 11, nastavení nové visual změny, včetně aktualizací na navigačním panelu, panelu vyhledávání a zobrazení tabulek. Kromě toho vylepšily k povolení pro větší flexibilitu v okraje a obsah celé obrazovky. Tyto změny jsou popsané v této příručce.

## <a name="uikit"></a>UIKit

Řádky UIKit byly přizpůsobeny v iOS 11, aby byly přístupnější pro koncové uživatele.

Jednu těchto změn je nové HUD zobrazení, které se zobrazí, když uživatel dlouho stisknutí na panelu položky. Chcete-li povolit, nastavte `largeContentSizeImage` vlastnost `UIBarItem` a přidejte větší bitovou kopii prostřednictvím [katalog asset](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Navigační panel
iOS 11 zavedl nové funkce usnadnění názvy navigačním panelu. Aplikace můžete zobrazit tento nadpis větší přiřazením `PrefersLargeTitles` vlastnost na hodnotu true:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Umožňuje větší názvy nastavení ve vaší aplikaci _všechny_ názvy řádky navigace mezi aplikace zobrazí větší, jak ukazuje následující snímek obrazovky:

![Navigační název](visual-design-images/image7.png)

Chcete-li řídit, kdy velké název se zobrazí na navigačním panelu, nastavte `LargeTitleDisplayMode` na navigační položka k `Always`, `Never`, nebo `Automatic`.

### <a name="search-controller"></a>Vyhledávání řadiče

iOS 11 je snazší přidat řadič vyhledávání přímo na navigačním panelu. Jakmile vytvoříte řadič vyhledávání, přidejte ji do navigačního panelu s `SearchController` vlastnost:

```csharp
NavigationItem.SearchController = searchController;
```

[![Název navigační panel hledání](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png)

V závislosti na funkce aplikace může nebo nemusí být žádoucí panelu Hledat skrytí potom, co uživatel posune prostřednictvím seznamu. Můžete upravit pomocí `HidesSearchBarWhenScrolling` vlastnost.

## <a name="margins"></a>Okraje

Apple vytvořil novou vlastnost – `directionalLayoutMargins` – které lze nastavit mezery mezi zobrazení a dílčích zobrazení. Použití `directionalLayoutMargins` s `leading` nebo `trailing` vsazení. Bez ohledu na to, zda je systém s jazykem zleva doprava nebo zprava doleva je správně nastavené mezer v aplikaci s iOS.

V iOS 10 a před všechna zobrazení měla minimální povolená velikost, ke kterému by zarovnat. iOS 11 zavedla možnost přepsání, že pomocí `ViewRespectsSystemMinimumLayoutMargins`. Například nastavení této vlastnosti na hodnotu false můžete upravit vaší hraniční vsazení na nulu:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Obrázek znázorňující rozpětí s nulové inset v ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Obsah celé obrazovky

iOS 7 [zavedená](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` a `bottomLayoutGuide` jako způsob omezíte zobrazení, která nejsou skryt UIKit řádky a jsou v viditelné oblasti na obrazovce. Tyto jsou zastaralé v iOS 11 pro _bezpečné oblasti_.

Bezpečné oblast je nový způsob přemýšlení o viditelné prostoru aplikace, a jak se přidají omezení mezi zobrazením a super zobrazení. Zvažte například na následujícím obrázku:

[![Oblast pro vs horní a dolní rozložení Průvodce](visual-design-images/image10-sml.png)](visual-design-images/image10.png)

Dříve, pokud byl přidán zobrazení a chtěli ho mají být zobrazeny v oblasti zelená výše, můžete by omezit její _dolní_ z `TopLayoutGuide` a _horní_ z `BottomLayoutGuide`. V iOS 11, můžete byste místo toho omezit její _horní_ a _dolní_ bezpečné oblasti. Příklad:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Zobrazení tabulky

UITableView zaznamenala počet malých, ale důležité, změny v iOS 11.

Ve výchozím nastavení záhlaví, zápatí a buněk jsou nyní automaticky velikost na základě jejich obsahu. Pro vyjádření výslovného nesouhlasu této sady automatickou chování `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, nebo `EstimatedSectionFooterHeight` na hodnotu nula.

Ale v některých případech (například při přidání UITableViewController v iOS Designer nebo při použití existující Storboards v Tvůrci rozhraní) budete muset ručně povolit automatickou velikostí buněk. K tomu, ujistěte se, že jste nastavili následující vlastnosti zobrazení tabulky pro buněk, záhlaví a zápatí stránky, v uvedeném pořadí:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 se rozšířila funkce řádek akce. `UISwipeActionsConfiguration` byla zavedena k definování sady akcí, které mají proběhnout při swipes uživatel v obou směrech na řádek v tabulce zobrazení. Toto chování je podobné jako nativní Mail.app. Další informace najdete v tématu [řádek akce](~/ios/user-interface/controls/tables/row-action.md) průvodce.

Zobrazení tabulek je podpora přetažení v iOS 11. Další informace najdete v tématu [přetažení](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) průvodce.


## <a name="related-links"></a>Související odkazy

- [Co je nového v iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Stránka produktu aktualizované obchodu s aplikacemi (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizace aplikace pro iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
