---
title: iOS 7 přehled uživatelského rozhraní
description: iOS 7 přináší množství změny uživatelského rozhraní. Tento článek popisuje některé větší změny, vzhled ovládacích prvků a rozhraní API, která podporují nový návrh.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7e7725418ed104bd9be014b80d20fd62de144ca5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242287"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 přehled uživatelského rozhraní

_iOS 7 přináší množství změny uživatelského rozhraní. Tento článek popisuje některé větší změny, vzhled ovládacích prvků a rozhraní API, která podporují nový návrh._

iOS 7 se zaměřuje na obsah v chrome. Prvky uživatelského rozhraní v Iosu 7 zdůraznit chrome odebráním atributům, jako je nadbytečné ohraničení, stavovém řádku a navigačních panelů, které snižují množství obsahu zobrazení používané místo na obrazovce. V systému iOS 7 obsah je určen k použití na celou obrazovku.

iOS 7 přináší několik dalších změn: barva se používá k rozlišení prvků uživatelského rozhraní, namísto atributům, jako je tlačítko ohraničení. Mnoho prvků, jako je například navigačních panelů a stavové řádky jsou nyní rozmazaný a více průchody průsvitných nebo průhledné s obsahu zobrazení trvá oblast pod nimi. Tato zobrazení obsahu vykreslení prostřednictvím rozmazaný pruhy, které odlišují pocit hloubka v uživatelském rozhraní.

Tento článek se týká několik změn pro prvky uživatelského rozhraní v iOS 7, jakož i různých rozhraní API související s nový návrh uživatelského rozhraní.

## <a name="view-and-control-changes"></a>Zobrazení a řízení změn

Všechna zobrazení v Uikitu odpovídat nový vzhled a chování systému iOS 7. Tato část ukazuje některé změny těchto zobrazeních, jakož i související rozhraní API, které se změnily pro podporu nové uživatelské rozhraní.

### <a name="uibutton"></a>Tlačítka uživatelského rozhraní

Vytvořené z tlačítka `UIButton` třídy jsou teď další bez pozadí ve výchozím nastavení, jak je znázorněno níže:

 ![](ios7-ui-images/button.png "Ukázka tlačítka uživatelského rozhraní")

`UIButtonType.RoundedRect` Style je zastaralá. Pokud se používá v systému iOS 7, `UIButtonType.RoundedRect` způsobí `UIButtonType.System` používán; produkuje výchozí styl tlačítka bez pozadí nebo viditelné hrany, jak je znázorněno výše.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Podobně jako `UIButton`, panel tlačítek jsou i další, jako výchozí se použije k novému `UIBarButtonItemStyle.Plain` style je uvedeno níže:

 ![](ios7-ui-images/barbuttonplain.png "Ukázka UIBarButtonItem")

Kromě toho `UIBarButtonItemStyle.Bordered` style je zastaralá. Nastavení `UIBarButtonItemStyle.Bordered` v iOS 7 způsobí `UIBarButtonItemStyle.Plain` stylu používá.

`UIBarButtonItemStyle.Done` Nebyl zastaralý styl. Však vytvoří také další tlačítko, pouze se stylem tučným písmem, jak je znázorněno:

 ![](ios7-ui-images/barbuttondone.png "Ukázka UIBarButtonItem ve stylu Hotovo")

### <a name="uialertview"></a>UIAlertView

Zobrazení výstrah kromě Změna stylu pro vzhled a chování nového systému iOS 7, už nebude podporovat přizpůsobení prostřednictvím dílčí zobrazení. I když `UIAlertView` dědí z `UIView`, volání `AddSubview` na `UIAlertView` nemá žádný vliv. Zvažte například následující kód:

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

