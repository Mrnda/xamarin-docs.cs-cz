---
title: iOS 7 přehled uživatelského rozhraní
description: iOS 7 zavádí nadbytku změny uživatelského rozhraní. V tomto článku klade důraz některé větší změny v vzhled ovládacích prvků a rozhraní API, které podporují nový design.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3f5ea8abd41e718f9ac947c5acb290dffe400ddd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 přehled uživatelského rozhraní

_iOS 7 zavádí nadbytku změny uživatelského rozhraní. V tomto článku klade důraz některé větší změny v vzhled ovládacích prvků a rozhraní API, které podporují nový design._

iOS 7 se zaměřuje na obsah přes chrome. Prvky uživatelského rozhraní v iOS 7 zdůraznit chrome odebráním atributy například nadbytečné ohraničení, stavové řádky a navigační panely, které snižují množství místa na obrazovce používá zobrazení obsahu. Obsah v iOS 7, je určen k použití na celé obrazovce.

iOS 7 přináší několik změn v jiných: barva se používá k rozlišení prvků uživatelského rozhraní, místo atributy, jako je tlačítko ohraničení. Mnoho prvky, jako jsou například navigační panely a stavové řádky jsou nyní rozostřeného a průhledná nebo transparentní, s obsahu zobrazení trvá oblasti pod nimi. Tato zobrazení obsahu vykreslit prostřednictvím rozostřeného řádky, zdůraznění pocit hloubka v uživatelském rozhraní.

Tento článek se zabývá řadu změny prvky uživatelského rozhraní v iOS 7 a také různé rozhraní API související s nový návrh uživatelského rozhraní.

## <a name="view-and-control-changes"></a>Zobrazení a řízení změn

Všechna zobrazení v UIKit požadavkům nový vzhled a chování systému iOS 7. V této části jsou zdůrazněné některé změny těchto zobrazeních a také související rozhraní API, které se změnily pro podporu nového uživatelského rozhraní.

### <a name="uibutton"></a>UIButton

Tlačítka vytvořené z `UIButton` třídy jsou nyní bez okrajů bez pozadí ve výchozím nastavení, jak je uvedeno níže:

 ![](ios7-ui-images/button.png "Ukázka UIButton")

`UIButtonType.RoundedRect` Styl je zastaralá. Pokud se používá v iOS 7, `UIButtonType.RoundedRect` bude mít za následek `UIButtonType.System` používají, který vytvoří výchozí styl tlačítko bez pozadí a viditelná hranami, jak je uvedeno výše.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Podobně jako `UIButton`, panelu tlačítka jsou i bez okrajů, jako výchozí bude použit k novému `UIBarButtonItemStyle.Plain` styl vidíte níže:

 ![](ios7-ui-images/barbuttonplain.png "Ukázka UIBarButtonItem")

Kromě toho `UIBarButtonItemStyle.Bordered` styl je zastaralá. Nastavení `UIBarButtonItemStyle.Bordered` v iOS 7 způsobí `UIBarButtonItemStyle.Plain` styl používá.

`UIBarButtonItemStyle.Done` Styl není zastaralá. Však vytvoří také bez okrajů tlačítko pouze s tučně styl, jak je znázorněno:

 ![](ios7-ui-images/barbuttondone.png "Ukázka UIBarButtonItem v styl Done")

### <a name="uialertview"></a>UIAlertView

Kromě Změna stylu pro nový iOS 7 vzhled a chování zobrazení výstrah již nepodporují přizpůsobení prostřednictvím dílčí zobrazení. I když `UIAlertView` dědí z `UIView`, volání `AddSubview` na `UIAlertView` nemá žádný vliv. Zvažte například následující kód:

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

