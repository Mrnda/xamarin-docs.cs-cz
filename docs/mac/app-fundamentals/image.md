---
title: Obrázky
description: Tento článek se zabývá práce s obrázky a ikony v aplikaci Xamarin.Mac. Popisuje vytváření a správa bitových kopií bylo nutné vytvořit ikona vaší aplikace a pomocí bitové kopie v kódu jazyka C# a rozhraní tvůrce pro Xcode.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: dc33dc78c09c0b5b7cb7533afdd2f95b8ebd9c4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="images"></a>Obrázky

_Tento článek se zabývá práce s obrázky a ikony v aplikaci Xamarin.Mac. Popisuje vytváření a správa bitových kopií bylo nutné vytvořit ikona vaší aplikace a pomocí bitové kopie v kódu jazyka C# a rozhraní tvůrce pro Xcode._


## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup k stejnou bitovou kopii a ikony nástroje, které vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje.

Existuje několik způsobů této bitové kopie, které prostředky se používají v rámci aplikace systému macOS (dříve označované jako Mac OS X). Jednoduše zobrazit obrázek v rámci uživatelského rozhraní aplikace, jeho přiřazení k ovládacímu prvku uživatelského rozhraní, například na panelu nástrojů nebo položka seznamu zdroj, k poskytování ikony, Xamarin.Mac umožňuje snadno přidat skvělé kresby do aplikace systému macOS následujícími způsoby : 

- **Prvky uživatelského rozhraní** -bitové kopie lze zobrazit jako pozadí nebo jako součást aplikace v zobrazení bitové kopie (`NSImageView`).
- **Tlačítko** -bitové kopie lze zobrazit v seznamu tlačítka (`NSButton`).
- **Obrázek buňky** – jako součást ovládacího prvku tabulka založená (`NSTableView` nebo `NSOutlineView`), obrázky lze použít v buňce bitové kopie (`NSImageCell`).
- **Položka panelu nástrojů** -bitové kopie lze přidat na panel nástrojů (`NSToolbar`) jako položka panelu nástrojů bitové kopie (`NSToolbarItem`).
- **Zdroj seznamu ikona** – jako součást Seznam zdrojů (speciálně formátovaný `NSOutlineView`).
- **Ikona aplikace** -řadu bitové kopie je možné seskupit do `.icns` nastavit a použít jako ikona aplikace. V tématu naše [ikona aplikace](~/mac/deploy-test/app-icon.md) Další informace naleznete v dokumentaci.

Kromě toho systému macOS poskytuje sadu předdefinovaných bitových kopií, které lze použít v celé vaší aplikaci.