Tímto se vytvoří standardní zobrazení výstrah s dílčí zobrazení bude ignorována, jak je znázorněno níže:

 ![](ios7-ui-images/alert.png "Ukázka UIAlertView")
 
 Poznámka: UIAlertView se přestala nabízet v iOS 8. Zobrazení [upozornění řadič](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) recept na pomocí zobrazení výstrah v iOS 8 a novějším.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Segmentované ovládací prvky v iOS 7 jsou transparentní a podporují zabarvení. Barva zabarvení se používá pro barvu textu a ohraničení. Při výběru segment, který je barva prohození mezi na pozadí a text, barvu pro zvýraznění vybraného segmentu, jak je znázorněno níže:

 ![](ios7-ui-images/segmentedcontrol.png "Ukázka UISegmentedControl")

Kromě toho `UISegmentedControlStyle` se už nepoužívá v systému iOS 7.

### <a name="picker-views"></a>Výběr zobrazení

Rozhraní API pro výběr zobrazení je do značné míry beze změny; Pokyny pro iOS 7 návrh však nyní stavu výběru zobrazení by se měla zobrazit vložené, ne jako vstupní zobrazení animovat v dolní části obrazovky nebo prostřednictvím nového řadiče vloženy do zásobníku kontroler navigace, stejně jako v předchozím iOS verze. To lze zobrazit v aplikaci systému kalendáře:

 ![](ios7-ui-images/inlinepicker.png "To lze zobrazit v aplikaci systému kalendáře")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Na panelu hledání se teď zobrazují v navigačním panelu, když `UISearchDisplayController.DisplaysSearchBarInNavigationBar` je nastavena na hodnotu true. Pokud je nastavena na false, výchozí hodnota - navigační panel skrytý, pokud kontroler hledání se zobrazí.

