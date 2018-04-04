---
title: Zobrazení tabulek
description: Tento článek se zabývá práci s zobrazení tabulek v aplikaci Xamarin.Mac. Popisuje vytváření zobrazení tabulek v Xcode a Tvůrce rozhraní a interakci s nimi v kódu.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c274405613f079cb61ad9c96497a9effdc7173f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="table-views"></a>Zobrazení tabulek

_Tento článek se zabývá práci s zobrazení tabulek v aplikaci Xamarin.Mac. Popisuje vytváření zobrazení tabulek v Xcode a Tvůrce rozhraní a interakci s nimi v kódu._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejné tabulky zobrazení, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat zobrazení tabulky (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Zobrazení tabulky zobrazí data v tabulkovém formátu, který obsahuje jeden nebo více sloupců informací ve více řádků. Na základě typu zobrazení tabulky vytváří, uživatel může seřadit podle sloupce, reorganizovat sloupců, přidat sloupce, sloupce odebrat nebo upravit data obsažená v tabulce.

[![](table-view-images/intro01.png "Příklad tabulky")](table-view-images/intro01.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s zobrazení tabulek v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Úvod do zobrazení tabulek

Zobrazení tabulky zobrazí data v tabulkovém formátu, který obsahuje jeden nebo více sloupců informací ve více řádků. Zobrazení tabulek se zobrazí uvnitř zobrazení Scroll (`NSScrollView`) a od verze systému macOS 10.7, můžete použít libovolnou `NSView` místo buněk (`NSCell`) k zobrazení řádků a sloupců. Ale nutné dodat, můžete dál používat `NSCell` však budete obvykle podtřídami `NSTableCellView` a vytvářet vlastní řádků a sloupců.

Zobrazení tabulky neukládá data vlastní, místo toho používá zdroje dat (`NSTableViewDataSource`) k poskytování řádků i sloupců, které jsou potřeba, na základě podle potřeby.

Zobrazení tabulky chování lze přizpůsobit zadáním podtřídou třídy delegáta zobrazení tabulky (`NSTableViewDelegate`) pro podporu správy sloupce tabulky, zadejte vyberte funkce, výběr řádků a úpravy, vlastní sledování a vlastní zobrazení pro jednotlivé sloupce a řádky.

Při vytváření zobrazení tabulek, Apple navrhuje následující:

* Povolí uživateli v tabulce můžete seřadit kliknutím na záhlaví sloupců.
* Vytvoření záhlaví sloupců, které jsou podstatná jména či fráze krátké podstatné jméno, které popisují dat zobrazených v tomto sloupci.

Další informace najdete v tématu [obsahu zobrazení](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Vytvoření a údržbu zobrazení tabulek v Xcode

Když vytvoříte novou aplikaci Xamarin.Mac kakao, zobrazí okno Standardní prázdné, ve výchozím nastavení. Toto systému windows je definována v `.storyboard` automaticky zahrnutý v projektu. Chcete-li upravit návrh vašeho systému windows v **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souboru:

[![](table-view-images/edit01.png "Výběr hlavní storyboard")](table-view-images/edit01.png#lightbox)

Otevře se okno návrhu v Xcode na rozhraní Tvůrce:

[![](table-view-images/edit02.png "Úpravy uživatelského rozhraní v Xcode")](table-view-images/edit02.png#lightbox)

Typ `table` do **knihovny Inspector** vyhledávací pole, aby bylo snazší najít ovládací prvky zobrazení tabulky:

[![](table-view-images/edit03.png "Výběr zobrazení tabulky z knihovny")](table-view-images/edit03.png#lightbox)

Přetažením na řadiče zobrazení v zobrazení tabulky **rozhraní editoru**, bylo vyplnil celou oblast obsahu řadiče zobrazení a nastavte ji na kde zmenšuje a roste s okno v **Editor omezení**:

[![](table-view-images/edit04.png "Úpravy omezení")](table-view-images/edit04.png#lightbox)

Vyberte zobrazení tabulky v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](table-view-images/edit05.png "Atribut Inspector")](table-view-images/edit05.png#lightbox)

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
> Pokud udržujete starší verze aplikace Xamarin.Mac `NSView` na základě zobrazení tabulek slouží přes `NSCell` na základě zobrazení tabulek. `NSCell` je považován za starší verze a nemusí být podporována do budoucna.

Vyberte sloupec tabulky v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](table-view-images/edit06.png "Atribut Inspector")](table-view-images/edit06.png#lightbox)

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

[![](table-view-images/edit07.png "Atribut Inspector")](table-view-images/edit07.png#lightbox)

Toto jsou všechny vlastnosti standardní zobrazení. Máte také možnost změny velikosti řádků pro tento sloupec sem.

Vyberte zobrazení buňky tabulky (ve výchozím nastavení je to `NSTextField`) v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](table-view-images/edit08.png "Atribut Inspector")](table-view-images/edit08.png#lightbox)

Budete mít všechny vlastnosti standardního textového pole pro nastavení sem. Ve výchozím nastavení standardní textové pole slouží k zobrazení dat pro buňku ve sloupci.

Vyberte zobrazení buněk tabulky (`NSTableFieldCell`) v **rozhraní hierarchie** a následující vlastnosti jsou k dispozici v **atribut Inspector**:

[![](table-view-images/edit09.png "Atribut Inspector")](table-view-images/edit09.png#lightbox)

Jsou zde nejdůležitější nastavení:

- **Rozložení** – vyberte, jak jsou rozloženy buňky v tomto sloupci.
- **Používá jeden řádek režim** – Pokud `true`, buňky je omezený na jeden řádek.
- **První modul Runtime rozložení šířka** – Pokud `true`, buňky bude raději šířku nastavit pro něj (ručně nebo automaticky) Pokud se zobrazí při prvním spuštění aplikace.
- **Akce** -Určuje, kdy upravit **akce** jsou odeslány buňky.
- **Chování** -Určuje, zda je buňku vybrat ani upravit.
- **Formátovaný Text** – Pokud `true`, buňky můžete zobrazit formátovaný a stylem text.
- **Vrátit zpět** – Pokud `true`, buňky odpovědnost pro je vrátit zpět chování.

Vyberte zobrazení buněk tabulky (`NSTableFieldCell`) v dolní části sloupec tabulky v **rozhraní hierarchie**:

[![](table-view-images/edit10.png "Výběr zobrazení buněk tabulky")](table-view-images/edit10.png#lightbox)

To umožňuje upravit tabulce buňku zobrazení použít jako základní _vzor_ pro všechny buňky vytvořené pro daný sloupec.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Přidání akce a výstupy

Stejně jako další řízení kakao uživatelského rozhraní budeme muset vystavit naše zobrazení tabulky a je sloupce a buňky pro C# kódu pomocí **akce** a **výstupy** (založené na funkci požadovanou).

Proces je stejný pro libovolný element zobrazení tabulky, který chcete vystavit:

1. Přepnout **pomocníka Editor** a ujistěte se, že `ViewController.h` vybraný soubor: 

    [![](table-view-images/edit11.png "Pomocník pro Editor")](table-view-images/edit11.png#lightbox)
2. Vyberte zobrazení tabulky z **rozhraní hierarchie**, řízení kliknutím a tažením `ViewController.h` souboru.
3. Vytvoření **výstupu** pro zobrazení tabulky názvem `ProductTable`: 

    [![](table-view-images/edit13.png "Konfigurace výstupu")](table-view-images/edit13.png#lightbox)
4. Vytvoření **výstupy** u sloupce tabulky se také označuje `ProductColumn` a `DetailsColumn`: 

    [![](table-view-images/edit14.png "Konfigurace výstupu")](table-view-images/edit14.png#lightbox)
5. Můžete uložit změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Dále jsme budete napište zobrazení kódu některá data pro tabulku při spuštění aplikace.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Naplnění tabulky zobrazení

S naše tabulka zobrazení určená v Tvůrci rozhraní a zveřejňovány prostřednictvím **výstupu**, další musíme vytvořit kód C# k jejímu naplnění.

Nejdříve vytvoříme nový `Product` třída pro uložení informací pro jednotlivé řádky. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `Product` pro **název** a klikněte na tlačítko **nový** tlačítko:

[![](table-view-images/populate01.png "Vytvoření prázdné třídy")](table-view-images/populate01.png#lightbox)

Ujistěte se, `Product.cs` soubor vypadá takto:

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
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

Dále je nutné vytvořit podtřídou třídy `NSTableDataSource` poskytovat data pro naše tabulku podle požadavků. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `ProductTableDataSource` pro **název** a klikněte na tlačítko **nový** tlačítko.

Upravit `ProductTableDataSource.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

Tato třída má úložiště pro naše tabulku zobrazit položky a přepíše `GetRowCount` vrátit počet řádků v tabulce.

Nakonec je potřeba vytvořit podtřídou třídy `NSTableDelegate` zajistit chování pro naše tabulky. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `ProductTableDelegate` pro **název** a klikněte na tlačítko **nový** tlačítko.

Upravit `ProductTableDelegate.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Když vytvoříme instanci `ProductTableDelegate`, jsme předat také v instanci systému `ProductTableDataSource` poskytující data pro tabulku. `GetViewForItem` Metoda je odpovědná za vrácení zobrazení (data) k zobrazení buňky pro udělení sloupce a řádku. Pokud je to možné existující zobrazení bude znovu použita k zobrazení buňky, není-li musí být vytvořeny nové zobrazení.

K vyplnění tabulky, Pojďme upravit `ViewController.cs` souboru a nastavit `AwakeFromNib` metoda vypadá takto:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

Pokud jsme aplikaci spustit, se zobrazí následující:

[![](table-view-images/populate02.png "Spuštění ukázkové aplikace")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Řazení podle sloupce

Umožňuje povolit uživateli řazení dat v tabulce kliknutím na záhlaví sloupce. První, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte `Product` sloupce, zadejte `Title` pro **klíč řazení**, `compare:` pro **selektor** a vyberte `Ascending` pro **pořadí**:

[![](table-view-images/sort01.png "Nastavení klíč řazení")](table-view-images/sort01.png#lightbox)

Vyberte `Details` sloupce, zadejte `Description` pro **klíč řazení**, `compare:` pro **selektor** a vyberte `Ascending` pro **pořadí**:

[![](table-view-images/sort02.png "Nastavení klíč řazení")](table-view-images/sort02.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Nyní Pojďme upravit `ProductTableDataSource.cs` souboru a přidejte následující metody:

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
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

`Sort` Metoda nám řazení dat ve zdroji dat na základě umožňují danou `Product` pole třídy ve vzestupném nebo sestupném pořadí. Přepsané `SortDescriptorsChanged` volání metody pokaždé, když použití klikne na záhlaví sloupce. Bude předáno **klíč** hodnotu, která nastaví Tvůrce rozhraní a řazení pro daný sloupec.

Pokud jsme spusťte aplikaci a klikněte na záhlaví sloupců, podle tohoto sloupce seřadí řádky:

[![](table-view-images/sort03.png "Příklad aplikace spustit")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Výběr řádku

Pokud chcete povolit uživateli vybrat jeden řádek, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení tabulky v **rozhraní hierarchie** a zrušte zaškrtnutí políčka **více** zaškrtnout políčko **atribut Inspector**:

[![](table-view-images/select01.png "Atribut Inspector")](table-view-images/select01.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.


Potom upravte `ProductTableDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

To vám umožní uživateli vybrat všechny jeden řádek v tabulce zobrazení. Vrátí `false` pro `ShouldSelectRow` pro žádný řádek, který nechcete, aby uživatel moct vybrat nebo `false` pro každý řádek, pokud nechcete, aby uživatel moct vybrat žádné řádky.

Zobrazení tabulky (`NSTableView`) obsahuje následující metody pro práci s výběr řádků:

- `DeselectRow(nint)` -Zruší výběr daný řádek v tabulce.
- `SelectRow(nint,bool)` -Vybere daný řádek. Předat `false` pro druhý parametr vybrat pouze jeden řádek v čase.
- `SelectedRow` -Vrátí aktuální řádek vybraný v tabulce.
- `IsRowSelected(nint)` -Vrátí `true` Pokud je vybrán daný řádek.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Vícenásobný výběr řádku

Pokud chcete povolit uživateli vybrat více řádků, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení tabulky v **rozhraní hierarchie** a zkontrolujte **více** zaškrtnout políčko **atribut Inspector**:

[![](table-view-images/select02.png "Atribut Inspector")](table-view-images/select02.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.


Potom upravte `ProductTableDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

To vám umožní uživateli vybrat všechny jeden řádek v tabulce zobrazení. Vrátí `false` pro `ShouldSelectRow` pro žádný řádek, který nechcete, aby uživatel moct vybrat nebo `false` pro každý řádek, pokud nechcete, aby uživatel moct vybrat žádné řádky.

Zobrazení tabulky (`NSTableView`) obsahuje následující metody pro práci s výběr řádků:

- `DeselectAll(NSObject)` -Zruší výběr všechny řádky v tabulce. Použití `this` pro první parametr odeslat v provádění výběr objektu. 
- `DeselectRow(nint)` -Zruší výběr daný řádek v tabulce.
- `SelectAll(NSobject)` -Vybere všechny řádky v tabulce. Použití `this` pro první parametr odeslat v provádění výběr objektu.
- `SelectRow(nint,bool)` -Vybere daný řádek. Předat `false` pro druhý parametr zrušte zaškrtnutí políčka a vyberte pouze jeden řádek, předat `true` rozšíření výběru a musí zahrnovat tento řádek.
- `SelectRows(NSIndexSet,bool)` -Vybere danou sadu řádků. Předat `false` pro druhý parametr zrušte zaškrtnutí políčka a vyberte pouze tyto řádky, předat `true` rozšířit výběr a zahrnout tyto řádky.
- `SelectedRow` -Vrátí aktuální řádek vybraný v tabulce.
- `SelectedRows` -Vrátí `NSIndexSet` obsahující indexy vybrané řádky.
- `SelectedRowCount` -Vrátí počet vybraných řádků.
- `IsRowSelected(nint)` -Vrátí `true` Pokud je vybrán daný řádek.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ, který má vyberte řádek

Pokud chcete povolit uživatelům zadejte znak s Tabulka zobrazení vybrané a vyberte první řádek, který má tento znak, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení tabulky v **rozhraní hierarchie** a zkontrolujte **vyberte typ** zaškrtnout políčko **atribut Inspector**:

[![](table-view-images/type01.png "Nastavení výběru typu")](table-view-images/type01.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Nyní Pojďme upravit `ProductTableDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

`GetNextTypeSelectMatch` Metoda trvá v dané `searchString` a vrátí řádek první `Product` , který má tento řetězec v jeho `Title`.

Pokud jsme aplikaci spustit a zadejte znak, je vybrán řádek:

[![](table-view-images/type02.png "Spuštění ukázkové aplikace")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Změna řazení sloupců

Pokud chcete povolit uživatelům přetáhněte změnit pořadí sloupců v tabulce zobrazení, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní. Vyberte zobrazení tabulky v **rozhraní hierarchie** a zkontrolujte **Reordering** zaškrtnout políčko **atribut Inspector**:

[![](table-view-images/reorder01.png "Atribut Inspector")](table-view-images/reorder01.png#lightbox)

Pokud nás dostanete hodnotu **automatické ukládání** vlastnost a kontrola **informace o sloupci** pole, všechny změny, provedeme rozložení tabulky budou automaticky uloženy pro nás a obnovit dalším spuštění aplikace je spuštěn.

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Nyní Pojďme upravit `ProductTableDelegate.cs` souboru a přidejte následující metodu:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Metoda by měla vrátit `true` pro všechny sloupce, který ho chcete povolit být přeuspořádány přetáhněte do `newColumnIndex`, jinak vrátí `false`;

Pokud jsme aplikaci spustit, jsme přetáhněte záhlaví sloupců kolem Změna uspořádání naše sloupců:

[![](table-view-images/reorder02.png "Příklad uspořádaný sloupců")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Úpravy buněk

Pokud chcete povolit uživatelům upravit hodnoty pro dané buňky, upravit `ProductTableDelegate.cs` soubor a změňte `GetViewForItem` metoda následujícím způsobem:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Nyní když jsme aplikaci spustit, můžete upravit uživatele buněk v tabulce zobrazení:

[![](table-view-images/editing01.png "Příklad úpravy buňku")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Pomocí bitové kopie v tabulkovém zobrazení

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

Další informace najdete v tématu [pomocí bitové kopie s zobrazení tabulek](~/mac/app-fundamentals/image.md) části našich [práce s bitovou kopií](~/mac/app-fundamentals/image.md) dokumentaci.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Přidání tlačítka Odstranit řádek

V závislosti na požadavcích vaší aplikace, mohou nastat situace kde budete muset zadat tlačítka akce pro každý řádek v tabulce. Jako příklad toho, můžeme rozbalte položku v tabulce zobrazení příkladu vytvořili výše, které pokud chcete zahrnout **odstranit** tlačítko na každém řádku.

Nejprve upravit `Main.storyboard` v Xcode na rozhraní tvůrce, vyberte zobrazení tabulky a zvýšit počet sloupců tří (3). V dalším kroku změnit **název** nové sloupce, který se `Action`:

[![](table-view-images/delete01.png "Úprava názvu sloupce")](table-view-images/delete01.png#lightbox)

Uložte změny do scénáře a vrátit k sadě Visual Studio pro Mac, aby synchronizovat změny.

Potom upravte `ViewController.cs` souboru a přidejte následující metodu veřejný:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Ve stejném souboru, upravte vytvoření nové tabulky zobrazení delegáta uvnitř `ViewDidLoad` metoda následujícím způsobem:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Nyní, upravit `ProductTableDelegate.cs` souboru zahrnout privátní připojení k řadiči zobrazení a provést řadičem jako parametr při vytvoření nové instance delegáta:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

V dalším kroku přidejte následující nové privátní metodu do třídy:

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

Tato akce trvá všechny konfigurace textového zobrazení, které byly dříve prováděná v `GetViewForItem` metoda a umístí je do jedné, kterou lze volat umístění (protože poslední sloupec tabulky nezahrnuje textového zobrazení, ale tlačítko).

Nakonec upravit `GetViewForItem` metodu a nastavit jej vypadat třeba takto:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btw = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Podívejme se na několika oddílů tohoto kódu podrobněji. První, pokud se nový `NSTableViewCell` je na základě vytváří akce je provedena na název sloupce. Pro první dva sloupce (**produktu** a **podrobnosti**), nový `ConfigureTextField` metoda je volána.

Pro **akce** sloupec, nový `NSButton` je vytvořen a přidán do buňky jako dílčí zobrazení:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Na tlačítko `Tag` vlastnost se používá k uložení všech na řádek, který se právě zpracovává. Toto číslo použít později, až uživatel požádá řádek k odstranění na tlačítku `Activated` událostí:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

Na začátku obslužné rutiny události se nám získat tlačítko a produkt, který je na daném řádku. Pak se zobrazí výstraha, uživateli potvrzení odstranění řádku. Pokud uživatel vybere možnost odstranit řádek, je znovu načíst tabulky a daný řádek je odebrána ze zdroje dat:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Navíc pokud buňku zobrazení tabulky je opakovaně používáno místo vytváří nový, následující kód konfiguruje podle sloupce zpracovává:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Pro **akce** sloupce, dokud se prohledávají všech dílčí zobrazení `NSButton` nenajde, pak je `Tag` vlastnost je aktualizovat tak, aby odkazoval na aktuálním řádku.

Tyto změny zavedené při spuštění aplikace každý řádek bude mít **odstranit** tlačítko:

[![](table-view-images/delete02.png "Zobrazení tabulky s odstranění tlačítka")](table-view-images/delete02.png#lightbox)

Když uživatel klikne **odstranit** tlačítko se zobrazí výstraha s žádostí o odstranění daného řádku:

[![](table-view-images/delete03.png "Upozornění na Odstranit řádek")](table-view-images/delete03.png#lightbox)

Pokud se uživatel rozhodne odstranit, se odeberou řádek a v tabulce bude překreslen.:

[![](table-view-images/delete04.png "Po odstranění řádek tabulky")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Zobrazení tabulky vazba dat

Pomocí technik klíč-hodnota kódování a datové vazby v aplikaci Xamarin.Mac může výrazně snížit množství kód, který máte k zápisu a udržovat naplnit a pracovat s prvky uživatelského rozhraní. Máte také výhodou další oddělení zálohování dat (_datový Model_) z vaší přední ukončení uživatelské rozhraní (_Model-View-Controller_), tečka na začátku k snadněji provádět údržbu, flexibilnější aplikace návrh.

Klíč-hodnota kódování (KVC) je mechanismus pro přístup k vlastnostem objektu nepřímo, použití klíče (speciálně formátované řetězce) k identifikaci vlastnosti místo k nim přistupovat pomocí instance proměnné nebo metody přístupových objektů (`get/set`). Implementací klíč-hodnota kódování kompatibilní přístupových objektů v aplikaci Xamarin.Mac získáte přístup k jiné funkce systému macOS například klíč-hodnota sledování (KVO), vazby dat, základní Data, kakao vazby a scriptability.

Další informace najdete v tématu [tabulky zobrazení datové vazby](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) části našich [datové vazby a klíč-hodnota kódování](~/mac/app-fundamentals/databinding.md) dokumentaci.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce se zobrazeními tabulku v aplikaci Xamarin.Mac trvá. Jsme viděli různé typy a používá zobrazení tabulek, jak vytvořit a udržovat zobrazení tabulek v Tvůrci rozhraní na Xcode a jak pracovat s zobrazení tabulek v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacTables (ukázka)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (ukázka)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Zobrazení osnov](~/mac/user-interface/outline-view.md)
- [Seznamy zdrojů](~/mac/user-interface/source-list.md)
- [Vazba dat a kódování párů klíč-hodnota](~/mac/app-fundamentals/databinding.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