Tímto se vytvoří standardní zobrazení výstrah, s dílčí zobrazení bude ignorována, jak je uvedeno níže:

 ![](ios7-ui-images/alert.png "Ukázka UIAlertView")
 
 Poznámka: UIAlertView byla zrušena v iOS 8. Zobrazení [výstrahy řadiče](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/) recept na pomocí zobrazení výstrah v iOS 8 a vyšší.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Segmentovaným ovládacích prvků v iOS 7 jsou transparentní a podporují odstín barvy. Odstín barvy se používá pro barvu textu a ohraničení. Pokud je vybraná segment, barva je si místo mezi na pozadí a text, se TINT – barvu použitou zvýrazněte vybraný segment, jak je uvedeno níže:

 ![](ios7-ui-images/segmentedcontrol.png "Ukázka UISegmentedControl")

Kromě toho `UISegmentedControlStyle` se již nepoužívá v iOS 7.

### <a name="picker-views"></a>Výběr zobrazení

Rozhraní API pro výběr zobrazení je z velké části beze změny; Pokyny pro iOS 7 návrh však nyní stavu výběr zobrazení by měla zobrazit vložené spíš než jako vstupní zobrazení animovaný z dolní části obrazovky nebo prostřednictvím nového řadiče nabídnutých do zásobníku řadič navigace, jako předchozí iOS verze. To se zobrazí v aplikaci kalendáře systému:

 ![](ios7-ui-images/inlinepicker.png "To můžete zobrazit v aplikaci systému kalendáře")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Panel hledání se teď zobrazí v navigačním panelu, když `UISearchDisplayController.DisplaysSearchBarInNavigationBar` je nastavena na hodnotu true. Pokud nastavíte hodnotu false, výchozí hodnota - navigačním panelu je skrytý, pokud se zobrazí řadičem vyhledávání.

