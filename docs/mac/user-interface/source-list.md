---
title: Zdroj seznamy
description: "Tento článek se zabývá práce se seznamy zdroje v Xamarin.Mac aplikace. Popisuje vytvoření a udržování seznamy zdroje v Xcode a Tvůrce rozhraní a interakci s nimi v kódu jazyka C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1cc74fb30e59ecd5f6be3cf3e1c84f60cd5ca0a6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="source-lists"></a>Zdroj seznamy

_Tento článek se zabývá práce se seznamy zdroje v Xamarin.Mac aplikace. Popisuje vytvoření a udržování seznamy zdroje v Xcode a Tvůrce rozhraní a interakci s nimi v kódu jazyka C#._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejného zdroje seznamy, které vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat, jsou uvedené zdroje (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Seznam zdrojů je zvláštní druh zobrazení osnovy používá k zobrazení, zdroj akce, jako je na straně panelu v vyhledávací nebo iTunes.

[ ![](source-list-images/source05.png "Seznam zdrojů příklad")](source-list-images/source05.png)

V tomto článku vám nabídneme základní informace o práci s uvádí zdroje v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Úvod do seznamů zdroje

Jak jsme uvedli výše, seznam zdrojů je že zvláštní druh zobrazení osnovy umožňuje zobrazit zdroj akce, jako je na straně panelu v vyhledávací nebo iTunes. Seznam zdrojů je typ tabulky, který uživateli umožňuje rozbalit nebo sbalit řádky hierarchické data. Na rozdíl od zobrazení tabulky položky v seznamu zdroj nejsou jako plochý seznam, jsou uspořádány do hierarchie, jako jsou soubory a složky na pevném disku. Pokud položku v seznamu zdroj obsahuje další položky, můžete rozbalit nebo sbalit uživatelem.

Seznam zdrojů je speciálně stylem zobrazení osnovy (`NSOutlineView`), které je podtřídou třídy tabulka zobrazení (`NSTableView`) a jako takový většinu své chování dědí ze své nadřazené třídy. Výsledkem je mnoho činností nepodporuje zobrazení osnovy, jsou podporovány také zdrojového seznamu. Aplikace Xamarin.Mac má kontrolu nad tyto funkce a můžete nakonfigurovat parametry zdrojového seznamu, (buď v kódu nebo rozhraní tvůrce) k povolí nebo zakáže určité operace.

Seznam zdrojů neukládá data vlastní, místo toho používá zdroje dat (`NSOutlineViewDataSource`) k poskytování řádků i sloupců, které jsou potřeba, na základě podle potřeby.

Seznam zdrojů chování lze přizpůsobit zadáním podtřídou třídy delegáta zobrazení osnovy (`NSOutlineViewDelegate`) pro podporu Outline typ vyberte funkce, položky Výběr a úpravy, vlastní sledování a vlastní zobrazení pro jednotlivé položky.

Vzhledem k tomu, že seznam zdrojů většinu funkcí a její chování sdílí s zobrazení tabulky a zobrazení osnovy, můžete chtít projít naše [zobrazení tabulek](~/mac/user-interface/table-view.md) a [zobrazení osnovy](~/mac/user-interface/outline-view.md) dokumentace před pokračováním s tímto článkem.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Práce s zdroj seznamy

Seznam zdrojů je zvláštní druh zobrazení osnovy používá k zobrazení, zdroj akce, jako je na straně panelu v vyhledávací nebo iTunes. Na rozdíl od zobrazení osnovy před jsme definovali naše zdrojového seznamu v Tvůrci rozhraní, vytvoříme základní třídy v Xamarin.Mac.

Nejdříve vytvoříme nový `SourceListItem` třída pro uložení dat pro naše zdrojového seznamu. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `SourceListItem` pro **název** a klikněte na tlačítko **nový** tlačítko:

[ ![](source-list-images/source01.png "Přidání prázdný – třída")](source-list-images/source01.png)

Ujistěte se, `SourceListItem.cs` soubor vypadá takto: 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `SourceListDataSource` pro **název** a klikněte na tlačítko **nový** tlačítko. Ujistěte se, `SourceListDataSource.cs` soubor vypadá takto:

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

To zajistí data pro naše zdrojového seznamu.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `SourceListDelegate` pro **název** a klikněte na tlačítko **nový** tlačítko. Ujistěte se, `SourceListDelegate.cs` soubor vypadá takto:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

To zajistí chování naše zdrojového seznamu.

Nakonec v **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** Vyberte **Obecné** > **prázdné třídy**, zadejte `SourceListView` pro **název** a klikněte na tlačítko **nový** tlačítko. Ujistěte se, `SourceListView.cs` soubor vypadá takto:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Tím se vytvoří vlastní, opakovaně použitelné podtřídou třídy `NSOutlineView` (`SourceListView`), jsme můžete použít k řízení zdrojového seznamu v libovolné aplikaci Xamarin.Mac, který jsme.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Vytvoření a údržbu seznamy zdroje v Xcode

Teď umožňuje návrh naše seznam zdrojů v Tvůrci rozhraní. Dvakrát klikněte na `Main.storyboard` soubor otevřete pro úpravy v Tvůrci rozhraní a přetáhněte ji rozdělení zobrazení z **knihovny Inspector**, přidejte ji do řadiče zobrazení a nastavte ji ke změně velikosti s zobrazení v **omezení editoru** :

[ ![](source-list-images/source00.png "Úpravy omezení")](source-list-images/source00.png)

V dalším kroku přetáhněte seznam zdrojů z **knihovny Inspector**, přidejte ho na levé straně zobrazení rozdělení a nastavte ji ke změně velikosti s zobrazení v **omezení Editor**:

[ ![](source-list-images/source02.png "Úpravy omezení")](source-list-images/source02.png)

Potom přepnout **zobrazení Identity**vyberte zdrojového seznamu a změňte ji na **– třída** k `SourceListView`:

[ ![](source-list-images/source03.png "Nastavení názvu – třída")](source-list-images/source03.png)

Nakonec vytvořte **výstupu** pro naše zdrojového seznamu názvem `SourceList` v `ViewController.h` souboru:

[ ![](source-list-images/source04.png "Konfigurace výstupu")](source-list-images/source04.png)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Naplnění seznamu zdrojů

Umožňuje upravit `RotationWindow.cs` souborů v sadě Visual Studio pro Mac a nastavte jej na `AwakeFromNib` metoda vypadá takto:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

`Initialize ()` Metoda muset volat před naše zdrojového seznamu **výstupu** _před_ všechny položky budou přidány do ní. Pro každou skupinu položek jsme vytvořit nadřazenou položku a pak přidejte do této skupiny položky dílčí položky. Každé skupiny se pak přidá do seznamu zdrojů kolekce `SourceList.AddItem (...)`. Poslední dva řádky načíst data pro zdrojového seznamu a rozbalí všechny skupiny:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Nakonec upravit `AppDelegate.cs` souboru a nastavit `DidFinishLaunching` metoda vypadá takto:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Pokud jsme spuštění aplikace, následující se zobrazí:

[ ![](source-list-images/source05.png "Příklad aplikace spustit")](source-list-images/source05.png)

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce se seznamy zdroje v aplikaci Xamarin.Mac trvá. Jsme viděli, jak vytvořit a udržovat zdroj seznamů v Xcode na rozhraní tvůrce a jak pracovat s zdroje jsou uvedené v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacOutlines (ukázka)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Zobrazení osnov](~/mac/user-interface/outline-view.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do zobrazení osnovy](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