Na následujícím snímku obrazovky se zobrazí na panelu hledání v rámci `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Ukázka UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Rozhraní API kolem `UITableView` hlavně jsou beze změny, ale styl změnil výrazně tak, aby odpovídal na nový návrh uživatelského rozhraní. Také se poněkud liší interní zobrazit hierarchii. Tato změna nebude mít vliv na většinu aplikací, ale je byste měli vědět.

#### <a name="grouped-table-style"></a>Styl seskupené tabulky

Seskupené změnit styl aktualizoval s obsahem nyní rozšíření na okraji obrazovky, jak je znázorněno níže:

 ![](ios7-ui-images/table1.png "Styl ukázka seskupené tabulky")

#### <a name="separatorinset"></a>SeparatorInset

Oddělovače řádků lze nyní odsazeny nastavením `UITableVIewCell.SeparatorInset` vlastnost. Například následující kód se použije pro odsazení buňky od levého okraje:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

To vytvoří v zobrazení tabulky s odsazené buňky, jak je znázorněno níže:

 ![](ios7-ui-images/separatorinset.png "Ukázka UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Styly tlačítek tabulky

O různá tlačítka používaných v zobrazeních tabulky všechny změnily. Na následujícím snímku obrazovky představuje zobrazení tabulky v režimu úprav:

 ![](ios7-ui-images/table2.png "Tento snímek obrazovky představuje zobrazení tabulky v režimu úprav")

### <a name="additional-control-changes"></a>Další ovládací prvek změny

Další ovládací prvky UIKit změnily, včetně posuvníky, přepínače a prvky krokování. Tyto změny jsou čistě visual. Další informace najdete v Apple [příručka přechod uživatelského rozhraní pro iOS 7](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Uživatelské rozhraní obecné změny

Kromě změn v Uikitu, iOS 7 přináší širokou škálu vizuální změny v uživatelském rozhraní, včetně:

-  Obsah zobrazení na celé obrazovce
-  Vzhled panelu
-  Barevný nádech

<a name="fullscreen" />

### <a name="full-screen-content"></a>Obsah celé obrazovky

iOS 7 je navržena tak, aby aplikace využívat celou obrazovku. Kontrolery zobrazení teď budou zobrazovat překryté ve stavovém řádku a v navigačním panelu – pokud existuje – na rozdíl od uvedených níže stav a navigačních panelů.

Během přípravy vaší aplikace pro iOS 7, můžete změnit zarovnání vizuálně pomocí dílčích zobrazení *Tvůrce rozhraní* nebo *Xamarin pro iOS Designer*. Můžete také použít jeden z nových rozhraní API a pomáhaly zvládat celou obrazovku obsahu prostřednictvím kódu programu. Tato rozhraní API jsou zavedeny níže.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide a BottomLayoutGuide

 `TopLayoutGuide` a `BottomLayoutGuide` sloužit jako referenční informace pro kde zobrazení by měl začínat ani končit, takže obsah není překryto neobjektovým více průchody průsvitných `UIKit` řádku, jako v následujícím příkladu:

 [![](ios7-ui-images/clipped.png "Ukázkový obsah není překryto neobjektovým více průchody průsvitných UIKit panelu")](ios7-ui-images/clipped.png#lightbox)

Tato rozhraní API slouží k výpočtu zobrazení skrytého z horní nebo dolní části obrazovky a odpovídajícím způsobem upravit umístění obsahu:

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

Můžeme použít hodnota vypočítaná výše nastavit naše `ImageView`posun od horní části obrazovky, takže celého obrázku je vidět:

 [![](ios7-ui-images/good2.png "Příklad skrytého ImageViews od horního okraje obrazovky")](ios7-ui-images/good2.png#lightbox)

Odkazovat [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) ukázku práce.

Hodnota posunutí se vygeneruje dynamicky po zobrazení je přidaný do hierarchie, tak při čtení `TopLayoutGuide` a `BottomLayoutGuide` hodnoty v `ViewDidLoad` vrátí hodnotu 0. Po zobrazení načtení – například v vypočítat hodnotu `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` a `BottomLayoutGuide` se považují za zastaralé v Iosu 11 a nové rozložení pro bezpečnou oblast. Apple uvedli, že je kompatibilní s iOS verze starší než iOS 11 pomocí bezpečnou oblast. Další informace najdete v tématu [aktualizovat vaši aplikaci pro iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) průvodce.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Toto rozhraní API Určuje, které okraji zobrazení by měl rozšířit na celou obrazovku, bez ohledu na to panelu průsvitnost. V systému iOS 7 navigačních panelů a panelů nástrojů zobrazí vrstvu nad kontroleru zobrazení – na rozdíl od předchozích IOS verze, ve kterém nebyla trvat až na stejném místě. Aplikace systému iOS 7 fotografie ukazuje takovouto výchozí `UIViewController.EdgesForExtendedLayout` hodnotu `UIRectEdge.All`. Toto nastavení vyplní všechny čtyři strany v okně s obsahem, vytváření účinek se překrývají a na celé obrazovce:

 [![](ios7-ui-images/photos.png "Ukázka EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Klepnutím na obrázek odebere pruhy a zobrazuje obrázek celé obrazovky:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout s pruhy odebrat")](ios7-ui-images/photos2.png#lightbox)

Vzhledem k tomu, že obsah celé obrazovky je výchozí nastavení, bude mít aplikace nastavené pro iOS 6 část zobrazení oříznut, stejně jako v následujícím snímku obrazovky:

 [![](ios7-ui-images/clipped.png "Aplikace nakonfigurované pro iOS 6, bude mít část zobrazení oříznut, stejně jako v tomto snímku obrazovky")](ios7-ui-images/clipped.png#lightbox)

Úpravy `UIViewController.EdgesForExtendedLayout` nastaví vlastnosti pro toto chování. Můžeme určit, že zobrazení není vyplnit jakékoli hran, tak naše zobrazení zabrání zobrazení obsahu v místo obsazené navigace nebo panelů nástrojů (na všechny orientace):

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

V naší aplikaci uvidíme, že zobrazení znovu přemístí, celého obrázku je vidět:

 [![](ios7-ui-images/good.png "Příklad s viditelné celého obrázku")](ios7-ui-images/good.png#lightbox)

Všimněte si, že při účinky `TopLayoutGuide/BottomLayoutGuide` a `EdgesForExtendedLayout` rozhraní API jsou podobné, jsou určené k vyplnění různé cíle. Změna `EdgesForExtendedLayout` výchozí nastavení lze pravděpodobně vyřešit zkrácený zobrazení v aplikacích pro iOS 6, ale návrh dobrý systému iOS 7 by měl případném dalším sdílení dodržovat aesthetic celé obrazovky a poskytovat celé obrazovce zobrazení prostředí, spoléhat na `TopLayoutGuide` a `BottomLayoutGuide`správně umístění obsahu, který se má do pohodlné místo, kde uživatel manipulovat.

Odkazovat [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) ukázku práce.

### <a name="status-and-navigation-bars"></a>Stav a navigačních panelů

Stavový řádek a navigačních panelů se vykreslují průhlednost. Stavové řádky jsou transparentní, panely nástrojů a navigačních panelů více průchody průsvitných a rozmazaný vyjádřit dojem hloubky v uživatelském rozhraní. Následující snímek obrazovky ukazuje tento rozostření a transparentnost, kde je barva modré pozadí zobrazení kolekcí zobrazeno stavu a navigační panely, jim světle modrá vzhled:

 ![](ios7-ui-images/transparent-navbar.png "Ukázka stavu a navigační panel rozostření")

#### <a name="status-bar-styles"></a>Styly stavového řádku

Spolu s rozostření a transparentnost může být popředí stavový řádek light nebo dark (tmavá se výchozí hodnota). Z řadiče zobrazení je možné nastavit styl stavového řádku. Kontroler zobrazení můžete také nastavit, zda stavový řádek je skryta nebo zobrazena.

Například následující kód přepsání `PreferredStatusBarStyle` metoda kontroler zobrazení, aby na stavovém řádku zobrazit světla popředí:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

To způsobí, že stavovém řádku zobrazí níže:

 ![](ios7-ui-images/light-status-bar.png "Ukázka stavového řádku")

Chcete-li skrýt stavový řádek z kontroleru zobrazení kódu, přepište `PrefersStatusBarHidden`, jak je znázorněno níže:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Skryje stavový řádek:

 ![](ios7-ui-images/status-bar-hidden.png "Stavový řádek skryté")

### <a name="tint-color"></a>Barevný nádech

Tlačítka se nyní zobrazují jako text bez chrome. Barva textu je možné řídit pomocí nového `TintColor` vlastnost `UIView`. Nastavení `TintColor` barva se vztahuje na celý zobrazit hierarchii pro zobrazení, které ji nastaví. Chcete-li použít `TintColor`v celé aplikaci, nastavte ho na `Window`. Můžete také zjistit, když se změní barva zabarvení prostřednictvím `UIView.TintColorDidChange` metody.

Například následující snímek obrazovky ukazuje účinek změna odstín barvy pro zobrazení kontroler navigace na fialový:

 ![](ios7-ui-images/tint-color.png "Fialový barevný nádech navigace kontrolerů zobrazení")

Odstín barvy lze použít u bitové kopie i když `RenderingMode` je nastavena na `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Barevný nádech nelze nastavit pomocí `UIAppearance`.


### <a name="dynamic-type"></a>Dynamický typ.

V systému iOS 7 může uživatel zadat velikost textu v systémových nastaveních. S dynamickým typem písmo upraví dynamicky vypadat dobře bez ohledu na velikost. `UIFont.PreferredFontForTextStyle` by měl být použít k získání písma, která je optimalizovaná pro velikost řízené uživatelem.

## <a name="summary"></a>Souhrn

Tento článek popisuje změny prvky uživatelského rozhraní v systému iOS 7. To prozkoumá některé změny provedené v zobrazení a ovládacích prvků v Uikitu zvýraznění vizuální změny, stejně jako se změní na související rozhraní API. A konečně zavádí nové rozhraní API pro práci s obsah zobrazení na celé obrazovce, nový odstín barvy podpory a dynamického typu.

## <a name="related-links"></a>Související odkazy

- [ImageViewer (ukázka)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