Následující snímek obrazovky ukazuje panelu Hledat v rámci `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Ukázka UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Rozhraní API související `UITableView` především jsou beze změny; ale se styl změnila výrazně tak, aby odpovídala nové návrh uživatelského rozhraní. Interní zobrazení hierarchie se taky poněkud liší. Tato změna nebude mít vliv na většinu aplikací, ale je něco znát.

#### <a name="grouped-table-style"></a>Styl seskupené tabulky

Styl seskupené změnit byl aktualizován s obsahem teď rozšíření s okraji obrazovky, jak je uvedeno níže:

 ![](ios7-ui-images/table1.png "Styl ukázku seskupené tabulky")

#### <a name="separatorinset"></a>SeparatorInset

Oddělovač řádku může být nyní odsazeny nastavením `UITableVIewCell.SeparatorInset` vlastnost. Například následující kód se použije pro odsazení v buňkách od levého okraje:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

To vytváří v tabulce zobrazení se zobrazují odsazené buněk, jak je uvedeno níže:

 ![](ios7-ui-images/separatorinset.png "Ukázka UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Styly tlačítek tabulky

Tlačítka různých používaných v zobrazeních tabulky všechny změnily. Na následujícím snímku obrazovky představuje zobrazení tabulky v režimu úprav:

 ![](ios7-ui-images/table2.png "Tomto snímku obrazovky představuje zobrazení tabulky v režimu úprav")

### <a name="additional-control-changes"></a>Změny další ovládací prvek

Další ovládací prvky UIKit taky změnily, včetně posuvníky, přepínače a steppers. Tyto změny jsou čistě visual. Další informace najdete v části společnosti Apple [iOS 7 přechod uživatelského rozhraní průvodce](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Uživatelské rozhraní obecné změny

Kromě změn v UIKit, iOS 7 zavádí řadu visual změny v uživatelském rozhraní, včetně:

-  Obsah celé obrazovky
-  Panel vzhledu
-  Odstín barvy

<a name="fullscreen" />

### <a name="full-screen-content"></a>Obsah celé obrazovky

iOS 7 slouží pro aplikace využívat výhod celé obrazovky. Řadiče zobrazení se nyní zobrazí překryté k stavového řádku a v navigačním panelu – pokud takové existuje - a která se objeví pod stav a navigační panely.

Během přípravy aplikace pro iOS 7, můžete změnit zarovnání dílčích zobrazení vizuálně pomocí *rozhraní tvůrce* nebo *Xamarin iOS Návrhář*. Také můžete jeden z nových rozhraní API a pomáhaly zvládat přes celou obrazovku obsahu prostřednictvím kódu programu. Tato rozhraní API jsou zavedené níže.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide a BottomLayoutGuide

 `TopLayoutGuide` a `BottomLayoutGuide` sloužit jako referenční kde by měl zobrazení začínat ani končit, tak, aby obsah není překrytý průhledná `UIKit` řádku, jako v následujícím příkladu:

 [![](ios7-ui-images/clipped.png "Ukázkový obsah není překrytý průhledná UIKit panelu")](ios7-ui-images/clipped.png#lightbox)

Tato rozhraní API slouží k výpočtu přestavění zobrazení z horní nebo dolní části obrazovky a odpovídajícím způsobem upravit umístění obsahu:

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

Můžeme použít hodnotu vypočítat výše nastavit naše `ImageView`na přestavění z horní části obrazovky, takže celého obrázku je vidět:

 [![](ios7-ui-images/good2.png "Příklad ImageViews přestavění z horní části obrazovky")](ios7-ui-images/good2.png#lightbox)

Odkazovat [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) pro pracovní vzorek.

Hodnota posunutí je generována dynamicky po přidání zobrazení do hierarchie, takže pokusu o čtení `TopLayoutGuide` a `BottomLayoutGuide` hodnoty v `ViewDidLoad` vrátí 0. Po zobrazení načetl – například v vypočítat hodnotu `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` a `BottomLayoutGuide` jsou zastaralé v iOS 11 považuje nové rozložení bezpečné oblasti. Apple uvedli, že je kompatibilní s verzí iOS starší než iOS 11 pomocí bezpečné oblasti. Další informace najdete v tématu [aktualizace aplikace pro iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) průvodce.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Toto rozhraní API Určuje, které hrany zobrazení je třeba rozšířit na celé obrazovce, bez ohledu na panelu průsvitnosti. V iOS 7 navigační panely a panelů nástrojů zobrazí vrstvu nad kontroleru zobrazení – na rozdíl od v iOS předchozí verze, kde nebyla trvat až stejnému adresnímu prostoru. Aplikace systému iOS 7 fotografie znázorňuje výchozí `UIViewController.EdgesForExtendedLayout` hodnotu `UIRectEdge.All`. Toto nastavení vyplní celé všechny čtyři strany v okně s obsahem, vytváření účinek překrývajících se a celé obrazovky:

 [![](ios7-ui-images/photos.png "Ukázka EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Klepnutím bitovou kopii odebere pruhů a ukazuje obrázek celé obrazovky:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout s pruhy odebrat")](ios7-ui-images/photos2.png#lightbox)

Protože výchozí hodnota je celé obrazovky obsah, budou mít aplikace, nakonfigurované pro iOS 6 část zobrazení oříznut, stejně jako na následující snímek obrazovky:

 [![](ios7-ui-images/clipped.png "Nakonfigurované pro iOS 6 aplikace bude mít část zobrazení oříznut, stejně jako tento snímek obrazovky")](ios7-ui-images/clipped.png#lightbox)

Úpravy `UIViewController.EdgesForExtendedLayout` upraví vlastnosti pro toto chování. Jsme můžete určit, že zobrazení nevyplní žádné okraje, vyhnete se naše zobrazení, zobrazení obsahu v prostoru obsazena navigace nebo panelů nástrojů (v každé orientaci):

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

V naší aplikaci jsme se zobrazí v že zobrazení je znovu změnit jejich umístění, takže celého obrázku je vidět:

 [![](ios7-ui-images/good.png "Příklad s viditelné celého obrázku")](ios7-ui-images/good.png#lightbox)

Všimněte si, že se při důsledky `TopLayoutGuide/BottomLayoutGuide` a `EdgesForExtendedLayout` rozhraní API jsou podobné, jsou určená k vyplnění jiné cíle. Změna `EdgesForExtendedLayout` ve výchozím nastavení může problém vyřešit oříznutí zobrazení v aplikací navržených pro systém iOS 6, ale dobrý iOS 7 návrh by měl respektovat estetické celé obrazovky a poskytují celé obrazovce zobrazení prostředí, spoléhat na `TopLayoutGuide` a `BottomLayoutGuide`k správně umístění obsahu, který má chtěli upravit do možnost místo pro uživatele.

Odkazovat [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) pro pracovní vzorek.

### <a name="status-and-navigation-bars"></a>Stav a navigační panely

Průhlednost se vykreslují stavového řádku a navigační panely. Stavové řádky jsou transparentní, panely nástrojů a navigační panely jsou průhledné a rozostřeného nesou pocit hloubka v uživatelském rozhraní. Následující snímek obrazovky ukazuje tento stírá a transparentnosti, kde je barva pozadí blue zobrazení kolekce zobrazeno stavu a navigační pruhy, poskytnete jim světla blue vzhled:

 ![](ios7-ui-images/transparent-navbar.png "Ukázka stavu a navigační panel stírá")

#### <a name="status-bar-styles"></a>Styly stavového řádku

Spolu s stírá a průhlednost může být aktivní stavového řádku světlý nebo tmavý (tmavý se výchozí hodnota). Styl panelu stavu můžete nastavit od řadiče zobrazení. Řadič zobrazení můžete také nastavit, zda je skrytý nebo zobrazí stavový řádek.

Například následující kód přepsání `PreferredStatusBarStyle` metoda řadič zobrazení, aby se na stavovém řádku zobrazit světla popředí:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

To způsobí, že stavový řádek zobrazí jako níže:

 ![](ios7-ui-images/light-status-bar.png "Ukázka stavového řádku")

Chcete-li skryje stavový řádek z řadiče zobrazení kódu, přepište `PrefersStatusBarHidden`, jak je uvedeno níže:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

To skryje stavový řádek:

 ![](ios7-ui-images/status-bar-hidden.png "Skrytý stavového řádku")

### <a name="tint-color"></a>Odstín barvy

Tlačítka se nyní zobrazují jako text bez chrome. Barva textu se dá řídit pomocí nové `TintColor` vlastnost `UIView`. Nastavení `TintColor` použije barvu celého zobrazení hierarchie pro zobrazení, které ji nastaví. Použít `TintColor`v celé aplikaci a nastavte ji na `Window`. Můžete také zjistit, kdy se změní barvu TINT – prostřednictvím `UIView.TintColorDidChange` metoda.

Například následující snímek obrazovky ukazuje účinek změna odstín barvy zobrazení řadič navigace na fialový:

 ![](ios7-ui-images/tint-color.png "Fialové odstín barvy na navigační řadiče zobrazení")

Odstín barvy lze použít pro bitové kopie i když `RenderingMode` je nastaven na `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Odstín barvy se nedají nastavit pomocí `UIAppearance`.


### <a name="dynamic-type"></a>Dynamic Type

V iOS 7 může uživatel zadat velikost textu v nastavení systému. Dynamické typu písmo objektů je upravena dynamicky vypadat dobře bez ohledu na velikost. `UIFont.PreferredFontForTextStyle` slouží k získání písma, která je optimalizovaná pro velikost řídí uživatele.

## <a name="summary"></a>Souhrn

Tento článek se zabývá změny prvky uživatelského rozhraní v iOS 7. Ho prozkoumá některé změny provedené v zobrazení a ovládacích prvků v UIKit, zvýraznění visual změny a také změny související rozhraní API. Nakonec zavádí nové rozhraní API pro práci s celé obrazovky obsahu, Nová podpora odstín barvy a dynamického typu.

## <a name="related-links"></a>Související odkazy

- [ImageViewer (ukázka)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
