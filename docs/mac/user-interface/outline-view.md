---
title: "Zobrazení osnovy"
description: "Tento článek se zabývá práci s zobrazení osnovy v aplikaci Xamarin.Mac. Popisuje vytvoření a udržování zobrazení osnovy v Xcode a Tvůrce rozhraní a práce s nimi prostřednictvím kódu programu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: dbbd10af046c0a8421e06e675364f92405b2317f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="outline-views"></a>Zobrazení osnovy

_Tento článek se zabývá práci s zobrazení osnovy v aplikaci Xamarin.Mac. Popisuje vytvoření a udržování zobrazení osnovy v Xcode a Tvůrce rozhraní a práce s nimi prostřednictvím kódu programu._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup ke stejné Outline zobrazení, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat zobrazení osnovy (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Zobrazení osnovy je typ tabulky, který uživateli umožňuje rozbalit nebo sbalit řádky hierarchické data. Podobně jako zobrazení a tabulky zobrazení osnovy zobrazí data pro sadu související položky s řádky, které představují jednotlivé položky a sloupce představující atributy tyto položky. Na rozdíl od zobrazení tabulky položky v zobrazení osnovy nejsou jako plochý seznam, jsou uspořádány do hierarchie, jako jsou soubory a složky na pevném disku.

[![](outline-view-images/populate03.png "Příklad aplikace spustit")](outline-view-images/populate03.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s zobrazení osnovy v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Úvod do zobrazení osnovy

Zobrazení osnovy je typ tabulky, který uživateli umožňuje rozbalit nebo sbalit řádky hierarchické data. Podobně jako zobrazení a tabulky zobrazení osnovy zobrazí data pro sadu související položky s řádky, které představují jednotlivé položky a sloupce představující atributy tyto položky. Na rozdíl od zobrazení tabulky položky v zobrazení osnovy nejsou jako plochý seznam, jsou uspořádány do hierarchie, jako jsou soubory a složky na pevném disku.

Pokud položku v zobrazení osnovy obsahuje další položky, můžete rozbalit nebo sbalit uživatelem. Rozšíření položky zobrazí popisným trojúhelníkem, který odkazuje na právo, když položka sbalena a body dolů, pokud je položka rozbalen. Kliknutím na zpřístupnění trojúhelníček způsobí, že položka, která má rozbalit nebo sbalit.

Zobrazení osnovy (`NSOutlineView`) je podtřídou třídy tabulka zobrazení (`NSTableView`) a jako takový většinu své chování dědí ze své nadřazené třídy. Výsledkem je mnoho činností nepodporuje zobrazení tabulky, jako je výběr řádků a sloupců, přemístění sloupců přetažením záhlaví sloupců, atd., jsou podporovány také zobrazení osnovy. Aplikace Xamarin.Mac má kontrolu nad tyto funkce a zobrazení osnovy parametry, (buď v kódu nebo rozhraní tvůrce) může nakonfigurovat tak, povolí nebo zakáže určité operace.

Zobrazení osnovy neukládá data vlastní, místo toho používá zdroje dat (`NSOutlineViewDataSource`) k poskytování řádků i sloupců, které jsou potřeba, na základě podle potřeby.

Zobrazení osnovy chování lze přizpůsobit zadáním podtřídou třídy delegáta zobrazení osnovy (`NSOutlineViewDelegate`) pro podporu správy sloupec Outline, zadejte vyberte funkce, výběr řádků a úpravy, vlastní sledování a vlastní zobrazení pro jednotlivé sloupců a řádků.

Vzhledem k tomu, že zobrazení osnovy většinu funkcí a její chování sdílí s zobrazení tabulky, můžete chtít projít naše [zobrazení tabulek](~/mac/user-interface/table-view.md) dokumentace před pokračováním v tomto článku.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Vytvoření a údržbu zobrazení osnovy v Xcode

Když vytvoříte novou aplikaci Xamarin.Mac kakao, zobrazí okno Standardní prázdné, ve výchozím nastavení. Toto systému windows je definována v `.storyboard` automaticky zahrnutý v projektu. Chcete-li upravit návrh vašeho systému windows v **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souboru:

[![](outline-view-images/edit01.png "Výběr hlavní storyboard")](outline-view-images/edit01.png#lightbox)

Otevře se okno návrhu v Xcode na rozhraní Tvůrce:

[![](outline-view-images/edit02.png "Úpravy uživatelského rozhraní v Xcode")](outline-view-images/edit02.png#lightbox)

Typ `outline` do **knihovny Inspector** vyhledávací pole, aby bylo snazší najít ovládací prvky zobrazení osnovy:

[![](outline-view-images/edit03.png "Výběr zobrazení osnovy z knihovny")](outline-view-images/edit03.png#lightbox)

Přetáhněte zobrazení osnovy na řadiče zobrazení v **rozhraní editoru**, bylo vyplnil celou oblast obsahu řadiče zobrazení a nastavte ji na kde zmenšuje a roste s oknem v **omezení Editor**:

[![](outline-view-images/edit04.png "Úpravy omezení")](outline-view-images/edit04.png#lightbox)

Vyberte zobrazení osnovy v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](outline-view-images/edit05.png "Atribut Inspector")](outline-view-images/edit05.png#lightbox)

- **Popisují sloupec** – sloupci tabulky, ve kterém se zobrazí hierarchické data.
- **Automatické ukládání Outline sloupec** – Pokud `true`, sloupci Outline budou automaticky uložena a obnovit mezi spouštět aplikace.
- **Odsazení** – velikost odsazovat sloupce pod rozbalená položka.
- **Odsazení následuje buněk** – Pokud `true`, označit odsazení bude odsazeny společně s buněk.
- **Automaticky ukládat rozšířit položky** – Pokud `true`, rozbalené/sbalené stav položky budou automaticky uložena a obnovit mezi spouštět aplikace.
- **Obsahu režimu** -umožňuje používat buď zobrazení (`NSView`) nebo buňky (`NSCell`) k zobrazení dat v řádků a sloupců. Od verze systému macOS 10.7, měli byste použít zobrazení.
- **Jako plovoucí seskupení řádků** – Pokud `true`, tabulka zobrazení bude kreslení seskupené buněk, jako by se s plovoucí čárkou.
- **Sloupce** -určuje počet zobrazených sloupců.
- **Hlavičky** – Pokud `true`, sloupce budou mít hlavičky.
- **Změna pořadí** – Pokud `true`, uživatel bude moct přetáhněte změnit pořadí sloupců v tabulce.
- **Změna velikosti** – Pokud `true`, uživatel bude moct přetáhněte záhlaví sloupce můžete změnit šířku sloupců.
- **Nastavení velikosti sloupce** -Určuje, jak automaticky velikost sloupců v tabulce.
- **Zvýrazněte** – ovládací prvky typu zvýraznění v tabulce používá při výběru buňky.
- **Alternativní řádky** – Pokud `true`, někdy druhý řádek bude mít jinou barvu pozadí.
- **Vodorovná mřížka** -vybere typ ohraničení vykreslován vodorovně mezi buněk.
- **Svislé mřížky** -vybere typ ohraničení mezi buněk vykreslován svisle.
- **Barva mřížky** -nastaví barvu ohraničení buňky.
- **Pozadí** -nastaví barvu pozadí buněk.
- **Výběr** – umožňují řídit, jak může uživatel vybrat buněk v tabulce jako:
    - **Více** – Pokud `true`, kterého uživatel může vybrat více řádků a sloupců.
    - **Sloupec** – Pokud `true`, kterého uživatel může vybrat sloupce.
    - **Vyberte typ** – Pokud `true`, uživatel může zadat znak k vybrání řádku.
    - **Prázdný** – Pokud `true`, vyberte sloupce či řádku nevyžaduje od uživatele, v tabulce umožňuje žádná výběru vůbec.
- **Automatické ukládání** -název, který není ve formátu tabulky automaticky uložit pod.
- **Informace o sloupci** – Pokud `true`, pořadí a šířku sloupců budou automaticky uloženy.
- **Konce řádků** – vyberte, jak buňky zpracovává konce řádků.
- **Zkrátí posledního řádku viditelného** – Pokud `true`, může data nevejdou uvnitř hranice jeho zkráceny buňky.

> [!IMPORTANT]
> Pokud udržujete starší verze aplikace Xamarin.Mac `NSView` na základě zobrazení osnovy slouží přes `NSCell` na základě zobrazení tabulek. `NSCell` je považován za starší verze a nemusí být podporována do budoucna.

Vyberte sloupec tabulky v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](outline-view-images/edit06.png "Atribut Inspector")](outline-view-images/edit06.png#lightbox)

- **Název** -nastaví název sloupce.
- **Zarovnání** -nastavit zarovnání textu v rámci buněk.
- **Title písma** -vybere písmo pro text záhlaví buňky.
- **Klíč řazení** -je klíč používaný k řazení dat ve sloupci. Ponechte prázdné, pokud uživatele nelze řadit v tomto sloupci.
- **Selektor** -je **akce** použít k provedení řazení. Ponechte prázdné, pokud uživatele nelze řadit v tomto sloupci.
- **Pořadí** -je pořadí řazení dat sloupců.
- **Změna velikosti** -vybere typ Změna velikosti sloupce.
- **Upravitelné** – Pokud `true`, uživatel může upravovat buněk v tabulce na základě buňky.
- **Skrytý** – Pokud `true`, sloupec je skrytý.

Můžete taky změnit velikost sloupec podle tažení jeho (svisle na střed na pravé straně sloupce) doleva nebo doprava.

Umožňuje vybrat každý sloupec v našem zobrazení tabulky a poskytněte první sloupec **název** z `Product` a druhý `Details`.

Vyberte zobrazení buněk tabulky (`NSTableViewCell`) v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](outline-view-images/edit07.png "Atribut Inspector")](outline-view-images/edit07.png#lightbox)

Toto jsou všechny vlastnosti standardní zobrazení. Máte také možnost změny velikosti řádků pro tento sloupec sem.

Vyberte zobrazení buňky tabulky (ve výchozím nastavení je to `NSTextField`) v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](outline-view-images/edit08.png "Atribut Inspector")](outline-view-images/edit08.png#lightbox)

Budete mít všechny vlastnosti standardní textové pole pro nastavení sem. Ve výchozím nastavení standardní textové pole slouží k zobrazení dat pro buňku ve sloupci.

Vyberte zobrazení buněk tabulky (`NSTableFieldCell`) v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](outline-view-images/edit09.png "Atribut Inspector")](outline-view-images/edit09.png#lightbox)

Jsou zde nejdůležitější nastavení:

- **Rozložení** – vyberte, jak jsou rozloženy buňky v tomto sloupci.
- **Používá jeden řádek režim** – Pokud `true`, buňky je omezený na jeden řádek.
- **První modul Runtime rozložení šířka** – Pokud `true`, buňky bude raději šířku nastavit pro něj (ručně nebo automaticky) Pokud se zobrazí při prvním spuštění aplikace.
- **Akce** -Určuje, kdy upravit **akce** jsou odeslány buňky.
- **Chování** -Určuje, zda je buňku vybrat ani upravit.
- **Formátovaný Text** – Pokud `true`, buňky můžete zobrazit formátovaný a stylem text.
- **Vrátit zpět** – Pokud `true`, buňky odpovědnost pro je vrátit zpět chování.

Vyberte zobrazení buněk tabulky (`NSTableFieldCell`) v dolní části sloupec tabulky v **rozhraní hierarchie**:

[![](outline-view-images/edit11.png "Výběr zobrazení buněk tabulky")](outline-view-images/edit10.png#lightbox)

To umožňuje upravit tabulce buňku zobrazení použít jako základní _vzor_ pro všechny buňky vytvořené pro daný sloupec.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Přidání akce a výstupy

Stejně jako všechny ostatní kakao kontrolní mechanismus uživatelského rozhraní, musíme vystavit naše zobrazení osnovy a je sloupce a buňky pro C# kódu pomocí **akce** a **výstupy** (založené na funkci požadovanou).

Proces je stejný pro libovolný element zobrazení osnovy, který chcete vystavit:

1. Přepnout **pomocníka Editor** a ujistěte se, že `ViewController.h` vybraný soubor: 

    [![](outline-view-images/edit11.png "Výběr souboru správné h")](outline-view-images/edit11.png#lightbox)
2. Vyberte zobrazení osnovy z **rozhraní hierarchie**, řízení kliknutím a tažením `ViewController.h` souboru.
3. Vytvoření **výstupu** pro zobrazení osnovy názvem `ProductOutline`: 

    [![](outline-view-images/edit13.png "Konfigurace výstupu")](outline-view-images/edit13.png#lightbox)
4. Vytvoření **výstupy** u sloupce tabulky se také označuje `ProductColumn` a `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Konfigurace výstupu")](outline-view-images/edit14.png#lightbox)
5. Můžete uložit změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Dále jsme budete napište zobrazení kódu některá data pro obrys při spuštění aplikace.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Naplnění zobrazení osnovy

S naše zobrazení osnovy určená v Tvůrci rozhraní a zveřejňovány prostřednictvím **výstupu**, další musíme vytvořit kód C# k jejímu naplnění.

Nejdříve vytvoříme nový `Product` třída pro uložení informací pro jednotlivé řádky a skupiny dílčí produktů. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `Product` pro **název** a klikněte na tlačítko **nový** tlačítko:

[![](outline-view-images/populate01.png "Vytvoření prázdné třídy")](outline-view-images/populate01.png#lightbox)

Ujistěte se, `Product.cs` soubor vypadá takto:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}
```

Dále je nutné vytvořit podtřídou třídy `NSOutlineDataSource` zajistit data pro naše outline podle požadavků. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `ProductOutlineDataSource` pro **název** a klikněte na tlačítko **nový** tlačítko.

Upravit `ProductTableDataSource.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }
                
        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }
        
        }
        #endregion
    }
}
```

Tato třída má úložiště pro naše zobrazení osnovy položky a přepíše `GetChildrenCount` vrátit počet řádků v tabulce. `GetChild` Vrátí konkrétní nadřazenou nebo podřízenou položku (jak vyžádala zobrazení osnovy) a `ItemExpandable` definuje zadanou položku jako nadřazená nebo podřízená.

Nakonec je potřeba vytvořit podtřídou třídy `NSOutlineDelegate` zajistit chování pro naše outline. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `ProductOutlineDelegate` pro **název** a klikněte na tlačítko **nový** tlačítko.

Upravit `ProductOutlineDelegate.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Když vytvoříme instanci `ProductOutlineDelegate`, jsme předat také v instanci systému `ProductOutlineDataSource` poskytující data pro obrys. `GetView` Metoda je odpovědná za vrácení zobrazení (data) k zobrazení buňky pro udělení sloupce a řádku. Pokud je to možné existující zobrazení bude znovu použita k zobrazení buňky, není-li musí být vytvořeny nové zobrazení.

K naplnění obrys, Pojďme upravit `MainWindow.cs` souboru a nastavit `AwakeFromNib` metoda vypadá takto:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Pokud jsme aplikaci spustit, se zobrazí následující:

[![](outline-view-images/populate02.png "Sbaleným zobrazením")](outline-view-images/populate02.png#lightbox)

Pokud jsme rozbalte uzel v zobrazení osnovy, bude vypadat takto:

[![](outline-view-images/populate03.png "Rozšířené zobrazení")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Řazení podle sloupce

Umožňuje povolit uživateli řazení dat v osnově kliknutím na záhlaví sloupce. První, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte `Product` sloupce, zadejte `Title` pro **klíč řazení**, `compare:` pro **selektor** a vyberte `Ascending` pro **pořadí**:

[![](outline-view-images/sort01.png "Nastavení klíče pořadí řazení")](outline-view-images/sort01.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Nyní Pojďme upravit `ProductOutlineDataSource.cs` souboru a přidejte následující metody:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

`Sort` Metoda nám řazení dat ve zdroji dat na základě umožňují danou `Product` pole třídy ve vzestupném nebo sestupném pořadí. Přepsané `SortDescriptorsChanged` volání metody pokaždé, když použití klikne na záhlaví sloupce. Bude předáno **klíč** hodnotu, která nastaví Tvůrce rozhraní a řazení pro daný sloupec.

Pokud jsme spusťte aplikaci a klikněte na záhlaví sloupců, podle tohoto sloupce seřadí řádky:

[![](outline-view-images/sort02.png "Příklad seřazené výstupu")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Výběr řádku

Pokud chcete povolit uživateli vybrat jeden řádek, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení osnovy v **rozhraní hierarchie** a zrušte zaškrtnutí políčka **více** zaškrtnout políčko **atribut Inspector**:

[![](outline-view-images/select01.png "Atribut Inspector")](outline-view-images/select01.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.


Potom upravte `ProductOutlineDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

To vám umožní uživateli vybrat všechny jeden řádek v zobrazení osnovy. Vrátí `false` pro `ShouldSelectItem` pro všechny položky, které nechcete, aby uživatel moct vybrat nebo `false` pro každou položku, pokud nechcete, aby uživatel moct vybrat žádné položky.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Vícenásobný výběr řádku

Pokud chcete povolit uživateli vybrat více řádků, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení osnovy v **rozhraní hierarchie** a zkontrolujte **více** zaškrtnout políčko **atribut Inspector**:

[![](outline-view-images/select02.png "Atribut Inspector")](outline-view-images/select02.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.


Potom upravte `ProductOutlineDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

To vám umožní uživateli vybrat všechny jeden řádek v zobrazení osnovy. Vrátí `false` pro `ShouldSelectRow` pro všechny položky, které nechcete, aby uživatel moct vybrat nebo `false` pro každou položku, pokud nechcete, aby uživatel moct vybrat žádné položky.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ, který má vyberte řádek

Pokud chcete povolit uživatelům zadejte znak s zobrazení osnovy vybrané a vyberte první řádek, který má tento znak, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení osnovy v **rozhraní hierarchie** a zkontrolujte **vyberte typ** zaškrtnout políčko **atribut Inspector**:

[![](outline-view-images/type01.png "Úpravy typ řádku typu")](outline-view-images/type01.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Nyní Pojďme upravit `ProductOutlineDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

`GetNextTypeSelectMatch` Metoda trvá v dané `searchString` a vrátí položku první `Product` , který má tento řetězec v jeho `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Změna řazení sloupců

Pokud chcete povolit uživatelům přetáhněte změnit pořadí sloupců v zobrazení osnovy, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení osnovy v **rozhraní hierarchie** a zkontrolujte **Reordering** zaškrtnout políčko **atribut Inspector**:

[![](outline-view-images/reorder01.png "Atribut Inspector")](outline-view-images/reorder01.png#lightbox)

Pokud nás dostanete hodnotu **automatické ukládání** vlastnost a kontrola **informace o sloupci** pole, všechny změny, provedeme rozložení tabulky budou automaticky uloženy pro nás a obnovit dalším spuštění aplikace je spuštěn.

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Nyní Pojďme upravit `ProductOutlineDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Metoda by měla vrátit `true` pro všechny sloupce, který ho chcete povolit být přeuspořádány přetáhněte do `newColumnIndex`, jinak vrátí `false`;

Pokud jsme aplikaci spustit, jsme přetáhněte záhlaví sloupců kolem Změna uspořádání naše sloupců:

[![](outline-view-images/reorder02.png "Příklad způsob sloupců")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Úpravy buněk

Pokud chcete povolit uživatelům upravit hodnoty pro dané buňky, upravit `ProductOutlineDelegate.cs` soubor a změňte `GetViewForItem` metoda následujícím způsobem:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Nyní když jsme aplikaci spustit, můžete upravit uživatele buněk v tabulce zobrazení:

[![](outline-view-images/editing01.png "Příklad úpravy buněk")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Použití obrázků v zobrazení osnovy

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

Další informace najdete v tématu [pomocí bitové kopie s zobrazení osnovy](~/mac/app-fundamentals/image.md) části našich [práce s bitovou kopií](~/mac/app-fundamentals/image.md) dokumentaci.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Zobrazení osnovy vazba dat

Pomocí technik klíč-hodnota kódování a datové vazby v aplikaci Xamarin.Mac může výrazně snížit množství kód, který máte k zápisu a udržovat naplnit a pracovat s prvky uživatelského rozhraní. Máte také výhodou další oddělení zálohování dat (_datový Model_) z vaší přední ukončení uživatelské rozhraní (_Model-View-Controller_), tečka na začátku k snadněji provádět údržbu, flexibilnější aplikace návrh.

Klíč-hodnota kódování (KVC) je mechanismus pro přístup k vlastnostem objektu nepřímo, použití klíče (speciálně formátované řetězce) k identifikaci vlastnosti místo k nim přistupovat pomocí instance proměnné nebo metody přístupových objektů (`get/set`). Implementací klíč-hodnota kódování kompatibilní přístupových objektů v aplikaci Xamarin.Mac získáte přístup k jiné funkce systému macOS například klíč-hodnota sledování (KVO), vazby dat, základní Data, kakao vazby a scriptability.

Další informace najdete v tématu [Outline zobrazení datové vazby](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) části našich [datové vazby a klíč-hodnota kódování](~/mac/app-fundamentals/databinding.md) dokumentaci.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce se zobrazeními obrysu v aplikaci Xamarin.Mac trvá. Jsme viděli různé typy a používá zobrazení osnovy, jak vytvořit a udržovat zobrazení osnovy v Xcode na rozhraní tvůrce a jak pracovat s zobrazení osnovy v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacOutlines (ukázka)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (ukázka)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Seznamy zdrojů](~/mac/user-interface/source-list.md)
- [Vazba dat a kódování párů klíč-hodnota](~/mac/app-fundamentals/databinding.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do zobrazení osnovy](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