[![Příklad spuštění aplikace](image-images/intro01.png "příklad spuštění aplikace")](image-images/intro01-large.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s obrázky a ikony v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.


## <a name="adding-images-to-a-xamarinmac-project"></a>Přidání bitové kopie do projektu Xamarin.Mac

Při přidávání obrazu pro použití v aplikaci Xamarin.Mac, existuje několik místech a způsoby, jak může vývojář zahrnout soubor bitové kopie do projektu zdroje:

- **Hlavní projekt stromu [zastaralé]** -bitové kopie lze přidat přímo do stromu projekty. Při volání metody bitové kopie, které jsou uložené ve stromové struktuře hlavní projekt z kódu, je zadána žádná umístění složky. Příklad: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Složky zdrojů [zastaralé]** -speciální **prostředky** složka je pro všechny soubory, které se stane součástí aplikace je sady například ikonu, spusťte obrazovky nebo obecné bitové kopie (nebo všechny ostatní bitovou kopii nebo soubor vývojáře které chcete přidat). Při volání metody bitové kopie, které jsou uložené v **prostředky** složku z kódu jenom jako obrázky uložené ve stromové struktuře hlavní projekt, je-li zadána žádná umístění složky. Příklad: `NSImage.ImageNamed("tags.png")`.
- **Vlastní složku nebo podsložku [zastaralé]** -Vývojář můžete přidat vlastní složku do stromu zdroj projekty a ukládání bitových kopií existuje. Umístění, kde se přidá soubor lze začlenit do podsložky, chcete-li další pomoc uspořádání projektu. Například, pokud vývojář přidat `Card` složku pro projekt a podsložkou `Hearts` do této složky, potom uložte bitovou kopii **Jack.png** v `Hearts` složky, `NSImage.ImageNamed("Card/Hearts/Jack.png")` by načíst bitové kopie modul runtime.
- **Nastaví Image katalog Asset [preferované]** – přidané v OS X El Capitan **Asset katalogů bitovou kopii sady** obsahovat všechny verze nebo reprezentace bitové kopie, které jsou nezbytné pro podporu různých zařízení a škálovat faktory pro vaše aplikace. Aniž byste museli spoléhat na název souboru obrázku prostředky (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Přidávání obrázků na bitovou kopii katalog asset nastavit

Jak jsme uvedli výše, **Asset katalogů bitovou kopii sady** obsahovat všechny verze nebo reprezentace bitové kopie, které jsou nezbytné pro podporu různých zařízení a škálovat faktory pro vaši aplikaci. Aniž byste museli spoléhat na název souboru obrázku prostředky (viz řešení nezávislé Image a Image klasifikace výše), **bitovou kopii sady** pomocí editoru Asset k určení, která image patří do které zařízení nebo řešení.

1. V **řešení Pad**, dvakrát klikněte **Assets.xcassets** soubor otevřete pro úpravy: 

    ![Výběr Assets.xcassets](image-images/imageset01.png "výběr Assets.xcassets")
2. Klikněte pravým tlačítkem na **seznamu datových zdrojů** a vyberte **nastavit novou bitovou kopii**: 

    [![Přidání nové bitové kopie sady](image-images/imageset02.png "přidání nové bitové kopie sady")](image-images/imageset02-large.png#lightbox)
3. Vyberte nové sady bitové kopie a zobrazí se editoru: 

    [![Výběr nové image sady](image-images/imageset03.png "výběr nové image sady")](image-images/imageset03-large.png#lightbox)
4. Zde jsme přetáhněte do bitových kopií pro každou z nejrůznějších zařízení a řešení vyžaduje. 
5. Dvakrát klikněte na nové bitové kopie sady **název** v **seznamu datových zdrojů** ho chcete upravit: 

    [![Název úpravy bitovou kopii sady](image-images/imageset04.png "název úpravy bitovou kopii sady.")](image-images/imageset04-large.png#lightbox)
    
Speciální **vektoru** třídy jako přidané do **bitovou kopii sady** , umožňuje zahrnout _PDF_ formátu vektoru bitové kopie v casset místo včetně rastrový obrázek jednotlivých souborů v různá řešení. Tuto metodu použijete, zadáte soubor jednoho vektoru pro **@1x** řešení (ve formátu jako soubor PDF vektoru) a **@2x** a **@3x** verze souboru, bude vygenerována v době kompilace a součástí sady aplikace.

[![Obrázek nastavení rozhraní editoru](image-images/imageset05.png "bitovou kopii nastavit rozhraní editoru")](image-images/imageset05-large.png#lightbox)

Například, pokud zahrnete `MonkeyIcon.pdf` souboru jako vektoru katalog Asset s rozlišením 150px x 150px, následující rastrový obrázek prostředky by obsažených v sadě konečné aplikace, když jeho kompilace:

1. **MonkeyIcon@1x.png** -150px x 150px řešení.
2. **MonkeyIcon@2x.png** -300px x 300px řešení.
3. **MonkeyIcon@3x.png** -450px x 450px řešení.

Při použití PDF vektoru obrázky v katalozích Asset musí vzít v úvahu následující skutečnosti:

- Toto není úplná vektoru podpory, jako PDF budou rastrového na bitmapu v době potřebné ke kompilaci a bitmap poskytuje konečné aplikace.
- Velikost bitové kopie nelze upravit, jakmile je nastavená v katalogu Asset. Pokud se pokusíte změní velikost obrázku (buď v kódu nebo pomocí automatického rozložení a velikost třídy) bitovou kopii bude zkreslený stejně jako ostatní rastrového obrázku.

Při použití **nastavit obrázek** v Xcode na tvůrce rozhraní, můžete jednoduše vybrat název sady pro z rozevíracího seznamu v **atribut Inspector**: **

![Vyberte bitovou kopii nastavit v Xcode na rozhraní tvůrce](image-images/imageset06.png "výběr image nastavit v Tvůrci rozhraní na Xcode")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Přidání nové prostředky kolekce

Při práci s obrázky v katalozích prostředky nemusí být vždy, když chcete vytvořit novou kolekci, místo přidávání obrázků na všechny **Assets.xcassets** kolekce. Například při navrhování prostředků na vyžádání.

Chcete-li přidat nový katalog prostředků do projektu:

1. Klikněte pravým tlačítkem na projekt v **řešení Pad** a vyberte **přidat** > **nový soubor...**
2. Vyberte **Mac** > **katalog Asset**, zadejte **název** kolekce a klikněte na tlačítko **nový** tlačítko: 

    ![Přidání nové katalog Asset](image-images/asset01.png "přidání nový katalog Asset")

Odsud můžete pracovat s kolekcí stejným způsobem jako výchozí **Assets.xcassets** kolekci automaticky zahrnutý v projektu.


### <a name="adding-images-to-resources"></a>Přidávání obrázků na prostředky

> [!IMPORTANT]
> Práce s obrázky v systému macOS aplikaci tato metoda je zastaralá společností Apple. Měli byste použít [sady obrázků katalogu Asset](#asset-catalogs) pro správce vaší aplikace Image místo.

Než budete moct použít soubor bitové kopie v aplikaci Xamarin.Mac (buď v kódu jazyka C# nebo z rozhraní tvůrce) musí být zahrnut do projektu **prostředky** složky, **sady prostředků**. Přidání souboru do projektu, postupujte takto:

1. Klikněte pravým tlačítkem na **prostředky** složku ve vašem projektu v **řešení Pad** a vyberte **přidat** > **přidat soubory...** : 

    ![Přidání souboru](image-images/add01.png "přidání souboru")
2. Z **přidat soubory** dialogové okno, vyberte soubory bitové kopie pro přidání do projektu, vyberte `BundleResource` pro **akce sestavení přepsání** a klikněte na tlačítko **otevřete** tlačítko:

    [![Výběr souborů k přidání](image-images/add02.png "výběru souborů k přidání")](image-images/add02-large.png#lightbox)
3. Pokud soubory již nejsou v **prostředky** složku, zobrazí se dotaz, pokud chcete **kopie**, **přesunout** nebo **odkaz** soubory. Vyberte, které každý sady vašim potřebám, obvykle které budou **kopie**:

    ![Výběr akce Přidat](image-images/add04.png "výběrem přidat akci")
4. Nové soubory bude zahrnutý v projektu a číst pro použití: 

    ![Nové soubory bitové kopie, které jsou přidány do panelu řešení pro](image-images/add03.png "přidány do panelu řešení pro nové soubory bitové kopie")
5. Opakujte postup pro soubory bitové kopie, které potřebují.

V aplikaci Xamarin.Mac, můžete použít jakoukoli png, jpg nebo soubor pdf jako zdrojové bitové kopie. V další části podíváme přidání s vysokým rozlišením verze naše bitových kopií a ikony pro podporu sítnice na základě Mac.

> [!IMPORTANT]
> Při přidávání bitové kopie do **prostředky** složky, můžete nechat **akce sestavení přepsání** nastavena na **výchozí**. Výchozí hodnota je sestavení akce pro tuto složku `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Zadejte verzím ve vysokém rozlišení všech grafických prostředků aplikace

Žádné grafické asset, který přidáte do aplikace Xamarin.Mac (ikony, vlastní ovládací prvky, vlastní kurzory, vlastní kresby atd.) musí být verzím ve vysokém rozlišení kromě jejich verze standard řešení. To je potřeba, aby vaše aplikace bude vypadat je nejlepší při spuštění na zobrazení sítnice vybavený počítači se systémem Mac.


### <a name="adopt-the-2x-naming-convention"></a>Přijmout @2x zásady vytváření názvů

> [!IMPORTANT]
> Práce s obrázky v systému macOS aplikaci tato metoda je zastaralá společností Apple. Měli byste použít [sady obrázků katalogu Asset](#asset-catalogs) pro správce vaší aplikace Image místo.

Při vytváření verze standard a ve vysokém rozlišení obrázku, postupujte při jejich zahrnutí do projektu Xamarin.Mac tyto zásady vytváření názvů pro dvojici bitové kopie:

- **Standardní řešení**  - **ImageName.filename rozšíření** (Příklad: **tags.png**)
- **S vysokým rozlišením**   -  **ImageName@2x.filename-extension** (Příklad: **tags@2x.png**)

Při přidání do projektu, by se následujícím způsobem:

![Soubory bitové kopie v řešení pro](image-images/add03.png "souborů imagí v řešení pro")

Když bitovou kopii je přiřazen k elementu uživatelského rozhraní v Tvůrci rozhraní budete jednoduše vyberte soubor v _ImageName_**.** _příponu názvu souboru_ formátu (Příklad: **tags.png**). Pro stejný využitím obrázku v kódu jazyka C#, budete vyberte soubor v _ImageName_**.** _příponu názvu souboru_ formátu.

Pokud jste Xamarin.Mac aplikace běží v systému Mac, _ImageName_**.** _příponu názvu souboru_ formátu image se použije na standardní řešení zobrazí **ImageName@2x.filename-extension** image bude automaticky odebrána zobrazení sítnice základny Mac.


## <a name="using-images-in-interface-builder"></a>Pomocí bitové kopie v Tvůrci rozhraní

Libovolný zdroj bitové kopie jste přidali do **prostředky** složky ve vašem Xamarin.Mac projektu a nastavili jste akce sestavení **BundleResource** se automaticky zobrazí v Tvůrci rozhraní a může být Vybrat v rámci prvku uživatelského rozhraní (pokud ho zpracuje bitové kopie).

V rozhraní tvůrce použít bitovou kopii, postupujte takto:

1. Přidejte bitovou kopii **prostředky** složku s **akce sestavení** z `BundleResource`: 

     ![Prostředek obrázku v řešení pro](image-images/ib00.png "prostředek obrázku v řešení pro")
2. Dvakrát klikněte **Main.storyboard** soubor otevřete pro úpravy v Tvůrci rozhraní: 

     [![Úpravy hlavní storyboard](image-images/ib01.png "úpravy hlavní storyboard")](image-images/ib01-large.png#lightbox)
3. Přetáhněte prvku uživatelského rozhraní, která přebírá bitové kopie na návrhovou plochu (například **položka panelu nástrojů Image**): 

     ![Úpravy položka panelu nástrojů](image-images/ib02.png "úpravy položka panelu nástrojů")
4. Vyberte bitovou kopii, která jste přidali do **prostředky** složku **název bitové kopie** rozevíracího seznamu: 

     [![Vyberte bitovou kopii pro položku panelu nástrojů](image-images/ib03.png "výběru obrázku pro položku panelu nástrojů")](image-images/ib03-large.png#lightbox)
5. Vybraná image se zobrazí na návrhovou plochu: 

     ![Image se zobrazí v editoru panelu nástrojů](image-images/ib04.png "bitovou kopii se zobrazuje v editoru panelu nástrojů")
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Výše uvedené kroky fungovat pro libovolný element uživatelského rozhraní, která umožňuje jejich vlastnost image v nastavit **atribut Inspector**. Znovu Pokud jste zahrnuli **@2x** verzi souboru bitové kopie, automaticky použije na sítnice zobrazení na základě Mac.

> [!IMPORTANT]
> Pokud není k dispozici v bitovou kopii **název bitové kopie** rozevíracím projektu .storyboard v Xcode zavřete a znovu ho otevřete v sadě Visual Studio for Mac. Pokud image ještě není k dispozici, ověřte, že jeho **akce sestavení** je `BundleResource` a že byl přidán bitovou kopii do **prostředky** složky.

## <a name="using-images-in-c-code"></a>Pomocí bitové kopie v kódu jazyka C#

Při načítání do paměti pomocí C# – kód ve vaší aplikaci Xamarin.Mac bitovou kopii, bitovou kopii se uloží v `NSImage` objektu. Pokud soubor bitové kopie byl součástí sady aplikací Xamarin.Mac (součástí prostředky), použijte následující kód se načíst obrázek:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Výše uvedený kód používá statickou `ImageNamed("...")` metodu `NSImage` třídy k načtení do paměti z daného bitové kopie **prostředky** složky, pokud nelze najít bitovou kopii, `null` bude vrácen. Jako bitové kopie přiřazené v Tvůrci rozhraní, pokud jste zahrnuli **@2x** verzi souboru bitové kopie, automaticky použije na sítnice zobrazení na základě Mac.

Chcete-li načíst Image mimo aplikace sady (ze souboru systému Mac), použijte následující kód:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Práce s obrázky šablony

Založená na návrh vašeho systému macOS aplikace, může nastat situace, když potřebujete přizpůsobit ikony nebo bitové kopie v rámci uživatelského rozhraní tak, aby odpovídaly změnu schématu (například uživatelské předvolby).

K dosažení tohoto efektu, přepněte _vykreslení režimu_ majetku vaší bitové kopie do **Image šablony**:

[![Nastavení image šablony](image-images/templateimage01.png "nastavení image šablony")](image-images/templateimage01-large.png#lightbox)

Z rozhraní tvůrce Xcode přiřaďte Asset bitové kopie do ovládacího prvku uživatelského rozhraní:

![Výběr obrázku v Xcode na rozhraní tvůrce](image-images/templateimage02.png "výběr image v Xcode na rozhraní tvůrce")

Nebo můžete nastavit zdroj bitové kopie v kódu:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Přidejte následující funkce, veřejné řadiče zobrazení:

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

Nakonec zbarvíte Image šablony, volání této funkce bitovou kopii Kolorovat:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Bitové kopie pomocí zobrazení tabulek

Zahrnout obrázek jako součást buněk v `NSTableView`, budete muset změnit, jak se data vrácené tabulky zobrazení `NSTableViewDelegate's` `GetViewForItem` metodu použít `NSTableCellView` místo typické `NSTextField`. Příklad:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Existuje několik řádků popište. Nejdříve pro sloupce, které chcete zahrnout obrázek, vytvoříme nový `NSImageView` požadovaná velikost a umístění, jsme také vytvořit novou `NSTextField` a umístěte výchozí pozice založené na tom, jestli se používá bitovou kopii:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Za druhé, je potřeba zahrnout nové zobrazení bitové kopie a textové pole do nadřazené `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

A konečně je potřeba říct textové pole, můžete zmenšit a růst s buňku zobrazení tabulky:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Příklad výstupu:

[![Příklad zobrazení obrázku v aplikaci](image-images/tables01.png "příklad zobrazení obrázku v aplikaci")](image-images/tables01-large.png#lightbox)

Další informace o práci se zobrazení tabulek, najdete v tématu naše [zobrazení tabulek](~/mac/user-interface/table-view.md) dokumentaci.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Zobrazení osnovy pomocí bitové kopie

Zahrnout obrázek jako součást buněk v `NSOutlineView`, budete muset změnit, jak se data vrácený zobrazení osnovy `NSTableViewDelegate's` `GetView` metodu použít `NSTableCellView` místo typické `NSTextField`. Příklad:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Existuje několik řádků popište. Nejdříve pro sloupce, které chcete zahrnout obrázek, vytvoříme nový `NSImageView` požadovaná velikost a umístění, jsme také vytvořit novou `NSTextField` a umístěte výchozí pozice založené na tom, jestli se používá bitovou kopii:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Za druhé, je potřeba zahrnout nové zobrazení bitové kopie a textové pole do nadřazené `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

A konečně je potřeba říct textové pole, můžete zmenšit a růst s buňku zobrazení tabulky:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Příklad výstupu:

[![Příklad bitové kopie se zobrazí v zobrazení osnovy](image-images/outline01.png "příklad bitové kopie se zobrazí v zobrazení osnovy")](image-images/outline01-large.png#lightbox)

Další informace o práci se zobrazení osnovy, najdete v tématu naše [zobrazení osnovy](~/mac/user-interface/outline-view.md) dokumentaci.


## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce s obrázky a ikony v aplikaci Xamarin.Mac trvá. Jsme viděli různé typy a používá bitové kopie, jak používat bitové kopie a ikony v Xcode na rozhraní tvůrce a jak pracovat s obrázky a ikony v kódu jazyka C#.



## <a name="related-links"></a>Související odkazy

- [MacImages (ukázka)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Zobrazení osnovy](~/mac/user-interface/outline-view.md)
- [systému macOS X Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [O vysokém rozlišení pro OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
